---
title: Was ist Azure Time Series Insights? | Microsoft Docs
description: "Einführung in Azure Time Series Insights, einem neuen Dienst zum Analysieren von Zeitreihendaten und für IoT-Lösungen."
services: time-series-insights
ms.service: time-series-insights
author: op-ravi
ms.author: omravi
manager: jhubbard
editor: MarkMcGeeAtAquent
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 95cb26ada6f8ea39bc1a437a755f80ee7ddb7698
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="what-is-azure-time-series-insights"></a>Was ist Azure Time Series Insights?

Time Series Insights ist zum Speichern, Visualisieren und Abfragen großer Mengen an Zeitreihendaten ausgelegt, z.B. der von IoT-Geräten generierten Daten.  Wenn Sie Zeitreihendaten in der Cloud speichern, verwalten, abfragen oder visualisieren möchten, ist Time Series Insights wahrscheinlich der richtige Dienst für Sie.  

Wenn Sie eine Anwendung zur internen Verwendung oder zur Verwendung durch externe Kunden erstellen, kann Time Series Insights als Back-End für die Indizierung, Speicherung und Aggregation von Zeitreihendaten verwendet werden.  Darüber hinaus können Sie eine benutzerdefinierte Visualisierungs- und Benutzeroberfläche erstellen.  Time Series Insights umfasst REST-Abfrage-APIs zur Ermöglichung dieses Szenarios.  

Wenn Sie nicht sicher sind, ob es sich bei Ihren Daten um Zeitreihendaten handelt, erhalten Sie hier alle wichtigen Informationen.  Zeitreihendaten stellen dar, wie eine Ressource oder ein Prozess sich im Zeitablauf ändert.  Sie sind insofern eindeutig, als sie über einen Zeitstempel verfügen und Zeit als Achse am aussagekräftigsten ist.  Zeitreihendaten gehen normalerweise in zeitlicher Reihenfolge ein und werden in der Regel als Einfügung und nicht als Aktualisierung in der Datenbank behandelt.  Da in Time Series Insights jedes neue Ereignis als Zeile erfasst und gespeichert wird, werden Änderungen im Lauf der Zeit gemessen, sodass Sie zurückblicken und zukünftige Änderungen vorhersagen können.  Die Speicherung, Indizierung, Abfrage, Analyse und Visualisierung großer Mengen an Zeitreihendaten können eine Herausforderung darstellen.  

## <a name="primary-scenarios"></a>Hauptszenarien

- Skalierbare Speicherung von Zeitreihendaten  
  - Im Kern enthält Time Series Insights eine Datenbank, die im Hinblick auf Zeitreihendaten entworfen wurde.  Da Time Series Insights skalierbar und vollständig verwaltet ist, wird die Speicherung und Verwaltung von Ereignissen verarbeitet.

- Datensuche nahezu in Echtzeit  
  - Time Series Insights umfasst einen Explorer, über den das gesamte Datenstreaming in einer Umgebung visualisiert wird.  Kurz nach der Verbindung mit einer Ereignisquelle können die Ereignisdaten in Time Series Insights angezeigt, durchsucht und abgefragt werden.  Die Daten eignen sich zum Überprüfen, ob ein Gerät Daten wie erwartet ausgibt, und zum Überwachen einer IoT-Ressource auf Integrität, Produktivität und Wirksamkeit.  

- Ursachenanalyse und Anomalieerkennung
  - Time Series Insights umfasst Tools wie Muster und perspektivische Ansichten zum Durchführen und Speichern von mehrstufigen Ursachenanalysen.  Darüber hinaus wird Time Series Insights in Verbindung mit Warnungsdiensten wie Azure Stream Analytics ausgeführt, sodass Warnungen und erkannte Anomalien im Time Series Insights-Explorer nahezu in Echtzeit angezeigt werden können.  

