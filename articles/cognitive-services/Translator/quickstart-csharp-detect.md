---
title: 'Schnellstart: Ermitteln der Textsprache, C#: Textübersetzungs-API'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie die Sprache des bereitgestellten Texts mithilfe von .NET Core und der Textübersetzungs-REST-API ermitteln.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: d27d385fa6fba93b7221d241f46894e9cfb9dea6
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729457"
---
# <a name="quickstart-use-the-translator-text-api-to-detect-text-language-using-c"></a>Schnellstart: Verwenden der Textübersetzungs-API zur Erkennung der Textsprache mit C#

In dieser Schnellstartanleitung erfahren Sie, wie Sie die Sprache des bereitgestellten Texts mithilfe von .NET Core und der Textübersetzungs-REST-API ermitteln.

Für diese Schnellstartanleitung wird ein [Azure Cognitive Services-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit einer Textübersetzungsressource benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/) einen Abonnementschlüssel abrufen.

## <a name="prerequisites"></a>Voraussetzungen

* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET-NuGet-Paket](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) oder ein anderer Editor
* Ein Azure-Abonnementschlüssel für die Textübersetzung

## <a name="create-a-net-core-project"></a>Erstellen eines .NET Core-Projekts

Öffnen Sie eine neue Eingabeaufforderung (oder Terminalsitzung), und führen Sie die folgenden Befehle aus:

```console
dotnet new console -o detect-sample
cd detect-sample
```

Mit dem ersten Befehl werden zwei Aufgaben ausgeführt. Es werden eine neue .NET-Konsolenanwendung und ein Verzeichnis mit dem Namen `detect-sample` erstellt. Mit dem zweiten Befehl wird in das Verzeichnis für Ihr Projekt gewechselt.

Als Nächstes müssen Sie Json.Net installieren. Führen Sie im Verzeichnis Ihres Projekts den folgenden Befehl aus:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Hinzufügen von erforderlichen Namespaces zu Ihrem Projekt

Mit dem zuvor ausgeführten Befehl `dotnet new console` wurde ein Projekt (einschließlich `Program.cs`) erstellt. In diese Datei fügen Sie Ihren Anwendungscode ein. Öffnen Sie `Program.cs`, und ersetzen Sie die vorhandenen using-Anweisungen. Mit diesen Anweisungen wird sichergestellt, dass Sie Zugriff auf alle Typen haben, die zum Erstellen und Ausführen der Beispiel-App erforderlich sind.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-detect-the-source-texts-language"></a>Erstellen einer Funktion zum Ermitteln der Sprache des Quelltexts

Erstellen Sie in der `Program`-Klasse eine Funktion mit dem Namen `Detect`. Diese Klasse kapselt den zum Aufrufen der Ermittlungsressource verwendeten Code und gibt das Ergebnis in der Konsole aus.

```csharp
static void Detect()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Festlegen des Abonnementschlüssels, des Hostnamens und des Pfads

Fügen Sie der Funktion `Detect` die folgenden Zeilen hinzu:

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/detect?api-version=3.0";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

Als Nächstes müssen Sie das JSON-Objekt erstellen und serialisieren, das den Text enthält, für den die Sprachenerkennung ausgeführt wird.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"Salve mondo!" } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>Instanziieren des Clients und Senden einer Anforderung

Diese Zeilen instanziieren `HttpClient` und `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Erstellen der Anforderung und Ausgeben der Antwort

In `HttpRequestMessage` führen Sie die folgenden Aufgaben aus:

* Deklarieren der HTTP-Methode
* Erstellen des Anforderungs-URIs
* Einfügen des Anforderungstexts (serialisiertes JSON-Objekt)
* Hinzufügen der erforderlichen Header
* Senden einer asynchronen Anforderung
* Ausgeben der Antwort

Fügen Sie `HttpRequestMessage` den folgenden Code hinzu:

```csharp
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(jsonResponse);
Console.WriteLine("Press any key to continue.");
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Im letzten Schritt rufen Sie `Detect()` in der `Main`-Funktion auf. Suchen Sie `static void Main(string[] args)`, und fügen Sie die folgenden Zeilen hinzu:

```csharp
Detect();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Sie können Ihre Beispiel-App jetzt ausführen. Navigieren Sie über die Befehlszeile (oder Terminalsitzung) zu Ihrem Projektverzeichnis, und führen Sie Folgendes aus:

```console
dotnet run
```

## <a name="sample-response"></a>Beispiel für eine Antwort

```json
[
  {
    "language": "it",
    "score": 1.0,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "en",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie unbedingt alle vertraulichen Informationen wie etwa Abonnementschlüssel aus dem Quellcode Ihrer Beispiel-App.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Beispielcode für diese und andere Schnellstartanleitungen (einschließlich Übersetzung und Transliteration) sowie weitere Beispielprojekte für die Textübersetzung auf GitHub an.

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>Weitere Informationen

* [Übersetzen von Text](quickstart-csharp-translate.md)
* [Transliteration von Text](quickstart-csharp-transliterate.md)
* [Ermitteln alternativer Übersetzungen](quickstart-csharp-dictionary.md)
* [Abrufen einer Liste der unterstützten Sprachen](quickstart-csharp-languages.md)
* [Bestimmen der Satzlänge aus einer Eingabe](quickstart-csharp-sentences.md)
