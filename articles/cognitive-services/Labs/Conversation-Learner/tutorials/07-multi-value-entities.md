---
title: Verwenden von mehrwertigen Entitäten in einem Conversation Learner-Modell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie mehrwertige Entitäten in einem Conversation Learner-Modell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8544d63f38f88a0e623dff343bf8b5133931b70b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228303"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Verwenden von mehrwertigen Entitäten in einem Unterhaltungslernmodell
In diesem Tutorial wird die mehrwertige Eigenschaft von Entitäten behandelt.

## <a name="video"></a>Video

[![Mehrwertige Entitäten: Tutorial (Vorschauversion)](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details
Mehrwertige Entitäten akkumulieren Werte in einer Liste, anstatt einen einzelnen Wert zu speichern.  Diese Entitäten sind hilfreich, wenn Benutzer mehrere Werte angeben können. Ein Beispiel wäre etwa die Zusammenstellung einer Pizza.

Bei als mehrwertig markierten Entitäten werden die einzelnen erkannten Instanzen der Entität jeweils einer Liste im Speicher des Bots hinzugefügt. Weitere Erkennungen werden an den Wert der Entität angefügt, anstatt ihn zu überschreiben.

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie auf der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „MultiValueEntities“ ein, und drücken Sie die EINGABETASTE.
3. Klicken Sie auf die Schaltfläche „Erstellen“.

### <a name="entity-creation"></a>Erstellung von Entitäten

1. Klicken Sie im linken Bereich auf „Entitäten“ und dann auf die Schaltfläche „Neue Entität“.
2. Wählen Sie unter „Entitätstyp“ die Option „Custom Trained“ (Benutzerdefiniert trainiert) aus.
3. Geben Sie als Entitätsname „toppings“ (Beläge) ein.
4. Aktivieren Sie das Kontrollkästchen „Mehrwertig“.
    - Mehrwertige Entitäten kumulieren Werte in der Entität.
5. Aktivieren Sie das Kontrollkästchen „Negierbar".
    - Die Negierbarkeit wurde in einem anderen Tutorial behandelt.
6. Klicken Sie auf die Schaltfläche „Erstellen“.

![](../media/tutorial6_entities.PNG)

### <a name="create-the-first-action"></a>Erstellen der ersten Aktion

1. Klicken Sie im linken Bereich auf „Aktionen“ und anschließend auf die Schaltfläche „Neue Aktion“.
2. Geben Sie im Feld „Bot's response...“ (Antwort des Bots...) die Antwort „Here are your toppings: $toppings“ (Hier sind Ihre Beläge: $toppings) ein.
    - Das führende Dollarzeichen gibt an, dass es sich um einen Entitätsverweis handelt.
3. Klicken Sie auf die Schaltfläche „Erstellen“.

### <a name="create-the-second-action"></a>Erstellen der zweiten Aktion

1. Klicken Sie im linken Bereich auf „Aktionen“ und anschließend auf die Schaltfläche „Neue Aktion“.
2. Geben Sie im Feld „Bot's response...“ (Antwort des Bots...) die Antwort „What toppings would you like?“ (Welche Beläge möchten Sie?) ein.
3. Geben Sie im Feld „Disqualifying Entities“ (Disqualifizierende Entitäten) die Zeichenfolge „toppings“ (Beläge) ein.
4. Klicken Sie auf die Schaltfläche „Erstellen“.

Jetzt sind zwei Aktionen vorhanden.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-model"></a>Trainieren des Modells

1. Klicken Sie im linken Bereich auf „Train Dialogs“ (Trainingsdialoge) und anschließend auf die Schaltfläche „New Train Dialog“ (Neuer Trainingsdialog).
2. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „hi“ ein.
3. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
4. Wählen Sie die Antwort „What toppings would you like?“ (Welche Beläge möchten Sie?) aus.
    - Das Perzentil beträgt 100 Prozent (als einzige gültige Aktion auf der Grundlage der Einschränkungen).
5. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „cheese and mushrooms“ (Käse und Pilze) ein.
6. Klicken Sie auf „cheese“ (Käse), und wählen Sie „+toppings“ aus.
7. Klicken Sie auf „mushrooms“ (Pilze), und wählen Sie „+toppings“ aus.
8. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
9. Wählen Sie die Antwort „Here are your toppings: $toppings“ (Hier sind Ihre Beläge: $toppings) aus.
10. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „add pepper“ (Paprika hinzufügen) ein.
11. Klicken Sie auf „pepper“ (Paprika), und wählen Sie „+toppings“ aus.
12. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
13. Wählen Sie die Antwort „Here are your toppings: $toppings“ (Hier sind Ihre Beläge: $toppings) aus.
14. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „remove cheese“ (Käse entfernen) ein.
15. Klicken Sie auf „cheese“ (Käse), und wählen Sie „-toppings“ aus.
16. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
17. Wählen Sie die Antwort „Here are your toppings: $toppings“ (Hier sind Ihre Beläge: $toppings) aus.

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vortrainierte Entitäten](./08-pre-trained-entities.md)
