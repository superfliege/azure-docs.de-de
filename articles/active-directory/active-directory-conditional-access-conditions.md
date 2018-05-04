---
title: Bedingungen beim bedingten Zugriff in Azure Active Directory | Microsoft-Dokumentation
description: Es wird beschrieben, wie Zuweisungen beim bedingten Zugriff in Azure Active Directory zum Auslösen einer Richtlinie verwendet werden.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 168301bbd0e7a59330ee6c87d1821db3fca39f67
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="conditions-in-azure-active-directory-conditional-access"></a>Bedingungen beim bedingten Zugriff in Azure Active Directory 

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie den Zugriff von autorisierten Benutzern auf Ihre Cloud-Apps steuern. Bei einer Richtlinie für bedingten Zugriff definieren Sie die Reaktion („do this“) für den Grund der Auslösung Ihrer Richtlinie („when this happens“). 

![Kontrolle](./media/active-directory-conditional-access-conditions/10.png)


Im Kontext des bedingten Zugriffs gilt:

- „**When this happens**“ fällt unter **Bedingungen**. 
- „**Then do this**“ fällt unter **Zugriffssteuerungen**.

Die Kombination aus Ihren Bedingungen und Ihren Zugriffssteuerungen ergibt eine Richtlinie für bedingten Zugriff.

![Kontrolle](./media/active-directory-conditional-access-conditions/61.png)

In diesem Artikel erhalten Sie eine Übersicht über die Bedingungen und ihre Verwendung in einer Richtlinie für bedingten Zugriff. 


## <a name="users-and-groups"></a>Benutzer und Gruppen

Die Bedingung für Benutzer und Gruppen ist in einer Richtlinie für bedingten Zugriff obligatorisch. In Ihrer Richtlinie können Sie entweder die Option **Alle Benutzer** oder bestimmte Benutzer und Gruppen auswählen.

![Kontrolle](./media/active-directory-conditional-access-conditions/02.png)

Auswahl von:

- **Alle Benutzer**: Ihre Richtlinie wird auf alle Benutzer im Verzeichnis angewendet. Hierzu gehören auch Gastbenutzer.

- **Benutzer und Gruppen auswählen**: Sie können die Richtlinie auf bestimmte Gruppen von Benutzern ausrichten. Beispielsweise können Sie eine Gruppe auswählen, die alle Mitglieder der HR-Abteilung enthält, wenn Sie eine HR-App als Cloud-App ausgewählt haben. 

- Dies kann eine Gruppe sein (beliebiger Typ von Gruppe in Azure AD), die dynamische oder zugewiesene Sicherheits- und Verteilungsgruppen umfasst.

Sie können bestimmte Benutzer und Gruppen auch aus einer Richtlinie ausschließen. Ein häufiger Anwendungsfall sind Dienstkonten, falls bei Ihrer Richtlinie die mehrstufige Authentifizierung erzwungen wird. 

Die Ausrichtung auf bestimmte Gruppen von Benutzern ist für die Bereitstellung einer neuen Richtlinie nützlich. Bei einer neuen Richtlinie sollten Sie nur eine Ausrichtung auf eine Anfangsgruppe von Benutzern festlegen, um das Verhalten der Richtlinie zu überprüfen. 



## <a name="cloud-apps"></a>Cloud-Apps 

