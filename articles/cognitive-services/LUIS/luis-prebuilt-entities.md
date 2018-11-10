---
title: Vordefinierte Entitäten für Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS umfasst eine Reihe von vordefinierten Entitäten zur Erkennung allgemeiner Informationstypen, z.B. Datumsangaben, Zeitangaben, Zahlen, Maße und Währungen. Die Unterstützung von vordefinierten Entität variiert je nach Kultur Ihrer LUIS-App.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/18/2018
ms.author: diberry
ms.openlocfilehash: 0fe9dbed302fd2d61305167a3bda25b1b403b761
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139973"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Vordefinierte Entitäten zum Erkennen allgemeiner Datentypen

LUIS umfasst eine Reihe von vordefinierten Entitäten zur Erkennung allgemeiner Informationstypen, z.B. Datumsangaben, Zeitangaben, Zahlen, Maße und Währungen. 

## <a name="add-a-prebuilt-entity"></a>Hinzufügen einer vordefinierten Entität

1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** auf ihren Namen und anschließend links auf **Entitäten** klicken. 

1. Klicken Sie auf der Seite **Entitäten** auf **Add prebuilt entity** (Vordefinierte Entität hinzufügen).

1. Wählen Sie im Dialogfeld **Add prebuilt entities** (Vordefinierte Entitäten hinzufügen) die vordefinierte Entität „datetimeV2“ aus. 

    ![Dialogfeld „Vordefinierte Entität hinzufügen“](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Wählen Sie **Fertig**aus.

## <a name="publish-the-app"></a>Veröffentlichen der App

Die einfachste Möglichkeit, den Wert einer vordefinierten Entität anzuzeigen, ist eine Abfrage an den veröffentlichten Endpunkt. 

1. Wählen Sie auf der oberen Symbolleiste **Veröffentlichen** aus. Veröffentlichen Sie für die **Produktion**. 

1. Wenn die grüne Erfolgsmeldung angezeigt wird, wählen Sie den Link **Refer to the list of endpoints** (Endpunktliste anzeigen) aus, um die Endpunkte anzuzeigen.

1. Wählen Sie einen Endpunkt aus. An diesen Endpunkt wird eine neue Browserregisterkarte geöffnet. Lassen Sie die Browserregisterkarte geöffnet, und fahren Sie mit dem Abschnitt **Testen** fort.

## <a name="test"></a>Test
Nachdem die Entität hinzugefügt wurde, müssen Sie die App nicht trainieren. 

Testen Sie die neue Absicht am Endpunkt, indem Sie einen Wert für den **q**-Parameter hinzufügen. Verwenden Sie die folgende Tabelle für vorgeschlagene Äußerungen zu **q**:

|Testen der Äußerung|Entitätswert|
|--|:--|
|Buche einen Flug morgen|2018-10-19|
|Sage Termin am 3. März ab|LUIS hat den letzten 3. März (3.3.2018) und den nächsten 3. März (3.3.2019) zurückgegeben, da in der Äußerung kein Jahr angegeben wurde.|
|Plane eine Besprechung um 10 Uhr|10:00:00|


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Referenz zu vordefinierten Entitäten](./luis-reference-prebuilt-entities.md)
