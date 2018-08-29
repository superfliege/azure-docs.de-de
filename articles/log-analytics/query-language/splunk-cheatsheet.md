---
title: Von Splunk zu Azure Log Analytics | Microsoft-Dokumentation
description: Bietet Unterstützung beim Erlernen der Log Analytics-Abfragesprache für Benutzer, die mit Splunk vertraut sind.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 5ea9790695b8afe7bd42b98b071869756b301350
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42447068"
---
# <a name="splunk-to-log-analytics"></a>Von Splunk zu Log Analytics

Dieser Artikel dient als Unterstützung beim Erlernen der Log Analytics-Abfragesprache für Benutzer, die mit Splunk vertraut sind. Die beiden Sprachen werden direkt miteinander verglichen, um wichtige Unterschiede sowie Ähnlichkeiten, die Ihnen eine Nutzung Ihrer vorhandenen Kenntnisse ermöglichen, zu veranschaulichen.

## <a name="structure-and-concepts"></a>Struktur und Konzepte

In der folgenden Tabelle werden Konzepte und Datenstrukturen von Splunk und Log Analytics miteinander verglichen.

 | Konzept  | Splunk | Log Analytics |  Kommentar
 | --- | --- | --- | ---
 | Bereitstellungseinheit  | cluster |  cluster |  Log Analytics ermöglicht beliebige clusterübergreifende Abfragen. Bei Splunk ist das nicht der Fall. |
 | Datencaches |  buckets  |  Caching und Aufbewahrungsrichtlinien |  Steuert den Zeitraum und die Cachingebene für die Daten. Diese Einstellung wirkt sich direkt auf die Leistung von Abfragen und die Kosten der Bereitstellung aus. |
 | Logische Partition von Daten  |  index  |  database  |  Ermöglicht die logische Trennung der Daten. Beide Implementierungen ermöglichen partitionsübergreifende Unions und Verknüpfungen. |
 | Strukturierte Ereignismetadaten | N/V | table |  Bei Splunk wird das Konzept der Suchsprache für Ereignismetadaten nicht verfügbar gemacht. Log Analytics weist das Konzept einer Tabelle mit Spalten auf. Jede Ereignisinstanz ist einer Zeile zugeordnet. |
 | Datensatz | event | row |  Nur Terminologieänderung. |
 | Datensatzattribut | field |  column |  In Log Analytics ist dies als Teil der Tabellenstruktur vordefiniert. In Splunk hat jedes Ereignis einen eigenen Satz von Feldern. |
 | Typen | datatype |  datatype |  Log Analytics-Datentypen sind expliziter, da sie für die Spalten festgelegt sind. Beide verfügen über die Möglichkeit, dynamisch mit Datentypen und einem sich weitgehend entsprechenden Satz von Datentypen, einschließlich JSON-Unterstützung, zu arbeiten. |
 | Abfrage und Suche  | search | query |  Die Konzepte sind bei Log Analytics und Splunk im Wesentlichen identisch. |
 | Ereigniserfassungszeit | Systemzeit | ingestion_time() |  In Splunk erhält jedes Ereignis einen Systemzeitstempel des Zeitpunkts, zu dem das Ereignis indiziert wurde. In Log Analytics können Sie eine Richtlinie namens „ingestion_time“ definieren, mit der eine Systemspalte bereitgestellt wird, auf die mit der Funktion „ingestion_time()“ verwiesen werden kann. |

## <a name="functions"></a>Funktionen

In der folgenden Tabelle sind Funktionen in Log Analytics angegeben, die Splunk-Funktionen entsprechen.

|Splunk | Log Analytics |Kommentar
|---|---|---
|strcat | strcat()| (1) |
|split  | split() | (1) |
|if     | iff()   | (1) |
|tonumber | todouble()<br>tolong()<br>toint() | (1) |
|upper<br>lower |toupper()<br>tolower()|(1) |
| replace | replace() | (1)<br> Beachten Sie auch, dass `replace()` zwar bei beiden Produkten drei Parameter verwendet, die Parameter aber unterschiedlich sind. |
| substr | substring() | (1)<br>Beachten Sie auch, dass Splunk einsbasierte Indizes verwendet. Log Analytics verwendet nullbasierte Indizes. |
| tolower |  tolower() | (1) |
| toupper | toupper() | (1) |
| match | matches regex |  (2)  |
| regex | matches regex | In Splunk ist `regex` ein Operator. In Log Analytics ist es ein relationaler Operator. |
| searchmatch | == | In Splunk ermöglicht `searchmatch` eine Suche nach der exakten Zeichenfolge.
| random | rand()<br>rand(n) | Die Splunk-Funktion gibt eine Zahl zwischen 0 (null) und 2<sup>31</sup>-1 zurück. Log Analytics gibt eine Zahl zwischen 0,0 und 1,0 oder bei angegebenem Parameter eine Zahl zwischen 0 und n-1 zurück.
| now | now() | (1)
| relative_time | totimespan() | (1)<br>Die Entsprechung der Splunk-Funktion „relative_time(datetimeVal, offsetVal)“ lautet in Log Analystics „datetimeVal + totimespan(offsetVal)“.<br>Beispielsweise wird <code>search &#124; eval n=relative_time(now(), "-1d@d")</code> zu <code>...  &#124; extend myTime = now() - totimespan("1d")</code>.

