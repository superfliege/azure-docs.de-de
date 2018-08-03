---
title: Tutorial zur Verwendung einer Ausdrucksliste zur Verbesserung der LUIS-Vorhersagen – Azure | Microsoft-Dokumentation
description: In diesem Tutorial fügen Sie einer LUIS-App eine Ausdrucksliste hinzu und sehen sich die Verbesserungen bei der Bewertung an.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 2fd53225a455a34d03772487a10f62a94ac86735
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868972"
---
# <a name="tutorial-add-phrase-list-to-improve-predictions"></a>Tutorial: Hinzufügen einer Ausdrucksliste zum Verbessern der Vorhersagen
In diesem Tutorial verbessern Sie die Genauigkeit der Bewertungen von Absichten und ermitteln Entitäten für Wörter mit derselben Bedeutung (Synonyme), indem Sie eine austauschbare [Ausdrucksliste als Feature](./luis-concept-feature.md) hinzufügen.

> [!div class="checklist"]
* Importieren einer neuen App  
* Abfragen des Endpunkts mit einer bekannten Äußerung 
* Abfragen des Endpunkts mit einer _unbekannten_ Äußerung
* Hinzufügen einer Ausdrucksliste zur Verbesserung der Bewertung der unbekannten Äußerung
* Überprüfen, ob die Entität bei Verwendung der Ausdrucksliste gefunden wird

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Konto für die Erstellung Ihrer LUIS-Anwendung.

## <a name="import-a-new-app"></a>Importieren einer neuen App
1. Laden Sie die [LUIS-Beispiel-App][LuisSampleApp] für dieses Tutorial herunter. Sie werden ihn im nächsten Schritt verwenden. 

