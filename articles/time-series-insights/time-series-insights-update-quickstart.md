---
title: 'Schnellstart: Erkunden der Azure Time Series Insights-Demoumgebung (Vorschauversion) | Microsoft-Dokumentation'
description: Machen Sie sich mit der Azure Time Series Insights-Demoumgebung (Vorschauversion) vertraut.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 12/03/2018
ms.openlocfilehash: de5e853db6c6a0e98dea9251cc07b526288574e1
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276825"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Schnellstart: Erkunden der Azure Time Series Insights-Demoumgebung (Vorschauversion)

Dieser Schnellstart veranschaulicht die Verwendung des Azure Time Series Insights-Explorers (Vorschauversion) in einer kostenlosen Demoumgebung. Sie erfahren, wie Sie große Mengen von industriellen historischen IoT-Daten im Webbrowser visualisieren, und lernen die wichtigsten Features des Time Series Insights-Explorers (Vorschauversion) kennen.

Time Series Insights ist ein End-to-End-PaaS-Angebot (Platform-as-a-Service). Der Dienst kann stark kontextualisierte, zeitreihenoptimierte IoT-Daten erfassen, verarbeiten, speichern und abfragen, sodass ohne Vorbereitung Datenuntersuchungen durchgeführt werden können. Zudem bietet er Betriebsanalysen. Time Series Insights ist ein spezielles Angebot, das auf die einzigartigen Anforderungen von IoT-Bereitstellungen in der Industrie zugeschnitten ist.

Die Demoumgebung zeigt das Stromerzeugungsunternehmen Contoso. In der Umgebung verwenden Sie Time Series Insights, um verwertbare Erkenntnisse aus den Daten von Contoso zu gewinnen und eine kurze Grundursachenanalyse durchzuführen. Contoso betreibt zwei Windfarmen mit jeweils zehn Windturbinen. Jede Turbine verfügt über 20 Sensoren, die jede Minute Daten an Azure IoT Hub melden. Die Sensoren erfassen Informationen zu Wetterbedingungen, Schaufelteilung und Gierposition, Generatorleistung, Getriebeverhalten und Sicherheitsüberwachung.

Mithilfe von Time Series Insights (Vorschauversion) analysieren Sie das stetig wachsende Dataset von Contoso der letzten zwei Jahre, das gegenwärtig 40 GB groß ist. Mithilfe der Analyse können Sie sowohl kritische Fehler als auch Probleme aufgrund langsamer Wartung besser verstehen und vorhersagen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein  [kostenloses Azure-Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)  erstellen, bevor Sie beginnen.

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Erkunden des Time Series Insights-Explorers in einer Demoumgebung

