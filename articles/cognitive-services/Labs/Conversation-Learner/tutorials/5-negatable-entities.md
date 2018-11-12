---
title: Verwenden von negierbaren Entitäten in einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie negierbare Entitäten in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: df6499410da3ae67715ade29dbc3cc4146fc2265
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51231990"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Erfahren Sie, wie Sie negierbare Entitäten in einem Unterhaltungslernmodell verwenden.

In diesem Tutorial wird die „negierbare“ Eigenschaft von Entitäten veranschaulicht.

## <a name="video"></a>Video

[![Tutorial 5 – Vorschau](https://aka.ms/cl-tutorial-05-preview)](https://aka.ms/blis-tutorial-05)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details
Kennzeichnen Sie eine Aktion als „negierbar“, wenn der Benutzer einen Entitätswert – wie in „Nein, ich möchte $entity nicht“ oder „Nein, keine $entity“ – „löschen“ kann. Beispiel: „Nein, ich möchte Köln nicht verlassen.“

Konkret gilt Folgendes, wenn die „negierbare“ Eigenschaft einer Entität festgelegt ist:

- Beim Bezeichnen von Entitätserwähnungen können Sie sowohl normale (positive) Instanzen einer Entität als auch „negative“ Instanzen der Entität bezeichnen.
- LUIS erlernt zwei Entitätsmodelle: ein Modell für positive Instanzen und ein zweites für negative Instanzen.
- Der Effekt einer negativen Instanz einer Entität ist es, diesen Wert aus der Entitätsvariablen zu löschen (falls vorhanden).

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie in der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „NegatableEntity“ ein. Klicken Sie dann auf „Erstellen“.

### <a name="create-an-entity"></a>Erstellen einer Entität

1. Klicken Sie auf „Entitäten“ und dann auf „Neue Entität“.
2. Geben Sie im Feld „Entitätsname“ die Zeichenfolge „name“ ein.
3. Aktivieren Sie „Negierbar“.
    - Mit dieser Eigenschaft wird angegeben, dass der Benutzer einen Wert für die Entität bereitstellen oder festlegen kann, dass etwas *nicht* der Wert der Entität ist. Im letzteren Fall führt dies zum Löschen eines übereinstimmenden Werts der Entität.
3. Klicken Sie auf „Erstellen“.

![](../media/tutorial5_entities.PNG)

### <a name="create-two-actions"></a>Erstellen von zwei Aktionen

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie im Feld „Antwort“ den Satz „Ich kenne Ihren Namen nicht“ ein.
3. Geben Sie im Feld „Disqualifizierende Entitäten“ die Zeichenfolge „name“ ein.
3. Klicken Sie auf „Erstellen“.

Erstellen Sie dann die zweite Aktion.

1. Klicken Sie auf „Aktionen“, und klicken Sie dann auf „Neue Aktion“, um eine zweite Aktion zu erstellen.
3. Geben Sie im Feld „Antwort“ den folgenden Satz ein: „Ich kenne Ihren Namen, er lautet $name“.
4. Klicken Sie auf „Erstellen“.

Jetzt sind zwei Aktionen vorhanden.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Ich kenne Ihren Namen nicht“ aus.
    - Die Bewertung ist 100 %, da es die einzige gültige Aktion ist.
2. Geben Sie „Mein Name ist David“ ein.
3. Wählen Sie „David“ aus, und wählen Sie die Bezeichnung „+name“ aus.
    - Es gibt zwei Instanzen von „name“: „+name“ und „-name“.  (+) Plus fügt den Wert hinzu oder überschreibt ihn. (-) Minus entfernt den Wert.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Der Wert „name“ ist jetzt im Speicher des Bots vorhanden.
    - „Ich kenne Ihren Namen, er lautet $name“ ist die einzige verfügbare Antwort. 
6. Wählen Sie Folgendes aus: „Ich kenne Ihren Namen, er lautet $name“.

Lassen Sie uns versuchen, die negierbare Entität zu löschen:

7. Geben Sie „Mein Name ist nicht David“ ein.
    - Beachten Sie, dass basierend auf dem vorherigen Muster „nicht“ als Name ausgewählt ist. Diese Bezeichnung ist falsch.
2. Klicken Sie auf „nicht“, und klicken Sie dann auf das rote X. 
3. Klicken Sie auf „David“.
    - Dies ist jetzt eine negative Entität, die dem System mitteilt, dass dies nicht der Wert der Entität „name“ ist.
2. Wählen Sie „-name“ aus.
3. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass der Wert aus dem Speicher gelöscht wurde.
2. Wählen Sie „Ich kenne Ihren Namen nicht“ aus. Dies ist die einzige Aktion.

Als Nächstes wird beschrieben, wie Sie einen neuen Wert für den Namen eingeben können.

3. Geben Sie „John“ als Namen ein. Wählen Sie dann „John“ aus, und klicken Sie auf „name“.
4. Klicken Sie auf „Bewertungsaktionen“.
5. Wählen Sie Folgendes aus: „Ich kenne Ihren Namen, er lautet $name“.

Versuchen Sie jetzt, den eingegebenen Namen zu ersetzen.

6. Geben Sie „Mein Name ist Susan“ ein.
7. Wählen Sie Folgendes aus: „Ich kenne Ihren Namen, er lautet $name“.
7. Klicken Sie auf „Bewertungsaktionen“.
8. Beachten Sie, dass in den Entitätswerten der Wert **Susan** den Wert **John** überschrieben hat.
9. Geben Sie „Mein Name ist nicht Susan“ ein.
    - Beachten Sie, dass das System dies als negative Instanz bezeichnet hat.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Wählen Sie „Ich kenne Ihren Namen nicht“ aus. Dies ist die einzige Aktion.
7. Klicken Sie auf „Done Teaching“ (Training abgeschlossen).

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Mehrwertige Entitäten](./6-multi-value-entities.md)
