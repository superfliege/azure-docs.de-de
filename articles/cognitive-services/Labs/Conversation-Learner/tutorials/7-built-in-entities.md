---
title: Hinzufügen von vorgefertigten Entitäten zu einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie vorgefertigte Entitäten in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2dbbf2a47cdc4240e5b0ba38658a4cb8d5307ff8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260056"
---
# <a name="how-to-add-pre-built-entities"></a>Hinzufügen von vorgefertigten Entitäten
In diesem Tutorial erfahren Sie, wie Sie „vorgefertigte“ Entitäten zu Ihrem Unterhaltungslernmodell hinzufügen.

## <a name="video"></a>Video

[![Tutorial 7 – Vorschau](https://aka.ms/cl-tutorial-07-preview)](https://aka.ms/blis-tutorial-07)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

Vorgefertigte Entitäten erkennen häufig verwendete Entitätstypen wie Nummern, Datumsangaben, Geldbeträge usw.  Im Gegensatz zu benutzerdefinierten Entitäten sind diese vorgefertigt und erfordern kein Training.  Im Gegensatz zu benutzerdefinierten Entitäten kann deren Verhalten nicht geändert werden.  Standardmäßig weisen vorgefertigte Entitäten mehrere Werte auf. D.h., der Speicher des Bots sammelt alle erkannten Instanzen der Entität.

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie in der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie unter „Name“ „BuiltInEntities“ ein. Klicken Sie dann auf „Erstellen“.

### <a name="create-an-entity"></a>Erstellen einer Entität

1. Klicken Sie erst auf „Entitäten“ und dann auf „Neue Entität“.
2. Klicken Sie in der Dropdownliste erst auf „EntityType“ und anschließend auf „datetimev2“.
    - Programmierbare und negierbare Optionen werden deaktiviert, weil sie nicht für vorgefertigte Entitäten gelten.
3. Klicken Sie auf „Erstellen“.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>Erstellen von zwei Aktionen

1. Klicken Sie erst auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie in das Antwortfeld „The date is $luis-datetimev2“ (Das Datum ist „$luis-datetimev2“) ein.
3. Klicken Sie auf „Erstellen“.

![](../media/tutorial7_actions.PNG)

Erstellen Sie dann die zweite Aktion:

1. Klicken Sie erst auf „Aktionen“ und dann auf „Neue Aktion“, um eine zweite Aktion zu erstellen.
3. Geben Sie in das Antwortfeld die Frage „Wie lautet das Datum?“ ein.
4. Geben Sie in das Feld „Disqualifizierende Entitäten“ „luis-datetimev2“ ein.
4. Klicken Sie auf „Erstellen“.

![](../media/tutorial7_actions2.PNG)

Jetzt sind zwei Aktionen vorhanden.

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie erst auf „Bewertungsaktionen“ und dann auf „Wie lautet das Datum?“.
2. Geben Sie „heute“ ein. 
    - Beachten Sie, dass „heute“ markiert ist und in der zweiten Zeile angezeigt wird, da es sich dabei um eine vorgefertigte Entität und nicht um eine nicht editierbare Entität handelt.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass das Datum jetzt im Abschnitt „Entitätsspeicher“ angezeigt wird. 
    - Wenn Sie mit dem Mauszeiger über das Datum fahren, werden Ihnen die zusätzlichen von LUIS zur Verfügung gestellten Daten angezeigt, die verwendet und in Code umgewandelt werden können. 
6. Klicken Sie auf „The date is $luis-datetimev2“ (Das Datum ist „$luis-datetimev2“).
7. Klicken Sie auf „Training abgeschlossen“.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Alternative Eingaben](./8-alternative-inputs.md)