2. Gehen Sie wie unter [Erstellen einer App](Create-new-app.md#import-new-app) beschrieben vor, um die heruntergeladene Datei auf der [LUIS][LUIS]-Website als neue App zu importieren. Der App-Name lautet „My Phrase List tutorial“. Sie enthält Absichten, Entitäten und Äußerungen. 

3. [Trainieren](luis-how-to-train.md) Sie Ihre App. Bevor sie nicht trainiert ist, können Sie sie nicht [interaktiv](interactive-test.md#interactive-testing) auf der [LUIS][LUIS]-Website testen. 

4. Aktivieren Sie auf der Seite [Veröffentlichen](luis-how-to-publish-app.md) das Kontrollkästchen **Include all predicted intent scores** (Alle vorhergesagten Absichtsbewertungen einschließen). Wenn dieses Kontrollkästchen aktiviert ist, werden alle Absichten zurückgegeben. Wenn dieses Kontrollkästchen deaktiviert ist, wird nur die Absicht mit der höchsten Bewertung zurückgegeben. 

5. [Veröffentlichen](luis-how-to-publish-app.md) Sie Ihre App. Durch das Veröffentlichen der App kann sie mit dem HTTPS-Endpunkt getestet werden. 

## <a name="test-a-trained-utterance"></a>Testen einer trainierten Äußerung
Verwenden Sie den veröffentlichten Endpunkt, um eine Äußerung abzufragen, die die App bereits kennt. Da LUIS die Äußerung bereits kennt, ist die Bewertung hoch, und die Entität wird erkannt.

1. Wählen Sie auf der [LUIS][LUIS]-Website (Language Understanding Intelligent Service) auf der Seite **Veröffentlichen** der neuen App im Abschnitt **Resources and Keys** (Ressourcen und Schlüssel) die Endpunkt-URL aus. 

    ![Veröffentlichen der Endpunkt-URL](./media/luis-tutorial-interchangeable-phrase-list/luis-publish-url.png)

2. Fügen Sie im Browser am Ende der URL die folgende Abfrage hinter `q=` ein.

    `I want a computer replacement`

    Der Endpunkt antwortet mit dem folgenden JSON-Code:
    
    ```JSON
    {
      "query": "I want a computer replacement",
      "topScoringIntent": {
        "intent": "GetHardware",
        "score": 0.9735458
      },
      "intents": [
        {
          "intent": "GetHardware",
          "score": 0.9735458
        },
        {
          "intent": "None",
          "score": 0.07053033
        },
        {
          "intent": "Whois",
          "score": 0.03760778
        },
        {
          "intent": "CheckCalendar",
          "score": 0.02285902
        },
        {
          "intent": "CheckInventory",
          "score": 0.0110072717
        }
      ],
      "entities": [
        {
          "entity": "computer",
          "type": "Hardware",
          "startIndex": 9,
          "endIndex": 16,
          "score": 0.8465915
        }
      ]
    }
    ```

    Die Bewertung der Absicht ist mit 0,973 ebenso wie die Bewertung der Entitätserkennung mit 0,846 sehr hoch, da die App mit dieser Äußerung trainiert wurde. Sie finden die Äußerung in der LUIS-App auf der Absichtsseite für **GetHardware**. Der Text der Äußerung (`computer`) ist als Entität **Hardware** bezeichnet. 
    
    |Status|Word| Absichtsbewertung | Entitätsbewertung |
    |--|--|--|--|
    |Trainiert| want | 0,973 | 0,846 |
    
    
## <a name="test-an-untrained-utterance"></a>Testen einer untrainierten Äußerung
Verwenden Sie im Browser denselben veröffentlichten Endpunkt für eine Abfrage mit einer Äußerung, die die App noch nicht kennt:

`I require a computer replacement`

Diese Äußerung verwendet ein Synonym für die vorherige Äußerung:

| Trainiertes Wort | Untrainiertes Synonym |
|--|--|
| want | require |

Die Endpunktantwort lautet:

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.840912163
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.840912163
    },
    {
      "intent": "None",
      "score": 0.0972757638
    },
    {
      "intent": "Whois",
      "score": 0.0448251367
    },
    {
      "intent": "CheckCalendar",
      "score": 0.0291390456
    },
    {
      "intent": "CheckInventory",
      "score": 0.0137837809
    }
  ],
  "entities": []
}
```

| Status | Word | Absichtsbewertung | Entitätsbewertung |
|--|--|--|--|
| Trainiert| want | 0,973 | 0,846 |
| Untrainiert| require | 0,840 | - |

Die Bewertung der Absicht in der untrainierten Äußerung ist niedriger als die der bezeichneten Äußerung. LUIS weiß zwar, dass der Satz grammatisch identisch ist, aber nicht, dass die Äußerungen dieselbe Bedeutung haben. Darüber hinaus wird die Entität **Hardware** ohne die Ausdrucksliste nicht gefunden.

Sie müssen LUIS beibringen, dass *want* und *require* in dieser App-Domäne dasselbe aussagen, da mehrere Wörter dieselbe Bedeutung haben können. 

## <a name="improve-the-score-of-untrained-utterance-with-phrase-list"></a>Verbessern der Bewertung einer untrainierten Äußerung mit einer Ausdrucksliste 
1. Fügen Sie das Feature [Ausdrucksliste](luis-how-to-add-features.md) mit dem Namen **want** und dem Wert `want` hinzu, und drücken Sie dann die **EINGABETASTE**.

    > [!TIP]
    > Drücken Sie nach jedem Wort oder Ausdruck die **EINGABETASTE**. Das Wort oder der Ausdruck wird im Feld der **Ausdruckslistenwerte** hinzugefügt, während der Cursor im Feld **Wert** verbleibt. Sie können mit diesem Feature viele Werte schnell eingeben.

2. Zum Anzeigen von Wörtern, die LUIS empfiehlt, wählen Sie **Empfehlen** aus. 

    ![Empfehlen von Werten](./media/luis-tutorial-interchangeable-phrase-list/recommend.png)

3. Fügen Sie alle Wörter hinzu. Wenn `require` nicht in der Liste der Empfehlungen enthalten ist, fügen Sie es als einen erforderlichen Wert hinzu. 

4. Da diese Wörter Synonyme sind, behalten Sie die Einstellung *austauschbar* bei und wählen dann **Speichern** aus.

    ![Ausdruckslistenwerte](./media/luis-tutorial-interchangeable-phrase-list/phrase-list-values.png)

5. Wählen Sie auf der oberen Navigationsleiste **Train** (Trainieren) aus, um die App zu trainieren, aber veröffentlichen Sie sie nicht. Sie verfügen jetzt über zwei Modelle. Sie können die Werte in den beiden Modellen vergleichen.

## <a name="compare-the-phrase-list-model-to-the-published-model"></a>Vergleichen des Modells der Ausdrucksliste mit dem veröffentlichten Modell
In dieser App wurde das veröffentlichte Modell nicht mit den Synonymen trainiert. Nur das derzeit bearbeitete Modell enthält die Ausdrucksliste mit Synonymen. Um die Modelle zu vergleichen, verwenden Sie [interaktive Tests](interactive-test.md#interactive-testing). 

1. Öffnen Sie den Bereich **Test**, und geben Sie die folgenden Äußerung ein:

    `I require a computer replacement`

2. Wählen Sie zum Öffnen des Überprüfungsbereich **Überprüfen** aus. 

    ![Auswählen von „Überprüfen“](./media/luis-tutorial-interchangeable-phrase-list/inspect-button.png)

3. Wählen Sie zum Vergleichen des veröffentlichten Modells mit dem neuen Modell mit Ausdrucksliste **Compare with published** (Mit veröffentlichtem Element vergleichen) aus.

    ![Überprüfen von veröffentlichtem und aktuellem Modell](./media/luis-tutorial-interchangeable-phrase-list/inspect.png)

Nach dem Hinzufügen der Ausdrucksliste erhöht sich die Genauigkeit der Äußerung, und die Entität **Hardware** wird gefunden. 

|Status | Ausdrucksliste| Absichtsbewertung | Entitätsbewertung |
|--|--|--|--|
| Veröffentlicht | - | 0,84 | - |
| Derzeit bearbeitet |✔| 0,92 | Hardware-Entität identifiziert |

> [!TIP]
> * Mithilfe von [interaktiven Tests](interactive-test.md#interactive-testing) können Sie das veröffentlichte Modell mit beliebigen trainierten Änderungen vergleichen, die nach dem Veröffentlichen vorgenommen wurden. 
> * Mithilfe von [Endpunkttests](luis-how-to-publish-app.md#test-your-published-endpoint-in-a-browser) können Sie sich die genaue JSON-Antwort von LUIS ansehen. 

## <a name="get-the-entity-score-with-the-endpoint-test"></a>Abrufen der Entitätsbewertung mit dem Endpunkttest
Um die Entitätsbewertung anzuzeigen, [veröffentlichen Sie das Modell](luis-how-to-publish-app.md) und fragen den Endpunkt ab. 

`I require a computer replacement`

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.916503668
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.916503668
    },
    {
      "intent": "None",
      "score": 0.136505231
    },
    {
      "intent": "Whois",
      "score": 0.02778677
    },
    {
      "intent": "CheckInventory",
      "score": 0.0144592477
    },
    {
      "intent": "CheckCalendar",
      "score": 0.01401332
    }
  ],
  "entities": [
    {
      "entity": "computer",
      "type": "Hardware",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.5959917
    }
  ]
}
```

Für die Entität **Hardware** wird eine Bewertung von 0,595 mit der Ausdrucksliste angezeigt. Bevor die Ausdrucksliste verfügbar war, wurde die Entität nicht erkannt. 

|Status | Ausdrucksliste| Absichtsbewertung | Entitätsbewertung |
|--|--|--|--|
| Veröffentlicht | - | 0,84 | - |
| Derzeit bearbeitet |✔| 0,92 | 0,595 |


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App, falls Sie sie nicht mehr benötigen. Klicken Sie dazu in der Liste rechts vom App-Namen auf die Auslassungspunkte (***...***) und dann auf **Löschen**. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen einer Vorhersage für die Äußerung per Endpunktabfrage](luis-get-started-cs-get-intent.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
[LuisFeatures]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[LuisSampleApp]: https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/phrase_list/interchangeable/luis-app-before-phrase-list.json
