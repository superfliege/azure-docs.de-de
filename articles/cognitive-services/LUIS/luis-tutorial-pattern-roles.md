---
title: Tutorial zur Verwendung von Musterrollen zur Verbesserung von LUIS-Vorhersagen – Azure | Microsoft-Dokumentation
titleSuffix: Cognitive Services
description: In diesem Tutorial erfahren Sie, wie mithilfe von Musterrollen für kontextabhängige Entitäten LUIS-Vorhersagen verbessert werden.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 08/03/2018
ms.author: diberry
ms.openlocfilehash: 633bf16790437ba83e9a2a99f33a23fef9255364
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43090525"
---
# <a name="tutorial-improve-app-with-pattern-roles"></a>Tutorial: Verbessern der App mit Musterrollen

In diesem Tutorial erfahren Sie, wie Sie eine einfache Entität mit Rollen verwenden, die mit Mustern kombiniert werden, um die Absichts- und Entitätsvorhersage zu erhöhen.  Bei der Verwendung von Mustern sind weniger Beispieläußerungen für die Absicht erforderlich.

> [!div class="checklist"]
* Grundlegendes zu Musterrollen
* Verwenden von einfachen Entitäten mit Rollen 
* Erstellen von Mustern für Äußerungen mithilfe von einfachen Entitäten mit Rollen
* Überprüfen der Verbesserungen bei Vorhersagen durch Muster

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voraussetzungen
Falls Sie nicht über die Personalabteilungs-App aus dem Tutorial [Muster](luis-tutorial-pattern.md) verfügen, [importieren](luis-how-to-start-new-app.md#import-new-app) Sie den JSON-Code in eine neue App (auf der [LUIS-Website](luis-reference-regions.md#luis-website)). Die zu importierende App befindet sich im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-patterns-HumanResources-v2.json).

Wenn Sie die ursprüngliche Personal-App behalten möchten, klonen Sie die Version auf der Seite [Einstellungen](luis-how-to-manage-versions.md#clone-a-version), und nennen Sie sie `roles`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. 

## <a name="the-purpose-of-roles"></a>Der Zweck der Rollen
Der Zweck der Rollen besteht darin, kontextabhängige Entitäten in einer Äußerung zu extrahieren. In der Äußerung `Move new employee Robert Williams from Sacramento and San Francisco` sind die Werte für den Ursprungs- und Zielort miteinander verknüpft, und sie verwenden eine gemeinsame Sprache zur Bezeichnung der einzelnen Standorte. 

Bei der Verwendung von Mustern müssen alle Entitäten im Muster erkannt werden. _Erst dann_ stimmt die Äußerung mit dem Muster überein. 

Wenn Sie ein Muster erstellen, besteht der erste Schritt darin, die Absicht für das Muster auszuwählen. Durch Auswählen der Absicht wird die richtige Absicht immer mit einem hohem Ergebnis (in der Regel zwischen 99 und 100 %) zurückgegeben, wenn eine Musterübereinstimmung gefunden wird. 

### <a name="compare-hierarchical-entity-to-simple-entity-with-roles"></a>Vergleichen der hierarchischen Entität mit der einfachen Entität mit Rollen

Im [Tutorial: Hinzufügen einer hierarchischen Entität](luis-quickstart-intent-and-hier-entity.md) hat die Absicht **MoveEmployee** erkannt, wann ein bestehender Mitarbeiter von einem Gebäude oder Büro in ein anderes verlegt werden soll. Die Beispieläußerungen wiesen zwar Ursprungs- und Zielorte auf, jedoch keine Rollen. Stattdessen waren Ursprung und Ziel untergeordnete Elemente der hierarchischen Entität. 

In diesem Tutorial erkennt die Personalabteilungs-App Äußerungen zur Verlegung neuer Mitarbeiter von einem Ort in einen anderen. Diese beiden Arten von Äußerungen sind ähnlich, werden jedoch mit unterschiedlichen LUIS-Funktionen gelöst.

|Tutorial|Beispieläußerung|Ursprungs- und Zielorte|
|--|--|--|
|[Tutorial: Hinzufügen einer hierarchischen Entität (ohne Rollen)](luis-quickstart-intent-and-hier-entity.md)|mv Jill Jones from **a-2349** to **b-1298**|a-2349, b-1298|
|Dieses Tutorial (mit Rollen)|Move Billy Patterson from **Yuma** to **Denver**.|Yuma, Denver|

Die hierarchische Entität kann nicht im Muster verwendet werden, da in Mustern nur hierarchisch übergeordnete Elemente verwendet werden. Um die benannten Standorte des Ursprungs und des Ziels zurückzugeben, müssen Sie ein Muster verwenden.

### <a name="simple-entity-for-new-employee-name"></a>Einfache Entität für den Namen eines neuen Mitarbeiters
Der Name des neuen Mitarbeiters, Billy Patterson, ist noch nicht Teil der Listenentität **Employee**. Der Name des neuen Mitarbeiters wird zuerst extrahiert, um den Namen für die Erstellung der Unternehmensanmeldeinformationen an ein externes System zu senden. Nachdem die Unternehmensanmeldeinformationen erstellt wurden, werden die Mitarbeiteranmeldeinformationen der Listenentität **Employee** hinzugefügt.

Die Liste **Employee** wurde im [Tutorial: Hinzufügen einer Listenentität](luis-quickstart-intent-and-list-entity.md) erstellt.

Die Entität **NewEmployee** ist eine einfache Entität ohne Rollen. 

### <a name="simple-entity-with-roles-for-relocation-cities"></a>Einfache Entität mit Rollen für die Ortsverlagerung
Der neue Mitarbeiter und seine Familie müssen von dem aktuellen Ort in den Ort verlegt werden, in dem das fiktive Unternehmen ansässig ist. Da ein neuer Mitarbeiter von einem beliebigen Ort herkommen kann, müssen die Standorte ermittelt werden. Eine festgelegte Liste wie eine Listenentität würde nicht funktionieren, da nur die Orte in der Liste extrahiert werden würden.

Die den Ursprungs- und Zielorten zugeordneten Rollennamen müssen für alle Entitäten eindeutig sein. Eine einfache Möglichkeit, sicherzustellen, dass die Rollen eindeutig sind, besteht darin, diese mittels einer Benennungsstrategie der enthaltenen Entität zuzuordnen. Die Entität **NewEmployeeRelocation** ist eine einfache Entität mit zwei Rollen: **NewEmployeeReloOrigin** und **NewEmployeeReloDestination**.

### <a name="simple-entities-need-enough-examples-to-be-detected"></a>Erkennung von einfachen Entitäten nur durch Bereitstellung ausreichender Beispiele
Da die Beispieläußerung `Move new employee Robert Williams from Sacramento and San Francisco` nur über per Machine Learning trainierte Entitäten verfügt, ist es wichtig, genügend Beispieläußerungen für die Absicht bereitzustellen, damit die Entitäten erkannt werden.  

**Muster ermöglichen Ihnen, weniger Beispieläußerungen bereitzustellen. Doch wenn die Entitäten nicht erkannt werden, stimmt das Muster nicht überein.**

Wenn Sie Schwierigkeiten mit der Erkennung einfacher Entitäten haben, da es um einen Namen wie den eines Orts geht, sollten Sie eventuell eine Liste von Ausdrücken mit ähnlichen Werten hinzufügen. Dadurch wird die Erkennung des Ortsnamens verbessert, da der LUIS-App ein zusätzlicher Hinweis für den jeweiligen Wort- oder Ausdruckstyp zur Verfügung gestellt wird. Ausdruckslisten sind für Muster nur im Hinblick auf die Entitätserkennung nützlich, die für die Musterübereinstimmung erforderlich ist. 

## <a name="create-new-entities"></a>Erstellen von neuen Entitäten
1. Wählen Sie oben im Menü **Erstellen** aus.

2. Wählen Sie im linken Navigationsmenü die Option **Entitäten** aus. 

3. Wählen Sie **Neue Entität erstellen** aus.

4. Geben Sie im Popupfenster `NewEmployee` als **Simple**-Entität ein.

5. Wählen Sie **Neue Entität erstellen** aus.

6. Geben Sie im Popupfenster `NewEmployeeRelocation` als **Simple**-Entität ein.

7. Wählen Sie aus der Liste der Entitäten **NewEmployeeRelocation** aus. 

8. Geben Sie als erste Rolle `NewEmployeeReloOrigin` ein, und drücken Sie die EINGABETASTE.

9. Geben Sie als zweite Rolle `NewEmployeeReloDestination` ein, und drücken Sie die EINGABETASTE.

## <a name="create-new-intent"></a>Erstellen einer neuen Absicht
Die Bezeichnung der Entitäten in den folgenden Schritten ist möglicherweise einfacher, wenn Sie vor der Durchführung der Schritte die vordefinierte keyPhrase-Entität entfernen und sie dann wieder hinzufügen, nachdem Sie die Schritte in diesem Abschnitt abgeschlossen haben. 

1. Wählen Sie im linken Navigationsbereich **Absichten** aus.

2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

3. Geben Sie `NewEmployeeRelocationProcess` als Namen für die Absicht in das Popupdialogfeld ein.

4. Geben Sie die folgenden Beispieläußerungen ein, die die neuen Entitäten bezeichnen. Die Entitäts- und Rollenwerte werden in Fettschrift angezeigt. Denken Sie daran, zu der **Tokenansicht** zu wechseln, wenn Sie es einfacher finden, den Text zu bezeichnen. 

    Die Angabe der Entitätsrolle erfolgt nicht bei der Bezeichnung in der Absicht. Dies geschieht später bei der Erstellung des Musters. 

    |Äußerung|NewEmployee|NewEmployeeRelocation|
    |--|--|--|
    |Move **Bob Jones** from **Seattle** to **Los Colinas**|Bob Jones|Seattle, Los Colinas|
    |Move **Dave C. Cooper** from **Redmond** to **New York City**|Dave C. Cooper|Redmond, New York City|
    |Move **Jim Paul Smith** from **Toronto** to **West Vancouver**|Jim Paul Smith|Toronto, West Vancouver|
    |Move **J. Benson** from **Boston** to **Staines-upon-Thames**|J. Benson|Boston, Staines-upon-Thames|
    |Move **Travis "Trav" Hinton** from **Castelo Branco** to **Orlando**|Travis "Trav" Hinton|Castelo Branco, Orlando|
    |Move **Trevor Nottington III** from **Aranda de Duero** to **Boise**|Trevor Nottington III|Aranda de Duero, Boise|
    |Move **Dr. Greg Williams** from **Orlando** to **Ellicott City**|Dr. Greg Williams|Orlando, Ellicott City|
    |Move **Robert "Bobby" Gregson** from **Kansas City** to **San Juan Capistrano**|Robert "Bobby" Gregson|Kansas City, San Juan Capistrano|
    |Move **Patti Owens** from **Bellevue** to **Rockford**|Leni Schneider|Bellevue, Rockford|
    |Move **Janet Bartlet** from **Tuscan** to **Santa Fe**|Janet Bartlet|Tuscan, Santa Fe|

    Die Mitarbeiternamen weisen eine Vielzahl von Präfixen, Wortzahlen, Syntaxen und Suffixen auf. Dies ist wichtig, damit die LUIS-App die Varianten eines Namens eines neuen Mitarbeiters auflösen kann. Die Ortsnamen weisen ebenfalls unterschiedliche Wortzahlen und Syntaxen auf. Diese Vielfalt ist wichtig, um der LUIS-App beizubringen, wie diese Entitäten in der Äußerung eines Benutzers vorkommen können. 
    
    Wenn beide Entitäten die gleiche Wortzahl und keine anderen Varianten aufweisen würden, würden Sie der LUIS-App damit beibringen, dass diese Entität nur diese Wortzahl und keine andere Varianten aufweist. Die LUIS-App wäre so nicht in der Lage, ein größeres Spektrum von Varianten richtig vorherzusagen, da diese nicht vermittelt wurden. 

    Wenn Sie die keyPhrase-Entität entfernt haben, fügen Sie sie nun wieder der App hinzu.

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-without-pattern"></a>Abfragen des Endpunkts ohne Muster

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Geben Sie in der Adressleiste am Ende der URL `Move Wayne Berry from Miami to Mount Vernon` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```JSON
    {
      "query": "Move Wayne Berry from Newark to Columbus",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.514479756
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.514479756
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.017118983
        },
        {
          "intent": "MoveEmployee",
          "score": 0.009982505
        },
        {
          "intent": "GetJobInformation",
          "score": 0.008637771
        },
        {
          "intent": "ApplyForJob",
          "score": 0.007115978
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.006120186
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00452428637
        },
        {
          "intent": "None",
          "score": 0.00400899537
        },
        {
          "intent": "OrgChart-Reports",
          "score": 0.00240071164
        },
        {
          "intent": "Utilities.Help",
          "score": 0.001770991
        },
        {
          "intent": "EmployeeFeedback",
          "score": 0.001697356
        },
        {
          "intent": "OrgChart-Manager",
          "score": 0.00168116146
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00163952739
        },
        {
          "intent": "FindForm",
          "score": 0.00112958835
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.629158735
        },
        {
          "entity": "newark",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 27,
          "score": 0.638941
        }
      ]
    }  
    ```

Das Ergebnis der Absichtsvorhersage beträgt nur etwa 50 %. Wenn für Ihre Clientanwendung eine höhere Zahl erforderlich ist, müssen Sie dies korrigieren. Die beiden Entitäten wurden nicht vorhergesagt.

Muster sind für das Vorhersageergebnis zwar hilfreich, allerdings müssen die Entitäten korrekt vorhergesagt werden, damit das Muster mit der Äußerung übereinstimmt. 

## <a name="add-a-pattern-that-uses-roles"></a>Hinzufügen eines Musters, das Rollen verwendet
1. Wählen Sie in der oberen Navigationsleiste **Erstellen** aus.

2. Wählen Sie im linken Navigationsbereich **Muster** aus.

3. Wählen Sie **NewEmployeeRelocationProcess** aus der Dropdownliste **Eine Absicht auswählen** aus. 

4. Geben Sie das folgende Muster ein: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Wenn Sie den Endpunkt trainieren, veröffentlichen und abfragen, werden Sie möglicherweise enttäuscht sein, wenn die Entitäten nicht gefunden werden, und daraus den Schluss ziehen, dass das Muster nicht übereinstimmt und die Vorhersage nicht verbessert wurde. Dies ist darauf zurückzuführen, dass nicht genügend Beispieläußerungen mit bezeichneten Entitäten vorhanden sind. Um dieses Problem zu beheben, fügen Sie statt weiteren Beispielen eine Liste von Ausdrücken hinzu.

## <a name="create-a-phrase-list-for-cities"></a>Erstellen einer Liste von Ausdrücken für Orte
Ortsnamen sind wie Namen von Personen schwierig, da sie eine beliebige Kombination von Wörtern und Interpunktion aufweisen können. Die Orte in der Region und auf der Welt sind jedoch bekannt, sodass die LUIS-App für den Einstieg in das Training lediglich eine Liste von Orten benötigt. 

1. Wählen Sie im Menü auf der linken Seite im Abschnitt **App-Leistung verbessern** die Option **Liste von Ausdrücken** aus. 

2. Versehen Sie die Liste mit dem Namen `Cities`, und fügen Sie die folgenden `values` für die Liste hinzu:

    |Werte der Ausdrucksliste|
    |--|
    |Seattle|
    |San Diego|
    |New York City|
    |Los Angeles|
    |Portland|
    |Philadelphia|
    |Miami|
    |Dallas|

    Fügen Sie nicht jeden Ort auf der Welt oder sogar jeden Ort in der Region hinzu. Die LUIS-App muss generalisieren können, welcher Ort aus der Liste stammt. 

    Achten Sie darauf, dass die Option **Diese Werte sind austauschbar** aktiviert bleibt. Diese Einstellung bedeutet, dass die Wörter in der Liste als Synonyme behandelt werden. Genau so sollten diese im Muster behandelt werden.

    Denken Sie an die [Tutorialreihe, in der eine Liste von Ausdrücken erstellt wurde](luis-quickstart-primary-and-secondary-data.md). Auch dabei sollte die Entitätserkennung einer einfachen Entität verbessert werden.  

3. Trainieren und veröffentlichen Sie die App.

## <a name="query-endpoint-for-pattern"></a>Abfrageendpunkt für Muster
1. Wählen Sie unten auf der Seite **Publish** (Veröffentlichen) den Link **endpoint** (Endpunkt) aus. Hierdurch wird ein weiteres Browserfenster mit der Endpunkt-URL in der Adressleiste geöffnet. 

2. Geben Sie in der Adressleiste am Ende der URL `Move wayne berry from miami to mount vernon` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```JSON
    {
      "query": "Move Wayne Berry from Miami to Mount Vernon",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.9999999
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.9999999
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.49678385E-06
        },
        {
          "intent": "MoveEmployee",
          "score": 8.240291E-07
        },
        {
          "intent": "GetJobInformation",
          "score": 6.3131273E-07
        },
        {
          "intent": "None",
          "score": 4.25E-09
        },
        {
          "intent": "OrgChart-Manager",
          "score": 2.8E-09
        },
        {
          "intent": "OrgChart-Reports",
          "score": 2.8E-09
        },
        {
          "intent": "EmployeeFeedback",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.StartOver",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.Help",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Stop",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Cancel",
          "score": 1.35E-09
        },
        {
          "intent": "FindForm",
          "score": 1.23846156E-09
        },
        {
          "intent": "ApplyForJob",
          "score": 5.692308E-10
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "builtin.keyPhrase",
          "startIndex": 5,
          "endIndex": 15
        },
        {
          "entity": "miami",
          "type": "builtin.keyPhrase",
          "startIndex": 22,
          "endIndex": 26
        },
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.9410646,
          "role": ""
        },
        {
          "entity": "miami",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 26,
          "score": 0.9853915,
          "role": "NewEmployeeReloOrigin"
        },
        {
          "entity": "mount vernon",
          "type": "NewEmployeeRelocation",
          "startIndex": 31,
          "endIndex": 42,
          "score": 0.986044347,
          "role": "NewEmployeeReloDestination"
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
}
    ```

Das Absichtsergebnis fällt nun sehr viel höher aus, und die Rollennamen sind Teil der Entitätsantwort.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Best Practices für LUIS-Apps kennenlernen](luis-concept-best-practices.md)
