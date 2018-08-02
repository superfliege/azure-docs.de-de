---
title: SendGrid-Bindungen in Azure Functions
description: Referenz für SendGrid-Bindungen in Azure Functions
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/29/2017
ms.author: glenga
ms.openlocfilehash: 2796a7d9fb44a05e1873e2fcbde82c837db202be
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346452"
---
# <a name="azure-functions-sendgrid-bindings"></a>SendGrid-Bindungen in Azure Functions

Dieser Artikel erläutert das Senden von E-Mail-Nachrichten mithilfe von [SendGrid](https://sendgrid.com/docs/User_Guide/index.html)-Bindungen in Azure Functions. Azure Functions bietet eine Ausgabebindung für SendGrid.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakete: Functions 1.x

Die SendGrid-Bindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.SendGrid](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid), Version 2.x, bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.SendGrid/).

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakete: Functions 2.x

Die SendGrid-Bindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.SendGrid](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid), Version 3.x, bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#c-example)
* [C#-Skript (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die einen Service Bus-Warteschlangentrigger und eine SendGrid-Ausgabebindung verwendet.

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

Sie müssen die `ApiKey`-Eigenschaft des Attributs nicht festlegen, wenn Ihr API-Schlüssel in einer App-Einstellung mit dem Namen „AzureWebJobsSendGridApiKey“ enthalten ist.

### <a name="c-script-example"></a>C#-Skriptbeispiel

Das folgende Beispiel zeigt eine SendGrid-Ausgabebindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet.

Bindungsdaten in der Datei *function.json*:

```json 
{
    "bindings": [
        {
            "name": "message",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#configuration).

Der C#-Skriptcode sieht wie folgt aus:

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

### <a name="javascript-example"></a>JavaScript-Beispiel

Das folgende Beispiel zeigt eine SendGrid-Ausgabebindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet.

Bindungsdaten in der Datei *function.json*:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};
```

## <a name="attributes"></a>Attribute

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das Attribut [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs).

Weitere Informationen zu Attributeigenschaften, die Sie konfigurieren können, finden Sie unter [Konfiguration](#configuration). Hier ist ein Beispiel für ein `SendGrid`-Attribut in einer Methodensignatur:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter [C#-Beispiel](#c-example).

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `SendGrid` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**|| Erforderlich – muss auf `sendGrid` festgelegt sein.|
|**direction**|| Erforderlich – muss auf `out` festgelegt sein.|
|**name**|| Erforderlich – der Variablenname, der im Funktionscode für die Anforderung oder den Anforderungstext verwendet wird. Dieser Wert ist ```$return```, wenn es nur einen Rückgabewert gibt. |
|**apiKey**|**ApiKey**| Der Name einer App-Einstellung, die Ihren API-Schlüssel enthält. Wenn sie nicht festgelegt wird, lautet der Standardname der App-Einstellung „AzureWebJobsSendGridApiKey“.|
|**to**|**An**| E-Mail-Adresse des Empfängers |
|**from**|**From**| E-Mail-Adresse des Absenders |
|**subject**|**Betreff**| Betreff der E-Mail |
|**text**|**Text**| Inhalt der E-Mail |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
