---
title: 'Tutorial 1: Ermitteln von Absichten in einer benutzerdefinierten LUIS-App'
titleSuffix: Azure Cognitive Services
description: Erstellen einer benutzerdefinierten App, die die Absicht eines Benutzers vorhersagt. Diese App stellt den einfachsten Typ von LUIS-App dar, da sie keine verschiedenen Datenelemente, wie E-Mail-Adressen oder Datumsangaben, aus dem Äußerungstext extrahiert.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: b229dbc90f3f6ecc226c88ee393114f233bcf1a2
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035404"
---
# <a name="tutorial-1-build-custom-app-to-determine-user-intentions"></a>Tutorial 1: Erstellen einer benutzerdefinierten App zum Bestimmen von Benutzerabsichten

In diesem Tutorial erstellen Sie eine benutzerdefinierte Personalwesen-App, die die Absicht eines Benutzers auf der Grundlage seiner Äußerung (Text) vorhersagt. Wenn Sie diesen Schnellstart abschließen, verfügen Sie über einen LUIS-Endpunkt in der Cloud.

Der Zweck der App besteht darin, die Intention von dialogorientiertem, natürlichsprachigem Text zu erkennen. Diese Intentionen sind in **Absichten** kategorisiert. Diese App verfügt über einige Absichten. Die erste Absicht (**`GetJobInformation`**) identifiziert, ob ein Benutzer Informationen über verfügbare Stellen in einem Unternehmen möchte. Die zweite Absicht, **`None`**, wird für alle Äußerungen des Benutzers verwendet, die außerhalb der _Domäne_ (des Bereichs) dieser App liegen. Später wird eine dritte Absicht, **`ApplyForJob`**, für alle Äußerungen, die sich auf Stellenbewerbungen beziehen, hinzugefügt. Diese dritte Absicht unterscheidet sich von `GetJobInformation`, da die Stelleninformationen bereits bekannt sein sollten, wenn sich jemand auf eine Stelle bewirbt. Abhängig von der Wortwahl kann die Bestimmung der Absicht aber schwierig sein, weil es sich bei beiden um eine Stelle handelt.

Nachdem LUIS die JSON-Antwort zurückgegeben hat, hat LUIS diese Anforderung erledigt. LUIS gibt keine Antworten auf Benutzeräußerungen, es identifiziert lediglich, nach welcher Art Information in natürlicher Sprache gefragt wird. 

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Erstellen einer neuen App 
> * Erstellen von Absichten
> * Hinzufügen von Beispieläußerungen
> * Trainieren der App
> * App veröffentlichen
> * Absicht beim Endpunkt abrufen

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Erstellen einer neuen App

1. Melden Sie sich beim LUIS-Portal mit der URL [https://www.luis.ai](https://www.luis.ai) an. 

2. Klicken Sie auf **Create new app** (Neue App erstellen).  

    [![](media/luis-quickstart-intents-only/app-list.png "Screenshot der Seite „Meine Apps“ von Language Understanding (LUIS)")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Geben Sie im Popupdialogfeld den Namen `HumanResources` ein, und behalten Sie die Standardkultur **Englisch** bei. Lassen Sie die Beschreibung leer.

    ![Neue LUIS-App](./media/luis-quickstart-intents-only/create-app.png)

    Als nächstes zeigt die App die Seite **Absichten** mit der Absicht **Keine** an.

## <a name="getjobinformation-intent"></a>GetJobInformation-Absicht

1. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. Geben Sie den Namen für die neue Absicht ein: `GetJobInformation`. Diese Absicht wird jedes Mal vorhergesagt, wenn ein Benutzer Informationen über offene Stellen im Unternehmen möchte.

    ![](media/luis-quickstart-intents-only/create-intent.png "Screenshot des Dialogfelds „Neus Absicht“ in Language Understanding (LUIS)")

2. Durch die Bereitstellung von _Beispieläußerungen_ trainieren Sie LUIS, welche Art von Äußerungen für diese Absicht vorhergesagt werden sollten. Fügen Sie dieser Absicht mehrere voraussichtliche Beispieläußerungen von Benutzern hinzu, z.B.:

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

    [!include[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    


## <a name="none-intent"></a>Absicht „None“ 
Die Clientanwendung muss wissen, ob eine Äußerung außerhalb der Themendomäne der Anwendung liegt. Wenn LUIS für eine Äußerung die Absicht **None** (Keine) zurückgibt, kann Ihre Anwendung fragen, ob der Benutzer die Unterhaltung beenden möchte. Die Clientanwendung kann außerdem weitere Anweisungen zum Fortsetzen der Unterhaltung geben, falls der Benutzer sie nicht beenden möchte. 

Diese Beispieläußerungen außerhalb der Themendomäne sind in der Absicht **None** (Keine) gruppiert. Lassen Sie sie nicht leer. 

1. Klicken Sie im linken Bereich auf **Intents** (Absichten).

2. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, die für Ihre Personalwesen-App jedoch nicht relevant sind. Wenn die App sich mit Stellenausschreibungen befasst, sind solche Äußerungen für **None** (Keine) etwa:

    | Beispiele für Äußerungen|
    |--|
    |Bellende Hunde sind nervig|
    |Bestell mir eine Pizza|
    |Pinguine im Ozean|


## <a name="train"></a>Trainieren 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent"></a>Abrufen der Absicht

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `I'm looking for a job with Natural Language Processing` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). Diese Äußerung stimmt nicht mit einer der Beispieläußerungen überein. Sie stellt einen guten Test dar und sollte die Absicht `GetJobInformation` als Absicht mit der höchsten Bewertung zurückgeben. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
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

    Die Ergebnisse umfassen **alle Absichten** in der App, aktuell 2. Das Entitätenarray ist leer, weil diese App aktuell keine Entitäten aufweist. 

    Das JSON-Ergebnis identifiziert die am höchsten bewertete Absicht als **`topScoringIntent`**-Eigenschaft. Alle Bewertungen liegen zwischen 1 und 0. Die bessere Bewertung liegt näher bei 1. 

## <a name="applyforjob-intent"></a>ApplyForJob-Absicht
Kehren Sie zur LUIS-Website zurück, und erstellen Sie eine neue Absicht, um zu bestimmen, ob sich die Benutzeräußerung um eine Stellenbewerbung dreht.

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

## <a name="train-again"></a>Erneutes Trainieren

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Erneutes Veröffentlichen

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-again"></a>Erneutes Abrufen der Absicht

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie im neuen Browserfenster `Can I submit my resume for job 235986` am Ende der URL ein. 

    ```JSON
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

    In den Ergebnissen ist die neue Absicht **ApplyForJob** ebenso enthalten wie die beiden vorhandenen Absichten. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden die Personalwesen-App und zwei Absichten erstellt, Beispieläußerungen für jede Absicht hinzugefügt, Beispieläußerungen zur Absicht „None“ (Keine) hinzugefügt, die App trainiert, veröffentlicht und am Endpunkt getestet. Dies sind die grundlegenden Schritte bei der Erstellung eines LUIS-Modells. 

> [!div class="nextstepaction"]
> [Add prebuilt intents and entities to this app (Hinzufügen von vordefinierten Absichten und Entitäten)](luis-tutorial-prebuilt-intents-entities.md)
