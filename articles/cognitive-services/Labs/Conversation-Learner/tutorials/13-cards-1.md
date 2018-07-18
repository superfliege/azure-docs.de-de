---
title: Karten in einer Unterhaltungslernanwendung, Teil 1 – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Verwenden von Karten in einer Unterhaltungslernanwendung
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e90ccd42b21eea6139c402937be7e20513d73c84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376203"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Verwenden von Karten (Teil 1 von 2)

Dieses Tutorial zeigt, wie Sie einfache Karte in Ihrem Bot hinzufügen und verwenden.

Das Unterhaltungslernmodul geht davon aus, dass Ihre Kartendefinitionsdateien in einem Verzeichnis namens „cards“ gespeichert sind, das sich in dem Verzeichnis befindet, in dem der Bot gestartet wird.

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

Karten sind Benutzeroberflächenelemente, mit denen Benutzer eine Option in der Unterhaltung auswählen können. 

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der App-Liste der Webbenutzeroberfläche auf „Tutorial-13-Cards-1“. 

### <a name="the-card"></a>Die Karte

Die Kartendefinition befindet sich am folgenden Speicherort: C:\<installedpath\>\src\cards\prompt.json.

Das System geht davon aus, dass Ihre Kartendefinitionen sich in diesem Kartenverzeichnis befindet.

![](../media/tutorial13_prompt.PNG)

- Beachten Sie „TextBlock“ und die Fragevorlage.
- Es gibt zwei Schaltflächen zum Übermitteln, für die jeweils ein anderer Text gesendet wird.

### <a name="actions"></a>Actions

Wir haben drei Aktionen erstellt. Wie Sie unten sehen, ist die erste Aktion eine Karte.

![](../media/tutorial13_actions.PNG)

Der Aktionstyp „Karte“ wurde folgendermaßen erstellt:

![](../media/tutorial13_cardaction.PNG)

Sehen Sie sich die Felder „Frage“, „Schaltfläche 1“ und „Schaltfläche 2“ an. Das sind Vorlagenreferenzen der Karte, denen Sie die Frage und die entsprechenden Antworten hinzufügen. Sie können auch auf Entitäten verweisen und diese verwenden oder eine Kombination aus Text und Entitäten.

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
