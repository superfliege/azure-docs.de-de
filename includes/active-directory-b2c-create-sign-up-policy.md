---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 17c0213d63879687e9c6d5f8dca06b9113c44af8
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742413"
---
Wenn Sie nur die Registrierung bei Ihrer Anwendung ermöglichen möchten, verwenden Sie einen Benutzerflow für die **Registrierung**. Dieser Benutzerflow beschreibt die Benutzeroberflächen, die Kunden bei der Registrierung durchlaufen, und den Inhalt der Token, die die Anwendung bei erfolgreichen Registrierungen erhält.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Wählen Sie unter **Verwalten** die Option **Benutzerflows** aus.

Klicken Sie oben auf dem Blatt auf **+Neuer Benutzerflow**.

Wählen Sie unter **Benutzerflowtyp auswählen** die Option **Alle** und dann die Version der zu verwendenden **Registrierung** aus.

Der **Name** bestimmt den in Ihrer Anwendung verwendeten Namen des Benutzerflows für die Registrierung. Geben Sie beispielsweise **SiUp** ein.

Wählen Sie unter **Identitätsanbieter** die Option **E-Mail-Registrierung** aus. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert.

Klicken Sie unter **Benutzerattribute und Ansprüche** auf **Mehr anzeigen**.

Wählen Sie in der Spalte **Attribut sammeln** die Attribute aus, die Sie während der Registrierung vom Kunden erfassen möchten. Wählen Sie z.B. **Land/Region**, **Angezeigter Name** und **Postleitzahl** aus.

Wählen Sie in der Spalte **Anspruch zurückgeben** die Ansprüche aus, die in den Token nach einer erfolgreichen Registrierung an die Anwendung zurückgegeben werden sollen. Wählen Sie z.B. **Angezeigter Name**, **Identitätsanbieter**, **Postleitzahl**, **User is new** (Benutzer ist neu) und **User's Object ID** (Objekt-ID des Benutzers) aus.

Klicken Sie auf **OK**.

Klicken Sie auf **Create**. Der erstellte Benutzerflow wird als **B2C_1_SiUp** angezeigt. (Das Fragment **B2C\_1\_** wird automatisch hinzugefügt.)

Klicken Sie auf **Benutzerflow ausführen**.

Wählen Sie in der Dropdownliste **Anwendung** die App **Contoso B2C** und `https://localhost:44321/` in der Dropdownliste **Antwort-URL** aus.

Klicken Sie auf **Benutzerflow ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Registrierung bei Ihrer Anwendung durchgehen.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Benutzerflows wirksam werden.
>