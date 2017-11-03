---
title: "Überwachen von Servermetriken in Azure Analysis Services | Microsoft-Dokumentation"
description: "Informationen zum Überwachen von Analysis Services-Servermetriken im Azure-Portal."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/14/2017
ms.author: owend
ms.openlocfilehash: f9b32029f0a7065fff73ddb6417fc5c1c7e658a5
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="monitor-server-metrics"></a>Überwachen von Servermetriken

Analysis Services bietet Metriken zum Überwachen von Leistung und Integrität Ihres Servers. Überwachen Sie z.B. Arbeitsspeicher- und CPU-Nutzung, Anzahl der Clientverbindungen und Ressourcenverbrauch für Abfragen. Analysis Services verwendet das gleiche Überwachungsframework wie die meisten anderen Azure-Dienste. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Um eine ausführlichere Diagnose auszuführen, Leistung nachzuverfolgen und mehrere Dienstressourcen in einer Ressourcengruppe oder einem Abonnement übergreifende Trends zu identifizieren, verwenden Sie [Azure Monitor](https://azure.microsoft.com/services/monitor/). Die Leistungen des Diensts Azure Monitor werden möglicherweise in Rechnung gestellt.


## <a name="to-monitor-metrics-for-an-analysis-services-server"></a>So überwachen Sie Metriken für einen Analysis Services-Server

1. Wählen Sie im Azure-Portal die Option **Metriken** aus.

    ![Überwachen im Azure-Portal](./media/analysis-services-monitor/aas-monitor-portal.png)

2. Wählen Sie in **Verfügbare Metriken** die Metriken aus, die Sie in Ihr Diagramm aufnehmen möchten. 

    ![Überwachungsdiagramm](./media/analysis-services-monitor/aas-monitor-chart.png)

## <a name="server-metrics"></a>Servermetriken
Bestimmen Sie mit dieser Tabelle, welche Metriken für Ihr Überwachungsszenario am besten geeignet sind. Nur Metriken derselben Einheit können im selben Diagramm angezeigt werden.

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CommandPoolJobQueueLength|Warteschlangenlänge für Aufträge im Befehlspool|Anzahl|Durchschnitt|Gibt die Anzahl von Aufträgen in der Warteschlange des Befehlsthreadpools an.|
|CurrentConnections|Verbindung: Aktuelle Verbindungen|Anzahl|Durchschnitt|Aktuelle Anzahl hergestellter Clientverbindungen.|
|CurrentUserSessions|Aktuelle Benutzersitzungen|Anzahl|Durchschnitt|Aktuelle Anzahl von eingerichteten Benutzersitzungen|
|mashup_engine_memory_metric|M-Modul – Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeichernutzung durch Mashup-Modulprozesse|
|mashup_engine_qpu_metric|M-Modul – QPU|Anzahl|Durchschnitt|QPU-Nutzung durch Mashup-Modulprozesse|
|memory_metric|Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeicher. Bereich: 0–25 GB für S1, 0–50 GB für S2 und 0–100 GB für S4|
|memory_thrashing_metric|Arbeitsspeicherüberlastung|Prozent|Durchschnitt|Durchschnittliche Arbeitsspeicherüberlastung.|
|CleanerCurrentPrice|Arbeitsspeicher: Bereinigung – aktueller Preis|Anzahl|Durchschnitt|Aktueller Preis des Arbeitsspeichers, $/Byte/Zeit, normalisiert auf 1000.|
|CleanerMemoryNonshrinkable|Arbeitsspeicher: Bereinigung – nicht verkleinerbarer Arbeitsspeicher|Byte|Durchschnitt|Die Menge des Arbeitsspeichers in Byte, die nicht durch den Hintergrundbereinigungsprozess bereinigt wird.|
|CleanerMemoryShrinkable|Arbeitsspeicher: Bereinigung – verkleinerbarer Arbeitsspeicher|Byte|Durchschnitt|Die Menge des Arbeitsspeichers in Byte, die durch den Hintergrundbereinigungsprozess bereinigt wird.|
|MemoryLimitHard|Arbeitsspeicher: Grenzwert für den festen Speicher|Byte|Durchschnitt|Grenzwert für den festen Speicher gemäß Konfigurationsdatei.|
|MemoryLimitHigh|Arbeitsspeicher: Obere Arbeitsspeichergrenze|Byte|Durchschnitt|Oberer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|
|MemoryLimitLow|Arbeitsspeicher: Untere Arbeitsspeichergrenze|Byte|Durchschnitt|Unterer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|
|MemoryLimitVertiPaq|Arbeitsspeicher: VertiPaq-Arbeitsspeichergrenze|Byte|Durchschnitt|In-Memory-Grenzwert gemäß Konfigurationsdatei.|
|MemoryUsage|Arbeitsspeicher: Speicherauslastung|Byte|Durchschnitt|Speicherauslastung des Serverprozesses, wie bei der Berechnung des Arbeitsspeicherpreises für die Bereinigung verwendet. Entspricht dem Indikator „Process\PrivateBytes“ zuzüglich der Größe der im Speicher abgebildeten Daten. Vom In-Memory-Analysemodul (VertiPaq) abgebildeter oder belegter Arbeitsspeicher, der über die Arbeitsspeichergrenze des Moduls hinausgeht, wird dabei ignoriert.|
|Kontingent|Arbeitsspeicher: Kontingent|Byte|Durchschnitt|Aktuelles Arbeitsspeicherkontingent in Byte. Das Arbeitsspeicherkontingent wird auch als Speicherzuweisung oder Speicherreservierung bezeichnet.|
|QuotaBlocked|Arbeitsspeicher: Kontingent blockiert|Anzahl|Durchschnitt|Aktuelle Anzahl von Kontingentanforderungen, die blockiert werden, bis andere Arbeitsspeicherkontingente freigegeben werden.|
|VertiPaqNonpaged|Arbeitsspeicher: Nicht ausgelagerte VertiPaq-Daten|Byte|Durchschnitt|Bytes von Arbeitsspeicher, die im Arbeitssatz zur Verwendung durch das In-Memory-Modul gesperrt sind.|
|VertiPaqPaged|Arbeitsspeicher: Ausgelagerte VertiPaq-Daten|Byte|Durchschnitt|Bytes von ausgelagertem Arbeitsspeicher, die für In-Memory-Daten verwendet werden.|
|ProcessingPoolJobQueueLength|Warteschlangenlänge für Verarbeitungspoolaufträge|Anzahl|Durchschnitt|Anzahl von Nicht-E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools|
|RowsConvertedPerSec|Verarbeitung: Konvertierte Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der Zeilen, die bei der Verarbeitung konvertiert werden.|
|RowsReadPerSec|Verarbeitung: Gelesene Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der aus allen relationalen Datenbanken gelesenen Zeilen.|
|RowsWrittenPerSec|Verarbeitung: Geschriebene Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der Zeilen, die bei der Verarbeitung geschrieben werden.|
|qpu_metric|QPU|Anzahl|Durchschnitt|QPU. Bereich: 0–100 für S1, 0–200 für S2 und 0–400 für S4|
|QueryPoolBusyThreads|Ausgelastete Abfragepoolthreads|Anzahl|Durchschnitt|Anzahl von ausgelasteten Threads im Abfragethreadpool|
|SuccessfullConnectionsPerSec|Erfolgreiche Verbindungen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der erfolgreichen Verbindungsabschlüsse|
|CommandPoolBusyThreads|Threads: Ausgelastete Threads im Befehlspool|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Befehlsthreadpool.|
|CommandPoolIdleThreads|Threads: Leerlaufthreads im Befehlspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Befehlsthreadpool.|
|LongParsingBusyThreads|Threads: Ausgelastete lange Analysethreads|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Pool für lange Analysethreads.|
|LongParsingIdleThreads|Threads: Lange Analysethreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Pool für lange Analysethreads.|
|LongParsingJobQueueLength|Threads: Warteschlangenlänge für lange Analyseaufträge|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Pools für lange Analysethreads.|
|ProcessingPoolIOJobQueueLength|Threads: Warteschlangenlänge für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools.|
|ProcessingPoolBusyIOJobThreads|Threads: Ausgelastete Threads für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Threads, die E/A-Aufträge im Verarbeitungsthreadpool ausführen.|
|ProcessingPoolBusyNonIOThreads|Threads: Ausgelastete Nicht-E/A-Threads im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Threads, die Nicht-E/A-Aufträge im Verarbeitungsthreadpool ausführen.|
|ProcessingPoolIdleIOJobThreads|Threads: Leerlaufthreads für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|
|ProcessingPoolIdleNonIOThreads|Threads: Nicht-E/A-Leerlaufthreads im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Verarbeitungsthreadpool, die für Nicht-E/A-Aufträge vorgesehen sind.|
|QueryPoolIdleThreads|Threads: Abfragepoolthreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|
|QueryPoolJobQueueLength|Threads: Auftragswarteschlangenlänge für Abfragepool|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Abfragethreadpools.|
|ShortParsingBusyThreads|Threads: Ausgelastete kurze Analysethreads|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Pool für kurze Analysethreads.|
|ShortParsingIdleThreads|Threads: Kurze Analysethreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Pool für kurze Analysethreads.|
|ShortParsingJobQueueLength|Threads: Warteschlangenlänge für kurze Analyseaufträge|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Pools für kurze Analysethreads.|
|TotalConnectionFailures|Verbindungsfehler gesamt|Anzahl|Durchschnitt|Gesamtanzahl von fehlerhaften Verbindungsversuchen|
|TotalConnectionRequests|Total Connection Requests (Verbindungsanforderungen gesamt)|Anzahl|Durchschnitt|Gesamtanzahl von Verbindungsanforderungen. |

## <a name="next-steps"></a>Nächste Schritte
[Überwachung in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md)   
[Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)   
[Metriken](https://msdn.microsoft.com/library/azure/dn931930.aspx)