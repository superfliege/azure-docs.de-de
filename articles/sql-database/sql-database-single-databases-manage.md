---
title: Erstellen und Verwalten von Servern und Einzeldatenbanken in Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Erstellen und Verwalten von logischen Servern und Einzeldatenbanken.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/19/2018
ms.openlocfilehash: ac3ce8a417a63ce0a91339c38c2a5ae8b3013228
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466782"
---
# <a name="create-and-manage-logical-servers-and-single-databases-in-azure-sql-database"></a>Erstellen und Verwalten von logischen Servern und Einzeldatenbanken in Azure SQL-Datenbank

Sie können logische Server und einzelne Datenbanken in Azure SQL-Datenbank mit dem Azure-Portal, mit PowerShell, der Azure CLI, der REST-API oder Transact-SQL erstellen und verwalten.

## <a name="azure-portal-manage-logical-servers-and-databases"></a>Azure-Portal: Verwalten von logischen Servern und Datenbanken

Sie können die Ressourcengruppe der Azure SQL-Datenbank im Voraus oder beim Erstellen des Servers selbst erstellen. Es existieren mehrere Methoden zum Erhalten eines Formulars für einen neuen SQL-Server. Sie erhalten ein solches entweder durch das Erstellen eines neuen SQL-Servers oder als Teil der Erstellung einer neuen Datenbank.

### <a name="create-a-blank-sql-server-logical-server"></a>Erstellen eines leeren SQL-Servers (logischen Servers)

Um mithilfe des [Azure-Portals](https://portal.azure.com) einen Azure SQL-Datenbankserver (ohne eine Datenbank) zu erstellen, navigieren Sie zu einem leeren SQL-Server-Formular (logischer Server).  

### <a name="create-a-blank-or-sample-sql-database"></a>Erstellen einer leeren oder einer Beispiel-SQL-Datenbank

Zum Erstellen einer Azure SQL-Datenbank mithilfe des [Azure-Portals](https://portal.azure.com) navigieren Sie zu einem leeren SQL-Datenbank-Formular, und stellen Sie die erforderlichen Informationen bereit. Sie können die Ressourcengruppe und den logischen Server der Azure SQL-Datenbank im Voraus oder beim Erstellen der Datenbank selbst erstellen. Sie können eine leere Datenbank oder eine Beispieldatenbank basierend auf Adventure Works LT erstellen.

  ![Datenbankerstellung 1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Informationen zum Auswählen des Tarifs für Ihre Datenbank finden Sie unter [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) und [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md).

Informationen zum Erstellen einer verwalteten Instanz finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md)

## <a name="manage-an-existing-sql-server"></a>Verwalten eines vorhandenen SQL-Servers

Zum Verwalten eines vorhandenen Servers navigieren Sie mithilfe einer Reihe von Methoden zum Server, z.B. über eine bestimmte Seite der SQL-Datenbank, die Seite der **SQL-Server** oder die Seite **All resources** (Alle Ressourcen).

Um eine vorhandene Datenbank zu verwalten, navigieren Sie zu der Seite **SQL-Datenbanken**, und klicken Sie auf die Datenbank, die Sie verwalten möchten. Der folgende Screenshot zeigt, wie Sie beginnen, über die Seite **Übersicht** für eine Datenbank eine Firewall auf Serverebene für eine Datenbank festzulegen.

   ![Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png)

> [!IMPORTANT]
> Informationen zum Konfigurieren der Leistungseigenschaften für eine Datenbank finden Sie unter [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md) und [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md).
> [!TIP]
> Eine Schnellstartanleitung zum Azure-Portal finden Sie unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](sql-database-get-started-portal.md).

## <a name="powershell-manage-logical-servers-and-databases"></a>PowerShell: Verwalten von logischen Servern und Datenbanken

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe von Azure PowerShell die folgenden PowerShell-Cmdlets. Wenn Sie PowerShell installieren oder aktualisieren müssen, helfen Ihnen die Informationen unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) weiter.

