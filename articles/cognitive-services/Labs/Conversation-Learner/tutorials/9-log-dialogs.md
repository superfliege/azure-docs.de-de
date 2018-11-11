---
title: Protokollieren von Dialogen in einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie Dialoge in einem Unterhaltungslernmodell protokollieren.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 5369e16e0f1adc48eb019f3790dc900577c144af
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243658"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>Protokollieren von Dialogen in einem Unterhaltungslernmodell

In diesem Tutorial wird gezeigt, wie Sie Endbenutzertests innerhalb der Unterhaltungslernschnittstelle durchführen und Dialoge protokollieren. Außerdem erfahren Sie, wie Sie Korrekturen an protokollierten Dialogen vornehmen, um Ihr Modell zu verbessern.

## <a name="video"></a>Video

[![Tutorial 9 – Vorschau](https://aka.ms/cl-tutorial-09-preview)](https://aka.ms/blis-tutorial-09)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details
Mithilfe der Dialogprotokollierung können Sie Dialoge, die mit Endbenutzern geführt werden, überprüfen und Korrekturen daran vornehmen.  Insbesondere können Sie Entitätsbezeichnungen und die Aktionsauswahl korrigieren, um die Leistung des trainierten Modells und des Gesamtsystems zu steigern. 

## <a name="steps"></a>Schritte

### <a name="create-the-model"></a>Erstellen des Modells

1. Klicken Sie in der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ die Bezeichnung „Dialogprotokoll“ ein. Klicken Sie dann auf „Erstellen“.

### <a name="create-an-entity"></a>Erstellen einer Entität

1. Klicken Sie auf „Entitäten“ und dann auf „Neue Entität“.
2. Geben Sie im Feld „Entitätsname“ die Bezeichnung „Stadt“ ein.
3. Klicken Sie auf „Erstellen“.

### <a name="create-two-actions"></a>Erstellen von zwei Aktionen

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie im Feld „Antwort“ die Frage „Welche Stadt?“ ein.
3. Geben Sie im Feld „Disqualifizierende Entitäten“ die Zeichenfolge „$city“ ein.
3. Klicken Sie auf „Erstellen“.

Erstellen Sie dann die zweite Aktion:

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
3. Geben Sie im Feld „Antwort“ den Satz „Das Wetter in $Stadt ist wahrscheinlich sonnig“ ein.
4. Geben Sie als erforderliche Entitäten die Zeichenfolge „$city“ ein.
4. Klicken Sie auf „Erstellen“.

Jetzt sind zwei Aktionen vorhanden.

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Wie ist das Wetter“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Welche Stadt?“ aus.
2. Geben Sie „Seattle“ ein.
3. Doppelklicken Sie auf „Seattle“, und wählen Sie „Stadt“ aus.
    - Dadurch wird der Eintrag als eine Stadtentität gekennzeichnet.
5. Klicken Sie auf „Bewertungsaktionen“.
6. Wählen Sie „Das Wetter in $Stadt ist wahrscheinlich sonnig“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

Fügen Sie einen weiteren Beispieldialog hinzu:

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „Wie ist das Wetter in Seattle?“ ein. Beachten Sie, dass Seattle als eine Entität gekennzeichnet ist.
5. Klicken Sie auf „Bewertungsaktionen“. 
6. Wählen Sie „Das Wetter in $Stadt ist wahrscheinlich sonnig“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

### <a name="try-the-bot-as-the-user"></a>Testen des Bots als Benutzer
Hier wird davon ausgegangen, dass dieser Bot für Benutzer bereitgestellt wurde.

1. Klicken Sie auf „Dialoge protokollieren“.
2. Klicken Sie auf „Neuen Dialog protokollieren“.
    - Dadurch wird der Bot so dargestellt, wie er dem Benutzer im Webchat-Steuerelement auf der linken Seite der Benutzeroberfläche angezeigt würde. Den weißen Bereich auf der rechten Seite können Sie ignorieren.
3. Geben Sie „hallo“ ein.
4. Botantwort: „welche Stadt?“
4. Geben Sie „Boston“ ein.
5. Botantwort: „welche Stadt?“
    - Das scheint nicht richtig zu sein. Also speichern Sie diesen Dialog.
2. Klicken Sie auf „Test abgeschlossen“.

Starten Sie eine neue Sitzung:

2. Klicken Sie auf „Neuen Dialog protokollieren“.
3. Geben Sie „Vorhersage für Boston“ ein.
4. Botantwort: „welche Stadt?“
2. Klicken Sie auf „Tests abgeschlossen“.

Nehmen Sie nun Korrekturen am zweiten Dialog vor:

1. Klicken Sie unter „Dialoge protokollieren“ auf „Vorhersage für Boston“.
    - Daraufhin wird die Unterhaltung geöffnet.
    - Wenn Sie auf die Benutzerseite der Unterhaltung klicken (hier auf „Vorhersage für Boston“), können Sie die Entitätsbezeichnungen ändern.
    - Wenn Sie auf die Systemseite klicken (hier auf „welche Stadt“), können Sie die ausgewählte Aktion ändern.
5. Klicken Sie auf „Vorhersage für Boston“. 
    - Die Ursache hier ist, dass Boston nicht als eine Entität gekennzeichnet wurde. Das muss geändert werden.
    - Doppelklicken Sie auf „Boston“, und wählen Sie dann „Stadt“ aus.
    - Klicken Sie auf „Änderungen übermitteln“ und dann auf „Speichern“. Dadurch wird ein Trainingsdialog auf Grundlage der von Ihnen vorgenommenen Änderungen erstellt, und Sie werden in den Trainingsdialogen an die Stelle mit der vorgenommen Änderung geführt.
6. Wählen Sie „Das Wetter in $Stadt ist wahrscheinlich sonnig“ aus.
7. Klicken Sie auf „Training abgeschlossen“. Wenn Sie nun zu „Trainingsdialoge“ wechseln, sehen Sie, dass die neue Aktion hinzugefügt wurde.

![](../media/tutorial9_logdiag1.PNG)

Nehmen Sie nun Korrekturen am anderen Dialog vor:

1. Klicken Sie unter „Dialoge protokollieren“ auf „hallo“.
    - Daraufhin wird die Unterhaltung geöffnet.
3. Auf „hallo“ folgt die Antwort „welche Stadt“. Dies soll nun in eine sinnvollere Aussage geändert werden. Eine bessere Antwort wäre z.B. „Hallo, ich bin der Wetterbot“. Da es dafür aber keine passende Aktion gibt, muss eine Aktion erstellt werden.
4. Klicken Sie auf „Aktion“.
    - Geben Sie unter „Antwort“ den Text „Ich bin der Wetterbot. Ich helfe bei Vorhersagen.“ ein.
6. Deaktivieren Sie das Kontrollkästchen „Auf Antwort warten“, damit dies eine Aktion ohne Wartezeit ist.
7. Klicken Sie auf „Erstellen“.
8. Dann wählen Sie diese neue Aktion durch Klicken aus. Klicken Sie anschließend auf „Speichern“.
    - Daraufhin werden Sie an diesen Punkt in der Trainingssitzung zurückgeführt.
6. Wählen Sie „welche Stadt?“ durch Klicken aus.
7. Geben Sie „Boston“ ein. Doppelklicken Sie, um Boston als eine Entität zu kennzeichnen (falls noch nicht geschehen).
8. Klicken Sie auf „Bewertungsaktionen“.
9. Wählen Sie „Das Wetter in $Stadt ist wahrscheinlich sonnig“ durch Klicken aus.
10. Klicken Sie auf „Done Teaching“ (Training abgeschlossen).

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Rückruf zur Entitätserkennung](./10-entity-detection-callback.md)
