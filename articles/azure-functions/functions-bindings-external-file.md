---
title: "Bindungen zu externen Dateien für Azure Functions (experimentell)"
description: Verwenden von Bindungen zu externen Dateien in Azure Functions
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/27/2017
ms.author: alkarche
ms.openlocfilehash: 4e9c2c336df465d7488de84bd2a02cc5d9e42f30
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="azure-functions-external-file-bindings-experimental"></a>Azure Functions – Bindungen zu externen Dateien (experimentell)
In diesem Artikel wird gezeigt, wie Dateien aus unterschiedlichen SaaS-Anbietern (z. B. Dropbox oder Google Drive) in Azure Functions verarbeitet werden können. Azure Functions unterstützt Trigger-, Eingabe- und Ausgabebindungen für externe Dateien. Diese Bindungen erstellen API-Verbindungen mit SaaS-Anbietern oder verwenden vorhandene API-Verbindungen aus der Ressourcengruppe Ihrer Funktionen-App.

> [!IMPORTANT]
> Die Bindungen zu externen Dateien sind experimentell und erreichen möglicherweise nie den Status „Allgemein verfügbar“. Die Bindungen sind nur in Azure Functions 1.x enthalten, und es ist nicht geplant, sie zu Azure Functions 2.x hinzuzufügen. Für Szenarien, in denen Zugriff auf Daten in SaaS-Anbietern erforderlich ist, empfiehlt sich das Verwenden von [Logik-Apps, aus denen Funktionen aufgerufen werden](functions-twitter-email.md). Weitere Informationen hierzu finden Sie unter [Dateisystem-Connector für Logik-Apps](../logic-apps/logic-apps-using-file-connector.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="available-file-connections"></a>Verfügbare Dateiverbindungen

|Connector|Trigger|Eingabe|Output|
|:-----|:---:|:---:|:---:|
|[Box](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive for Business](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google Drive](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Verbindungen mit externen Dateien können auch in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list) verwendet werden.

## <a name="trigger"></a>Trigger

Der Trigger für externe Dateien ermöglicht es Ihnen, einen Remoteordner zu überwachen und Ihren Funktionscode auszuführen, wenn Änderungen erkannt werden.

## <a name="trigger---example"></a>Trigger: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#-Skript](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-script-example"></a>Trigger: C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Triggerbindung für eine externe Datei in einer *function.json*-Datei sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), in der die Bindung verwendet wird. In der Funktion wird der Inhalt jeder Datei protokolliert, die dem überwachten Ordner hinzugefügt wurde.

Bindungsdaten in der Datei *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

### <a name="trigger---javascript-example"></a>Trigger: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung für eine externe Datei in einer *function.json*-Datei sowie eine [JavaScript-Funktion](functions-reference-node.md), in der die Bindung verwendet wird. In der Funktion wird der Inhalt jeder Datei protokolliert, die dem überwachten Ordner hinzugefügt wurde.

Bindungsdaten in der Datei *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

## <a name="trigger---configuration"></a>Trigger: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen.

|Eigenschaft von „function.json“ | BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | Muss auf `apiHubFileTrigger` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | Der Name der Variablen, die das Ereigniselement im Funktionscode darstellt. | 
|**Verbindung**| Kennzeichnet die App-Einstellung, in der die Verbindungszeichenfolge gespeichert wird. Die App-Einstellung wird automatisch erstellt, wenn Sie im Azure-Portal in der Benutzeroberfläche für „Integrieren“ eine Verbindung hinzufügen.|
|**path** | Der zu überwachende Ordner und optional ein Namensmuster.|

### <a name="name-patterns"></a>Namensmuster

Sie können in der `path`-Eigenschaft ein Dateinamensmuster angeben. Der Ordner, auf den verwiesen wird, muss im SaaS-Anbieter vorhanden sein.

Beispiele:

```json
"path": "input/original-{name}",
```

Mit diesem Pfad wird eine Datei namens *original-File1.txt* im Ordner *input* gefunden, und der Wert der Variablen `name` im Funktionscode lautet `File1.txt`.

Ein weiteres Beispiel:

```json
"path": "input/{filename}.{fileextension}",
```

Mit diesem Pfad wird ebenfalls eine Datei namens *original-File1.txt* gefunden, und die Werte der Variablen `filename` und `fileextension` im Funktionscode lauten *original-File1* und *txt*.

Sie können den Dateityp von Dateien beschränken, indem Sie einen festen Wert als Dateierweiterung verwenden. Beispiel: 

```json
"path": "samples/{name}.png",
```

In diesem Fall wird die Funktion nur für *.png*-Dateien im Ordner *samples* ausgelöst.

Geschweifte Klammern werden in Namensmustern als Sonderzeichen angesehen. Sie können die geschweiften Klammern verdoppeln, um einen Dateinamen anzugeben, der geschweifte Klammern enthält.
Beispiel: 

```json
"path": "images/{{20140101}}-{name}",
```

Mit diesem Pfad wird eine Datei namens *{20140101}-soundfile.mp3* im Ordner *images* gefunden, und der Wert der Variablen `name` im Funktionscode lautet *soundfile.mp3*.

## <a name="trigger---usage"></a>Trigger: Verwendung

In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung zu Eingabedateidaten her.
Dabei ist `T` der Datentyp, in den Sie die Daten deserialisieren möchten, und `paramName` ist der Name, den Sie im [JSON-Code des Triggers](#trigger) angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabedateidaten zu.

Die Datei kann in jeden der folgenden Typen deserialisiert werden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-serialisierte Dateidaten.
  Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren (z.B. `FooType`), versucht Azure Functions, die JSON-Daten in den angegebenen Typ zu deserialisieren.
* Zeichenfolge – nützlich für Textdateidaten.

In C#-Funktionen können Sie auch eine Bindung zu jedem der folgenden Typen erstellen. Die Functions-Runtime versucht dann, die Dateidaten mithilfe dieses Typs zu deserialisieren:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

<!--- ## Trigger - file receipts
The Azure Functions runtime makes sure that no external file trigger function gets called more than once for the same new or updated file.
It does so by maintaining *file receipts* to determine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in the Azure storage account for your function app
(specified by the `AzureWebJobsStorage` app setting). A file receipt has the following information:

* The triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* The folder name
* The file type ("BlockFile" or "PageFile")
* The file name
* The ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

To force reprocessing of a file, delete the file receipt for that file from the *azure-webjobs-hosts* folder manually.
--->

## <a name="trigger---poison-files"></a>Trigger: nicht verarbeitbare Dateien

Wenn beim Ausführen einer Triggerfunktion für externe Dateien ein Fehler auftritt, versucht Azure Functions standardmäßig bis zu fünf Mal (einschließlich des ersten Versuchs), diese Funktion für die jeweilige Datei auszuführen.
Wenn alle fünf Versuche fehlschlagen, fügt Functions eine Nachricht zu einer Storage-Warteschlange namens *webjobs-apihubtrigger-poison* hinzu. Die Warteschlangennachricht für nicht verarbeitbare Dateien ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *&lt;Funktionen-App-Name>*.Functions.*&lt;Funktionsname>*)
* FileType
* FolderName
* FileName
* ETag (eine Dateiversions-ID. Beispiel: „0x8D1DC6E70A277EF“)

## <a name="input"></a>Eingabe

Die Azure-Eingabebindung zu externen Dateien ermöglicht es Ihnen, in Ihrer Funktion eine Datei aus einem externen Ordner zu verwenden.

## <a name="input---example"></a>Eingabe: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#-Skript](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Eingabe: C#-Skriptbeispiel

Das folgende Beispiel zeigt Eingabe- und Ausgabebindungen für externe Dateien in einer *function.json*-Datei sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), in der die Bindung verwendet wird. In der Funktion wird eine Eingabedatei in eine Ausgabedatei kopiert.

