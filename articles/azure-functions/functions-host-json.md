---
title: host.json-Referenz für Azure Functions
description: Referenzdokumentation für die host.json-Datei von Azure Functions.
services: functions
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/12/2018
ms.author: tdykstra
ms.openlocfilehash: d89170f796355b734facc5e08ad1815a2b865d49
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342091"
---
# <a name="hostjson-reference-for-azure-functions"></a>host.json-Referenz für Azure Functions

Die Metadatendatei *host.json* enthält globale Konfigurationsoptionen, die sich auf alle Funktionen einer Funktionen-App auswirken. In diesem Artikel werden die verfügbaren Einstellungen aufgelistet. Das JSON-Schema finden Sie unter http://json.schemastore.org/host.

Es gibt andere globale Konfigurationsoptionen in [App-Einstellungen](functions-app-settings.md) und in der Datei [local.settings.json](functions-run-local.md#local-settings-file).

## <a name="sample-hostjson-file"></a>host.json-Beispieldatei

Für die folgende *host.json*-Beispieldatei sind alle möglichen Optionen angegeben.

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

In den folgenden Abschnitten dieses Artikels werden die einzelnen allgemeinen Eigenschaften erläutert. Alle Eigenschaften sind optional, sofern nicht anders angegeben.

## <a name="aggregator"></a>aggregator

Gibt an, wie viele Funktionsaufrufe aggregiert werden, wenn [Metriken für Application Insights berechnet werden](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Eigenschaft |Standard  | BESCHREIBUNG |
|---------|---------|---------| 
|batchSize|1000|Maximale Anzahl der zu aggregierenden Anforderungen.| 
|flushTimeout|00:00:30|Maximal zu aggregierender Zeitraum.| 

Funktionsaufrufe werden aggregiert, wenn der erste der beiden Grenzwerte erreicht wird.

## <a name="applicationinsights"></a>applicationInsights

Steuert das [Stichprobenfeature in Application Insights](functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|isEnabled|true|Aktiviert oder deaktiviert die Stichprobenentnahme.| 
|maxTelemetryItemsPerSecond|5|Der Schwellenwert, bei dem die Stichprobenentnahme beginnt.| 

## <a name="durabletask"></a>durableTask

Konfigurationseinstellungen für [Durable Functions](durable-functions-overview.md).

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

Aufgabenhubnamen müssen mit einem Buchstaben beginnen und bestehen nur aus Buchstaben und Ziffern. Wenn nicht angegeben, lautet der standardmäßige Aufgabenhubname für eine Funktionen-App **DurableFunctionsHub**. Weitere Informationen finden Sie unter [Aufgabenhubs in Durable Functions (Azure Functions)](durable-functions-task-hubs.md).

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternative Namen für [Aufgabenhubs](durable-functions-task-hubs.md) können zum Isolieren von mehreren Durable Functions-Anwendungen verwendet werden, auch wenn sie dasselbe Speicher-Back-End verwenden.|
|ControlQueueBatchSize|32|Die Anzahl der aus der Steuerelement-Warteschlange jeweils abzurufenden Nachrichten.|
|PartitionCount |4|Die Anzahl der Partitionen für die Steuerelement-Warteschlange. Kann ein positiver Integerwert zwischen 1 und 16 sein.|
|ControlQueueVisibilityTimeout |5 Minuten|Das Sichtbarkeitstimeout von aus der Steuerelement-Warteschlange entfernten Nachrichten.|
|WorkItemQueueVisibilityTimeout |5 Minuten|Das Sichtbarkeitstimeout von aus der Warteschlange für Arbeitsaufgaben (work item queue) entfernten Nachrichten.|
|MaxConcurrentActivityFunctions |Zehnfache Anzahl der Prozessoren auf dem aktuellen Computer|Die maximale Anzahl von Aktivitätsfunktionen, die gleichzeitig auf einer einzelnen Hostinstanz verarbeitet werden können.|
|MaxConcurrentOrchestratorFunctions |Zehnfache Anzahl der Prozessoren auf dem aktuellen Computer|Die maximale Anzahl von Aktivitätsfunktionen, die gleichzeitig auf einer einzelnen Hostinstanz verarbeitet werden können.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|Der Name der App-Einstellung mit der Azure Storage-Verbindungszeichenfolge, die zum Verwalten der zugrunde liegenden Azure Storage-Ressourcen verwendet wird.|
|TraceInputsAndOutputs |false|Ein Wert, der angibt, ob die Eingaben und Ausgaben von Funktionsaufrufen nachverfolgt werden. Das Standardverhalten beim Nachverfolgen von Ereignissen zur Funktionsausführung besteht darin, die Anzahl der Bytes in die serialisierten Eingaben und Ausgaben für Funktionsaufrufe einzuschließen. Dies ermöglicht minimale Informationen zu den Eingaben und Ausgaben, ohne die Protokolle zu überfrachten oder irrtümlich vertrauliche Informationen in den Protokollen verfügbar zu machen. Wenn diese Eigenschaft auf TRUE festgelegt wird, werden bei der standardmäßigen Funktionsprotokollierung die gesamten Inhalte der Eingaben und Ausgaben der Funktionen protokolliert.|
|LogReplayEvents|false|Ein Wert, der angibt, ob Orchestrierungswiedergabeereignisse in Application Insights geschrieben werden.|
|EventGridTopicEndpoint ||Die URL eines benutzerdefinierten Azure Event Grid-Themenendpunkts. Wenn diese Eigenschaft festgelegt ist, werden Benachrichtigungsereignisse zum Orchestrierungslebenszyklus an diesem Endpunkt veröffentlicht. Diese Eigenschaft unterstützt die Auflösung von App-Einstellungen.|
|EventGridKeySettingName ||Der Name der App-Einstellung, die den Schlüssel für die Authentifizierung mit dem benutzerdefinierten Azure Event Grid-Thema unter `EventGridTopicEndpoint` enthält.|
|EventGridPublishRetryCount|0|Die Anzahl der Wiederholungsversuche, wenn bei der Veröffentlichung im Event Grid-Thema Fehler auftreten.|
|EventGridPublishRetryInterval|5 Minuten|Das Wiederholungsintervall der Event Grid-Veröffentlichung im Format *hh:mm:ss*.|

Viele werden zur Optimierung der Leistung verwendet. Weitere Informationen finden Sie unter [Leistung und Skalierbarkeit](durable-functions-perf-and-scale.md).

## <a name="eventhub"></a>eventHub

Konfigurationseinstellungen für [Event Hub-Trigger und -Bindungen](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>functions

Eine Liste der Funktionen, die vom Auftragshost ausgeführt werden. Ein leeres Array bedeutet, dass alle Funktionen ausgeführt werden. Nur bei [lokaler Ausführung](functions-run-local.md) für die Verwendung vorgesehen. Verwenden Sie in Funktionen-Apps die *function.json* `disabled`-Eigenschaft anstelle dieser Eigenschaft in *host.json*.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Gibt die Timeoutdauer für alle Funktionen an. Im Verbrauchstarif liegt der gültige Bereich zwischen 1 Sekunde und 10 Minuten, wobei der Standardwert bei 5 Minuten liegt. In App Service-Plänen besteht keine Einschränkung und der Standardwert ist NULL, d. h. kein Timeout.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Konfigurationseinstellungen für [Host Health Monitor](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor) (Überwachung der Hostintegrität).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|Aktiviert|true|Gibt an, ob die Funktion aktiviert ist. | 
|healthCheckInterval|10 Sekunden|Das Zeitintervall zwischen den regelmäßigen Integritätsüberprüfungen im Hintergrund. | 
|healthCheckWindow|2 Minuten|Ein variables Zeitfenster, das in Zusammenhang mit der `healthCheckThreshold`-Einstellung verwendet wird.| 
|healthCheckThreshold|6|Maximale Anzahl von fehlerhaften Integritätsüberprüfungen, bevor ein Neustart des Hosts initiiert wird.| 
|counterThreshold|0,80|Der Schwellenwert, an dem ein Leistungsindikator als fehlerhaft betrachtet wird.| 

## <a name="http"></a>http

Konfigurationseinstellungen für [HTTP-Trigger und -Bindungen](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

Die eindeutige ID für einen Auftragshost. Die kann eine GUID in Kleinbuchstaben mit entfernten Bindestrichen sein. Dies ist für die lokale Ausführung erforderlich. Beim Ausführen in Azure Functions wird eine ID automatisch generiert, wenn `id` ausgelassen wird.

Wenn Sie ein Speicherkonto für mehrere Funktions-Apps verwenden, stellen Sie sicher, dass jede Funktions-App einen anderen Wert für `id` aufweist. Sie können die `id`-Eigenschaft auslassen oder `id` für jede Funktions-App manuell auf einen anderen Wert festlegen. Der Trigger mit Timer verwendet eine Speichersperre, um sicherzustellen, dass nur eine Timerinstanz vorhanden ist, wenn eine Funktions-App auf mehrere Instanzen horizontal hochskaliert wird. Wenn zwei Funktions-Apps denselben `id`-Wert aufweisen und beide einen Trigger mit Timer verwenden, wird nur ein Timer ausgeführt.


```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>Protokollierungstool

Steuerelemente, die nach Protokollen filtern, die von einem [ILogger-Objekt](functions-monitoring.md#write-logs-in-c-functions) oder von [context.log](functions-monitoring.md#write-logs-in-javascript-functions) geschrieben werden.

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|categoryFilter|–|Gibt die Filterung nach Kategorie an.| 
|defaultLevel|Information|Für alle nicht im `categoryLevels`-Array angegebenen Kategorien, werden Protokolle auf dieser und darüber liegenden Ebenen an Application Insights gesendet.| 
|categoryLevels|–|Ein Array von Kategorien, das den minimalen Protokolliergrad angibt, der für die einzelnen Kategorien an Application Insights gesendet wird. Die hier angegebene Kategorie steuert alle Kategorien, die mit demselben Wert beginnen und längere Werte haben Vorrang. In der vorhergehenden *host.json*-Beispieldatei werden alle Kategorien, die mit „Host.Aggregator“ beginnen, auf der `Information`-Ebene protokolliert. Alle anderen Kategorien, die mit „Host“ beginnen, z. B. „Host.Executor“, werden auf der `Error`-Ebene protokolliert.| 

## <a name="queues"></a>queues

Konfigurationseinstellungen für [Trigger und Bindungen der Speicherwarteschlange](functions-bindings-storage-queue.md).

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="servicebus"></a>serviceBus

Konfigurationseinstellung für [Service Bus-Trigger und -Bindungen](functions-bindings-service-bus.md).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>singleton

Konfigurationseinstellungen für das Singleton-Sperrverhalten. Weitere Informationen finden Sie unter [GitHub-Problem zur Singleton-Unterstützung](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|lockPeriod|00:00:15|Der Zeitraum, für den Sperren auf Funktionsebene gelten. Die Sperren werden automatisch verlängert.| 
|listenerLockPeriod|00:01:00|Der Zeitraum, für den Listenersperren gelten.| 
|listenerLockRecoveryPollingInterval|00:01:00|Das Zeitintervall für die Wiederherstellung der Listenersperre, wenn eine Listenersperre nicht beim Start abgerufen werden konnte.| 
|lockAcquisitionTimeout|00:01:00|Die maximale Zeitspanne, in der die Laufzeit versucht, eine Sperre abzurufen.| 
|lockAcquisitionPollingInterval|–|Das Intervall zwischen den Versuchen, eine Sperre abzurufen.| 

## <a name="tracing"></a>tracing

Konfigurationseinstellungen für Protokolle, die Sie mithilfe eines `TraceWriter`-Objekts erstellen. Weitere Informationen finden Sie unter [C#-Protokollierung](functions-reference-csharp.md#logging) und [Node.js-Protokollierung](functions-reference-node.md#writing-trace-output-to-the-console). 

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|consoleLevel|info|Die Ablaufverfolgungsebene für die Konsolenprotokollierung. Optionen sind: `off`, `error`, `warning`, `info` und `verbose`.|
|fileLoggingMode|debugOnly|Die Ablaufverfolgungsebene für die Dateiprotokollierung. Optionen sind `never`, `always`, `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Eine Reihe von [Verzeichnissen mit freigegebenem Code](functions-reference-csharp.md#watched-directories), die auf Änderungen überwacht werden sollte.  Dadurch wird sichergestellt, dass Änderungen am Code in diesen Verzeichnissen von Ihren Funktionen übernommen werden.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Aktualisieren der host.json-Datei](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Weitere Informationen finden Sie unter den globalen Einstellungen in Umgebungsvariablen](functions-app-settings.md)
