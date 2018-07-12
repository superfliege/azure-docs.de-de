---
title: Includedatei
description: Includedatei
services: active-directory-b2c
author: davidmu1
ms.service: active-directory-b2c
ms.topic: include
ms.date: 04/09/2018
ms.author: davidmu
ms.custom: include file
ms.openlocfilehash: 8363d023e89c77aabc0d123f19264c9a0758a656
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740462"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Verwenden Sie zum Registrieren der mobilen oder nativen Anwendung die in der Tabelle angegebenen Einstellungen.

![Beispielregistrierungseinstellungen für eine neue mobile oder native Anwendung](./media/active-directory-b2c-register-mobile-native-app/b2c-new-mobile-native-app-settings.png)

| Einstellung      | Beispielwert  | BESCHREIBUNG                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Name** | Contoso B2C-App | Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. |
| **Nativer Client** | Ja | Wählen Sie **Ja** für eine mobile oder native Anwendung. |
| **Benutzerdefinierter Umleitungs-URI** | `com.onmicrosoft.contoso.appname://redirect/path` | Geben Sie einen Umleitungs-URI mit einem benutzerdefinierten Schema ein. Stellen Sie sicher, dass Sie einen [gültigen Umleitungs-URI](../articles/active-directory-b2c/active-directory-b2c-app-registration.md) auswählen und keine Sonderzeichen wie Unterstriche eingeben. |

Klicken Sie auf **Erstellen** , um Ihre Anwendung zu registrieren.

Die neu registrierte Anwendung wird in der Anwendungsliste für den B2C-Mandanten angezeigt. Wählen Sie Ihre mobile oder native App in der Liste aus. Der Eigenschaftenbereich für die Anwendung wird angezeigt.

![Anwendungseigenschaften](./media/active-directory-b2c-register-mobile-native-app/b2c-mobile-native-app-properties.png)

Notieren Sie sich die global eindeutige **Anwendungsclient-ID**. Die ID wird im Code Ihrer Anwendung verwendet.

Wenn Ihre native Anwendung eine durch Azure AD B2C geschützte Web-API aufruft, gehen Sie wie folgt vor:
   1. Erstellung eines Anwendungsgeheimnisses. Klicken Sie hierzu auf dem Blatt **Schlüssel** auf die Schaltfläche **Schlüssel generieren**. Notieren Sie sich den Wert für **App-Schlüssel**. Sie können den Wert als Anwendungsgeheimnis im Code Ihrer Anwendung verwenden.
   2. Klicken Sie auf **API-Zugriff** und dann auf **Hinzufügen**, und wählen Sie Ihre Web-API und Bereiche (Berechtigungen) aus.

> [!NOTE]
> **Geheime Anwendungsschlüssel** sind wichtige Sicherheitsanmeldeinformationen, die entsprechend geschützt werden müssen.
> 
