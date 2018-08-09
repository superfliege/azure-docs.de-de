---
title: Was sind Bedingungen beim bedingten Zugriff in Azure Active Directory? | Microsoft-Dokumentation
description: Es wird beschrieben, wie Bedingungen beim bedingten Zugriff in Azure Active Directory zum Auslösen einer Richtlinie verwendet werden.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 5f5e2051f9c67fa4e37ce0e1213e14e197222f05
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627541"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Was sind Bedingungen beim bedingten Zugriff in Azure Active Directory? 

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal) können Sie den Zugriff von autorisierten Benutzern auf Ihre Cloud-Apps steuern. Mit einer Richtlinie für bedingten Zugriff definieren Sie die Reaktion für die Ursache der Richtlinienauslösung. Ein Beispiel für eine Reaktion ist **Then do this**. Ein Beispiel für eine Ursache ist **When this happens**.

![Ursache und Reaktion](./media/conditions/10.png)


Im Kontext des bedingten Zugriffs ist **When this happens** eine **Bedingung**. **Then do this** ist eine **Zugriffssteuerung**. Die Kombination aus Ihren Bedingungen und Zugriffssteuerungen ergibt eine Richtlinie für bedingten Zugriff.

![Richtlinie für bedingten Zugriff](./media/conditions/61.png)


Bedingungen, die Sie nicht in einer Richtlinie für bedingten Zugriff konfigurieren, werden nicht angewendet. Einige Bedingungen sind [obligatorisch](best-practices.md) für die Anwendung einer Richtlinie für bedingten Zugriff auf eine Umgebung. 

Dieser Artikel bietet eine Übersicht über die Bedingungen und ihre Verwendung in einer Richtlinie für bedingten Zugriff. 

## <a name="users-and-groups"></a>Benutzer und Gruppen

Die Bedingung für Benutzer und Gruppen ist in einer Richtlinie für bedingten Zugriff obligatorisch. In Ihrer Richtlinie können Sie entweder die Option **Alle Benutzer** oder bestimmte Benutzer und Gruppen auswählen.

![Benutzer und Gruppen](./media/conditions/111.png)

Wenn Sie **Alle Benutzer** auswählen, wird Ihre Richtlinie auf alle Benutzer (einschließlich Gastbenutzern) im Verzeichnis angewendet.

Wenn Sie **Benutzer und Gruppen auswählen** auswählen, können Sie die folgenden Optionen festlegen:

* Mit **Alle Gastbenutzer** können Sie eine Richtlinie auf B2B-Gastbenutzer anwenden. Diese Bedingung gilt für jedes Benutzerkonto, dessen Attribut **userType** **guest** entspricht. Sie können diese Einstellung verwenden, damit eine Richtlinie angewendet wird, sobald das Konto in einem Einladungsfluss in Azure AD erstellt wurde.

* Mit **Verzeichnisrollen** können Sie eine Richtlinie basierend auf der zugewiesenen Benutzerrolle anwenden. Diese Bedingung unterstützt Verzeichnisrollen wie **Globaler Administrator** oder **Kennwortadministrator**.

* Mit **Benutzer und Gruppen** können Sie bestimmte Benutzergruppen adressieren. Beispielsweise können Sie eine Gruppe auswählen, die alle Mitglieder der HR-Abteilung enthält, wenn eine HR-App als Cloud-App ausgewählt wurde. Als Gruppe gilt eine beliebige Gruppe in Azure AD, einschließlich dynamischer oder zugewiesener Sicherheits- und Verteilungsgruppen.

Sie können bestimmte Benutzer und Gruppen auch aus einer Richtlinie ausschließen. Ein häufiger Anwendungsfall sind Dienstkonten, falls bei Ihrer Richtlinie die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) erzwungen wird. 

Die Ausrichtung auf bestimmte Gruppen von Benutzern ist für die Bereitstellung einer neuen Richtlinie nützlich. Bei einer neuen Richtlinie sollten Sie nur eine Ausrichtung auf eine Anfangsgruppe von Benutzern festlegen, um das Verhalten der Richtlinie zu überprüfen. 



## <a name="cloud-apps"></a>Cloud-Apps 

