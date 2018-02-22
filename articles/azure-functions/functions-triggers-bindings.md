---
title: Trigger und Bindungen in Azure Functions
description: "Erfahren Sie, wie Sie Trigger und Bindungen in Azure Functions verwenden, um die Verbindung Ihrer Codeausführung mit Onlineereignissen und cloudbasierten Diensten herzustellen."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 90a192f58f0e4b285f7aece8a3555c08df051f38
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Konzepte für Azure Functions-Trigger und -Bindungen

Dieser Artikel ist eine konzeptionelle Übersicht über Trigger und Bindungen in Azure Functions. Hier werden die Funktionen beschrieben, die für alle Bindungen und alle unterstützten Sprachen identisch sind.

## <a name="overview"></a>Übersicht

Ein *Trigger* definiert, wie eine Funktion aufgerufen wird. Eine Funktion darf nur einen Trigger haben. Trigger haben zugeordnete Daten, in üblicherweise mit der Nutzlast identisch sind, von der die Funktion ausgelöst wurde.

Eingabe- und Ausgabe-*Bindungen* bieten eine deklarative Möglichkeit, aus Code heraus Verbindungen mit Daten herzustellen. Bindungen sind optional, und eine Funktion kann mehrere Eingabe- und Ausgabebindungen haben. 

Mithilfe von Triggern und Bindungen können Sie die Hartcodierung der Informationen zu den Diensten vermeiden, mit denen Sie arbeiten. Ihre Funktion empfängt Daten (z.B. den Inhalt einer Warteschlangennachricht) in Funktionsparametern. Sie senden Daten (z.B. um eine Warteschlangennachricht zu erstellen), indem Sie den Rückgabewert der Funktion, einen `out`-Parameter oder ein [Sammlerobjekt](functions-reference-csharp.md#writing-multiple-output-values) verwenden.

Wenn Sie Funktionen mithilfe des Azure-Portals entwickeln, werden Trigger und Bindungen in einer *function.json*-Datei konfiguriert. Das Portal stellt eine Benutzeroberfläche für diese Konfiguration zur Verfügung, Sie können die Datei jedoch direkt bearbeiten, indem Sie in den **erweiterten Editor** wechseln.

Wenn Sie beim Entwickeln von Funktionen Visual Studio verwenden, um eine Klassenbibliothek zu erstellen, konfigurieren Sie Trigger und Bindungen, indem Sie Methoden und Parameter mit Attributen ausstatten.

## <a name="supported-bindings"></a>Unterstützte Bindungen

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Informationen darüber, welche Bindungen sich in der Vorschauversion befinden oder für die Produktion zugelassen sind, finden Sie unter [Unterstützte Sprachen](supported-languages.md).

## <a name="example-queue-trigger-and-table-output-binding"></a>Beispiel: Triggerwarteschlange und Tabellenausgabebindung

Ein Beispiel: Sie möchten immer dann eine neue Zeile in Azure Table Storage schreiben, wenn in Azure Queue Storage eine neue Nachricht eintrifft. Dieses Szenario kann mit einem Azure Queue Storage-Trigger und einer Azure Table Storage-Ausgabebindung implementiert werden. 

Hier sehen Sie eine *function.json*-Datei für dieses Szenario. 

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Das erste Element im Array `bindings` ist der Queue Storage-Trigger. Die Eigenschaften `type` und `direction` identifizieren den Trigger. Die Eigenschaft `name` identifiziert den Funktionsparameter, der den Inhalt der Warteschlangennachricht empfangen wird. Der Name der zu überwachenden Warteschlange befindet sich in `queueName`, und die Verbindungszeichenfolge befindet sich in der App-Einstellung, die durch `connection` festgelegt ist.

Das zweite Element im Array `bindings` ist die Azure Table Storage-Ausgabebindung. Die Eigenschaften `type` und `direction` identifizieren die Bindung. Die Eigenschaft `name` gibt an, wie die Funktion die neue Tabellenzeile bereitstellt. In diesem Fall wird der Rückgabewert der Funktion verwendet. Der Name der Tabelle befindet sich in `tableName`, und die Verbindungszeichenfolge befindet sich in der App-Einstellung, die durch `connection` festgelegt ist.

Wenn Sie den Inhalt von *function.json* im Azure-Portal anzeigen und bearbeiten möchten, klicken Sie für die Funktion auf der Registerkarte **Integrieren** auf die Option **Erweiterter Editor**.

> [!NOTE]
> Beim Wert von `connection` handelt es sich nicht um die Verbindungszeichenfolge selbst, sondern um den Namen einer App-Einstellung, die die Verbindungszeichenfolge enthält. In Bindungen werden Verbindungszeichenfolgen in App-Einstellungen gespeichert, damit sichergestellt ist, dass *function.json* keine Geheimnisse eines Diensts enthält.

Folgender C#-Skriptcode arbeitet mit Triggern und Bindungen. Beachten Sie, dass der Parameter `order` den Inhalt der Warteschlangennachricht bereitstellt. Dieser Name ist erforderlich, da der Eigenschaftswert `name` in *function.json* `order` lautet. 

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

Die gleiche function.json-Datei kann mit einer JavaScript-Funktion verwendet werden:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

In einer Klassenbibliothek werden die gleichen Trigger- und Bindungsinformationen – Warteschlangen- und Tabellennamen, Speicherkonten sowie Funktionsparameter für Ein- und Ausgabe – durch Attribute bereitgestellt:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         TraceWriter log)
     {
         return new Person() {
                 PartitionKey = "Orders",
                 RowKey = Guid.NewGuid().ToString(),
                 Name = order["Name"].ToString(),
                 MobileNumber = order["MobileNumber"].ToString() };
     }
 }

 public class Person
 {
     public string PartitionKey { get; set; }
     public string RowKey { get; set; }
     public string Name { get; set; }
     public string MobileNumber { get; set; }
 }
