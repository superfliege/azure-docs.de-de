---
title: 'Schnellstart: Ändern des Modells und Trainieren der LUIS-App mit PHP – Azure Cognitive Services | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung für PHP fügen Sie einer Home Automation-App Beispieläußerungen hinzu, und Sie trainieren die App. Bei Beispieläußerungen handelt es sich um Konversationstext von Benutzern, der einer Absicht zugeordnet ist. Indem Sie Beispieläußerungen für Absichten bereitstellen, teilen Sie LUIS mit, welche Arten des vom Benutzer angegebenen Texts zu welcher Absicht gehören.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: ae2d3624cb3f8314a613af356730fb1d8b5d4b29
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43770047"
---
# <a name="quickstart-change-model-using-php"></a>Schnellstart: Ändern des Modells mit PHP 

[!include[Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!include[Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Aktuelle [**PHP**](http://php.net/)-Version
* Sicherstellen, dass OpenSSL als Abhängigkeit für PHP verfügbar ist  

[!include[Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!include[Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Erstellen des Schnellstartcodes 

Fügen Sie die Abhängigkeiten der Datei namens `add-utterances.php` hinzu.

   [!code-php[PHP and LUIS Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/php/add-utterances.php?range=1-22 "PHP and LUIS Dependencies")]

Fügen Sie die für den Trainingsstatus verwendete GET-Anforderung hinzu:

   [!code-php[SendGet](~/samples-luis/documentation-samples/quickstarts/change-model/php/add-utterances.php?range=24-43 "SendGet")]

Fügen Sie die POST-Anforderung zum Erstellen von Äußerungen oder zum Starten des Trainings hinzu: 

   [!code-php[SendPost](~/samples-luis/documentation-samples/quickstarts/change-model/php/add-utterances.php?range=45-65 "SendPost")]

Fügen Sie die Funktion `AddUtterances` hinzu:

   [!code-php[AddUtterances method](~/samples-luis/documentation-samples/quickstarts/change-model/php/add-utterances.php?range=67-72 "AddUtterances method")]


Fügen Sie die Funktion `Train` hinzu: 

   [!code-php[Train](~/samples-luis/documentation-samples/quickstarts/change-model/php/add-utterances.php?range=74-81 "Train")]

Fügen Sie die Funktion `Status` hinzu:

   [!code-php[Status](~/samples-luis/documentation-samples/quickstarts/change-model/php/add-utterances.php?range=83-87 "Status")]

Fügen Sie zum Verwalten der Befehlszeilenargumente den Hauptcodeblock hinzu.

   [!code-php[Main code](~/samples-luis/documentation-samples/quickstarts/change-model/php/add-utterances.php?range=89-93 "Main code")]

## <a name="run-code"></a>Ausführen des Codes

Führen Sie die Anwendung über eine Befehlszeile mit PHP aus.

### <a name="add-an-utterance-from-the-command-line"></a>Hinzufügen einer Äußerung über die Befehlszeile

Führen Sie die Anwendung über eine Befehlszeile mit PHP aus.

Durch den Aufruf von `add-utterances.php` werden die Äußerungen hinzugefügt und trainiert sowie der Trainingsstatus abgerufen.

```CMD
> php add-utterances.php 
```

Der folgende JSON-Code wird beim Aufruf der API zum Hinzufügen von Äußerungen zurückgegeben. Das Feld `response` weist dieses Format für hinzugefügte Äußerungen auf. Für `hasError` ist „false“ festgelegt. Dadurch wird angegeben, dass die Äußerung hinzugefügt wurde.  

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


```JSON
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

Entfernen Sie nach dem Durchführen dieses Schnellstarts alle Dateien, die Sie im Rahmen der Schnellstartanleitung erstellt haben. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Programmgesteuertes Erstellen einer LUIS-App](luis-tutorial-node-import-utterances-csv.md)