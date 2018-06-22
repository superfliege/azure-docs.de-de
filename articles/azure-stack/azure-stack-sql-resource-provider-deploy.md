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
ms.date: 05/24/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 8643e75a24ff7840b71dfaceae9934cdda566d30
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604419"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Verwenden von SQL-Datenbanken in Microsoft Azure Stack
Verwenden Sie den SQL Server-Ressourcenanbieter von Azure Stack, um SQL-Datenbanken als Dienst in Azure Stack verfügbar zu machen. Der SQL-Ressourcenanbieterdienst wird auf der VM des SQL-Ressourcenanbieters ausgeführt, was ein virtueller Computer unter Windows Server Core ist.

## <a name="prerequisites"></a>Voraussetzungen
Es gibt mehrere Voraussetzungen, die erfüllt werden müssen, bevor Sie den Azure Stack-SQL-Ressourcenanbieter bereitstellen können. Führen Sie die folgenden Schritte auf einem Computer aus, der auf die privilegierte Endpunkt-VM zugreifen kann:

- [Registrieren Sie Azure Stack](.\azure-stack-registration.md) mit Azure, damit Sie Azure Marketplace-Elemente herunterladen können, falls Sie dies noch nicht getan haben.
- Fügen Sie dem Azure Stack-Marketplace die erforderliche Windows Server Core-VM hinzu, indem Sie das Image **Windows Server 2016 Server Core** herunterladen. Wenn Sie ein Update installieren müssen, können Sie unter dem lokalen Abhängigkeitspfad ein einzelnes MSU-Paket anordnen. Wenn mehr als eine MSU-Datei gefunden wird, schlägt die Installation des SQL-Ressourcenanbieters fehl.
- Laden Sie die Binärdatei des SQL-Ressourcenanbieters herunter, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren. Der Ressourcenanbieter verfügt über einen entsprechenden Mindestbuild für Azure Stack. Vergewissern Sie sich, dass Sie die richtige Binärdatei für die ausgeführte Azure Stack-Version herunterladen:

    |Azure Stack-Version|SQL RP Version|
    |-----|-----|
    |Version 1804 (1.0.180513.1)|[SQL RP Version 1.1.24.0](https://aka.ms/azurestacksqlrp1804)
    |Version 1802 (1.0.180302.1)|[SQL RP Version 1.1.18.0](https://aka.ms/azurestacksqlrp1802)|
    |Version 1712 (1.0.180102.3, 1.0.180103.2 oder 1.0.180106.1 (integrierte Systeme))|[SQL RP Version 1.1.14.0](https://aka.ms/azurestacksqlrp1712)|
    |     |     |
- Sie müssen das SQL-PaaS-PKI-Zertifikat wie im Abschnitt zu optionalen PaaS-Zertifikaten der [Azure Stack-PKI-Bereitstellungsanforderungen](.\azure-stack-pki-certs.md#optional-paas-certificates) beschrieben nur für integrierte Systeminstallationen angeben, indem Sie die PFX-Datei in dem Speicherort platzieren, der vom Parameter **DependencyFilesLocalPath** angegeben wird.

## <a name="deploy-the-sql-resource-provider"></a>Bereitstellen des SQL-Ressourcenanbieters
Nachdem Sie die Installation des SQL-Ressourcenanbieters erfolgreich vorbereitet haben, indem Sie alle Voraussetzungen erfüllen, können Sie nun das Skript **DeploySqlProvider.ps1** ausführen, um den SQL-Ressourcenanbieter bereitzustellen. Das Skript „DeploySqlProvider.ps1“ wird als Teil der Binärdatei vom SQL-Ressourcenanbieter extrahiert, den Sie entsprechend Ihrer Azure Stack-Version heruntergeladen haben. 

> [!IMPORTANT]
> Das System, in dem das Skript ausgeführt wird, muss ein Windows 10- oder Windows Server 2016-System mit der aktuellen Version der .NET-Laufzeit sein.


Öffnen Sie eine neue PowerShell-Konsole mit erhöhten Rechten (Administrator), und wechseln Sie zum Verzeichnis, in das Sie die Binärdateien des SQL-Ressourcenanbieters extrahiert haben, um den SQL-Ressourcenanbieter bereitzustellen.

> [!NOTE]
> Verwenden Sie ein neues Fenster der PowerShell-Konsole, um Probleme zu umgehen, die bei falschen, bereits im System geladenen PowerShell-Modulen auftreten können.

Führen Sie das Skript „DeploySqlProvider.ps1“ aus, das die folgenden Schritte erledigt:
- Lädt die Zertifikate und andere Artefakte in ein Azure Stack-Speicherkonto hoch.
- Veröffentlicht Katalogpakete, damit Sie SQL-Datenbanken über den Katalog bereitstellen können.
- Veröffentlicht ein Katalogpaket für die Bereitstellung von Hostservern.
- Stellt einen virtuellen Computer mithilfe des in Schritt 1 erstellten Windows Server 2016-Images bereit, und installiert dann den Ressourcenanbieter.
- Registriert einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
- Registriert Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz (Benutzer und Administrator).
- Installiert während der Installation des Ressourcenanbieters optional ein einzelnes Windows-Update.

Die Bereitstellung des SQL-Ressourcenanbieters beginnt, und die Ressourcengruppe „system.local.sqladapter“ wird erstellt. Es kann bis zu 75 Minuten dauern, bis die vier erforderlichen Bereitstellungen für diese Ressourcengruppe abgeschlossen sind.

### <a name="deploysqlproviderps1-parameters"></a>Parameter „DeploySqlProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

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
> Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst eine Datenbank erstellen, wenn die SKU erstellt wurde.


## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Bereitstellen des SQL-Ressourcenanbieters mithilfe eines benutzerdefinierten Skripts
Sie können das folgende Skriptbeispiel anpassen, indem Sie die Standardkontoinformationen und Kennwörter nach Bedarf ändern, um das manuelle Eingeben von Informationen bei der Ausführung vom „DeploySqlProvider.ps1“-Skript zu umgehen:

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

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
$tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert
 ```

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals
Die Schritte in diesem Abschnitt können dazu verwendet werden, sicherzustellen, dass der SQL-Ressourcenanbieter erfolgreich bereitgestellt wurde.

> [!NOTE]
>  Nachdem das Installationsskript vollständig ausgeführt wurde, müssen Sie das Portal aktualisieren, damit das Blatt „Administrator“ und Katalogelemente angezeigt werden.

1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an.

2. Überprüfen Sie, ob die Bereitstellung erfolgreich war. Wechseln Sie zu **Ressourcengruppen**. Wählen Sie dann die Ressourcengruppe **system.\<Speicherort\>.sqladapter** aus. Überprüfen Sie, ob alle vier Bereitstellungen erfolgreich ausgeführt wurden.

      ![Überprüfen der Bereitstellung des SQL-Ressourcenanbieters](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Hostservern](azure-stack-sql-resource-provider-hosting-servers.md)
