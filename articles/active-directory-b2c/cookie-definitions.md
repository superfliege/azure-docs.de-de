---
title: Cookie-Definitionen – Azure Active Directory B2C | Microsoft-Dokumentation
description: Enthält Definitionen für die Cookies, die in Azure Active Directory B2C verwendet werden.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7864320b71416d1b06661b8ae96c6113962250dd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703977"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Cookie-Definitionen für Azure Active Directory B2C

Die folgende Tabelle enthält die Cookies, die in Azure Active Directory B2C verwendet werden.

| NAME | Domäne | Ablauf | Zweck |
| ----------- | ------ | -------------------------- | --------- |
| x-ms-cpim-admin | main.b2cadmin.ext.azure.com | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Enthält mandantenübergreifende Daten zur Benutzermitgliedschaft: Mandanten, zu denen ein Benutzer gehört, und die Ebene der Mitgliedschaft („Admin“ oder „Benutzer“). |
| x-ms-cpim-slice | login.microsoftonline.com, b2clogin.com, mit Branding versehene Domäne | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Weiterleiten von Anforderungen an die entsprechende Produktionsinstanz. |
| x-ms-cpim-trans | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Nachverfolgen von Transaktionen (Anzahl der Authentifizierungsanforderungen an Azure AD B2C) und der aktuellen Transaktion. |
| x-ms-cpim-sso:{Id} | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Verwalten der Sitzung mit einmaligem Anmelden (SSO). |
| x-ms-cpim-cache:{id}_n | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md), erfolgreiche Authentifizierung | Zum Verwalten des Anforderungsstatus. |
| x-ms-cpim-csrf | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Token der websiteübergreifenden Anforderungsfälschung zum Schutz vor CSRF-Angriffen. |
| x-ms-cpim-dc | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Für Azure AD B2C-Netzwerkrouting. |
| x-ms-cpim-ctx | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Kontext |
| x-ms-cpim-rp | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Speichern der Mitgliedschaftsdaten für den Ressourceourcenanbieter-Mandanten. |
| x-ms-cpim-rc | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Speichern des Relay-Cookies. |

