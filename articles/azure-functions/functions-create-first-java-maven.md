---
title: Erstellen der ersten Funktion in Azure mit Java und Maven | Microsoft-Dokumentation
description: Erstellen und veröffentlichen Sie mit Java und Maven eine einfache durch HTTP ausgelöste Funktion in Azure.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, Compute, serverlose Architektur
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 16d6dd6a589256ad98a37465e64e847778d0cc7e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092592"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Erstellen der ersten Funktion mit Java und Maven (Vorschau)

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

In diesem Schnellstart wird erläutert, wie Sie mit Maven ein Projekt für eine [serverlose](https://azure.microsoft.com/overview/serverless-computing/) Funktion erstellen, diese lokal testen und in Azure Functions bereitstellen. Nach Fertigstellung haben Sie eine durch HTTP ausgelöste Funktionen-App erstellt, die in Azure ausgeführt wird.

![Zugreifen auf eine Hallo Welt-Funktion mit cURL über die Befehlszeile](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen
Um die Funktionen-App mit Java zu entwickeln, muss Folgendes installiert sein:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), Version 8.
-  [Apache Maven](https://maven.apache.org), Version 3.0 oder höher
-  [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Damit Sie diesen Schnellstart durchführen können, muss die Umgebungsvariable JAVA_HOME auf den Installationsspeicherort des JDK festgelegt sein.

## <a name="install-the-azure-functions-core-tools"></a>Installieren von Azure Functions Core Tools

Die Azure Functions Core-Tools bieten eine lokale Entwicklungsumgebung zum Schreiben, Ausführen und Debuggen von Azure Functions über das Terminal oder die Eingabeaufforderung. 

Installieren Sie [Version 2 der Core-Tools](functions-run-local.md#v2) auf Ihrem lokalen Computer, bevor Sie fortfahren.

## <a name="generate-a-new-functions-project"></a>Generieren eines neuen Functions-Projekts

Führen Sie in einem leeren Ordner den folgenden Befehl aus, um das Functions-Projekt über einen [Maven-Archetyp](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) zu generieren.

### <a name="linuxmacos"></a>Linux/macOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven fordert Sie zur Eingabe der erforderlichen Werte für die Projektgenerierung auf. Informationen zu den Werten _groupId_, _artifactId_ und _version_ finden Sie in der Referenz [Maven-Benennungskonventionen](https://maven.apache.org/guides/mini/guide-naming-conventions.html). Der Wert _appName_ muss in Azure eindeutig sein. Deshalb generiert Maven standardmäßig einen App-Namen basierend auf den zuvor eingegebenen Wert _artifactId_. Der Wert _packageName_ bestimmt das Java-Paket für den generierten Funktionscode.

Der Wert `appRegion` gibt an, in welcher [Azure-Region](https://azure.microsoft.com/global-infrastructure/regions/) Sie die bereitgestellte Funktions-App ausführen möchten. Mit dem Befehl `az account list-locations` in der Azure-Befehlszeilenschnittstelle können Sie eine Liste mit den Werten der Regionsnamen abrufen. Der Wert `resourceGroup` gibt an, in welcher Azure-Ressourcengruppe die Funktions-App erstellt wird.

Die weiter unten angegebenen Bezeichner `com.fabrikam.functions` und `fabrikam-functions` dienen als Beispiele und verbessern die Lesbarkeit der weiteren Schritte in dieser Schnellstartanleitung. Sie können in diesem Schritt aber auch gerne eigene Werte für Maven angeben.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Define value for property 'appRegion' westus : 
Define value for property 'resourceGroup' java-functions-group: 
Confirm properties configuration: Y
```

Maven erstellt die Projektdateien in einem neuen Ordner mit dem Namen _artifactId_ (in diesem Beispiel: `fabrikam-functions`). Bei dem generierten ausführungsbereiten Code im Projekt handelt es sich um eine einfache, [durch HTTP ausgelöste](/azure/azure-functions/functions-bindings-http-webhook) Funktion, die den Text der Anforderung nach der Zeichenfolge „Hello, “ ausgibt:

```java
public class Function {
    @FunctionName("HttpTrigger-Java")
    public HttpResponseMessage HttpTriggerJava(
    @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
        } else {
            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        }
    }
}
```

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Ändern Sie das Verzeichnis in den neu erstellten Projektordner. Erstellen Sie dann mit Maven die Funktion, und führen Sie sie aus:

```
cd fabrikam-functions
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Sollte bei Ihnen mit Java 9 die Ausnahme `javax.xml.bind.JAXBException` auftreten, sehen Sie sich die Problemumgehung auf [GitHub](https://github.com/jOOQ/jOOQ/issues/6477) an.

Wenn die Funktion lokal auf Ihrem System ausgeführt wird und für HTTP-Anforderungen bereit ist, wird die folgende Ausgabe angezeigt:

```Output
Listening on http://0.0.0.0:7071/
Hit CTRL-C to exit...

Http Functions:

        HttpTrigger-Java: http://localhost:7071/api/HttpTrigger-Java
```

Lösen Sie die Funktion über die Befehlszeile aus. Verwenden Sie dazu cURL in einem neuen Terminalfenster:

```
curl -w '\n' -d LocalFunctionTest http://localhost:7071/api/HttpTrigger-Java
```

```Output
Hello, LocalFunctionTest
```

Verwenden Sie `Ctrl-C` im Terminal, um den Funktionscode anzuhalten.

## <a name="deploy-the-function-to-azure"></a>Bereitstellen der Funktion in Azure

Bei dem Bereitstellungsprozess in Azure Functions werden die Anmeldeinformationen aus der Azure CLI verwendet. [Melden Sie sich zunächst über die Azure-Befehlszeilenschnittstelle an.](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)

```azurecli
az login
```

Stellen Sie Ihren Code mit `azure-functions:deploy` als Maven-Ziel in einer neuen Funktionen-App bereit.

```
mvn azure-functions:deploy
```

Wenn die Bereitstellung abgeschlossen ist, wird die URL angezeigt, mit der Sie auf Ihre Azure-Funktionen-App zugreifen können:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Testen Sie die in Azure ausgeführte Funktionen-App mithilfe von `cURL`. Ersetzen Sie die URL im folgenden Beispiel durch die bereitgestellte URL für Ihre eigene Funktions-App aus dem vorherigen Schritt.

```
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

```Output
Hello, AzureFunctionsTest
```

## <a name="make-changes-and-redeploy"></a>Vornehmen von Änderungen und erneutes Bereitstellen

Bearbeiten Sie die Quelldatei `src/main.../Function.java` im erstellten Projekt, um den von Ihrer Funktions-App zurückgegebenen Text zu ändern. Ändern Sie diese Zeile:

```java
return request.createResponse(200, "Hello, " + name);
```

in Folgendes:

```java
return request.createResponse(200, "Hi, " + name);
```

Speichern Sie die Änderungen, und führen Sie eine erneute Bereitstellung durch, indem Sie wie zuvor `azure-functions:deploy` über das Terminal ausführen. Die Funktions-App wird aktualisiert, und die folgende Anforderung:

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

hat die folgende aktualisierte Ausgabe:

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Java-Funktions-App mit einem einfachen HTTP-Trigger erstellt und für Azure Functions bereitgestellt.

- Weitere Informationen zum Entwickeln von Java-Funktionen finden Sie im [Azure Functions-Java-Entwicklerhandbuch](functions-reference-java.md).
- Fügen Sie mit `azure-functions:add` als Maven-Ziel zusätzliche Funktionen mit verschiedenen Triggern zu Ihrem Projekt hinzu.
- Schreiben und debuggen Sie Funktionen lokal mit [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) und [Eclipse](functions-create-maven-eclipse.md). 
- Debuggen Sie in Azure bereitgestellte Funktionen mit Visual Studio Code. Eine entsprechende Anleitung finden Sie in der Visual Studio Code-Dokumentation für [serverlose Java-Anwendungen](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud).
