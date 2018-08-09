---
title: 'Azure: Erstellen einer einfachen App mit zwei Absichten | Microsoft-Dokumentation'
description: In diesem Schnellstart erfahren Sie, wie Sie eine einfache LUIS-App mit zwei Absichten und ohne Entitäten zur Identifizierung von Benutzeräußerungen erstellen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: de295a93d395cee4c4dfbea4f2e7f7338036feb8
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494372"
---
# <a name="tutorial-1-build-app-with-custom-domain"></a>Tutorial 1: Erstellen einer App mit einer benutzerdefinierten Domäne
In diesem Tutorial erstellen Sie eine App, die die Verwendung von **Absichten** veranschaulicht, um die _Intention_ des Benutzers basierend auf der Äußerung (Text) zu ermitteln, die sie an die App senden. Wenn Sie diesen Schnellstart abschließen, verfügen Sie über einen LUIS-Endpunkt in der Cloud.

Diese App ist die einfachste Art von LUIS-App, da sie keine Daten von Äußerungen extrahiert. Sie bestimmt lediglich die Absicht der Äußerung des Benutzers.

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen einer neuen App für den Personalbereich 
> * Hinzufügen der Absicht GetJobInformation
> * Hinzufügen von Beispieläußerungen zur Absicht GetJobInformation 
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort
> * Hinzufügen der Absicht ApplyForJob
> * Hinzufügen von Beispieläußerungen zur Absicht ApplyForJob 
> * Erneutes Trainieren, Veröffentlichen und Abfragen des Endpunkts 

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="purpose-of-the-app"></a>Zweck der App
Diese App verfügt über einige Absichten. Die erste Absicht (**`GetJobInformation`**) identifiziert, ob ein Benutzer Informationen über verfügbare Stellen in einem Unternehmen möchte. Die zweite Absicht (**`None`**) identifiziert jede andere Art von Äußerung. Im späteren Verlauf dieses Schnellstarts wird eine dritte Absicht (`ApplyForJob`) hinzugefügt. 

