---
title: Worum handelt es sich beim Verbund mit Azure AD? | Microsoft-Dokumentation
description: Hier wird der Verbund mit Azure AD beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 278935ae7d29680e4fa741d59370e75ee0e1e93d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197703"
---
# <a name="what-is-federation-with-azure-ad"></a>Worum handelt es sich beim Verbund mit Azure AD?

Ein Verbund ist eine Sammlung von Domänen mit gegenseitiger Vertrauensbeziehung. Der Vertrauensgrad kann variieren, beinhaltet aber in der Regel eine Authentifizierung und fast immer eine Autorisierung. Ein typischer Verbund kann eine Reihe von Organisationen umfassen, die sich gegenseitig vertrauen und gemeinsam auf bestimmte Ressourcen zugreifen.

Sie können für Ihre lokale Umgebung einen Verbund mit Azure AD einrichten und diesen zur Authentifizierung und Autorisierung verwenden.  Diese Anmeldemethode stellt sicher, dass die gesamte Benutzerauthentifizierung lokal abgewickelt wird.  Mit dieser Methode können Administratoren eine striktere Zugriffssteuerung implementieren. Es sind Verbünde mit AD FS und PingFederate verfügbar.

![Identitätsverbund](./media/whatis-hybrid-identity/federated-identity.png)


> [!TIP]
> Wenn Sie sich für einen Verbund mit Active Directory Federation Services (AD FS) entscheiden, können Sie optional die Kennworthashsynchronisierung als zusätzliche Sicherheitsmaßnahme für den Fall aktivieren, dass Ihre AD FS-Infrastruktur ausfällt.


## <a name="next-steps"></a>Nächste Schritte

- [Was ist eine Hybrididentität?](whatis-phs.md)
- [Worum handelt es sich bei Azure AD Connect und Connect Health?](whatis-azure-ad-connect.md)
- [Was ist Kennworthashsynchronisierung?](whatis-phs.md)
- [Was ist ein Verbund?](whatis-fed.md)
- [Nahtlose einmalige Anmeldung mit Azure Active Directory](how-to-connect-sso.md)
- [Funktionsweise eines Verbunds](how-to-connect-fed-whatis.md)
- [Verbund mit PingFederate](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)
