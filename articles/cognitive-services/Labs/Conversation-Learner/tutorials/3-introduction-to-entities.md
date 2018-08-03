---
title: Verwenden von Entitäten in einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie Entitäten in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f851d43d69999a848dea01c9457a379adb63353b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172380"
---
# <a name="introduction-to-entities"></a>Einführung in Entitäten

In diesem Tutorial erhalten Sie eine Einführung in Entitäten, und es wird erörtert, wie die Felder „Disqualifizierende Entitäten“ und „Erforderliche Entitäten“ in Aktionen verwendet werden.

## <a name="video"></a>Video

[![Tutorial 3 – Vorschau](http://aka.ms/cl-tutorial-03-preview)](http://aka.ms/blis-tutorial-03)

## <a name="requirements"></a>Requirements (Anforderungen)

Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

In diesem Tutorial werden zwei häufige Verwendungsmöglichkeiten für Entitäten veranschaulicht.  Erstens können Entitäten Teilzeichenfolgen aus einer Benutzernachricht extrahieren. Ein Beispiel hierfür ist die Identifizierung einer Stadt in „Wie ist das Wetter in Frankfurt?“.  Zweitens können Entitäten einschränken, wenn Aktionen verfügbar sind.  Insbesondere kann eine Aktion eine Entität als „erforderliche“ oder „disqualifizierende“ Entität auflisten:
- Die erforderlichen Entitäten einer Aktion müssen im Speicher des Bots vorhanden sein, damit die Aktion verfügbar ist.
- Die disqualifizierenden Entitäten müssen *nicht* im Speicher des Bots vorhanden sein, damit die Aktion verfügbar ist.

In anderen Tutorials werden andere Aspekte von Entitäten behandelt, z. B. vorgefertigte Entitäten, mehrwertige Entitäten, negierbare Entitäten, programmgesteuerte Entitäten und das Bearbeiten von Entitäten im Code.

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie in der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „IntroToEntities“ ein. Klicken Sie dann auf „Erstellen“.

### <a name="create-entity"></a>Erstellen der Entität

1. Klicken Sie auf „Entitäten“ und dann auf „Neue Entität“.
2. Geben Sie im Feld „Entitätsname“ die Bezeichnung „Stadt“ ein.
3. Klicken Sie auf „Erstellen“.

> [!NOTE]
> Der Entitätstyp ist „benutzerdefiniert“ ist. Dies bedeutet, dass die Entität trainiert werden kann.  Es gibt auch vorgefertigte Entitäten, was bedeutet, dass ihr Verhalten nicht angepasst werden kann. Diese Entitäten werden in einem anderen Tutorial behandelt.

### <a name="create-two-actions"></a>Erstellen von zwei Aktionen

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie im Feld „Antwort“ den Satz „Ich weiß nicht, welche Stadt Sie möchten“ ein.
3. Geben Sie im Feld „Disqualifizierende Entitäten“ die Zeichenfolge „$city“ ein. Klicken Sie auf Speichern.
    - Wenn diese Entität im Speicher des Bots definiert ist, steht diese Aktion *nicht* zur Verfügung.
2. Klicken Sie auf „Aktionen“, und klicken Sie dann auf „Neue Aktion“, um eine zweite Aktion zu erstellen.
3. Geben Sie im Feld „Antwort“ den Satz „Das Wetter in $Stadt ist wahrscheinlich sonnig“ ein.
4. Unter „Erforderliche Entitäten“ wurde die Entität „Stadt“ automatisch hinzugefügt, weil darauf verwiesen wurde.
5. Klicken Sie auf Speichern.

Jetzt sind zwei Aktionen vorhanden.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Ich weiß nicht, welche Stadt Sie möchten“ aus.
    - Die Antwort, in der die Entität „Stadt“ erforderlich ist, kann nicht ausgewählt werden, weil die Entität „Stadt“ nicht im Speicher des Bots definiert ist.
2. Wählen Sie „Ich weiß nicht, welche Stadt Sie möchten“ aus.
4. Geben Sie „Frankfurt“ ein. Markieren Sie „Frankfurt“, und klicken Sie dann auf „Stadt“.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Der Wert „Stadt“ ist jetzt im Speicher des Bots vorhanden.
    - „Das Wetter in $Stadt ist wahrscheinlich sonnig“ ist jetzt als Antwort verfügbar. 
6. Wählen Sie „Das Wetter in $Stadt ist wahrscheinlich sonnig“ aus.

Nehmen wir an, der Benutzer gibt „Wiederholen Sie das“ ein. 
1. Geben Sie das ein. Die Entität „Stadt“ und der zugehörige Wert sind im Speicher vorhanden und verfügbar.
2. Wählen Sie „Das Wetter in $Stadt ist wahrscheinlich sonnig“ aus.

![](../media/tutorial3_entities.PNG)

Sie haben jetzt eine Entität erstellt und Instanzen davon in Benutzernachrichten bezeichnet.  Außerdem haben Sie das Vorhandensein/Fehlen der Entität im Speicher des Bots verwendet, um über die Felder „Disqualifizierende Entitäten“ und „Erforderliche Entitäten“ einer Aktion zu steuern, wann Aktionen verfügbar sind.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erwartete Entität](./4-expected-entity.md)