Bindungsdaten in der Datei *function.json*:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

### <a name="input---javascript-example"></a>Eingabe: JavaScript-Beispiel

Das folgende Beispiel zeigt Eingabe- und Ausgabebindungen für externe Dateien in einer *function.json*-Datei sowie eine [JavaScript-Funktion](functions-reference-node.md), in der die Bindung verwendet wird. In der Funktion wird eine Eingabedatei in eine Ausgabedatei kopiert.

Bindungsdaten in der Datei *function.json*:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="input---configuration"></a>Eingabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen.

|Eigenschaft von „function.json“ | BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | Muss auf `apiHubFile` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | Der Name der Variablen, die das Ereigniselement im Funktionscode darstellt. | 
|**Verbindung**| Kennzeichnet die App-Einstellung, in der die Verbindungszeichenfolge gespeichert wird. Die App-Einstellung wird automatisch erstellt, wenn Sie im Azure-Portal in der Benutzeroberfläche für „Integrieren“ eine Verbindung hinzufügen.|
|**path** | Muss den Ordnernamen und den Dateinamen enthalten. Wenn Sie in Ihrer Funktion beispielsweise über einen [Warteschlangentrigger](functions-bindings-storage-queue.md) verfügen, können Sie mit `"path": "samples-workitems/{queueTrigger}"` auf eine Datei im Ordner `samples-workitems` mit einem Namen verweisen, der mit dem in der Triggernachricht angegebenen Dateinamen übereinstimmt.   

