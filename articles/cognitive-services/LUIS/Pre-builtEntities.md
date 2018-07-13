---
title: Vordefinierte Entitäten in LUIS | Microsoft-Dokumentation
description: Dieser Artikel enthält Listen mit vordefinierten Entitäten, die in LUIS (Language Understanding Intelligent Services) enthalten sind.
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/15/2018
ms.author: cahann
ms.reviewer: v-geberr
ms.openlocfilehash: 4858de8b8517016ca385e9105ca8948b66aa683b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321802"
---
# <a name="prebuilt-entities"></a>Vordefinierte Entitäten

LUIS umfasst eine Reihe von vordefinierten Entitäten zur Erkennung allgemeiner Informationstypen, z.B. Datumsangaben, Zeitangaben, Zahlen, Maße und Währungen. Die Unterstützung von vordefinierten Entität variiert je nach Kultur Ihrer LUIS-App. Eine vollständige Liste vordefinierter Entitäten, die von LUIS unterstützt werden, einschließlich der Unterstützung durch die Kultur, finden Sie unter [Referenz zu vordefinierten Entitäten](./luis-reference-prebuilt-entities.md).

> [!NOTE]
> Die Entität **builtin.datetime** ist veraltet. Sie wurde durch [**builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md) ersetzt. Diese stellt die Erkennung von Datums- und Zeitbereichen sowie verbesserte Erkennung mehrdeutiger Datums- und Zeitangaben bereit.

## <a name="add-a-prebuilt-entity"></a>Hinzufügen einer vordefinierten Entität

1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** auf ihren Namen und anschließend links auf **Entitäten** klicken. 
2. Klicken Sie auf der Seite **Entitäten** auf **Manage prebuilt entities** (Vordefinierte Entitäten verwalten).

    ![„Vordefinierte Entitäten verwalten“ auf der Seite „Entitäten“](./media/luis-use-prebuilt-entity/add-prebuilt-entity-button.png)
3. Klicken Sie im Dialogfeld **Add prebuilt entities** (Vordefinierte Entitäten hinzufügen) auf die Entität, die Sie hinzufügen möchten (z.B: „datetimeV2“). Klicken Sie anschließend auf **Speichern**.

    ![Dialogfeld „Vordefinierte Entität hinzufügen“](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

## <a name="use-a-prebuilt-number-entity"></a>Verwenden einer vordefinierten Zahlenentität
Wenn Ihre Anwendung eine vordefinierte Entität enthält, werden ihre Vorhersagen in der veröffentlichten Anwendung eingeschlossen. Das Verhalten vordefinierter Entitäten ist vortrainiert und kann **nicht** angepasst werden. Führen Sie die folgenden Schritte aus, um zu erfahren, wie eine vordefinierte Entität funktioniert:

1. Fügen Sie der App eine **Zahlenentität** hinzu, und [trainieren](interactive-test.md) und [veröffentlichen](PublishApp.md) Sie anschließend die App.
2. Klicken Sie auf der Seite **App veröffentlichen** auf die Endpunkt-URL, um den LUIS-Endpunkt in einem Webbrowser zu öffnen. 
3. Fügen Sie der URL eine Äußerung an, die einen numerischen Ausdruck enthält. Wenn Sie beispielsweise `buy two plane ticktets` eingeben, werden Sie sehen, dass LUIS `two` als eine `builtin.number`-Entität und `2` als ihren Wert im `resolution`-Feld identifiziert. Mit dem Feld `resolution` können Sie Zahlen und Datumsangaben in ein kanonisches Format auflösen, das die Clientanwendung einfacher verwenden kann. 

    ![Zahlenentität in einer Äußerung im Browser](./media/luis-use-prebuilt-entity/browser-query.png)

LUIS kann Zahlen auf intelligente Weise erkennen, die nicht im Standardformat vorhanden sind. Probieren Sie verschiedene numerische Ausdrücke in den Äußerungen aus, und sehen Sie sich an, was LUIS zurückgibt.

Im folgenden Beispiel wird eine JSON-Antwort von LUIS gezeigt, die die Auflösung des Werts „24“ für die Äußerung „two dozen“ enthält.

```json
{
  "query": "order two dozen tickets for group travel",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.905443209
  },
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```
## <a name="use-a-prebuilt-datetimev2-entity"></a>Verwenden einer vordefinierten datetimeV2-Entität
Die vordefinierte Entität **datetimeV2** erkennt Datumsangaben, Zeitangaben, Datumsbereiche und Zeiträume. Führen Sie die folgenden Schritte aus, um zu erfahren, wie die vordefinierte Entität `datetimeV2` funktioniert:

1. Fügen Sie der App eine **datetimeV2**-Entität hinzu, und [trainieren](interactive-test.md) und [veröffentlichen](PublishApp.md) Sie anschließend die App.
2. Klicken Sie auf der Seite **App veröffentlichen** auf die Endpunkt-URL, um den LUIS-Endpunkt in einem Webbrowser zu öffnen. 
3. Fügen Sie der URL eine Äußerung an, die einen Datumsbereich enthält. Wenn Sie beispielsweise `book a flight tomorrow` eingeben, werden Sie sehen, dass LUIS `tomorrow` als eine `builtin.datetimeV2.date`-Entität und das morgige Datum als ihren Wert im `resolution`-Feld identifiziert. 

Im folgenden Beispiel wird gezeigt, wie die JSON-Antwort von LUIS aussehen könnte, wenn das heutige Datum der 31. Oktober 2017 wäre.

```json
{
  "query": "book a flight tomorrow",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9063408
  },
  "entities": [
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 14,
      "endIndex": 21,
      "resolution": {
        "values": [
          {
            "timex": "2017-11-01",
            "type": "date",
            "value": "2017-11-01"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Referenz zu vordefinierten Entitäten](./luis-reference-prebuilt-entities.md)
