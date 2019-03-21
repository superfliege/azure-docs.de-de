---
title: Erstellen von Triggerabhängigkeiten für rollierende Fenster in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie in Azure Data Factory eine Abhängigkeit von einem Trigger für ein rollierendes Fenster erstellt wird.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/11/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: f3a547f5b953262d7263d1be0897161cf4091a1d
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57574388"
---
# <a name="create-a-tumbling-window-trigger-dependency"></a>Erstellen einer Triggerabhängigkeit für ein rollierendes Fenster

Dieser Artikel enthält die Schritte zum Erstellen einer Abhängigkeit von einem Trigger für ein rollierendes Fenster. Allgemeine Informationen zu Triggern für rollierende Fenster finden Sie unter [Erstellen eines Triggers für ein rollierendes Fenster](how-to-create-tumbling-window-trigger.md).

Verwenden Sie diese erweiterte Funktion zum Erstellen einer Abhängigkeit für das rollierende Fenster, um eine Abhängigkeitskette zu erstellen und sicherzustellen, dass ein Trigger erst nach der erfolgreichen Ausführung eines anderen Triggers in der Data Factory ausgeführt wird.

## <a name="create-a-dependency-in-the-data-factory-ui"></a>Erstellen einer Abhängigkeit mithilfe der Data Factory-Benutzeroberfläche

Wählen Sie zum Erstellen einer Abhängigkeit von einem Trigger die Optionen **Trigger > Erweitert > Neu** aus, und wählen Sie dann den gewünschten Trigger mit dem entsprechenden Offset und der entsprechenden Größe aus. Wählen Sie **Fertig stellen** aus, und veröffentlichen Sie die Data Factory-Änderungen, damit die Abhängigkeiten wirksam werden.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency01.png)

## <a name="tumbling-window-dependency-properties"></a>Eigenschaften der Abhängigkeit für ein rollierendes Fenster

Eine Triggerabhängigkeit für ein rollierendes Fenster weist die folgenden Eigenschaften auf:

```json
{
    "name": "DemoTrigger",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00.000Z",
            "delay": "00:00:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "TumblingWindowTriggerDependencyReference",
                    "size": "-02:00:00",
                    "offset": "-01:00:00",
                    "referenceTrigger": {
                        "referenceName": "DemoDependency1",
                        "type": "TriggerReference"
                    }
                }
            ]
        }
    }
}
```

Die folgende Tabelle enthält die Liste der Attribute, die zum Definieren einer Abhängigkeit für ein rollierendes Fenster erforderlich sind.

| **Eigenschaftenname** | **Beschreibung**  | **Typ** | **Erforderlich** |
|---|---|---|---|
| Trigger  | In dieser Dropdownliste werden alle vorhandenen Trigger für rollierende Fenster angezeigt. Wählen Sie den Trigger aus, auf den sich die Abhängigkeit beziehen soll.  | TumblingWindowTrigger | Ja |
| Offset | Der Offset des Abhängigkeitstriggers. Geben Sie einen Wert im Format einer Zeitspanne an. Dabei sind positive und negative Offsets zulässig. Dieser Parameter ist obligatorisch, wenn der Trigger von sich selbst abhängig ist. In allen anderen Fällen ist er optional. Die Selbstabhängigkeit muss immer ein negativer Offset sein. | Timespan | Selbstabhängigkeit: Ja, andernfalls: Nein  |
| Fenstergröße | Die Größe des abhängigen rollierenden Fensters. Geben Sie einen Wert im Format einer Zeitspanne an. Dieser Parameter ist optional. | Timespan | Nein   |
|||||

## <a name="tumbling-window-self-dependency-properties"></a>Eigenschaften der Selbstabhängigkeit für ein rollierendes Fenster

Erstellen Sie eine Selbstabhängigkeit in Szenarien, in denen der Trigger erst mit dem nächsten Fenster fortfahren soll, wenn das vorherige Fenster erfolgreich abgeschlossen ist. Von sich selbst abhängige Trigger weisen die folgenden Eigenschaften auf:

```json
{
    "name": "DemoSelfDependency",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00Z",
            "delay": "00:01:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": "01:00:00",
                    "offset": "-01:00:00"
                }
            ]
        }
    }
}
```

Nachfolgend finden Sie Abbildungen der Szenarien für und der Verwendung von Eigenschaften der Abhängigkeit für rollierende Fenster.

## <a name="dependency-offset"></a>Offset der Abhängigkeit

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency02.png)

## <a name="dependency-size"></a>Größe der Abhängigkeit

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency03.png)

## <a name="self-dependency"></a>Selbstabhängigkeit

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency04.png)

## <a name="usage-scenarios"></a>Verwendungsszenarien

### <a name="dependency-on-another-tumbling-window-trigger"></a>Abhängigkeit von einem anderen Trigger für ein rollierendes Fenster

Hier ist ein Beispiel für einen täglichen Auftrag zur Verarbeitung von Telemetriedaten, der von einem anderen täglichen Auftrag abhängig ist, der die Ausgabe der letzten sieben Tage aggregiert und Datenströme von sieben Tagen des rollierenden Fensters generiert:

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency05.png)

### <a name="dependency-on-itself"></a>Abhängigkeit von sich selbst

Ein täglicher Auftrag ohne Lücken in den Ausgabedatenströmen des Auftrags:

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency06.png)

## <a name="monitor-dependencies"></a>Überwachen von Abhängigkeiten

Sie können die Abhängigkeitskette und die entsprechenden Fenster auf der Seite für die Überwachung von Triggerausführungen überwachen. Navigieren Sie zu **Überwachung > Triggerausführungen**.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency07.png)

Klicken Sie auf das Lupensymbol, um alle abhängigen Triggerausführungen des ausgewählten Fensters anzuzeigen.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency08.png)

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Erstellen eines Triggers für ein rollierendes Fenster](how-to-create-tumbling-window-trigger.md).