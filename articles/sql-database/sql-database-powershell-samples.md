---
title: Azure PowerShell-Skriptbeispiele für SQL-Datenbank | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiele zum Erstellen und Verwalten von Azure SQL-Datenbank-Servern, Pools für elastische Datenbanken, Datenbanken und Firewalls.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: 6be6021ef828202ad37a8af4eba942e6898963ca
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59259982"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure PowerShell-Beispiele für Azure SQL-Datenbank

Azure SQL-Datenbank ermöglicht Ihnen das Konfigurieren Ihrer Datenbanken, Instanzen und Pools mithilfe von Azure PowerShell.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 1.4.0 von Azure PowerShell verwenden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="single-database-and-elastic-pools"></a>Einzeldatenbank und Pools für elastische Datenbanken

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure SQL-Datenbank.

| |  |
|---|---|
|**Erstellen und Konfigurieren von Einzeldatenbanken und Pools für elastische Datenbanken**||
| [Verwenden von PowerShell zum Erstellen einer einzelnen Azure SQL-Datenbank und Konfigurieren einer Firewallregel für einen SQL-Datenbankserver](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript erstellt eine einzelne Azure SQL-Datenbank und konfiguriert eine Firewallregel auf Serverebene. |
| [Erstellen von Pools für elastische Datenbanken und Verschieben von Datenbanken zwischen Pools für elastische Datenbanken mit PowerShell](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript erstellt Pools für elastische Azure SQL-Datenbank-Instanzen, verschiebt in einem Pool zusammengefasste Datenbanken und ändert Computegrößen.|
|**Konfigurieren von Georeplikation und Failover**||
| [Verwenden von PowerShell zum Konfigurieren der aktiven Georeplikation für eine Einzeldatenbank in Azure SQL-Datenbank](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dieses PowerShell-Skript konfiguriert die aktive Georeplikation für eine einzelne Azure SQL-Datenbank und führt ein Failover zum sekundären Replikat aus. |
| [Verwenden von PowerShell zum Konfigurieren der aktiven Georeplikation für eine in einem Pool zusammengefasste Datenbank in Azure SQL-Datenbank](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dieses PowerShell-Skript konfiguriert die aktive Georeplikation für eine Azure SQL-Datenbank in einem Pool für elastische SQL-Datenbanken und führt ein Failover zum sekundären Replikat aus. |
| [Verwenden von PowerShell zum Konfigurieren einer Failovergruppe für die aktive Georeplikation für eine Einzeldatenbank in Azure SQL-Datenbank](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript konfiguriert eine Failover-Gruppe für eine Azure SQL-Datenbank-Serverinstanz, fügt der Failovergruppe eine Datenbank hinzu und führt ein Failover auf den sekundären Server durch. |
|**Skalieren einer Einzeldatenbank und eines Pools für elastische Datenbanken**||
| [Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript überwacht die Leistungsmetriken einer SQL-Datenbank von Azure, skaliert sie auf eine höhere Computegröße und erstellt eine Warnungsregel für eine der Leistungsmetriken. |
| [Skalieren eines Pools für elastische Datenbanken](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript überwacht die Leistungsmetriken eines Pools für elastische Azure SQL-Datenbank-Instanzen, skaliert ihn auf eine höhere Computegröße und erstellt eine Warnungsregel für eine der Leistungsmetriken.  |
| **Überwachung und Bedrohungserkennung** |
| [Konfigurieren der Überwachung und Bedrohungserkennung](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dieses PowerShell-Skript konfiguriert Richtlinien zur Überwachung und Bedrohungserkennung für eine Azure SQL-Datenbank. |
| **Wiederherstellen, Kopieren und Importieren einer Datenbank**||
| [Wiederherstellen einer Datenbank](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dieses PowerShell-Skript stellt eine Azure SQL-Datenbank auf Basis einer georedundanten Sicherung und eine gelöschte Azure SQL-Datenbank gemäß der letzten Sicherung wieder her. |
| [Kopieren einer SQL-Datenbank auf einen neuen Server mit PowerShell](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dieses PowerShell-Skript erstellt eine Kopie einer vorhandenen Azure SQL-Datenbank auf einem neuen Azure SQL-Server. |
| [Verwenden von PowerShell zum Importieren einer BACPAC-Datei in eine Azure SQL-Datenbank](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dieses PowerShell-Skript importiert eine Datenbank aus einer BACPAC-Datei auf einen Azure SQL-Server. |
| **Synchronisieren von Daten zwischen Datenbanken**||
| [Verwenden von PowerShell zum Synchronisieren zwischen mehreren SQL-Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | In diesem PowerShell-Skript wird die Datensynchronisierung für die Synchronisierung zwischen mehreren Azure SQL-Datenbanken konfiguriert. |
| [Verwenden von PowerShell zum Synchronisieren zwischen einer SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript konfiguriert die Datensynchronisierung für die Synchronisierung zwischen einer Azure SQL-Datenbank und einem lokalen SQL-Server. |
| [Verwenden von PowerShell zum Aktualisieren des Synchronisierungsschemas in einer bestehenden Synchronisierungsgruppe](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript fügt dem Synchronisierungsschema der Datensynchronisierung Elemente hinzu oder entfernt sie daraus. |
|||

Erfahren Sie mehr über die [Azure PowerShell-API für Einzeldatenbanken](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases).

## <a name="managed-instance"></a>Verwaltete Instanz

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für verwaltete Azure SQL-Datenbank-Instanzen.

| |  |
|---|---|
|**Erstellen und Konfigurieren von verwalteten Instanzen**||
| [Erstellen und Verwalten einer verwalteten Instanz](scripts/sql-database-create-configure-managed-instance-powershell.md) | Dieses PowerShell-Skript zeigt, wie Sie mithilfe von Azure PowerShell eine verwaltete Instanz erstellen und verwalten. |
| [Verwenden von PowerShell mit einer Azure Resource Manager-Vorlage zum Erstellen einer verwalteten Instanz in Azure SQL-Datenbank](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript zeigt, wie Sie mithilfe von Azure PowerShell und einer Azure Resource Manager-Vorlage eine verwaltete Instanz erstellen und verwalten.|
| **Konfigurieren von Transparent Data Encryption (TDE)**||
| [Verwalten von Transparent Data Encryption in einer verwalteten Instanz mithilfe eines eigenen Azure Key Vault-Schlüssels](scripts/transparent-data-encryption-byok-sql-managed-instance-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| In diesem PowerShell-Skript wird Transparent Data Encryption (TDE) in einem Bring Your Own Key-Szenario (BYOK) für eine verwaltete Azure SQL-Datenbank-Instanz unter Verwendung eines Schlüssels aus Azure Key Vault konfiguriert.|
|||

Erfahren Sie mehr über die [Azure PowerShell-API für verwaltete Instanzen](sql-database-managed-instance-create-manage.md#powershell-create-and-manage-managed-instances).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

In den Beispielen auf dieser Seite werden zum Erstellen und Verwalten von Azure SQL-Ressourcen die [Azure SQL-Datenbank-Cmdlets](/powershell/module/az.sql/) verwendet. Weitere Cmdlets zum Ausführen von Abfragen und vielen anderen Datenbankaufgaben befinden sich im Modul [sqlserver](/powershell/module/sqlserver/). Weitere Informationen finden Sie unter [SQL Server-PowerShell](/sql/powershell/sql-server-powershell/).
