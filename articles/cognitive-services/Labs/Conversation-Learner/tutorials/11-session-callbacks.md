---
title: Sitzungsrückrufe in einer Unterhaltungslernanwendung – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Informationen zum Verwenden von Sitzungsrückrufen in einer Unterhaltungslernanwendung
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f8970620c1f0f87ccae13d031092a048144ffb19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376251"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-application"></a>Sitzungsrückrufe in einer Unterhaltungslernanwendung

Dieses Tutorial veranschaulicht die Rückrufe „onSessionStart“ und „onSessionEnd“.

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der tutorialSessionCallbacks.ts-Bot ausgeführt werden.

    npm run tutorial-session-callbacks

## <a name="details"></a>Details
Dieses Tutorial behandelt das Konzept einer Sitzung, wie Sitzungen standardmäßig ablaufen, und wie Sie dieses Verhalten überschreiben können.

Eine Sitzung ist eine Unterhaltung mit dem Bot. Sie kann mehrere Aktionen umfassen, jedoch keine langen Pausen (z. B. 30 Minuten).  Auf der Hilfeseite für „Grenzen“ finden Sie den Standardwert, wann ein Sitzungstimeout eintritt.

Bei langen Pausen wechselt der Bot zu seiner nächsten Sitzung.  Beim Start einer neuen Sitzung wird das rekurrente neuronale Netzwerk in seinen ursprünglichen Status versetzt.  Außerdem werden standardmäßig alle Entitätswerte gelöscht. Dieses Verhalten lässt sich wie unten dargestellt ändern.

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der App-Liste auf „Tutorial-11-SessionCallbacks“. 

### <a name="entities"></a>Entitäten

Wir haben in der Anwendung vier Entitäten definiert.

![](../media/tutorial11_entities.PNG)

„BotName“ ist eine Entität des Typs „Programmgesteuert“.  Sie wird vom Bot beim Beginn der Sitzung festgelegt.

### <a name="actions"></a>Actions

Wir haben vier Aktionen erstellt. 

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
- OnSessionEndCallback: Mit dieser Methode können Sie angeben, was Sie löschen möchten. Alle Entitäten außer dem Namen und der Telefonnummer des Benutzers werden gelöscht.

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
