---
title: Hinzufügen vortrainierter Entitäten zu einem Conversation Learner-Modell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie vortrainierte Entitäten in einem Conversation Learner-Modell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c198dfc19a350188f500af86c531be9a9ac424ce
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796479"
---
# <a name="how-to-add-pre-trained-entities"></a>Hinzufügen vortrainierter Entitäten
In diesem Tutorial erfahren Sie, wie Sie Ihrem Conversation Learner-Modell vortrainierte Entitäten hinzufügen.

## <a name="video"></a>Video

[![Vortrainierte Entitäten: Tutorial (Vorschauversion)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Anforderungen
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

Vortrainierte Entitäten erkennen häufig verwendete Entitätstypen wie Nummern, Datumsangaben, Geldbeträge und Ähnliches.  Sie können direkt verwendet werden, müssen nicht trainiert werden, und ihr Verhalten kann im Gegensatz zum Verhalten benutzerdefinierter Entitäten nicht geändert werden.  Vortrainierte Entitäten sind standardmäßig mehrwertig und kumulieren jede erkannte Instanz der Entität.

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie auf der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „PretrainedEntities“ ein, und drücken Sie die EINGABETASTE.
3. Klicken Sie auf die Schaltfläche „Erstellen“.

### <a name="entity-creation"></a>Entitätserstellung

1. Klicken Sie im linken Bereich auf „Entitäten“ und dann auf die Schaltfläche „Neue Entität“.
2. Wählen Sie als Entitätstyp die Option „Pre-Trained/datetimeV2“ aus.
3. Aktivieren Sie das Kontrollkästchen „Mehrwertig“.
    - Mehrwertige Entitäten kumulieren Werte in der Entität.
    - Negierbare Eigenschaften sind bei vortrainierten Entitäten deaktiviert.
4. Klicken Sie auf die Schaltfläche „Erstellen“.

![](../media/tutorial7_entities_a.PNG)

### <a name="create-the-first-action"></a>Erstellen der ersten Aktion

1. Klicken Sie im linken Bereich auf „Aktionen“ und anschließend auf die Schaltfläche „Neue Aktion“.
2. Geben Sie im Feld „Bot's response...“ (Antwort des Bots...) die Zeichenfolge „The date is $builtin-datetimev2“ (Das Datum lautet $builtin-datetimev2) ein.
3. Klicken Sie auf die Schaltfläche „Erstellen“.

![](../media/tutorial7_actions_a.PNG)

### <a name="create-the-second-action"></a>Erstellen der zweiten Aktion

1. Klicken Sie im linken Bereich auf „Aktionen“ und anschließend auf die Schaltfläche „Neue Aktion“.
2. Geben Sie im Feld „Bot's response...“ (Antwort des Bots...) den Satz „What's the date?“ (Wie lautet das Datum?) ein.
    - Vortrainierte Entitäten können keine erforderlichen Entitäten sein, da sie standardmäßig für alle Benutzeräußerungen erkannt werden.
3. Geben Sie im Feld „Disqualifying Entities“ (Disqualifizierende Entitäten) die Zeichenfolge „builtin-datetimev2“ ein.
4. Klicken Sie auf die Schaltfläche „Erstellen“.

![](../media/tutorial7_actions2_a.PNG)

### <a name="train-the-model"></a>Trainieren des Modells

1. Klicken Sie im linken Bereich auf „Train Dialogs“ (Trainingsdialoge) und anschließend auf die Schaltfläche „New Train Dialog“ (Neuer Trainingsdialog).
2. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „hello“ (Hallo) ein.
3. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
4. Wählen Sie die Antwort „What's the date?“ (Wie lautet das Datum?) aus.
5. Geben Sie im Chatbereich unter „Type your message...“ (Nachricht eingeben...) die Zeichenfolge „today“ (heute) ein.
    - Die Äußerung „today“ (heute) wird von vortrainierten Modellen in LUIS automatisch erkannt.
    - Wenn Sie auf Werte vortrainierter Entitäten zeigen, werden zusätzliche, von LUIS bereitgestellte Daten angezeigt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entitäts-Resolver](./09-entity-resolvers.md)
