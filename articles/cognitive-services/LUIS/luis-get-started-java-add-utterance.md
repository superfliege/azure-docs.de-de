---
title: 'Tutorial: Informationen zum Hinzufügen von Äußerungen zu einer LUIS-App mithilfe von Java | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine LUIS-App mithilfe von Java aufrufen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 5b9c7b90ca96b23fbabfeb1e1d06e4124e65a0dc
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266038"
---
# <a name="tutorial-add-utterances-to-app-using-java"></a>Tutorial: Hinzufügen von Äußerungen zu einer App mithilfe von Java 
In diesem Tutorial schreiben Sie mithilfe der Erstellungs-APIs in Java ein Programm, um einer Absicht eine Äußerung hinzuzufügen.

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen eines Visual Studio-Konsolenprojekts 
> * Hinzufügen einer Methode zum Aufrufen der LUIS-API, um eine Äußerung hinzuzufügen und die App zu trainieren
> * Hinzufügen einer JSON-Datei mit Beispieläußerungen für die Absicht „BookFlight“ (Flug buchen)
> * Ausführen der Konsole und Anzeigen des Trainingsstatus für Äußerungen

Weitere Informationen finden Sie in der technischen Dokumentation für die APIs zum [Hinzufügen einer Beispieläußerung zu einer Absicht](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [Trainieren](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) und Abrufen des [Trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="prerequisites"></a>Voraussetzungen

* Aktuelle [**Java-/JDK-Version**](http://www.oracle.com/technetwork/java/javase/downloads/index.html) von Oracle
* [GSON-JSON-Bibliothek von Google](https://github.com/google/gson)
* LUIS-**[Erstellungsschlüssel](luis-concept-keys.md#authoring-key)**. Sie finden diesen Schlüssel auf der [LUIS](luis-reference-regions.md)-Website unter „Kontoeinstellungen“.
* Ihre vorhandene LUIS-[**Anwendungs-ID**](./luis-get-started-create-app.md). Die Anwendungs-ID wird auf dem Anwendungsdashboard angezeigt. Die LUIS-Anwendung mit den in der Datei `utterances.json` verwendeten Absichten und Entitäten muss vor der Ausführung des Codes in `AddUtterances.java` vorhanden sein. Mit dem Code in diesem Artikel werden die Absichten und Entitäten nicht erstellt. Er fügt nur die Äußerungen für vorhandene Absichten und Entitäten hinzu. 
* Die **Versions-ID** in der Anwendung, die die Äußerungen empfängt. Die Standard-ID lautet „0.1“.
* Erstellen Sie eine neue Textdatei mit dem Namen `AddUtterances.java`.

> [!NOTE] 
> Die vollständige `add-utterances.cs`-Datei und eine `utterances.json`-Beispieldatei stehen im [Github-Repository mit **LUIS-Beispielen**](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/authoring-api-samples/java) zur Verfügung.


## <a name="write-the-java-code"></a>Schreiben des Java-Codes

Fügen Sie die Java-Abhängigkeiten zur Datei hinzu:

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=31-34 "Java Dependencies")]

Erstellen Sie die Klasse `AddUtterances`:

```Java
public class AddUtterances {
    // Insert code here
}
```

Diese Klasse enthält alle folgenden Codeausschnitte.

Fügen Sie die LUIS-Konstanten zur Klasse hinzu. Kopieren Sie den folgenden Code, und fügen Sie Ihren Erstellungsschlüssel, die Anwendungs-ID und die Versions-ID ein.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=41-53 "LUIS-based IDs")]

Fügen Sie die Methode hinzu, um die LUIS-API aufzurufen: 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=55-178 "HTTP request to LUIS")]

Fügen Sie die Methode für die HTTP-Antwort aus der LUIS-API hinzu:

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=180-226 "HTTP response from LUIS")]


Fügen Sie die Ausnahmebehandlung hinzu: 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=228-256 "Exception Handling")]

Fügen Sie die Ausgabebehandlung hinzu:

   [!code-java[Add output handling](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=258-267 "Add configuration information for adding utterance")]

Fügen Sie die main-Funktion hinzu:

   [!code-java[Add main function](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=269-345 "Add main function")]


## <a name="specify-utterances-to-add"></a>Angeben der hinzuzufügenden Äußerungen
Erstellen und bearbeiten Sie die Datei `utterances.json`, um das **Array von Äußerungen** anzugeben, das Sie der LUIS-App hinzufügen möchten. Die Absicht und Entitäten **müssen** bereits in der LUIS-App enthalten sein.

> [!NOTE]
> Die LUIS-Anwendung mit den in der Datei `utterances.json` verwendeten Absichten und Entitäten muss vor der Ausführung des Codes in `AddUtterances.java` vorhanden sein. Mit dem Code in diesem Artikel werden die Absichten und Entitäten nicht erstellt. Er fügt nur die Äußerungen für vorhandene Absichten und Entitäten hinzu.

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

Kompilieren Sie „AddUtterance“ mit den Abhängigkeiten:

```
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

Wenn Sie `AddUtterance` ohne Argumente aufrufen, werden die LUIS-Äußerungen zur App hinzugefügt, ohne die App zu trainieren.
````
> java -classpath .;gson-2.8.2.jar AddUtterances
````

Dieses Beispiel erstellt eine Datei mit dem Objekt `results.json`, das die Ergebnisse des Aufrufs der API zum Hinzufügen von Äußerungen enthält. Das Feld `response` weist dieses Format für hinzugefügte Äußerungen auf. Für `hasError` ist „false“ festgelegt. Dadurch wird angegeben, dass die Äußerung hinzugefügt wurde.  

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
Rufen Sie `add-utterance` mit dem Argument `-train` auf, um eine Anforderung zum Trainieren zu senden. 

````
> java -classpath .;gson-2.8.2.jar AddUtterances -train
````

> [!NOTE]
> Doppelte Äußerungen werden nicht erneut hinzugefügt, verursachen jedoch keinen Fehler. Das Feld `response` enthält die ID der ursprünglichen Äußerung.

Beim Aufrufen der App mit dem Argument `-train` wird die Datei `training-results.json` erstellt, die angibt, dass die Anforderung zum Trainieren der LUIS-App in die Warteschlange eingereiht wurde. 

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
Rufen Sie die App mit dem Argument `-status` auf, um den Trainingsstatus zu überprüfen und Statusdetails in eine Datei zu schreiben.

````
> java -classpath .;gson-2.8.2.jar AddUtterances -status
````

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie das Tutorial abgeschlossen haben, entfernen Sie Visual Studio und die Konsolenanwendung, wenn Sie sie nicht mehr benötigen. 

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"] 
> [Programmgesteuertes Erstellen einer LUIS-App](luis-tutorial-node-import-utterances-csv.md) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website