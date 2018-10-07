---
title: 'LUIS und QnAMaker: Botintegration'
titleSuffix: Azure Cognitive Services
description: Ein Schritt-für-Schritt-Tutorial zur Integration von QnA Maker und LUIS in einem Bot.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 53e46fa84bcd7b96403dcb0ec70b45b800bc4acb
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042005"
---
# <a name="integrate-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Integrieren Sie QnA Maker und LUIS, um Ihre Wissensdatenbank zu verteilen
Wenn Ihre QnA Maker-Wissensdatenbank eine bestimmte Größe erreicht, wird es schwierig, sie als einzelnen monolithischen Satz zu warten, und es zeigt sich die Notwendigkeit, die Wissensdatenbank in kleinere logische Abschnitte aufzuteilen.

Zwar ist es ein geradliniger Vorgang, mehrere Wissensdatenbanken in QnA Maker zu erstellen, Sie benötigen jedoch geeignete Logik, um die eingehende Frage an die passende Wissensdatenbank weiterzuleiten. Das können Sie mithilfe von LUIS erledigen.

## <a name="architecture"></a>Architecture

![LUIS-Architektur von QnA Maker](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

Im Szenario oben ruft QnA Maker zuerst die Absicht der eingehenden Frage bei einem LUIS-Modell ab und verwendet diese anschließend, um die Frage an die richtige QnA Maker-Wissensdatenbank weiterzuleiten.

## <a name="prerequisites"></a>Voraussetzungen
- Melden Sie sich beim [LUIS](https://www.luis.ai/)-Portal an, und [erstellen Sie eine App](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app).
- [Fügen Sie Absichten](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) nach Maßgabe Ihres Szenarios hinzu.
- [Trainieren](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) und [veröffentlichen](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp) Sie Ihre LUIS-App.
- Melden Sie sich bei [QnA Maker](https://qnamaker.ai) an, und [erstellen Sie Wissensdatenbanken](https://www.qnamaker.ai/Create) nach Maßgabe Ihres Szenarios.
- Testen und veröffentlichen Sie die Wissensdatenbanken.

## <a name="qna-maker--luis-bot"></a>QnA Maker + LUIS-Bot
1. Erstellen Sie zuerst einen Web-App-Bot mit der LUIS-Vorlage, verknüpfen Sie ihn mit der LUIS-App, die Sie oben erstellt haben, und bearbeiten Sie die Absichten. Detaillierte Schritte finden Sie [hier](https://docs.microsoft.com/azure/cognitive-services/luis/luis-csharp-tutorial-build-bot-framework-sample).

2. Fügen Sie am Anfang der Datei bei den Abhängigkeiten weitere Abhängigkeiten hinzu:

    ```
    using RestSharp;
    using System.Collections.Generic;
    using Newtonsoft.Json;
    ```
3. Fügen Sie die Klasse unten zum Aufrufen Ihres QnA Maker-Diensts hinzu:

    ```
        /// <summary>
        /// QnAMakerService is a wrapper over the QnA Maker REST endpoint
        /// </summary>
        [Serializable]
        public class QnAMakerService
        {
            private string qnaServiceHostName;
            private string knowledgeBaseId;
            private string endpointKey;
    
            /// <summary>
            /// Initialize a particular endpoint with it's details
            /// </summary>
            /// <param name="hostName">Hostname of the endpoint</param>
            /// <param name="kbId">Knowledge base ID</param>
            /// <param name="ek">Endpoint Key</param>
            public QnAMakerService(string hostName, string kbId, string ek)
            {
                qnaServiceHostName = hostName;
                knowledgeBaseId = kbId;
                endpointKey = ek;
            }
    
            /// <summary>
            /// Call the QnA Maker endpoint and get a response
            /// </summary>
            /// <param name="query">User question</param>
            /// <returns></returns>
            public string GetAnswer(string query)
            {
                var client = new RestClient( qnaServiceHostName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer");
                var request = new RestRequest(Method.POST);
                request.AddHeader("authorization", "EndpointKey " + endpointKey);
                request.AddHeader("content-type", "application/json");
                request.AddParameter("application/json", "{\"question\": \"" + query + "\"}", ParameterType.RequestBody);
                IRestResponse response = client.Execute(request);
    
                // Deserialize the response JSON
                QnAAnswer answer = JsonConvert.DeserializeObject<QnAAnswer>(response.Content);
    
                // Return the answer if present
                if (answer.answers.Count > 0)
                    return answer.answers[0].answer;
                else
                    return "No good match found.";
            }
        }
    
        /* START - QnA Maker Response Class */
        public class Metadata
        {
            public string name { get; set; }
            public string value { get; set; }
        }
    
        public class Answer
        {
            public IList<string> questions { get; set; }
            public string answer { get; set; }
            public double score { get; set; }
            public int id { get; set; }
            public string source { get; set; }
            public IList<object> keywords { get; set; }
            public IList<Metadata> metadata { get; set; }
        }
    
        public class QnAAnswer
        {
            public IList<Answer> answers { get; set; }
        }
        /* END - QnA Maker Response Class */
    ```

3. Gehen Sie mit https://qnamaker.ai > „My knowledge bases“ > „View“ zum Code Ihrer entsprechenden Wissensdatenbank. Rufen Sie die folgenden Informationen ab:

    ![QnA Maker-HTTP-Anforderung](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

4. Instanziieren Sie die QnAMakerService-Klasse für jede Ihrer Wissensdatenbanken:
    ```
            // Instantiate the knowledge bases
            public QnAMakerService hrQnAService = new QnAMakerService("https://hrkb.azurewebsites.net", "<HR knowledge base id>", "<HR endpoint key>");
            public QnAMakerService payrollQnAService = new QnAMakerService("https://payrollkb.azurewebsites.net", "<Payroll knowledge base id>", "<Payroll endpoint key>");
            public QnAMakerService financeQnAService = new QnAMakerService("https://financekb.azurewebsites.net", "<Finance knowledge base id>", "<Finance endpoint key>");
    ```

5. Rufen Sie für die Absicht die entsprechende Wissensdatenbank auf.
    ```
            // HR Intent
            [LuisIntent("HR")]
            public async Task CancelIntent(IDialogContext context, LuisResult result)
            {
                // Ask the HR knowledge base
                await context.PostAsync(hrQnAService.GetAnswer(result.Query));
            }
    
            // Payroll intent
            [LuisIntent("Payroll")]
            public async Task GreetingIntent(IDialogContext context, LuisResult result)
            {
                // Ask the payroll knowledge base
                await context.PostAsync(payrollQnAService.GetAnswer(result.Query));
            }
    
            // Finance intent
            [LuisIntent("Finance")]
            public async Task HelpIntent(IDialogContext context, LuisResult result)
            {
                // Ask the finance knowledge base
                await context.PostAsync(financeQnAService.GetAnswer(result.Query));
            }
    ```

## <a name="build-the-bot"></a>Erstellen des Bots
1. Klicken Sie im Code-Editor mit der rechten Maustaste auf `build.cmd`, und wählen Sie **Run from Console** (In Konsole ausführen) aus.

    ![run from console (In Konsole ausführen)](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. Die Codeansicht wird durch ein Terminalfenster ersetzt, in dem der Fortschritt und die Ergebnisse des Builds angezeigt werden.

    ![console build (Build in der Konsole)](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Testen des Bots
Wählen Sie im Azure-Portal **Test in Web Chat** (Im Webchat testen) aus, um den Bot zu testen. Geben Sie Nachrichten aus verschiedenen Absichten ein, um die Antwort von der entsprechenden Wissensdatenbank zu erhalten.

![Webchattest](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines Geschäftskontinuitätsplans für QnA Maker](../How-To/business-continuity-plan.md)
