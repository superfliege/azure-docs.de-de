---
title: Java-Entwicklerreferenz zu Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Java Funktionen entwickeln können.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: azure functions, funktionen, ereignisverarbeitung, webhooks, dynamisches computing, serverlose architektur, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: routlaw
ms.openlocfilehash: 9258b58783d4670620a251fef866211f7634480f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58096723"
---
# <a name="azure-functions-java-developer-guide"></a>Java-Entwicklerhandbuch für Azure Functions

Die Azure Functions-Runtime unterstützt [Java SE 8 LTS (zulu8.31.0.2-jre8.0.181-win_x64)](https://repos.azul.com/azure-only/zulu/packages/zulu-8/8u181/).

Dieser Leitfaden enthält Informationen zu den Feinheiten beim Schreiben von Azure Functions mit Java.

Eine Java-Funktion ist eine `public`-Methode, die mit Anmerkungen der Form `@FunctionName` versehen ist. Diese Methode definiert die Eingabe für eine Java-Funktion und muss in jedem Paket eindeutig sein. 

In diesem Artikel wird davon ausgegangen, dass Sie bereits die [Entwicklerreferenz zu Azure Functions](functions-reference.md)gelesen haben. Es empfiehlt sich zudem, die Schnellstartanleitung zu Functions zu lesen, um Ihre erste Funktion mit [Visual Studio Code](functions-create-first-function-vs-code.md) oder [Maven](functions-create-first-java-maven.md) zu erstellen.

## <a name="programming-model"></a>Programmiermodell 

Die Konzepte von [Triggern und Bindungen](functions-triggers-bindings.md) sind für Azure Functions von grundlegender Bedeutung. Trigger starten die Ausführung Ihres Codes. Bindungen bieten Ihnen eine Möglichkeit, Daten an eine Funktion zu übergeben und von einer Funktion zurückgeben zu lassen, ohne benutzerdefinierten Datenzugriffscode schreiben zu müssen.

## <a name="folder-structure"></a>Ordnerstruktur

Im Folgenden wird die Ordnerstruktur eines Azure Functions-Java-Projekts gezeigt:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

Sie können die freigegebene Datei [host.json](functions-host-json.md) zum Konfigurieren der Funktions-App verwenden. Jede Funktion verfügt über eine eigene Codedatei (JAVA-Datei) sowie über eine eigene Bindungskonfigurationsdatei („function.json“).

Sie können mehr als eine Funktion in ein Projekt einfügen. Vermeiden Sie es, Ihre Funktionen in separaten JAR-Dateien hinzuzufügen. Die Funktions-App im Zielverzeichnis wird in Ihrer Funktions-App in Azure bereitgestellt.

## <a name="triggers-and-annotations"></a>Trigger und Anmerkungen

 Azure-Funktionen werden durch einen Trigger, z.B. eine HTTP-Anforderung, einen Timer oder ein Datenupdate, aufgerufen. Ihre Funktion muss diesen Trigger und alle weiteren Eingaben verarbeiten, um mindestens eine Ausgabe zu erstellen.

Verwenden Sie die Java-Anmerkungen im Paket [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation), um Eingaben und Ausgaben an Ihre Methoden zu binden. Weitere Informationen finden Sie in der [Java-Referenzdokumentation](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> Sie müssen ein Azure Storage-Konto in [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) konfigurieren, um Trigger für Azure Storage Blob, Azure Queue Storage oder Azure Table Storage lokal auszuführen.

Beispiel:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Hier wird die entsprechende `function.json` gezeigt, die durch [azure-functions-maven-plugin](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin) generiert wurde:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.Function.echo",
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

## <a name="jdk-runtime-availability-and-support"></a>Verfügbarkeit und Unterstützung der JDK-Runtime 

Laden Sie die [Azul Zulu Enterprise for Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) Java 8-JDKs von [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) für die lokale Entwicklung von Java-Funktions-Apps herunter, und verwenden Sie sie. Azure Functions verwendet die Azul Java 8 JDK-Runtime, wenn Sie Ihre Funktions-Apps in der Cloud bereitstellen.

[Azure-Support](https://azure.microsoft.com/en-us/support/) bei Problemen mit den JDKs und Funktions-Apps steht mit einem [qualifizierten Supportplan](https://azure.microsoft.com/support/plans/) zur Verfügung.

## <a name="third-party-libraries"></a>Drittanbieterbibliotheken 

Azure Functions unterstützt die Verwendung von Drittanbieterbibliotheken. Standardmäßig werden alle in der Projektdatei `pom.xml` angegebenen Abhängigkeiten automatisch während des Ziels von [`mvn package`](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage) gebündelt. Bibliotheken, die nicht in der Datei `pom.xml` als Abhängigkeiten angegeben sind, platzieren Sie in einem `lib`-Verzeichnis im Stammverzeichnis der Funktion. Abhängigkeiten im Verzeichnis `lib` werden dem Systemklassen-Ladeprogramm zur Laufzeit hinzugefügt.

Die Abhängigkeit `com.microsoft.azure.functions:azure-functions-java-library` wird standardmäßig im Klassenpfad bereitgestellt und muss nicht in das Verzeichnis `lib` eingeschlossen werden. Darüber hinaus werden die [hier](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) aufgelisteten Abhängigkeiten durch [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) dem Klassenpfad hinzugefügt.

## <a name="data-type-support"></a>Unterstützung von Datentypen

Zum Binden an Eingabe-/Ausgabebindungen können Sie POJOs (Plain Old Java Objects), in `azure-functions-java-library` definierte Typen oder primitive Datentypen wie z.B. „String“ oder „Integer“ verwenden.

### <a name="plain-old-java-objects-pojos"></a>Plain Old Java Objects (POJOs, „ganz normale“ Java-Objekte)

Zum Konvertieren von Eingabedaten in POJO verwendet [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) die [gson](https://github.com/google/gson)-Bibliothek. Als Eingabe für Funktionen verwendete POJO-Typen müssen `public` sein.

### <a name="binary-data"></a>Binärdaten

Binden Sie binäre Eingaben oder Ausgaben an `byte[]`, indem Sie das Feld `dataType` in der Datei „function.json“ auf `binary` festlegen:

```java
   @FunctionName("BlobTrigger")
    @StorageAccount("AzureWebJobsStorage")
     public void blobTrigger(
        @BlobTrigger(name = "content", path = "myblob/{fileName}", dataType = "binary") byte[] content,
        @BindingName("fileName") String fileName,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Blob trigger function processed a blob.\n Name: " + fileName + "\n Size: " + content.length + " Bytes");
    }
```

Verwenden Sie `Optional<T>`, wenn NULL-Werte erwartet werden.

## <a name="bindings"></a>Bindungen

Eingabe- und Ausgabebindungen bieten eine deklarative Möglichkeit, aus Code heraus Verbindungen mit Daten herzustellen. Eine Funktion kann mehrere Eingabe- und Ausgabebindungen aufweisen.

### <a name="example-input-binding"></a>Beispiel für eine Eingabebindung

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String inputReq,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") TestInputData inputData
        @TableOutput(name = "myOutputTable", tableName = "Person", connection = "AzureWebJobsStorage") OutputBinding<Person> testOutputData,
    ) {
        testOutputData.setValue(new Person(httpbody + "Partition", httpbody + "Row", httpbody + "Name"));
        return "Hello, " + inputReq + " and " + inputData.getKey() + ".";
    }

    public static class TestInputData {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
    public static class Person {
        public String PartitionKey;
        public String RowKey;
        public String Name;

        public Person(String p, String r, String n) {
            this.PartitionKey = p;
            this.RowKey = r;
            this.Name = n;
        }
    }
}
```

Diese Funktion wird mit einer HTTP-Anforderung aufgerufen. 
- Die Nutzlast der HTTP-Anforderung wird dem Argument `inputReq` als `String` übergeben.
- Ein Eintrag wird aus Azure Table Storage abgerufen und dem Argument `inputData` als `TestInputData` übergeben.

Um einen Batch von Eingaben zu erhalten, können Sie eine Bindung an `String[]`, `POJO[]`, `List<String>` oder `List<POJO>` durchführen.

```java
@FunctionName("ProcessIotMessages")
    public void processIotMessages(
        @EventHubTrigger(name = "message", eventHubName = "%AzureWebJobsEventHubPath%", connection = "AzureWebJobsEventHubSender", cardinality = Cardinality.MANY) List<TestEventData> messages,
        final ExecutionContext context)
    {
        context.getLogger().info("Java Event Hub trigger received messages. Batch size: " + messages.size());
    }
    
    public class TestEventData {
    public String id;
}

```

Diese Funktion wird immer dann ausgelöst, wenn im konfigurierten Event Hub neue Daten vorliegen. Da die `cardinality` auf `MANY` festgelegt ist, empfängt die Funktion einen Batch von Nachrichten von Event Hub. EventData aus Event Hub wird für die Ausführung der Funktion in `TestEventData` konvertiert.

### <a name="example-output-binding"></a>Beispiel für eine Ausgabebindung

Mit `$return` können Sie eine Ausgabebindung an den Rückgabewert binden. 

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Wenn mehrere Ausgabebindungen vorhanden sind, verwenden Sie den Rückgabewert für nur eine davon.

Um mehrere Ausgabewerte zu senden, verwenden Sie `OutputBinding<T>` gemäß der Definition im Paket `azure-functions-java-library`. 

```java
@FunctionName("QueueOutputPOJOList")
    public HttpResponseMessage QueueOutputPOJOList(@HttpTrigger(name = "req", methods = { HttpMethod.GET,
            HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "itemsOut", queueName = "test-output-java-pojo", connection = "AzureWebJobsStorage") OutputBinding<List<TestData>> itemsOut, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");
       
        String query = request.getQueryParameters().get("queueMessageId");
        String queueMessageId = request.getBody().orElse(query);
        itemsOut.setValue(new ArrayList<TestData>());
        if (queueMessageId != null) {
            TestData testData1 = new TestData();
            testData1.id = "msg1"+queueMessageId;
            TestData testData2 = new TestData();
            testData2.id = "msg2"+queueMessageId;

            itemsOut.getValue().add(testData1);
            itemsOut.getValue().add(testData2);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + queueMessageId).build();
        } else {
            return request.createResponseBuilder(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Did not find expected items in CosmosDB input list").build();
        }
    }

     public static class TestData {
        public String id;
    }
```

Die obige Funktion wird für einen HttpRequest aufgerufen und schreibt mehrere Werte in die Azure-Warteschlange.

## <a name="httprequestmessage-and-httpresponsemessage"></a>HttpRequestMessage und HttpResponseMessage

 Die in `azure-functions-java-library` definierten HttpRequestMessage- und HttpResponseMessage-Typen sind Hilfstypen, die mit HttpTrigger-Funktionen verwendet werden.

| Spezialisierter Typ      |       Ziel        | Typische Verwendung                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP-Trigger     | Get-Methode, Header oder Abfragen |
| `HttpResponseMessage` | HTTP-Ausgabebindung | Anderer Rückgabestatus als 200   |

## <a name="metadata"></a>Metadaten

Einige Trigger senden [Triggermetadaten](/azure/azure-functions/functions-triggers-bindings) zusammen mit den Eingabedaten. Sie können die Anmerkung `@BindingName` für die Bindung an Triggermetadaten verwenden.


```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class Function {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```
Im obigen Beispiel ist `queryValue` an den Abfragezeichenfolgen-Parameter `name` in der HTTP-Anforderungs-URL `http://{example.host}/api/metadata?name=test` gebunden. Es folgt ein weiteres Beispiel für die Bindung an die `Id` aus den Metadaten des Warteschlangentriggers.

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " with metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> Der in der Anmerkung angegebene Name muss der Metadateneigenschaft entsprechen.

## <a name="execution-context"></a>Ausführungskontext

Der in der `azure-functions-java-library` definierte `ExecutionContext` enthält Hilfsmethoden für die Kommunikation mit der Funktionsruntime.

### <a name="logger"></a>Protokollierungstool

Verwenden Sie den im `ExecutionContext` definierten `getLogger` zum Schreiben von Protokollen aus Funktionscode.

Beispiel:

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Anzeigen von Protokollen und Ablaufverfolgung

Mit der Azure CLI können Sie die Java-Protokollierung von stdout und stderr sowie andere Anwendungsprotokolle streamen. 

Konfigurieren Sie Ihre Funktionsanwendung, um die Anwendungsprotokollierung mithilfe der Azure CLI zu schreiben:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Zum Streaming der Protokollausgabe Ihrer Funktions-App mithilfe der Azure CLI öffnen Sie eine neue Eingabeaufforderungs-, Bash- oder Terminalsitzung und geben den folgenden Befehl ein:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
Der Befehl [az webapp log tail](/cli/azure/webapp/log) ermöglicht das Filtern der Ausgabe mithilfe der Option `--provider`. 

Zum Herunterladen der Protokolldateien mit der Azure CLI als einzelne ZIP-Datei öffnen Sie eine neue Eingabeaufforderungs-, Bash- oder Terminalsitzung und geben den folgenden Befehl ein:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Sie müssen die Dateisystemprotokollierung im Azure-Portal oder über die Azure CLI aktiviert haben, bevor Sie diesen Befehl ausführen können.

## <a name="environment-variables"></a>Umgebungsvariablen

In Functions werden [App-Einstellungen](https://docs.microsoft.com/azure/azure-functions/functions-app-settings), z.B. Dienstverbindungszeichenfolgen, während der Ausführung als Umgebungsvariablen verfügbar gemacht. Sie können über `System.getenv("AzureWebJobsStorage")` auf diese Einstellungen zugreifen.

Beispiel:

Hinzufügen von [AppSetting](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings) mit dem Namen „testAppSetting“ und dem Wert „testAppSettingValue“

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("testAppSetting "+ System.getenv("testAppSettingValue"));
        return String.format(req);
    }
}

```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Java-Entwicklung für Azure Functions finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* Lokale Entwicklung und Debuggen mit [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) und [Eclipse](functions-create-maven-eclipse.md). 
* [Remote Debug Java Azure Functions with Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) (Remotedebuggen von Java Azure Functions mit Visual Studio Code)
* [Maven-Plug-In für Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md): Optimieren der Erstellung von Funktionen mithilfe des Ziels `azure-functions:add` und Vorbereiten eines Stagingverzeichnisses für die [Bereitstellung von ZIP-Dateien](deployment-zip-push.md).
