---
title: Konversationen mit Mehrfachdurchläufen
titleSuffix: Azure Cognitive Services
description: Verwalten Sie für Ihren Bot von einer Frage zur nächsten die verschiedenen Durchläufe (Multi-Turn) mit Eingabeaufforderungen und Kontext. Mehrfachdurchläufe sind die Fähigkeit, eine wechselseitige Konversation zu führen, wobei der Kontext der vorherigen Frage die nächste Frage und die nächste Antwort beeinflusst.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 24158d5949c0a31e5444c609cbfced970bdeca07
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65471969"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Erstellen von Mehrfachdurchläufen einer Konversation mit Folgeaufforderungen

Verwalten Sie für Ihren Bot von einer Frage zur nächsten die Mehrfachdurchläufe (_Multi-Turn_) mit Folgeaufforderungen und Kontext.

Erfahren Sie mehr im [Demovideo](https://aka.ms/multiturnexample).

## <a name="what-is-a-multi-turn-conversation"></a>Was ist eine Konversation mit Mehrfachdurchläufen?

Einige Konversationen können nicht in einem einzigen Durchlauf abgeschlossen werden. Wenn Sie Konversationen (Chatbot) in Ihrer Clientanwendung entwerfen, kann ein Benutzer eine Frage stellen, die gefiltert oder verfeinert werden muss, um die richtige Antwort zu bestimmen. Ein derartiger Flow durch die Fragen kann erreicht werden, indem dem Benutzer **Folgeaufforderungen** präsentiert werden.

Wenn der Benutzer die Frage stellt, gibt QnA Maker die Antwort _und_ etwaige Folgeaufforderungen zurück. Dadurch können Folgefragen als Auswahlmöglichkeiten präsentiert werden. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Beispiel für Konversation mit Mehrfachdurchläufen mit Chatbot

Ein Chatbot verwaltet die Konversation Frage für Frage, wobei der Benutzer die abschließende Antwort bestimmt.

![Verwalten Sie im Konversationsablauf den Konversationszustand in einem Mehrfachdurchlauf-Dialogsystem, indem Sie innerhalb der Antworten Eingabeaufforderungen als Optionen präsentieren, mit denen die Konversation fortgesetzt werden kann.](../media/conversational-context/conversation-in-bot.png)

In der vorherigen Abbildung muss die Frage des Benutzers verfeinert werden, bevor die Antwort zurückgegeben werden kann. In der Wissensdatenbank hat die Frage (#1) vier Folgeaufforderungen, die im Chatbot als vier Auswahlmöglichkeiten (#2) angezeigt werden. 

Wenn der Benutzer eine Auswahlmöglichkeit (#3) auswählt, wird die Liste der Verfeinerungsoptionen (#4) angezeigt. Dieser Vorgang kann fortgesetzt werden (#5), bis die richtige und abschließende Antwort (#6) bestimmt wird.

Sie müssen Ihre Clientanwendung ändern, um die kontextbezogene Konversation zu verwalten.

## <a name="create-a-multi-turn-conversation-from-documents-structure"></a>Erstellen einer Mehrfachdurchlauf-Konversation anhand der Dokumentstruktur
Beim Erstellen einer Wissensdatenbank sehen Sie ein optionales Kontrollkästchen, mit dem Sie die Mehrfachdurchlauf-Extrahierung aktivieren können. Bei Aktivierung dieser Option kann die Mehrfachdurchlauf-Konversation beim Importieren eines Dokuments aus der Struktur abgeleitet werden. Ist diese Struktur vorhanden, erstellt QnA Maker automatisch die QnA-Paare mit Folgeaufforderungen. Die Mehrfachdurchlauf-Struktur kann nur aus URLs, PDF- und DOCX-Dateien abgeleitet werden. 

Nutzen Sie die folgende Abbildung einer Microsoft Surface-[PDF-Datei](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf) als Anleitung. 

![![Wenn Sie ein Dokument importieren, kann eine kontextbezogene Konversation aus der Struktur abgeleitet werden. Ist diese Struktur vorhanden, erstellt QnA Maker im Rahmen des Dokumentimports automatisch die QnA-Paare mit Folgeaufforderungen.](../media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

Beim Importieren des PDF-Dokuments bestimmt QnA Maker Folgeaufforderungen anhand der Struktur, um den Konversationsablauf zu erstellen. 

![![Beim Importieren des PDF-Dokuments bestimmt QnA Maker Folgeaufforderungen anhand der Struktur, um den Konversationsablauf zu erstellen. ](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

## <a name="filter-questions-and-answers-by-context"></a>Filtern von Fragen und Antworten gemäß dem Kontext

1. Reduzieren Sie die Anzahl der angezeigten Frage- und Antwortpaare auf diejenigen, die kontextbezogene Konversationen aufweisen. Wählen Sie **Optionen anzeigen** und anschließend **Show context (PREVIEW)** (Kontext anzeigen (VORSCHAU)) aus. Die Liste ist leer, bis Sie das erste Frage- und Antwortpaar mit einer Folgeaufforderung hinzufügen. 

    ![Filtern von Frage- und Antwortpaaren nach kontextbezogenen Konversationen](../media/conversational-context/filter-question-and-answers-by-context.png)

## <a name="add-new-qna-pair-as-follow-up-prompt"></a>Hinzufügen eines neuen Frage- und Antwortpaars als Folgeaufforderung

1. Wählen Sie **Add QnA pair** (Frage- und Antwortpaar hinzufügen) aus. 
1. Geben Sie den neuen Fragentext `Give feedback.` mit der Antwort `What kind of feedback do you have?` ein.

1. Wählen Sie in der Spalte **Antwort** für diese Frage **Add follow-up prompt** (Folgeaufforderung hinzufügen) aus. 
1. Im Popup-Dialogfeld **Follow-up prompt** (Folgeaufforderung) können Sie nach einer vorhandenen Frage suchen oder eine neue Frage eingeben. Geben Sie in diesem Verfahren den Text `Feedback on an QnA Maker service` für **Anzeigetext** ein. 
1. Aktivieren Sie die Option **Context-only** (Nur Kontext). Durch die Option **Context-only** (Nur Kontext) wird angegeben, dass dieser Benutzertext _nur_ verständlich ist, wenn er als Antwort auf eine vorherige Frage ausgegeben wird. In diesem Szenario ist der Aufforderungstext als eigenständige Frage unverständlich; er lässt sich nur im Kontext der vorherigen Frage verstehen.
1. Geben Sie im Textfeld **Link to answer** (Mit Antwort verknüpfen) die Antwort `How would you rate QnA Maker?` ein.
1. Wählen Sie **Neu erstellen** und anschließend **Speichern** aus. 

    ![Erstellen eines neuen Frage- und Antwortpaars mit Aufforderung](../media/conversational-context/create-child-prompt-from-parent.png)

    Dadurch wurde ein neues Frage- und Antwortpaar erstellt, und die ausgewählte Frage wurde als Folgeaufforderung verknüpft. In der Spalte **Kontext** wird für beide Fragen eine Folgeaufforderungsbeziehung angegeben. 

    ![![In der Spalte „Kontext“ wird für beide Fragen eine Folgeaufforderungsbeziehung angegeben.](../media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Wählen Sie **Add follow-up prompt** (Folgeaufforderung hinzufügen) für die Frage `Give feedback` aus, um eine weitere Folgeaufforderung hinzufügen. 
1. Erstellen Sie eine neue Frage, indem Sie `Feedback on an existing feature` mit der Antwort `Which feature would you like to give feedback on?` eingeben.  

1.  Aktivieren Sie die Option **Context-only** (Nur Kontext). Durch die Option **Context-only** (Nur Kontext) wird angegeben, dass dieser Benutzertext _nur_ verständlich ist, wenn er als Antwort auf eine vorherige Frage ausgegeben wird. In diesem Szenario ist der Aufforderungstext als eigenständige Frage unverständlich; er lässt sich nur im Kontext der vorherigen Frage verstehen.
1.  Wählen Sie **Speichern** aus. 

    Dadurch wurde eine neue Frage erstellt, und diese wurde als Folgeaufforderung mit der Frage `Give feedback` verknüpft.
    
    Hier verfügt die übergeordnete Frage über zwei Folgeaufforderungen, die mit der vorherigen Frage `Give feedback` verknüpft sind.

    ![![Hier verfügt die übergeordnete Frage über zwei Folgeaufforderungen, die mit der vorherigen Frage `Give feedback` verknüpft sind.](../media/conversational-context/all-child-prompts-created.png)](../media/conversational-context/all-child-prompts-created.png#lightbox)

1. Wählen Sie **Save and Train** (Speichern und trainieren) aus, um die neuen Fragen in die Wissensdatenbank einzulernen. 

## <a name="add-existing-qna-pair-as-follow-up-prompt"></a>Hinzufügen eines vorhandenen Frage- und Antwortpaars als Folgeaufforderung

1. Wenn Sie ein vorhandenes Frage- und Antwortpaar als Folgeaufforderung verknüpfen möchten, wählen Sie die Zeile für das betreffende Frage- und Antwortpaar aus.
1. Wählen Sie in der Zeile **Add follow-up prompt** (Folgeaufforderung hinzufügen) aus.
1. Geben Sie im Popup-Dialogfeld im Suchfeld den Text der Frage ein. Alle Übereinstimmungen werden zurückgegeben. Wählen Sie die Frage, die als Folgeaufforderung festgelegt werden soll, aktivieren Sie **Context-only** (Nur Kontext), und wählen Sie dann **Speichern** aus. 

    Wenn Sie die Folgeaufforderung hinzugefügt haben, müssen Sie **Save and train** (Speichern und trainieren) auswählen.
  
## <a name="add-metadata-to-follow-up-prompts"></a>Hinzufügen von Metadaten zu Folgeaufforderungen 

Wenn in der Wissensdatenbank ein Frage- und Antwortpaar mit Folgeaufforderungen verknüpft ist, werden zuerst die Metadatenfilter angewendet und dann die Folgeaufforderungen zurückgegeben.

1. Fügen Sie den zwei Frage- und Antwortpaaren mit Folgeaufforderung Metadaten hinzu:

    |Frage|Hinzufügen von Metadaten|
    |--|--|
    |`Feedback on an QnA Maker service`|„Feature“:„all“|
    |`Feedback on an existing feature`|„Feature“:„one“|
    
    ![Hinzufügen von Metadaten zur Folgeaufforderung, sodass sie in der Konversationsantwort vom Dienst gefiltert werden kann](../media/conversational-context/add-metadata-feature-to-follow-up-prompt.png) 

1. Speichern und trainieren. 

    Wenn Sie die Frage `Give feedback` mit dem Metadatenfilter `Feature` mit dem Wert `all` senden, wird nur das Frage- und Antwortpaar mit den betreffenden Metadaten zurückgegeben. Beide Frage- und Antwortpaare werden nicht zurückgegeben, da sie nicht dem Filter entsprechen. 

## <a name="test-the-qna-set-to-get-all-the-follow-up-prompts"></a>Testen der Fragen und Antworten, um alle Folgeaufforderungen abzurufen

Wenn Sie die Frage mit Folgeaufforderungen im Bereich **Test** testen, wählen Sie **Mehrfachdurchlauf aktivieren**, und geben Sie Ihre Frage ein. Die Antwort enthält die Folgeaufforderungen.

![Beim Testen der Frage im Testbereich schließt die Antwort die Folgeaufforderungen ein.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

## <a name="json-request-to-return-initial-answer-and-follow-up-prompts"></a>JSON-Anforderung für Rückgabe der ersten Antwort und der Folgeaufforderungen

Fordern Sie mit dem leeren `context`-Objekt die Antwort auf die Frage des Benutzers an, und schließen Sie Folgeaufforderungen ein. 

```JSON
{
  "question": "accounts and signing in",
  "top": 30,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="json-response-to-return-initial-answer-and-follow-up-prompts"></a>JSON-Antwort für Rückgabe der ersten Antwort und der Folgeaufforderungen

Im vorherigen Abschnitt wurden eine Antwort und etwaige Folgeaufforderungen für `Accounts and signing in` angefordert. Die Antwort enthält die Aufforderungsinformationen, die sich unter `answers[0].context` befinden und den Text einschließen, der für den Benutzer angezeigt werden soll. 

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way he or she likes. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 86.96,
            "id": 37,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 38,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    },
                    {
                        "displayOrder": 1,
                        "qnaId": 39,
                        "qna": null,
                        "displayText": "Use Windows Hello to sign in"
                    },
                    {
                        "displayOrder": 2,
                        "qnaId": 40,
                        "qna": null,
                        "displayText": "Sign out"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 32.27,
            "id": 38,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start , and right-click your name. Then select Sign out. ",
            "score": 27.0,
            "id": 40,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ],
    "debugInfo": null
}
```

Das `prompts`-Array gibt Text in der `displayText`-Eigenschaft sowie den Wert für `qnaId` an, sodass Sie diese Antworten als nächste angezeigte Auswahlmöglichkeiten im Konversationsfluss präsentieren können. 

## <a name="json-request-to-return-non-initial-answer-and-follow-up-prompts"></a>JSON-Anforderung für Rückgabe einer nachfolgenden Antwort und der Folgeaufforderungen

Füllen Sie das `context`-Objekt aus, um den vorherigen Kontext einzuschließen.

In der folgenden JSON-Anforderung ist die aktuelle Frage `Use Windows Hello to sign in`, und die vorherige Frage war `Accounts and signing in`. 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 30,
  "userId": "Default",
  "isTest": false,
  "qnaId": 39,
  "context": {
    "previousQnAId": 37,
    "previousUserQuery": "accounts and signing in"
  }
}
``` 

##  <a name="json-response-to-return-non-initial-answer-and-follow-up-prompts"></a>JSON-Antwort für Rückgabe einer nachfolgenden Antwort und der Folgeaufforderungen

QnA Maker-JSON-Antwort _GenerateAnswer_ enthält die Folgeaufforderungen in der `context`-Eigenschaft des ersten Elements im `answers`-Objekt:

```JSON
{
    "answers": [
        {
            "questions": [
                "Give feedback"
            ],
            "answer": "What kind of feedback do you have?",
            "score": 100.0,
            "id": 288,
            "source": "Editorial",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 291,
                        "qna": null,
                        "displayText": "Feedback on an QnA Maker service"
                    },
                    {
                        "displayOrder": 0,
                        "qnaId": 292,
                        "qna": null,
                        "displayText": "Feedback on an existing feature"
                    }
                ]
            }
        }
    ]
}
```

## <a name="displaying-prompts-and-sending-context-in-the-client-application"></a>Anzeigen von Eingabeaufforderungen und Senden von Kontext in der Clientanwendung 

In der Clientanwendung werden alle Fragen zusammen mit einer Option für den Benutzer angezeigt, mit der die Aufforderungen als vorgeschlagene Aktionen oder Schaltflächen angezeigt werden können.
Die Clientanwendung speichert das aktuelle Frage- und Antwortpaar und die Benutzerabfrage als Kontext, der mit der nächsten Benutzerabfrage übergeben wird. 

In diesem [Bot Framework-Beispiel](https://aka.ms/qnamakermultiturnsample) wird der End-to-End-Ablauf eines Mehrfachdurchlauf-Dialogs in einem QnA Maker-Bot veranschaulicht.


## <a name="prompt-order-supported-in-api"></a>Unterstützung der Aufforderungsreihenfolge in der API

Die in der JSON-Antwort zurückgegebene Aufforderungsreihenfolge kann nur in der API bearbeitet werden. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über kontextbezogene Konversationen im [Dialogbeispiel](https://aka.ms/qnamakermultiturnsample), oder informieren Sie sich über [konzeptionelles Bot-Design für Konversationen mit Mehrfachdurchläufen](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Migrieren einer Wissensdatenbank](../Tutorials/migrate-knowledge-base.md)
