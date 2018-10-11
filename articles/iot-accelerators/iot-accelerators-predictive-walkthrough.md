---
title: Übersicht über den Solution Accelerator für Predictive Maintenance – Azure | Microsoft-Dokumentation
description: Eine Übersicht für den Azure IoT-Solution Accelerator für Predictive Maintenance.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: dobett
ms.openlocfilehash: 822e02d42be8ce516901190af4be579d13ac841d
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888321"
---
# <a name="predictive-maintenance-solution-accelerator-overview"></a>Übersicht über den Solution Accelerator für Predictive Maintenance

Der Solution Accelerator für Predictive Maintenance ist eine End-to-End-Lösung für ein Geschäftsszenario, mit der der Zeitpunkt prognostiziert wird, zu dem voraussichtlich ein Fehler auftritt. Sie können diesen Solution Accelerator beispielsweise zur Optimierung von Wartungsroutinen nutzen. Bei dieser Lösung werden zentrale Azure IoT-Solution Accelerator-Dienste wie IoT Hub, Stream Analytics und ein [Azure Machine Learning][lnk-machine-learning]-Arbeitsbereich kombiniert. Der Arbeitsbereich enthält ein Modell zum Vorhersagen der Restlebensdauer (Remaining Useful Life, RUL) eines Flugzeugtriebwerks auf der Grundlage eines öffentlichen Datasets mit Beispielwerten. Bei der Lösung wird das IoT-Geschäftsszenario vollständig als Ausgangspunkt implementiert, damit Sie eine Lösung planen und implementieren können, die Ihre besonderen Geschäftsanforderungen erfüllt.

## <a name="logical-architecture"></a>Logische Architektur

Im folgenden Diagramm werden die logischen Komponenten des Solution Accelerators beschrieben:

![][img-architecture]

Die blauen Elemente sind Azure-Dienste, die in der Region bereitgestellt werden, in der Sie den Solution Accelerator bereitgestellt haben. Die Liste mit den Regionen, in denen Sie den Solution Accelerator bereitstellen können, wird auf der [Bereitstellungsseite][lnk-azureiotsuite] angezeigt.

