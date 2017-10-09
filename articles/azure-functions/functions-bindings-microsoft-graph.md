---
title: Microsoft Graph-Bindungen in Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Microsoft Graph-Trigger und -Bindungen in Azure Functions verwendet werden.
services: functions
author: mattchenderson
manager: cfowler
editor: 
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/19/2017
ms.author: mahender
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: b0eb0ac63c7dbb9d6cbba093937231e93670529e
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="azure-functions-microsoft-graph-bindings"></a>Microsoft Graph-Bindungen in Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren von und Arbeiten mit Microsoft Graph-Triggern und -Bindungen in Azure Functions.
Mit diesen können Sie mithilfe von Azure Functions Daten, Einblicke und Ereignisse aus [Microsoft Graph](https://graph.microsoft.io) bearbeiten.

Die Microsoft Graph-Erweiterung bietet die folgenden Bindungen:
- Eine [Autorisierungstoken-Eingabebindung](#token-input) ermöglicht Ihnen die Interaktion mit jeder Microsoft Graph-API.
- Eine [Excel-Tabelleneingabebindung](#excel-input) ermöglicht Ihnen, Daten aus Excel zu lesen.
- Eine [Excel-Tabellenausgabebindung](#excel-output) ermöglicht Ihnen, Excel-Daten zu ändern.
- Eine [OneDrive-Dateieingabebindung](#onedrive-input) ermöglicht Ihnen, Dateien von OneDrive zu lesen.
- Eine [OneDrive-Dateiausgabebindung](#onedrive-output) ermöglicht Ihnen, Dateien in OneDrive zu schreiben.
- Eine [Outlook-Nachrichtenausgabebindung](#outlook-output) ermöglicht Ihnen, E-Mails über Outlook zu senden.
- Mit einer Sammlung von [Microsoft Graph-Webhooktriggern und -bindungen](#webhooks) können Sie auf Ereignisse aus Microsoft Graph reagieren.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note] 
> Microsoft Graph-Bindungen befinden sich derzeit in der Vorschau.

## <a name="setting-up-the-extensions"></a>Einrichten der Erweiterungen

Microsoft Graph-Bindungen sind über _Bindungserweiterungen_ verfügbar. Bindungserweiterungen sind optionale Komponenten für die Azure Functions-Runtime. In diesem Abschnitt erfahren Sie, wie Sie die Microsoft Graph- und Autorisierungstokenerweiterungen einrichten.

### <a name="enabling-functions-20-preview"></a>Aktivieren von Functions 2.0 Preview

Bindungserweiterungen sind nur für Azure Functions 2.0 Preview verfügbar. Legen Sie zum Aktivieren von Functions 2.0 die Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` auf „beta“ fest.  Weitere Informationen zum Konfigurieren von Anwendungseinstellungen finden Sie unter [Anwendungseinstellungen in Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings).

### <a name="installing-the-extension"></a>Installieren der Erweiterung

Um eine Erweiterung über das Azure-Portal zu installieren, müssen Sie zu einer Vorlage oder Bindung navigieren, die darauf verweist. Erstellen Sie eine neue Funktion, und wählen Sie auf dem Bildschirm für die Vorlagenauswahl das Szenario „Microsoft Graph“ aus. Wählen Sie eine der Vorlagen in diesem Szenario aus. Alternativ können Sie zur Registerkarte „Integration“ einer vorhandenen Funktion navigieren und eine der in diesem Thema behandelten Bindungen auswählen.

In beiden Fällen wird eine Warnung angezeigt, die die zu installierende Erweiterung angibt. Klicken Sie auf **Installieren**, um die Erweiterung zu erhalten.

> [!Note] 
> Jede Erweiterung muss nur einmal pro Funktions-App installiert werden. Der Installationsvorgang im Portal kann bis zu 10 Minuten eines Verbrauchstarifs aufbrauchen.

Wenn Sie Visual Studio verwenden, können Sie die Erweiterungen durch Installieren dieser NuGet-Pakete erhalten:
- [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/)
- [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/)

### <a name="configuring-app-service-authentication--authorization"></a>Konfigurieren der App Service-Authentifizierung/-Autorisierung

Für die in diesem Thema beschriebenen Bindungen muss eine Identität verwendet werden. Dies ermöglicht Microsoft Graph, Berechtigungen zu erzwingen und Interaktionen zu überwachen. Die Identität kann ein Benutzer sein, der auf Ihre Anwendung zugreift, oder die Anwendung selbst. Um diese Identität zu konfigurieren, müssen Sie [App Service-Authentifizierung/-Autorisierung](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) bei Azure Active Directory einrichten. Sie müssen auch alle Ressourcenberechtigungen anfordern, die in Ihren Funktionen benötigt werden.

> [!Note] 
> Die Microsoft Graph-Erweiterung unterstützt nur die AAD-Authentifizierung. Benutzer müssen sich mit einem Geschäfts-, Schul- oder Unikonto anmelden.

Wenn Sie das Azure-Portal verwenden, wird unter der Aufforderung zum Installieren der Erweiterung eine Warnung angezeigt, in der Sie dazu aufgefordert werden, die App Service-Authentifizierung/-Autorisierung zu konfigurieren und Berechtigungen anzufordern, die für die Vorlage oder Bindung erforderlich sind. Klicken Sie nach Bedarf auf **AAD jetzt konfigurieren** oder **Jetzt Berechtigungen hinzufügen**.






<a name="token-input"></a>
## <a name="auth-token-input-binding"></a>Authentifizierungstoken-Eingabebindung

Diese Bindung ruft ein AAD-Token für eine bestimmte Ressource ab und stellt es Ihrem Code als Zeichenfolge zur Verfügung. Bei der Ressource kann es sich um jede handeln, für die die Anwendung über Berechtigungen verfügt. 

### <a name="configuring-an-auth-token-input-binding"></a>Konfigurieren einer Authentifizierungstoken-Eingabebindung

Die Bindung selbst erfordert keine AAD-Berechtigungen, aber je nach Verwendung des Tokens müssen Sie möglicherweise zusätzliche Berechtigungen anfordern. Überprüfen Sie die Anforderungen der Ressource, auf die Sie mit dem Token zugreifen möchten.

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für das Autorisierungstoken verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer Autorisierungstoken-Eingabebindung in Code](#token-input-code).|
|**type**|Erforderlich – muss auf `token` festgelegt sein.|
|**direction**|Erforderlich – muss auf `in` festgelegt sein.|
|**Identität**|Erforderlich – die Identität, die zum Ausführen der Aktion verwendet wird. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>userFromRequest</code> – nur gültig mit [HTTP-Trigger]. Verwendet die Identität des aufrufenden Benutzers.</li><li><code>userFromId</code> – verwendet die Identität eines zuvor angemeldeten Benutzers mit der angegebenen ID. Weitere Informationen finden Sie unter der <code>userId</code>-Eigenschaft.</li><li><code>userFromToken</code> – verwendet die Identität, die durch das angegebene Token dargestellt wird. Weitere Informationen finden Sie unter der <code>userToken</code>-Eigenschaft.</li><li><code>clientCredentials</code> – verwendet die Identität der Funktions-App.</li></ul>|
|**userId** |Genau dann erforderlich, wenn _Identität_ auf `userFromId` festgelegt ist. Eine Benutzerprinzipal-ID, die einem zuvor angemeldeten Benutzer zugeordnet ist.|
|**userToken**|Genau dann erforderlich, wenn _Identität_ auf `userFromToken` festgelegt ist. Ein gültiges Token für die Funktions-App. |
|**resource**|Erforderlich – eine AAD-Ressourcen-URL, für die das Token angefordert wird.|

<a name="token-input-code"></a>
### <a name="using-an-auth-token-input-binding-from-code"></a>Verwenden einer Autorisierungstoken-Eingabebindung in Code

In Code wird das Token immer als Zeichenfolge angegeben.

#### <a name="sample-getting-user-profile-information"></a>Beispiel: Abrufen von Benutzerprofilinformationen

Angenommen, Sie haben die folgende „function.json“, die einen HTTP-Trigger mit einer Tokeneingabebindung definiert:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel wird mit dem Token ein HTTP-Aufruf an Microsoft Graph durchgeführt und das Ergebnis zurückgegeben:

```csharp
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers; 

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, TraceWriter log)
{
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
    return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");
}
```

Im folgenden JS-Beispiel wird mit dem Token ein HTTP-Aufruf an Microsoft Graph durchgeführt und das Ergebnis zurückgegeben. Ändern Sie in `function.json` oben zuerst `$return` in `res`.

```js
const rp = require('request-promise');

module.exports = function (context, req) {
    let token = "Bearer " + context.bindings.graphToken;

    let options = {
        uri: 'https://graph.microsoft.com/v1.0/me/',
        headers: {
            'Authorization': token
        }
    };
    
    rp(options)
        .then(function(profile) {
            context.res = {
                body: profile
            };
            context.done();
        })
        .catch(function(err) {
            context.res = {
                status: 500,
                body: err
            };
            context.done();
        });
};
```





<a name="excel-input"></a>
## <a name="excel-table-input-binding"></a>Excel-Tabelleneingabebindung

Diese Bindung liest den Inhalt einer Excel-Tabelle, die in OneDrive gespeichert ist.

### <a name="configuring-an-excel-table-input-binding"></a>Konfigurieren einer Excel-Tabelleneingabebindung

Diese Bindung erfordert folgende AAD-Berechtigungen:
|Ressource|Berechtigung|
|--------|--------|
|Microsoft Graph|Benutzerdateien lesen|

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für die Excel-Tabelle verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer Excel-Tabelleneingabebindung in Code](#excel-input-code).|
|**type**|Erforderlich – muss auf `excel` festgelegt sein.|
|**direction**|Erforderlich – muss auf `in` festgelegt sein.|
|**Identität**|Erforderlich – die Identität, die zum Ausführen der Aktion verwendet wird. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>userFromRequest</code> – nur gültig mit [HTTP-Trigger]. Verwendet die Identität des aufrufenden Benutzers.</li><li><code>userFromId</code> – verwendet die Identität eines zuvor angemeldeten Benutzers mit der angegebenen ID. Weitere Informationen finden Sie unter der <code>userId</code>-Eigenschaft.</li><li><code>userFromToken</code> – verwendet die Identität, die durch das angegebene Token dargestellt wird. Weitere Informationen finden Sie unter der <code>userToken</code>-Eigenschaft.</li><li><code>clientCredentials</code> – verwendet die Identität der Funktions-App.</li></ul>|
|**userId** |Genau dann erforderlich, wenn _Identität_ auf `userFromId` festgelegt ist. Eine Benutzerprinzipal-ID, die einem zuvor angemeldeten Benutzer zugeordnet ist.|
|**userToken**|Genau dann erforderlich, wenn _Identität_ auf `userFromToken` festgelegt ist. Ein gültiges Token für die Funktions-App. |
|**path**|Erforderlich – der Pfad in OneDrive zur Excel-Arbeitsmappe.|
|**worksheetName**|Das Arbeitsblatt, in dem die Tabelle zu finden ist.|
|**tableName**|Der Name der Tabelle. Wenn nicht angegeben, wird der Inhalt des Arbeitsblatts verwendet.|

<a name="excel-input-code"></a>
### <a name="using-an-excel-table-input-binding-from-code"></a>Verwenden einer Excel-Tabelleneingabebindung in Code

Die Bindung macht die folgenden Typen für .NET-Funktionen verfügbar:
- string[][]
- Microsoft.Graph.WorkbookTable
- Benutzerdefinierte Objekttypen (unter Verwendung von struktureller Modellbindung)

#### <a name="sample-reading-an-excel-table"></a>Beispiel: Lesen einer Excel-Tabelle

Angenommen, Sie haben die folgende „function.json“, die einen HTTP-Trigger mit einer Excel-Eingabebindung definiert:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel wird der Inhalt der angegebenen Tabelle gelesen und an den Benutzer zurückgegeben:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives; 

public static IActionResult Run(HttpRequest req, string[][] excelTableData, TraceWriter log)
{
    return new OkObjectResult(excelTableData);
}
```

Im folgenden JS-Beispiel wird der Inhalt der angegebenen Tabelle gelesen und an den Benutzer zurückgegeben. Ändern Sie in `function.json` oben zuerst `$return` in `res`.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

<a name="excel-output"></a>
## <a name="excel-table-output-binding"></a>Excel-Tabellenausgabebindung

Diese Bindung ändert den Inhalt einer Excel-Tabelle, die in OneDrive gespeichert ist.

### <a name="configuring-an-excel-table-output-binding"></a>Konfigurieren einer Excel-Tabellenausgabebindung

Diese Bindung erfordert folgende AAD-Berechtigungen:
|Ressource|Berechtigung|
|--------|--------|
|Microsoft Graph|Vollzugriff auf Benutzerdateien|

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für das Autorisierungstoken verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer Excel-Tabellenausgabebindung in Code](#excel-output-code).|
|**type**|Erforderlich – muss auf `excel` festgelegt sein.|
|**direction**|Erforderlich – muss auf `out` festgelegt sein.|
|**Identität**|Erforderlich – die Identität, die zum Ausführen der Aktion verwendet wird. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>userFromRequest</code> – nur gültig mit [HTTP-Trigger]. Verwendet die Identität des aufrufenden Benutzers.</li><li><code>userFromId</code> – verwendet die Identität eines zuvor angemeldeten Benutzers mit der angegebenen ID. Weitere Informationen finden Sie unter der <code>userId</code>-Eigenschaft.</li><li><code>userFromToken</code> – verwendet die Identität, die durch das angegebene Token dargestellt wird. Weitere Informationen finden Sie unter der <code>userToken</code>-Eigenschaft.</li><li><code>clientCredentials</code> – verwendet die Identität der Funktions-App.</li></ul>|
|**userId** |Genau dann erforderlich, wenn _Identität_ auf `userFromId` festgelegt ist. Eine Benutzerprinzipal-ID, die einem zuvor angemeldeten Benutzer zugeordnet ist.|
|**userToken**|Genau dann erforderlich, wenn _Identität_ auf `userFromToken` festgelegt ist. Ein gültiges Token für die Funktions-App. |
|**path**|Erforderlich – der Pfad in OneDrive zur Excel-Arbeitsmappe.|
|**worksheetName**|Das Arbeitsblatt, in dem die Tabelle zu finden ist.|
|**tableName**|Der Name der Tabelle. Wenn nicht angegeben, wird der Inhalt des Arbeitsblatts verwendet.|
|**updateType**|Erforderlich – die Art der Änderung, die an der Tabelle vorgenommen werden soll. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>update</code> – ersetzt den Inhalt der Tabelle in OneDrive.</li><li><code>append</code> – fügt die Nutzlast durch Erstellen neuer Zeilen am Ende der Tabelle in OneDrive hinzu.</li></ul>|

<a name="excel-output-code"></a>
### <a name="using-an-excel-table-output-binding-from-code"></a>Verwenden einer Excel-Tabellenausgabebindung in Code

Die Bindung macht die folgenden Typen für .NET-Funktionen verfügbar:
- string[][]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Benutzerdefinierte Objekttypen (unter Verwendung von struktureller Modellbindung)

#### <a name="sample-adding-rows-to-an-excel-table"></a>Beispiel: Hinzufügen von Zeilen zu einer Excel-Tabelle

Angenommen, Sie haben die folgende „function.json“, die einen HTTP-Trigger mit einer Excel-Ausgabebindung definiert:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```


Im folgenden C#-Beispiel wird der Tabelle (hier mit nur einer Spalte) anhand einer Eingabe aus der Abfragezeichenfolge eine neue Zeile hinzugefügt:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, IAsyncCollector<object> newExcelRow, TraceWriter log)
{
    string input = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await newExcelRow.AddAsync(new {
        Text = input
        // Add other properties for additional columns here
    });
    return;
}
```

Im folgenden JS-Beispiel wird der Tabelle (hier mit nur einer Spalte) anhand einer Eingabe aus der Abfragezeichenfolge eine neue Zeile hinzugefügt. Ändern Sie in `function.json` oben zuerst `$return` in `res`.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```




<a name="onedrive-input"></a>
## <a name="onedrive-file-input-binding"></a>OneDrive-Dateieingabebindung

Diese Bindung liest den Inhalt einer in OneDrive gespeicherten Datei.

### <a name="configuring-a-onedrive-file-input-binding"></a>Konfigurieren einer OneDrive-Dateieingabebindung

Diese Bindung erfordert folgende AAD-Berechtigungen:
|Ressource|Berechtigung|
|--------|--------|
|Microsoft Graph|Benutzerdateien lesen|

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für die Datei verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer OneDrive-Dateieingabebindung in Code](#onedrive-input-code).|
|**type**|Erforderlich – muss auf `onedrive` festgelegt sein.|
|**direction**|Erforderlich – muss auf `in` festgelegt sein.|
|**Identität**|Erforderlich – die Identität, die zum Ausführen der Aktion verwendet wird. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>userFromRequest</code> – nur gültig mit [HTTP-Trigger]. Verwendet die Identität des aufrufenden Benutzers.</li><li><code>userFromId</code> – verwendet die Identität eines zuvor angemeldeten Benutzers mit der angegebenen ID. Weitere Informationen finden Sie unter der <code>userId</code>-Eigenschaft.</li><li><code>userFromToken</code> – verwendet die Identität, die durch das angegebene Token dargestellt wird. Weitere Informationen finden Sie unter der <code>userToken</code>-Eigenschaft.</li><li><code>clientCredentials</code> – verwendet die Identität der Funktions-App.</li></ul>|
|**userId** |Genau dann erforderlich, wenn _Identität_ auf `userFromId` festgelegt ist. Eine Benutzerprinzipal-ID, die einem zuvor angemeldeten Benutzer zugeordnet ist.|
|**userToken**|Genau dann erforderlich, wenn _Identität_ auf `userFromToken` festgelegt ist. Ein gültiges Token für die Funktions-App. |
|**path**|Erforderlich – der Pfad in OneDrive zur Datei.|

<a name="onedrive-input-code"></a>
### <a name="using-a-onedrive-file-input-binding-from-code"></a>Verwenden einer OneDrive-Dateieingabebindung in Code

Die Bindung macht die folgenden Typen für .NET-Funktionen verfügbar:
- Byte[]
- Datenstrom
- string
- Microsoft.Graph.DriveItem

#### <a name="sample-reading-a-file-from-onedrive"></a>Beispiel: Lesen einer Datei aus OneDrive

Angenommen, Sie haben die folgende „function.json“, die einen HTTP-Trigger mit einer OneDrive-Eingabebindung definiert:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel wird die in der Abfragezeichenfolge angegebene Datei gelesen und ihre Länge protokolliert:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

Im folgenden JS-Beispiel wird die in der Abfragezeichenfolge angegebene Datei gelesen und ihre Länge zurückgegeben. Ändern Sie in `function.json` oben zuerst `$return` in `res`.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```


<a name="onedrive-output"></a>
## <a name="onedrive-file-output-binding"></a>OneDrive-Dateiausgabebindung

Diese Bindung ändert den Inhalt einer in OneDrive gespeicherten Datei.

### <a name="configuring-a-onedrive-file-output-binding"></a>Konfigurieren einer OneDrive-Dateiausgabebindung

Diese Bindung erfordert folgende AAD-Berechtigungen:
|Ressource|Berechtigung|
|--------|--------|
|Microsoft Graph|Vollzugriff auf Benutzerdateien|

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für die Datei verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer OneDrive-Dateiausgabebindung in Code](#onedrive-output-code).|
|**type**|Erforderlich – muss auf `onedrive` festgelegt sein.|
|**direction**|Erforderlich – muss auf `out` festgelegt sein.|
|**Identität**|Erforderlich – die Identität, die zum Ausführen der Aktion verwendet wird. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>userFromRequest</code> – nur gültig mit [HTTP-Trigger]. Verwendet die Identität des aufrufenden Benutzers.</li><li><code>userFromId</code> – verwendet die Identität eines zuvor angemeldeten Benutzers mit der angegebenen ID. Weitere Informationen finden Sie unter der <code>userId</code>-Eigenschaft.</li><li><code>userFromToken</code> – verwendet die Identität, die durch das angegebene Token dargestellt wird. Weitere Informationen finden Sie unter der <code>userToken</code>-Eigenschaft.</li><li><code>clientCredentials</code> – verwendet die Identität der Funktions-App.</li></ul>|
|**userId** |Genau dann erforderlich, wenn _Identität_ auf `userFromId` festgelegt ist. Eine Benutzerprinzipal-ID, die einem zuvor angemeldeten Benutzer zugeordnet ist.|
|**userToken**|Genau dann erforderlich, wenn _Identität_ auf `userFromToken` festgelegt ist. Ein gültiges Token für die Funktions-App. |
|**path**|Erforderlich – der Pfad in OneDrive zur Datei.|

<a name="onedrive-output-code"></a>
### <a name="using-a-onedrive-file-output-binding-from-code"></a>Verwenden einer OneDrive-Dateiausgabebindung in Code

Die Bindung macht die folgenden Typen für .NET-Funktionen verfügbar:
- Byte[]
- Datenstrom
- string
- Microsoft.Graph.DriveItem

#### <a name="sample-writing-to-a-file-in-onedrive"></a>Beispiel: Schreiben in eine Datei in OneDrive

Angenommen, Sie haben die folgende „function.json“, die einen HTTP-Trigger mit einer OneDrive-Ausgabebindung definiert:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel wird Text aus der Abfragezeichenfolge abgerufen und in eine Textdatei („FunctionsTest.txt“ gemäß Definition in der Konfiguration oben) im Stammverzeichnis des OneDrive des Aufrufers geschrieben:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, TraceWriter log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    return;
}
```
Im folgenden JS-Beispiel wird Text aus der Abfragezeichenfolge abgerufen und in eine Textdatei („FunctionsTest.txt“ gemäß Definition in der Konfiguration oben) im Stammverzeichnis des OneDrive des Aufrufers geschrieben. Ändern Sie in `function.json` oben zuerst `$return` in `res`.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```



<a name="outlook-output"></a>
## <a name="outlook-message-output-binding"></a>Outlook-Nachrichtenausgabebindung

Sendet eine E-Mail-Nachricht über Outlook.

### <a name="configuring-an-outlook-message-output-binding"></a>Konfigurieren einer Outlook-Nachrichtenausgabebindung

Diese Bindung erfordert folgende AAD-Berechtigungen:
|Ressource|Berechtigung|
|--------|--------|
|Microsoft Graph|E-Mail als Benutzer senden|

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für die E-Mail-Nachricht verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer Outlook-Nachrichtenausgabebindung in Code](#outlook-output-code).|
|**type**|Erforderlich – muss auf `outlook` festgelegt sein.|
|**direction**|Erforderlich – muss auf `out` festgelegt sein.|
|**Identität**|Erforderlich – die Identität, die zum Ausführen der Aktion verwendet wird. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>userFromRequest</code> – nur gültig mit [HTTP-Trigger]. Verwendet die Identität des aufrufenden Benutzers.</li><li><code>userFromId</code> – verwendet die Identität eines zuvor angemeldeten Benutzers mit der angegebenen ID. Weitere Informationen finden Sie unter der <code>userId</code>-Eigenschaft.</li><li><code>userFromToken</code> – verwendet die Identität, die durch das angegebene Token dargestellt wird. Weitere Informationen finden Sie unter der <code>userToken</code>-Eigenschaft.</li><li><code>clientCredentials</code> – verwendet die Identität der Funktions-App.</li></ul>|
|**userId** |Genau dann erforderlich, wenn _Identität_ auf `userFromId` festgelegt ist. Eine Benutzerprinzipal-ID, die einem zuvor angemeldeten Benutzer zugeordnet ist.|
|**userToken**|Genau dann erforderlich, wenn _Identität_ auf `userFromToken` festgelegt ist. Ein gültiges Token für die Funktions-App. |

<a name="outlook-output-code"></a>
### <a name="using-an-outlook-message-output-binding-from-code"></a>Verwenden einer Outlook-Nachrichtenausgabebindung in Code

Die Bindung macht die folgenden Typen für .NET-Funktionen verfügbar:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- string
- Benutzerdefinierte Objekttypen (unter Verwendung von struktureller Modellbindung)

#### <a name="sample-sending-an-email-through-outlook"></a>Beispiel: Senden einer E-Mail über Outlook

Angenommen, Sie haben die folgende „function.json“, die einen HTTP-Trigger mit einer Outlook-Nachrichtenausgabebindung definiert:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel wird eine E-Mail vom Aufrufer an einen in der Abfragezeichenfolge angegebenen Empfänger gesendet:

```csharp
using System.Net;

public static void Run(HttpRequest req, out Message message, TraceWriter log)
{ 
    string emailAddress = req.Query["to"];
    message = new Message(){
        subject = "Greetings",
        body = "Sent from Azure Functions",
        recipient = new Recipient() {
            address = emailAddress
        }
    };
}

public class Message {
    public String subject {get; set;}
    public String body {get; set;}
    public Recipient recipient {get; set;}
}

public class Recipient {
    public String address {get; set;}
    public String name {get; set;}
}
```

Im folgenden JS-Beispiel wird eine E-Mail vom Aufrufer an einen in der Abfragezeichenfolge angegebenen Empfänger gesendet:

```js
module.exports = function (context, req) {
    context.bindings.message = {
        subject: "Greetings",
        body: "Sent from Azure Functions with JavaScript",
        recipient: {
            address: req.query.to 
        } 
    };
    context.done();
};
```





<a name="webhooks"></a>
## <a name="microsoft-graph-webhook-bindings"></a>Microsoft Graph-Webhookbindungen

Mit Webhooks können Sie auf Ereignisse in Microsoft Graph reagieren. Zur Unterstützung von Webhooks sind Funktionen erforderlich, die _Webhookabonnements_ erstellen, aktualisieren und auf diese reagieren. Für eine vollständige Webhooklösung ist eine Kombination aus den folgenden Bindungen erforderlich:
- Ein [Microsoft Graph-Webhooktrigger](#webhook-trigger) ermöglicht es Ihnen, auf einen eingehenden Webhook zu reagieren.
- Eine [Microsoft Graph-Webhookabonnement-Eingabebindung](#webhook-input) ermöglicht Ihnen das Auflisten und optionale Aktualisieren von vorhandenen Abonnements.
- Eine [Microsoft Graph-Webhookabonnement-Ausgabebindung](#webhook-output) ermöglicht Ihnen das Erstellen und Löschen von Webhookabonnements.

Für die eigentlichen Bindungen sind keine AAD-Berechtigungen erforderlich, aber Sie benötigen die entsprechenden Berechtigungen für den Ressourcentyp, auf den Sie reagieren möchten. Eine Liste der für jeden Ressourcentyp erforderlichen Berechtigungen finden Sie unter [Abonnementberechtigungen](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions#permissions).

Weitere Informationen über Webhooks finden Sie unter [Arbeiten mit Webhooks in Microsoft Graph].





<a name="webhook-trigger"></a>
### <a name="microsoft-graph-webhook-trigger"></a>Microsoft Graph-Webhooktrigger

Ein Trigger ermöglicht es einer Funktion, auf einen eingehenden Webhook von Microsoft Graph zu reagieren. Jede Instanz dieses Triggers kann auf einen Microsoft Graph Ressourcentyp reagieren.

#### <a name="configuring-a-microsoft-graph-webhook-trigger"></a>Konfigurieren eines Microsoft Graph-Webhooktriggers

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für die E-Mail-Nachricht verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer Outlook-Nachrichtenausgabebindung in Code](#outlook-output-code).|
|**type**|Erforderlich – muss auf `graphWebhook` festgelegt sein.|
|**direction**|Erforderlich – muss auf `trigger` festgelegt sein.|
|**resourceType**|Erforderlich – die Graph-Ressource, für die diese Funktion auf Webhooks reagieren soll. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>#Microsoft.Graph.Message</code> – Änderungen an Outlook-Nachrichten.</li><li><code>#Microsoft.Graph.DriveItem</code> – Änderungen an OneDrive-Stammelementen.</li><li><code>#Microsoft.Graph.Contact - changes made to personal contacts in Outlook.</code></li><li><code>#Microsoft.Graph.Event - changes made to Outlook calendar items.</code></li></ul>|

> [!Note]
> Eine Funktions-App kann nur über eine Funktion verfügen, die für einen bestimmten `resourceType`-Wert registriert wurde.

#### <a name="using-a-microsoft-graph-webhook-trigger-from-code"></a>Verwenden eines Microsoft Graph-Webhooktriggers in Code

Die Bindung macht die folgenden Typen für .NET-Funktionen verfügbar:
- Relevante Microsoft Graph SDK-Typen für die Ressource, z.B. Microsoft.Graph.Message, Microsoft.Graph.DriveItem
- Benutzerdefinierte Objekttypen (unter Verwendung von struktureller Modellbindung)

Beispiele für die Verwendung dieser Bindung in Code finden Sie unter [Microsoft Graph-Webhookbeispiele](#webhook-samples).



<a name="webhook-input"></a>
### <a name="microsoft-graph-webhook-subscription-input-binding"></a>Microsoft Graph-Webhookabonnement-Eingabebindung

Mit dieser Bindung können Sie die Liste der von dieser Funktions-App verwalteten Abonnements abrufen. Die Bindung liest aus dem Speicher der Funktions-App und gibt keine anderen Abonnements außerhalb der App wieder.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-input-binding"></a>Konfigurieren einer Microsoft Graph-Webhookabonnement-Eingabebindung

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für die E-Mail-Nachricht verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer Outlook-Nachrichtenausgabebindung in Code](#outlook-output-code).|
|**type**|Erforderlich – muss auf `graphWebhookSubscription` festgelegt sein.|
|**direction**|Erforderlich – muss auf `in` festgelegt sein.|
|**filter**| Bei Festlegung auf `userFromRequest` ruft die Bindung nur Abonnements im Besitz des aufrufenden Benutzers ab (nur gültig mit [HTTP-Trigger]).| 

#### <a name="using-a-microsoft-graph-webhook-subscription-input-binding-from-code"></a>Verwenden einer Microsoft Graph-Webhookabonnement-Eingabebindung in Code

Die Bindung macht die folgenden Typen für .NET-Funktionen verfügbar:
- string[]
- Benutzerdefinierte Objekttyparrays
- Newtonsoft.Json.Linq.JObject[]
- Microsoft.Graph.Subscription[]

Beispiele für die Verwendung dieser Bindung in Code finden Sie unter [Microsoft Graph-Webhookbeispiele](#webhook-samples).


<a name="webhook-output"></a>
### <a name="microsoft-graph-webhook-subscription-output-binding"></a>Microsoft Graph-Webhookabonnement-Ausgabebindung

Mit dieser Bindung können Sie Webhookabonnements in Microsoft Graph erstellen, löschen und aktualisieren.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-output-binding"></a>Konfigurieren einer Microsoft Graph-Webhookabonnement-Ausgabebindung

Die Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|
|--------|--------|
|**name**|Erforderlich – der Variablenname, der im Funktionscode für die E-Mail-Nachricht verwendet wird. Weitere Informationen finden Sie unter [Verwenden einer Outlook-Nachrichtenausgabebindung in Code](#outlook-output-code).|
|**type**|Erforderlich – muss auf `graphWebhookSubscription` festgelegt sein.|
|**direction**|Erforderlich – muss auf `out` festgelegt sein.|
|**Identität**|Erforderlich – die Identität, die zum Ausführen der Aktion verwendet wird. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>userFromRequest</code> – nur gültig mit [HTTP-Trigger]. Verwendet die Identität des aufrufenden Benutzers.</li><li><code>userFromId</code> – verwendet die Identität eines zuvor angemeldeten Benutzers mit der angegebenen ID. Weitere Informationen finden Sie unter der <code>userId</code>-Eigenschaft.</li><li><code>userFromToken</code> – verwendet die Identität, die durch das angegebene Token dargestellt wird. Weitere Informationen finden Sie unter der <code>userToken</code>-Eigenschaft.</li><li><code>clientCredentials</code> – verwendet die Identität der Funktions-App.</li></ul>|
|**userId** |Genau dann erforderlich, wenn _Identität_ auf `userFromId` festgelegt ist. Eine Benutzerprinzipal-ID, die einem zuvor angemeldeten Benutzer zugeordnet ist.|
|**userToken**|Genau dann erforderlich, wenn _Identität_ auf `userFromToken` festgelegt ist. Ein gültiges Token für die Funktions-App. |
|**action**|Erforderlich – gibt die Aktion an, die die Bindung durchführen soll. Es kann sich um einen der folgenden Werte handeln:<ul><li><code>create</code> – registriert ein neues Abonnement.</li><li><code>delete</code> – löscht ein angegebenes Abonnement.</li><li><code>refresh</code> – aktualisiert ein angegebenes Abonnement, damit es nicht abläuft.</li></ul>|
|**subscriptionResource**|Genau dann erforderlich, wenn die _Aktion_ auf `create` festgelegt ist. Gibt die Microsoft Graph-Ressource an, die auf Änderungen überwacht werden soll. Weitere Informationen finden Sie unter [Arbeiten mit Webhooks in Microsoft Graph]. |
|**changeType**|Genau dann erforderlich, wenn die _Aktion_ auf `create` festgelegt ist. Gibt den Typ der Änderung in der abonnierten Ressource an, der eine Benachrichtigung auslöst. Die unterstützten Werte sind: `created`, `updated`, `deleted`. Mehrere Werte können mit einer durch Trennzeichen getrennten Liste kombiniert werden.|

#### <a name="using-a-microsoft-graph-webhook-subscription-output-binding-from-code"></a>Verwenden einer Microsoft Graph-Webhookabonnement-Ausgabebindung in Code

Die Bindung macht die folgenden Typen für .NET-Funktionen verfügbar:
- string
- Microsoft.Graph.Subscription

Beispiele für die Verwendung dieser Bindung in Code finden Sie unter [Microsoft Graph-Webhookbeispiele](#webhook-samples).
 
<a name="webhook-samples"></a>
### <a name="microsoft-graph-webhook-samples"></a>Beispiele für Microsoft Graph-Webhooks

#### <a name="sample-creating-a-subscription"></a>Beispiel: Erstellen eines Abonnements

Angenommen, Sie haben die folgende „function.json“, die einen HTTP-Trigger mit einer Abonnementausgabebindung mit der create-Aktion definiert:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphwebhook",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "listen": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel wird ein Webhook registriert, der diese Funktions-App benachrichtigt, wenn der aufrufende Benutzer eine Outlook-Nachricht empfängt:

```csharp
using System;
using System.Net;

public static HttpResponseMessage run(HttpRequestMessage req, out string clientState, TraceWriter log)
{
  log.Info("C# HTTP trigger function processed a request.");
    clientState = Guid.NewGuid().ToString();
    return new HttpResponseMessage(HttpStatusCode.OK);
}
```

Im folgenden JS-Beispiel wird ein Webhook registriert, der diese Funktions-App benachrichtigt, wenn der aufrufende Benutzer eine Outlook-Nachricht empfängt:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

#### <a name="sample-handling-notifications"></a>Beispiel: Behandeln von Benachrichtigungen

Angenommen, Sie haben die folgende „function.json“, die Graph-Webhooktrigger zur Behandlung von Outlook-Nachrichten definiert:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel wird auf eingehende E-Mail-Nachrichten reagiert und der Text der durch den Empfänger gesendeten Nachrichten, die „Azure Functions“ in der Betreffzeile enthalten, protokolliert:

```csharp
#r "Microsoft.Graph"
using Microsoft.Graph;
using System.Net;

public static async Task Run(Message msg, TraceWriter log)  
{
    log.Info("Microsoft Graph webhook trigger function processed a request.");

    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if (msg.Subject.Contains("Azure Functions") && msg.From.Equals(msg.Sender)) {
        log.Info($"Processed email: {msg.BodyPreview}");
    }
}
```

Im folgenden JS-Beispiel wird auf eingehende E-Mail-Nachrichten reagiert und der Text der durch den Empfänger gesendeten Nachrichten, die „Azure Functions“ in der Betreffzeile enthalten, protokolliert:

```js
module.exports = function (context) {
    context.log("Microsoft Graph webhook trigger function processed a request.");
    const msg = context.bindings.msg
    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if((msg.subject.indexOf("Azure Functions") > -1) && (msg.from === msg.sender) ) {
      context.log(`Processed email: ${msg.bodyPreview}`);
    }
    context.done();
};
```

#### <a name="sample-deleting-subscriptions"></a>Beispiel: Löschen von Abonnements

Angenommen, Sie haben die folgende „function.json“, die einen HTTP-Trigger mit einer Abonnementeingabebindung und einer Abonnementausgabebindung mit der delete-Aktion definiert:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel werden alle Abonnements für den aufrufenden Benutzer abgerufen und dann gelöscht:

```csharp
using System.Net;

public static async Task Run(HttpRequest req, string[] existingSubscriptions, IAsyncCollector<string> subscriptionsToDelete, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    foreach (var subscription in existingSubscriptions)
    {
        log.Info($"Deleting subscription {subscription}");
        await subscriptionsToDelete.AddAsync(subscription);
    }
}
```

Im folgenden JS-Beispiel werden alle Abonnements für den aufrufenden Benutzer abgerufen und dann gelöscht:

```js
module.exports = function (context, req) {
    const existing = context.bindings.existingSubscriptions;
    var toDelete = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToDelete = toDelete;
    context.done();
};
```

#### <a name="sample-refreshing-subscriptions"></a>Beispiel: Aktualisieren von Abonnements

Es gibt zwei Ansätze zum Aktualisieren von Abonnements:
- Verwenden der Anwendungsidentität zum Verarbeiten aller Abonnements. Dies erfordert die Zustimmung eines Azure Active Directory-Administrators. Dieser Ansatz kann in allen Sprachen verwendet werden, die von Azure Functions unterstützt werden.
- Verwenden der Identität für jedes Abonnement durch manuelles Binden jeder Benutzer-ID. Dies erfordert einigen benutzerdefinierten Code zum Durchführen der Bindung. Dieser Ansatz kann nur von .NET Funktionen verwendet werden.

Beide Optionen werden nachstehend vorgestellt.

**Verwenden der Anwendungsidentität**

Angenommen, Sie haben die folgende „function.json“, die einen Timertrigger mit einer Abonnementeingabebindung und einer Abonnementausgabebindung mit der Anwendungsidentität definiert:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel werden die Abonnements über einen Timer mithilfe der Anwendungsidentität aktualisiert:

```csharp
using System;

public static void Run(TimerInfo myTimer, string[] existingSubscriptions, ICollector<string> subscriptionsToRefresh, TraceWriter log)
{
    // This template uses application permissions and requires consent from an Azure Active Directory admin.
    // See https://go.microsoft.com/fwlink/?linkid=858780
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
      log.Info($"Refreshing subscription {subscription}");
      subscriptionsToRefresh.Add(subscription);
    }
}
```

Im folgenden JS-Beispiel werden die Abonnements über einen Timer mithilfe der Anwendungsidentität aktualisiert:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

**Verwenden von dynamischen Benutzeridentitäten**

Für die alternative Option wird angenommen, Sie haben die folgende „function.json“, die einen Timertrigger definiert, um die Bindung an die Abonnementeingabebindung an den Funktionscode zu verzögern:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Im folgenden C#-Beispiel werden die Abonnements über einen Timer mithilfe der Identität der einzelnen Benutzers durch Erstellen der Ausgabebindung im Code aktualisiert:
```csharp
using System;

public static async Task Run(TimerInfo myTimer, UserSubscription[] existingSubscriptions, IBinder binder, TraceWriter log)
{
  log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
        // binding in code to allow dynamic identity
        using (var subscriptionsToRefresh = await binder.BindAsync<IAsyncCollector<string>>(
            new GraphWebhookSubscriptionAttribute() {
                Action = "refresh",
                Identity = "userFromId",
                UserId = subscription.UserId
            }
        ))
        {
            log.Info($"Refreshing subscription {subscription}");
            await subscriptionsToRefresh.AddAsync(subscription);
        }

    }
}

public class UserSubscription {
    public string UserId {get; set;}
    public string Id {get; set;}
}
```



[HTTP-Trigger]: functions-bindings-http-webhook.md
[Arbeiten mit Webhooks in Microsoft Graph]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks

