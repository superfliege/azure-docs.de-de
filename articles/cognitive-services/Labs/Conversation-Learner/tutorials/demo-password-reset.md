---
title: 'Demo: Unterhaltungslernanwendung, Kennwortzurücksetzung – Microsoft Cognitive Services | Microsoft-Dokumentation'
titleSuffix: Azure
description: Erstellen einer Demo in einer Unterhaltungslernanwendung
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 24d61787a79ee1a1a9737c417aa966cc8fd75930
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376219"
---
# <a name="demo-password-reset"></a>Demo: Kennwortzurücksetzung
Diese Demo zeigt, wie Sie mithilfe eines einfachen Bots für den technischen Support Ihr Kennwort zurücksetzen. 

Sie erfahren, wie das Unterhaltungslernmodul schwierige Dialogabläufe und Sequenzen aus mehreren Aktionen (einschließlich domänenexterner Klasse) erlernen kann. Diese Demo verwendet keine Codes oder Entitäten.

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der Kennwortzurücksetzung-Bot ausgeführt werden.

    npm run demo-password

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der App-Liste der Webbenutzeroberfläche auf „Demotutorial: Kennwortzurücksetzung“. 

### <a name="actions"></a>Actions

Wir haben mehrere Aktionen erstellt, bei denen der Benutzer Hilfe mit dem Kennwort benötigt (inklusive Lösungen).

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Trainingsdialoge

Es gibt mehrere Trainingsdialoge. Außerdem gibt es Demos für eine domänenexterne Klasse. Benutzeranforderungen wie „Wegbeschreibung“ sind beispielsweise domänenextern. Der Bot erhielt einige Beispiele für solche Anforderungen und kann diese mit „Dabei kann ich leider nicht helfen“ beantworten.

![](../media/tutorial_pw_reset_entities.PNG)

Probieren Sie als Beispiel eine Trainingssitzung aus.

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
1. Geben Sie „Ich hab mein Kennwort vergessen“ ein.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Für Ihr lokales Konto oder Ihr Microsoft-Konto?“.
4. Geben Sie „Lokales Konto“ ein.
5. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Welche Windows-Version verwenden Sie?“.
4. Geben Sie „Windows 8“ ein.
5. Klicken Sie auf „Bewertungsaktionen“.
6. Wählen Sie „LÖSUNG: Kennwortzurücksetzung unter Windows 8“.
4. Klicken Sie auf „Training abgeschlossen“.

So trainieren Sie den Bot, domänenexterne Klassen zu erlernen.

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
1. Geben Sie „Websuche“ ein.
    - Dies ist ein Beispiel für eine domänenexterne Klasse. 
2. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Dabei kann ich leider nicht helfen“.
    - Der Wert für diese Option ist zurzeit niedrig. Nachdem Sie den Bot mehr trainiert haben, wird der Wert allerdings steigen.
4. Klicken Sie auf „Training abgeschlossen“.

Sie haben gelernt, wie Sie eine grundlegende Demo für den technischen Support erstellen und wie diese erlernt, Lösungen bereitzustellen, und mit nichtmusterhaften Abfragen umzugehen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Demo – Pizzabestellung](./demo-pizza-order.md)
