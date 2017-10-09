---
title: Systemvariablen in Azure Data Factory | Microsoft-Dokumentation
description: "Dieser Artikel beschreibt die Systemvariablen, die von Azure Data Factory unterstützt werden. Sie können diese Variablen in Ausdrücken verwenden, wenn Sie Data Factory-Entitäten definieren."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: shlo
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: dbf0a12ec1a5348e7c73248cc389ec1a5a5d4a75
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="system-variables-supported-by-azure-data-factory"></a>Von Azure Data Factory unterstützte Systemvariablen
Dieser Artikel beschreibt die Systemvariablen, die von Azure Data Factory unterstützt werden. Sie können diese Variablen in Ausdrücken verwenden, wenn Sie Data Factory-Entitäten definieren. 

## <a name="pipeline-scope"></a>Pipelinebereich:

| Variablenname | Beschreibung |
| --- | --- |
| @pipeline().DataFactory |Name der Data Factory, in der die Pipelineausführung ausgeführt wird | 
| @pipeline().Pipeline |Name der Pipeline |
| @pipeline().RunId | ID der jeweiligen Pipelineausführung | 
| @pipeline().TriggerType | Typ des Triggers, der die Pipeline aufgerufen hat (manuell, Planer) | 
| @pipeline().TriggerId| ID des Triggers, der die Pipeline aufruft |
| @pipeline().TriggerName| Name des Triggers, der die Pipeline aufruft |
| @pipeline().TriggerTime| Zeit, zu der der Trigger die Pipeline aufgerufen hat. Die Triggerzeit ist die Zeit, zu der die Pipeline tatsächlich ausgelöst wurde, nicht die geplante Zeit. Beispiel: `13:20:08.0149599Z` wird anstelle von `13:20:00.00Z` zurückgegeben |

## <a name="trigger-scope"></a>Triggerbereich:

| Variablenname | Beschreibung |
| --- | --- |
| trigger().scheduledTime |Zeit, zu der der Trigger die Pipelineausführung planmäßig aufrufen sollte. Beispiel: Für einen Trigger, der alle 5 Minuten ausgelöst wird, würde diese Variable `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z` bzw. `2017-06-01T22:29:00Z` zurückgeben.|
| trigger().startTime |Zeit, zu der der Trigger **tatsächlich** ausgelöst wurde, um die Pipelineausführung aufzurufen. Beispiel: Für einen Trigger, der alle 5 Minuten ausgelöst wird, könnte diese Variable Werte wie `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z` bzw. `2017-06-01T22:29:00.9935483Z` zurückgeben.|

## <a name="next-steps"></a>Nächste Schritte
Informationen darüber, wie diese Variablen in Ausdrücken verwendet werden, finden Sie unter [Ausdruckssprache und Funktionen](control-flow-expression-language-functions.md). 

