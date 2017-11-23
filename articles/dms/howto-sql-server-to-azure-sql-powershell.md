---
title: Verwenden des Azure Database Migration Service-Moduls in Microsoft Azure PowerShell zum Migrieren von lokalem SQL Server zur Azure SQL-Datenbank | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mit Azure PowerShell eine Migration von lokalem SQL Server zu Azure SQL durchführen."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/10/2017
ms.openlocfilehash: ad6469fcf86aeb7a0076ab5909fbe593596df695
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-db-using-azure-powershell"></a>Migrieren von lokalem SQL Server zur Azure SQL-Datenbank mithilfe von Azure PowerShell
In diesem Artikel migrieren Sie die Datenbank **Adventureworks2012**, die in einer lokalen Instanz von SQL Server 2016 (oder höher) wiederhergestellt wurde, mithilfe von Microsoft Azure PowerShell zu einer Azure SQL-Datenbank.  Mithilfe des Moduls `AzureRM.DataMigration` in Microsoft Azure PowerShell können Sie Datenbanken aus einer lokalen Instanz von SQL Server zur Azure SQL-Datenbank migrieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen eines Migrationsprojekts in einer Instanz von Azure Database Migration Service
> * Ausführen der Migration


## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen dieser Schritte benötigen Sie Folgendes:

- [SQL Server 2016 oder höher](https://www.microsoft.com/sql-server/sql-server-downloads) (beliebige Edition)
- Das TCP/IP-Protokoll ist in einer SQL Server Express-Installation standardmäßig deaktiviert. Aktivieren Sie es anhand der [Anweisungen in diesem Artikel](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Konfigurieren Sie Ihre [Windows-Firewall für Datenbankmodulzugriff](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Eine Azure SQL-Datenbankinstanz. Sie können eine Azure SQL-Datenbankinstanz erstellen, indem Sie die Details im Artikel [Erstellen einer Azure SQL-Datenbank im Azure-Portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) befolgen.
- [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595), Version 3.3 oder höher.
- Für Azure Database Migration Service ist ein VNET erforderlich, das über das Azure Resource Manager-Bereitstellungsmodell erstellt wurde und entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Konnektivität zwischen Standorten für Ihre lokalen Quellserver bietet.
- Abgeschlossene Bewertung der lokalen Datenbank und Schemamigration mithilfe von Data Migration Assistant wie im Artikel [Durchführen einer SQL Server-Migrationsbewertung](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) beschrieben.
- Herunterladen und Installieren des AzureRM.DataMigration-Moduls aus dem PowerShell-Katalog mithilfe des [PowerShell-Cmdlets „Install-Module“](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1).
- Die für die Verbindung mit der SQL Server-Quellinstanz verwendeten Anmeldeinformationen müssen über [SERVER STEUERN](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql)-Berechtigungen verfügen.
- Die für die Verbindung mit der Azure SQL DB-Zielinstanz verwendeten Anmeldeinformationen müssen für die Azure SQL DB-Zieldatenbanken die Berechtigung DATENBANK STEUERN besitzen.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Anmelden bei Ihrem Microsoft Azure-Abonnement
Folgen Sie den Anweisungen im Artikel [Anmelden mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.4.1), um sich mit PowerShell bei Ihrem Azure-Abonnement anzumelden.


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Bevor Sie einen virtuellen Computer erstellen können, müssen Sie eine Ressourcengruppe erstellen.

Erstellen Sie eine Ressourcengruppe mit dem Befehl [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1). 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *EastUS* erstellt.

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-azure-database-migration-service-instance"></a>Erstellen einer Instanz von Azure Database Migration Service 
Mit dem Cmdlet `New-AzureRmDataMigrationService` können Sie eine neue Instanz von Azure Database Migration Service erstellen. Dieses Cmdlet erwartet die folgenden erforderlichen Parameter:
- *ResourceGroupName*: Mit dem Befehl [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) können Sie wie oben gezeigt eine Azure-Ressourcengruppe erstellen und deren Namen als Parameter bereitstellen.
- *ServiceName*: Die Zeichenfolge, die dem gewünschten eindeutigen Dienstnamen für Azure Database Migration Service entspricht. 
- *Location*: Gibt den Standort des Diensts an. Geben Sie einen Azure-Rechenzentrumsstandort an, z. B. „USA, Westen“ oder „Asien, Südosten“.
- *Sku*: Dieser Parameter entspricht dem Sku-Namen des DMS. Derzeit unterstützte Sku-Namen sind *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*.
- *VirtualSubnetId*: Mit dem Cmdlet [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-4.4.1) können Sie ein Subnetz erstellen. 

Im folgenden Beispiel wird ein Dienst mit dem Namen *MyDMS* in der Ressourcengruppe *MyDMSResourceGroup*, die sich in der Region *East US* befindet, mit einem virtuellen Subnetz namens *MySubnet* erstellt.

```powershell
$service = New-AzureRmDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId
$vnet.Id`
```

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts
Nachdem Sie eine Instanz von Azure Database Migration Service erstellt haben, erstellen Sie ein Migrationsprojekt. Ein Azure Database Migration Service-Projekt erfordert Verbindungsinformationen sowohl für die Quell- als auch die Zielinstanz sowie eine Liste der Datenbanken, die Sie als Teil des Projekts migrieren möchten.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Erstellen eines Objekts mit Datenbankverbindungsinformationen für die Quell- und Zielverbindung
Mit dem Cmdlet `New-AzureRmDmsConnInfo` können Sie ein Objekt mit Datenbankverbindungsinformationen erstellen.  Dieses Cmdlet erwartet die folgenden Parameter:
- *ServerType*: Der Typ der angeforderten Datenbankverbindung, z. B. „SQL“, „Oracle“ oder „MySQL“. Verwenden Sie „SQL“ für SQL Server und SQL Azure.
- *DataSource*: Der Name oder die IP-Adresse einer SQL-Instanz oder eines SQL Azure-Servers.
- *AuthType*: Der Authentifizierungstyp für die Verbindung. Dieser kann „SqlAuthentication“ oder „WindowsAuthentication“ sein.
- *TrustServerCertificate*: Mit diesem Parameter wird ein Wert festgelegt, der angibt, ob der Kanal verschlüsselt wird, während das Durchlaufen der Zertifikatskette zum Überprüfen der Vertrauensstellung umgangen wird. Der Wert kann „true“ oder „false“ sein.

Im folgenden Beispiel wird das Objekt mit Verbindungsinformationen für die SQL Server-Quellinstanz namens „MySQLSourceServer“ mithilfe der SQL-Authentifizierung erstellt. 

```powershell
$sourceConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource MySQLSourceServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Das nächste Beispiel zeigt die Erstellung der Verbindungsinformationen für den SQL Azure-Datenbankserver namens „MySQLAzureTarget“ mithilfe der SQL-Authentifizierung.

```powershell
$targetConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource "mysqlazuretarget.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Bereitstellen von Datenbanken für das Migrationsprojekt
Erstellen Sie eine Liste von `AzureRmDataMigrationDatabaseInfo`-Objekten, in der Datenbanken als Teil des Azure-Datenbankmigrationsprojekts angegeben sind, die als Parameter für die Erstellung des Projekts bereitgestellt werden können. Das Cmdlet `New-AzureRmDataMigrationDatabaseInfo` kann zum Erstellen von „AzureRmDataMigrationDatabaseInfo“ verwendet werden. 

Im folgenden Beispiel wird das Projekt `AzureRmDataMigrationDatabaseInfo` für die Datenbank „AdventureWorks2016“ erstellt und zur Liste hinzugefügt, die als Parameter für die Projekterstellung bereitgestellt wird.

```powershell
$dbInfo1 = New-AzureRmDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks2016
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
  -TargetType SQLDB `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfos $dbList
```

## <a name="create-and-start-a-migration-task"></a>Erstellen und Starten einer Migrationsaufgabe

Zum Schluss erstellen und starten Sie eine Azure-Datenbankmigrationsaufgabe. Eine Azure-Datenbankmigrationsaufgabe erfordert außer den Informationen, die bereits durch das als Voraussetzung erstellte Projekt bereitgestellt werden, die Anmeldeinformationen sowohl für die Quell- als auch die Zielverbindung sowie die Liste der zu migrierenden Datenbanktabellen. 

### <a name="create-credential-parameters-for-source-and-target"></a>Erstellen von Parametern für Anmeldeinformationen für Quelle und Ziel

Sicherheitsanmeldeinformationen für die Verbindung können als [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0)-Objekt erstellt werden. 

Das folgende Beispiel zeigt die Erstellung von *PSCredential*-Objekten sowohl für die Quell- als auch die Zielverbindung, wobei Kennwörter als Zeichenfolgenvariablen *$sourcePassword* und *$targetPassword* bereitgestellt werden. 

```powershell
secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-table-map-and-select-source-and-target-parameters-for-migration"></a>Erstellen einer Tabellenzuordnung und Auswählen der Quell- und Zielparameter für die Migration
Ein weiterer für die Migration erforderlicher Parameter ist eine Zuordnung der Tabellen, die von der Quelle zum Ziel migriert werden sollen. Erstellen Sie ein Wörterbuch der Tabellen, das eine Zuordnung zwischen Quell- und Zieltabellen für die Migration enthält. Das folgende Beispiel veranschaulicht die Zuordnung zwischen Quell- und Zieltabellen des Schemas „Human Resources“ für die Datenbank „AdventureWorks 2016“.

```powershell
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]'
$tableMap.Add("HumanResources.Department", "HumanResources.Department")
$tableMap.Add("HumanResources.Employee","HumanResources.Employee")
$tableMap.Add("HumanResources.EmployeeDepartmentHistory","HumanResources.EmployeeDepartmentHistory")
$tableMap.Add("HumanResources.EmployeePayHistory","HumanResources.EmployeePayHistory")
$tableMap.Add("HumanResources.JobCandidate","HumanResources.JobCandidate")
$tableMap.Add("HumanResources.Shift","HumanResources.Shift")
```

Der nächste Schritt besteht darin, die Quell- und Zieldatenbanken auszuwählen und eine Tabellenzuordnung für die Migration als Parameter mithilfe des Cmdlets `New-AzureRmDmsSqlServerSqlDbSelectedDB` bereitzustellen, wie es im folgenden Beispiel gezeigt wird:

```powershell
$selectedDbs = New-AzureRmDmsSqlServerSqlDbSelectedDB -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -TableMap $tableMap
```

### <a name="create-and-start-a-migration-task"></a>Erstellen und Starten einer Migrationsaufgabe

Verwenden Sie das Cmdlet `New-AzureRmDataMigrationTask` zum Erstellen und Starten einer Migrationsaufgabe. Dieses Cmdlet erwartet die folgenden Parameter:
- *TaskType*:  Als Typ der Migrationsaufgabe, die für die Migration von SQL Server nach SQL Azure zu erstellen ist, wird *MigrateSqlServerSqlDb* erwartet. 
- *ResourceGroupName*: Name der Azure-Ressourcengruppe, in der die Aufgabe erstellt werden soll.
- *ServiceName*:  Instanz von Azure Database Migration Service, in der die Aufgabe erstellt werden soll.
- *ProjectName*: Name des Azure-Datenbankmigrationsprojekts, in dem die Aufgabe erstellt werden soll. 
- *TaskName*: Name der zu erstellenden Aufgabe. 
- *SourceConnection*: Das „AzureRmDmsConnInfo“-Objekt, das die Quellverbindung darstellt.
- *TargetConnection*: Das „AzureRmDmsConnInfo“-Objekt, das die Zielverbindung darstellt.
- *SourceCred*: Das [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0)-Objekt für die Verbindung mit dem Quellserver.
- *TargetCred*: Das [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0)-Objekt für die Verbindung mit dem Zielserver.
- *SelectedDatabase*: Das „AzureRmDmsSqlServerSqlDbSelectedDB“-Objekt, das die Quell- und Zieldatenbankzuordnung darstellt.

Im folgenden Beispiel wird eine Migrationsaufgabe mit dem Namen „MyDMSTask“ erstellt und gestartet:

```powershell
$migTask = New-AzureRmDataMigrationTask -TaskType MigrateSqlServerSqlDb `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs`
```

## <a name="monitor-the-migration"></a>Überwachen der Migration
Sie können die Ausführung der Migrationsaufgabe überwachen, indem Sie die Zustandseigenschaft der Aufgabe abfragen, wie es im folgenden Beispiel gezeigt wird:

```powershell
if (($task.Properties.State -eq "Running") -or ($task.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="next-steps"></a>Nächste Schritte
- Überprüfen der Migrationsanleitung im [Leitfaden zur Datenbankmigration von Microsoft](https://datamigration.microsoft.com/)