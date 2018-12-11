---
title: Erkunden der Azure Time Series Insights-Demoumgebung (Vorschauversion) | Microsoft-Dokumentation
description: Grundlagen der Azure Time Series Insights-Demoumgebung (Vorschauversion)
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 12/03/2018
ms.openlocfilehash: 76bdc122cab5b4ee87717c9c4df9774ecc3bd8f7
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888774"
---
# <a name="explore-the-azure-time-series-insights-preview-demo-environment"></a>Erkunden der Azure Time Series Insights-Demoumgebung (Vorschauversion)

Diese Schnellstartanleitung veranschaulicht die ersten Schritte mit dem Explorer für Azure Time Series Insights (TSI) (Vorschauversion) in einer kostenlosen Demoumgebung. Sie erfahren, wie Sie eine große Menge von industriellen IoT-Verlaufsdaten im Webbrowser visualisieren, und lernen die wichtigsten Features des Azure Time Series Insights-Explorers (Vorschauversion) kennen.

Mit Azure TSI wird ein umfassendes PaaS-Angebot (Platform-as-a-Service) bereitgestellt, mit dem Sie stark kontextualisierte IoT-Daten mit Zeitreihenoptimierung für die Ad-hoc-Datenuntersuchung und betriebsbezogene Analysen erfassen, verarbeiten, speichern und abfragen können. Time Series Insights ist ein spezielles Angebot, das auf die einzigartigen Anforderungen von IoT-Bereitstellungen in der Industrie zugeschnitten ist.

In der Demoumgebung wird TSI von dem Stromerzeugungsunternehmen Contoso genutzt, um aus den Daten verwertbare Erkenntnisse zu ermitteln und eine kurze Analyse der Grundursache durchzuführen. Contoso betreibt zwei Windfarmen, die jeweils zehn Windturbinen umfassen. Jede Turbine verfügt über 20 Sensoren, die jede Minute Daten an Azure IoT Hub melden. Die Sensoren sammeln Informationen zu Wetterbedingungen, Schaufelteilung und Gierposition, Generatorleistung, Getriebeverhalten und Sicherheitsüberwachung.

TSI (Vorschauversion) wird zum Analysieren der ständig wachsenden Datenmenge der letzten beiden Jahre – derzeit 40 GB – verwendet, um sowohl kritische Fehler als auch Probleme aufgrund langsamer Wartung besser zu verstehen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein  [kostenloses Azure-Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)  erstellen, bevor Sie beginnen.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Erkunden des Time Series Insights-Explorers in einer Demoumgebung

