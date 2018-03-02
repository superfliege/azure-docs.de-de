---
title: Empfangen von Ereignissen von Azure Event Grid an einem HTTP-Endpunkt
description: "Beschreibt, wie ein HTTP-Endpunkt überprüft und dann Ereignisse aus Azure Event Grid empfangen und deserialisiert werden."
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: article
ms.date: 02/16/2018
ms.author: babanisa
<<<<<<< HEAD
ms.openlocfilehash: 179f7c46186762eed2f7f8ac90620ac2fec9caf3
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
=======
<<<<<<< HEAD
ms.openlocfilehash: 179f7c46186762eed2f7f8ac90620ac2fec9caf3
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
=======
ms.openlocfilehash: da4d1aff45c39d8b4ce7cdd003dd4af35dcd33b3
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
---
# <a name="receive-events-to-an-http-endpoint"></a>Empfangen von Ereignissen an einem HTTP-Endpunkt

In diesem Artikel wird beschrieben, wie Sie [einen HTTP-Endpunkt überprüfen](security-authentication.md#webhook-event-delivery), um Ereignisse von einem Ereignisabonnement zu empfangen, und dann Ereignisse empfangen und deserialisieren. In diesem Artikel wird zu Demonstrationszwecken eine Azure-Funktion verwendet, doch treffen die gleichen Konzepte unabhängig davon zu, wo die Anwendung gehostet wird.

> [!NOTE]
> Es wird **dringend** empfohlen, beim Auslösen einer Azure-Funktion mit Event Grid einen [Event Grid-Trigger](../azure-functions/functions-bindings-event-grid.md) zu verwenden. Die Verwendung eines generischen WebHook-Triggers dient hier lediglich zur Veranschaulichung.

## <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen eine Funktionen-App mit einer [durch HTTP ausgelösten Funktion](../azure-functions/functions-create-generic-webhook-triggered-function.md).

## <a name="add-dependencies"></a>Hinzufügen von Abhängigkeiten

Wenn Sie die Entwicklung in .Net vornehmen, [fügen Sie der Funktion eine Abhängigkeit](../azure-functions/functions-reference-csharp.md#referencing-custom-assemblies) für das [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) `Microsoft.Azure.EventGrid` hinzu. SDKs für andere Sprachen stehen über den Verweis auf [Veröffentlichungs-SDKs](./sdk-overview.md#publish-sdks) zur Verfügung. Diese Pakete enthalten die Modelle für systemeigene Ereignistypen wie z. B. `EventGridEvent`, `StorageBlobCreatedEventData` und `EventHubCaptureFileCreatedEventData`.

<<<<<<< HEAD
Klicken Sie zu diesem Zweck auf den Link "Dateien anzeigen" in Ihrer Azure-Funktion (der Bereich ganz rechts im Azure Functions-Portal), und erstellen Sie eine Datei mit dem Namen „project.json“. Fügen Sie der Datei `project.json` den folgenden Inhalt hinzu, und speichern Sie sie:

 ```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.Azure.EventGrid": "1.1.0-preview"
      }
    }
   }
}

=======
<<<<<<< HEAD
Klicken Sie zu diesem Zweck auf den Link "Dateien anzeigen" in Ihrer Azure-Funktion (der Bereich ganz rechts im Azure Functions-Portal), und erstellen Sie eine Datei mit dem Namen „project.json“. Fügen Sie der Datei `project.json` den folgenden Inhalt hinzu, und speichern Sie sie:

 ```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.Azure.EventGrid": "1.1.0-preview"
      }
    }
   }
}

=======
Klicken Sie zu diesem Zweck auf den Link "Dateien anzeigen" in Ihrer Azure-Funktion (der Bereich ganz rechts im Azure Functions-Portal), und erstellen Sie eine Datei mit dem Namen „project.json“. Fügen Sie der Datei `project.json` den folgenden Inhalt hinzu, und speichern Sie sie: 
 
 ```json
{ 
  "frameworks": { 
    "net46":{ 
      "dependencies": { 
        "Microsoft.Azure.EventGrid": "1.1.0-preview" 
      } 
    } 
   } 
} 
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
```

![Hinzugefügtes NuGet-Paket](./media/receive-events/add-dependencies.png)

## <a name="endpoint-validation"></a>Endpunktüberprüfung

