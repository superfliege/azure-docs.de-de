---
title: Verwenden von SQL-Datenbanken in Azure Stack | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie in wenigen Schritten SQL-Datenbanken als Dienst in Azure Stack und den SQL Server-Ressourcenanbieteradapter bereitstellen können."
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: JeffGo
ms.openlocfilehash: bf52ed4986b4e0930b57721c0e38bbf748045a36
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Verwenden von SQL-Datenbanken in Microsoft Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Verwenden Sie den SQL Server-Ressourcenanbieteradapter, um SQL-Datenbanken als Dienst in [Azure Stack](azure-stack-poc.md) verfügbar zu machen. Nachdem Sie den Ressourcenanbieter installiert und mit mindestens einer SQL Server-Instanz verbunden haben, können Sie und Ihre Benutzer damit Folgendes erstellen:
- Datenbanken für native Cloud-Apps.
- Websites, die auf SQL basieren.
- Workloads, die auf SQL basieren.
Sie müssen nicht jedes Mal einen virtuellen Computer bereitstellen, der SQL Server hostet.

Der Ressourcenanbieter unterstützt nicht alle Datenbankverwaltungsfunktionen von [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/). Pools für elastische Datenbanken und die automatische Skalierung der Datenbankleistung nach oben oder unten sind beispielsweise nicht verfügbar. Der Ressourcenanbieter unterstützt jedoch ähnliche Vorgänge für das Erstellen, Lesen, Aktualisieren und Löschen. Die API ist nicht mit SQL-Datenbank kompatibel.

## <a name="sql-resource-provider-adapter-architecture"></a>Architektur des SQL-Ressourcenanbieteradapters
Der Ressourcenanbieter besteht aus drei Komponenten:

- **Dem virtuellen Computer des SQL-Ressourcenanbieteradapters**, einem virtuellen Windows-Computer, der die Anbieterdienste ausführt.
- **Dem Ressourcenanbieter selbst**, der die Bereitstellungsanforderungen verarbeitet und Datenbankressourcen verfügbar macht.
- **Servern, die SQL Server hosten**, die Kapazität für Datenbanken bereitstellen und als „Hostserver“ bezeichnet werden.

Sie müssen mindestens eine SQL Server-Instanz erstellen und/oder Zugriff auf externe SQL Server-Instanzen bereitstellen.

## <a name="deploy-the-resource-provider"></a>Bereitstellen des Ressourcenanbieters

1. Wenn nicht bereits geschehen, registrieren Sie Ihr Development Kit und laden das Image „Windows Server 2016 Datacenter Core“ über die Marketplace-Verwaltung herunter. Sie müssen ein Windows Server 2016 Core-Image verwenden. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden. (Achten Sie darauf, dass die Option „Core“ ausgewählt ist.) Die .NET 3.5-Laufzeit ist nicht mehr erforderlich.

2. Melden Sie sich bei einem Host an, der Zugriff auf die VM mit dem privilegierten Endpunkt hat.

    - Melden Sie sich in Azure Stack Development Kit-Installationen beim physischen Host an.

    - In Systemen mit mehreren Knoten muss der Host ein System sein, das auf den privilegierten Endpunkt zugreifen kann.
    
    >[!NOTE]
    > Das System, in dem das Skript ausgeführt wird, *muss* ein Windows 10- oder Windows Server 2016-System mit der aktuellen Version der .NET-Laufzeit sein. Andernfalls tritt bei der Installation ein Fehler auf. Das Azure Stack SDK erfüllt dieses Kriterium.