> [!TIP]
> Eine Schnellstartanleitung zu PowerShell finden Sie unter [Erstellen einer einzelnen Azure SQL-Datenbank mithilfe von PowerShell](sql-database-get-started-portal.md). PowerShell-Beispielskripts finden Sie unter [Verwenden von PowerShell zum Erstellen einer einzelnen Azure SQL-Datenbank und Konfigurieren einer Firewallregel](scripts/sql-database-create-and-configure-database-powershell.md) und [Überwachen und Skalieren einer einzelnen SQL-Datenbank mithilfe von PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

| Cmdlet | BESCHREIBUNG |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Erstellt eine Datenbank |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Ruft mindestens eine Datenbank ab|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Legt Eigenschaften für eine Datenbank fest oder verschiebt eine vorhandene Datenbank in einen Pool für elastische Datenbanken|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Entfernt eine Datenbank|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Erstellt eine Ressourcengruppe|
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Erstellt einen Server|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Gibt Informationen zu Servern zurück|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Ändert die Eigenschaften eines Servers|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Entfernt einen Server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Erstellt eine Firewallregel auf Serverebene |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Ruft Firewallregeln für einen Server ab|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Ändert eine Firewallregel auf einem Server|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Löscht eine Firewallregel von einem Server|
| New-AzureRmSqlServerVirtualNetworkRule | Erstellt eine [*virtuelle Netzwerkregel*](sql-database-vnet-service-endpoint-rule-overview.md), die auf einem Subnetz basiert, das einen Dienstendpunkt für ein virtuelles Netzwerk darstellt. |

## <a name="azure-cli-manage-logical-servers-and-databases"></a>Azure CLI: Verwalten von logischen Servern und Datenbanken

Verwenden Sie zum Erstellen und Verwalten von Servern, Datenbanken und Firewalls in Azure SQL-Datenbank mithilfe der [Azure CLI](/cli/azure) die folgenden [Azure CLI-SQL-Datenbank](/cli/azure/sql/db)-Befehle. Führen Sie die CLI mithilfe von [Cloud Shell](/azure/cloud-shell/overview) in Ihrem Browser aus, oder [installieren](/cli/azure/install-azure-cli) Sie sie unter macOS, Linux oder Windows. Informationen zum Erstellen und Verwalten von Pools für elastische Datenbanken finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

> [!TIP]
> Eine Schnellstartanleitung zur Azure CLI finden Sie unter [Erstellen einer einzelnen Azure SQL-Datenbank mithilfe der Azure CLI](sql-database-cli-samples.md). Azure CLI-Beispielskripts finden Sie unter [Verwenden der Befehlszeilenschnittstelle zum Erstellen einer einzelnen Azure SQL-Datenbank und Konfigurieren einer Firewallregel](scripts/sql-database-create-and-configure-database-cli.md) und [Verwenden der Befehlszeilenschnittstelle zum Überwachen und Skalieren einer einzelnen SQL-Datenbank](scripts/sql-database-monitor-and-scale-database-cli.md).
>

| Cmdlet | BESCHREIBUNG |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az-sql-db-create) |Erstellt eine Datenbank|
|[az sql db list](/cli/azure/sql/db#az-sql-db-list)|Listet alle Datenbanken und Data Warehouses eines Servers oder alle Datenbanken eines Pools für elastische Datenbanken auf|
|[az sql db list-editions](/cli/azure/sql/db#az-sql-db-list-editions)|Listet verfügbare Dienstziele und Speicherlimits auf|
|[az sql db list-usages](/cli/azure/sql/db#az-sql-db-list-usages)|Gibt Informationen zur Datenbankverwendung zurück|
|[az sql db show](/cli/azure/sql/db#az-sql-db-show)|Ruft eine Datenbank oder ein Data Warehouse ab|
|[az sql db update](/cli/azure/sql/db#az-sql-db-update)|Aktualisiert eine Datenbank|
|[az sql db delete](/cli/azure/sql/db#az-sql-db-delete)|Entfernt eine Datenbank|
|[az group create](/cli/azure/group#az-group-create)|Erstellt eine Ressourcengruppe|
|[az sql server create](/cli/azure/sql/server#az-sql-server-create)|Erstellt einen Server|
|[az sql server list](/cli/azure/sql/server#az-sql-server-list)|Listet Server auf|
|[az sql server list-usages](/cli/azure/sql/server#az-sql-server-list-usages)|Gibt Informationen zur Server-Verwendung zurück|
|[az sql server show](/cli/azure/sql/server#az-sql-server-show)|Ruft einen Server ab|
|[az sql server update](/cli/azure/sql/server#az-sql-server-update)|Aktualisiert einen Server|
|[az sql server delete](/cli/azure/sql/server#az-sql-server-delete)|Löscht einen Server|
|[az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Erstellt eine Serverfirewallregel|
|[az sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Listet die Firewallregeln auf einem Server auf|
|[az sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Zeigt die Details einer Firewallregel an|
|[az sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Aktualisiert eine Firewallregel|
|[az sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Löscht eine Firewallregel|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: Verwalten von logischen Servern und Datenbanken

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe von Transact-SQL die folgenden T-SQL-Befehle. Sie können diese Befehle mit dem Azure-Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs) oder einem beliebigen anderen Programm ausführen, mit dem eine Verbindung mit einem Azure SQL-Datenbankserver hergestellt und Transact-SQL-Befehle übergeben werden können. Informationen zum Verwalten von Pools für elastische Datenbanken finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

> [!TIP]
> Eine Schnellstartanleitung mit SQL Server Management Studio unter Microsoft Windows finden Sie unter [Azure SQL-Datenbank: Verwenden von SQL Server Management Studio zum Herstellen der Verbindung und Abfragen von Daten](sql-database-connect-query-ssms.md). Eine Schnellstartanleitung mit Visual Studio Code unter macOS, Linux oder Windows finden Sie unter [Azure SQL-Datenbank: Verwenden von Visual Studio Code zum Herstellen einer Verbindung mit und Abfragen von Daten](sql-database-connect-query-vscode.md).
> [!IMPORTANT]
> Sie können einen Server mithilfe von Transact-SQL nicht erstellen oder löschen.

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[CREATE DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/create-database-azure-sql-database)|Erstellt eine neue Datenbank. Sie müssen über eine Verbindung mit der Masterdatenbank verfügen, um eine neue Datenbank erstellen zu können.|
| [ALTER DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/alter-database-azure-sql-database) |Ändert eine Azure SQL-Datenbank. |
|[ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Ändert ein Azure SQL Data Warehouse.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Löscht eine Datenbank.|
|[sys.database_service_objectives (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Gibt die Edition (Dienstebene), das Dienstziel (Tarif) und den Namen des Pools für elastische Datenbanken, falls vorhanden, für eine Azure SQL-Datenbank oder ein Azure SQL Data Warehouse zurück. Wenn eine Anmeldung an der Masterdatenbank in einem Azure SQL-Datenbankserver besteht, werden Informationen zu allen Datenbanken zurückgegeben. Für Azure SQL Data Warehouse müssen Sie über eine Verbindung mit der Masterdatenbank verfügen.|
|[sys.dm_db_resource_stats (Azure SQL-Datenbank)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Gibt die CPU-, E/A- und Arbeitsspeichernutzung für eine Azure SQL-Datenbank zurück. Für alle 15 Sekunden ist eine Zeile vorhanden, selbst wenn keine Aktivität in der Datenbank erfolgt ist.|
|[sys.resource_stats (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Gibt CPU-Nutzungs- und Speicherdaten für eine Azure SQL-Datenbank zurück. Die Daten werden in Intervallen von fünf Minuten gesammelt und aggregiert.|
|[sys.database_connection_stats (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Enthält Statistiken zu Verbindungsereignissen für SQL-Datenbank und eine Übersicht über erfolgreiche und nicht erfolgreiche Datenbankverbindungen. |
|[sys.event_log (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Gibt erfolgreiche Datenbankverbindungen, Verbindungsfehler und Deadlocks für Azure SQL-Datenbank zurück. Sie können diese Informationen nutzen, um die Datenbankaktivität mit SQL-Datenbank nachzuverfolgen oder um Probleme zu beheben.|
|[sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Erstellt oder aktualisiert die Firewalleinstellungen auf Serverebene für Ihre SQL-Datenbankserver. Diese gespeicherte Prozedur ist nur in der Masterdatenbank für die Prinzipalanmeldung auf Serverebene verfügbar. Eine Firewallregel auf Serverebene kann erst mithilfe von Transact-SQL erstellt werden, nachdem die erste Firewallregel auf Serverebene von einem Benutzer mit Azure-Berechtigungen erstellt wurde.|
|[sys.firewall_rules (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Gibt Informationen zu den Firewalleinstellungen auf Serverebene im Zusammenhang mit Ihrer Microsoft Azure SQL-Datenbank zurück.|
|[sp_delete_firewall_rule (Azure SQL-Datenbank)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Entfernt Firewalleinstellungen auf Serverebene von Ihrem SQL-Datenbankserver. Diese gespeicherte Prozedur ist nur in der Masterdatenbank für die Prinzipalanmeldung auf Serverebene verfügbar.|
|[sp_set_database_firewall_rule (Azure SQL-Datenbank)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Erstellt oder aktualisiert die Firewallregeln auf Datenbankebene für Ihre Azure SQL-Datenbank oder SQL Data Warehouse. Datenbank-Firewallregeln können für die Masterdatenbank und für Benutzerdatenbanken in SQL-Datenbank konfiguriert werden. Datenbank-Firewallregeln sind bei der Verwendung von Benutzern eigenständiger Datenbanken nützlich. |
|[sys.database_firewall_rules (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Gibt Informationen zu den Firewalleinstellungen auf Datenbankebene im Zusammenhang mit Ihrer Microsoft Azure SQL-Datenbank zurück. |
|[sp_delete_database_firewall_rule (Azure SQL-Datenbank)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Entfernt die Firewalleinstellung auf Datenbankebene von Ihrer Azure SQL-Datenbank oder SQL Data Warehouse. |

## <a name="rest-api-manage-logical-servers-and-databases"></a>REST-API: Verwalten von logischen Servern und Datenbanken

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls diese REST-API-Anforderungen.

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[Servers - Create Or Update](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Erstellt oder aktualisiert einen neuen Server.|
|[Servers - Delete](https://docs.microsoft.com/rest/api/sql/servers/delete)|Löscht eine SQL Server-Instanz.|
|[Servers - Get](https://docs.microsoft.com/rest/api/sql/servers/get)|Ruft einen Server ab.|
|[Servers - List](https://docs.microsoft.com/rest/api/sql/servers/list)|Gibt eine Serverliste zurück.|
|[Servers - List By Resource Group](https://docs.microsoft.com/rest/api/sql/servers/listbyresourcegroup)|Gibt eine Liste aller Server in einer Ressourcengruppe zurück.|
|[Server - Update](https://docs.microsoft.com/rest/api/sql/servers/update)|Aktualisiert einen vorhandenen Server.|
|[Datenbanken – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)|Erstellt eine neue Datenbank oder aktualisiert eine bereits vorhandene Datenbank|
|[Datenbanken – Löschen](https://docs.microsoft.com/rest/api/sql/databases/delete)|Löscht eine Datenbank.|
|[Datenbanken – Abrufen](https://docs.microsoft.com/rest/api/sql/databases/get)|Ruft eine Datenbank ab|
|[Datenbanken – Auflisten nach Pool für elastische Datenbanken](https://docs.microsoft.com/rest/api/sql/databases/listbyelasticpool)|Gibt eine Liste der Datenbanken in einem Pool für elastische Datenbanken zurück.|
|[Datenbanken – Auflisten nach Server](https://docs.microsoft.com/rest/api/sql/databases/listbyserver)|Gibt eine Liste der Datenbanken auf einem Server zurück|
|[Datenbanken – Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/update)|Aktualisiert eine vorhandene Datenbank.|
|[Firewall Rules - Create Or Update](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate)|Erstellt oder aktualisiert eine Firewallregel.|
|[Firewall Rules - Delete](https://docs.microsoft.com/rest/api/sql/firewallrules/delete)|Löscht eine Firewallregel.|
|[Firewall Rules - Get](https://docs.microsoft.com/rest/api/sql/firewallrules/get)|Ruft eine Firewallregel ab.|
|[Firewall Rules - List By Server](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver)|Gibt eine Liste von Firewallregeln zurück.|

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Migrieren einer SQL Server-Datenbank zu Azure finden Sie unter [Migrieren zu Azure SQL-Datenbank](sql-database-cloud-migrate.md).
- Informationen zu unterstützten Funktionen finden Sie unter [Features (Funktionen)](sql-database-features.md).
