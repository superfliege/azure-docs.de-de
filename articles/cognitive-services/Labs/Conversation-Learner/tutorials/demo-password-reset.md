---
title: 'Demo: Unterhaltungslernmodell, Kennwortzurücksetzung – Microsoft Cognitive Services | Microsoft-Dokumentation'
titleSuffix: Azure
description: Erstellen einer Demo in einem Unterhaltungslernmodell
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f633dd375d690a1c3e66a2a6e02ae69665dbe960
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170632"
---
# <a name="demo-password-reset"></a>Demo: Kennwortzurücksetzung
Diese Demo zeigt, wie Sie mithilfe eines einfachen Bots für den technischen Support Ihr Kennwort zurücksetzen. 

Sie erfahren, wie das Unterhaltungslernmodul schwierige Dialogabläufe und Sequenzen aus mehreren Aktionen (einschließlich domänenexterner Klasse) erlernen kann. Diese Demo verwendet keine Codes oder Entitäten.

## <a name="video"></a>Video

[![Demo: Kennwort – Vorschau](http://aka.ms/cl-demo-password-preview)](http://aka.ms/blis-demo-password)

## <a name="requirements"></a>Anforderungen
Für dieses Tutorial muss der Kennwortzurücksetzung-Bot ausgeführt werden.

    npm run demo-password

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der Modellliste der Webbenutzeroberfläche auf „Tutorial Demo Password Reset“ (Demotutorial: Kennwortzurücksetzung). 

### <a name="actions"></a>Aktionen

Wir haben mehrere Aktionen erstellt, bei denen der Benutzer Hilfe mit dem Kennwort benötigt (inklusive Lösungen).

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Trainingsdialoge

Es gibt mehrere Trainingsdialoge. Außerdem gibt es Demos für eine domänenexterne Klasse. Benutzeranforderungen wie „Wegbeschreibung“ sind beispielsweise domänenextern. Der Bot erhielt einige Beispiele für solche Anforderungen und kann diese mit „I can't help with that“ (Dabei kann ich leider nicht helfen) beantworten.

![](../media/tutorial_pw_reset_entities.PNG)

Probieren Sie als Beispiel eine Trainingssitzung aus.

1. Klicken Sie auf „Train Dialogs“ (Trainingsdialoge) und dann auf „New Train Dialog“ (Neuer Trainingsdialog).
1. Geben Sie „I lost my password“ (Ich habe mein Kennwort vergessen) ein.
2. Klicken Sie auf „Score Action“ (Bewertungsaktion).
3. Klicken Sie zum Auswählen auf „Is that for your local account or Microsoft account?“ (Für Ihr lokales Konto oder Ihr Microsoft-Konto?).
4. Geben Sie „Local account“ (Lokales Konto) ein.
5. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
3. Klicken Sie zum Auswählen auf „Which version of Windows do you have?“ (Welche Windows-Version verwenden Sie?).
4. Geben Sie „Windows 8“ ein.
5. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
6. Wählen Sie „SOLUTION: how to reset password on Windows 8“ (LÖSUNG: Kennwortzurücksetzung unter Windows 8).
4. Klicken Sie auf „Done Teaching“ (Training abgeschlossen).

So trainieren Sie den Bot, domänenexterne Klassen zu erlernen.

1. Klicken Sie auf „Train Dialogs“ (Trainingsdialoge) und dann auf „New Train Dialog“ (Neuer Trainingsdialog).
1. Geben Sie „web search“ (Websuche) ein.
    - Dies ist ein Beispiel für eine domänenexterne Klasse. 
2. Klicken Sie auf „Score Action“ (Bewertungsaktion).
3. Klicken Sie zum Auswählen auf „Sorry, I can't help with that“ (Dabei kann ich leider nicht helfen).
    - Der Wert für diese Option ist zurzeit niedrig. Nachdem Sie den Bot mehr trainiert haben, wird der Wert allerdings steigen.
4. Klicken Sie auf „Done Teaching“ (Training abgeschlossen).

Sie haben gelernt, wie Sie eine grundlegende Demo für den technischen Support erstellen und wie diese erlernt, Lösungen bereitzustellen, und mit nichtmusterhaften Abfragen umzugehen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Demo – Pizzabestellung](./demo-pizza-order.md)
