---
title: 'Beispiel: Erstellen eines benutzerdefinierten Skills in einer Pipeline der kognitiven Suche (Azure Search) | Microsoft-Dokumentation'
description: Zeigt die Verwendung der Textübersetzungs-API in benutzerdefinierten Skills, die einer Indizierungspipeline der kognitiven Suche in Azure Search zugeordnet sind.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a295bf741862bb58a86234b5c85f48d7a1b52be7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33786859"
---
# <a name="example-create-a-custom-skill-using-the-text-translate-api"></a>Beispiel: Erstellen eines benutzerdefinierten Skills mithilfe der Textübersetzungs-API

Erfahren Sie in diesem Beispiel, wie ein benutzerdefinierter Web-API-Skill erstellt wird, der Text in einer beliebigen Sprache akzeptiert und ins Englische übersetzt. Das Beispiel verwendet eine [Azure-Funktion](https://azure.microsoft.com/services/functions/), um die [Textübersetzungs-API](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) so einzubinden, dass sie die Schnittstelle des benutzerdefinierten Skills implementiert.

## <a name="prerequisites"></a>Voraussetzungen

+ Lesen Sie den Artikel über die [Schnittstelle des benutzerdefinierten Skills](cognitive-search-custom-skill-interface.md), wenn Sie nicht mit der Eingabe/Ausgabe-Schnittstelle vertraut sind, die einen benutzerdefinierten Skill implementieren sollte.

+ [Registrieren Sie sich für die Textübersetzungs-API](../cognitive-services/translator/translator-text-how-to-signup.md), und erhalten Sie hierfür einen API-Schlüssel.

+ Installieren Sie [Visual Studio 2017 Version 15.5 oder höher](https://www.visualstudio.com/vs/) mit der Workload für die Azure-Entwicklung.

## <a name="create-an-azure-function"></a>Erstellen einer Azure Function

Obwohl in diesem Beispiel eine Azure-Funktion zum Hosten einer Web-API verwendet wird, ist dies nicht erforderlich.  Solange Sie die [Schnittstellenanforderungen für einen kognitiven Skill](cognitive-search-custom-skill-interface.md) erfüllen, ist der gewählte Ansatz unerheblich. Mit Azure Functions ist das Erstellen eines benutzerdefinierten Skills jedoch einfacher.

### <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

1. Wählen Sie in Visual Studio im Menü „Datei“ die Optionen **Neu** > **Projekt** aus.

1. Wählen Sie im Dialogfeld „Neues Projekt“ die Option **Installiert**, erweitern Sie **Visual C#** > **Cloud**, und wählen Sie **Azure Functions** aus. Geben Sie unter „Name“ einen Namen für Ihr Projekt ein, und wählen Sie **OK**. Der Name der Funktions-App muss als C#-Namespace gültig sein, verwenden Sie daher keine Unterstriche, Bindestriche oder andere nicht alphanumerische Zeichen.

1. Wählen Sie für den Typ **HTTP-Trigger** aus.

1. Für das Speicherkonto können Sie **Kein** auswählen, da Sie für diese Funktion keinen Speicher benötigen.

1. Wählen Sie **OK**, um das Funktionsprojekt und die per HTTP ausgelöste Funktion zu erstellen.

### <a name="modify-the-code-to-call-the-translate-cognitive-service"></a>Ändern des Codes zum Aufrufen des kognitiven Übersetzungsdiensts

Visual Studio erstellt ein Projekt, das eine Klasse mit den Codebausteinen für den gewählten Funktionstyp enthält. Mit dem *FunctionName*-Attribut der Methode wird der Name der Funktion festgelegt. Mit dem *HttpTrigger*-Attribut wird angegeben, dass die Funktion mit einer HTTP-Anforderung ausgelöst wird.

Ersetzen Sie alle Inhalte der Datei *Function1.cs* nun durch den folgenden Code:

```csharp
using System.IO;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using System.Net.Http;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Text;

namespace TranslateFunction
{
    // This function will simply translate messages sent to it.
    public static class Function1
    {
        #region classes used to serialize the response
        private class WebApiResponseError
        {
            public string message { get; set; }
        }

        private class WebApiResponseWarning
        {
            public string message { get; set; }
        }

        private class WebApiResponseRecord
        {
            public string recordId { get; set; }
            public Dictionary<string, object> data { get; set; }
            public List<WebApiResponseError> errors { get; set; }
            public List<WebApiResponseWarning> warnings { get; set; }
        }

        private class WebApiEnricherResponse
        {
            public List<WebApiResponseRecord> values { get; set; }
        }
        #endregion


        /// <summary>
        /// Note that this function can translate up to 1000 characters. If you expect to need to translate more characters, use 
        /// the paginator skill before calling this custom enricher
        /// </summary>
        [FunctionName("Translate")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)]HttpRequest req, 
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            string recordId = null;
            string originalText = null;
            string originalLanguage = null;
            string translatedText = null;

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            dynamic data = JsonConvert.DeserializeObject(requestBody);

            // Validation
            if (data?.values == null)
            {
                return new BadRequestObjectResult(" Could not find values array");
            }
            if (data?.values.HasValues == false || data?.values.First.HasValues == false)
            {
                // It could not find a record, then return empty values array.
                return new BadRequestObjectResult(" Could not find valid records in values array");
            }

            recordId = data?.values?.First?.recordId?.Value as string;
            originalText = data?.values?.First?.data?.text?.Value as string;
            originalLanguage = data?.values?.First?.data?.language?.Value as string;

            if (recordId == null)
            {
                return new BadRequestObjectResult("recordId cannot be null");
            }

            // Only translate records that actually need to be translated. 
            if (!originalLanguage.Contains("en"))
            {
                translatedText = TranslateText(originalText, "en-us").Result;
            }
            else
            {
                // text is already in English.
                translatedText = originalText;
            }

            // Put together response.
            WebApiResponseRecord responseRecord = new WebApiResponseRecord();
            responseRecord.data = new Dictionary<string, object>();
            responseRecord.recordId = recordId;
            responseRecord.data.Add("text", translatedText);

            WebApiEnricherResponse response = new WebApiEnricherResponse();
            response.values = new List<WebApiResponseRecord>();
            response.values.Add(responseRecord);

            return (ActionResult)new OkObjectResult(response); 
        }

        /// <summary>
        /// Use Cognitive Service to translate text from one language to antoher.
        /// </summary>
        /// <param name="myText">The text to translate</param>
        /// <param name="destinationLanguage">The language you want to translate to.</param>
        /// <returns>Asynchronous task that returns the translated text. </returns>
        async static Task<string> TranslateText(string myText, string destinationLanguage)
        {
            string host = "https://api.microsofttranslator.com";
            string path = "/V2/Http.svc/Translate";

            // NOTE: Replace this example key with a valid subscription key.
            string key = "064d8095730d4a99b49f4bcf16ac67f8";

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            List<KeyValuePair<string, string>> list = new List<KeyValuePair<string, string>>() {
                new KeyValuePair<string, string>(myText, "en-us")
            };

            StringBuilder totalResult = new StringBuilder();

            foreach (KeyValuePair<string, string> i in list)
            {
                string uri = host + path + "?to=" + i.Value + "&text=" + System.Net.WebUtility.UrlEncode(i.Key);

                HttpResponseMessage response = await client.GetAsync(uri);

                string result = await response.Content.ReadAsStringAsync();

                // Parse the response XML
                System.Xml.XmlDocument xmlResponse = new System.Xml.XmlDocument();
                xmlResponse.LoadXml(result);
                totalResult.Append(xmlResponse.InnerText); 
            }

            return totalResult.ToString();
        }
    }
}
```

Stellen Sie sicher, dass Sie Ihren eigenen *key*-Wert in die *TranslateText*-Methode eingeben, der auf dem Schlüssel basiert, den Sie bei der Anmeldung für die Textübersetzungs-API erhalten haben.

Dieses Beispiel ist eine einfache Anreicherungsfunktion, die immer nur an einem Datensatz arbeitet. Diese Tatsache wird später wichtig, wenn die Batchgröße für den Skillset festgelegt wird.

## <a name="test-the-function-from-visual-studio"></a>Testen der Funktion aus Visual Studio

Drücken Sie **F5**, um das Programm- und Testfunktionsverhalten auszuführen. Verwenden Sie Postman oder Fiddler, um einen Aufruf wie den unten gezeigten auszugeben:

```http
POST https://localhost:7071/api/Translate
```
### <a name="request-body"></a>Anforderungstext
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "es"
            }
        }
   ]
}
```
### <a name="response"></a>response
Die Antwort sollte in etwa wie hier dargestellt aussehen:

```json
{
    "values": [
        {
            "recordId": "a1",
            "data": {
                "text": "This is a contract in English"
            },
            "errors": null,
            "warnings": null
        }
    ]
}
```

## <a name="publish-the-function-to-azure"></a>Veröffentlichen der Funktion in Azure

Wenn Sie mit dem Funktionsverhalten zufrieden sind, können Sie sie veröffentlichen.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus. Wählen Sie **Neu erstellen** > **Veröffentlichen**.

1. Wenn Sie Visual Studio noch nicht mit Ihrem Azure-Konto verbunden haben, wählen Sie **Konto hinzufügen...** aus.

1. Befolgen Sie die Anweisungen auf dem Bildschirm. Sie werden aufgefordert, das Azure-Konto, die Ressourcengruppe, den Hostingplan und das gewünschte Speicherkonto anzugeben. Sie können eine neue Ressourcengruppe, einen neuen Hostingplan und ein Speicherkonto erstellen, wenn Sie noch nicht über diese verfügen. Wenn Sie fertig sind, klicken Sie auf **Erstellen**.

1. Notieren Sie nach Abschluss der Bereitstellung die Website-URL. Es ist die Adresse der Funktions-App in Azure. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Ressourcengruppe, und suchen Sie nach der von Ihnen veröffentlichten Übersetzungsfunktion. Im Abschnitt **Verwalten** sollten Sie Hostschlüssel angezeigt werden. Wählen Sie das Symbol **Kopieren** für den Hostschlüssel *Standard* aus.  


## <a name="test-the-function-in-azure"></a>Testen der Funktion in Azure

Nachdem Sie nun den Standardhostschlüssel haben, testen Sie Ihre Funktion wie folgt:

```http
POST https://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]
```
### <a name="request-body"></a>Anforderungstext
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "es"
            }
        }
   ]
}
```

