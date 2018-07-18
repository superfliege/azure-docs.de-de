---
title: Verwenden von SQL-Datenbanken in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in wenigen Schritten SQL-Datenbanken als Dienst in Azure Stack und den SQL Server-Ressourcenanbieteradapter bereitstellen können.
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
ms.openlocfilehash: e1505761a0bd1ea9dabdd0b2cbab7af902198311
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938331"
---
# <a name="deploy-the-sql-server-resource-provider-on-azure-stack"></a>Bereitstellen des SQL Server-Ressourcenanbieters in Azure Stack

Verwenden Sie den SQL Server-Ressourcenanbieter von Azure Stack, um SQL-Datenbank als Azure Stack-Dienst verfügbar zu machen. Der SQL Server-Ressourcenanbieter wird als Dienst auf einem virtuellen Windows Server 2016 Server Core-Computer ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen

Es gibt mehrere Voraussetzungen, die erfüllt werden müssen, bevor Sie den Azure Stack-SQL-Ressourcenanbieter bereitstellen können. Führen Sie zum Erfüllen dieser Anforderungen die folgenden Schritte auf einem Computer aus, der auf die privilegierte Endpunkt-VM zugreifen kann:

- Falls Sie dies noch nicht getan haben, [registrieren Sie Azure Stack](.\azure-stack-registration.md) bei Azure, damit Sie Azure Marketplace-Elemente herunterladen können.
- Fügen Sie dem Azure Stack-Marketplace die erforderliche Windows Server Core-VM hinzu, indem Sie das Image **Windows Server 2016 Datacenter – Server Core** herunterladen. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden. Stellen Sie sicher, dass Sie beim Ausführen des Skripts die Core-Option auswählen.

  >[!NOTE]
  >Wenn Sie ein Update installieren müssen, können Sie unter dem lokalen Abhängigkeitspfad ein einzelnes MSU-Paket platzieren. Wenn mehr als eine MSU-Datei gefunden wird, kann die Installation des SQL-Ressourcenanbieters nicht erfolgreich ausgeführt werden.

- Laden Sie die Binärdatei des SQL-Ressourcenanbieters herunter, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren. Der Ressourcenanbieter verfügt über einen entsprechenden Mindestbuild für Azure Stack. Vergewissern Sie sich, dass Sie die richtige Binärdatei für die ausgeführte Azure Stack-Version herunterladen.

    |Azure Stack-Version|SQL RP Version|
    |-----|-----|
    |Version 1804 (1.0.180513.1)|[SQL RP Version 1.1.24.0](https://aka.ms/azurestacksqlrp1804)
    |Version 1802 (1.0.180302.1)|[SQL RP Version 1.1.18.0](https://aka.ms/azurestacksqlrp1802)|
    |Version 1712 (1.0.180102.3, 1.0.180103.2 oder 1.0.180106.1 (integrierte Systeme))|[SQL RP Version 1.1.14.0](https://aka.ms/azurestacksqlrp1712)|
    |     |     |

### <a name="certificates"></a>Zertifikate

Nur bei Installationen in integrierten Systemen. Sie müssen das SQL-PaaS-PKI-Zertifikat bereitstellen, das im Abschnitt „Optionale PaaS-Zertifikate“ der [PKI-Anforderungen für die Azure Stack-Bereitstellung](.\azure-stack-pki-certs.md#optional-paas-certificates) beschrieben ist. Platzieren Sie die .pfx-Datei an dem durch den **DependencyFilesLocalPath**-Parameter festgelegten Speicherort.

## <a name="deploy-the-sql-resource-provider"></a>Bereitstellen des SQL-Ressourcenanbieters

Nachdem Sie alle Voraussetzungen erfüllt haben, führen Sie das Skript **DeploySqlProvider.ps1** aus, um den SQL-Ressourcenanbieter bereitzustellen. Das Skript „DeploySqlProvider.ps1“ wird als Teil der Binärdatei vom SQL-Ressourcenanbieter extrahiert, die Sie entsprechend Ihrer Azure Stack-Version heruntergeladen haben.

> [!IMPORTANT]
> Das System, in dem Sie das Skript ausführen, muss ein Windows 10- oder Windows Server 2016-System mit der neuesten Version der .NET-Runtime sein.

Öffnen Sie ein **neues** PowerShell-Konsolenfenster mit erhöhten Rechten, und wechseln Sie zum Verzeichnis, in das Sie die Binärdateien des SQL-Ressourcenanbieters extrahiert haben, um den SQL-Ressourcenanbieter bereitzustellen. Die Verwendung eines neuen PowerShell-Fensters wird empfohlen, um mögliche Probleme durch bereits geladene PowerShell-Module zu vermeiden.

Führen Sie das Skript „DeploySqlProvider.ps1“ aus, das die folgenden Aufgaben ausführt:

- Lädt die Zertifikate und andere Artefakte in ein Azure Stack-Speicherkonto hoch.
- Veröffentlicht Katalogpakete, damit Sie SQL-Datenbank-Instanzen über den Katalog bereitstellen können.
- Veröffentlicht ein Katalogpaket für die Bereitstellung von Hostservern.
- Implementiert eine VM unter Verwendung des heruntergeladenen Windows Server 2016 Core-Images und installiert dann den SQL-Ressourcenanbieter.
- Registriert einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
- Registriert Ihren Ressourcenanbieter beim lokalen Azure Resource Manager für die Operator- und Benutzerkonten.
- Installiert optional ein einzelnes Windows Server-Update während der Installation des Ressourcenanbieters.

> [!NOTE]
> Sobald die Bereitstellung des SQL-Ressourcenanbieters beginnt, wird die Ressourcengruppe **system.local.sqladapter** erstellt. Es kann bis zu 75 Minuten dauern, bis die vier erforderlichen Bereitstellungen für diese Ressourcengruppe fertig gestellt sind.

### <a name="deploysqlproviderps1-parameters"></a>Parameter „DeploySqlProvider.ps1“

Sie können die folgenden Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** | Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ (für integrierte Systeme _erforderlich_) |
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _Erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** | Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |

>[!NOTE]
> Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst eine Datenbank erstellen, wenn die SKU bereitgestellt wurde und ausgeführt wird.

## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Bereitstellen des SQL-Ressourcenanbieters mithilfe eines benutzerdefinierten Skripts

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
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account can be Azure Active Directory or Active Directory Federation Services.
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change to the directory If folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert

 ```

Wenn das Skript zur Installation des Ressourcenanbieters abgeschlossen ist, aktualisieren Sie Ihren Browser, um sicherzustellen, dass die neuesten Updates angezeigt werden.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals

Mit den folgenden Schritten können Sie überprüfen, ob der SQL-Ressourcenanbieter erfolgreich bereitgestellt wurde.

1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an.
2. Wählen Sie **Ressourcengruppen** aus.
3. Wählen Sie die Ressourcengruppe **system.\<Speicherort\>.sqladapter** aus.
4. Die Meldung unter **Bereitstellungen**, die im nächsten Bildschirm angezeigt wird, sollte **4 erfolgreich** sein.

      ![Überprüfen der Bereitstellung des SQL-Ressourcenanbieters](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)

5. Detaillierte Informationen über die Bereitstellung des Ressourcenanbieters erhalten Sie unter **EINSTELLUNGEN**. Wählen Sie **Bereitstellungen** aus, um z.B. folgende Informationen abzurufen: STATUS, ZEITSTEMPEL und DAUER für die einzelnen Bereitstellungen.

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Hostservern](azure-stack-sql-resource-provider-hosting-servers.md)
