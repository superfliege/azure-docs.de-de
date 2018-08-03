---
title: Verwenden eines Unterhaltungslernmoduls mit anderen Botentwicklungstechnologien – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie ein Unterhaltungslernmodul mit anderen Botentwicklungstechnologien verwenden.
services: cognitive-services
author: mattm
manager: larsliden
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 07/13/2018
ms.author: v-jaswel
ms.openlocfilehash: a03596ff8383a085314508d4a25d0ba89bcc3094
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174479"
---
# <a name="how-to-use-conversation-learner-with-other-bot-building-technologies"></a>So verwenden Sie ein Unterhaltungslernmodul mit anderen Botentwicklungstechnologien.

Dieses Tutorial beschreibt, wie ein Unterhaltungslernmodul mit anderen Botentwicklungstechnologien verwendet wird und wie Speicher (oder der Zustand) zwischen diesen Technologien geteilt werden kann. 

## <a name="video"></a>Video

[![Tutorial 15 – Vorschau](http://aka.ms/cl-tutorial-15-preview)](http://aka.ms/blis-tutorial-15)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial ist die Verwendung des Botemulators zum Erstellen von Protokolldialogen erforderlich, nicht die Protokolldialog-Webbenutzeroberfläche.  

Für dieses Tutorial muss der Hybrid-Tutorialbot ausgeführt werden.

    npm run tutorial-hybrid

## <a name="details"></a>Details

Während das Unterhaltungslernmodul die Kontrolle hat, müssen alle Zustände bezüglich der Sitzung des Unterhaltungslernmodells im Speicher-Manager des Unterhaltungslernmodells gespeichert werden. Dies ist notwendig, da das maschinelle Lernen den Zustand verwendet, um zu bestimmen, wie das Gespräch geführt werden soll. Der externe Zustand kann an das Unterhaltungslernmodul über OnSessionStartCallback weitergegeben werden, das zu Beginn der Sitzung aufgerufen wird. Der interne Zustand kann am Ende der Sitzung über OnSessionEndCallback zurückgegeben werden.

Sie können sich das Unterhaltungslernmodul fast wie einen Funktionsaufruf vorstellen, der einen Anfangszustand einnimmt und Werte zurückgibt.

In diesem Beispiel erstellen Sie einen Hybrid-Bot mithilfe von zwei verschiedenen Systemen:
1. Ein Unterhaltungslernmodell <br />
Verwendet ein Unterhaltungslernmodell, um die nächste Aktion des Bot basierend auf der aktuellen Sitzung zu bestimmen.
Dieser Teil des Bots nimmt einen Teil des Ausgangszustandes ein `isOpen` (wodurch angegeben wird, ob ein Store geöffnet oder geschlossen ist) und gibt einen anderen Zustand zurück `purchaseItem` (der Name eines Artikels, den der Benutzer kauft.)

2. Textabgleich <br />
Durchsucht einfach eingehenden Text nach bestimmten Zeichenfolgen und antwortet.
Dieser Teil des Bot verwaltet die anderen Speichermechanismen der Bots und ist für den Start der CL-Sitzung verantwortlich. Insbesondere verwaltet er drei Variablen: `usingConversationLearner`, `storeIsOpen` und `purchaseItem`.

Schauen wir uns zunächst das in dieser Demo verwendete Modell an.

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der Modellliste der Webbenutzeroberfläche auf „Tutorial-15-Hybrid“.

## <a name="entities"></a>Entitäten

Öffnen Sie die Entitätenseite und beachten Sie zwei Entitäten: `isOpen` und `purchaseItem`.

Um zu verstehen, wie diese Entitäten verwendet werden, öffnen Sie die Datei: `C:\<installedpath\>\src\demos\tutorialHybrid.ts`, um die Rückrufe anzusehen.

Beachten Sie, dass der Code in `OnSessionStartCallback` den Wert von `storeIsOpen` aus dem BotBuilder-Konversationsspeicher als Wert der Entität `isOpen` kopiert, sodass er für das Unterhaltungslernmodul verfügbar ist. Schauen Sie sich den folgenden Code an:

![](../media/tutorial17_sessionstart.PNG)

Ebenso kopiert der Code in `OnSessionEndCallback` (wenn die Sitzung aufgrund einer gelernten Aktivität und nicht nur aufgrund eines Timeout beendet wurde) den Wert der Entität `purchaseItem` in den BotBuilder-Speicher `purchaseItem`. Schauen Sie sich den folgenden Code an:

![](../media/tutorial17_sessionend.PNG)

Betrachten wir nun die Aktionen.

## <a name="actions"></a>Actions

Beachten Sie, dass das Modell über vier Aktionen verfügt.

Die gewünschten Regeln für die Aktionen sehen wie folgt aus:

- Wenn die `isOpen`-Entität festgelegt ist, wird der Bot „What would you like to buy?“ fragen. und speichert dies im `puchaseItem`-Slot.
- Wenn `isOpen` nicht festgelegt ist, wird der Bot „I’m sorry we’re closed“ sagen.
- Die anderen beiden Aktionen sind vom Typ `END_SESSION`.
- Die Aktion END_SESSION zeigt dem Unterhaltungslernmodul an, dass die Konversation abgeschlossen wurde.

### <a name="overall-bot-logic"></a>Allgemeine Botlogik

Zuerst sehen Sie, dass wir die Kontrolle an das Unterhaltungslernmodul übergeben, wenn das Flag `usingConversationLearner` des Botzustands gesetzt wurde. Wenn dies nicht der Fall ist, übergeben wir die Kontrolle an ein anderes Element.  In diesem Beispiel zeigen wir einen einfachen Textabgleich, das könnte aber auch jede andere Bottechnologie sein, wie z.B LUIS, QnA Maker und sogar eine weitere Instanz des Unterhaltungslernmoduls.

Wir benötigen eine Möglichkeit für den Benutzer, den Store zu öffnen und zu schließen, also vergleichen wir einen String mit „open store“ und „close store“ und setzen das Flag „storeIsOpen“.

Als Nächstes benötigen wir eine Möglichkeit, um die Übergabe der Kontrolle an unser Unterhaltungslernmodell auszulösen. Beim Abgleich mit der Zeichenfolge „shop“ gehen wir wie folgt vor:
- Legen Sie das Flag `usingConversationLearner` im Speicher des Bots fest.
- Rufen Sie die Methode „StartSession“ in unserem Unterhaltungslernmodell auf.  Dadurch wird „onSessionStartCallback“ ausgelöst, wodurch der `isOpen`-Entitätswert initialisiert wird.

Siehe unten:

![](../media/tutorial17_useConversationLearner.PNG)

Wir führen auch einen Textabgleich mit „history“ durch, sodass der zuletzt gekaufte Artikel angezeigt wird.
Wenn etwas anderes eingegeben wird, werden die verfügbaren Benutzerbefehle angezeigt.

## <a name="train-dialog"></a>Trainingsdialog

Für dieses Tutorial wurde das Modell bereits vorab trainiert.  Wir werden den kompletten Bot testen, um die Auswirkung der Rückrufe zum Starten und Beenden der Sitzung in der Praxis zu sehen.

## <a name="testing-the-bot"></a>Testen des Bots

Im Gegensatz zu einzelnen Unterhaltungslernmodell-Bots können Sie dies nicht in der Benutzeroberfläche des Unterhaltungslernmoduls testen, da nur angezeigt werden kann, was vom Unterhaltungslernmodell verarbeitet wird.

### <a name="install-the-bot-framework-emulator"></a>Installieren Sie den Bot Framework-Emulator

- Navigieren Sie zu [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Laden Sie den Emulator herunter, und installieren Sie ihn.

### <a name="configure-the-emulator"></a>Konfigurieren des Emulators

- Öffnen Sie den Emulator und stellen Sie sicher, dass die URL auf denselben Port zeigt, auf dem Ihr Bot läuft. Wie: `http://localhost:3978/api/messages`

### <a name="test"></a>Test 

#### <a name="scenario-1-store-is-closed"></a>Szenario 1: Store ist geschlossen
1. Geben Sie „shop“ ein. Dies wird durch den Textabgleich verarbeitet und übergibt die Kontrolle an das Unterhaltungslernmodell.
2. Geben Sie „Hallo“ ein.  Das der Wert `isOpen` nicht festgelegt ist, wird der Bot „I’m sorry we’re closed“ sagen und die Sitzung beenden.

#### <a name="scenario-2-store-is-open"></a>Szenario 2: Store ist geöffnet
3. Geben Sie „open store“ ein.  Dadurch wird `isOpen` auf „true“ festgelegt.
4. Geben Sie „shop“ ein.
5. Geben Sie „Hallo“ ein.  Da der Wert `isOpen` auf „true“ festgelegt ist, wird der Bot „What would you like to buy?“ sagen.
6. Geben Sie „chair“ ein. „Chair“ wird im CL-Speicher als Entität `purchaseItem` gespeichert. Es wird der Rückruf zum Beenden der Sitzung aufgerufen, der diesen Wert in den Konversationsspeicher kopiert.
7. Geben Sie „history“ ein.  Der Bot sagt „You bought chair“, da dies Ihr letzter `purchaseItem` war.

## <a name="conclusion"></a>Zusammenfassung

Mit dem, was Sie oben gelernt haben, sollten Sie in der Lage sein, ein Unterhaltungslernmodul mit jeder anderen Bottechnologie zu kombinieren.