- Globale Ansicht eingehender Zeitreihendaten aus verschiedenen Standorten zum Vergleich mehrerer Ressourcen und Standorte
  - Sie können mehrere Ereignisquellen mit einer Time Series Insights-Umgebung verbinden.  Das bedeutet, dass Daten, die aus mehreren verschiedenen Standorten eingehen, nahezu in Echtzeit zusammen angezeigt werden können.  Benutzer können diese Sichtbarkeit nutzen, um Daten mit Führungskräften zu teilen und eine bessere Kollaboration mit Experten im jeweiligen Bereich zu ermöglichen, die ihre Kenntnisse einfließen lassen können, um Probleme zu lösen, bewährte Methoden anzuwenden und Erfahrungen auszutauschen.

- Erstellen einer auf Time Series Insights aufbauenden Kundenanwendung 
  - Time Series Insights umfasst REST-Abfrage-APIs, sodass Sie Anwendungen erstellen können, in denen Zeitreihendaten verwendet werden.

## <a name="capabilities"></a>Funktionen

- **Schneller Einstieg:** Azure Time Series Insights erfordert keine Vorbereitung der Daten. Stellen Sie binnen weniger Minuten eine Verbindung mit unzähligen Ereignissen in Ihrem Azure IoT Hub oder Event Hub her. Sobald eine Verbindung besteht, können Sie Sensordaten visualisieren, mit ihnen interagieren und Ihre IoT-Lösungen überprüfen. Sie können mit Ihren Daten interagieren, ohne Code zu schreiben.
Sie müssen keine neue Sprache lernen: Für fortgeschrittene Benutzer steht in Time Series Insights eine Oberfläche für präzise Freitextabfragen und eine Point-and-Click-Datenuntersuchung zur Verfügung.
- **Einblicke nahezu in Echtzeit:** Mit Time Series Insights können pro Tag unzählige Sensorereignisse erfasst werden – mit einer Latenz von einer Minute. Time Series Insights bietet Einblicke in Ihre Sensordaten. So können Sie Trends und Anomalien erkennen, Ursachenanalysen durchführen und teure Ausfallzeiten vermeiden. Dank der Korrelation von Echtzeit- und Verlaufsdaten können Sie mit Time Series Insights versteckte Trends in den Daten aufdecken.
- **Erstellen von benutzerdefinierten Lösungen:** Integrieren Sie die Azure Time Series Insights-Daten in Ihre bestehenden Anwendungen, oder erstellen Sie mithilfe der Time Series Insights-REST-APIs neue benutzerdefinierte Lösungen. Erstellen Sie personalisierte Ansichten, die Sie mit anderen teilen können, um ihnen die Untersuchung Ihrer Daten zu ermöglichen.
- **Skalierbarkeit:** Time Series Insights ist für die skalierte IoT-Nutzung ausgelegt. Es können zwischen 1 Million und 100 Millionen Ereignisse pro Tag erfasst werden, und die Standardaufbewahrungsdauer beträgt 31 Tage. Sie können sowohl Verlaufsdaten als auch Livedatenströme nahezu in Echtzeit visualisieren und analysieren. Für die Zukunft ist geplant, die Eingangs- und Aufbewahrungsraten weiter zu erhöhen, um unternehmensweite Skalierungen zu ermöglichen.

## <a name="getting-started"></a>Erste Schritte
Die ersten Schritte nehmen weniger als 5 Minuten in Anspruch. 

1.  Stellen Sie zunächst eine Time Series Insights-Umgebung über das Azure-Portal bereit. 
2.  Verbinden Sie eine Ereignisquelle, beispielsweise einen Azure IoT Hub oder Event Hub.  
3.  Laden Sie Verweisdaten hoch. (Dies ist kein zusätzlicher Dienst.)
4.  Sie können Ihre Daten innerhalb von Minuten mit dem Time Series Insights-Explorer anzeigen.

## <a name="time-series-insights-explorer"></a>Time Series Insights-Explorer
Dieses Diagramm ist ein Beispiel für über den Explorer angezeigte Time Series Insights-Daten: ![Time Series Insights-Explorer] (media/time-series-insights-explorer/explorer4.png)


## <a name="next-steps"></a>Nächste Schritte
 - [Erkunden des Time Series Insights-Explorers in einer Demoumgebung](./time-series-quickstart.md)
 - [Planen Ihrer Time Series Insights-Umgebung](time-series-insights-environment-planning.md)

