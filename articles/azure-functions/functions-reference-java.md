---
title: Java-Entwicklerreferenz zu Azure Functions | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe von Java Funktionen entwickeln können."
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: azure functions, funktionen, ereignisverarbeitung, webhooks, dynamisches computing, serverlose architektur, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/20/2017
ms.author: routlaw
ms.openlocfilehash: dc9a1b6061c41cd623e1ddb3bb9dbb87530a13d5
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2017
---
# <a name="azure-functions-java-developer-guide"></a>Java-Entwicklerhandbuch für Azure Functions
> [!div class="op_single_selector"]
[!INCLUDE [functions-selector-languages](../../includes/functions-selector-languages.md)]

## <a name="programming-model"></a>Programmiermodell 

Ihre Azure-Funktion sollte eine zustandslose Klassenmethode sein, die Eingaben verarbeitet und Ausgaben erzeugt. Obwohl Sie Instanzmethoden schreiben dürfen, darf Ihre Funktion nicht von Instanzfeldern der Klasse abhängig sein. Alle Funktionsmethoden benötigen einen Zugriffsmodifizierer des Typs `public`.

## <a name="triggers-and-annotations"></a>Trigger und Anmerkungen

Eine Azure-Funktion wird in der Regel aufgrund eines externen Triggers aufgerufen. Ihre Funktion muss diesen Trigger und die zugehörigen Eingaben verarbeiten und mindestens eine Ausgabe erstellen.

Java-Anmerkungen sind im Paket `azure-functions-java-core` zur Bindung von Eingaben und Ausgaben an Ihre Methoden enthalten. Die folgende Tabelle enthält die unterstützten Eingabetrigger und Anmerkungen zur Ausgabebindung:

Bindung | Anmerkung
---|---
CosmosDB | N/V
HTTP | <ul><li>`HttpTrigger`</li><li>`HttpOutput`</li></ul>
Mobile Apps | N/V
Notification Hubs | N/V
Speicherblob | <ul><li>`BlobTrigger`</li><li>`BlobOutput`</li><li>`BlobOutput`</li></ul>
Speicherwarteschlange | <ul><li>`QueueTrigger`</li><li>`QueueOutput`</li></ul>
Speichertabelle | <ul><li>`TableInput`</li><li>`TableOutput`</li></ul>
Timer | <ul><li>`TimerTrigger`</li></ul>
Twilio | N/V

Triggereingaben und -ausgaben können auch in [function.json](/azure/azure-functions/functions-reference#function-code) für Ihre Anwendung definiert werden.

> [!IMPORTANT] 
> Sie müssen ein Azure Storage-Konto in [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) konfigurieren, um Trigger für Azure Storage Blob, Azure Queue Storage oder Azure Table Storage lokal auszuführen.

Beispiel für die Verwendung von Anmerkungen:

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Die gleiche Funktion ohne Anmerkungen:

```java
package com.example;

public class MyClass {
    public static String echo(String in) {
        return in;
    }
}
```

Mit der entsprechenden Datei `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="data-types"></a>Datentypen

Sie können alle Datentypen in Java für die Eingabe- und Ausgabedaten verwenden, native Typen inbegriffen, sowie benutzerdefinierte Java-Typen und spezialisierte Azure-Typen, die im Paket `azure-functions-java-core` definiert sind. Die Azure Functions-Runtime versucht, die empfangene Eingabe in den von Ihrem Code angeforderten Typ zu konvertieren.

### <a name="strings"></a>Zeichenfolgen

In Funktionsmethoden übergebene Werte werden in Zeichenfolgen umgewandelt, wenn der entsprechende Eingabeparameter für die Funktion vom Typ `String` ist. 

### <a name="plain-old-java-objects-pojos"></a>Plain Old Java Objects (POJOs, „ganz normale“ Java-Objekte)

Mit JSON formatierte Zeichenfolgen werden in Java-Typen umgewandelt, wenn die Eingabe der Funktionsmethode diesen Java-Typ erwartet. Durch diese Konvertierung können Sie JSON-Eingaben in Ihre Funktionen übergeben und mit Java-Typen in Ihrem Code arbeiten, ohne die Konvertierung in Ihrem eigenen Code implementieren zu müssen.

POJO-Typen, die als Eingaben für Funktionen verwendet werden, müssen den gleichen Zugriffsmodifizierer des Typs `public` aufweisen wie die Funktionsmethoden, in denen sie verwendet werden. Sie müssen POJO-Klassenfelder nicht als `public` deklarieren. Die JSON-Zeichenfolge `{ "x": 3 }` kann beispielsweise in den folgenden POJO-Typ konvertiert werden:

```Java
public class MyData {
    private int x;
}
```

### <a name="binary-data"></a>Binärdaten

Binäre Daten werden in Ihrem Azure-Funktionscode als `byte[]` dargestellt. Binden Sie binäre Eingaben oder Ausgaben an Ihre Funktionen, indem Sie das Feld `dataType` in der Datei „function.json“ auf `binary` festlegen:

```json
 {
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "blob",
      "name": "content",
      "direction": "in",
      "dataType": "binary",
      "path": "container/myfile.bin",
      "connection": "ExampleStorageAccount"
    },
  ]
}
```

Verwenden Sie anschließend Folgendes in Ihrem Funktionscode:

```java
// Class definition and imports are omitted here
public static String echoLength(byte[] content) {
}
```

Verwenden Sie den Typ `OutputBinding<byte[]>` für eine binäre Ausgabebindung.


## <a name="function-method-overloading"></a>Überladung von Funktionsmethoden

Sie dürfen Funktionsmethoden überladen, die den gleichen Namen haben, aber unterschiedliche Typen aufweisen. In einer Klasse können beispielsweise `String echo(String s)` und `String echo(MyType s)` enthalten sein. Die Azure Functions-Runtime entscheidet dann, welche Funktionsmethode aufgerufen werden soll, indem sie den tatsächlichen Eingabetyp überprüft (bei der HTTP-Eingabe führt der MIME-Typ `text/plain` zu `String`, während `application/json` `MyType` darstellt).

## <a name="inputs"></a>Eingaben

Eingaben werden in Azure Functions in zwei Kategorien unterteilt: die Triggereingabe und die zusätzliche Eingabe. Obwohl sie sich in der Datei `function.json` unterscheiden, ist die Verwendung im Java-Code identisch. Der folgende Codeausschnitt soll als Beispiel dienen:

```java
package com.example;