Das grüne Element ist ein simuliertes Gerät, das für ein Flugzeugtriebwerk steht. Der Abschnitt [Simulierte Geräte](#simulated-devices) enthält weitere Informationen zu diesen simulierten Geräten.

Die grauen Elemente stehen für Komponenten, mit denen Funktionen für die *Geräteverwaltung* implementiert werden. Im aktuellen Release des Solution Accelerators für Predictive Maintenance werden diese Ressourcen nicht bereitgestellt. Weitere Informationen zur Geräteverwaltung finden Sie unter [Erkunden der Funktionen des Solution Accelerators für Remoteüberwachung][lnk-remote-monitoring].

## <a name="azure-resources"></a>Azure-Ressourcen

Navigieren Sie im Azure-Portal zur Ressourcengruppe mit dem von Ihnen gewählten Lösungsnamen, um Ihre bereitgestellten Ressourcen anzuzeigen.

![Accelerator-Ressourcen][img-resource-group]

Beim Bereitstellen des Solution Accelerators erhalten Sie eine E-Mail mit einem Link zum Machine Learning-Arbeitsbereich. Sie können auch über die Seite [Microsoft Azure IoT Solution Accelerators][lnk-azureiotsuite] Ihrer bereitgestellten Lösung zum Machine Learning-Arbeitsbereich navigieren. Wenn sich die Lösung im Zustand **Bereit** befindet, steht auf der Seite eine Kachel zur Verfügung.

![Machine Learning-Modell][img-machine-learning]

## <a name="simulated-devices"></a>Simulierte Geräte

Im Solution Accelerator steht ein simuliertes Gerät für ein Flugzeugtriebwerk. Die Lösung wird mit zwei Triebwerken bereitgestellt, die einem Flugzeug zugeordnet sind. Jedes Triebwerk gibt vier Arten von Telemetriedaten aus: Sensor 9, Sensor 11, Sensor 14 und Sensor 15. Auf der Grundlage dieser Daten berechnet das Machine Learning-Modell die Restlebensdauer des Triebwerks. Jedes simulierte Gerät sendet die folgenden Telemetriemeldungen an IoT Hub:

*Zyklusanzahl*. Ein Zyklus steht für einen absolvierten Flug mit einer Dauer von zwei bis zehn Stunden. Während des Flugs werden jede halbe Stunde Telemetriedaten erfasst.

*Telemetrie*. Es sind vier Sensoren vorhanden, die für die Triebwerkattribute stehen. Die Sensoren tragen die generischen Bezeichnungen Sensor 9, Sensor 11, Sensor 14 und Sensor 15. Diese vier Sensoren liefern genügend Telemetriedaten, um auf der Grundlage des Restlebensdauer-Modells sinnvolle Ergebnisse zu erhalten. Das im Solution Accelerator verwendete Modell basiert auf einem öffentlichen Dataset mit echten Daten von Triebwerksensoren. Weitere Informationen dazu, wie das Modell aus dem ursprünglichen Dataset erstellt wurde, finden Sie unter [Cortana Intelligence-Katalog: Vorlage für die vorbeugende Wartung][lnk-cortana-analytics].

Außerdem können die simulierten Geräte auch die folgenden Befehle verarbeiten, die vom IoT-Hub in der Lösung gesendet werden:

| Get-Help | BESCHREIBUNG |
| --- | --- |
| StartTelemetry |Steuert den Status der Simulation.<br/>Startet das Senden der Telemetriedaten vom Gerät. |
| StopTelemetry |Steuert den Status der Simulation.<br/>Beendet das Senden der Telemetriedaten vom Gerät. |

IoT Hub führt die Bestätigung der Gerätebefehle durch.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-Auftrag

**Auftrag: Telemetrie** verarbeitet den eingehenden Datenstrom mit Gerätetelemetriedaten mit zwei Anweisungen:

* Mit der ersten Anweisung werden alle Telemetriedaten von den Geräten ausgewählt und an Blobspeicher gesendet. Von dort aus werden sie in der Web-App visualisiert.
* Mit der zweiten Anweisung werden die durchschnittlichen Sensorwerte für ein gleitendes Fenster von zwei Minuten berechnet und über den Event Hub an einen **Ereignisprozessor** gesendet.

## <a name="event-processor"></a>Ereignisprozessor
Der **Ereignisprozessorhost** wird im Rahmen eines Azure-Webauftrags ausgeführt. Der **Ereignisprozessor** verwendet die durchschnittlichen Sensorwerte eines abgeschlossenen Zyklus. Diese Werte übergibt er anschließend an eine API, die das trainierte Modell zum Berechnen der Restlebensdauer eines Triebwerks verfügbar macht. Die API wird von einem Machine Learning-Arbeitsbereich verfügbar gemacht, der als Teil der Lösung bereitgestellt wird.

## <a name="machine-learning"></a>Machine Learning
Die Machine Learning-Komponente verwendet ein Modell, das sich von Daten ableitet, die von echten Flugzeugtriebwerken gesammelt wurden. Über die Kachel auf der Seite [azureiotsuite.com][lnk-azureiotsuite] Ihrer Lösung können Sie zum Machine Learning-Arbeitsbereich navigieren. Die Kachel ist verfügbar, wenn sich die Lösung im Zustand **Bereit** befindet.

Das Azure Machine Learning-Modell steht als Vorlage zur Verfügung, um die Anwendung dieser Funktionen auf Gerätetelemetriedaten zu veranschaulichen, die über IoT-Solution Accelerator-Dienste erfasst wurden. Microsoft hat anhand öffentlich zugänglicher Daten <sup>\[1\]</sup> ein [Regressionsmodell][lnk_regression_model] eines Flugzeugtriebwerks erstellt und eine schrittweise Anleitung zur Verwendung des Modells veröffentlicht.

Der Azure IoT-Solution Accelerator für Predictive Maintenance verwendet das mit dieser Vorlage erstellte Regressionsmodell. Das Modell wird in Ihrem Azure-Abonnement bereitgestellt und über eine automatisch erstellte API verfügbar gemacht. Die Lösung umfasst eine Teilmenge der Testdaten mit vier (von insgesamt 100) Triebwerken und vier (von insgesamt 21) Sensordatenströmen. Diese Daten sind ausreichend, um ein genaues Ergebnis aus dem trainierten Modell bereitzustellen.

*\[1\] A. Saxena und K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set“, NASA Ames Prognostics Data Repository (https://c3.nasa.gov/dashlink/resources/139/), NASA Ames Research Center, Moffett Field, CA*

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun die zentralen Komponenten des Solution Accelerators für Predictive Maintenance kennen, können Sie ihn anpassen.

Sie können auch einige andere Features und Funktionen der IoT-Solution Accelerators ausprobieren:

* [Häufig gestellte Fragen zu IoT-Solution Accelerators][lnk-faq]
* [Sicherheit im Internet der Dinge von Anfang an][lnk-security-groundup]

[img-architecture]: media/iot-accelerators-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-accelerators-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-accelerators-predictive-walkthrough/machine-learning.png

[lnk-remote-monitoring]: quickstart-predictive-maintenance-deploy.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsolutions.com/
[lnk-faq]: iot-accelerators-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
