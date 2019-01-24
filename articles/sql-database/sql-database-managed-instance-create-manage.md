---
title: Referenz zur Verwaltungs-API für eine verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Erstellen und Verwalten von verwalteten Azure SQL-Datenbank-Instanzen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/17/2019
ms.openlocfilehash: 8d108df187e7d1fe549f284cffe3c5cbe4d56f7c
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389940"
---
# <a name="managed-api-reference-for-azure-sql-database-managed-instances"></a>Referenz zur Verwaltungs-API für verwaltete Azure SQL-Datenbank-Instanzen

Sie können verwaltete Azure SQL-Datenbank-Instanzen mit dem Azure-Portal, mit PowerShell, der Azure CLI, der REST-API oder Transact-SQL erstellen und verwalten. Dieser Artikel gibt Ihnen eine Übersicht über Funktionen und API, die Sie zum Erstellen und Konfigurieren einer verwalteten Instanz verwenden können.

## <a name="azure-portal-create-a-managed-instance"></a>Azure-Portal: Erstellen einer verwalteten Instanz

Eine Schnellstartanleitung zum Erstellen einer verwalteten Azure SQL-Datenbank-Instanz finden Sie unter [Schnellstart: Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).

## <a name="powershell-create-and-manage-a-managed-instance"></a>PowerShell: Erstellen und Verwalten einer verwalteten Instanz

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe von Azure PowerShell die folgenden PowerShell-Cmdlets. Wenn Sie PowerShell installieren oder aktualisieren müssen, helfen Ihnen die Informationen unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) weiter.

> [!TIP]
> PowerShell-Beispielskripts finden Sie unter [Schnellstartskript: Erstellen einer verwalteten Azure SQL-Instanz mithilfe der PowerShell-Bibliothek](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | BESCHREIBUNG |
| --- | --- |
|[New-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstance)|Erstellt eine verwaltete Azure SQL-Datenbank-Instanz |
|[Get-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlinstance)|Gibt Informationen zur verwalteten Azure SQL-Instanz zurück|
|[Set-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlinstance)|Legt Eigenschaften für eine verwaltete Azure SQL-Datenbank-Instanz fest|
|[Remove-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlinstance)|Entfernt eine verwaltete Azure SQL-Datenbank-Instanz|
|[New-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstancedatabase)|Erstellt eine verwaltete Azure SQL-Datenbank-Instanz-Datenbank|
|[Get-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlinstancedatabase)|Gibt Informationen zur verwalteten Azure SQL-Instanz-Datenbank zurück|
|[Remove-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlinstancedatabase)|Entfernt eine verwaltete Azure SQL-Datenbank-Instanz-Datenbank|
|[Restore-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqlinstancedatabase)|Stellt eine verwaltete Azure SQL-Datenbank-Instanz-Datenbank wieder her|

## <a name="azure-cli-manage-logical-servers-and-databases"></a>Azure-Befehlszeilenschnittstelle: Verwalten von logischen Servern und Datenbanken

Verwenden Sie zum Erstellen und Verwalten von Azure SQL-Servern, -Datenbanken und -Firewalls mithilfe der [Azure CLI](/cli/azure) die folgenden [Azure CLI-Befehle für eine verwaltete SQL-Instanz](/cli/azure/sql/mi). Führen Sie die CLI mithilfe von [Cloud Shell](/azure/cloud-shell/overview) in Ihrem Browser aus, oder [installieren](/cli/azure/install-azure-cli) Sie sie unter macOS, Linux oder Windows.

> [!TIP]
> Eine Azure CLI-Schnellstartanleitung finden Sie unter [Arbeiten mit einer verwalteten SQL-Instanz mithilfe der Azure CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).

| Cmdlet | BESCHREIBUNG |
| --- | --- |
|[az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Erstellt eine verwaltete Instanz|
|[az sql mi list](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Listet verfügbare verwaltete Instanzen auf|
|[az sql mi show](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Ruft die Details für eine verwaltete Instanz ab|
|[az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Aktualisiert eine verwaltete Instanz|
|[az sql mi delete](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Entfernt eine verwaltete Instanz|
|[az sql midb create](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Erstellt eine verwaltete Datenbank|
|[az sql midb list](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Listet verfügbare verwaltete Datenbanken auf|
|[az sql midb restore](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Stellt eine verwaltete Datenbank wieder her|
|[az sql midb delete](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Entfernt eine verwaltete Datenbank|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: Verwalten von logischen Servern und Datenbanken

Verwenden Sie zum Erstellen und Verwalten einer Datenbank der verwalteten Azure SQL-Datenbank-Instanz nach dem Erstellen der verwalteten Instanz die folgenden T-SQL-Befehle. Sie können diese Befehle über das Azure-Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is), [Visual Studio Code](https://code.visualstudio.com/docs) oder ein anderes Programm ausgeben, mit dem eine Verbindung mit einem Azure SQL-Datenbank-Server hergestellt und Transact-SQL-Befehle übergeben werden können.

> [!TIP]
> Schnellstartanleitungen, in denen gezeigt wird, wie Sie eine verwaltete Instanz mithilfe von SQL Server Management Studio unter Microsoft Windows konfigurieren und eine Verbindung herstellen, finden Sie unter [Schnellstart: Konfigurieren einer Azure-VM für das Herstellen einer Verbindung mit einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-configure-vm.md) und [Schnellstart: Konfigurieren einer Point-to-Site-Verbindung von einem lokalen Computer mit einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-configure-p2s.md).
> [!IMPORTANT]
> Sie können eine verwaltete Instanz mithilfe von Transact-SQL nicht erstellen oder löschen.

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Erstellt eine neue Datenbank der verwalteten Instanz. Sie müssen über eine Verbindung mit der Masterdatenbank verfügen, um eine neue Datenbank erstellen zu können.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Ändert eine Datenbank der verwalteten Azure SQL-Instanz.|

## <a name="rest-api-manage-logical-servers-and-databases"></a>REST-API: Verwalten von logischen Servern und Datenbanken

Verwenden Sie die folgenden REST-API-Anforderungen zum Erstellen und Verwalten einer verwalteten Azure SQL-Datenbank-Instanz.

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[Verwaltete Instanzen – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Erstellt oder aktualisiert eine verwaltete Instanz.|
|[Verwaltete Instanzen – Löschen](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Löscht eine verwaltete Instanz.|
|[Verwaltete Instanzen – Abrufen](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Ruft eine verwaltete Instanz ab.|
|[Verwaltete Instanzen – Auflisten](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Gibt eine Liste der verwalteten Instanzen in einem Abonnement zurück.|
|[Verwaltet Instanzen – Auflisten nach Ressourcengruppe](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Gibt eine Liste der verwalteten Instanzen in einer Ressourcengruppe zurück.|
|[Verwaltete Instanzen – Aktualisieren](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Aktualisiert eine verwaltete Instanz.|

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Migrieren einer SQL Server-Datenbank zu Azure finden Sie unter [Migrieren zu Azure SQL-Datenbank](sql-database-cloud-migrate.md).
- Informationen zu unterstützten Funktionen finden Sie unter [Features (Funktionen)](sql-database-features.md).