## <a name="input---usage"></a>Eingabe: Verwendung

In C#-Funktionen stellen Sie mit einem benannten Parameter in Ihrer Funktionssignatur, z.B. `<T> <name>`, eine Bindung zu Eingabedateidaten her. `T` ist der Datentyp, in den Sie die Daten deserialisieren möchten, und `name` ist der Name, den Sie in der Eingabebindung angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Eingabedateidaten zu.

Die Datei kann in jeden der folgenden Typen deserialisiert werden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-serialisierte Dateidaten.
  Wenn Sie einen benutzerdefinierten Eingabetyp deklarieren (z.B. `InputType`), versucht Azure Functions, die JSON-Daten in den angegebenen Typ zu deserialisieren.
* Zeichenfolge – nützlich für Textdateidaten.

In C#-Funktionen können Sie auch eine Bindung zu jedem der folgenden Typen erstellen. Die Functions-Runtime versucht dann, die Dateidaten mithilfe dieses Typs zu deserialisieren:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="output"></a>Output

Die Azure-Ausgabebindung zu externen Dateien ermöglicht es Ihnen, in Ihrer Funktion Dateien in einen externen Ordner zu schreiben.

## <a name="output---example"></a>Ausgabe: Beispiel

Lesen Sie das [Beispiel für eine Eingabebindung](#input---example).

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen.

|Eigenschaft von „function.json“ | BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | Muss auf `apiHubFile` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | Muss auf `out` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | Der Name der Variablen, die das Ereigniselement im Funktionscode darstellt. | 
|**Verbindung**| Kennzeichnet die App-Einstellung, in der die Verbindungszeichenfolge gespeichert wird. Die App-Einstellung wird automatisch erstellt, wenn Sie im Azure-Portal in der Benutzeroberfläche für „Integrieren“ eine Verbindung hinzufügen.|
|**path** | Muss den Ordnernamen und den Dateinamen enthalten. Wenn Sie in Ihrer Funktion beispielsweise über einen [Warteschlangentrigger](functions-bindings-storage-queue.md) verfügen, können Sie mit `"path": "samples-workitems/{queueTrigger}"` auf eine Datei im Ordner `samples-workitems` mit einem Namen verweisen, der mit dem in der Triggernachricht angegebenen Dateinamen übereinstimmt.   

## <a name="output---usage"></a>Ausgabe: Verwendung

In C#-Funktionen erstellen Sie eine Bindung zu einer Ausgabedatei, indem Sie den benannten `out`-Parameter in Ihrer Funktionssignatur verwenden, etwa `out <T> <name>`. Hierbei ist `T` der Datentyp, in den Sie die Daten serialisieren möchten, und `name` ist der Name, den Sie in der Ausgabebindung angegeben haben. In Node.js-Funktionen greifen Sie mit `context.bindings.<name>` auf die Ausgabedatei zu.

Sie können in die Ausgabedatei schreiben, indem Sie einen der folgenden Typen verwenden:

* Beliebiges [Objekt](https://msdn.microsoft.com/library/system.object.aspx) – nützlich für JSON-Serialisierung.
  Wenn Sie einen benutzerdefinierten Ausgabetyp deklarieren (z.B. `out OutputType paramName`), versucht Azure Functions, das Objekt in JSON zu serialisieren. Wenn der Ausgabeparameter bei Beendigung der Funktion einen Nullwert hat, erstellt die Functions-Runtime eine Datei als NULL-Objekt.
* Zeichenfolge – (`out string paramName`) nützlich für Textdateidaten. Die Functions-Runtime erstellt nur dann eine Datei, wenn der Zeichenfolgenparameter bei Rückgabe durch die Funktion keinen Nullwert enthält.

In C#-Funktionen können Sie auch eine Ausgabe in einen der folgenden Typen erstellen:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