Eine Cloud-App ist eine Website- oder Dienst-App. Dies schließt auch Websites ein, die per Azure-Anwendungsproxy geschützt sind. Eine ausführliche Beschreibung der unterstützten Cloud-Apps finden Sie unter [Zuweisungen von Cloud-Apps](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

Die Bedingung für Cloud-Apps ist in einer Richtlinie für bedingten Zugriff obligatorisch. In Ihrer Richtlinie können Sie die Option **Alle Cloud-Apps** oder bestimmte Apps wählen.

![Kontrolle](./media/active-directory-conditional-access-conditions/03.png)

Sie können Folgendes auswählen:

- **Alle Cloud-Apps**: Dies ist die Basisoption für Richtlinien, die auf die gesamte Organisation angewendet werden sollen. Ein häufiger Anwendungsfall für diese Auswahl ist eine Richtlinie, für die die mehrstufige Authentifizierung erforderlich ist, wenn für eine Cloud-App ein Anmelderisiko erkannt wird. Eine auf **Alle Cloud-Apps** angewendete Richtlinie gilt für den Zugriff auf alle Websites und Dienste. Diese Einstellung ist nicht auf die Cloud-Apps in der Liste **Select Cloud apps** (Cloud-Apps auswählen) beschränkt.

- Einzelne Cloud-Apps zur Ausrichtung auf bestimmte Dienste anhand der Richtlinie. Sie können beispielsweise vorgeben, dass Benutzer über ein [konformes Gerät](active-directory-conditional-access-policy-connected-applications.md) verfügen müssen, um auf SharePoint Online zuzugreifen. Diese Richtlinie wird auch auf andere Dienste angewendet, wenn diese auf SharePoint-Inhalte zugreifen, z.B. Microsoft Teams. 

Sie können auch bestimmte Apps aus einer Richtlinie ausschließen. Diese Apps unterliegen dann aber weiterhin den Richtlinien, die auf die Dienste angewendet wurden, auf die zugegriffen wird. 



## <a name="sign-in-risk"></a>Anmelderisiko

Ein Anmelderisiko ist ein Hinweis auf die Wahrscheinlichkeit (hoch, mittel oder niedrig), dass ein Anmeldeversuch nicht vom rechtmäßigen Besitzer eines Benutzerkontos durchgeführt wurde. Azure AD berechnet die Anmelderisikostufe während der Anmeldung eines Benutzers. Sie können die berechnete Anmelderisikostufe als Bedingung in einer Richtlinie für den bedingten Zugriff verwenden. 

![Bedingungen](./media/active-directory-conditional-access-conditions/22.png)

Für die Nutzung dieser Bedingung muss [Azure Active Directory Identity Protection](active-directory-identityprotection.md) aktiviert sein.
 
Häufige Anwendungsfälle für diese Bedingung sind Richtlinien, für die Folgendes gilt:

- Blockieren von Benutzern mit einem hohen Anmelderisiko, um zu verhindern, dass potenziell unberechtigte Benutzer auf Ihre Cloud-Apps zugreifen. 
- Erzwingen der mehrstufigen Authentifizierung für Benutzer mit einem mittleren Anmelderisiko. Indem die mehrstufige Authentifizierung erzwungen wird, können Sie zusätzliches Vertrauen schaffen, dass die Anmeldung vom rechtmäßigen Besitzer eines Kontos durchgeführt wird.

Weitere Informationen finden Sie unter [Riskante Anmeldungen](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Geräteplattformen

Die Geräteplattform ist durch das Betriebssystem gekennzeichnet, das auf dem Gerät ausgeführt wird. Azure AD identifiziert die Plattform, indem vom Gerät, z.B. einem Benutzer-Agent, bereitgestellte Informationen verwendet werden. Da dies unbestätigte Informationen sind, ist es ratsam, auf alle Plattformen eine Richtlinie anzuwenden: entweder durch das Blockieren des Zugriffs, das Erzwingen der Einhaltung von Intune-Richtlinien oder das Erzwingen einer Einbindung des Geräts in die Domäne. Die Standardvorgehensweise ist die Anwendung einer Richtlinie auf alle Geräteplattformen. 


![Bedingungen](./media/active-directory-conditional-access-conditions/24.png)

Eine vollständige Liste der unterstützten Geräteplattformen finden Sie unter [Geräteplattformbedingung](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Ein häufiger Anwendungsfall für diese Bedingung ist eine Richtlinie, mit der der Zugriff auf Ihre Cloud-Apps auf [vertrauenswürdige Geräte](active-directory-conditional-access-policy-connected-applications.md#trusted-devices) beschränkt wird. Informationen zu weiteren Szenarien, z.B. zur Geräteplattformbedingung, finden Sie unter [App-basierter bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-mam.md).


## <a name="locations"></a>Standorte

Mit Standorten haben Sie die Möglichkeit Bedingungen zu definieren, je nachdem wo ein Verbindungsversuch initiiert wurde. 
     
![Bedingungen](./media/active-directory-conditional-access-conditions/25.png)

Häufige Anwendungsfälle für diese Bedingung sind Richtlinien, für die Folgendes gilt:

- Erzwingen der mehrstufigen Authentifizierung für Benutzer beim Zugriff auf einen Dienst, wenn sie sich außerhalb des Unternehmensnetzwerks befinden  

- Blockieren des Zugriffs auf einen Dienst für Benutzer, die sich in bestimmten Ländern oder Regionen aufhalten 

Weitere Informationen finden Sie unter [Standortbedingungen beim bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-locations.md).


## <a name="client-apps"></a>Client-Apps

Mit der Bedingung für Client-Apps können Sie eine Richtlinie auf unterschiedliche Arten von Anwendungen anwenden, z.B.:

- Websites und Dienste
- Mobile Apps und Desktopanwendungen 

![Bedingungen](./media/active-directory-conditional-access-conditions/04.png)

Eine Anwendung wird wie folgt klassifiziert:

- Als Website oder Dienst, wenn die Web-SSO-Protokolle, SAML, WS-Fed oder OpenID Connect für einen vertraulichen Client verwendet werden.

- Als mobile App oder Desktopanwendung, wenn für einen nativen Client OpenID Connect für eine mobile App verwendet wird.

Eine vollständige Liste der Client-Apps, die Sie in der Richtlinie für bedingten Zugriff verwenden können, bietet Ihnen die [Technische Referenz zum bedingten Azure Active Directory-Zugriff](active-directory-conditional-access-technical-reference.md#client-apps-condition).

Häufige Anwendungsfälle für diese Bedingung sind Richtlinien, für die Folgendes gilt:

- Erzwingen der Nutzung eines [konformen Geräts](active-directory-conditional-access-policy-connected-applications.md) für mobile Apps und Desktopanwendungen, mit denen große Datenmengen auf das Gerät heruntergeladen werden, während der Browserzugriff mit jedem Gerät zulässig ist.

- Blockieren des Zugriffs über Webanwendungen und Zulassen des Zugriffs über mobile Apps und Desktopanwendungen.

Zusätzlich zur Verwendung von Web-SSO und modernen Authentifizierungsprotokollen können Sie diese Bedingung auch auf E-Mail-Anwendungen anwenden, für die Exchange ActiveSync verwendet wird, z.B. die nativen E-Mail-Apps auf den meisten Smartphones. Derzeit müssen Client-Apps, für die Legacyprotokolle genutzt werden, mit AD FS geschützt werden.

 Weitere Informationen finden Sie unter 

- [Einrichten von SharePoint Online und Exchange Online für bedingten Zugriff mit Azure Active Directory](active-directory-conditional-access-no-modern-authentication.md)
 
- [App-basierter bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-mam.md) 








## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 

