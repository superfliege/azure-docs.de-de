---
title: Karten in einem Unterhaltungslernmodell, Teil 2 – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Verwenden von Karten in einem Unterhaltungslernmodell
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1c7c88742c69041594006add76f7e3c642c64dec
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170571"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Verwenden von Karten (Teil 1 von 2)
In diesem Tutorial wird gezeigt, wie Sie Ihrem Bot eine ausfüllbare Formularkarte hinzufügen. Es wird veranschaulicht, wie Formularfelder in Entitäten verschoben werden.

Das Unterhaltungslernmodul geht davon aus, dass Ihre Kartendefinitionsdateien in einem Verzeichnis namens „cards“ gespeichert sind, das sich in dem Verzeichnis befindet, in dem der Bot gestartet wird.

## <a name="video"></a>Video

[![Tutorial 14 – Vorschau](http://aka.ms/cl-tutorial-14-preview)](http://aka.ms/blis-tutorial-14)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

Karten sind Benutzeroberflächenelemente, mit denen Benutzer eine Option in der Unterhaltung auswählen können. 

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der Modellliste der Webbenutzeroberfläche auf „Tutorial-14-Cards-2“. 

### <a name="the-card"></a>Die Karte

Die Kartendefinition befindet sich am folgenden Speicherort: C:\<Installationspfad\>\src\cards\shippingAddress.json.

Diese Karte erfasst drei Felder der Lieferanschrift: Ort, Straße und Bundesland.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Actions

Wir haben drei Aktionen erstellt. Wie Sie unten sehen, ist die erste Aktion eine Karte.

![](../media/tutorial14_actions.PNG)

Der Aktionstyp „Karte“ wurde wie folgt erstellt:

- Beachten Sie „Address-Street“ vom Typ „Input.text“ und zugehörige ID.
- Ebenso gibt es „Address-City“ und ein Dropdown mit der ID von „Address-State“.

Die IDs sind wichtig, denn wenn die Felder aufgefüllt und übermittelt werden, sind das die Entitätsnamen, die diese Werte im Bot erhalten.

## <a name="entities"></a>Entitäten
Wir haben drei Entitäten definiert, die mit der Karte übereinstimmen, wie oben veranschaulicht.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Actions

Wir haben zwei Aktionen definiert.

![](../media/tutorial14_actions.PNG)

- Die erste Entität ist die Karte „Lieferanschrift“. Der Aktionstyp ist „KARTE“, und im Dropdown ist „Vorlage“ als „shippingAddress“ ausgewählt.
- Die zweite Entität ist eine einfache Aktion, um die Lieferanschrift einzulesen.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Trainingsdialog

Im Folgenden sehen wir uns einen Trainingsdialog an.

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
1. Geben Sie „Hi“ ein.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Lieferanschrift“.
4. Füllen Sie die Karte aus, und übermitteln Sie sie.
    - Beachten Sie, dass diese Werte jetzt in den Entitätsspeicher verschoben wurden. Es ist keine Analyse erforderlich, weil das Formular die Eingaben bereits partitioniert hat.
5. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Liefern an $Adresse“.
4. Klicken Sie auf „Test abgeschlossen“.

![](../media/tutorial14_train_dialog.PNG)

Sie wissen jetzt, wie Sie Werte von der Karte mit ausfüllbaren Feldern und Dropdowns abrufen und diese in Botentitäten erfassen und sammeln können.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Branchen und Rückgängigmachen](./15-branching-and-undo.md)
