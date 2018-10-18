---
title: HTTP-Trigger und -Bindungen in Azure Functions
description: Hier wird erläutert, wie HTTP-Trigger und -Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Computing, serverlose Architektur, HTTP, API, REST
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: e989152ece19168138597a96d1246ec64498ce69
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227553"
---
# <a name="azure-functions-http-triggers-and-bindings"></a>HTTP-Trigger und -Bindungen in Azure Functions

In diesem Artikel wird erläutert, wie HTTP-Trigger und -Ausgabebindungen in Azure Functions verwendet werden.

Ein HTTP-Trigger kann so angepasst werden, dass er auf [Webhooks](https://en.wikipedia.org/wiki/Webhook) antwortet.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="packages---functions-1x"></a>Pakete: Functions 1.x

Die HTTP-Bindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) der Version 1.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http).

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Pakete: Functions 2.x

Die HTTP-Bindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) der Version 3.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/).

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Trigger

Mit dem HTTP-Trigger können Sie eine Funktion mit einer HTTP-Anforderung aufrufen. Sie können einen HTTP-Trigger zum Erstellen von serverlosen APIs und zum Antworten auf Webhooks verwenden. 

Standardmäßig gibt ein HTTP-Trigger in Functions 1.x den HTTP-Statuscode 200 „OK“ ohne Text oder in Functions 2.x den HTTP-Statuscode 204 „No Content“ ohne Text zurück. Um diese Antwort zu ändern, konfigurieren Sie eine [HTTP-Ausgabebindung](#http-output-binding).

## <a name="trigger---example"></a>Trigger: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#trigger---c-example)
* [C#-Skript (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

### <a name="trigger---c-example"></a>Trigger: C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter sucht. Beachten Sie, dass der Rückgabewert für die Ausgabebindung verwendet wird, jedoch kein Attribut vom Rückgabewert erforderlich ist.

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

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
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
```

### <a name="trigger---f-example"></a>Trigger: F#-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [F#-Funktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
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

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
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

### <a name="trigger---java-example"></a>Trigger: Java-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [Java-Funktion](functions-reference-java.md), die die Bindung verwendet. Die Funktion gibt eine Antwort mit dem HTTP-Statuscode 200 und einem Anforderungstext zurück, in der dem Text der auslösenden Anforderung die Begrüßung „Hello, “ vorangestellt ist.


Die Datei *function.json* sieht wie folgt aus:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "anonymous",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

Dies ist der Java-Code:

```java
@FunctionName("hello")
public HttpResponseMessage<String> hello(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS), Optional<String> request,
                        final ExecutionContext context) 
    {
        // default HTTP 200 response code
        return String.format("Hello, %s!", request);
    }
}
```

## <a name="trigger---attributes"></a>Trigger: Attribute

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das Attribut [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs).

Sie können die Autorisierungsebene und die zulässigen HTTP-Methoden in den Konstruktorparametern für das Attribut festlegen. Außerdem gibt es Eigenschaften für den Webhooktyp und die Routenvorlage. Weitere Informationen zu diesen Einstellungen finden Sie unter [Trigger: Konfiguration](#trigger---configuration). Dies ist ein `HttpTrigger`-Attribut in einer Methodensignatur:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous)] HttpRequestMessage req)
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
| **methods** |**Methoden** | Ein Array der HTTP-Methoden, auf die diese Funktion antwortet. Wird dieses Array nicht angegeben, antwortet die Funktion auf alle HTTP-Methoden. Siehe [Anpassen des HTTP-Endpunkts](#customize-the-http-endpoint). |
| **route** | **Route** | Definiert die Routenvorlage, mit der gesteuert wird, auf welche Anforderungs-URLs die Funktion antwortet. Wenn kein anderer Wert angegeben wird, lautet der Standardwert `<functionname>`. Weitere Informationen finden Sie unter [Anpassen des HTTP-Endpunkts](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** | _Nur für die Laufzeit der Version 1.x unterstützt._<br/><br/>Konfiguriert den HTTP-Trigger so, dass er als [Webhookempfänger](https://en.wikipedia.org/wiki/Webhook) für den angegebenen Anbieter fungiert. Legen Sie die `methods` Eigenschaft nicht fest, wenn Sie diese Eigenschaft festlegen. Der Webhooktyp kann folgende Werte annehmen:<ul><li><code>genericJson</code>: Ein allgemeiner Webhookendpunkt ohne Logik für einen bestimmten Anbieter. Diese Einstellung beschränkt Anforderungen auf solche, die HTTP POST verwenden und den Inhaltstyp `application/json` aufweisen.</li><li><code>github</code>: Die Funktion antwortet auf [GitHub-Webhooks](https://developer.github.com/webhooks/). Verwenden Sie die _authLevel_-Eigenschaft nicht mit GitHub-Webhooks. Weitere Informationen finden Sie im Abschnitt zu GitHub-Webhooks weiter unten in diesem Artikel.</li><li><code>slack</code>: Die Funktion antwortet auf [Slack-Webhooks](https://api.slack.com/outgoing-webhooks). Verwenden Sie die _authLevel_-Eigenschaft nicht mit Slack-Webhooks. Weitere Informationen finden Sie im Abschnitt zu Slack-Webhooks weiter unten in diesem Artikel.</li></ul>|

## <a name="trigger---usage"></a>Trigger: Verwendung

Bei C#- und F#-Funktionen können Sie den Typ der Triggereingabe als `HttpRequestMessage` oder als benutzerdefinierten Typ deklarieren. Wenn Sie `HttpRequestMessage` auswählen, erhalten Sie Vollzugriff auf das Anforderungsobjekt. Bei einem benutzerdefinierten Typ versucht die Laufzeit, den JSON-Anforderungstext zu analysieren, um die Objekteigenschaften festzulegen.

Für JavaScript-Funktionen stellt die Functions-Runtime den Anforderungstext anstelle des Anforderungsobjekts bereit. Weitere Informationen finden Sie im [JavaScript-Triggerbeispiel](#trigger---javascript-example).


### <a name="customize-the-http-endpoint"></a>Anpassen des HTTP-Endpunkts

Wenn Sie eine Funktion für einen HTTP-Trigger erstellen, ist die Funktion mit einer Route der folgenden Form erreichbar:

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
            // status defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status defaults to 200 */
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

Mit Functions können Sie Schlüssel verwenden, wodurch der Zugriff auf Ihre HTTP-Funktionsendpunkte während der Entwicklung erschwert wird.  Möglicherweise wird von einem HTTP-Standardtrigger verlangt, dass solch ein API-Schlüssel in der Anforderung vorhanden ist. 

> [!IMPORTANT]
> Schlüssel helfen zwar Ihre HTTP-Endpunkte während der Entwicklung zu verschleiern, sie sind jedoch nicht als Möglichkeit zum Schützen eines HTTP-Triggers in einer Produktionsumgebung vorgesehen. Weitere Informationen hierzu finden Sie unter [Schützen eines HTTP-Endpunkts in einer Produktionsumgebung](#secure-an-http-endpoint-in-production).

> [!NOTE]
> In der Functions Laufzeit 1.x können Webhookanbieter Schlüssel verwenden, um Anforderungen auf unterschiedliche Arten zu autorisieren, je nachdem, welche Methode vom Anbieter unterstützt wird. Dieses Thema wird unter [Webhooks und Schlüssel](#webhooks-and-keys) behandelt. Die Version 2.x der Laufzeit beinhaltet keine Unterstützung für Webhookanbieter.

Es gibt zwei Arten von Schlüsseln:

* **Hostschlüssel:** Diese Schlüssel werden von allen Funktionen innerhalb der Funktionen-App gemeinsam genutzt. Bei Verwendung als API-Schlüssel ermöglichen diese Zugriff auf jede Funktion in der Funktionen-App.
* **Funktionsschlüssel**: Diese Schlüssel gelten nur für die Funktionen, für die sie definiert sind. Bei Verwendung als API-Schlüssel ermöglichen diese nur Zugriff auf diese spezielle Funktion.

Jeder Schlüssel ist zu Referenzzwecken benannt. Auf Funktions- und Hostebene gibt es einen Standardschlüssel (mit dem Namen „default“). Funktionsschlüssel haben Vorrang vor Hostschlüsseln. Wenn zwei Schlüssel mit dem gleichen Namen definiert wurden, wird immer der Funktionsschlüssel verwendet.

Jede Funktions-App weist einen speziellen **Hauptschlüssel** auf. Bei diesem Schlüssel handelt es sich um einen Hostschlüssel mit dem Namen `_master`. Er stellt den Administratorzugriff auf Laufzeit-APIs bereit. Dieser Schlüssel kann nicht widerrufen werden. Wenn Sie die Autorisierungsstufe `admin` festlegen, muss für Anforderungen der Hauptschlüssel verwendet werden. Alle anderen Schlüssel führen zu einem Autorisierungsfehler.

> [!CAUTION]  
> Aufgrund der erhöhten Berechtigungen, die der Hauptschlüssel in Ihrer Funktions-App gewährt, sollten Sie diesen Schlüssel nicht für Dritte freigeben oder in nativen Clientanwendungen verteilen. Gehen Sie umsichtig vor, wenn Sie die Autorisierungsstufe „Administrator“ auswählen.

### <a name="obtaining-keys"></a>Abrufen von Schlüsseln

Schlüssel werden als Teil Ihrer Funktionen-App in Azure gespeichert und sind im Ruhezustand verschlüsselt. Um vorhandene Schlüssel anzuzeigen, neue Schlüssel zu erstellen oder Schlüsseln neue Werte zuzuweisen, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu einer Ihrer über HTTP ausgelösten Funktionen, und wählen Sie **Verwalten** aus.

![Verwalten Sie Funktionsschlüssel im Portal.](./media/functions-bindings-http-webhook/manage-function-keys.png)

Zum programmgesteuerten Abrufen von Funktionsschlüsseln gibt es keine unterstützte API.

### <a name="api-key-authorization"></a>Autorisierung per API-Schlüssel

Für die meisten HTTP-Triggervorlagen muss in der Anforderung ein API-Schlüssel vorhanden sein. Ihre HTTP-Anforderung sieht daher normalerweise wie folgende URL aus:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Der Schlüssel kann in einer Abfragezeichenfolgenvariablen namens `code` (wie oben zu sehen) enthalten sein. Er kann auch in einem `x-functions-key`-HTTP-Header enthalten sein. Der Wert des Schlüssels kann ein beliebiger für die Funktion definierter Funktionsschlüssel oder ein beliebiger Hostschlüssel sein.

Sie können anonyme Anforderungen zulassen, die keine Schlüssel erfordern. Sie können auch anfordern, dass der Hauptschlüssel verwendet wird. Zum Ändern der Standardautorisierungsstufe verwenden Sie die `authLevel`-Eigenschaft in der Bindungs-JSON. Weitere Informationen finden Sie unter [Trigger: Konfiguration](#trigger---configuration).

> [!NOTE]
> Beim lokalen Ausführen von Funktionen ist die Autorisierung unabhängig von der festgelegten Authentifizierungsebene deaktiviert. Die `authLevel`-Einstellung in Ihrem Trigger wird nach der Veröffentlichung in Azure angewendet.



### <a name="secure-an-http-endpoint-in-production"></a>Schützen eines HTTP-Endpunkts in einer Produktionsumgebung

Wenn Sie Ihre Funktionsendpunkte in einer Produktionsumgebung umfassend schützen möchten, sollten Sie eine der folgenden Sicherheitsoptionen auf Funktions-App-Ebene implementieren:

* Aktivieren Sie für Ihre Funktions-App die App Service-Authentifizierung/Autorisierung. Die App Service-Plattform ermöglicht die Verwendung von Azure Active Directory (AAD) sowie von verschiedenen anderen Identitätsanbietern zum Authentifizieren von Clients. Sie können sie zum Implementieren von benutzerdefinierten Autorisierungsregeln für Ihre Funktionen sowie Benutzerinformationen aus Ihrem Funktionscode verwenden. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](../app-service/app-service-authentication-overview.md).

* Verwenden Sie Azure API Management (APIM) zum Authentifizieren von Anforderungen. APIM stellt verschiedene API-Sicherheitsoptionen für eingehende Anforderungen bereit. Weitere Informationen hierzu finden Sie unter [API Management-Authentifizierungsrichtlinien](../api-management/api-management-authentication-policies.md). Wenn APIM vorhanden ist, können Sie Ihre Funktions-App so konfigurieren, dass Anforderungen nur von den IP-Adressen Ihrer APIM-Instanz angenommen werden. Weitere Informationen hierzu finden Sie unter [Einschränkungen für IP-Adressen](ip-addresses.md#ip-address-restrictions).

* Stellen Sie Ihre Funktions-App in einer Azure App Service-Umgebung (ASE, App Service Environment) bereit. ASE stellt eine spezielle Hostingumgebung bereit, in der Sie Ihre Funktionen ausführen können. Mit ASE können Sie ein Front-End-Gateway konfigurieren, das Sie zum Authentifizieren aller eingehenden Anforderungen verwenden können. Weitere Informationen hierzu finden Sie unter [Konfigurieren einer Web Application Firewall (WAF) für eine App Service-Umgebung](../app-service/environment/app-service-app-service-environment-web-application-firewall.md).

Wenn Sie eine dieser Sicherheitsmethoden auf Funktions-App-Ebene verwenden, sollten Sie die Authentifizierungsebene der über HTTP ausgelösten Funktion auf `anonymous` festlegen.

### <a name="webhooks"></a>Webhooks

> [!NOTE]
> Der Webhookmodus ist nur für die Version 1.x der Functions-Laufzeit verfügbar.

Der Webhookmodus stellt zusätzliche Überprüfungsoptionen für Webhooknutzlasten bereit. In Version 2.x kann der HTTP-Basistrigger weiterhin verwendet werden. Zudem wird er für Webhooks empfohlen.

#### <a name="github-webhooks"></a>GitHub-Webhooks

Erstellen Sie zum Antworten auf GitHub-Webhooks zuerst eine Funktion mit einem HTTP-Trigger, und legen Sie die **webHookType**-Eigenschaft auf `github` fest. Kopieren Sie dann die URL und den API-Schlüssel auf die Seite **Webhook hinzufügen** Ihres GitHub-Repositorys. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Ein Beispiel finden Sie unter [Erstellen einer Funktion, die durch einen GitHub-Webhook ausgelöst wird](functions-create-github-webhook-triggered-function.md).

#### <a name="slack-webhooks"></a>Slack-Webhooks

Der Slack-Webhook lässt Sie das Token nicht angeben, sondern generiert es für Sie. Daher müssen Sie einen funktionsspezifischen Schlüssel mit dem Token aus Slack konfigurieren. Weitere Informationen finden Sie unter [Autorisierungsschlüssel](#authorization-keys).

### <a name="webhooks-and-keys"></a>Webhooks und Schlüssel

Die Webhookautorisierung wird von der Empfangskomponente für Webhooks verarbeitet, die zum HTTP-Trigger gehört. Der Mechanismus variiert je nach Webhooktyp. Für jeden Mechanismus wird ein Schlüssel benötigt. Standardmäßig wird der Funktionsschlüssel namens „default“ verwendet. Um einen anderen Schlüssel zu verwenden, konfigurieren Sie den Webhookanbieter, sodass der Schlüsselname auf eine der folgenden Arten mit der Anforderung gesendet wird:

* **Abfragezeichenfolge**: Der Anbieter übergibt den Schlüsselnamen im `clientid`-Abfragezeichenfolgenparameter, z. B. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
* **Anforderungsheader**: Der Anbieter übergibt den Schlüsselnamen im `x-functions-clientid`-Header.

## <a name="trigger---limits"></a>Trigger – Grenzwerte

Die Länge der HTTP-Anforderung ist auf 100 MB (104.857.600 Bytes) und die URL-Länge auf 4 KB (4.096 Bytes) beschränkt. Diese Grenzwerte werden durch das `httpRuntime`-Element der Datei [Web.config](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config) der Runtime angegeben.

Wenn eine Funktion, die den HTTP-Trigger verwendet, nicht innerhalb von etwa 2,5 Minuten abgeschlossen ist, tritt auf dem Gateway ein Timeout auf, und es wird ein HTTP 502-Fehler zurückgegeben. Die Funktion wird weiterhin ausgeführt, kann aber keine HTTP-Antwort zurückgeben. Bei Funktionen mit langer Ausführungsdauer empfiehlt es sich, asynchrone Muster zu befolgen und einen Speicherort zurückzugeben, von dem aus Sie den Status der Anforderung pingen können. Informationen dazu, wie lang eine Funktion ausgeführt werden kann, finden Sie unter [Skalierung und Hosting – Verbrauchsplan](functions-scale.md#consumption-plan). 

## <a name="trigger---hostjson-properties"></a>Trigger: Eigenschaften von „host.json“

Die Datei [host.json](functions-host-json.md) enthält Einstellungen, mit denen das Verhalten des HTTP-Triggers gesteuert werden kann.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Output

Verwenden Sie die HTTP-Ausgabebindung, um eine Antwort an den Absender der HTTP-Anforderung zu senden. Diese Bindung erfordert einen HTTP-Trigger und ermöglicht Ihnen, die Antwort, die der Anforderung des Triggers zugeordnet ist, benutzerdefiniert anzupassen. Wenn keine HTTP-Ausgabebindung angegeben wird, gibt ein HTTP-Trigger in Functions 1.x den HTTP-Statuscode 200 „OK“ ohne Text oder in Functions 2.x den HTTP-Statuscode 204 „No Content“ ohne Text zurück.

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen. Für C#-Klassenbibliotheken gibt es keine Attributeigenschaften, die den folgenden Eigenschaften der Datei *function.json* entsprechen. 

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
| **type** |Muss auf `http` festgelegt sein. |
| **direction** | Muss auf `out` festgelegt sein. |
|**name** | Der Variablenname, der im Funktionscode für die Antwort verwendet wird, oder `$return` für die Verwendung des Rückgabewerts. |

## <a name="output---usage"></a>Ausgabe: Verwendung

Verwenden Sie zum Senden einer HTTP-Antwort die Antwortmuster des Sprachstandards. Legen Sie in C# oder im C#-Skript den Funktionsrückgabetyp auf `HttpResponseMessage` oder `Task<HttpResponseMessage>` fest. In C# ist kein Attribut des Rückgabewerts erforderlich.

Beispielantworten finden Sie im [Triggerbeispiel](#trigger---example).

## <a name="next-steps"></a>Nächste Schritte

[Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