Dieser Code sollte ein ähnliches Ergebnis liefern wie der, den Sie zuvor bei der Ausführung der Funktion in der lokalen Umgebung gesehen haben.

## <a name="connect-to-your-pipeline"></a>Herstellen einer Verbindung mit Ihrer Pipeline
Nun, da Sie über einen neuen benutzerdefinierten Skill verfügen, können Sie diesen zu Ihrem Skillset hinzufügen. Im folgenden Beispiel wird gezeigt, wie Sie den Skill aufrufen. Da der Skill keine Batches verarbeitet, fügen Sie eine Anweisung hinzu, dass die maximale Batchgröße nur ```1``` sein darf, um Dokumente einzeln zu versenden.

```json
{
    "skills": [
      ...,  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new translator custom skill",
        "uri": "http://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]",
        "batchSize":1,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/languageCode"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "translatedText"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte
Glückwunsch! Sie haben Ihre erste benutzerdefinierte Anreicherungsfunktion erstellt. Nun können Sie Ihre eigene benutzerdefinierte Funktionalität nach dem gleichen Muster hinzufügen. 

+ [Hinzufügen eines benutzerdefinierten Skills zu einer Pipeline der kognitiven Suche](cognitive-search-custom-skill-interface.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Erstellen eines Skillsets (REST)](ref-create-skillset.md)
+ [Zuordnen angereicherter Felder](cognitive-search-output-field-mapping.md)