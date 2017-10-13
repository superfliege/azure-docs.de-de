---
title: "Intelligent Insights-Diagnoseprotokoll für die Leistung von Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Intelligent Insights bietet ein Diagnoseprotokoll für das Beheben von Leistungsproblemen von Azure SQL-Datenbank"
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
editor: carlrab
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 09/25/2017
ms.author: v-daljep
ms.openlocfilehash: 4890baa4ead3323834a82b3f9340cf751bf0c755
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-intelligent-insights-azure-sql-database-performance-diagnostics-log"></a>Verwenden des Intelligent Insights-Diagnoseprotokolls für die Leistung von Azure SQL-Datenbank

Auf dieser Seite finden Sie Informationen zur Verwendung des von [Intelligent Insights](sql-database-intelligent-insights.md) erstellten Diagnoseprotokolls zur Leistung von Azure SQL-Datenbank, zu seinem Format und den Daten, die es für benutzerdefinierte Entwicklungsanforderungen enthält. Dieses Diagnoseprotokoll kann an [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md), [Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-azure-storage) oder eine Drittanbieterlösung für benutzerdefinierte DevOps-Warnungs- und Berichterstellungsfunktionen gesendet werden.

## <a name="log-header"></a>Protokollheader

Das Diagnoseprotokoll nutzt das JSON-Standardformat zum Ausgeben von Intelligent Insights-Ergebnissen. Die genaue Kategorieeigenschaft für den Zugriff auf das Intelligent Insights-Protokoll ist der feste Wert **SQLInsights**.

Der Header des Protokolls ist allgemein und besteht aus dem Zeitstempel (TimeGenerated) der Erstellung eines Eintrags, einschließlich einer Ressourcen-ID (ResourceId), die auf eine bestimmte Azure SQL-Datenbank-Instanz verweist, zu der der Eintrag gehört. „Category“ (Kategorie), „Level“ (Ebene) und „OperationName“ (Vorgangsname) sind feste Eigenschaften, deren Wert sich nicht ändert. Sie geben an, dass der Protokolleintrag Informationszwecken dient und aus Intelligent Insights (SQLInsights) stammt.

```json
"TimeGenerated" : "2017-9-25 11:00:00", // time stamp of the log entry
"ResourceId" : "database identifier", // value points to a database resource
"Category": "SQLInsights", // fixed property
"Level" : "Informational", // fixed property
"OperationName" : "Insight", // fixed property
```

## <a name="issue-id-and-database-affected"></a>Problem-ID und betroffene Datenbank

Die ID zur Bestimmung des Problems (IssueId_d) bietet eine Möglichkeit zum besonderen Nachverfolgen von Leistungsproblemen bis zu ihrer Lösung. Intelligent Insights versieht den Lebenszyklus eines Problems mit den Angaben „Active“ (Aktiv), Verifying (Wird überprüft) und „Completed“ (Abgeschlossen). In jeder dieser Phasen kann Intelligent Insights mehrere Ereignisdatensätze im Protokoll aufzeichnen. Für jeden dieser Einträge bleibt die ID-Nummer des Problems eindeutig. Intelligent Insights verfolgt das Problem in seinem gesamten Lebenszyklus und ermöglicht alle 15 Minuten einen Einblick in das Diagnoseprotokoll.

Sobald ein Leistungsproblem erkannt wurde und solange es vorliegt, wird das Problem unter der Eigenschaft „Status“ als „Active“ (Aktiv) gemeldet. Sobald ein erkanntes Problem behandelt wird, wird es überprüft und unter der Eigenschaft „Status“ als „Verifying“ (Wird überprüft) gemeldet. Sobald das Problem nicht mehr vorhanden ist, wird es unter der Eigenschaft „Status“ als „Completed“ (Abgeschlossen) gemeldet.
Neben der „Issue ID“ (Problem-ID) meldet das Diagnoseprotokoll den Zeitstempel des Starts (intervalStartTime_t) und Endes (intervalEndTime_t) des jeweiligen Ereignisses, das in Zusammenhang mit einem im Diagnoseprotokoll gemeldeten Problem steht.

Die Eigenschaft „ElasticPoolName_s“ (Name des Pools für elastische Datenbanken) gibt an, zu welchem Pool für elastische Datenbanken die problematische Datenbank gehört. Wenn die Datenbank nicht zu einem Pool für elastische Datenbanken gehört, hat diese Eigenschaft keinen Wert. Die Datenbank, in der ein Problem erkannt wurde, wird in der Eigenschaft „DatabaseName_s“ (Datenbankname) angegeben.

