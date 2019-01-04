---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: f23d2b02bc2a23c5333a48a50532c03f3aa6a031
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742504"
---
[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Wählen Sie unter **Verwalten** die Option **Benutzerflows** aus, und klicken Sie auf **+Neuer Benutzerflow**.

![Auswählen von „Neuer Benutzerflow“](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-user-flow.png)

Wählen Sie auf der Registerkarte **Empfohlen** die Option **Registrierung und Anmeldung** aus.

![Auswählen des Benutzerflows „Registrierung und Anmeldung“](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-user-flow-type.png)

Geben Sie einen **Namen** für den Benutzerflow an, auf den in der App verwiesen werden kann. Geben Sie z. B. Folgendes ein: `SiUpIn`.

Aktivieren Sie unter **Identitätsanbieter** die Option **E-Mail-Registrierung**. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert.

Wählen Sie unter **Mehrstufige Authentifizierung** eine der Optionen **Aktiviert** oder **Deaktiviert** aus.

![Eingeben eines Namens und Auswählen von „E-Mail-Registrierung“ als Identitätsanbieter](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-name-identity-providers.png)

Wählen Sie unter **Benutzerattribute und Ansprüche** die Option **Mehr anzeigen** aus, um die vollständige Liste der Attribute und Ansprüche anzuzeigen, die Sie auswählen können.

Wählen Sie in der Spalte **Attribut sammeln** die Attribute aus, die der Kunde bei der Registrierung angeben soll. Wählen Sie beispielsweise **Land/Region**, **Anzeigename** und **Postleitzahl** aus.

Wählen Sie in der Spalte **Anspruch zurückgeben** die Ansprüche aus, die nach einer erfolgreichen Registrierung oder Anmeldung in den an Ihre Anwendung gesendeten Autorisierungstoken zurückgegeben werden sollen. Wählen Sie beispielsweise **Anzeigename**, **Identitätsanbieter**, **Postleitzahl**, **User is new** (Benutzer ist neu) und **User's Object ID** (Objekt-ID des Benutzers) aus.

Klicken Sie auf **OK**.

![Auswählen von Benutzerattributen und Ansprüchen und Klicken auf die Schaltfläche „OK“](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-all-attributes.png)

Klicken Sie auf **Erstellen**, um den Benutzerflow hinzuzufügen. Der Benutzerflow wird als **B2C_1_SiUpIn** aufgeführt. Das Präfix **B2C_1_** wird an den Namen angehängt.

Wählen Sie **Benutzerflow ausführen** aus. Überprüfen Sie die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Benutzerflow ausführen**.

![Auswählen von „Benutzerflow ausführen“](media/active-directory-b2c-create-sign-in-sign-up-policy/run-user-flow-b2c-signup-signin.png)

| Einstellung      | Wert  |
| ------------ | ------ |
| **Anwendung** | Contoso B2C-App |
| **Antwort-URL** | `https://localhost:44316/` |

Eine neue Browserregisterkarte wird geöffnet, und Sie können entsprechend der Konfiguration die Benutzeroberfläche für die Registrierung oder Anmeldung überprüfen.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Benutzerflows wirksam werden.
>