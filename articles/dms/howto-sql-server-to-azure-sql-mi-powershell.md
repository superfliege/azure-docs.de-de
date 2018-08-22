---
title: Verwenden des Azure Database Migration Service-Moduls in Microsoft Azure PowerShell zum Migrieren von lokalem SQL Server zur Azure SQL DB MI | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure PowerShell eine Migration von lokalem SQL Server zu Azure SQL DB MI durchführen.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 08/13/2018
ms.openlocfilehash: 7bd7e7a4cb78cf8a9f818936c980b47a2e7865e7
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2018
ms.locfileid: "40099705"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-db-using-azure-powershell"></a>Migrieren von lokalem SQL Server zur Azure SQL-Datenbank mithilfe von Azure PowerShell
In diesem Artikel migrieren Sie die Datenbank **Adventureworks2012**, die in einer lokalen Instanz von SQL Server 2005 (oder höher) wiederhergestellt wurde, mithilfe von Microsoft Azure PowerShell zu einer Azure SQL-Datenbank. Mithilfe des Moduls `AzureRM.DataMigration` in Microsoft Azure PowerShell können Sie Datenbanken aus einer lokalen Instanz von SQL Server zur Azure SQL-Datenbank migrieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
> * Erstellen Sie eine Ressourcengruppe.
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen eines Migrationsprojekts in einer Instanz von Azure Database Migration Service
> * Ausführen der Migration

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen dieser Schritte benötigen Sie Folgendes:

- [SQL Server 2016 oder höher](https://www.microsoft.com/sql-server/sql-server-downloads) (beliebige Edition)
- Aktivieren Sie das TCP/IP-Protokoll, das in einer SQL Server Express-Installation standardmäßig deaktiviert ist. Aktivieren Sie das TCP/IP-Protokoll anhand des Artikels [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Konfigurieren Sie Ihre [Windows-Firewall für Datenbank-Engine-Zugriff](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Eine Azure SQL-Datenbankinstanz. Sie können eine Azure SQL-Datenbankinstanz erstellen, indem Sie die Details im Artikel [Erstellen einer Azure SQL-Datenbank im Azure-Portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) befolgen.
- [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595), Version 3.3 oder höher.
- Erstellen Sie ein VNET mithilfe des Azure Resource Manager-Bereitstellungsmodells, das den Azure Database Migration Service mit Konnektivität zwischen Standorten für Ihre lokalen Quellserver entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) bietet.
- Schließen Sie die Bewertung der lokalen Datenbank und Schemamigration mithilfe von Data Migration Assistant ab, wie im Artikel [Durchführen einer SQL Server-Migrationsbewertung](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) beschrieben.
- Laden Sie das AzureRM.DataMigration-Modul aus dem PowerShell-Katalog mit dem [PowerShell-Cmdlets „Install-Module“](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1) herunter und installieren Sie es.
- Stellen Sie sicher, dass die für die Verbindung mit der SQL Server-Quellinstanz verwendeten Anmeldeinformationen über [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql)-Berechtigungen verfügen.
- Stellen Sie sicher, dass die für die Verbindung zur Azure SQL DB-Zielinstanz verwendeten Anmeldeinformationen für die Azure SQL-Datenbank-Zieldatenbanken über die Berechtigung CONTROL DATABASE-Berechtigung verfügen.
- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses](https://azure.microsoft.com/free/) Konto erstellen, bevor Sie beginnen.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Anmelden bei Ihrem Microsoft Azure-Abonnement
Folgen Sie den Anweisungen im Artikel [Anmelden mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.4.1), um sich mit PowerShell bei Ihrem Azure-Abonnement anzumelden.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Bevor Sie einen virtuellen Computer erstellen können, müssen Sie eine Ressourcengruppe erstellen.

Erstellen Sie eine Ressourcengruppe mit dem Befehl [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1). 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *EastUS* erstellt.

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```
## <a name="create-an-instance-of-the-azure-database-migration-service"></a>Erstellen einer Instanz von Azure Database Migration Service 
Mit dem Cmdlet `New-AzureRmDataMigrationService` können Sie eine neue Instanz von Azure Database Migration Service erstellen. Dieses Cmdlet erwartet die folgenden erforderlichen Parameter:
- *ResourceGroupName*: Mit dem Befehl [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) können Sie wie oben gezeigt eine Azure-Ressourcengruppe erstellen und deren Namen als Parameter bereitstellen.
- *ServiceName*: Die Zeichenfolge, die dem gewünschten eindeutigen Dienstnamen für Azure Database Migration Service entspricht. 
- *Standort*: Gibt den Standort des Diensts an. Geben Sie einen Azure-Rechenzentrumsstandort an, z. B. „USA, Westen“ oder „Asien, Südosten“.
- *Sku*: Dieser Parameter entspricht dem Sku-Namen des DMS. Derzeit unterstützte Sku-Namen sind *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*.
- *VirtualSubnetId*: Mit dem Cmdlet [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-4.4.1) können Sie ein Subnetz erstellen. 

Im folgenden Beispiel wird ein Dienst mit dem Namen *MyDMS* in der Ressourcengruppe *MyDMSResourceGroup*, die sich in der Region *USA, Osten* befindet, mit einem virtuellen Netzwerk namens *MyVNET* und einem Subnetz *MySubnet* erstellt.

```powershell
 $vNet = Get-AzureRmVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzureRmDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts
Nachdem Sie eine Instanz von Azure Database Migration Service erstellt haben, erstellen Sie ein Migrationsprojekt. Ein Azure Database Migration Service-Projekt erfordert Verbindungsinformationen sowohl für die Quell- als auch die Zielinstanz sowie eine Liste der Datenbanken, die Sie als Teil des Projekts migrieren möchten.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Erstellen eines Objekts mit Datenbankverbindungsinformationen für die Quell- und Zielverbindung
Mit dem Cmdlet `New-AzureRmDmsConnInfo` können Sie ein Objekt mit Datenbankverbindungsinformationen erstellen. Dieses Cmdlet erwartet die folgenden Parameter:
- *ServerType*: Der Typ der angeforderten Datenbankverbindung, z. B. „SQL“, „Oracle“ oder „MySQL“. Verwenden Sie „SQL“ für SQL Server und Azure SQL.
- *DataSource*: Der Name oder die IP-Adresse einer SQL Server-Instanz oder einer Azure SQL-Datenbank.
- *AuthType*: Der Authentifizierungstyp für die Verbindung. Dieser kann „SqlAuthentication“ oder „WindowsAuthentication“ sein.
- *TrustServerCertificate*: Mit diesem Parameter wird ein Wert festgelegt, der angibt, ob der Kanal verschlüsselt wird, während das Durchlaufen der Zertifikatskette zum Überprüfen der Vertrauensstellung umgangen wird. Der Wert kann „true“ oder „false“ sein.

Im folgenden Beispiel wird das Objekt mit Verbindungsinformationen für die SQL Server-Quellinstanz namens „MySourceSQLServer“ mithilfe der SQL-Authentifizierung erstellt. 

```powershell
$sourceConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Das nächste Beispiel zeigt die Erstellung der Verbindungsinformationen für einen verwalteten Azure SQL-Datenbank-Instanzserver namens „targetmanagedinstance.database.windows.net“ mithilfe der SQL-Authentifizierung:

```powershell
$targetConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource "targetmanagedinstance.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Bereitstellen von Datenbanken für das Migrationsprojekt
Erstellen Sie eine Liste von `AzureRmDataMigrationDatabaseInfo`-Objekten, in der Datenbanken als Teil des Azure-Datenbankmigrationsprojekts angegeben sind, die als Parameter für die Erstellung des Projekts bereitgestellt werden können. Das Cmdlet `New-AzureRmDataMigrationDatabaseInfo` kann zum Erstellen von „AzureRmDataMigrationDatabaseInfo“ verwendet werden. 

Im folgenden Beispiel wird das Projekt `AzureRmDataMigrationDatabaseInfo` für die Datenbank **AdventureWorks** erstellt und zur Liste hinzugefügt, die als Parameter für die Projekterstellung bereitgestellt wird.

```powershell
$dbInfo1 = New-AzureRmDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Erstellen eines Projektobjekts
Abschließend können Sie mit `New-AzureRmDataMigrationProject` ein Azure-Datenbankmigrationsprojekt mit Namen *MyDMSProject* erstellen, das sich in *East US* befindet, und die zuvor erstellte Quell- und Zielverbindung sowie die Liste der zu migrierenden Datenbanken hinzufügen.

```powershell
$project = New-AzureRmDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLMI `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Erstellen und Starten einer Migrationsaufgabe
Zum Schluss erstellen und starten Sie eine Azure-Datenbankmigrationsaufgabe. Eine Azure-Datenbankmigrationsaufgabe erfordert außer den Informationen, die bereits durch das als Voraussetzung erstellte Projekt bereitgestellt werden, die Anmeldeinformationen sowohl für die Quell- als auch die Zielverbindung sowie die Liste der zu migrierenden Datenbanktabellen. 

### <a name="create-credential-parameters-for-source-and-target"></a>Erstellen von Parametern für Anmeldeinformationen für Quelle und Ziel
Sicherheitsanmeldeinformationen für die Verbindung können als ein [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0)-Objekt erstellt werden. 

Das folgende Beispiel zeigt die Erstellung von *PSCredential*-Objekten sowohl für die Quell- als auch die Zielverbindung, wobei Kennwörter als Zeichenfolgenvariablen *$sourcePassword* und *$targetPassword* bereitgestellt werden. 

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-backup-fileshare-object"></a>Erstellen eines Sicherungs-FileShare-Objekts
Erstellen Sie nun ein FileShare-Objekt, das die lokale SMB-Netzwerkfreigabe darstellt, die der Azure Database Migration Service mit dem Cmdlet „New-AzureRmDmsFileShare“ ausführen kann.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzureRmDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Erstellen eines ausgewählten Datenbankobjekts
Der nächste Schritt besteht darin, die Quell- und Zieldatenbanken mithilfe des Cmdlets „New-AzureRmDmsSelectedDB“ bereitzustellen, wie im folgenden Beispiel gezeigt:

```powershell
$selectedDbs = @()
$selectedDbs += New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

### <a name="sas-uri-for-azure-storage-container"></a>SAS-URI für Azure-Speichercontainer
Erstellen Sie eine Variable, die den SAS-URI enthält, mit dem für den Azure Database Migration Service der Zugriff auf den Speicherkontocontainer gewährt wird, in den der Dienst die Sicherungsdateien hochlädt.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

### <a name="select-logins"></a>Auswählen von Benutzernamen
Erstellen Sie eine Liste der zu migrierenden Anmeldungen, wie im folgenden Beispiel gezeigt:  Beachten Sie, dass DMS derzeit nur die Migration von SQL-Anmeldung unterstützt. 

```powershell
$selectedLogins = @("user1", "user2")
```

### <a name="select-agent-jobs"></a>Auswählen von Agent-Aufträgen
Erstellen Sie eine Liste der zu migrierenden Agent-Aufträge, wie im folgenden Beispiel gezeigt:

>[!NOTE]
>DMS unterstützt derzeit nur die Aufträge mit Auftragsschritten des T-SQL-Subsystems.

```powershell
$selectedAgentJobs = @("agentJob1", "agentJob2")
```

### <a name="create-and-start-a-migration-task"></a>Erstellen und Starten einer Migrationsaufgabe

Verwenden Sie das Cmdlet `New-AzureRmDataMigrationTask` zum Erstellen und Starten einer Migrationsaufgabe. Dieses Cmdlet erwartet die folgenden Parameter:
- *TaskType*: Als Typ der Migrationsaufgabe, die für die Migration von SQL Server zur verwalteten Azure SQL-Datenbankinstanz zu erstellen ist, wird *MigrateSqlServerSqlDbMi* erwartet. 
- *ResourceGroupName*: Name der Azure-Ressourcengruppe, in der die Aufgabe erstellt werden soll.
- *ServiceName*: Instanz von Azure Database Migration Service, in der die Aufgabe erstellt werden soll.
- *ProjectName*: Name des Azure Database Migration Service-Projekts, in dem die Aufgabe erstellt werden soll. 
- *TaskName*: Name der zu erstellenden Aufgabe. 
- *SourceConnection*. Das „AzureRmDmsConnInfo“-Objekt, das die SQL Server-Quellverbindung darstellt.
- *TargetConnection*. Das „AzureRmDmsConnInfo“-Objekt, das die verwaltete Azure SQL-Datenbankinstanz-Zielverbindung darstellt.
- *SourceCred*: Das [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0)-Objekt für die Verbindung mit dem Quellserver.
- *TargetCred*: Das [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0)-Objekt für die Verbindung mit dem Zielserver.
- *SelectedDatabase*: Das „AzureRmDataMigrationSelectedDB“-Objekt, das die Quell- und Zieldatenbankzuordnung darstellt.
- *BackupFileShare*. FileShare-Objekt, das die lokale Netzwerkfreigabe darstellt, auf die Azure Database Migration Service die Quelldatenbanksicherungen übertragen kann.
- *BackupBlobSasUri*. Der SAS-URI, mit dem für den Azure Database Migration Service der Zugriff auf den Speicherkontocontainer gewährt wird, in den der Dienst die Sicherungsdateien hochlädt. Erfahren Sie, wie der SAS-URI für Blobcontainer abgerufen wird.
- *SelectedLogins*. Liste der ausgewählten zu migrierenden Anmeldungen.
- *SelectedAgentJobs*. Liste der ausgewählten zu migrierenden Agent-Aufträge.

Im folgenden Beispiel wird eine Migrationsaufgabe mit dem Namen „MyDMSTask“ erstellt und gestartet:

```powershell
$migTask = New-AzureRmDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs`
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```

## <a name="monitor-the-migration"></a>Überwachen der Migration
Sie können die Ausführung der Migrationsaufgabe überwachen, indem Sie die Zustandseigenschaft der Aufgabe abfragen, wie es im folgenden Beispiel gezeigt wird:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="next-steps"></a>Nächste Schritte
- Überprüfen der Migrationsanleitung im [Leitfaden zur Datenbankmigration von Microsoft](https://datamigration.microsoft.com/)