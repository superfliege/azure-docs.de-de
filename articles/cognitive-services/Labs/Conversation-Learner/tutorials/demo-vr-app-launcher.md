---
title: Demo eines Unterhaltungslernmodells, Virtual Reality-App-Starter – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie eine Demo in einem Unterhaltungslernmodell erstellen.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 31efcb2b2f05bd7a49701be708a89db8f2ef26f7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229696"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demo: Virtual Reality-App-Startfeld

Diese Demo veranschaulicht einen Virtual Reality-Modellstarter, der Befehle wie z. B. „Skype starten und an der Wand platzieren“ unterstützt. Ein Benutzer muss den Namen und Speicherort einer App nennen, um die App zu starten. Das Starten des Modells erfolgt über einen API-Aufruf. Wenn ein App-Name des Benutzers erkannt wird, überprüft die „EntityDetectionCallback“-Methode, ob die angeforderte App mit einer oder mehreren Apps in der Liste der installierten Apps übereinstimmt. Sie behandelt die Fälle, in denen die angeforderte App nicht installiert ist und der App-Name nicht eindeutig ist (mehr als einer installierten App entspricht).

## <a name="video"></a>Video

[![Demo-VR-App – Vorschau](https://aka.ms/cl-demo-vrapp-preview)](https://aka.ms/blis-demo-vrapp)

## <a name="requirements"></a>Requirements (Anforderungen)

Für dieses Tutorial muss der VRAppLauncher-Bot ausgeführt werden:

    npm run demo-vrapp
    
### <a name="open-the-demo"></a>Öffnen der Demo

Klicken Sie in der Modellliste der Webbenutzeroberfläche auf „VRAppLauncher“. 

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
    - LUIS erkennt dies als Entität.
5. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
3. Klicken Sie zum Auswählen auf „Wo soll sie platziert werden?“
4. Geben Sie „an der Wand“ ein.
    - LUIS erkennt dies als PlacementLocation.
2. Geben Sie „Bewertungsaktionen“ ein.
6. Wählen Sie „LaunchApp“ aus.
7. System: „Outlook an der Wand starten“.
    - Dadurch wird ein API-Aufruf ausgelöst. Der Code für diesen Aufruf befindet sich unter „C:\<\Installationspfad>\src\demos\demoVRAppLauncher.ts“. Er verfügt jedoch nicht tatsächlich über den Code, um Outlook für diese Demo zu starten.
    - Er löscht die Entitäten „AppName“ und „PlacementLocation“. Die obige Zeichenfolge wird als Antwort zurückgegeben.
4. Klicken Sie auf „Training abgeschlossen“.

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Als Nächstes beginnen wir mit einer anderen Trainingssitzung für den Umgang mit unbekannten und mehrdeutigen Entitäten.

1. Klicken Sie auf „Neuer Trainingsdialog“.
1. Geben Sie „OneNote starten“ ein. 
    - Das Modell erkennt OneNote als App-Name. Die im Code definierte Funktion `EntityDetectionCallback` löst den vom Benutzer eingegebenen Namen in einen App-Namen auf, indem sie ihn mit der im Code definierten App-Liste abgleicht. Anschließend wird der Satz aller übereinstimmenden Apps zurückgegeben. 
    - Wenn die Liste der Übereinstimmungen dem Wert „Null“ entspricht, bedeutet dies, dass die App nicht installiert ist. Sie wird in „UnknownAppName“ abgelegt.
    - Wird mehr als eine App gefunden, werden die Apps in `DisambigAppNames` kopiert, und die Entität „AppName“ wird gelöscht.
2. Klicken Sie auf „Score Action“ (Bewertungsaktion).
3. Klicken Sie zum Auswählen auf „Leider kenne ich die App $UnknownAppName nicht“.
4. Geben Sie „start Amazon“ ein. Wir probieren den anderen Pfad aus.
5. Klicken Sie auf „Score Actions“ (Bewertungsaktionen).
    - Amazon Video und Amazon Music befinden sich jetzt im `DisambigAppNames`-Speicher, und OneNote wurde gelöscht.
3. Klicken Sie zum Auswählen auf „Es gibt einige Apps, die so klingen“.
    - Die Bewertung ist nicht sehr hoch, weil wir bis zu diesem Zeitpunkt nur wenige Trainingsdialoge hinzugefügt haben. Durch die Definition weiterer Trainingsdialoge trifft das Modell noch bessere Entscheidungen.
2. Geben Sie „Bewertungsaktionen“ ein.
4. Klicken Sie auf „Training abgeschlossen“.

Sie haben jetzt gesehen, wie die Entitätsauflösung funktioniert. In der Demo wurden auch API-Rückrufe veranschaulicht, und es wurde eine Vorlage für das Sammeln von Informationen, das Überprüfen auf Vorhandensein und Mehrdeutigkeit und das Auswählen der richtigen Aktion auf dieser Grundlage vorgestellt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bereitstellen eines Unterhaltungslernmodul-Bots](../deploy-to-bf.md)
