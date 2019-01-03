---
title: host.json-Referenz für Azure Functions 1.x
description: Referenzdokumentation für die host.json-Datei von Azure Functions mit der v1 Runtime.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: glenga
ms.openlocfilehash: ee82aab37973117b0c1960d8b75a29bfad38b7c7
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252072"
---
# <a name="hostjson-reference-for-azure-functions-1x"></a>host.json-Referenz für Azure Functions 1.x

> [!div class="op_single_selector" title1="Wählen Sie die Version der Laufzeitumgebung Azure Functions aus: "]
> * [Version 1](functions-host-json-v1.md)
> * [Version 2](functions-host-json.md)

Die Metadatendatei *host.json* enthält globale Konfigurationsoptionen, die sich auf alle Funktionen einer Funktionen-App auswirken. In diesem Artikel werden die verfügbaren Einstellungen für die v1 Runtime aufgelistet. Das JSON-Schema finden Sie unter http://json.schemastore.org/host.

> [!NOTE]
> Dieser Artikel gilt für Azure Functions 1.x.  Eine Referenz für „host.json“ in Functions 2.x finden Sie unter [host.json-Referenz für Azure Functions 2.x](functions-host-json.md).

Weitere Konfigurationsoptionen Ihrer Funktions-App werden in den [Anwendungseinstellungen](functions-app-settings.md) verwaltet.

Einige Einstellungen in host.json werden nur bei lokaler Ausführung in der [local.settings.json](functions-run-local.md#local-settings-file)-Datei verwendet.

## <a name="sample-hostjson-file"></a>host.json-Beispieldatei

Für die folgenden *host.json*-Beispieldateien sind alle möglichen Optionen angegeben.


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

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>applicationInsights

[!INCLUDE [applicationInsights](../../includes/functions-host-json-applicationinsights.md)]

## <a name="durabletask"></a>durableTask

[!INCLUDE [durabletask](../../includes/functions-host-json-durabletask.md)]

## <a name="eventhub"></a>eventHub

Konfigurationseinstellungen für [Event Hub-Trigger und -Bindungen](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>functions

Eine Liste der Funktionen, die vom Auftragshost ausgeführt werden. Ein leeres Array bedeutet, dass alle Funktionen ausgeführt werden. Nur bei [lokaler Ausführung](functions-run-local.md) für die Verwendung vorgesehen. In Funktions-Apps in Azure sollten Sie anstelle dieser Einstellung die in [How to disable functions in Azure Functions](disable-function.md) (Deaktivieren von Funktionen in Azure Functions) aufgeführten Schritte ausführen, um bestimmte Funktionen zu deaktivieren.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Gibt die Timeoutdauer für alle Funktionen an. Bei einem serverlosen Verbrauchsplan liegt der gültige Bereich zwischen 1 Sekunde und 10 Minuten, wobei der Standardwert bei 5 Minuten liegt. Bei einem App Service-Plan gibt es keine allgemeine Beschränkung, und der Standardwert hängt von der Version der Runtime ab.

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

*Nur Version 1.x.*

Die eindeutige ID für einen Auftragshost. Die kann eine GUID in Kleinbuchstaben mit entfernten Bindestrichen sein. Dies ist für die lokale Ausführung erforderlich. Bei der Ausführung in Azure empfehlen wir, keinen ID-Wert festzulegen. Wenn `id` nicht angegeben ist, wird in Azure automatisch eine ID generiert. 

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

*Version 1.x*

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
