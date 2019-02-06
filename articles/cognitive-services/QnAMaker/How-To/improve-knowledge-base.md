---
title: Verbessern der Wissensdatenbank – QnA Maker
titleSuffix: Azure Cognitive Services
description: ''
author: diberry
manager: cgronlun
displayName: active learning, suggestion, dialog prompt, train api, feedback loop, autolearn, auto-learn, user setting, service setting, services setting
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/29/2019
ms.author: diberry
ms.openlocfilehash: 7f519729f3ad94324b847ca6b15b254ea7c6abbb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463734"
---
# <a name="use-active-learning-to-improve-knowledge-base"></a>Nutzen des aktiven Lernens zum Verbessern der Wissensdatenbank

Durch aktives Lernen lässt sich die Qualität Ihrer Wissensdatenbank verbessern, indem auf der Grundlage von Benutzerbeiträgen alternative Fragen zu Ihrem Frage-Antwort-Paar vorgeschlagen werden. Sie überprüfen diese Vorschläge, indem Sie sie entweder zu vorhandenen Fragen hinzufügen oder ablehnen. 

Ihre Wissensdatenbank wird nicht automatisch geändert. Sie müssen die Vorschläge akzeptieren, damit eine Änderung wirksam wird. Mit diesen Vorschlägen werden Fragen hinzugefügt, vorhandene Fragen werden aber weder geändert noch entfernt.

## <a name="active-learning"></a>Aktives Lernen

QnA Maker erlernt neue Fragevarianten über implizites und explizites Feedback.
 
* Implizites Feedback: Die Bewertung erkennt, wenn für eine Benutzerfrage mehrere Antworten vorliegen, deren Bewertungen sehr nah beieinander liegen, und betrachtet dies als Feedback. 
* Explizites Feedback: Wenn von der Wissensdatenbank mehrere Antworten zurückgegeben werden, deren Bewertungen sich kaum unterscheiden, fordert die Clientanwendung den Benutzer auf, die richtige Frage auszuwählen. Das explizite Feedback des Benutzers wird mit der Trainings-API an QnA Maker gesendet. 

Über beide Methoden werden der Bewertung ähnliche Abfragen bereitgestellt, die gruppiert werden.

Wenn ähnliche Abfragen gruppiert werden, sendet QnA Maker die benutzerbasierten Fragen als Vorschläge an den Entwickler der Wissensdatenbank, um diese zu akzeptieren oder abzulehnen.

## <a name="how-active-learning-works"></a>Aktives Lernen: Funktionsweise

Das Feature für aktives Lernen wird basierend auf den Bewertungen der relevantesten Antworten ausgelöst, die von QnA Maker für eine bestimmte Abfrage zurückgegeben werden. Liegt der Bewertungsunterschied innerhalb einer geringen Bandbreite, dann wird die Abfrage als möglicher _Vorschlag_ für jede der möglichen Antworten betrachtet. 

Alle Vorschläge werden nach Ähnlichkeit gruppiert, und die besten Vorschläge für alternative Fragen werden basierend auf der Häufigkeit der jeweiligen Abfragen von Endbenutzern angezeigt. Das Feature für aktives Lernen liefert die bestmöglichen Vorschläge in Fällen, in denen die Endpunkte eine angemessene Anzahl und Vielfalt von Nutzungsabfragen erhalten.

## <a name="upgrade-version-to-use-active-learning"></a>Versionsupgrade für aktives Lernen

