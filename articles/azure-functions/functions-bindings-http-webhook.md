---
title: HTTP- und Webhookbindungen in Azure Functions
description: Erfahren Sie, wie HTTP- und Webhooktrigger und -Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Computing, serverlose Architektur, HTTP, API, REST
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: mahender
ms.openlocfilehash: 01f845e0cb987eb4e4e9baa62478d3ff6991fb7e
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2018
---
# <a name="azure-functions-http-and-webhook-bindings"></a>HTTP- und Webhookbindungen in Azure Functions

Dieser Artikel erläutert das Arbeiten mit HTTP-Bindungen in Azure Functions. Azure Functions unterstützt HTTP-Trigger und -Ausgabebindungen.

Ein HTTP-Trigger kann so angepasst werden, dass er auf [Webhooks](https://en.wikipedia.org/wiki/Webhook) antwortet. Ein Webhooktrigger akzeptiert nur eine JSON-Nutzlast und überprüft den JSON-Code. Es gibt spezielle Versionen des Webhooktriggers, die das Verarbeiten von Webhooks von bestimmten Anbietern wie GitHub oder Slack vereinfachen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="trigger"></a>Trigger

Mit dem HTTP-Trigger können Sie eine Funktion mit einer HTTP-Anforderung aufrufen. Sie können einen HTTP-Trigger zum Erstellen von serverlosen APIs und zum Antworten auf Webhooks verwenden. 

Standardmäßig antwortet ein HTTP-Trigger auf die Anforderung mit dem Statuscode „HTTP 200 OK“ und einem leeren Text. Um diese Antwort zu ändern, konfigurieren Sie eine [HTTP-Ausgabebindung](#http-output-binding).

## <a name="trigger---example"></a>Trigger: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#trigger---c-example)
* [C#-Skript (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger: C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter sucht.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequestMessage req, 
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

### <a name="trigger---c-script-example"></a>Trigger: C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

C#-Skriptcode für die Bindung an `HttpRequestMessage`:

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

Sie können anstelle von `HttpRequestMessage` auch an ein benutzerdefiniertes Objekt binden. Dieses Objekt wird aus dem Text der Anforderung im JSON-Format erstellt. Analog dazu kann ein Typ an die Ausgabebindung der HTTP-Antwort übergeben und zusammen mit dem Statuscode 200 als Antworttext zurückgegeben werden.

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
}
```

### <a name="trigger---f-example"></a>Trigger: F#-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [F#-Funktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der F#-Code lautet wie folgt:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Sie benötigen eine Datei `project.json`, die wie im folgenden Beispiel gezeigt mithilfe von NuGet auf die Assemblys `FSharp.Interop.Dynamic` und `Dynamitey` verweist:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

### <a name="trigger---javascript-example"></a>Trigger: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```
     
## <a name="trigger---webhook-example"></a>Trigger: Webhookbeispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#webhook---c-example)
* [C#-Skript (.csx)](#webhook---c-script-example)
* [F#](#webhook---f-example)
* [JavaScript](#webhook---javascript-example)

### <a name="webhook---c-example"></a>Webhook: C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die HTTP 200 als Antwort auf eine generische JSON-Anforderung sendet.

```cs
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

### <a name="webhook---c-script-example"></a>Webhook: C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Webhook-Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion protokolliert GitHub-Kommentare zu Problemen.

Bindungsdaten in der Datei *function.json*:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

### <a name="webhook---f-example"></a>Webhook: F#-Beispiel

Das folgende Beispiel zeigt eine Webhook-Triggerbindung in einer Datei *function.json* sowie eine [F#-Funktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion protokolliert GitHub-Kommentare zu Problemen.

Bindungsdaten in der Datei *function.json*:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der F#-Code lautet wie folgt:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

### <a name="webhook---javascript-example"></a>Webhook: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Webhook-Triggerbindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion protokolliert GitHub-Kommentare zu Problemen.

Bindungsdaten in der Datei *function.json*:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="trigger---attributes"></a>Trigger: Attribute

Verwenden Sie in [C#-Bibliotheken](functions-dotnet-class-library.md) das im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) definierte [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs)-Attribut.

Sie können die Autorisierungsebene und die zulässigen HTTP-Methoden in den Konstruktorparametern für das Attribut festlegen. Außerdem gibt es Eigenschaften für den Webhooktyp und die Routenvorlage. Weitere Informationen zu diesen Einstellungen finden Sie unter [Trigger: Konfiguration](#trigger---configuration). Dies ist ein `HttpTrigger`-Attribut in einer Methodensignatur:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    ...
}
 ```

Ein vollständiges Beispiel finden Sie unter [Trigger: C#-Beispiel](#trigger---c-example).

## <a name="trigger---configuration"></a>Trigger: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `HttpTrigger` festlegen:


|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
| **type** | –| Erforderlich – muss auf `httpTrigger` festgelegt sein. |
| **direction** | –| Erforderlich – muss auf `in` festgelegt sein. |
| **name** | –| Erforderlich – der Variablenname, der im Funktionscode für die Anforderung oder den Anforderungstext verwendet wird. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Bestimmt, welche Schlüssel (sofern erforderlich) in der Anforderung vorhanden sein müssen, um die Funktion aufzurufen. Bei den Autorisierungsebenen kann es sich um einen der folgenden Werte handeln: <ul><li><code>anonymous</code>: Es ist kein API-Schlüssel erforderlich.</li><li><code>function</code>: Es ist ein funktionsspezifischer API-Schlüssel erforderlich. Dies ist der Standardwert, wenn kein anderer Wert angegeben wird.</li><li><code>admin</code>: Der Hauptschlüssel ist erforderlich.</li></ul> Weitere Informationen finden Sie im Abschnitt zu [Autorisierungsschlüsseln](#authorization-keys). |
| **methods** |**Methoden** | Ein Array der HTTP-Methoden, auf die diese Funktion antwortet. Wird dieses Array nicht angegeben, antwortet die Funktion auf alle HTTP-Methoden. Siehe [Anpassen des HTTP-Endpunkts](#trigger---customize-the-http-endpoint). |
| **route** | **Route** | Definiert die Routenvorlage, mit der gesteuert wird, auf welche Anforderungs-URLs die Funktion antwortet. Wenn kein anderer Wert angegeben wird, lautet der Standardwert `<functionname>`. Weitere Informationen finden Sie unter [Anpassen des HTTP-Endpunkts](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** |Konfiguriert den HTTP-Trigger so, dass er als [Webhookempfänger](https://en.wikipedia.org/wiki/Webhook) für den angegebenen Anbieter fungiert. Legen Sie die `methods` Eigenschaft nicht fest, wenn Sie diese Eigenschaft festlegen. Der Webhooktyp kann folgende Werte annehmen:<ul><li><code>genericJson</code>: Ein allgemeiner Webhookendpunkt ohne Logik für einen bestimmten Anbieter. Diese Einstellung beschränkt Anforderungen auf solche, die HTTP POST verwenden und den Inhaltstyp `application/json` aufweisen.</li><li><code>github</code>: Die Funktion antwortet auf [GitHub-Webhooks](https://developer.github.com/webhooks/). Verwenden Sie die _authLevel_-Eigenschaft nicht mit GitHub-Webhooks. Weitere Informationen finden Sie im Abschnitt zu GitHub-Webhooks weiter unten in diesem Artikel.</li><li><code>slack</code>: Die Funktion antwortet auf [Slack-Webhooks](https://api.slack.com/outgoing-webhooks). Verwenden Sie die _authLevel_-Eigenschaft nicht mit Slack-Webhooks. Weitere Informationen finden Sie im Abschnitt zu Slack-Webhooks weiter unten in diesem Artikel.</li></ul>|

## <a name="trigger---usage"></a>Trigger: Verwendung

Bei C#- und F#-Funktionen können Sie den Typ der Triggereingabe als `HttpRequestMessage` oder als benutzerdefinierten Typ deklarieren. Wenn Sie `HttpRequestMessage` auswählen, erhalten Sie Vollzugriff auf das Anforderungsobjekt. Bei einem benutzerdefinierten Typ versucht Functions, den JSON-Anforderungstext zu analysieren, um die Objekteigenschaften festzulegen. 

Für JavaScript-Funktionen stellt die Functions-Runtime den Anforderungstext anstelle des Anforderungsobjekts bereit. Weitere Informationen finden Sie im [JavaScript-Triggerbeispiel](#trigger---javascript-example).

### <a name="github-webhooks"></a>GitHub-Webhooks

Erstellen Sie zum Antworten auf GitHub-Webhooks zuerst eine Funktion mit einem HTTP-Trigger, und legen Sie die **webHookType**-Eigenschaft auf `github` fest. Kopieren Sie dann die URL und den API-Schlüssel auf die Seite **Webhook hinzufügen** Ihres GitHub-Repositorys. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Ein Beispiel finden Sie unter [Erstellen einer Funktion, die durch einen GitHub-Webhook ausgelöst wird](functions-create-github-webhook-triggered-function.md).

### <a name="slack-webhooks"></a>Slack-Webhooks

Der Slack-Webhook lässt Sie das Token nicht angeben, sondern generiert es für Sie. Daher müssen Sie einen funktionsspezifischen Schlüssel mit dem Token aus Slack konfigurieren. Weitere Informationen finden Sie unter [Autorisierungsschlüssel](#authorization-keys).

### <a name="customize-the-http-endpoint"></a>Anpassen des HTTP-Endpunkts

Wenn Sie eine Funktion für einen HTTP-Trigger oder einen Webhook erstellen, ist die Funktion mit einer Route der folgenden Form erreichbar:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Sie können diese Route mit der optionalen `route`-Eigenschaft in der Eingabebindung des HTTP-Triggers anpassen. In diesem Beispiel wird mit der Datei *function.json* eine `route`-Eigenschaft für einen HTTP-Trigger definiert:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Mit dieser Konfiguration ist die Funktion jetzt über die folgende Route erreichbar, anstatt über die ursprüngliche Route.

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

So kann der Funktionscode zwei Parameter in der Adresse unterstützen: _category_ und _id_. Sie können für Ihre Parameter eine beliebige [Web-API-Routeneinschränkung](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) verwenden. Im folgenden C#-Funktionscode werden beide Parameter verwendet.

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

Dies ist der Node.js-Funktionscode zur Verwendung der gleichen Routenparameter.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Standardmäßig verfügen alle Funktionsrouten über das Präfix *api*. Sie können das Präfix auch mit der `http.routePrefix`-Eigenschaft in der Datei [host.json](functions-host-json.md) anpassen oder entfernen. Im folgenden Beispiel wird das Routenpräfix *api* entfernt, indem in der Datei *host.json* eine leere Zeichenfolge als Präfix verwendet wird.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="authorization-keys"></a>Autorisierungsschlüssel

HTTP-Trigger ermöglichen die Verwendung von Schlüsseln zum Verbessern der Sicherheit. Ein standardmäßiger HTTP-Trigger kann diese als API-Schlüssel verwenden, sodass der Schlüssel in der Anforderung vorhanden sein muss. Webhooks können Schlüssel verwenden, um Anforderungen auf unterschiedliche Arten zu autorisieren, je nachdem, welche Methode vom Anbieter unterstützt wird.

> [!NOTE]
> Bei der lokalen Ausführung von Funktionen wird die Autorisierung unabhängig von der in `function.json` festgelegten `authLevel` deaktiviert. Mit der Veröffentlichung in Azure Functions wird `authLevel` sofort wirksam.

Schlüssel werden als Teil Ihrer Funktionen-App in Azure gespeichert und sind im Ruhezustand verschlüsselt. Um vorhandene Schlüssel anzuzeigen, neue Schlüssel zu erstellen oder Schlüsseln neue Werte zuzuweisen, navigieren Sie im Portal zu einer Ihrer Funktionen, und wählen Sie „Verwalten“ aus. 

Es gibt zwei Arten von Schlüsseln:

- **Hostschlüssel:** Diese Schlüssel werden von allen Funktionen innerhalb der Funktionen-App gemeinsam genutzt. Bei Verwendung als API-Schlüssel ermöglichen diese Zugriff auf jede Funktion in der Funktionen-App.
- **Funktionsschlüssel**: Diese Schlüssel gelten nur für die Funktionen, für die sie definiert sind. Bei Verwendung als API-Schlüssel ermöglichen diese nur Zugriff auf diese spezielle Funktion.

Jeder Schlüssel ist zu Referenzzwecken benannt. Auf Funktions- und Hostebene gibt es einen Standardschlüssel (mit dem Namen „default“). Funktionsschlüssel haben Vorrang vor Hostschlüsseln. Wenn zwei Schlüssel mit dem gleichen Namen definiert wurden, wird immer der Funktionsschlüssel verwendet.

Der **Hauptschlüssel** ist ein Standardhostschlüssel namens „_master“, der für jede Funktionen-App definiert ist. Dieser Schlüssel kann nicht widerrufen werden. Er bietet administrativen Zugriff auf die Laufzeit-APIs. Bei Verwendung von `"authLevel": "admin"` in der Bindungs-JSON muss dieser Schlüssel in der Anforderung bereitgestellt werden. Jeder andere Schlüssel führt zu einem Autorisierungsfehler.

> [!IMPORTANT]  
> Aufgrund der erhöhten Berechtigungen, die der Hauptschlüssel gewährt, sollten Sie diesen Schlüssel nicht für Dritte freigeben oder in nativen Clientanwendungen verteilen. Gehen Sie umsichtig vor, wenn Sie die Autorisierungsstufe „Administrator“ auswählen.

### <a name="api-key-authorization"></a>Autorisierung per API-Schlüssel

Ein HTTP-Trigger erfordert standardmäßig einen API-Schlüssel in der HTTP-Anforderung. Ihre HTTP-Anforderung sieht daher normalerweise wie folgt aus:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Der Schlüssel kann in einer Abfragezeichenfolgenvariablen namens `code` (wie oben zu sehen) oder in einem `x-functions-key`-HTTP-Header enthalten sein. Der Wert des Schlüssels kann ein beliebiger für die Funktion definierter Funktionsschlüssel oder ein beliebiger Hostschlüssel sein.

Sie können anonyme Anforderungen zulassen, die keine Schlüssel erfordern. Sie können auch anfordern, dass der Hauptschlüssel verwendet wird. Zum Ändern der Standardautorisierungsstufe verwenden Sie die `authLevel`-Eigenschaft in der Bindungs-JSON. Weitere Informationen finden Sie unter [Trigger: Konfiguration](#trigger---configuration).

### <a name="keys-and-webhooks"></a>Schlüssel und Webhooks

Die Webhookautorisierung wird von der Empfangskomponente für Webhooks verarbeitet, die zum HTTP-Trigger gehört. Der Mechanismus variiert je nach Webhooktyp. Für jeden Mechanismus wird allerdings ein Schlüssel benötigt. Standardmäßig wird der Funktionsschlüssel namens „default“ verwendet. Um einen anderen Schlüssel zu verwenden, konfigurieren Sie den Webhookanbieter, sodass der Schlüsselname auf eine der folgenden Arten mit der Anforderung gesendet wird:

- **Abfragezeichenfolge**: Der Anbieter übergibt den Schlüsselnamen im `clientid`-Abfragezeichenfolgenparameter, z. B. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **Anforderungsheader**: Der Anbieter übergibt den Schlüsselnamen im `x-functions-clientid`-Header.

## <a name="trigger---limits"></a>Trigger – Grenzwerte

Die Länge der HTTP-Anforderung ist auf 100.000 (102.400) Bytes und die URL-Länge auf 4.000 (4.096) Bytes beschränkt. Diese Grenzwerte werden durch das `httpRuntime`-Element der Datei [Web.config](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config) der Runtime angegeben.

Wenn eine Funktion, die den HTTP-Trigger verwendet, nicht innerhalb von etwa 2,5 Minuten abgeschlossen ist, tritt auf dem Gateway ein Timeout auf, und es wird ein HTTP 502-Fehler zurückgegeben. Die Funktion wird weiterhin ausgeführt, kann aber keine HTTP-Antwort zurückgeben. Bei Funktionen mit langer Ausführungsdauer empfiehlt es sich, asynchrone Muster zu befolgen und einen Speicherort zurückzugeben, von dem aus Sie den Status der Anforderung pingen können. Informationen dazu, wie lang eine Funktion ausgeführt werden kann, finden Sie unter [Skalierung und Hosting – Verbrauchsplan](functions-scale.md#consumption-plan). 

## <a name="trigger---hostjson-properties"></a>Trigger: Eigenschaften von „host.json“

Die Datei [host.json](functions-host-json.md) enthält Einstellungen, mit denen das Verhalten des HTTP-Triggers gesteuert werden kann.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Output

Verwenden Sie die HTTP-Ausgabebindung, um eine Antwort an den Absender der HTTP-Anforderung zu senden. Diese Bindung erfordert einen HTTP-Trigger und ermöglicht Ihnen, die Antwort, die der Anforderung des Triggers zugeordnet ist, benutzerdefiniert anzupassen. Wenn keine HTTP-Ausgabebindung angegeben wird, gibt ein HTTP-Trigger den Statuscode „HTTP 200 OK“ mit leerem Text zurück. 

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Für C#-Klassenbibliotheken gibt es keine Konfigurationseigenschaften für ausgabespezifische Bindungen. Um eine HTTP-Antwort zu senden, legen Sie als Rückgabetyp der Funktion `HttpResponseMessage` oder `Task<HttpResponseMessage>` fest.

Bei anderen Sprachen wird eine HTTP-Ausgabebindung wie im folgenden Beispiel dargestellt durch ein JSON-Objekt im `bindings`-Array von „function.json“ definiert:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
| **type** |Muss auf `http` festgelegt sein. |
| **direction** | Muss auf `out` festgelegt sein. |
|**name** | Der Variablenname, der im Funktionscode für das Antwortobjekt verwendet wird. |

## <a name="output---usage"></a>Ausgabe: Verwendung

Sie können den Ausgabeparameter für die Antwort an den HTTP- oder Webhookaufrufer verwenden. Sie können auch die Antwortmuster des Sprachstandards verwenden. Beispielantworten finden Sie im [Triggerbeispiel](#trigger---example) und im [Webhookbeispiel](#trigger---webhook-example).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
