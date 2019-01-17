---
title: Systemvariablen in Azure Data Factory | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Systemvariablen, die von Azure Data Factory unterstützt werden. Sie können diese Variablen in Ausdrücken verwenden, wenn Sie Data Factory-Entitäten definieren.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 9a4d5acfe16a2fdbb3b631cb8baf6cb8e90a7d58
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016287"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Von Azure Data Factory unterstützte Systemvariablen
Dieser Artikel beschreibt die Systemvariablen, die von Azure Data Factory unterstützt werden. Sie können diese Variablen in Ausdrücken verwenden, wenn Sie Data Factory-Entitäten definieren.

## <a name="pipeline-scope"></a>Bereich „Pipeline“
Auf diese Systemvariablen kann überall im Pipeline-JSON verwiesen werden.

| Variablenname | BESCHREIBUNG |
| --- | --- |
| @pipeline().DataFactory |Name der Data Factory, in der die Pipelineausführung ausgeführt wird |
| @pipeline().Pipeline |Name der Pipeline |
| @pipeline().RunId | ID der jeweiligen Pipelineausführung |
| @pipeline().TriggerType | Typ des Triggers, der die Pipeline aufgerufen hat (manuell, Planer) |
| @pipeline().TriggerId| ID des Triggers, der die Pipeline aufruft |
| @pipeline().TriggerName| Name des Triggers, der die Pipeline aufruft |
| @pipeline().TriggerTime| Zeit, zu der der Trigger die Pipeline aufgerufen hat. Die Triggerzeit ist die Zeit, zu der die Pipeline tatsächlich ausgelöst wurde, nicht die geplante Zeit. Beispiel: `13:20:08.0149599Z` wird anstelle von `13:20:00.00Z` zurückgegeben |

## <a name="schedule-trigger-scope"></a>Bereich „ScheduleTrigger“
Auf diese Systemvariablen kann überall im Trigger-JSON verwiesen werden, wenn der Trigger den Typ „ScheduleTrigger“ hat.

| Variablenname | BESCHREIBUNG |
| --- | --- |
| @trigger().scheduledTime |Zeit, zu der der Trigger die Pipelineausführung planmäßig aufrufen sollte. Beispiel: Für einen Trigger, der alle 5 Minuten ausgelöst wird, würde diese Variable `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z` bzw. `2017-06-01T22:29:00Z` zurückgeben.|
| @trigger().startTime |Zeit, zu der der Trigger **tatsächlich** ausgelöst wurde, um die Pipelineausführung aufzurufen. Beispiel: Für einen Trigger, der alle 5 Minuten ausgelöst wird, könnte diese Variable Werte wie `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z` bzw. `2017-06-01T22:29:00.9935483Z` zurückgeben.|

## <a name="tumbling-window-trigger-scope"></a>Bereich „TumblingWindowTrigger“
Auf diese Systemvariablen kann überall im Trigger-JSON verwiesen werden, wenn der Trigger den Typ „TumblingWindowTrigger“ hat.

| Variablenname | BESCHREIBUNG |
| --- | --- |
| @trigger().outputs.windowStartTime |Start des Zeitfensters, zu dem der Trigger die Pipelineausführung planmäßig aufrufen sollte. Wenn der rollierende Fenstertrigger die Frequenz „hourly“ (stündlich) hat, ist dies die Uhrzeit am Anfang der Stunde.|
| @trigger().outputs.windowEndTime |Ende des Zeitfensters, zu dem der Trigger die Pipelineausführung planmäßig aufrufen sollte. Wenn der rollierende Fenstertrigger die Frequenz „hourly“ (stündlich) hat, ist dies die Uhrzeit am Ende der Stunde.|
## <a name="next-steps"></a>Nächste Schritte
Informationen darüber, wie diese Variablen in Ausdrücken verwendet werden, finden Sie unter [Ausdruckssprache und Funktionen](control-flow-expression-language-functions.md).