Als Erstes sollen Ereignisse des Typs `Microsoft.EventGrid.SubscriptionValidationEvent` behandelt werden. Jedes Mal, wenn ein neues Ereignisabonnement erstellt wird, sendet Event Grid ein Überprüfungsereignis an den Endpunkt mit einem `validationCode` in der Datennutzlast. Der Endpunkt muss diesen als Echo im Antworttext zurückgeben, um [nachzuweisen, dass der Endpunkt gültig ist und Sie der Besitzer sind](security-authentication.md#webhook-event-delivery). Wenn Sie einen [Event Grid-Trigger](../azure-functions/functions-bindings-event-grid.md) statt einer durch WebHook ausgelösten Funktion verwenden, erfolgt die Endpunktüberprüfung automatisch.

Verwenden Sie den folgenden Code zur Abonnementüberprüfung:

```csharp
<<<<<<< HEAD
=======
<<<<<<< HEAD
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688

using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{

    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
<<<<<<< HEAD
=======
=======
using System.Net; 
using Newtonsoft.Json; 
using Newtonsoft.Json.Linq; 
using Newtonsoft.Json.Serialization; 
using Microsoft.Azure.EventGrid.Models; 
 
class SubscriptionValidationEventData 
{ 
    public string ValidationCode { get; set; } 
} 
 
class SubscriptionValidationResponseData 
{ 
    public string ValidationResponse { get; set; } 
} 
 
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log) 
{ 

    log.Info($"C# HTTP trigger function begun");  
    string response = string.Empty; 
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent"; 
 
    string requestContent = await req.Content.ReadAsStringAsync(); 
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent); 
 
    foreach (EventGridEvent eventGridEvent in eventGridEvents) 
    { 
        JObject dataObject = eventGridEvent.Data as JObject; 
       
        // Deserialize the event data into the appropriate type based on event type 
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>(); 
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}"); 
            // Do any additional validation (as required) and then return back the below response 
            var responseData = new SubscriptionValidationResponseData(); 
            responseData.ValidationResponse = eventData.ValidationCode; 
            return req.CreateResponse(HttpStatusCode.OK, responseData);    
        }             
    } 
     
    return req.CreateResponse(HttpStatusCode.OK, response);     
} 
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688

```

```javascript
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
var http = require('http');

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";

    for (var events in req.body) {
        var body = req.body[events];
<<<<<<< HEAD
        // Deserialize the event data into the appropriate type based on event type
=======
<<<<<<< HEAD
        // Deserialize the event data into the appropriate type based on event type
=======
        // Deserialize the event data into the appropriate type based on event type  
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }
    }
    context.done();
<<<<<<< HEAD
};

=======
<<<<<<< HEAD
};

=======
}; 
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
```

### <a name="test-validation-response"></a>Testen der Überprüfungsantwort

Testen Sie die Funktion der Überprüfungsantwort, indem Sie das Beispielereignis in das Testfeld für die Funktion einfügen:

```json
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
```

Wenn Sie auf „Ausführen“ klicken, sollte die Ausgabe den Status „200 OK“ aufweisen und `{"ValidationResponse":"512d38b6-c7b8-40c8-89fe-f46f9e9622b6"}` im Text enthalten sein:

![Überprüfungsantwort](./media/receive-events/validation-response.png)

## <a name="handle-blob-storage-events"></a>Behandeln von Blob Storage-Ereignissen

Die Funktion kann jetzt auf die Behandlung von `Microsoft.Storage.BlobCreated` erweitert werden:

```cs
<<<<<<< HEAD
=======
<<<<<<< HEAD
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688

using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
<<<<<<< HEAD
=======
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type 
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");

            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>();
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
=======
using System.Net; 
using Newtonsoft.Json; 
using Newtonsoft.Json.Linq; 
using Newtonsoft.Json.Serialization; 
using Microsoft.Azure.EventGrid.Models; 
 
class SubscriptionValidationEventData 
{ 
    public string ValidationCode { get; set; } 
} 
 
class SubscriptionValidationResponseData 
{ 
    public string ValidationResponse { get; set; } 
} 
 
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log) 
{ 
    log.Info($"C# HTTP trigger function begun");  
    string response = string.Empty; 
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type 
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");

            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>();
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
    }

<<<<<<< HEAD
    return req.CreateResponse(HttpStatusCode.OK, response);
=======
        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>(); 
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}"); 
        }    
    } 
     
    return req.CreateResponse(HttpStatusCode.OK, response);     
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
}

```

```javascript
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
var http = require('http');

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type  
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }
    }
    context.done();
};
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
```

### <a name="test-blob-created-event-handling"></a>Testen der Behandlung durch Blobs erstellter Ereignisse

Testen Sie die neue Funktionalität der Funktion, indem Sie ein [Blob Storage-Ereignis](./event-schema-blob-storage.md#example-event) in das Testfeld einfügen und ausführen:

```json
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

Die Blob-URL sollte im Funktionsprotokoll ausgegeben werden:

![Ausgabeprotokoll](./media/receive-events/blob-event-response.png)

Sie können dies auch live testen, indem Sie ein Blob Storage-Konto oder ein Speicherkonto des Typs „General Purpose V2“ (GPv2) erstellen, [ein Ereignisabonnement hinzufügen](../storage/blobs/storage-blob-event-quickstart.md) und den Endpunkt auf die Funktions-URL festlegen:

![Funktions-URL](./media/receive-events/function-url.png)

## <a name="handle-custom-events"></a>Behandeln benutzerdefinierter Ereignisse

Abschließend wird die Funktion noch einmal erweitert, sodass auch benutzerdefinierte Ereignisse behandelt werden können. Es wird eine Überprüfung für das eigene Ereignis `Contoso.Items.ItemReceived` hinzugefügt. Der resultierende Code sollte wie folgt aussehen:

```cs
<<<<<<< HEAD
=======
<<<<<<< HEAD
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688

using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

class ContosoItemReceivedEventData
{
    public string id { get; set; }
    public string message { get; set; }
<<<<<<< HEAD
=======
    public string time { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    const string CustomTopicEvent = "Contoso.Items.ItemReceived";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>();
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }

        else if (string.Equals(eventGridEvent.EventType, CustomTopicEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<ContosoItemReceivedEventData>();
            log.Info($"Got ContosoItemReceived event data, item URI {eventData.id}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
=======
using System.Net; 
using Newtonsoft.Json; 
using Newtonsoft.Json.Linq; 
using Newtonsoft.Json.Serialization; 
using Microsoft.Azure.EventGrid.Models; 
 
class SubscriptionValidationEventData 
{ 
    public string ValidationCode { get; set; } 
} 
 
class SubscriptionValidationResponseData 
{ 
    public string ValidationResponse { get; set; } 
} 

class ContosoItemReceivedEventData 
{ 
    public string id { get; set; }     
    public string message { get; set; }   
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
    public string time { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    const string CustomTopicEvent = "Contoso.Items.ItemReceived";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>();
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }

<<<<<<< HEAD
        else if (string.Equals(eventGridEvent.EventType, CustomTopicEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<ContosoItemReceivedEventData>();
            log.Info($"Got ContosoItemReceived event data, item URI {eventData.id}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
=======
        else if (string.Equals(eventGridEvent.EventType, CustomTopicEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<ContosoItemReceivedEventData>(); 
            log.Info($"Got ContosoItemReceived event data, item URI {eventData.id}"); 
        } 
    } 
     
    return req.CreateResponse(HttpStatusCode.OK, response);     
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
}

```

```javascript
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
var http = require('http');
var t = require('tcomb');

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    var customEventType = "Contoso.Items.ItemReceived";
    var ContosoItemReceivedEventData = t.struct({
        id: t.Str,
        message: t.Str,
        time: t.Str,
    })

    for (var events in req.body) {
        var body = req.body[events];
<<<<<<< HEAD
        // Deserialize the event data into the appropriate type based on event type
=======
<<<<<<< HEAD
        // Deserialize the event data into the appropriate type based on event type
=======
        // Deserialize the event data into the appropriate type based on event type  
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }

        else if (body.data && body.eventType == customEventType) {
            var payload = new ContosoItemReceivedEventData(body.data);
            context.log("Relaying received custom payload:" + JSON.stringify(payload));
            context.log(payload instanceof ContosoItemReceivedEventData);
        }
    }
    context.done();
};
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
```

### <a name="test-custom-event-handling"></a>Testen der Behandlung benutzerdefinierter Ereignisse

Testen Sie zum Schluss, ob die erweiterte Funktion jetzt den benutzerdefinierten Ereignistyp behandeln kann:

```json
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
[{
    "subject": "Contoso/foo/bar/items",
    "eventType": "Microsoft.EventGrid.CustomEventType",
    "eventTime": "2017-08-16T01:57:26.005121Z",
    "id": "602a88ef-0001-00e6-1233-1646070610ea",
    "data": { 
            "id": "831e1650-001e-001b-66ab-eeb76e069631",
            "message": "Contoso item Foo",
            "time": "2017-06-26T18:41:00.9584103Z"
            },
    "dataVersion": "",
    "metadataVersion": "1"
}]
<<<<<<< HEAD

=======
<<<<<<< HEAD

=======
>>>>>>> 6def4612c80a1e9bab4008c57d68ccdbec8d0794
>>>>>>> bb0780f466c4ede2eb00246e1afe01b19bb40688
```

Sie können diese Funktionalität auch live testen, indem Sie [ein benutzerdefiniertes Ereignis mit CURL vom Portal senden](./custom-event-quickstart-portal.md) oder mithilfe von Diensten oder Anwendungen, die an einen Endpunkt posten können (z. B. [Postman](https://www.getpostman.com/)), [in einem benutzerdefinierten Thema posten](./post-to-custom-topic.md). Erstellen Sie ein benutzerdefiniertes Thema und ein Ereignisabonnement, wobei der Endpunkt als Funktions-URL festgelegt ist.

## <a name="next-steps"></a>Nächste Schritte

* Erkunden Sie die [Azure Event Grid-SDKs für Verwaltung und Veröffentlichung](./sdk-overview.md).
* Erfahren Sie, wie Sie [in einem benutzerdefinierten Thema posten](./post-to-custom-topic.md).
* Probieren Sie eines der umfassenden Tutorials zu Event Grid und Functions aus, z. B. [Ändern der Größe von in Blob Storage hochgeladenen Bildern](resize-images-on-storage-blob-upload-event.md).
