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
ms.date: 12/15/2017
ms.author: JeffGo
ms.openlocfilehash: 80b693420768d574b2371211298562ba35e7ed97
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2017
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Verwenden von SQL-Datenbanken in Microsoft Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Verwenden Sie den SQL Server-Ressourcenanbieteradapter, um SQL-Datenbanken als Dienst in [Azure Stack](azure-stack-poc.md) verfügbar zu machen. Nachdem Sie den Ressourcenanbieter installiert und mit mindestens einer SQL Server-Instanz verbunden haben, können Sie und Ihre Benutzer damit Folgendes erstellen:
- Datenbanken für native Cloud-Apps
- Websites, die auf SQL basieren
- Workloads, die auf SQL basieren. Sie müssen nicht jedes Mal einen virtuellen Computer bereitstellen, der SQL Server hostet.

Der Ressourcenanbieter unterstützt nicht alle Datenbankverwaltungsfunktionen von [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/). Pool für elastische Datenbanken und das automatische Skalieren der Datenbankleistung nach oben oder unten sind beispielsweise nicht verfügbar. Der Ressourcenanbieter unterstützt jedoch ähnliche Vorgänge für das Erstellen, Lesen, Aktualisieren und Löschen. Die API ist nicht mit SQL DB kompatibel.

## <a name="sql-resource-provider-adapter-architecture"></a>Architektur des SQL-Ressourcenanbieteradapters
Der Ressourcenanbieter besteht aus drei Komponenten:

- **Dem virtuellen Computer des SQL-Ressourcenanbieteradapters**, ein virtueller Windows-Computer, der die Anbieterdienste ausführt
- **Dem Ressourcenanbieter selbst**, der die Bereitstellungsanforderungen verarbeitet und Datenbankressourcen verfügbar macht
- **Servern, die SQL Server hosten**, die Kapazität für Datenbanken bereitstellen und als „Hostserver“ bezeichnet werden

Sie müssen mindestens eine SQL Server-Instanz erstellen oder Zugriff auf externe SQL-Instanzen bereitstellen.

## <a name="deploy-the-resource-provider"></a>Bereitstellen des Ressourcenanbieters

1. Wenn nicht bereits geschehen, registrieren Sie Ihr Development Kit und laden das Image „Windows Server 2016 Datacenter Core“ über die Marketplace-Verwaltung herunter. Sie müssen ein Windows Server 2016 Core-Image verwenden. Sie können auch ein Skript zum Erstellen eines [Windows Server 2016-Images](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) verwenden – achten Sie aber darauf, die Option „Core“ auszuwählen. Die .NET 3.5-Laufzeit ist nicht mehr erforderlich.

2. Melden Sie sich auf einem Host an, der Zugriff auf die VM mit dem privilegierten Endpunkt hat.

    a. Melden Sie sich bei Installationen mit dem Azure Stack Development Kit auf dem physischen Host an.

    b. Auf Systemen mit mehreren Knoten muss der Host ein System sein, das auf den privilegierten Endpunkt zugreifen kann. 
    
    >[!NOTE]
    > Das System, in dem das Skript ausgeführt wird, *muss* ein Windows 10- oder Windows Server 2016-System mit der aktuellen Version der .NET-Laufzeit sein. Andernfalls schlägt die Installation fehl. Der ASDK-Host erfüllt diese Kriterien.


