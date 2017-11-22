---
title: "Azure Functions – Blob Storage-Bindungen"
description: Hier erfahren Sie, wie Sie Azure Blob Storage-Trigger und -Bindungen in Azure Functions verwenden.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2017
ms.author: glenga
ms.openlocfilehash: e0c608fe3a80c9d704774e592a1eba383bbdffe8
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Azure Functions – Blob Storage-Bindungen

In diesem Artikel erfahren Sie, wie Sie Azure Blob Storage-Bindungen in Azure Functions verwenden. Azure Functions unterstützt Trigger-, Eingabe- und Ausgabebindungen für Blobs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> [Reine Blob Storage-Konten](../storage/common/storage-create-storage-account.md#blob-storage-accounts) werden nicht unterstützt. Blob Storage-Trigger und -Bindungen erfordern ein allgemeines Speicherkonto. 

## <a name="blob-storage-trigger"></a>Blob Storage-Trigger

Verwenden Sie einen Blob Storage-Trigger, um eine Funktion zu starten, wenn ein neues oder aktualisiertes Blob erkannt wird. Der Blob-Inhalt wird als Eingabe für die Funktion bereitgestellt.

> [!NOTE]
> Bei Verwendung eines Blobtriggers in einem Verbrauchsplan kann es bis zu 10 Minuten dauern, bis neue Blobs verarbeitet werden, nachdem eine Funktions-App in den Leerlauf gewechselt ist. Sobald die Funktions-App ausgeführt wird, werden die Blobs sofort verarbeitet. Um diese anfängliche Verzögerung zu vermeiden, sollten Sie eine der folgenden Möglichkeiten erwägen:
> - Verwenden Sie einen App Service-Plan mit aktivierter Option „Always On“.
> - Verwenden Sie einen anderen Mechanismus zum Auslösen der Blobverarbeitung, z.B. eine Warteschlangennachricht, die den Blobnamen enthält. Ein Beispiel finden Sie weiter unten in diesem Artikel im [Beispiel für Blobeingabe-/-ausgabebindungen](#input--output---example).

## <a name="trigger---example"></a>Trigger: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [Vorkompilierter C#-Code](#trigger---c-example)
* [C#-Skript](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger: C#-Beispiel

Der [vorkompilierte C#-Code](functions-dotnet-class-library.md) im folgenden Beispiel schreibt ein Protokoll, wenn im Container `samples-workitems` ein Blob hinzugefügt oder aktualisiert wird.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Weitere Informationen zum Attribut `BlobTrigger` finden Sie unter [Trigger: Attribute für vorkompilierten C#-Code](#trigger---attributes-for-precompiled-c).

### <a name="trigger---c-script-example"></a>Trigger: C#-Skriptbeispiel

Das folgende Beispiel zeigt einen Blobtrigger in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion schreibt ein Protokoll, wenn im Container `samples-workitems` ein Blob hinzugefügt oder aktualisiert wird.

Bindungsdaten in der Datei *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

C#-Skriptcode für die Bindung an `Stream`:

```cs
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

C#-Skriptcode für die Bindung an `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

### <a name="trigger---javascript-example"></a>Trigger: JavaScript-Beispiel

Das folgende Beispiel zeigt einen Blobtrigger in einer Datei vom Typ *function.json* sowie [JavaScript-Code] (functions-reference-node.md), der die Bindung verwendet. Die Funktion schreibt ein Protokoll, wenn im Container `samples-workitems` ein Blob hinzugefügt oder aktualisiert wird.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#trigger---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

## <a name="trigger---attributes-for-precompiled-c"></a>Trigger: Attribute für vorkompilierten C#-Code

Verwenden Sie für Funktionen mit [vorkompiliertem C#-Code](functions-dotnet-class-library.md) die folgenden Attribute, um einen Blobtrigger zu konfigurieren:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobTriggerAttribute.cs) (definiert im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs))

  Der Konstruktor des Attributs akzeptiert eine Pfadzeichenfolge, die den zu überwachenden Container angibt, und optional ein [Blobnamensmuster](#trigger---blob-name-patterns). Hier sehen Sie ein Beispiel:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image, 
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  ```

  Durch Festlegen der Eigenschaft `Connection` können Sie das zu verwendende Speicherkonto angeben, wie im folgenden Beispiel zu sehen:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image, 
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  ```

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) (definiert im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs))

  Eine weitere Möglichkeit zum Angeben des zu verwendenden Speicherkontos. Der Konstruktor akzeptiert den Namen einer App-Einstellung mit einer Speicherverbindungszeichenfolge. Das Attribut kann auf Parameter-, Methoden- oder Klassenebene angewendet werden. Das folgende Beispiel zeigt die Anwendung auf Klassen- und Methodenebene:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("BlobTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  ```

Das zu verwendende Speicherkonto wird anhand von Folgendem bestimmt (in der angegebenen Reihenfolge):

* Die Eigenschaft `Connection` des Attributs `BlobTrigger`.
* Das Attribut `StorageAccount`, das auf den gleichen Parameter angewendet wird wie das Attribut `BlobTrigger`.
* Das Attribut `StorageAccount`, das auf die Funktion angewendet wird.
* Das Attribut `StorageAccount`, das auf die Klasse angewendet wird.
* Das Standardspeicherkonto für die Funktions-App (App-Einstellung „AzureWebJobsStorage“).

## <a name="trigger---configuration"></a>Trigger: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `BlobTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |Beschreibung|
|---------|---------|----------------------|
|**type** | – | Muss auf `blobTrigger` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | – | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. Ausnahmen sind im Abschnitt [Verwendung](#trigger---usage) angegeben. |
|**name** | – | Der Name der Variablen, die das Blob im Funktionscode darstellt. | 
|**path** | **BlobPath** |Der zu überwachende Container.  Kann ein [Blobnamensmuster](#trigger-blob-name-patterns) sein. | 
|**connection** | **Connection** | Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Falls der Name der App-Einstellung mit „AzureWebJobs“ beginnt, können Sie hier nur den Rest des Namens angeben. Wenn Sie `connection` also beispielsweise auf „MyStorage“ festlegen, sucht die Functions-Laufzeit nach einer App-Einstellung namens „AzureWebJobsMyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.<br><br>Bei der Verbindungszeichenfolge muss es sich um eine Verbindungszeichenfolge für ein allgemeines Speicherkonto (nicht für ein [reines Blob Storage-Konto](../storage/common/storage-create-storage-account.md#blob-storage-accounts)) handeln.<br>Wenn Sie lokal entwickeln, werden App-Einstellungen in den Werten der [Datei „local.settings.json“](functions-run-local.md#local-settings-file) gespeichert.|

## <a name="trigger---usage"></a>Trigger: Verwendung

Verwenden Sie in C# und C#-Skripts einen Methodenparameter wie `Stream paramName`, um auf die Blobdaten zuzugreifen. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Eine Bindung kann mit folgenden Typen erstellt werden:

* `TextReader`
* `Stream`
* `ICloudBlob` (erfordert die Bindungsrichtung „inout“ in *function.json*)
* `CloudBlockBlob` (erfordert die Bindungsrichtung „inout“ in *function.json*)
* `CloudPageBlob` (erfordert die Bindungsrichtung „inout“ in *function.json*)
* `CloudAppendBlob` (erfordert die Bindungsrichtung „inout“ in *function.json*)

Wie angemerkt muss für einige dieser Typen in *function.json* die Bindungsrichtung `inout` angegeben werden. Diese Richtung wird vom Standard-Editor im Azure-Portal nicht unterstützt, sodass Sie den erweiterten Editor verwenden müssen.

Wenn Textblobs erwartet werden, kann für die Bindung der .NET-Typ `string` verwendet werden. Dies wird nur empfohlen, wenn die Blobgröße klein ist, da der gesamte Blobinhalt in den Arbeitsspeicher geladen wird. Im Allgemeinen ist es günstiger, die Typen `Stream` oder `CloudBlockBlob` zu verwenden.

Verwenden Sie in JavaScript `context.bindings.<name>`, um auf die Eingabeblobdaten zuzugreifen.

## <a name="trigger---blob-name-patterns"></a>Trigger: Blobnamensmuster

Ein Blobnamensmuster kann in der Eigenschaft `path` (in *function.json*) oder im Konstruktor des Attributs `BlobTrigger` angegeben werden. Das Namensmuster kann ein [Filter oder Bindungsausdruck](functions-triggers-bindings.md#binding-expressions-and-patterns) sein.

### <a name="filter-on-blob-name"></a>Filtern nach Blobname

Im folgenden Beispiel wird der Trigger nur für Blobs im Container `input` ausgelöst, deren Name mit „original-“ beginnt:

```json
"path": "input/original-{name}",
```
 
Wenn der Blobname *original-Blob1.txt* lautet, hat die Variable `name` im Funktionscode den Wert `Blob1`.

### <a name="filter-on-file-type"></a>Filtern nach Dateityp

Im folgenden Beispiel wird der Trigger nur für *PNG-Dateien* ausgelöst:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Filtern nach geschweiften Klammern in Dateinamen

Wenn in Dateinamen nach geschweiften Klammern gesucht werden soll, müssen doppelte Klammern verwendet werden. Im folgenden Beispiel wird nach Blobs gefiltert, deren Name geschweifte Klammern enthält:

```json
"path": "images/{{20140101}}-{name}",
```

Wenn der Blobname *{20140101}-soundfile.mp3* lautet, erhält die Variable `name` im Funktionscode den Wert *soundfile.mp3*. 

### <a name="get-file-name-and-extension"></a>Abrufen von Dateiname und Erweiterung

Das folgende Beispiel zeigt, wie Sie den Blobdateinamen und die Erweiterung separat binden:

```json
"path": "input/{blobname}.{blobextension}",
```
Wenn das Blob *original-Blob1.txt* heißt, lauten die Werte der Variablen `blobname` und `blobextension` im Funktionscode *original-Blob1* und *txt*.

## <a name="trigger---metadata"></a>Trigger: Metadaten

Der Blobtrigger stellt mehrere Metadateneigenschaften bereit. Diese Eigenschaften können als Teil der Bindungsausdrücke in anderen Bindungen oder als Parameter im Code verwendet werden. Diese Werte haben die gleiche Semantik wie der Typ [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).


|Eigenschaft  |Typ  |Beschreibung  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Der Pfad des auslösenden Blobs.|
|`Uri`|`System.Uri`|Der Blob-URI für den primären Speicherort|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties)|Die Systemeigenschaften des Blobs |
|`Metadata` |`IDictionary<string,string>`|Die benutzerdefinierten Metadaten für das Blob|

## <a name="trigger---blob-receipts"></a>Trigger: Blobbelege

Die Azure Functions-Runtime stellt sicher, dass Blobtriggerfunktionen für ein neues oder aktualisiertes Blob nicht mehrmals aufgerufen werden. Zu diesem Zweck wird mittels Verwaltung der *Blobbelege* ermittelt, ob eine bestimmte Blobversion verarbeitet wurde.

Azure Functions speichert Blobbelege in einem Container mit dem Namen *azure-webjobs-hosts* im Azure Storage-Konto für Ihre Funktions-App (per App-Einstellung `AzureWebJobsStorage` definiert). Ein Blobbeleg enthält die folgenden Informationen:

* Die ausgelöste Funktion („*&lt;Funktions-App-Name>*.Functions.*&lt;Funktionsname>*“, z.B. „MyFunctionApp.Functions.CopyBlob“)
* Containername
* Blobtyp ("BlockBlob" oder "PageBlob")
* Blobname
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

Um eine erneute Verarbeitung eines Blobs zu erzwingen, können Sie den Blobbeleg für dieses Blob manuell aus dem Container *azure-webjobs-hosts* löschen.

## <a name="trigger---poison-blobs"></a>Trigger: Nicht verarbeitbare Blobs

Wenn bei Ausführung einer Blobtriggerfunktion für ein Blob ein Fehler auftritt, versucht Azure Functions standardmäßig bis zu fünfmal, diese Funktion für ein bestimmtes Blob auszuführen. 

Wenn bei allen fünf Versuchen Fehler auftreten, fügt Azure Functions der Storage-Warteschlange *webjobs-blobtrigger-poison* eine Nachricht hinzu. Die Warteschlangennachricht für nicht verarbeitbare Blobs ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *&lt;Funktionen-App-Name>*.Functions.*&lt;Funktionsname>*)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (eine Blobversions-ID. Beispiel: 0x8D1DC6E70A277EF)

## <a name="trigger---polling-for-large-containers"></a>Trigger: Abfragen für große Container

Wenn der überwachte Blobcontainer mehr als 10.000 Blobs enthält, überprüft die Functions-Runtime Protokolldateien auf neue oder geänderte Blobs. Dieser Vorgang kann zu Verzögerungen führen. Eine Funktion wird unter Umständen erst mehrere Minuten nach der Bloberstellung oder noch später ausgelöst. Außerdem erfolgt das [Erstellen von Storage-Protokollen auf bestmögliche Weise](/rest/api/storageservices/About-Storage-Analytics-Logging). Es gibt keine Garantie, dass alle Ereignisse erfasst werden. Unter bestimmten Umständen können Protokolle fehlen. Wenn eine schnellere oder zuverlässigere Blobverarbeitung erforderlich ist, sollten Sie beim Erstellen des Blobs eine [Warteschlangennachricht](../storage/queues/storage-dotnet-how-to-use-queues.md) erstellen. Verwenden Sie dann einen [Warteschlangentrigger](functions-bindings-storage-queue.md) anstelle eines Blobtriggers für die Verarbeitung des Blobs. Eine andere Möglichkeit ist die Verwendung von Event Grid. Weitere Informationen finden Sie im Tutorial [Automatisieren der Größenänderung von hochgeladenen Bildern mit Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md).

## <a name="blob-storage-input--output-bindings"></a>Eingabe- und Ausgabebindungen für Blob Storage

Verwenden Sie Eingabe- und Ausgabebindungen für Blob Storage zum Lesen und Schreiben von Blobs.

## <a name="input--output---example"></a>Eingabe und Ausgabe: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [Vorkompilierter C#-Code](#input--output---c-example)
* [C#-Skript](#input--output---c-script-example)
* [JavaScript](#input--output---javascript-example)

### <a name="input--output---c-example"></a>Eingabe und Ausgabe: C#-Beispiel

In der Funktion mit [vorkompiliertem C#-Code](functions-dotnet-class-library.md) des folgenden Beispiels werden eine Eingabeblobbindung und zwei Ausgabeblobbindungen verwendet. Die Funktion wird durch die Erstellung eines Bildblobs im Container *sample-images* ausgelöst. Sie erstellt kleine und mittelgroße Kopien der Bildblobs. 

```csharp
[FunctionName("ResizeImage")]
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

### <a name="input--output---c-script-example"></a>Eingabe und Ausgabe: C#-Skriptbeispiel

Das folgende Beispiel zeigt einen Blobtrigger in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion erstellt eine Kopie eines Blobs. Sie wird durch eine Warteschlangennachricht ausgelöst, die den Namen des zu kopierenden Blobs enthält. Der Name des neuen Blobs lautet *{Name des Originalblobs}-Copy*.

In der Datei *function.json* wird die Metadateneigenschaft `queueTrigger` verwendet, um den Blobnamen in den Eigenschaften vom Typ `path` anzugeben:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input--output---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```cs
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="input--output---javascript-example"></a>Eingabe und Ausgabe: JavaScript-Beispiel

Das folgende Beispiel zeigt einen Blobtrigger in einer Datei vom Typ *function.json* sowie [JavaScript-Code] (functions-reference-node.md), der die Bindung verwendet. Die Funktion erstellt eine Kopie eines Blobs. Sie wird durch eine Warteschlangennachricht ausgelöst, die den Namen des zu kopierenden Blobs enthält. Der Name des neuen Blobs lautet *{Name des Originalblobs}-Copy*.

In der Datei *function.json* wird die Metadateneigenschaft `queueTrigger` verwendet, um den Blobnamen in den Eigenschaften vom Typ `path` anzugeben:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input--output---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="input--output---attributes-for-precompiled-c"></a>Eingabe und Ausgabe: Attribute für vorkompilierten C#-Code

Verwenden Sie für Funktionen mit [vorkompiliertem C#-Code](functions-dotnet-class-library.md) das im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) definierte Attribut [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs).

Der Konstruktor des Attributs akzeptiert den Pfad des Blobs sowie einen Parameter vom Typ `FileAccess`, der angibt, ob es sich um einen Lesevorgang oder um einen Schreibvorgang handelt. Beispiel:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
```

Durch Festlegen der Eigenschaft `Connection` können Sie das zu verwendende Speicherkonto angeben, wie im folgenden Beispiel zu sehen:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
```

Mit dem Attribut `StorageAccount` können Sie das Speicherkonto auf Klassen-, Methoden- oder Parameterebene angeben. Weitere Informationen finden Sie unter [Trigger: Attribute für vorkompilierten C#-Code](#trigger---attributes-for-precompiled-c).

## <a name="input--output---configuration"></a>Eingabe und Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `Blob` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |Beschreibung|
|---------|---------|----------------------|
|**type** | – | Muss auf `blob` festgelegt sein. |
|**direction** | – | Muss für eine Eingabebindung auf `in` und für eine Ausgabebindung auf „out“ festgelegt werden. Ausnahmen sind im Abschnitt [Verwendung](#input--output---usage) angegeben. |
|**name** | – | Der Name der Variablen, die das Blob im Funktionscode darstellt.  Legen Sie diesen Wert auf `$return` fest, um auf den Rückgabewert der Funktion zu verweisen.|
|**path** |**BlobPath** | Der Pfad des Blobs. | 
|**connection** |**Connection**| Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Falls der Name der App-Einstellung mit „AzureWebJobs“ beginnt, können Sie hier nur den Rest des Namens angeben. Wenn Sie `connection` also beispielsweise auf „MyStorage“ festlegen, sucht die Functions-Laufzeit nach einer App-Einstellung namens „AzureWebJobsMyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.<br><br>Bei der Verbindungszeichenfolge muss es sich um eine Verbindungszeichenfolge für ein allgemeines Speicherkonto (nicht für ein [reines Blob Storage-Konto](../storage/common/storage-create-storage-account.md#blob-storage-accounts)) handeln.<br>Wenn Sie lokal entwickeln, werden App-Einstellungen in den Werten der [Datei „local.settings.json“](functions-run-local.md#local-settings-file) gespeichert.|
|– | **Access** | Gibt an, ob Sie einen Lesevorgang oder einen Schreibvorgang ausführen möchten. |

## <a name="input--output---usage"></a>Eingabe und Ausgabe: Verwendung

Verwenden Sie in vorkompiliertem C#-Code und in C#-Skripts einen Methodenparameter wie `Stream paramName`, um auf das Blob zuzugreifen. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Eine Bindung kann mit folgenden Typen erstellt werden:

* `out string`
* `TextWriter` 
* `TextReader`
* `Stream`
* `ICloudBlob` (erfordert die Bindungsrichtung „inout“ in *function.json*)
* `CloudBlockBlob` (erfordert die Bindungsrichtung „inout“ in *function.json*)
* `CloudPageBlob` (erfordert die Bindungsrichtung „inout“ in *function.json*)
* `CloudAppendBlob` (erfordert die Bindungsrichtung „inout“ in *function.json*)

Wie angemerkt muss für einige dieser Typen in *function.json* die Bindungsrichtung `inout` angegeben werden. Diese Richtung wird vom Standard-Editor im Azure-Portal nicht unterstützt, sodass Sie den erweiterten Editor verwenden müssen.

Beim Lesen von Textblobs kann eine Bindung mit einem `string`-Typ erstellt werden. Dies wird nur bei kleinen Blobs empfohlen, da der gesamte Blobinhalt in den Arbeitsspeicher geladen wird. Im Allgemeinen ist es günstiger, die Typen `Stream` oder `CloudBlockBlob` zu verwenden.

Verwenden Sie in JavaScript `context.bindings.<name>`, um auf die Blobdaten zuzugreifen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Funktion, die durch Azure Blob Storage ausgelöst wird](functions-create-storage-blob-triggered-function.md)

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
