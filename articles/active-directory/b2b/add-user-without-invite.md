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
ms.openlocfilehash: 589d9a417dae5c40d24d25c4ef864ce903fbfbe3
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
ms.locfileid: "34259577"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Hinzufügen von Gastbenutzern der B2B-Zusammenarbeit ohne Einladung

> [!NOTE]
> Die E-Mail-Einladung wird nur noch in einigen Sonderfällen benötigt. Weitere Informationen finden Sie unter [Azure Active Directory B2B collaboration invitation redemption](redemption-experience.md) (Azure Active Directory B2B-Zusammenarbeit: Einlösen von Einladungen).  

Sie können festlegen, dass ein Benutzer, z.B. ein Mitarbeiter einer Partnerorganisation, Ihrer Organisation Benutzer der Partnerorganisation hinzufügen kann, ohne dass dazu Einladungen erforderlich sind. Dazu müssen Sie dem entsprechenden Benutzer lediglich in dem Verzeichnis, das Sie für die Partnerorganisation verwenden, Enumerationsberechtigungen zuweisen. 

Die Erteilung dieser Berechtigungen empfiehlt sich in den folgenden Fällen:

1. Ein Benutzer in der Hostorganisation (z.B. WoodGrove) lädt einen Benutzer aus der Partnerorganisation (z.B. Sam@litware.com) als Gast ein.
2. Der Administrator der Hostorganisation [legt Richtlinien fest](delegate-invitations.md), die es Sam erlauben, weitere Benutzer aus der Partnerorganisation (Litware) zu identifizieren und hinzuzufügen.
3. Jetzt kann Sam dem Verzeichnis sowie den Gruppen und Anwendungen von WoodGrove weitere Benutzer von Litware hinzufügen, ohne dass Einladungen eingelöst werden müssen. Wenn Sam über entsprechende Enumerationsberechtigungen in Litware verfügt, erfolgt dies automatisch.

### <a name="next-steps"></a>Nächste Schritte

- [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
- [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](add-users-information-worker.md)
- [B2B-Zusammenarbeit: Einlösen von Einladungen](redemption-experience.md)
- [Delegieren von Einladungen zur Azure Active Directory B2B-Zusammenarbeit](delegate-invitations.md)