Das Feature für aktives Lernen wird in der Runtimeversion 4.4.0 und höher unterstützt. Wenn Ihre Wissensdatenbank mit einer Vorgängerversion erstellt wurde, [führen Sie ein Upgrade Ihrer Runtime durch](troubleshooting-runtime.md#how-to-get-latest-qnamaker-runtime-updates), um dieses Feature zu nutzen. 

## <a name="best-practices"></a>Bewährte Methoden

Bewährte Methoden bei der Verwendung des Features für aktives Lernen finden Sie unter [Best Practices](../Concepts/best-practices.md#active-learning).

## <a name="score-proximity-between-knowledge-base-questions"></a>Nähe der Bewertungen bei Fragen der Wissensdatenbank

Wenn die Bewertung einer Frage eine hohe Zuverlässigkeit aufweist, wie z.B. 80%, ist die Bandbreite der für das aktive Lernen in Betracht gezogenen Bewertungen mit etwa 10% recht groß. Wenn der Zuverlässigkeitswert einer Bewertung sinkt, z.B. auf 40%, nimmt auch die Bandbreite der Bewertungen ab und liegt nur etwa innerhalb von 4%. 

Der Algorithmus zur Bestimmung der Nähe ist keine einfache Berechnung. Die Bereiche in den vorstehenden Beispielen sind keine festen Werte, sondern dienen als Richtlinie zum Verständnis der Auswirkungen des Algorithmus.

## <a name="turn-on-active-learning"></a>Aktivieren des Features für aktives Lernen

Das Feature für aktives Lernen ist standardmäßig deaktiviert. Aktivieren Sie das Feature, um die vorgeschlagenen Fragen anzuzeigen. 

1. Um das Feature für aktives Lernen zu aktivieren, wechseln Sie in der oberen rechten Ecke des QnA Maker-Portals zu den **Diensteinstellungen**.  

    ![Aktivieren des Features für aktives Lernen auf der Seite mit den Diensteinstellungen](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Suchen Sie nach dem QnA Maker-Dienst, und aktivieren Sie die Option **Active Learning**. 

    [![Aktivieren des Features für aktives Lernen auf der Seite mit den Diensteinstellungen](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    Nachdem Sie **Active Learning** aktiviert haben, schlägt die Wissensdatenbank basierend auf den von Benutzern übermittelten Fragen in regelmäßigen Abständen neue Fragen vor. Sie können **Active Learning** deaktivieren, indem Sie die Einstellung wieder umschalten.

## <a name="add-active-learning-suggestion-to-knowledge-base"></a>Hinzufügen von Vorschlägen zur Wissensdatenbank über das Feature für aktives Lernen

1. Um die vorgeschlagenen Fragen anzuzeigen, wählen Sie auf der Seite **Bearbeiten** der Wissensdatenbank die Option **Vorschläge anzeigen** aus. 

    [![Umschalten der Option „Vorschläge anzeigen“ auf der Seite mit den Diensteinstellungen](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Filtern Sie die Wissensdatenbank nach Frage-Antwort-Paaren, um nur Vorschläge anzuzeigen, indem Sie **Nach Vorschlägen filtern** auswählen.

    [![Filtern nach Vorschlägen auf der Seite mit den Diensteinstellungen, um nur diese Frage-Antwort-Paare anzuzeigen](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1.  Jeder Fragenabschnitt mit Vorschlägen zeigt die neuen Fragen mit einem Häkchen (`✔`) an, um die Frage zu akzeptieren, oder mit einem Kreuz (`x`), um die Vorschläge abzulehnen. Aktivieren Sie das Häkchen, um die Frage hinzuzufügen. 

    [![Umschalten des Features für aktives Lernen auf der Seite mit den Diensteinstellungen](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    Sie können _alle Vorschläge_ hinzufügen oder löschen, indem Sie **Alle hinzufügen** oder **Alle ablehnen** auswählen.

1. Wählen Sie **Speichern und trainieren** aus, um die Änderungen an der Wissensdatenbank zu speichern.


## <a name="determine-best-choice-when-several-questions-have-similar-scores"></a>Bestimmen der besten Auswahl bei mehreren Fragen mit ähnlichen Bewertungen

Wenn eine Frage bezüglich ihrer Bewertung zu nahe an anderen Fragen liegt, kann der Entwickler der Clientanwendung um Klärung bitten.

### <a name="use-the-top-property-in-the-generateanswer-request"></a>Verwenden der top-Eigenschaft in der GenerateAnswer-Anforderung

Wenn Sie eine Frage zur Beantwortung an QnA Maker übermitteln, ermöglicht es der JSON-Text, mehr als eine Top-Antwort zurückzugeben:

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

Wenn die Clientanwendung (in diesem Fall der Chatbot) die Antwort empfängt, werden die drei relevantesten Fragen zurückgegeben:

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

### <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Nachverfolgung durch die Clientanwendung bei Fragen mit ähnlicher Bewertung

Die Clientanwendung zeigt alle Fragen an und gibt dem Benutzer die Möglichkeit, die am besten geeignete Frage auszuwählen. 

Der Benutzer wählt eine der vorhandenen Fragen aus. Das Benutzerfeedback wird an die [Trainings](http://www.aka.ms/activelearningsamplebot)-API von QnA Maker gesendet, um den Zyklus für das aktive Lernen fortzusetzen. 

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

Weitere Informationen zur Verwendung des Features für aktives Lernen mit einem [Azure Bot-Beispiel für C#](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-activelearning-bot)

## <a name="next-steps"></a>Nächste Schritte
 
> [!div class="nextstepaction"]
> [Verwenden der QnA Maker-API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
