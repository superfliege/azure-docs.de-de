---
title: 'Tutorial: Informationen zum Hinzufügen von Äußerungen zu einer LUIS-App mithilfe von C# | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine LUIS-App mithilfe von C# aufrufen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: v-geberr
ms.openlocfilehash: d9b3ca46cc635d961edadf3e3555f9656b6b5a1d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264242"
---
# <a name="tutorial-add-utterances-to-a-luis-app-using-c"></a>Tutorial: Hinzufügen von Äußerungen zu einer LUIS-App mithilfe von C# 
In diesem Tutorial schreiben Sie mithilfe der Erstellungs-APIs in C# ein Programm, um einer Absicht eine Äußerung hinzuzufügen.

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen eines Visual Studio-Konsolenprojekts 
> * Hinzufügen einer Methode zum Aufrufen der LUIS-API, um eine Äußerung hinzuzufügen und die App zu trainieren
> * Hinzufügen einer JSON-Datei mit Beispieläußerungen für die Absicht „BookFlight“ (Flug buchen)
> * Ausführen der Konsole und Anzeigen des Trainingsstatus für Äußerungen

Weitere Informationen finden Sie in der technischen Dokumentation für die APIs zum [Hinzufügen einer Beispieläußerung zu einer Absicht](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [Trainieren](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) und Abrufen des [Trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="prerequisites"></a>Voraussetzungen

* Aktuelle [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/) 
* LUIS-**[Erstellungsschlüssel](luis-concept-keys.md#authoring-key)**. Sie finden diesen Schlüssel auf der [LUIS](luis-reference-regions.md)-Website unter „Kontoeinstellungen“.
* Ihre vorhandene LUIS-[**Anwendungs-ID**](./luis-get-started-create-app.md). Die Anwendungs-ID wird auf dem Anwendungsdashboard angezeigt. Die LUIS-Anwendung mit den in der Datei `utterances.json` verwendeten Absichten und Entitäten muss vor der Ausführung dieses Codes vorhanden sein. Mit dem Code in diesem Artikel werden die Absichten und Entitäten nicht erstellt. Er fügt die Äußerungen für vorhandene Absichten und Entitäten hinzu. 
* Die **Versions-ID** in der Anwendung, die die Äußerungen empfängt. Die Standard-ID lautet „0.1“.
* Erstellen einer neuen Projektdatei mit dem Namen `add-utterances.cs` in VSCode

> [!NOTE] 
> Die vollständige C#-Projektmappe, einschließlich einer `utterances.json`-Beispieldatei, steht im [Github-Repository mit **LUIS-Beispielen**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/csharp/) zur Verfügung.

## <a name="create-the-project-in-visual-studio"></a>Erstellen des Projekts in Visual Studio

Erstellen Sie in Visual Studio mithilfe von .Net Framework eine neue App für eine **klassische Windows-Desktopkonsole**. 

![Visual Studio-Projekttyp](./media/luis-quickstart-cs-add-utterance/vs-project-type.png)

## <a name="add-the-systemweb-dependency"></a>Hinzufügen der System.Web-Abhängigkeit

Im Visual Studio-Projekt ist **System.Web** erforderlich. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise** und dann auf **Verweis hinzufügen**.

![Hinzufügen des Verweises auf „System.web“](./media/luis-quickstart-cs-add-utterance/system.web.png)

## <a name="write-the-c-code"></a>Schreiben des C#-Codes
Die Datei **Program.cs** sollte wie folgt aussehen:

```CSharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp3
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}

```

Fügen Sie die Abhängigkeiten „System.IO“ und „System.Net.Http“ hinzu.

   [!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=1-6 "Add the dependencies")]


Fügen Sie die LUIS-IDs und Zeichenfolgen zur **Program**-Klasse hinzu.

   [!code-csharp[Add the LUIS IDs and strings](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=12-30&dedent=8 "Add the LUIS IDs and strings")]

Fügen Sie die JsonPrettyPrint-Methode hinzu.

   [!code-csharp[Add the JsonPrettyPrint method](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=32-92 "Add the JsonPrettyPrint method")]

Fügen Sie die GET-Anforderung zum Erstellen von Äußerungen oder zum Starten des Trainings hinzu: 

   [!code-csharp[SendGet](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=93-103 "SendGet")]


Fügen Sie die POST-Anforderung zum Erstellen von Äußerungen oder zum Starten des Trainings hinzu: 

   [!code-csharp[SendPost](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=104-115 "SendPost")]

Fügen Sie die Funktion `AddUtterances` hinzu:

   [!code-csharp[AddUtterances method](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=116-125 "AddUtterances method")]


Fügen Sie die Funktion `Train` hinzu: 

   [!code-csharp[Train](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=126-136 "Train")]

Fügen Sie die Funktion `Status` hinzu:

   [!code-csharp[Status](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=137-143 "Status")]

Fügen Sie zum Verwalten von Argumenten den Hauptcode hinzu.

   [!code-csharp[Main code](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=144-172 "Main code")]

## <a name="specify-utterances-to-add"></a>Angeben der hinzuzufügenden Äußerungen
Erstellen und bearbeiten Sie die Datei `utterances.json`, um das **Array von Äußerungen** anzugeben, das Sie der LUIS-App hinzufügen möchten. Die Absicht und Entitäten **müssen** bereits in der LUIS-App enthalten sein.

> [!NOTE]
> Die LUIS-Anwendung mit den in der Datei `utterances.json` verwendeten Absichten und Entitäten muss vor der Ausführung des Codes in `add-utterances.cs` vorhanden sein. Mit dem Code in diesem Artikel werden die Absichten und Entitäten nicht erstellt. Er fügt nur die Äußerungen für vorhandene Absichten und Entitäten hinzu.

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

## <a name="mark-the-json-file-as-content"></a>Markieren der JSON-Datei als Inhalt
Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf `utterances.json`, und wählen Sie **Eigenschaften** aus. Markieren Sie im Fenster mit den Eigenschaften **Buildaktion** von `Content` und **In Ausgabeverzeichnis kopieren** von `Copy Always`.  

![Markieren der JSON-Datei als Inhalt](./media/luis-quickstart-cs-add-utterance/content-properties.png)

## <a name="add-an-utterance-from-the-command-line"></a>Hinzufügen einer Äußerung über die Befehlszeile

Erstellen Sie die Anwendung mit C# über eine Befehlszeile im Verzeichnis „/bin/Debug“, und führen Sie sie aus. Stellen Sie sicher, dass sich die Datei „utterances.json“ ebenfalls in diesem Verzeichnis befindet.

Wenn Sie „add-utterances.cs“ nur mit „utterance.json“ als Argument aufrufen, wird LUIS für die neuen Äußerungen zwar hinzugefügt, aber nicht trainiert.
````
ConsoleApp\bin\Debug> ConsoleApp1.exe utterances.json
````

Die folgende Befehlszeile zeigt das Ergebnis des Aufrufs der API zum Hinzufügen von Äußerungen an. Das Feld `response` weist dieses Format für hinzugefügte Äußerungen auf. Für `hasError` ist „false“ festgelegt. Dadurch wird angegeben, dass die Äußerung hinzugefügt wurde.  

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
ConsoleApp\bin\Debug> ConsoleApp1.exe -train utterances.json
````

> [!NOTE]
> Doppelte Äußerungen werden nicht erneut hinzugefügt, verursachen jedoch keinen Fehler. Das Feld `response` enthält die ID der ursprünglichen Äußerung.

Die folgende JSON-Ausgabe enthält das Ergebnis einer erfolgreichen Anforderung zum Trainieren:

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
Rufen Sie die App mit dem Argument `-status` auf, um den Trainingsstatus zu überprüfen und Statusdetails anzuzeigen.

````
ConsoleApp\bin\Debug> ConsoleApp1.exe -status
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
