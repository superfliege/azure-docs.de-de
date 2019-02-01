---
title: PowerShell-Cmdlets für Azure SQL Data Warehouse
description: Hier finden Sie die wichtigsten PowerShell-Cmdlets für Azure SQL Data Warehouse, einschließlich der Cmdlets zum Anhalten und Fortsetzen einer Datenbank.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 412c2faead0102d3995e3df9db31b81c4dd4aa22
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55452004"
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-Cmdlets und REST-APIs für SQL Data Warehouse
Viele SQL Data Warehouse-Verwaltungsaufgaben können entweder mithilfe von Azure PowerShell-Cmdlets oder REST-APIs verwaltet werden.  Die folgenden Beispiele zeigen einen Einblick, wie PowerShell-Befehle verwendet werden, um häufige Aufgaben in SQL Data Warehouse zu automatisieren.  Einige gute REST-Beispiele finden Sie im Artikel [Verwalten der Skalierbarkeit mit REST][Manage scalability with REST].

> [!NOTE]
> Damit Sie Azure Powershell mit SQL Data Warehouse verwenden können, benötigen Sie Azure Powershell Version 1.0.3 oder höher.  Sie können die Version überprüfen, indem Sie **Get-Module -ListAvailable -Name Azure**ausführen.  Sie können die neueste Version über den [Microsoft-Webplattform-Installer][Microsoft Web Platform Installer] installieren.  Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][How to install and configure Azure PowerShell].
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Erste Schritte mit Azure PowerShell-Cmdlets
1. Öffnen Sie Windows PowerShell.
2. Zum Anmelden bei Azure Resource Manager und Auswählen Ihres Abonnements führen Sie bei der PowerShell-Eingabeaufforderung die folgenden Befehle aus:
   
    ```PowerShell
    Connect-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Beispiel für das Anhalten von Azure SQL Data Warehouse
Halten Sie eine Datenbank mit dem Namen „Database02“ an, die auf einem Server mit dem Namen „Server01“ gehostet wird.  Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Variation: Dieses Beispiel leitet das abgerufene Objekt an [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase] weiter.  Dadurch wird die Datenbank angehalten. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Beispiel für das Starten von Azure SQL Data Warehouse
Setzen Sie den Betrieb der auf dem Server „Server01“ gehosteten Datenbank „Database02“ fort. Der Server befindet sich in einer Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Abweichung: Eine Datenbank „Database02“ von einem Server „Server01“, der sich in einer Ressourcengruppe „ResourceGroup1“ befindet, wird abgerufen. Das abgerufene Objekt wird an [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase] weitergeleitet.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Hinweis: Wenn Ihr Server „foo.database.windows.net“ heißt, verwenden Sie in den PowerShell-Cmdlets für „-ServerName“ den Namen „foo“.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Andere unterstützte PowerShell-Cmdlets
Diese PowerShell-Cmdlets werden für Azure SQL Data Warehouse unterstützt.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase]
* [Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [Select-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

## <a name="next-steps"></a>Nächste Schritte
Weitere PowerShell-Beispiele finden Sie unter:

* [Erstellen von SQL Data Warehouse mithilfe von PowerShell][Create a SQL Data Warehouse using PowerShell]
* [Datenbankwiederherstellung][Database restore]

Weitere Aufgaben, die mit PowerShell automatisiert werden können, finden Sie unter [Azure SQL-Datenbank-Cmdlets][Azure SQL Database Cmdlets]. Beachten Sie, dass nicht alle Azure SQL-Datenbank-Cmdlets für Azure SQL Data Warehouse unterstützt werden.  Eine Liste mit Aufgaben, die mit REST automatisiert werden können, finden Sie unter [Vorgänge für Azure SQL-Datenbank][Operations for Azure SQL Database].

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./create-data-warehouse-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.sql
[Operations for Azure SQL Database]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabase
[Get-AzureRmSqlDeletedDatabaseBackup]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzureRmSqlDeletedDatabaseBackup
[Get-AzureRmSqlDatabaseRestorePoints]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzureRmSqlDatabaseRestorePoints
[New-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/New-AzureRmSqlDatabase
[Remove-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzureRmSqlDatabase
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Restore-AzureRmSqlDatabase
[Resume-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Resume-AzureRmSqlDatabase
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzureRmSqlDatabase
[Suspend-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Suspend-AzureRmSqlDatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