```

## <a name="binding-direction"></a>Bindungsrichtung

Alle Trigger und Bindungen enthalten eine `direction`-Eigenschaft in der Datei *function.json*:

- Für Trigger ist die Richtung immer gleich `in`.
- Für Eingabe- und Ausgabebindungen werden `in` und `out` verwendet.
- Einige Bindungen unterstützen die spezielle Richtung `inout`. Wenn Sie `inout` verwenden, ist nur **Erweiterter Editor** auf der Registerkarte **Integrieren** verfügbar.

Wenn Sie zum Konfigurieren von Triggern und Bindungen [Attribute in einer Klassenbibliothek](functions-dotnet-class-library.md) verwenden, wird die Richtung in einem Attributkonstruktor angegeben oder aus dem Parametertyp abgeleitet.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Verwenden des Funktionsrückgabetyps, um eine einzelne Ausgabe zurückzugeben

Im vorhergehenden Beispiel wird gezeigt, wie der Funktionsrückgabewert verwendet wird, um eine Ausgabe für eine Bindung bereitzustellen, die in *function.json* durch den speziellen Wert `$return` für die Eigenschaft `name` festgelegt ist. (Dies wird nur in Sprachen unterstützt, die Rückgabewerte haben, z.B. C#, JavaScript und F#.) Hat eine Funktion mehrere Ausgabebindungen, verwenden Sie `$return` nur für eine der Ausgabebindungen. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

In den folgenden Beispielen wird gezeigt, wie Rückgabetypen mit Ausgabebindungen in C#, JavaScript und F# verwendet werden.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

```javascript
// JavaScript: return a value in the second parameter to context.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>Binden der „dataType“-Eigenschaft

Verwenden Sie in .NET den Parametertyp, um den Datentyp für die Eingabedaten zu definieren. Verwenden Sie zum Beispiel `string`, um eine Bindung mit dem Text eines Warteschlangentriggers zu erstellen – einem Bytearray zum Lesen im Binärformat und einem benutzerdefinierten Typ zum Deserialisieren in einem POCO-Objekt.

Für dynamisch typisierte Sprachen wie JavaScript verwenden Sie die Eigenschaft `dataType` in der *function.json*-Datei. Um z.B. den Inhalt einer HTTP-Anforderung im Binärformat zu lesen, legen Sie `dataType` auf `binary` fest:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Andere Optionen für `dataType` sind `stream` und `string`.

## <a name="resolving-app-settings"></a>Auflösen von App-Einstellungen

Es hat sich bewährt, Geheimnisse und Verbindungszeichenfolgen nicht in Konfigurationsdateien, sondern über App-Einstellungen zu verwalten. Dies schränkt den Zugriff auf diese Geheimnisse ein und bewirkt, dass es hinsichtlich der Sicherheit unbedenklich ist, *function.json* in einem öffentlichen Quellcodeverwaltungs-Repository zu speichern.

App-Einstellungen sind auch nützlich, wenn Sie die jeweilige Konfiguration entsprechend der Umgebung ändern möchten. Beispielsweise kann es sein, dass Sie in einer Testumgebung eine andere Warteschlange oder einen anderen Blob Storage-Container überwachen möchten.

App-Einstellungen werden immer dann aufgelöst, wenn ein Wert in Prozentzeichen steht, etwa `%MyAppSetting%`. Die `connection`Eigenschaft von Triggern und Bindungen ist ein Sonderfall, denn für sie werden Werte automatisch als App-Einstellungen aufgelöst. 

Im folgenden Beispiel ist ein Azure Queue Storage-Trigger gezeigt, in dem die App-Einstellung `%input-queue-name%` verwendet wird, um die Warteschlange anzugeben, für die die Auslösung erfolgen werden soll.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Sie können den gleichen Ansatz auch in Klassenbibliotheken verwenden:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-metadata-properties"></a>Metadateneigenschaften von Triggern

Viele Trigger stellen zusätzlich zur Datennutzlast (z. B. die Warteschlangennachricht, von der eine Funktion ausgelöst wurde) weitere Metadatenwerte bereit. Diese Werte können als Eingabeparameter in C# und F# oder als Eigenschaften für das `context.bindings`-Objekt in JavaScript verwendet werden. 

