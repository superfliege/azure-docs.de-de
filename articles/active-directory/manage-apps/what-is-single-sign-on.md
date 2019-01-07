---
title: Einmaliges Anmelden bei Anwendungen – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie beim Konfigurieren von Anwendungen in Azure Active Directory (Azure AD) eine Methode für einmaliges Anmelden auswählen. Verwenden Sie einmaliges Anmelden, damit Benutzer sich nicht die Kennwörter für jede Anwendung merken müssen, und um die Kontoverwaltung zu vereinfachen.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: barbkess
ms.reviewer: arvindh
ms.openlocfilehash: 627bd114504de20517abcd05f45dc6c58fe80117
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322703"
---
# <a name="single-sign-on-to-applications-in-azure-active-directory"></a>Einmaliges Anmelden bei Anwendungen in Azure Active Directory
Erfahren Sie, wie Sie beim Konfigurieren von Anwendungen in Azure Active Directory (Azure AD) die am besten geeignete Methode für einmaliges Anmelden auswählen. 

- **Mit einmaligem Anmelden** melden sich Benutzer einmal mit einem Konto an, um auf in die Domäne eingebundene Geräte, Unternehmensressourcen, SaaS-Anwendungen (Software-as-a-Service) und Webanwendungen zuzugreifen. Nach der Anmeldung kann der Benutzer Anwendungen aus dem Office 365-Portal oder dem Azure AD-Zugriffsbereich MyApps starten. Administratoren können die Verwaltung von Benutzerkonten zentralisieren und den Benutzerzugriff auf Anwendungen basierend auf der Gruppenmitgliedschaft automatisch hinzufügen oder entfernen. 

- **Ohne einmaliges Anmelden** müssen sich die Benutzer anwendungsspezifische Kennwörter merken und sich für jede Anwendung anmelden. IT-Mitarbeiter müssen Benutzerkonten für jede Anwendung erstellen und aktualisieren, z.B. Office 365, Box oder Salesforce. Benutzer müssen sich ihre Kennwörter merken und außerdem Zeit für die Anmeldung bei jeder Anwendung verbringen.

Dieser Artikel beschreibt die Methoden für einmaliges Anmelden und hilft beim Auswählen der besten Methode für Ihre Anwendungen.

## <a name="choosing-a-single-sign-on-method"></a>Auswählen einer Methode für einmaliges Anmelden

Es gibt mehrere Möglichkeiten, eine Anwendung für einmaliges Anmelden zu konfigurieren. Die Auswahl einer Methode für einmaliges Anmelden für eine Anwendung hängt davon ab, wie die Anwendung für die Authentifizierung konfiguriert ist. Alle Methoden für einmaliges Anmelden (mit Ausnahme von „Deaktiviert“) melden Benutzer automatisch ohne zweite Anmeldung bei Anwendungen an.  

- Cloudanwendungen können für einmaliges Anmelden die Methoden „SAML“, „Kennwort“, „Verknüpft“ oder „Deaktiviert“ verwenden. SAML ist die sicherste Methode für einmaliges Anmelden.
- Lokale Anwendungen können für einmaliges Anmelden die Methoden „Kennwort“, „Integrierte Windows-Authentifizierung“, „Header“, „Verknüpft“ oder „Deaktiviert“ verwenden. Die lokalen Optionen funktionieren, wenn die Anwendungen für den Anwendungsproxy konfiguriert wurden. 

Dieses Flussdiagramm erleichtert Ihnen die Entscheidung, welche Methode des einmaligen Anmeldens für Ihre Situation am besten geeignet ist. 

![Auswählen einer Methode für einmaliges Anmelden](./media/what-is-single-sign-on/choose-single-sign-on-method.png)

In der folgenden Tabelle werden die Methoden für einmaliges Anmelden zusammengefasst und Links zu weiteren Details angegeben. 

