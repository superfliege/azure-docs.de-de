---
title: Event Grid-Trigger für Azure Functions
description: Behandlung von Event Grid-Ereignissen in Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/26/2018
ms.author: tdykstra
ms.openlocfilehash: 5039798d76017d93b77d724b2e6bca6712af0370
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="event-grid-trigger-for-azure-functions"></a>Event Grid-Trigger für Azure Functions

In diesem Artikel wird die Behandlung von [Event Grid](../event-grid/overview.md)-Ereignissen in Azure Functions erläutert.

Event Grid ist ein Azure-Dienst, mit dem HTTP-Anforderungen gesendet werden, um Sie über Ereignisse zu benachrichtigen, die in *Herausgebern* erfolgen. Ein Herausgeber ist der Dienst oder die Ressource, von dem bzw. der das Ereignis stammt. Ein Azure Blob Storage-Konto ist beispielsweise ein Herausgeber, der Upload oder die Löschung eines Blobs ist dagegen ein Ereignis. Einige [Azure-Dienste bieten eine integrierte Unterstützung zum Veröffentlichen von Ereignissen in Event Grid](../event-grid/overview.md#event-publishers). 

*Ereignishandler* empfangen und verarbeiten Ereignisse. Azure Functions ist einer von mehreren [Azure-Diensten, die eine integrierte Unterstützung für die Behandlung von Event Grid-Ereignissen bieten](../event-grid/overview.md#event-handlers). In diesem Artikel wird beschrieben, wie Sie mithilfe eines Event Grid-Triggers eine Funktion aufrufen, wenn ein Ereignis von Event Grid empfangen wird.

Bei Bedarf können Sie einen HTTP-Trigger zur Behandlung von Event Grid-Ereignissen verwenden. Informationen dazu finden Sie weiter unten in diesem Artikel unter [Verwenden eines HTTP-Triggers als Event Grid-Trigger](#use-an-http-trigger-as-an-event-grid-trigger).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Pakete

Der Event Grid-Trigger wird im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension).

<!--
If you want to bind to the `Microsoft.Azure.EventGrid.Models.EventGridEvent` type instead of `JObject`, install the [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) package.
-->

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>Beispiel

Siehe das jeweilige sprachspezifische Beispiel für einen Event Grid-Trigger:

* [C#](#c-example)
* [C#-Skript (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

Ein Beispiel für einen HTTP-Trigger finden Sie unter [Verwenden eines HTTP-Triggers als Event Grid-Trigger](#use-an-http-trigger-as-an-event-grid-trigger) weiter unten in diesem Artikel.

### <a name="c-example"></a>C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md) für die Bindung an `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, TraceWriter log)
        {
            log.Info(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

<!--
The following example shows a [C# function](functions-dotnet-class-library.md) that binds to `EventGridEvent`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
            public static void EventGridTest([EventGridTrigger] Microsoft.Azure.EventGrid.Models.EventGridEvent eventGridEvent, TraceWriter log)
        {
            log.Info("C# Event Grid function processed a request.");
            log.Info($"Subject: {eventGridEvent.Subject}");
            log.Info($"Time: {eventGridEvent.EventTime}");
            log.Info($"Data: {eventGridEvent.Data.ToString()}");
        }
    }
}
```
-->

Weitere Informationen finden Sie unter [Pakete](#packages), [Attribute](#attributes), [Konfiguration](#configuration) und [Verwendung](#usage).

### <a name="c-script-example"></a>C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet.

Bindungsdaten in der Datei *function.json*:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C#-Skriptcode für die Bindung an `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

<!--
Here's C# script code that binds to `EventGridEvent`:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.Azure.WebJobs.Extensions.EventGrid"
#r "Microsoft.Azure.EventGrid"

using Microsoft.Azure.WebJobs.Extensions.EventGrid;
Using Microsoft.Azure.EventGrid.Models;

public static void Run(EventGridEvent eventGridEvent, TraceWriter log)
{
    log.Info("C# Event Grid function processed a request.");
    log.Info($"Subject: {eventGridEvent.Subject}");
    log.Info($"Time: {eventGridEvent.EventTime}");
    log.Info($"Data: {eventGridEvent.Data.ToString()}");
}
```
-->

Weitere Informationen finden Sie unter [Pakete](#packages), [Attribute](#attributes), [Konfiguration](#configuration) und [Verwendung](#usage).

### <a name="javascript-example"></a>JavaScript-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet.

Bindungsdaten in der Datei *function.json*:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```
     
## <a name="attributes"></a>Attribute

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) verwenden Sie das [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/EventGridTriggerAttribute.cs)-Attribut.

Dies ist ein `EventGridTrigger`-Attribut in einer Methodensignatur:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, TraceWriter log)
{
    ...
}
 ```

Ein vollständiges Beispiel finden Sie unter [C#-Beispiel](#c-example).

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen. Im Attribut `EventGridTrigger` müssen keine Konstruktorparameter oder -eigenschaften festgelegt werden.

|Eigenschaft von „function.json“ |BESCHREIBUNG|
|---------|---------|----------------------|
| **type** | Erforderlich – muss auf `eventGridTrigger` festgelegt sein. |
| **direction** | Erforderlich – muss auf `in` festgelegt sein. |
| **name** | Erforderlich – der Variablenname, der im Funktionscode für den Parameter verwendet wird, der die Ereignisdaten empfängt. |

## <a name="usage"></a>Verwendung

In C#- und F#-Funktionen können Sie die folgenden Parametertypen für den Event Grid-Trigger verwenden:

* `JObject`
* `string`
* `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent` definiert Eigenschaften für die Felder, die für alle Ereignistypen gelten. **Dieser Typ ist veraltet**, seine Ersetzung wurde jedoch noch nicht in NuGet veröffentlicht.

Bei JavaScript-Funktionen enthält der durch die `name`-Eigenschaft in *function.json* benannte Parameter einen Verweis auf das Ereignisobjekt.

## <a name="event-schema"></a>Ereignisschema

Daten für ein Event Grid-Ereignis werden als JSON-Objekt im Text einer HTTP-Anforderung empfangen. Das JSON-Objekt ähnelt dem folgenden Beispiel:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Das Beispiel stellt ein Array mit einem Element dar. Event Grid sendet immer ein Array und kann mehrere Ereignisse im Array senden. Die Runtime ruft die Funktion einmal für jedes Arrayelement auf.

Die Eigenschaften der obersten Ebene in den JSON-Ereignisdaten sind für alle Ereignistypen identisch, die Inhalte der Eigenschaft `data` sind dagegen für jeden Ereignistyp spezifisch. Das Beispiel oben bezieht sich auf ein Blob Storage-Ereignis.

Erläuterungen zu den allgemeinen und ereignisspezifischen Eigenschaften finden Sie unter [Ereigniseigenschaften](../event-grid/event-schema.md#event-properties) in der Event Grid-Dokumentation.

Mit dem Typ `EventGridEvent` werden lediglich die Eigenschaften der obersten Ebene definiert. Bei der `Data`-Eigenschaft handelt es sich um ein `JObject`. 

## <a name="create-a-subscription"></a>Erstellen eines Abonnements

Erstellen Sie für den Empfang von HTTP-Anforderungen in Event Grid ein Event Grid-Abonnement, das die Endpunkt-URL angibt, über die die Funktion aufgerufen wird.

### <a name="azure-portal"></a>Azure-Portal

Wählen Sie für Funktionen, die Sie im Azure-Portal mit dem Event Grid-Trigger entwickeln, die Option **Event Grid-Abonnement hinzufügen** aus.

![Erstellen eines Abonnements im Portal](media/functions-bindings-event-grid/portal-sub-create.png)

Bei Auswahl dieses Links wird das Portal mit der Seite **Ereignisabonnement erstellen** geöffnet, auf der die Endpunkt-URL bereits ausgefüllt ist.

![Bereits ausgefüllte Endpunkt-URL](media/functions-bindings-event-grid/endpoint-url.png)

Weitere Informationen zum Erstellen von Abonnements über das Azure-Portal finden Sie unter [Erstellen eines benutzerdefinierten Ereignisses – Azure-Portal](../event-grid/custom-event-quickstart-portal.md) in der Event Grid-Dokumentation.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create), um ein Abonnement über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) zu erstellen.

Für den Befehl ist die Endpunkt-URL erforderlich, über die die Funktion aufgerufen wird. Das folgende Beispiel zeigt das URL-Muster:

```
https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}
```

Der Systemschlüssel ist ein Autorisierungsschlüssel, der in der Endpunkt-URL für einen Event Grid-Trigger eingefügt werden muss. Im folgenden Abschnitt wird erläutert, wie der Systemschlüssel abgerufen wird.

Beispiel, in dem ein Blob Storage-Konto abonniert wird (mit einem Platzhalter für den Systemschlüssel):

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name glengablobstorage --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://glengastorageevents.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=LUwlnhIsNtSiUjv/sNtSiUjvsNtSiUjvsNtSiUjvYb7XDonDUr/RUg==
```

Weitere Informationen zum Erstellen eines Abonnements finden Sie im [Schnellstart für Blob Storage](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) oder in den anderen Schnellstarts für Event Grid.

### <a name="get-the-system-key"></a>Abrufen des Systemschlüssels

Sie können den Systemschlüssel mithilfe der folgenden API (HTTP GET) abrufen:

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={adminkey}
```

Dabei handelt es sich um eine Admin-API, für die Ihr [Administratorschlüssel](functions-bindings-http-webhook.md#authorization-keys) erforderlich ist. Verwechseln Sie den Systemschlüssel (zum Aufrufen einer Event Grid-Triggerfunktion) nicht mit dem Administratorschlüssel (zum Ausführen von Verwaltungsaufgaben in der Funktions-App). Verwenden Sie zum Abonnieren eines Event Grid-Themas immer den Systemschlüssel.

Beispiel für die Antwort, die den Systemschlüssel enthält:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

Weitere Informationen finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook.md#authorization-keys) im Referenzartikel zu HTTP-Triggern. 

Alternativ können Sie eine HTTP-PUT-Anforderung senden, um den Schlüsselwert selbst anzugeben.

## <a name="local-testing-with-requestbin"></a>Lokales Testen mit RequestBin

Zum lokalen Testen eines Event Grid-Triggers müssen Event Grid-HTTP-Anforderungen von ihrem Ursprung in der Cloud an Ihren lokalen Computer gesendet werden. Dies kann erfolgen, indem Anforderungen online erfasst und manuell erneut an Ihren lokalen Computer gesendet werden:

2. [Erstellen Sie einen RequestBin-Endpunkt](#create-a-RequestBin-endpoint).
3. [Erstellen Sie ein Event Grid-Abonnement](#create-an-event-grid-subscription), mit dem Ereignisse an den RequestBin-Endpunkt gesendet werden.
4. [Generieren Sie eine Anforderung](#generate-a-request), und kopieren Sie den Anforderungstext von der RequestBin-Website.
5. [Stellen Sie die Anforderung manuell](#manually-post-the-request) für die localhost-URL der Event Grid-Triggerfunktion bereit.

Nach Abschluss der Tests können Sie dasselbe Abonnement für die Produktion verwenden, indem Sie den Endpunkt aktualisieren. Verwenden Sie dazu den Azure CLI-Befehl [az eventgrid event-subscription update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update).

### <a name="create-a-requestbin-endpoint"></a>Erstellen eines RequestBin-Endpunkts

RequestBin ist ein Open-Source-Tool, das HTTP-Anforderungen akzeptiert und den Anforderungstext anzeigt. Die URL http://requestb.in erhält in Azure Event Grid eine Sonderbehandlung. Zum vereinfachten Testen sendet Event Grid Ereignisse an die RequestBin-URL, ohne dass eine richtige Antwort auf Anforderungen zur Abonnementüberprüfung erforderlich ist. Zwei andere Testtools werden auf die gleiche Weise behandelt: http://webhookinbox.com und http://hookbin.com.

RequestBin ist nicht für hohen Durchsatz konzipiert. Wenn Sie gleichzeitig mehrere Ereignisse pushen, werden möglicherweise nicht alle Ereignisse im Tool angezeigt.

Erstellen Sie einen Endpunkt.

![Erstellen eines RequestBin-Endpunkts](media/functions-bindings-event-grid/create-requestbin.png)

Kopieren Sie die Endpunkt-URL.

![Kopieren des RequestBin-Endpunkts](media/functions-bindings-event-grid/save-requestbin-url.png)

### <a name="create-an-event-grid-subscription"></a>Erstellen eines Event Grid-Abonnements

Erstellen Sie ein Event Grid-Abonnement des Abonnementtyps, der getestet werden soll, und legen Sie für das Abonnement Ihren RequestBin-Endpunkt fest. Informationen zum Erstellen eines Abonnements finden Sie unter [Erstellen eines Abonnements](#create-a-subscription) weiter oben in diesem Artikel.

### <a name="generate-a-request"></a>Generieren einer Anforderung

Lösen Sie ein Ereignis aus, mit dem HTTP-Datenverkehr für den RequestBin-Endpunkt generiert wird.  Wenn Sie z.B. ein Blob Storage-Abonnement erstellt haben, können Sie ein Blob hochladen oder löschen. Wenn eine Anforderung auf der RequestBin-Seite angezeigt wird, kopieren Sie den Anforderungstext.

Die Anforderung zur Abonnementüberprüfung wird zuerst empfangen. Ignorieren Sie sämtliche Überprüfungsanforderungen, und kopieren Sie die Ereignisanforderung.

![Kopieren des Anforderungstexts von RequestBin](media/functions-bindings-event-grid/copy-request-body.png)

### <a name="manually-post-the-request"></a>Manuelles Bereitstellen der Anforderung

Führen Sie die Event Grid-Funktion lokal aus.

Verwenden Sie ein Tool wie z.B. [Postman](https://www.getpostman.com/) oder [cURL](https://curl.haxx.se/docs/httpscripting.html), um eine HTTP POST-Anforderung zu erstellen:

* Legen Sie einen `Content-Type: application/json`-Header fest.
* Legen Sie einen `aeg-event-type: Notification`-Header fest.
* Fügen Sie die RequestBin-Daten im Anforderungstext ein. 
* Stellen Sie die Anforderung mithilfe des folgenden Musters für die URL der Event Grid-Triggerfunktion bereit:

```
http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

Der `functionName`-Parameter muss der im `FunctionName`-Attribut angegebene Name sein.

In den folgenden Screenshots sind die Header und der Anforderungstext in Postman dargestellt:

![Header in Postman](media/functions-bindings-event-grid/postman2.png)

![Anforderungstext in Postman](media/functions-bindings-event-grid/postman.png)

Mit der Event Grid-Triggerfunktion werden Protokolle ähnlich denen im folgenden Beispiel ausgeführt und angezeigt:

![Beispielprotokolle der Event Grid-Triggerfunktion](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Lokales Testen mit ngrok

Eine weitere Möglichkeit zum lokalen Testen eines Event Grid-Triggers ist die Automatisierung der HTTP-Verbindung zwischen dem Internet und dem Entwicklungscomputer. Dazu können Sie das Open-Source-Tool [ngrok](https://ngrok.com/) verwenden:

3. [Erstellen Sie einen ngrok-Endpunkt](#create-an-ngrok-endpoint).
4. [Führen Sie die Event Grid-Triggerfunktion aus](#run-the-event-grid-trigger-function).
5. [Erstellen Sie ein Event Grid-Abonnement](#create-a-subscription), mit dem Ereignisse an den ngrok-Endpunkt gesendet werden.
6. [Lösen Sie ein Ereignis aus](#trigger-an-event).

Nach Abschluss der Tests können Sie dasselbe Abonnement für die Produktion verwenden, indem Sie den Endpunkt aktualisieren. Verwenden Sie dazu den Azure CLI-Befehl [az eventgrid event-subscription update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update).

### <a name="create-an-ngrok-endpoint"></a>Erstellen eines ngrok-Endpunkts

Laden Sie die Datei *ngrok.exe* von [ngrok](https://ngrok.com/) herunter, und führen Sie sie mit dem folgenden Befehl aus:

```
ngrok http -host-header=localhost 7071
```

Der Parameter „-host-header“ ist erforderlich, da die Functions-Runtime Anforderungen von „localhost“ erwartet, wenn sie auf „localhost“ ausgeführt wird. Wenn die Runtime lokal ausgeführt wird, ist 7071 die Standardportnummer.

Mit dem Befehl wird eine Ausgabe ähnlich der folgenden erstellt:

```
Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://263db807.ngrok.io -> localhost:7071
Forwarding                    https://263db807.ngrok.io -> localhost:7071

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

Sie verwenden die URL https://{Unterdomäne}.ngrok.io für Ihr Event Grid-Abonnement.

### <a name="run-the-event-grid-trigger-function"></a>Ausführen der Event Grid-Triggerfunktion

Die ngrok-URL erhält in Event Grid keine spezielle Behandlung, daher muss die Funktion bei der Erstellung des Abonnements lokal ausgeführt werden. Wenn dies nicht der Fall ist, wird die Überprüfungsantwort nicht gesendet, und bei der Erstellung des Abonnements treten Fehler auf.

### <a name="create-a-subscription"></a>Erstellen eines Abonnements

Erstellen Sie entsprechend dem folgenden Muster ein Event Grid-Abonnement des Abonnementtyps, der getestet werden soll, und legen Sie für das Abonnement Ihren ngrok-Endpunkt fest:

```
https://{subdomain}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

Der `functionName`-Parameter muss der im `FunctionName`-Attribut angegebene Name sein.

Beispiel für die Verwendung der Azure-Befehlszeilenschnittstelle:

```
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName=EventGridTrigger
```

Informationen zum Erstellen eines Abonnements finden Sie unter [Erstellen eines Abonnements](#create-a-subscription) weiter oben in diesem Artikel.

### <a name="trigger-an-event"></a>Auslösen eines Ereignisses

Lösen Sie ein Ereignis aus, mit dem HTTP-Datenverkehr für den ngrok-Endpunkt generiert wird.  Wenn Sie z.B. ein Blob Storage-Abonnement erstellt haben, können Sie ein Blob hochladen oder löschen.

Mit der Event Grid-Triggerfunktion werden Protokolle ähnlich denen im folgenden Beispiel ausgeführt und angezeigt:

![Beispielprotokolle der Event Grid-Triggerfunktion](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>Verwenden eines HTTP-Triggers als Event Grid-Trigger

Event Grid-Ereignisse werden als HTTP-Anforderungen empfangen. Somit können Ereignisse unter Verwendung eines HTTP-Triggers anstelle eines Event Grid-Triggers verarbeitet werden. Ein möglicher Grund dafür ist die bessere Steuerung der Endpunkt-URL, über die die Funktion aufgerufen wird. 

Bei Verwendung eines HTTP-Triggers müssen Sie Code für folgende Vorgänge schreiben, die über den Event Grid-Trigger automatisch ausgeführt werden:

* Senden einer Überprüfungsantwort an eine [Anforderung zur Abonnementüberprüfung](../event-grid/security-authentication.md#webhook-event-delivery)
* Aufrufen der Funktion einmal pro Element des im Anforderungstext enthaltenen Ereignisarrays

Mit dem folgenden C#-Beispielcode für einen HTTP-Trigger wird das Verhalten eines Event Grid-Triggers simuliert:

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"], 
        "Microsoft.EventGrid.SubscriptionValidationEvent", 
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.Info("Validate request received");
        return req.CreateResponse<object>(new
        {
            validationResponse = messages[0]["data"]["validationCode"]
        });
    }

    // The request is not for subscription validation, so it's for one or more events.
    foreach (JObject message in messages)
    {
        // Handle one event.
        EventGridEvent eventGridEvent = message.ToObject<EventGridEvent>();
        log.Info($"Subject: {eventGridEvent.Subject}");
        log.Info($"Time: {eventGridEvent.EventTime}");
        log.Info($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Mit dem folgenden JavaScript-Beispielcode für einen HTTP-Trigger wird das Verhalten eines Event Grid-Triggers simuliert:

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        context.res = { status: 200, body: messages[0].data.validationCode }
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        for (var i = 0; i < messages.length; i++) {
            // Handle one event.
            var message = messages[i];
            context.log('Subject: ' + message.subject);
            context.log('Time: ' + message.eventTime);
            context.log('Data: ' + JSON.stringify(message.data));
        }
    }
    context.done();
};
```

Der Code zur Ereignisbehandlung wird über das `messages`-Array in die Schleife eingefügt.

Informationen zu der zum lokalen Aufrufen der Funktion verwendeten URL oder ihrer Ausführung in Azure finden Sie in der [Referenzdokumentation zu HTTP-Triggerbindungen](functions-bindings-http-webhook.md). 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Weitere Informationen zu Event Grid](../event-grid/overview.md)
