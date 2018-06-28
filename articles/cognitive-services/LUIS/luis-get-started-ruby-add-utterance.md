---
title: 'Tutorial: Informationen zum Hinzufügen von Äußerungen zu einer LUIS-App mithilfe von Ruby | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine LUIS-App mithilfe von Ruby aufrufen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 7a470fd551a58978e6f2be0450a2e2a6cd471fc4
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266055"
---
# <a name="tutorial-add-utterances-to-app-using-ruby"></a>Tutorial: Hinzufügen von Äußerungen zu einer App mithilfe von Ruby 
In diesem Tutorial schreiben Sie mithilfe der Erstellungs-APIs in Ruby ein Programm, um einer Absicht eine Äußerung hinzuzufügen.

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen eines Visual Studio-Konsolenprojekts 
> * Hinzufügen einer Methode zum Aufrufen der LUIS-API, um eine Äußerung hinzuzufügen und die App zu trainieren
> * Hinzufügen einer JSON-Datei mit Beispieläußerungen für die Absicht „BookFlight“ (Flug buchen)
> * Ausführen der Konsole und Anzeigen des Trainingsstatus für Äußerungen

Weitere Informationen finden Sie in der technischen Dokumentation für die APIs zum [Hinzufügen einer Beispieläußerung zu einer Absicht](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [Trainieren](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) und Abrufen des [Trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="prerequisites"></a>Voraussetzungen

* [Ruby](http://rubyinstaller.org/) 
* LUIS-**[Erstellungsschlüssel](luis-concept-keys.md#authoring-key)**. Sie finden diesen Schlüssel auf der [LUIS](luis-reference-regions.md)-Website unter „Kontoeinstellungen“.
* Ihre vorhandene LUIS-[**Anwendungs-ID**](./luis-get-started-create-app.md). Die Anwendungs-ID wird auf dem Anwendungsdashboard angezeigt. Die LUIS-Anwendung mit den in der Datei `utterances.json` verwendeten Absichten und Entitäten muss vor der Ausführung des Codes in `add-utterances.rb` vorhanden sein. Mit dem Code in diesem Artikel werden die Absichten und Entitäten nicht erstellt. Er fügt nur die Äußerungen für vorhandene Absichten und Entitäten hinzu. 
* Die **Versions-ID** in der Anwendung, die die Äußerungen empfängt. Die Standard-ID lautet „0.1“.
* Erstellen einer neuen Projektdatei mit dem Namen `add-utterances.rb` in VSCode

> [!NOTE] 
> Die vollständige `add-utterances.cs`-Datei und eine `utterances.json`-Beispieldatei stehen im [Github-Repository mit **LUIS-Beispielen**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/ruby/) zur Verfügung.


## <a name="write-the-ruby-code"></a>Schreiben des Ruby-Codes

Fügen Sie die Abhängigkeiten zur Datei hinzu:

   [!code-ruby[Ruby and LUIS Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=1-28 "Ruby and LUIS Dependencies")]

Fügen Sie die für den Trainingsstatus verwendete GET-Anforderung hinzu:

   [!code-ruby[SendGet](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=30-40 "SendGet")]

Fügen Sie die POST-Anforderung zum Erstellen von Äußerungen oder zum Starten des Trainings hinzu: 

   [!code-ruby[SendPost](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=42-54 "SendPost")]

Fügen Sie die Funktion `AddUtterances` hinzu:

   [!code-ruby[AddUtterances method](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=56-61 "AddUtterances method")]


Fügen Sie die Funktion `Train` hinzu: 

   [!code-ruby[Train](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=63-69 "Train")]

Fügen Sie die Funktion `Status` hinzu:

   [!code-ruby[Status](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=71-75 "Status")]

Fügen Sie zum Verwalten von Argumenten den Hauptcode hinzu.

   [!code-ruby[Main code](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=77-93 "Main code")]

## <a name="specify-utterances-to-add"></a>Angeben der hinzuzufügenden Äußerungen
Erstellen und bearbeiten Sie die Datei `utterances.json`, um das **Array von Äußerungen** anzugeben, das Sie der LUIS-App hinzufügen möchten. Die Absicht und Entitäten **müssen** bereits in der LUIS-App enthalten sein.

> [!NOTE]
> Die LUIS-Anwendung mit den in der Datei `utterances.json` verwendeten Absichten und Entitäten muss vor der Ausführung des Codes in `add-utterances.rb` vorhanden sein. Mit dem Code in diesem Artikel werden die Absichten und Entitäten nicht erstellt. Er fügt nur die Äußerungen für vorhandene Absichten und Entitäten hinzu.

Das Feld `text` enthält den Text der Äußerung. Das Feld `intentName` muss dem Namen einer Absicht in der LUIS-App entsprechen. `entityLabels` ist ein Pflichtfeld. Wenn Sie keine Bezeichnungen für Entitäten angeben möchten, geben Sie eine leere Liste an, wie im folgenden Beispiel gezeigt.

Ist die entityLabels-Liste nicht leer, müssen `startCharIndex` und `endCharIndex` die Entität markieren, auf die im Feld `entityName` verwiesen wird. Bei beiden Indizes beginnt die Zählung bei 0. Das bedeutet, dass sich 6 im oberen Beispiel auf das „S“ von Seattle und nicht die Leerstelle vor dem großen S bezieht.

```json
[
    {
        "text": "go to Seattle",
        "intentName": "BookFlight",
        "entityLabels": [
            {
                "entityName": "Location::LocationTo",
                "startCharIndex": 6,
                "endCharIndex": 12
            }
        ]
    },
    {
        "text": "book a flight",
        "intentName": "BookFlight",
        "entityLabels": []
    }
]
```

## <a name="add-an-utterance-from-the-command-line"></a>Hinzufügen einer Äußerung über die Befehlszeile

Führen Sie die Anwendung über eine Befehlszeile mit Ruby aus.

Wenn Sie `add-utterances.rb` nur mit „utterance.json“ als Argument aufrufen, wird LUIS für die neuen Äußerungen zwar hinzugefügt, aber nicht trainiert.
````
> ruby add-utterances.rb ./utterances.json
````

Das folgende Ergebnis wird beim Aufrufen der API zum Hinzufügen von Äußerungen angezeigt. Das Feld `response` weist dieses Format für hinzugefügte Äußerungen auf. Für `hasError` ist „false“ festgelegt. Dadurch wird angegeben, dass die Äußerung hinzugefügt wurde.  

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

## <a name="add-an-utterance-and-train-from-the-command-line"></a>Hinzufügen einer Äußerung und Trainieren über die Befehlszeile
Rufen Sie „add-utterance“ mit dem Argument `-train` auf, um eine Anforderung zum Trainieren zu senden.

````
> ruby add-utterances.rb ./utterances.json -train
````

> [!NOTE]
> Doppelte Äußerungen werden nicht erneut hinzugefügt, verursachen jedoch keinen Fehler. Das Feld `response` enthält die ID der ursprünglichen Äußerung.

Nachfolgend sehen Sie das Ergebnis einer erfolgreichen Anforderung zum Trainieren:
```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

Nachdem die Anforderung zum Trainieren in die Warteschlange eingereiht wurde, dauert es einen Moment, bis das Training abgeschlossen ist.

## <a name="get-training-status-from-the-command-line"></a>Abrufen des Trainingsstatus über die Befehlszeile
Rufen Sie das Beispiel mit dem Argument `-status` auf, um den Trainingsstatus zu überprüfen.

````
> ruby add-utterances.rb ./utterances.json -status
````

```
Requested training status.
[
   {
      "modelId": "eb2f117c-e10a-463e-90ea-1a0176660acc",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c1bdfbfc-e110-402e-b0cc-2af4112289fb",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "863023ec-2c96-4d68-9c44-34c1cbde8bc9",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "82702162-73ba-4ae9-a6f6-517b5244c555",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "37121f4c-4853-467f-a9f3-6dfc8cad2763",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "de421482-753e-42f5-a765-ad0a60f50d69",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "80f58a45-86f2-4e18-be3d-b60a2c88312e",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c9eb9772-3b18-4d5f-a1e6-e0c31f91b390",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "2afec2ff-7c01-4423-bb0e-e5f6935afae8",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "95a81c87-0d7b-4251-8e07-f28d180886a1",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   }
]
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie das Tutorial abgeschlossen haben, entfernen Sie Visual Studio und die Konsolenanwendung, wenn Sie sie nicht mehr benötigen.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Programmgesteuertes Erstellen einer LUIS-App](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
