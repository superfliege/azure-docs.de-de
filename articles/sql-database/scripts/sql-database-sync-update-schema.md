---
title: 'PowerShell-Beispiel: Aktualisieren des Synchronisierungsschemas für die SQL-Datensynchronisierung (Vorschauversion) | Microsoft-Dokumentation'
description: Azure PowerShell-Beispielskript zum Aktualisieren des Synchronisierungsschemas für die SQL-Datensynchronisierung
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: ''
tags: ''
ms.assetid: ''
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 01/10/2018
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: 5f9f0a5921951669d451a9755c5da2091287c909
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Verwenden von PowerShell zum Aktualisieren des Synchronisierungsschemas in einer bestehenden Synchronisierungsgruppe

In diesem PowerShell-Beispiel wird das Synchronisierungsschema in einer vorhandenen Synchronisierungsgruppe der SQL-Datensynchronisierung (Vorschauversion) aktualisiert. Wenn Sie mehrere Tabellen synchronisieren, hilft Ihnen dieses Skript beim effizienten Aktualisieren des Synchronisierungsschemas.

Dieses Beispiel veranschaulicht die Verwendung des Skripts **UpdateSyncSchema**, das auf GitHub als [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1) verfügbar ist.

Eine Übersicht über die SQL-Datensynchronisierung finden Sie unter [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](../sql-database-sync-data.md).
## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel ist das Azure PowerShell-Modul Version 4.2 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.
 
Führen Sie zum Starten `Login-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen.

## <a name="examples"></a>Beispiele

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Beispiel 1: Hinzufügen aller Tabellen zum Synchronisierungsschema

Im folgenden Beispiel wird das Datenbankschema aktualisiert, und dem Synchronisierungsschema werden alle gültigen Tabellen in der Hub-Datenbank hinzugefügt.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Beispiel 2: Hinzufügen und Entfernen von Tabellen und Spalten

Im folgenden Beispiel werden dem Synchronisierungsschema `[dbo].[Table1]` und `[dbo].[Table2].[Column1]` hinzugefügt und `[dbo].[Table3]` entfernt.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Skriptparameter

Das Skript **UpdateSyncSchema** weist die folgenden Parameter auf:

| Parameter | Notizen |
|---|---|
| $SubscriptionId | Das Abonnement, in dem die Synchronisierungsgruppe erstellt wird |
| $ResourceGroupName | Die Ressourcengruppe, in der die Synchronisierungsgruppe erstellt wird|
| $ServerName | Der Servername der Hub-Datenbank|
| $DatabaseName | Der Name der Hub-Datenbank |
| $SyncGroupName | Der Name der Synchronisierungsgruppe |
| $MemberName | Geben Sie den Namen des Elements an, wenn Sie das Datenbankschema aus dem Synchronisierungselement und nicht aus der Hub-Datenbank laden möchten. Wenn Sie das Datenbankschema vom Hub laden möchten, lassen Sie diesen Parameter leer. |
| $TimeoutInSeconds | Das Timeout, nach dem das Skript das Datenbankschema aktualisiert. Der Standardwert ist 900 Sekunden. |
| $RefreshDatabaseSchema | Geben Sie an, ob das Skript das Datenbankschema aktualisieren muss. Wenn das Datenbankschema gegenüber der früheren Konfiguration geändert wurde (z.B. wenn Sie eine neue Tabelle oder Spalte hinzugefügt haben), müssen Sie das Schema aktualisieren, bevor Sie es neu konfigurieren. Die Standardeinstellung ist "false". |
| $AddAllTables | Wenn dieser Wert TRUE ist, werden dem Synchronisierungsschema alle gültigen Tabellen und Spalten hinzugefügt. Die Werte von $TablesAndColumnsToAdd und $TablesAndColumnsToRemove werden ignoriert. |
| $TablesAndColumnsToAdd | Geben Sie Tabellen oder Spalten an, die dem Synchronisierungsschema hinzugefügt werden sollen. Jeder Name einer Tabelle oder Spalte muss vollständig vom Schemanamen getrennt werden. Beispiel: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Bei Angabe von mehreren Tabellen- oder Spaltennamen können diese durch Kommas (,) voneinander getrennt werden. |
| $TablesAndColumnsToRemove | Geben Sie Tabellen oder Spalten an, die aus dem Synchronisierungsschema entfernt werden sollen. Jeder Name einer Tabelle oder Spalte muss vollständig vom Schemanamen getrennt werden. Beispiel: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Bei Angabe von mehreren Tabellen- oder Spaltennamen können diese durch Kommas (,) voneinander getrennt werden. |
|||

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript **UpdateSyncSchema** verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Gibt Informationen zu einer Synchronisierungsgruppe zurück |
| [Update-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Aktualisiert eine Synchronisierungsgruppe |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Gibt Informationen zu einem Synchronisierungselement zurück |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Gibt Informationen zu einem Synchronisierungsschema zurück |
| [Update-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Aktualisiert ein Synchronisierungsschema |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank](../sql-database-powershell-samples.md).

Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter:

-   [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](../sql-database-sync-data.md)
-   [Einrichten von Azure SQL-Datensynchronisierung](../sql-database-get-started-sql-data-sync.md)
-   [Best practices for Azure SQL Data Sync (Preview)](../sql-database-best-practices-data-sync.md) (Bewährte Methoden für die Azure SQL-Datensynchronisierung-Vorschauversion)
-   [Überwachen der Azure SQL-Datensynchronisierung mit OMS Log Analytics](../sql-database-sync-monitor-oms.md)
-   [Troubleshoot issues with SQL Data Sync (Preview)](../sql-database-troubleshoot-data-sync.md) (Behandeln von Problemen mit der Azure SQL-Datensynchronisierung-Vorschauversion)

-   Vollständige PowerShell-Beispiele, die die Konfiguration der SQL-Datensynchronisierung veranschaulichen:
    -   [Verwenden von PowerShell zum Synchronisieren von Daten zwischen mehreren Azure SQL-­Datenbanken](sql-database-sync-data-between-sql-databases.md)
    -   [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](sql-database-sync-data-between-azure-onprem.md)

-   [Download the SQL Data Sync REST API documentation (Herunterladen der Dokumentation zur REST-API von SQL-Datensynchronisierung)](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Weitere Informationen zu SQL-Datenbank finden Sie unter:

-   [Übersicht über die SQL-Datenbank](../sql-database-technical-overview.md)
-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)
