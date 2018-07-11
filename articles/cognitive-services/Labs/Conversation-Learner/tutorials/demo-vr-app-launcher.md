---
title: Demo einer Unterhaltungslernanwendung, Virtual Reality-App-Startfeld – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie eine Demo einer Unterhaltungslernanwendung erstellen.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3e41125bf7da9ee64d666d22cb275af01af54012
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376323"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demo: Virtual Reality-App-Startfeld

Diese Demo veranschaulicht einen Virtual Reality-Anwendungsstarter, der Befehle wie z. B. „Skype starten und an der Wand platzieren“ unterstützt. Ein Benutzer muss den Namen und Speicherort einer App nennen, um die App zu starten. Das Starten der App erfolgt über einen API-Aufruf. Wenn ein App-Name des Benutzers erkannt wird, überprüft die „EntityDetectionCallback“-Methode, ob die angeforderte App mit einer oder mehreren Apps in der Liste der installierten Apps übereinstimmt. Sie behandelt die Fälle, in denen die angeforderte App nicht installiert ist und der App-Name nicht eindeutig ist (mehr als einer installierten App entspricht).

## <a name="requirements"></a>Anforderungen

Für dieses Tutorial muss der VRAppLauncher-Bot ausgeführt werden.

    npm run demo-vrapp

### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der App-Liste der Webbenutzeroberfläche auf „VRAppLauncher“. 

## <a name="entities"></a>Entitäten

Wir haben vier Entitäten erstellt:

- AppName: z. B. Skype
- PlacementLocation: z. B. Wand
- UnknownAppName: eine programmgesteuerte Entität, die das System festlegt, wenn es einen vom Benutzer genannten Entitätsnamen nicht erkennt, weil die Entität z. B. noch nicht installiert wurde.
- DisambigAppNames: ein Array von mindestens zwei installierten App-Namen, die mit der Benutzeräußerung übereinstimmen. 

![](../media/tutorial_vrapplauncher_entities.PNG)

### <a name="actions"></a>Aktionen

Wir haben eine Reihe von Aktionen erstellt, die eine API namens LaunchApp enthalten, die den Funktionsaufruf zum Starten einer App startet.

![](../media/tutorial_vrapplauncher_actions.PNG)

### <a name="training-dialogs"></a>Trainingsdialoge
Wir haben mehrere Trainingsdialoge definiert.

![](../media/tutorial_vrapplauncher_dialogs.PNG)

Probieren Sie als Beispiel eine Trainingssitzung aus.

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
1. Geben Sie „Hi“ ein.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Welche App möchten Sie starten?“
4. Geben Sie „Outlook“ ein.
    - Beachten Sie, dass LUIS diese App als Entität erkennt.
5. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Wo soll sie platziert werden?“
4. Geben Sie „an der Wand“ ein.
    - Beachten Sie, dass LUIS dies als PlacementLocation erkennt.
2. Geben Sie „Bewertungsaktionen“ ein.
6. Wählen Sie „LaunchApp“ aus.
7. System: „Outlook an der Wand starten“.
    - Beachten Sie, dass dadurch ein API-Aufruf ausgelöst wurde. Der Code für diesen Aufruf befindet sich unter „C:\<\Installationspfad>\src\demos\demoVRAppLauncher.ts“. Er verfügt jedoch nicht über die Logik, um Outlook für diese Demo zu starten.
    - Er löscht die Entitäten „AppName“ und „PlacementLocation“. Die obige Zeichenfolge wird als Antwort zurückgegeben.
4. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Als Nächstes beginnen wir mit einer anderen Trainingssitzung für den Umgang mit unbekannten und mehrdeutigen Entitäten.

1. Klicken Sie auf „Neuer Trainingsdialog“.
1. Geben Sie „OneNote starten“ ein. 
    - Dies wird als App-Name erkannt, weil die im Code definierte „EntityDetectionCallback-Methode den vom Benutzer eingegebenen Namen verwendet und ihn in einen App-Namen auflöst, indem er mit der im Code definierten App-Liste abgeglichen wird. Anschließend wird der Satz aller übereinstimmenden Apps zurückgegeben. 
    - Wenn die Liste der Übereinstimmungen dem Wert „Null“ entspricht, bedeutet dies, dass die App nicht installiert ist. Sie wird in „UnknownAppName“ abgelegt.
    - Wird mehr als eine App gefunden, werden die Apps in „DisambigAppNames“ kopiert, und die Entität „AppName“ wird gelöscht.
2. Klicken Sie auf „Bewertungsaktionen“.
3. Klicken Sie zum Auswählen auf „Leider kenne ich die App $UnknownAppName nicht“.
4. Geben Sie „Amazon starten“ ein. Wir probieren den anderen Pfad aus.
5. Klicken Sie auf „Bewertungsaktionen“.
    - Beachten Sie, dass „Amazon Video“ und „Amazon Music“ jetzt im Speicher von „DisambigAppNames“ vorhanden sind. Und OneNote wurde gelöscht.
3. Klicken Sie zum Auswählen auf „Es gibt einige Apps, die so klingen“.
    - Beachten Sie, dass die Bewertung nicht sehr hoch ist, weil wir bis zu diesem Zeitpunkt nur wenige Trainingsdialoge hinzugefügt haben. Wir benötigen hier wohl weitere Dialoge, um das Modell aussagekräftiger zu gestalten.
2. Geben Sie „Bewertungsaktionen“ ein.
4. Klicken Sie auf „Training abgeschlossen“.

Sie haben jetzt gesehen, wie die Entitätsauflösung funktioniert. In der Demo wurden auch API-Rückrufe veranschaulicht, und es wurde eine Vorlage für das Sammeln von Informationen, das Überprüfen auf Vorhandensein und Mehrdeutigkeit und das Auswählen der richtigen Aktion auf dieser Grundlage vorgestellt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bereitstellen eines Unterhaltungslernmodul-Bots](../deploy-to-bf.md)
