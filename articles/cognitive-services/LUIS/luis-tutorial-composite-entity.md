---
title: 'Tutorial 6: Extrahieren von zusammengesetzten Daten mit der zusammengesetzten LUIS-Entität'
titleSuffix: Azure Cognitive Services
description: Hinzufügen einer zusammengesetzten Entität, um extrahierte Daten verschiedenen Typs in einer einzelnen enthaltenden Entität zu bündeln. Durch Bündeln der Daten kann die Clientanwendung aufeinander bezogene Daten verschiedener Datentypen leicht extrahieren.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 99e0b22b663f6edab9646111b390186a6f89a90f
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035180"
---
# <a name="tutorial-6-group-and-extract-related-data"></a>Tutorial 6: Gruppieren und Extrahieren zugehöriger Daten
In diesem Tutorial fügen Sie eine zusammengesetzte Entität hinzu, um extrahierte Daten verschiedenen Typs in einer einzelnen enthaltenden Entität zu bündeln. Durch Bündeln der Daten kann die Clientanwendung aufeinander bezogene Daten verschiedener Datentypen leicht extrahieren.

Zusammengesetzte Entitäten dienen dazu, verknüpfte Entitäten in einer Entität der übergeordneten Kategorie zu gruppieren. Die Informationen sind vor dem Erstellen einer zusammengesetzten Entität jeweils separate Entitäten. Sie ähnelt einer hierarchischen Entität, kann aber verschiedene Typen von Entitäten enthalten. 

Die zusammengesetzte Entität ist eine gute Wahl für diese Art von Daten, da für die Daten Folgendes gilt:

* Sind aufeinander bezogen. 
* Verwenden eine Vielzahl von Entitätstypen.
* Müssen gruppiert und von einer Client-App als eine Informationseinheit verarbeitet werden.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Verwenden der vorhandenen Tutorial-App
> * Zusammengesetzte Entität hinzufügen 
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten von einem Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Verwenden der vorhandenen App
Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **Personalwesen** fort. 

Wenn Sie nicht über die Personalwesen-App aus dem vorhergehenden Tutorial verfügen, befolgen Sie diese Schritte:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-hier-HumanResources.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und benennen Sie sie `composite`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf der Name keine Zeichen enthalten, die in einer URL ungültig sind.


## <a name="composite-entity"></a>Entität vom Typ „Composite“
Erstellen Sie eine zusammengesetzte Entität, wenn die einzelnen Entitäten logisch gruppiert werden können und diese logische Gruppierung hilfreich für die Clientanwendung ist. 

In dieser App ist der Name des Mitarbeiters in der Listenentität **Employee** definiert, und er umfasst Synonyme zum Namen, E-Mail-Adresse, die Durchwahl der Firmentelefonnummer, die Mobiltelefonnummer und die US-amerikanische Steuernummer. 

Die Absicht **MoveEmployee** weist Beispieläußerungen auf, um anzufordern, dass ein Mitarbeiter von einem Gebäude oder Büro in ein anderes verlegt wird. Gebäudenamen enthalten Buchstaben („A“, „B“ usw.), während Büros Nummern („1234“, „13245“) enthalten. 

Zu Beispieläußerungen in der Absicht **MoveEmployee** zählen Folgende:

|Beispiele für Äußerungen|
|--|
|Move John W . Smith to a-2345|
|shift x12345 to h-1234 tomorrow (x12345 morgen nach h-1234 verlagern)|
 
Die Anforderung zur Verlegung sollte den Mitarbeiter (mit sämtlichen Synonymen) und den endgültigen Gebäude- bzw. Bürostandort umfassen. Die Anforderung kann zudem das ursprüngliche Büro sowie ein Datum umfassen, an dem die Verlegung geschehen soll. 

Die aus dem Endpunkt extrahierten Daten sollten diese Informationen enthalten und in einer zusammengesetzten Entität namens `RequestEmployeeMove` zurückgeben:

```JSON
"compositeEntities": [
  {
    "parentType": "RequestEmployeeMove",
    "value": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
    "children": [
      {
        "type": "builtin.datetimeV2.datetime",
        "value": "march 3 2 p.m"
      },
      {
        "type": "Locations::Destination",
        "value": "z - 2345"
      },
      {
        "type": "Employee",
        "value": "jill jones"
      },
      {
        "type": "Locations::Origin",
        "value": "a - 1234"
      }
    ]
  }
]
```

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie auf der Seite **Absichten** die Option **MoveEmployee** aus. 

