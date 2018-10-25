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
ms.openlocfilehash: fe2b02b2495b4f37cbc90e1ddbeaca43b41d008c
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843145"
---
## <a name="add-the-applications-registration-to-your-code"></a>Hinzufügen der Registrierung der Anwendung zu Ihrem Code

In diesem Schritt müssen Sie die Anwendungs-/Client-ID Ihrem Projekt hinzufügen.

1.  Öffnen Sie `MainActivity` (unter `app` > `java` > *`{host}.{namespace}`*).
2.  Ersetzen Sie die Zeile, die mit `final static String CLIENT_ID` beginnt, durch:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Öffnen Sie: `app` > `manifests` > `AndroidManifest.xml`
4. Fügen Sie die folgende Aktivität zu `manifest\application` hinzu. Die `BrowserTabActivity` gestattet Microsoft den Rückruf an Ihre Anwendung nach Abschluss der Authentifizierung:

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
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