(1) In Splunk wird die Funktion mit dem `eval`-Operator aufgerufen. In Log Analytics wird sie als Teil von `extend` oder `project` verwendet.<br>(2) In Splunk wird die Funktion mit dem `eval`-Operator aufgerufen. In Log Analytics kann sie mit dem `where`-Operator verwendet werden.


## <a name="operators"></a>Operatoren

Die folgenden Abschnitte enthalten Beispiele für die Verwendung verschiedener Operatoren in Splunk und Log Analytics.

> [!NOTE]
> Im folgenden Beispiel ist das Splunk-Feld _rule_ einer Tabelle in Azure Log Analytics zugeordnet, und der Standardzeitstempel von Splunk ist der Spalte _ingestion_time()_ in Log Analytics zugeordnet.

### <a name="search"></a>Suche
In Splunk können Sie das Schlüsselwort `search` auslassen und eine Zeichenfolge ohne Anführungszeichen eingeben. In Azure Log Analytics müssen Sie jede Suche mit `find` beginnen, eine nicht in Anführungszeichen eingeschlossene Zeichenfolge ist ein Spaltenname und der Suchwert muss eine Zeichenfolge in Anführungszeichen sein. 

| |  | |
|:---|:---|:---|
| Splunk | **search** | <code>search Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" earliest=-24h</code> |
| Log Analytics | **find** | <code>find Session.Id=="c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time()> ago(24h)</code> |
| | |

### <a name="filter"></a>Filter
Abfragen in Azure Log Analytics beginnen in einem tabellarischen Resultset mit dem where-Filter. In Splunk ist Filtern der Standardvorgang für den aktuellen Index. Sie können auch den `where`-Operator in Splunk verwenden, doch wird dies nicht empfohlen.

| |  | |
|:---|:---|:---|
| Splunk | **search** | <code>Event.Rule="330009.2" Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" _indextime>-24h</code> |
| Log Analytics | **where** | <code>Office_Hub_OHubBGTaskError<br>&#124; where Session_Id == "c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time() > ago(24h)</code> |
| | |


### <a name="getting-n-eventsrows-for-inspection"></a>Abrufen von n Ereignissen/Zeilen zur Überprüfung 
Azure Log Analytics unterstützt auch `take` als Alias für `limit`. Wenn in Splunk die Ergebnisse sortiert sind, gibt `head` die ersten n Ergebnisse zurück. In Azure Log Analytics ist „limit“ nicht sortiert, doch werden die ersten n Zeilen zurückgegeben, die gefunden werden.

| |  | |
|:---|:---|:---|
| Splunk | **head** | <code>Event.Rule=330009.2<br>&#124; head 100</code> |
| Log Analytics | **limit** | <code>Office_Hub_OHubBGTaskError<br>&#124; limit 100</code> |
| | |



### <a name="getting-the-first-n-eventsrows-ordered-by-a-fieldcolumn"></a>Abrufen der ersten n Ereignisse/Zeilen nach Feld/Spalte sortiert
Für niedrigste Ergebnisse in Splunk verwenden Sie `tail`. In Azure Log Analytics können Sie die Sortierrichtung mit `asc` angeben.

| |  | |
|:---|:---|:---|
| Splunk | **head** |  <code>Event.Rule="330009.2"<br>&#124; sort Event.Sequence<br>&#124; head 20</code> |
| Log Analytics | **top** | <code>Office_Hub_OHubBGTaskError<br>&#124; top 20 by Event_Sequence</code> |
| | |




### <a name="extending-the-result-set-with-new-fieldscolumns"></a>Erweitern des Resultset mit neuen Feldern/Spalten
Splunk verfügt auch über eine `eval`-Funktion, die nicht mit dem `eval`-Operator vergleichbar ist. Sowohl der `eval`-Operator in Splunk als auch der `extend`-Operator in Azure Log Analytics unterstützen nur skalare Funktionen und arithmetische Operatoren.

| |  | |
|:---|:---|:---|
| Splunk | **eval** |  <code>Event.Rule=330009.2<br>&#124; eval state= if(Data.Exception = "0", "success", "error")</code> |
| Log Analytics | **extend** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend state = iif(Data_Exception == 0,"success" ,"error")</code> |
| | |


