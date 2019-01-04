---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 78abb190dccd27c5bf70dfe12f978e1118601815
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742433"
---
Um eine differenzierte Kennwortzurücksetzung in Ihrer Anwendung zu ermöglichen, verwenden Sie einen Benutzerflow für die **Kennwortzurücksetzung**. Beachten Sie, dass die Option zur mandantenweiten Kennwortzurücksetzung [hier](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md) angegeben wird. Dieser Benutzerflow beschreibt die Benutzeroberflächen, die die Kunden während der Kennwortzurücksetzung durchlaufen, und die Inhalte der Token, die die Anwendung beim erfolgreichen Abschluss erhält.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Wählen Sie unter **Verwalten** die Option **Benutzerflows** aus, und klicken Sie auf **+Neuer Benutzerflow**.

![Auswählen von „Neuer Benutzerflow“](media/active-directory-b2c-create-password-reset-policy/add-b2c-new-user-flow.png)

Wählen Sie auf der Registerkarte **Empfohlen** die Option **Kennwortzurücksetzung** aus.

Geben Sie einen **Namen** für den Benutzerflow an, auf den in der App verwiesen werden kann. Geben Sie z. B. Folgendes ein: `SSPR`.

Aktivieren Sie unter **Identitätsanbieter** das Kontrollkästchen **Kennwort mittels E-Mail-Adresse zurücksetzen**.

![Eingeben des Namens und Auswählen von „Kennwort mittels E-Mail-Adresse zurücksetzen“ als Identitätsanbieter](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Klicken Sie unter **Anwendungsansprüche** auf **Mehr anzeigen**, und wählen Sie die Ansprüche aus, die nach einer erfolgreichen Kennwortzurücksetzung in den an die Anwendung gesendeten Autorisierungstoken zurückgegeben werden sollen. Wählen Sie beispielsweise **Objekt-ID des Benutzers**.

Klicken Sie auf **OK**.

![Einige Anwendungsansprüche auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Klicken Sie auf **Erstellen**, um den Benutzerflow hinzuzufügen. Der Benutzerflow wird als **B2C_1_SSPR** aufgeführt. Das Präfix **B2C_1_** wird an den Namen angehängt.

Klicken Sie auf **Benutzerflow ausführen**. Überprüfen Sie die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Benutzerflow ausführen**.

![Auswählen und Ausführen des Benutzerflows](media/active-directory-b2c-create-password-reset-policy/add-b2c-sspr-run-user-flow.png)

| Einstellung      | Wert  |
| ------------ | ------ |
| **Anwendung** | Contoso B2C-App |
| **Antwort-URL auswählen** | `https://localhost:44316/` |

Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Kennwortzurücksetzung in Ihrer Anwendung überprüfen.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
>