3. Wählen Sie das Lupensymbol auf der Symbolleiste aus, um die Liste von Äußerungen zu filtern. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Screenshot der LUIS-App zur Absicht „MoveEmployee“ mit hervorgehobenem Lupensymbol")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Geben Sie `tomorrow` in das Filtertextfeld ein, um nach der Äußerung `shift x12345 to h-1234 tomorrow` zu suchen.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Screenshot der LUIS-App zur Absicht „MoveEmployee“ mit hervorgehobenem Filter „tomorrow“")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Eine andere Methode besteht darin, die Entität nach „datetimeV2“ zu filtern und dabei **Entitätsfilter** und dann **datetimeV2** aus der Liste auszuwählen. 

5. Wählen Sie als erste Entität `Employee` aus, und klicken Sie dann in der Liste des Popupmenüs auf **Zusammengesetzte Entität umschließen**. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "Screenshot der LUIS-App zur Absicht „MoveEmployee“ mit hervorgehobener Auswahl der ersten Entität in der zusammengesetzten Entität")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. Wählen Sie dann die letzte Entität `datetimeV2` in der Äußerung aus. Die markierten Wörter werden grün unterstrichen, was auf eine zusammengesetzte Entität hinweist. Geben Sie im Popupmenü den zusammengesetzten Namen `RequestEmployeeMove` ein, und drücken Sie die EINGABETASTE. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Screenshot der LUIS-App zur Absicht „MoveEmployee“ mit hervorgehobener Auswahl der letzten Entität in der zusammengesetzten Entität und Hervorhebung der Schaltfläche „Neue zusammengesetzte Entität erstellen“")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. Unter **Welche Arten von Entitäten möchten Sie erstellen?** sind fast alle Pflichtfelder in der Liste aufgeführt. Nur der Ursprungsort fehlt. Klicken Sie auf **Untergeordnete Entität hinzufügen**, auf **Locations::Origin** in der Liste der vorhandenen Entitäten und anschließend auf **Fertig**. 

    Beachten Sie, dass der zusammengesetzten Entität die vordefinierte Entität „Zahl“ hinzugefügt wurde. Wenn eine vordefinierte Entität zwischen dem Anfangs- und dem Endtoken einer zusammengesetzten Entität auftreten kann, muss die zusammengesetzte Entität diese vordefinierten Entitäten enthalten. Wenn die vordefinierten Entitäten nicht enthalten sind, wird die zusammengesetzte Entität nicht ordnungsgemäß vorhergesagt, wohl aber die Einzelelemente.

    ![Screenshot der LUIS-App zur Absicht „MoveEmployee“ mit Hinzufügung einer weiteren Entität im Popupfenster](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Klicken Sie auf der Symbolleiste auf das Lupensymbol, um den Filter zu entfernen. 

9. Entfernen Sie das Wort `tomorrow` aus dem Filter, so dass Sie wieder alle Beispieläußerungen sehen können. 

## <a name="label-example-utterances-with-composite-entity"></a>Bezeichnen von Beispieläußerungen mit zusammengesetzter Entität


1. Wählen Sie in jeder Beispieläußerung die Entität ganz links aus, die sich in der zusammengesetzten Entität befinden sollte. Klicken Sie dann auf **Zusammengesetzte Entität umschließen**.

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "Screenshot der LUIS-App zur Absicht „MoveEmployee“ mit hervorgehobener Auswahl der ersten Entität in der zusammengesetzten Entität")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Wählen Sie das letzte Wort in der zusammengesetzten Entität und im Popupmenü dann **RequestEmployeeMove** aus. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Screenshot der LUIS-App zur Absicht „MoveEmployee“ mit hervorgehobener Auswahl der letzten Entität in der zusammengesetzten Entität")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Überprüfen Sie, ob alle Äußerungen in der Absicht mit der zusammengesetzten Entität bezeichnet sind. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Screenshot der LUIS-App zu „MoveEmployee“ mit allen bezeichneten Äußerungen")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL `Move Jill Jones from a-1234 to z-2345 on March 3 2 p.m.` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (query) der Äußerung). 

    Da mit diesem Test überprüft werden soll, ob die zusammengesetzte Entität korrekt extrahiert wurde, kann ein Test entweder eine vorhandene Beispieläußerung oder eine neue Äußerung enthalten. Ein guter Test beinhaltet alle untergeordneten Entitäten in der zusammengesetzten Entität.

    ```JSON
    {
      "query": "Move Jill Jones from a-1234 to z-2345 on March 3  2 p.m",
      "topScoringIntent": {
        "intent": "MoveEmployee",
        "score": 0.9959525
      },
      "intents": [
        {
          "intent": "MoveEmployee",
          "score": 0.9959525
        },
        {
          "intent": "GetJobInformation",
          "score": 0.009858314
        },
        {
          "intent": "ApplyForJob",
          "score": 0.00728598563
        },
        {
          "intent": "FindForm",
          "score": 0.0058053555
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.005371796
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00266987388
        },
        {
          "intent": "None",
          "score": 0.00123299169
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00116407464
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00102653319
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.0006628214
        }
      ],
      "entities": [
        {
          "entity": "march 3 2 p.m",
          "type": "builtin.datetimeV2.datetime",
          "startIndex": 41,
          "endIndex": 54,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03T14",
                "type": "datetime",
                "value": "2018-03-03 14:00:00"
              },
              {
                "timex": "XXXX-03-03T14",
                "type": "datetime",
                "value": "2019-03-03 14:00:00"
              }
            ]
          }
        },
        {
          "entity": "jill jones",
          "type": "Employee",
          "startIndex": 5,
          "endIndex": 14,
          "resolution": {
            "values": [
              "Employee-45612"
            ]
          }
        },
        {
          "entity": "z - 2345",
          "type": "Locations::Destination",
          "startIndex": 31,
          "endIndex": 36,
          "score": 0.9690751
        },
        {
          "entity": "a - 1234",
          "type": "Locations::Origin",
          "startIndex": 21,
          "endIndex": 26,
          "score": 0.9713137
        },
        {
          "entity": "-1234",
          "type": "builtin.number",
          "startIndex": 22,
          "endIndex": 26,
          "resolution": {
            "value": "-1234"
          }
        },
        {
          "entity": "-2345",
          "type": "builtin.number",
          "startIndex": 32,
          "endIndex": 36,
          "resolution": {
            "value": "-2345"
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 47,
          "endIndex": 47,
          "resolution": {
            "value": "3"
          }
        },
        {
          "entity": "2",
          "type": "builtin.number",
          "startIndex": 50,
          "endIndex": 50,
          "resolution": {
            "value": "2"
          }
        },
        {
          "entity": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
          "type": "RequestEmployeeMove",
          "startIndex": 5,
          "endIndex": 54,
          "score": 0.4027723
        }
      ],
      "compositeEntities": [
        {
          "parentType": "RequestEmployeeMove",
          "value": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
          "children": [
            {
              "type": "builtin.datetimeV2.datetime",
              "value": "march 3 2 p.m"
            },
            {
              "type": "Locations::Destination",
              "value": "z - 2345"
            },
            {
              "type": "Employee",
              "value": "jill jones"
            },
            {
              "type": "Locations::Origin",
              "value": "a - 1234"
            }
          ]
        }
      ]
    }
    ```

  Diese Äußerung gibt ein Array von zusammengesetzten Entitäten zurück. Jede Entität ist mit einem Typ und einem Wert versehen. Um die Genauigkeit für jede untergeordnete Entität zu erhöhen, verwenden Sie die Kombination aus Typ und Wert des Arrays von zusammengesetzten Elementen, um nach dem entsprechenden Element im Entitätenarray zu suchen.  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine zusammengesetzte Entität zur Verkapselung vorhandener Entitäten verwendet. Dadurch kann die Clientanwendung eine Gruppe auf einander bezogener Daten in verschiedenen Datentypen finden, um die Unterhaltung fortzusetzen. Eine Clientanwendung für diese Personalwesen-App könnte fragen, an welchem Tag und um welche Uhrzeit die Verlegung beginnen und enden muss. Sie könnte auch nach anderen Logistikdetails der Verlegung fragen, wie etwa einem physischen Telefon. 

> [!div class="nextstepaction"] 
> [Erfahren Sie, wie eine einfache Entität mit einer Ausdrucksliste hinzugefügt wird.](luis-quickstart-primary-and-secondary-data.md)  