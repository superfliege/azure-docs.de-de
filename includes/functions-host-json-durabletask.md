---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 92eb13165326f44432f09322ea97f3cee5ccec2b
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251242"
---
Konfigurationseinstellungen für [Durable Functions](../articles/azure-functions/durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub",
    "ControlQueueBatchSize": 32,
    "PartitionCount": 4,
    "ControlQueueVisibilityTimeout": "00:05:00",
    "WorkItemQueueVisibilityTimeout": "00:05:00",
    "MaxConcurrentActivityFunctions": 10,
    "MaxConcurrentOrchestratorFunctions": 10,
    "AzureStorageConnectionStringName": "AzureWebJobsStorage",
    "TraceInputsAndOutputs": false,
    "LogReplayEvents": false,
    "EventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "EventGridKeySettingName":  "EventGridKey",
    "EventGridPublishRetryCount": 3,
    "EventGridPublishRetryInterval": "00:00:30"
  }
}
```

Aufgabenhubnamen müssen mit einem Buchstaben beginnen und bestehen nur aus Buchstaben und Ziffern. Wenn nicht angegeben, lautet der standardmäßige Aufgabenhubname für eine Funktionen-App **DurableFunctionsHub**. Weitere Informationen finden Sie unter [Aufgabenhubs in Durable Functions (Azure Functions)](../articles/azure-functions/durable-functions-task-hubs.md).

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternative Namen für [Aufgabenhubs](../articles/azure-functions/durable-functions-task-hubs.md) können zum Isolieren von mehreren Durable Functions-Anwendungen verwendet werden, auch wenn sie dasselbe Speicher-Back-End verwenden.|
|ControlQueueBatchSize|32|Die Anzahl der aus der Steuerelement-Warteschlange jeweils abzurufenden Nachrichten.|
|PartitionCount |4|Die Anzahl der Partitionen für die Steuerelement-Warteschlange. Kann ein positiver Integerwert zwischen 1 und 16 sein.|
|ControlQueueVisibilityTimeout |5 Minuten|Das Sichtbarkeitstimeout von aus der Steuerelement-Warteschlange entfernten Nachrichten.|
|WorkItemQueueVisibilityTimeout |5 Minuten|Das Sichtbarkeitstimeout von aus der Warteschlange für Arbeitsaufgaben (work item queue) entfernten Nachrichten.|
|MaxConcurrentActivityFunctions |Zehnfache Anzahl der Prozessoren auf dem aktuellen Computer|Die maximale Anzahl von Aktivitätsfunktionen, die gleichzeitig auf einer einzelnen Hostinstanz verarbeitet werden können.|
|MaxConcurrentOrchestratorFunctions |Zehnfache Anzahl der Prozessoren auf dem aktuellen Computer|Die maximale Anzahl von Orchestratorfunktionen, die gleichzeitig auf einer einzelnen Hostinstanz verarbeitet werden können.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|Der Name der App-Einstellung mit der Azure Storage-Verbindungszeichenfolge, die zum Verwalten der zugrunde liegenden Azure Storage-Ressourcen verwendet wird.|
|TraceInputsAndOutputs |false|Ein Wert, der angibt, ob die Eingaben und Ausgaben von Funktionsaufrufen nachverfolgt werden. Das Standardverhalten beim Nachverfolgen von Ereignissen zur Funktionsausführung besteht darin, die Anzahl der Bytes in die serialisierten Eingaben und Ausgaben für Funktionsaufrufe einzuschließen. Dies ermöglicht minimale Informationen zu den Eingaben und Ausgaben, ohne die Protokolle zu überfrachten oder irrtümlich vertrauliche Informationen in den Protokollen verfügbar zu machen. Wenn diese Eigenschaft auf TRUE festgelegt wird, werden bei der standardmäßigen Funktionsprotokollierung die gesamten Inhalte der Eingaben und Ausgaben der Funktionen protokolliert.|
|LogReplayEvents|false|Ein Wert, der angibt, ob Orchestrierungswiedergabeereignisse in Application Insights geschrieben werden.|
|EventGridTopicEndpoint ||Die URL eines benutzerdefinierten Azure Event Grid-Themenendpunkts. Wenn diese Eigenschaft festgelegt ist, werden Benachrichtigungsereignisse zum Orchestrierungslebenszyklus an diesem Endpunkt veröffentlicht. Diese Eigenschaft unterstützt die Auflösung von App-Einstellungen.|
|EventGridKeySettingName ||Der Name der App-Einstellung, die den Schlüssel für die Authentifizierung mit dem benutzerdefinierten Azure Event Grid-Thema unter `EventGridTopicEndpoint` enthält.|
|EventGridPublishRetryCount|0|Die Anzahl der Wiederholungsversuche, wenn bei der Veröffentlichung im Event Grid-Thema Fehler auftreten.|
|EventGridPublishRetryInterval|5 Minuten|Das Wiederholungsintervall der Event Grid-Veröffentlichung im Format *hh:mm:ss*.|

Viele werden zur Optimierung der Leistung verwendet. Weitere Informationen finden Sie unter [Leistung und Skalierbarkeit](../articles/azure-functions/durable-functions-perf-and-scale.md).