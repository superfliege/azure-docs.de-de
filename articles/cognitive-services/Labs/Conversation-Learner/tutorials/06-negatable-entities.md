---
title: Verwenden von negierbaren Entitäten in einem Conversation Learner-Modell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie negierbare Entitäten in einem Conversation Learner-Modell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: fea950e2c13d9b5ce0c3619990961e611edd6626
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207376"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Verwenden von negierbaren Entitäten in einem Conversation Learner-Modell

In diesem Tutorial wird die Eigenschaft „Negierbar“ von Entitäten veranschaulicht.

## <a name="video"></a>Video

[![Negierbare Entitäten: Tutorial (Vorschauversion)](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details
Die Eigenschaft „Negierbar“ einer Entität ermöglicht Ihnen das Bezeichnen von normalen (positiven) und negativen Instanzen der Entität, das Vermitteln basierend auf positiven und negativen Modellen sowie das Löschen des Werts einer vorhandenen Entität. Entitäten, bei denen die Eigenschaften „Negierbar“ festgelegt ist, werden in Conversation Learner als „Verneinbare Entitäten“ bezeichnet.

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie auf der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „NegatableEntity“, und drücken Sie die EINGABETASTE.
3. Klicken Sie auf die Schaltfläche „Erstellen“.

### <a name="entity-creation"></a>Erstellung von Entitäten

1. Klicken Sie im linken Bereich auf „Entitäten“ und dann auf die Schaltfläche „Neue Entität“.
2. Wählen Sie unter „Entitätstyp“ die Option „Benutzerdefiniert“ aus.
3. Geben Sie als Entitätsname „name“ (Name) ein.
4. Aktivieren Sie das Kontrollkästchen „Negierbar".
    - Das Aktvieren dieser Eigenschaft ermöglicht es dem Benutzer, einen Entitätswert bereitzustellen oder etwas zu sagen, das *kein* Entitätswert ist. In letzterem Fall ist das Ergebnis die Löschung des entsprechenden Entitätswerts.
5. Klicken Sie auf die Schaltfläche „Erstellen“.

![](../media/tutorial5_entities.PNG)

### <a name="create-the-first-action"></a>Erstellen der ersten Aktion

1. Klicken Sie im linken Bereich auf „Aktionen“ und anschließend auf die Schaltfläche „Neue Aktion“.
2. Geben Sie im Feld „Bot's response...“ (Antwort des Bots...) die Zeichenfolge „I don't know your name.“ (Ich kenne Ihren Namen nicht.) ein.
3. Geben Sie im Feld „Disqualifying Entities“ (Disqualifizierende Entitäten) die Zeichenfolge „name“ (Name) ein.
4. Klicken Sie auf die Schaltfläche „Erstellen“.

### <a name="create-the-second-action"></a>Erstellen der zweiten Aktion

1. Klicken Sie im linken Bereich auf „Aktionen“ und anschließend auf die Schaltfläche „Neue Aktion“.
2. Geben Sie im Feld „Bot's response...“ (Antwort des Bots...) die Zeichenfolge „I know your name. It is $name.“ (Ich kenne Ihren Namen. Er lautet $name) ein.
3. Klicken Sie auf die Schaltfläche „Erstellen“.

> [!NOTE]
> Die Entität „Name“ wurde durch einen Verweis in der Antwort automatisch den erforderlichen Entitäten hinzugefügt.

Jetzt sind zwei Aktionen vorhanden.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-model"></a>Trainieren des Modells

1. Klicken Sie im linken Bereich auf „Train Dialogs“ (Trainingsdialoge) und anschließend auf die Schaltfläche „New Train Dialog“ (Neuer Trainingsdialog).
2. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „hello“ (Hallo) ein.
3. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
4. Wählen Sie die Antwort „I don't know your name.“ (Ich kenne Ihren Namen nicht.) aus.
    - Das Perzentil beträgt 100 Prozent (als einzige gültige Aktion auf der Grundlage der Einschränkungen).
5. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „My name is Frank“ (Mein Name ist Frank) ein.
6. Wählen Sie „Frank“ aus, und wählen Sie die Bezeichnung „+name“ aus.
    - Es gibt zwei Instanzen die Entität „Name“: „+name“ und „-name“.  (+) Plus fügt den Wert hinzu oder überschreibt ihn. (-) Minus entfernt den Wert.
7. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
    - Die Entität „Name“ ist im Speicher des Modells jetzt als „Frank“ definiert, daher ist die Aktion „I know your name. It is $name“ (Ich kenne Ihren Namen. Er lautet $name) verfügbar.
8. Wählen Sie die Antwort „I know your name. It is $name.“ (Ich kenne Ihren Namen. Er lautet $name) aus.
9. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „My name is not Frank“ (Mein Name ist nicht Frank) ein.
10. Wählen Sie „Frank“ aus, und wählen Sie die Bezeichnung „-name“ aus.
    - Wir wählen „-name“, um den aktuellen Wert der Entität zu löschen.
11. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
12. Wählen Sie die Antwort „I don't know your name.“ (Ich kenne Ihren Namen nicht.) aus.
13. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „My name is Susan“ (Mein Name ist Susan) ein.
14. Wählen Sie „Susan“ aus, und wählen Sie die Bezeichnung „+name“ aus.

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Mehrwertige Entitäten](./07-multi-value-entities.md)
