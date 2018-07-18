---
title: Verwenden von MySQL-Datenbank-Instanzen in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in wenigen Schritten MySQL-Datenbank-Instanzen als Dienst in Azure Stack und den MySQL Server-Ressourcenanbieteradapter bereitstellen können.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e4c3eb1d7dfd4894576d5fbed52cf4de5fed9e44
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938112"
---
# <a name="deploy-the-mysql-resource-provider-on-azure-stack"></a>Bereitstellen des MySQL-Ressourcenanbieters in Azure Stack

Verwenden Sie den MySQL Server-Ressourcenanbieter, um MySQL-Datenbanken als Azure Stack-Dienst verfügbar zu machen. Der MySQL Server-Ressourcenanbieter wird als Dienst auf einem virtuellen Windows Server 2016 Server Core-Computer ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen

Es gibt mehrere Voraussetzungen, die erfüllt werden müssen, bevor Sie den Azure Stack-MySQL-Ressourcenanbieter bereitstellen können. Führen Sie zum Erfüllen dieser Anforderungen die Schritte in diesem Artikel auf einem Computer aus, der auf die privilegierte Endpunkt-VM zugreifen kann.

* Falls Sie dies noch nicht getan haben, [registrieren Sie Azure Stack](.\azure-stack-registration.md) bei Azure, damit Sie Azure Marketplace-Elemente herunterladen können.
* Fügen Sie dem Azure Stack-Marketplace die erforderliche Windows Server Core-VM hinzu, indem Sie das Image **Windows Server 2016 Datacenter – Server Core** herunterladen. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden. Stellen Sie sicher, dass Sie beim Ausführen des Skripts die Core-Option auswählen.

  >[!NOTE]
  >Wenn Sie ein Update installieren müssen, können Sie unter dem lokalen Abhängigkeitspfad ein einzelnes MSU-Paket anordnen. Wenn mehr als eine MSU-Datei gefunden wird, kann die Installation des MySQL-Ressourcenanbieters nicht erfolgreich ausgeführt werden.

* Laden Sie die Binärdatei des MySQL-Ressourcenanbieters herunter, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren.

  >[!NOTE]
  >Zum Bereitstellen des MySQL-Anbieters in einem System ohne Internetzugriff kopieren Sie die Datei [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) in eine lokale Freigabe. Geben Sie den Namen dieser Freigabe an, wenn Sie dazu aufgefordert werden. Sie müssen die PowerShell-Module für Azure und Azure Stack installieren.

