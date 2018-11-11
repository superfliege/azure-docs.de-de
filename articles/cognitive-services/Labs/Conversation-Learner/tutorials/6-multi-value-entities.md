---
title: Verwenden von mehrwertigen Entitäten in einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie mehrwertige Entitäten in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 9ef86ea728e197540180d486dc1cb171f78a2590
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229457"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Verwenden von mehrwertigen Entitäten in einem Unterhaltungslernmodell
In diesem Tutorial wird die Eigenschaft „mehrwertig“ von Entitäten erörtert.

## <a name="video"></a>Video

[![Tutorial 6 – Vorschau](https://aka.ms/cl-tutorial-06-preview)](https://aka.ms/blis-tutorial-06)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details
Eine Entität, die „mehrwertig“ ist, akkumuliert Werte in einer Liste, statt einen einzelnen Wert zu speichern.  Dies ist bei Entitäten hilfreich, für die der Benutzer mehrere Werte (z. B. Beläge auf einer Pizza) angeben kann.

Konkret bedeutet das Folgendes: Wenn eine Entität als „mehrwertig“ gekennzeichnet ist, wird jede erkannte Instanz der Entität an eine Liste im Speicher des Bots angefügt (statt einen einzelnen Entitätswert zu überschreiben).

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie in der Webbenutzeroberfläche auf „Neues Modell“.
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
    - Die beiden Werte sind jetzt in der Entität „Toppings“ vorhanden. 
6. Wählen Sie „Hier sind Ihre Beläge: $Beläge“ aus.

Wir können noch mehr hinzufügen:

7. Geben Sie „Paprika hinzufügen“ ein.
    - Klicken Sie unter „Entitätserkennung“ auf „Paprika“, und wählen Sie „Beläge“ aus.
3. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
    - „Paprika“ jetzt als zusätzlicher Wert für die Beläge angezeigt.
6. Wählen Sie „Hier sind Ihre Beläge: $Beläge“ aus.

Als Nächstes werden wir einen Belag entfernen und einen Belag hinzufügen:

2. Geben Sie „Paprika entfernen und Salami hinzufügen“ ein.
1. Klicken Sie auf „Paprika“, und klicken Sie auf das rote X, um diesen Belag zu entfernen.
2. Klicken Sie auf „Paprika“, und wählen Sie „-Beläge“ aus.
3. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
    - „Paprika“ wurde gelöscht und „Salami“ wurde hinzugefügt.
6. Wählen Sie „Hier sind Ihre Beläge: $Beläge“ aus.

Jetzt versuchen wir, alles zu entfernen:

6. Geben Sie „Pilze entfernen, Käse entfernen und Salami entfernen“ ein.
7. Klicken Sie auf jeden der drei Beläge, und wählen Sie „-Beläge“ aus.
7. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
    - Alle Beläge werden entfernt.
2. Wählen Sie „Welche Beläge möchten Sie?“ aus.
3. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Integrierte Entitäten](./7-built-in-entities.md)