import com.microsoft.azure.serverless.functions.annotation.BindingName;

public class MyClass {
    public static String echo(String in, @BindingName("item") MyObject obj) {
        return "Hello, " + in + " and " + obj.getKey() + ".";
    }

    private static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

Die Anmerkung `@BindingName` akzeptiert eine Eigenschaft des Typs `String`, die den Namen der bzw. des in der Datei `function.json` definierten Bindung/Triggers darstellt:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "put" ],
      "route": "items/{id}"
    },
    {
      "type": "table",
      "name": "item",
      "direction": "in",
      "tableName": "items",
      "partitionKey": "Example",
      "rowKey": "{id}",
      "connection": "ExampleStorageAccount"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```

Wenn diese Funktion aufgerufen wird, übergibt die HTTP-Anforderungsnutzlast `String` für das Argument `in`. Zudem wird ein `MyObject`-Typ von Azure Table Storage an das Argument `obj` übergeben.

## <a name="outputs"></a>Ausgaben

Ausgaben können sowohl im Rückgabewert als auch in Ausgabeparametern angegeben werden. Wenn es nur eine Ausgabe gibt, wird empfohlen, den Rückgabewert zu verwenden. Bei mehreren Ausgaben müssen Sie Ausgabeparameter verwenden.

Der Rückgabewert ist die einfachste Ausgabeform. Sie geben nur den Wert eines beliebigen Typs zurück, und die Azure Functions-Runtime versucht, den tatsächlichen Typ zurückzumarshallen (z.B. eine HTTP-Antwort). In der Datei `functions.json` verwenden Sie `$return` als Namen der Ausgabebindung.

Mit dem Typ `OutputBinding<T>`, der im Paket `azure-functions-java-core` definiert ist, können mehrere Ausgabewerte erzeugt werden. Wenn Sie eine HTTP-Antwort erstellen und eine Nachricht mithilfe von Push in eine Warteschlange übertragen müssen, können Sie Folgendes schreiben:

```java
package com.example;

import com.microsoft.azure.serverless.functions.OutputBinding;
import com.microsoft.azure.serverless.functions.annotation.BindingName;

public class MyClass {
    public static String echo(String body, 
    @QueueOutput(queueName = "messages", connection = "AzureWebJobsStorage", name = "queue") OutputBinding<String> queue) {
        String result = "Hello, " + body + ".";
        queue.setValue(result);
        return result;
    }
}
```

Dadurch sollte die Ausgabebindung in der Datei `function.json` definiert werden:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "queue",
      "name": "queue",
      "direction": "out",
      "queueName": "messages",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```
## <a name="specialized-types"></a>Spezialisierte Typen

In einigen Fällen muss eine Funktion Eingaben und Ausgaben genau steuern. Spezialisierte Typen in dem Paket `azure-functions-java-core` werden für Sie zur Bearbeitung von Anforderungsinformationen und Anpassung des Rückgabestatus eines HTTP-Triggers bereitgestellt:

| Spezialisierter Typ      |       Ziel        | Typische Verwendung                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage`  |    HTTP-Trigger     | Get-Methode, Header oder Abfragen |
| `HttpResponseMessage` | HTTP-Ausgabebindung | Anderer Rückgabestatus als 200   |

> [!NOTE] 
> Sie können zum Abrufen von HTTP-Headern und Abfragen auch die Anmerkung `@BindingName` verwenden. `@Bind("name") String query` durchläuft beispielsweise die HTTP-Anforderungsheader und Abfragen und übergibt diesen Wert an die Methode. Beispiel: `query` wird zu `"test"`, wenn die Anforderungs-URL wie folgt lautet: `http://example.org/api/echo?name=test`.

## <a name="functions-execution-context"></a>Functions-Ausführungskontext

Sie interagieren über das Objekt `ExecutionContext`, das im Paket `azure-functions-java-core` definiert ist, mit der Azure Functions-Ausführungsumgebung. Verwenden Sie das Objekt `ExecutionContext`, um Aufrufinformationen und Funktionsruntimeinformationen in Ihrem Code zu verwenden.

### <a name="logging"></a>Protokollierung

Zugriff auf die Functions-Runtimeprotokollierung ist über das Objekt `ExecutionContext` verfügbar. Diese Protokollierung ist an Azure Monitor gebunden und ermöglicht es Ihnen, während der Funktionsausführung aufgetretene Warnungen und Fehler zu kennzeichnen.

Der folgende Beispielcode protokolliert beim Empfang eines leeren Anforderungstexts eine Warnmeldung.

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received in " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Azure Functions developer reference (Azure Functions-Entwicklerreferenz) (Azure Functions-Entwicklerreferenz)](functions-reference.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