3. Laden Sie die Binärdatei des SQL-Ressourcenanbieters herunter. Führen Sie das selbstextrahierende Programm aus, um die Inhalte in ein temporäres Verzeichnis zu extrahieren.

    >[!NOTE] 
    > Der Build des Ressourcenanbieters entspricht den Azure Stack-Builds. Vergewissern Sie sich, dass Sie die richtige Binärdatei für die ausgeführte Azure Stack-Version herunterladen.

    | Azure Stack-Build | Installationsprogramm für SQL-Ressourcenanbieter |
    | --- | --- |
    |1.0.180102.3, 1.0.180103.2 oder 1.0.180106.1 (mehrere Knoten) | [SQL RP Version 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1.0.171122.1 | [SQL RP Version 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1.0.171028.1 | [SQL RP Version 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. Das Azure Stack-Stammzertifikat wird vom privilegierten Endpunkt abgerufen. Für das Azure Stack SDK wird im Rahmen dieses Prozesses ein selbstsigniertes Zertifikat erstellt. Bei mehreren Knoten müssen Sie ein entsprechendes Zertifikat bereitstellen.

   Um Ihr eigenes Zertifikat bereitzustellen, platzieren Sie wie folgt eine PFX-Datei in **DependencyFilesLocalPath**:

    - Ein Platzhalterzertifikat für „\*.dbadapter.\<Region\>.\<externer FQDN\>“ oder ein einzelnes Zertifizierungsstellenzertifikat mit einem allgemeinen Namen wie „sqladapter.dbadapter.\<Region\>.\<externer FQDN\>“.

    - Dem Zertifikat muss vertraut werden. Das bedeutet, die Vertrauenskette muss vorhanden sein, ohne dass Zwischenzertifikate erforderlich sind.

    - Unter „DependencyFilesLocalPath“ ist nur eine Zertifikatdatei vorhanden.

    - Der Dateiname darf keine Sonderzeichen enthalten.


5. Öffnen Sie eine **neue** PowerShell-Konsole mit erhöhten Rechten (Administrator), und wechseln Sie zu dem Verzeichnis, in dem Sie die Dateien extrahiert haben. Verwenden Sie ein neues Fenster, um Probleme aufgrund von falschen PowerShell-Modulen zu vermeiden, die bereits in Ihrem System geladen sind.

6. [Installieren Sie Azure PowerShell, Version 1.2.11](azure-stack-powershell-install.md).

7. Führen Sie das Skript „DeploySqlProvider.ps1“ aus, das folgende Aufgaben erledigt:

    - Lädt die Zertifikate und andere Artefakte in ein Azure Stack-Speicherkonto hoch.
    - Veröffentlicht Katalogpakete, damit Sie SQL-Datenbanken über den Katalog bereitstellen können.
    - Veröffentlicht ein Katalogpaket für die Bereitstellung von Hostservern.
    - Stellt einen virtuellen Computer mithilfe des in Schritt 1 erstellten Windows Server 2016-Images bereit, und installiert dann den Ressourcenanbieter.
    - Registriert einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
    - Registriert Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz (Benutzer und Administrator).

> [!NOTE]
> Wenn die Installation mehr als 90 Minuten dauert, tritt möglicherweise ein Fehler auf. In diesem Fall wird auf dem Bildschirm und in der Protokolldatei eine Fehlermeldung angezeigt. Die Bereitstellung wird jedoch ab dem fehlerhaften Schritt wiederholt. Systeme, die nicht die empfohlenen Arbeitsspeicher- und vCPU-Spezifikationen aufweisen, können den SQL-Ressourcenanbieter möglicherweise nicht bereitstellen.
>

Hier finden Sie ein Beispiel, das Sie an der PowerShell-Eingabeaufforderung ausführen können. (Achten Sie darauf, die Kontoinformationen und Kennwörter wie erforderlich zu ändern.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>Parameter „DeploySqlProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** | Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _Erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** | Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals

> [!NOTE]
>  Nachdem das Installationsskript vollständig ausgeführt wurde, müssen Sie das Portal aktualisieren, damit das Blatt „Administrator“ angezeigt wird.


1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an.

2. Überprüfen Sie, ob die Bereitstellung erfolgreich war. Wechseln Sie zu **Ressourcengruppen**. Wählen Sie dann die Ressourcengruppe **system.\<Speicherort\>.sqladapter** aus. Überprüfen Sie, ob alle vier Bereitstellungen erfolgreich ausgeführt wurden.

      ![Überprüfen der Bereitstellung des SQL-Ressourcenanbieters](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Aktualisieren des SQL-Ressourcenanbieteradapters (nur mehrere Knoten, Build 1710 und höher)
Bei jeder Aktualisierung des Azure Stack-Builds wird ein neuer SQL-Ressourcenanbieteradapter veröffentlicht. Der vorhandene Adapter funktioniert möglicherweise weiterhin. Es wird jedoch empfohlen, nach der Aktualisierung des Azure Stack-Builds den Adapter so schnell wie möglich ebenfalls zu aktualisieren. 

Der Updateprozess ähnelt dem oben beschriebenen Installationsvorgang. Sie erstellen einen neuen virtuellen Computer mit dem neuesten Ressourcenanbietercode. Darüber hinaus migrieren Sie Einstellungen auf diese neue Instanz, einschließlich der Informationen zu Datenbank und Hostserver. Sie migrieren auch den erforderlichen DNS-Eintrag.

Verwenden Sie das Skript „UpdateSQLProvider.ps1“ mit den gleichen Argumenten wie oben beschrieben. Sie müssen das Zertifikat hier ebenfalls angeben.

> [!NOTE]
> Dieser Updateprozess wird nur in Systemen mit mehreren Knoten unterstützt.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>Parameter in „UpdateSQLProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn Sie keine Parameter angeben oder bei der Überprüfung eines Parameters ein Fehler auftritt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _Erforderlich_ |
| **AzCredential** | Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack. | _Erforderlich_ |
| **VMLocalCredential** | Die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _Erforderlich_ |
| **PrivilegedEndpoint** | Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts. |  _Erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _Optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Das Kennwort für das PFX-Zertifikat. | _erforderlich_ |
| **MaxRetryCount** | Die Anzahl von Wiederholungsversuchen für jeden Vorgang, wenn ein Fehler auftritt.| 2 |
| **RetryDuration** |Das Timeoutintervall zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung nach einem Fehler. | Nein  |



## <a name="remove-the-sql-resource-provider-adapter"></a>Entfernen des SQL-Ressourcenanbieteradapters

Zum Entfernen des Ressourcenanbieters müssen unbedingt zuerst alle Abhängigkeiten entfernt werden.

1. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des SQL-Ressourcenanbieteradapters heruntergeladen haben.

2. Alle Benutzerdatenbanken müssen aus dem Ressourcenanbieter gelöscht werden. (Die Daten werden dabei nicht gelöscht.) Diese Aufgabe sollte von den Benutzern selbst ausgeführt werden.

3. Der Administrator muss die Hostserver aus dem SQL-Ressourcenanbieteradapter löschen.

4. Der Administrator muss alle Pläne löschen, die auf den SQL-Ressourcenanbieteradapter verweisen.

5. Der Administrator muss alle SKUs und Kontingente löschen, die dem SQL-Ressourcenanbieteradapter zugewiesen sind.

6. Führen Sie das Bereitstellungsskript mit den folgenden Elementen erneut aus:
    - Parameter „-Uninstall“
    - Azure Resource Manager-Endpunkte
    - DirectoryTenantID
    - Anmeldeinformationen für das Dienstadministratorkonto


## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Hostservern](azure-stack-sql-resource-provider-hosting-servers.md) und [Erstellen von Datenbanken](azure-stack-sql-resource-provider-databases.md).

Probieren Sie andere [PaaS-Dienste](azure-stack-tools-paas-services.md) wie den [MySQL Server-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md) und den [App Services-Ressourcenanbieter](azure-stack-app-service-overview.md) aus.
