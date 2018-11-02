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
ms.openlocfilehash: 423661b8a459abf0b3028da92d6fd3ec885bb2c9
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025021"
---
# <a name="azure-functions-java-developer-guide"></a>Java-Entwicklerhandbuch für Azure Functions

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Programmiermodell 

Ihre Azure-Funktion sollte eine zustandslose Klassenmethode sein, die Eingaben verarbeitet und Ausgaben erzeugt. Obwohl Sie Instanzmethoden schreiben können, darf Ihre Funktion nicht von Instanzfeldern der Klasse abhängig sein. Alle Funktionsmethoden benötigen einen Zugriffsmodifizierer des Typs `public`.

## <a name="folder-structure"></a>Ordnerstruktur

Die Ordnerstruktur für ein Java-Projekt sieht wie folgt aus:

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

Verwenden Sie die Java-Anmerkungen im Paket [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation), um Eingaben und Ausgaben an Ihre Methoden zu binden. Beispielcode, in dem Anmerkungen verwendet werden, finden Sie in den [Java-Referenzdokumenten](/java/api/com.microsoft.azure.functions.annotation) zu den einzelnen Anmerkungen sowie in der Referenzdokumentation zu Azure Functions-Bindungen, z.B. der für [HTTP-Trigger](/azure/azure-functions/functions-bindings-http-webhook).

