---
title: Azure PowerShell-Skriptbeispiele für SQL-Datenbank | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiele zum Erstellen und Verwalten von Azure SQL-Datenbank-Servern, Pools für elastische Datenbanken, Datenbanken und Firewalls.
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: craigg
editor: tysonn
tags: azure-service-management
ms.service: sql-database
ms.custom: overview-samples, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: 81f39c2ecc014ee53dcff2fab88ff0f890e1a610
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091883"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure PowerShell-Beispiele für Azure SQL-Datenbank

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure SQL-Datenbank.

| |  |
|---|---|
|**Erstellen einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Create a single SQL database and configure a firewall rule using PowerShell](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Erstellen einer einzelnen SQL-Datenbank und Konfigurieren einer Firewallregel mit PowerShell) | Dieses PowerShell-Skript erstellt eine einzelne Azure SQL-Datenbank und konfiguriert eine Firewallregel auf Serverebene. |
| [Create elastic pools and move databases between pools and out of a pool using PowerShell](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Erstellen von Pools für elastische Datenbanken und Verschieben von Datenbanken zwischen Pools und aus einem Pool mit PowerShell) | Dieses PowerShell-Skript erstellt Pools für elastische Azure SQL-Datenbank-Instanzen, in einem Pool zusammengefasste Datenbanken verschoben und Leistungsstufen geändert.|
|**Konfigurieren von Georeplikation und Failover**||
| [Configure and failover a single database using active geo-replication](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Konfiguration und Failover einer einzelnen Datenbank mithilfe von aktiver Georeplikation)| Dieses PowerShell-Skript konfiguriert die aktive Georeplikation für eine einzelne Azure SQL-Datenbank und führt ein Failover zum sekundären Replikat aus. |
| [Configure and failover a pooled database using active geo-replication](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Konfiguration und Failover einer gepoolten Datenbank mithilfe von aktiver Georeplikation)| Dieses PowerShell-Skript konfiguriert die aktive Georeplikation für eine Azure SQL-Datenbank in einem Pool für elastische SQL-Datenbanken und führt ein Failover zum sekundären Replikat aus. |
| [Konfiguration und Failover einer Failovergruppe für eine einzelne Datenbank](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript konfiguriert eine Failover-Gruppe für eine Azure SQL-Datenbank-Serverinstanz, fügt der Failovergruppe eine Datenbank hinzu und führt ein Failover auf den sekundären Server durch. |
|**Skalieren einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Monitor and scale a single SQL database using the Azure CLI](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Überwachen und Skalieren einer einzelnen SQL-Datenbank mit Azure CLI) | Dieses PowerShell-Skript überwacht die Leistungsmetriken einer Azure-SQL-Datenbank, skaliert sie auf eine höhere Leistungsstufe und erstellt eine Warnungsregel auf einer der Leistungsmetriken. |
| [Scale an elastic pool in Azure SQL Database using the Azure CLI](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Skalieren eines Pools für elastische Datenbanken in Azure SQL-Datenbank mit Azure CLI) | Dieses PowerShell-Skript überwacht die Leistungsmetriken eines Pools für elastische Azure SQL-Datenbank-Instanzen, skaliert ihn auf eine höhere Leistungsstufe und erstellt eine Warnungsregel für eine der Leistungsmetriken.  |
| **Überwachung und Bedrohungserkennung** |
| [Configure SQL Database auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Konfigurieren der Überwachung und Bedrohungserkennung für SQL-Datenbank mit PowerShell)| Dieses PowerShell-Skript konfiguriert Richtlinien zur Überwachung und Bedrohungserkennung für eine Azure SQL-Datenbank. |
| **Wiederherstellen, Kopieren und Importieren einer Datenbank**||
| [Restore a SQL database using PowerShell (Wiederherstellen einer SQL-Datenbank mit PowerShell)](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dieses PowerShell-Skript stellt eine Azure SQL-Datenbank auf Basis einer georedundanten Sicherung und eine gelöschte Azure SQL-Datenbank gemäß der letzten Sicherung wieder her. |
| [Copy a SQL database to a new server using PowerShell](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Kopieren einer SQL-Datenbank auf einen neuen Server mit PowerShell)| Dieses PowerShell-Skript erstellt eine Kopie einer vorhandenen Azure SQL-Datenbank auf einem neuen Azure SQL-Server. |
| [Import from a bacpac into a SQL database using PowerShell](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Importieren aus einer BACPAC-Datei in eine SQL-Datenbank mit PowerShell)| Dieses PowerShell-Skript importiert eine Datenbank aus einer BACPAC-Datei auf einen Azure SQL-Server. |
| **Synchronisieren von Daten zwischen Datenbanken**||
| [Synchronisieren von Daten zwischen SQL-Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | In diesem PowerShell-Skript wird die Datensynchronisierung für die Synchronisierung zwischen mehreren Azure SQL-Datenbanken konfiguriert. |
| [Synchronisieren von Daten zwischen SQL-Datenbank und lokalem SQL Server](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript konfiguriert die Datensynchronisierung für die Synchronisierung zwischen einer Azure SQL-Datenbank und einem lokalen SQL-Server. |
| [Aktualisieren des Synchronisierungsschemas der SQL-Datensynchronisierung](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript fügt dem Synchronisierungsschema der Datensynchronisierung Elemente hinzu oder entfernt sie daraus. |
|||
