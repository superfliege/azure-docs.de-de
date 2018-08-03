---
title: Sitzungsrückrufe in einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie Sitzungsrückrufe in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 0f51b232470e4e4da3f25d40d025dd3b09dd1204
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171914"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Verwenden von Sitzungsrückrufen in einem Unterhaltungslernmodell

Dieses Tutorial veranschaulicht die Rückrufe „onSessionStart“ und „onSessionEnd“.

## <a name="video"></a>Video

[![Tutorial 11 – Vorschau](http://aka.ms/cl-tutorial-11-preview)](http://aka.ms/blis-tutorial-11)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der `tutorialSessionCallbacks`-Bot ausgeführt werden.

    npm run tutorial-session-callbacks

## <a name="details"></a>Details
Dieses Tutorial behandelt das Konzept einer Sitzung, wie Sitzungen standardmäßig ablaufen, und wie Sie dieses Verhalten überschreiben können.

Eine Sitzung ist eine Unterhaltung mit dem Bot. Sie kann mehrere Aktionen umfassen, jedoch keine langen Pausen (z. B. 30 Minuten).  Auf der Hilfeseite für „Grenzen“ finden Sie den Standardwert, wann ein Sitzungstimeout eintritt.

Bei langen Pausen wechselt der Bot zu seiner nächsten Sitzung.  Beim Start einer neuen Sitzung wird das rekurrente neuronale Netzwerk in seinen ursprünglichen Status versetzt.  Außerdem werden standardmäßig alle Entitätswerte gelöscht. Dieses Verhalten lässt sich wie unten dargestellt ändern.

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der Modellliste auf „Tutorial-11-SessionCallbacks“. 

### <a name="entities"></a>Entitäten

In diesem Modell sind vier Entitäten definiert.

![](../media/tutorial11_entities.PNG)

„BotName“ ist eine Entität des Typs „Programmgesteuert“.  Die Entität wird vom Bot beim Beginn der Sitzung festgelegt.

### <a name="actions"></a>Actions

In diesem Modell sind vier Aktionen definiert.

![](../media/tutorial11_actions.PNG)

Zunächst zeigt dieses Tutorial, wie Sie Entitätswerte zu Sitzungsbeginn steuern können, z. B. indem Sie die Entität „BotName“ festlegen, bevor der Benutzer etwas äußert.

Zudem veranschaulicht dieses Tutorial, wie Werte aus einer Sitzung für die nächste übernommen werden.  In diesem Tutorial gehen wir davon aus, dass der Name und die Telefonnummer des Benutzers aus einer Sitzung in die nächste übernommen werden, sich jedoch möglicherweise der Ort ändert.  Deswegen übernehmen wir den Namen und die Telefonnummer aus einer Sitzung in die nächste, und entfernen den Benutzerstandort.

### <a name="train-dialog"></a>Trainingsdialog

Sie sehen einen Beispieldialog. Es handelt sich um eine Sitzung, d. h., dieser Dialog hat keine langen Pausen.

![](../media/tutorial11_traindialog.PNG)

### <a name="code-for-the-callbacks"></a>Rückrufcode

Der Code für die Rückrufmethoden befindet sich in folgender Datei: C: \<installedpath > \src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial11_code.PNG)

Die beiden folgenden Methoden sind optional.

- OnSessionStartCallback: Diese Methode bestimmt die Entität „BotName“.
- OnSessionEndCallback: Mit dieser Methode können Sie angeben, was Sie erhalten möchten. Alle Entitäten außer dem Namen und der Telefonnummer des Benutzers werden gelöscht.

### <a name="try-the-bot"></a>Testen des Bots

Wechseln Sie zur Webbenutzeroberfläche, und klicken Sie auf „Dialoge protokollieren“.

1. Geben Sie „Hallo“ ein.
2. Das System erwidert: „Hallo, ich bin Botty. Wie heißen Sie?“ Das System hat den Namen „Botty“, von OnSessionStartCallback erhalten.
3. Geben Sie „Jason“ ein.
4. System: „Hi Jason. Wie lautet Ihre Telefonnummer?“
5. Geben Sie „555-555-5555“ ein.
6. System: „Würden Sie Botty bitte Ihren Standort mitteilen, Jason?“
7. Geben Sie „Redmond“ ein.

Das zählt als eine Sitzung. Um eine neue Sitzung zu beginnen, müssen Sie diese zuerst beenden. 

1. Klicken Sie auf „Sitzungstimeout“. So gelangen Sie in die nächste Sitzung.
    - Die Schaltfläche „Sitzungstimeout“ dient zum Debuggen.  In einer realen Sitzung tritt ein Timeout nach einer längeren Pause (ca. 30 Minuten) ein.  Auf der Hilfeseite für „Grenzen“ finden Sie den Standardwert, wann ein Sitzungstimeout eintritt.
1. Geben Sie „Hi“ ein.
2. System: „Würden Sie Botty bitte Ihren Standort mitteilen, Jason?“
    - Das System erinnert sich an den Namen und die Telefonnummer.
2. Geben Sie einen neuen Ort ein: „Seattle“.
3. System: „Sie sind also in Seattle, Jason“.
4. Klicken Sie auf „Tests abgeschlossen“.

Gehen Sie zurück zu „Dialoge protokollieren“. Die letzte Unterhaltung wurde in zwei aufgeteilt, weil jeder protokollierte Dialog einer Sitzung entspricht.  

![](../media/tutorial11_splitdialogs.PNG)

- In der ersten Interaktion ist Botty festgelegt, jedoch nicht der Name und die Telefonnummer.
- In der zweiten Aktivität werden der Name und die Telefonnummer angezeigt.

Sie haben gelernt, wie Sitzungen standardmäßig ablaufen, und wie Sie das Standardverhalten überschreiben können. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [API-Aufrufe](./12-api-calls.md)
