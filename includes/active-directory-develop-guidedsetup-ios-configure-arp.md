---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: 594b4090fcdfa18432563269743b88cb9d2d723b
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58214105"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen der Registrierungsinformationen der Anwendung zu Ihrer App

In diesem Schritt müssen Sie die Anwendungs-ID Ihrem Projekt hinzufügen:

1. Ersetzen Sie in `ViewController.swift` die Zeile, die mit „`let kClientID`“ beginnt, durch:

```swift
let kClientID = "[Enter the application Id here]"
```

<!-- Workaround for Docs conversion bug -->
<ol start="2">
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
            <string>msal[Enter the application Id here]</string>
        </array>
    </dict>
</array>
```
