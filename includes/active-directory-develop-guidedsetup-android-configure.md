---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7ff04789a4ba5e5a689b3d3815852bc0fbcdc6a7
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988347"
---
## <a name="register-your-application"></a>Anwendung registrieren

Es gibt zwei Methoden für die Registrierung Ihrer Anwendung. Diese werden in den beiden folgenden Abschnitten beschrieben.

### <a name="option-1-express"></a>Option 1: Express

1. Navigieren Sie zum [Anwendungsregistrierungsportal von Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2. Geben Sie in **Anwendungsname** einen Namen für Ihre Anwendung ein.
3. Vergewissern Sie sich, dass das Kontrollkästchen **Guided Setup** (Geführtes Setup) aktiviert ist, und klicken Sie anschließend auf **Erstellen**.
4. Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="option-2-advanced"></a>Option 2: Erweitert

1. Navigieren Sie zum [Anwendungsregistrierungsportal von Microsoft](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie im Feld **Anwendungsname** einen Namen für Ihre Anwendung ein.
3. Vergewissern Sie sich, dass das Kontrollkästchen **Guided Setup** (Geführtes Setup) deaktiviert ist, und klicken Sie anschließend auf **Erstellen**.
4. Klicken Sie auf **Plattform hinzufügen** > **Native Anwendung** und anschließend auf **Speichern**.
5. Öffnen Sie `MainActivity` unter **App** > **Java** > **{host}.{namespace}**. 
6. Ersetzen Sie *[Hier die Anwendungs-ID eingeben]* durch Ihre Anwendungs-/Client-ID:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. Öffnen Sie unter **App** > **Manifests** die Datei *AndroidManifest.xml*.
8. Fügen Sie in `manifest\application` die folgende Aktivität hinzu. Die `BrowserTabActivity`-Aktivität, die Microsoft den Rückruf an Ihre Anwendung nach Abschluss der Authentifizierung ermöglicht:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            
            <!--Add in your scheme/host from registered redirect URI-->
            <!--By default, the scheme should be similar to 'msal[appId]' -->
            <data android:scheme="msal[Enter the application Id here]"
                android:host="auth" />
        </intent-filter>
    </activity>
    ```
<!-- Workaround for Docs conversion bug -->
9. Ersetzen Sie in `BrowserTabActivity` `[Enter the application Id here]` durch die Anwendungs-/Client-ID.
