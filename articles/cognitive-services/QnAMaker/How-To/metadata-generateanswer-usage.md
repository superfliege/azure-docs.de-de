---
title: 'Verwenden von Metadaten mit der GenerateAnswer-API: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker ermöglicht Ihnen das Hinzufügen von Metadaten in Form von Schlüssel-Wert-Paaren zu Ihren Frage-Antwort-Sätzen. Mit diesen Informationen können Sie Ergebnisse nach Benutzerabfragen filtern und zusätzliche Informationen speichern, die in Folgekonversationen verwendet werden können.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/18/2018
ms.author: tulasim88
ms.openlocfilehash: 8712f9e79965e09e21df768b1c06561a896b8e01
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214091"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Verwenden von Metadaten und der GenerateAnswer-API

QnA Maker ermöglicht das Hinzufügen von Metadaten in Form von Schlüssel-Wert-Paaren zu Ihren Frage-Antwort-Sätzen. Mit diesen Informationen können Sie Ergebnisse nach Benutzerabfragen filtern und zusätzliche Informationen speichern, die in Folgekonversationen verwendet werden können. Weitere Informationen finden Sie unter [Knowledge Base](../Concepts/knowledge-base.md).

## <a name="qna-entity"></a>QnA-Entität

Zunächst ist es wichtig zu verstehen, wie QnA Maker die Frage-Antwort-Daten speichert. Die folgende Abbildung zeigt eine QnA-Entität:

![QnA-Entität](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Jede QnA-Entität hat eine eindeutige und dauerhafte ID. Die ID kann verwendet werden, um bestimmte QnA-Entitäten zu aktualisieren.

## <a name="generateanswer-api"></a>GenerateAnswer-API

Sie verwenden die GenerateAnswer-API in Ihrem Bot oder Ihrer Anwendung, um die Knowledge Base zu einer Benutzerfrage abzufragen und die beste Übereinstimmung aus den Frage-Antwort-Sätzen zu erhalten.

### <a name="generateanswer-endpoint"></a>GenerateAnswer-Endpunkt

Nachdem Sie Ihre Knowledge Base im [QnA Maker-Portal](https://www.qnamaker.ai) oder mithilfe der [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) veröffentlicht haben, können Sie die Details von Ihrem GenerateAnswer-Endpunkt abrufen.

So rufen Sie Endpunktdetails ab
1. Melden Sie sich unter [https://www.qnamaker.ai](https://www.qnamaker.ai) an.
2. Klicken Sie in **My knowledge bases** (Meine Knowledge Bases) für Ihre Knowledge Base auf **Code anzeigen**.
![My Knowledge Bases (Meine Knowledge Bases)](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
3. Rufen Sie Ihre GenerateAnswer-Endpunktdetails ab.

![Endpunktdetails](../media/qnamaker-how-to-metadata-usage/view-code.png)

Sie können Ihre Endpunktdetails auch auf der Registerkarte **Einstellungen** Ihrer Knowledge Base abrufen.

### <a name="generateanswer-request"></a>GenerateAnswer-Anforderung

Sie rufen GenerateAnswer über eine HTTP POST-Anforderung auf. Beispielcode, der zeigt, wie GenerateAnswer aufgerufen wird, finden Sie unter [Schnellstarts](../quickstarts/csharp.md).

- **Anforderungs-URL:** https://{QnA Maker-Endpunkt}/knowledgebases/{Knowledge Base-ID}/generateAnswer

- **Anforderungsparameter:** 
    - **Knowledge base ID** (Zeichenfolge): GUID der Knowledge Base
    - **QnAMaker endpoint** (Zeichenfolge): Hostname des Endpunkts, der in Ihrem Azure-Abonnement bereitgestellt wurde
- **Anforderungsheader**
    - **Content-Type** (Zeichenfolge): Medientyp des an die API gesendeten Texts
    - **Authorization** (Zeichenfolge): Ihr Endpunktschlüssel (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).
- **Anforderungstext**
    - **question** (Zeichenfolge): Benutzerfrage, nach der die Knowledge Base abgefragt werden soll
    - **top** (optional, Integer): Anzahl der priorisierten Ergebnisse für die Ausgabe Der Standardwert ist 1.
    - **userId** (optional, Zeichenfolge): Eindeutige ID zur Identifizierung des Benutzers. Diese ID wird in den Chatprotokollen aufgezeichnet.
    - **strictFilters** (optional, Zeichenfolge): Weist (sofern angegeben) QnA Maker an, nur Antworten mit den angegebenen Metadaten zurückzugeben. Weitere Informationen finden Sie unten.
    ```json
    {
        "question": "qna maker and luis",
        "top": 6,
        "strictFilters": [
        {
            "name": "category",
            "value": "api"
        }],
        "userId": "sd53lsY="
    }
    ```

### <a name="generateanswer-response"></a>GenerateAnswer-Antwort

- **Antwort 200:** Ein erfolgreicher Aufruf gibt das Ergebnis der Frage zurück. Die Antwort enthält die folgenden Felder:
    - **answers:** Liste von Antworten auf die Benutzerfrage, sortiert in absteigender Rangfolge
        - **score:** Rangfolgewert zwischen 0 und 100
        - **questions:** Fragen des Benutzers
        - **answer:** Die Antwort auf die Frage
        - **source:** Name der Quelle, aus der die Antwort extrahiert oder in der Wissensdatenbank gespeichert wurde
        - **metadata:** Die der Antwort zugeordneten Metadaten
            - name: Metadatenname (Zeichenfolge, maximale Länge: 100, erforderlich)
            - value: Metadatenwert (Zeichenfolge, maximale Länge: 100, erforderlich)
        - **Id:** Die der Antwort zugewiesene eindeutige ID
    ```json
    {
        "answers": [
            {
                "score": 28.54820341616869,
                "Id": 20,
                "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
                "source": "Custom Editorial",
                "questions": [
                    "How can I integrate LUIS with QnA Maker?"
                ],
                "metadata": [
                    {
                        "name": "category",
                        "value": "api"
                    }
                ]
            }
        ]
    }
    ```

## <a name="metadata-example"></a>Metadatenbeispiel

Sehen Sie sich die folgenden FAQ-Daten an. Fügen Sie Ihrer Wissensdatenbank Metadaten hinzu, indem Sie auf das Metadatensymbol klicken.

![Hinzufügen von Metadaten](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Filtern von Ergebnissen mit „strictFilters“ nach Metadatentags

Betrachten Sie die Benutzerfrage „When does this hotel close?“ (Wann schließt dieses Hotel?) mit dem Restaurant „Paradise“ als implizierter Absicht.

Da nur Ergebnisse für das Restaurant „Paradise“ erforderlich sind, können Sie im GenerateAnswer-Aufruf wie folgt einen Filter für die Metadaten zum Restaurantnamen festlegen.

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

### <a name="keep-context"></a>Beibehalten des Kontexts
Die Antwort auf den GenerateAnswer-Aufruf enthält die entsprechenden Metadateninformationen des übereinstimmenden Frage-Antwort-Satzes.

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

Diese Informationen können verwendet werden, um den Kontext der vorherigen Konversation für die Verwendung in späteren Konversationen aufzuzeichnen. 

## <a name="next-steps"></a>Nächste Schritte

Die Veröffentlichungsseite enthält auch Informationen zum Generieren einer Antwort mit [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) und [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Erstellen einer Wissensdatenbank](./create-knowledge-base.md)
