---
title: "Azure Functions – Table Storage-Bindungen"
description: Hier erfahren Sie, wie Sie Azure Table Storage-Bindungen in Azure Functions verwenden.
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: chrande
ms.openlocfilehash: 2f54df931d03318a50e9397211e3c50d0898556d
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="azure-functions-table-storage-bindings"></a>Azure Functions – Table Storage-Bindungen

In diesem Artikel erfahren Sie, wie Sie Azure Table Storage-Bindungen in Azure Functions verwenden. Azure Functions unterstützt Eingabe- und Ausgabebindungen für Azure-Tabellenspeicher.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="table-storage-input-binding"></a>Table Storage-Eingabebindung

Verwenden Sie die Azure Table Storage-Eingabebindung, um eine Tabelle in einem Azure Storage-Konto zu lesen.

## <a name="input---example"></a>Eingabe: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [Vorkompilierter C#-Code zum Lesen einer einzelnen Entität](#input---c-example-1)
* [Vorkompilierter C#-Code zum Lesen mehrerer Entitäten](#input---c-example-2)
* [C#-Skript: Lesen einer einzelnen Entität](#input---c-script-example-1)
* [C#-Skript: Lesen mehrerer Entitäten](#input---c-script-example-2)
* [F#](#input---f-example-2)
* [JavaScript](#input---javascript-example)

### <a name="input---c-example-1"></a>Eingabe: C#-Beispiel 1

Das folgende Beispiel zeigt [vorkompilierten C#-Code](functions-dotnet-class-library.md) zum Lesen einer einzelnen Tabellenzeile. 

Der Zeilenschlüsselwert „{queueTrigger}“ gibt an, dass der Zeilenschlüssel aus der Zeichenfolge der Warteschlangennachricht stammt.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        TraceWriter log)
    {
        log.Info($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}";
    }
}
```

### <a name="input---c-example-2"></a>Eingabe: C#-Beispiel 2

Das folgende Beispiel zeigt [vorkompilierten C#-Code](functions-dotnet-class-library.md) zum Lesen mehrerer Tabellenzeilen. Die Klasse `MyPoco` leitet sich von `TableEntity` ab.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        TraceWriter log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.Info($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}";
        }
    }
}
```

### <a name="input---c-script-example-1"></a>Eingabe: C#-Skriptbeispiel 1

Das folgende Beispiel zeigt eine Tabelleneingabebindung in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion verwendet einen Warteschlangentrigger zum Lesen einer einzelnen Tabellenzeile. 

Die Datei *function.json* gibt jeweils einen Wert für `partitionKey` und `rowKey` zurück. Der `rowKey`-Wert „{queueTrigger}“ gibt an, dass der Zeilenschlüssel aus der Zeichenfolge der Warteschlangennachricht stammt.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="input---c-script-example-2"></a>Eingabe: C#-Skriptbeispiel 2

Das folgende Beispiel zeigt eine Tabelleneingabebindung in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion liest Entitäten für einen Partitionsschlüssel, der in einer Warteschlangenmeldung angegeben ist.

Die Datei *function.json* sieht wie folgt aus:

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
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der C#-Skriptcode fügt einen Verweis auf das Azure Storage SDK hinzu, sodass der Entitätstyp von `TableEntity` abgeleitet werden kann:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="input---f-example"></a>Eingabe: F#-Beispiel

Das folgende Beispiel zeigt eine Tabelleneingabebindung in einer Datei vom Typ *function.json* sowie [F#-Skriptcode](functions-reference-fsharp.md), der die Bindung verwendet. Die Funktion verwendet einen Warteschlangentrigger zum Lesen einer einzelnen Tabellenzeile. 

Die Datei *function.json* gibt jeweils einen Wert für `partitionKey` und `rowKey` zurück. Der `rowKey`-Wert „{queueTrigger}“ gibt an, dass der Zeilenschlüssel aus der Zeichenfolge der Warteschlangennachricht stammt.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der F#-Code lautet wie folgt:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

### <a name="input---javascript-example"></a>Eingabe: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Tabelleneingabebindung in einer Datei vom Typ *function.json* sowie [JavaScript-Code] (functions-reference-node.md), der die Bindung verwendet. Die Funktion verwendet einen Warteschlangentrigger zum Lesen einer einzelnen Tabellenzeile. 

Die Datei *function.json* gibt jeweils einen Wert für `partitionKey` und `rowKey` zurück. Der `rowKey`-Wert „{queueTrigger}“ gibt an, dass der Zeilenschlüssel aus der Zeichenfolge der Warteschlangennachricht stammt.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

## <a name="input---attributes-for-precompiled-c"></a>Eingabe: Attribute für vorkompilierten C#-Code
 
Verwenden Sie für Funktionen mit [vorkompiliertem C#-Code](functions-dotnet-class-library.md) die folgenden Attribute, um eine Tabelleneingabebindung zu konfigurieren:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs) (definiert im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs))

  Der Konstruktor des Attributs akzeptiert den Tabellennamen, Partitionsschlüssel und Zeilenschlüssel. Er kann für einen out-Parameter oder für den Rückgabewert der Funktion verwendet werden, wie im folgenden Beispiel zu sehen:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      TraceWriter log)
  ```

  Durch Festlegen der Eigenschaft `Connection` können Sie das zu verwendende Speicherkonto angeben, wie im folgenden Beispiel zu sehen:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      TraceWriter log)
  ```

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) (definiert im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs))

  Eine weitere Möglichkeit zum Angeben des zu verwendenden Speicherkontos. Der Konstruktor akzeptiert den Namen einer App-Einstellung mit einer Speicherverbindungszeichenfolge. Das Attribut kann auf Parameter-, Methoden- oder Klassenebene angewendet werden. Das folgende Beispiel zeigt die Anwendung auf Klassen- und Methodenebene:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  ```

Das zu verwendende Speicherkonto wird anhand von Folgendem bestimmt (in der angegebenen Reihenfolge):

* Die Eigenschaft `Connection` des Attributs `Table`.
* Das Attribut `StorageAccount`, das auf den gleichen Parameter angewendet wird wie das Attribut `Table`.
* Das Attribut `StorageAccount`, das auf die Funktion angewendet wird.
* Das Attribut `StorageAccount`, das auf die Klasse angewendet wird.
* Das Standardspeicherkonto für die Funktions-App (App-Einstellung „AzureWebJobsStorage“).

## <a name="input---configuration"></a>Eingabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `Table` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |Beschreibung|
|---------|---------|----------------------|
|**type** | – | Muss auf `table` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen.|
|**direction** | – | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen. |
|**name** | – | Der Name der Variablen, die die Tabelle oder Entität im Funktionscode darstellt. | 
|**tableName** | **TableName** | Der Name der Tabelle.| 
|**partitionKey** | **PartitionKey** |Optional. Der Partitionsschlüssel der zu lesenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#input---usage).| 
|**rowKey** |**Zeilenschlüssel** | Optional. Der Zeilenschlüssel der zu lesenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#input---usage).| 
|**take** |**Take** | Optional. Die maximale Anzahl von Entitäten, die in JavaScript gelesen werden sollen. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#input---usage).| 
|**filter** |**Filter** | Optional. Ein OData-Filterausdruck für die Tabelleneingabe in JavaScript. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#input---usage).| 
|**connection** |**Connection** | Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Falls der Name der App-Einstellung mit „AzureWebJobs“ beginnt, können Sie hier nur den Rest des Namens angeben. Wenn Sie `connection` also beispielsweise auf „MyStorage“ festlegen, sucht die Functions-Laufzeit nach einer App-Einstellung namens „AzureWebJobsMyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.<br/>Wenn Sie lokal entwickeln, werden App-Einstellungen in den Werten der [Datei „local.settings.json“](functions-run-local.md#local-settings-file) gespeichert.|

## <a name="input---usage"></a>Eingabe: Verwendung

Die Table Storage-Eingabebindung unterstützt folgende Szenarien:

* **Lesen einer einzelnen Zeile in C# oder C#-Skript**

  Legen Sie `partitionKey` und `rowKey` fest. Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `T <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. `T` ist üblicherweise ein Typ, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Die Eigenschaften `filter` und `take` werden in diesem Szenario nicht verwendet. 

* **Lesen von Zeilen in C# oder C#-Skript**

  Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `IQueryable<T> <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Bei `T` muss es sich um einen Typ handeln, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Für ggf. erforderliche Filtervorgänge können Sie `IQueryable`-Methoden verwenden. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet.  

> [!NOTE]
> `IQueryable` kann nicht in .NET Core und somit auch nicht in der [Laufzeit von Functions v2](functions-versions.md) verwendet werden.

  Als Alternative kann beispielsweise ein `CloudTable paramName`-Methodenparameter verwendet werden, um die Tabelle unter Verwendung des Azure Storage SDKs zu lesen.

* **Lesen von Zeilen in JavaScript**

  Legen Sie die Eigenschaften `filter` und `take` fest. Legen Sie `partitionKey` oder `rowKey` nicht fest. Verwenden Sie `context.bindings.<name>`, um auf die Eingabetabellenentität(en) zuzugreifen. Die deserialisierten Objekte verfügen über die Eigenschaften `RowKey` und `PartitionKey`.

## <a name="table-storage-output-binding"></a>Table Storage-Ausgabebindung

Verwenden Sie eine Azure Table Storage-Ausgabebindung, um Entitäten in eine Tabelle in einem Azure Storage-Konto zu schreiben.

## <a name="output---example"></a>Ausgabe: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [Vorkompilierter C#-Code](#output---c-example)
* [C#-Skript](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Ausgabe: C#-Beispiel

Das folgende Beispiel zeigt [vorkompilierten C#-Code](functions-dotnet-class-library.md) mit einem HTTP-Trigger zum Schreiben einer einzelnen Tabellenzeile. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### <a name="output---c-script-example"></a>Ausgabe: C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Tabellenausgabebindung in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion schreibt mehrere Tabellenentitäten.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

### <a name="output---f-example"></a>Ausgabe: F#-Beispiel

Das folgende Beispiel zeigt eine Tabellenausgabebindung in einer Datei vom Typ *function.json* sowie [F#-Skriptcode](functions-reference-fsharp.md), der die Bindung verwendet. Die Funktion schreibt mehrere Tabellenentitäten.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der F#-Code lautet wie folgt:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### <a name="output---javascript-example"></a>Ausgabe: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Tabellenausgabebindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion schreibt mehrere Tabellenentitäten.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

## <a name="output---attributes-for-precompiled-c"></a>Ausgabe: Attribute für vorkompilierten C#-Code

 Verwenden Sie für Funktionen mit [vorkompiliertem C#-Code](functions-dotnet-class-library.md) das im NuGet-Paket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) definierte Attribut [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs).

Der Konstruktor des Attributs akzeptiert den Tabellennamen. Er kann für einen `out`-Parameter oder für den Rückgabewert der Funktion verwendet werden, wie im folgenden Beispiel zu sehen:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    TraceWriter log)
```

Durch Festlegen der Eigenschaft `Connection` können Sie das zu verwendende Speicherkonto angeben, wie im folgenden Beispiel zu sehen:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    TraceWriter log)
```

Mit dem Attribut `StorageAccount` können Sie das Speicherkonto auf Klassen-, Methoden- oder Parameterebene angeben. Weitere Informationen finden Sie unter [Eingabe: Attribute für vorkompilierten C#-Code](#input---attributes-for-precompiled-c).

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `Table` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |Beschreibung|
|---------|---------|----------------------|
|**type** | – | Muss auf `table` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen.|
|**direction** | – | Muss auf `out` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen. |
|**name** | – | Der Variablenname, der in Funktionscode zur Darstellung der Tabelle oder Entität verwendet wird. Legen Sie diesen Wert auf `$return` fest, um auf den Rückgabewert der Funktion zu verweisen.| 
|**tableName** |**TableName** | Der Name der Tabelle.| 
|**partitionKey** |**PartitionKey** | Der Partitionsschlüssel der zu schreibenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#output---usage).| 
|**rowKey** |**Zeilenschlüssel** | Der Zeilenschlüssel der zu schreibenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#output---usage).| 
|**connection** |**Connection** | Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Falls der Name der App-Einstellung mit „AzureWebJobs“ beginnt, können Sie hier nur den Rest des Namens angeben. Wenn Sie `connection` also beispielsweise auf „MyStorage“ festlegen, sucht die Functions-Laufzeit nach einer App-Einstellung namens „AzureWebJobsMyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.<br/>Wenn Sie lokal entwickeln, werden App-Einstellungen in den Werten der [Datei „local.settings.json“](functions-run-local.md#local-settings-file) gespeichert.|

## <a name="output---usage"></a>Ausgabe: Verwendung

Die Table Storage-Ausgabebindung unterstützt folgende Szenarien:

* **Schreiben einer einzelnen Zeile in einer beliebigen Sprache**

  Verwenden Sie in C# und C#-Skripts einen Methodenparameter wie `out T paramName` oder den Rückgabewert der Funktion, um auf die Ausgabetabellenentität zuzugreifen. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. `T` kann ein beliebiger serialisierbarer Typ sein, wenn der Partitionsschlüssel und der Zeilenschlüssel durch die Datei *function.json* oder das Attribut `Table` angegeben werden. Andernfalls muss `T` ein Typ sein, der die Eigenschaften `PartitionKey` und `RowKey` enthält. In diesem Szenario implementiert `T` in der Regel `ITableEntity` oder wird von `TableEntity` abgeleitet. Dies ist jedoch nicht zwingend erforderlich.

* **Schreiben von Zeilen in C# oder C#-Skript**

  Verwenden Sie in C# und C#-Skripts einen Methodenparameter vom Typ `ICollector<T> paramName` oder `ICollectorAsync<T> paramName`, um auf die Ausgabetabellenentität zuzugreifen. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. `T` gibt das Schema der hinzuzufügenden Entitäten an. In der Regel leitet sich `T` von `TableEntity` ab oder implementiert `ITableEntity`, dies ist aber nicht zwingend erforderlich. Die Partitionsschlüssel- und Zeilenschlüsselwerte in *function.json* oder im Konstruktor des Attributs `Table` werden in diesem Szenario nicht verwendet.

  Als Alternative kann beispielsweise ein `CloudTable paramName`-Methodenparameter verwendet werden, um unter Verwendung des Azure Storage SDKs in die Tabelle zu schreiben.

* **Schreiben von Zeilen in JavaScript**

  In JavaScript-Funktionen erfolgt der Zugriff auf die Tabellenausgabe mithilfe von `context.bindings.<name>`.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