Beispielsweise unterstützt ein Azure Queue Storage-Trigger die folgenden Eigenschaften:

* QueueTrigger – Auslösen von Nachrichteninhalt, wenn gültige Zeichenfolge
* DequeueCount
* ExpirationTime
* id
* InsertionTime
* NextVisibleTime
* PopReceipt

Der Zugriff auf diese Metadatenwerte ist über die *function.json*-Dateieigenschaften möglich. Angenommen, Sie verwenden einen Warteschlangentrigger und die Warteschlangennachricht enthält den Namen eines Blobs, den Sie lesen möchten. In der *function.json*-Datei können Sie die Metadateneigenschaft `queueTrigger` in der Blobeigenschaft `path` verwenden, wie im folgenden Beispiel gezeigt:

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Details der Metadateneigenschaften für jeden Trigger sind im entsprechenden Referenzartikel beschrieben. Ein Beispiel finden Sie unter [Warteschlangentrigger-Metadaten](functions-bindings-storage-queue.md#trigger---message-metadata). Dokumentation ist auch im Portal auf der Registerkarte **Integrieren** im Abschnitt **Dokumentation** verfügbar, der sich unter dem Bereich für Bindungskonfigurationen befindet.  

## <a name="binding-expressions-and-patterns"></a>Bindungsausdrücke und Muster

Eines der leistungsstärksten Merkmale von Triggern und Bindungen sind *Bindungsausdrücke*. In der Konfiguration für eine Bindung können Sie Musterausdrücke definieren, die dann in anderen Bindungen oder in Code verwendet werden können. Triggermetadaten können auch in Bindungsausdrücken verwendet werden, wie im vorherigen Abschnitt gezeigt.

Angenommen, Sie möchten die Größe der Bilder in einem bestimmten Blobspeichercontainer ähnlich ändern wie mit der Vorlage für Bildgrößenänderung (**Image Resizer**) auf der Seite **Neue Funktion** des Azure-Portals (siehe das Szenario **Beispiele**). 

So sieht die *function.json*-Definition aus:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Beachten Sie, dass der Parameter `filename` sowohl in der Definition des Blobtriggers als auch in der Blobausgabebindung verwendet wird. Dieser Parameter kann auch in Funktionscode verwendet werden.

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->

Sie haben auch die Möglichkeit, Bindungsausdrücke und -muster für Attribute in Klassenbibliotheken zu verwenden. Hier ist z.B. eine Funktion zur Bildgrößenänderung in einer Klassenbibliothek:

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```

### <a name="create-guids"></a>Erstellen von GUIDs

Der Bindungsausdruck `{rand-guid}` erstellt eine GUID. Im folgenden Beispiel wird eine GUID verwendet, um einen eindeutigen Blobnamen zu generieren: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Die aktuelle Zeit

Der Bindungsausdruck `DateTime` wird in `DateTime.UtcNow` aufgelöst.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties"></a>Binden an benutzerdefinierte Eingabeeigenschaften

In Bindungsausdrücken kann auch auf Eigenschaften verwiesen werden, die in der Triggernutzlast definiert sind. Beispielsweise könnte es sein, dass Sie über einen Dateinamen, der in einem Webhook bereitgestellt wird, ein dynamisches Binden an eine Blob Storage-Datei vornehmen möchten.

In der folgenden *function.json*-Datei wird beispielsweise eine Eigenschaft namens `BlobName` aus der Triggernutzlast verwendet:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson"
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

Um dies in C# und F# zu implementieren, müssen Sie eine POCO-Entität definieren, in der die Felder definiert sind, die in der Triggernutzlast deserialisiert werden.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

In JavaScript wird JSON-Deserialisierung automatisch ausgeführt, und Sie können die Eigenschaften direkt verwenden.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a>Konfigurieren von Bindungsdaten zur Laufzeit

In C# und anderen .NET-Sprachen können Sie ein imperatives Bindungsmuster verwenden, im Gegensatz zu den deklarativen Bindungen in *function.json* und Attributen. Imperative Bindung eignet sich, wenn Bindungsparameter zur Laufzeit statt zur Entwurfszeit berechnet werden müssen. Weitere Informationen finden Sie in der C#-Referenz für Entwickler unter [Bindung zur Laufzeit durch imperative Bindungen](functions-reference-csharp.md#imperative-bindings).

## <a name="functionjson-file-schema"></a>function.json-Dateischema

Das *function.json*-Dateischema finden Sie unter [http://json.schemastore.org/function](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Beheben von Bindungsfehlern

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

Links zu allen relevanten Fehlerthemen für die unterschiedlichen von Functions unterstützten Dienste finden Sie im Abschnitt [Bindungsfehlercodes](functions-bindings-error-pages.md#binding-error-codes) des Übersichtsthemas [Fehlerbehandlung bei Azure Functions](functions-bindings-error-pages.md).  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu einer bestimmten Bindung finden Sie in den folgenden Artikeln:

- [HTTP und Webhooks](functions-bindings-http-webhook.md)
- [Zeitgeber](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Event Hub](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-cosmosdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Externe Datei](functions-bindings-external-file.md)
