---
title: Bindungen des SignalR-Diensts für Azure Functions
description: Erfahren Sie, wie Bindungen des SignalR-Diensts in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: anthonychu
manager: cfowler
editor: ''
tags: ''
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/23/2018
ms.author: antchu
ms.openlocfilehash: 2892481dca9ce62d96e954656341925b4c8110f9
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802014"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Bindungen des SignalR-Diensts für Azure Functions

In diesem Artikel wird erläutert, wie mit dem [Azure SignalR-Dienst](https://azure.microsoft.com/services/signalr-service/) verbundene Clients mithilfe von Bindungen des SignalR-Diensts in Azure Functions authentifiziert und Nachrichten in Echtzeit an sie gesendet werden. Azure Functions unterstützt Eingabe- und Ausgabebindungen für den SignalR-Dienst.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Pakete: Functions 2.x

Die SignalR-Dienst-Bindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.SignalRService](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService), Version 1.0.0-preview1-* bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-functions-signalrservice-extension](https://github.com/Azure/azure-functions-signalrservice-extension).

> [!NOTE]
> Der Azure SignalR-Dienst ist allgemein verfügbar. Die Bindungen des SignalR-Diensts für Azure Functions befinden sich aktuell jedoch in der Vorschau.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]

## <a name="signalr-connection-info-input-binding"></a>SignalR-Verbindungsinfo-Eingabebindung

Bevor ein Client eine Verbindung mit dem Azure SignalR-Dienst herstellen kann, muss er die Endpunkt-URL des Diensts und ein gültiges Zugriffstoken abrufen. Die *SignalRConnectionInfo*-Eingabebindung erzeugt die Endpunkt-URL des SignalR-Diensts und ein gültiges Token. Beide werden verwendet, um die Verbindung mit dem Dienst herzustellen. Da das Token zeitlich begrenzt ist und für die Authentifizierung eines bestimmten Benutzers gegenüber einer Verbindung verwendet werden kann, sollten Sie das Token nicht zwischenspeichern oder es für mehrere Clients verwenden. Ein HTTP-Trigger, der diese Bindung verwendet, kann von Clients dazu verwendet werden, die Verbindungsinformationen abzurufen.

Sehen Sie sich das sprachspezifische Beispiel an:

