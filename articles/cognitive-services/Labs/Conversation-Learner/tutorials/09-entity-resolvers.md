---
title: Entitäts-Resolver in einem Conversation Learner-Modell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie Entitäts-Resolver in Conversation Learner verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e6a671470a87f4509e466bcdfe7845b7bf7ec8dc
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209147"
---
# <a name="entity-resolvers"></a>Entitäts-Resolver

Dieses Tutorial zeigt, wie Sie Entitäts-Resolver in Conversation Learner verwenden.

## <a name="video"></a>Video

[![Tutorial zu Entitäts-Resolvern (Vorschauversion)](https://aka.ms/cl_Tutorial_v3_EntityResolvers_Preview)](https://aka.ms/cl_Tutorial_v3_EntityResolvers)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

- Der Resolvertyp ist eine optionale Eigenschaft von benutzerdefinierten Entitäten.
- Entity-Resolver nutzen die Leistungsfähigkeit von vortrainierten Entitätserkennungen in LUIS, um weitere Details und bessere Übersichtlichkeit für Ihre benutzerdefinierte Entität bereitzustellen.

## <a name="steps"></a>Schritte

### <a name="create-a-new-model"></a>Erstellen eines neuen Modells

1. Klicken Sie auf der Webbenutzeroberfläche auf die Schaltfläche „Neues Modell“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „Entitäts-Resolver“ ein, und drücken Sie die EINGABETASTE, oder klicken Sie auf die Schaltfläche „Erstellen“.

### <a name="create-a-pair-of-entities"></a>Erstellen eines Paars von Entitäten

1. Klicken Sie im linken Bereich auf „Entitäten“ und dann auf die Schaltfläche „Neue Entität“.
2. Geben Sie im Feld „Entitätsname“ den Text „Abreise“ ein.
3. Wählen Sie in der Dropdownliste „Resolvertyp“ die Option „datetimeV2“ aus.
4. Klicken Sie auf die Schaltfläche „Erstellen“.
5. Klicken Sie auf die Schaltfläche „OK“, nachdem Sie die Informationen im Popup gelesen haben.
6. Erstellen Sie mit den gleichen Schritten eine weitere Entität mit dem Namen „Rückreise“, die auch den Resolvertyp „datetimeV2“ aufweist.

### <a name="create-a-pair-of-actions"></a>Erstellen eines Paars von Aktionen

1. Klicken Sie im linken Bereich auf „Aktionen“ und anschließend auf die Schaltfläche „Neue Aktion“.
2. Geben Sie im Feld „Antwort des Bots“ den Text „Sie reisen am $departure ab und kehren am $return zurück“ ein.
    - WICHITG: Bei der Eingabe von „$[entityName]“ müssen Sie die EINGABETASTE drücken oder in der Dropdownliste auf die Entität klicken. Andernfalls erkennt Conversation Learner die Eingabe als Text anstelle einer Entität.
    - Beachten Sie, dass das Feld „Erforderliche Entitäten“ diese Entitäten ebenfalls erhält, und sie können nicht entfernt werden. Dies verhindert, dass diese Aktion verfügbar wird, bevor beide erforderlichen Entitäten vorhanden sind.
3. Klicken Sie auf die Schaltfläche „Erstellen“.
4. Klicken Sie erneut auf die Schaltfläche „Neue Aktion“, um eine zweite Aktion zu erstellen.
5. Geben Sie im Feld „Antwort des Bots“ den Text „Wann möchten Sie reisen?“ ein.
6. Geben Sie im Feld „Disqualifizierende Entitäten“ den Text „Abreise“ und „Rückreise“ ein.
    - Dadurch wird der Bot angewiesen, diese Aktion NICHT durchzuführen, wenn eine dieser Entitäten einen Wert enthält.
7. Klicken Sie auf die Schaltfläche „Erstellen“.


### <a name="training"></a>Training

1. Sehen Sie sich den Fortschritt von „Training: [Status]“ auf der Seite oben links an, und warten Sie, bis das Training abgeschlossen ist.
    - Wenn dies zu lange dauert, können Sie auf den Link „Aktualisieren“ klicken.
    - Der Trainingsstatus „Abgeschlossen“, ist erforderlich, damit die Entitäts-Resolver funktionieren, wenn wir das Modell trainieren.
2. Klicken Sie im linken Bereich auf „Train Dialogs“ (Trainingsdialoge) und anschließend auf die Schaltfläche „New Train Dialog“ (Neuer Trainingsdialog).
3. Geben Sie die erste Benutzeräußerung „Flug buchen“ ein. 
4. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
5. Wählen Sie die Antwort „Wann möchten Sie reisen?“ aus.
6. Antworten Sie als Benutzer mit „Morgen abreisen und Sonntag nächste Woche zurückkommen“.
    - Beachten Sie, dass Conversation Learner in diesem Benutzerdurchlauf zwei Entitäten mit vortrainiertem Datum erkannt hat.
7. Wählen Sie im Bereich „Entitätserkennung“ den Text „morgen“ aus, und markieren Sie ihn als „Abreise“.
8. Bezeichnen Sie auch den Text „Sonntag nächste Woche“ als „Rückreise“.
9. Klicken Sie auf die Schaltfläche „Score Actions“ (Bewertungsaktionen).
    - Beachten Sie, dass der Bereich „Speicher“ Ihre Ab- und Rückreisedaten enthält.
    - Zeigen Sie mit der Maus auf jedes Datum. Sie sehen, dass die Entitäten Datumsobjekte sind, die im Gegensatz zu „Sonntag“ oder „morgen“ das tatsächliche Kalenderdatum genau erfassen.
10. Wählen Sie die Botantwort „Sie reisen am ... ab“ aus.
11. Klicken Sie auf die Schaltfläche „Speichern“.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Alternative Eingaben](./10-alternative-inputs.md)
