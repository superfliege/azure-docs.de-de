---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 0ab34d6234db9c13ffe82ccd0e8580217085f631
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742319"
---
Wenn Sie nur die Anmeldung bei Ihrer Anwendung ermöglichen möchten, verwenden Sie einen Benutzerflow für die **Anmeldung**. Dieser Benutzerflow beschreibt die Benutzeroberflächen, die Kunden bei der Anmeldung durchlaufen, und den Inhalt der Token, die die Anwendung bei erfolgreichen Anmeldungen erhält.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
Wählen Sie unter **Verwalten** die Option **Benutzerflows** aus.

Klicken Sie oben auf dem Blatt auf **+Neuer Benutzerflow**.

Wählen Sie unter **Benutzerflowtyp auswählen** die Option **Alle** und dann die zu verwendende Version der **Anmeldung** aus.

Der **Name** bestimmt den in Ihrer Anwendung verwendeten Namen des Benutzerflows für die Anmeldung. Geben Sie beispielsweise **SiIn** ein.

Wählen Sie unter **Identitätsanbieter** eine Option aus. Sie können auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert. Klicken Sie auf **OK**.

Klicken Sie unter **Anwendungsansprüche** auf **Mehr anzeigen**.

Wählen Sie in der Spalte **Anspruch zurückgeben** die Ansprüche aus, die nach einer erfolgreichen Anmeldung in den an die Anwendung gesendeten Token zurückgegeben werden sollen. Wählen Sie beispielsweise **Anzeigename**, **Identitätsanbieter**, **Postleitzahl** und **Objekt-ID des Benutzers** aus. Klicken Sie auf **OK**.

Klicken Sie auf **Create**. Beachten Sie, dass der soeben erstellte Benutzerflow als **B2C_1_SiIn** angezeigt wird. (Das Fragment **B2C\_1\_** wird automatisch hinzugefügt.)

Klicken Sie auf **Benutzerflow ausführen**.

Wählen Sie in der Dropdownliste **Anwendung** die App **Contoso B2C** und `https://localhost:44321/` in der Dropdownliste **Antwort-URL** aus.

Klicken Sie auf **Benutzerflow ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Anmeldung bei Ihrer Anwendung durchgehen.

> [!NOTE]
> Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Benutzerflows wirksam werden.
>