---
title: Verwenden der Eigenschaft „Erwartete Entität“ von Aktionen im Unterhaltungslernmodul – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie die Eigenschaft „Erwartete Entität“ in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 9e013e237a996d722d958920a1310e3aaea36c52
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170907"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Verwenden der Eigenschaft „Erwartete Entität“ von Aktionen

In diesem Tutorial wird das Feld „Erwartete Entität“ von Aktionen veranschaulicht.

## <a name="video"></a>Video

[![Tutorial 4 – Vorschau](http://aka.ms/cl-tutorial-04-preview)](http://aka.ms/blis-tutorial-04)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details
Verwenden Sie das Feld "Erwartete Entität" einer Aktion, um dem System mitzuteilen, dass Sie erwarten, dass die Antwort des Benutzers auf eine Aktion die Angabe einer Entität ist.

Wenn das Feld „Erwartete Entität“ einer Aktion auf „$entity“ festgelegt ist, führt das System bei der nächsten Benutzeräußerung die folgenden Aufgaben aus:

1. Zuerst versucht das System wie üblich, mithilfe des auf maschinellem Lernen basierenden Entitätsextraktionsmodells nach Entitäten zu suchen.
2. Wenn in Schritt 1 keine Entitäten gefunden wurden, weist das System – als Heuristik – der gesamten Benutzeräußerung „$entity“ zu.
3. Das System ruft wie üblich `EntityDetectionCallback` auf und fährt mit der Aktionsauswahl fort.

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie in der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „ExpectedEntities“ ein. Klicken Sie dann auf „Erstellen“.

### <a name="create-an-entity"></a>Erstellen einer Entität

1. Klicken Sie auf „Entitäten“ und dann auf „Neue Entität“.
2. Geben Sie im Feld „Entitätsname“ die Zeichenfolge „name“ ein.
3. Klicken Sie auf „Erstellen“.

> [!NOTE]
> Der Entitätstyp ist „benutzerdefiniert“. Dieser Wert bedeutet, dass die Entität trainiert werden kann.  Es gibt auch vorgefertigte Entitäten, was bedeutet, dass ihr Verhalten nicht angepasst werden kann.  Diese Entitäten werden im [Tutorial „Vordefinierte Entitäten“](./7-built-in-entities.md) behandelt.

![](../media/tutorial4_entities.PNG)

### <a name="create-two-actions"></a>Erstellen von zwei Aktionen

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie im Feld „Antwort“ die Frage „Wie lautet Ihr Name?“ ein.
3. Geben Sie im Feld „Erwartete Entitäten“ die Zeichenfolge „$name“ ein. Klicken Sie auf Speichern.
    - Dieser Wert bedeutet Folgendes: Wenn diese Frage gestellt wird und für die Benutzerantwort keine Entitäten erkannt werden, sollte der Bot davon ausgehen, dass die gesamte Antwort des Benutzers diese Entität ist.
2. Klicken Sie auf „Aktionen“, und klicken Sie dann auf „Neue Aktion“, um eine zweite Aktion zu erstellen.
3. Geben Sie im Feld „Antwort“ die Grußformel „Hallo $name“ ein.
    - Die Entität wird automatisch als disqualifizierende Entität hinzugefügt. 
4. Klicken Sie auf Speichern.

Jetzt sind zwei Aktionen vorhanden.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Wie lautet Ihr Name?“ aus.
    - Die Antwort „Hallo $name“ kann nicht ausgewählt werden, weil dazu die Entität „$name“ definiert sein muss. $name ist jedoch nicht im Speicher des Bots vorhanden.
2. Geben Sie „David“ ein. 
    - Der Name wird als Entität hervorgehoben. Das liegt an der Heuristik, die wir oben eingerichtet haben, um die Antwort als Entität auszuwählen.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Der Wert „name“ ist jetzt im Speicher des Bots vorhanden.
    - „Hallo $name“ ist jetzt als Antwort verfügbar. 
6. Wählen Sie „Hallo $name“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

Hier sind zwei Beispiele, in denen das auf maschinellem Lernen basierende Entitätsextraktionsmodell einen Namen identifiziert, sodass die Heuristik „Erwartete Entität“ nicht ausgelöst wird.

1. Klicken Sie auf „Neuer Trainingsdialog“.
2. Geben Sie „Mein Name ist David“ ein.
    - Das Modell identifiziert David als Namensentität, weil dieses Wort bereits vorgekommen ist und daher erkannt wurde.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Wählen Sie „Hallo $name“ aus.
4. Geben Sie „Mein Name ist Susan“ ein.
    - Das Modell identifiziert Susan als Name, weil dieses Muster bereits vorgekommen ist.
2. Klicken Sie auf „Bewertungsaktionen“.
2. Wählen Sie „Hallo Susan“ aus.
3. Klicken Sie auf „Training abgeschlossen“.

In den folgenden Beispielen wird die Heuristik „erwartete Entität“ ausgelöst, ist aber falsch. Anschließend wird veranschaulicht, wie eine Korrektur vorgenommen wird.

1. Geben Sie „Nennen Sie mich Jose“ ein.
    - Das Modell erkennt den Name nicht als Entität.
2. Klicken Sie auf „Jose“, und wählen Sie „name“ aus.
3. Klicken Sie auf „Bewertungsaktionen“.
4. Wählen Sie „Hallo $name“ aus.
5. Klicken Sie auf „Training abgeschlossen“.
1. Klicken Sie auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Geben Sie als Antwort auf die Frage „Wie lautet Ihr Name?“ den Satz „Ich heiße Frank“ ein.
    - Der gesamte Ausdruck wird hervorgehoben. Das liegt daran, dass das statistische Modell keinen Namen gefunden hat, sodass die Heuristik ausgelöst und die gesamte Antwort als Namensentität ausgewählt wurde.
2. Um dies zu korrigieren, klicken Sie auf den markierten Ausdruck, und klicken Sie dann auf das rote X. 
3. Klicken Sie zum Auswählen auf „Frank“, und klicken Sie dann auf „name“.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Wählen Sie „Hallo $name“ aus.
4. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Negierbare Entitäten](./5-negatable-entities.md)
