---
title: 'Azure Monitor-Metriken: Unterstützte Metriken pro Ressourcentyp | Microsoft Docs'
description: Liste der Metriken, die mit Azure Monitor für jeden Ressourcentyp zur Verfügung stehen.
author: anirudhcavale
manager: ashwink
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: ancav
ms.openlocfilehash: c9dab276b39b1ceb55851e865f9166e3e9e7fee8
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="supported-metrics-with-azure-monitor"></a>Unterstützte Metriken von Azure Monitor
Azure Monitor bietet verschiedene Methoden für die Interaktion mit Metriken, z.B. die Diagrammdarstellung im Portal, den Zugriff über die REST-API oder die Abfrage über PowerShell oder CLI. Unten ist eine vollständige Liste aller Metriken aufgeführt, die derzeit mit der Metrikpipeline von Azure Monitor verfügbar sind. Weitere Metriken stehen möglicherweise im Portal oder über Legacy-APIs zur Verfügung. Die unten angegebene Liste enthält nur Metriken, die über die konsolidierte Azure Monitor-Metrikpipeline verfügbar sind. Verwenden Sie die [API-Version 2018-01-01](https://docs.microsoft.com/rest/api/monitor/metricdefinitions), um diese Metriken abzufragen und darauf zuzugreifen.

> [!NOTE]
> Das Senden mehrdimensionaler Metriken über die Diagnoseeinstellungen wird derzeit nicht unterstützt. Metriken mit Dimensionen werden als vereinfachte eindimensionale Metriken exportiert und dimensionswertübergreifend aggregiert.
>
> *Beispiel:* Die Metrik „Eingehende Nachrichten“ eines Event Hubs kann auf einer warteschlangenspezifischen Ebene untersucht und in einem Diagramm dargestellt werden. Wenn Sie die Metrik allerdings über die Diagnoseeinstellungen exportieren, umfasst die Darstellung alle eingehenden Nachrichten für alle Warteschlangen im Event Hub.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|qpu_metric|QPU|Count|Durchschnitt|QPU. Bereich: 0–100 für S1, 0–200 für S2 und 0–400 für S4|ServerResourceType|
|memory_metric|Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeicher. Bereich: 0–25 GB für S1, 0–50 GB für S2 und 0–100 GB für S4|ServerResourceType|
|TotalConnectionRequests|Total Connection Requests (Verbindungsanforderungen gesamt)|Count|Durchschnitt|Gesamtanzahl von Verbindungsanforderungen. Dies sind erhaltene Anforderungen.|ServerResourceType|
|SuccessfullConnectionsPerSec|Erfolgreiche Verbindungen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der erfolgreichen Verbindungsabschlüsse|ServerResourceType|
|TotalConnectionFailures|Verbindungsfehler gesamt|Count|Durchschnitt|Gesamtanzahl von fehlerhaften Verbindungsversuchen|ServerResourceType|
|CurrentUserSessions|Aktuelle Benutzersitzungen|Count|Durchschnitt|Aktuelle Anzahl von eingerichteten Benutzersitzungen|ServerResourceType|
|QueryPoolBusyThreads|Ausgelastete Abfragepoolthreads|Count|Durchschnitt|Anzahl von ausgelasteten Threads im Abfragethreadpool|ServerResourceType|
|CommandPoolJobQueueLength|Warteschlangenlänge für Aufträge im Befehlspool|Count|Durchschnitt|Gibt die Anzahl von Aufträgen in der Warteschlange des Befehlsthreadpools an.|ServerResourceType|
|ProcessingPoolJobQueueLength|Warteschlangenlänge für Verarbeitungspoolaufträge|Count|Durchschnitt|Anzahl von Nicht-E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools|ServerResourceType|
|CurrentConnections|Verbindung: Aktuelle Verbindungen|Count|Durchschnitt|Aktuelle Anzahl hergestellter Clientverbindungen.|ServerResourceType|
|CleanerCurrentPrice|Arbeitsspeicher: Bereinigung – aktueller Preis|Count|Durchschnitt|Aktueller Preis des Arbeitsspeichers, $/Byte/Zeit, normalisiert auf 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Arbeitsspeicher: Bereinigung – verkleinerbarer Arbeitsspeicher|Byte|Durchschnitt|Die Menge des Arbeitsspeichers in Byte, die durch den Hintergrundbereinigungsprozess bereinigt wird.|ServerResourceType|
|CleanerMemoryNonshrinkable|Arbeitsspeicher: Bereinigung – nicht verkleinerbarer Arbeitsspeicher|Byte|Durchschnitt|Die Menge des Arbeitsspeichers in Byte, die nicht durch den Hintergrundbereinigungsprozess bereinigt wird.|ServerResourceType|
|MemoryUsage|Arbeitsspeicher: Speicherauslastung|Byte|Durchschnitt|Speicherauslastung des Serverprozesses, wie bei der Berechnung des Arbeitsspeicherpreises für die Bereinigung verwendet. Entspricht dem Indikator „Process\PrivateBytes“ zuzüglich der Größe der im Speicher abgebildeten Daten. Von der xVelocity-In-Memory-Analyse-Engine (VertiPaq) abgebildeter oder belegter Arbeitsspeicher, der über die Arbeitsspeichergrenze der xVelocity-Engine hinausgeht, wird dabei ignoriert.|ServerResourceType|
|MemoryLimitHard|Arbeitsspeicher: Grenzwert für den festen Speicher|Byte|Durchschnitt|Grenzwert für den festen Speicher gemäß Konfigurationsdatei.|ServerResourceType|
|MemoryLimitHigh|Arbeitsspeicher: Obere Arbeitsspeichergrenze|Byte|Durchschnitt|Oberer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|ServerResourceType|
|MemoryLimitLow|Arbeitsspeicher: Untere Arbeitsspeichergrenze|Byte|Durchschnitt|Unterer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|ServerResourceType|
|MemoryLimitVertiPaq|Arbeitsspeicher: VertiPaq-Arbeitsspeichergrenze|Byte|Durchschnitt|In-Memory-Grenzwert gemäß Konfigurationsdatei.|ServerResourceType|
|Kontingent|Arbeitsspeicher: Kontingent|Byte|Durchschnitt|Aktuelles Arbeitsspeicherkontingent in Byte. Das Arbeitsspeicherkontingent wird auch als Speicherzuweisung oder Speicherreservierung bezeichnet.|ServerResourceType|
|QuotaBlocked|Arbeitsspeicher: Kontingent blockiert|Count|Durchschnitt|Aktuelle Anzahl von Kontingentanforderungen, die blockiert werden, bis andere Arbeitsspeicherkontingente freigegeben werden.|ServerResourceType|
|VertiPaqNonpaged|Arbeitsspeicher: Nicht ausgelagerte VertiPaq-Daten|Byte|Durchschnitt|Bytes von Arbeitsspeicher, die im Arbeitssatz zur Verwendung durch die In-Memory-Engine gesperrt sind.|ServerResourceType|
|VertiPaqPaged|Arbeitsspeicher: Ausgelagerte VertiPaq-Daten|Byte|Durchschnitt|Bytes von ausgelagertem Arbeitsspeicher, die für In-Memory-Daten verwendet werden.|ServerResourceType|
|RowsReadPerSec|Verarbeitung: Gelesene Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der aus allen relationalen Datenbanken gelesenen Zeilen.|ServerResourceType|
|RowsConvertedPerSec|Verarbeitung: Konvertierte Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der Zeilen, die bei der Verarbeitung konvertiert werden.|ServerResourceType|
|RowsWrittenPerSec|Verarbeitung: Geschriebene Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der Zeilen, die bei der Verarbeitung geschrieben werden.|ServerResourceType|
|CommandPoolBusyThreads|Threads: Ausgelastete Threads im Befehlspool|Count|Durchschnitt|Anzahl ausgelasteter Threads im Befehlsthreadpool.|ServerResourceType|
|CommandPoolIdleThreads|Threads: Leerlaufthreads im Befehlspool|Count|Durchschnitt|Anzahl von Leerlaufthreads im Befehlsthreadpool.|ServerResourceType|
|LongParsingBusyThreads|Threads: Ausgelastete lange Analysethreads|Count|Durchschnitt|Anzahl ausgelasteter Threads im Pool für lange Analysethreads.|ServerResourceType|
|LongParsingIdleThreads|Threads: Lange Analysethreads im Leerlauf|Count|Durchschnitt|Anzahl von Leerlaufthreads im Pool für lange Analysethreads.|ServerResourceType|
|LongParsingJobQueueLength|Threads: Warteschlangenlänge für lange Analyseaufträge|Count|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Pools für lange Analysethreads.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Threads: Ausgelastete Threads für E/A-Aufträge im Verarbeitungspool|Count|Durchschnitt|Anzahl von Threads, die E/A-Aufträge im Verarbeitungsthreadpool ausführen.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Threads: Ausgelastete Nicht-E/A-Threads im Verarbeitungspool|Count|Durchschnitt|Anzahl von Threads, die Nicht-E/A-Aufträge im Verarbeitungsthreadpool ausführen.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Threads: Warteschlangenlänge für E/A-Aufträge im Verarbeitungspool|Count|Durchschnitt|Anzahl von E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Threads: Leerlaufthreads für E/A-Aufträge im Verarbeitungspool|Count|Durchschnitt|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Threads: Nicht-E/A-Leerlaufthreads im Verarbeitungspool|Count|Durchschnitt|Anzahl von Leerlaufthreads im Verarbeitungsthreadpool, die für Nicht-E/A-Aufträge vorgesehen sind.|ServerResourceType|
|QueryPoolIdleThreads|Threads: Abfragepoolthreads im Leerlauf|Count|Durchschnitt|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|ServerResourceType|
|QueryPoolJobQueueLength|Threads: Auftragswarteschlangenlänge für Abfragepool|Count|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Abfragethreadpools.|ServerResourceType|
|ShortParsingBusyThreads|Threads: Ausgelastete kurze Analysethreads|Count|Durchschnitt|Anzahl ausgelasteter Threads im Pool für kurze Analysethreads.|ServerResourceType|
|ShortParsingIdleThreads|Threads: Kurze Analysethreads im Leerlauf|Count|Durchschnitt|Anzahl von Leerlaufthreads im Pool für kurze Analysethreads.|ServerResourceType|
|ShortParsingJobQueueLength|Threads: Warteschlangenlänge für kurze Analyseaufträge|Count|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Pools für kurze Analysethreads.|ServerResourceType|
|memory_thrashing_metric|Arbeitsspeicherüberlastung|Prozent|Durchschnitt|Durchschnittliche Arbeitsspeicherüberlastung.|ServerResourceType|
|mashup_engine_qpu_metric|M-Engine – QPU|Count|Durchschnitt|QPU-Nutzung durch Mashup-Engine-Prozesse|ServerResourceType|
|mashup_engine_memory_metric|M-Engine – Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeichernutzung durch Mashup-Engine-Prozesse|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|TotalRequests|Total Gateway Requests (Gatewayanforderungen gesamt)|Count|Gesamt|Anzahl von Gatewayanforderungen|Speicherort, Hostname|
|SuccessfulRequests|Successful Gateway Requests (Erfolgreiche Gatewayanforderungen)|Count|Gesamt|Anzahl von erfolgreichen Gatewayanforderungen|Speicherort, Hostname|
|UnauthorizedRequests|Unauthorized Gateway Requests (Nicht autorisierte Gatewayanforderungen)|Count|Gesamt|Anzahl von nicht autorisierten Gatewayanforderungen|Speicherort, Hostname|
|FailedRequests|Failed Gateway Requests (Fehlgeschlagene Gatewayanforderungen)|Count|Gesamt|Anzahl von Fehlern bei Gatewayanforderungen|Speicherort, Hostname|
|OtherRequests|Other Gateway Requests (Sonstige Gatewayanforderungen)|Count|Gesamt|Anzahl von anderen Gatewayanforderungen|Speicherort, Hostname|
|Duration|Gesamtdauer von Gatewayanforderungen|Millisekunden|Durchschnitt|Gesamtdauer von Gatewayanforderungen in Millisekunden|Speicherort, Hostname|
|Capacity|Kapazität (Vorschau)|Prozent|Maximum|Auslastungsmetrik für ApiManagement-Dienst|Speicherort|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|TotalJob|Gesamtzahl an Einzelvorgängen (Jobs)|Count|Gesamt|Die Gesamtzahl an Einzelvorgängen (Jobs)|RunbookName, Status|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CoreCount|Dedizierte Anzahl von Kernen|Count|Gesamt|Gesamtzahl der dedizierten Kerne im Batch-Konto|Keine Dimensionen|
|TotalNodeCount|Dedizierte Knotenanzahl|Count|Gesamt|Gesamtzahl der dedizierten Knoten im Batch-Konto|Keine Dimensionen|
|LowPriorityCoreCount|Anzahl von LowPriority-Kernen|Count|Gesamt|Gesamtzahl der Kerne mit niedriger Priorität im Batch-Konto|Keine Dimensionen|
|TotalLowPriorityNodeCount|Anzahl der Knoten mit niedriger Priorität|Count|Gesamt|Gesamtzahl der Knoten mit niedriger Priorität im Batchkonto|Keine Dimensionen|
|CreatingNodeCount|Anzahl erstellter Knoten|Count|Gesamt|Anzahl von Knoten, die gerade erstellt werden|Keine Dimensionen|
|StartingNodeCount|Anzahl gestarteter Knoten|Count|Gesamt|Anzahl von Knoten, die gerade gestartet werden|Keine Dimensionen|
|WaitingForStartTaskNodeCount|Anzahl von Knoten, die auf den Starttask warten|Count|Gesamt|Anzahl von Knoten, die auf den Abschluss des Starttasks warten|Keine Dimensionen|
|StartTaskFailedNodeCount|Anzahl von Knoten mit Starttaskfehlern|Count|Gesamt|Anzahl von Knoten, bei denen der Starttask nicht durchgeführt werden konnte|Keine Dimensionen|
|IdleNodeCount|Anzahl von Knoten im Leerlauf|Count|Gesamt|Anzahl von Knoten im Leerlauf|Keine Dimensionen|
|OfflineNodeCount|Anzahl von Offlineknoten|Count|Gesamt|Anzahl offline geschalteter Knoten|Keine Dimensionen|
|RebootingNodeCount|Anzahl neu gestarteter Knoten|Count|Gesamt|Anzahl von Knoten, die neu gestartet werden|Keine Dimensionen|
|ReimagingNodeCount|Anzahl von Knoten mit Reimaging|Count|Gesamt|Anzahl von Knoten, für die ein Reimaging durchgeführt wird|Keine Dimensionen|
|RunningNodeCount|Anzahl ausgeführter Knoten|Count|Gesamt|Anzahl ausgeführter Knoten|Keine Dimensionen|
|LeavingPoolNodeCount|Anzahl von Knoten, die den Pool verlassen|Count|Gesamt|Anzahl von Knoten, die den Pool verlassen|Keine Dimensionen|
|UnusableNodeCount|Anzahl nicht verwendbarer Knoten|Count|Gesamt|Anzahl nicht verwendbarer Knoten|Keine Dimensionen|
|PreemptedNodeCount|Anzahl der vorzeitig entfernten Knoten|Count|Gesamt|Die Anzahl der vorzeitig entfernten Knoten|Keine Dimensionen|
|TaskStartEvent|Taskstartereignisse|Count|Gesamt|Gesamtanzahl gestarteter Tasks|Keine Dimensionen|
|TaskCompleteEvent|Taskabschlussereignisse|Count|Gesamt|Gesamtanzahl abgeschlossener Tasks|Keine Dimensionen|
|TaskFailEvent|Taskfehlerereignisse|Count|Gesamt|Gesamtanzahl von Tasks, die mit Fehlerstatus abgeschlossen wurden|Keine Dimensionen|
|PoolCreateEvent|Poolerstellungsereignisse|Count|Gesamt|Gesamtanzahl erstellter Pools|Keine Dimensionen|
|PoolResizeStartEvent|Ereignisse zum Start der Größenänderung von Pools|Count|Gesamt|Gesamtanzahl gestarteter Größenänderungen von Pools|Keine Dimensionen|
|PoolResizeCompleteEvent|Ereignisse zum Abschluss der Größenänderung von Pools|Count|Gesamt|Gesamtanzahl abgeschlossener Größenänderungen von Pools|Keine Dimensionen|
|PoolDeleteStartEvent|Ereignisse zum Starten des Löschvorgangs von Pools|Count|Gesamt|Gesamtanzahl gestarteter Poollöschvorgänge|Keine Dimensionen|
|PoolDeleteCompleteEvent|Ereignisse zum Abschluss des Löschvorgangs von Pools|Count|Gesamt|Gesamtanzahl abgeschlossener Poollöschvorgänge|Keine Dimensionen|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|connectedclients|Verbundene Clients|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed|Vorgänge gesamt|Count|Gesamt||Keine Dimensionen|
|cachehits|Cachetreffer|Count|Gesamt||Keine Dimensionen|
|cachemisses|Cachefehler|Count|Gesamt||Keine Dimensionen|
|getcommands|get-Vorgänge|Count|Gesamt||Keine Dimensionen|
|setcommands|set-Vorgänge|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond|Vorgänge pro Sekunde|Count|Gesamt||Keine Dimensionen|
|evictedkeys|Entfernte Schlüssel|Count|Gesamt||Keine Dimensionen|
|totalkeys|Schlüssel insgesamt|Count|Maximum||Keine Dimensionen|
|expiredkeys|Abgelaufene Schlüssel|Count|Gesamt||Keine Dimensionen|
|usedmemory|Verwendeter Arbeitsspeicher|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss|Verwendeter Arbeitsspeicher (RSS)|Byte|Maximum||Keine Dimensionen|
|serverLoad|Serverlast|Prozent|Maximum||Keine Dimensionen|
|cacheWrite|Cache-Schreibvorgänge|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead|Cache-Lesevorgänge|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime|CPU|Prozent|Maximum||Keine Dimensionen|
|connectedclients0|Verbundene Clients (Shard 0)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed0|Vorgänge gesamt (Shard 0)|Count|Gesamt||Keine Dimensionen|
|cachehits0|Cachetreffer (Shard 0)|Count|Gesamt||Keine Dimensionen|
|cachemisses0|Cachefehler (Shard 0)|Count|Gesamt||Keine Dimensionen|
|getcommands0|Get-Vorgänge (Shard 0)|Count|Gesamt||Keine Dimensionen|
|setcommands0|Set-Vorgänge (Shard 0)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond0|Vorgänge pro Sekunde (Shard 0)|Count|Gesamt||Keine Dimensionen|
|evictedkeys0|Entfernte Schlüssel (Shard 0)|Count|Gesamt||Keine Dimensionen|
|totalkeys0|Schlüssel gesamt (Shard 0)|Count|Maximum||Keine Dimensionen|
|expiredkeys0|Abgelaufene Schlüssel (Shard 0)|Count|Gesamt||Keine Dimensionen|
|usedmemory0|Verwendeter Arbeitsspeicher (Shard 0)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss0|Verwendeter Arbeitsspeicher (RSS, Shard 0)|Byte|Maximum||Keine Dimensionen|
|serverLoad0|Serverauslastung (Shard 0)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite0|Cacheschreibvorgänge (Shard 0)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead0|Cachelesevorgänge (Shard 0)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime0|CPU (Shard 0)|Prozent|Maximum||Keine Dimensionen|
|connectedclients1|Verbundene Clients (Shard 1)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed1|Vorgänge gesamt (Shard 1)|Count|Gesamt||Keine Dimensionen|
|cachehits1|Cachetreffer (Shard 1)|Count|Gesamt||Keine Dimensionen|
|cachemisses1|Cachefehler (Shard 1)|Count|Gesamt||Keine Dimensionen|
|getcommands1|Get-Vorgänge (Shard 1)|Count|Gesamt||Keine Dimensionen|
|setcommands1|Set-Vorgänge (Shard 1)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond1|Vorgänge pro Sekunde (Shard 1)|Count|Gesamt||Keine Dimensionen|
|evictedkeys1|Entfernte Schlüssel (Shard 1)|Count|Gesamt||Keine Dimensionen|
|totalkeys1|Schlüssel gesamt (Shard 1)|Count|Maximum||Keine Dimensionen|
|expiredkeys1|Abgelaufene Schlüssel (Shard 1)|Count|Gesamt||Keine Dimensionen|
|usedmemory1|Verwendeter Arbeitsspeicher (Shard 1)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss1|Verwendeter Arbeitsspeicher (RSS, Shard 1)|Byte|Maximum||Keine Dimensionen|
|serverLoad1|Serverauslastung (Shard 1)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite1|Cacheschreibvorgänge (Shard 1)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead1|Cachelesevorgänge (Shard 1)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime1|CPU (Shard 1)|Prozent|Maximum||Keine Dimensionen|
|connectedclients2|Verbundene Clients (Shard 2)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed2|Vorgänge gesamt (Shard 2)|Count|Gesamt||Keine Dimensionen|
|cachehits2|Cachetreffer (Shard 2)|Count|Gesamt||Keine Dimensionen|
|cachemisses2|Cachefehler (Shard 2)|Count|Gesamt||Keine Dimensionen|
|getcommands2|Get-Vorgänge (Shard 2)|Count|Gesamt||Keine Dimensionen|
|setcommands2|Set-Vorgänge (Shard 2)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond2|Vorgänge pro Sekunde (Shard 2)|Count|Gesamt||Keine Dimensionen|
|evictedkeys2|Entfernte Schlüssel (Shard 2)|Count|Gesamt||Keine Dimensionen|
|totalkeys2|Schlüssel gesamt (Shard 2)|Count|Maximum||Keine Dimensionen|
|expiredkeys2|Abgelaufene Schlüssel (Shard 2)|Count|Gesamt||Keine Dimensionen|
|usedmemory2|Verwendeter Arbeitsspeicher (Shard 2)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss2|Verwendeter Arbeitsspeicher (RSS, Shard 2)|Byte|Maximum||Keine Dimensionen|
|serverLoad2|Serverauslastung (Shard 2)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite2|Cacheschreibvorgänge (Shard 2)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead2|Cachelesevorgänge (Shard 2)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime2|CPU (Shard 2)|Prozent|Maximum||Keine Dimensionen|
|connectedclients3|Verbundene Clients (Shard 3)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed3|Vorgänge gesamt (Shard 3)|Count|Gesamt||Keine Dimensionen|
|cachehits3|Cachetreffer (Shard 3)|Count|Gesamt||Keine Dimensionen|
|cachemisses3|Cachefehler (Shard 3)|Count|Gesamt||Keine Dimensionen|
|getcommands3|Get-Vorgänge (Shard 3)|Count|Gesamt||Keine Dimensionen|
|setcommands3|Set-Vorgänge (Shard 3)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond3|Vorgänge pro Sekunde (Shard 3)|Count|Gesamt||Keine Dimensionen|
|evictedkeys3|Entfernte Schlüssel (Shard 3)|Count|Gesamt||Keine Dimensionen|
|totalkeys3|Schlüssel gesamt (Shard 3)|Count|Maximum||Keine Dimensionen|
|expiredkeys3|Abgelaufene Schlüssel (Shard 3)|Count|Gesamt||Keine Dimensionen|
|usedmemory3|Verwendeter Arbeitsspeicher (Shard 3)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss3|Verwendeter Arbeitsspeicher (RSS, Shard 3)|Byte|Maximum||Keine Dimensionen|
|serverLoad3|Serverauslastung (Shard 3)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite3|Cacheschreibvorgänge (Shard 3)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead3|Cachelesevorgänge (Shard 3)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime3|CPU (Shard 3)|Prozent|Maximum||Keine Dimensionen|
|connectedclients4|Verbundene Clients (Shard 4)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed4|Vorgänge gesamt (Shard 4)|Count|Gesamt||Keine Dimensionen|
|cachehits4|Cachetreffer (Shard 4)|Count|Gesamt||Keine Dimensionen|
|cachemisses4|Cachefehler (Shard 4)|Count|Gesamt||Keine Dimensionen|
|getcommands4|Get-Vorgänge (Shard 4)|Count|Gesamt||Keine Dimensionen|
|setcommands4|Set-Vorgänge (Shard 4)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond4|Vorgänge pro Sekunde (Shard 4)|Count|Gesamt||Keine Dimensionen|
|evictedkeys4|Entfernte Schlüssel (Shard 4)|Count|Gesamt||Keine Dimensionen|
|totalkeys4|Schlüssel gesamt (Shard 4)|Count|Maximum||Keine Dimensionen|
|expiredkeys4|Abgelaufene Schlüssel (Shard 4)|Count|Gesamt||Keine Dimensionen|
|usedmemory4|Verwendeter Arbeitsspeicher (Shard 4)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss4|Verwendeter Arbeitsspeicher (RSS, Shard 4)|Byte|Maximum||Keine Dimensionen|
|serverLoad4|Serverauslastung (Shard 4)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite4|Cacheschreibvorgänge (Shard 4)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead4|Cachelesevorgänge (Shard 4)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime4|CPU (Shard 4)|Prozent|Maximum||Keine Dimensionen|
|connectedclients5|Verbundene Clients (Shard 5)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed5|Vorgänge gesamt (Shard 5)|Count|Gesamt||Keine Dimensionen|
|cachehits5|Cachetreffer (Shard 5)|Count|Gesamt||Keine Dimensionen|
|cachemisses5|Cachefehler (Shard 5)|Count|Gesamt||Keine Dimensionen|
|getcommands5|Get-Vorgänge (Shard 5)|Count|Gesamt||Keine Dimensionen|
|setcommands5|Set-Vorgänge (Shard 5)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond5|Vorgänge pro Sekunde (Shard 5)|Count|Gesamt||Keine Dimensionen|
|evictedkeys5|Entfernte Schlüssel (Shard 5)|Count|Gesamt||Keine Dimensionen|
|totalkeys5|Schlüssel gesamt (Shard 5)|Count|Maximum||Keine Dimensionen|
|expiredkeys5|Abgelaufene Schlüssel (Shard 5)|Count|Gesamt||Keine Dimensionen|
|usedmemory5|Verwendeter Arbeitsspeicher (Shard 5)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss5|Verwendeter Arbeitsspeicher (RSS, Shard 5)|Byte|Maximum||Keine Dimensionen|
|serverLoad5|Serverauslastung (Shard 5)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite5|Cacheschreibvorgänge (Shard 5)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead5|Cachelesevorgänge (Shard 5)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime5|CPU (Shard 5)|Prozent|Maximum||Keine Dimensionen|
|connectedclients6|Verbundene Clients (Shard 6)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed6|Vorgänge gesamt (Shard 6)|Count|Gesamt||Keine Dimensionen|
|cachehits6|Cachetreffer (Shard 6)|Count|Gesamt||Keine Dimensionen|
|cachemisses6|Cachefehler (Shard 6)|Count|Gesamt||Keine Dimensionen|
|getcommands6|Get-Vorgänge (Shard 6)|Count|Gesamt||Keine Dimensionen|
|setcommands6|Set-Vorgänge (Shard 6)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond6|Vorgänge pro Sekunde (Shard 6)|Count|Gesamt||Keine Dimensionen|
|evictedkeys6|Entfernte Schlüssel (Shard 6)|Count|Gesamt||Keine Dimensionen|
|totalkeys6|Schlüssel gesamt (Shard 6)|Count|Maximum||Keine Dimensionen|
|expiredkeys6|Abgelaufene Schlüssel (Shard 6)|Count|Gesamt||Keine Dimensionen|
|usedmemory6|Verwendeter Arbeitsspeicher (Shard 6)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss6|Verwendeter Arbeitsspeicher (RSS, Shard 6)|Byte|Maximum||Keine Dimensionen|
|serverLoad6|Serverauslastung (Shard 6)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite6|Cacheschreibvorgänge (Shard 6)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead6|Cachelesevorgänge (Shard 6)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime6|CPU (Shard 6)|Prozent|Maximum||Keine Dimensionen|
|connectedclients7|Verbundene Clients (Shard 7)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed7|Vorgänge gesamt (Shard 7)|Count|Gesamt||Keine Dimensionen|
|cachehits7|Cachetreffer (Shard 7)|Count|Gesamt||Keine Dimensionen|
|cachemisses7|Cachefehler (Shard 7)|Count|Gesamt||Keine Dimensionen|
|getcommands7|Get-Vorgänge (Shard 7)|Count|Gesamt||Keine Dimensionen|
|setcommands7|Set-Vorgänge (Shard 7)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond7|Vorgänge pro Sekunde (Shard 7)|Count|Gesamt||Keine Dimensionen|
|evictedkeys7|Entfernte Schlüssel (Shard 7)|Count|Gesamt||Keine Dimensionen|
|totalkeys7|Schlüssel gesamt (Shard 7)|Count|Maximum||Keine Dimensionen|
|expiredkeys7|Abgelaufene Schlüssel (Shard 7)|Count|Gesamt||Keine Dimensionen|
|usedmemory7|Verwendeter Arbeitsspeicher (Shard 7)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss7|Verwendeter Arbeitsspeicher (RSS, Shard 7)|Byte|Maximum||Keine Dimensionen|
|serverLoad7|Serverauslastung (Shard 7)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite7|Cacheschreibvorgänge (Shard 7)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead7|Cachelesevorgänge (Shard 7)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime7|CPU (Shard 7)|Prozent|Maximum||Keine Dimensionen|
|connectedclients8|Verbundene Clients (Shard 8)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed8|Vorgänge gesamt (Shard 8)|Count|Gesamt||Keine Dimensionen|
|cachehits8|Cachetreffer (Shard 8)|Count|Gesamt||Keine Dimensionen|
|cachemisses8|Cachefehler (Shard 8)|Count|Gesamt||Keine Dimensionen|
|getcommands8|Get-Vorgänge (Shard 8)|Count|Gesamt||Keine Dimensionen|
|setcommands8|Set-Vorgänge (Shard 8)|Count|Gesamt||Keine Dimensionen|
|operationsPerSecond8|Vorgänge pro Sekunde (Shard 8)|Count|Gesamt||Keine Dimensionen|
|evictedkeys8|Entfernte Schlüssel (Shard 8)|Count|Gesamt||Keine Dimensionen|
|totalkeys8|Schlüssel gesamt (Shard 8)|Count|Maximum||Keine Dimensionen|
|expiredkeys8|Abgelaufene Schlüssel (Shard 8)|Count|Gesamt||Keine Dimensionen|
|usedmemory8|Verwendeter Arbeitsspeicher (Shard 8)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss8|Verwendeter Arbeitsspeicher (RSS, Shard 8)|Byte|Maximum||Keine Dimensionen|
|serverLoad8|Serverauslastung (Shard 8)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite8|Cacheschreibvorgänge (Shard 8)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead8|Cachelesevorgänge (Shard 8)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime8|CPU (Shard 8)|Prozent|Maximum||Keine Dimensionen|
|connectedclients9|Verbundene Clients (Shard 9)|Count|Maximum||Keine Dimensionen|
|totalcommandsprocessed9|Vorgänge gesamt (Shard 9)|Count|Gesamt||Keine Dimensionen|
|cachehits9|Cachetreffer (Shard 9)|Count|Gesamt||Keine Dimensionen|
|cachemisses9|Cachefehler (Shard 9)|Count|Gesamt||Keine Dimensionen|
|getcommands9|Get-Vorgänge (Shard 9)|Count|Gesamt||Keine Dimensionen|
|setcommands9|Set-Vorgänge (Shard 9)|Anzahl|Gesamt||Keine Dimensionen|
|operationsPerSecond9|Vorgänge pro Sekunde (Shard 9)|Count|Gesamt||Keine Dimensionen|
|evictedkeys9|Entfernte Schlüssel (Shard 9)|Count|Gesamt||Keine Dimensionen|
|totalkeys9|Schlüssel gesamt (Shard 9)|Count|Maximum||Keine Dimensionen|
|expiredkeys9|Abgelaufene Schlüssel (Shard 9)|Count|Gesamt||Keine Dimensionen|
|usedmemory9|Verwendeter Arbeitsspeicher (Shard 9)|Byte|Maximum||Keine Dimensionen|
|usedmemoryRss9|Verwendeter Arbeitsspeicher (RSS, Shard 9)|Byte|Maximum||Keine Dimensionen|
|serverLoad9|Serverauslastung (Shard 9)|Prozent|Maximum||Keine Dimensionen|
|cacheWrite9|Cacheschreibvorgänge (Shard 9)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|cacheRead9|Cachelesevorgänge (Shard 9)|Bytes pro Sekunde|Maximum||Keine Dimensionen|
|percentProcessorTime9|CPU (Shard 9)|Prozent|Maximum||Keine Dimensionen|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Disk Read Bytes/Sec|Datenträgerlesevorgänge|Bytes pro Sekunde|Durchschnitt|Durchschnitt an Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Disk Write Bytes/Sec|Datenträgerschreibvorgänge|Bytes pro Sekunde|Durchschnitt|Durchschnitt an Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Disk Read Bytes/Sec|Datenträgerlesevorgänge|Bytes pro Sekunde|Durchschnitt|Durchschnitt an Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Disk Write Bytes/Sec|Datenträgerschreibvorgänge|Bytes pro Sekunde|Durchschnitt|Durchschnitt an Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|TotalCalls|Aufrufe gesamt|Count|Gesamt|Gesamtanzahl von Aufrufen|Keine Dimensionen|
|SuccessfulCalls|Erfolgreiche Aufrufe|Count|Gesamt|Anzahl erfolgreicher Aufrufe|Keine Dimensionen|
|TotalErrors|Fehler insgesamt|Count|Gesamt|Gesamtzahl von Aufrufen mit Fehlerantwort (HTTP-Antwortcode 4xx oder 5xx)|Keine Dimensionen|
|BlockedCalls|Blockierte Aufrufe|Count|Gesamt|Anzahl von Aufrufen, die das Raten- oder Kontingentlimit überschritten haben|Keine Dimensionen|
|ServerErrors|Serverfehler|Count|Gesamt|Anzahl von Aufrufen mit internem Dienstfehler (HTTP-Antwortcode 5xx)|Keine Dimensionen|
|ClientErrors|Clientfehler|Count|Gesamt|Anzahl von Aufrufen mit Fehler auf Clientseite (HTTP-Antwortcode 4xx)|Keine Dimensionen|
|DataIn|Eingehende Daten|Byte|Gesamt|Menge eingehender Daten in Byte|Keine Dimensionen|
|DataOut|Datenausgabe|Byte|Gesamt|Menge ausgehender Daten in Byte|Keine Dimensionen|
|Latency|Latency|Millisekunden|Durchschnitt|Latenz in Millisekunden|Keine Dimensionen|
|CharactersTranslated|Übersetzte Zeichen|Count|Gesamt|Gesamtanzahl von Zeichen in einer eingehenden Textanforderung|Keine Dimensionen|
|SpeechSessionDuration|Dauer der Sprachsitzung|Sekunden|Gesamt|Gesamtdauer der Sprachsitzung in Sekunden|Keine Dimensionen|
|TotalTransactions|Transaktionen gesamt|Count|Gesamt|Gesamtzahl der Transaktionen|Keine Dimensionen|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Count|Durchschnitt|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine Dimensionen|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Count|Durchschnitt|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine Dimensionen|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Count|Durchschnitt|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine Dimensionen|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Count|Durchschnitt|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine Dimensionen|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CPU in Prozent|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|Keine Dimensionen|
|Netzwerk eingehend|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|Keine Dimensionen|
|Netzwerk ausgehend|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|Keine Dimensionen|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|Keine Dimensionen|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|Keine Dimensionen|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|Keine Dimensionen|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|Keine Dimensionen|
|Verbleibende CPU-Guthaben|Verbleibende CPU-Guthaben|Count|Durchschnitt|Gesamtanzahl von Guthaben, die für den Burst verfügbar sind|Keine Dimensionen|
|Verbrauchte CPU-Guthaben|Verbrauchte CPU-Guthaben|Count|Durchschnitt|Gesamtanzahl von Guthaben, die vom virtuellen Computer verwendet werden|Keine Dimensionen|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CpuUsage|CPU-Auslastung|Count|Durchschnitt|CPU-Auslastung für alle Kerne in Millicores|containerName|
|MemoryUsage|Speicherauslastung|Byte|Durchschnitt|Gesamtspeicherauslastung in Byte|containerName|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|DCIApiCalls|Customer Insights-API-Aufrufe|Count|Gesamt||Keine Dimensionen|
|DCIMappingImportOperationSuccessfulLines|Erfolgreiche Zeilen beim Zuordnen von Importvorgängen|Count|Gesamt||Keine Dimensionen|
|DCIMappingImportOperationFailedLines|Fehlgeschlagene Zeilen beim Zuordnen von Importvorgängen|Count|Gesamt||Keine Dimensionen|
|DCIMappingImportOperationTotalLines|Gesamte Zeilen beim Zuordnen von Importvorgängen|Count|Gesamt||Keine Dimensionen|
|DCIMappingImportOperationRuntimeInSeconds|Laufzeit in Sekunden beim Zuordnen von Importvorgängen|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundProfileExportSucceeded|Ausgehender Profilexport erfolgreich|Count|Gesamt||Keine Dimensionen|
|DCIOutboundProfileExportFailed|Fehler beim ausgehenden Profilexport|Count|Gesamt||Keine Dimensionen|
|DCIOutboundProfileExportDuration|Dauer des ausgehenden Profilexports|Sekunden|Gesamt||Keine Dimensionen|
|DCIOutboundKpiExportSucceeded|Ausgehender KPI-Export erfolgreich|Count|Gesamt||Keine Dimensionen|
|DCIOutboundKpiExportFailed|Fehler beim ausgehenden KPI-Export|Count|Gesamt||Keine Dimensionen|
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
|ImportASAValuesFailed|Anzahl der fehlgeschlagenen Importe von ASA-Werten|Count|Gesamt||Keine Dimensionen|
|ImportASAValuesSucceeded|Anzahl der erfolgreichen Importe von ASA-Werten|Count|Gesamt||Keine Dimensionen|
|DCIProfilesCount|Anzahl von Profilinstanzen|Count|Last (Letzter)||Keine Dimensionen|
|DCIInteractionsPerMonthCount|Anzahl von Interaktionen pro Monat|Count|Last (Letzter)||Keine Dimensionen|
|DCIKpisCount|Anzahl von KPIs|Count|Last (Letzter)||Keine Dimensionen|
|DCISegmentsCount|Anzahl von Segmenten|Count|Last (Letzter)||Keine Dimensionen|
|DCIPredictiveMatchPoliciesCount|Anzahl von vorhersagbaren Treffern|Count|Last (Letzter)||Keine Dimensionen|
|DCIPredictionsCount|Anzahl von Vorhersagen|Count|Last (Letzter)||Keine Dimensionen|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|FailedRuns|Fehlerhafte Ausführungen|Count|Gesamt||pipelineName, activityName, windowEnd, windowStart |
|SuccessfulRuns|Erfolgreiche Ausführungen|Count|Gesamt||pipelineName, activityName, windowEnd, windowStart |

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|PipelineFailedRuns|Metriken zu fehlerhaften Pipelineausführungen|Count|Gesamt||FailureType, Name|
|PipelineSucceededRuns|Metriken zu erfolgreichen Pipelineausführungen|Count|Gesamt||FailureType, Name|
|ActivityFailedRuns|Metriken zu fehlerhaften Aktivitätsausführungen|Count|Gesamt||ActivityType, PipelineName, FailureType, Name|
|ActivitySucceededRuns|Metriken zu erfolgreichen Aktivitätsausführungen|Count|Gesamt||ActivityType, PipelineName, FailureType, Name|
|TriggerFailedRuns|Metriken zu fehlerhaften Triggerausführungen|Count|Gesamt||Name, FailureType|
|TriggerSucceededRuns|Metriken zu erfolgreichen Triggerausführungen|Count|Gesamt||Name, FailureType|
|IntegrationRuntimeCpuPercentage|CPU-Auslastung der Integration Runtime|Prozent|Durchschnitt||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableMemory|Verfügbarer Speicher für Integration Runtime|Byte|Durchschnitt||IntegrationRuntimeName, NodeName|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|JobEndedSuccess|Erfolgreiche Vorgänge|Count|Gesamt|Die Anzahl der erfolgreichen Vorgänge|Keine Dimensionen|
|JobEndedFailure|Fehlgeschlagene Vorgänge|Count|Gesamt|Die Anzahl der fehlgeschlagenen Vorgänge|Keine Dimensionen|
|JobEndedCancelled|Abgebrochene Vorgänge|Count|Gesamt|Die Anzahl der abgebrochenen Vorgänge|Keine Dimensionen|
|JobAUEndedSuccess|Erfolgreiche AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für erfolgreiche Vorgänge|Keine Dimensionen|
|JobAUEndedFailure|Fehlgeschlagene AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für fehlgeschlagene Vorgänge|Keine Dimensionen|
|JobAUEndedCancelled|Abgebrochene AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für abgebrochene Vorgänge|Keine Dimensionen|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|TotalStorage|Gesamtspeicher|Byte|Maximum|Gesamtmenge der im Konto gespeicherten Daten|Keine Dimensionen|
|DataWritten|Geschriebene Daten|Byte|Gesamt|Gesamtmenge der in das Konto geschriebenen Daten|Keine Dimensionen|
|DataRead|Gelesene Daten|Byte|Gesamt|Gesamtmenge der im Konto gelesenen Daten|Keine Dimensionen|
|WriteRequests|Schreibanforderungen|Count|Gesamt|Anzahl der Datenschreibanforderungen für das Konto|Keine Dimensionen|
|ReadRequests|Leseanforderungen|Count|Gesamt|Anzahl der Datenleseanforderungen für das Konto|Keine Dimensionen|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Durchschnitt|CPU in Prozent|Keine Dimensionen|
|compute_limit|Grenzwert für Computeeinheit|Count|Durchschnitt|Grenzwert für Computeeinheit|Keine Dimensionen|
|compute_consumption_percent|Prozentsatz von Computeeinheit|Prozent|Durchschnitt|Prozentsatz von Computeeinheit|Keine Dimensionen|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Durchschnitt|Arbeitsspeicher in Prozent|Keine Dimensionen|
|io_consumption_percent|E/A in Prozent|Prozent|Durchschnitt|E/A in Prozent|Keine Dimensionen|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|Keine Dimensionen|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|Keine Dimensionen|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|Keine Dimensionen|
|active_connections|Aktive Verbindungen gesamt|Count|Durchschnitt|Aktive Verbindungen gesamt|Keine Dimensionen|
|connections_failed|Fehlerhafte Verbindungen gesamt|Count|Durchschnitt|Fehlerhafte Verbindungen gesamt|Keine Dimensionen|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Durchschnitt|CPU in Prozent|Keine Dimensionen|
|compute_limit|Grenzwert für Computeeinheit|Count|Durchschnitt|Grenzwert für Computeeinheit|Keine Dimensionen|
|compute_consumption_percent|Prozentsatz von Computeeinheit|Prozent|Durchschnitt|Prozentsatz von Computeeinheit|Keine Dimensionen|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Durchschnitt|Arbeitsspeicher in Prozent|Keine Dimensionen|
|io_consumption_percent|E/A in Prozent|Prozent|Durchschnitt|E/A in Prozent|Keine Dimensionen|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|Keine Dimensionen|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|Keine Dimensionen|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|Keine Dimensionen|
|active_connections|Aktive Verbindungen gesamt|Count|Durchschnitt|Aktive Verbindungen gesamt|Keine Dimensionen|
|connections_failed|Fehlerhafte Verbindungen gesamt|Count|Durchschnitt|Fehlerhafte Verbindungen gesamt|Keine Dimensionen|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)|Count|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten|Keine Dimensionen|
|d2c.telemetry.ingress.success|Telemetry messages sent (Gesendete Telemetrienachrichten)|Count|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden|Keine Dimensionen|
|c2d.commands.egress.complete.success|Commands completed (Abgeschlossene Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät erfolgreich abgeschlossen wurden|Keine Dimensionen|
|c2d.commands.egress.abandon.success|Commands abandoned (Abgebrochene Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgebrochen wurden|Keine Dimensionen|
|c2d.commands.egress.reject.success|Commands rejected (Abgelehnte Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgelehnt wurden|Keine Dimensionen|
|devices.totalDevices|Total devices (Geräte gesamt)|Count|Gesamt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine Dimensionen|
|devices.connectedDevices.allProtocol|Verbundene Geräte|Count|Gesamt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine Dimensionen|
|d2c.telemetry.egress.success|Telemetry messages delivered (Übermittelte Telemetrienachrichten)|Count|Gesamt|Erfolgreich auf Endpunkte geschriebene Nachrichten (Häufigkeit gesamt)|Keine Dimensionen|
|d2c.telemetry.egress.dropped|Dropped messages (Gelöschte Nachrichten)|Count|Gesamt|Anzahl der Nachrichten, die aufgrund eines inaktiven Zustellungsendpunkts gelöscht wurden|Keine Dimensionen|
|d2c.telemetry.egress.orphaned|Verwaiste Nachrichten|Count|Gesamt|Anzahl von Nachrichten, die keiner Route entsprechen (einschließlich der Fallbackroute)|Keine Dimensionen|
|d2c.telemetry.egress.invalid|Invalid messages (Ungültige Nachrichten)|Count|Gesamt|Anzahl von Nachrichten, die aufgrund von Inkompatibilität mit dem Endpunkt nicht übermittelt wurden|Keine Dimensionen|
|d2c.telemetry.egress.fallback|Messages matching fallback condition (Nachrichten, die die Fallbackbedingung erfüllen)|Count|Gesamt|Gibt die Anzahl von Nachrichten an, die auf den Fallbackendpunkt geschrieben werden.|Keine Dimensionen|
|d2c.endpoints.egress.eventHubs|Messages delivered to Event Hub endpoints (An Event Hub-Endpunkte übermittelte Nachrichten)|Count|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Event Hub-Endpunkte geschrieben wurden.Gibt an, wie oft Nachrichten erfolgreich auf Event Hub-Endpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.eventHubs|Message latency for Event Hub endpoints (Nachrichtenwartezeit für Event Hub-Endpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Event Hub-Endpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.serviceBusQueues|Messages delivered to Service Bus Queue endpoints (An Service Bus-Warteschlangenendpunkte übermittelte Nachrichten)|Count|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Service Bus-Warteschlangenendpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.serviceBusQueues|Message latency for Service Bus Queue endpoints (Nachrichtenwartezeit für Service Bus-Warteschlangenendpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.serviceBusTopics|Messages delivered to Service Bus Topic endpoints (An Service Bus-Themenendpunkte übermittelte Nachrichten)|Count|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Service Bus-Themenendpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.serviceBusTopics|Message latency for Service Bus Topic endpoints (Nachrichtenwartezeit für Service Bus-Themenendpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Themenendpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.builtIn.events|Messages delivered to the built-in endpoint (messages/events) (An den integrierten Endpunkt (messages/events) übermittelte Nachrichten)|Count|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf den integrierten Endpunkt (messages/events) geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.builtIn.events|Message latency for the built-in endpoint (messages/events) (Nachrichtenwartezeit für den integrierten Endpunkt (messages/events))|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht beim integrierten Endpunkt (Nachrichten/Ereignisse) in Millisekunden |Keine Dimensionen|
|d2c.endpoints.egress.storage|An Storage-Endpunkte übermittelte Nachrichten|Count|Gesamt|Anzahl der erfolgreichen Schreibvorgänge von Nachrichten auf Storage-Endpunkten|Keine Dimensionen|
|d2c.endpoints.latency.storage|Nachrichtenwartezeit für Storage-Endpunkte|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht auf einem Storage-Endpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.storage.bytes|In den Speicher geschriebene Daten|Byte|Gesamt|Menge der Daten in Bytes, die auf Storage-Endpunkten geschrieben wurden|Keine Dimensionen|
|d2c.endpoints.egress.storage.blobs|In den Speicher geschriebene Blobs|Count|Gesamt|Anzahl von Blobs, die auf Storage-Endpunkten geschrieben werden|Keine Dimensionen|
|d2c.twin.read.success|Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.read.failure|Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.read.size|Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.success|Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.failure|Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.size|Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Gerät initiiert wurden.|Keine Dimensionen|
|c2d.methods.success|Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)|Count|Gesamt|Gibt an, wie viele direkte Methodenaufrufe erfolgreich durchgeführt wurden.|Keine Dimensionen|
|c2d.methods.failure|Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)|Count|Gesamt|Gibt an, wie viele direkte Methodenaufrufe nicht erfolgreich waren.|Keine Dimensionen|
|c2d.methods.requestSize|Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen direkten Methodenaufrufe an.|Keine Dimensionen|
|c2d.methods.responseSize|Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Antworten für die direkte Methode an.|Keine Dimensionen|
|c2d.twin.read.success|Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.read.failure|Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.read.size|Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.success|Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.failure|Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.size|Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Back-End initiiert wurden.|Keine Dimensionen|
|twinQueries.success|Successful twin queries (Erfolgreiche Zwillingsabfragen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine Dimensionen|
|twinQueries.failure|Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine Dimensionen|
|twinQueries.resultSize|Twin queries result size (Ergebnisgröße von Zwillingsabfragen)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert der Ergebnisgröße aller erfolgreichen Zwillingsabfragen.|Keine Dimensionen|
|jobs.createTwinUpdateJob.success|Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine Dimensionen|
|jobs.createTwinUpdateJob.failure|Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Count|Gesamt|Gibt die Anzahl von allen nicht erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine Dimensionen|
|jobs.createDirectMethodJob.success|Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine Dimensionen|
|jobs.createDirectMethodJob.failure|Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine Dimensionen|
|jobs.listJobs.success|Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.listJobs.failure|Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.cancelJob.success|Successful job cancellations (Erfolgreiche Auftragsabbrüche)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine Dimensionen|
|jobs.cancelJob.failure|Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine Dimensionen|
|jobs.queryJobs.success|Successful job queries (Erfolgreiche Auftragsabfragen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.queryJobs.failure|Failed job queries (Nicht erfolgreiche Auftragsabfragen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.completed|Abgeschlossene Aufträge|Count|Gesamt|Gibt die Anzahl von abgeschlossenen Aufträgen an.|Keine Dimensionen|
|jobs.failed|Fehlerhafte Aufträge|Count|Gesamt|Gibt die Anzahl aller fehlerhaften Aufträge an.|Keine Dimensionen|
|d2c.telemetry.ingress.sendThrottle|Anzahl von Drosselungsfehlern|Count|Gesamt|Anzahl von Drosselungsfehlern aufgrund von Drosselungen des Gerätedurchsatzes|Keine Dimensionen|
|dailyMessageQuotaUsed|Gesamtzahl verwendeter Nachrichten|Count|Durchschnitt|Gesamtzahl der heute verwendeten Nachrichten. Dies ist ein kumulativer Wert, der jeden Tag um 00:00 Uhr UTC auf 0 zurückgesetzt wird.|Keine Dimensionen|
|deviceDataUsage|Gerätedatennutzung gesamt|Count|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine Dimensionen|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|RegistrationAttempts|Registrierungsversuche|Count|Gesamt|Anzahl von versuchten Geräteregistrierungen|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Zugewiesene Geräte|Count|Gesamt|Anzahl von Geräten, die einem IoT-Hub zugewiesen sind|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Nachweisversuche|Count|Gesamt|Anzahl von versuchten Gerätenachweisen|ProvisioningServiceName, Status, Protokoll|

## <a name="microsoftdeviceselasticpools"></a>Microsoft.Devices/ElasticPools

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|elasticPool.requestedUsageRate|Angeforderte Nutzungsrate|Prozent|Durchschnitt|Angeforderte Nutzungsrate|Keine Dimensionen|

## <a name="microsoftdeviceselasticpoolsiothubtenants"></a>Microsoft.Devices/ElasticPools/IotHubTenants

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|tenantHub.requestedUsageRate|Angeforderte Nutzungsrate|Prozent|Durchschnitt|Angeforderte Nutzungsrate|Keine Dimensionen|
|deviceDataUsage|Gerätedatennutzung gesamt|Count|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine Dimensionen|
|d2c.telemetry.Ingress.allProtocol|Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)|Count|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten|Keine Dimensionen|
|d2c.telemetry.ingress.success|Telemetry messages sent (Gesendete Telemetrienachrichten)|Count|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden|Keine Dimensionen|
|c2d.commands.egress.complete.success|Commands completed (Abgeschlossene Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät erfolgreich abgeschlossen wurden|Keine Dimensionen|
|c2d.commands.egress.abandon.success|Commands abandoned (Abgebrochene Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgebrochen wurden|Keine Dimensionen|
|c2d.commands.egress.reject.success|Commands rejected (Abgelehnte Befehle)|Count|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgelehnt wurden|Keine Dimensionen|
|devices.totalDevices|Total devices (Geräte gesamt)|Count|Gesamt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine Dimensionen|
|devices.connectedDevices.allProtocol|Verbundene Geräte|Count|Gesamt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine Dimensionen|
|d2c.telemetry.egress.success|Telemetry messages delivered (Übermittelte Telemetrienachrichten)|Count|Gesamt|Erfolgreich auf Endpunkte geschriebene Nachrichten (Häufigkeit gesamt)|Keine Dimensionen|
|d2c.telemetry.egress.dropped|Dropped messages (Gelöschte Nachrichten)|Count|Gesamt|Anzahl der Nachrichten, die aufgrund eines inaktiven Zustellungsendpunkts gelöscht wurden|Keine Dimensionen|
|d2c.telemetry.egress.orphaned|Verwaiste Nachrichten|Count|Gesamt|Anzahl von Nachrichten, die keiner Route entsprechen (einschließlich der Fallbackroute)|Keine Dimensionen|
|d2c.telemetry.egress.invalid|Invalid messages (Ungültige Nachrichten)|Count|Gesamt|Anzahl von Nachrichten, die aufgrund von Inkompatibilität mit dem Endpunkt nicht übermittelt wurden|Keine Dimensionen|
|d2c.telemetry.egress.fallback|Messages matching fallback condition (Nachrichten, die die Fallbackbedingung erfüllen)|Count|Gesamt|Gibt die Anzahl von Nachrichten an, die auf den Fallbackendpunkt geschrieben werden.|Keine Dimensionen|
|d2c.endpoints.egress.eventHubs|Messages delivered to Event Hub endpoints (An Event Hub-Endpunkte übermittelte Nachrichten)|Count|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Event Hub-Endpunkte geschrieben wurden.Gibt an, wie oft Nachrichten erfolgreich auf Event Hub-Endpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.eventHubs|Message latency for Event Hub endpoints (Nachrichtenwartezeit für Event Hub-Endpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Event Hub-Endpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.serviceBusQueues|Messages delivered to Service Bus Queue endpoints (An Service Bus-Warteschlangenendpunkte übermittelte Nachrichten)|Count|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Service Bus-Warteschlangenendpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.serviceBusQueues|Message latency for Service Bus Queue endpoints (Nachrichtenwartezeit für Service Bus-Warteschlangenendpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.serviceBusTopics|Messages delivered to Service Bus Topic endpoints (An Service Bus-Themenendpunkte übermittelte Nachrichten)|Count|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Service Bus-Themenendpunkte geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.serviceBusTopics|Message latency for Service Bus Topic endpoints (Nachrichtenwartezeit für Service Bus-Themenendpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Themenendpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.builtIn.events|Messages delivered to the built-in endpoint (messages/events) (An den integrierten Endpunkt (messages/events) übermittelte Nachrichten)|Count|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf den integrierten Endpunkt (messages/events) geschrieben wurden.|Keine Dimensionen|
|d2c.endpoints.latency.builtIn.events|Message latency for the built-in endpoint (messages/events) (Nachrichtenwartezeit für den integrierten Endpunkt (messages/events))|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht beim integrierten Endpunkt (Nachrichten/Ereignisse) in Millisekunden |Keine Dimensionen|
|d2c.endpoints.egress.storage|An Storage-Endpunkte übermittelte Nachrichten|Count|Gesamt|Anzahl der erfolgreichen Schreibvorgänge von Nachrichten auf Storage-Endpunkten|Keine Dimensionen|
|d2c.endpoints.latency.storage|Nachrichtenwartezeit für Storage-Endpunkte|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht auf einem Storage-Endpunkt in Millisekunden|Keine Dimensionen|
|d2c.endpoints.egress.storage.bytes|In den Speicher geschriebene Daten|Byte|Gesamt|Menge der Daten in Bytes, die auf Storage-Endpunkten geschrieben wurden|Keine Dimensionen|
|d2c.endpoints.egress.storage.blobs|In den Speicher geschriebene Blobs|Count|Gesamt|Anzahl von Blobs, die auf Storage-Endpunkten geschrieben werden|Keine Dimensionen|
|d2c.twin.read.success|Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.read.failure|Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.read.size|Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.success|Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.failure|Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine Dimensionen|
|d2c.twin.update.size|Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Gerät initiiert wurden.|Keine Dimensionen|
|c2d.methods.success|Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)|Count|Gesamt|Gibt an, wie viele direkte Methodenaufrufe erfolgreich durchgeführt wurden.|Keine Dimensionen|
|c2d.methods.failure|Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)|Count|Gesamt|Gibt an, wie viele direkte Methodenaufrufe nicht erfolgreich waren.|Keine Dimensionen|
|c2d.methods.requestSize|Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen direkten Methodenaufrufe an.|Keine Dimensionen|
|c2d.methods.responseSize|Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Antworten für die direkte Methode an.|Keine Dimensionen|
|c2d.twin.read.success|Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.read.failure|Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.read.size|Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.success|Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.failure|Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine Dimensionen|
|c2d.twin.update.size|Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Back-End initiiert wurden.|Keine Dimensionen|
|twinQueries.success|Successful twin queries (Erfolgreiche Zwillingsabfragen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine Dimensionen|
|twinQueries.failure|Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine Dimensionen|
|twinQueries.resultSize|Twin queries result size (Ergebnisgröße von Zwillingsabfragen)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert der Ergebnisgröße aller erfolgreichen Zwillingsabfragen.|Keine Dimensionen|
|jobs.createTwinUpdateJob.success|Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine Dimensionen|
|jobs.createTwinUpdateJob.failure|Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Count|Gesamt|Gibt die Anzahl von allen nicht erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine Dimensionen|
|jobs.createDirectMethodJob.success|Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Count|Gesamt|Gibt die Anzahl von erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine Dimensionen|
|jobs.createDirectMethodJob.failure|Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Count|Gesamt|Gibt die Anzahl von nicht erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine Dimensionen|
|jobs.listJobs.success|Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.listJobs.failure|Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.cancelJob.success|Successful job cancellations (Erfolgreiche Auftragsabbrüche)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine Dimensionen|
|jobs.cancelJob.failure|Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine Dimensionen|
|jobs.queryJobs.success|Successful job queries (Erfolgreiche Auftragsabfragen)|Count|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.queryJobs.failure|Failed job queries (Nicht erfolgreiche Auftragsabfragen)|Count|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine Dimensionen|
|jobs.completed|Abgeschlossene Aufträge|Count|Gesamt|Gibt die Anzahl von abgeschlossenen Aufträgen an.|Keine Dimensionen|
|jobs.failed|Fehlerhafte Aufträge|Count|Gesamt|Gibt die Anzahl aller fehlerhaften Aufträge an.|Keine Dimensionen|
|d2c.telemetry.ingress.sendThrottle|Anzahl von Drosselungsfehlern|Count|Gesamt|Anzahl von Drosselungsfehlern aufgrund von Drosselungen des Gerätedurchsatzes|Keine Dimensionen|
|dailyMessageQuotaUsed|Gesamtzahl verwendeter Nachrichten|Count|Durchschnitt|Gesamtzahl der heute verwendeten Nachrichten. Dies ist ein kumulativer Wert, der jeden Tag um 00:00 Uhr UTC auf 0 zurückgesetzt wird.|Keine Dimensionen|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|MetadataRequests|Anforderungen von Metadaten|Count|Count|Anzahl der Metadatenanforderungen. Cosmos DB unterhält eine Sammlung von Systemmetadaten für jedes Konto, wodurch Sie Sammlungen, Datenbanken usw. und deren Konfigurationen ohne anfallende Kosten auflisten können.|GlobalDatabaseAccountName, DatabaseName, CollectionName, Region, StatusCode|
|MongoRequestCharge|Kosten der Mongo-Anforderung|Count|Gesamt|Verbrauchte Mongo-Anforderungseinheiten|GlobalDatabaseAccountName, DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequests|Mongo-Anforderungen|Count|Count|Anzahl der ausgegebenen Mongo-Anforderungen|GlobalDatabaseAccountName, DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|TotalRequestUnits|Anforderungseinheiten gesamt|Count|Gesamt|Verbrauchte Anforderungseinheiten|GlobalDatabaseAccountName, DatabaseName, CollectionName, Region, StatusCode|
|TotalRequests|Anzahl von Anforderungen|Count|Count|Anzahl von gesendeten Anforderungen|GlobalDatabaseAccountName, DatabaseName, CollectionName, Region, StatusCode|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|SuccessfulRequests|Erfolgreiche Anforderungen (Vorschau)|Count|Gesamt|Erfolgreiche Anforderungen für Microsoft.EventHub. (Vorschau)|EntityName|
|ServerErrors|Serverfehler. (Vorschau)|Count|Gesamt|Serverfehler für Microsoft.EventHub. (Vorschau)|EntityName|
|UserErrors|Benutzerfehler. (Vorschau)|Count|Gesamt|Benutzerfehler für Microsoft.EventHub. (Vorschau)|EntityName|
|QuotaExceededErrors|Fehler aufgrund von Kontingentüberschreitung. (Vorschau)|Count|Gesamt|Fehler aufgrund von Kontingentüberschreitung für Microsoft.EventHub. (Vorschau)|EntityName|
|ThrottledRequests|Gedrosselte Anforderungen. (Vorschau)|Count|Gesamt|Gedrosselte Anforderungen für Microsoft.EventHub. (Vorschau)|EntityName|
|IncomingRequests|Eingehende Anforderungen (Vorschau)|Count|Gesamt|Eingehende Anforderungen für Microsoft.EventHub. (Vorschau)|EntityName|
|IncomingMessages|Eingehende Nachrichten (Vorschau)|Count|Gesamt|Eingehende Nachrichten für Microsoft.EventHub. (Vorschau)|EntityName|
|OutgoingMessages|Ausgehende Nachrichten (Vorschau)|Count|Gesamt|Ausgehende Nachrichten für Microsoft.EventHub. (Vorschau)|EntityName|
|IncomingBytes|Eingehende Bytes. (Vorschau)|Byte|Gesamt|Eingehende Bytes für Microsoft.EventHub. (Vorschau)|EntityName|
|OutgoingBytes|Ausgehende Bytes. (Vorschau)|Byte|Gesamt|Ausgehende Bytes für Microsoft.EventHub. (Vorschau)|EntityName|
|ActiveConnections|Aktive Verbindungen (Vorschau)|Count|Durchschnitt|Aktive Verbindungen gesamt für Microsoft.EventHub. (Vorschau)|Keine Dimensionen|
|ConnectionsOpened|Geöffnete Verbindungen. (Vorschau)|Count|Durchschnitt|Geöffnete Verbindungen für Microsoft.EventHub. (Vorschau)|EntityName|
|ConnectionsClosed|Geschlossene Verbindungen. (Vorschau)|Count|Durchschnitt|Geschlossene Verbindungen für Microsoft.EventHub. (Vorschau)|EntityName|
|CaptureBacklog|Backlog erfassen. (Vorschau)|Count|Gesamt|Erfassen des Backlogs für Microsoft.EventHub. (Vorschau)|EntityName|
|CapturedMessages|Erfasste Nachrichten. (Vorschau)|Count|Gesamt|Erfasste Nachrichten für Microsoft.EventHub. (Vorschau)|EntityName|
|CapturedBytes|Erfasste Bytes. (Vorschau)|Byte|Gesamt|Erfasste Bytes für Microsoft.EventHub. (Vorschau)|EntityName|
|Größe|Größe (Vorschau)|Byte|Durchschnitt|Größe eines EventHub in Bytes. (Vorschau)|EntityName|
|INREQS|Eingehende Anforderungen|Count|Gesamt|Gesamtanzahl von eingehenden Sendeanforderungen für einen Namespace|Keine Dimensionen|
|SUCCREQ|Erfolgreiche Anforderungen|Count|Gesamt|Gesamtzahl der erfolgreichen Anforderungen für einen Namespace|Keine Dimensionen|
|FAILREQ|Anforderungsfehler|Count|Gesamt|Gesamtzahl der fehlgeschlagenen Anforderungen für einen Namespace|Keine Dimensionen|
|SVRBSY|Fehler durch ausgelasteten Server|Count|Gesamt|Gesamtzahl der Fehler durch ausgelasteten Server für einen Namespace|Keine Dimensionen|
|INTERR|Interne Serverfehler|Count|Gesamt|Gesamtzahl der internen Serverfehler für einen Namespace|Keine Dimensionen|
|MISCERR|Andere Fehler|Count|Gesamt|Gesamtzahl der fehlgeschlagenen Anforderungen für einen Namespace|Keine Dimensionen|
|INMSGS|Eingehende Nachrichten (veraltet)|Count|Gesamt|Gesamtzahl von eingehenden Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für eingehende Nachrichten.|Keine Dimensionen|
|EHINMSGS|Eingehende Nachrichten|Count|Gesamt|Gesamtzahl der eingehenden Nachrichten für einen Namespace|Keine Dimensionen|
|OUTMSGS|Ausgehende Nachrichten (veraltet)|Count|Gesamt|Gesamtzahl von ausgehenden Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für ausgehende Nachrichten.|Keine Dimensionen|
|EHOUTMSGS|Ausgehende Nachrichten|Count|Gesamt|Gesamtzahl der ausgehenden Nachrichten für einen Namespace|Keine Dimensionen|
|EHINMBS|Eingehende Bytes (veraltet)|Byte|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für eingehende Bytes.|Keine Dimensionen|
|EHINBYTES|Eingehende Bytes|Byte|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace|Keine Dimensionen|
|EHOUTMBS|Ausgehende Bytes (veraltet)|Byte|Gesamt|Event Hub-Durchsatz ausgehender Nachrichten für einen Namespace Diese Metrik ist veraltet. Verwenden Sie stattdessen die Metrik für ausgehende Bytes.|Keine Dimensionen|
|EHOUTBYTES|Ausgehende Bytes|Byte|Gesamt|Event Hub-Durchsatz ausgehender Nachrichten für einen Namespace|Keine Dimensionen|
|EHABL|Archivierte Backlognachrichten|Count|Gesamt|Event Hub-Archivnachrichten im Backlog für einen Namespace|Keine Dimensionen|
|EHAMSGS|Archivnachrichten|Count|Gesamt|Event Hub-Archivnachrichten in einem Namespace|Keine Dimensionen|
|EHAMBS|Durchsatz von Archivnachrichten|Byte|Gesamt|Event Hub-Durchsatz archivierter Nachrichten in einem Namespace|Keine Dimensionen|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|GatewayRequests|Gatewayanforderungen|Count|Gesamt|Anzahl von Gatewayanforderungen|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Kategorisierte Gatewayanforderungen|Count|Gesamt|Anzahl von Gatewayanforderungen nach Kategorien (1xx/2xx/3xx/4xx/5xx)|ClusterDnsName, HttpStatus|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|ObservedMetricValue|Beobachteter Metrikwert|Count|Durchschnitt|Der Wert, der beim Ausführen der automatischen Skalierung berechnet wurde.|MetricTriggerSource|
|MetricThreshold|Schwellenwert der Metrik|Count|Durchschnitt|Der bei der Ausführung der automatischen Skalierung konfigurierte Schwellenwert für die automatische Skalierung.|MetricTriggerRule|
|ObservedCapacity|Beobachtete Kapazität|Count|Durchschnitt|Die Kapazität, die beim Ausführen der automatischen Skalierung gemeldet wurde.|Keine Dimensionen|
|ScaleActionsInitiated|Initiierte Skalierungsaktionen|Count|Gesamt|Die Richtung des Skalierungsvorgangs.|ScaleDirection|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|ServiceApiHit|Dienst-API-Treffer, gesamt|Count|Count|Gesamtanzahl der Dienst-API-Treffer|ActivityType, ActivityName|
|ServiceApiLatency|Gesamtwartezeit für Dienst-API|Millisekunden|Durchschnitt|Gesamtwartezeit für Dienst-API-Anforderungen|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Dienst-API-Ergebnisse, gesamt|Count|Count|Gesamtanzahl der Dienst-API-Ergebnisse|ActivityType, ActivityName, StatusCode|

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|Latency|Latency|Millisekunden|Durchschnitt|Dauer von API-Aufrufen|OperationName, OperationResult|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|RunsStarted|Gestartete Ausführungen|Count|Gesamt|Anzahl gestarteter Workflowausführungen.|Keine Dimensionen|
|RunsCompleted|Abgeschlossene Ausführungen|Count|Gesamt|Anzahl abgeschlossener Workflowausführungen.|Keine Dimensionen|
|RunsSucceeded|Erfolgreiche Ausführungen|Count|Gesamt|Anzahl erfolgreicher Workflowausführungen.|Keine Dimensionen|
|RunsFailed|Ausführungsfehler|Count|Gesamt|Anzahl fehlerhafter Workflowausführungen.|Keine Dimensionen|
|RunsCancelled|Abgebrochene Ausführungen|Count|Gesamt|Anzahl abgebrochener Workflowausführungen.|Keine Dimensionen|
|RunLatency|Ausführungslatenz|Sekunden|Durchschnitt|Latenz abgeschlossener Workflowausführungen.|Keine Dimensionen|
|RunSuccessLatency|Latenz erfolgreicher Ausführungen|Sekunden|Durchschnitt|Latenz erfolgreicher Workflowausführungen.|Keine Dimensionen|
|RunThrottledEvents|Ereignisse zu gedrosselten Ausführungen|Count|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen oder Triggern.|Keine Dimensionen|
|RunFailurePercentage|Prozentsatz der Ausführungsfehler|Prozent|Gesamt|Prozentsatz fehlerhafter Workflowausführungen|Keine Dimensionen|
|ActionsStarted|Gestartete Aktionen |Count|Gesamt|Anzahl gestarteter Workflowaktionen.|Keine Dimensionen|
|ActionsCompleted|Abgeschlossene Aktionen |Count|Gesamt|Anzahl abgeschlossener Workflowaktionen.|Keine Dimensionen|
|ActionsSucceeded|Erfolgreiche Aktionen |Count|Gesamt|Anzahl erfolgreicher Workflowaktionen.|Keine Dimensionen|
|ActionsFailed|Aktionsfehler|Count|Gesamt|Anzahl fehlerhafter Workflowaktionen.|Keine Dimensionen|
|ActionsSkipped|Übersprungene Aktionen |Count|Gesamt|Anzahl übersprungener Workflowaktionen.|Keine Dimensionen|
|ActionLatency|Aktionslatenz |Sekunden|Durchschnitt|Latenz abgeschlossener Workflowaktionen.|Keine Dimensionen|
|ActionSuccessLatency|Latenz erfolgreicher Aktionen |Sekunden|Durchschnitt|Latenz erfolgreicher Workflowaktionen.|Keine Dimensionen|
|ActionThrottledEvents|Ereignisse zu gedrosselten Aktionen|Count|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen.|Keine Dimensionen|
|TriggersStarted|Gestartete Trigger |Count|Gesamt|Anzahl gestarteter Workflowtrigger.|Keine Dimensionen|
|TriggersCompleted|Abgeschlossene Trigger |Count|Gesamt|Anzahl abgeschlossener Workflowtrigger.|Keine Dimensionen|
|TriggersSucceeded|Erfolgreiche Trigger |Count|Gesamt|Anzahl erfolgreicher Workflowtrigger.|Keine Dimensionen|
|TriggersFailed|Triggerfehler |Count|Gesamt|Anzahl fehlerhafter Workflowtrigger.|Keine Dimensionen|
|TriggersSkipped|Übersprungene Trigger|Count|Gesamt|Anzahl übersprungener Workflowtrigger.|Keine Dimensionen|
|TriggersFired|Ausgelöste Trigger |Count|Gesamt|Anzahl ausgelöster Workflowtrigger.|Keine Dimensionen|
|TriggerLatency|Triggerlatenz |Sekunden|Durchschnitt|Latenz abgeschlossener Workflowtrigger.|Keine Dimensionen|
|TriggerFireLatency|Latenz beim Auslösen von Triggern |Sekunden|Durchschnitt|Latenz ausgelöster Workflowtrigger.|Keine Dimensionen|
|TriggerSuccessLatency|Latenz erfolgreicher Trigger |Sekunden|Durchschnitt|Latenz erfolgreicher Workflowtrigger.|Keine Dimensionen|
|TriggerThrottledEvents|Ereignisse zu gedrosselten Triggern|Count|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowtriggern.|Keine Dimensionen|
|BillableActionExecutions|Ausführungen abrechenbarer Aktionen|Count|Gesamt|Anzahl der Workflowaktionsausführungen, die in Rechnung gestellt werden|Keine Dimensionen|
|BillableTriggerExecutions|Ausführungen abrechenbarer Trigger|Count|Gesamt|Anzahl der Workflowtriggerausführungen, die in Rechnung gestellt werden|Keine Dimensionen|
|TotalBillableExecutions|Gesamtzahl der abrechenbaren Ausführungen|Count|Gesamt|Anzahl der Workflowausführungen, die in Rechnung gestellt werden|Keine Dimensionen|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|VipAvailability|VIP-Verfügbarkeit|Count|Durchschnitt|Verfügbarkeit von VIP-Endpunkten, basierend auf Testergebnissen|VipAddress, VipPort|
|DipAvailability|DIP-Verfügbarkeit|Count|Durchschnitt|Verfügbarkeit von DIP-Endpunkten, basierend auf Testergebnissen|ProtocolType, DipPort, VipAddress, VipPort, DipAddress|
|ByteCount|Byteanzahl|Count|Gesamt|Gesamtanzahl von Bytes, die im Zeitraum übertragen wurden|VipAddress, VipPort, Richtung|
|PacketCount|Paketzahl|Count|Gesamt|Gesamtanzahl von Paketen, die im Zeitraum übertragen wurden|VipAddress, VipPort, Richtung|
|SYNCount|SYN-Anzahl|Count|Gesamt|Gesamtanzahl von SYN-Paketen, die im Zeitraum übertragen wurden|VipAddress, VipPort, Richtung|
|SnatConnectionCount|Anzahl von SNAT-Verbindungen|Count|Gesamt|Gesamtanzahl von neuen SNAT-Verbindungen, die innerhalb des Zeitraums erstellt wurden|VipAddress, DipAddress, ConnectionState|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|QueryVolume|Abfragevolume|Count|Gesamt|Anzahl von Abfragen für eine DNS-Zone|Keine Dimensionen|
|RecordSetCount|Anzahl von Datensatzgruppen|Count|Maximum|Anzahl von Datensatzgruppen in einer DNS-Zone|Keine Dimensionen|
|RecordSetCapacityUtilization|Kapazitätsauslastung von Datensatzgruppen|Prozent|Maximum|Von einer DNS-Zone genutzte Kapazität einer Datensatzgruppe in Prozent|Keine Dimensionen|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|PacketsInDDoS|Als DDoS eingehende Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende Pakete|Keine Dimensionen|
|PacketsDroppedDDoS|Als DDoS eingehende gelöschte Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende gelöschte Pakete|Keine Dimensionen|
|PacketsForwardedDDoS|Weitergeleitete als DDoS eingehende Pakete|Anzahl pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende Pakete|Keine Dimensionen|
|TCPPacketsInDDoS|Als DDoS eingehende TCP-Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende TCP-Pakete|Keine Dimensionen|
|TCPPacketsDroppedDDoS|Als DDoS eingehende gelöschte TCP-Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende gelöschte TCP-Pakete|Keine Dimensionen|
|TCPPacketsForwardedDDoS|Weitergeleitete als DDoS eingehende TCP-Pakete|Anzahl pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende TCP-Pakete|Keine Dimensionen|
|UDPPacketsInDDoS|Als DDoS eingehende UDP-Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende UDP-Pakete|Keine Dimensionen|
|UDPPacketsDroppedDDoS|Als DDoS eingehende gelöschte UDP-Pakete|Anzahl pro Sekunde|Maximum|Als DDoS eingehende gelöschte UDP-Pakete|Keine Dimensionen|
|UDPPacketsForwardedDDoS|Weitergeleitete als DDoS eingehende UDP-Pakete|Anzahl pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende UDP-Pakete|Keine Dimensionen|
|BytesInDDoS|Als DDoS eingehende Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende Bytes|Keine Dimensionen|
|BytesDroppedDDoS|Als DDoS eingehende gelöschte Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende gelöschte Bytes|Keine Dimensionen|
|BytesForwardedDDoS|Weitergeleitete als DDoS eingehende Bytes|Bytes pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende Bytes|Keine Dimensionen|
|TCPBytesInDDoS|Als DDoS eingehende TCP-Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende TCP-Bytes|Keine Dimensionen|
|TCPBytesDroppedDDoS|Als DDoS eingehende gelöschte TCP-Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende gelöschte TCP-Bytes|Keine Dimensionen|
|TCPBytesForwardedDDoS|Weitergeleitete als DDoS eingehende TCP-Bytes|Bytes pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende TCP-Bytes|Keine Dimensionen|
|UDPBytesInDDoS|Als DDoS eingehende UDP-Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende UDP-Bytes|Keine Dimensionen|
|UDPBytesDroppedDDoS|Als DDoS eingehende gelöschte UDP-Bytes|Bytes pro Sekunde|Maximum|Als DDoS eingehende gelöschte UDP-Bytes|Keine Dimensionen|
|UDPBytesForwardedDDoS|Weitergeleitete als DDoS eingehende UDP-Bytes|Bytes pro Sekunde|Maximum|Weitergeleitete als DDoS eingehende UDP-Bytes|Keine Dimensionen|
|IfUnderDDoSAttack|Unter DDoS-Angriff oder nicht|Count|Maximum|Unter DDoS-Angriff oder nicht|Keine Dimensionen|
|DDoSTriggerTCPPackets|Eingehende TCP-Pakete, um die DDoS-Entschärfung auszulösen|Anzahl pro Sekunde|Maximum|Eingehende TCP-Pakete, um die DDoS-Entschärfung auszulösen|Keine Dimensionen|
|DDoSTriggerUDPPackets|Eingehende UDP-Pakete, um die DDoS-Entschärfung auszulösen|Anzahl pro Sekunde|Maximum|Eingehende UDP-Pakete, um die DDoS-Entschärfung auszulösen|Keine Dimensionen|
|DDoSTriggerSYNPackets|Eingehende SYN-Pakete, um die DDoS-Entschärfung auszulösen|Anzahl pro Sekunde|Maximum|Eingehende SYN-Pakete, um die DDoS-Entschärfung auszulösen|Keine Dimensionen|
|VipAvailability|Verfügbarkeit|Count|Durchschnitt|Durchschnittliche Verfügbarkeit der IP-Adresse innerhalb des Zeitraums|Port|
|ByteCount|Byteanzahl|Count|Gesamt|Gesamtanzahl von Bytes, die im Zeitraum übertragen wurden|Port, Richtung|
|PacketCount|Paketzahl|Count|Gesamt|Gesamtanzahl von Paketen, die im Zeitraum übertragen wurden|Port, Richtung|
|SynCount|SYN-Anzahl|Count|Gesamt|Gesamtanzahl von SYN-Paketen, die im Zeitraum übertragen wurden|Port, Richtung|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|Throughput|Throughput|Bytes pro Sekunde|Gesamt|Anzahl von Bytes pro Sekunde, die das Application Gateway bereitgestellt hat|Keine Dimensionen|
|UnhealthyHostCount|Anzahl von fehlerhaften Hosts|Count|Durchschnitt|Die Anzahl fehlerhafter Back-End-Hosts. Sie können auf Back-End-Pool-Basis filtern, um fehlerfreie/fehlerhafte Hosts in einem bestimmten Back-End-Pool anzuzeigen.|BackendSettingsPool|
|HealthyHostCount|Anzahl von fehlerfreien Hosts|Count|Durchschnitt|Die Anzahl fehlerfreier Back-End-Hosts. Sie können auf Back-End-Pool-Basis filtern, um fehlerfreie/fehlerhafte Hosts in einem bestimmten Back-End-Pool anzuzeigen.|BackendSettingsPool |
|TotalRequests|Anzahl von Anforderungen|Count|Gesamt|Anzahl von erfolgreichen Anforderungen über Application Gateway|BackendSettingsPool|
|FailedRequests|Anforderungsfehler|Count|Gesamt|Anzahl von fehlerhaften Anforderungen über Application Gateway|BackendSettingsPool|
|ResponseStatus|Antwortstatus|Count|Gesamt|Von Application Gateway zurückgegebener HTTP-Antwortstatus. Die Antwortstatuscode-Verteilung kann weiter kategorisiert werden, um Antworten in 2xx-, 3xx-, 4xx- und 5xx-Kategorien anzuzeigen.|HttpStatusGroup|
|CurrentConnections|Aktuelle Verbindungen|Count|Gesamt|Anzahl von aktuellen Verbindungen mit Application Gateway|Keine Dimensionen|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Bandbreite des Tunnels|Bytes pro Sekunde|Durchschnitt|Durchschnittliche Bandbreite eines Tunnels in Bytes pro Sekunde|ConnectionName, RemoteIP|
|TunnelEgressBytes|Vom Tunnel ausgehende Bytes|Byte|Gesamt|Ausgehende Bytes eines Tunnels|ConnectionName, RemoteIP|
|TunnelIngressBytes|In Tunnel eingehende Bytes|Byte|Gesamt|Eingehende Bytes eines Tunnels|ConnectionName, RemoteIP|
|TunnelEgressPackets|Vom Tunnel ausgehende Pakete|Count|Gesamt|Ausgehende Paketanzahl für einen Tunnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|In Tunnel eingehende Pakete|Count|Gesamt|Eingehende Paketanzahl für einen Tunnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Ausgehende gelöschte Pakete eines Tunnels durch einen TS-Konflikt|Count|Gesamt|Anzahl der durch einen Konflikt mit dem Datenverkehrsselektor eines Tunnels gelöschten ausgehenden Pakete|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Eingehende gelöschte Pakete eines Tunnels durch einen TS-Konflikt|Count|Gesamt|Anzahl der durch einen Konflikt mit dem Datenverkehrsselektor eines Tunnels gelöschten eingehenden Pakete|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|Anzahl pro Sekunde|Durchschnitt|In Azure eingehende Bits pro Sekunde|Keine Dimensionen|
|BitsOutPerSecond|BitsOutPerSecond|Anzahl pro Sekunde|Durchschnitt|Aus Azure ausgehende Bits pro Sekunde|Keine Dimensionen|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|QpsByEndpoint|Abfragen nach zurückgegebenem Endpunkt|Count|Gesamt|Häufigkeit, mit der ein Endpunkt des Traffic Managers innerhalb des angegebenen Zeitrahmens zurückgegeben wurde|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Endpunktstatus nach Endpunkt|Count|Maximum|1, wenn der Teststatus des Endpunkts „Aktiviert“ ist, andernfalls 0.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|ProbesFailedPercent|Fehlerhafte Tests in Prozent|Prozent|Durchschnitt|Fehlerhafte Tests bei Konnektivitätsüberwachung in Prozent|Keine Dimensionen|
|AverageRoundtripMs|Durchschn. Roundtripzeit (ms)|Millisekunden|Durchschnitt|Durchschnittliche Netzwerk-Roundtripzeit (ms) für die Konnektivitätsüberwachung von Testdatenverkehr, der zwischen Quelle und Ziel gesendet wurde.|Keine Dimensionen|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|registration.all|Registrierungsvorgänge|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsvorgängen an (Erstellungen, Aktualisierungen, Abfragen und Löschungen). |Keine Dimensionen|
|registration.create|Registrierungserstellungsvorgänge|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungserstellungen an.|Keine Dimensionen|
|registration.update|Registrierungsaktualisierungsvorgänge|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsaktualisierungen an.|Keine Dimensionen|
|registration.get|Registrierungslesevorgänge|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsabfragen an.|Keine Dimensionen|
|registration.delete|Registrierungslöschvorgänge|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungslöschungen an.|Keine Dimensionen|
|incoming|Eingehende Nachrichten|Count|Gesamt|Gibt die Anzahl von allen erfolgreichen Übermittlungs-API-Aufrufen an. |Keine Dimensionen|
|incoming.scheduled|Geplante Pushbenachrichtigungen (gesendet)|Count|Gesamt|Geplante Pushbenachrichtigungen (abgebrochen)|Keine Dimensionen|
|incoming.scheduled.cancel|Geplante Pushbenachrichtigungen (abgebrochen)|Count|Gesamt|Geplante Pushbenachrichtigungen (abgebrochen)|Keine Dimensionen|
|scheduled.pending|Ausstehende Pushbenachrichtigungen|Count|Gesamt|Ausstehende Pushbenachrichtigungen|Keine Dimensionen|
|installation.all|Installationsverwaltungsvorgänge|Count|Gesamt|Installationsverwaltungsvorgänge|Keine Dimensionen|
|installation.get|Installationsvorgänge abrufen|Count|Gesamt|Installationsvorgänge abrufen|Keine Dimensionen|
|installation.upsert|Installationsvorgänge erstellen oder aktualisieren|Count|Gesamt|Installationsvorgänge erstellen oder aktualisieren|Keine Dimensionen|
|installation.patch|Patchinstallationsvorgänge|Count|Gesamt|Patchinstallationsvorgänge|Keine Dimensionen|
|installation.delete|Installationsvorgänge löschen|Count|Gesamt|Installationsvorgänge löschen|Keine Dimensionen|
|outgoing.allpns.success|Erfolgreiche Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.allpns.invalidpayload|Nutzlastfehler|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil vom PNS ein Nutzlastfehler zurückgegeben wurde.|Keine Dimensionen|
|outgoing.allpns.pnserror|Fehler des externen Benachrichtigungssystems|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil ein Problem bei der Kommunikation mit dem PNS vorlag (ausschließlich Authentifizierungsprobleme).|Keine Dimensionen|
|outgoing.allpns.channelerror|Kanalfehler|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der Kanal ungültig, nicht der richtigen App zugeordnet, gedrosselt oder abgelaufen war.|Keine Dimensionen|
|outgoing.allpns.badorexpiredchannel|Kanalfehler (unzulässig oder abgelaufen)|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der Kanal, das Token oder die Registrierungs-ID in der Registrierung abgelaufen oder ungültig war.|Keine Dimensionen|
|outgoing.wns.success|Erfolgreiche WNS-Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.wns.invalidcredentials|WNS-Autorisierungsfehler (ungültige Anmeldeinformationen)|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden. (Windows Live erkennt die Anmeldeinformationen nicht.)|Keine Dimensionen|
|outgoing.wns.badchannel|WNS-Fehler durch fehlerhaften Kanal|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der ChannelURI in der Registrierung nicht erkannt wurde (WNS-Status: 404 – Nicht gefunden).|Keine Dimensionen|
|outgoing.wns.expiredchannel|WNS-Fehler durch abgelaufenen Kanal|Count|Gesamt|Die Anzahl von nicht erfolgreichen Pushvorgängen, weil der ChannelURI abgelaufen ist (WNS-Status: 410 – Fehlend).|Keine Dimensionen|
|outgoing.wns.throttled|Gedrosselte WNS-Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App vom WNS gedrosselt wird (WNS-Status: 406 – Nicht annehmbar).|Keine Dimensionen|
|outgoing.wns.tokenproviderunreachable|WNS-Autorisierungsfehler (nicht erreichbar)|Count|Gesamt|Windows Live ist nicht erreichbar.|Keine Dimensionen|
|outgoing.wns.invalidtoken|WNS-Autorisierungsfehler (ungültiges Token)|Count|Gesamt|Das für WNS bereitgestellte Token ist nicht gültig (WNS-Status: 401 – Nicht autorisiert).|Keine Dimensionen|
|outgoing.wns.wrongtoken|WNS-Autorisierungsfehler (falsches Token)|Count|Gesamt|Das für WNS bereitgestellte Token ist gültig, gilt aber für eine andere Anwendung (WNS-Status: 403 – Verboten). Dieser Fall kann eintreten, wenn der ChannelURI in der Registrierung einer anderen App zugeordnet wird. Stellen Sie sicher, dass die Client-App der App zugeordnet ist, deren Anmeldeinformationen sich im Notification Hub befinden.|Keine Dimensionen|
|outgoing.wns.invalidnotificationformat|Ungültiges WNS-Benachrichtigungsformat|Count|Gesamt|Das Format der Benachrichtigung ist ungültig (WNS-Status: 400). Beachten Sie, dass von WNS nicht alle ungültigen Nutzlasten abgelehnt werden.|Keine Dimensionen|
|outgoing.wns.invalidnotificationsize|WNS-Fehler durch ungültige Benachrichtigungsgröße|Count|Gesamt|Die Benachrichtigungsnutzlast ist zu groß (WNS-Status: 413).|Keine Dimensionen|
|outgoing.wns.channelthrottled|WNS-Kanal gedrosselt|Count|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde (WNS-Antwortheader: X-WNS-NotificationStatus:channelThrottled).|Keine Dimensionen|
|outgoing.wns.channeldisconnected|WNS-Kanal nicht verbunden|Count|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde (WNS-Antwortheader: X-WNS-DeviceConnectionStatus: disconnected).|Keine Dimensionen|
|outgoing.wns.dropped|Verworfene WNS-Benachrichtigungen|Count|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde („X-WNS-NotificationStatus: dropped“, aber nicht „X-WNS-DeviceConnectionStatus: disconnected“).|Keine Dimensionen|
|outgoing.wns.pnserror|WNS-Fehler|Count|Gesamt|Die Benachrichtigung wurde aufgrund von Fehlern bei der Kommunikation mit WNS nicht übermittelt.|Keine Dimensionen|
|outgoing.wns.authenticationerror|WNS-Authentifizierungsfehler|Count|Gesamt|Die Benachrichtigung wurde nicht übermittelt, weil Fehler bei der Kommunikation mit Windows Live aufgetreten sind, die Anmeldeinformationen ungültig waren oder das falsche Token verwendet wurde.|Keine Dimensionen|
|outgoing.apns.success|Erfolgreiche APNS-Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.apns.invalidcredentials|APNS-Autorisierungsfehler|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine Dimensionen|
|outgoing.apns.badchannel|APNS-Fehler durch fehlerhaften Kanal|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das Token ungültig ist (APNS-Statuscode: 8).|Keine Dimensionen|
|outgoing.apns.expiredchannel|APNS-Fehler durch abgelaufenen Kanal|Count|Gesamt|Die Anzahl von Token, die aufgrund des APNS-Feedbackkanals ungültig gemacht wurden.|Keine Dimensionen|
|outgoing.apns.invalidnotificationsize|APNS-Fehler durch ungültige Benachrichtigungsgröße|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast zu groß war (APNS-Statuscode: 7).|Keine Dimensionen|
|outgoing.apns.pnserror|APNS-Fehler|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit APNS aufgetreten sind.|Keine Dimensionen|
|outgoing.gcm.success|Erfolgreiche GCM-Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.gcm.invalidcredentials|GCM-Autorisierungsfehler (ungültige Anmeldeinformationen)|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine Dimensionen|
|outgoing.gcm.badchannel|GCM-Fehler durch fehlerhaften Kanal|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung nicht erkannt wurde (GCM-Ergebnis: Ungültige Registrierung).|Keine Dimensionen|
|outgoing.gcm.expiredchannel|GCM-Fehler durch abgelaufenen Kanal|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung abgelaufen war (GCM-Ergebnis: NotRegistered).|Keine Dimensionen|
|outgoing.gcm.throttled|Gedrosselte GCM-Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App von GCM gedrosselt wurde (GCM-Statuscode: 501 - 599 oder result:Unavailable).|Keine Dimensionen|
|outgoing.gcm.invalidnotificationformat|Ungültiges GCM-Benachrichtigungsformat|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast nicht richtig formatiert war (GCM-Ergebnis: InvalidDataKey oder InvalidTtl).|Keine Dimensionen|
|outgoing.gcm.invalidnotificationsize|GCM-Fehler durch ungültige Benachrichtigungsgröße|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast zu groß war (GCM-Ergebnis: MessageTooBig).|Keine Dimensionen|
|outgoing.gcm.wrongchannel|GCM-Fehler durch falschen Kanal|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung nicht der aktuellen App zugeordnet war (GCM-Ergebnis: InvalidPackageName).|Keine Dimensionen|
|outgoing.gcm.pnserror|GCM-Fehler|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit GCM aufgetreten sind.|Keine Dimensionen|
|outgoing.gcm.authenticationerror|GCM-Authentifizierungsfehler|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die angegebenen Anmeldeinformationen vom PNS nicht akzeptiert wurden, die Anmeldeinformationen blockiert wurden oder die SenderId in der App nicht richtig konfiguriert wurde (GCM-Ergebnis: MismatchedSenderId).|Keine Dimensionen|
|outgoing.mpns.success|Erfolgreiche MPNS-Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|Keine Dimensionen|
|outgoing.mpns.invalidcredentials|Ungültige MPNS-Anmeldeinformationen|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine Dimensionen|
|outgoing.mpns.badchannel|MPNS-Fehler durch fehlerhaften Kanal|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der ChannelURI in der Registrierung nicht erkannt wurde (MPNS-Status: 404 – Nicht gefunden).|Keine Dimensionen|
|outgoing.mpns.throttled|Gedrosselte MPNS-Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App vom MPNS gedrosselt wird (WNS-MPNS: 406 – Nicht annehmbar).|Keine Dimensionen|
|outgoing.mpns.invalidnotificationformat|Ungültiges MPNS-Benachrichtigungsformat|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast der Benachrichtigung zu groß war.|Keine Dimensionen|
|outgoing.mpns.channeldisconnected|MPNS-Kanal nicht verbunden|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Verbindung für den ChannelURI in der Registrierung getrennt wurde (MPNS-Status: 412 – Nicht gefunden).|Keine Dimensionen|
|outgoing.mpns.dropped|Verworfene MPNS-Benachrichtigungen|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die von MPNS verworfen wurden (MPNS-Antwortheader: X-NotificationStatus: QueueFull or Suppressed).|Keine Dimensionen|
|outgoing.mpns.pnserror|MPNS-Fehler|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit MPNS aufgetreten sind.|Keine Dimensionen|
|outgoing.mpns.authenticationerror|MPNS-Authentifizierungsfehler|Count|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|Keine Dimensionen|
|notificationhub.pushes|Alle ausgehenden Benachrichtigungen|Count|Gesamt|Alle ausgehenden Benachrichtigungen des Benachrichtigungs-Hub|Keine Dimensionen|
|incoming.all.requests|Alle eingehenden Anforderungen|Count|Gesamt|Gesamtzahl eingehender Anforderungen für einen Notification Hub|Keine Dimensionen|
|incoming.all.failedrequests|Alle eingehenden fehlerhaften Anforderungen|Count|Gesamt|Gesamtzahl eingehender fehlerhafter Anforderungen für einen Notification Hub|Keine Dimensionen|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|QueryDuration|Abfragedauer|Millisekunden|Durchschnitt|DAX-Abfragedauer im letzten Intervall|Keine Dimensionen|
|QueryPoolJobQueueLength|Threads: Auftragswarteschlangenlänge für Abfragepool|Count|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Abfragethreadpools.|Keine Dimensionen|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces
(Öffentliche Vorschau)

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|Average_% Free Inodes|Average_% Free Inodes|Count|Durchschnitt|Average_% Free Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|Average_% Free Space|Count|Durchschnitt|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Inodes|Average_% Used Inodes|Count|Durchschnitt|Average_% Used Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Space|Average_% Used Space|Count|Durchschnitt|Average_% Used Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Read Bytes/sec|Average_Disk Read Bytes/sec|Count|Durchschnitt|Average_Disk Read Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Average_Disk Reads/sec|Count|Durchschnitt|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Average_Disk Transfers/sec|Count|Durchschnitt|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Write Bytes/sec|Average_Disk Write Bytes/sec|Count|Durchschnitt|Average_Disk Write Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Average_Disk Writes/sec|Count|Durchschnitt|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Average_Free Megabytes|Count|Durchschnitt|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Logical Disk Bytes/sec|Average_Logical Disk Bytes/sec|Count|Durchschnitt|Average_Logical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Memory|Average_% Available Memory|Count|Durchschnitt|Average_% Available Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Swap Space|Average_% Available Swap Space|Count|Durchschnitt|Average_% Available Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Memory|Average_% Used Memory|Count|Durchschnitt|Average_% Used Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Swap Space|Average_% Used Swap Space|Count|Durchschnitt|Average_% Used Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Memory|Average_Available MBytes Memory|Count|Durchschnitt|Average_Available MBytes Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Swap|Average_Available MBytes Swap|Count|Durchschnitt|Average_Available MBytes Swap|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Reads/sec|Average_Page Reads/sec|Count|Durchschnitt|Average_Page Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Writes/sec|Average_Page Writes/sec|Count|Durchschnitt|Average_Page Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pages/sec|Average_Pages/sec|Count|Durchschnitt|Average_Pages/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used MBytes Swap Space|Average_Used MBytes Swap Space|Count|Durchschnitt|Average_Used MBytes Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory MBytes|Average_Used Memory MBytes|Count|Durchschnitt|Average_Used Memory MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Transmitted|Average_Total Bytes Transmitted|Count|Durchschnitt|Average_Total Bytes Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Received|Average_Total Bytes Received|Count|Durchschnitt|Average_Total Bytes Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes|Average_Total Bytes|Count|Durchschnitt|Average_Total Bytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Transmitted|Average_Total Packets Transmitted|Count|Durchschnitt|Average_Total Packets Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Received|Average_Total Packets Received|Count|Durchschnitt|Average_Total Packets Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx Errors|Average_Total Rx Errors|Count|Durchschnitt|Average_Total Rx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Tx Errors|Average_Total Tx Errors|Count|Durchschnitt|Average_Total Tx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Collisions|Average_Total Collisions|Count|Durchschnitt|Average_Total Collisions|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Datenträger s/gelesen|Average_Avg. Datenträger s/gelesen|Count|Durchschnitt|Average_Avg. Datenträger s/gelesen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Datenträger s/übertragen|Average_Avg. Datenträger s/übertragen|Count|Durchschnitt|Average_Avg. Datenträger s/übertragen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Datenträger s/geschrieben|Average_Avg. Datenträger s/geschrieben|Count|Durchschnitt|Average_Avg. Datenträger s/geschrieben|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Physical Disk Bytes/sec|Average_Physical Disk Bytes/sec|Count|Durchschnitt|Average_Physical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct Privileged Time|Average_Pct Privileged Time|Count|Durchschnitt|Average_Pct Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct User Time|Average_Pct User Time|Count|Durchschnitt|Average_Pct User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory kBytes|Average_Used Memory kBytes|Count|Durchschnitt|Average_Used Memory kBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Virtual Shared Memory|Average_Virtual Shared Memory|Count|Durchschnitt|Average_Virtual Shared Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% DPC Time|Average_% DPC Time|Count|Durchschnitt|Average_% DPC Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Idle Time|Average_% Idle Time|Count|Durchschnitt|Average_% Idle Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Interrupt Time|Average_% Interrupt Time|Count|Durchschnitt|Average_% Interrupt Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% IO Wait Time|Average_% IO Wait Time|Count|Durchschnitt|Average_% IO Wait Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Nice Time|Average_% Nice Time|Count|Durchschnitt|Average_% Nice Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Privileged Time|Average_% Privileged Time|Count|Durchschnitt|Average_% Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|Average_% Processor Time|Count|Durchschnitt|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% User Time|Average_% User Time|Count|Durchschnitt|Average_% User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Physical Memory|Average_Free Physical Memory|Count|Durchschnitt|Average_Free Physical Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Space in Paging Files|Average_Free Space in Paging Files|Count|Durchschnitt|Average_Free Space in Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Virtual Memory|Average_Free Virtual Memory|Count|Durchschnitt|Average_Free Virtual Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processes|Average_Processes|Count|Durchschnitt|Average_Processes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Size Stored In Paging Files|Average_Size Stored In Paging Files|Count|Durchschnitt|Average_Size Stored In Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Uptime|Average_Uptime|Count|Durchschnitt|Average_Uptime|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Users|Average_Users|Count|Durchschnitt|Average_Users|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Datenträger s/gelesen|Average_Avg. Datenträger s/gelesen|Count|Durchschnitt|Average_Avg. Datenträger s/gelesen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Datenträger s/geschrieben|Average_Avg. Datenträger s/geschrieben|Count|Durchschnitt|Average_Avg. Datenträger s/geschrieben|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Current Disk Queue Length|Average_Current Disk Queue Length|Count|Durchschnitt|Average_Current Disk Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Average_Disk Reads/sec|Count|Durchschnitt|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Average_Disk Transfers/sec|Count|Durchschnitt|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Average_Disk Writes/sec|Count|Durchschnitt|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Average_Free Megabytes|Count|Durchschnitt|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|Average_% Free Space|Count|Durchschnitt|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes|Average_Available MBytes|Count|Durchschnitt|Average_Available MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Committed Bytes In Use|Average_% Committed Bytes In Use|Count|Durchschnitt|Average_% Committed Bytes In Use|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Received/sec|Average_Bytes Received/sec|Count|Durchschnitt|Average_Bytes Received/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Sent/sec|Average_Bytes Sent/sec|Count|Durchschnitt|Average_Bytes Sent/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Total/sec|Average_Bytes Total/sec|Count|Durchschnitt|Average_Bytes Total/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|Average_% Processor Time|Count|Durchschnitt|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processor Queue Length|Average_Processor Queue Length|Count|Durchschnitt|Average_Processor Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Heartbeat|Heartbeat|Count|Durchschnitt|Heartbeat|Computer, OSType, Version, SourceComputerId|
|Aktualisieren|Aktualisieren|Count|Durchschnitt|Aktualisieren|Computer, Product, Classification, UpdateState, Optional, Approved|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Count|Gesamt|Erfolgreiche ListenerConnections für Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Count|Gesamt|ClientError bei ListenerConnections für Microsoft.Relay.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Count|Gesamt|ServerError bei ListenerConnections für Microsoft.Relay.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Count|Gesamt|Erfolgreiche SenderConnections für Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Count|Gesamt|ClientError bei SenderConnections für Microsoft.Relay.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Count|Gesamt|ServerError bei SenderConnections für Microsoft.Relay.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Count|Gesamt|Gesamtzahl der ListenerConnections für Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Count|Gesamt|Gesamtzahl der SenderConnections-Anforderungen für Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Count|Gesamt|Gesamtzahl der ActiveConnections für Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Count|Gesamt|Gesamtzahl der ActiveListeners für Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Count|Gesamt|Gesamtzahl der BytesTransferred für Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Count|Gesamt|Gesamtzahl der ListenerDisconnects für Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Count|Gesamt|Gesamtzahl der SenderDisconnects für Microsoft.Relay.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|SearchLatency|Suchlatenz|Sekunden|Durchschnitt|Durchschnittliche Suchlatenz für den Suchdienst|Keine Dimensionen|
|SearchQueriesPerSecond|Suchabfragen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Suchabfragen pro Sekunde für den Suchdienst|Keine Dimensionen|
|ThrottledSearchQueriesPercentage|Gedrosselte Suchabfragen in Prozent|Prozent|Durchschnitt|Prozentsatz der Suchabfragen, die für den Suchdienst gedrosselt wurden|Keine Dimensionen|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|SuccessfulRequests|Erfolgreiche Anforderungen (Vorschau)|Count|Gesamt|Gesamtzahl der erfolgreichen Anforderungen für einen Namespace (Vorschau)|EntityName, |
|ServerErrors|Serverfehler. (Vorschau)|Count|Gesamt|Serverfehler für Microsoft.ServiceBus. (Vorschau)|EntityName, |
|UserErrors|Benutzerfehler. (Vorschau)|Count|Gesamt|Benutzerfehler für Microsoft.ServiceBus. (Vorschau)|EntityName, |
|ThrottledRequests|Gedrosselte Anforderungen. (Vorschau)|Count|Gesamt|Gedrosselte Anforderungen für Microsoft.ServiceBus. (Vorschau)|EntityName, |
|IncomingRequests|Eingehende Anforderungen (Vorschau)|Count|Gesamt|Eingehende Anforderungen für Microsoft.ServiceBus. (Vorschau)|EntityName|
|IncomingMessages|Eingehende Nachrichten (Vorschau)|Count|Gesamt|Eingehende Nachrichten für Microsoft.ServiceBus. (Vorschau)|EntityName|
|OutgoingMessages|Ausgehende Nachrichten (Vorschau)|Count|Gesamt|Ausgehende Nachrichten für Microsoft.ServiceBus. (Vorschau)|EntityName|
|ActiveConnections|Aktive Verbindungen (Vorschau)|Count|Gesamt|Aktive Verbindungen gesamt für Microsoft.ServiceBus. (Vorschau)|Keine Dimensionen|
|Größe|Größe (Vorschau)|Byte|Durchschnitt|Größe einer Warteschlange/eines Themas in Bytes (Vorschau)|EntityName|
|Meldungen|Anzahl von Nachrichten in einer Warteschlange/einem Thema (Vorschau)|Count|Durchschnitt|Anzahl von Nachrichten in einer Warteschlange/einem Thema (Vorschau)|EntityName|
|ActiveMessages|Anzahl von aktiven Nachrichten in einer Warteschlange/einem Thema (Vorschau)|Count|Durchschnitt|Anzahl von aktiven Nachrichten in einer Warteschlange/einem Thema (Vorschau)|EntityName|
|CPUXNS|CPU-Auslastung pro Namespace|Prozent|Maximum|CPU-Auslastungsmetrik für Service Bus-Premium-Namespace|Keine Dimensionen|
|WSXNS|Auslastung der Arbeitsspeichergröße pro Namespace|Prozent|Maximum|Speicherauslastungsmetrik für Service Bus-Premium-Namespace|Keine Dimensionen|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Keine Dimensionen|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|Keine Dimensionen|
|log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|Keine Dimensionen|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|Keine Dimensionen|
|storage|Datenbankgröße gesamt|Byte|Maximum|Datenbankgröße gesamt|Keine Dimensionen|
|connection_successful|Erfolgreiche Verbindungen|Count|Gesamt|Erfolgreiche Verbindungen|Keine Dimensionen|
|connection_failed|Verbindungsfehler|Count|Gesamt|Verbindungsfehler|Keine Dimensionen|
|blocked_by_firewall|Von der Firewall blockiert|Count|Gesamt|Von der Firewall blockiert|Keine Dimensionen|
|deadlock|Deadlocks|Count|Gesamt|Deadlocks|Keine Dimensionen|
|storage_percent|Datenbankgröße als Prozentsatz|Prozent|Maximum|Datenbankgröße als Prozentsatz|Keine Dimensionen|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent|Prozent|Durchschnitt|In-Memory-OLTP-Speicher in Prozent|Keine Dimensionen|
|workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|Keine Dimensionen|
|sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|Keine Dimensionen|
|dtu_limit|DTU-Grenzwert|Count|Durchschnitt|DTU-Grenzwert|Keine Dimensionen|
|dtu_used|DTU-Verbrauch|Count|Durchschnitt|DTU-Verbrauch|Keine Dimensionen|
|dwu_limit|DWU-Grenzwert|Count|Maximum|DWU-Grenzwert|Keine Dimensionen|
|dwu_consumption_percent|DWU in Prozent|Prozent|Maximum|DWU in Prozent|Keine Dimensionen|
|dwu_used|DWU-Verbrauch|Count|Maximum|DWU-Verbrauch|Keine Dimensionen|
|dw_cpu_percent|DW-Knotenebene, CPU-Prozentsatz|Prozent|Durchschnitt|DW-Knotenebene, CPU-Prozentsatz|DwLogicalNodeId|
|dw_physical_data_read_percent|DW-Knotenebene, E/A-Prozentsatz für Daten|Prozent|Durchschnitt|DW-Knotenebene, E/A-Prozentsatz für Daten|DwLogicalNodeId|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Keine Dimensionen|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|Keine Dimensionen|
|log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|Keine Dimensionen|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|Keine Dimensionen|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|Keine Dimensionen|
|workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|Keine Dimensionen|
|sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|Keine Dimensionen|
|eDTU_limit|eDTU-Grenzwert|Count|Durchschnitt|eDTU-Grenzwert|Keine Dimensionen|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|Keine Dimensionen|
|eDTU_used|eDTU-Verbrauch|Count|Durchschnitt|eDTU-Verbrauch|Keine Dimensionen|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|Keine Dimensionen|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent|Prozent|Durchschnitt|In-Memory-OLTP-Speicher in Prozent|Keine Dimensionen|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|ElasticPoolResourceId|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|UsedCapacity|Verwendete Kapazität|Byte|Durchschnitt|Vom Konto verwendete Kapazität|Keine Dimensionen|
|Transaktionen|Transaktionen|Count|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Verfügbarkeit|Verfügbarkeit|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|BlobCapacity|Blob-Kapazität|Byte|Gesamt|Die Größe des vom Blob-Dienst des Speicherkontos genutzten Speichers in Byte.|BlobType|
|BlobCount|Anzahl von Blobs|Count|Gesamt|Die Anzahl von Blobs im Blob-Dienst des Speicherkontos.|BlobType|
|ContainerCount|Anzahl von Blob-Containern|Count|Durchschnitt|Die Anzahl von Containern im Blob-Dienst des Speicherkontos.|Keine Dimensionen|
|Transaktionen|Transaktionen|Count|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Verfügbarkeit|Verfügbarkeit|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|TableCapacity|Tabellenkapazität|Byte|Durchschnitt|Die Größe des vom Tabellendienst des Speicherkontos genutzten Speichers in Byte.|Keine Dimensionen|
|TableCount|Anzahl von Tabellen|Count|Durchschnitt|Die Anzahl von Tabellen im Tabellendienst des Speicherkontos|Keine Dimensionen|
|TableEntityCount|Anzahl von Tabellenentitäten|Count|Durchschnitt|Die Anzahl von Tabellenentitäten im Tabellendienst des Speicherkontos|Keine Dimensionen|
|Transaktionen|Transaktionen|Count|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Verfügbarkeit|Verfügbarkeit|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|QueueCapacity|Warteschlangenkapazität|Byte|Durchschnitt|Die Größe des vom Warteschlangendienst des Speicherkontos genutzten Speichers in Byte.|Keine Dimensionen|
|QueueCount|Anzahl von Warteschlangen|Count|Durchschnitt|Die Anzahl von Warteschlangen im Warteschlangendienst des Speicherkontos|Keine Dimensionen|
|QueueMessageCount|Anzahl von Warteschlangennachrichten|Count|Durchschnitt|Die ungefähre Anzahl von Warteschlangennachrichten im Warteschlangendienst des Speicherkontos|Keine Dimensionen|
|Transaktionen|Transaktionen|Count|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert enthält die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Bestätigung der Antwort.|GeoType, ApiName|
|Verfügbarkeit|Verfügbarkeit|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|FileCapacity|Dateikapazität|Byte|Durchschnitt|Die Größe des vom Dateidienst des Speicherkontos genutzten Speichers in Byte.|Keine Dimensionen|
|FileCount|Dateianzahl|Count|Durchschnitt|Die Anzahl von Dateien im Dateidienst des Speicherkontos|Keine Dimensionen|
|FileShareCount|Anzahl von Dateifreigaben|Count|Durchschnitt|Die Anzahl von Dateifreigaben im Dateidienst des Speicherkontos|Keine Dimensionen|
|Transaktionen|Transaktionen|Count|Gesamt|Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und fehlgeschlagene Anforderungen sowie Anforderungen, die Fehler erzeugt haben. Verwenden Sie die Dimension „ResponseType“ für die Anzahl von verschiedenen Antworttypen.|ResponseType, GeoType, ApiName|
|Eingehende Daten|Eingehende Daten|Byte|Gesamt|Die Menge der Eingangsdaten in Byte. Diese Anzahl umfasst die eingehenden Daten von einem externen Client in Azure Storage sowie die eingehenden Daten innerhalb von Azure.|GeoType, ApiName|
|Ausgehende Daten|Ausgehende Daten|Byte|Gesamt|Die Menge der Ausgangsdaten in Byte. Diese Anzahl umfasst die ausgehenden Daten von einem externen Client in Azure Storage sowie die ausgehenden Daten innerhalb von Azure. Diese Anzahl gibt daher keine gebührenpflichtigen ausgehenden Daten an.|GeoType, ApiName|
|SuccessServerLatency|Serverlatenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche Latenz in Millisekunden, die von Azure Storage verwendet wurde, um eine erfolgreiche Anforderung zu verarbeiten. Dieser Wert enthält nicht die in „AverageE2ELatency“ angegebene Netzwerklatenz.|GeoType, ApiName|
|SuccessE2ELatency|E2E-Latenz (erfolgreich)|Millisekunden|Durchschnitt|Die durchschnittliche End-to-End-Latenz für erfolgreiche Anforderungen in Millisekunden, die an einen Speicherdienst oder den angegebenen API-Vorgang gesendet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung.|GeoType, ApiName|
|Verfügbarkeit|Verfügbarkeit|Prozent|Durchschnitt|Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Die Verfügbarkeit wird berechnet, indem der Wert „TotalBillableRequests“ durch die Anzahl von zutreffenden Anforderungen, einschließlich der, die unerwartete Fehler erzeugt haben, geteilt wird. Alle unerwarteten Fehler führen zu einer reduzierten Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang.|GeoType, ApiName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|ResourceUtilization|Speichereinheitnutzung in %|Prozent|Maximum|Speichereinheitnutzung in %|Keine Dimensionen|
|InputEvents|Eingabeereignisse|Count|Gesamt|Eingabeereignisse|Keine Dimensionen|
|InputEventBytes|Eingabeereignisbytes|Byte|Gesamt|Eingabeereignisbytes|Keine Dimensionen|
|LateInputEvents|Ereignisse bei verspäteter Eingabe|Count|Gesamt|Ereignisse bei verspäteter Eingabe|Keine Dimensionen|
|OutputEvents|Ausgabeereignisse|Count|Gesamt|Ausgabeereignisse|Keine Dimensionen|
|ConversionErrors|Konvertierungsfehler|Count|Gesamt|Konvertierungsfehler|Keine Dimensionen|
|Errors|Laufzeitfehler|Count|Gesamt|Laufzeitfehler|Keine Dimensionen|
|DroppedOrAdjustedEvents|Ereignisse für falsche Reihenfolge|Count|Gesamt|Ereignisse für falsche Reihenfolge|Keine Dimensionen|
|AMLCalloutRequests|Funktionsanforderungen|Count|Gesamt|Funktionsanforderungen|Keine Dimensionen|
|AMLCalloutFailedRequests|Fehlerhafte Funktionsanforderungen|Count|Gesamt|Fehlerhafte Funktionsanforderungen|Keine Dimensionen|
|AMLCalloutInputEvents|Funktionsereignisse|Count|Gesamt|Funktionsereignisse|Keine Dimensionen|
|DeserializationError|Eingabefehler bei Deserialisierung|Count|Gesamt|Eingabefehler bei Deserialisierung|Keine Dimensionen|
|EarlyInputEvents|Ereignisse, deren Anwendungszeit vor dem Zeitpunkt ihres Eintreffens liegt.|Count|Gesamt|Ereignisse, deren Anwendungszeit vor dem Zeitpunkt ihres Eintreffens liegt.|Keine Dimensionen|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Instanz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Prozent|Durchschnitt|Arbeitsspeicherprozentsatz|Instanz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Count|Durchschnitt|Warteschlangenlänge des Datenträgers|Instanz|
|HttpQueueLength|Länge der HTTP-Warteschlange|Count|Durchschnitt|Länge der HTTP-Warteschlange|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (Funktionen ausgenommen)

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|Instanz|
|Requests|Requests|Count|Gesamt|Requests|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Count|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Count|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Count|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Count|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Count|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Count|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Count|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Count|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Count|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|Instanz|
|AppConnections|Verbindungen|Count|Durchschnitt|Verbindungen|Instanz|
|Handles|Anzahl von Handles|Count|Durchschnitt|Anzahl von Handles|Instanz|
|Threads|Threadanzahl|Count|Durchschnitt|Threadanzahl|Instanz|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (Funktionen)

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http5xx|HTTP-Serverfehler|Count|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Count|Gesamt|Ausführungseinheiten für Funktion|Instanz|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Count|Gesamt|Ausführungsanzahl für Funktion|Instanz|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|Instanz|
|Requests|Requests|Count|Gesamt|Requests|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Count|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Count|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Count|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Count|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Count|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Count|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Count|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Count|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Count|Gesamt|HTTP-Serverfehler|Instanz|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|Instanz|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Count|Gesamt|Ausführungseinheiten für Funktion|Instanz|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Count|Gesamt|Ausführungsanzahl für Funktion|Instanz|
|AppConnections|Verbindungen|Count|Durchschnitt|Verbindungen|Instanz|
|Handles|Anzahl von Handles|Count|Durchschnitt|Anzahl von Handles|Instanz|
|Threads|Threadanzahl|Count|Durchschnitt|Threadanzahl|Instanz|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|Requests|Requests|Count|Gesamt|Requests|Instanz|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|Instanz|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|Instanz|
|Http101|HTTP 101|Count|Gesamt|HTTP 101|Instanz|
|Http2xx|HTTP 2xx|Count|Gesamt|HTTP 2xx|Instanz|
|Http3xx|HTTP 3xx|Count|Gesamt|HTTP 3xx|Instanz|
|Http401|HTTP 401|Count|Gesamt|HTTP 401|Instanz|
|Http403|HTTP 403|Count|Gesamt|HTTP 403|Instanz|
|Http404|HTTP 404|Count|Gesamt|HTTP 404|Instanz|
|Http406|HTTP 406|Count|Gesamt|HTTP 406|Instanz|
|Http4xx|HTTP 4xx|Count|Gesamt|HTTP 4xx|Instanz|
|Http5xx|HTTP-Serverfehler|Count|Gesamt|HTTP-Serverfehler|Instanz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|Instanz|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Instanz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Prozent|Durchschnitt|Arbeitsspeicherprozentsatz|Instanz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Count|Durchschnitt|Warteschlangenlänge des Datenträgers|Instanz|
|HttpQueueLength|Länge der HTTP-Warteschlange|Count|Durchschnitt|Länge der HTTP-Warteschlange|Instanz|
|ActiveRequests|Aktive Anforderungen|Count|Gesamt|Aktive Anforderungen|Instanz|
|TotalFrontEnds|Gesamtanzahl an Front-Ends|Count|Durchschnitt|Gesamtanzahl an Front-Ends|Keine Dimensionen|
|SmallAppServicePlanInstances|Kleine Worker im App Service-Plan|Count|Durchschnitt|Kleine Worker im App Service-Plan|Keine Dimensionen|
|MediumAppServicePlanInstances|Mittlere Worker im App Service-Plan|Count|Durchschnitt|Mittlere Worker im App Service-Plan|Keine Dimensionen|
|LargeAppServicePlanInstances|Große Worker im App Service-Plan|Count|Durchschnitt|Große Worker im App Service-Plan|Keine Dimensionen|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Metrik|Metrikanzeigename|Unit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|WorkersTotal|Gesamtanzahl an Workern|Count|Durchschnitt|Gesamtanzahl an Workern|Keine Dimensionen|
|WorkersAvailable|Verfügbare Worker|Count|Durchschnitt|Verfügbare Worker|Keine Dimensionen|
|WorkersUsed|Verwendete Worker|Count|Durchschnitt|Verwendete Worker|Keine Dimensionen|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|Instanz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Prozent|Durchschnitt|Arbeitsspeicherprozentsatz|Instanz|

## <a name="next-steps"></a>Nächste Schritte
* [Informationen zu Metriken in Azure Monitor](monitoring-overview-metrics.md)
* [Erstellen von Warnungen zu Metriken](insights-receive-alert-notifications.md)
* [Exportieren von Metriken in Storage, Event Hub oder Log Analytics](monitoring-overview-of-diagnostic-logs.md)
