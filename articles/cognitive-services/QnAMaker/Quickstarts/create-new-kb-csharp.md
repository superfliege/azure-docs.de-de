---
title: 'Schnellstart: Erstellen einer Wissensdatenbank – REST, C# – QnA Maker'
titlesuffix: Azure Cognitive Services
description: Diese Schnellstartanleitung führt Sie durch das programmgesteuerte Erstellen einer Beispielwissensdatenbank für QnA Maker, die auf dem Azure-Dashboard Ihres Cognitive Services-API-Kontos angezeigt wird.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/01/2018
ms.author: diberry
ms.openlocfilehash: 260ec46d292e244dfe51a3714e8b97e9d4689068
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883414"
---
# <a name="quickstart-create-a-qna-maker-knowledge-base-in-c"></a>Schnellstart: Erstellen einer QnA Maker-Wissensdatenbank in C#

In dieser Schnellstartanleitung wird das programmgesteuerte Erstellen eines Beispiels für eine QnA Maker-Wissensdatenbank Schritt für Schritt beschrieben. QnA Maker extrahiert automatisch Fragen und Antworten aus teilweise strukturiertem Inhalt (z.B. häufig gestellten Fragen) von [Datenquellen](../Concepts/data-sources-supported.md). Das Modell für die Wissensdatenbank ist im JSON-Code definiert, der im Text der API-Anforderung gesendet wird. 

In dieser Schnellstartanleitung werden QnA Maker-APIs aufgerufen:
* [Erstellen einer Wissensdatenbank](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
* [Abrufen von Vorgangsdetails](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Voraussetzungen

* Aktuelle [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/)
* Sie benötigen einen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md). Wählen Sie zum Abrufen des Schlüssels auf Ihrem Dashboard unter **Ressourcenverwaltung** die Option **Schlüssel** aus. 

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-csharp-repo-note.md)]

## <a name="create-a-knowledge-base-project"></a>Erstellen eines Wissensdatenbank-Projekts

[!INCLUDE [Create Visual Studio Project](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-create-project.md)] 

## <a name="add-the-required-dependencies"></a>Hinzufügen der erforderlichen Abhängigkeiten

[!INCLUDE [Add required constants to code file](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-required-dependencies.md)]  

## <a name="add-the-required-constants"></a>Hinzufügen der erforderlichen Konstanten

[!INCLUDE [Add required constants to code file](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-required-constants.md)] 

## <a name="add-the-kb-definition"></a>Hinzufügen der Definition der Wissensdatenbank

Fügen Sie nach den Konstanten die folgende Definition für die Wissensdatenbank hinzu:

```csharp
static string kb = @"
{
  'name': 'QnA Maker FAQ from quickstart',
  'qnaList': [
    {
      'id': 0,
      'answer': 'You can use our REST APIs to manage your knowledge base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/58994a073d9e04097c7ba6fe/operations/58994a073d9e041ad42d9baa',
      'source': 'Custom Editorial',
      'questions': [
        'How do I programmatically update my knowledge base?'
      ],
      'metadata': [
        {
          'name': 'category',
          'value': 'api'
        }
      ]
    }
  ],
  'urls': [
    'https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs',
    'https://docs.microsoft.com/en-us/bot-framework/resources-bot-framework-faq'
  ],
  'files': []
}
";
```

## <a name="add-supporting-functions-and-structures"></a>Hinzufügen von unterstützenden Funktionen und Strukturen

[!INCLUDE [Add supporting functions and structures](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-support-functions.md)] 

## <a name="add-a-post-request-to-create-kb"></a>Hinzufügen einer POST-Anforderung zum Erstellen der Wissensdatenbank

Der folgende Code sendet eine HTTPS-Anforderung an die QnA Maker-API, um eine Wissensdatenbank zu erstellen, und empfängt die Antwort:

```csharp
async static Task<Response> PostCreateKB(string kb)
{
    // Builds the HTTP request URI.
    string uri = host + service + method;

    // Writes the HTTP request URI to the console, for display purposes.
    Console.WriteLine("Calling " + uri + ".");

    // Asynchronously invokes the Post(string, string) method, using the
    // HTTP request URI and the specified data source.
    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = HttpMethod.Post;
        request.RequestUri = new Uri(uri);
        request.Content = new StringContent(kb, Encoding.UTF8, "application/json");
        request.Headers.Add("Ocp-Apim-Subscription-Key", key);

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();
        return new Response(response.Headers, responseBody);
    }
}
```

