---
title: Erkunden von Daten mit dem Azure Time Series Insights-Explorer | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure Time Series Insights-Explorers im Webbrowser schnell eine globale Ansicht Ihrer Big Data anzeigen und die IoT-Umgebung überprüfen können.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: db24c2c7a64c7a5cdca3eb27088dd82d9bb2bf3d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237717"
---
# <a name="azure-time-series-insights-explorer"></a>Azure Time Series Insights-Explorer

In diesem Artikel werden die Features und Optionen in „Allgemeine Verfügbarkeit“ für die Web-App des [Azure Time Series Insights-Explorers](https://insights.timeseries.azure.com/) beschrieben. Der Time Series Insights-Explorer veranschaulicht die leistungsstarken Datenvisualisierungsfunktionen, die der Dienst bereitstellt und auf die innerhalb Ihrer eigenen Umgebung zugegriffen werden kann.

Azure Time Series Insights ist ein vollständig verwalteter Analyse-, Speicher- und Visualisierungsdienst, der die simultane Untersuchung und Analyse von Milliarden von IoT-Ereignissen ermöglicht. Sie erhalten globale Einblicke in Ihre Daten und können basierend darauf Ihre IoT-Lösung schneller überprüfen und kostenintensive Ausfallzeiten unternehmenskritischer Geräte vermeiden. Sie können versteckte Trends aufdecken, Abweichungen erkennen und Ursachenanalysen nahezu in Echtzeit durchführen. Der Time Series Insights-Explorer ist derzeit als öffentliche Vorschau verfügbar.

> [!TIP]
> Eine interaktive Tour durch die Demoumgebung finden Sie in [Azure Time Series Insights – Schnellstart](time-series-quickstart.md).

## <a name="video"></a>Video

### <a name="learn-about-querying-data-using-the-time-series-insights-explorer-br"></a>Informieren Sie sich über die Abfrage von Daten mit dem Time Series Insights-Explorer. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>Sehen Sie sich das vorstehende Video <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">„Erste Schritte mit TSI unter Einsatz eines Azure IoT Solution Accelerators“</a> an.

## <a name="prerequisites"></a>Voraussetzungen

Vor der Verwendung des Time Series Insights-Explorers müssen Sie folgende Schritte ausführen:

- Erstellen Sie eine Time Series Insights-Umgebung. Weitere Informationen finden Sie unter [Erste Schritte mit Time Series Insights](./time-series-insights-get-started.md).
- [Erteilen Sie Zugriff](time-series-insights-data-access.md) auf Ihr Konto in der Umgebung.
- Fügen Sie ihm einen [IoT Hub](time-series-insights-how-to-add-an-event-source-iothub.md) oder eine [Event Hub](time-series-insights-how-to-add-an-event-source-eventhub.md)-Ereignisquelle hinzu.

## <a name="explore-and-query-data"></a>Untersuchen und Abfragen von Daten

Binnen weniger Minuten nach der Verbindung der Ereignisquelle mit der Time Series Insights-Umgebung können Sie Ihre Zeitreihendaten untersuchen und abfragen.

1. Öffnen Sie zunächst den [Time Series Insights-Explorer](https://insights.timeseries.azure.com/) in Ihrem Webbrowser, und wählen Sie links im Fenster eine Umgebung aus. Alle Umgebungen, auf die Sie Zugriff haben, sind in alphabetischer Reihenfolge aufgeführt.

1. Nachdem Sie eine Umgebung ausgewählt haben, verwenden Sie entweder die Konfigurationen **VON** und **BIS** im oberen Bereich, oder ziehen Sie die gewünschte Zeitspanne.  Klicken Sie auf die Lupe oben rechts, oder klicken Sie mit der rechten Maustaste auf die ausgewählte Zeitspanne, und wählen Sie **Suchen** aus.  

1. Sie können zudem die Verfügbarkeit automatisch einmal pro Minute aktualisieren, indem Sie die Schaltfläche **Auto On** (Automatisch ein) auswählen. Die Schaltfläche **Auto On**gilt nur für das Verfügbarkeitsdiagramm, nicht für den Inhalt der Hauptvisualisierung.

1. Beachten Sie, dass Sie über das Azure-Cloudsymbol zu Ihrer Umgebung im Azure-Portal gelangen.

   [![Time Series Insights-Umgebung](media/time-series-insights-explorer/explorer1.png)](media/time-series-insights-explorer/explorer1.png#lightbox)

1. Als Nächstes sehen Sie ein Diagramm, in dem die Anzahl aller Ereignisse in der ausgewählten Zeitspanne angezeigt wird.  Hier stehen mehrere Steuerelemente zur Verfügung:

    **Editor-Bereich für Begriffe**:  Im Begriffsbereich führen Sie Abfragen in Ihrer Umgebung durch.  Er befindet sich auf der linken Seite des Bildschirms:
      - **Measure**:  In dieser Dropdownliste werden alle numerischen Spalten (**Doubles**) angezeigt.
      - **Aufteilen nach**: In dieser Dropdownliste werden Kategoriespalten (**Strings**) angezeigt.
      - Über die Systemsteuerung neben „Measure“ können Sie die Schrittinterpolation aktivieren, Mindest- und Höchstwerte anzeigen und die Y-Achse anpassen.  Zudem können Sie festlegen, ob die angezeigten Daten als Anzahl, Durchschnitt oder Summe der Daten angezeigt wird.
      - Sie können bis zu fünf Begriffe hinzufügen, die auf derselben X-Achse angezeigt werden.  Verwenden Sie die Schaltfläche **copy-down** (Kopieren), um einen zusätzlichen Begriff hinzuzufügen, oder klicken Sie auf **Hinzufügen**, um einen neuen Begriff hinzuzufügen.

        [![Editor-Bereich für Begriffe](media/time-series-insights-explorer/explorer2.png)](media/time-series-insights-explorer/explorer2.png#lightbox)

      - **Prädikat**:  Über das Prädikat können Sie die Ereignisse mithilfe der unten aufgeführten Operanden schnell filtern. Wenn Sie eine Suche durch Auswählen/Klicken durchführen, wird das Prädikat basierend auf dieser Suche automatisch aktualisiert. Folgende Operandentypen werden unterstützt:

         |Vorgang  |Unterstützte Typen  |Notizen  |
         |---------|---------|---------|
         |`<`, `>`, `<=`, `>=`     |  Double, DateTime, TimeSpan       |         |
         |`=`, `!=`, `<>`     | String, Bool, Double, DateTime, TimeSpan, NULL        |         |
         |IN     | String, Bool, Double, DateTime, TimeSpan, NULL        |  Alle Operanden müssen denselben Typ aufweisen oder eine NULL-Konstante sein.        |
         |HAS     | Zeichenfolge        |  Im rechten Bereich sind nur konstante Zeichenfolgenliterale zulässig. Leere Zeichenfolgen und NULL sind nicht zulässig.       |

      - **Beispiele für Abfragen**

         [![Beispielabfragen](media/time-series-insights-explorer/explorer9.png)](media/time-series-insights-explorer/explorer9.png#lightbox)

1. Mit dem Schieberegler für die **Intervallgröße** können Sie die Intervalle in einer Zeitspanne vergrößern und verkleinern.  Dies ermöglicht eine genauere Steuerung der Bewegung zwischen größeren Zeitabschnitten, die Trends gleichmäßig bis hin zu kleinsten Zeitabschnitten in Millisekunden anzeigen, sodass Sie präzise, hochauflösende Schnitte Ihrer Daten anzeigen können. Der Standardstartpunkt des Schiebereglers ist als optimierte Ansicht der Daten aus Ihrer Auswahl festgelegt und gleicht somit Auflösung, Abfragegeschwindigkeit und Granularität aus.

1. Mit dem Tool **Time Brush** können Sie problemlos zwischen den einzelnen Zeitspannen navigieren, sodass in der intuitiven Benutzeroberfläche eine nahtlose Bewegung zwischen Zeitspannen möglich ist.

1. Mit dem Befehl **Speichern** können Sie die aktuelle Abfrage speichern und zur Freigabe für andere Benutzer der Umgebung aktivieren. Über **Öffnen** können Sie alle Ihre gespeicherten Abfragen und alle für andere Benutzer in Umgebungen, auf die Sie Zugriff haben, freigegebenen Abfragen anzeigen.

   [![Abfragen](media/time-series-insights-explorer/explorer3.png)](media/time-series-insights-explorer/explorer3.png#lightbox)

## <a name="visualize-data"></a>Visualisieren von Daten

1. Das Tool **Perspektivische Ansicht** ermöglicht eine parallele Ansicht von bis zu vier eindeutigen Abfragen. Die Schaltfläche „Perspektivische Ansicht“ finden Sie oben rechts im Diagramm.  

   [![Perspektivische Ansicht](media/time-series-insights-explorer/explorer4.png)](media/time-series-insights-explorer/explorer4.png#lightbox)

1. Im **Diagramm** können Sie Ihre Daten visuell untersuchen. Folgende Diagrammtools sind verfügbar:

    - **Auswählen/Klicken**, wodurch eine Auswahl einer bestimmten Zeitspanne oder einer einzelnen Datenreihe aktiviert wird.  
    - Innerhalb einer Zeitspannenauswahl können Sie Ereignisse vergrößern oder untersuchen.  
    - Innerhalb einer Datenreihe können Sie die Reihe nach einer anderen Spalte teilen, die Reihe als neuen Begriff hinzufügen, nur die ausgewählte Reihe anzeigen, die ausgewählte Reihe ausschließen, die Reihe anheften oder Ereignisse aus der ausgewählten Reihe untersuchen.
    - Im Filterbereich links neben dem Diagramm können Sie alle angezeigten Datenreihen sehen und nach Wert oder Namen neu anordnen sowie alle Datenreihen oder aber angeheftete oder getrennte Reihen anzeigen.  Sie können außerdem eine einzelne Datenreihe auswählen und nach einer anderen Spalte teilen, die Reihe als neuen Begriff hinzufügen, nur die ausgewählte Reihe anzeigen, die ausgewählte Reihe ausschließen, die Reihe anheften oder Ereignisse aus der ausgewählten Reihe untersuchen.
    - Beim gleichzeitigen Anzeigen von mehreren Begriffen können Sie über die Schaltflächen oben rechts im Diagramm Stapel bilden, die Stapelung aufheben, weitere Daten zu einer Datenreihe anzeigen und für alle Begriffe dieselbe Y-Achse verwenden.

    [![Diagrammtool](media/time-series-insights-explorer/explorer5.png)](media/time-series-insights-explorer/explorer5.png#lightbox)

1. Mithilfe des **Wärmebilds** können Sie schnell eindeutige oder ungewöhnliche Datenreihen in einer bestimmten Abfrage erkennen. Nur ein Suchbegriff kann als Wärmebild visualisiert werden.

    [![Wärmebild](media/time-series-insights-explorer/explorer6.png)](media/time-series-insights-explorer/explorer6.png#lightbox)

1. **Ereignisse**:  Wenn Sie oben beim Auswählen oder Klicken mit der rechten Maustaste Ereignisse untersuchen möchten, wird der Ereignisbereich angezeigt.  Hier können Sie alle Ihre Rohereignisse anzeigen und die Ereignisse als JSON- oder CSV-Dateien exportieren. In Time Series Insights werden alle Rohdaten gespeichert.

    [![Ereignisse](media/time-series-insights-explorer/explorer7.png)](media/time-series-insights-explorer/explorer7.png#lightbox)

1. Klicken Sie nach dem Untersuchen von Ereignissen auf die Registerkarte **STATISTIK**, um die Funktionen für Muster und Spaltenstatistik anzuzeigen.  

    - **Muster:** Mit diesem Feature werden die meisten statistisch signifikanten Muster in einer ausgewählten Datenregion angezeigt. So müssen Sie nicht Tausende von Ereignissen prüfen, um festzustellen, welche Muster am meisten Zeit und Energie rechtfertigen. Darüber hinaus können Sie in Time Series Insights direkt in diese statistisch signifikanten Muster springen und dann eine Analyse durchführen. Dieses Feature ist auch nützlich für nachträgliche Überprüfungen von Verlaufsdaten.

    - **Spaltenstatistik**:  Mit der Spaltenstatistik werden Diagramme und Tabellen angezeigt, in denen Daten aus den einzelnen Spalten der ausgewählten Datenreihe über die ausgewählte Zeitspanne unterteilt sind.  

      [![STATISTIK](media/time-series-insights-explorer/explorer8.png)](media/time-series-insights-explorer/explorer8.png#lightbox)

Damit kennen Sie nun die verschiedenen in der Web-App des Time Series Insights-Explorers verfügbaren Funktionen und Optionen.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Diagnostizieren und Lösen von Problemen](time-series-insights-diagnose-and-solve-problems.md) in Ihrer Time Series Insights-Umgebung.

- Nehmen Sie an der interaktiven Tour [Azure Time Series Insights – Schnellstart](time-series-quickstart.md) teil.
