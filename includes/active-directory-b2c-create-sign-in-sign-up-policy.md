---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 3485cc96ad1d9ab8f89facf33687a7ab2be43b1e
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134289"
---
[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Wählen Sie im Richtlinienabschnitt der Einstellungen die Option **Registrierungs- oder Anmelderichtlinien**, und klicken Sie auf **+ Hinzufügen**.

![Registrierungs- oder Anmelderichtlinien auswählen und auf die Schaltfläche „Hinzufügen“ klicken](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-policy.png)

Geben Sie einen **Richtliniennamen** ein, auf den Ihre App verweisen kann. Geben Sie z. B. Folgendes ein: `SiUpIn`.

Wählen Sie **Identitätsanbieter** und dann **E-Mail-Registrierung** aus. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert. Klicken Sie auf **OK**.

![„E-Mail-Registrierung“ als Identitätsanbieter auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-identity-providers.png)

Wählen Sie **Registrierungsattribute** aus. Wählen Sie die Attribute aus, die der Kunde bei der Registrierung angeben soll. Wählen Sie beispielsweise **Land/Region**, **Anzeigename** und **Postleitzahl** aus. Klicken Sie auf **OK**.

![Einige Attribute auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-attributes.png)

Wählen Sie **Anwendungsansprüche** aus. Wählen Sie die Ansprüche aus, die in den Autorisierungstoken nach einer erfolgreichen Registrierung oder Anmeldung an die Anwendung zurückgegeben werden sollen. Wählen Sie beispielsweise **Anzeigename**, **Identitätsanbieter**, **Postleitzahl**, **User is new** (Benutzer ist neu) und **User's Object ID** (Objekt-ID des Benutzers) aus.

![Einige Anwendungsansprüche auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-application-claims.png)

Klicken Sie auf **Erstellen**, um die Richtlinie hinzuzufügen. Die Richtlinie wird als **B2C_1_SiUpIn** aufgeführt. Das Präfix **B2C_1_** wird an den Namen angehängt.

Öffnen Sie die Richtlinie, indem Sie **B2C_1_SiUpIn** auswählen. Überprüfen Sie die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Jetzt ausführen**.

![Richtlinie auswählen und ausführen](media/active-directory-b2c-create-sign-in-sign-up-policy/run-b2c-signup-signin-policy.png)

| Einstellung      | Wert  |
| ------------ | ------ |
| **Anwendungen** | Contoso B2C-App |
| **Antwort-URL auswählen** | `https://localhost:44316/` |

Eine neue Browserregisterkarte wird geöffnet, und Sie können entsprechend der Konfiguration die Benutzeroberfläche für die Registrierung oder Anmeldung überprüfen.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
>