```json
"intervalStartTime_t": "2017-9-25 11:00", // start of the issue reported timestamp
"intervalEndTme_t":"2017-9-25 12:00", // end of the issue reported timestamp
"elasticPoolName_s" : "", // resource elastic pool (if applicable) 
"databaseName_s" : "db_name",  // database name
"issueId_d" : 1525, // unique ID of the issue detected
"status_s" : "Active" // status of the issue – possible values: Active, Verifying and Complete
```

## <a name="detected-issues"></a>Erkannte Probleme

Der nächste Abschnitt eines Intelligent Insights-Leistungsprotokolls enthält Leistungsprobleme, die mithilfe integrierter künstlicher Intelligenz erkannt wurden. Erkennungen werden in der JSON-Eigenschaft „detections_s“ offengelegt, die aus der Kategorie eines Problems (category), der Auswirkungen des Problems (impact), betroffenen Abfragen sowie Metriken besteht. Es muss angemerkt werden, dass die Eigenschaft „detections_s“ mehrere erkannte Leistungsprobleme enthalten kann.

Erkannte Leistungsprobleme werden mit der folgenden Struktur der Eigenschaft „detections_s“ gemeldet:

```json
"detections_s" : [{
"impact" : 1 to 3, // impact of the issue detected, possible values 1-3 (1 low, 2 moderate, 3 high impact)
"category" : "Detectable performance pattern", // tperformance issue detected, see the table
"details": <Details outputted> // details of an issue (see the table)
}] 
```

Erkennbare Leistungsmuster und Details, die im Diagnoseprotokoll ausgegeben werden, befinden sich in der nachstehenden Tabelle.

### <a name="detection-category"></a>Erkennungskategorie

Die Eigenschaft „Category“ (Kategorie) beschreibt eine Kategorie erkennbarer Leistungsmuster. In der folgenden Tabelle finden Sie alle möglichen Kategorien erkennbarer Leistungsmuster. Weitere Details finden Sie auf der Seite [Behandeln von Problemen mit der Leistung von Azure SQL-Datenbank mithilfe von Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md).

Je nach erkanntem Leistungsproblem unterscheiden sich die in der Diagnoseprotokolldatei ausgegebenen Details.

| Erkennbare Leistungsmuster | Ausgegebene Details |
| :------------------- | ------------------- |
| Erreichen von Ressourcengrenzwerten | <li>Betroffene Ressourcen</li><li>Abfragehashes</li><li>Prozentsatz des Ressourcenverbrauchs</li> |
| Gestiegene Workload | <li>Anzahl der Abfragen, deren Ausführung sich erhöht hat</li><li>Abfragehashes von Abfragen mit dem größten Beitrag zur Zunahme der Workload</li> |
| Hohe Arbeitsspeicherauslastung | <li>Arbeitsspeicherclerk</li> |
| Sperren | <li>Betroffene Abfragehashes</li><li>Blockierende Abfragehashes</li> |
| Erhöhter Wert für „Maximaler Grad an Parallelität“ | <li>Abfragehashes</li><li>CXP-Wartezeiten</li><li>Wartezeiten</li> |
| Seitenlatchkonflikt | <li>Abfragehashes von Abfragen, die einen Konflikt verursachen</li> |
| Fehlender Index | <li>Abfragehashes</li> |
| Neue Abfrage | <li>Abfragehash der neuen Abfragen</li> |
| Ungewöhnliche Statistik bei Wartezeiten | <li>Ungewöhnliche Wartetypen</li><li>Abfragehashes</li><li>Abfragewartezeiten</li> |
| TempDB-Konflikt | <li>Abfragehashes von Abfragen, die einen Konflikt verursachen</li><li>Anteil der Abfrage an der gesamten Wartezeit aufgrund des Seitenlatchkonflikts in der Datenbank [%]</li> |
| Mangel an DTUs im Pool für elastische Datenbanken | <li>Pool für elastische Datenbanken</li><li>Datenbank mit dem höchsten DTU-Verbrauch</li><li>Prozentsatz der vom größten Consumer genutzten Pool-DTUs</li> |
| Planregression | <li>Abfragehash</li><li>IDs von guten Plänen</li><li>IDs von schlechten Plänen</li><li>Abfragehashes</li> |
| Änderung eines Werts der datenbankweit gültigen Konfiguration | <li>Datenbankweit gültige Konfiguration ändert sich im Vergleich mit den Standardwerten</li> |
| Langsamer Client | <li>Abfragehashes</li><li>Wartezeiten</li> |
| Tarifdowngrade | <li>Textbenachrichtigung</li> |

