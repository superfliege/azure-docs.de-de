---
title: Verwenden von Versionsverwaltung und Markierungen in einer Unterhaltungslernanwendung – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erfahren Sie, wie Sie Versionsverwaltung und Markierungen in einer Unterhaltungslernanwendung verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ea013db078ff33f8597b0e15a8fc951e8ae320e8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376283"
---
# <a name="how-to-use-versioning-and-tagging"></a>Verwenden von Versionsverwaltung und Markierungen

Dieses Tutorial veranschaulicht das Markieren von Versionen Ihrer Unterhaltungslernanwendung und das Festlegen der Live-Version.  

## <a name="requirements"></a>Anforderungen
Für dieses Tutorial ist die Verwendung des Botemulators zum Erstellen von Protokolldialogen erforderlich, nicht die Protokolldialog-Webbenutzeroberfläche.  

Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden:

    npm run tutorial-general

## <a name="details"></a>Details

Beim Bearbeiten arbeiten Sie immer mit dem Tag, das als „master“ bezeichnet wird -- Sie können markierte (getaggte) Versionen von „master“ erstellen (die im Wesentlichen eine Momentaufnahme von „master“ darstellen), diese markierten Versionen können aber nicht bearbeitet werden.

## <a name="steps"></a>Schritte

### <a name="install-the-bot-framework-emulator"></a>Installieren Sie den Bot Framework-Emulator

- Navigieren Sie zu [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Laden Sie den Emulator herunter, und installieren Sie ihn.

### <a name="create-an-app"></a>Erstellen einer App

1. Klicken Sie auf „Neue App“
2. Geben Sie im Feld „Name“ „Tutorial-16-Versionsverwaltung“ ein
3. Klicken Sie auf „Erstellen“ 
4. Klicken Sie auf „Einstellungen“
5. Kopieren Sie die App-ID

### <a name="configure-the-emulator"></a>Konfigurieren des Emulators

- Öffnen Sie im Stammordner der Unterhaltungslernanwendung die ENV-Datei.
- Fügen Sie die App-ID als Wert von CONVERSATION_LEARNER_APP_ID ein
- Starten Sie den Unterhaltungslerndienst neu, indem Sie die Eingabeaufforderung schließen und diesen Befehl noch einmal ausführen:
 
    npm run tutorial-general 

### <a name="actions"></a>Aktionen

Erstellen wir eine Aktion:

1. Wechseln Sie zur Webbenutzeroberfläche.
1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie unter „Response“ (Antwort) „Hallo Ihr (Version 1)“ ein.
3. Klicken Sie auf Speichern.


![](../media/tutorial16_action1.PNG)

Erstellen einer neuen Markierung:

3. Klicken Sie auf „Settings“ (Einstellungen), und erstellen Sie ein neues „Tag“ (Markierung).
    - Nennen Sie es „Version 1“
4. Legen Sie „Version 1“ als „live“ fest.  
    - Wenn die Live-Markierung auf „Version 1“ festgelegt ist, hat das die Wirkung, dass Kanäle, die diesen Bot verwenden, die Markierung „Version 1“ verwenden.
    - Markierte Versionen von Anwendungen sind nicht von Bearbeitungen (Änderung von Aktionen, Entitäten, Hinzufügen von Trainingsdialogen) betroffen.  
    - Bearbeitungen an einer Anwendung (Änderung von Aktionen, Entitäten, Hinzufügen von Trainingsdialogen) werden immer an der Markierung „master“ vorgenommen.  Anders ausgedrückt ist „master“ die einzige Markierung, die zu Änderungen imstande ist; alle anderen Markierungen sind fixierte Momentaufnahmen.
    - Protokolldialoge auf der Benutzeroberfläche des Unterhaltungslernmoduls verwenden immer die Master-Markierung (nicht die Live-Markierung).

![](../media/tutorial16_v1_create.PNG)

Beachten Sie, dass die Version in „Einstellungen“ erstellt wurde:

![](../media/tutorial16_settings.PNG)

Fügen wir eine zweite Aktion hinzu:

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie unter „Response“ (Antwort) „Bye-bye (Version 2)“ ein.

Bearbeiten Sie die erste Aktion:

1. Klicken Sie auf „Aktionen“.
2. Klicken Sie unter „Aktionen“ auf „Hallo Ihr (Version 1)“.
3. Ändern Sie die Antwort in „Hallo Ihr (Version 2)“.

![](../media/tutorial16_hi_there_v2.PNG)

### <a name="switch-to-the-bot-emulator"></a>Wechseln Sie zum Botemulator

1. Geben Sie auf der Bot-Benutzeroberfläche „Auf Wiedersehen“ ein.
2. Beachten Sie, dass der Bot mit „Hallo Ihr (Version 1)“ antwortet.
    - Dies zeigt, dass die Version 1 live ist. 

![](../media/tutorial16_bf_response.PNG)

### <a name="switch-to-the-web-ui"></a>Wechseln zur Webbenutzeroberfläche

1. Klicken Sie auf „Protokolldialoge“ (wenn keine Dialoge angezeigt werden, aktualisieren Sie die App).
2. Klicken Sie auf „Hallo Ihr (Version 2)“

Beachten Sie, dass für Korrekturen alle aktuell verfügbaren Aktionen zur Wahl stehen. Diese Bearbeitungen erfolgen am Master.

Sie haben jetzt gesehen, wie die Versionsverwaltung funktioniert und wie Sie mit dem Bot mithilfe des Bot Framework-Emulators interagieren können.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Demo – Kennwortzurücksetzung](./demo-password-reset.md)
