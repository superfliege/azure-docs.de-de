---
title: Azure Monitor – Log Analytics-Abfragesprache | Microsoft-Dokumentation
description: Verweise auf Ressourcen zum Erlernen des Schreibens von Abfragen in Log Analytics
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: bwren
ms.openlocfilehash: 32e64ce7772d562ea34a0d74afbd737be27d247d
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968867"
---
# <a name="log-analytics-query-language"></a>Log Analytics-Abfragesprache
Mit Log Analytics können Sie Protokolle für Azure Monitor erfassen und analysieren. Der Dienst basiert auf Azure Data Explorer und verwendet eine Version der gleichen Abfragesprache. Die Dokumentation über die [Azure Data Explorer-Abfragesprache](/azure/kusto/query) enthält alle Informationen zur Sprache und ist die wichtigste Ressource zum Schreiben von Log Analytics-Abfragen. Diese Seite enthält Links zu anderen Ressourcen, die Sie beim Erlernen des Schreibens von Abfragen unterstützen, und die Unterschiede der Sprachversionen bei der Log Analytics-Implementierung erläutern.

## <a name="getting-started"></a>Erste Schritte

- [Erste Schritte mit Log Analytics im Azure-Portal](get-started-portal.md) erläutert das Schreiben von Abfragen und das Arbeiten mit Ergebnissen im Azure Portal.
-  [Erste Schritte mit Abfragen in Log Analytics](get-started-queries.md) beschreibt das Schreiben von Abfragen mit Log Analytics-Daten.

## <a name="concepts"></a>Konzepte
- [Analysieren von Log Analytics-Daten in Azure Monitor](../../azure-monitor/log-query/log-query-overview.md) gibt einen kurzen Überblick über Protokollabfragen und beschreibt, wie Log Analytics-Daten strukturiert sind.
- [Anzeigen und Analysieren von Daten in Log Analytics](../../azure-monitor/log-query/portals.md) erklärt die Portale, in denen Sie Log Analytics-Abfragen erstellen und ausführen.

## <a name="reference"></a>Verweis

- [Referenz zur Abfragesprache](/azure/kusto/query) ist die vollständige Sprachreferenz für die Data Explorer-Abfragesprache.
- [Log Analytics-Abfragesprache – Unterschiede](data-explorer-difference.md) beschreibt die Unterschiede zwischen den Versionen der Data Explorer-Abfragesprache.
- [Standardeigenschaften in Log Analytics-Datensätzen](../../azure-monitor/platform/log-standard-properties.md) beschreibt Standardeigenschaften aller Log Analytics-Daten.
- [Ausführen ressourcenübergreifender Protokollsuchen in Log Analytics](../../azure-monitor/log-query/cross-workspace-query.md) beschreibt, wie Sie Abfragen schreiben, die Daten aus mehreren Log Analytics-Arbeitsbereichen und Application Insights-Anwendungen verwenden.


## <a name="examples"></a>Beispiele

- [Log Analytics-Abfragebeispiele](examples.md) enthält Beispielabfragen mit Log Analytics-Daten.



## <a name="lessons"></a>Lektionen

- [Arbeiten mit Zeichenfolgen in Log Analytics-Abfragen](string-operations.md) beschreibt das Arbeiten mit Zeichenfolgendaten.
- [Arbeiten mit Werten für Datum und Uhrzeit in Log Analytics-Abfragen](datetime-operations.md) beschreibt das Arbeiten mit Daten für Datum und Uhrzeit. 
- [Aggregationen in Log Analytics-Abfragen](aggregations.md) und [Erweiterte Aggregationen in Log Analytics-Abfragen](advanced-aggregations.md) erläutern, wie Sie Daten aggregieren und zusammenfassen.
- [Joins in Log Analytics-Abfragen](joins.md) beschreibt, wie Sie Daten aus mehreren Tabellen verknüpfen.
- [Arbeiten mit JSON und Datenstrukturen in Log Analytics-Abfragen](json-data-structures.md) beschreibt, wie Sie JSON-Daten analysieren.
- [Schreiben erweiterter Abfragen in Log Analytics](advanced-query-writing.md) erläutert Strategien zum Erstellen komplexer Abfragen und die Wiederverwendung von Code.
- [Erstellen von Grafiken und Diagrammen aus Log Analytics-Abfragen](charts.md) beschreibt, wie Sie Daten aus einer Abfrage visualisieren.

## <a name="cheatsheets"></a>Spickzettel

-  [Cheat Sheet für den Wechsel von der Abfragesprache SQL zu Log Analytics](sql-cheatsheet.md) ist eine hilfreiche Ressource für Benutzer, die bereits mit SQL vertraut sind.
-  [Cheat Sheet für den Wechsel von der Abfragesprache Splunk zu Log Analytics](sql-cheatsheet.md) ist eine hilfreiche Ressource für Benutzer, die bereits mit Splunk vertraut sind.
 
## <a name="next-steps"></a>Nächste Schritte

- [Hier finden Sie die vollständige Referenzdokumentation für die Data Explorer-Abfragesprache.](/azure/kusto/query/)