Eine Cloud-App ist eine Website oder ein Dienst. Websites, die durch den Azure AD-Anwendungsproxy geschützt sind, sind ebenfalls Cloud-Apps. Eine ausführliche Beschreibung der unterstützten Cloud-Apps finden Sie unter [Zuweisungen von Cloud-Apps](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference#cloud-apps-assignments). 

Die Bedingung für **Cloud-Apps** ist in einer Richtlinie für bedingten Zugriff obligatorisch. In Ihrer Richtlinie können Sie die Option **Alle Cloud-Apps** oder bestimmte Apps wählen.

![Einschließen von Cloud-Apps](./media/conditions/03.png)

- Wählen Sie **Alle Cloud-Apps** als Basisoption für Richtlinien aus, die auf die gesamte Organisation angewendet werden sollen. Verwenden Sie diese Auswahl für Richtlinien, die eine mehrstufige Authentifizierung erfordern, wenn für eine Cloud-App ein Anmelderisiko erkannt wird. Eine auf **Alle Cloud-Apps** angewendete Richtlinie gilt für den Zugriff auf alle Websites und Dienste. Diese Einstellung beschränkt sich nicht auf die Cloud-Apps in der Liste **Cloud-Apps auswählen**. 

- Wählen Sie einzelne Cloud-Apps aus, um bestimmte Dienste nach Richtlinien auszurichten. Sie können beispielsweise vorgeben, dass Benutzer über ein [konformes Gerät](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) verfügen müssen, um auf SharePoint Online zuzugreifen. Diese Richtlinie wird auch auf andere Dienste angewendet, wenn diese auf SharePoint-Inhalte zugreifen. Ein Beispiel ist Microsoft Teams. 

Sie können bestimmte Apps von einer Richtlinie ausschließen. Diese Apps unterliegen jedoch weiterhin den Richtlinien, die für die Dienste gelten, auf die sie zugreifen. 



## <a name="sign-in-risk"></a>Anmelderisiko

Ein Anmelderisiko ist ein Indikator für die hohe, mittlere oder geringe Wahrscheinlichkeit, dass ein Anmeldeversuch nicht vom rechtmäßigen Besitzer eines Benutzerkontos durchgeführt wurde. Azure AD berechnet die Stufe des Anmelderisikos bei der Benutzeranmeldung. Die berechnete Anmelderisikostufe kann als Bedingung in einer Richtlinie für bedingten Zugriff dienen. 

![Anmelderisikostufen](./media/conditions/22.png)

Für die Nutzung dieser Bedingung muss [Azure Active Directory Identity Protection](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-identityprotection-enable) aktiviert sein.
 
Häufige Anwendungsfälle für diese Bedingung sind Richtlinien, für die folgende Schutzmaßnahmen gelten: 

- Blockieren von Benutzern mit einem hohen Anmelderisiko. Diese Schutzmaßnahme verhindert, dass potenziell unrechtmäßige Benutzer auf Ihre Cloud-Apps zugreifen. 
- Erzwingen der mehrstufigen Authentifizierung für Benutzer mit einem mittleren Anmelderisiko. Durch das Erzwingen der mehrstufigen Authentifizierung erhalten Sie zusätzliche Sicherheit, dass die Anmeldung vom rechtmäßigen Besitzer eines Kontos durchgeführt wird.

Weitere Informationen finden Sie unter [Riskante Anmeldungen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-security-risky-sign-ins).  

## <a name="device-platforms"></a>Geräteplattformen

Die Geräteplattform ist durch das Betriebssystem gekennzeichnet, das auf dem Gerät ausgeführt wird. Azure AD identifiziert die Plattform, indem vom Gerät, z.B. einem Benutzer-Agent, bereitgestellte Informationen verwendet werden. Diese Informationen wurden nicht überprüft. Es wird empfohlen, auf alle Plattformen eine Richtlinie anzuwenden. Die Richtlinie sollte den Zugriff blockieren, die Einhaltung der Microsoft Intune-Richtlinien erfordern oder festlegen, dass das Gerät in die Domäne eingebunden ist. Standardmäßig wird eine Richtlinie auf alle Geräteplattformen angewendet. 


![Konfigurieren von Geräteplattformen](./media/conditions/24.png)

Eine Liste der unterstützten Geräteplattformen finden Sie unter [Geräteplattformbedingung](technical-reference.md#device-platform-condition).


Ein häufiger Anwendungsfall für diese Bedingung ist eine Richtlinie, mit der der Zugriff auf Ihre Cloud-Apps auf [verwaltete Geräte](require-managed-devices.md) beschränkt wird. Informationen zu weiteren Szenarien, z.B. zur Geräteplattformbedingung, finden Sie unter [App-basierter bedingter Zugriff mit Azure Active Directory](app-based-conditional-access.md).



## <a name="device-state"></a>Gerätestatus

Die Gerätestatusbedingung schließt in Hybrid-Azure AD eingebundene und als konform gekennzeichnete Geräte von einer Richtlinie für bedingten Zugriff aus. Diese Bedingung ist hilfreich, wenn eine Richtlinie nur auf ein nicht verwaltetes Gerät angewendet werden soll, um zusätzliche Sitzungssicherheit zu bieten. Erzwingen Sie z.B. nur dann die Microsoft Cloud App Security-Sitzungssteuerung, wenn ein Gerät nicht verwaltet wird. 


![Konfigurieren des Gerätstatus](./media/conditions/112.png)

Wenn Sie den Zugriff für nicht verwaltete Geräte blockieren möchten, implementieren Sie den [gerätebasierten bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online).


## <a name="locations"></a>Standorte

Mit Standorten können Sie Bedingungen definieren, die darauf basieren, wo ein Verbindungsversuch stattgefunden hat. 

![Konfigurieren von Standorten](./media/conditions/25.png)

Häufige Anwendungsfälle für diese Bedingung sind Richtlinien mit den folgenden Schutzmaßnahmen:

- Erzwingen der mehrstufigen Authentifizierung für Benutzer beim Zugriff auf einen Dienst, wenn sie sich außerhalb des Unternehmensnetzwerks befinden  

- Blockieren des Zugriffs auf einen Dienst für Benutzer, die sich in bestimmten Ländern oder Regionen aufhalten 

Weitere Informationen finden Sie unter [Was sind Standortbedingungen beim bedingten Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).


## <a name="client-apps"></a>Client-Apps

Durch das Verwenden der Bedingung für Client-Apps können Sie eine Richtlinie auf unterschiedliche Arten von Anwendungen anwenden. Beispiele sind Websites, Dienste, mobile Apps und Desktopanwendungen. 



Eine Anwendung wird wie folgt klassifiziert:

- Als Website oder Dienst, wenn Web-SSO-Protokolle, SAML, WS-Fed oder OpenID Connect für einen vertraulichen Client verwendet werden.

- Als mobile App oder Desktopanwendung, wenn für einen nativen Client OpenID Connect für eine mobile App verwendet wird.

Eine vollständige Liste der Client-Apps, die Sie in der Richtlinie für bedingten Zugriff verwenden können, finden Sie in der technischen Referenz zum bedingten Zugriff in Azure Active Directory unter [Client-Apps-Bedingung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#client-apps-condition).

Häufige Anwendungsfälle für diese Bedingung sind Richtlinien mit den folgenden Schutzmaßnahmen: 

- Erzwingen der Nutzung eines [konformen Geräts](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) für mobile Apps und Desktopanwendungen, mit denen große Datenmengen auf das Gerät heruntergeladen werden. Der Browserzugriff ist dabei mit jedem Gerät zulässig.

- Blockieren des Zugriffs über Webanwendungen und Zulassen des Zugriffs über mobile Apps und Desktopanwendungen.

Sie können diese Bedingung auf Web-SSO und moderne Authentifizierungsprotokolle anwenden. Außerdem können Sie sie auch auf E-Mail-Apps anwenden, die Microsoft Exchange ActiveSync verwenden. Ein Beispiel sind native E-Mail-Apps auf den meisten Smartphones. 

Sie können die Client-Apps-Bedingung nur auswählen, wenn Microsoft Office 365 Exchange Online die einzige ausgewählte Cloud-App ist.

![Cloud-Apps](./media/conditions/32.png)

Die Auswahl von **Exchange ActiveSync** als Client-App-Bedingung wird nur unterstützt, wenn Sie keine anderen Bedingungen in einer Richtlinie konfiguriert haben. Sie können diese Bedingung aber so eingrenzen, dass sie nur für unterstützte Plattformen gilt.

 
![Anwendend er Richtlinie nur auf unterstützte Plattformen](./media/conditions/33.png)

Das ausschließliche Anwenden dieser Bedingung auf unterstützte Plattformen entspricht der Verwendung von allen Geräteplattformen bei einer [Geräteplattformbedingung](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online).

![Konfigurieren von Geräteplattformen](./media/conditions/34.png)


 Weitere Informationen und Beispiele finden Sie in diesen Artikeln:

- [Einrichten von SharePoint Online und Exchange Online für bedingten Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication)
 
- [App-basierter bedingter Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) 


### <a name="legacy-authentication"></a>Legacyauthentifizierung  

Der bedingte Zugriff gilt nun auch für ältere Microsoft Office-Clients, die keine moderne Authentifizierung unterstützen. Dies gilt auch für Clients, die E-Mail-Protokolle wie POP, IMAP und SMTP verwenden. Durch die Verwendung von Legacyauthentifizierung können Sie Richtlinien wie **Zugriff über andere Clients blockieren** konfigurieren.


![Konfigurieren von Client-Apps](./media/conditions/160.png)  


#### <a name="known-issues"></a>Bekannte Probleme

- Durch das Konfigurieren einer Richtlinie für **Andere Clients** wird die gesamte Organisation für bestimmte Clients blockiert, z.B. SPConnect. Dies tritt ein, weil sich ältere Clients auf unerwartete Weise authentifizieren. Dieses Problem gilt nicht für Office-Hauptanwendungen wie ältere Office-Clients. 

- Es kann bis zu 24 Stunden dauern, bis die Richtlinie wirksam wird. 


#### <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**F:** Blockiert diese Authentifizierung die Microsoft Exchange-Webdienste?

Dies hängt vom Authentifizierungsprotokoll ab, das die Exchange-Webdienste verwenden. Wenn die Anwendung der Exchange-Webdienste eine moderne Authentifizierung nutzt, wird dies von der Client-App **Mobile Apps und Desktopclients** abgedeckt. Die Standardauthentifizierung wird durch die Client-App **Andere Clients** abgedeckt.


**F:** Welche Steuerelemente kann ich für **Andere Clients** verwenden?

Für **Andere Clients** können alle Steuerelemente konfiguriert werden. Allerdings wird in der Endbenutzerumgebung der Zugriff für alle Fälle blockiert. **Andere Clients** unterstützt nicht Steuerelemente wie „MFA“, „Konformes Gerät“, „Domänenbeitritt“. 
 
**F:** Welche Bedingungen kann ich für **Andere Clients** verwenden?

Für **Andere Clients** können alle Bedingungen konfiguriert werden.

**F:** Unterstützt Exchange ActiveSync alle Bedingungen und Steuerelemente?

Nein. Die folgende Liste enthält weitere Informationen zur Exchange ActiveSync-Unterstützung: 

- Exchange ActiveSync unterstützt nur Zielgruppenadressierung für Benutzer und Gruppen. Gäste und Rollen werden nicht unterstützt. Wenn eine Gast- oder Rollenbedingung konfiguriert wird, werden alle Benutzer blockiert. Exchange ActiveSync blockiert alle Benutzer, weil das Protokoll nicht ermittelt kann, ob die Richtlinie für einen Benutzer gilt.

- Exchange ActiveSync funktioniert nur mit Microsoft Exchange Online als Cloud-App. 

- Exchange ActiveSync unterstützt keine Bedingungen, mit Ausnahme der Client-App selbst. 

- Exchange ActiveSync kann mit jedem Steuerelement konfiguriert werden. Alle Steuerelemente mit Ausnahme der Gerätecompliance werden blockiert.

**F:** Gelten die Richtlinien standardmäßig für alle zukünftigen Client-Apps?

Nein. Es ist gibt keine Änderung beim Standardverhalten von Richtlinien. Die Richtlinien gelten weiterhin standardmäßig für Browser, mobile Apps und Desktopclients.



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Konfigurieren einer Richtlinie für bedingten Zugriff finden Sie unter [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md).

- Wie Sie Richtlinien für bedingten Zugriff für Ihre Umgebung konfigurieren, erfahren Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](best-practices.md). 

