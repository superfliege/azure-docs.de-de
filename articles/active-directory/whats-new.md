---
title: Neuerungen Anmerkungen zur Version Azure Active Directory Premium | Microsoft-Dokumentation
description: "Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory (Azure AD), z.B. aktuelle Anmerkungen zu dieser Version, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 7cdeb0a996835f976e6c80e9831479a2ff917434
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="whats-new-in-azure-active-directory"></a>Neuerungen in Azure Active Directory


> Abonnieren Sie den [![RSS](./media/whats-new/feed-icon-16x16.png)](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us)-[Feed](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us), um in Bezug auf Neuerungen in Azure Active Directory (Azure AD) auf dem Laufenden zu bleiben.



Es werden fortlaufend Verbesserungen an Azure AD vorgenommen. Damit Sie bezüglich der neuesten Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

-   Neueste Versionen
-   Bekannte Probleme
-   Fehlerbehebungen
-   Veraltete Funktionen
-   Pläne für Änderungen

Besuchen Sie regelmäßig diese Seite. Diese wird monatlich aktualisiert.


## <a name="january-2018"></a>Januar 2018
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar 

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 

Im Januar 2018 wurden die folgenden neuen Apps mit Verbundunterstützung im App-Katalog hinzugefügt:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Privacy Management Software](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [Verbundverzeichnis IriusRisk](https://go.microsoft.com/fwlink/?linkid=864699) und [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Eine vollständige Übersicht über alle verfügbaren Tutorials finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).
 

---
 


### <a name="sign-in-with-additional-risk-detected"></a>Anmeldung mit erhöhtem Risiko erkannt

**Typ:** Neue Funktion  
**Dienstkategorie**: Identitätsschutz  
**Produktfunktion**: Identitätssicherheit und -schutz
 

Welche Informationen Sie zu einem erkannten Risikoereignis erhalten, richtet sich nach Ihrem Azure AD-Abonnement. Die Azure AD Premium P2-Edition bietet die ausführlichsten Informationen zu allen zugrunde liegenden erkannten Ereignissen.

Mit der Azure AD Premium P1-Edition werden erkannte Ereignisse, die nicht durch Ihre Lizenz abgedeckt sind, als das Risikoereignis „Anmeldung mit erhöhtem Risiko erkannt“ angezeigt.

Weitere Informationen finden Sie unter [Azure Active Directory-Risikoereignisse](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 

---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Ausblenden von Office 365-Anwendungen in Zugriffsbereichen von Endbenutzern

**Typ:** Neue Funktion  
**Dienstkategorie**: Meine Apps  
**Produktfunktion**: SSO
 

Durch eine neue Benutzereinstellung können Sie jetzt besser steuern, wie Office 365-Anwendungen in den Zugriffsbereichen Ihrer Benutzer angezeigt werden. Diese Option ist hilfreich, um die Anzahl der Apps in den Zugriffsbereichen eines Benutzers zu reduzieren, sofern nur Office-Apps im Office-Portal anzeigen werden sollen. Die Einstellung befindet sich in den **Benutzereinstellungen** und lautet **Benutzer können Office 365-Apps nur im Office 365-Portal anzeigen**.
 

Weitere Informationen finden Sie unter [Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 


### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Nahtloses Anmelden bei Apps mit direkt über die App-URL aktiviertem Kennwort-SSO 

**Typ:** Neue Funktion  
**Dienstkategorie**: Meine Apps  
**Produktfunktion**: SSO
 

Die Browsererweiterung „Meine Apps“ ist jetzt über ein nützliches Tool verfügbar, mit dem Sie die Funktion zum einmaligen Anmelden „Meine Apps“ als Verknüpfung in Ihrem Browser verwenden können. Nach der Installation wird dem Benutzer ein Waffelsymbol im Browser angezeigt, das Schnellzugriff auf Apps bietet. Benutzer können ab sofort von folgenden Vorteilen profitieren:

- Die Möglichkeit, sich direkt bei Apps per Kennwort-SSO über die Anmeldeseite der App anzumelden
- Starten einer App mithilfe der Schnellsuchfunktion
- Verknüpfungen für zuletzt verwendete Apps über die Erweiterung
- Die Erweiterung ist für Edge, Chrome und Firefox verfügbar.
 
Weitere Informationen finden Sie unter [Erweiterung zur sicheren Anmeldung bei „Meine Apps“](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Die Azure AD-Administratoroberfläche im klassischen Azure-Portal wurde eingestellt.

**Typ**: Veraltet   
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Verzeichnis
 

Die Azure AD-Administratoroberfläche im klassischen Azure-Portal ist seit dem 8. Januar 2018 eingestellt. Diese Änderung fand im Zuge der Einstellung des klassischen Azure-Portals selbst statt. Für sämtliche portalbasierte Verwaltungsaufgaben von Azure AD sollten Sie zukünftig das [Azure AD Admin Center](https://aad.portal.azure.com) verwenden.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>Das PhoneFactor-Webportal wurde eingestellt

**Typ**: Veraltet  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Verzeichnis
 

Am 8. Januar 2018 wurde das PhoneFactor-Webportal eingestellt. Die Funktionen des Portals, nämlich die Verwaltung des MFA-Servers, wurden zum Azure-Portal unter portal.azure.com migriert. 

Die MFA-Konfiguration befindet sich unter **Azure Active Directory \> MFA-Server**.
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Einstellung von Azure AD-Berichten


**Typ**: Veraltet  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus  


Im Zuge der allgemeinen Verfügbarkeit der neuen Azure Active Directory-Verwaltungskonsole und der neuen APIs, die ab sofort für Aktivitäts- und Sicherheitsberichte verfügbar sind, wurden die Berichts-APIs auf dem Endpunkt unter „/reports“ am 31. Dezember 2017 eingestellt.


**Was ist verfügbar?**

Im Rahmen der Umstellung auf die neue Verwaltungskonsole haben wir zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung gestellt. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Die Daten, die bisher in den Sicherheitsberichten verfügbar waren, können jetzt über die API für Identity Protection-Risikoereignisse in Microsoft Graph abgerufen werden.

Weitere Informationen finden Sie unter 

- [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)


---


## <a name="december-2017"></a>Dezember 2017
 

### <a name="terms-of-use-in-the-access-panel"></a>Nutzungsbedingungen im Zugriffsbereich

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Governance/Compliance
 
Ab sofort können Sie die von Ihnen zuvor akzeptierten Nutzungsbedingungen im Zugriffsbereich einsehen.

Folgen Sie diesen Schritten:

1. Navigieren Sie zum [MyApps-Portal](https://myapps.microsoft.com), und melden Sie sich an.

2. Wählen Sie oben rechts Ihren Namen und dann in der Liste **Profil** aus. 

3. Klicken Sie in Ihrem **Profil** auf **Nutzungsbedingungen lesen**. 

4. Nun können Sie sich die akzeptierten Nutzungsbedingungen ansehen. 

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature (Vorschauversion) für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 

### <a name="new-azure-ad-sign-in-experience"></a>Neuer Anmeldevorgang für Azure AD

**Typ:** Neue Funktion  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Benutzerauthentifizierung
 
Die Benutzeroberfläche des Azure AD- und Microsoft-Kontoidentitätssystems wurden hinsichtlich eines konsistenten Aussehens und Verhaltens neu gestaltet. Darüber hinaus erfasst die Azure AD-Anmeldeseite zuerst den Benutzernamen, gefolgt von den Anmeldeinformationen auf einem zweiten Bildschirm.

Weitere Informationen finden Sie unter [The new Azure AD Signin Experience is now in Public Preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/) (Die neue Umgebung für die Azure AD-Anmeldung ist jetzt als Public Preview verfügbar).
 
---
 

### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Weniger Anmeldeeingabeaufforderungen: eine neue „Angemeldet bleiben“-Umgebung für die Azure AD-Anmeldung

**Typ:** Neue Funktion  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Benutzerauthentifizierung
 
Das Kontrollkästchen **Angemeldet bleiben** auf der Azure AD-Anmeldeseite wurde durch eine neue Eingabeaufforderung ersetzt, die nach erfolgreicher Authentifizierung angezeigt wird. 

Wenn Sie für diese Eingabeaufforderung **Ja** auswählen, erhalten Sie vom Dienst ein beständiges Aktualisierungstoken. Dies ist das gleiche Verhalten wie beim Aktivieren des Kontrollkästchens **Angemeldet bleiben** in der alten Umgebung. Bei Verbundmandanten wird diese Eingabeaufforderung angezeigt, nachdem Sie sich erfolgreich bei dem Verbunddienst authentifiziert haben.

Weitere Informationen finden Sie unter [Fewer login prompts: The new “Keep me signed in” experience for Azure AD is in preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) (Weniger Anmeldeeingabeaufforderungen: Die neue „Angemeldet bleiben“-Umgebung für Azure AD befindet sich in der Vorschauphase). 

---
 

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Hinzufügen einer Konfiguration, um vor dem Akzeptieren das Erweitern der Nutzungsbedingungen zu erfordern

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Governance/Compliance
 
Eine Option für Administratoren wurde hinzugefügt, mit der Benutzer aufgefordert werden können, die Nutzungsbedingungen vor dem Akzeptieren zu erweitern.

Wählen Sie **Ein** oder **Aus** aus, um die Erweiterung der Nutzungsbedingungen durch den Benutzer zu erfordern. Bei der Einstellung **Ein** wird vorausgesetzt, dass sich Benutzer diese vor dem Akzeptieren der Nutzungsbedingungen ansehen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature (Vorschauversion) für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Bereichsbezogene Aktivierung für berechtigte Rollenzuweisungen

**Typ:** Neue Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Mit der bereichsbezogenen Aktivierung können Sie berechtigte Rollenzuweisungen für Azure-Ressourcen mit weniger Autonomie aktivieren, als die ursprünglichen Standardwerte für die Zuweisung vorsehen. Beispielsweise wird Ihnen „Besitzer“ für ein Abonnement in Ihrem Mandanten zugewiesen. Mit der bereichsbezogenen Aktivierung können Sie die Besitzerrolle für bis zu fünf innerhalb des Abonnements enthaltene Ressourcen aktivieren (z.B. für Ressourcengruppen und virtuelle Computer). Durch Festlegen des Gültigkeitsbereichs der Aktivierung kann die Wahrscheinlichkeit verringert werden, dass unerwünschte Änderungen an wichtigen Azure-Ressourcen vorgenommen werden.

Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 

### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Im Dezember 2017 wurden die folgenden neuen Apps mit Verbundunterstützung im App-Katalog hinzugefügt:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD-Integration](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO for Bamboo by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO for Bitbucket by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD-Integration.

Eine vollständige Übersicht über alle verfügbaren Tutorials finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

 
---
 

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Genehmigungsworkflows für Azure AD-Verzeichnisrollen

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Ein Genehmigungsworkflow für Azure AD-Verzeichnisrollen ist allgemein verfügbar.

Mit dem Genehmigungsworkflow können Administratoren für privilegierte Rollen festlegen, dass berechtigte Rollenmitglieder die Rollenaktivierung anfordern müssen, bevor sie die privilegierte Rolle verwenden können. Es können Genehmigungsverantwortlichkeiten an verschiedene Benutzer und Gruppen delegiert werden. Berechtigte Rollenmitglieder erhalten eine Benachrichtigung, wenn die Genehmigung abgeschlossen und ihre Rolle aktiv ist.

---
 

### <a name="pass-through-authentication-skype-for-business-support"></a>Pass-Through-Authentifizierung: Unterstützung von Skype for Business

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung


Die Pass-Through-Authentifizierung unterstützt jetzt Benutzeranmeldungen bei Skype for Business-Clientanwendungen, die die moderne Authentifizierung, einschließlich Online- und Hybridtopologien, unterstützen. 

Weitere Informationen finden Sie unter [Mit moderner Authentifizierung unterstützte Skype for Business-Topologien](https://technet.microsoft.com/library/mt803262.aspx).
 
---
 

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Updates für die Azure RBAC in Azure AD Privileged Identity Management (Vorschauversion)

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Mit der aktualisierten Public Preview von Azure AD Privileged Identity Management (PIM) für die rollenbasierte Zugriffssteuerung in Azure (RBAC) haben Sie nun folgende Möglichkeiten:

* Nicht mehr Verwaltung als nötig
* Erfordern der Genehmigung zum Aktivieren von Ressourcenrollen
* Planen einer zukünftigen Rollenaktivierung, die eine Genehmigung für Azure AD- und Azure RBAC-Rollen erfordert

 
Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

 
---
 
## <a name="november-2017"></a>November 2017
 
### <a name="access-control-service-retirement"></a>Einstellung des Access Control Service



**Typ:** Plan für Änderung  
**Dienstkategorie**: Access Control Service  
**Produktfunktion**: Access Control Service 


 Der Azure Active Directory Access Control (auch als Access Control Service bezeichnet) wird Ende 2018 eingestellt. Weitere Informationen, u.a. ein ausführlicher Zeitplan und allgemeine Migrationsanweisungen, werden in den nächsten Wochen bereitgestellt. Auf dieser Seite können Sie uns Ihre Kommentare oder Fragen zum Access Control Service mitteilen. Ein Teammitglied wird diese dann beantworten.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Beschränkung des Browserzugriffs auf Intune Managed Browser 


**Typ:** Plan für Änderung  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz




Sie können den Browserzugriff auf Office 365 und weitere mit Azure AD verbundene Cloud-Apps mithilfe von Intune Managed Browser als genehmigte App einschränken. 

Ab sofort können Sie folgende Bedingung für den anwendungsbasierten bedingten Zugriff konfigurieren:

**Client-Apps:** Browser

**Welche Auswirkung hat die Änderung?**

Der Zugriff wird blockiert, wenn Sie diese Bedingung verwenden. Sobald die Vorschauversion verfügbar ist, ist für den gesamten Zugriff die Verwendung der Managed Browser-Anwendung erforderlich. 

Achten Sie in kommenden Blogbeiträgen und Anmerkungen zu dieser Version auf Informationen zu dieser Funktionalität. 

Weitere Informationen finden Sie unter [Bedingter Zugriff in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD

 
**Typ:** Plan für Änderung  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz




Die folgenden Apps sind zur Aufnahme in die Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) vorgesehen:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Weitere Informationen finden Sie unter 

- [Genehmigte Client-App als Voraussetzung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Unterstützung von Nutzungsbedingungen für mehrere Sprachen



**Typ:** Neue Funktion    
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Governance/Compliance





Administratoren können jetzt neue Nutzungsbedingungen erstellen, die mehrere PDF-Dokumente enthalten. Diese PDF-Dateien können mit einer zugehörigen Sprache gekennzeichnet werden. Benutzern wird dann die PDF-Datei angezeigt, die der Sprache in den Einstellungen entspricht. Falls keine Übereinstimmung vorliegt, wird die Standardsprache angezeigt.


---
 

### <a name="real-time-password-writeback-client-status"></a>Echtzeitclientstatus beim Kennwortrückschreiben



**Typ:** Neue Funktion  
**Dienstkategorie**: Self-Service-Kennwortzurücksetzung  
**Produktfunktion**: Benutzerauthentifizierung


 

Nun können Sie den Status Ihres lokalen Clients für das Kennwortrückschreiben anzeigen. Diese Option steht im Abschnitt **Lokale Integration** auf der Seite [Kennwortzurücksetzung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) zur Verfügung. 

Wenn Verbindungsprobleme bei Ihrem lokalen Client für das Kennwortrückschreiben vorliegen, wird eine Fehlermeldung mit folgenden Informationen angezeigt:

- Informationen dazu, warum keine Verbindung mit dem lokalen Client für das Kennwortrückschreiben möglich ist
- Ein Link zur Dokumentation, die Sie bei der Problembehandlung unterstützt 


Weitere Informationen finden Sie unter [Lokale Integration](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>App-basierter bedingter Zugriff mit Azure AD 



 
**Typ:** Neue Funktion  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Identitätssicherheit und -schutz





Sie können jetzt über den [App-basierten bedingten Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) den Zugriff auf Office 365 und weitere mit Azure AD verbundene Cloud-Apps auf [genehmigte Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) beschränken, die Intune-App-Schutzrichtlinien unterstützen. Intune-App-Schutzrichtlinien werden verwendet, um Unternehmensdaten in diesen Clientanwendungen zu konfigurieren und zu schützen.

Durch das Kombinieren von [App-basierten](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) und [gerätebasierten](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) Richtlinien für den bedingten Zugriff können Sie Daten für private Geräte und Unternehmensgeräte flexibel schützen.

Für den App-basierten bedingten Zugriff stehen ab sofort die folgenden Bedingungen und Steuerelemente zur Verfügung:

**Bedingung für unterstützte Plattform**

- iOS
- Android

**Client-Apps-Bedingung**

- Mobile Apps und Desktop-Apps

**Zugriffssteuerung**

- Genehmigte Client-App erforderlich


Weitere Informationen finden Sie unter [App-basierter bedingter Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam).

 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Verwalten von Azure AD-Geräten im Azure-Portal



**Typ:** Neue Funktion  
**Dienstkategorie**: Geräteregistrierung und -verwaltung  
**Produktfunktion**: Identitätssicherheit und -schutz

 



Ab jetzt finden Sie alle mit Azure AD verbundenen Geräte sowie die gerätebezogenen Aktivitäten an einem Ort. Es steht eine neue Verwaltungsoberfläche zur Verfügung, mit der Sie all ihre Geräteidentitäten und -einstellungen im Azure-Portal verwalten können. Dieses Release ermöglicht Folgendes:

- Anzeigen all Ihrer Geräte, die für den bedingten Zugriff in Azure AD zur Verfügung stehen
- Anzeigen von Eigenschaften, einschließlich in Hybrid-Azure AD eingebundenen Geräten
- Auffinden von BitLocker-Schlüsseln für Ihre in Azure AD eingebundenen Geräte, Verwalten Ihrer Geräte mit Intune und vieles mehr
- Verwalten von gerätebezogenen Azure AD-Einstellungen

Weitere Informationen finden Sie unter [Verwalten von Geräten mit dem Azure-Portal](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).



 
---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Unterstützung der macOS-Geräteplattform für den bedingten Azure AD-Zugriff 



**Typ:** Neue Funktion    
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz 
 

Ab sofort können Sie macOS als Geräteplattformbedingung in Ihre Richtlinie für den bedingten Azure AD-Zugriff einschließen (oder ausschließen). Durch das Hinzufügen von macOS als unterstützte Geräteplattform haben Sie folgende Möglichkeiten:

- **Registrieren und Verwalten von macOS-Geräten mit Intune**: Ähnlich wie für andere Plattformen (z.B. iOS und Android) steht für macOS eine Unternehmensportalanwendung bereit, um einheitliche Registrierungen durchzuführen. Sie können ein Gerät mithilfe der neuen Unternehmensportal-App für macOS bei Intune und Azure AD registrieren.
- **Sicherstellen, dass macOS-Geräte die in Intune definierten Konformitätsrichtlinien Ihrer Organisation einhalten**: In Intune im Azure-Portal können Sie nun Konformitätsrichtlinien für macOS-Geräte einrichten. 
- **Beschränken des Zugriffs auf Anwendungen in Azure AD auf ausschließlich konforme macOS-Geräte**: Für die Erstellung bedingter Zugriffsrichtlinien ist macOS als separate Geräteplattformoption festgelegt. Nun können Sie macOS-spezifische Richtlinien für den bedingten Zugriff für die festgelegte Zielanwendung in Azure erstellen.

Weitere Informationen finden Sie unter 

- [Erstellen einer Gerätekonformitätsrichtlinie für macOS-Geräte mit Intune](https://aka.ms/macoscompliancepolicy)
- [Bedingter Zugriff in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)


 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Erweiterung „Netzwerkrichtlinienserver“ für Azure Multi-Factor Authentication 


**Typ:** Neue Funktion    
**Dienstkategorie:** Multi-Factor Authentication  
**Produktfunktion**: Benutzerauthentifizierung




Die Erweiterung „Netzwerkrichtlinienserver“ für Azure Multi-Factor Authentication fügt Ihrer Authentifizierungsinfrastruktur unter Verwendung Ihrer vorhandenen Server cloudbasierte Funktionen zur mehrstufigen Authentifizierung hinzu. Mit der Erweiterung „Netzwerkrichtlinienserver“ können Sie zu Ihrem bestehenden Authentifizierungsablauf eine Überprüfung per Telefonanruf, SMS oder Telefon-App hinzufügen. Dabei müssen keine neuen Server installiert, konfiguriert und verwaltet werden. 

Diese Erweiterung wurde für Organisationen entwickelt, die VPN-Verbindungen schützen möchten, ohne den Azure Multi-Factor Authentication-Server bereitzustellen. Die Erweiterung „Netzwerkrichtlinienserver“ fungiert als Adapter zwischen RADIUS und cloudbasierter Azure Multi-Factor Authentication, um eine zweite Stufe der Authentifizierung für Verbund- oder synchronisierte Benutzer bereitzustellen.


Weitere Informationen finden Sie unter [Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Wiederherstellen oder dauerhaftes Entfernen gelöschter Benutzer


**Typ:** Neue Funktion    
**Dienstkategorie**: Benutzerverwaltung  
**Produktfunktion**: Verzeichnis 



Im Azure AD Admin Center haben Sie jetzt folgende Möglichkeiten:

- Wiederherstellen eines gelöschten Benutzers 
- Dauerhaftes Löschen eines Benutzers


**So können Sie es ausprobieren**

1. Wählen Sie im Azure AD Admin Center im Abschnitt **Verwalten** die Option [Alle Benutzer](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) aus. 

2. Wählen Sie in der Liste **Anzeigen** die Option **Kürzlich gelöschte Benutzer** aus. 

3. Wählen Sie mindestens einen kürzlich gelöschten Benutzer aus, und stellen Sie den Benutzer dann entweder wieder her, oder löschen Sie ihn dauerhaft.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD

 
**Typ:** Geänderte Funktion  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz


Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) hinzugefügt:

- Microsoft Planner
- Azure Information Protection 


Weitere Informationen finden Sie unter 

- [Genehmigte Client-App als Voraussetzung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Einfügen von „OR“ zwischen Steuerelementen in einer Richtlinie für den bedingten Zugriff 


**Typ:** Geänderte Funktion    
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz

 
Ab sofort können Sie „OR“ (eines der ausgewählten Steuerelemente ist erforderlich) zwischen Steuerelementen für den bedingten Zugriff einfügen. Durch dieses Feature können Sie Richtlinien mit einem „OR“ zwischen Steuerelementen für den Zugriff erstellen. Beispielsweise können Sie mit diesem Feature eine Richtlinie erstellen, durch die der Benutzer sich entweder über die mehrstufige Authentifizierung anmelden „OR“ (oder) ein konformes Gerät verwenden muss.

Weitere Informationen finden Sie unter [Steuerelemente beim bedingten Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).

 
---

### <a name="aggregation-of-real-time-risk-events"></a>Aggregation von Echtzeitrisikoereignissen


**Typ:** Geänderte Funktion    
**Dienstkategorie**: Identitätsschutz  
**Produktfunktion**: Identitätssicherheit und -schutz


Ab sofort werden jetzt in Azure AD Identity Protection alle Echtzeitrisikoereignisse, die von derselben IP-Adresse stammen, für einen vorgegebenen Tag für jeden Risikoereignistyp aggregiert. Durch diese Änderung werden weniger Risikoereignisse ohne Änderung der Benutzersicherheit angezeigt.

Die zugrunde liegende Echtzeiterkennung erfolgt bei jeder Benutzeranmeldung. Wenn Sie eine Sicherheitsrichtlinie für das Anmelderisiko auf Multi-Factor Authentication oder eine Blockierung des Zugriffs festgelegt haben, wird diese bei jeder riskanten Anmeldung weiterhin ausgelöst.

 
---
 




## <a name="october-2017"></a>Oktober 2017


### <a name="deprecate-azure-ad-reports"></a>Einstellung von Azure AD-Berichten


**Typ:** Plan für Änderung  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus  



Azure-Portal bietet Ihnen Folgendes:

- Eine neue Azure AD-Verwaltungskonsole
- Neue APIs für Aktivitäts- und Sicherheitsberichte
 
Aufgrund dieser neuen Funktionen werden die Berichts-APIs auf dem Endpunkt unter „/reports“ am 10. Dezember 2017 eingestellt. 

---

### <a name="automatic-sign-in-field-detection"></a>Automatische Erkennung von Anmeldefeldern


**Typ:** Korrigiert   
**Dienstkategorie**: Meine Apps  
**Produktfunktion:** Einmaliges Anmelden  



Azure AD unterstützt die automatische Erkennung von Anmeldefeldern für Anwendungen, die ein HTML-Benutzernamen- und Kennwortfeld rendern. Diese Schritte sind unter [So erfassen Sie Anmeldefelder für eine Anwendung automatisch](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application) dokumentiert. Sie finden diese Funktion, indem Sie im [Azure-Portal](http://aad.portal.azure.com) auf der Seite **Unternehmensanwendungen** eine *Nicht-Katalog*-Anwendung hinzufügen. Zusätzlich können Sie den Modus **Einmaliges Anmelden** in dieser neuen Anwendung auf **Kennwortbasiertes einmaliges Anmelden** festlegen, indem Sie eine Web-URL eingeben und die Seite anschließend speichern.
 
Aufgrund eines Dienstproblems war diese Funktion vorübergehend deaktiviert. Das Problem wurde behoben, und die automatische Erkennung des Anmeldefelds ist nun wieder verfügbar.

---

### <a name="new-multi-factor-authentication-features"></a>Neue Multi-Factor Authentication-Features


**Typ:** Neue Funktion  
**Dienstkategorie**: Multi-Factor Authentication  
**Produktfunktion**: Identitätssicherheit und -schutz  



Die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) ist ein wesentlicher Bestandteil des Schutzes Ihrer Organisation. Um die Anmeldeinformationen anpassungsfähiger und das Benutzererlebnis nahtloser zu gestalten, wurden die folgenden Features hinzugefügt: 

- Die Captcha-Ergebnisse bei der mehrstufigen Authentifizierung sind unmittelbar in den Azure AD-Anmeldebericht integriert, einschließlich des programmgesteuerten Zugriffs auf MFA-Ergebnisse.
- Die MFA-Konfiguration wird stärker in die zentrale Azure AD-Konfigurationsumgebung im Azure-Portal integriert.

Mit dieser öffentlichen Vorschau werden MFA-Verwaltung und -Berichterstellung zu einem integrierten Bestandteil der zentralen Azure AD-Konfigurationsumgebung. Nun können Sie die MFA-Verwaltungsportalfunktionalität in der Azure AD-Oberfläche verwalten.

Weitere Informationen finden Sie in der [Referenz zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 


---

### <a name="terms-of-use"></a>Nutzungsbedingungen



**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Governance/Compliance  



Mithilfe der Azure AD-Nutzungsbedingungen können Sie Benutzern Informationen wie relevante Haftungsausschlüsse zur Erfüllung gesetzlicher oder Complianceanforderungen zur Verfügung stellen.

Sie können Azure AD-Nutzungsbedingungen in den folgenden Szenarien verwenden:

- Allgemeine Nutzungsbedingungen für alle Benutzer in Ihrer Organisation
- Spezifische Nutzungsbedingungen basierend auf Benutzerattributen (z.B. Ärzte im Vergleich zu Krankenschwestern oder einheimische Mitarbeiter im Vergleich zu ausländischen Mitarbeitern) von dynamischen Gruppen
- Spezifische Nutzungsbedingungen für den Zugriff auf bedeutende Geschäftsanwendungen wie Salesforce

Weitere Informationen finden Sie im [Azure AD B2C](https://docs.microsoft.com/azure/active-directory/active-directory-tou).


---

### <a name="enhancements-to-privileged-identity-management"></a>Verbesserungen an Privileged Identity Management


**Typ:** Neue Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management  


Mit Azure AD Privileged Identity Management können Sie den Zugriff auf Azure-Ressourcen (Vorschauversion) für folgende Elemente innerhalb Ihrer Organisation verwalten, steuern und überwachen:

- Abonnements
- Ressourcengruppen
- Virtuelle Computer 

Alle Ressourcen im Azure-Portal, die die Azure RBAC-Funktion nutzen, können von sämtlichen Sicherheits- und Lebenszyklusverwaltungsfunktionen profitieren, die Azure AD Privileged Identity Management bereitstellt.

Weitere Informationen finden Sie unter [Privileged Identity Management für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).


---

### <a name="access-reviews"></a>Zugriffsüberprüfungen


**Typ:** Neue Funktion  
**Dienstkategorie** : Zugriffsüberprüfungen  
**Produktfunktion:** Governance/Compliance  



Organisationen können mithilfe von Zugriffsüberprüfungen (in der Vorschauversion) effizient Gruppenmitgliedschaften und den Zugriff auf Unternehmensanwendungen verwalten: 

- Sie können den Gastbenutzerzugriff erneut bestätigen, indem Sie den Zugriff auf Anwendungen und Gruppenmitgliedschaften im Rahmen einer Zugriffsüberprüfung überprüfen. Anhand der Erkenntnisse aus den Zugriffsüberprüfungen können Prüfer effizient entscheiden, ob Gäste weiterhin Zugriff haben sollen.
- Mithilfe von Zugriffsüberprüfungen können Sie den Anwendungszugriff von Mitarbeitern sowie deren Gruppenmitgliedschaften neu zertifizieren.

Die Kontrollen der Zugriffsüberprüfung können in unternehmensrelevanten Programmen zusammengefasst werden, um Überprüfungen für compliance- oder risikokritische Anwendungen nachzuverfolgen.

Weitere Informationen finden Sie unter [Azure AD-Zugriffsüberprüfungen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Möglichkeit zum Ausblenden von Drittanbieteranwendungen in „Meine Apps“ und im Office 365-App-Startprogramm



**Typ:** Neue Funktion  
**Dienstkategorie**: Meine Apps  
**Produktfunktion:** Einmaliges Anmelden  



Mit der neuen Eigenschaft **App ausblenden** können Sie nun Apps, die in Ihren Benutzerportalen angezeigt werden, besser verwalten. Sie können Apps ausblenden für den Fall, dass App-Kacheln für Back-End-Dienste angezeigt werden oder Kacheln doppelt vorhanden sind und die App-Startprogramme von Benutzern überladen. Die Umschaltfläche befindet sich im Abschnitt **Eigenschaften** der Drittanbieter-App und hat die Bezeichnung **Für Benutzer sichtbar?**. Sie können eine App auch programmgesteuert über PowerShell ausblenden. 

Weitere Informationen finden Sie unter [Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Was ist verfügbar?**

 Im Rahmen der Umstellung auf die neue Verwaltungskonsole stehen zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Die Daten, die bisher in den Sicherheitsberichten verfügbar waren, können jetzt über die API für Identity Protection-Risikoereignisse in Microsoft Graph abgerufen werden.


## <a name="september-2017"></a>September 2017

### <a name="hotfix-for-identity-manager"></a>Hotfix für Identity Manager


**Typ:** Geänderte Funktion  
**Dienstkategorie**: Identity Manager  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus  



Ein Hotfixrollup-Paket (Build 4.4.1642.0) ist ab dem 25. September 2017 für Identity Manager 2016 Service Pack 1 verfügbar. Dieser Rolluppaket enthält Folgendes:

- Problembehebungen und Verbesserungen
- Ein kumulatives Update, das alle Identity Manager 2016 Service Pack 1-Updates bis Build 4.4.1459.0 für Identity Manager 2016 ersetzt 
- Identity Manager 2016 Build 4.4.1302.0 wird vorausgesetzt. 

Weitere Informationen finden Sie unter [Hotfixrollup-Paket (Build 4.4.1642.0) ist für Identity Manager 2016 Service Pack 1 verfügbar](https://support.microsoft.com/help/4021562). 

---