1. Navigieren Sie in Ihrem Browser zu  [insights.timeseries.azure.com/preview/samples](https://insights.timeseries.azure.com/preview/samples).  

1. Melden Sie sich bei entsprechender Aufforderung mit Ihren Anmeldeinformationen für das Azure-Konto am TSI-Explorer an.

### <a name="demo-step-one"></a>Demo: Schritt 1

1. Wir sehen uns **Windturbine #7 der Windfarm #1** an.  

    * **Aktion**: Aktualisieren Sie den Anzeigebereich in `1/1/17 20:00 – 3/10/17 20:00 (UTC)`, und fügen Sie den Sensor `Farm 1 > W7 > Generator > GeneratorSpeed` hinzu. Zeigen Sie anschließend die sich ergebenden Werte an.

       ![Schnellstart 1][1]

1. Vor Kurzem hat **Contoso einen Brand in Windturbine #7 erkannt**. Dies sehen wir uns genauer an. Wir sehen, dass der Sensor für den Feueralarm während des Brands aktiviert wurde.

    * **Aktion**: Aktualisieren Sie den Anzeigebereich in `3/9/17 20:00 – 3/10/17 20:00 (UTC)`, und fügen Sie den Sensor `Safety > FireAlert` hinzu.

      ![Schnellstart 2][2]

1. Wir sehen uns an, was zum Zeitpunkt des Brands sonst noch passiert ist. Sowohl der Öldruck als auch die aktiven Warnungen sind kurz vor dem Brand stark gestiegen, aber an diesem Punkt war es zu spät, um das Problem abzuwenden.

    * **Aktion**: Fügen Sie den Sensor `Pitch > HydraulicOilPressure` und den Sensor `Pitch > ActiveWarning` hinzu.

      ![Schnellstart 3][3]

1. Wenn wir die Ansicht verkleinern, ist zu erkennen, dass es Anzeichen für das Auftreten des Brands gab. Beide Sensoren haben schwankende Werte aufgewiesen. Ist dies schon einmal passiert?

    * **Aktion**: Aktualisieren Sie den Anzeigebereich in `2/24/17 20:00 – 3/10/17 20:00 (UTC)`.

      ![Schnellstart 4][4]

1. Wenn wir uns die Daten für die gesamten zwei Jahre ansehen, fällt uns auf, dass schon einmal ein Brand mit den gleichen Anzeichen aufgetreten ist. Mit diesen Daten erstellen wir Systeme, mit denen Probleme dieser Art frühzeitig erkannt werden können.

    * **Aktion**: Aktualisieren Sie den Anzeigebereich in `1/1/16 – 12/31/17` (alle Daten).

       ![Schnellstart 5][5]

### <a name="demo-step-two"></a>Demo: Schritt 2

1. Andere Probleme sind subtiler und schwieriger zu diagnostizieren. Time Series Insights enthält verschiedene Funktionen, mit denen wir schwierige Probleme aufspüren können. Hier wird der Ausfall eines Warnsensors für `turbine #6` am `6/25` angezeigt. Aber was ist tatsächlich passiert?

    * **Aktion**: Entfernen Sie die aktuellen Sensoren. Aktualisieren Sie anschließend den Anzeigebereich in `6/1/17 20:00 – 7/1/17 20:00 (UTC)`, und fügen Sie `Farm 1 > W6 > Safety > VoltageActuatorSwitchWarning` hinzu.

       ![Schnellstart 6][6]

1. Mit der Warnung wird auf ein Problem mit der Ausgangsspannung des Generators hingewiesen. Aber was ist die Ursache? Die gesamte Ausgangsleistung des Generators sieht für kleinere Intervalle unproblematisch aus. Aber wenn wir die Daten aggregieren, ist ein deutlicher Abfall zu erkennen.

    * **Aktion**: Stellen Sie eine Remoteverbindung mit `VoltageActuatorSwitchWarning` her, und fügen Sie `Generator > ActivePower` hinzu. Aktualisieren Sie das Intervall anschließend in `3d`.

       ![Schnellstart 7][7]

1. Wenn wir im Dataset in Vorwärtsrichtung navigieren, wird klar, dass dies nicht nur ein vorübergehendes Problem ist. Es ist ein dauerhaftes Problem.

    * **Aktion**: Erweitern Sie die Zeitspanne nach rechts.

       ![Schnellstart 8][8]

1. Wir rufen weitere Informationen auf. Wir können uns andere Sensordatenpunkte ansehen, um die Spannung nach Phase anzuzeigen. Es sieht aber alles ähnlich aus. Wir setzen einen Marker, um die tatsächlichen Werte anzuzeigen. Scheinbar liegt ein Problem mit dem Ausgang von Phase 3 vor.

    * **Aktion**: `Add Generator > GridVoltagePhase1, 2, & 3`. Setzen Sie einen Marker für den letzten Datenpunkt im sichtbaren Bereich.

       ![Schnellstart 8][8]

1. Wenn wir für alle drei denselben Maßstab verwenden, ist der Abfall für Phase 3 noch besser zu erkennen. An diesem Punkt können wir dieses Problem an unser Wartungsteam weiterleiten und nützliche Hinweise zur Ursache der Warnung bereitstellen.  

    * **Aktion**: Aktualisieren Sie die Anzeige, um für alle Sensoren denselben Diagrammmaßstab zu verwenden.

       ![Schnellstart 9][9]

## <a name="next-steps"></a>Nächste Schritte

Sie können nun Ihre eigene Azure TSI-Umgebung (Vorschauversion) erstellen:

> [!div class="nextstepaction"]
> [Plan your Azure TSI (Preview) environment](time-series-insights-update-plan.md) (Planen Ihrer Azure TSI-Umgebung (Vorschauversion))

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