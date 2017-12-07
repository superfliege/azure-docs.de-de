---
title: "Trigger mit Timer für Azure Functions"
description: Erfahren Sie, wie Trigger mit Timer in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: tdykstra
ms.custom: 
ms.openlocfilehash: fd9c1d40ba1398c7ca3f48f0423457482da9a483
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="timer-trigger-for-azure-functions"></a>Trigger mit Timer für Azure Functions 

Dieser Artikel erläutert das Arbeiten mit Triggern mit Timer in Azure Functions. Mit einem Trigger mit Timer können Sie eine Funktion nach einem Zeitplan ausführen. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [Vorkompilierter C#-Code](#trigger---c-example)
* [C#-Skript](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="c-example"></a>C#-Beispiel

Das folgende Beispiel zeigt eine [vorkompilierte C#-Funktion](functions-dotnet-class-library.md), die alle fünf Minuten ausgeführt wird:

```cs
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

### <a name="c-script-example"></a>C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Triggerbindung mit Timer in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion schreibt ein Protokoll, das angibt, ob dieser Funktionsaufruf aufgrund eines versäumten Zeitplantermins erfolgt.

Bindungsdaten in der Datei *function.json*:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Der C#-Skriptcode sieht wie folgt aus:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

### <a name="f-example"></a>F#-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung mit Timer in einer Datei *function.json* sowie eine [F#-Skriptfunktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion schreibt ein Protokoll, das angibt, ob dieser Funktionsaufruf aufgrund eines versäumten Zeitplantermins erfolgt.

Bindungsdaten in der Datei *function.json*:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Der F#-Skriptcode sieht wie folgt aus:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

### <a name="javascript-example"></a>JavaScript-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung mit Timer in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion schreibt ein Protokoll, das angibt, ob dieser Funktionsaufruf aufgrund eines versäumten Zeitplantermins erfolgt.

Bindungsdaten in der Datei *function.json*:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Der JavaScript-Skriptcode sieht wie folgt aus:

```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="attributes"></a>Attribute

Verwenden Sie für Funktionen mit [vorkompiliertem C#-Code](functions-dotnet-class-library.md) das im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) definierte Attribut [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs).

Der Konstruktor des Attributs nimmt einen CRON-Ausdruck an, wie das folgende Beispiel zeigt:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
   ...
}
 ```

Sie können `TimeSpan` anstelle eines CRON-Ausdrucks angeben, wenn die Funktions-App für einen App Service-Plan (keinen Verbrauchsplan) ausgeführt wird.

Ein vollständiges Beispiel finden Sie unter [Vorkompiliertes C#-Beispiel](#c-example).

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `TimerTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |Beschreibung|
|---------|---------|----------------------|
|**type** | – | Muss auf „timerTrigger“ festgelegt werden. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | – | Muss auf „in“ festgelegt werden. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | – | Der Name der Variablen, die das Timerobjekt im Funktionscode darstellt. | 
|**schedule**|**ScheduleExpression**|Im Verbrauchsplan können Sie Zeitpläne mit einem CRON-Ausdruck definieren. Wenn Sie einen App Service-Plan verwenden, können Sie auch eine `TimeSpan`-Zeichenfolge verwenden. In den folgenden Abschnitten werden CRON-Ausdrücke erläutert. Sie können den Zeitplanausdruck in eine App-Einstellung einfügen und diese Eigenschaft auf einen Wert festlegen, der wie in diesem Beispiel **%**-Zeichen als Wrapper verwendet: „%NameOfAppSettingWithCRONExpression%“. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="cron-format"></a>CRON-Format 

Ein [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression) für den Azure Functions-Trigger mit Timer enthält die folgenden sechs Felder: 

```
{second} {minute} {hour} {day} {month} {day-of-week}
```

>[!NOTE]   
>In vielen der CRON-Ausdrücke, die Sie online finden, wird das `{second}`-Feld ausgelassen. Wenn Sie einen dieser Ausdrücke kopieren, fügen das fehlende `{second}`-Feld hinzu.

### <a name="cron-time-zones"></a>CRON-Zeitzonen

Als Standardzeitzone wird in Verbindung mit den CRON-Ausdrücken die Coordinated Universal Time (UTC) verwendet. Wenn Sie möchten, dass Ihr CRON-Ausdruck auf einer anderen Zeitzone basiert, erstellen Sie eine neue App-Einstellung für die Funktionen-App mit dem Namen `WEBSITE_TIME_ZONE`. Legen Sie den Wert auf den Namen der gewünschten Zeitzone gemäß [Microsoft Time Zone Index](https://technet.microsoft.com/library/cc749073(v=ws.10).aspx) (Microsoft-Zeitzonenindex) fest. 

Beispiel: *Eastern Normalzeit* ist UTC-05:00. Wenn Sie Ihren Trigger mit Timer täglich um 10:00 Uhr EST auslösen möchten, verwenden Sie den folgenden CRON-Ausdruck, der die UTC-Zeitzone berücksichtigt:

```json
"schedule": "0 0 15 * * *",
``` 

Sie könnten auch eine neue App-Einstellung für die Funktions-App mit dem Namen `WEBSITE_TIME_ZONE` hinzufügen und den Wert auf **Eastern Standard Time** (Eastern Normalzeit) festlegen.  Dann könnte der folgende CRON-Ausdruck für 10:00 Uhr EST verwendet werden: 

```json
"schedule": "0 0 10 * * *",
``` 
### <a name="cron-examples"></a>CRON-Beispiele

Die folgenden Beispiele zeigen CRON-Ausdrücke, die Sie für den Trigger mit Timer in Azure Functions verwenden können. 

Alle fünf Minuten auslösen:

```json
"schedule": "0 */5 * * * *"
```

Einmal zu jeder vollen Stunde auslösen:

```json
"schedule": "0 0 * * * *",
```

Einmal alle zwei Stunden auslösen:

```json
"schedule": "0 0 */2 * * *",
```

Zwischen 9:00 und 17:00 Uhr jeweils einmal pro Stunde auslösen:

```json
"schedule": "0 0 9-17 * * *",
```

Täglich um 9:30 Uhr auslösen:

```json
"schedule": "0 30 9 * * *",
```

Werktags um 9:30 Uhr auslösen:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="usage"></a>Verwendung

Bei Aufruf einer Trigger-mit-Timer-Funktion wird das [Timerobjekt](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) an die Funktion übergeben. Der folgende JSON-Code ist eine Beispieldarstellung des Timerobjekts. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

## <a name="scale-out"></a>Horizontales Skalieren

Der Trigger mit Timer unterstützt das horizontale Hochskalieren mit mehreren Instanzen. Eine einzelne Instanz einer bestimmten Timerfunktion wird auf allen Instanzen ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zu einem Schnellstart navigieren, der einen Trigger mit Timer verwendet](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
