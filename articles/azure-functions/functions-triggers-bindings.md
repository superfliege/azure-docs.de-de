---
title: Trigger und Bindungen in Azure Functions
description: Erfahren Sie, wie Sie Trigger und Bindungen in Azure Functions verwenden, um die Verbindung Ihrer Codeausführung mit Onlineereignissen und cloudbasierten Diensten herzustellen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/07/2018
ms.author: glenga
ms.openlocfilehash: d008e94186dd3c5a18c92617ebddfffa966358d5
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
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

## <a name="register-binding-extensions"></a>Registrieren von Bindungserweiterungen

In Version 2.x der Azure Functions Runtime müssen Sie die in Ihrer Funktions-App verwendeten [Bindungserweiterungen](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/README.md) explizit registrieren. 

Erweiterungen werden als NuGet-Pakete bereitgestellt. Der Paketnamen beginnt in der Regel mit [microsoft.azure.webjobs.extensions](https://www.nuget.org/packages?q=microsoft.azure.webjobs.extensions).  Das Installieren und Registrieren Ihrer Bindungserweiterungen hängt davon ab, wie Sie Ihre Funktionen entwickeln: 

+ [Lokal in C# mit Visual Studio oder VS Code](#local-c-development-using-visual-studio-or-vs-code)
+ [Lokal mit Azure Functions Core Tools](#local-development-azure-functions-core-tools)
+ [Im Azure-Portal](#azure-portal-development) 

Es gibt einen Kernsatz von Bindungen in Version 2.x, die nicht als Erweiterungen bereitgestellt werden. Für die folgenden Trigger und Bindungen müssen Sie Erweiterungen nicht registrieren: HTTP, Timer und Azure Storage. 

Informationen dazu, wie Sie eine Functions-App so einrichten, dass sie die Version 2.x der Functions Runtime verwendet, finden Sie unter [Einstellen von Runtimeversionen von Azure Functions als Ziel](set-runtime-version.md). Version 2.x der Functions Runtime ist derzeit als Vorschau verfügbar. 

Die in diesem Abschnitt gezeigten Paketversionen dienen nur als Beispiele. Auf der [NuGet.org-Website](https://www.nuget.org/packages?q=microsoft.azure.webjobs.extensions) können Sie ermitteln, welche Version einer bestimmten Erweiterung für die anderen Abhängigkeiten in Ihrer Funktions-App erforderlich sind.    

###  <a name="local-c-development-using-visual-studio-or-vs-code"></a>Lokale Entwicklung in C# mit Visual Studio oder Visual Studio-Code 

Wenn Sie Funktionen lokal in C# mit Visual Studio oder Visual Studio Code entwickeln, müssen Sie lediglich das NuGet-Paket für die Erweiterung hinzufügen. 

+ **Visual Studio**: Verwenden Sie die NuGet-Paket-Manager-Tools. Der folgende Befehl [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) installiert die Azure Cosmos DB-Erweiterung aus der Paket-Manager-Konsole:

    ```
    Install-Package Microsoft.Azure.WebJobs.Extensions.CosmosDB -Version 3.0.0-beta6 
    ```
+ **Visual Studio Code**: Sie können Pakete über die Eingabeaufforderung installieren. Verwenden Sie dazu wie folgt den Befehl [dotnet add package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) in der .NET-Befehlszeilenschnittstelle:

    ```
    dotnet add package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version 3.0.0-beta6 
    ```

### <a name="local-development-azure-functions-core-tools"></a>Lokale Entwicklung mit Azure Functions Core Tools

[!INCLUDE [Full bindings table](../../includes/functions-core-tools-install-extension.md)]

### <a name="azure-portal-development"></a>Entwicklung im Azure-Portal

Wenn Sie eine Funktion erstellen oder einer vorhandenen Funktion eine Bindung hinzufügen, erhalten Sie möglicherweise eine Meldung, dass zum Hinzufügen der Erweiterung für den Trigger bzw. die Bindung eine Registrierung erforderlich ist.   

Nachdem eine Warnung zur Installation der spezifischen Erweiterung angezeigt wurde, klicken Sie auf **Installieren**, um die Erweiterung zu registrieren. Sie müssen jede Erweiterung nur ein Mal für eine bestimmte Funktions-App installieren. 

>[!Note] 
>Der Installationsvorgang im Portal kann bis zu 10 Minuten eines Verbrauchstarifs aufbrauchen.

## <a name="example-trigger-and-binding"></a>Beispiele für Trigger und Bindungen

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

Das erste Element im Array `bindings` ist der Queue Storage-Trigger. Die Eigenschaften `type` und `direction` identifizieren den Trigger. Die Eigenschaft `name` identifiziert den Funktionsparameter, der den Inhalt der Warteschlangennachricht empfängt. Der Name der zu überwachenden Warteschlange befindet sich in `queueName`, und die Verbindungszeichenfolge befindet sich in der App-Einstellung, die durch `connection` festgelegt ist.

Das zweite Element im Array `bindings` ist die Azure Table Storage-Ausgabebindung. Die Eigenschaften `type` und `direction` identifizieren die Bindung. Die Eigenschaft `name` gibt an, wie die Funktion die neue Tabellenzeile bereitstellt – in diesem Fall mit dem Rückgabewert der Funktion. Der Name der Tabelle befindet sich in `tableName`, und die Verbindungszeichenfolge befindet sich in der App-Einstellung, die durch `connection` festgelegt ist.

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

In einer Klassenbibliothek werden die gleichen Trigger- und Bindungsinformationen (Warteschlangen- und Tabellennamen, Speicherkonten sowie Funktionsparameter für Ein- und Ausgabe) durch Attribute anstatt über eine function.json-Datei bereitgestellt: Hier sehen Sie ein Beispiel:

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

## <a name="using-the-function-return-value"></a>Verwenden des Funktionsrückgabewerts

In Sprachen mit Rückgabewert können Sie eine Ausgabebindung an den Rückgabewert binden:

* Wenden Sie in einer C#-Klassenbibliothek das Attribut der Ausgabebindung auf den Rückgabewert der Methode an.
* In anderen Sprachen legen Sie die Eigenschaft `name` in *function.json* auf `$return` fest.

Wenn Sie mehrere Elemente schreiben müssen, verwenden Sie anstatt des Rückgabewerts ein [Sammlerobjekt](functions-reference-csharp.md#writing-multiple-output-values). Wenn mehrere Ausgabebindungen vorhanden sind, verwenden Sie den Rückgabewert für nur eine davon.

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#c-example)
* [C#-Skript (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-Beispiel

Hier sehen Sie C#-Code, der den Rückgabewert für eine Ausgabebindung gefolgt von einem asynchronen Beispiel verwendet:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="c-script-example"></a>C#-Skriptbeispiel

Hier sehen Sie die Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Hier sehen Sie den C#-Skriptcode gefolgt von einem asynchronen Beispiel:

```cs
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="f-example"></a>F#-Beispiel

Hier sehen Sie die Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Der F#-Code lautet wie folgt:

```fsharp
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

### <a name="javascript-example"></a>JavaScript-Beispiel

Hier sehen Sie die Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

In JavaScript befindet sich der Rückgabewert im zweiten Parameter für `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
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

## <a name="binding-expressions-and-patterns"></a>Bindungsausdrücke und Muster

Eines der leistungsstärksten Merkmale von Triggern und Bindungen sind *Bindungsausdrücke*. In der Datei *function.json*, in Funktionsparametern und in Code können Sie Ausdrücke verwenden, die mit Werten aus verschiedenen Quellen aufgelöst werden.

Die meisten Ausdrücke sind von geschweiften Klammern umschlossen. Beispielsweise wird in einer Trigger-Funktion für die Eingabewarteschlange `{queueTrigger}` in den Text der Warteschlangenmeldung aufgelöst. Lautet die Eigenschaft `path` für eine Blobausgabebindung `container/{queueTrigger}`, und wird die Funktion durch eine Warteschlangennachricht `HelloWorld` ausgelöst, so wird ein Blob mit dem Namen `HelloWorld` erstellt.

Arten von Bindungsausdrücken

* [App-Einstellungen](#binding-expressions---app-settings)
* [Name der Triggerdatei](#binding-expressions---trigger-file-name)
* [Metadaten für Trigger](#binding-expressions---trigger-metadata)
* [JSON-Nutzlasten](#binding-expressions---json-payloads)
* [Neue GUID](#binding-expressions---create-guids)
* [Aktuelles Datum und Uhrzeit](#binding-expressions---current-time)

### <a name="binding-expressions---app-settings"></a>Bindungsausdrücke – App-Einstellungen

Es hat sich bewährt, Geheimnisse und Verbindungszeichenfolgen nicht in Konfigurationsdateien, sondern über App-Einstellungen zu verwalten. Dies schränkt den Zugriff auf diese Geheimnisse ein und ermöglicht das sichere Speichern von Dateien wie *function.json* in öffentlichen Repositorys zur Quellcodeverwaltung.

App-Einstellungen sind auch nützlich, wenn Sie die jeweilige Konfiguration entsprechend der Umgebung ändern möchten. Beispielsweise kann es sein, dass Sie in einer Testumgebung eine andere Warteschlange oder einen anderen Blob Storage-Container überwachen möchten.

Bindungsausdrücke für App-Einstellungen werden anders dargestellt als andere Bindungsausdrücke: Sie sind von Prozentzeichen anstatt von geschweiften Klammern umschlossen. Wenn der Pfad der Blobausgabebindung z.B. `%Environment%/newblob.txt` und der Wert der App-Einstellung `Environment` lautet `Development`, wird ein Blob im Container `Development` erstellt.

Beim lokalen Ausführen einer Funktion werden die App-Einstellungen aus der Datei *local.settings.json* verwendet.

Die Eigenschaft `connection` von Triggern und Bindungen ist ein Sonderfall, denn für sie werden Werte automatisch als App-Einstellungen ohne Prozentzeichen aufgelöst. 

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

### <a name="binding-expressions---trigger-file-name"></a>Bindungsausdrücke – Name der Triggerdateiname

Der `path` für einen Blob-Trigger kann ein Muster sein, mit dem Sie auf den Namen des auslösenden Blobs in anderen Bindungen und Funktionscode verweisen. Das Muster kann auch Filterkriterien enthalten, die angeben, welche Blobs einen Funktionsaufruf auslösen können.

Das Muster `path` für die Blobtriggerbindung im folgenden Beispiel ist `sample-images/{filename}`. Sie erstellt einen Bindungsausdruck mit dem Namen `filename`:

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
    ...
```

Der Ausdruck `filename` kann dann in einer Ausgabebindung verwendet werden, um den Namen des erstellten Blobs anzugeben:

```json
    ...
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

Der Funktionscode hat Zugriff auf diesen Wert, da er `filename` als Parametername verwendet:

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

Sie haben auch die Möglichkeit, Bindungsausdrücke und -muster für Attribute in Klassenbibliotheken zu verwenden. Im folgenden Beispiel sind die Parameter des Attributkonstruktors dieselben `path`-Werte wie in den vorigen Beispielen für *function.json*: 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    TraceWriter log)
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
}

```

Sie können auch Ausdrücke für Teile des Dateinamens erstellen, z.B. die Erweiterung. Weitere Informationen zum Verwenden von Ausdrücken und Mustern in der Blob-Pfadzeichenfolge finden Sie im Artikel zu [Azure Blob Storage-Bindungen](functions-bindings-storage-blob.md).
 
### <a name="binding-expressions---trigger-metadata"></a>Bindungsausdrücke – Metadaten für Trigger

Viele Trigger stellen zusätzlich zur Datennutzlast (z.B. der Inhalt der Warteschlangennachricht, von der eine Funktion ausgelöst wurde) weitere Metadatenwerte bereit. Diese Werte können als Eingabeparameter in C# und F# oder als Eigenschaften für das `context.bindings`-Objekt in JavaScript verwendet werden. 

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

### <a name="binding-expressions---json-payloads"></a>Bindungsausdrücke – JSON-Nutzlasten

Wenn eine Triggernutzlast JSON ist, finden Sie die Eigenschaften in der Konfiguration für andere Bindungen in der gleichen Funktion und im gleichen Funktionscode.

Das folgende Beispiel zeigt die Datei *function.json* für eine Webhook-Funktion, die einen Blobnamen im JSON-Format erhält: `{"BlobName":"HelloWorld.txt"}`. Eine Blobeingabebindung liest den Blob, und die HTTP-Ausgabebindung gibt den Blob-Inhalt in der HTTP-Antwort zurück. Beachten Sie, dass die Blob-Eingabebindung den Blobnamen abruft, indem sie direkt auf die `BlobName`-Eigenschaft verweist (`"path": "strings/{BlobName}"`)

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
      "path": "strings/{BlobName.FileName}.{BlobName.Extension}",
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

Damit das in C# und F# funktioniert, benötigen Sie eine Klasse, die die zu deserialisierenden Felder im folgenden Beispiel definiert:

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

In JavaScript wird JSON-Deserialisierung automatisch ausgeführt.

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

#### <a name="dot-notation"></a>Punktnotation

Wenn einige der Eigenschaften in Ihrer JSON-Nutzlast Objekte mit Eigenschaften sind, finden Sie diese durch die Punktnotation. Nehmen wir beispielsweise an, dass Ihr JSON wie folgt aussieht:

```json
{"BlobName": {
  "FileName":"HelloWorld",
  "Extension":"txt"
  }
}
```

Sie können direkt auf `FileName` verweisen: `BlobName.FileName`. In diesem JSON-Format sieht die Eigenschaft `path` im vorherigen Beispiel folgendermaßen aus:

```json
"path": "strings/{BlobName.FileName}.{BlobName.Extension}",
```

In C# benötigen Sie zwei Klassen:

```csharp
public class BlobInfo
{
    public BlobName BlobName { get; set; }
}
public class BlobName
{
    public string FileName { get; set; }
    public string Extension { get; set; }
}
```

### <a name="binding-expressions---create-guids"></a>Bindungsausdrücke – Erstellen von GUIDs

Der Bindungsausdruck `{rand-guid}` erstellt eine GUID. Der folgende Blobpfad in einer `function.json`-Datei erstellt einen Blob mit einem Namen wie *50710cb5-84b9 - 4D 87 9d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="binding-expressions---current-time"></a>Bindungsausdrücke – Aktuelle Uhrzeit

Der Bindungsausdruck `DateTime` wird in `DateTime.UtcNow` aufgelöst. Der folgende Blobpfad in einer `function.json`-Datei erstellt einen Blob mit einem Namen wie *2018-02-16T17-59-55Z.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="binding-at-runtime"></a>Binden zur Laufzeit

In C# und anderen .NET-Sprachen können Sie ein imperatives Bindungsmuster verwenden, im Gegensatz zu den deklarativen Bindungen in *function.json* und Attributen. Imperative Bindung eignet sich, wenn Bindungsparameter zur Laufzeit statt zur Entwurfszeit berechnet werden müssen. Weitere Informationen finden Sie in der [C#-Entwicklerreferenz ](functions-dotnet-class-library.md#binding-at-runtime) oder in der [C#-Skriptentwicklerreferenz](functions-reference-csharp.md#binding-at-runtime).

## <a name="functionjson-file-schema"></a>function.json-Dateischema

Das Dateischema *function.json* steht unter [http://json.schemastore.org/function](http://json.schemastore.org/function) zur Verfügung.

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