Mit diesem API-Aufruf wird eine JSON-Antwort zurückgegeben, die die Vorgangs-ID enthält. Verwenden Sie die Vorgangs-ID, um zu ermitteln, ob die Erstellung der Wissensdatenbank erfolgreich war. 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-get-request-to-determine-creation-status"></a>Hinzufügen einer GET-Anforderung zum Ermitteln des Erstellungsstatus

Überprüfen Sie den Status des Vorgangs.

```csharp
async static Task<Response> GetStatus(string operationID)
{
    // Builds the HTTP request URI.
    string uri = host + service + operationID;

    // Writes the HTTP request URI to the console, for display purposes.
    Console.WriteLine("Calling " + uri + ".");

    // Asynchronously invokes the Get(string) method, using the
    // HTTP request URI.
    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = HttpMethod.Get;
        request.RequestUri = new Uri(uri);
        request.Headers.Add("Ocp-Apim-Subscription-Key", key);

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();
        return new Response(response.Headers, responseBody);
    }
}
```

Dieser API-Aufruf gibt eine JSON-Antwort zurück, die den Vorgangsstatus enthält: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Wiederholen Sie den Aufruf, bis er erfolgreich abgeschlossen wird oder ein Fehler auftritt: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-createkb-method"></a>Hinzufügen der CreateKB-Methode

Mit der folgenden Methode wird die Wissensdatenbank erstellt, und der Status wird wiederholt überprüft. Da die Erstellung der Wissensdatenbank einige Zeit dauern kann, müssen Sie die Aufrufe zum Überprüfen des Status wiederholen, bis der Status entweder „Erfolgreich“ oder „Fehler“ lautet.

```csharp
async static void CreateKB()
{
    try
    {
        // Starts the QnA Maker operation to create the knowledge base.
        var response = await PostCreateKB(kb);

        // Retrieves the operation ID, so the operation's status can be
        // checked periodically.
        var operation = response.headers.GetValues("Location").First();

        // Displays the JSON in the HTTP response returned by the 
        // PostCreateKB(string) method.
        Console.WriteLine(PrettyPrint(response.response));

        // Iteratively gets the state of the operation creating the
        // knowledge base. Once the operation state is set to something other
        // than "Running" or "NotStarted", the loop ends.
        var done = false;
        while (true != done)
        {
            // Gets the status of the operation.
            response = await GetStatus(operation);

            // Displays the JSON in the HTTP response returned by the
            // GetStatus(string) method.
            Console.WriteLine(PrettyPrint(response.response));

            // Deserialize the JSON into key-value pairs, to retrieve the
            // state of the operation.
            var fields = JsonConvert.DeserializeObject<Dictionary<string, string>>(response.response);

            // Gets and checks the state of the operation.
            String state = fields["operationState"];
            if (state.CompareTo("Running") == 0 || state.CompareTo("NotStarted") == 0)
            {
                // QnA Maker is still creating the knowledge base. The thread is 
                // paused for a number of seconds equal to the Retry-After header value,
                // and then the loop continues.
                var wait = response.headers.GetValues("Retry-After").First();
                Console.WriteLine("Waiting " + wait + " seconds...");
                Thread.Sleep(Int32.Parse(wait) * 1000);
            }
            else
            {
                // QnA Maker has completed creating the knowledge base. 
                done = true;
            }
        }
    }
    catch
    {
        // An error occurred while creating the knowledge base. Ensure that
        // you included your QnA Maker subscription key where directed in the sample.
        Console.WriteLine("An error occurred while creating the knowledge base.");
    }
    finally
    {
        Console.WriteLine("Press any key to continue.");
    }

}
``` 

## <a name="add-the-createkb-method-to-main"></a>Hinzufügen der CreateKB-Methode zu „Main“

Ändern Sie die Main-Methode, um die CreateKB-Methode aufzurufen:

```csharp
static void Main(string[] args)
{
    // Call the CreateKB() method to create a knowledge base, periodically 
    // checking the status of the QnA Maker operation until the 
    // knowledge base is created.
    CreateKB();

    // The console waits for a key to be pressed before closing.
    Console.ReadLine();
}
```

## <a name="build-and-run-the-program"></a>Erstellen und Ausführen des Programms

Erstellen Sie das Programm, und führen Sie es aus. Die Anforderung wird automatisch an die QnA Maker-API gesendet, um die Wissensdatenbank zu erstellen, und dann werden die Ergebnisse alle 30 Sekunden abgefragt. Jede Antwort wird im Konsolenfenster ausgegeben.

Nach der Erstellung der Wissensdatenbank können Sie sie im QnA Maker-Portal auf der Seite [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (Meine Wissensdatenbanken) anzeigen. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
