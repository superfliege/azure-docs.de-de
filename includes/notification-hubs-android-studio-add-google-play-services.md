---
title: Includedatei
description: Includedatei
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 01/04/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: f00ca7ddf44a9d5b850cd47520970a0396a0c1b5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453089"
---
1. Öffnen Sie Android SDK Manager, indem Sie in der Symbolleiste auf das Symbol von Android Studio klicken oder indem Sie im Menü auf **Tools** > **Android** > **SDK Manager** klicken. Suchen Sie die Zielversion des Android SDK, das in diesem Projekt verwendet wird, öffnen Sie es durch Klicken auf **Paketdetails anzeigen**, und wählen Sie **Google APIs**, falls noch nicht installiert.
2. Klicken Sie auf die Registerkarte **SDK-Tools** . Wenn Sie Google Play Services noch nicht installiert haben, klicken Sie, wie unten dargestellt, auf **Google Play Services** . Klicken Sie dann auf **Übernehmen** , um die Installation auszuführen. Notieren Sie den SDK-Pfad, den Sie in einem späteren Schritt angeben müssen.

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Öffnen Sie die Datei `build.gradle` im App-Verzeichnis.

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Fügen Sie diese Zeile unter `dependencies` hinzu:

    ```text
    compile 'com.google.android.gms:play-services-gcm:12.0.0'
    ```
5. Klicken Sie auf der Symbolleiste auf das Symbol **Sync Project with Gradle Files** .
6. Öffnen Sie die Datei **AndroidManifest.xml** , und fügen Sie dem *application* -Tag das folgende Tag hinzu.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
