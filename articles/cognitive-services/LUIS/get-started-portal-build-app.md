---
title: 'Schnellstart: Erstellen einer neuen App im LUIS-Portal'
titleSuffix: Language Understanding - Azure Cognitive Services
description: In diesem Schnellstart erstellen Sie im LUIS-Portal eine neue App. Erstellen Sie die grundlegenden Teile der App, die Absichten und Entitäten, und testen Sie die App anschließend, indem Sie im interaktiven Testbereich eine Beispielbenutzeräußerung eingeben, um die vorhergesagte Absicht zu erhalten. Das Erstellen einer App ist kostenlos. Es ist kein Azure-Abonnement erforderlich.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: e97be28261d28c2a72e507adcdac0248691745c7
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57783169"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Schnellstart: Erstellen einer neuen App im LUIS-Portal

In diesem Schnellstart erstellen Sie eine neue App im [LUIS-Portal](https://www.luis.ai). Erstellen Sie die grundlegenden Teile der App, die **Absichten** und **Entitäten**, und testen Sie die App anschließend, indem Sie im interaktiven Testbereich eine Beispielbenutzeräußerung eingeben, um die vorhergesagte Absicht zu erhalten.

Das Erstellen einer App ist kostenlos. Es ist kein Azure-Abonnement erforderlich. Wenn Sie zum Bereitstellen Ihrer App bereit sind, erstellen Sie eine Azure Cognitive Services-Ressource und weisen sie der App zu. Der Bereitstellungsprozess wird im [nächsten Schnellstart](get-started-portal-deploy-app.md) erläutert.

## <a name="create-app"></a>Erstellen einer App 

1. Öffnen Sie das [LUIS-Portal](https://www.luis.ai) in einem Browser, und melden Sie sich an. Wenn Sie sich zum ersten Mal anmelden, müssen Sie ein kostenloses LUIS-Portalbenutzerkonto erstellen.

1. Wählen Sie auf der Kontextsymbolleiste die Option **Neue App erstellen** aus.

    [![Erstellen einer neuen App im LUIS-Portal](./media/get-started-portal-build-app/create-app-in-portal.png)](./media/get-started-portal-build-app/create-app-in-portal.png#lightbox)

1. Konfigurieren Sie die App im Popupfenster mit den folgenden Einstellungen, und wählen Sie dann **Fertig** aus.

    |Einstellungsname| Wert | Zweck|
    |--|--|--|
    |NAME|`myEnglishApp`|Eindeutiger Name der LUIS-App<br>required|
    |Kultur|**Englisch**|Sprache der Äußerungen von Benutzern, **en-us**<br>required|
    |BESCHREIBUNG|`App made with LUIS Portal`|Beschreibung der App<br>optional|

    ![Eingeben der Einstellungen für die neue App](./media/get-started-portal-build-app/create-new-app-settings.png)


## <a name="create-intent"></a>Erstellen einer Absicht 

Nachdem die App erstellt wurde, erstellen Sie als Nächstes die Absichten. Absichten dienen zum Kategorisieren der Texteingaben von Benutzern. Angenommen, Sie haben eine Personal-App mit zwei Funktionen: erstens Suchen nach Personen und Bewerben um Stellen und zweitens Suchen nach Formularen zur Bewerbung um Stellen. In diesem Fall entsprechen diese zwei unterschiedlichen _Absichten bzw. Funktionen_ den folgenden Absichten:

|Absicht|Beispieltext vom Benutzer<br>bezeichnet als _Äußerung_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

1. Nachdem die App erstellt wurde, wird die Seite **Intents** (Absichten) des Abschnitts **Erstellen** angezeigt. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

    [![Auswählen der Schaltfläche „Create new intent“ (Neue Absicht erstellen)](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Geben Sie den Namen der Absicht `FindForm` ein, und klicken Sie auf **Fertig**.

    ![Eingeben des Absichtsnamens „FindForm“](./media/get-started-portal-build-app/create-new-intent-dialog.png)

## <a name="add-example-utterance"></a>Hinzufügen einer Beispieläußerung 

Nachdem Sie die Absicht erstellt haben, fügen Sie als Nächstes Beispieläußerungen hinzu. Diese Äußerungen sind in einem Chatbot oder einer anderen Clientanwendung vom Benutzer eingegebene Texte, durch die die Absicht des Benutzers einer LUIS-Absicht zugeordnet wird. 

Für die Absicht `FindForm` dieser Beispielanwendung enthalten Beispieläußerungen die Formularnummer. Dies ist eine wichtige Information innerhalb der Äußerung, die von der Clientanwendung zum Erfüllen der Anforderung des Benutzers benötigt wird. 

Fügen Sie der Absicht `FindForm` die folgenden 15 Beispieläußerungen hinzu (die Übersetzungen in Klammern dienen lediglich zur Information). 

|#|Beispiele für Äußerungen|
|--|--|
|1|Looking for hrf-123456 (Nach hrf-123456 suchen)|
|2|Where is the human resources form hrf-234591? (Wo ist das Personalformular hrf-234591?)|
|3|hrf-345623, where is it (hrf-345623, wo)|
|4|Is it possible to send me hrf-345794 (Kann mir hrf-345794 zugesendet werden)|
|5|Do I need hrf-234695 to apply for an internal job? (Benötige ich hrf-234695 zur Bewerbung um eine interne Stelle?)|
|6|Does my manager need to know I'm applying for a job with hrf-234091 (Muss mein Vorgesetzter wissen, dass ich mich mit hrf-234091 um eine Stelle bewerbe)|
|7|Where do I send hrf-234918? Do I get an email response it was received? (Wohin muss ich hrf-234918 senden? Wird der Empfang per E-Mail bestätigt?)|
|8|hrf-234555|
|9|When was hrf-234987 updated? (Wann wurde hrf-234987 aktualisiert?)|
|10|Do I use form hrf-876345 to apply for engineering positions (Verwende ich das Formular hrf-876345 zur Bewerbung um Technikerstellen)|
|11|Was a new version of hrf-765234 submitted for my open req? (Wurde für meine offene Anfrage eine neue Version von hrf-765234 übermittelt?)|
|12|Do I use hrf-234234 for international jobs? (Muss ich für interne Stellen hrf-234234 verwenden?)|
|13|hrf-234598 spelling mistake (Rechtschreibfehler in hrf-234598)|
|14|will hrf-234567 be edited for new requirements (Wird hrf-234567 mit neuen Anforderungen überarbeitet)|
|15|hrf-123456, hrf-123123, hrf-234567|

Diese Beispieläußerungen unterscheiden sich gewollt in folgenden Punkten:

* Länge der Äußerung
* Interpunktion
* Wortwahl
* Zeitform des Verbs (is, was, will be)
* Reihenfolge der Wörter

[![Eingeben von Beispieläußerungen für die Absicht „FindForm“](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

## <a name="create-regular-expression-entity"></a>Erstellen einer Entität vom Typ „Regulärer Ausdruck“ 

Um die Formularnummer abzurufen, die als Teil der Vorhersageantwort zur Laufzeit zurückgegeben wird, muss das Formular als Entität markiert werden. Da der Text mit der Formularnummer sehr strukturiert ist, kann er als Entität vom Typ „Regulärer Ausdruck“ markiert werden. Gehen Sie wie folgt vor, um die Entität zu erstellen. 

1. Wählen Sie im linken Navigationsmenü die Option **Entitäten** aus. 

1. Wählen Sie auf der Seite „Entitäten“ die Option **Neue Entität erstellen** aus.

1. Geben Sie den Namen `Human Resources Form Number` ein, wählen Sie den Entitätstyp **RegEx** aus, und geben Sie den regulären Ausdruck `hrf-[0-9]{6}` ein. Dieser Ausdruck entspricht den Literalzeichen `hrf-` und lässt genau sechs Ziffern zu. 

    ![Eingeben der Entitätsinformationen für die Entität vom Typ „Regulärer Ausdruck“](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. Wählen Sie **Fertig**aus. 

## <a name="add-example-utterances-to-none-intent"></a>Hinzufügen von Beispieläußerungen zur Absicht „None“

Die Absicht **None** ist die Fallbackabsicht und sollte nicht leer gelassen werden. Diese Absicht sollte für jeweils zehn der restlichen Absichten der App eine Äußerung enthalten. 

Die Beispieläußerungen der Absicht **None** dürfen nicht im Bereich der Clientanwendung enthalten sein. 

1. Klicken Sie im linken Menü auf **Intents** (Absichten), und wählen Sie dann in der Liste der Absichten **None** aus.

1. Fügen Sie der Absicht die folgenden Beispieläußerungen hinzu.

    |Beispieläußerungen für die Absicht „None“|
    |--|
    |Bellende Hunde sind nervig|
    |Bestell mir eine Pizza|
    |Pinguine im Ozean|

    Diese Beispieläußerungen sind nicht im Bereich der Personal-App enthalten. Falls der Bereich Ihrer Personal-App Begriffe aus den Bereichen Tiere und Lebensmittel oder das Wort Ozean enthält, sollten diese Beispieläußerungen nicht für die Absicht **None** verwendet werden. 

## <a name="train-the-app"></a>Trainieren der App

Wählen Sie im oberen rechten Navigationsbereich **Train** (Trainieren) aus, um die Änderungen der Absicht und des Entitätsmodells auf die aktuelle Version der App anzuwenden. 

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Überprüfen der Entität vom Typ „Regulärer Ausdruck“ in den Beispieläußerungen

1. Überprüfen Sie, ob die Entität in der Absicht **FindForm** gefunden wird, indem Sie im linken Menü **Intents** (Absichten) und dann **FindForm** auswählen. 

    Die Entität ist an den Stellen markiert, an denen sie in den Beispieläußerungen vorkommt. Falls Sie anstelle des Entitätsnamens den ursprünglichen Text anzeigen möchten, schalten Sie über die Symbolleiste die **Entities View** (Entitätsansicht) um.

    [![Alle Beispieläußerungen mit markierten Entitäten](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Testen der neuen App im interaktiven Testbereich

Verwenden Sie den interaktiven **Testbereich** im LUIS-Portal, um zu überprüfen, ob die Entität aus neuen Äußerungen, die die App noch nicht kennt, extrahiert wird.

1. Wählen Sie im Menü oben rechts die Option **Testen** aus.

1. Fügen Sie eine neue Äußerung hinzu, und drücken Sie dann die EINGABETASTE:

    ```Is there a form named hrf-234098```

    ![Testen einer neuen Äußerung im Testbereich](./media/get-started-portal-build-app/test-new-utterance.png)

    Als Absicht mit der höchsten Bewertung wird korrekt **FindForm** mit einer Zuverlässigkeit von mehr als 90 % (0,977) angezeigt, und die Entität **Human Resources Form Number** wurde mit dem Wert „hrf-234098“ extrahiert. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie mit diesem Schnellstart fertig sind und nicht mit dem nächsten Schnellstart fortfahren möchten, wählen Sie im oberen Navigationsmenü **Meine Apps** aus. Aktivieren Sie dann in der Liste das Kontrollkästchen links neben der App, und wählen Sie auf der Kontextsymbolleiste über der Liste **Löschen** aus. 

[![Löschen der App aus der Liste „Meine Apps“](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [2. Bereitstellen einer App](get-started-portal-deploy-app.md)