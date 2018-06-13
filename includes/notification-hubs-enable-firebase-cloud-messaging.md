---
title: Includedatei
description: Includedatei
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 04/05/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 4dc67175f2ccc569ab6dfe5338607fa6bebe3882
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33814695"
---
1. Melden Sie sich bei der [Firebase-Konsole](https://firebase.google.com/console/) an. Erstellen Sie ein neues Firebase-Projekt, falls Sie noch keins besitzen.
2. Klicken Sie nach der Erstellung Ihres Projekts auf **Add Firebase to your Android app** (Firebase der Android-App hinzufügen). Führen Sie anschließend die angegebenen Schritte aus. Laden Sie die Datei **google-services.json** herunter. 

    ![Hinzufügen von Firebase zu Ihrer Android-App](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Klicken Sie in der Firebase-Konsole auf das Zahnrad für Ihr Projekt. Klicken Sie dann auf **Projekteinstellungen**.

    ![Klicken auf „Projekteinstellungen“](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Klicken Sie in den Projekteinstellungen auf die Registerkarte **Allgemein**. Laden Sie anschließend die Datei **google-services.json** herunter. Diese enthält den Server-API-Schlüssel und die Client-ID.
5. Klicken Sie in den Projekteinstellungen auf die Registerkarte **Cloud Messaging**, und kopieren Sie den Wert unter **Legacy server key** (Legacyserverschlüssel). Dieser Wert dient zum Konfigurieren der Notification Hub-Zugriffsrichtlinie.