* [2.x – C#](#2x-c-input-example)
* [2.x – JavaScript](#2x-javascript-input-example)

### <a name="2x-c-input-example"></a>C#-Eingabebeispiel für 2.x

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die SignalR-Verbindungsinformationen mithilfe der Eingabebindung erwirbt und sie über HTTP zurückgibt.

```cs
[FunctionName("GetSignalRInfo")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Authentifizierte Token

Wenn die Funktion von einem authentifizierten Client ausgelöst wird, können Sie dem erzeugten Token einen Benutzer-ID-Anspruch hinzufügen. Mithilfe von [App Service-Authentifizierung] (../app-service/app-service-authentication-overview.md) können Sie einer Funktions-App problemlos Authentifizierung hinzufügen.

App Service-Authentifizierung legt HTTP-Header mit den Namen `x-ms-client-principal-id` und `x-ms-client-principal-name` fest, die die Clientprinzipal-ID bzw. den Namen des authentifizierten Benutzers enthalten. Sie können die `UserId`-Eigenschaft der Bindung mithilfe eines [Bindungsausdrucks](functions-triggers-bindings.md#binding-expressions-and-patterns) auf den Wert eines der beiden Header festlegen: `{headers.x-ms-client-principal-id}` oder `{headers.x-ms-client-principal-name}`. 

```cs
[FunctionName("GetSignalRInfo")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

### <a name="2x-javascript-input-example"></a>JavaScript-Eingabebeispiel für 2.x

Das folgende Beispiel zeigt eine SignalR-Verbindungsinfo-Eingabebindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet, um die Verbindungsinformationen zurückzugeben.

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, req, connectionInfo) {
    context.res = { body: connectionInfo };
    context.done();
};
```

#### <a name="authenticated-tokens"></a>Authentifizierte Token

Wenn die Funktion von einem authentifizierten Client ausgelöst wird, können Sie dem erzeugten Token einen Benutzer-ID-Anspruch hinzufügen. Mithilfe von [App Service-Authentifizierung] (../app-service/app-service-authentication-overview.md) können Sie einer Funktions-App problemlos Authentifizierung hinzufügen.

App Service-Authentifizierung legt HTTP-Header mit den Namen `x-ms-client-principal-id` und `x-ms-client-principal-name` fest, die die Clientprinzipal-ID bzw. den Namen des authentifizierten Benutzers enthalten. Sie können die `userId`-Eigenschaft der Bindung mithilfe eines [Bindungsausdrucks](functions-triggers-bindings.md#binding-expressions-and-patterns) auf den Wert eines der beiden Header festlegen: `{headers.x-ms-client-principal-id}` oder `{headers.x-ms-client-principal-name}`. 

Beispiel für „function.json“:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier 
    // claim set to the authenticated user
    context.res = { body: connectionInfo };
    context.done();
};
```

## <a name="signalr-output-binding"></a>SignalR-Ausgabebindung

Verwenden Sie die *SignalR*-Ausgabebindung, um eine oder mehrere Nachrichten mithilfe des Azure SignalR-Diensts zu senden. Sie können eine Nachricht als Broadcast an alle verbundenen Clients senden oder den Broadcast auf verbundene Clients beschränken, die für einem bestimmten Benutzer authentifiziert wurden.

Sehen Sie sich das sprachspezifische Beispiel an:

* [2.x – C#](#2x-c-output-example)
* [2.x – JavaScript](#2x-javascript-output-example)

### <a name="2x-c-output-example"></a>C#-Ausgabebeispiel für 2.x

#### <a name="broadcast-to-all-clients"></a>Broadcast an alle Clients

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine Nachricht mithilfe der Ausgabebindung an alle verbundenen Clients sendet. Das `Target` ist der Name der Methode, die auf den einzelnen Clients aufgerufen werden soll. Die `Arguments`-Eigenschaft ist ein Array von null oder mehr Objekten, die an die Clientmethode übergeben werden sollen.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

#### <a name="send-to-a-user"></a>Senden an einen Benutzer

Sie können eine Nachricht ausschließlich an Verbindungen senden, die für einen Benutzer authentifiziert wurden, indem Sie die `UserId`-Eigenschaft der SignalR-Nachricht festlegen.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will only be sent to these user IDs
            UserId = "userId1",
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

### <a name="2x-javascript-output-example"></a>JavaScript-Ausgabebeispiel für 2.x

#### <a name="broadcast-to-all-clients"></a>Broadcast an alle Clients

Das folgende Beispiel zeigt eine SignalR-Ausgabebindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet, um mit dem Azure SignalR-Dienst eine Nachricht zu senden. Legen Sie die Ausgabebindung auf ein Array von einer oder mehreren SignalR-Nachrichten fest. Eine SignalR-Nachricht besteht aus einer `target`-Eigenschaft, die den Namen der auf jedem Client aufzurufenden Methode angibt, und einer `arguments`-Eigenschaft, bei der es sich um ein Array von Objekten handelt, die der Clientmethode als Argumente übergeben werden sollen.

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
    context.done();
};
```

#### <a name="send-to-a-user"></a>Senden an einen Benutzer

Sie können eine Nachricht ausschließlich an Verbindungen senden, die für einen Benutzer authentifiziert wurden, indem Sie die `userId`-Eigenschaft der SignalR-Nachricht festlegen.

*function.json* bleibt unverändert. Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to these user IDs
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
    context.done();
};
```

## <a name="configuration"></a>Konfiguration

### <a name="signalrconnectioninfo"></a>SignalRConnectionInfo

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `SignalRConnectionInfo` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**|| Muss auf `signalRConnectionInfo` festgelegt sein.|
|**direction**|| Muss auf `in` festgelegt sein.|
|**name**|| Variablenname, der im Funktionscode für das Verbindungsinfoobjekt verwendet wird. |
|**hubName**|**HubName**| Dieser Wert muss auf den Namen des SignalR-Hubs festgelegt werden, für den die Verbindungsinformationen erzeugt werden.|
|**userId**|**UserId**| Optional: Der Wert des Benutzer-ID-Anspruchs, der im Zugriffsschlüsseltoken festgelegt werden soll. |
|**connectionStringSetting**|**ConnectionStringSetting**| Der Name der Anwendungseinstellung, die die Verbindungszeichenfolge des SignalR-Diensts enthält (standardmäßig „AzureSignalRConnectionString“) |

### <a name="signalr"></a>SignalR

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `SignalR` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**|| Muss auf `signalR` festgelegt sein.|
|**direction**|| Muss auf `out` festgelegt sein.|
|**name**|| Variablenname, der im Funktionscode für das Verbindungsinfoobjekt verwendet wird. |
|**hubName**|**HubName**| Dieser Wert muss auf den Namen des SignalR-Hubs festgelegt werden, für den die Verbindungsinformationen erzeugt werden.|
|**connectionStringSetting**|**ConnectionStringSetting**| Der Name der Anwendungseinstellung, die die Verbindungszeichenfolge des SignalR-Diensts enthält (standardmäßig „AzureSignalRConnectionString“) |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)

