---
title: Anwendungsfälle für Azure Time Series Insights Preview | Microsoft-Dokumentation
description: Grundlagen zu Anwendungsfällen für Azure Time Series Insights Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: a6ee03197a78a75ca57d0ed049cdd6a8df8b2149
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53272965"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Anwendungsfälle für Azure Time Series Insights Preview

Dieser Artikel bietet eine Übersicht über verschiedene häufige Anwendungsfälle für Azure Time Series Insights Preview. Die Empfehlungen in diesem Artikel dienen als Ausgangspunkt für die Entwicklung Ihrer Anwendungen und Lösungen mit Time Series Insights.

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

* Was sind häufige Anwendungsfälle für Time Series Insights?
* Was sind die Vorteile der Verwendung von Time Series Insights für die Datenuntersuchung und visuelle Anomalieerkennung?
* Was sind die Vorteile der Verwendung von Time Series Insights für die Betriebsanalyse und Prozesseffizienz?
* Was sind die Vorteile der Verwendung von Time Series Insights für komplexere Analysen?

Dieses Dokument bietet eine Übersicht von Anwendungsfällen, für die Azure Time Series Insights Preview entwickelt wurde.

## <a name="introduction"></a>Einführung

Time Series Insights ist ein End-to-End-PaaS-Angebot (Platform-as-a-Service). Es wird zum Erfassen, Verarbeiten, Speichern und Abfragen stark kontextualisierter, zeitreihenoptimierter Daten im IoT-Maßstab verwendet. Time Series Insights ist ideal für Ad-hoc-Datenuntersuchungen und Betriebsanalysen geeignet. Bei Time Series Insights handelt es sich um ein speziell erweiterbares, angepasstes Dienstangebot, das für die weit reichenden Anforderungen von IoT-Bereitstellungen in der Industrie konzipiert ist.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Datenuntersuchung und visuelle Anomalieerkennung

Durchsuchen und analysiere Sie Milliarden von Ereignissen in Echtzeit, um Anomalien und versteckte Trends in Ihren Daten zu erkennen. Time Series Insights bietet Leistung in nahezu Echtzeit für Ihre IoT- und DevOps-Analyseworkloads.

![Daten-Explorer][1]

Die meisten Kunden sind sich einige, dass die Geschwindigkeit des Erkenntnisgewinns (time to insight) zu den wichtigsten Eigenschaften von Time Series Insights gehört. Time Series Insights erfordert keine Vorbereitung der Daten. Es arbeitet so schnell, dass es Sie binnen weniger Minuten mit unzähligen Ereignissen in Ihrem Azure IoT Hub oder Event Hub verbindet. Sobald eine Verbindung hergestellt ist, können Sie Milliarden von Ereignissen visualisieren und analysieren, um Anomalien und versteckte Trends in Ihren Daten zu erkennen. 

Time Series Insights ist intuitiv und einfach zu verwenden. Sie können mit Ihren Daten interagieren, ohne auch nur eine Zeile Code schreiben zu müssen. Sie müssen auch keine neue Programmiersprache lernen. Time Series Insights bietet granulare, textbasierte Abfragen für erfahrene Benutzer, die mit SQL vertraut sind. Darüber hinaus bietet es Novizen die Möglichkeit der Untersuchung per Auswählen und Klicken.

Kunden profitieren von der hohen Geschwindigkeit der Diagnose ressourcenbezogener Probleme. Sie können DevOps ausführen, um die Grundursache eines Fehlers in einer IoT-Lösung ausfindig zu machen. Sie können außerdem Bereiche für die Untersuchung durch Data Science-Initiativen ermitteln.  

Es gibt drei primäre Methoden zur Interaktion mit in Time Series Insights gespeicherten Daten:

- Die erste und einfachste Möglichkeit für den Einstieg ist der Time Series Insights Preview-Explorer. Sie können ihn verwenden, um alle Ihrer IoT-Daten an einem Ort schnell zu visualisieren. Er bietet Tools wie das Wärmebild, das Ihnen beim Erkennen von Anomalien in Ihren Daten hilft. Ferner bietet er eine perspektivische Ansicht. Mit dieser können Sie bis zu vier Ansichten aus einer oder mehreren Time Series Insights-Umgebungen in einem einzelnen Dashboard vergleichen. Das Dashboard bietet Ihnen eine Ansicht Ihrer Zeitreihendaten über alle Orte hinweg. Weitere Informationen zum [Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md). Lesen Sie zur Planung Ihrer Time Series Insights-Umgebung [Time Series Insights-Planung](./time-series-insights-update-plan.md).

