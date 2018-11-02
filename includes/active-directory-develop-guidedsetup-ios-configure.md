---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: brandwe
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: brandwe
ms.custom: include file
ms.openlocfilehash: 1604b7c9ee9888375e65aa679803c6e996e13b14
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988262"
---
## <a name="register-your-application"></a>Anwendung registrieren

Es gibt zwei Methoden für die Registrierung Ihrer Anwendung. Diese werden in den beiden folgenden Abschnitten beschrieben.

### <a name="option-1-express-mode"></a>Option 1: Expressmodus

Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:

1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3. Überprüfen Sie, ob die Option für „Angeleitetes Setup“ aktiviert ist.
4. Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="option-2-advanced-mode"></a>Option 2: Erweiterter Modus

1. Navigieren Sie zum [Microsoft App-Registrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung ein.
3. Stellen Sie sicher, dass die Option für „Angeleitetes Setup“ deaktiviert ist.
4. Wählen Sie `Add Platform` und dann `Native Application` aus.
5. Wählen Sie `Save` aus.
6. Kehren Sie zu Xcode zurück. Ersetzen Sie in `ViewController.swift` die mit „`let kClientID`“ beginnende Zeile mit der Anwendungs-ID, die Sie gerade registriert haben:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Halten Sie STRG gedrückt, und klicken Sie auf <code>Info.plist</code> im Kontextmenü. Klicken Sie dann auf: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
Fügen Sie unter dem <code>dict</code>-Stammknoten Folgendes hinzu:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Ersetzen Sie <i><code>[Your_Application_Id_Here]</code></i> durch die Anwendungs-ID, die Sie gerade registriert haben
</li>
</ol>