### <a name="impact"></a>Auswirkung

Die Eigenschaft „Impact“ (Auswirkung) beschreibt den Anteil eines erkannten Verhaltens am Datenbankproblem auf einer Skala von 1 bis 3. Dabei steht 3 für den höchsten Anteil, 2 für eine mittelhohen Anteil und 1 für den niedrigsten Anteil. Der Wert von „Impact“ kann entsprechend Ihren Anforderungen als Eingabe in eine benutzerdefinierte Warnungsautomatisierung verwendet werden. Die Eigenschaft „QueryHashes“ (Abfragehashes) gibt eine Liste der Abfragehashes zurück, die von einer bestimmten Erkennung betroffen waren.

### <a name="impacted-queries"></a>Betroffene Abfragen

Der nächste Abschnitt eines Intelligent Insights-Protokolls enthält Informationen zu bestimmten Abfragen, die von den erkannten Leistungsproblemen betroffen waren. Diese Informationen werden als ein Array von Objekten offengelegt, das in die Eigenschaft „impact_s“ eingebettet ist. Die Eigenschaft „impact“ besteht aus Entitäten und Metriken. Entitäten beziehen sich auf eine bestimmte Abfrage (type: Query) und eindeutige unter dem Eigenschaftswert „Value“ offengelegte Abfragehashes. Darüber hinaus folgen auf alle offengelegten Abfragen eine Metrik und ein Wert, die zusammen auf ein erkanntes Leistungsproblem deuten.

Im folgenden Protokollbeispiel lässt sich feststellen, dass bei der Abfrage mit dem Hash „0x9102EXZ4“ eine längere Ausführungsdauer (Metric: DurationIncreaseSeconds) mit dem Wert 110 Sekunden erkannt wurde. Dies bedeutet, dass die Ausführung dieser bestimmten Abfrage 110 Sekunden länger gedauert hat. Wichtig ist der Hinweis, dass mehrere Abfragen erkannt werden können, weshalb dieser bestimmte Protokollabschnitt mehrere Abfrageeinträge enthalten kann.

```json
"impact" : [{
"entity" : { 
"Type" : "Query", // type of entity - query
"Value" : "query hash value", // for example "0x9102EXZ4" query hash value },
"Metric" : "DurationIncreaseSeconds", // measured metric and the measurement unit (in this case seconds)
"Value" : 110 // value of the measured metric (in this case seconds)
}]
```

### <a name="metrics"></a>Metriken

Die Maßeinheit für jede gemeldete Metrik wird unter der Eigenschaft „metric“ (Metrik) mit den möglichen Werten (Sekunden, Zahl und Prozent) angegeben. Der Wert einer gemessenen Metrik wird von der Eigenschaft „value“ (Wert) gemeldet.

Die Eigenschaft „DurationIncreaseSeconds“ arbeitet mit der Maßeinheit Sekunden. Für „CriticalErrorCount“ ist die Maßeinheit die Anzahl der Fehler.

```json
"metric" : "DurationIncreaseSeconds", // issue metric type – possible values: DurationIncreaseSeconds, CriticalErrorCount, WaitingSeconds
"value" : 102 // value of the measured metric (in this case seconds)
```

## <a name="root-cause-analysis-and-improvement-recommendations"></a>Fehlerursachenanalyse und Verbesserungsempfehlungen

Der letzte Teil des Intelligent Insights-Leistungsprotokolls ist die Fehlerursachenanalyse einer erkannten Leistungsminderung in benutzerfreundlicher Sprache mithilfe der Eigenschaft „rootCauseAnalysis_s“. Verbesserungsempfehlungen befinden sich, sofern möglich, in dem im Protokoll aufgezeichneten Text.

```json
// example of reported root cause analysis of the detected performance issue, in a human-readable format

"rootCauseAnalysis_s" : "High data IO caused performance to degrade. It seems that this database is missing some indexes that could help."
```

Das Intelligent Insights-Leistungsprotokoll kann an [Azure Log Analytics]( https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-sql) oder eine Drittanbieterlösung für benutzerdefinierte DevOps-Warnungs- und Berichterstellungsfunktionen gesendet werden.

## <a name="next-steps"></a>Nächste Schritte
- Kennenlernen der [Intelligent Insights](sql-database-intelligent-insights.md)-Konzepte
- Informationen zum [Behandeln von Problemen mit der Leistung von Azure SQL-Datenbank mithilfe von Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md)
- Informationen zum [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-sql)
- [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)



