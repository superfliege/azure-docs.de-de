---
title: Karten in einem Unterhaltungslernanwendungsmodell, Teil 1 – Microsoft Cognitive Services | Microsoft-Dokumentation
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
ms.openlocfilehash: 988a2433f098f41bca4796299825293efd4de44b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171133"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Verwenden von Karten (Teil 1 von 2)

Dieses Tutorial zeigt, wie Sie einfache Karte in Ihrem Bot hinzufügen und verwenden.

> [!NOTE]
> Derzeit geht das Unterhaltungslernmodul davon aus, dass Ihre Kartendefinitionsdateien in einem Verzeichnis namens „cards“ gespeichert sind, das sich in dem Verzeichnis befindet, in dem der Bot gestartet wird. Dies wird in der Zukunft konfigurierbar sein.

## <a name="video"></a>Video

[![Tutorial 13 – Vorschau](http://aka.ms/cl-tutorial-13-preview)](http://aka.ms/blis-tutorial-13)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

Karten sind Benutzeroberflächenelemente, mit denen Benutzer eine Option in der Unterhaltung auswählen können. 

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der Modellliste der Webbenutzeroberfläche auf „Tutorial-13-Cards-1“. 

### <a name="the-card"></a>Die Karte

Die Kartendefinition befindet sich am folgenden Speicherort: C:\<installedpath\>\src\cards\prompt.json.

Das System geht davon aus, dass Ihre Kartendefinitionen sich in diesem Kartenverzeichnis befindet.

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> Beachten Sie den Textkörper `TextBlock` und den Platzhalter `{{question}}` im Textfeld.
> Es gibt zwei Schaltflächen zum Übermitteln, für die jeweils ein anderer Text gesendet wird.

### <a name="actions"></a>Actions

Wir haben drei Aktionen erstellt. Wie Sie unten sehen, ist die erste Aktion eine Karte.

![](../media/tutorial13_actions.PNG)

Der Aktionstyp „Karte“ wurde folgendermaßen erstellt:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> Die Felder „Frage“, „Schaltfläche 1“ und „Schaltfläche 2“. Das sind Vorlagenreferenzen der Karte, denen Sie die Frage und die entsprechenden Antworten hinzufügen. Sie können auch auf Entitäten verweisen und diese verwenden oder eine Kombination aus Text und Entitäten.

Das Augensymbol zeigt, wie die Karte aussieht.

### <a name="train-dialog"></a>Trainingsdialog

Im Folgenden sehen wir uns einen Trainingsdialog an.

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
1. Geben Sie „Hi“ ein.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Eingabeaufforderung links oder rechts“.
    - Das Klicken auf „Links“ oder „Rechts“ entspricht der Eingabe der Begriffe „Links“ bzw. „Rechts“. 
4. Klicken Sie auf „Bewertungsaktionen“.
4. Klicken Sie zum Auswählen auf „Links“. Dies ist eine Aktion ohne Wartezeit.
6. Klicken Sie zum Auswählen auf „Eingabeaufforderung links oder rechts“.
4. Klicken Sie auf „Rechts“.
5. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Rechts“.
6. Klicken Sie zum Auswählen auf „Eingabeaufforderung links oder rechts“.
4. Klicken Sie auf „Tests abgeschlossen“.

Nun wissen Sie, wie Karten funktionieren. Sie sind im Kartenverzeichnis als JSON-Vorlagen definiert. Die Vorlagen erscheinen in der Benutzeroberfläche, die Sie mithilfe einer Zeichenfolge, einer Entität oder einer Kombination aus beidem auffüllen können.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Karten – Teil 2](./14-cards-2.md)
