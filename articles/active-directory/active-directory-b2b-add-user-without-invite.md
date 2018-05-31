---
title: Hinzufügen von Benutzern der B2B-Zusammenarbeit zu Azure Active Directory ohne Einladung | Microsoft-Dokumentation
description: Sie können festlegen, dass ein Gastbenutzer Ihrem Azure AD andere Gastbenutzer hinzufügen kann, ohne dass eine Einladung in der Azure Active Directory B2B-Zusammenarbeit erforderlich ist.
services: active-directory
documentationcenter: ''
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5a37a5a14dcb07db7e078558072f7edad7432d9b
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075622"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Hinzufügen von Gastbenutzern der B2B-Zusammenarbeit ohne Einladung

> [!NOTE]
> Die E-Mail-Einladung wird nur noch in einigen Sonderfällen benötigt. Weitere Informationen finden Sie unter [Azure Active Directory B2B collaboration invitation redemption](active-directory-b2b-redemption-experience.md) (Azure Active Directory B2B-Zusammenarbeit: Einlösen von Einladungen).  

Sie können festlegen, dass ein Benutzer, z.B. ein Mitarbeiter einer Partnerorganisation, Ihrer Organisation Benutzer der Partnerorganisation hinzufügen kann, ohne dass dazu Einladungen erforderlich sind. Dazu müssen Sie dem entsprechenden Benutzer lediglich in dem Verzeichnis, das Sie für die Partnerorganisation verwenden, Enumerationsberechtigungen zuweisen. 

Die Erteilung dieser Berechtigungen empfiehlt sich in den folgenden Fällen:

1. Ein Benutzer in der Hostorganisation (z.B. WoodGrove) lädt einen Benutzer aus der Partnerorganisation (z.B. Sam@litware.com) als Gast ein.
2. Der Administrator der Hostorganisation [richtet Richtlinien ein](active-directory-b2b-delegate-invitations.md), die es Sam erlauben, weitere Benutzer aus der Partnerorganisation (Litware) zu identifizieren und hinzuzufügen.
3. Jetzt kann Sam dem Verzeichnis sowie den Gruppen und Anwendungen von WoodGrove weitere Benutzer von Litware hinzufügen, ohne dass Einladungen eingelöst werden müssen. Wenn Sam über entsprechende Enumerationsberechtigungen in Litware verfügt, erfolgt dies automatisch.

### <a name="next-steps"></a>Nächste Schritte

- [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
- [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
- [Delegieren von Einladungen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-delegate-invitations.md)

