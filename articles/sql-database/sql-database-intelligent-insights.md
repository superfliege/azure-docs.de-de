---
title: "Überwachen der Datenbanknutzung mit Intelligent Insights – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Die in Intelligent Insights von Azure SQL-Datenbank integrierte Logik überwacht kontinuierlich die Datenbanknutzung durch künstliche Intelligenz und ermittelt Störungen, die zu schlechter Leistung führen."
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
ms.openlocfilehash: 8e55fa02d5edda21fbf547397d63f9d1cf987a60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="intelligent-insights"></a>Intelligent Insights

***&amp;#8220;Intelligent Insights informiert Sie über die Vorgänge Ihrer Datenbankleistung.&amp;#8221;***

Die in Intelligent Insights von Azure SQL-Datenbank integrierte Logik überwacht kontinuierlich die Datenbanknutzung durch künstliche Intelligenz und ermittelt Störungen, die zu schlechter Leistung führen. Sobald diese ermittelt wurden, wird eine detaillierte Analyse ausgeführt, die ein Diagnoseprotokoll mit einer intelligenten Bewertung des Problems generiert. Die Bewertung besteht aus einer Analyse der Grundursache des Leistungsproblems der Datenbank und nach Möglichkeit Empfehlungen für Leistungsverbesserungen – dazu zählt ***&#8220;Intelligent Insights&#8221;***. 

## <a name="what-can-intelligent-insights-do-for-you"></a>Was kann Intelligent Insights für Sie tun?

Bei Intelligent Insights handelt es sich um eine einzigartige Funktion der in Azure integrierten Logik, die folgende Werte bereitstellt:

- Proaktive Überwachung
- Angepasste Einblicke in die Leistung
- Frühe Erkennung von Leistungsminderungen der Datenbank
- Analyse der Grundursache (Root Cause Analysis, RCA) der erkannten Probleme
- Empfehlungen für Leistungsverbesserungen
- Funktion für das horizontale Hochskalieren von einigen Hunderttausend Datenbanken
- Positive Auswirkung auf DevOps-Ressourcen und die Gesamtkosten

## <a name="how-does-intelligent-insights-work"></a>Wie funktioniert Intelligent Insights?

Intelligent Insights analysiert die Leistung von Azure SQL-Datenbank, indem die Datenbankworkload der letzten Stunde mit der Baselineworkload der letzten sieben Tage verglichen wird. Die Datenbankworkload besteht aus den Abfragen, die für die Datenbankleistung am wichtigsten sind, dazu gehören z.B. die am meisten wiederholten und die größten Abfragen. Jede Datenbank ist eindeutig durch ihre Struktur, ihre Daten, die Nutzung und die Anwendung. Durch diese ist jede generierte Workloadbaseline spezifisch und eindeutig für eine einzelne Instanz. Intelligent Insights überwacht unabhängig von der Workloadbaseline auch die absoluten Betriebsschwellenwerte und erkennt Probleme mit übermäßigen Wartezeiten, kritischen Ausnahmen und Probleme mit der Abfrageparametrisierung, die sich auf die Leistung auswirken können.

Sobald durch die künstliche Intelligenz ein Problem bei mehreren beobachteten Metriken erkannt wird, das die Leistung mindert, wird eine Analyse ausgeführt und ein Diagnoseprotokoll ausgegeben, das einen intelligenten Einblick in die Vorgänge Ihrer Datenbank enthält. Durch Intelligent Insights kann das Problem mit der Datenbankleistung vom ersten Auftreten bis hin zu dessen Behebung leicht nachverfolgt werden. Dafür werden die Statuswerte jedes ermittelten Problems während seines Lebenszyklus nachverfolgt, von der ersten Erkennung des Problems bis hin zur Überprüfung der Leistungssteigerung und dessen Behebung. Updates werden im Diagnoseprotokoll alle 15 Minuten bereitgestellt. 

<center>![Server](./media/sql-database-intelligent-insights/intelligent-insights-concept.png)</center>

