---
title: JavaScript-Entwicklerreferenz zu Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von JavaScript Funktionen entwickeln können.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: azure-functions
ms.devlang: nodejs
ms.topic: reference
ms.date: 03/04/2018
ms.author: glenga
ms.openlocfilehash: 24f7faa0fb111e4e537a7db3f5e1eea709d1ca59
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957732"
---
# <a name="azure-functions-javascript-developer-guide"></a>JavaScript-Entwicklerhandbuch für Azure Functions
Dieses Handbuch enthält Informationen zu den Feinheiten des Schreibens von Azure Functions mit JavaScript.

Eine JavaScript-Funktion ist eine exportierte `function`, die ausgeführt wird, wenn sie ausgelöst wird ([Trigger werden in „function.json“ konfiguriert](functions-triggers-bindings.md)). An jede Funktion wird ein `context`-Objekt übergeben, das zum Empfangen und Senden von Bindungsdaten, für die Protokollierung und die Kommunikation mit der Laufzeit verwendet wird.

In diesem Artikel wird davon ausgegangen, dass Sie bereits die [Entwicklerreferenz zu Azure Functions](functions-reference.md)gelesen haben. Es wird auch empfohlen, dass Sie ein Tutorial unter „Schnellstarts“ ausgeführt haben, um [Ihre erste Funktion](functions-create-first-function-vs-code.md) zu erstellen.

## <a name="folder-structure"></a>Ordnerstruktur