1. Navigieren Sie in Ihrem Browser zur  [Umgebung „Contoso Wind Farm“](https://insights.timeseries.azure.com/preview/samples).  

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto beim Time Series Insights-Explorer an, wenn Sie dazu aufgefordert werden.

### <a name="demo-step-1"></a>Demo: Schritt 1

1. Wir sehen uns die Windturbine **W7** in **Contoso Plant 1** an.  

    * **Aktion**: Aktualisieren Sie den Anzeigebereich in **1/1/17 20:00 bis 3/10/17 20:00 (UTC)**, fügen Sie den Sensor **Contoso Plant 1** > **W7** > **Generator System** > **GeneratorSpeed** hinzu, und zeigen Sie dann die resultierenden Werte an.

       ![Schnellstart 1][1]

1. Vor Kurzem hat Contoso einen Brand in der Windturbine **W7** erkannt. Dies sehen wir uns genauer an. Wir sehen, dass der Sensor für den Feueralarm während des Brands aktiviert wurde.

    * **Aktion**: Aktualisieren Sie den Anzeigebereich in **3/9/17 20:00 bis 3/10/17 20:00 (UTC)**, und fügen Sie den Sensor **Safety System** > **FireAlert** hinzu.

      ![Schnellstart 2][2]

1. Wir sehen uns an, was zum Zeitpunkt des Brands sonst noch passiert ist. Sowohl der Öldruck als auch die aktiven Warnungen sind kurz vor dem Brand stark gestiegen, an diesem Punkt war es jedoch zu spät, um das Problem abzuwenden.

    * **Aktion**: Fügen Sie die Sensoren **Pitch System** > **HydraulicOilPressure** und **Pitch System** > **ActiveWarning** hinzu.

      ![Schnellstart 3][3]

1. Wenn wir die Ansicht verkleinern, ist zu erkennen, dass es Anzeichen für das Auftreten des Brands gab. Beide Sensoren haben schwankende Werte aufgewiesen. Ist dieses Problem schon einmal aufgetreten?

    * **Aktion**: Aktualisieren Sie den Anzeigebereich in **2/24/17 20:00 bis 3/10/17 20:00 (UTC)**.

      ![Schnellstart 4][4]

1. Wenn wir uns die Daten für die gesamten zwei Jahre ansehen, fällt uns auf, dass schon einmal ein Brand mit den gleichen Anzeichen aufgetreten ist. Mit diesen Daten können wir Systeme zur frühzeitigen Erkennung von Problemen dieser Art erstellen.

    * **Aktion**: Aktualisieren Sie den Anzeigebereich in **1/1/16 bis 12/31/17** (alle Daten).

       ![Schnellstart 5][5]

### <a name="demo-step-2"></a>Demo: Schritt 2

1. Andere Probleme sind subtiler und schwieriger zu diagnostizieren. Time Series Insights bietet eine Reihe von Möglichkeiten zum Aufspüren schwieriger Probleme. Hier wird der Ausfall eines Warnsensors von **W6** für das Datum **6/25** angezeigt. Aber was ist tatsächlich passiert?

    * **Aktion**: Entfernen Sie die aktuellen Sensoren, aktualisieren Sie den Anzeigebereich in **6/1/17 20:00 bis 7/1/17 20:00 (UTC)**, und fügen Sie dann den Sensor **Contoso Plant 1** > **W6** > **Safety System** > **VoltageActuatorSwitchWarning** hinzu.

       ![Schnellstart 6][6]

1. Mit der Warnung wird auf ein Problem mit der Ausgangsspannung des Generators hingewiesen. Aber was ist die Ursache? Die gesamte Ausgangsleistung des Generators sieht für kleinere Intervalle unproblematisch aus. Aber wenn wir die Daten aggregieren, ist ein deutlicher Abfall zu erkennen.

    * **Aktion**: Entfernen Sie den Sensor **VoltageActuatorSwitchWarning**, fügen Sie den Sensor **Generator System** > **ActivePower** hinzu, und aktualisieren Sie das Intervall in **3d**.

       ![Schnellstart 7][7]

1. Wenn wir im Dataset vorwärts navigieren, sehen wir, dass dies kein vorübergehendes Problem ist. Es besteht noch immer.

    * **Aktion**: Erweitern Sie die Zeitspanne nach rechts.

       ![Schnellstart 8][8]

1. Wir rufen weitere Informationen auf. Wir können andere Sensordatenpunkte hinzufügen, um die Spannung nach Phase anzuzeigen. Die Datenpunkte sehen jedoch alle ähnlich aus. Wir setzen einen Marker, um die tatsächlichen Werte anzuzeigen. Scheinbar liegt ein Problem mit dem Ausgang von Phase 3 vor.

    * **Aktion**: Fügen Sie die Sensoren **Generator System** > **GridVoltagePhase1**, **GridVoltagePhase2** und **GridVoltagePhase3** hinzu. Setzen Sie einen Marker für den letzten Datenpunkt im sichtbaren Bereich.

       ![Schnellstart 8][8]

1. Wenn wir für alle drei Datenpunkte denselben Maßstab verwenden, ist der Abfall für Phase 3 noch besser zu erkennen. An diesem Punkt können wir das Problem an unser Wartungsteam weiterleiten und nützliche Hinweise zur Ursache der Warnung bereitstellen.  

    * **Aktion**: Aktualisieren Sie die Anzeige, um für alle Sensoren denselben Diagrammmaßstab zu verwenden.

       ![Schnellstart 9][9]

## <a name="next-steps"></a>Nächste Schritte

Nun können Sie Ihre eigene Time Series Insights-Umgebung (Vorschauversion) erstellen:

> [!div class="nextstepaction"]
> [Plan your Time Series Insights Preview environment](time-series-insights-update-plan.md) (Planen Ihrer Time Series Insights-Umgebung (Vorschauversion))

<!-- Images -->
[1]: media/v2-update-quickstart/quickstart-one.png
[2]: media/v2-update-quickstart/quickstart-two.png
[3]: media/v2-update-quickstart/quickstart-three.png
[4]: media/v2-update-quickstart/quickstart-four.png
[5]: media/v2-update-quickstart/quickstart-five.png
[6]: media/v2-update-quickstart/quickstart-six.png
[7]: media/v2-update-quickstart/quickstart-seven.png
[8]: media/v2-update-quickstart/quickstart-eight.png
[9]: media/v2-update-quickstart/quickstart-nine.png