### <a name="rename"></a>Umbenennen 
Azure Log Analytics verwendet den gleichen Operator zum Umbenennen und zum Erstellen eines neuen Felds. Splunk verfügt über zwei separate Operatoren, `eval` und `rename`.

| |  | |
|:---|:---|:---|
| Splunk | **rename** |  <code>Event.Rule=330009.2<br>&#124; rename Date.Exception as execption</code> |
| Log Analytics | **extend** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend exception = Date_Exception</code> |
| | |




### <a name="format-resultsprojection"></a>Formatieren von Ergebnissen/Projektion
Splunk scheint keinen Operator aufzuweisen, der `project-away` ähnelt. Sie können die Benutzeroberfläche verwenden, um Felder auszufiltern.

| |  | |
|:---|:---|:---|
| Splunk | **table** |  <code>Event.Rule=330009.2<br>&#124; table rule, state</code> |
| Log Analytics | **project**<br>**project-away** | <code>Office_Hub_OHubBGTaskError<br>&#124; project exception, state</code> |
| | |



### <a name="aggregation"></a>Aggregation
Informationen zu den verschiedenen Aggregationsfunktionen finden Sie unter [Aggregationen in Log Analytics-Abfragen](aggregations.md).

| |  | |
|:---|:---|:---|
| Splunk | **stats** |  <code>search (Rule=120502.*)<br>&#124; stats count by OSEnv, Audience</code> |
| Log Analytics | **summarize** | <code>Office_Hub_OHubBGTaskError<br>&#124; summarize count() by App_Platform, Release_Audience</code> |
| | |



### <a name="join"></a>Verknüpfen
Das Verknüpfen in Splunk weist wesentliche Einschränkungen auf. Für die Unterabfrage gilt ein Grenzwert von 10000 Ergebnissen (in der Bereitstellungskonfigurationsdatei festgelegt) sowie eine begrenzte Anzahl von Verknüpfungskonfigurationen.

| |  | |
|:---|:---|:---|
| Splunk | **join** |  <code>Event.Rule=120103* &#124; stats by Client.Id, Data.Alias | join Client.Id max=0 [search earliest=-24h Event.Rule="150310.0" Data.Hresult=-2147221040]</code> |
| Log Analytics | **join** | <code>cluster("OAriaPPT").database("Office PowerPoint").Office_PowerPoint_PPT_Exceptions<br>&#124; where  Data_Hresult== -2147221040<br>&#124; join kind = inner (Office_System_SystemHealthMetadata<br>&#124; summarize by Client_Id, Data_Alias)on Client_Id</code>   |
| | |



### <a name="sort"></a>Sortieren
In Splunk müssen Sie zum Sortieren in aufsteigender Reihenfolge den `reverse`-Operator verwenden. In Azure Log Analytics kann auch definiert werden, wo Nullen zu platzieren sind, am Anfang oder am Ende.

| |  | |
|:---|:---|:---|
| Splunk | **sort** |  <code>Event.Rule=120103<br>&#124; sort Data.Hresult<br>&#124; reverse</code> |
| Log Analytics | **order by** | <code>Office_Hub_OHubBGTaskError<br>&#124; order by Data_Hresult,  desc</code> |
| | |



### <a name="multivalue-expand"></a>Erweiterung für mehrere Werte
Dies ist in Splunk und Log Analytics ein ähnlicher Operator.

| |  | |
|:---|:---|:---|
| Splunk | **mvexpand** |  `mvexpand foo` |
| Log Analytics | **mvexpand** | `mvexpand foo` |
| | |




### <a name="results-facets-interesting-fields"></a>Ergebnisfacets, interessante Felder
Im Log Analytics-Portal wird nur die erste Spalte verfügbar gemacht. Alle Spalten stehen über die API zur Verfügung.

| |  | |
|:---|:---|:---|
| Splunk | **fields** |  <code>Event.Rule=330009.2<br>&#124; fields App.Version, App.Platform</code> |
| Log Analytics | **facets** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; facet by App_Branch, App_Version</code> |
| | |




### <a name="de-duplicate"></a>Deduplizieren
Sie können stattdessen `summarize arg_min()` verwenden, um die Reihenfolge umzukehren, in der ein Datensatz ausgewählt wird.

| |  | |
|:---|:---|:---|
| Splunk | **dedup** |  <code>Event.Rule=330009.2<br>&#124; dedup device_id sortby -batterylife</code> |
| Log Analytics | **summarize arg_max()** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; summarize arg_max(batterylife, *) by device_id</code> |
| | |




## <a name="next-steps"></a>Nächste Schritte

- Arbeiten Sie eine Lektion über das [Schreiben von Abfragen in Log Analytics](get-started-queries.md) durch.