Metriken, die zum Messen und Erkennen von Leistungsproblemen bei Datenbanken verwendet werden, basieren auf der Abfragedauer, Anforderungen mit Zeitüberschreitung, übermäßigen Wartezeiten und fehlerhaften Anforderungen. Diese werden im Abschnitt [Detection Metrics (Erkennungsmetriken)](sql-database-intelligent-insights.md#detection-metrics) in diesem Dokument weiter ausgearbeitet.

## <a name="degradations-detected"></a>Erkannte Leistungsminderungen

Identifizierte Leistungseinbußen von Azure SQL-Datenbank werden im Diagnoseprotokolle mit intelligenten Einträgen aufgezeichnet, die aus folgenden Eigenschaften bestehen:

| Eigenschaft             | Details              |
| :------------------- | ------------------- |
| Datenbankinformationen  | Metadaten zu einer Datenbank, auf der ein Einblick erkannt wurde, z.B. eine Ressourcen-URI |
| Beobachteter Zeitbereich | Start- und Endzeit für den Zeitraum der erkannten Einblicke |
| Betroffene Metriken | Metriken, die das Generieren eines Einblicks verursachen: <ul><li>Erhöhte Abfragedauer [Sekunden]</li><li>Übermäßige Wartezeiten [Sekunden]</li><li>Zeitüberschreitung bei Anforderungen [Sekunden]</li><li>Fehler bei Anforderungen [Sekunden]</li></ul>|
| Auswirkungswert | Gemessener Wert einer Metrik |
| Betroffene Abfragen und Fehlercodes | Abfragehash oder Fehlercode Diese können verwendet werden, um leicht mit betroffenen Abfragen zu korrelieren. Metriken werden bereitgestellt, die entweder aus erhöhten Abfragedauern, Wartezeiten, Zeitüberschreitungen oder Fehlercodes bestehen. |
| Erkennungen | Während eines Ereignisses wurde eine Erkennung auf der Datenbank identifiziert. Es gibt 15 Erkennungsmuster. Siehe [Troubleshoot Azure SQL Database performance issues with Intelligent Insights (Behandeln von Problemen mit der Leistung von Azure SQL-Datenbank mithilfe von Intelligent Insights)](sql-database-intelligent-insights-troubleshoot-performance.md). |
| Analyse der Grundursache | Die Analyse der Grundursache des Problems (Root Cause Analysis, RCA), die in einem durch Menschen lesbaren Format identifiziert wurde. Einige Einblicke enthalten nach Möglichkeit Empfehlungen zur Leistungsverbesserung. |
|||

## <a name="issues-state-lifecycle-active-verifying-and-completed"></a>Statuswerte im Lebenszyklus des Problems: „Aktiv“, „Wird überprüft“, „Abgeschlossen“

Leistungsprobleme, die im Diagnoseprotokoll aufgezeichnet werden, werden mit einem der drei Statuswerte im Lebenszyklus eines Problems gekennzeichnet: „Aktiv“, „Wird überprüft“, „Abgeschlossen“ Sobald ein Leistungsproblem erkannt und von der in Azure SQL integrierten Logik als vorhanden angesehen wird, wird das Problem als &#8220;***Aktiv***&#8221; gekennzeichnet. Sobald das Problem als gemindert angesehen wird, wird es überprüft und der Status des Problems wird in &#8220;***Wird überprüft***&#8221 geändert. Sobald die in Azure SQL integrierte Logik das Problem als gelöst ansieht, wird der Status des Problems als &#8220;***Abgeschlossen***&#8221 gekennzeichnet.

## <a name="using-intelligent-insights"></a>Verwenden von Intelligent Insights

Das Diagnoseprotokoll von Intelligent Insights kann an Azure Log Analytics, Azure Event Hub und Azure Storage gesendet werden. Dies wird unter [Protokollierung von Metriken und Diagnosen für Azure SQL-Datenbank](sql-database-metrics-diag-logging.md) beschrieben. Sobald das Protokoll an eines dieser Ziele gesendet wurde, kann es für die Entwicklung von benutzerdefinierten Warnungen und Überwachungen mithilfe von Microsoft- oder Drittanbietertools verwendet werden. Weitere Informationen zum Behandeln von Problemen mit der Leistung von Azure SQL-Datenbank mithilfe von Intelligent Insights finden Sie unter [Troubleshoot Azure SQL Database performance issues with Intelligent Insights (Behandeln von Problemen mit der Leistung von Azure SQL-Datenbank mithilfe von Intelligent Insights)](sql-database-intelligent-insights-troubleshoot-performance.md).

## <a name="built-in-intelligent-insights-analytics-with-azure-log-analytics"></a>Integrierte Intelligent Insights-Analyse mit Azure Log Analytics 

Azure Log Analytics bietet zusätzlich zu den Diagnoseprotokolldaten von Intelligent Insights Funktionen für die Berichterstellung und für Warnungen. Im Folgenden finden Sie ein Beispiel des Intelligent Insights-Berichts in Azure SQL-Analyse.

![Server](./media/sql-database-intelligent-insights/intelligent-insights-azure-sql-analytics.png)

Nachdem das Diagnoseprotokoll von Intelligent Insights dafür konfiguriert wurde, Daten an Azure SQL-Analyse zu streamen, können Sie [Azure SQL-Datenbank mithilfe von Azure SQL-Analyse überwachen](../log-analytics/log-analytics-azure-sql.md).

## <a name="custom-integrations-of-intelligent-insights-log"></a>Benutzerdefinierte Integrationen von Intelligent Insights-Protokollen

Weitere Informationen zur Entwicklung von benutzerdefinierten Warnungen und Überwachungen mithilfe von Microsoft- oder Drittanbietertools finden Sie unter [Use Intelligent Insights Azure SQL Database performance diagnostics log (Verwenden des Intelligent Insights-Diagnoseprotokolls für die Leistung von Azure SQL-Datenbank)](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="how-to-set-up-intelligent-insights-with-azure-event-hub"></a>Einrichten von Intelligent Insights mit Azure Event Hub

- Konfigurieren Sie Intelligent Insights zum Streamen der Protokollereignisse in Azure Event Hub mithilfe von [Streamen von Azure-Diagnoseprotokollen an Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md).
- Verwenden Sie Azure Event Hub für benutzerdefinierte Überwachungen und Warnungen mithilfe von [Welche Vorgänge können mit Metrik- und Diagnoseprotokollen in Event Hub ausgeführt werden?](sql-database-metrics-diag-logging.md#stream-into-azure-storage). 

## <a name="how-to-set-up-intelligent-insights-with-azure-storage"></a>Einrichten von Intelligent Insights mit Azure Storage

- Konfigurieren Sie Intelligent Insights für das Speichern mit Azure Storage mithilfe von [Streamen nach Azure Storage](sql-database-metrics-diag-logging.md#stream-into-azure-storage).

## <a name="detection-metrics"></a>Erkennungsmetriken

Metriken, die für Erkennungsmodelle verwendet werden, die intelligente Einblicke generieren, basieren auf der Überwachung der:

- Abfragedauer
- Anforderungen mit Zeitüberschreitung
- Übermäßigen Wartezeiten 
- Fehlerhaften Anforderungen

Die Abfragedauer und Anforderungen mit Zeitüberschreitung werden als primäre Modelle beim Erkennen von Problemen mit der Workloadleistung der Datenbank verwendet. Der Grund dafür ist, dass diese direkt die Vorgänge in der Workload messen. Zum Erkennen aller möglichen Fälle von Leistungsminderungen der Workload werden übermäßige Wartezeiten und fehlerhafte Anforderungen als zusätzliche Modelle verwendet, um Probleme anzugeben, die sich auf die Leistung der Workload auswirken.

Das System berücksichtigt automatisch Änderungen an der Workload und Änderungen an der Anzahl von Abfrageanforderungen, die an die Datenbank gesendet werden, um die normalen und abweichenden Schwellenwerte der Datenbankleistung dynamisch zu bestimmen.

Alle Metriken werden in verschiedenen Beziehungen über ein wissenschaftlich abgeleitetes Datenmodell berücksichtigt, das die erkannten Leistungsprobleme kategorisiert. Die Informationen, die durch einen intelligenten Einblick bereitgestellt werden, enthalten Details zum erkannten Leistungsproblem, eine Analyse der Grundursache für das Auftreten des Problems und nach Möglichkeit Empfehlungen zum Verbessern der Leistung der überwachten Azure SQL-Datenbank.

## <a name="query-duration"></a>Abfragedauer

Das Modell für die Leistungsminderung durch die Abfragedauer analysiert einzelne Abfragen und erkennt erhöhte Kompilier- und Ausführungszeiten für eine Abfrage im Vergleich zur Leistungsbaseline.

Wenn die in Azure SQL-Datenbank integrierte Logik eine beträchtliche Zunahme der Kompilier- oder Ausführungszeit einer Abfrage erkennt, die die Workloadleistung beeinträchtigt, werden diese Abfragen dafür gekennzeichnet, das Leistungsminderungsproblem durch die Abfragedauer aufzuweisen. 

Das Intelligent Insights-Diagnoseprotokoll gibt den Abfragehash der Abfrage an, deren Leistung gemindert ist, sowie eine Angabe, ob die Leistungsminderung auf der erhöhten Kompilier- oder Ausführungszeit der Abfrage basiert, und die erhöhte Abfragedauer.

## <a name="timeout-requests"></a>Anforderungen mit Zeitüberschreitung

Das Modell für die Leistungsminderung durch Abfragen mit Zeitüberschreitung analysiert einzelne Abfragen und erkennt erhöhte Zeitüberschreitungen auf Ausführungsebene der Abfragen sowie die gesamten Zeitüberschreitungen von Anfragen auf Datenbankebene im Vergleich zum Zeitraum der Leistungsbaseline.

Da bei einigen der Abfragen eine Zeitüberschreitung vorliegt, bevor die Ausführungsphase erreicht wird, misst und analysiert die in Azure SQL-Datenbank integrierte Logik durch einen Vergleich von abgebrochenen Workern mit gesendeten Anforderungen alle Abfragen, die die Datenbank erreichen, unabhängig davon, ob diese die Ausführungsphase erreicht haben oder nicht. 

Sobald die Anzahl von Timeouts für ausgeführte Abfragen oder von abgebrochenen Anforderungsworkern den vom System verwalteten Schwellenwert überschreitet, wird ein Diagnoseprotokoll mit intelligenten Einblicken aufgefüllt.

Der generierte Einblick enthält die Anzahl von Anforderungen mit Zeitüberschreitung, die Anzahl von Abfragen mit Zeitüberschreitung und Angaben, ob die Leistungsminderung aufgrund der vermehrten Zeitüberschreitungen in der Ausführungsphase oder auf der gesamten Datenbankebene vorliegt. Wenn die vermehrten Zeitüberschreitungen als relevant für die Datenbankleistung angesehen werden, werden diese Abfragen dafür gekennzeichnet, ein Problem mit der Leistungsminderung durch Zeitüberschreitung aufzuweisen. 

## <a name="excessive-wait-times"></a>Übermäßige Wartezeiten

Das Modell für übermäßige Wartezeiten überwacht die einzelnen Datenbankabfragen und erkennt ungewöhnlich hohe Wartestatistiken für Abfragen, die über den vom System verwalteten absoluten Schwellenwerten liegen. Die folgenden Metriken für übermäßige Abfragewartezeiten werden mithilfe der neuen SQL Server-Funktion beobachtet, der Funktion für Wartestatistiken des Abfragespeichers (sys.query_store_wait_stats):

- Erreichen von Ressourcengrenzwerten
- Erreichen von Ressourcenbegrenzungen von Pools für elastische Datenbanken
- Übermäßige Anzahl von Worker- oder Sitzungsthreads
- Übermäßiges Sperren der Datenbank
- Hohe Arbeitsspeicherauslastung
- Andere Wartestatistiken

Das Erreichen von Ressourcengrenzwerten von Abonnements oder Pools für elastische Datenbanken weist darauf hin, dass die Nutzung der verfügbaren Ressourcen eines Abonnements oder eines Pools für elastische Datenbanken den absoluten Schwellenwert überschritten hat und die Leistung der Workload dadurch vermindert wurde. Eine übermäßige Anzahl von Worker- oder Sitzungsthreads weist darauf hin, dass einige initiierte Workerthreads oder Sitzungen die absoluten Schwellenwerte erreicht haben und die Leistung der Workload dadurch vermindert wurde.

Ein übermäßiges Sperren der Datenbank weist darauf hin, dass die Anzahl von Sperren für eine Datenbank den absoluten Schwellenwert überschritten hat und die Leistung der Workload dadurch vermindert wurde. Eine hohe Arbeitsspeicherauslastung, bei der die Anzahl von Threads, die Arbeitsspeicherzuweisungen anfordern, den absoluten Schwellenwert überschreiten, gibt eine Leistungsminderung der Workload an.

Andere Erkennungen der Wartestatistiken geben an, dass verschiedene Metriken den absoluten Schwellenwert überschreiten, die über die Wartestatistiken des Abfragespeichers gemessen werden und die Leistung der Workload dadurch vermindert wurde.

Sobald übermäßige Wartezeiten erkannt werden, gibt das Diagnoseprotokoll von Intelligent Insights abhängig von den verfügbaren Daten die Hashes von beeinträchtigenden und beeinträchtigten Abfragen aus, deren Leistung gemindert ist, sowie Details zu den Metriken, die die Wartezeiten der Abfragen in der Ausführung verursachen, und die gemessenen Wartezeiten.

## <a name="errored-requests"></a>Fehlerhafte Anforderungen

Das Modell für Leistungsminderung durch fehlerhafte Anforderungen überwacht einzelne Abfragen und erkennt eine Erhöhung der Anzahl von Abfragen, die im Vergleich zum Zeitraum der Baseline fehlgeschlagen sind. Das Modell überwacht auch kritische Ausnahmen, die die von der in Azure-Datenbank integrierten Logik verwalteten absoluten Schwellenwerte erreicht haben. Das System berücksichtigt automatisch die Anzahl der Abfrageanforderungen für Workloadänderungen im überwachten Zeitraum, die an die Datenbank und die Konten gesendet wurden.

Wenn die gemessene Erhöhung der fehlgeschlagenen Anforderungen in Beziehung zur Gesamtzahl der gesendeten Anforderungen als relevant für die Workloadleistung angesehen wird, werden die betroffenen Abfragen dafür gekennzeichnet, Probleme mit der Leistungsminderung durch fehlerhafte Anforderungen aufzuweisen.

Das Protokoll von Intelligent Insights gibt die Anzahl von fehlgeschlagenen Anforderungen sowie Angaben dazu aus, ob die Leistungsminderung auf die vermehrten fehlerhaften Anforderungen oder auf das Erreichen der überwachten Schwellenwerte für kritische Ausnahmen zurückzuführen ist, und die gemessene Zeit der Leistungsminderung. 

Falls eine der überwachten kritischen Ausnahmen die vom System verwalteten absoluten Schwellenwerte erreicht, wird ein intelligenter Einblick mit Details zu den kritischen Ausnahmen generiert.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum [Behandeln von Problemen mit der Leistung von Azure SQL-Datenbank mithilfe von Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md)
* Verwenden des [Intelligent Insights-Diagnoseprotokolls für die Leistung von Azure SQL-Datenbank](sql-database-intelligent-insights-use-diagnostics-log.md)
* Erfahren Sie mehr über das [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse](../log-analytics/log-analytics-azure-sql.md).
* Erfahren Sie mehr über das [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).


