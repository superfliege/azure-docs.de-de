---
title: "Azure Functions – Twilio-Bindung"
description: Erfahren Sie, wie Twilio-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 52a45f1b67e3194739fe97daad56de2d3515dee3
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="twilio-binding-for-azure-functions"></a>Twilio-Bindung für Azure Functions

Dieser Artikel erläutert das Senden von Textnachrichten mithilfe von [Twilio](https://www.twilio.com/)-Bindungen in Azure Functions. Azure Functions unterstützt Ausgabebindungen für Twilio.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#c-example)
* [C#-Skript (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine Textnachricht sendet, wenn sie durch eine Warteschlangennachricht ausgelöst wird.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

Dieses Beispiel verwendet das `TwilioSms`-Attribut mit dem Rückgabewert der Methode. Alternativ können Sie das Attribut auch mit einem `out SMSMessage`-Parameter oder den Parametern `ICollector<SMSMessage>` oder `IAsyncCollector<SMSMessage>` verwenden.

### <a name="c-script-example"></a>C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Twilio-Ausgabebindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion verwendet einen `out`-Parameter, um eine Textnachricht zu senden.

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Der C#-Skriptcode:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

Sie können in asynchronem Code keine Ausgabeparameter verwenden. Ein Beispiel für asynchronen C#-Skriptcode:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="javascript-example"></a>JavaScript-Beispiel

Das folgende Beispiel zeigt eine Twilio-Ausgabebindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet.

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="attributes"></a>Attribute

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) verwenden Sie das im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) definierte Attribut [TwilioSms](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs).

Weitere Informationen zu Attributeigenschaften, die Sie konfigurieren können, finden Sie unter [Konfiguration](#configuration). Hier ist ein Beispiel für ein `TwilioSms`-Attribut in einer Methodensignatur:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(
    AccountSidSetting = "TwilioAccountSid", 
    AuthTokenSetting = "TwilioAuthToken", 
    From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    ...
}
 ```

Ein vollständiges Beispiel finden Sie unter [C#-Beispiel](#c-example).

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `TwilioSms` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**|| auf `twilioSms` festgelegt werden muss.|
|**direction**|| auf `out` festgelegt werden muss.|
|**name**|| Variablenname, der im Funktionscode für die Twilio-SMS-Textnachricht verwendet wird |
|**accountSid**|**AccountSid**| Dieser Wert muss auf den Namen einer App-Einstellung festgelegt werden, die Ihre Twilio-Konto-SID enthält.|
|**authToken**|**AuthToken**| Dieser Wert muss auf den Namen einer App-Einstellung festgelegt werden, die Ihr Twilio-Authentifizierungstoken enthält.|
|**to**|**An**| Dieser Wert wird auf die Telefonnummer festgelegt, an die die SMS-Textnachricht gesendet wird.|
|**from**|**From**| Dieser Wert wird auf die Telefonnummer festgelegt, von der die SMS-Textnachricht gesendet wird.|
|**body**|**Text**| Dieser Wert kann verwendet werden, um die SMS-Textnachricht als vordefinierten Code festzulegen und nicht dynamisch im Code für die Funktion. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)