| Methode für einmaliges Anmelden | Anwendungstypen | Einsatzgebiete |
| :------ | :------- | :----- |
| [SAML](#saml-sso) | Nur Cloud | Verwenden Sie nach Möglichkeit SAML. SAML funktioniert, wenn Apps für die Verwendung eines SAML-Protokolls konfiguriert wurden.|
| [Kennwortbasiert](#password-based-sso) | Cloud und lokal | Verwenden Sie diese Methode, wenn die Anwendung die Authentifizierung mit Benutzername und Kennwort vornimmt. Das kennwortbasierte einmalige Anmelden ermöglicht die sichere Speicherung des Anwendungskennworts und dessen Wiedergabe mit einer Webbrowsererweiterung oder einer mobilen App. Mit dieser Methode wird der von der Anwendung bereitgestellte vorhandene Anmeldevorgang genutzt, die Kennwortverwaltung kann jedoch der Administrator übernehmen. |
| [Verknüpft](#linked-sso) | Cloud und lokal | Verwenden Sie das verknüpfte einmalige Anmelden, wenn die Anwendung für einmaliges Anmelden bei einem anderen Identitätsanbieterdienst konfiguriert ist. Mit dieser Option wird der Anwendung kein einmaliges Anmelden hinzugefügt. Die Anwendung kann das einmalige Anmelden jedoch möglicherweise bereits über einen anderen Dienst implementiert haben, z.B. Active Directory-Verbunddienste.|
| [Disabled](#disabled-sso) | Cloud und lokal | Verwenden Sie das deaktivierte einmalige Anmelden, wenn die App nicht für einmaliges Anmelden konfiguriert werden kann. Benutzer müssen bei jedem Start dieser Anwendung ihren Benutzernamen und ihr Kennwort eingeben.|
| [Integrierte Windows-Authentifizierung (IWA)](#integrated-windows-authentication-iwa-sso) | Nur lokal | Verwenden Sie diese Methode für einmaliges Anmelden für Anwendungen, die die [integrierte Windows-Authentifizierung (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication) verwenden oder Ansprüche unterstützen. Die Anwendungsproxyconnectors verwenden die eingeschränkte Kerberos-Delegierung (Kerberos Constrained Delegation, KCD), um Benutzer bei der Anwendung zu authentifizieren. | 
| [Headerbasiert](#header-based-sso) | Nur lokal | Verwenden Sie das headerbasierte einmalige Anmelden, wenn die Anwendung für die Authentifizierung Header verwendet. Headerbasiertes einmaliges Anmelden erfordert PingAccess für Azure Active Directory. Der Anwendungsproxy verwendet Azure AD, um den Benutzer zu authentifizieren, und leitet Datenverkehr dann über den Connectordienst weiter.  | 

## <a name="saml-sso"></a>SAML SSO
Mit **SAML SSO** nimmt Azure AD die Authentifizierung bei der Anwendung mithilfe des Azure AD-Kontos des Benutzers vor. Azure AD gibt die Informationen für das einmalige Anmelden über ein Verbindungsprotokoll an die Anwendung weiter. Mit SAML-basiertem einmaligem Anmelden können Sie Benutzer basierend auf Regeln, die Sie in Ihren SAML-Ansprüchen definieren, bestimmten Anwendungsrollen zuordnen.

Für SAML-basiertes einmaliges Anmelden gilt Folgendes:

- Sicherer als kennwortbasiertes einmaliges Anmelden und alle anderen Anmeldemethoden
- Die empfohlene Methode für einmaliges Anmelden

SAML-basiertes einmaliges Anmelden wird für Anwendungen unterstützt, die eines dieser Protokolle verwenden:

- SAML 2.0
- WS-Federation-

Informationen zum Konfigurieren einer Anwendung für SAML-basiertes einmaliges Anmelden finden Sie unter [Konfigurieren des SAML-basierten einmaligen Anmeldens](configure-single-sign-on-portal.md). Darüber hinaus gibt es für viele Anwendungen [anwendungsspezifische Tutorials](../saas-apps/tutorial-list.md), die Sie durch die Konfiguration des SAML-basierten einmaligen Anmeldens für bestimmte Anwendungen führen. 

Weitere Informationen zur Funktionsweise des SAML-Protokolls finden Sie unter [SAML-Protokoll für einmaliges Anmelden](../develop/single-sign-on-saml-protocol.md).

## <a name="password-based-sso"></a>Kennwortbasiertes einmaliges Anmelden
Bei der kennwortbasierten Anmeldung melden sich Endbenutzer mit einem Benutzernamen und einem Kennwort bei der Anwendung an, wenn sie erstmals auf sie zugreifen. Nach der ersten Anmeldung werden der Benutzername und das Kennwort von Azure Active Directory für die Anwendung bereitgestellt. 

Beim kennwortbasierten einmaligen Anmelden wird der vorhandene Authentifizierungsvorgang der Anwendung verwendet. Wenn Sie einmaliges Anmelden per Kennwort für eine Anwendung aktivieren, sammelt Azure AD Benutzernamen und Kennwörter für die Anwendung und speichert diese sicher. Anmeldeinformationen des Benutzers werden in einem verschlüsselten Zustand im Verzeichnis gespeichert. 

Verwenden Sie das kennwortbasierte einmalige Anmelden in folgenden Fällen:

- Eine Anwendung kann das SAML-SSO-Protokoll nicht unterstützen.
- Eine Anwendung authentifiziert sich mit Benutzername und Kennwort anstelle von Zugriffstoken und Headern.

Das kennwortbasierte einmalige Anmelden wird für jede cloudbasierte Anwendung unterstützt, die über eine HTML-basierte Anmeldeseite verfügt. Der Benutzer kann jeden der folgenden Browser verwenden:

- Internet Explorer 11 unter Windows 7 oder höher
- Microsoft Edge in Windows 10 Anniversary Edition oder höher 
- Chrome unter Windows 7 oder höher und macOS X oder höher
- Firefox 26.0 oder höher unter Windows XP SP2 oder höher und Mac OS X 10.6 oder höher

Informationen zum Konfigurieren einer Cloudanwendung für kennwortbasiertes einmaliges Anmelden finden Sie unter [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](application-sign-in-problem-password-sso-gallery.md#configure-the-application-for-password-single-sign-on).

Informationen zum Konfigurieren einer lokalen Anwendung für einmaliges Anmelden über den Anwendungsproxy finden Sie unter [Kennworttresore (Password Vaulting) für einmaliges Anmelden mit Anwendungsproxy](application-proxy-configure-single-sign-on-password-vaulting.md).

### <a name="managing-credentials-for-password-based-sso"></a>Verwalten von Anmeldeinformationen für kennwortbasiertes einmaliges Anmelden

Um einen Benutzer bei einer Anwendung zu authentifizieren, ruft Azure AD die Anmeldeinformationen des Benutzers aus dem Verzeichnis ab und gibt sie auf der Anmeldeseite der Anwendung ein.  Azure AD übermittelt die Anmeldeinformationen des Benutzers sicher über eine Webbrowsererweiterung oder eine mobile App. Durch diesen Vorgang können Administratoren Anmeldeinformationen von Benutzern verwalten, ohne dass Benutzer sich ihr Kennwort merken müssen.

> [!IMPORTANT]
> Die Anmeldeinformationen werden während des automatisierten Anmeldevorgangs vor dem Endbenutzer verborgen. Die Anmeldeinformationen können jedoch mithilfe von Webdebugtools ermittelt werden. Benutzer und Administratoren müssen die gleichen Sicherheitsrichtlinien befolgen, als wären die Anmeldeinformationen direkt vom Benutzer eingegeben worden.

Die Kennwörter für die einzelnen Anwendungen können vom Azure AD-Administrator oder von den Benutzern verwaltet werden.

Wenn der Azure AD-Administrator die Anmeldeinformationen verwaltet, gilt Folgendes:  

- Der Benutzer muss seinen Benutzernamen und sein Kennwort nicht zurücksetzen oder sie sich merken. Der Benutzer kann durch Klicken auf die Anwendung in seinem Zugriffsbereich oder über einen bereitgestellten Link auf diese zugreifen.
- Der Administrator kann Verwaltungsaufgaben für die Anmeldeinformationen ausführen. Beispielsweise kann der Administrator den Anwendungszugriff entsprechend Gruppenmitgliedschaften und dem Mitarbeiterstatus des Benutzers aktualisieren.
- Der Administrator kann mithilfe von administrativen Anmeldeinformationen Zugriff auf Anwendungen bereitstellen, die von vielen Benutzern gemeinsam verwendet werden. Beispielsweise kann der Administrator jeder Person mit Zugriff auf eine Anwendung den Zugriff auf eine Anwendung für sozialen Medien oder zum Freigeben von Dokumenten gewähren.

Wenn der Endbenutzer die Anmeldeinformationen verwaltet, gilt Folgendes:

- Benutzer können ihre Kennwörter verwalten, indem Sie sie nach Bedarf aktualisieren oder löschen. 
- Administratoren können weiterhin neue Anmeldeinformationen für die Anwendung festlegen.


## <a name="linked-sso"></a>Verknüpftes einmaliges Anmelden
Über das verknüpfte einmalige Anmelden kann Azure AD einmaliges Anmelden bei einer Anwendung, die bereits in einem anderen Dienst für einmaliges Anmelden konfiguriert ist, bereitstellen. Die verknüpfte Anwendung kann Endbenutzern im Office 365-Portal oder im Azure AD-MyApps-Portal angezeigt werden. Beispielsweise kann ein Benutzer eine Anwendung, die für einmaliges Anmelden in Active Directory-Verbunddienste 2.0 (AD FS) konfiguriert ist, über das Office 365-Portal starten. Für verknüpfte Anwendungen, die über das Office 365-Portal oder das Azure AD-MyApps-Portal gestartet werden, steht außerdem eine zusätzliche Berichterstellung zur Verfügung. 

Verwenden Sie verknüpftes einmaliges Anmelden für Folgendes:

- Bereitstellen einer konsistenten Benutzererfahrung während der Migration von Anwendungen über einen längeren Zeitraum. Wenn Sie Anwendungen in Azure Active Directory migrieren, können Sie mithilfe des verknüpften einmaligen Anmeldens schnell Links für alle Anwendungen veröffentlichen, die Sie migrieren möchten.  Benutzer können alle Links im [MyApps-Portal](../user-help/active-directory-saas-access-panel-introduction.md) und im [Office 365-Anwendungsstarter](https://support.office.com/article/meet-the-office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) finden. Benutzer erkennen nicht, dass sie auf eine verknüpfte Anwendung bzw. eine migrierte Anwendung zugreifen.  

Nachdem ein Benutzer sich mit einer verknüpften Anwendung authentifiziert hat, muss ein Kontodatensatz erstellt werden, bevor der Endbenutzer Zugriff durch das einmalige Anmelden erhält. Die Bereitstellung dieses Kontodatensatzes kann automatisch oder manuell durch einen Administrator erfolgen.

## <a name="disabled-sso"></a>Deaktiviertes einmaliges Anmelden

Der deaktivierte Modus bedeutet, dass einmaliges Anmelden nicht für die Anwendung verwendet wird. Wenn einmaliges Anmelden deaktiviert ist, müssen sich Benutzer möglicherweise zweimal authentifizieren. Beim ersten Mal authentifizieren die Benutzer sich in Azure AD, danach melden sie sich bei der Anwendung an. 

Verwenden Sie den deaktivierten Modus für das einmalige Anmelden in folgenden Fällen:

- Wenn Sie noch nicht bereit sind, diese Anwendung in das einmalige Anmelden mit Azure AD-SSO zu integrieren
- Wenn Sie andere Aspekte der Anwendung testen
- Als Sicherheitsebene für eine lokale Anwendung, die keine Authentifizierung von Benutzern erfordert. Im deaktivierten Modus muss der Benutzer sich authentifizieren. 

## <a name="integrated-windows-authentication-iwa-sso"></a>Einmaliges Anmelden mit der integrierten Windows-Authentifizierung (IWA)

Der Azure AD-Anwendungsproxy ermöglicht einmaliges Anmelden (SSO) für Anwendungen, die die [integrierte Windows-Authentifizierung (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication) verwenden oder Ansprüche unterstützen. Wenn Ihre Anwendung die integrierte Windows-Authentifizierung verwendet, authentifiziert sich der Anwendungsproxy mithilfe der eingeschränkten Kerberos-Delegierung (Kerberos Constrained Delegation, KCD) bei der Anwendung. Bei einer Anwendung, die Ansprüche unterstützt und Azure Active Directory vertraut, funktioniert das einmalige Anmelden, da der Benutzer bereits über Azure AD authentifiziert wurde.

Verwenden des einmaligen Anmeldens im Modus der integrierten Windows-Authentifizierung für Folgendes:

- Zum Bereitstellen des einmaligen Anmeldens für eine lokale App, bei der die Authentifizierung über IWA erfolgt. 

Informationen zum Konfigurieren einer lokalen App für IWA finden Sie unter [Eingeschränkte Kerberos-Delegierung für einmaliges Anmelden bei Ihren Anwendungen mit dem Anwendungsproxy](application-proxy-configure-single-sign-on-with-kcd.md). 

### <a name="how-single-sign-on-with-kcd-works"></a>So funktioniert das einmalige Anmelden mit KCD
Dieses Diagramm erläutert die Vorgänge, die bei einem Zugriff eines Benutzers auf eine lokale Anwendung, die IWA verwendet, ablaufen.

![Flussdiagramm für Microsoft AAD-Authentifizierung](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. Der Benutzer gibt die URL ein, um über den Anwendungsproxy auf die lokale Anwendung zuzugreifen.
2. Der Anwendungsproxy leitet die Anforderung zur Vorauthentifizierung an Azure AD-Authentifizierungsdienste weiter. Zu diesem Zeitpunkt wendet Azure AD alle gültigen Authentifizierungs- und Autorisierungsrichtlinien an, wie z. B. mehrstufige Authentifizierung. Nachdem der Benutzer überprüft wurde, erstellt Azure AD ein Token und sendet es an den Benutzer.
3. Der Benutzer übergibt das Token an den Anwendungsproxy.
4. Der Anwendungsproxy überprüft das Token und ruft den Benutzerprinzipalnamen aus dem Token ab. Dann sendet er die Anforderung, den UPN und den Dienstprinzipalnamen über einen zweifach authentifizierten sicheren Kanal an den Connector.
5. Der Connector führt eine KCD-Aushandlung mit dem lokalen AD aus und nimmt dabei die Identität des Benutzers an, um ein Kerberos-Token für die Anwendung zu erhalten.
6. Active Directory sendet das Kerberos-Token für die Anwendung an den Connector.
7. Der Connector sendet die ursprüngliche Anforderung an den Anwendungsserver und verwendet dabei das von AD empfangene Kerberos-Token.
8. Die Anwendung sendet die Antwort an den Connector, die dann an den Anwendungsproxydienst und schließlich an den Benutzer zurückgegeben wird.

## <a name="header-based-sso"></a>Headerbasiertes einmaliges Anmelden

Das headerbasierte einmalige Anmelden funktioniert für Anwendungen, die für die Authentifizierung HTTP-Header verwenden. Diese Anmeldemethode verwendet einen Drittanbieter-Authentifizierungsdienst mit dem Namen PingAccess. Ein Benutzer muss sich nur bei Azure AD authentifizieren. 

Verwenden Sie das headerbasierte einmalige Anmelden in folgenden Fällen:

- Anwendungsproxy und PingAccess sind für die Anwendung konfiguriert.

Informationen zum Konfigurieren der headerbasierten Authentifizierung finden Sie unter [Headerbasierte Authentifizierung für einmaliges Anmelden mit Anwendungsproxy und PingAccess](application-proxy-configure-single-sign-on-with-ping-access.md). 

### <a name="what-is-pingaccess-for-azure-ad"></a>Was ist PingAccess für Azure AD?

Mit PingAccess für Azure AD können Benutzer auf Anwendungen zugreifen, die Header für die Authentifizierung verwenden, und sich bei diesen einmalig anmelden. Der Anwendungproxy behandelt diese Anwendungen wie alle anderen und verwendet Azure AD zum Authentifizieren des Zugriffs und zum Leiten des Datenverkehrs durch den Connectordienst. Nach der Authentifizierung übersetzt der PingAccess-Dienst das Azure AD-Zugriffstoken in ein Headerformat, das an die Anwendung gesendet wird.

Ihre Benutzer bemerken keinen Unterschied, wenn sie sich für die Nutzung Ihrer Unternehmensanwendungen anmelden. Sie können weiterhin überall und auf beliebigen Geräten arbeiten. Die Anwendungsproxyconnectors leiten Remotedatenverkehr an alle Apps weiter und sorgen auch weiterhin für automatischen Lastenausgleich.

### <a name="how-do-i-get-a-license-for-pingaccess"></a>Wie erhalte ich eine Lizenz für PingAccess?

Da dieses Szenario im Rahmen einer Partnerschaft von Azure Active Directory und PingAccess angeboten wird, benötigen Sie Lizenzen für beide Dienste. Azure Active Directory Premium-Abonnements enthalten aber eine grundlegende PingAccess-Lizenz, die bis zu 20 Anwendungen abdeckt. Wenn Sie mehr als 20 headerbasierte Anwendungen veröffentlichen müssen, können Sie bei PingAccess eine weitere Lizenz erhalten. 

Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](../fundamentals/active-directory-whatis.md).

## <a name="related-articles"></a>Verwandte Artikel
* [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](../saas-apps/tutorial-list.md)
* [Tutorial zum Konfigurieren des einmaligen Anmeldens](configure-single-sign-on-portal.md)
* [Einführung in die Verwaltung des Zugriffs auf Anwendungen](what-is-access-management.md)
* Downloadlink: [Single sign-on deployment plan](https://aka.ms/SSODeploymentPlan) (Bereitstellungsplan für einmaliges Anmelden)


