---
title: Delegieren von Einladungen zur B2B-Zusammenarbeit – Azure Active Directory | Microsoft-Dokumentation
description: Die Eigenschaften von Benutzern der Azure Active Directory B2B-Zusammenarbeit sind konfigurierbar.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18d40397f30b471699f42878a38c88efebcc6305
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674512"
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

Wählen Sie in Azure Active Directory die Option **Benutzereinstellungen** aus. Wählen Sie unter **Externe Benutzer** die Option **Externe Einstellungen zur Zusammenarbeit verwalten** aus.

> [!NOTE]
> Sie können auch über die Seite **Organisationsbeziehungen** auf die **Einstellungen für externe Zusammenarbeit** zugreifen. Navigieren Sie in Azure Active Directory unter **Verwalten** zu **Organisationsbeziehungen** > **Einstellungen**.

![Einstellungen für externe Zusammenarbeit](./media/delegate-invitations/control-who-to-invite.png)

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