## <a name="create-a-new-app"></a>Erstellen einer neuen App
1. Melden Sie sich bei der [LUIS](luis-reference-regions.md#luis-website)-Website an. Achten Sie darauf, sich unter der [Region](luis-reference-regions.md#publishing-regions) anzumelden, in der die LUIS-Endpunkte veröffentlicht werden sollen.

2. Klicken Sie auf der [LUIS](luis-reference-regions.md#luis-website)-Website auf **Create new app** (Neue App erstellen).  

    [![](media/luis-quickstart-intents-only/app-list.png "Screenshot der Seite „Meine Apps“")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Geben Sie im Popupdialogfenster den Namen `HumanResources` ein. Diese App umfasst Fragen über die Personalabteilung Ihres Unternehmens. Diese Art von Abteilung kümmert sich um Angelegenheiten im Zusammenhang mit der Beschäftigung, z.B. Stellen im Unternehmen, die gefüllt werden müssen.

    ![Neue LUIS-App](./media/luis-quickstart-intents-only/create-app.png)

4. Nach Abschluss dieses Vorgangs wird in der App die Seite **Intents** (Absichten) mit der Absicht **None** (Keine) angezeigt. 

## <a name="create-getjobinformation-intention"></a>Erstellen der Absicht GetJobInformation
1. Klicken Sie auf **Create new intent** (Neue Absicht erstellen). Geben Sie den Namen für die neue Absicht ein: `GetJobInformation`. Diese Absicht wird jedes Mal vorhergesagt, wenn ein Benutzer Informationen über offene Stellen in Ihrem Unternehmen möchte.

    ![](media/luis-quickstart-intents-only/create-intent.png "Screenshot vom Dialogfenster zum Erstellen einer neuen Absicht")

    Indem Sie eine Absicht erstellen, erstellen Sie auch eine Kategorie der zu ermittelnden Informationen. Durch Angabe der Kategorie können andere Anwendungen, die die Ergebnisse der LUIS-Abfrage nutzen, anhand des Kategorienamens eine passende Antwort finden. LUIS beantwortet diese Fragen nicht, sondern gibt lediglich an, welche Art von Information in natürlicher Sprache erfragt wird. 

2. Fügen Sie dieser Absicht sieben Äußerungen hinzu, von denen Sie erwarten, dass ein Benutzer nach ihnen fragt. Zum Beispiel:

    | Beispiele für Äußerungen|
    |--|
    |Wurden heute neue Stellen ausgeschrieben?|
    |Welche Positionen sind für Oberingenieure verfügbar?|
    |Gibt es offene Stellen im Datenbankbereich?|
    |Ich möchte mich beruflich neu orientieren und suche nach Aufgaben in der Buchhaltung.|
    |Wo finde ich die Stellenangebote?|
    |Neue Stellen?|
    |Gibt es neue Stellen in Seattle?|

    [![](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Screenshot der Eingabe neuer Äußerungen für die Absicht MyStore")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

3. Die LUIS-App enthält derzeit keine Äußerungen für die Absicht **None** (Keine). Sie benötigt Äußerungen, die von der App nicht beantwortet werden. Lassen Sie sie nicht leer. Klicken Sie im linken Bereich auf **Intents** (Absichten). 

4. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, die für Ihre App jedoch nicht relevant sind. Wenn die App sich mit Stellenausschreibungen befasst, sind folgende Äußerungen für **None** (Keine) empfehlenswert:

    | Beispiele für Äußerungen|
    |--|
    |Bellende Hunde sind nervig|
    |Bestell mir eine Pizza|
    |Pinguine im Ozean|

    In der Anwendung, die den LUIS-Dienst aufruft (beispielsweise ein Chatbot), kann der Bot den Benutzer fragen, ob er die Unterhaltung beenden möchte, wenn LUIS für eine Äußerung die Absicht vom Typ **None** (Keine) zurückgibt. Der Chatbot kann außerdem weitere Anweisungen zum Fortsetzen der Unterhaltung geben, falls der Benutzer sie nicht beenden möchte. 

## <a name="train-and-publish-the-app"></a>Trainieren und Veröffentlichen der App

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Veröffentlichen der App im Endpunkt

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="query-endpoint-for-getjobinformation-intent"></a>Abfragen des Endpunkts für die Absicht GetJobInformation

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `I'm looking for a job with Natual Language Processing` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). Diese Äußerung entspricht keiner der Beispieläußerungen in Schritt 4. Sie ist daher ein guter Test und sollte die Absicht `GetJobInformation` als höchst bewertete Absicht zurückgeben. 

    ```
    {
      "query": "I'm looking for a job with Natual Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.8965092
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.8965092
        },
        {
          "intent": "None",
          "score": 0.147104025
        }
      ],
      "entities": []
    }
    ```

## <a name="create-applyforjob-intention"></a>Erstellen der Absicht ApplyForJob
Kehren Sie zur Browserregisterkarte der LUIS-Website zurück, und erstellen Sie eine neue Absicht zum Bewerben auf eine Stelle.

1. Klicken Sie im Menü oben rechts auf **Build** (Erstellen), um zur App-Erstellung zurückzukehren.

2. Wählen Sie im linken Menü die Option **Intents** (Absichten) aus.

3. Klicken Sie auf **Create new intent** (Neue Absicht erstellen), und geben Sie den Namen `ApplyForJob` ein. 

    ![LUIS-Dialogfeld zum Erstellen einer neuen Absicht](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

4. Fügen Sie dieser Absicht mehrere voraussichtliche Äußerungen von Benutzern hinzu, z.B.:

    | Beispiele für Äußerungen|
    |--|
    |Ich möchte mich für die neue Stelle in der Buchhaltung bewerben|
    |Bewerbungsbogen für die Stelle 123456 ausfüllen|
    |Lebenslauf für die Stelle als Techniker übermitteln|
    |Hier ist mein Lebenslauf für die Position 654234|
    |Stelle 567890 und meine Unterlagen|

    [![](media/luis-quickstart-intents-only/utterance-applyforjob.png "Screenshot der Eingabe neuer Äußerungen für die Absicht ApplyForJob")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    Die bezeichnete Absicht ist rot umrandet, da LUIS nicht sicher ist, ob diese Absicht richtig ist. Durch Trainieren der App lernt LUIS, ob die Äußerungen der richtigen Absicht zugeordnet sind. 

    [Trainieren und veröffentlichen](#train-and-publish-the-app) Sie die App erneut. 

## <a name="query-endpoint-for-applyforjob-intent"></a>Abfragen des Endpunkts für die Absicht ApplyForJob

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie im neuen Browserfenster `Can I submit my resume for job 235986` am Ende der URL ein. 

    ```
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9166808
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9166808
        },
        {
          "intent": "GetJobInformation",
          "score": 0.07162977
        },
        {
          "intent": "None",
          "score": 0.0262826588
        }
      ],
      "entities": []
    }
    ```

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Mit nur wenigen Absichten hat diese App eine Abfrage in natürlicher Sprache identifiziert, die die gleiche Absicht hat, aber anders formuliert wurde. 

Das JSON-Ergebnis identifiziert die am höchsten bewertete Absicht. Alle Bewertungen liegen zwischen 1 und 0. Die bessere Bewertung liegt näher bei 1. Die Bewertung der Absichten `GetJobInformation` und `None` liegt deutlich näher an 0 (null). 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
LUIS hat diese Anforderung abgeschlossen. Die aufrufende Anwendung, z.B. ein Chatbot, kann das Ergebnis der am höchsten bewerteten Absicht zum Suchen von Informationen (die nicht in LUIS gespeichert sind) verwenden, um die Frage zu beantworten oder die Konversation zu beenden. Dies sind programmgesteuerte Optionen für den Bot oder die aufrufende Anwendung. Diese Schritte werden nicht von LUIS ausgeführt. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Add prebuilt intents and entities to this app (Hinzufügen von vordefinierten Absichten und Entitäten)](luis-tutorial-prebuilt-intents-entities.md)