Die erforderlichen Ordnerstruktur für ein JavaScript-Projekt sieht folgendermaßen aus. Beachten Sie, dass diese Standardeinstellung geändert werden kann. Weitere Details finden Sie im Abschnitt [scriptFile](functions-reference-node.md#using-scriptfile) unten.

```
FunctionsProject
 | - MyFirstFunction
 | | - index.js
 | | - function.json
 | - MySecondFunction
 | | - index.js
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.js
 | | - mySecondHelperFunction.js
 | - node_modules
 | - host.json
 | - package.json
 | - extensions.csproj
 | - bin
```

Im Stammverzeichnis des Projekts befindet sich eine freigegebene Datei [host.json](functions-host-json.md), die zum Konfigurieren der Funktions-App verwendet werden kann. Jede Funktion verfügt über einen Ordner mit einer eigenen Codedatei (JS-Datei) und Bindungskonfigurationsdatei („function.json“).

Die in [Version 2.x](functions-versions.md) der Functions-Laufzeit erforderlichen Bindungserweiterungen sind in der Datei `extensions.csproj` definiert, die eigentlichen Bibliotheksdateien befinden sich im Ordner `bin`. Wenn Sie lokal entwickeln, müssen Sie [Bindungserweiterungen registrieren](functions-triggers-bindings.md#local-development-azure-functions-core-tools). Wenn Sie Funktionen im Azure-Portal entwickeln, wird diese Registrierung für Sie ausgeführt.

## <a name="exporting-a-function"></a>Exportieren einer Funktion

JavaScript-Funktionen müssen über [`module.exports`](https://nodejs.org/api/modules.html#modules_module_exports) (oder [`exports`](https://nodejs.org/api/modules.html#modules_exports)) exportiert werden. Im Standardfall sollte Ihre exportierte Funktion der einzige Export aus ihrer Datei sein, der Export namens `run` oder der Export namens `index`. Der Standardspeicherort Ihrer Funktion ist `index.js`, wobei `index.js` sich das gleiche übergeordnete Verzeichnis mit `function.json` teilt. Beachten Sie, dass der Name des übergeordneten Verzeichnisses von `function.json` immer der Name Ihrer Funktion ist. 

Um den Dateispeicherort zu konfigurieren und den Namen Ihrer Funktion zu exportieren, lesen Sie weiter unten die Beschreibung zum [Konfigurieren des Einstiegspunkts Ihrer Funktion](functions-reference-node.md#configure-function-entry-point).

Der Einstiegspunkt für Ihre exportierte Funktion muss immer ein `context`-Objekt als ersten Parameter annehmen.

```javascript
// You must include a context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
// You can also use 'arguments' to dynamically handle inputs
module.exports = async function(context) {
    context.log('Number of inputs: ' + arguments.length);
    // Iterates through trigger and input binding data
    for (i = 1; i < arguments.length; i++){
        context.log(arguments[i]);
    }
};
```

Trigger und Eingabebindungen (Bindungen des Typs `direction === "in"`) können als Parameter an die Funktion übergeben werden. Sie werden in der Reihenfolge, in der sie in *function.json* definiert sind, an die Funktion übergeben. Sie können Eingaben mithilfe des JavaScript-Objekts [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) auch dynamisch behandeln. Wenn Sie also beispielsweise `function(context, a, b)` in `function(context, a)` ändern, können Sie dennoch den Wert von `b` im Funktionscode abrufen, indem Sie auf `arguments[2]` verweisen.

Alle Bindungen werden unabhängig von der Richtung auch mit dem `context`-Objekt über die `context.bindings`-Eigenschaft übergeben.

## <a name="context-object"></a>context-Objekt
Die Laufzeit verwendet ein `context`-Objekt, um Daten an Ihre und von Ihrer Funktion zu übergeben und Ihnen die Kommunikation mit der Laufzeit zu ermöglichen.

Das `context`-Objekt ist immer der erste Parameter in einer Funktion und muss immer angegeben werden, da es Methoden wie `context.done` und `context.log` enthält, die für die ordnungsgemäße Verwendung der Laufzeit benötigt werden. Sie können dem Objekt einen beliebigen Namen geben (also etwa `ctx` oder `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(ctx) {
    // function logic goes here :)
    ctx.done();
};
```

### <a name="contextbindings-property"></a>context.bindings-Eigenschaft

```
context.bindings
```
Gibt ein benanntes Objekt zurück, das alle Eingabe- und Ausgabedaten enthält. Mit den folgenden Bindungsdefinitionen in Ihrer Datei *function.json* können Sie beispielsweise auf den Inhalt einer Warteschlange von `context.bindings.myInput` zugreifen und Ausgaben mit `context.bindings.myOutput` einer Warteschlange zuweisen.

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
},
{
    "type":"queue",
    "direction":"out",
    "name":"myOutput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

Beachten Sie, dass Sie die Ausgabebindungsdaten mit der `context.done`-Methode anstelle des `context.binding`-Objekts definieren können (siehe unten).

### <a name="contextbindingdata-property"></a>context.bindingData-Eigenschaft

```
context.bindingData
```
Gibt ein benanntes Objekt zurück, das Triggermetadaten und Funktionsaufrufdaten (`invocationId`, `sys.methodName`, `sys.utcNow`, `sys.randGuid`) enthält. Ein Beispiel für Triggermetadaten finden Sie in diesem [Event Hubs-Beispiel](functions-bindings-event-hubs.md#trigger---javascript-example).

### <a name="contextdone-method"></a>context.Done-Methode
```
context.done([err],[propertyBag])
```

Teilt der Laufzeit mit, dass Ihr Code beendet ist. Wenn die Funktion die JavaScript-Deklaration [`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) verwendet (verfügbar ab Node 8 in Functions Version 2.x), müssen Sie `context.done()` nicht verwenden. Der Rückruf `context.done` wird implizit aufgerufen.

Wenn Ihre Funktion keine asynchrone Funktion ist, **müssen Sie `context.done` aufrufen**, um der Laufzeit mitzuteilen, dass Ihre Funktion vollständig ist. Wenn die Methode fehlt, tritt ein Timeout für die Ausführung auf.

Mit der `context.done`-Methode können Sie sowohl einen benutzerdefinierten Fehler an die Laufzeit als auch ein JSON-Objekt mit Ausgabebindungsdaten zurückgeben. Eigenschaften, die an `context.done` übergeben werden, überschreiben alles, was für das `context.bindings`-Objekt festgelegt wurde.

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>context.log-Methode  

```
context.log(message)
```
Ermöglicht das Schreiben in die Streamingfunktionsprotokolle auf Standard-Ablaufverfolgungsebene. Es sind zusätzliche Protokollierungsmethoden in `context.log` verfügbar, mit denen Sie auf anderen Ablaufverfolgungsebenen in das Funktionsprotokoll schreiben können:


| Methode                 | BESCHREIBUNG                                |
| ---------------------- | ------------------------------------------ |
| **Fehler(_Meldung_)**   | Schreibt in Protokollierung auf Fehlerebene oder niedriger.   |
| **warn(_Meldung_)**    | Schreibt in Protokollierung auf Warnungsebene oder niedriger. |
| **info(_Meldung_)**    | Schreibt in Protokollierung auf Informationsebene oder niedriger.    |
| **verbose(_Meldung_)** | Schreibt in Protokollierung auf ausführlicher Ebene.           |

Das folgende Beispiel schreibt auf Warnungs-Ablaufverfolgungsebene ein Protokoll:

```javascript
context.log.warn("Something has happened."); 
```
Sie können den [Ablaufverfolgungsebenen-Schwellenwert für die Protokollierung](#configure-the-trace-level-for-console-logging) in der Datei „host.json“ konfigurieren. Weitere Informationen zum Schreiben von Protokollen finden Sie unter [Schreiben von Ablaufverfolgungsausgaben ](#writing-trace-output-to-the-console) weiter unten.

Lesen Sie [Überwachen von Azure Functions](functions-monitoring.md), um weitere Informationen zum Anzeigen und Abfragen von Funktionsprotokollen zu erhalten.

## <a name="binding-data-type"></a>Datentyp für Bindungen

Verwenden Sie zum Definieren des Datentyps für eine Eingabebindung die `dataType`-Eigenschaft in der Bindungsdefinition. Um z.B. den Inhalt einer HTTP-Anforderung im Binärformat zu lesen, verwenden Sie den Typ `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Optionen für `dataType` sind `binary`, `stream` und `string`.

## <a name="writing-trace-output-to-the-console"></a>Schreiben der Ablaufverfolgungsausgabe in die Konsole 

In Functions verwenden Sie die `context.log`-Methoden, um die Ablaufverfolgungsausgabe in die Konsole zu schreiben. In Functions v2.x werden Ablaufverfolgungsausgaben über `console.log` auf der Ebene der Funktions-App erfasst. Dies bedeutet, dass Ausgaben von `console.log` nicht an einen bestimmten Funktionsaufruf gebunden sind und daher nicht in den Protokollen einer bestimmten Funktion angezeigt werden. Sie werden jedoch an Application Insights weitergegeben. In Functions v1.x können Sie `console.log` nicht verwenden, um in die Konsole zu schreiben. 

Beim Aufruf von `context.log()` wird Ihre Meldung auf der Standard-Ablaufverfolgungsebene, also der _info_-Ablaufverfolgungsebene, in die Konsole geschrieben. Das folgende Beispiel schreibt auf der „info“-Ablaufverfolgungsebene in die Konsole:

```javascript
context.log({hello: 'world'});  
```

Dieser Code entspricht dem Code oben:

```javascript
context.log.info({hello: 'world'});  
```

Dieser Code schreibt auf der Fehlerebene in die Konsole:

```javascript
context.log.error("An error has occurred.");  
```

Da _error_ die höchste Ablaufverfolgungsebene ist, wird diese Ablaufverfolgung auf allen Ablaufverfolgungsebenen in die Ausgabe geschrieben, solange die Protokollierung aktiviert ist.

Alle `context.log`-Methoden unterstützen das gleiche Parameterformat, das auch von der [util.format](https://nodejs.org/api/util.html#util_util_format_format)-Methode in Node.js unterstützt wird. Beachten Sie den folgenden Code, der auf der standardmäßigen Ablaufverfolgungsebene Funktionsprotokolle schreibt:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Sie können den gleichen Code auch im folgenden Format schreiben:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Konfigurieren der Ablaufverfolgungsebene für die Konsolenprotokollierung

Mit Functions können Sie den Ablaufverfolgungsebenen-Schwellenwert zum Schreiben in die Konsole definieren, sodass Sie mühelos mit Ihren Funktionen steuern können, wie Ablaufverfolgungen in die Konsole geschrieben werden. Legen Sie mit der `tracing.consoleLevel`-Eigenschaft in der Datei „host.json“ den Schwellenwert für alle Ablaufverfolgungen fest, die in die Konsole geschrieben werden. Diese Einstellung gilt für alle Funktionen in Ihrer Funktionen-App. Im folgenden Beispiel wird der Schwellenwert für die Ablaufverfolgung festgelegt, um die ausführliche Protokollierung zu aktivieren:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Die Werte von **consoleLevel** entsprechen den Namen der `context.log`-Methoden. Um die gesamte Ablaufverfolgungsprotokollierung in der Konsole zu deaktivieren, setzen Sie **consoleLevel** auf _off_. Weitere Informationen finden Sie in der [host.json-Referenz](functions-host-json.md).

## <a name="http-triggers-and-bindings"></a>HTTP: Trigger und Bindungen

HTTP- und Webhooktrigger und HTTP-Ausgabebindungen verwenden Request- und Response-Objekte, um das HTTP-Messaging darzustellen.  

### <a name="request-object"></a>Anforderungsobjekt

Das `context.req`-Objekt (Anforderungsobjekt) weist die folgenden Eigenschaften auf:

| Eigenschaft      | BESCHREIBUNG                                                    |
| ------------- | -------------------------------------------------------------- |
| _body_        | Ein Objekt, das den Hauptteil der Anforderung enthält.               |
| _headers_     | Ein Objekt, das die Header der Anforderung enthält.                   |
| _method_      | Die HTTP-Methode der Anforderung.                                |
| _originalUrl_ | Die URL der Anforderung.                                        |
| _params_      | Ein Objekt, das die Routingparameter der Anforderung enthält. |
| _query_       | Ein Objekt, das die Abfrageparameter enthält.                  |
| _rawBody_     | Der Hauptteil der Meldung als Zeichenfolge.                           |


### <a name="response-object"></a>Antwortobjekt

Das `context.res`-Objekt (Antwortobjekt) weist die folgenden Eigenschaften auf:

| Eigenschaft  | BESCHREIBUNG                                               |
| --------- | --------------------------------------------------------- |
| _body_    | Ein Objekt, das den Hauptteil der Antwort enthält.         |
| _headers_ | Ein Objekt, das die Header der Antwort enthält.             |
| _isRaw_   | Gibt an, dass die Formatierung für die Antwort übersprungen wird.    |
| _status_  | Der HTTP-Statuscode der Antwort.                     |

### <a name="accessing-the-request-and-response"></a>Zugreifen auf Anforderung und Antwort 

Beim Arbeiten mit HTTP-Triggern bestehen verschiedene Möglichkeiten, auf die HTTP-Anforderungsobjekte und -Antwortobjekte zuzugreifen:

+ Über die `req`- und `res`-Eigenschaft des `context`-Objekts. Auf diese Weise können Sie die herkömmlichen Muster für den Zugriff auf HTTP-Daten über das context-Objekt verwenden, anstatt das gesamte `context.bindings.name`-Muster verwenden zu müssen. Das folgende Beispiel veranschaulicht den Zugriff auf das `req`- und `res`-Objekt des `context`-Objekts:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Über die benannten Eingabe- und Ausgabebindungen. Hierbei funktionieren die HTTP-Trigger und -Bindungen genauso wie jede andere Bindung. Im folgenden Beispiel wird das Antwortobjekt mit einer als `response` benannten Bindung festgelegt: 

    ```json
    {
        "type": "http",
        "direction": "out",
        "name": "response"
    }
    ```
    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```
+ _[Nur Antwort]_  Durch Aufrufen von `context.res.send(body?: any)`. Eine HTTP-Antwort wird mit der Eingabe `body` als Antworttext erstellt. `context.done()` wird implizit aufgerufen.

+ _[Nur Antwort]_  Durch Aufrufen von `context.done()`. Eine besondere Art der HTTP-Bindung gibt die Antwort zurück, die der `context.done()`-Methode übergeben wird. Die folgende HTTP-Ausgabebindung definiert einen `$return`-Ausgabeparameter:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version"></a>Node-Version

Die folgende Tabelle zeigt die jeweilige von den Hauptversionen von Functions Runtime verwendete Node.js-Version:

| Functions-Version | Node.js-Version | 
|---|---|
| 1.x | 6.11.2 (durch die Laufzeit gesperrt) |
| 2.x  | Node.js.-Versionen _Active LTS_ und _Current_ (8.11.1 und 10.6.0 empfohlen). Legen Sie die Version mithilfe der [App-Einstellung](functions-how-to-use-azure-function-app-settings.md#settings) „WEBSITE_NODE_DEFAULT_VERSION“ fest.|

Sie können die aktuelle Version anzeigen, die die Laufzeit verwendet, indem Sie die oben gezeigte App-Einstellung überprüfen oder `process.version` aus einer beliebigen Funktion ausgeben.

## <a name="dependency-management"></a>Verwaltung von Abhängigkeiten
Um Communitybibliotheken in Ihrem JavaScript-Code zu verwenden (wie im folgenden Beispiel gezeigt), müssen Sie sicherstellen, dass alle Abhängigkeiten für Ihre Funktions-App in Azure installiert sind.

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

Beachten Sie, dass Sie eine Datei `package.json` im Stammverzeichnis Ihrer Funktions-App definieren sollten. Wenn Sie die Datei definieren, nutzen alle Funktionen in der App gemeinsam die gleichen zwischengespeicherten Pakete, was die Leistung optimiert. Wenn ein Versionskonflikt auftritt, können Sie ihn beheben, indem Sie eine `package.json`-Datei im Ordner einer bestimmten Funktion hinzufügen.  

Es gibt zwei Möglichkeiten zum Installieren von Paketen für Ihre Funktions-App: 

### <a name="deploying-with-dependencies"></a>Bereitstellen mit Abhängigkeiten
1. Installieren Sie alle erforderlichen Pakete lokal, indem Sie `npm install` ausführen.

2. Stellen Sie Ihren Code bereit, und stellen sicher, dass der Ordner `node_modules` in der Bereitstellung enthalten ist. 


### <a name="using-kudu"></a>Verwenden von Kudu
1. Wechseln Sie zur Adresse `https://<function_app_name>.scm.azurewebsites.net`.

2. Klicken Sie auf **Debugkonsole** > **CMD**.

3. Gehen Sie zu `D:\home\site\wwwroot`, und ziehen Sie dann die Datei „package.json“ auf den **wwwroot**-Ordner in der oberen Hälfte der Seite.  
    Es gibt auch andere Möglichkeiten, Dateien in Ihre Funktionen-App hochzuladen. Weitere Informationen finden Sie unter [Aktualisieren von Funktionen-App-Dateien](functions-reference.md#fileupdate). 

4. Sobald die Datei „package.json“ hochgeladen ist, führen Sie den `npm install`-Befehl in der **Kudu-Remoteausführungskonsole** aus.  
    Mit dieser Aktion werden die in der Datei „package.json“ angegebenen Pakete heruntergeladen und die Funktionen-App neu gestartet.

## <a name="environment-variables"></a>Umgebungsvariablen
Verwenden Sie `process.env` zum Abrufen einer Umgebungsvariablen oder eines App-Einstellungswerts, wie hier in der `GetEnvironmentVariable`-Funktion zu sehen:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));

    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="configure-function-entry-point"></a>Konfigurieren des Funktionseinstiegspunkts

Die `function.json`-Eigenschaften `scriptFile` und `entryPoint` können verwendet werden, um den Speicherort und den Namen Ihrer exportierten Funktion zu konfigurieren. Diese können wichtig sein, wenn Ihr JavaScript transpiliert ist.

### <a name="using-scriptfile"></a>Verwenden von `scriptFile`

Standardmäßig wird eine JavaScript-Funktion aus der Datei `index.js` ausgeführt, einer Datei, die sich das gleiche übergeordnete Verzeichnis mit der entsprechenden Datei `function.json` teilt.

`scriptFile` kann verwendet werden, um eine Ordnerstruktur zu erhalten, die folgendermaßen aussieht:
```
FunctionApp
 | - host.json
 | - myNodeFunction
 | | - function.json
 | - lib
 | | - nodeFunction.js
 | - node_modules
 | | - ... packages ...
 | - package.json
```

Die Datei `function.json` für `myNodeFunction` sollte eine `scriptFile`-Eigenschaft enthalten, die auf Datei mit der exportierten Funktion verweist, die ausgeführt werden soll.
```json
{
  "scriptFile": "../lib/nodeFunction.js",
  "bindings": [
    ...
  ]
}
```

### <a name="using-entrypoint"></a>Verwenden von `entryPoint`

In `scriptFile` (oder `index.js`) muss eine Funktion mit `module.exports` exportiert werden, um gefunden und ausgeführt zu werden. Standardmäßig ist die Funktion, die ausgeführt wird, wenn sie ausgelöst wird, der einzige Export aus dieser Datei, der Export mit dem Namen `run` oder der Export mit dem Namen `index`.

Dies kann mit `entryPoint` in `function.json` konfiguriert werden:
```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

In Functions v2.x wird der Parameter `this` in Benutzerfunktionen unterstützt. In dieser Version könnte der Funktionscode dann wie folgt aussehen:
```javascript
class MyObj {
    constructor() {
        this.foo = 1;
    };
    
    function logFoo(context) { 
        context.log("Foo is " + this.foo); 
        context.done(); 
    }
}

const myObj = new MyObj();
module.exports = myObj;
```

In diesem Beispiel ist es wichtig zu beachten, dass es keine Garantie dafür gibt, dass der Zustand zwischen den Ausführungen erhalten bleibt, obwohl ein Objekt exportiert wird.

## <a name="considerations-for-javascript-functions"></a>Überlegungen zu JavaScript-Funktionen

Beachten Sie beim Arbeiten mit JavaScript-Funktionen die Überlegungen in den folgenden Abschnitten.

### <a name="choose-single-vcpu-app-service-plans"></a>Auswählen von App Service-Plänen mit einzelner vCPU

Wenn Sie eine Funktions-App erstellen, die den App Service-Plan verwendet, sollten Sie statt eines Plans mit mehreren vCPUs einen Plan mit einer einzelnen vCPU auswählen. Derzeit führt Functions JavaScript-Funktionen auf virtuellen Computern mit einer einzelnen vCPU effizienter aus. Die Verwendung größerer virtueller Computer führt nicht zu den erwarteten Leistungsverbesserungen. Bei Bedarf können Sie manuell horizontal hochskalieren, indem Sie weitere Instanzen von virtuellen Computern mit einer einzelnen vCPU hinzufügen. Sie können aber auch die automatische Skalierung aktivieren. Weitere Informationen finden Sie unter [Manuelles oder automatisches Skalieren der Instanzenzahl](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>TypeScript- und CoffeeScript-Unterstützung
Weil es noch keine direkte Unterstützung für die automatische Kompilierung von TypeScript bzw. CoffeeScript über die Laufzeit gibt, muss eine solche Unterstützung außerhalb der Laufzeit zum Zeitpunkt der Bereitstellung geschehen. 

### <a name="cold-start"></a>Kaltstart
Bei der Entwicklung von Azure Functions im serverlosen Hostingmodell sind Kaltstarts Realität. Der Begriff „Kaltstart“ bezieht sich auf die Tatsache, dass es beim ersten Start Ihrer Funktions-App nach einer Zeit der Inaktivität länger dauert, bis sie gestartet wird. Insbesondere bei JavaScript-Funktionen mit großen Abhängigkeitsbäumen kann dies zu einer erheblichen Verlangsamung führen. Um den Prozess zu beschleunigen, führen Sie nach Möglichkeit [Ihre Funktionen als Paketdatei](run-functions-from-deployment-package.md) aus. Viele Bereitstellungsmethoden entscheiden sich standardmäßig für dieses Modell, aber wenn bei Ihnen große Kaltstarts stattfinden und die Ausführung nicht aus einer Paketdatei erfolgt, kann dies eine massive Verbesserung bedeuten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Azure Functions developer reference (Azure Functions-Entwicklerreferenz) (Azure Functions-Entwicklerreferenz)](functions-reference.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)

