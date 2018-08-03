---
title: Erstellen eines Unterhaltungslernmodells des Typs „Hallo Welt“ – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Informationen zum Erstellen einer Demo des Typs „Hallo Welt“ in einem Unterhaltungslernmodell
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 70b8f25bd699cbdb069892d65bf766ef3953f59d
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170870"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Erstellen eines Hallo Welt-Modells mit dem Unterhaltungslernmodul

Dieses Tutorial ist eine Einführung in das Unterhaltungslernmodul. Hier lernen Sie, wie Sie Aktionen erstellen, interaktive Trainings durchführen, und Korrekturen an protokollierten Dialogen von Endbenutzern vornehmen.

## <a name="video"></a>Video

[![Tutorial 1 – Vorschau](http://aka.ms/cl-tutorial-01-preview)](http://aka.ms/blis-tutorial-01)


## <a name="requirements"></a>Requirements (Anforderungen)
Überprüfen Sie als Erstes, ob alle Setupschritte abgeschlossen wurden, einschließlich der Erstellung einer `.env`-Datei mit Ihrem LUIS-Erstellungsschlüssel.  Weitere Informationen finden Sie unter [Schnellstart](https://github.com/Microsoft/ConversationLearner-Samples).

Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="steps"></a>Schritte

Beginnen Sie auf der Homepage in der Webbenutzeroberfläche.

### <a name="create-the-model"></a>Erstellen des Modells
1. Klicken Sie auf „Neues Modell“.
2. Geben Sie im Feld „Name“ „Hallo Welt“ ein.
3. Klicken Sie auf „Erstellen“.

### <a name="create-an-action"></a>Erstellen einer Aktion

1. Klicken Sie auf das „Hallo Welt-Modell“, um dieses zu starten.
2. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
    - Eine Aktion kann eine Textnachricht sein, die das Unterhaltungslernmodul an den Benutzer zurückgibt, ein API-Aufruf oder eine Karte.
3. Geben Sie im Feld „Antwort“ „Hallo Welt“ ein.
    - Dies ist die Antwort, die der Bot zurückgibt.
4. Klicken Sie auf „Erstellen“.

Sie haben die erste Aktion für den Bot erstellt. Nun kann er eine Textantwort zurückgeben.

### <a name="train-the-bot"></a>Trainieren des Bots

#### <a name="create-the-first-dialog"></a>Erstellen des ersten Dialogs

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie ein Beispiel dafür ein, wie der Benutzer die Unterhaltung beginnen könnte, z. B. „Hallo“.
3. Klicken Sie auf „Bewertungsaktionen“.
4. Wählen Sie „Hallo Welt“ aus.
    - Dadurch wird ein Beispieldialog mit einer Aktion erstellt. 
2. Geben Sie „Auf Wiedersehen“ ein.
3. Klicken Sie auf „Bewertungsaktionen“.
4. Klicken Sie auf „Aktion hinzufügen“, und geben Sie „Auf Wiedersehen“ ein. Klicken Sie im Feld „Antwort“ auf „Erstellen“.
5. Klicken Sie auf „Training abgeschlossen“. Dadurch wird dieser Trainingsdialog beendet.

Nun haben Sie einen Trainingsdialog im System.

#### <a name="continue-teaching-the-bot"></a>Weiteres Trainieren des Bots
Führen Sie ein weiteres Training aus, um die Antwort des Bots zu testen.

1. Klicken Sie auf „Neuer Trainingsdialog“.
2. Geben Sie „Hey“ ein.
    - Diese Eingabe ähnelt dem ersten Dialog. Deswegen wird erwartet, dass der Bot eine hohe Punktzahl zurückgibt.
2. Klicken Sie auf „Bewertungsaktionen“.
    - Die Position und die Punktzahl sind möglicherweise noch nicht präzise genug. Gegebenenfalls ist zusätzliches Training erforderlich.
3. Klicken Sie neben „Hallo Welt“ auf „Auswählen“.
4. Geben Sie dann „Tschüss“ ein.
5. Klicken Sie auf „Bewertungsaktionen“.
6. Wählen Sie „Auf Wiedersehen“ aus.
7. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial1_actions.PNG)

Führen Sie eine weitere Trainingssitzung aus, um die Funktionsweise des Bots zu prüfen.

Wiederholen Sie die oben genannten Schritte mit „Hi“ und „Tschüss“, und achten Sie auf die Veränderungen bei der Position und der Punktzahl der Botantwort, wenn Sie auf „Bewertungsaktionen“ klicken.

Nun können Sie die Schritte mit „Hallöchen“ und „Auf Wiedersehen“ wiederholen. Die Punktzahl fällt jetzt besser aus, d. h., der Bot hat diese Interaktion gelernt.

![](../media/tutorial1_dialogs.PNG)

### <a name="test-the-bot-as-an-end-user"></a>Testen des Bots als Endbenutzer

1. Klicken Sie auf „Dialoge protokollieren“ und dann auf „Neuen Dialog protokollieren“.
2. Geben Sie „Hallo“ ein.
3. Geben Sie dann „Tschüss“ ein.

Sie können auch eine Unterhaltung mit „Tschüss“ beginnen. Achten Sie auf die Antwort des Bots.

### <a name="view-conversations-in-the-log-dialogs"></a>Anzeigen von Unterhaltungen in protokollierten Dialogen

Sie können in protokollierten Dialogen die Unterhaltungsliste ansehen sowie die Interaktionen als Trainingsdialoge aktualisieren und speichern. Gehen Sie dafür folgendermaßen vor:

1. Klicken Sie auf das Protokoll einer Unterhaltung.
2. Wenn die Unterhaltung gut aussieht, klicken Sie auf die letzte Aktion, z. B. aus „Auf Wiedersehen“.
3. Klicken Sie, um die vorgeschlagene Antwort auszuwählen. 
    - Sie können auch eine andere Aktion auswählen oder hinzufügen.
4. Klicken Sie auf „Fertig“, um dies als ein Trainingsdialog zu speichern.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aktionen mit und ohne Wartezeit](./2-wait-vs-nonwait-actions.md)