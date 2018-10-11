---
title: 'Tutorial 7: Simple-Entität mit Begriffsliste in LUIS'
titleSuffix: Azure Cognitive Services
description: Extrahieren von maschinell gelernten Daten aus einer Äußerung
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: e0cdda629ddded121a424af61377c04ee8d958d3
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867543"
---
# <a name="tutorial-7-extract-names-with-simple-entity-and-phrase-list"></a>Tutorial 7: Extrahieren von Namen mit der Simple-Entität und einer Begriffsliste

In diesem Tutorial werden maschinell gelernte Daten einer Stellenbezeichnung mithilfe der **Simple**-Entität aus einer Äußerung extrahiert. Um die Genauigkeit der Extraktion zu steigern, fügen Sie der Simple-Entität eine Begriffsliste mit spezifischen Ausdrücken hinzu.

In diesem Tutorial wird eine neue Simple-Entität hinzugefügt, um die Stellenbezeichnung zu extrahieren. Mithilfe der Entität vom Typ „Einfach“ für diese LUIS-App wird LUIS beigebracht, was ein Stellenname ist und wie er in einer Äußerung ermittelt werden kann. Der Teil der Äußerung, der die Stellenbezeichnung darstellt, kann sich basierend auf der Wortwahl und der Länge der Äußerung bei den einzelnen Äußerungen unterscheiden. LUIS benötigt Beispiele für Stellenbezeichnungen aus allen Absichten, in denen Stellenbezeichnungen verwendet werden.  

Die Simple-Entität eignet sich gut für diese Art von Daten, wenn folgende Bedingungen erfüllt sind:

* Die Daten stellen nur ein Konzept dar.
* Die Daten sind nicht wohlgeformt, wie etwa ein regulärer Ausdruck.
* Die Daten sind nicht gängig, etwa wie eine vorkonfigurierte Entität von Telefonnummer oder Daten.
* Die Daten stimmen nicht exakt mit einer Liste bekannter Wörter überein, wie etwa eine Listenentität.
* Die Daten enthalten keine anderen Datenelemente, wie etwa eine zusammengesetzte Entität oder eine hierarchische Entität.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App
> * Hinzufügen einer einfachen Entität zum Extrahieren von Stellen aus der App
> * Hinzufügen einer Begriffsliste, um das Signal von Stellenwörtern zu verstärken
> * Trainieren 
> * Veröffentlichen 
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App

Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Wenn Sie nicht über die HumanResources-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-composite-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `simple`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

## <a name="simple-entity"></a>Entität vom Typ „Simple“
Die Simple-Entität erkennt ein einfaches Datenkonzept, das in Wörtern oder Formulierungen enthalten ist.

Sehen Sie sich die folgenden Äußerungen eines Chatbots an:

|Äußerung|Extrahierbarer Stellenname|
|:--|:--|
|I want to apply for the new accounting job. (Ich möchte mich für die neue Stelle in der Buchhaltung bewerben.)|accounting (Buchhaltung)|
|Submit my resume for the engineering position. (Meinen Lebenslauf für die Technikerstelle einsenden.)|engineering (Techniker)|
|Fill out application for job 123456 (Bewerbungsbogen für die Stelle 123456 ausfüllen)|123456|

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

Diese LUIS-App verfügt über Stellennamen in verschiedenen Absichten. Durch die Kennzeichnung dieser Wörter in allen Äußerungen der Absichten lernt LUIS mehr darüber, was eine Stellenbezeichnung ist und wo sie sich in Äußerungen befindet.

Nachdem die Entitäten in den Beispieläußerungen markiert sind, ist es wichtig, eine Begriffsliste hinzuzufügen, um das Signal der Simple-Entität zu verstärken. Eine Begriffsliste wird **nicht** als genaue Übereinstimmung verwendet und braucht nicht jeden möglichen erwartbaren Wert zu enthalten. 

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie auf der Seite **Absichten** die Option **ApplyForJob** aus. 

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

## <a name="label-entity-in-example-utterances"></a>Etikettieren der Entität in Beispieläußerungen

Das Etikettieren oder _Markieren_ der Entität zeigt LUIS, wo sich die Entität in den Beispieläußerungen findet.

1. Wählen Sie im linken Menü die Option **Intents** (Absichten) aus.

2. Wählen Sie in der Liste mit den Absichten die Option **GetJobInformation** aus. 

3. Kennzeichnen Sie die Stellen in den Beispieläußerungen:

    |Äußerung|Entität „Job“|
    |:--|:--|
    |Is there any work in databases? (Gibt es offene Stellen im Datenbankbereich?)|Datenbanken|
    |Ich möchte mich beruflich neu orientieren und suche nach Aufgaben in der Buchhaltung.|accounting (Buchhaltung)|
    |What positions are available for senior engineers? (Welche Positionen sind für Oberingenieure verfügbar?)|senior engineers (Oberingenieure)|

    Es gibt noch andere Beispieläußerungen, diese enthalten jedoch keine Wörter für Stellen.

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt 

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
    
    LUIS hat die richtige Absicht, **ApplyForJob**, gefunden und die richtige Entität extrahiert, **Stelle**, mit dem Wert `programmer`.


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

## <a name="to-boost-signal-add-phrase-list"></a>Begriffsliste hinzufügen, um das Signal zu verstärken

Öffnen Sie die Datei [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) aus dem GitHub-Repository „LUIS-Samples“. Die Liste enthält über 1.000 stellenbezogene Wörter und Begriffe. Suchen Sie in der Liste nach Stellenwörtern, die für Sie relevant sind. Sollten die gewünschten Wörter oder Begriffe nicht in der Liste enthalten sein, fügen Sie eigene Wörter/Begriffe hinzu.

1. Klicken Sie im Abschnitt **Build** der LUIS-App im Menü **Improve app performance** (App-Leistung verbessern) auf **Phrase lists** (Begriffslisten).

2. Klicken Sie auf **Create new phrase list** (Neue Begriffsliste erstellen). 

3. Nennen Sie die Begriffsliste `Job`, und kopieren Sie die Liste aus „jobs-phrase-list.csv“ in das Textfeld **Werte**. Drücken Sie die EINGABETASTE. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Screenshot: Popupdialogfeld zum Erstellen einer neuen Begriffsliste")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Wenn Sie der Begriffsliste weitere Wörter hinzufügen möchten, können Sie sich die verwandten Werte (**Related Values**) ansehen und alle Wörter hinzufügen, die für Sie relevant sind. 

4. Klicken Sie auf **Speichern**, um die Begriffsliste zu aktivieren.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Screenshot des Popupdialogfelds zum Erstellen einer neuen Begriffsliste mit Wörtern im Feld für die Begriffslistenwerte")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. [Trainieren](#train) und [veröffentlichen](#publish) Sie die App erneut, um die Begriffsliste zu verwenden.

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

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial verwendet die Personalwesen-App eine maschinell gelernte Simple-Entität, um Stellenbezeichnungen in Äußerungen zu finden. Da Stellenbezeichnungen eine so große Bandbreite an Wörtern und Formulierungen aufweisen können, benötigte die App eine Begriffsliste, um die Wörter zu verstärken. 

> [!div class="nextstepaction"]
> [Hinzufügen einer vorgefertigten keyPhrase-Entität](luis-quickstart-intent-and-key-phrase.md)