3. Laden Sie die Binärdatei des SQL-Ressourcenanbieters herunter, und führen Sie Self-Extractor aus, um den Inhalt in einem temporären Verzeichnis zu extrahieren.

    >[!NOTE] 
    > Der Build des Ressourcenanbieters entspricht den Azure Stack-Builds. Sie müssen die richtige Binärdatei für die ausgeführte Azure Stack-Version herunterladen.

    | Azure Stack-Build | SQL RP-Installationsprogramm |
    | --- | --- |
    | 1.0.171122.1 | [SQL RP Version 1.1.12.0](https://aka.ms/azurestacksqlrp) |
    | 1.0.171028.1 | [SQL RP Version 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
    | 1.0.170928.3 | [SQL RP Version 1.1.3.0](https://aka.ms/azurestacksqlrp1709) |
   

4. Das Azure Stack-Stammzertifikat wird vom privilegierten Endpunkt abgerufen. Für ASDK wird im Rahmen dieses Prozesses ein selbstsigniertes Zertifikat erstellt. Bei mehreren Knoten müssen Sie ein entsprechendes Zertifikat bereitstellen.

    Wenn Sie Ihr eigenes Zertifikat bereitstellen müssen, benötigen Sie eine PFX-Datei unter **DependencyFilesLocalPath** (siehe unten):

    - Ein Platzhalterzertifikat für „\*.dbadapter.\<Region\>.\<externer FQDN\>“ oder ein einzelnes ZS-Zertifikat mit einem allgemeinen Namen wie „sqladapter.dbadapter.\<Region\>.\<externer FQDN\>“
    - Diesem Zertifikat muss so vertraut werden, als würde es z.B. von einer Zertifizierungsstelle ausgestellt werden. Das bedeutet, die Vertrauenskette muss vorhanden sein, ohne dass Zwischenzertifikate erforderlich sind.
    - Unter „DependencyFilesLocalPath“ ist nur eine Zertifikatdatei vorhanden.
    - Der Dateiname darf keine Sonderzeichen enthalten.


5. Öffnen Sie eine **neue** PowerShell-Konsole mit erhöhten Rechten (Administrator), und wechseln Sie zu dem Verzeichnis, in dem Sie die Dateien extrahiert haben. Verwenden Sie ein neues Fenster, um Probleme aufgrund von falschen PowerShell-Modulen, die bereits in Ihrem System geladen sind, zu vermeiden.

6. [Installieren Sie Azure PowerShell, Version 1.2.11](azure-stack-powershell-install.md).

7. Führen Sie das Skript „DeploySqlProvider.ps1“ mit den unten aufgeführten Parametern aus.

Mit diesem Skript werden folgende Schritte ausgeführt:

- Laden Sie die Zertifikate und andere Artefakte in Ihr Azure Stack-Speicherkonto hoch.
- Veröffentlichen Sie Katalogpakete, damit Sie SQL-Datenbanken über den Katalog bereitstellen können.
- Veröffentlichen eines Katalogpakets für die Bereitstellung von Hostservern
- Stellen Sie einen virtuellen Computer mithilfe des in Schritt 1 erstellten Windows Server 2016-Images bereit, und installieren Sie den Ressourcenanbieter.
- Registrieren Sie einen lokalen DNS-Eintrag, der dem virtuellen Computer mit dem Ressourcenanbieter zugeordnet wird.
- Registrieren Sie Ihren Ressourcenanbieter bei der lokalen Azure Resource Manager-Instanz (Benutzer und Administrator).

> [!NOTE]
> Wenn die Installation länger als 90 Minuten dauert, schlägt sie möglicherweise fehl, und auf dem Bildschirm sowie in der Protokolldatei wird eine Fehlermeldung angezeigt. Die Bereitstellung wird jedoch ab dem fehlgeschlagenen Schritt wiederholt. Systeme, die den empfohlenen Arbeitsspeicher- und vCPU-Spezifikationen nicht entsprechen, können den SQL-Ressourcenanbieter möglicherweise nicht bereitstellen.
>

Hier ist ein Beispiel, das Sie über die PowerShell-Eingabeaufforderung ausführen können (ändern Sie jedoch die Kontoinformationen und Kennwörter entsprechend):

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>Parameter „DeploySqlProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn sie nicht festgelegt werden oder eine Parameterüberprüfung fehlschlägt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _erforderlich_ |
| **AzCredential** | Geben Sie die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto an. Verwenden Sie die gleichen Anmeldeinformationen wie für das Bereitstellen von Azure Stack. | _erforderlich_ |
| **VMLocalCredential** | Definieren Sie die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _erforderlich_ |
| **PrivilegedEndpoint** | Geben Sie die IP-Adresse oder den DNS-Namen des privilegierten Endpunkts an. |  _erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatsdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Kennwort für das PFX-Zertifikat | _erforderlich_ |
| **MaxRetryCount** | Definieren Sie, wie oft Sie jeden Vorgang wiederholen möchten, wenn ein Fehler vorliegt.| 2 |
| **RetryDuration** | Definieren Sie das Timeout zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernen Sie den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung bei einem Fehler | Nein  |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfung der Bereitstellung mithilfe des Azure Stack-Portals

> [!NOTE]
>  Nachdem das Installationsskript abgeschlossen ist, müssen Sie das Portal aktualisieren, damit das Blatt „Administrator“ angezeigt wird.


1. Melden Sie sich als Dienstadministrator beim Verwaltungsportal an.

2. Überprüfen Sie, ob die Bereitstellung erfolgreich war. Suchen Sie nach **Ressourcengruppen** &gt;, klicken Sie auf die Ressourcengruppe **system.\<location\>.sqladapter**, und überprüfen Sie, ob alle vier Bereitstellungen erfolgreich waren.

      ![Überprüfen der Bereitstellung des SQL-Ressourcenanbieters](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Aktualisieren Sie den SQL-Ressourcenanbieteradapter (nur mehrere Knoten, Build 1710 und höher)
Bei jeder Aktualisierung des Azure Stack-Builds wird ein neuer SQL-Ressourcenanbieteradapter veröffentlicht. Der vorhandene Adapter funktioniert zwar unter Umständen weiterhin, es wird jedoch empfohlen, nach der Aktualisierung von Azure Stack baldmöglichst ein Update auf den aktuellen Build auszuführen. Der Updateprozess ist dem oben beschriebenen Installationsvorgang sehr ähnlich. Ein neuer virtueller Computer mit dem aktuellen Ressourcenanbietercode wird erstellt, und die Einstellungen (etwa Datenbank- und Hostserverinformationen und der erforderliche DNS-Eintrag) werden zu dieser neuen Instanz migriert.

Verwenden Sie das Skript „UpdateSQLProvider.ps1“ mit den gleichen Argumenten wie oben. Sie müssen das Zertifikat hier ebenfalls angeben.

> [!NOTE]
> Das Update wird nur auf Systemen mit mehreren Knoten unterstützt.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>Parameter in „UpdateSQLProvider.ps1“
Sie können diese Parameter in der Befehlszeile angeben. Wenn sie nicht festgelegt werden oder eine Parameterüberprüfung fehlschlägt, werden Sie aufgefordert, die erforderlichen Parameter anzugeben.

| Parametername | BESCHREIBUNG | Kommentar oder Standardwert |
| --- | --- | --- |
| **CloudAdminCredential** | Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind. | _erforderlich_ |
| **AzCredential** | Geben Sie die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto an. Verwenden Sie die gleichen Anmeldeinformationen wie für das Bereitstellen von Azure Stack. | _erforderlich_ |
| **VMLocalCredential** | Definieren Sie die Anmeldeinformationen für das lokale Administratorkonto des virtuellen Computers mit dem SQL-Ressourcenanbieter. | _erforderlich_ |
| **PrivilegedEndpoint** | Geben Sie die IP-Adresse oder den DNS-Namen des privilegierten Endpunkts an. |  _erforderlich_ |
| **DependencyFilesLocalPath** | Ihre Zertifikatsdatei (PFX) muss ebenfalls in diesem Verzeichnis abgelegt werden. | _optional_ (_obligatorisch_ bei mehreren Knoten) |
| **DefaultSSLCertificatePassword** | Kennwort für das PFX-Zertifikat | _erforderlich_ |
| **MaxRetryCount** | Definieren Sie, wie oft Sie jeden Vorgang wiederholen möchten, wenn ein Fehler vorliegt.| 2 |
| **RetryDuration** | Definieren Sie das Timeout zwischen Wiederholungen in Sekunden. | 120 |
| **Deinstallieren** | Entfernen Sie den Ressourcenanbieter und alle zugeordneten Ressourcen (siehe folgende Hinweise). | Nein  |
| **DebugMode** | Verhindert die automatische Bereinigung bei einem Fehler | Nein  |



## <a name="remove-the-sql-resource-provider-adapter"></a>Entfernen des SQL-Ressourcenanbieteradapters

Zum Entfernen des Ressourcenanbieters müssen unbedingt zuerst alle Abhängigkeiten entfernt werden.

1. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des SQL-Ressourcenanbieteradapters heruntergeladen haben.

2. Alle Benutzerdatenbanken müssen aus dem Ressourcenanbieter gelöscht werden (die Daten werden dabei nicht gelöscht). Diese Aktion sollte von den Benutzern selbst durchgeführt werden.

3. Der Administrator muss die Hostserver aus dem SQL-Ressourcenanbieteradapter löschen.

4. Der Administrator muss alle Pläne löschen, die auf den SQL-Ressourcenanbieteradapter verweisen.

5. Der Administrator muss alle Kontingente löschen, die dem SQL-Ressourcenanbieteradapter zugewiesen sind.

6. Führen Sie das Bereitstellungsskript mit dem Parameter „-Uninstall“, Azure Resource Manager-Endpunkten, DirectoryTenantID und Anmeldeinformationen für das Dienstadministratorkonto erneut aus.


## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Hostservern](azure-stack-sql-resource-provider-hosting-servers.md) und [Erstellen von Datenbanken](azure-stack-sql-resource-provider-databases.md).

Probieren Sie andere [PaaS-Dienste](azure-stack-tools-paas-services.md) wie den [MySQL Server-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md) und den [App Services-Ressourcenanbieter](azure-stack-app-service-overview.md) aus.
