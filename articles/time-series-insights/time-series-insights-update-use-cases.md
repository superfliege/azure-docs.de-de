---
title: Anwendungsfälle für Azure Time Series Insights Preview | Microsoft-Dokumentation
description: Grundlagen zu Anwendungsfällen für Azure Time Series Insights Preview.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: c7339dc683a062d4a74708d2e9f155f20f4d0ea8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244081"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Anwendungsfälle für Azure Time Series Insights Preview

In diesem Artikel werden einige häufige Anwendungsfälle für die Azure Time Series Insights Preview zusammengefasst. Die Empfehlungen in diesem Artikel dienen als Ausgangspunkt für die Entwicklung Ihrer Anwendungen und Lösungen mit Time Series Insights.

Im Einzelnen werden in diesem Artikel die folgenden Fragen beantwortet:

* Was sind häufige Anwendungsfälle für Time Series Insights?
* Welche Vorteile bietet die Verwendung von Time Series Insights für das [Durchsuchen von Daten und die Erkennung visueller Anomalien](#data-exploration-and-visual-anomaly-detection)?
* Welche Vorteile bietet die Verwendung von Time Series Insights für die [Betriebsanalyse und Prozesseffizienz](#operational-analysis-and-driving-process-efficiency)?
* Welche Vorteile bietet die Verwendung von Time Series Insights für die [erweiterte Analyse](#advanced-analytics)?

Nachfolgend wird eine Übersicht über diese Nutzungsszenarien beschrieben.

## <a name="introduction"></a>Einführung

Azure Time Series Insights ist ein End-to-End-PaaS-Angebot (Platform-as-a-Service). Es wird verwendet, um stark kontextualisierte, zeitreihenoptimierte IoT-Daten zu sammeln, zu verarbeiten, zu speichern, zu analysieren und abzufragen. Time Series Insights ist ideal für Ad-hoc-Datenuntersuchungen und Betriebsanalysen geeignet. Bei Time Series Insights handelt es sich um ein speziell erweiterbares, angepasstes Dienstangebot, das für die weit reichenden Anforderungen von IoT-Bereitstellungen in der Industrie konzipiert ist.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Datenuntersuchung und visuelle Anomalieerkennung

Durchsuchen und analysiere Sie Milliarden von Ereignissen in Echtzeit, um Anomalien und versteckte Trends in Ihren Daten zu erkennen. Time Series Insights bietet Leistung in nahezu Echtzeit für Ihre IoT- und DevOps-Analyseworkloads.

[![Daten-Explorer](media/v2-update-use-cases/data-explorer.svg)](media/v2-update-use-cases/data-explorer.svg#lightbox)

Die meisten Kunden sind sich einige, dass die Geschwindigkeit des Erkenntnisgewinns (time to insight) zu den wichtigsten Eigenschaften von Time Series Insights gehört. Time Series Insights erfordert keine Vorbereitung der Daten. Es arbeitet so schnell, dass es Sie binnen weniger Minuten mit unzähligen Ereignissen in Ihrem Azure IoT Hub oder Event Hub verbindet. Sobald eine Verbindung hergestellt ist, können Sie Milliarden von Ereignissen visualisieren und analysieren, um Anomalien und versteckte Trends in Ihren Daten zu erkennen.

Time Series Insights ist intuitiv und einfach zu verwenden. Sie können mit Ihren Daten interagieren, ohne auch nur eine Zeile Code schreiben zu müssen. Sie müssen auch keine neue Programmiersprache lernen. Time Series Insights bietet granulare, textbasierte Abfragen für erfahrene Benutzer, die mit SQL vertraut sind. Darüber hinaus bietet es Novizen die Möglichkeit der Untersuchung per Auswählen und Klicken.

Kunden profitieren von der hohen Geschwindigkeit der Diagnose ressourcenbezogener Probleme. Sie können DevOps ausführen, um die Grundursache eines Fehlers in einer IoT-Lösung ausfindig zu machen. Sie können außerdem Bereiche für die Untersuchung durch Data Science-Initiativen ermitteln.  

Es gibt drei primäre Methoden zur Interaktion mit in Time Series Insights gespeicherten Daten:

- Die erste und einfachste Möglichkeit für den Einstieg ist der Time Series Insights Preview-Explorer. Sie können ihn verwenden, um alle Ihrer IoT-Daten an einem Ort schnell zu visualisieren. Er bietet Tools wie das Wärmebild, das Ihnen beim Erkennen von Anomalien in Ihren Daten hilft. Ferner bietet er eine perspektivische Ansicht. Mit dieser können Sie bis zu vier Ansichten aus einer oder mehreren Time Series Insights-Umgebungen in einem einzelnen Dashboard vergleichen. Das Dashboard bietet Ihnen eine Ansicht Ihrer Zeitreihendaten über alle Orte hinweg. Weitere Informationen zum [Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md). Lesen Sie zur Planung Ihrer Time Series Insights-Umgebung [Time Series Insights-Planung](./time-series-insights-update-plan.md).

- Die zweite Methode für den Einstieg besteht in der Verwendung des JavaScript SDK, um leistungsstarke Diagramme und Grafiken schnell in Ihre Webanwendung einzubetten. Mit nur wenigen Codezeilen können Sie leistungsstarke Abfragen schreiben. Verwenden Sie diese zum Auffüllen von Liniendiagrammen, Kreisdiagrammen, Balkendiagrammen, Wärmebildern, Datenrastern und mehr. Alle diese Elemente sind bei Verwendung des SDK vorgefertigt und sofort einsatzbereit vorhanden. Das SDK abstrahiert außerdem Time Series Insights-Abfrage-APIs. Sie können sie zum Erstellen von SQL-ähnlichen Prädikaten verwenden, um die Daten abzufragen, die Sie in einem Dashboard anzeigen möchten. Für Hybridpräsentationsebenen-Lösungen bietet Time Series Insights parametrisierte URLs. Diese bieten nahtlose Verbindungspunkte mit dem Time Series Insights Preview-Explorer für tief gehende Einblicke in Ihre Daten.

    * Lesen Sie die Dokumentationen [Time Series Insights JS-Clientbibliothek](tutorial-explore-js-client-lib.md) und [Time Series Insights-Client](https://github.com/Microsoft/tsiclient), um mehr über das JavaScript-SDK zu erfahren.

    * Weitere Informationen zum Freigeben von URLs und zur neuen Benutzeroberfläche finden Sie unter [Visualisieren von Daten in der Explorer-Vorschauversion](time-series-insights-update-explorer.md).

- Die dritte Methode für den Einstieg besteht in der Verwendung leistungsfähiger APIs zum Abfragen von in Time Series Insights gespeicherten Daten. Time Series Insights verfügt über temporale Operatoren wie `from`, `to`, `first` und `last`. Es verfügt über Aggregationen und Transformationen wie `average`, `min`, `max`, `split by`, `order by` und `DateHistogram`. Es gibt auch Filteroperatoren wie `has`, `in`, `and`, `or`, `greater than` und `REGEX`. Alle diese Operatoren ermöglichen Downstreamanwendungen das schnelle Auffinden interessanter Trends und Muster in Ihren Daten. Verwenden Sie sie zum Auffüllen selbst entwickelter Visualisierungen, um Anomalien zu erkennen.

## <a name="operational-analysis-and-driving-process-efficiency"></a>Betriebsanalyse und Optimieren der Prozesseffizienz

Verwenden Sie Time Series Insights, um die Integrität, Verwendung und Leistung von Geräten im Verhältnis zu überwachen. Time Series Insights bietet eine einfache Möglichkeit zur Messung der Betriebseffizienz. Mit Time Series Insights können Sie verschiedene und unvorhersehbare IoT-Workloads verwalten, ohne die Leistung bei eingehendem Datenverkehr oder Abfragen zu beeinträchtigen.

[![Übersicht](media/v2-update-use-cases/overview.svg)](media/v2-update-use-cases/overview.svg#lightbox)

Das Streamen und die fortlaufende Verarbeitung von Daten, die aus operativen Prozessen stammen, können jedes Unternehmen erfolgreich transformieren, wenn sie mit der richtigen Technologie oder Lösung kombiniert werden. Häufig sind diese Lösungen eine Kombination aus mehreren Systemen. Sie ermöglichen die Untersuchung und Analyse von Daten, die sich ständig ändern, insbesondere im IoT-Bereich, und ein gemeinsames Muster aufweisen.

Diese Muster beginnen häufig mit IoT-fähigen Plattformen, die Milliarden von Ereignissen von Geräten und Sensoren empfangen, die sich an verschiedenen Standorten befinden. Diese Systeme verarbeiten und analysieren Datenströme, um in Echtzeit Erkenntnisse und Aktionen aus diesen Daten abzuleiten. Daten werden normalerweise auf Warm und Cold Storage archiviert, um Batchanalysen nahezu in Echtzeit zu ermöglichen.

Gesammelte Daten durchlaufen eine Reihe von Verarbeitungsstufen, um sie zu bereinigen und in Kontext zu setzen für Downstreamabfrage- und Analyseszenarien. Azure bietet umfassende Dienste, die auf IoT-Szenarien wie Objektwartung und -fertigung angewendet werden können. Zu diesen Diensten gehören Time Series Insights, IoT Hub, Event Hubs, Azure Stream Analytics, Azure Functions, Azure Logic Apps, Azure Databricks, Azure Machine Learning und Power BI.

Eine Lösungsarchitektur lässt sich auf folgende Weise aufbauen:

- Erfassen von Daten über IoT Hub oder Event Hubs für klassenbeste Sicherheit, Durchsatz und Latenz.
- Durchführen der Datenverarbeitung und -berechnungen. Durchleiten der erfassten Daten durch Dienste wie Stream Analytics, Logic Apps und Azure Functions. Der Dienst, den Sie verwenden, hängt von den spezifischen Datenverarbeitungsanforderungen ab.
- Aus den Verarbeitungspipelines berechnete Signale werden zur Speicherung und Analyse per Push an Time Series Insights gesendet.

Time Series Insights bietet Datenuntersuchung nahezu in Echtzeit sowie objektbasierte Erkenntnisse aus historischen Daten. Je nach Ihren geschäftlichen Anforderungen können MapReduce- und Hive-Aufträge mit in Time Series Insights gespeicherten Daten ausgeführt werden, indem Time Series Insights mit Azure HDInsight verbunden wird. In Time Series Insights gespeicherte Daten sind für Power BI und andere Kundenanwendungen über die Abfrage-APIs der öffentlichen Oberfläche von Time Series Insights verfügbar. Diese Daten können für tief gehende Geschäfts- und Operational Intelligence-Szenarios verwendet werden.

## <a name="advanced-analytics"></a>Erweiterte Analyse

Integration mit erweiterten Analysediensten wie Machine Learning und Azure Databricks. Time Series Insights erfasst Rohdaten von Millionen von Geräten. Es fügt Kontextdaten hinzu, die nahtlos von einer Suite von Azure-Analysediensten verwendet werden können.

[![Analysen](media/v2-update-use-cases/advanced-analytics.svg)](media/v2-update-use-cases/advanced-analytics.svg#lightbox)

Erweiterte Analysen und Machine Learning nutzen und verarbeiten große Mengen von Daten. Diese Daten werden verwendet, um datengesteuerte Entscheidungen zu treffen und Vorhersageanalysen durchzuführen. Bei IoT-Anwendungsfällen lernen erweiterte Analysealgorithmen aus den von Millionen von Geräten gesammelten Daten. Diese Geräte übertragen mehrmals pro Sekunde Daten. Die von IoT-Geräten erfassten Daten sind roh. Ihnen fehlen Kontextinformationen wie der Ort des Geräts und die Einheit der Sensormessung. Hieraus folgt, dass sich Rohdaten schwer direkt für erweiterte Analysen verwenden lassen.

Time Series Insights schließt die Lücke zwischen IoT-Daten und erweiterten Analysen auf zwei einfache und kostengünstige Weisen:

- Erstens erfasst Time Series Insights Telemetrierohdaten von Millionen von Geräten mithilfe von IoT Hub. Es reichert die Daten mit Kontextinformationen an und transformiert die Daten in ein Parquet-Format. Dieses Format lässt sich einfach mit anderen erweiterten Analysediensten wie Machine Learning, Azure Databricks und Drittanbieteranwendungen integrieren.

    Time Series Insights kann als Quelle der Wahrheit für alle Daten in der gesamten Organisation dienen. Er erstellt ein zentrales Repository für die Nutzung von Downstreamanalyse-Workloads. Da es sich bei Time Series Insights um einen nahezu in Echtzeit arbeitenden Speicherdienst handelt, können erweiterte Analysemodelle kontinuierlich aus den eingehenden IoT-Telemetriedaten lernen. Deshalb können die Modelle exaktere Vorhersagen treffen.

- Zweitens kann die Ausgabe von Machine Learning und Vorhersagemodellen an Time Series Insights übergeben werden, deren Ergebnisse zu visualisieren und zu speichern. Dieses Verfahren hilft Organisationen dabei, ihre Modelle zu optimieren. Time Series Insights vereinfacht die Visualisierung von Telemetrie-Streamingdaten in derselben Ebene wie die Ausgaben der trainierten Modelle. Auf diese Weise hilft es Data Science-Teams dabei, Anomalien und Muster zu erkennen.  

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).

- Lesen Sie zur Planung Ihrer Umgebung [Time Series Insights Preview-Planung](./time-series-insights-update-plan.md).

- Lesen Sie die Dokumentation zum [Time Series Insights-Client](https://github.com/Microsoft/tsiclient).
