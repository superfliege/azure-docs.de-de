---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 0d9f0a24d84bd18bdf1fac84c744cc34a7d89ab3
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742457"
---
Wenn Sie die Profilbearbeitung in Ihrer Anwendung ermöglichen möchten, verwenden Sie einen Benutzerflow für die **Profilbearbeitung**. Dieser Benutzerflow beschreibt die Benutzeroberflächen, die Kunden bei der Profilbearbeitung durchlaufen, und den Inhalt der Token, die die Anwendung beim erfolgreichen Abschluss erhält.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Wählen Sie unter **Verwalten** die Option **Benutzerflows** aus, und klicken Sie auf **+Neuer Benutzerflow**.

![Auswählen von „Neuer Benutzerflow“](media/active-directory-b2c-create-profile-editing-policy/add-b2c-new-user-flow.png)

Wählen Sie auf der Registerkarte **Empfohlen** die Option **Profilbearbeitung** aus.

Geben Sie einen **Namen** für den Benutzerflow an, auf den in der App verwiesen werden kann. Geben Sie z. B. Folgendes ein: `SiPe`.

Aktivieren Sie unter **Identitätsanbieter** das Kontrollkästchen **Anmeldung mit lokalem Konto**. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert.

![„Anmeldung für lokales Konto“ als Identitätsanbieter auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-profile-editing-policy/add-b2c-profile-editing-identity-providers.png)

Klicken Sie unter **Benutzerattribute** auf **Mehr anzeigen**. Wählen Sie in der Spalte **Attribut sammeln** Attribute aus, die der Kunde in seinem Profil anzeigen und bearbeiten kann. Wählen Sie beispielsweise **Land/Region**, **Anzeigename** und **Postleitzahl** aus.

Wählen Sie in der Spalte **Anspruch zurückgeben** die Ansprüche aus, die nach einer erfolgreichen Profilbearbeitung in den an die Anwendung gesendeten Autorisierungstoken zurückgegeben werden sollen. Wählen Sie beispielsweise **Anzeigename** und **Postleitzahl** aus.

Klicken Sie auf **OK**.

![Einige Anwendungsansprüche auswählen und auf die Schaltfläche „OK“ klicken](media/active-directory-b2c-create-profile-editing-policy/add-b2c-user-attributes.png)

Klicken Sie auf **Erstellen**, um den Benutzerflow hinzuzufügen. Der Benutzerflow wird als **B2C_1_SiPe** aufgeführt. Das Präfix **B2C_1_** wird an den Namen angehängt.

Wählen Sie **Benutzerflow ausführen** aus. Überprüfen Sie die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Benutzerflow ausführen**.

![Auswählen und Ausführen des Benutzerflows](media/active-directory-b2c-create-profile-editing-policy/add-b2c-profile-editing-run-user-flow.png)

| Einstellung      | Wert  |
| ------------ | ------ |
| **Anwendung** | Contoso B2C-App |
| **Antwort-URL** | `https://localhost:44316/` |

Eine neue Browserregisterkarte wird geöffnet, auf der Sie die Benutzeroberfläche für die konfigurierte Profilbearbeitung überprüfen können.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Benutzerflows wirksam werden.
>