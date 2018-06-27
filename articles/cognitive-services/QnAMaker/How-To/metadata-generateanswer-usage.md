---
title: Verwenden von Metadaten in der Knowledge Base mit der GenerateAnswer-API | Microsoft-Dokumentation
description: Verwenden von Metadaten mit der GenerateAnswer-API
services: cognitive-services
author: pchoudhari
manager: rsrikan
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/18/2018
ms.author: pchoudh
ms.openlocfilehash: 94e3632884d7033971ff1c45b455afb9a09ee798
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "35378777"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Verwenden von Metadaten und der GenerateAnswer-API

QnA Maker ermöglicht Ihnen das Hinzufügen von Metadaten in Form von Schlüssel-Wert-Paaren zu Ihren Frage-Antwort-Sätzen. Diese Informationen können auf verschiedene Weise (z.B. zum Filtern der Ergebnisse von Benutzerfragen, zum Verstärken bestimmter Ergebnisse und zum Speichern zusätzlicher Informationen) verwendet werden, die dann bei nachfolgenden Konversationen verwendet werden können. Weitere Informationen finden Sie unter [Knowledge Base](../Concepts/knowledge-base.md).

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
    - **Authorization** (Zeichenfolge): Ihr Endpunktschlüssel
- **Anforderungstext**
    - **question** (Zeichenfolge): Benutzerfrage, nach der die Knowledge Base abgefragt werden soll
    - **top** (optional, Integer): Anzahl der priorisierten Ergebnisse für die Ausgabe Der Standardwert ist 1.
    - **userId** (optional, Zeichenfolge): eindeutige ID zur Identifizierung des Benutzers. Diese ID wird in den Chatprotokollen aufgezeichnet.
    - **strictFilters** (optional, Zeichenfolge): weist (sofern angegeben) QnA Maker an, nur Antworten mit den angegebenen Metadaten zurückzugeben. Weitere Informationen finden Sie unten.
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
        - **source:** Name der Quelle, aus der die Antwort extrahiert oder in der Knowledge Base gespeichert wurde
        - **metadata:** die der Antwort zugeordneten Metadaten
            - name: Metadatenname (Zeichenfolge, maximale Länge: 100, erforderlich)
            - value: Metadatenwert (Zeichenfolge, maximale Länge: 100, erforderlich)
        - **Id:** die der Antwort zugewiesene eindeutige ID
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

Sehen Sie sich die unten gezeigten Daten mit häufig gestellten Fragen für Restaurants in Hyderabad an. Sie fügen Ihrer Knowledge Base Metadaten hinzu, indem Sie auf das Zahnradsymbol klicken.

![Hinzufügen von Metadaten](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters"></a>Filtern von Ergebnissen mit strictFilters

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

> [!div class="nextstepaction"]
> [Erstellen einer Wissensdatenbank](./create-knowledge-base.md)
