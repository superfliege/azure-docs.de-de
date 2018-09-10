---
title: 'Tutorial: Erstellen einer LUIS-App zum Extrahieren von Daten: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine einfache LUIS-App erstellen, die Absichten und eine Entität vom Typ „Einfach“ verwendet, um Machine Learning-Daten (ML) zu extrahieren.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: a69ea8ea45a02399b7c6ad22f0dc514ad8537e06
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159655"
---
# <a name="tutorial-7-add-simple-entity-and-phrase-list"></a>Tutorial: 7. Hinzufügen einer Entität vom Typ „Einfach“ sowie einer Begriffsliste
In diesem Tutorial erstellen Sie eine App, die veranschaulicht, wie mithilfe der Entität vom Typ **Einfach** ML-Daten aus einer Äußerung extrahiert werden.

<!-- green checkmark -->
> [!div class="checklist"]
> * Grundlegendes zu einfachen Entitäten 
> * Erstellen einer neuen LUIS-App für den Personalbereich 
> * Hinzufügen einer einfachen Entität zum Extrahieren von Stellen aus der App
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort
> * Hinzufügen einer Begriffsliste, um das Signal von Stellenwörtern zu verstärken
> * Trainieren und Veröffentlichen der App und erneutes Abfragen des Endpunkts

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personal-App aus dem [Tutorial zur zusammengesetzten Entität](luis-tutorial-composite-entity.md) verfügen, [importieren](luis-how-to-start-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS-Website](luis-reference-regions.md#luis-website)). Die zu importierende App befindet sich im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-composite-HumanResources.json).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `simple`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren.  

## <a name="purpose-of-the-app"></a>Zweck der App
Diese App veranschaulicht, wie Sie Daten aus einer Äußerung extrahieren. Sehen Sie sich die folgenden Äußerungen eines Chatbots an:

|Äußerung|Extrahierbarer Stellenname|
|:--|:--|
|I want to apply for the new accounting job. (Ich möchte mich für die neue Stelle in der Buchhaltung bewerben.)|accounting (Buchhaltung)|
|Please submit my resume for the engineering position. (Ich möchte meinen Lebenslauf für die Stelle als Techniker übermitteln.)|engineering (Techniker)|
|Fill out application for job 123456 (Bewerbungsbogen für die Stelle 123456 ausfüllen)|123456|

In diesem Tutorial wird eine neue Entität hinzugefügt, um den Stellennamen zu extrahieren. 

## <a name="purpose-of-the-simple-entity"></a>Zweck der Entität vom Typ „Einfach“
Mithilfe der Entität vom Typ „Einfach“ für diese LUIS-App wird LUIS beigebracht, was ein Stellenname ist und wie er in einer Äußerung ermittelt werden kann. Der Teil der Äußerung, der die Stelle darstellt, kann sich basierend auf der Wortwahl und der Länge der Äußerung bei den einzelnen Äußerungen unterscheiden. LUIS benötigt Beispiele für Stellen in jeder Äußerung und für alle Absichten.  

Der Stellenname ist nicht ganz einfach zu ermitteln, da es sich dabei um ein Substantiv, um ein Verb oder um einen Ausdruck mit mehreren Wörtern handeln kann. Beispiel: 

|Aufträge|
|--|
|mitzuteilen.|
|software engineer (Softwareentwickler)|
|senior software engineer (leitender Softwareentwickler)|
|engineering team lead (Leiter des Technikerteams) |
|air traffic controller (Fluglotse)|
|motor vehicle operator (Kraftfahrer)|
|ambulance driver (Krankenwagenfahrer)|
|tender (Pfleger)|
|extruder (Strangpresser)|
|millwright (Mühlenbauer)|

Diese LUIS-App verfügt über Stellennamen in verschiedenen Absichten. Durch die Kennzeichnung dieser Wörter in allen Äußerungen der Absichten lernt LUIS mehr darüber, was eine Stelle ist und wo sie sich in Äußerungen befindet.

## <a name="create-job-simple-entity"></a>Erstellen einer einfachen Stellenentität

1. Vergewissern Sie sich, dass sich Ihre Personal-App im LUIS-Abschnitt **Build** befindet. Zu diesem Abschnitt gelangen Sie, indem Sie rechts oben auf der Menüleiste **Build** auswählen. 

2. Wählen Sie auf der Seite **Absichten** die Option **ApplyForJob** aus. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-select-applyforjob.png "Screenshot: LUIS mit hervorgehobener Absicht „ApplyForJob“")](media/luis-quickstart-primary-and-secondary-data/hr-select-applyforjob.png#lightbox)

3. Wählen Sie in der Äußerung `I want to apply for the new accounting job` das Wort `accounting` aus, geben Sie `Job` in das oberste Feld des Popupmenüs ein, und klicken Sie anschließend im Popupmenü auf **Create new entity** (Neue Entität erstellen). 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png "Screenshot: LUIS mit Absicht „ApplyForJob“ und hervorgehobenen Schritten für die Entitätserstellung")](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png#lightbox)

4. Überprüfen Sie Entitätsname und -typ im Popupfenster, und klicken Sie anschließend auf **Fertig**.

    ![Modales Popupdialogfeld zum Erstellen einer einfachen Entität mit dem Namen „Job“ und dem Typ „Simple“](media/luis-quickstart-primary-and-secondary-data/hr-create-simple-entity-popup.png)

5. Kennzeichnen Sie in der Äußerung `Submit resume for engineering position` das Wort `engineering` als Entität „Job“. Wählen Sie das Wort `engineering` und anschließend im Popupmenü die Option **Job** aus. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png "Screenshot: LUIS mit hervorgehobener Entitätskennzeichnung „Job“")](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png#lightbox)

    Alle Äußerungen wurden gekennzeichnet, aber fünf Äußerungen sind nicht genug, um LUIS mit stellenbezogenen Wörtern und Ausdrücken vertraut zu machen. Für Stellen mit Zahlenwert sind keine weiteren Beispiele erforderlich, da hierfür eine Entität vom Typ „Regulärer Ausdruck“ verwendet wird. Für Stellen mit Wörtern oder Ausdrücken werden mindestens 15 weitere Beispiele benötigt. 

6. Fügen Sie weitere Äußerungen hinzu, und kennzeichnen Sie die Wörter und Ausdrücke für die Stelle jeweils mit der Entität **Job**. Bei den Stellentypen handelt es sich um allgemeine Beschäftigungen für eine Arbeitsvermittlung. Wenn Sie Stellen für eine bestimmte Branche verwenden möchten, sollten die Wörter für die Stellen dies widerspiegeln. 

    |Äußerung|Entität „Job“|
    |:--|:--|
    |I'm applying for the Program Manager desk in R&D (Ich bewerbe mich für die Position als Programm-Manager in der Entwicklungsabteilung.)|Program Manager (Programm-Manager)|
    |Here is my line cook application. (Hier ist meine Bewerbung als Gardemanger.)|line cook (Gardemanger)|
    |My resume for camp counselor is attached. (Anbei mein Lebenslauf für die Stelle als Camp Counselor.)|camp counselor|
    |This is my c.v. for administrative assistant. (Hier ist mein CV für die Stelle als Verwaltungsassistent.)|administrative assistant (Verwaltungsassistent)|
    |I want to apply for the management job in sales. (Hiermit bewerbe ich mich für die Management-Stelle im Vertrieb.)|management, sales (Management, Vertrieb)|
    |This is my resume for the new accounting position. (Hiermit übersende ich Ihnen meinen Lebenslauf für die neue Position in der Buchhaltung.)|accounting (Buchhaltung)|
    |My application for barback is included. (Meine Bewerbung als Barassistent finden Sie anbei.)|barback (Barassistent)|
    |I'm submitting my application for roofer and framer. (Hiermit übersende ich Ihnen meine Bewerbung als Dachdecker und Zimmerer.)|roofer, framer (Dachdecker, Zimmerer)|
    |My c.v. for bus driver is here. (Mein Lebenslauf für die Stelle als Busfahrer ist beigefügt.)|bus driver (Busfahrer)|
    |I'm a registered nurse. Here is my resume. (Ich bin ausgebildete Krankenschwester. Hier ist mein Lebenslauf.)|registered nurse (ausgebildete Krankenschwester)|
    |I would like to submit my paperwork for the teaching position I saw in the paper. (Hiermit möchte ich Ihnen meine Unterlagen für die ausgeschriebene Stelle als Lehrkraft zukommen lassen.)|teaching (Lehrkraft)|
    |This is my c.v. for the stocker post in fruits and vegetables. (Hier ist mein Lebenslauf für die Stelle als Auffüller in der Obst- und Gemüseabteilung.)|stocker (Auffüller)|
    |Apply for tile work. (Bewerbung als Fliesenleger.)|Kachel|
    |Attached resume for landscape architect. (Anbei mein Lebenslauf für die Position als Landschaftsarchitekt.)|landscape architect (Landschaftsarchitekt)|
    |My curriculum vitae for professor of biology is enclosed. (Anbei finden Sie mein Curriculum Vitae für die Stelle als Biologieprofessor.)|professor of biology (Biologieprofessor)|
    |I would like to apply for the position in photography. (Ich möchte mich gerne als Fotograf bewerben.)|photography (Fotografie)|Git 

## <a name="label-entity-in-example-utterances-for-getjobinformation-intent"></a>Kennzeichnen der Entität in Beispieläußerungen für die Absicht „GetJobInformation“
1. Wählen Sie im linken Menü die Option **Intents** (Absichten) aus.

2. Wählen Sie in der Liste mit den Absichten die Option **GetJobInformation** aus. 

3. Kennzeichnen Sie die Stellen in den Beispieläußerungen:

    |Äußerung|Entität „Job“|
    |:--|:--|
    |Is there any work in databases? (Gibt es offene Stellen im Datenbankbereich?)|Datenbanken|
    |Ich möchte mich beruflich neu orientieren und suche nach Aufgaben in der Buchhaltung.|accounting (Buchhaltung)|
    |What positions are available for senior engineers? (Welche Positionen sind für Oberingenieure verfügbar?)|senior engineers (Oberingenieure)|

    Es gibt noch andere Beispieläußerungen, diese enthalten jedoch keine Wörter für Stellen.

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Abfragen des Endpunkts mit einer anderen Äußerung

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `Here is my c.v. for the programmer job` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (die Äußerung **query** (Abfrage)). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Äußerungen vom Typ `ApplyForJob` zurückgeben.

```JSON
{
  "query": "Here is my c.v. for the programmer job",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.9826467
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.9826467
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0218927357
    },
    {
      "intent": "MoveEmployee",
      "score": 0.007849265
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00349470088
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.00348804821
    },
    {
      "intent": "None",
      "score": 0.00319909188
    },
    {
      "intent": "FindForm",
      "score": 0.00222647213
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00211193133
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00172086991
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.00138010911
    }
  ],
  "entities": [
    {
      "entity": "programmer",
      "type": "Job",
      "startIndex": 24,
      "endIndex": 33,
      "score": 0.5230502
    }
  ]
}
```

## <a name="names-are-tricky"></a>Das Problem mit Namen
Die LUIS-App hat die korrekte Absicht mit hoher Zuverlässigkeit ermittelt und den Stellennamen extrahiert, aber Namen sind kompliziert. Nehmen wir beispielsweise die Äußerung `This is the lead welder paperwork`.  

Im folgenden JSON-Code antwortet LUIS zwar mit der korrekten Absicht (`ApplyForJob`), extrahiert aber den Stellennamen `lead welder` nicht: 

```JSON
{
  "query": "This is the lead welder paperwork.",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.468558252
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.468558252
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0102701457
    },
    {
      "intent": "MoveEmployee",
      "score": 0.009442534
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00639619166
    },
    {
      "intent": "None",
      "score": 0.005859333
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.005087704
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00315379258
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00259344373
    },
    {
      "intent": "FindForm",
      "score": 0.00193389168
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.000420796918
    }
  ],
  "entities": []
}
```

Da ein Name eine beliebige Zeichenfolge sein kann, sagt LUIS Entitäten präziser voraus, wenn das Signal durch eine Begriffsliste mit Wörtern verstärkt wird.

## <a name="to-boost-signal-add-jobs-phrase-list"></a>Hinzufügen einer Begriffsliste für Stellen, um das Signal zu verstärken
Öffnen Sie die Datei [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) aus dem GitHub-Repository „LUIS-Samples“. Die Liste enthält über 1.000 stellenbezogene Wörter und Begriffe. Suchen Sie in der Liste nach Stellenwörtern, die für Sie relevant sind. Sollten die gewünschten Wörter oder Begriffe nicht in der Liste enthalten sein, fügen Sie eigene Wörter/Begriffe hinzu.

1. Klicken Sie im Abschnitt **Build** der LUIS-App im Menü **Improve app performance** (App-Leistung verbessern) auf **Phrase lists** (Begriffslisten).

    [![](media/luis-quickstart-primary-and-secondary-data/hr-select-phrase-list-left-nav.png "Screenshot: Hervorgehobene Begriffslistenoption auf der linken Navigationsleiste")](media/luis-quickstart-primary-and-secondary-data/hr-select-phrase-list-left-nav.png#lightbox)

2. Klicken Sie auf **Create new phrase list** (Neue Begriffsliste erstellen). 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-new-phrase-list.png "Screenshot: Hervorgehobene Schaltfläche zum Erstellen einer neuen Begriffsliste")](media/luis-quickstart-primary-and-secondary-data/hr-create-new-phrase-list.png#lightbox)

3. Nennen Sie die Begriffsliste `Jobs`, und kopieren Sie die Liste aus „jobs-phrase-list.csv“ in das Textfeld **Werte**. Drücken Sie die EINGABETASTE. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Screenshot: Popupdialogfeld zum Erstellen einer neuen Begriffsliste")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Wenn Sie der Begriffsliste weitere Wörter hinzufügen möchten, können Sie sich die verwandten Werte (**Related Values**) ansehen und alle Wörter hinzufügen, die für Sie relevant sind. 

4. Klicken Sie auf **Speichern**, um die Begriffsliste zu aktivieren.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Screenshot des Popupdialogfelds zum Erstellen einer neuen Begriffsliste mit Wörtern im Feld für die Begriffslistenwerte")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. [Trainieren](#train-the-luis-app) und [veröffentlichen](#publish-the-app-to-get-the-endpoint-URL) Sie die App erneut, um die Begriffsliste zu verwenden.

6. Fragen Sie den Endpunkt erneut mit der gleichen Äußerung ab: `This is the lead welder paperwork.`

    Die JSON-Antwort enthält die extrahierte Entität:

    ```JSON
    {
        "query": "This is the lead welder paperwork.",
        "topScoringIntent": {
            "intent": "ApplyForJob",
            "score": 0.920025647
        },
        "intents": [
            {
            "intent": "ApplyForJob",
            "score": 0.920025647
            },
            {
            "intent": "GetJobInformation",
            "score": 0.003800706
            },
            {
            "intent": "Utilities.StartOver",
            "score": 0.00299335527
            },
            {
            "intent": "MoveEmployee",
            "score": 0.0027167045
            },
            {
            "intent": "None",
            "score": 0.00259556063
            },
            {
            "intent": "FindForm",
            "score": 0.00224019377
            },
            {
            "intent": "Utilities.Stop",
            "score": 0.00200693542
            },
            {
            "intent": "Utilities.Cancel",
            "score": 0.00195913855
            },
            {
            "intent": "Utilities.Help",
            "score": 0.00162656687
            },
            {
            "intent": "Utilities.Confirm",
            "score": 0.0002851904
            }
        ],
        "entities": [
            {
            "entity": "lead welder",
            "type": "Job",
            "startIndex": 12,
            "endIndex": 22,
            "score": 0.8295959
            }
        ]
    }
    ```

## <a name="phrase-lists"></a>Begriffslisten
Durch Hinzufügen der Begriffsliste wurde das Signal der Wörter in der Liste verstärkt. Sie wird jedoch **nicht** für exakte Übereinstimmungen verwendet. Die Begriffsliste enthält sowohl mehrere Stellen, die mit dem Wort `lead` beginnen, als auch die Stelle `welder`, aber nicht die Stelle `lead welder`. Diese Begriffsliste für Stellen ist wahrscheinlich nicht vollständig. Fügen Sie Ihrer Begriffsliste daher nach und nach weitere Stellenwörter hinzu, die Sie im Zuge der regelmäßigen [Überprüfung von Endpunktäußerungen](luis-how-to-review-endoint-utt.md) ermitteln. Trainieren und veröffentlichen Sie die App anschließend erneut.

## <a name="what-has-this-luis-app-accomplished"></a>Was wurde mit dieser LUIS-App erreicht?
Diese App mit einer einfachen Entität und einer Begriffsliste mit Wörtern hat eine Abfrageabsicht in natürlicher Sprache ermittelt und die Auftragsdaten zurückgegeben. 

Ihr Chatbot verfügt nun über genügend Informationen, um die primäre Aktion (Stellenbewerbung) zu ermitteln, sowie über einen Parameter für diese Aktion (um welche Stelle es geht). 

## <a name="where-is-this-luis-data-used"></a>Wo werden diese LUIS-Daten verwendet? 
LUIS hat diese Anforderung abgeschlossen. Die aufrufende Anwendung (etwa ein Chatbot) kann das Ergebnis für „topScoringIntent“ und die Daten aus der Entität verwenden, um die Stelleninformationen über eine Drittanbieter-API an einen Mitarbeiter der Personalabteilung zu senden. Sind andere programmgesteuerte Optionen für den Bot oder die aufrufende Anwendung vorhanden, werden sie nicht von LUIS verwendet. LUIS bestimmt lediglich die Absicht des Benutzers. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer vorgefertigten keyPhrase-Entität](luis-quickstart-intent-and-key-phrase.md)