Triggereingaben und -ausgaben können auch in [function.json](/azure/azure-functions/functions-reference#function-code) für Ihre Funktion definiert werden anstatt in Anmerkungen. Die Verwendung von `function.json` anstelle von Anmerkungen auf diese Weise wird nicht empfohlen.

> [!IMPORTANT] 
> Sie müssen ein Azure Storage-Konto in [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) konfigurieren, um Trigger für Azure Storage Blob, Azure Queue Storage oder Azure Table Storage lokal auszuführen.

Beispiel für die Verwendung von Anmerkungen:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
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

## <a name="jdk-runtime-availability-and-support"></a>Verfügbarkeit und Unterstützung der JDK-Runtime 

Laden Sie die [Azul Zulu for Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf)-JDKs von [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/) für die lokale Entwicklung von Java-Funktions-Apps herunter, und verwenden Sie sie. JDKs sind für Windows, Linux und macOS verfügbar, und mit einem [qualifizierten Supportplan](https://azure.microsoft.com/support/plans/) steht [Azure-Support](https://support.microsoft.com/en-us/help/4026305/sql-contact-microsoft-azure-support) für Probleme bei der Entwicklung zur Verfügung.

## <a name="third-party-libraries"></a>Drittanbieterbibliotheken 

Azure Functions unterstützt die Verwendung von Drittanbieterbibliotheken. Standardmäßig werden alle im Projekt angegebenen Abhängigkeiten in der Projektdatei `pom.xml` automatisch während des Ziels von `mvn package` gebündelt. Bibliotheken, die nicht in der Datei `pom.xml` als Abhängigkeiten angegeben sind, platzieren Sie in einem `lib`-Verzeichnis im Stammverzeichnis der Funktion. Abhängigkeiten im Verzeichnis `lib` werden dem Systemklassen-Ladeprogramm zur Laufzeit hinzugefügt.

Die Abhängigkeit `com.microsoft.azure.functions:azure-functions-java-library` wird standardmäßig im Klassenpfad bereitgestellt und muss nicht in das Verzeichnis `lib` eingeschlossen werden.

## <a name="data-type-support"></a>Unterstützung von Datentypen

Sie können alle Datentypen in Java für die Eingabe- und Ausgabedaten verwenden, native Typen inbegriffen, sowie benutzerdefinierte Java-Typen und spezialisierte Azure-Typen, die im Paket `azure-functions-java-library` definiert sind. Die Azure Functions-Runtime versucht, die empfangene Eingabe in den von Ihrem Code angeforderten Typ zu konvertieren.

### <a name="strings"></a>Zeichenfolgen

In Funktionsmethoden übergebene Werte werden in Zeichenfolgen umgewandelt, wenn der entsprechende Eingabeparameter für die Funktion vom Typ `String` ist. 

### <a name="plain-old-java-objects-pojos"></a>Plain Old Java Objects (POJOs, „ganz normale“ Java-Objekte)

Mit JSON formatierte Zeichenfolgen werden in Java-Typen umgewandelt, wenn die Eingabesignatur der Funktion diesen Java-Typ erwartet. Diese Konvertierung erlaubt es Ihnen, JSON-Code zu übergeben und mit Java-Typen zu arbeiten.

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

Leere Eingabewerte können für das Funktionsargument `null` sein. Es wird aber empfohlen, für potenziell leere Werte `Optional<T>` zu verwenden.


## <a name="function-method-overloading"></a>Überladung von Funktionsmethoden

Sie dürfen Funktionsmethoden überladen, die den gleichen Namen haben, aber unterschiedliche Typen aufweisen. Sie können z.B. in einer Klasse sowohl `String echo(String s)` als auch `String echo(MyType s)` verwenden. Azure Functions entscheidet basierend auf dem Eingabetyp (für HTTP-Eingaben führt der MIME-Typ `text/plain` zu einer `String`, während `application/json` für `MyType` steht), welche Methode aufgerufen wird.

## <a name="inputs"></a>Eingaben

Eingaben werden in Azure Functions in zwei Kategorien unterteilt: die Triggereingabe und die zusätzliche Eingabe. Obwohl sie sich in der Datei `function.json` unterscheiden, ist die Verwendung im Java-Code identisch. Der folgende Codeausschnitt soll als Beispiel dienen:

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String in,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") MyObject obj
    ) {
        return "Hello, " + in + " and " + obj.getKey() + ".";
    }

    public static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

Bei Aufruf dieser Funktion wird die HTTP-Anforderung durch `String in` an die Funktion übergeben. Es wird ein Eintrag, der auf der ID in der Routen-URL basiert, aus Azure Table Storage abgerufen und als `obj` im Funktionstext verfügbar gemacht.

## <a name="outputs"></a>Ausgaben

Ausgaben können sowohl im Rückgabewert als auch in Ausgabeparametern angegeben werden. Wenn es nur eine Ausgabe gibt, wird empfohlen, den Rückgabewert zu verwenden. Bei mehreren Ausgaben müssen Sie Ausgabeparameter verwenden.

Der Rückgabewert ist die einfachste Ausgabeform. Sie geben nur den Wert eines beliebigen Typs zurück, und die Azure Functions-Runtime versucht, den tatsächlichen Typ zurückzumarshallen (z.B. eine HTTP-Antwort).  Sie können alle Ausgabeanmerkungen der Funktionsmethode (die name-Eigenschaft der Anmerkung muss $return lauten) zum Definieren des Rückgabewerts der Ausgabe verwenden.

Mit dem Typ `OutputBinding<T>`, der im Paket `azure-functions-java-library` definiert ist, können mehrere Ausgabewerte erzeugt werden. Wenn Sie eine HTTP-Antwort erstellen und eine Nachricht mithilfe von Push in eine Warteschlange übertragen müssen, können Sie Folgendes schreiben:

Beispielsweise könnte eine Funktion, die den Inhalt eines Blobs kopiert, wie im folgenden Code definiert werden. Die Anmerkung `@StorageAccount` wird hier verwendet, um zu verhindern, dass die connection-Eigenschaft für `@BlobTrigger` und `@BlobOutput` dupliziert wird.

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Verwenden Sie `OutputBinding<byte[]`> für binäre Ausgabewerte (für Parameter) und für Rückgabewerte einfach `byte[]`.

## <a name="specialized-types"></a>Spezialisierte Typen

In einigen Fällen muss eine Funktion Eingaben und Ausgaben genau steuern. Spezialisierte Typen im Paket `azure-functions-java-core` werden für Sie zur Bearbeitung von Anforderungsinformationen und Anpassung des Rückgabestatus eines HTTP-Triggers bereitgestellt:

| Spezialisierter Typ      |       Ziel        | Typische Verwendung                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP-Trigger     | Get-Methode, Header oder Abfragen |
| `HttpResponseMessage<T>` | HTTP-Ausgabebindung | Anderer Rückgabestatus als 200   |

> [!NOTE] 
> Sie können zum Abrufen von HTTP-Headern und Abfragen auch die Anmerkung `@BindingName` verwenden. `@BindingName("name") String query` durchläuft beispielsweise die HTTP-Anforderungsheader und Abfragen und übergibt diesen Wert an die Methode. Beispiel: `query` wird zu `"test"`, wenn die Anforderungs-URL wie folgt lautet: `http://example.org/api/echo?name=test`.

### <a name="metadata"></a>Metadaten

Metadaten stammen aus verschiedenen Quellen wie HTTP-Headern, HTTP-Abfragen und [Metadateneigenschaften von Triggern](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties). Verwenden Sie die `@BindingName`-Anmerkung zusammen mit dem Metadatennamen, um den Wert abzurufen.

Der `queryValue` im folgenden Codeausschnitt wird `"test"`, wenn die angeforderte URL `http://{example.host}/api/metadata?name=test` ist.

```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class MyClass {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```

## <a name="execution-context"></a>Ausführungskontext

Sie interagieren über das `ExecutionContext`-Objekt, das im Paket `azure-functions-java-library` definiert ist, mit der Azure Functions-Ausführungsumgebung. Verwenden Sie das Objekt `ExecutionContext`, um Aufrufinformationen und Funktionsruntimeinformationen in Ihrem Code zu verwenden.

### <a name="custom-logging"></a>Benutzerdefinierte Protokollierung

Zugriff auf die Functions-Runtimeprotokollierung ist über das Objekt `ExecutionContext` verfügbar. Diese Protokollierung ist an Azure Monitor gebunden und ermöglicht es Ihnen, während der Funktionsausführung aufgetretene Warnungen und Fehler zu kennzeichnen.

Der folgende Beispielcode protokolliert beim Empfang eines leeren Anforderungstexts eine Warnmeldung.

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

Mit der Azure CLI können Sie die Java-Standardausgabe und -Fehlerprotokollierung sowie andere Anwendungsprotokolle streamen. Konfigurieren Sie zunächst Ihre Funktions-App, um die Anwendungsprotokollierung mithilfe der Azure CLI zu schreiben:

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

Fügen Sie aus Sicherheitsgründen keine Informationen zum Geheimnis, wie z.B. Schlüssel oder Token, in Ihren Quellcode ein. Verwenden Sie Schlüssel und Token in Ihrem Funktionscode, indem Sie sie aus den Umgebungsvariablen auslesen.

Zum Festlegen von Umgebungsvariablen bei der lokalen Ausführung von Azure Functions können Sie diese Variablen der Datei „local.settings.json“ hinzufügen. Falls diese Datei im Stammverzeichnis Ihres Funktionsprojekts nicht vorhanden ist, können Sie sie erstellen. Die Datei sollte wie folgt aussehen:

```xml
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "AzureWebJobsDashboard": ""
  }
}
```

Jede Schlüssel-Wert-Zuordnung in der Zuordnung `values` wird zur Laufzeit als Umgebungsvariable verfügbar gemacht. Sie können darauf zugreifen, indem Sie `System.getenv("<keyname>")` aufrufen (Beispiel: `System.getenv("AzureWebJobsStorage")`). Das Hinzufügen von zusätzlichen Schlüssel-Wert-Paaren ist akzeptabel und eine empfohlene Vorgehensweise.

> [!NOTE]
> Fügen Sie bei diesem Ansatz die Datei „local.settings.json“ der ignore-Datei Ihres Repositorys hinzu, damit dafür kein Commit erfolgt.

Da Ihr Code jetzt von diesen Umgebungsvariablen abhängig ist, können Sie sich beim Azure-Portal anmelden, um die gleichen Schlüssel-Wert-Paare in den Einstellungen Ihrer Funktions-App festzulegen. Das Ziel hierbei ist, dass Ihr Code unabhängig davon, ob Sie lokale Tests durchführen oder ob der Code in Azure bereitgestellt wird, auf die gleiche Weise funktioniert.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Java-Entwicklung für Azure Functions finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
- Lokale Entwicklung und Debuggen mit [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) und [Eclipse](functions-create-maven-eclipse.md). 
* [Remote Debug Java Azure Functions with Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) (Remotedebuggen von Java Azure Functions mit Visual Studio Code)
* [Maven-Plug-In für Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md): Optimieren der Erstellung von Funktionen mithilfe des Ziels `azure-functions:add` und Vorbereiten eines Stagingverzeichnisses für die [Bereitstellung von ZIP-Dateien](deployment-zip-push.md).