- Die zweite Methode für den Einstieg besteht in der Verwendung des JavaScript SDK, um leistungsstarke Diagramme und Grafiken schnell in Ihre Webanwendung einzubetten. Mit nur wenigen Codezeilen können Sie leistungsstarke Abfragen schreiben. Verwenden Sie diese zum Auffüllen von Liniendiagrammen, Kreisdiagrammen, Balkendiagrammen, Wärmebildern, Datenrastern und mehr. Alle diese Elemente sind bei Verwendung des SDK vorgefertigt und sofort einsatzbereit vorhanden. Das SDK abstrahiert außerdem Time Series Insights-Abfrage-APIs. Sie können sie zum Erstellen von SQL-ähnlichen Prädikaten verwenden, um die Daten abzufragen, die Sie in einem Dashboard anzeigen möchten. Für Hybridpräsentationsebenen-Lösungen bietet Time Series Insights parametrisierte URLs. Diese bieten nahtlose Verbindungspunkte mit dem Time Series Insights Preview-Explorer für tief gehende Einblicke in Ihre Daten. Weitere Informationen zum JavaScript-SDK finden Sie in Dokumentation zur [Time Series Insights-JS-Clientbibliothek](https://docs.microsoft.com/azure/time-series-insights/tutorial-explore-js-client-lib) und zum [Time Series Insights-Client](https://github.com/Microsoft/tsiclient). Weitere Informationen zu parametrisierten URLs finden Sie unter [Parametrisierte URLs](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-parameterized-urls).

- Die dritte Methode für den Einstieg besteht in der Verwendung leistungsfähiger APIs zum Abfragen von in Time Series Insights gespeicherten Daten. Time Series Insights umfasst temporale Operatoren wie von, bis, erster und letzter (from, to, first, last). Es verfügt über Aggregationen und Transformationen wie Durchschnitt, Min, Max, Teilen nach, Sortieren nach und Datenhistogramm (average, min, max, split by, order by, DateHistogram). Ferner verfügt es über Filteroperatoren wie „hat“, „in und“, „oder“, „größer als“ und „regulärer Ausdruck“ (has, in and, or, greater than, REGEX). Alle diese Operatoren ermöglichen Downstreamanwendungen das schnelle Auffinden interessanter Trends und Muster in Ihren Daten. Verwenden Sie sie zum Auffüllen selbst entwickelter Visualisierungen, um Anomalien zu erkennen.

## <a name="operational-analysis-and-driving-process-efficiency"></a>Betriebsanalyse und Optimieren der Prozesseffizienz

Verwenden Sie Time Series Insights, um die Integrität, Verwendung und Leistung von Geräten im Verhältnis zu überwachen. Time Series Insights bietet eine einfache Möglichkeit zur Messung der Betriebseffizienz. Mit Time Series Insights können Sie verschiedene und unvorhersehbare IoT-Workloads verwalten, ohne die Leistung bei eingehendem Datenverkehr oder Abfragen zu beeinträchtigen.

![Übersicht][2]

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

![Analytics][3]

Erweiterte Analysen und Machine Learning nutzen und verarbeiten große Mengen von Daten. Diese Daten werden verwendet, um datengesteuerte Entscheidungen zu treffen und Vorhersageanalysen durchzuführen. Bei IoT-Anwendungsfällen lernen erweiterte Analysealgorithmen aus den von Millionen von Geräten gesammelten Daten. Diese Geräte übertragen mehrmals pro Sekunde Daten. Die von IoT-Geräten erfassten Daten sind roh. Ihnen fehlen Kontextinformationen wie der Ort des Geräts und die Einheit der Sensormessung. Hieraus folgt, dass sich Rohdaten schwer direkt für erweiterte Analysen verwenden lassen.

Time Series Insights schließt die Lücke zwischen IoT-Daten und erweiterten Analysen auf zwei einfache und kostengünstige Weisen: 

- Erstens erfasst Time Series Insights Telemetrierohdaten von Millionen von Geräten mithilfe von IoT Hub. Es reichert die Daten mit Kontextinformationen an und transformiert die Daten in ein Parquet-Format. Dieses Format lässt sich einfach mit anderen erweiterten Analysediensten wie Machine Learning, Azure Databricks und Drittanbieteranwendungen integrieren. 

    Time Series Insights kann als Quelle der Wahrheit für alle Daten in der gesamten Organisation dienen. Er erstellt ein zentrales Repository für die Nutzung von Downstreamanalyse-Workloads. Da es sich bei Time Series Insights um einen nahezu in Echtzeit arbeitenden Speicherdienst handelt, können erweiterte Analysemodelle kontinuierlich aus den eingehenden IoT-Telemetriedaten lernen. Deshalb können die Modelle exaktere Vorhersagen treffen.

- Zweitens kann die Ausgabe von Machine Learning und Vorhersagemodellen an Time Series Insights übergeben werden, deren Ergebnisse zu visualisieren und zu speichern. Dieses Verfahren hilft Organisationen dabei, ihre Modelle zu optimieren. Time Series Insights vereinfacht die Visualisierung von Telemetrie-Streamingdaten in derselben Ebene wie die Ausgaben der trainierten Modelle. Auf diese Weise hilft es Data Science-Teams dabei, Anomalien und Muster zu erkennen.  

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).
- Lesen Sie zur Planung Ihrer Umgebung [Time Series Insights Preview-Planung](./time-series-insights-update-plan.md).
- Lesen Sie die Dokumentation zum [Time Series Insights-Client](https://github.com/Microsoft/tsiclient).

<!-- Images -->
[1]: media/v2-update-use-cases/data-explorer.svg
[2]: media/v2-update-use-cases/overview.svg
[3]: media/v2-update-use-cases/advanced-analytics.svg
