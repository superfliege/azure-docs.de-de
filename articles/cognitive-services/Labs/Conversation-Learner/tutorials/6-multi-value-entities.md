---
title: Verwenden von mehrwertigen Entitäten in einer Unterhaltungslernanwendung – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie mehrwertige Entitäten in einer Unterhaltungslernanwendung verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 865b50747b2c9574b5f88d4902bea9e4c8e0e032
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376242"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-application"></a>Verwenden von mehrwertigen Entitäten in einer Unterhaltungslernanwendung
In diesem Tutorial wird die Eigenschaft „mehrwertig“ von Entitäten erörtert.

##<a name="requirements"></a>Anforderungen
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details
Eine Entität, die „mehrwertig“ ist, akkumuliert Werte in einer Liste, statt einen einzelnen Wert zu speichern.  Dies ist bei Entitäten hilfreich, für die der Benutzer mehrere Werte (z. B. Beläge auf einer Pizza) angeben kann.

Konkret bedeutet das Folgendes: Wenn eine Entität als „mehrwertig“ gekennzeichnet ist, wird jede erkannte Instanz der Entität an eine Liste im Speicher des Bots angefügt (statt einen einzelnen Entitätswert zu überschreiben).

## <a name="steps"></a>Schritte

### <a name="create-the-application"></a>Erstellen der Anwendung

1. Klicken Sie auf der Webbenutzeroberfläche auf „Neue App“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „MultiValueEntities“ ein. Klicken Sie dann auf „Erstellen“.

### <a name="create-an-entity"></a>Erstellen einer Entität

1. Klicken Sie auf „Entitäten“ und dann auf „Neue Entität“.
2. Geben Sie im Feld „Entitätsname“ die Bezeichnung „Beläge“ ein.
3. Aktivieren Sie „Mehrwertig“.
    - Mehrwertige Entitäten sammeln einen oder mehrere Werte in der Entität.
2. Aktivieren Sie „Negierbar“.  
    - Dadurch kann der Benutzer Beläge aus der Liste der akkumulierten Pizzabeläge entfernen.
3. Klicken Sie auf „Erstellen“.

![](../media/tutorial6_entities.PNG)

### <a name="create-two-actions"></a>Erstellen von zwei Aktionen

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie im Feld „Antwort“ die Frage „Welche Beläge möchten Sie?“ ein.
3. Geben Sie im Feld „Disqualifizierende Entitäten“ die Bezeichnung „Beläge“ ein.
3. Klicken Sie auf „Erstellen“.

Erstellen Sie dann die zweite Aktion.

1. Klicken Sie auf „Aktionen“, und klicken Sie dann auf „Neue Aktion“, um eine zweite Aktion zu erstellen.
3. Geben Sie im Feld „Antwort“ den Satz „Hier sind Ihre Beläge: $Beläge“ ein.
4. Klicken Sie auf „Erstellen“.

Jetzt sind zwei Aktionen vorhanden.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Welche Beläge möchten Sie?“ aus.
2. Geben Sie „Pilze und Käse“ ein. 
    - Sie können keine (null), eine oder mehrere der Entitäten bezeichnen.
3. Klicken Sie auf „Pilze“, und wählen Sie „Beläge“ aus.
4. Klicken Sie auf „Käse“, und wählen Sie „Beläge“ aus.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass die beiden Werte jetzt in der Entität „Beläge“ vorhanden sind. 
6. Wählen Sie „Hier sind Ihre Beläge: $Beläge“ aus.

Wir können noch mehr hinzufügen:

7. Geben Sie „Paprika hinzufügen“ ein.
    - Klicken Sie unter „Entitätserkennung“ auf „Paprika“, und wählen Sie „Beläge“ aus.
3. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass „Paprika“ jetzt als zusätzlicher Wert für „Beläge“ angezeigt wird.
6. Wählen Sie „Hier sind Ihre Beläge: $Beläge“ aus.

Als Nächstes werden wir einen Belag entfernen und einen Belag hinzufügen:

2. Geben Sie „Paprika entfernen und Salami hinzufügen“ ein.
1. Klicken Sie auf „Paprika“, und klicken Sie auf das rote X, um diesen Belag zu entfernen.
2. Klicken Sie auf „Paprika“, und wählen Sie „-Beläge“ aus.
3. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass „Paprika“ gelöscht und „Salami“ hinzugefügt wurde.
6. Wählen Sie „Hier sind Ihre Beläge: $Beläge“ aus.

Jetzt versuchen wir, alles zu entfernen:

6. Geben Sie „Pilze entfernen, Käse entfernen und Salami entfernen“ ein.
7. Klicken Sie auf jeden der drei Beläge, und wählen Sie „-Beläge“ aus.
7. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass alle Beläge entfernt werden.
2. Wählen Sie „Welche Beläge möchten Sie?“ aus.
3. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Integrierte Entitäten](./7-built-in-entities.md)
