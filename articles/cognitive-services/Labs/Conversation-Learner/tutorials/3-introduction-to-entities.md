---
title: Verwenden von Entitäten in einer Unterhaltungslernanwendung – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie Entitäten in einer Unterhaltungslernanwendung verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 85df31c2e2ff3ca81698921a1f17f415daefb6c5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376234"
---
# <a name="introduction-to-entities"></a>Einführung in Entitäten

In diesem Tutorial erhalten Sie eine Einführung in Entitäten, und es wird erörtert, wie die Felder „Disqualifizierende Entitäten“ und „Erforderliche Entitäten“ in Aktionen verwendet werden.

## <a name="requirements"></a>Anforderungen

Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

In diesem Tutorial werden zwei häufige Verwendungsmöglichkeiten für Entitäten veranschaulicht.  Erstens können Entitäten Teilzeichenfolgen aus einer Benutzernachricht extrahieren. Ein Beispiel hierfür ist die Identifizierung einer Stadt in „Wie ist das Wetter in Frankfurt?“.  Zweitens können Entitäten einschränken, wenn Aktionen verfügbar sind.  Insbesondere kann eine Aktion eine Entität als „erforderliche“ oder „disqualifizierende“ Entität auflisten:
- Die erforderlichen Entitäten einer Aktion müssen im Speicher des Bots vorhanden sein, damit die Aktion verfügbar ist.
- Die disqualifizierenden Entitäten müssen *nicht* im Speicher des Bots vorhanden sein, damit die Aktion verfügbar ist.

In anderen Tutorials werden andere Aspekte von Entitäten behandelt, z. B. vorgefertigte Entitäten, mehrwertige Entitäten, negierbare Entitäten, programmgesteuerte Entitäten und das Bearbeiten von Entitäten im Code.

## <a name="steps"></a>Schritte

### <a name="create-the-application"></a>Erstellen der Anwendung

1. Klicken Sie auf der Webbenutzeroberfläche auf „Neue App“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „IntroToEntities“ ein. Klicken Sie dann auf „Erstellen“.

### <a name="create-entity"></a>Erstellen der Entität

1. Klicken Sie auf „Entitäten“ und dann auf „Neue Entität“.
2. Geben Sie im Feld „Entitätsname“ die Bezeichnung „Stadt“ ein.
3. Klicken Sie auf „Erstellen“.

Beachten Sie, dass der Entitätstyp „benutzerdefiniert“ ist. Dies bedeutet, dass die Entität trainiert werden kann.  Es gibt auch vorgefertigte Entitäten, was bedeutet, dass ihr Verhalten nicht angepasst werden kann. Diese Entitäten werden in einem anderen Tutorial behandelt.

### <a name="create-two-actions"></a>Erstellen von zwei Aktionen

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie im Feld „Antwort“ den Satz „Ich weiß nicht, welche Stadt Sie möchten“ ein.
3. Geben Sie im Feld „Disqualifizierende Entitäten“ die Zeichenfolge „$Stadt“ ein. Klicken Sie auf „Speichern“.
    - Wenn diese Entität im Speicher des Bots definiert ist, steht diese Aktion *nicht* zur Verfügung.
2. Klicken Sie auf „Aktionen“, und klicken Sie dann auf „Neue Aktion“, um eine zweite Aktion zu erstellen.
3. Geben Sie im Feld „Antwort“ den Satz „Das Wetter in $Stadt ist wahrscheinlich sonnig“ ein.
4. Beachten Sie unter „Erforderliche Entitäten“, dass die Entität „Stadt“ automatisch hinzugefügt wurde, weil darauf verwiesen wurde.
5. Klicken Sie auf Speichern.

Jetzt sind zwei Aktionen vorhanden.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Ich weiß nicht, welche Stadt Sie möchten“ aus.
    - Beachten Sie, dass die Antwort, in der die Entität „Stadt“ erforderlich ist, nicht ausgewählt werden kann, weil die Entität „Stadt“ nicht im Speicher des Bots definiert ist.
2. Wählen Sie „Ich weiß nicht, welche Stadt Sie möchten“ aus.
4. Geben Sie „Frankfurt“ ein. Markieren Sie „Frankfurt“, und klicken Sie dann auf „Stadt“.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass der Wert „Stadt“ jetzt im Speicher des Bots vorhanden ist.
    - „Das Wetter in $Stadt ist wahrscheinlich sonnig“ ist jetzt als Antwort verfügbar. 
6. Wählen Sie „Das Wetter in $Stadt ist wahrscheinlich sonnig“ aus.

Nehmen wir an, der Benutzer gibt „Wiederholen Sie das“ ein. 
1. Geben Sie das ein. Beachten Sie, dass die Entität „Stadt“ und der zugehörige Wert im Speicher vorhanden und verfügbar sind.
2. Wählen Sie „Das Wetter in $Stadt ist wahrscheinlich sonnig“ aus.

![](../media/tutorial3_entities.PNG)

Sie haben jetzt eine Entität erstellt und Instanzen davon in Benutzernachrichten bezeichnet.  Außerdem haben Sie das Vorhandensein/Fehlen der Entität im Speicher des Bots verwendet, um über die Felder „Disqualifizierende Entitäten“ und „Erforderliche Entitäten“ einer Aktion zu steuern, wann Aktionen verfügbar sind.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erwartete Entität](./4-expected-entity.md)
