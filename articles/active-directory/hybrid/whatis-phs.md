---
title: Was ist die Kennworthashsynchronisierung mit Azure AD? | Microsoft-Dokumentation
description: Hier wird die Kennworthashsynchronisierung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 12/05/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: d11759fbc3354364af3f78599ff9cfb25674c612
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175141"
---
# <a name="what-is-password-hash-synchronization-with-azure-ad"></a>Was ist die Kennworthashsynchronisierung mit Azure AD?
Die Kennworthashsynchronisierung ist eine der Anmeldemethoden, die zur Implementierung von Hybrididentitäten verwendet wird. Azure AD Connect synchronisiert einen Hash eines Benutzerkennworthashs aus einer lokalen Active Directory-Instanz mit einer cloudbasierten Azure AD-Instanz.

Die Kennworthashsynchronisierung ist eine Erweiterung des durch die Azure AD Connect-Synchronisierung implementierten Verzeichnissynchronisierungsfeatures. Dieses Feature kann für die Anmeldung bei Azure AD-Dienste wie Office 365 verwendet werden. Sie melden sich bei dem Dienst mit dem gleichen Kennwort an, das Sie zur Anmeldung bei Ihrer lokalen Active Directory-Instanz verwenden.

![Was ist Azure AD Connect?](./media/how-to-connect-password-hash-synchronization/arch1.png)

Dank der Kennworthashsynchronisierung benötigen Benutzer nur noch ein einzelnes Kennwort. Die Kennworthashsynchronisierung bietet folgende Vorteile:

* Verbessern der Produktivität Ihrer Benutzer
* Reduzieren der Helpdeskkosten  

Wenn Sie sich bei der Anmeldemethode für einen [Verbund mit Active Directory Federation Services (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) entscheiden, können Sie die Kennworthashsynchronisierung optional als zusätzliche Sicherheitsmaßnahme aktivieren.

Um die Kennworthashsynchronisierung in Ihrer Umgebung zu verwenden, müssen Sie Folgendes ausführen:

* Installieren von Azure AD Connect  
* Konfigurieren der Verzeichnissynchronisierung zwischen Ihrer lokalen Active Directory-Instanz und Ihrer Azure Active Directory-Instanz
* Aktivieren Sie die Kennworthashsynchronisierung.



Weitere Informationen finden Sie unter [Hybrididentität und Identitätslösungen von Microsoft?](whatis-hybrid-identity.md).




## <a name="next-steps"></a>Nächste Schritte

- [Was ist eine Hybrididentität?](whatis-phs.md)
- [Worum handelt es sich bei Azure AD Connect und Connect Health?](whatis-azure-ad-connect.md)
- [Worum handelt es sich bei der Passthrough-Authentifizierung (PTA)?](how-to-connect-pta.md)
- [Was ist ein Verbund?](whatis-fed.md)
- [Nahtlose einmalige Anmeldung mit Azure Active Directory](how-to-connect-sso.md)
- [So funktioniert die Kennworthashsynchronisierung](how-to-connect-password-hash-synchronization.md)
