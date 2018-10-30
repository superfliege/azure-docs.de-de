---
title: 'Schnellstart: Veröffentlichen einer Wissensdatenbank – REST, C# – QnA Maker'
titleSuffix: Azure Cognitive Services
description: In dieser REST-basierten Schnellstartanleitung wird das Veröffentlichen Ihrer Wissensdatenbank beschrieben, mit der die neueste Version der getesteten Wissensdatenbank per Pushvorgang in einen dedizierten Azure Search-Index übertragen wird, der die veröffentlichte Wissensdatenbank darstellt. Außerdem wird ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: ce027abb75423d0174a7175c3bbafe5c0fb3e157
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646261"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-c"></a>Schnellstart: Veröffentlichen einer Wissensdatenbank in QnA Maker mit C#

In dieser Schnellstartanleitung wird das programmgesteuerte Veröffentlichen Ihrer Wissensdatenbank (Knowledge Base, KB) Schritt für Schritt beschrieben. Bei der Veröffentlichung wird die aktuelle Version der Wissensdatenbank per Pushvorgang an einen dedizierten Azure Search-Index übertragen und ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.

In dieser Schnellstartanleitung werden QnA Maker-APIs aufgerufen:
* [Veröffentlichen](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): Für diese API sind keine Informationen im Text der Anforderung erforderlich.

## <a name="prerequisites"></a>Voraussetzungen

* Aktuelle [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/)
* Sie benötigen einen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md). Wählen Sie zum Abrufen des Schlüssels auf Ihrem Dashboard unter **Ressourcenverwaltung** die Option **Schlüssel** aus. 
* ID der QnA Maker-Wissensdatenbank, die in der URL des Abfragezeichenfolgenparameters „kbid“ enthalten ist, wie nachfolgend gezeigt:

    ![ID der QnA Maker-Wissensdatenbank](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Falls Sie noch keine Wissensdatenbank besitzen, können Sie für diese Schnellstartanleitung eine Beispieldatenbank erstellen: [Create a new knowledge base in Python](create-new-kb-csharp.md) (Erstellen einer neuen Wissensdatenbank in Python)

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-csharp-repo-note.md)]

## <a name="create-knowledge-base-project"></a>Erstellen eines Wissensdatenbank-Projekts

[!INCLUDE [Create Visual Studio Project](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-create-project.md)] 

## <a name="add-required-dependencies"></a>Hinzufügen von erforderlichen Abhängigkeiten

[!INCLUDE [Add required dependencies to code file](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-required-dependencies.md)] 

## <a name="add-required-constants"></a>Hinzufügen von erforderlichen Konstanten

[!INCLUDE [Add required constants to code file](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-required-constants.md)]  

## <a name="add-knowledge-base-id"></a>Hinzufügen der Wissensdatenbank-ID

[!INCLUDE [Add knowledge base ID as constant](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-kb-id.md)] 

## <a name="add-supporting-functions-and-structures"></a>Hinzuzufügen von unterstützenden Funktionen und Strukturen

Fügen Sie innerhalb der Program-Klasse den folgenden Codeblock hinzu:

```csharp
static string PrettyPrint(string s)
{
    return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
}
```

## <a name="add-post-request-to-publish-kb"></a>Hinzufügen einer POST-Anforderung zur Veröffentlichung der Wissensdatenbank

Der folgende Code sendet eine HTTPS-Anforderung an die QnA Maker-API, um eine KB zu veröffentlichen, und empfängt die Antwort:

```csharp
async static void PublishKB()
{
    string responseText;

    var uri = host + service + method + kb;
    Console.WriteLine("Calling " + uri + ".");
    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = HttpMethod.Post;
        request.RequestUri = new Uri(uri);
        request.Headers.Add("Ocp-Apim-Subscription-Key", key);

        var response = await client.SendAsync(request);

        // successful status doesn't return an JSON so create one
        if (response.IsSuccessStatusCode)
        {
            responseText = "{'result' : 'Success.'}";
        }
        else
        {
            responseText =  await response.Content.ReadAsStringAsync();
        }
    }
    Console.WriteLine(PrettyPrint(responseText));
    Console.WriteLine("Press any key to continue.");
}
```

Der API-Aufruf gibt den Status 204 für eine erfolgreiche Veröffentlichung ohne Inhalt im Text der Antwort zurück. Mit dem Code wird Inhalt für Antworten vom Typ 204 hinzugefügt.

Für alle anderen Antworten wird diese Antwort unverändert zurückgegeben.
 
## <a name="add-the-publishkb-method-to-main"></a>Hinzufügen der PublishKB-Methode zu „Main“

Ändern Sie die Main-Methode, um die CreateKB-Methode aufzurufen:

```csharp
static void Main(string[] args)
{

    // Call the PublishKB() method to publish a knowledge base.
    PublishKB();

    // The console waits for a key to be pressed before closing.
    Console.ReadLine();
}
```

## <a name="build-and-run-the-program"></a>Erstellen und Ausführen des Programms

Erstellen Sie das Programm, und führen Sie es aus. Die Anforderung wird automatisch an die QnA Maker-API gesendet, um die KB zu veröffentlichen. Anschließend wird die Antwort im Konsolenfenster angezeigt.

Nachdem Ihre Wissensdatenbank veröffentlicht wurde, können Sie sie vom Endpunkt mit einer Clientanwendung oder einem Chatbot abfragen. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
