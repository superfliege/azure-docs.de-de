---
title: SQL Data Warehouse-Empfehlungen – Konzepte | Microsoft-Dokumentation
description: Erfahren Sie mehr über SQL Data Warehouse-Empfehlungen und wie sie erzeugt werden
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/27/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 57bce631a570f549d46a9b0beefcb5adce4decfc
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380113"
---
# <a name="sql-data-warehouse-recommendations"></a>SQL Data Warehouse-Empfehlungen

Dieser Artikel beschreibt die Empfehlungen, die SQL Data Warehouse mit dem Azure Advisor unterbreitet.  

SQL Data Warehouse bietet Empfehlungen, um sicherzustellen, dass Ihr Data Warehouse konsistent für Leistung optimiert ist. Data Warehouse-Empfehlungen sind eng in den [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) integriert, um Sie direkt im [Azure-Portal](https://aka.ms/Azureadvisor) mit bewährten Methoden vertraut zu machen. Auf täglicher Basis analysiert SQL Data Warehouse den aktuellen Zustand Ihres Data Warehouse, sammelt Telemetriedaten und unterbreitet Empfehlungen für Ihre aktive Workload. Die unterstützten Data Warehouse-Empfehlungsszenarien werden im Folgenden zusammen mit der Anwendung empfohlener Aktionen beschrieben.

Wenn Sie uns Feedback zum SQL Data Warehouse Advisor geben möchten oder Probleme auftreten, wenden Sie sich an [sqldwadvisor@service.microsoft.com](mailto:sqldwadvisor@service.microsoft.com).   

Klicken Sie [hier](https://aka.ms/Azureadvisor), um zu erfahren, welche Empfehlungen Sie heute erhalten! Dieses Feature ist zurzeit nur auf Gen2-Data Warehouses anwendbar. 

## <a name="data-skew"></a>Datenschiefe

Datenschiefe kann eine zusätzliche Datenverschiebung oder Ressourcenengpässe beim Ausführen Ihrer Workload verursachen. Die folgende Dokumentation beschreibt, wie Datenschiefe identifiziert und ihr Auftreten durch Auswahl eines Schlüssels für die optimale Verteilung verhindert wird.

- [Identifizieren und Entfernen von Datenschiefe](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Keine oder veraltete Statistiken

Suboptimale Statistiken können die Abfrageleistung erheblich beeinträchtigen, weil sie dazu führen können, dass der Abfrageoptimierer von SQL Data Warehouse suboptimale Abfragepläne generiert. Die folgende Dokumentation beschreibt die bewährten Methoden zum Erstellen und Aktualisieren von Statistiken:

- [Erstellen und Aktualisieren von Tabellenstatistiken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics)

Führen Sie zum Anzeigen der Liste der Tabellen, auf die sich diese Empfehlungen auswirken, dieses [T-SQL-Skript](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) aus. Der Advisor führt kontinuierlich dasselbe T-SQL-Skript aus, um die Empfehlungen zu generieren.
