---
title: "Azure Monitor-Metriken: Unterstützte Metriken pro Ressourcentyp | Microsoft Docs"
description: "Liste der Metriken, die mit Azure Monitor für jeden Ressourcentyp zur Verfügung stehen."
author: anirudhcavale
manager: ashwink
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: ancav
ms.openlocfilehash: 05830547a5b8a24a59571edf6dd44d101b660189
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Unterstützte Metriken von Azure Monitor
Azure Monitor bietet verschiedene Methoden für die Interaktion mit Metriken, z.B. die Diagrammdarstellung im Portal, den Zugriff über die REST-API oder die Abfrage über PowerShell oder CLI. Unten ist eine vollständige Liste aller Metriken aufgeführt, die derzeit mit der Metrikpipeline von Azure Monitor verfügbar sind.

> [!NOTE]
> Weitere Metriken stehen möglicherweise im Portal oder über Legacy-APIs zur Verfügung. Diese Liste enthält nur Metriken, die über die konsolidierte Azure Monitor-Metrikpipeline verfügbar sind. Verwenden Sie zum Abfragen von Metriken mit Dimensionen und zum Zugreifen auf diese [2017-05-01-preview (API-Version)](https://docs.microsoft.com/en-us/rest/api/monitor/metricdefinitions).
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|qpu_metric|QPU|Anzahl|Durchschnitt|QPU. Bereich: 0–100 für S1, 0–200 für S2 und 0–400 für S4|Keine Dimensionen|
|memory_metric|Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeicher. Bereich: 0–25 GB für S1, 0–50 GB für S2 und 0–100 GB für S4|Keine Dimensionen|
|TotalConnectionRequests|Total Connection Requests (Verbindungsanforderungen gesamt)|Anzahl|Durchschnitt|Gesamtanzahl von Verbindungsanforderungen. Dies sind erhaltene Anforderungen.|Keine Dimensionen|
|SuccessfullConnectionsPerSec|Erfolgreiche Verbindungen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der erfolgreichen Verbindungsabschlüsse|Keine Dimensionen|
|TotalConnectionFailures|Verbindungsfehler gesamt|Anzahl|Durchschnitt|Gesamtanzahl von fehlerhaften Verbindungsversuchen|Keine Dimensionen|
|CurrentUserSessions|Aktuelle Benutzersitzungen|Anzahl|Durchschnitt|Aktuelle Anzahl von eingerichteten Benutzersitzungen|Keine Dimensionen|
|QueryPoolBusyThreads|Ausgelastete Abfragepoolthreads|Anzahl|Durchschnitt|Anzahl von ausgelasteten Threads im Abfragethreadpool|Keine Dimensionen|
|CommandPoolJobQueueLength|Warteschlangenlänge für Aufträge im Befehlspool|Anzahl|Durchschnitt|Gibt die Anzahl von Aufträgen in der Warteschlange des Befehlsthreadpools an.|Keine Dimensionen|
|ProcessingPoolJobQueueLength|Warteschlangenlänge für Verarbeitungspoolaufträge|Anzahl|Durchschnitt|Anzahl von Nicht-E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools|Keine Dimensionen|
|CurrentConnections|Verbindung: Aktuelle Verbindungen|Anzahl|Durchschnitt|Aktuelle Anzahl hergestellter Clientverbindungen.|Keine Dimensionen|
|CleanerCurrentPrice|Arbeitsspeicher: Bereinigung – aktueller Preis|Anzahl|Durchschnitt|Aktueller Preis des Arbeitsspeichers, $/Byte/Zeit, normalisiert auf 1000.|Keine Dimensionen|
|CleanerMemoryShrinkable|Arbeitsspeicher: Bereinigung – verkleinerbarer Arbeitsspeicher|Byte|Durchschnitt|Die Menge des Arbeitsspeichers in Byte, die durch den Hintergrundbereinigungsprozess bereinigt wird.|Keine Dimensionen|
|CleanerMemoryNonshrinkable|Arbeitsspeicher: Bereinigung – nicht verkleinerbarer Arbeitsspeicher|Byte|Durchschnitt|Die Menge des Arbeitsspeichers in Byte, die nicht durch den Hintergrundbereinigungsprozess bereinigt wird.|Keine Dimensionen|
|MemoryUsage|Arbeitsspeicher: Speicherauslastung|Byte|Durchschnitt|Speicherauslastung des Serverprozesses, wie bei der Berechnung des Arbeitsspeicherpreises für die Bereinigung verwendet. Entspricht dem Indikator „Process\PrivateBytes“ zuzüglich der Größe der im Speicher abgebildeten Daten. Vom xVelocity-In-Memory-Analysemodul (VertiPaq) abgebildeter oder belegter Arbeitsspeicher, der über die Arbeitsspeichergrenze des xVelocity-Moduls hinausgeht, wird dabei ignoriert.|Keine Dimensionen|
|MemoryLimitHard|Arbeitsspeicher: Grenzwert für den festen Speicher|Byte|Durchschnitt|Grenzwert für den festen Speicher gemäß Konfigurationsdatei.|Keine Dimensionen|
|MemoryLimitHigh|Arbeitsspeicher: Obere Arbeitsspeichergrenze|Byte|Durchschnitt|Oberer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|Keine Dimensionen|
|MemoryLimitLow|Arbeitsspeicher: Untere Arbeitsspeichergrenze|Byte|Durchschnitt|Unterer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|Keine Dimensionen|
|MemoryLimitVertiPaq|Arbeitsspeicher: VertiPaq-Arbeitsspeichergrenze|Byte|Durchschnitt|In-Memory-Grenzwert gemäß Konfigurationsdatei.|Keine Dimensionen|
|Kontingent|Arbeitsspeicher: Kontingent|Byte|Durchschnitt|Aktuelles Arbeitsspeicherkontingent in Byte. Das Arbeitsspeicherkontingent wird auch als Speicherzuweisung oder Speicherreservierung bezeichnet.|Keine Dimensionen|
|QuotaBlocked|Arbeitsspeicher: Kontingent blockiert|Anzahl|Durchschnitt|Aktuelle Anzahl von Kontingentanforderungen, die blockiert werden, bis andere Arbeitsspeicherkontingente freigegeben werden.|Keine Dimensionen|
|VertiPaqNonpaged|Arbeitsspeicher: Nicht ausgelagerte VertiPaq-Daten|Byte|Durchschnitt|Bytes von Arbeitsspeicher, die im Arbeitssatz zur Verwendung durch das In-Memory-Modul gesperrt sind.|Keine Dimensionen|
|VertiPaqPaged|Arbeitsspeicher: Ausgelagerte VertiPaq-Daten|Byte|Durchschnitt|Bytes von ausgelagertem Arbeitsspeicher, die für In-Memory-Daten verwendet werden.|Keine Dimensionen|
|RowsReadPerSec|Verarbeitung: Gelesene Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der aus allen relationalen Datenbanken gelesenen Zeilen.|Keine Dimensionen|
|RowsConvertedPerSec|Verarbeitung: Konvertierte Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der Zeilen, die bei der Verarbeitung konvertiert werden.|Keine Dimensionen|
|RowsWrittenPerSec|Verarbeitung: Geschriebene Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der Zeilen, die bei der Verarbeitung geschrieben werden.|Keine Dimensionen|
|CommandPoolBusyThreads|Threads: Ausgelastete Threads im Befehlspool|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Befehlsthreadpool.|Keine Dimensionen|
|CommandPoolIdleThreads|Threads: Leerlaufthreads im Befehlspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Befehlsthreadpool.|Keine Dimensionen|
|LongParsingBusyThreads|Threads: Ausgelastete lange Analysethreads|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Pool für lange Analysethreads.|Keine Dimensionen|
|LongParsingIdleThreads|Threads: Lange Analysethreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Pool für lange Analysethreads.|Keine Dimensionen|
|LongParsingJobQueueLength|Threads: Warteschlangenlänge für lange Analyseaufträge|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Pools für lange Analysethreads.|Keine Dimensionen|
|ProcessingPoolBusyIOJobThreads|Threads: Ausgelastete Threads für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Threads, die E/A-Aufträge im Verarbeitungsthreadpool ausführen.|Keine Dimensionen|
|ProcessingPoolBusyNonIOThreads|Threads: Ausgelastete Nicht-E/A-Threads im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Threads, die Nicht-E/A-Aufträge im Verarbeitungsthreadpool ausführen.|Keine Dimensionen|
|ProcessingPoolIOJobQueueLength|Threads: Warteschlangenlänge für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools.|Keine Dimensionen|
|ProcessingPoolIdleIOJobThreads|Threads: Leerlaufthreads für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|Keine Dimensionen|
|ProcessingPoolIdleNonIOThreads|Threads: Nicht-E/A-Leerlaufthreads im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Verarbeitungsthreadpool, die für Nicht-E/A-Aufträge vorgesehen sind.|Keine Dimensionen|
|QueryPoolIdleThreads|Threads: Abfragepoolthreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|Keine Dimensionen|
|QueryPoolJobQueueLength|Threads: Auftragswarteschlangenlänge für Abfragepool|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Abfragethreadpools.|Keine Dimensionen|
|ShortParsingBusyThreads|Threads: Ausgelastete kurze Analysethreads|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Pool für kurze Analysethreads.|Keine Dimensionen|
|ShortParsingIdleThreads|Threads: Kurze Analysethreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Pool für kurze Analysethreads.|Keine Dimensionen|
|ShortParsingJobQueueLength|Threads: Warteschlangenlänge für kurze Analyseaufträge|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Pools für kurze Analysethreads.|Keine Dimensionen|
|memory_thrashing_metric|Arbeitsspeicherüberlastung|Prozent|Durchschnitt|Durchschnittliche Arbeitsspeicherüberlastung.|Keine Dimensionen|
|mashup_engine_qpu_metric|M-Modul – QPU|Anzahl|Durchschnitt|QPU-Nutzung durch Mashup-Modulprozesse|Keine Dimensionen|
|mashup_engine_memory_metric|M-Modul – Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeichernutzung durch Mashup-Modulprozesse|Keine Dimensionen|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalRequests|Total Gateway Requests (Gatewayanforderungen gesamt)|Anzahl|Gesamt|Anzahl von Gatewayanforderungen|Speicherort, Hostname|
|SuccessfulRequests|Successful Gateway Requests (Erfolgreiche Gatewayanforderungen)|Anzahl|Gesamt|Anzahl von erfolgreichen Gatewayanforderungen|Speicherort, Hostname|
|UnauthorizedRequests|Unauthorized Gateway Requests (Nicht autorisierte Gatewayanforderungen)|Anzahl|Gesamt|Anzahl von nicht autorisierten Gatewayanforderungen|Speicherort, Hostname|
|FailedRequests|Failed Gateway Requests (Fehlgeschlagene Gatewayanforderungen)|Anzahl|Gesamt|Anzahl von Fehlern bei Gatewayanforderungen|Speicherort, Hostname|
|OtherRequests|Other Gateway Requests (Sonstige Gatewayanforderungen)|Anzahl|Gesamt|Anzahl von anderen Gatewayanforderungen|Speicherort, Hostname|
|Duration|Gesamtdauer von Gatewayanforderungen|Millisekunden|Durchschnitt|Gesamtdauer von Gatewayanforderungen in Millisekunden|Speicherort, Hostname|
|Capacity|Kapazität (Vorschau)|Prozent|Maximum|Auslastungsmetrik für ApiManagement-Dienst|Standort|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalJob|Gesamtzahl an Einzelvorgängen (Jobs)|Anzahl|Gesamt|Die Gesamtzahl an Einzelvorgängen (Jobs)|Keine Dimensionen|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CoreCount|Dedizierte Anzahl von Kernen|Anzahl|Gesamt|Gesamtzahl der dedizierten Kerne im Batch-Konto|Keine Dimensionen|
|TotalNodeCount|Dedizierte Knotenanzahl|Anzahl|Gesamt|Gesamtzahl der dedizierten Knoten im Batch-Konto|Keine Dimensionen|
|LowPriorityCoreCount|Anzahl von LowPriority-Kernen|Anzahl|Gesamt|Gesamtzahl der Kerne mit niedriger Priorität im Batch-Konto|Keine Dimensionen|
|TotalLowPriorityNodeCount|Anzahl der Knoten mit niedriger Priorität|Anzahl|Gesamt|Gesamtzahl der Knoten mit niedriger Priorität im Batchkonto|Keine Dimensionen|
|CreatingNodeCount|Anzahl erstellter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die gerade erstellt werden|Keine Dimensionen|
|StartingNodeCount|Anzahl gestarteter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die gerade gestartet werden|Keine Dimensionen|
|WaitingForStartTaskNodeCount|Anzahl von Knoten, die auf den Starttask warten|Anzahl|Gesamt|Anzahl von Knoten, die auf den Abschluss des Starttasks warten|Keine Dimensionen|
|StartTaskFailedNodeCount|Anzahl von Knoten mit Starttaskfehlern|Anzahl|Gesamt|Anzahl von Knoten, bei denen der Starttask nicht durchgeführt werden konnte|Keine Dimensionen|
|IdleNodeCount|Anzahl von Knoten im Leerlauf|Anzahl|Gesamt|Anzahl von Knoten im Leerlauf|Keine Dimensionen|
|OfflineNodeCount|Anzahl von Offlineknoten|Anzahl|Gesamt|Anzahl offline geschalteter Knoten|Keine Dimensionen|
|RebootingNodeCount|Anzahl neu gestarteter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die neu gestartet werden|Keine Dimensionen|
|ReimagingNodeCount|Anzahl von Knoten mit Reimaging|Anzahl|Gesamt|Anzahl von Knoten, für die ein Reimaging durchgeführt wird|Keine Dimensionen|
|RunningNodeCount|Anzahl ausgeführter Knoten|Anzahl|Gesamt|Anzahl ausgeführter Knoten|Keine Dimensionen|
|LeavingPoolNodeCount|Anzahl von Knoten, die den Pool verlassen|Anzahl|Gesamt|Anzahl von Knoten, die den Pool verlassen|Keine Dimensionen|
|UnusableNodeCount|Anzahl nicht verwendbarer Knoten|Anzahl|Gesamt|Anzahl nicht verwendbarer Knoten|Keine Dimensionen|
|PreemptedNodeCount|Anzahl der vorzeitig entfernten Knoten|Anzahl|Gesamt|Die Anzahl der vorzeitig entfernten Knoten|Keine Dimensionen|
|TaskStartEvent|Taskstartereignisse|Anzahl|Gesamt|Gesamtanzahl gestarteter Tasks|Keine Dimensionen|
|TaskCompleteEvent|Taskabschlussereignisse|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Tasks|Keine Dimensionen|
|TaskFailEvent|Taskfehlerereignisse|Anzahl|Gesamt|Gesamtanzahl von Tasks, die mit Fehlerstatus abgeschlossen wurden|Keine Dimensionen|
|PoolCreateEvent|Poolerstellungsereignisse|Anzahl|Gesamt|Gesamtanzahl erstellter Pools|Keine Dimensionen|
|PoolResizeStartEvent|Ereignisse zum Start der Größenänderung von Pools|Anzahl|Gesamt|Gesamtanzahl gestarteter Größenänderungen von Pools|Keine Dimensionen|
|PoolResizeCompleteEvent|Ereignisse zum Abschluss der Größenänderung von Pools|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Größenänderungen von Pools|Keine Dimensionen|
|PoolDeleteStartEvent|Ereignisse zum Starten des Löschvorgangs von Pools|Anzahl|Gesamt|Gesamtanzahl gestarteter Poollöschvorgänge|Keine Dimensionen|
|PoolDeleteCompleteEvent|Ereignisse zum Abschluss des Löschvorgangs von Pools|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Poollöschvorgänge|Keine Dimensionen|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|connectedclients|Verbundene Clients|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed|Vorgänge gesamt|Anzahl|Gesamt||Keine Dimensionen|
|cachehits|Cachetreffer|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses|Cachefehler|Anzahl|Gesamt||Keine Dimensionen|
|getcommands|get-Vorgänge|Anzahl|Gesamt||Keine Dimensionen|
|setcommands|set-Vorgänge|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys|Entfernte Schlüssel|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys|Schlüssel insgesamt|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys|Abgelaufene Schlüssel|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory|Verwendeter Arbeitsspeicher|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss|Verwendeter Arbeitsspeicher (RSS)|Byte|Maximum||Keine Dimensionen|
|serverLoad|Serverlast|Prozent|Maximum||Keine Dimensionen|
|cacheWrite|Cache-Schreibvorgänge|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead|Cache-Lesevorgänge|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime|CPU|Prozent|Maximum||Keine Dimensionen|
|connectedclients0|Verbundene Clients (Shard 0)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed0|Vorgänge gesamt (Shard 0)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits0|Cachetreffer (Shard 0)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses0|Cachefehler (Shard 0)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands0|Get-Vorgänge (Shard 0)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands0|Set-Vorgänge (Shard 0)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys0|Entfernte Schlüssel (Shard 0)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys0|Schlüssel gesamt (Shard 0)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys0|Abgelaufene Schlüssel (Shard 0)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory0|Verwendeter Arbeitsspeicher (Shard 0)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss0|Verwendeter Arbeitsspeicher (RSS, Shard 0)|Byte|Maximum||Keine Dimensionen|
|serverLoad0|Serverauslastung (Shard 0)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite0|Cacheschreibvorgänge (Shard 0)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead0|Cachelesevorgänge (Shard 0)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime0|CPU (Shard 0)|Prozent|Maximum||Keine Dimensionen|
|connectedclients1|Verbundene Clients (Shard 1)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed1|Vorgänge gesamt (Shard 1)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits1|Cachetreffer (Shard 1)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses1|Cachefehler (Shard 1)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands1|Get-Vorgänge (Shard 1)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands1|Set-Vorgänge (Shard 1)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys1|Entfernte Schlüssel (Shard 1)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys1|Schlüssel gesamt (Shard 1)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys1|Abgelaufene Schlüssel (Shard 1)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory1|Verwendeter Arbeitsspeicher (Shard 1)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss1|Verwendeter Arbeitsspeicher (RSS, Shard 1)|Byte|Maximum||Keine Dimensionen|
|serverLoad1|Serverauslastung (Shard 1)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite1|Cacheschreibvorgänge (Shard 1)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead1|Cachelesevorgänge (Shard 1)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime1|CPU (Shard 1)|Prozent|Maximum||Keine Dimensionen|
|connectedclients2|Verbundene Clients (Shard 2)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed2|Vorgänge gesamt (Shard 2)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits2|Cachetreffer (Shard 2)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses2|Cachefehler (Shard 2)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands2|Get-Vorgänge (Shard 2)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands2|Set-Vorgänge (Shard 2)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys2|Entfernte Schlüssel (Shard 2)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys2|Schlüssel gesamt (Shard 2)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys2|Abgelaufene Schlüssel (Shard 2)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory2|Verwendeter Arbeitsspeicher (Shard 2)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss2|Verwendeter Arbeitsspeicher (RSS, Shard 2)|Byte|Maximum||Keine Dimensionen|
|serverLoad2|Serverauslastung (Shard 2)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite2|Cacheschreibvorgänge (Shard 2)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead2|Cachelesevorgänge (Shard 2)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime2|CPU (Shard 2)|Prozent|Maximum||Keine Dimensionen|
|connectedclients3|Verbundene Clients (Shard 3)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed3|Vorgänge gesamt (Shard 3)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits3|Cachetreffer (Shard 3)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses3|Cachefehler (Shard 3)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands3|Get-Vorgänge (Shard 3)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands3|Set-Vorgänge (Shard 3)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys3|Entfernte Schlüssel (Shard 3)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys3|Schlüssel gesamt (Shard 3)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys3|Abgelaufene Schlüssel (Shard 3)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory3|Verwendeter Arbeitsspeicher (Shard 3)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss3|Verwendeter Arbeitsspeicher (RSS, Shard 3)|Byte|Maximum||Keine Dimensionen|
|serverLoad3|Serverauslastung (Shard 3)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite3|Cacheschreibvorgänge (Shard 3)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead3|Cachelesevorgänge (Shard 3)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime3|CPU (Shard 3)|Prozent|Maximum||Keine Dimensionen|
|connectedclients4|Verbundene Clients (Shard 4)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed4|Vorgänge gesamt (Shard 4)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits4|Cachetreffer (Shard 4)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses4|Cachefehler (Shard 4)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands4|Get-Vorgänge (Shard 4)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands4|Set-Vorgänge (Shard 4)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys4|Entfernte Schlüssel (Shard 4)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys4|Schlüssel gesamt (Shard 4)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys4|Abgelaufene Schlüssel (Shard 4)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory4|Verwendeter Arbeitsspeicher (Shard 4)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss4|Verwendeter Arbeitsspeicher (RSS, Shard 4)|Byte|Maximum||Keine Dimensionen|
|serverLoad4|Serverauslastung (Shard 4)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite4|Cacheschreibvorgänge (Shard 4)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead4|Cachelesevorgänge (Shard 4)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime4|CPU (Shard 4)|Prozent|Maximum||Keine Dimensionen|
|connectedclients5|Verbundene Clients (Shard 5)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed5|Vorgänge gesamt (Shard 5)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits5|Cachetreffer (Shard 5)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses5|Cachefehler (Shard 5)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands5|Get-Vorgänge (Shard 5)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands5|Set-Vorgänge (Shard 5)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys5|Entfernte Schlüssel (Shard 5)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys5|Schlüssel gesamt (Shard 5)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys5|Abgelaufene Schlüssel (Shard 5)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory5|Verwendeter Arbeitsspeicher (Shard 5)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss5|Verwendeter Arbeitsspeicher (RSS, Shard 5)|Byte|Maximum||Keine Dimensionen|
|serverLoad5|Serverauslastung (Shard 5)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite5|Cacheschreibvorgänge (Shard 5)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead5|Cachelesevorgänge (Shard 5)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime5|CPU (Shard 5)|Prozent|Maximum||Keine Dimensionen|
|connectedclients6|Verbundene Clients (Shard 6)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed6|Vorgänge gesamt (Shard 6)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits6|Cachetreffer (Shard 6)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses6|Cachefehler (Shard 6)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands6|Get-Vorgänge (Shard 6)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands6|Set-Vorgänge (Shard 6)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys6|Entfernte Schlüssel (Shard 6)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys6|Schlüssel gesamt (Shard 6)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys6|Abgelaufene Schlüssel (Shard 6)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory6|Verwendeter Arbeitsspeicher (Shard 6)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss6|Verwendeter Arbeitsspeicher (RSS, Shard 6)|Byte|Maximum||Keine Dimensionen|
|serverLoad6|Serverauslastung (Shard 6)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite6|Cacheschreibvorgänge (Shard 6)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead6|Cachelesevorgänge (Shard 6)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime6|CPU (Shard 6)|Prozent|Maximum||Keine Dimensionen|
|connectedclients7|Verbundene Clients (Shard 7)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed7|Vorgänge gesamt (Shard 7)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits7|Cachetreffer (Shard 7)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses7|Cachefehler (Shard 7)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands7|Get-Vorgänge (Shard 7)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands7|Set-Vorgänge (Shard 7)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys7|Entfernte Schlüssel (Shard 7)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys7|Schlüssel gesamt (Shard 7)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys7|Abgelaufene Schlüssel (Shard 7)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory7|Verwendeter Arbeitsspeicher (Shard 7)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss7|Verwendeter Arbeitsspeicher (RSS, Shard 7)|Byte|Maximum||Keine Dimensionen|
|serverLoad7|Serverauslastung (Shard 7)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite7|Cacheschreibvorgänge (Shard 7)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead7|Cachelesevorgänge (Shard 7)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime7|CPU (Shard 7)|Prozent|Maximum||Keine Dimensionen|
|connectedclients8|Verbundene Clients (Shard 8)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed8|Vorgänge gesamt (Shard 8)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits8|Cachetreffer (Shard 8)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses8|Cachefehler (Shard 8)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands8|Get-Vorgänge (Shard 8)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands8|Set-Vorgänge (Shard 8)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys8|Entfernte Schlüssel (Shard 8)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys8|Schlüssel gesamt (Shard 8)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys8|Abgelaufene Schlüssel (Shard 8)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory8|Verwendeter Arbeitsspeicher (Shard 8)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss8|Verwendeter Arbeitsspeicher (RSS, Shard 8)|Byte|Maximum||Keine Dimensionen|
|serverLoad8|Serverauslastung (Shard 8)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite8|Cacheschreibvorgänge (Shard 8)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead8|Cachelesevorgänge (Shard 8)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime8|CPU (Shard 8)|Prozent|Maximum||Keine Dimensionen|
|connectedclients9|Verbundene Clients (Shard 9)|Anzahl|Maximum||Keine Dimensionen|
|totalcommandsprocessed9|Vorgänge gesamt (Shard 9)|Anzahl|Gesamt||Keine Dimensionen|
|cachehits9|Cachetreffer (Shard 9)|Anzahl|Gesamt||Keine Dimensionen|
|cachemisses9|Cachefehler (Shard 9)|Anzahl|Gesamt||Keine Dimensionen|
|getcommands9|Get-Vorgänge (Shard 9)|Anzahl|Gesamt||Keine Dimensionen|
|setcommands9|Set-Vorgänge (Shard 9)|Anzahl|Gesamt||Keine Dimensionen|
|evictedkeys9|Entfernte Schlüssel (Shard 9)|Anzahl|Gesamt||Keine Dimensionen|
|totalkeys9|Schlüssel gesamt (Shard 9)|Anzahl|Maximum||Keine Dimensionen|
|expiredkeys9|Abgelaufene Schlüssel (Shard 9)|Anzahl|Gesamt||Keine Dimensionen|
|usedmemory9|Verwendeter Arbeitsspeicher (Shard 9)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss9|Verwendeter Arbeitsspeicher (RSS, Shard 9)|Byte|Maximum||Keine Dimensionen|
|serverLoad9|Serverauslastung (Shard 9)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite9|Cacheschreibvorgänge (Shard 9)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead9|Cachelesevorgänge (Shard 9)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime9|CPU (Shard 9)|Prozent|Maximum||Keine Dimensionen|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Disk Read Bytes/Sec|Datenträgerlesevorgänge|Bytes pro Sekunde|Durchschnitt|Durchschnitt an Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Disk Write Bytes/Sec|Datenträgerschreibvorgänge|Bytes pro Sekunde|Durchschnitt|Durchschnitt an Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalCalls|Aufrufe gesamt|Anzahl|Gesamt|Gesamtanzahl von Aufrufen|Keine Dimensionen|
|SuccessfulCalls|Erfolgreiche Aufrufe|Anzahl|Gesamt|Anzahl erfolgreicher Aufrufe|Keine Dimensionen|
|TotalErrors|Fehler insgesamt|Anzahl|Gesamt|Gesamtzahl von Aufrufen mit Fehlerantwort (HTTP-Antwortcode 4xx oder 5xx)|Keine Dimensionen|
|BlockedCalls|Blockierte Aufrufe|Anzahl|Gesamt|Anzahl von Aufrufen, die das Raten- oder Kontingentlimit überschritten haben|Keine Dimensionen|
|ServerErrors|Serverfehler|Anzahl|Gesamt|Anzahl von Aufrufen mit internem Dienstfehler (HTTP-Antwortcode 5xx)|Keine Dimensionen|
|ClientErrors|Clientfehler|Anzahl|Gesamt|Anzahl von Aufrufen mit Fehler auf Clientseite (HTTP-Antwortcode 4xx)|Keine Dimensionen|
|DataIn|Eingehende Daten|Byte|Gesamt|Menge eingehender Daten in Byte|Keine Dimensionen|
|DataOut|Datenausgabe|Byte|Gesamt|Menge ausgehender Daten in Byte|Keine Dimensionen|
|Latenz|Latenz|Millisekunden|Durchschnitt|Latenz in Millisekunden|Keine Dimensionen|
|CharactersTranslated|Übersetzte Zeichen|Anzahl|Gesamt|Gesamtanzahl von Zeichen in einer eingehenden Textanforderung|Keine Dimensionen|
|SpeechSessionDuration|Dauer der Sprachsitzung|Sekunden|Gesamt|Gesamtdauer der Sprachsitzung in Sekunden|Keine Dimensionen|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Anzahl|Durchschnitt|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine Dimensionen|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Anzahl|Durchschnitt|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine Dimensionen|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Anzahl|Durchschnitt|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine Dimensionen|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Anzahl|Durchschnitt|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine Dimensionen|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Anzahl|Durchschnitt|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine Dimensionen|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Anzahl|Durchschnitt|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine Dimensionen|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|DCIApiCalls|Customer Insights-API-Aufrufe|Anzahl|Gesamt||Keine Dimensionen|
|DCIMappingImportOperationSuccessfulLines|Erfolgreiche Zeilen beim Zuordnen von Importvorgängen|Anzahl|Gesamt||Keine Dimensionen|
|DCIMappingImportOperationFailedLines|Fehlgeschlagene Zeilen beim Zuordnen von Importvorgängen|Anzahl|Gesamt||Keine Dimensionen|
|DCIMappingImportOperationTotalLines|Gesamte Zeilen beim Zuordnen von Importvorgängen|Anzahl|Gesamt||Keine Dimensionen|
|DCIMappingImportOperationRuntimeInSeconds|Laufzeit in Sekunden beim Zuordnen von Importvorgängen|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundProfileExportSucceeded|Ausgehender Profilexport erfolgreich|Anzahl|Gesamt||Keine Dimensionen|
|DCIOutboundProfileExportFailed|Fehler beim ausgehenden Profilexport|Anzahl|Gesamt||Keine Dimensionen|
|DCIOutboundProfileExportDuration|Dauer des ausgehenden Profilexports|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundKpiExportSucceeded|Ausgehender KPI-Export erfolgreich|Anzahl|Gesamt||Keine Dimensionen|
|DCIOutboundKpiExportFailed|Fehler beim ausgehenden KPI-Export|Anzahl|Gesamt||Keine Dimensionen|
|DCIOutboundKpiExportDuration|Dauer des ausgehenden KPI-Exports|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundKpiExportStarted|Ausgehender KPI-Export gestartet|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundKpiRecordCount|Anzahl der ausgehenden KPI-Datensätze|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundProfileExportCount|Anzahl der ausgehenden Profilexporte|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundInitialProfileExportFailed|Fehler beim ausgehenden ursprünglichen Profilexport|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundInitialProfileExportSucceeded|Ausgehender ursprünglicher Profilexport erfolgreich|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundInitialKpiExportFailed|Fehler beim ausgehenden ursprünglichen KPI-Export|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundInitialKpiExportSucceeded|Ausgehender ursprünglicher KPI-Export erfolgreich|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundInitialProfileExportDurationInSeconds|Dauer des ausgehenden ursprünglichen Profilexports in Sekunden|Sekunden|Gesamt||Keine Dimensionen|
|AdlaJobForStandardKpiFailed|Fehler beim ADLA-Vorgang für Standard-KPI in Sekunden|Sekunden|Gesamt||Keine Dimensionen|
|AdlaJobForStandardKpiTimeOut|Timeout bei ADLA-Vorgang für Standard-KPI in Sekunden|Sekunden|Gesamt||Keine Dimensionen|
|AdlaJobForStandardKpiCompleted|ADLA-Vorgang für Standard-KPI abgeschlossen in Sekunden|Sekunden|Gesamt||Keine Dimensionen|
|ImportASAValuesFailed|Anzahl der fehlgeschlagenen Importe von ASA-Werten|Anzahl|Gesamt||Keine Dimensionen|
|ImportASAValuesSucceeded|Anzahl der erfolgreichen Importe von ASA-Werten|Anzahl|Gesamt||Keine Dimensionen|
|DCIProfilesCount|Anzahl von Profilinstanzen|Anzahl|Last (Letzter)||Keine Dimensionen|
|DCIInteractionsPerMonthCount|Anzahl von Interaktionen pro Monat|Anzahl|Last (Letzter)||Keine Dimensionen|
|DCIKpisCount|Anzahl von KPIs|Anzahl|Last (Letzter)||Keine Dimensionen|
|DCISegmentsCount|Anzahl von Segmenten|Anzahl|Last (Letzter)||Keine Dimensionen|
|DCIPredictiveMatchPoliciesCount|Anzahl von vorhersagbaren Treffern|Anzahl|Last (Letzter)||Keine Dimensionen|
|DCIPredictionsCount|Anzahl von Vorhersagen|Anzahl|Last (Letzter)||Keine Dimensionen|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|JobEndedSuccess|Erfolgreiche Vorgänge|Anzahl|Gesamt|Die Anzahl der erfolgreichen Vorgänge|Keine Dimensionen|
|JobEndedFailure|Fehlgeschlagene Vorgänge|Anzahl|Gesamt|Die Anzahl der fehlgeschlagenen Vorgänge|Keine Dimensionen|
|JobEndedCancelled|Abgebrochene Vorgänge|Anzahl|Gesamt|Die Anzahl der abgebrochenen Vorgänge|Keine Dimensionen|
|JobAUEndedSuccess|Erfolgreiche AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für erfolgreiche Vorgänge|Keine Dimensionen|
|JobAUEndedFailure|Fehlgeschlagene AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für fehlgeschlagene Vorgänge|Keine Dimensionen|
|JobAUEndedCancelled|Abgebrochene AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für abgebrochene Vorgänge|Keine Dimensionen|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalStorage|Gesamtspeicher|Byte|Maximum|Gesamtmenge der im Konto gespeicherten Daten|Keine Dimensionen|
|DataWritten|Geschriebene Daten|Byte|Gesamt|Gesamtmenge der in das Konto geschriebenen Daten|Keine Dimensionen|
|DataRead|Gelesene Daten|Byte|Gesamt|Gesamtmenge der im Konto gelesenen Daten|Keine Dimensionen|
|WriteRequests|Schreibanforderungen|Anzahl|Gesamt|Anzahl der Datenschreibanforderungen für das Konto|Keine Dimensionen|
|ReadRequests|Leseanforderungen|Anzahl|Gesamt|Anzahl der Datenleseanforderungen für das Konto|Keine Dimensionen|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Durchschnitt|CPU in Prozent|Keine Dimensionen|
|compute_limit|Grenzwert für Computeeinheit|Anzahl|Durchschnitt|Grenzwert für Computeeinheit|Keine Dimensionen|
|compute_consumption_percent|Prozentsatz von Computeeinheit|Prozent|Durchschnitt|Prozentsatz von Computeeinheit|Keine Dimensionen|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Durchschnitt|Arbeitsspeicher in Prozent|Keine Dimensionen|
|io_consumption_percent|E/A in Prozent|Prozent|Durchschnitt|E/A in Prozent|Keine Dimensionen|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|Keine Dimensionen|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|Keine Dimensionen|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|Keine Dimensionen|
|active_connections|Aktive Verbindungen gesamt|Anzahl|Durchschnitt|Aktive Verbindungen gesamt|Keine Dimensionen|
|connections_failed|Fehlerhafte Verbindungen gesamt|Anzahl|Durchschnitt|Fehlerhafte Verbindungen gesamt|Keine Dimensionen|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Durchschnitt|CPU in Prozent|Keine Dimensionen|
|compute_limit|Grenzwert für Computeeinheit|Anzahl|Durchschnitt|Grenzwert für Computeeinheit|Keine Dimensionen|
|compute_consumption_percent|Prozentsatz von Computeeinheit|Prozent|Durchschnitt|Prozentsatz von Computeeinheit|Keine Dimensionen|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Durchschnitt|Arbeitsspeicher in Prozent|Keine Dimensionen|
|io_consumption_percent|E/A in Prozent|Prozent|Durchschnitt|E/A in Prozent|Keine Dimensionen|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|Keine Dimensionen|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|Keine Dimensionen|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|Keine Dimensionen|
|active_connections|Aktive Verbindungen gesamt|Anzahl|Durchschnitt|Aktive Verbindungen gesamt|Keine Dimensionen|
|connections_failed|Fehlerhafte Verbindungen gesamt|Anzahl|Durchschnitt|Fehlerhafte Verbindungen gesamt|Keine Dimensionen|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten|Keine Dimensionen|
|d2c.telemetry.ingress.success|Telemetry messages sent (Gesendete Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden|Keine Dimensionen|
|c2d.commands.egress.complete.success|Commands completed (Abgeschlossene Befehle)|Anzahl|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät erfolgreich abgeschlossen wurden|Keine Dimensionen|
|c2d.commands.egress.abandon.success|Commands abandoned (Abgebrochene Befehle)|Anzahl|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgebrochen wurden|Keine Dimensionen|
|c2d.commands.egress.reject.success|Commands rejected (Abgelehnte Befehle)|Anzahl|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgelehnt wurden|Keine Dimensionen|
|devices.totalDevices|Total devices (Geräte gesamt)|Anzahl|Gesamt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine Dimensionen|
|devices.connectedDevices.allProtocol|Verbundene Geräte|Anzahl|Gesamt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine Dimensionen|
|d2c.telemetry.egress.success|Telemetry messages delivered (Übermittelte Telemetrienachrichten)|Anzahl|Gesamt|Erfolgreich auf Endpunkte geschriebene Nachrichten (Häufigkeit gesamt)|Keine Dimensionen|
|d2c.telemetry.egress.dropped|Dropped messages (Gelöschte Nachrichten)|Anzahl|Gesamt|Anzahl der Nachrichten, die aufgrund eines inaktiven Zustellungsendpunkts gelöscht wurden|Keine Dimensionen|
|d2c.telemetry.egress.orphaned|Verwaiste Nachrichten|Anzahl|Gesamt|Anzahl von Nachrichten, die keiner Route entsprechen (einschließlich der Fallbackroute)|Keine Dimensionen|
|d2c.telemetry.egress.invalid|Invalid messages (Ungültige Nachrichten)|Anzahl|Gesamt|Anzahl von Nachrichten, die aufgrund von Inkompatibilität mit dem Endpunkt nicht übermittelt wurden|Keine Dimensionen|
|d2c.telemetry.egress.fallback|Messages matching fallback condition (Nachrichten, die die Fallbackbedingung erfüllen)|Anzahl|Gesamt|Gibt die Anzahl von Nachrichten an, die auf den Fallbackendpunkt geschrieben werden.|Keine Dimensionen|
|d2c.endpoints.egress.eventHubs|Messages delivered to Event Hub endpoints (An Event Hub-Endpunkte übermittelte Nachrichten)|Anzahl|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Event Hub-Endpunkte geschrieben wurden.Gibt an, wie oft Nachrichten erfolgreich auf Event Hub-Endpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.eventHubs|Message latency for Event Hub endpoints (Nachrichtenwartezeit für Event Hub-Endpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Event Hub-Endpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.serviceBusQueues|Messages delivered to Service Bus Queue endpoints (An Service Bus-Warteschlangenendpunkte übermittelte Nachrichten)|Anzahl|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Service Bus-Warteschlangenendpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.serviceBusQueues|Message latency for Service Bus Queue endpoints (Nachrichtenwartezeit für Service Bus-Warteschlangenendpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.serviceBusTopics|Messages delivered to Service Bus Topic endpoints (An Service Bus-Themenendpunkte übermittelte Nachrichten)|Anzahl|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Service Bus-Themenendpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.serviceBusTopics|Message latency for Service Bus Topic endpoints (Nachrichtenwartezeit für Service Bus-Themenendpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Themenendpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.builtIn.events|Messages delivered to the built-in endpoint (messages/events) (An den integrierten Endpunkt (messages/events) übermittelte Nachrichten)|Anzahl|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf den integrierten Endpunkt (messages/events) geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.builtIn.events|Message latency for the built-in endpoint (messages/events) (Nachrichtenwartezeit für den integrierten Endpunkt (messages/events))|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht beim integrierten Endpunkt (Nachrichten/Ereignisse) in Millisekunden |Keine Dimensionen|
|d2c.endpoints.egress.storage|An Storage-Endpunkte übermittelte Nachrichten|Anzahl|Gesamt|Anzahl der erfolgreichen Schreibvorgänge von Nachrichten auf Storage-Endpunkten|Keine Dimensionen|
|d2c.endpoints.latency.storage|Nachrichtenwartezeit für Storage-Endpunkte|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht auf einem Storage-Endpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.storage.bytes|In den Speicher geschriebene Daten|Byte|Gesamt|Menge der Daten in Bytes, die auf Storage-Endpunkten geschrieben wurden|Keine Dimensionen|
|d2c.endpoints.egress.storage.blobs|In den Speicher geschriebene Blobs|Anzahl|Gesamt|Anzahl von Blobs, die auf Storage-Endpunkten geschrieben werden|Keine Dimensionen|
|d2c.twin.read.success|Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.read.failure|Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.read.size|Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.success|Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.failure|Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.size|Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Gerät initiiert wurden.|Keine Dimensionen|
|c2d.methods.success|Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe erfolgreich durchgeführt wurden.|Keine Dimensionen|
|c2d.methods.failure|Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe nicht erfolgreich waren.|Keine Dimensionen|
|c2d.methods.requestSize|Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen direkten Methodenaufrufe an.|Keine Dimensionen|
|c2d.methods.responseSize|Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Antworten für die direkte Methode an.|Keine Dimensionen|
|c2d.twin.read.success|Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.read.failure|Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.read.size|Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.success|Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.failure|Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.size|Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Back-End initiiert wurden.|Keine Dimensionen|
|twinQueries.success|Successful twin queries (Erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine Dimensionen|
|twinQueries.failure|Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine Dimensionen|
|twinQueries.resultSize|Twin queries result size (Ergebnisgröße von Zwillingsabfragen)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert der Ergebnisgröße aller erfolgreichen Zwillingsabfragen.|Keine Dimensionen|
|jobs.createTwinUpdateJob.success|Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine Dimensionen|
|jobs.createTwinUpdateJob.failure|Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen nicht erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine Dimensionen|
|jobs.createDirectMethodJob.success|Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine Dimensionen|
|jobs.createDirectMethodJob.failure|Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine Dimensionen|
|jobs.listJobs.success|Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.listJobs.failure|Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.cancelJob.success|Successful job cancellations (Erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine Dimensionen|
|jobs.cancelJob.failure|Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine Dimensionen|
|jobs.queryJobs.success|Successful job queries (Erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.queryJobs.failure|Failed job queries (Nicht erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.completed|Abgeschlossene Aufträge|Anzahl|Gesamt|Gibt die Anzahl von abgeschlossenen Aufträgen an.|Keine Dimensionen|
|jobs.failed|Fehlerhafte Aufträge|Anzahl|Gesamt|Gibt die Anzahl aller fehlerhaften Aufträge an.|Keine Dimensionen|
|d2c.telemetry.ingress.sendThrottle|Anzahl von Drosselungsfehlern|Anzahl|Gesamt|Anzahl von Drosselungsfehlern aufgrund von Drosselungen des Gerätedurchsatzes|Keine Dimensionen|
|dailyMessageQuotaUsed|Gesamtzahl verwendeter Nachrichten|Anzahl|Durchschnitt|Gesamtzahl der heute verwendeten Nachrichten|Keine Dimensionen|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|RegistrationAttempts|Registrierungsversuche|Anzahl|Gesamt|Anzahl von versuchten Geräteregistrierungen|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Zugewiesene Geräte|Anzahl|Gesamt|Anzahl von Geräten, die einem IoT-Hub zugewiesen sind|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Nachweisversuche|Anzahl|Gesamt|Anzahl von versuchten Gerätenachweisen|ProvisioningServiceName, Status, Protokoll|

## <a name="microsoftdocumentdbdatabaseaccounts-cosmosdb"></a>Microsoft.DocumentDB/databaseAccounts (CosmosDB)
|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TotalRequests|Anzahl von Anforderungen|Anzahl|Anzahl|Anzahl von gesendeten Anforderungen|DatabaseAccount, CollectionName, DatabaseName, Region, StatusCode|
|MongoRequests|Mongo-Anforderungen|Anzahl|Anzahl|Anzahl von gestellten Mongo-Anforderungen|DatabaseAccount, CollectionName, DatabaseName, Region, ErrorCode, CommandName|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|INREQS|Eingehende Sendeanforderungen|Anzahl|Gesamt|Gesamtanzahl von eingehenden Sendeanforderungen für einen Namespace|Keine Dimensionen|
|SUCCREQ|Erfolgreiche Anforderungen|Anzahl|Gesamt|Gesamtzahl der erfolgreichen Anforderungen für einen Namespace|Keine Dimensionen|
|FAILREQ|Anforderungsfehler|Anzahl|Gesamt|Gesamtzahl der fehlgeschlagenen Anforderungen für einen Namespace|Keine Dimensionen|
|SVRBSY|Fehler durch ausgelasteten Server|Anzahl|Gesamt|Gesamtzahl der Fehler durch ausgelasteten Server für einen Namespace|Keine Dimensionen|
|INTERR|Interne Serverfehler|Anzahl|Gesamt|Gesamtzahl der internen Serverfehler für einen Namespace|Keine Dimensionen|
|MISCERR|Andere Fehler|Anzahl|Gesamt|Gesamtzahl der fehlgeschlagenen Anforderungen für einen Namespace|Keine Dimensionen|
|INMSGS|Eingehende Nachrichten (veraltet)|Anzahl|Gesamt|Gesamtzahl von eingehenden Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für eingehende Nachrichten.|Keine Dimensionen|
|EHINMSGS|Eingehende Nachrichten|Anzahl|Gesamt|Gesamtzahl der eingehenden Nachrichten für einen Namespace|Keine Dimensionen|
|OUTMSGS|Ausgehende Nachrichten (veraltet)|Anzahl|Gesamt|Gesamtzahl von ausgehenden Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für ausgehende Nachrichten.|Keine Dimensionen|
|EHOUTMSGS|Ausgehende Nachrichten|Anzahl|Gesamt|Gesamtzahl der ausgehenden Nachrichten für einen Namespace|Keine Dimensionen|
|EHINMBS|Eingehende Bytes (veraltet)|Byte|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für eingehende Bytes.|Keine Dimensionen|
|EHINBYTES|Eingehende Bytes|Byte|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace|Keine Dimensionen|
|EHOUTMBS|Ausgehende Bytes (veraltet)|Byte|Gesamt|Event Hub-Durchsatz ausgehender Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für ausgehende Bytes.|Keine Dimensionen|
|EHOUTBYTES|Ausgehende Bytes|Byte|Gesamt|Event Hub-Durchsatz ausgehender Nachrichten für einen Namespace|Keine Dimensionen|
|EHABL|Archivierte Backlognachrichten|Anzahl|Gesamt|Event Hub-Archivnachrichten im Backlog für einen Namespace|Keine Dimensionen|
|EHAMSGS|Archivnachrichten|Anzahl|Gesamt|Event Hub-Archivnachrichten in einem Namespace|Keine Dimensionen|
|EHAMBS|Durchsatz von Archivnachrichten|Byte|Gesamt|Event Hub-Durchsatz archivierter Nachrichten in einem Namespace|Keine Dimensionen|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ObservedMetricValue|Beobachteter Metrikwert|Anzahl|Durchschnitt|Der Wert, der beim Ausführen der automatischen Skalierung berechnet wurde.|MetricTriggerSource|
|MetricThreshold|Schwellenwert der Metrik|Anzahl|Durchschnitt|Der bei der Ausführung der automatischen Skalierung konfigurierte Schwellenwert für die automatische Skalierung.|MetricTriggerRule|
|ObservedCapacity|Beobachtete Kapazität|Anzahl|Durchschnitt|Die Kapazität, die beim Ausführen der automatischen Skalierung gemeldet wurde.|Keine Dimensionen|
|ScaleActionsInitiated|Initiierte Skalierungsaktionen|Anzahl|Gesamt|Die Richtung des Skalierungsvorgangs.|ScaleDirection|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|RunsStarted|Gestartete Ausführungen|Anzahl|Gesamt|Anzahl gestarteter Workflowausführungen.|Keine Dimensionen|
|RunsCompleted|Abgeschlossene Ausführungen|Anzahl|Gesamt|Anzahl abgeschlossener Workflowausführungen.|Keine Dimensionen|
|RunsSucceeded|Erfolgreiche Ausführungen|Anzahl|Gesamt|Anzahl erfolgreicher Workflowausführungen.|Keine Dimensionen|
|RunsFailed|Ausführungsfehler|Anzahl|Gesamt|Anzahl fehlerhafter Workflowausführungen.|Keine Dimensionen|
|RunsCancelled|Abgebrochene Ausführungen|Anzahl|Gesamt|Anzahl abgebrochener Workflowausführungen.|Keine Dimensionen|
|RunLatency|Ausführungslatenz|Sekunden|Durchschnitt|Latenz abgeschlossener Workflowausführungen.|Keine Dimensionen|
|RunSuccessLatency|Latenz erfolgreicher Ausführungen|Sekunden|Durchschnitt|Latenz erfolgreicher Workflowausführungen.|Keine Dimensionen|
|RunThrottledEvents|Ereignisse zu gedrosselten Ausführungen|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen oder Triggern.|Keine Dimensionen|
|RunFailurePercentage|Prozentsatz der Ausführungsfehler|Prozent|Gesamt|Prozentsatz fehlerhafter Workflowausführungen|Keine Dimensionen|
|ActionsStarted|Gestartete Aktionen |Anzahl|Gesamt|Anzahl gestarteter Workflowaktionen.|Keine Dimensionen|
|ActionsCompleted|Abgeschlossene Aktionen |Anzahl|Gesamt|Anzahl abgeschlossener Workflowaktionen.|Keine Dimensionen|
|ActionsSucceeded|Erfolgreiche Aktionen |Anzahl|Gesamt|Anzahl erfolgreicher Workflowaktionen.|Keine Dimensionen|
|ActionsFailed|Aktionsfehler|Anzahl|Gesamt|Anzahl fehlerhafter Workflowaktionen.|Keine Dimensionen|
|ActionsSkipped|Übersprungene Aktionen |Anzahl|Gesamt|Anzahl übersprungener Workflowaktionen.|Keine Dimensionen|
|ActionLatency|Aktionslatenz |Sekunden|Durchschnitt|Latenz abgeschlossener Workflowaktionen.|Keine Dimensionen|
|ActionSuccessLatency|Latenz erfolgreicher Aktionen |Sekunden|Durchschnitt|Latenz erfolgreicher Workflowaktionen.|Keine Dimensionen|
|ActionThrottledEvents|Ereignisse zu gedrosselten Aktionen|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen.|Keine Dimensionen|
|TriggersStarted|Gestartete Trigger |Anzahl|Gesamt|Anzahl gestarteter Workflowtrigger.|Keine Dimensionen|
|TriggersCompleted|Abgeschlossene Trigger |Anzahl|Gesamt|Anzahl abgeschlossener Workflowtrigger.|Keine Dimensionen|
|TriggersSucceeded|Erfolgreiche Trigger |Anzahl|Gesamt|Anzahl erfolgreicher Workflowtrigger.|Keine Dimensionen|
|TriggersFailed|Triggerfehler |Anzahl|Gesamt|Anzahl fehlerhafter Workflowtrigger.|Keine Dimensionen|
|TriggersSkipped|Übersprungene Trigger|Anzahl|Gesamt|Anzahl übersprungener Workflowtrigger.|Keine Dimensionen|
|TriggersFired|Ausgelöste Trigger |Anzahl|Gesamt|Anzahl ausgelöster Workflowtrigger.|Keine Dimensionen|
|TriggerLatency|Triggerlatenz |Sekunden|Durchschnitt|Latenz abgeschlossener Workflowtrigger.|Keine Dimensionen|
|TriggerFireLatency|Latenz beim Auslösen von Triggern |Sekunden|Durchschnitt|Latenz ausgelöster Workflowtrigger.|Keine Dimensionen|
|TriggerSuccessLatency|Latenz erfolgreicher Trigger |Sekunden|Durchschnitt|Latenz erfolgreicher Workflowtrigger.|Keine Dimensionen|
|TriggerThrottledEvents|Ereignisse zu gedrosselten Triggern|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowtriggern.|Keine Dimensionen|
|BillableActionExecutions|Ausführungen abrechenbarer Aktionen|Anzahl|Gesamt|Anzahl der Workflowaktionsausführungen, die in Rechnung gestellt werden|Keine Dimensionen|
|BillableTriggerExecutions|Ausführungen abrechenbarer Trigger|Anzahl|Gesamt|Anzahl der Workflowtriggerausführungen, die in Rechnung gestellt werden|Keine Dimensionen|
|TotalBillableExecutions|Gesamtzahl der abrechenbaren Ausführungen|Anzahl|Gesamt|Anzahl der Workflowausführungen, die in Rechnung gestellt werden|Keine Dimensionen|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|VipAvailability|VIP-Verfügbarkeit|Anzahl|Durchschnitt|Verfügbarkeit von VIP-Endpunkten, basierend auf Testergebnissen|VipAddress, VipPort|
|DipAvailability|DIP-Verfügbarkeit|Anzahl|Durchschnitt|Verfügbarkeit von DIP-Endpunkten, basierend auf Testergebnissen|ProtocolType, DipPort, VipAddress, VipPort, DipAddress|
|ByteCount|Byteanzahl|Anzahl|Gesamt|Gesamtanzahl von Bytes, die im Zeitraum übertragen wurden|VipAddress, VipPort, Richtung|
|PacketCount|Paketzahl|Anzahl|Gesamt|Gesamtanzahl von Paketen, die im Zeitraum übertragen wurden|VipAddress, VipPort, Richtung|
|SYNCount|SYN-Anzahl|Anzahl|Gesamt|Gesamtanzahl von SYN-Paketen, die im Zeitraum übertragen wurden|VipAddress, VipPort, Richtung|
|SnatConnectionCount|Anzahl von SNAT-Verbindungen|Anzahl|Gesamt|Gesamtanzahl von neuen SNAT-Verbindungen, die innerhalb des Zeitraums erstellt wurden|VipAddress, DipAddress, ConnectionState|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|PacketsInDDoS|Als DDoS eingehende Pakete|Anzahl pro Sekunde|Durchschnitt|Als DDoS eingehende Pakete|Keine Dimensionen|
|PacketsDroppedDDoS|Als DDoS eingehende gelöschte Pakete|Anzahl pro Sekunde|Durchschnitt|Als DDoS eingehende gelöschte Pakete|Keine Dimensionen|
|PacketsForwardedDDoS|Weitergeleitete als DDoS eingehende Pakete|Anzahl pro Sekunde|Durchschnitt|Weitergeleitete als DDoS eingehende Pakete|Keine Dimensionen|
|TCPPacketsInDDoS|Als DDoS eingehende TCP-Pakete|Anzahl pro Sekunde|Durchschnitt|Als DDoS eingehende TCP-Pakete|Keine Dimensionen|
|TCPPacketsDroppedDDoS|Als DDoS eingehende gelöschte TCP-Pakete|Anzahl pro Sekunde|Durchschnitt|Als DDoS eingehende gelöschte TCP-Pakete|Keine Dimensionen|
|TCPPacketsForwardedDDoS|Weitergeleitete als DDoS eingehende TCP-Pakete|Anzahl pro Sekunde|Durchschnitt|Weitergeleitete als DDoS eingehende TCP-Pakete|Keine Dimensionen|
|UDPPacketsInDDoS|Als DDoS eingehende UDP-Pakete|Anzahl pro Sekunde|Durchschnitt|Als DDoS eingehende UDP-Pakete|Keine Dimensionen|
|UDPPacketsDroppedDDoS|Als DDoS eingehende gelöschte UDP-Pakete|Anzahl pro Sekunde|Durchschnitt|Als DDoS eingehende gelöschte UDP-Pakete|Keine Dimensionen|
|UDPPacketsForwardedDDoS|Weitergeleitete als DDoS eingehende UDP-Pakete|Anzahl pro Sekunde|Durchschnitt|Weitergeleitete als DDoS eingehende UDP-Pakete|Keine Dimensionen|
|BytesInDDoS|Als DDoS eingehende Bytes|Bytes pro Sekunde|Durchschnitt|Als DDoS eingehende Bytes|Keine Dimensionen|
|BytesDroppedDDoS|Als DDoS eingehende gelöschte Bytes|Bytes pro Sekunde|Durchschnitt|Als DDoS eingehende gelöschte Bytes|Keine Dimensionen|
|BytesForwardedDDoS|Weitergeleitete als DDoS eingehende Bytes|Bytes pro Sekunde|Durchschnitt|Weitergeleitete als DDoS eingehende Bytes|Keine Dimensionen|
|TCPBytesInDDoS|Als DDoS eingehende TCP-Bytes|Bytes pro Sekunde|Durchschnitt|Als DDoS eingehende TCP-Bytes|Keine Dimensionen|
|TCPBytesDroppedDDoS|Als DDoS eingehende gelöschte TCP-Bytes|Bytes pro Sekunde|Durchschnitt|Als DDoS eingehende gelöschte TCP-Bytes|Keine Dimensionen|
|TCPBytesForwardedDDoS|Weitergeleitete als DDoS eingehende TCP-Bytes|Bytes pro Sekunde|Durchschnitt|Weitergeleitete als DDoS eingehende TCP-Bytes|Keine Dimensionen|
|UDPBytesInDDoS|Als DDoS eingehende UDP-Bytes|Bytes pro Sekunde|Durchschnitt|Als DDoS eingehende UDP-Bytes|Keine Dimensionen|
|UDPBytesDroppedDDoS|Als DDoS eingehende gelöschte UDP-Bytes|Bytes pro Sekunde|Durchschnitt|Als DDoS eingehende gelöschte UDP-Bytes|Keine Dimensionen|
|UDPBytesForwardedDDoS|Weitergeleitete als DDoS eingehende UDP-Bytes|Bytes pro Sekunde|Durchschnitt|Weitergeleitete als DDoS eingehende UDP-Bytes|Keine Dimensionen|
|IfUnderDDoSAttack|Unter DDoS-Angriff oder nicht|Anzahl|Durchschnitt|Unter DDoS-Angriff oder nicht|Keine Dimensionen|
|DDoSTriggerTCPPackets|Eingehende TCP-Pakete, um die DDoS-Entschärfung auszulösen|Anzahl pro Sekunde|Durchschnitt|Eingehende TCP-Pakete, um die DDoS-Entschärfung auszulösen|Keine Dimensionen|
|DDoSTriggerUDPPackets|Eingehende UDP-Pakete, um die DDoS-Entschärfung auszulösen|Anzahl pro Sekunde|Durchschnitt|Eingehende UDP-Pakete, um die DDoS-Entschärfung auszulösen|Keine Dimensionen|
|VipAvailability|Availability|Anzahl|Durchschnitt|Durchschnittliche Verfügbarkeit der IP-Adresse innerhalb des Zeitraums|Port|
|ByteCount|Byteanzahl|Anzahl|Gesamt|Gesamtanzahl von Bytes, die im Zeitraum übertragen wurden|Port, Richtung|
|PacketCount|Paketzahl|Anzahl|Gesamt|Gesamtanzahl von Paketen, die im Zeitraum übertragen wurden|Port, Richtung|
|SynCount|SYN-Anzahl|Anzahl|Gesamt|Gesamtanzahl von SYN-Paketen, die im Zeitraum übertragen wurden|Port, Richtung|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Durchsatz|Durchsatz|Bytes pro Sekunde|Durchschnitt|Anzahl von Bytes pro Sekunde, die das Application Gateway bereitgestellt hat|Keine Dimensionen|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Bandbreite des Tunnels|Bytes pro Sekunde|Durchschnitt|Durchschnittliche Bandbreite eines Tunnels in Bytes pro Sekunde|ConnectionName, RemoteIP|
|TunnelPeakBandwidth|Maximale Bandbreite des Tunnels|Bytes pro Sekunde|Durchschnitt|Maximale Bandbreite eines Tunnels in Bytes pro Sekunde|ConnectionName, RemoteIP|
|TunnelEgressBytes|Vom Tunnel ausgehende Bytes|Byte|Durchschnitt|Ausgehende Bytes eines Tunnels in einem Intervall von fünf Minuten|ConnectionName, RemoteIP|
|TunnelIngressBytes|In Tunnel eingehende Bytes|Byte|Durchschnitt|Eingehende Bytes eines Tunnels in einem Intervall von fünf Minuten|ConnectionName, RemoteIP|
|TunnelEgressPackets|Vom Tunnel ausgehende Pakete|Anzahl|Durchschnitt|Anzahl von ausgehenden Paketen eines Tunnels in einem Intervall von fünf Minuten|ConnectionName, RemoteIP|
|TunnelIngressPackets|In Tunnel eingehende Pakete|Anzahl|Durchschnitt|Anzahl von eingehenden Paketen eines Tunnels in einem Intervall von fünf Minuten|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Ausgehende gelöschte Pakete eines Tunnels durch einen TS-Konflikt|Anzahl|Durchschnitt|Ausgehende gelöschte Pakete durch einen Konflikt mit dem Datenverkehrsselektor eines Tunnels in einem Intervall von fünf Minuten|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Eingehende gelöschte Pakete eines Tunnels durch einen TS-Konflikt|Anzahl|Durchschnitt|Eingehende gelöschte Pakete durch einen Konflikt mit dem Datenverkehrsselektor eines Tunnels in einem Intervall von fünf Minuten|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BytesIn|BytesIn|Anzahl|Gesamt|In Azure eingehende Bytes|Keine Dimensionen|
|BytesOut|BytesOut|Anzahl|Gesamt|Aus Azure ausgehende Bytes|Keine Dimensionen|
|BitsInPerSecond|BitsInPerSecond|Anzahl pro Sekunde|Durchschnitt|In Azure eingehende Bits pro Sekunde|Keine Dimensionen|
|BitsOutPerSecond|BitsOutPerSecond|Anzahl pro Sekunde|Durchschnitt|Aus Azure ausgehende Bits pro Sekunde|Keine Dimensionen|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|QpsByEndpoint|Abfragen nach zurückgegebenem Endpunkt|Anzahl|Gesamt|Häufigkeit, mit der ein Endpunkt des Traffic Managers innerhalb des angegebenen Zeitrahmens zurückgegeben wurde|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Endpunktstatus nach Endpunkt|Anzahl|Maximum|1, wenn der Teststatus des Endpunkts „Aktiviert“ ist, andernfalls 0.|EndpointName|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|registration.all|Registrierungsvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsvorgängen an (Erstellungen, Aktualisierungen, Abfragen und Löschungen). |Keine Dimensionen|
|registration.create|Registrierungserstellungsvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungserstellungen an.|Keine Dimensionen|
|registration.update|Registrierungsaktualisierungsvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsaktualisierungen an.|Keine Dimensionen|
|registration.get|Registrierungslesevorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsabfragen an.|Keine Dimensionen|
|registration.delete|Registrierungslöschvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungslöschungen an.|Keine Dimensionen|
|incoming|Eingehende Nachrichten|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Übermittlungs-API-Aufrufen an. |Keine Dimensionen|
|incoming.scheduled|Geplante Pushbenachrichtigungen (gesendet)|Anzahl|Gesamt|Geplante Pushbenachrichtigungen (abgebrochen)|Keine Dimensionen|
|incoming.scheduled.cancel|Geplante Pushbenachrichtigungen (abgebrochen)|Anzahl|Gesamt|Geplante Pushbenachrichtigungen (abgebrochen)|Keine Dimensionen|
|scheduled.pending|Ausstehende Pushbenachrichtigungen|Anzahl|Gesamt|Ausstehende Pushbenachrichtigungen|Keine Dimensionen|
|installation.all|Installationsverwaltungsvorgänge|Anzahl|Gesamt|Installationsverwaltungsvorgänge|Keine Dimensionen|
|installation.get|Installationsvorgänge abrufen|Anzahl|Gesamt|Installationsvorgänge abrufen|Keine Dimensionen|
|installation.upsert|Installationsvorgänge erstellen oder aktualisieren|Anzahl|Gesamt|Installationsvorgänge erstellen oder aktualisieren|Keine Dimensionen|
|installation.patch|Patchinstallationsvorgänge|Anzahl|Gesamt|Patchinstallationsvorgänge|Keine Dimensionen|
|installation.delete|Installationsvorgänge löschen|Anzahl|Gesamt|Installationsvorgänge löschen|Keine Dimensionen|
|outgoing.allpns.success|Erfolgreiche Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.allpns.invalidpayload|Nutzlastfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil vom PNS ein Nutzlastfehler zurückgegeben wurde.|Keine Dimensionen|
|outgoing.allpns.pnserror|Fehler des externen Benachrichtigungssystems|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil ein Problem bei der Kommunikation mit dem PNS vorlag (ausschließlich Authentifizierungsprobleme).|Keine Dimensionen|
|outgoing.allpns.channelerror|Kanalfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der Kanal ungültig, nicht der richtigen App zugeordnet, gedrosselt oder abgelaufen war.|Keine Dimensionen|
|outgoing.allpns.badorexpiredchannel|Kanalfehler (unzulässig oder abgelaufen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der Kanal, das Token oder die Registrierungs-ID in der Registrierung abgelaufen oder ungültig war.|Keine Dimensionen|
|outgoing.wns.success|Erfolgreiche WNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.wns.invalidcredentials|WNS-Autorisierungsfehler (ungültige Anmeldeinformationen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden. (Windows Live erkennt die Anmeldeinformationen nicht.)|Keine Dimensionen|
|outgoing.wns.badchannel|WNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der ChannelURI in der Registrierung nicht erkannt wurde (WNS-Status: 404 – Nicht gefunden).|Keine Dimensionen|
|outgoing.wns.expiredchannel|WNS-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Die Anzahl von nicht erfolgreichen Pushvorgängen, weil der ChannelURI abgelaufen ist (WNS-Status: 410 – Fehlend).|Keine Dimensionen|
|outgoing.wns.throttled|Gedrosselte WNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App vom WNS gedrosselt wird (WNS-Status: 406 – Nicht annehmbar).|Keine Dimensionen|
|outgoing.wns.tokenproviderunreachable|WNS-Autorisierungsfehler (nicht erreichbar)|Anzahl|Gesamt|Windows Live ist nicht erreichbar.|Keine Dimensionen|
|outgoing.wns.invalidtoken|WNS-Autorisierungsfehler (ungültiges Token)|Anzahl|Gesamt|Das für WNS bereitgestellte Token ist nicht gültig (WNS-Status: 401 – Nicht autorisiert).|Keine Dimensionen|
|outgoing.wns.wrongtoken|WNS-Autorisierungsfehler (falsches Token)|Anzahl|Gesamt|Das für WNS bereitgestellte Token ist gültig, gilt aber für eine andere Anwendung (WNS-Status: 403 – Verboten). Dieser Fall kann eintreten, wenn der ChannelURI in der Registrierung einer anderen App zugeordnet wird. Stellen Sie sicher, dass die Client-App der App zugeordnet ist, deren Anmeldeinformationen sich im Notification Hub befinden.|Keine Dimensionen|
|outgoing.wns.invalidnotificationformat|Ungültiges WNS-Benachrichtigungsformat|Anzahl|Gesamt|Das Format der Benachrichtigung ist ungültig (WNS-Status: 400). Beachten Sie, dass von WNS nicht alle ungültigen Nutzlasten abgelehnt werden.|Keine Dimensionen|
|outgoing.wns.invalidnotificationsize|WNS-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Die Benachrichtigungsnutzlast ist zu groß (WNS-Status: 413).|Keine Dimensionen|
|outgoing.wns.channelthrottled|WNS-Kanal gedrosselt|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde (WNS-Antwortheader: X-WNS-NotificationStatus:channelThrottled).|Keine Dimensionen|
|outgoing.wns.channeldisconnected|WNS-Kanal nicht verbunden|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde (WNS-Antwortheader: X-WNS-DeviceConnectionStatus: disconnected).|Keine Dimensionen|
|outgoing.wns.dropped|Verworfene WNS-Benachrichtigungen|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde („X-WNS-NotificationStatus: dropped“, aber nicht „X-WNS-DeviceConnectionStatus: disconnected“).|Keine Dimensionen|
|outgoing.wns.pnserror|WNS-Fehler|Anzahl|Gesamt|Die Benachrichtigung wurde aufgrund von Fehlern bei der Kommunikation mit WNS nicht übermittelt.|Keine Dimensionen|
|outgoing.wns.authenticationerror|WNS-Authentifizierungsfehler|Anzahl|Gesamt|Die Benachrichtigung wurde nicht übermittelt, weil Fehler bei der Kommunikation mit Windows Live aufgetreten sind, die Anmeldeinformationen ungültig waren oder das falsche Token verwendet wurde.|Keine Dimensionen|
|outgoing.apns.success|Erfolgreiche APNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.apns.invalidcredentials|APNS-Autorisierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine Dimensionen|
|outgoing.apns.badchannel|APNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das Token ungültig ist (APNS-Statuscode: 8).|Keine Dimensionen|
|outgoing.apns.expiredchannel|APNS-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Die Anzahl von Token, die aufgrund des APNS-Feedbackkanals ungültig gemacht wurden.|Keine Dimensionen|
|outgoing.apns.invalidnotificationsize|APNS-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast zu groß war (APNS-Statuscode: 7).|Keine Dimensionen|
|outgoing.apns.pnserror|APNS-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit APNS aufgetreten sind.|Keine Dimensionen|
|outgoing.gcm.success|Erfolgreiche GCM-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.gcm.invalidcredentials|GCM-Autorisierungsfehler (ungültige Anmeldeinformationen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine Dimensionen|
|outgoing.gcm.badchannel|GCM-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung nicht erkannt wurde (GCM-Ergebnis: Ungültige Registrierung).|Keine Dimensionen|
|outgoing.gcm.expiredchannel|GCM-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung abgelaufen war (GCM-Ergebnis: NotRegistered).|Keine Dimensionen|
|outgoing.gcm.throttled|Gedrosselte GCM-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App von GCM gedrosselt wurde (GCM-Statuscode: 501 - 599 oder result:Unavailable).|Keine Dimensionen|
|outgoing.gcm.invalidnotificationformat|Ungültiges GCM-Benachrichtigungsformat|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast nicht richtig formatiert war (GCM-Ergebnis: InvalidDataKey oder InvalidTtl).|Keine Dimensionen|
|outgoing.gcm.invalidnotificationsize|GCM-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast zu groß war (GCM-Ergebnis: MessageTooBig).|Keine Dimensionen|
|outgoing.gcm.wrongchannel|GCM-Fehler durch falschen Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung nicht der aktuellen App zugeordnet war (GCM-Ergebnis: InvalidPackageName).|Keine Dimensionen|
|outgoing.gcm.pnserror|GCM-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit GCM aufgetreten sind.|Keine Dimensionen|
|outgoing.gcm.authenticationerror|GCM-Authentifizierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die angegebenen Anmeldeinformationen vom PNS nicht akzeptiert wurden, die Anmeldeinformationen blockiert wurden oder die SenderId in der App nicht richtig konfiguriert wurde (GCM-Ergebnis: MismatchedSenderId).|Keine Dimensionen|
|outgoing.mpns.success|Erfolgreiche MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.mpns.invalidcredentials|Ungültige MPNS-Anmeldeinformationen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine Dimensionen|
|outgoing.mpns.badchannel|MPNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der ChannelURI in der Registrierung nicht erkannt wurde (MPNS-Status: 404 – Nicht gefunden).|Keine Dimensionen|
|outgoing.mpns.throttled|Gedrosselte MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App vom MPNS gedrosselt wird (WNS-MPNS: 406 – Nicht annehmbar).|Keine Dimensionen|
|outgoing.mpns.invalidnotificationformat|Ungültiges MPNS-Benachrichtigungsformat|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast der Benachrichtigung zu groß war.|Keine Dimensionen|
|outgoing.mpns.channeldisconnected|MPNS-Kanal nicht verbunden|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Verbindung für den ChannelURI in der Registrierung getrennt wurde (MPNS-Status: 412 – Nicht gefunden).|Keine Dimensionen|
|outgoing.mpns.dropped|Verworfene MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die von MPNS verworfen wurden (MPNS-Antwortheader: X-NotificationStatus: QueueFull or Suppressed).|Keine Dimensionen|
|outgoing.mpns.pnserror|MPNS-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit MPNS aufgetreten sind.|Keine Dimensionen|
|outgoing.mpns.authenticationerror|MPNS-Authentifizierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine Dimensionen|
|notificationhub.pushes|Alle ausgehenden Benachrichtigungen|Anzahl|Gesamt|Alle ausgehenden Benachrichtigungen des Benachrichtigungs-Hub|Keine Dimensionen|
|incoming.all.requests|Alle eingehenden Anforderungen|Anzahl|Gesamt|Gesamtzahl eingehender Anforderungen für einen Notification Hub|Keine Dimensionen|
|incoming.all.failedrequests|Alle eingehenden fehlerhaften Anforderungen|Anzahl|Gesamt|Gesamtzahl eingehender fehlerhafter Anforderungen für einen Notification Hub|Keine Dimensionen|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|SearchLatency|Suchlatenz|Sekunden|Durchschnitt|Durchschnittliche Suchlatenz für den Suchdienst|Keine Dimensionen|
|SearchQueriesPerSecond|Suchabfragen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Suchabfragen pro Sekunde für den Suchdienst|Keine Dimensionen|
|ThrottledSearchQueriesPercentage|Gedrosselte Suchabfragen in Prozent|Prozent|Durchschnitt|Prozentsatz der Suchabfragen, die für den Suchdienst gedrosselt wurden|Keine Dimensionen|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CPUXNS|CPU-Auslastung pro Namespace|Prozent|Maximum|CPU-Auslastungsmetrik für Service Bus-Premium-Namespace|Keine Dimensionen|
|WSXNS|Auslastung der Arbeitsspeichergröße pro Namespace|Prozent|Maximum|Speicherauslastungsmetrik für Service Bus-Premium-Namespace|Keine Dimensionen|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Keine Dimensionen|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|Keine Dimensionen|
|log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|Keine Dimensionen|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|Keine Dimensionen|
|storage|Datenbankgröße gesamt|Byte|Maximum|Datenbankgröße gesamt|Keine Dimensionen|
|connection_successful|Erfolgreiche Verbindungen|Anzahl|Gesamt|Erfolgreiche Verbindungen|Keine Dimensionen|
|connection_failed|Verbindungsfehler|Anzahl|Gesamt|Verbindungsfehler|Keine Dimensionen|
|blocked_by_firewall|Von der Firewall blockiert|Anzahl|Gesamt|Von der Firewall blockiert|Keine Dimensionen|
|deadlock|Deadlocks|Anzahl|Gesamt|Deadlocks|Keine Dimensionen|
|storage_percent|Datenbankgröße als Prozentsatz|Prozent|Maximum|Datenbankgröße als Prozentsatz|Keine Dimensionen|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent|Prozent|Durchschnitt|In-Memory-OLTP-Speicher in Prozent|Keine Dimensionen|
|workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|Keine Dimensionen|
|sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|Keine Dimensionen|
|dtu_limit|DTU-Grenzwert|Anzahl|Durchschnitt|DTU-Grenzwert|Keine Dimensionen|
|dtu_used|DTU-Verbrauch|Anzahl|Durchschnitt|DTU-Verbrauch|Keine Dimensionen|
|dwu_limit|DWU-Grenzwert|Anzahl|Maximum|DWU-Grenzwert|Keine Dimensionen|
|dwu_consumption_percent|DWU in Prozent|Prozent|Maximum|DWU in Prozent|Keine Dimensionen|
|dwu_used|DWU-Verbrauch|Anzahl|Maximum|DWU-Verbrauch|Keine Dimensionen|
|dw_cpu_percent|DW-Knotenebene, CPU-Prozentsatz|Prozent|Durchschnitt|DW-Knotenebene, CPU-Prozentsatz|dw_logical_node_id|
|dw_physical_data_read_percent|DW-Knotenebene, E/A-Prozentsatz für Daten|Prozent|Durchschnitt|DW-Knotenebene, E/A-Prozentsatz für Daten|dw_logical_node_id|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Keine Dimensionen|
|database_cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|DatabaseResourceId|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|Keine Dimensionen|
|database_physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|DatabaseResourceId|
|log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|Keine Dimensionen|
|database_log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|DatabaseResourceId|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|Keine Dimensionen|
|database_dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|DatabaseResourceId|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|Keine Dimensionen|
|workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|Keine Dimensionen|
|database_workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|DatabaseResourceId|
|sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|Keine Dimensionen|
|database_sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|DatabaseResourceId|
|eDTU_limit|eDTU-Grenzwert|Anzahl|Durchschnitt|eDTU-Grenzwert|Keine Dimensionen|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|Keine Dimensionen|
|eDTU_used|eDTU-Verbrauch|Anzahl|Durchschnitt|eDTU-Verbrauch|Keine Dimensionen|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|Keine Dimensionen|
|database_storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|DatabaseResourceId|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent|Prozent|Durchschnitt|In-Memory-OLTP-Speicher in Prozent|Keine Dimensionen|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|ElasticPoolResourceId|
|database_dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|DatabaseResourceId, ElasticPoolResourceId|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|ElasticPoolResourceId|
|database_storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|DatabaseResourceId, ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|UsedCapacity|Verwendete Kapazität|Byte|Durchschnitt|Vom Konto verwendete Kapazität|Keine Dimensionen|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Availability|Availability|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BlobCapacity|Blob-Kapazität|Byte|Durchschnitt|Die Größe des vom Blob-Dienst des Speicherkontos genutzten Speichers in Byte.|BlobType|
|BlobCount|Anzahl von Blobs|Anzahl|Durchschnitt|Die Anzahl von Blobs im Blob-Dienst des Speicherkontos.|BlobType|
|ContainerCount|Anzahl von Blob-Containern|Anzahl|Durchschnitt|Die Anzahl von Containern im Blob-Dienst des Speicherkontos.|Keine Dimensionen|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Availability|Availability|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|TableCapacity|Tabellenkapazität|Byte|Durchschnitt|Die Größe des vom Tabellendienst des Speicherkontos genutzten Speichers in Byte.|Keine Dimensionen|
|TableCount|Anzahl von Tabellen|Anzahl|Durchschnitt|Die Anzahl von Tabellen im Tabellendienst des Speicherkontos|Keine Dimensionen|
|TableEntityCount|Anzahl von Tabellenentitäten|Anzahl|Durchschnitt|Die Anzahl von Tabellenentitäten im Tabellendienst des Speicherkontos|Keine Dimensionen|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Availability|Availability|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|QueueCapacity|Warteschlangenkapazität|Byte|Durchschnitt|Die Größe des vom Warteschlangendienst des Speicherkontos genutzten Speichers in Byte.|Keine Dimensionen|
|QueueCount|Anzahl von Warteschlangen|Anzahl|Durchschnitt|Die Anzahl von Warteschlangen im Warteschlangendienst des Speicherkontos|Keine Dimensionen|
|QueueMessageCount|Anzahl von Warteschlangennachrichten|Anzahl|Durchschnitt|Die ungefähre Anzahl von Warteschlangennachrichten im Warteschlangendienst des Speicherkontos|Keine Dimensionen|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Availability|Availability|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|FileCapacity|Dateikapazität|Byte|Durchschnitt|Die Größe des vom Dateidienst des Speicherkontos genutzten Speichers in Byte.|Keine Dimensionen|
|FileCount|Dateianzahl|Anzahl|Durchschnitt|Die Anzahl von Dateien im Dateidienst des Speicherkontos|Keine Dimensionen|
|FileShareCount|Anzahl von Dateifreigaben|Anzahl|Durchschnitt|Die Anzahl von Dateifreigaben im Dateidienst des Speicherkontos|Keine Dimensionen|
|Transaktionen|Transaktionen|Anzahl|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Availability|Availability|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|ResourceUtilization|Speichereinheitnutzung in %|Prozent|Maximum|Speichereinheitnutzung in %|Keine Dimensionen|
|InputEvents|Eingabeereignisse|Anzahl|Gesamt|Eingabeereignisse|Keine Dimensionen|
|InputEventBytes|Eingabeereignisbytes|Byte|Gesamt|Eingabeereignisbytes|Keine Dimensionen|
|LateInputEvents|Ereignisse bei verspäteter Eingabe|Anzahl|Gesamt|Ereignisse bei verspäteter Eingabe|Keine Dimensionen|
|OutputEvents|Ausgabeereignisse|Anzahl|Gesamt|Ausgabeereignisse|Keine Dimensionen|
|ConversionErrors|Konvertierungsfehler|Anzahl|Gesamt|Konvertierungsfehler|Keine Dimensionen|
|Errors|Laufzeitfehler|Anzahl|Gesamt|Laufzeitfehler|Keine Dimensionen|
|DroppedOrAdjustedEvents|Ereignisse für falsche Reihenfolge|Anzahl|Gesamt|Ereignisse für falsche Reihenfolge|Keine Dimensionen|
|AMLCalloutRequests|Funktionsanforderungen|Anzahl|Gesamt|Funktionsanforderungen|Keine Dimensionen|
|AMLCalloutFailedRequests|Fehlerhafte Funktionsanforderungen|Anzahl|Gesamt|Fehlerhafte Funktionsanforderungen|Keine Dimensionen|
|AMLCalloutInputEvents|Funktionsereignisse|Anzahl|Gesamt|Funktionsereignisse|Keine Dimensionen|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Instanz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Prozent|Durchschnitt|Arbeitsspeicherprozentsatz|Instanz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Anzahl|Gesamt|Warteschlangenlänge des Datenträgers|Instanz|
|HttpQueueLength|Länge der HTTP-Warteschlange|Anzahl|Gesamt|Länge der HTTP-Warteschlange|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (Funktionen ausgenommen)

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|Instanz|
|Anforderungen|Anforderungen|Anzahl|Gesamt|Anforderungen|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|Instanz|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Anzahl|Durchschnitt|Ausführungseinheiten für Funktion|Instanz|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Anzahl|Durchschnitt|Ausführungsanzahl für Funktion|Instanz|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (Funktionen)

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Anzahl|Durchschnitt|Ausführungseinheiten für Funktion|Instanz|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Anzahl|Durchschnitt|Ausführungsanzahl für Funktion|Instanz|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|Instanz|
|Anforderungen|Anforderungen|Anzahl|Gesamt|Anforderungen|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|Instanz|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Anzahl|Durchschnitt|Ausführungseinheiten für Funktion|Instanz|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Anzahl|Durchschnitt|Ausführungsanzahl für Funktion|Instanz|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|Anforderungen|Anforderungen|Anzahl|Gesamt|Anforderungen|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|Instanz|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Instanz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Prozent|Durchschnitt|Arbeitsspeicherprozentsatz|Instanz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Anzahl|Gesamt|Warteschlangenlänge des Datenträgers|Instanz|
|HttpQueueLength|Länge der HTTP-Warteschlange|Anzahl|Gesamt|Länge der HTTP-Warteschlange|Instanz|
|ActiveRequests|Aktive Anforderungen|Anzahl|Gesamt|Aktive Anforderungen|Instanz|
|TotalFrontEnds|Gesamtanzahl an Front-Ends|Anzahl|Durchschnitt|Gesamtanzahl an Front-Ends|Instanz|
|SmallAppServicePlanInstances|Kleine Worker im App Service-Plan|Anzahl|Durchschnitt|Kleine Worker im App Service-Plan|Instanz|
|MediumAppServicePlanInstances|Mittlere Worker im App Service-Plan|Anzahl|Durchschnitt|Mittlere Worker im App Service-Plan|Instanz|
|LargeAppServicePlanInstances|Große Worker im App Service-Plan|Anzahl|Durchschnitt|Große Worker im App Service-Plan|Instanz|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|WorkersTotal|Gesamtanzahl an Workern|Anzahl|Durchschnitt|Gesamtanzahl an Workern|Instanz|
|WorkersAvailable|Verfügbare Worker|Anzahl|Durchschnitt|Verfügbare Worker|Instanz|
|WorkersUsed|Verwendete Worker|Anzahl|Durchschnitt|Verwendete Worker|Instanz|

## <a name="next-steps"></a>Nächste Schritte
* [Informationen zu Metriken in Azure Monitor](monitoring-overview-metrics.md)
* [Erstellen von Warnungen zu Metriken](insights-receive-alert-notifications.md)
* [Exportieren von Metriken in Storage, Event Hub oder Log Analytics](monitoring-overview-of-diagnostic-logs.md)
