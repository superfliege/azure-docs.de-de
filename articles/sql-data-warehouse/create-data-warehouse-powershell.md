---
title: 'Schnellstart: Erstellen eines Azure SQL Data Warehouse: Azure PowerShell | Microsoft Docs'
description: Erstellen Sie schnell einen logischen SQL-Datenbankserver, eine Firewallregel auf Serverebene und ein Data Warehouse mit Azure PowerShell.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/01/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 72ed9e921d96faea155c1da88dd32fcbd467d549
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413999"
---
# <a name="quickstart-create-and-query-an-azure-sql-data-warehouse-with-azure-powershell"></a>Schnellstart: Erstellen und Abfragen eines Azure SQL Data Warehouse mit Azure PowerShell

Erstellen Sie schnell ein Azure SQL Data Warehouse mit Azure PowerShell.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Für dieses Tutorial ist das Azure PowerShell-Modul Version 5.1.1 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um zu ermitteln, welche Version Sie verwenden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu. 


> [!NOTE]
> Wenn Sie eine SQL Data Warehouse-Instanz erstellen, wird unter Umständen auch ein neuer abrechenbarer Dienst erstellt.  Weitere Informationen finden Sie unter [SQL Data Warehouse – Preise](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>
>

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) an Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Add-AzureRmAccount
```

Verwenden Sie [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription), um zu ermitteln, welches Abonnement Sie verwenden.

```powershell
Get-AzureRmSubscription
```

Wenn Sie ein anderes Abonnement als das Standardabonnement verwenden müssen, führen Sie [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription) aus.

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```


## <a name="create-variables"></a>Erstellen von Variablen

Definieren Sie Variablen zur Verwendung in den Skripts dieser Schnellstartanleitung.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# The login information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDataWarehosue"
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope` erstellt.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Erstellen eines logischen Servers

Erstellen Sie mit dem Befehl [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) einen [logischen Azure SQL-Server](../sql-database/sql-database-logical-servers.md). Ein logischer Server enthält eine Gruppe von Datenbanken, die als Gruppe verwaltet werden. Im folgenden Beispiel wird in Ihrer Ressourcengruppe ein zufällig benannter Server mit einem Administrator namens `ServerAdmin` und dem Kennwort `ChangeYourAdminPassword1` erstellt. Ersetzen Sie ggf. diese vordefinierten Werte.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurieren einer Serverfirewallregel

Erstellen Sie mit dem Befehl [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) eine [Firewallregel für Azure SQL auf Serverebene](../sql-database/sql-database-firewall-configure.md). Eine Firewallregel auf Serverebene lässt zu, dass eine externe Anwendung wie SQL Server Management Studio oder das SQLCMD-Hilfsprogramm über die Firewall des SQL Data Warehouse-Diensts eine Verbindung mit einem SQL Data Warehouse herstellt. Im folgenden Beispiel wird die Firewall nur für andere Azure-Ressourcen geöffnet. Ändern Sie die IP-Adresse in eine für Ihre Umgebung geeignete Adresse, um die externe Konnektivität zu ermöglichen. Verwenden Sie 0.0.0.0 als IP-Startadresse und 255.255.255.255 als Endadresse, wenn Sie alle IP-Adressen öffnen möchten.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> Die SQL-Datenbank und SQL Data Warehouse kommunizieren über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Server herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>


## <a name="create-a-data-warehouse-with-sample-data"></a>Erstellen eines Data Warehouse mit Beispieldaten
Dieses Beispiel erstellt ein Data Warehouse mit den zuvor definierten Variablen.  Es gibt das Dienstziel als DW400 an. Dies ist ein kostengünstiger Ausgangspunkt für Ihr Data Warehouse. 

```Powershell
New-AzureRmSqlDatabase `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -Edition "DataWarehouse" `
    -RequestedServiceObjectiveName "DW400" `
    -CollationName "SQL_Latin1_General_CP1_CI_AS" `
    -MaxSizeBytes 10995116277760
```

Erforderliche Parameter:

* **RequestedServiceObjectiveName**: die Menge der angeforderten [Data Warehouse-Einheiten](what-is-a-data-warehouse-unit-dwu-cdwu.md). Durch das Erhöhen dieses Werts steigen die Computekosten. Eine Liste der unterstützten Werte finden Sie unter [Grenzwerte für Arbeitsspeicher und Parallelität](memory-and-concurrency-limits.md).
* **DatabaseName**: der Name des SQL Data Warehouse, das Sie erstellen.
* **ServerName**: der Name des Servers, den Sie für die Erstellung verwenden.
* **ResourceGroupName**: die Ressourcengruppe, die Sie verwenden. Verwenden Sie zum Abrufen der in Ihrem Abonnement verfügbaren Ressourcengruppen das Cmdlet „Get-AzureResource“.
* **Edition**: Muss für die Erstellung einer SQL Data Warehouse-Instanz auf „Data Warehouse“ festgelegt werden.

Optionale Parameter:

- **CollationName**: Ohne Angabe wird die Standardsortierung „SQL_Latin1_General_CP1_CI_AS“ verwendet. Die Sortierung kann für eine Datenbank nicht geändert werden.
- **MaxSizeBytes**: Die maximale Größe einer Datenbank beträgt standardmäßig 10 GB.

Weitere Informationen zu den Parameteroptionen finden Sie unter [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase).


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstarttutorials in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. 

> [!TIP]
> Wenn Sie planen, mit den nachfolgenden Schnellstarttutorials fortzufahren, sollten Sie die in dieser Schnellstartanleitung erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt ein Data Warehouse sowie eine Firewallregel erstellt, diese mit Ihrem Data Warehouse verbunden und einige Abfragen ausgeführt. Weitere Informationen zu Azure SQL Data Warehouse erhalten Sie im Tutorial zum Laden von Daten.
> [!div class="nextstepaction"]
>[Laden von Daten in ein SQL-Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
