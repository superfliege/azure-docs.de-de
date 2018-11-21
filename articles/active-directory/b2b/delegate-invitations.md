---
title: Delegieren von Einladungen zur Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation
description: Die Eigenschaften von Benutzern der Azure Active Directory B2B-Zusammenarbeit sind konfigurierbar.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: fd00fb8da3cf36518f9e28e827e59fd7ff45d687
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622209"
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Delegieren von Einladungen zur Azure Active Directory B2B-Zusammenarbeit

Mit der Azure Active Directory (Azure AD) Business-to-Business-Zusammenarbeit (B2B) müssen Sie jetzt kein globaler Administrator mehr sein, um Einladungen zu senden. Stattdessen können Sie Richtlinien verwenden und Einladungen an Benutzer delegieren, deren Rollen ihnen erlauben, Einladungen zu senden. Eine wichtige neue Möglichkeit, die Einladung von Gastbenutzern zu delegieren, ist die Rolle „Gasteinladender“.

## <a name="guest-inviter-role"></a>Rolle „Gasteinladender“
Sie können einem Benutzer die Rolle „Gasteinladender“ zuweisen, damit dieser Einladungen senden kann. Sie müssen kein Mitglied der Rolle der globalen Administratoren sein, um Einladungen zu senden. Standardmäßig können auch normale Benutzer die Einladungs-API aufrufen, sofern ein globaler Administrator nicht Einladungen für normale Benutzer deaktiviert hat. Benutzer können eine API auch über das Azure-Portal oder über PowerShell aufrufen.

Hier ist ein Beispiel, das zeigt, wie PowerShell verwendet wird, um einen Benutzer der Rolle „Gasteinladender“ hinzuzufügen:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Steuern, wer einladen kann

Klicken Sie auf „Azure Active Directory > Benutzereinstellungen > Externe Einstellungen zur Zusammenarbeit verwalten“.

![externalusers](https://user-images.githubusercontent.com/13383753/45905128-2c47f680-bda4-11e8-955d-6219c67935e0.PNG)

In der Azure AD B2B-Zusammenarbeit kann ein Mandantenadministrator folgende Einladungsrichtlinien festlegen:

- Einladungen deaktivieren.
- Nur Administratoren und Benutzer in der Rolle „Gasteinladender“ dürfen einladen.
- Administratoren, Benutzer in der Rolle „Gasteinladender“ und Mitglieder dürfen einladen.
- Alle Benutzer, einschließlich Gästen, dürfen einladen.

Standardmäßig wird die Zahl der Mandanten auf 4 festgelegt. (Alle Benutzer, einschließlich Gästen, dürfen B2B-Benutzer einladen.)

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

- [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
- [Hinzufügen von Gastbenutzern der B2B-Zusammenarbeit ohne Einladung](add-user-without-invite.md)
- [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](add-guest-to-role.md)