* Der Ressourcenanbieter verfügt über einen entsprechenden Mindestbuild für Azure Stack. Vergewissern Sie sich, dass Sie die richtige Binärdatei für die ausgeführte Azure Stack-Version herunterladen.

    | Azure Stack-Version | MySQL RP Version|
    | --- | --- |
    | Version 1804 (1.0.180513.1)|[MySQL RP Version 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) |
    | Version 1802 (1.0.180302.1) | [MySQL RP Version 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) |
    | Version 1712 (1.0.180102.3 oder 1.0.180106.1 (integrierte Systeme)) | [MySQL RP Version 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |

### <a name="certificates"></a>Zertifikate

Für das ASDK wird im Rahmen des Installationsprozesses ein selbstsigniertes Zertifikat erstellt. Bei einem integrierten Azure Stack-System müssen Sie ein entsprechendes Zertifikat bereitstellen. Wenn Sie Ihr eigenes Zertifikat bereitstellen müssen, platzieren Sie eine PFX-Datei in **DependencyFilesLocalPath**, die folgende Kriterien erfüllt:

* Ein Platzhalterzertifikat für „\*.dbadapter.\<Region\>.\<externer FQDN\>“ oder ein einzelnes Zertifizierungsstellenzertifikat mit einem allgemeinen Namen wie „mysqladapter.dbadapter.\<Region\>.\<externer FQDN\>“.
* Dem Zertifikat muss vertraut werden. Das bedeutet, dass die Vertrauenskette vorhanden sein muss, ohne dass Zwischenzertifikate erforderlich sind.
* Unter „DependencyFilesLocalPath“ ist nur eine Zertifikatdatei vorhanden.
* Der Dateiname darf keine Sonderzeichen oder Leerzeichen enthalten.

## <a name="deploy-the-resource-provider"></a>Bereitstellen des Ressourcenanbieters

Nachdem Sie alle Voraussetzungen erfüllt haben, führen Sie das Skript **DeployMySqlProvider.ps1** aus, um den MySQL-Ressourcenanbieter bereitzustellen. Das Skript „DeployMySqlProvider.ps1“ wird als Teil der Binärdatei vom MySQL-Ressourcenanbieter extrahiert, die Sie entsprechend Ihrer Azure Stack-Version heruntergeladen haben.

> [!IMPORTANT]
> Das System, in dem Sie das Skript ausführen, muss ein Windows 10- oder Windows Server 2016-System mit der neuesten Version der .NET-Runtime sein.

Öffnen Sie ein neues PowerShell-Konsolenfenster mit erhöhten Rechten, und wechseln Sie zu dem Verzeichnis, in das Sie die Binärdateien des MySQL-Ressourcenanbieters extrahiert haben, um den MySQL-Ressourcenanbieter bereitzustellen. Die Verwendung eines neuen PowerShell-Fensters wird empfohlen, um mögliche Probleme durch bereits geladene PowerShell-Module zu vermeiden.

Führen Sie das Skript **DeployMySqlProvider.ps1** aus, das die folgenden Aufgaben ausführt:

* Lädt die Zertifikate und andere Artefakte in ein Azure Stack-Speicherkonto hoch.
* Veröffentlicht Katalogpakete, damit Sie MySQL-Datenbank-Instanzen über den Katalog bereitstellen können.
* Veröffentlicht ein Katalogpaket für die Bereitstellung von Hostservern.
* Implementiert eine VM unter Verwendung des heruntergeladenen Windows Server 2016 Core-Images und installiert dann den MySQL-Ressourcenanbieter.
* Registriert einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
* Registriert Ihren Ressourcenanbieter beim lokalen Azure Resource Manager für die Operator- und Benutzerkonten.
* Installiert optional ein einzelnes Windows Server-Update während der Installation des Ressourcenanbieters.

> [!NOTE]
> Sobald die Bereitstellung des MySQL-Ressourcenanbieters beginnt, wird die Ressourcengruppe **system.local.mysqladapter** erstellt. Es kann bis zu 75 Minuten dauern, bis die vier erforderlichen Bereitstellungen für diese Ressourcengruppe fertig gestellt sind.

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1-Parameter

Sie können diese Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** | Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem MySQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **DependencyFilesLocalPath** | Pfad zu einer lokalen Freigabe, die [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi) enthält. Wenn Sie einen dieser Pfade angeben, muss die Zertifikatdatei ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ für einzelne Knoten, _obligatorisch_ für mehrere Knoten. |
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _Erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** | Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |
| **AcceptLicense** | Überspringt die Aufforderung zum Akzeptieren der GPL-Lizenz.  <http://www.gnu.org/licenses/old-licenses/gpl-2.0.html> | |

> [!NOTE]
> Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst dann eine Datenbank erstellen, wenn die SKU bereitgestellt wurde und ausgeführt wird.

## <a name="deploy-the-mysql-resource-provider-using-a-custom-script"></a>Bereitstellen des MySQL-Ressourcenanbieters mithilfe eines benutzerdefinierten Skripts

Um jegliche manuelle Konfiguration bei der Bereitstellung des Ressourcenanbieters zu vermeiden, können Sie das folgende Skript anpassen. Ändern Sie die Standardkontoinformationen und -kennwörter für Ihre Azure Stack-Bereitstellung nach Bedarf.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"  

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files.
# Find the ERCS01 IP address first, and make sure the certificate file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert `
    -AcceptLicense

```

Wenn das Skript zur Installation des Ressourcenanbieters abgeschlossen ist, aktualisieren Sie Ihren Browser, um sicherzustellen, dass die neuesten Updates angezeigt werden.

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals

1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an.
2. Wählen Sie **Ressourcengruppen** aus.
3. Wählen Sie die Ressourcengruppe **system.\<Speicherort\>.mysqladapter** aus.
4. Auf der Zusammenfassungsseite der Ressourcengruppenübersicht sollte unter **Bereitstellungen** die Meldung **3 – erfolgreich** angezeigt werden.
5. Detaillierte Informationen über die Bereitstellung des Ressourcenanbieters erhalten Sie unter **EINSTELLUNGEN**. Wählen Sie **Bereitstellungen** aus, um z.B. folgende Informationen abzurufen: STATUS, ZEITSTEMPEL und DAUER für die einzelnen Bereitstellungen.

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Hostservern](azure-stack-mysql-resource-provider-hosting-servers.md)
