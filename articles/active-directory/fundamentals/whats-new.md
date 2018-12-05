---
title: Neuerungen Anmerkungen zur Version für Azure AD | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory (Azure AD), z.B. aktuelle Anmerkungen zu dieser Version, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: active-directory
author: eross-msft
manager: mtillman
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: dc4e421808ab0f79070224edea4b75f527affaf0
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426236"
---
# <a name="whats-new-in-azure-active-directory"></a>Neuerungen in Azure Active Directory

>Sie können eine Benachrichtigung erhalten, wann auf dieser Seite Updates vorhanden sind, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` kopieren und in Ihren ![RSS-Symbol](./media/whats-new/feed-icon-16x16.png)-Feedreader einfügen.

Es werden fortlaufend Verbesserungen an Azure AD vorgenommen. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Fehlerbehebungen
- Veraltete Funktionen
- Pläne für Änderungen

Besuchen Sie regelmäßig diese Seite. Diese wird monatlich aktualisiert. Wenn Sie nach Elementen suchen, die älter als 6 Monate sind können Sie sie im [Archiv für Neuheiten in Azure Active Directory](whats-new-archive.md) finden.

---
## <a name="october-2018"></a>Oktober 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD-Protokolle funktionieren jetzt mit Azure Log Analytics (öffentliche Vorschau)

**Typ:** Neue Funktion  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir freuen uns, Ihnen mitteilen zu können, dass Sie Ihre Azure AD-Protokolle jetzt an Azure Log Analytics weiterleiten können. Dieses Feature – das auf der Wunschliste unserer Kunden ganz oben stand – ermöglicht einen noch besseren Zugang zu Analysefunktionen für Business, Betrieb und Sicherheit und bietet eine Methode für die Überwachung Ihrer Infrastruktur. Weitere Informationen finden Sie im Blog [Azure Active Directory Activity logs in Azure Log Analytics now available](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) (Azure Active Directory-Aktivitätsprotokolle jetzt in Azure Log Analytics verfügbar).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Oktober 2018

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Im Oktober 2018 haben wir diese 14 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[My Award Points](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), Dialpad, [ON24 Virtual Environment](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler Three](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot Control](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-domain-services-email-notifications"></a>Azure AD Domain Services – E-Mail-Benachrichtigungen

**Typ:** Neue Funktion  
**Dienstkategorie**: Azure AD Domain Services  
**Produktfunktion**: Azure AD Domain Services

Azure AD Domain Services bietet Warnungen im Azure-Portal zu fehlerhaften Konfigurationen oder Problemen in Ihrer verwalteten Domäne. Diese Warnungen umfassen Schrittanleitungen, sodass Sie die Probleme beheben können, ohne sich an den Support wenden zu müssen.

Ab Oktober können Sie die Benachrichtigungseinstellungen für Ihre verwaltete Domäne anpassen, sodass beim Auftreten neuer Warnungen eine E-Mail an eine angegebene Gruppe von Personen gesendet wird. So müssen Sie nicht mehr dauernd das Portal auf Updates überprüfen.

Weitere Informationen finden Sie unter [Benachrichtigungseinstellungen in Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications).

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Das Azure AD-Portal unterstützt die Verwendung der ForceDelete-Domänen-API zum Löschen von benutzerdefinierten Domänen 

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Verzeichnisverwaltung  
**Produktfunktion**: Verzeichnis

Wir freuen uns, Ihnen mitteilen zu können, dass Sie jetzt die ForceDelete-Domänen-API verwenden können, um Ihre benutzerdefinierten Domänennamen zu löschen. Über die API werden Verweise wie Benutzer, Gruppen und Apps mit Ihrem benutzerdefinierten Domänennamen (contoso.com) asynchron wieder in den standardmäßigen Domänennamen (contoso.onmicrosoft.com) umbenannt.

Dank dieser Änderung können Sie Ihren benutzerdefinierten Domänennamen schneller löschen, wenn dieser von Ihrem Unternehmen nicht mehr verwendet wird oder wenn Sie den Domänennamen für eine andere Azure AD-Instanz verwenden möchten.

Weitere Informationen finden Sie unter [Löschen eines benutzerdefinierten Domänennamens](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>September 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Aktualisierte Berechtigungen der Administratorrolle für dynamische Gruppen

**Typ:** Korrigiert  
**Dienstkategorie**: Gruppenverwaltung  
**Produktfunktion:** Kollaboration

Wir haben ein Problem behoben, damit mit bestimmten Administratorrollen jetzt dynamische Mitgliedschaftsregeln erstellt und aktualisiert werden können, ohne dass der Benutzer der Besitzer der Gruppe sein müssen.

Die Rollen lauten:

- Globaler Administrator oder Unternehmensredakteur

- Intune-Dienstadministrator

- Benutzerkontoadministrator

Weitere Informationen finden Sie unter [Erstellen einer dynamischen Gruppe und Überprüfen des Status](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule).

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Vereinfachte Konfigurationseinstellungen für einmaliges Anmelden (SSO) bei einigen Drittanbieter-Apps

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: SSO

Uns ist bewusst, dass die Einrichtung des einmaligen Anmeldens (SSO) für SaaS-Apps (Software-as-a-Service) aufgrund der speziellen Konfiguration von Apps eine Herausforderung darstellen kann. Wir haben eine vereinfachte Benutzeroberfläche für die Konfiguration erstellt, um die SSO-Konfigurationseinstellungen für die folgenden Drittanbieter-SaaS-Apps automatisch einzufügen:

- Zendesk

- ArcGis Online

- Jamf Pro

Sie können diese 1-Klick-Oberfläche verwenden, indem Sie für die App zu **Azure-Portal** > **SSO-Konfiguration** navigieren. Weitere Informationen finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list).

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Seite „Azure Active Directory – An welchem Ort befinden sich Ihre Daten?“

**Typ:** Neue Funktion  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** GoLocal

Wählen Sie auf der Seite **Azure Active Directory – An welchem Ort befinden sich Ihre Daten?** die Region Ihres Unternehmens aus, um anzuzeigen, in welchem Azure-Rechenzentrum sich Ihre ruhenden Azure AD-Daten für alle Azure AD-Dienste befinden. Sie können die Informationen nach bestimmten Azure AD-Diensten für die Region Ihres Unternehmens filtern.

Informationen zum Zugriff auf dieses Feature und weitere Informationen finden Sie unter [Azure Active Directory – An welchem Ort befinden sich Ihre Daten?](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Neuer Bereitstellungsplan verfügbar für Zugriffsbereich „Meine Apps“

**Typ:** Neue Funktion  
**Dienstkategorie**: Meine Apps  
**Produktfunktion**: SSO

Erkunden Sie den neuen Bereitstellungsplan, der für den Zugriffsbereich „Meine Apps“ verfügbar ist (https://aka.ms/deploymentplans).
Im Zugriffsbereich „Meine Apps“ können Benutzer an einem zentralen Ort nach ihren Apps suchen und darauf zugreifen. In diesem Portal können Benutzer auch Self-Service-Funktionen verwenden, z.B. das Anfordern des Zugriffs auf Apps und Gruppen oder das Verwalten des Zugriffs auf diese Ressourcen im Namen von anderen Personen.

Weitere Informationen finden Sie unter [Was ist das MyApps-Portal?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction).

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Neue Registerkarte „Problembehandlung und Support“ auf der Seite „Anmeldungen“ im Azure-Portal

**Typ:** Neue Funktion  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Die neue Registerkarte **Problembehandlung und Support** auf der Seite **Anmeldungen** des Azure-Portals dient Administratoren und Engineers als Hilfe beim Behandeln von Problemen mit der Azure AD-Anmeldung. Diese neue Registerkarte enthält den Fehlercode, die Fehlermeldung und Empfehlungen zur Problemlösung (falls zutreffend), um die Behebung des Problems zu ermöglichen. Wenn das Problem nicht lösbar ist, können Sie auch eine neue Möglichkeit zum Erstellen eines Supporttickets nutzen, indem Sie die Option **In Zwischenablage kopieren** verwenden. Bei dieser Option werden die Felder **Anforderungs-ID** und **Datum (UTC)** für die Protokolldatei in Ihrem Supportticket ausgefüllt.  

![Anmeldeprotokolle mit der neuen Registerkarte](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Verbesserte Unterstützung für benutzerdefinierte Erweiterungseigenschaften, die zum Erstellen von dynamischen Mitgliedschaftsregeln verwendet werden

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Gruppenverwaltung  
**Produktfunktion:** Kollaboration

Mit diesem Update können Sie jetzt im Regel-Generator für dynamische Benutzergruppen auf den Link **Get custom extension properties** (Benutzerdefinierte Erweiterungseigenschaften abrufen) klicken, Ihre eindeutige App-ID eingeben und die vollständige Liste mit benutzerdefinierten Erweiterungseigenschaften erhalten, die Sie beim Erstellen einer dynamischen Mitgliedschaftsregeln für Benutzer verwenden. Diese Liste kann auch aktualisiert werden, um neue benutzerdefinierte Erweiterungseigenschaften für diese App zu erhalten.

Weitere Informationen zur Verwendung von benutzerdefinierten Erweiterungseigenschaften für dynamische Mitgliedschaftsregeln finden Sie unter [Erweiterungseigenschaften und benutzerdefinierte Erweiterungseigenschaften](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties).

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD

**Typ:** Plan für Änderung  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz

Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement) hinzugefügt:

- Microsoft To-Do

- Microsoft Stream

Weitere Informationen finden Sie unter

- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Neue Unterstützung für Self-Service-Kennwortzurücksetzung über den Windows 7/8/8.1-Sperrbildschirm

**Typ:** Neue Funktion  
**Dienstkategorie:** SSPR  
**Produktfunktion**: Benutzerauthentifizierung

Nachdem Sie dieses neue Feature eingerichtet haben, wird Ihren Benutzern ein Link zum Zurücksetzen Ihres Kennworts über den **Sperrbildschirm** eines Geräts mit Windows 7, Windows 8 oder Windows 8.1 angezeigt. Wenn Benutzer auf diesen Link klicken, werden sie durch den gleichen Ablauf zur Kennwortzurücksetzung wie im Webbrowser geleitet.

Weitere Informationen finden Sie unter [How to enable password reset from Windows 7, 8, and 8.1](https://aka.ms/ssprforwindows78) (Aktivieren der Kennwortzurücksetzung unter Windows 7, 8 und 8.1).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Änderungshinweis: Autorisierungscodes stehen nicht mehr für die Wiederverwendung zur Verfügung 

**Typ:** Plan für Änderung  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung

Ab dem 15. November 2018 akzeptiert Azure AD bereits zuvor verwendete Authentifizierungscodes für Apps nicht mehr. Diese Sicherheitsänderung trägt dazu bei, Azure AD an die OAuth-Spezifikation anzupassen. Sie wird auf v1- und v2-Endpunkten erzwungen.

Wenn Ihre App Autorisierungscodes zum Abrufen von Token für mehrere Ressourcen wiederverwendet, sollten Sie den Code für das Abrufen eines Aktualisierungstokens verwenden. Verwenden Sie dieses Aktualisierungstoken anschließend, um die zusätzlichen Token für andere Ressourcen abzurufen. Autorisierungscodes können nur einmal verwendet werden, Aktualisierungstoken hingegen können mehrere Male und für mehrere Ressourcen verwendet werden. Für jede App, die einen Authentifizierungscode im OAuth-Codefluss erneut verwenden möchte, wird ein Fehler vom Typ „invalid_grant“ zurückgegeben.

Informationen hierzu und zu anderen protokollbezogenen Änderungen finden Sie in der [vollständigen Liste mit den Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – September 2018

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Im September 2018 haben wir diese 16 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet Recruiting Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Enlyft SSO for Azure, SurveyMonkey, [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="support-for-additional-claims-transformations-methods"></a>Unterstützung für zusätzliche Transformationsmethoden für Ansprüche

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: SSO

Wir haben die neuen Transformationsmethoden „ToLower()“ und „ToUpper()“ eingeführt, die über die SAML-basierte Seite **SSO-Konfiguration** auf SAML-Token angewendet werden können.

Weitere Informationen finden Sie unter [Gewusst wie: Anpassen ausgestellter Ansprüche im SAML-Token für Unternehmensanwendungen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Aktualisierte SAML-basierte Benutzeroberfläche für die App-Konfiguration (Vorschauversion)

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: SSO

Im Rahmen unserer aktualisierten SAML-basierten Benutzeroberfläche für die App-Konfiguration erhalten Sie Folgendes:

- Eine aktualisierte Benutzeroberfläche mit Schritt-für-Schritt-Anleitung für die Konfiguration Ihrer SAML-basierten Apps

- Bessere Informationen zu fehlenden oder fehlerhaften Teilen der Konfiguration

- Möglichkeit zum Hinzufügen von mehreren E-Mail-Adressen für Benachrichtigungen zum Zertifikatablauf

- Neue Transformationsmethoden „ToLower()“ und „ToUpper()“ für Ansprüche und mehr

- Möglichkeit zum Hochladen Ihres eigenen Tokensignaturzertifikats für Ihre Unternehmens-Apps

- Möglichkeit zum Festlegen des NameID-Formats für SAML-Apps und des NameID-Werts als Verzeichniserweiterungen

Klicken Sie zum Aktivieren dieser aktualisierten Ansicht oben auf der Seite **Einmaliges Anmelden** auf den Link **Neue Benutzeroberfläche ausprobieren**. Weitere Informationen finden Sie unter [Tutorial: Konfigurieren des SAML-basierten einmaligen Anmeldens für eine Anwendung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).

---

## <a name="august-2018"></a>August 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Änderungen an Azure Active Directory-IP-Adressbereichen

**Typ:** Plan für Änderung  
**Dienstkategorie:** Sonstige  
**Produktfunktion**: Plattform

Es werden größere IP-Bereiche in Azure AD eingeführt. Wenn Sie Azure AD-IP-Adressbereiche für Ihre Firewalls, Router oder Netzwerksicherheitsgruppen konfiguriert haben, müssen Sie diese daher aktualisieren. Wir führen dieses Update durch, sodass Sie die Konfiguration der IP-Bereiche für die Firewalls, Router oder Netzwerksicherheitsgruppen nicht erneut ändern müssen, wenn in Azure AD neue Endpunkte hinzugefügt werden. 

Der Netzwerkdatenverkehr wird in den nächsten zwei Monaten in diese neuen Bereiche verschoben. Um einen ununterbrochenen Dienst gewährleisten zu können, müssen Sie Ihren IP-Adressen vor dem 10. September 2018 die folgenden aktualisierten Werten hinzufügen:

- 20.190.128.0/18 

- 40.126.0.0/18 

Es wird dringend empfohlen, die alten IP-Adressbereiche erst zu entfernen, nachdem Ihr gesamter Netzwerkdatenverkehr in die neuen Bereiche verschoben wurde. Aktualisierungen zu dieser Verschiebung und Informationen dazu, wann die alten Bereiche entfernt werden können, finden Sie unter [URLs und IP-Adressbereiche von Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Änderungshinweis: Autorisierungscodes stehen nicht mehr für die Wiederverwendung zur Verfügung 

**Typ:** Plan für Änderung  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung

Ab dem 15. November 2018 akzeptiert Azure AD bereits zuvor verwendete Authentifizierungscodes für Apps nicht mehr. Diese Sicherheitsänderung trägt dazu bei, Azure AD an die OAuth-Spezifikation anzupassen. Sie wird auf v1- und v2-Endpunkten erzwungen.

Wenn Ihre App Autorisierungscodes zum Abrufen von Token für mehrere Ressourcen wiederverwendet, sollten Sie den Code für das Abrufen eines Aktualisierungstokens verwenden. Verwenden Sie dieses Aktualisierungstoken anschließend, um die zusätzlichen Token für andere Ressourcen abzurufen. Autorisierungscodes können nur einmal verwendet werden, Aktualisierungstoken hingegen können mehrere Male und für mehrere Ressourcen verwendet werden. Für jede App, die einen Authentifizierungscode im OAuth-Codefluss erneut verwenden möchte, wird ein Fehler vom Typ „invalid_grant“ zurückgegeben.

Informationen hierzu und zu anderen protokollbezogenen Änderungen finden Sie in der [vollständigen Liste mit den Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Zusammengeführte Sicherheitsinformationenverwaltung für Self-Service-Kennwortzurücksetzung (SSPR) und Multi-Factor Authentication (MFA)

**Typ:** Neue Funktion  
**Dienstkategorie:** SSPR  
**Produktfunktion**: Benutzerauthentifizierung
 
Dieses neue Feature hilft Benutzern bei der Verwaltung ihrer Sicherheitsinformationen (z.B. Telefonnummer, mobile App usw.) für SSPR und MFA an einem zentralen Ort und mit einer einheitlichen Benutzeroberfläche, wofür früher zwei verschiedenen Umgebungen notwendig waren.

Diese zusammengeführte Benutzeroberfläche funktioniert auch für Personen, die entweder SSPR oder MFA verwenden. Wenn darüber hinaus Ihre Organisation die MFA- oder SSPR-Registrierung nicht erzwingt, können sich Personen trotzdem mit beliebigen Verfahren unter Verwendung von MFA- oder SSPR-Sicherheitsinformationen anmelden, sofern diese von Ihrer Organisation im Meine Apps-Portal zugelassen wurden.

Dies ist eine abonnierbare Public Preview. Administratoren können die neue Oberfläche (falls gewünscht) für eine ausgewählte Gruppe von Benutzern oder alle Benutzer in einem Mandanten aktivieren. Weitere Informationen zur zusammengeführten Benutzeroberfläche finden Sie im Blogbeitrag [Converged experience](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/) (Zusammengeführte Benutzeroberfläche).

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Neue Einstellung „HTTP-Only Cookies“ (Nur HTTP-Cookies) in Azure AD-Anwendungsproxy-Apps

**Typ:** Neue Funktion  
**Dienstkategorie:** App-Proxy  
**Produktfunktion**: Zugriffssteuerung

Für Anwendungsproxy-Apps steht die neue Einstellung **HTTP-Only Cookies** (Nur HTTP-Cookies) zur Verfügung. Diese Einstellung bietet zusätzliche Sicherheit, da in HTTP-Antwortheader für den Zugriff auf den Anwendungsproxy und für Sitzungscookies das Flag „HTTPOnly“ eingefügt wird. Dieses beendet den Zugriff auf das Cookie von einem clientseitigen Skript und verhindert damit weitere Aktionen wie das Kopieren oder Ändern des Cookies. Auch wenn dieses Flag zuvor nicht verwendet wurde, wurden Ihre Cookies immer verschlüsselt und über eine SSL-Verbindung übertragen, um sie vor unerwünschten Änderungen zu schützen.

Diese Einstellung nicht mit Apps kompatibel, die ActiveX-Steuerelemente verwenden, z.B. Remotedesktop. In diesen Fällen wird das Deaktivieren dieser Einstellung empfohlen.

Weitere Informationen zur Einstellung „HTTP-Only Cookies“ (Nur HTTP-Cookies) finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Privileged Identity Management (PIM) für Azure-Ressourcen unterstützt Verwaltungsgruppen-Ressourcentypen

**Typ:** Neue Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Die Einstellungen für die Just-In-Time-Aktivierung und -Zuweisung können jetzt auf Verwaltungsgruppen-Ressourcentypen angewandt werden, wie dies bereits für Abonnements, Ressourcengruppen und Ressourcen (z.B. virtuelle Computer, App Services und mehr) möglich war. Darüber hinaus kann jede Person mit einer Rolle mit Administratorzugriff auf eine Verwaltungsgruppe diese Ressource in PIM ermitteln und verwalten.

Weitere Informationen zu PIM und Azure-Ressourcen finden Sie unter [Ermitteln und Verwalten von Azure-Ressourcen mit Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources).
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Anwendungszugriff (Vorschau) mit schnellerem Zugriff auf das Azure AD-Portal

**Typ:** Neue Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Wenn Sie heute eine Rolle mithilfe von PIM aktivieren, kann die Anwendung der Berechtigungen mehr als 10 Minuten dauern. Wenn Sie den Anwendungszugriff, der sich derzeit in der Public Preview befindet, verwenden, können Administratoren direkt auf das Azure AD-Portal zugreifen, nachdem die Aktivierungsanforderung abgeschlossen ist.

Derzeit unterstützt der Anwendungszugriff nur das Azure AD-Portal und Azure-Ressourcen. Weitere Informationen zu PIM und dem Anwendungszugriff finden Sie unter [Was ist Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure).
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – August 2018

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Im August 2018 haben wir diese 16 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [Bridgeline Unbound](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Sauce Labs - Mobile and Web Testing](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta Networks Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [Way We Do](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [ProMaster (by Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - Expense reports](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 Live Chat](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Native Tableau-Unterstützung jetzt im Azure AD-Anwendungsproxy verfügbar

**Typ:** Geänderte Funktion  
**Dienstkategorie:** App-Proxy  
**Produktfunktion**: Zugriffssteuerung

Mit unserem Update von OpenID Connect auf das OAuth 2.0-Codeberechtigungsprotokoll für unser Vorauthentifizierungsprotokoll müssen Sie keine zusätzliche Konfiguration zum Verwenden von Tableau mit dem Anwendungsproxy mehr vornehmen. Diese Protokolländerung hilft auch dem Anwendungsproxy bei der besseren Unterstützung moderner Apps mit Nur-HTTP-Umleitungen, die häufig in JavaScript- und HTML-Tags unterstützt werden.

Weitere Informationen zu unserer nativen Unterstützung für Tableau finden Sie unter [Azure AD Application Proxy now with native Tableau support](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support) (Azure AD-Anwendungsproxy jetzt auch mit nativer Unterstützung für Tableau).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Neu: Unterstützung für das Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer in Azure Active Directory (Vorschau)

**Typ:** Neue Funktion  
**Dienstkategorie:** B2B  
**Produktfunktion**: B2B/B2C

Durch das Einrichten eines Verbunds mit Google in Ihrer Organisation können Sie eingeladenen Gmail-Benutzern die Anmeldung bei Ihren freigegebenen Apps und Ressourcen mit ihren bestehenden Google-Konten ermöglichen, ohne dass sie ein persönliches Microsoft-Konto (MSA) oder Azure AD-Konto erstellen müssen.

Dies ist eine abonnierbare Public Preview. Weitere Informationen zum Google-Verbund finden Sie unter [Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Juli 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Verbesserungen an Azure Active Directory-E-Mail-Benachrichtigungen

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Sonstige  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus
 
Azure Active Directory-E-Mails (Azure AD) weisen nun ein aktualisiertes Layout sowie Änderungen an der E-Mail-Adresse des Absenders und dem Anzeigenamen des Empfängers auf, wenn sie über die folgenden Dienste gesendet wurden:
 
- Azure AD-Zugriffsüberprüfungen
- Azure AD Connect Health 
- Azure AD Identity Protection 
- Azure AD Privileged Identity Management
- Benachrichtigungen zu ablaufenden Zertifikaten von Unternehmen-Apps
- Benachrichtigungen zum Bereitstellungsdienst von Unternehmen-Apps
 
Die E-Mail-Benachrichtigungen werden von folgender E-Mail-Adresse und folgendem Anzeigenamen gesendet:

- E-Mail-Adresse: azure-noreply@microsoft.com
- Anzeigename: Microsoft Azure
 
Ein Beispiel für einige der neuen E-Mail-Layouts und weitere Informationen finden Sie unter [E-Mail-Benachrichtigungen in Azure AD PIM](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD-Aktivitätsprotokolle sind jetzt über Azure Monitor verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Die Azure AD-Aktivitätsprotokolle stehen jetzt in der Public Preview für Azure Monitor (die gesamte Plattform umfassender Überwachungsdienst von Azure) zur Verfügung. Azure Monitor bietet langfristige Aufbewahrung und nahtlose Integration, zusätzlich zu diesen Verbesserungen:

- Langfristige Aufbewahrung durch Routing Ihrer Protokolldateien an Ihr eigenes Azure-Speicherkonto.

- Nahtlose SIEM-Integration, ohne dass Sie benutzerdefinierte Skripts schreiben oder verwalten müssen.

- Nahtlose Integration in Ihre eigenen benutzerdefinierten Lösungen, Analysetools oder Lösungen zur Verwaltung von Incidents.

Weitere Informationen zu diesen neuen Funktionen finden Sie in unserem Blog [Azure AD activity logs in Azure Monitor diagnostics is now in public preview (Azure AD-Aktivitätsprotokolle in der Azure Monitor-Diagnose stehen jetzt in der Public Preview zur Verfügung)](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) und unserer Dokumentation [Azure AD-Aktivitätsprotokolle in Azure Monitor (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Informationen für den bedingten Zugriff werden dem Azure AD-Anmeldebericht hinzugefügt

**Typ:** Neue Funktion  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion**: Identitätssicherheit und -schutz
 
Dieses Update zeigt Ihnen, welche Richtlinien ausgewertet werden, wenn ein Benutzer sich mit dem Ergebnis der Richtlinie anmeldet. Darüber hinaus enthält der Bericht jetzt den Typ der vom Benutzer verwendeten Client-App, sodass Sie älteren Protokolldatenverkehr identifizieren können. Berichtseinträge können jetzt auch nach einer Korrelations-ID durchsucht werden, die in der an den Benutzer gerichteten Fehlermeldung enthalten ist und dazu verwendet werden kann, die entsprechende Anmeldungsanforderung zu identifizieren und dort ggf. eine Problembehandlung durchzuführen.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Anzeige von Legacyauthentifizierungen über Protokolle zur Anmeldeaktivität

**Typ:** Neue Funktion  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Dank der Einführung des Felds **Client-App** in den Protokollen zur Anmeldeaktivität können Kunden jetzt Benutzer anzeigen, die Authentifizierungen einer Vorgängerversion nutzen. Kunden können auf diese Informationen zugreifen, indem sie die Microsoft Graph-API für Anmeldungen oder die Protokolle zur Anmeldeaktivität im Azure AD-Portal verwenden. Hier können Sie das Steuerelement **Client-App** nutzen, um nach Legacyauthentifizierungen zu filtern. Weitere Informationen finden Sie in der Dokumentation.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Juli 2018

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Im Juli 2018 haben wir diese 16 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Innovation Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [Certain Admin SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC Staging, [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [Screencast-O-Matic](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial), PowerSchool Unified Classroom, [Eli Onboarding](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar Remote Support](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial), [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [Skills Base](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Neue Integrationen zur Benutzerbereitstellung in SaaS-Apps – Juli 2018

**Typ:** Neue Funktion  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Azure AD ermöglicht Ihnen das automatisierte Erstellen, Warten und Entfernen von Benutzeridentitäten in SaaS-Anwendungen, z.B. Dropbox, Salesforce, ServiceNow und anderen. Für Juli 2018 haben wir für die folgenden Anwendungen im Azure AD-App-Katalog Unterstützung für die Benutzerbereitstellung hinzugefügt:

- [Cisco Spark](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial)

- [Cisco Webex](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Eine Liste mit allen Anwendungen, für die die Benutzerbereitstellung im Azure AD-Katalog unterstützt wird, finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Connect Health für die Synchronisierung – eine einfachere Möglichkeit, um Synchronisierungsfehler in Verbindung mit verwaisten und doppelten Attributen zu beheben.

**Typ:** Neue Funktion  
**Dienstkategorie**: AD Connect  
**Produktfunktion:** Überwachung und Berichterstellung
 
Azure AD Connect Health führt die Self-Service-Wartung ein, die Sie beim Hervorheben und Beheben von Synchronisierungsfehlern unterstützt. Dieses Feature kann für die Problembehandlung bei Synchronisierungsfehlern aufgrund doppelter Attribute und zur Behebung von in Azure AD verwaisten Objekten verwendet werden. Diese Diagnose hat folgende Vorteile:

- Eingrenzung von Synchronisierungsfehlern mit doppelten Attributen, Bereitstellung spezifischer Behebungsverfahren

- Anwendung einer Korrektur für dedizierte Azure AD-Szenarien, Beheben von Fehlern in einem einzigen Schritt

- Kein Upgrade- oder Konfigurationsaufwand zur Aktivierung und Verwendung dieses Features

Weitere Informationen finden Sie unter [Diagnose und Behebung von Synchronisierungsfehlern aufgrund doppelter Attribute](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors).

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Visuelle Updates der Azure AD- und MSA-Anmeldebenutzeroberflächen

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Benutzerauthentifizierung

Wir haben die Benutzeroberfläche für die Anmeldung bei Microsoft-Onlinediensten wie Office 365 und Azure aktualisiert. Durch diese Änderung sind die Bildschirme weniger überladen und übersichtlicher. Weitere Informationen zu dieser Änderung finden Sie im Blog [Upcoming improvements to the Azure AD sign-in experience (Bevorstehende Verbesserungen der Azure AD-Anmeldebenutzeroberfläche)](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/).

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Neues Release von Azure AD Connect – Juli 2018

**Typ:** Geänderte Funktion  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus

Das neueste Release von Azure AD Connect umfasst: 

- Programmfehlerbehebungen und Unterstützungsupdates 

- Allgemeine Verfügbarkeit der PingFederate-Integration

- Updates des aktuellen SQL 2012-Clients 

Weitere Informationen zu diesem Update finden Sie unter [Azure AD Connect: Versionsveröffentlichungsverlauf](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history).

---

### <a name="updates-to-the-terms-of-use-tou-end-user-ui"></a>Updates der Endbenutzeroberfläche der Nutzungsbedingungen (Terms of Use, ToU)

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion**: Governance

Wir aktualisieren die Zustimmungszeichenfolge in der Endbenutzeroberfläche der ToU.

**Aktueller Text.** Für den Zugriff auf die [Mandantenname]-Ressourcen müssen Sie die Nutzungsbedingungen lesen.<br>**Neuer Text.** Für den Zugriff auf die [Mandantenname]-Ressource müssen Sie die Nutzungsbedingungen lesen.

**Aktueller Text:** Wenn Sie „Akzeptieren“ auswählen, bedeutet dies, dass Sie allen oben aufgeführten Nutzungsbedingungen zustimmen.<br>**Neuer Text:** Klicken Sie auf „Akzeptieren“, um zu bestätigen, dass Sie die Nutzungsbedingungen gelesen und verstanden haben.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Passthrough-Authentifizierung unterstützt ältere Protokolle und Anwendungen

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung
 
Passthrough-Authentifizierung unterstützt jetzt ältere Protokolle und Apps. Folgende Einschränkungen werden jetzt durch vollständige Unterstützung aufgehoben:

- Benutzeranmeldungen bei älteren Office-Clientanwendungen, Office 2010 und Office 2013, ohne moderne Authentifizierung.

- Zugriff auf Kalenderfreigabe und Frei-/Gebucht-Informationen in Exchange-Hybridumgebungen nur in Office 2010.

- Benutzeranmeldungen bei Skype for Business-Clientanwendungen ohne moderne Authentifizierung.

- Benutzeranmeldungen bei PowerShell Version 1.0.

- Das Apple Device Enrollment Program (Apple DEP. Programm zur Geräteregistrierung) mit dem iOS-Setup-Assistenten. 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Zusammengeführte Sicherheitsinformationenverwaltung für Self-Service-Kennwortzurücksetzung und Multi-Factor Authentication

**Typ:** Neue Funktion  
**Dienstkategorie:** SSPR  
**Produktfunktion**: Benutzerauthentifizierung

Diese neue Funktion ermöglicht Benutzern die Verwaltung ihrer Sicherheitsinformationen (z.B. Telefonnummer, E-Mail-Adresse, mobile App usw.) für die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) und Multi-Factor Authentication (MFA) in einer einzigen Benutzeroberfläche. Benutzer müssen nicht mehr die gleichen Sicherheitsinfos für SSPR und MFA in zwei unterschiedliche Benutzeroberflächen registrieren. Diese neue Benutzeroberfläche gilt auch für Benutzer, die entweder über SSPR oder MFA verfügen.

Wenn eine Organisation die MFA- oder SSPR-Registrierung nicht erzwingt, können Benutzer ihre Sicherheitsinformationen über das **Meine Apps**-Portal registrieren. Von dort aus können Benutzer mit beliebigen für MFA oder SSPR aktivierten Methoden registrieren. 

Dies ist eine abonnierbare Public Preview. Administratoren können die neue Oberfläche (falls gewünscht) für eine ausgewählte Gruppe von Benutzern oder alle Benutzer in einem Mandanten aktivieren.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Verwenden Sie die Microsoft Authenticator-App, um Ihre Identität zu bestätigen, wenn Sie Ihr Kennwort zurücksetzen.

**Typ:** Geänderte Funktion  
**Dienstkategorie:** SSPR  
**Produktfunktion**: Benutzerauthentifizierung

Mit diesem Feature können Benutzer, die keine Administratoren sind, ihre Identität bestätigen, während sie ein Kennwort mithilfe einer Benachrichtigung oder mittels Code aus der Microsoft Authenticator-App (oder einer beliebigen anderen Authenticator-App) zurücksetzen. Nachdem Administratoren diese Methode zur Self-Service-Kennwortzurücksetzung aktiviert haben, können Benutzer, die eine mobile App über „aka.ms/mfasetup“ oder „aka.ms/setupsecurityinfo“ registriert haben, ihre mobile App beim Zurücksetzen des Kennworts als Überprüfungsmethode verwenden.

Die Benachrichtigung über die mobile App kann nur als Teil einer Richtlinie aktiviert werden, die zwei Methoden zum Zurücksetzen Ihres Kennworts erfordert.

---

## <a name="june-2018"></a>Juni 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Änderungshinweis: Sicherheitsfix für den Flow für die delegierte Autorisierung für Apps, die die Azure AD-Aktivitätsprotokolle-API verwenden.

**Typ:** Plan für Änderung  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Aufgrund unserer strengeren Durchsetzung von Sicherheitsmaßnahmen mussten wir die Berechtigungen für Apps ändern, die für den Zugriff auf [Azure AD-Aktivitätsprotokolle-APIs](https://aka.ms/aadreportsapi) einen Flow für die delegierte Autorisierung verwenden. Diese Änderung erfolgt am **26. Juni 2018**.

Wenn eine Ihrer Apps Azure AD-Aktivitätsprotokolle-APIs verwendet, gehen Sie folgendermaßen vor, um sicherzustellen, dass die App nach Durchführung der Änderung noch betriebsfähig ist.

**So aktualisieren Sie Ihre App-Berechtigungen**

1. Melden Sie sich beim Azure-Portal an. Wählen Sie **Azure Active Directory** und dann **App-Registrierungen** aus.
2. Wählen Sie die App aus, die die Azure AD-Aktivitätsprotokolle-API nutzt. Wählen Sie dann **Einstellungen**, danach **Erforderliche Berechtigungen** und schließlich die API **Microsoft Azure Active Directory** aus.
3. Aktivieren Sie im Bereich **Delegierte Berechtigungen** des Blatts **Zugriff aktivieren** das Kontrollkästchen neben **Verzeichnisdaten lesen**, und wählen Sie dann **Speichern** aus.
4. Wählen Sie **Berechtigungen erteilen** und dann **Ja** aus.
    
    >[!Note]
    >Sie müssen globaler Administrator sein, um Berechtigungen für die App zu erteilen.

Weitere Informationen finden Sie im Abschnitt [Erteilen von Berechtigungen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) des Artikels „Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API“.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>Konfigurieren von TLS-Einstellungen zum Herstellen einer Verbindung mit Azure AD-Diensten zum Zweck der PCI-DSS-Konformität

**Typ:** Neue Funktion  
**Dienstkategorie:** Nicht verfügbar  
**Produktfunktion**: Plattform

Transport Layer Security (TLS) ist ein Protokoll, das Datenschutz und -integrität zwischen zwei kommunizierenden Anwendungen gewährleistet, und derzeit das am weitesten verbreitete Sicherheitsprotokoll.

Der [PCI Security Standards Council](https://www.pcisecuritystandards.org/) hat festgelegt, dass frühe Versionen von TLS und Secure Sockets Layer (SSL) deaktiviert werden müssen, um neue und sicherere Anwendungsprotokolle zu ermöglichen, wobei Konformität ab dem **30. Juni 2018** gegeben sein muss. Diese Änderung bedeutet, dass Sie TLS 1.0 deaktivieren müssen, wenn Sie eine Verbindung mit Azure AD-Diensten herstellen und PCI-DSS-Konformität benötigen. Mehrere TLS-Versionen sind verfügbar, aber TLS 1.2 ist die neueste für Azure Active Directory Services verfügbare Version. Es wird ausdrücklich empfohlen, für Client/Server- und Browser/Server-Kombinationen direkt zu TLS 1.2 zu wechseln.

Veraltete Browser unterstützen möglicherweise keine neueren TLS-Versionen wie z.B. TLS 1.2. Um festzustellen, welche Versionen von TLS von Ihrem Browser unterstützt werden, besuchen Sie die Website [Qualys SSL Labs](https://www.ssllabs.com/), und klicken Sie auf **Test your browser**. Sie sollten auf die neueste Version Ihres Webbrowsers aktualisieren und vorzugsweise nur TLS 1.2 aktivieren.

**Aktivieren von TLS 1.2 je nach Browser**

- **Microsoft Edge und Internet Explorer (beide werden mit Internet Explorer festgelegt)**

    1. Öffnen Sie Internet Explorer, und wählen Sie **Tools** > **Internetoptionen** > **Erweitert** aus.
    2. Wählen Sie im Bereich **Sicherheit** die Option **TLS 1.2 verwenden** aus, und klicken Sie dann auf **OK**.
    3. Schließen Sie alle Browserfenster, und starten Sie Internet Explorer neu. 

- **Google Chrome**

    1. Öffnen Sie Google Chrome. Geben Sie in die Adressleiste *chrome://settings/* ein, und drücken Sie die **EINGABETASTE**.
    2. Blenden Sie die Optionen unter **Erweitert** ein. Wechseln Sie zum Abschnitt **System**, und klicken Sie auf **Proxy-Einstellungen öffnen**.
    3. Wählen Sie im Feld **Interneteigenschaften** die Registerkarte **Erweitert** aus. Wechseln Sie zum Abschnitt **Sicherheit**, wählen Sie **TLS 1.2 verwenden** aus, und klicken Sie dann auf  **OK**.
    4. Schließen Sie alle Browserfenster, und starten Sie Google Chrome neu.

- **Mozilla Firefox**

    1. Öffnen Sie Firefox. Geben Sie in die Adressleiste *about:config* ein, und drücken Sie dann die **EINGABETASTE**.
    2. Suchen Sie nach dem Begriff *TLS*, und wählen Sie dann den Eintrag **security.tls.version.max** aus.
    3. Legen Sie den Wert auf **3** fest, um den Browser zu zwingen, Versionen bis TLS 1.2 zu verwenden, und klicken Sie dann auf **OK**.

        >[!NOTE]
        >Die Firefox-Version 60.0 unterstützt TLS 1.3, sodass Sie den Wert „security.tls.version.max“ auch auf **4** festlegen können.

    4. Schließen Sie alle Browserfenster, und starten Sie Mozilla Firefox neu.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Juni 2018

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Im Juni 2018 haben wir diese 15 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [Settling music](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [Branchenspezifische App mit Unterstützung von SAML 1.1-Token](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Endpoint Backup](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh Networks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [lokales SharePoint](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [ForeSee CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Kennwortschutz für Azure AD in Public Preview verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: Identitätsschutz  
**Produktfunktion**: Benutzerauthentifizierung

Nutzen Sie den Kennwortschutz für Azure AD, um leicht zu erratende Kennwörter aus Ihrer Umgebung zu verbannen. Das Verbannen dieser Kennwörter trägt dazu bei, das Risiko einer Gefährdung durch einen Kennwort-Spray-Angriff zu verringern.

Der Kennwortschutz für Azure AD hilft insbesondere bei folgenden Aufgaben:

- Schützen der Konten Ihrer Organisation in sowohl Azure AD als auch Windows Server Active Directory (AD). 
- Verhindern, dass Ihre Benutzer Kennwörter in einer Liste der mehr als 500 am häufigsten verwendeten Kennwörter und über 1 Mio. Zeichenersetzungsvarianten dieser Kennwörter verwenden.
- Verwalten des Kennwortschutzes für Azure AD an zentraler Stelle im Azure AD-Portal, sowohl für Azure AD als auch für lokales Windows Server AD.

Weitere Informationen zum Kennwortschutz für Azure AD finden Sie unter [Beseitigen falscher Kennwörter in Ihrer Organisation](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Neue Vorlage für eine Richtlinie für bedingten Zugriff namens „Alle Gäste“, die während der Erstellung der Nutzungsbedingungen erstellt wird

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion**: Governance

Während der Erstellung der Nutzungsbedingungen wird eine neue Vorlage für eine Richtlinie für bedingten Zugriff auch für „Alle Gäste“ und „Alle Apps“ erstellt. Diese neue Richtlinienvorlage wendet die neu erstellten Nutzungsbedingungen an und optimiert den Erstellungs- und Erzwingungsprozess für Gäste.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Neue „benutzerdefinierte“ Vorlage für eine Richtlinie für bedingten Zugriff, die während der Erstellung der Nutzungsbedingungen erstellt wird

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion**: Governance

Während der Erstellung der Nutzungsbedingungen wird auch eine neue „benutzerdefinierte“ Vorlage für eine Richtlinie für bedingten Zugriff erstellt. Mit dieser neuen Richtlinienvorlage können Sie die Nutzungsbedingungen erstellen und dann sofort zum Blatt für die Erstellung von Richtlinien mit bedingtem Zugriff wechseln, ohne manuell durch das Portal navigieren zu müssen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Neue und umfassende Anleitung zum Bereitstellen von Azure Multi-Factor Authentication

**Typ:** Neue Funktion  
**Dienstkategorie:** Sonstige  
**Produktfunktion**: Identitätssicherheit und -schutz
 
Wir haben eine neue detaillierte Anleitung zum Bereitstellen von Azure Multi-Factor Authentication (MFA) in Ihrer Organisation veröffentlicht.

Die MFA-Bereitstellungsanleitung finden Sie auf GitHub im Repository [Identity Deployment Guides](https://aka.ms/DeploymentPlans). Um Feedback zu den Bereitstellungsanleitungen zu geben, verwenden Sie das Formular [Deployment Plan Feedback](https://aka.ms/deploymentplanfeedback). Bei Fragen zu den Bereitstellungsanleitungen wenden Sie sich unter [IDGitDeploy](mailto:idgitdeploy@microsoft.com) an uns.

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Delegierte Azure AD-App-Verwaltungsrollen in Public Preview

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Zugriffssteuerung

Administratoren können nun App-Verwaltungsaufgaben delegieren, ohne die Rolle „Globaler Administrator“ zuzuweisen. Es gibt die folgenden neuen Rollen und Funktionen:

- **Neue Azure AD-Standardadministratorrollen:**

    - **Anwendungsadministrator.** Erteilt die Berechtigung, alle Aspekte aller Apps zu verwalten, einschließlich Registrierung, Einstellungen für einmaliges Anmelden (SSO), Zuweisung und Lizenzierung von Apps, Proxyeinstellungen für Apps und Zustimmung (mit Ausnahme von Azure AD-Ressourcen).

    - **Cloudanwendungsadministrator.** Erteilt alle Berechtigungen des Anwendungsadministrators außer für den App-Proxy, da dieser keinen lokalen Zugriff bietet.

    - **Anwendungsentwickler.** Erteilt die Berechtigung zum Erstellen von App-Registrierungen, auch wenn die Option **Benutzer die Registrierung von Apps erlauben** deaktiviert ist.

- **Besitz (wird vergleichbar mit dem Gruppenbesitzprozess je App-Registrierung und Unternehmens-App eingerichtet):**
 
    - **App-Registrierungsbesitzer.** Erteilt die Berechtigung, alle Aspekte der eigenen App-Registrierung zu verwalten, einschließlich App-Manifest und Hinzufügung weiterer Besitzer.

    - **Unternehmens-App-Besitzer.** Erteilt die Berechtigung, zahlreiche Aspekte eigener Unternehmens-Apps zu verwalten, einschließlich Einstellungen für einmaliges Anmelden (SSO), App-Zuweisungen und Zustimmung (mit Ausnahme von Azure AD-Ressourcen).

Weitere Informationen zur Public Preview finden Sie im Blogbeitrag [Azure AD delegated application management roles are in public preview! (Delegierte Azure AD-App-Verwaltungsrollen in Public Preview)](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) . Weitere Informationen zu Rollen und Berechtigungen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Mai 2018

### <a name="expressroute-support-changes"></a>Änderungen der Unterstützung für ExpressRoute

**Typ:** Plan für Änderung  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Plattform  

„Software-as-a-Service“-Angebote wie Azure Active Directory (Azure AD) sind so konzipiert, dass sie direkt über das Internet – ohne ExpressRoute oder sonstige private VPN-Tunnel – am besten funktionieren. Aus diesem Grund beenden wir am **1. August 2018** die Unterstützung für Dienste für ExpressRoute für Azure AD, die öffentliches Azure-Peering und Azure-Communitys in Microsoft-Peering nutzen. Bei allen Diensten, die von dieser Änderung betroffen sind, ist möglicherweise die allmähliche Verschiebung des Azure AD-Datenverkehrs von ExpressRoute zum Internet spürbar.

Während wir unsere Unterstützung ändern, wissen wir allerdings auch, dass es noch Situationen gibt, in denen Sie möglicherweise eine dedizierte Gruppe von Leitungen für Ihren Authentifizierungsdatenverkehr verwenden müssen. Aus diesem Grund wird Azure AD weiterhin Einschränkungen des IP-Adressbereichs pro Mandant mithilfe von ExpressRoute und Diensten, die bereits im Microsoft-Peering enthalten sind, mit der Community „Other Office 365 Online services“ unterstützen. Wenn Ihre Dienste betroffen sind, Sie jedoch ExpressRoute benötigen, müssen Sie die folgenden Schritte ausführen:

- **Wenn Sie sich im öffentlichen Azure-Peering befinden.** Wechseln Sie zu Microsoft-Peering, und registrieren Sie sich für die Community **Other Office 365 Online services (12076:5100)**. Weitere Informationen über den Wechsel vom öffentlichen Azure-Peering zum Microsoft-Peering finden Sie im Artikel [Umstellen von öffentlichem Peering auf Microsoft-Peering](https://docs.microsoft.com/azure/expressroute/how-to-move-peering).

- **Wenn Sie sich im Microsoft-Peering befinden.** Registrieren Sie sich für die Community **Other Office 365 Online service (12076:5100)**. Weitere Informationen zu Routinganforderungen finden Sie im Artikel über ExpressRoute-Routinganforderungen im Abschnitt [Unterstützung für BGP-Communitys](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp).

Wenn Sie weiterhin dedizierte Leitungen verwenden müssen, müssen Sie mit Ihrem Microsoft-Kontoteam erörtern, wie Sie die Autorisierung für die Nutzung der Community **Other Office 365 Online service (12076:5100)** erhalten. Das von MS Office verwaltete Prüfungsgremium überprüft, ob Sie diese Leitungen benötigen, und stellt sicher, dass Sie verstehen, welche technischen Auswirkungen damit verbunden sind, sie beizubehalten. Nicht autorisierte Abonnements, die versuchen, Weiterleitungsfilter für Office 365 zu erstellen, erhalten eine Fehlermeldung. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph-APIs zum Verwalten der Nutzungsbedingungen

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion**: Entwickleroberfläche
 
Wir haben Microsoft Graph-APIs für die Verwaltung der Azure AD-Nutzungsbedingungen hinzugefügt. Sie können das Objekt mit den Nutzungsbedingungen erstellen, aktualisieren und löschen.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Möglichkeit zum Hinzufügen des mehrinstanzenfähigen Azure AD-Endpunkts als Identitätsanbieter in Azure AD B2C

**Typ:** Neue Funktion  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion**: B2B/B2C
 
Mit benutzerdefinierten Richtlinien können Sie jetzt den gemeinsamen Endpunkt für Azure AD als Identitätsanbieter in Azure AD B2C hinzufügen. Auf diese Weise können Sie einen zentralen Zugangspunkt (Single Point of Entry) für alle Azure AD-Benutzer verwenden, die sich an Ihren Anwendungen anmelden. Weitere Informationen finden Sie unter [Azure Active Directory B2C: Benutzern mithilfe von benutzerdefinierten Richtlinien die Anmeldung bei einem mehrinstanzenfähigen Azure AD-Identitätsanbieter ermöglichen](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Verwendung interner URLs für einen standortunabhängigen App-Zugriff – dank Erweiterung zur sicheren Anmeldung bei „Meine Apps“ und Azure AD-Anwendungsproxy

**Typ:** Neue Funktion  
**Dienstkategorie**: Meine Apps  
**Produktfunktion**: SSO
 
Benutzer können über interne URLs jetzt auch dann auf Anwendungen zugreifen, wenn sie sich außerhalb Ihres Unternehmensnetzwerks befinden, indem sie die Erweiterung zur sicheren Anmeldung bei „Meine Apps“ verwenden. Dies funktioniert mit allen Anwendungen, die Sie per Azure AD-Anwendungsproxy veröffentlicht haben, und mit jedem Browser, für den die Browsererweiterung für den Zugriffsbereich installiert ist. Die Funktion für die URL-Umleitung wird automatisch aktiviert, nachdem sich ein Benutzer an der Erweiterung angemeldet hat. Die Erweiterung ist für [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367) und [Firefox](https://go.microsoft.com/fwlink/?linkid=866366) als Download verfügbar.

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory – Datenspeicherung in Europa für europäische Kunden

**Typ:** Neue Funktion  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** GoLocal

Die Daten von Kunden in Europa müssen in Europa bleiben und dürfen nicht außerhalb von europäischen Rechenzentren repliziert werden. Dies ist erforderlich, um den Datenschutz zu gewährleisten und EU-Gesetze einzuhalten. [Dieser Artikel](https://go.microsoft.com/fwlink/?linkid=872328) enthält spezifische Details dazu, welche Identitätsinformationen in Europa gespeichert werden, und außerdem Details zu den Informationen, die außerhalb von europäischen Datencentern gespeichert werden. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Neue Integrationen zur Benutzerbereitstellung in SaaS-Apps – Mai 2018

**Typ:** Neue Funktion  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Azure AD ermöglicht Ihnen das automatisierte Erstellen, Warten und Entfernen von Benutzeridentitäten in SaaS-Anwendungen, z.B. Dropbox, Salesforce, ServiceNow und anderen. Für Mai 2018 haben wir für die folgenden Anwendungen im Azure AD-App-Katalog Unterstützung für die Benutzerbereitstellung hinzugefügt:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Cornerstone OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Eine Liste mit allen Anwendungen, für die die Benutzerbereitstellung im Azure AD-Katalog unterstützt wird, finden Sie unter [https://aka.ms/appstutorial](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Möglichkeit zur regelmäßigen Durchführung von Azure AD-Zugriffsüberprüfungen für Gruppen- und App-Zugriff

**Typ:** Neue Funktion  
**Dienstkategorie**: Zugriffsüberprüfungen  
**Produktfunktion**: Governance
 
Die Zugriffsüberprüfung von Gruppen und Apps ist für Azure AD Premium P2 jetzt allgemein verfügbar.  Administratoren können die Zugriffsüberprüfungen von Gruppenmitgliedschaften und Anwendungszuordnungen so konfigurieren, dass sie automatisch in bestimmten Intervallen durchgeführt werden, z.B. monatlich oder einmal pro Quartal.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD-Aktivitätsprotokolle (Anmeldevorgänge und Überwachung) jetzt über MS Graph verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Azure AD-Aktivitätsprotokolle, die auch Anmeldungen und Überwachungsprotokolle umfassen, sind jetzt über MS Graph verfügbar. Wir haben zwei Endpunkte über MS Graph verfügbar gemacht, um auf diese Protokolle zuzugreifen. Sehen Sie sich zum Einstieg unsere [Dokumente](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) zum programmgesteuerten Zugriff auf Azure AD-Berichterstellungs-APIs an. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>Verbesserungen an der B2B-Oberfläche für die Einlösung und einfacheres Verlassen einer Organisation

**Typ:** Neue Funktion  
**Dienstkategorie:** B2B  
**Produktfunktion**: B2B/B2C

**Just-in-Time-Einlösung:** Nachdem Sie eine Ressource für einen Gastbenutzer per B2B-API freigegeben haben, müssen Sie keine spezielle Einladungs-E-Mail mehr senden. In den meisten Fällen kann der Gastbenutzer auf die Ressource zugreifen, und der Einlösevorgang wird „just in time“ durchgeführt. Es entstehen keine Probleme aufgrund von verloren gegangenen E-Mails mehr. Sie müssen Ihren Gastbenutzern nicht mehr die folgende Frage stellen: „Haben Sie auf den vom System gesendeten Link für die Einlösung geklickt?“ Sobald SPO den Einladungs-Manager verwendet, können Anhänge aus der Cloud für alle Benutzer dieselbe kanonische URL verwenden – sowohl intern als auch extern und in jedem Stadium der Einlösung.

**Moderne Oberfläche für die Einlösung:** Es gibt keine Startseite mit geteiltem Bildschirm mehr. Benutzer sehen eine moderne Oberfläche für die Zustimmung, auf der die Datenschutzerklärung der einladenden Organisation angezeigt wird – genau wie bei Drittanbieter-Apps.

**Gastbenutzer können die Organisation verlassen:** Sobald die Beziehung eines Benutzers zu einer Organisation beendet ist, kann dieser die Organisation selbständig verlassen. Anrufe beim Administrator der einladenden Organisation zum Entfernen des Benutzers entfallen, ebenso wie das Erstellen von Supporttickets.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Mai 2018

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Im Mai 2018 haben wir diese 18 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty Govern, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate Prime](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial), [まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [Arc Publishing - SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Neue detaillierte Bereitstellungsanleitungen für Azure Active Directory

**Typ:** Neue Funktion  
**Dienstkategorie:** Sonstige  
**Produktfunktion**: Verzeichnis
 
Neue detaillierte Anleitungen für die Bereitstellung von Azure Active Directory (Azure AD), einschließlich Self-Service-Kennwortzurücksetzung, einmaliges Anmelden, bedingter Zugriff, App-Proxy, Benutzerbereitstellung, Active Directory-Verbunddienste (AD FS) zu Pass-Through-Authentifizierung und AD FS zu Kennworthashsynchronisierung.

Die Bereitstellungsanleitungen finden Sie auf GitHub im Repository [Identity Deployment Guides](https://aka.ms/DeploymentPlans). Um Feedback zu den Bereitstellungsanleitungen zu geben, verwenden Sie das Formular [Deployment Plan Feedback](https://aka.ms/deploymentplanfeedback). Bei Fragen zu den Bereitstellungsanleitungen wenden Sie sich unter [IDGitDeploy](mailto:idgitdeploy@microsoft.com) an uns.

---

### <a name="enterprise-applications-search---load-more-apps"></a>Suche nach Unternehmensanwendungen – Laden weiterer Apps

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: SSO
 
Haben Sie Probleme, Ihre Anwendungen bzw. Dienstprinzipale zu finden? Wir haben eine Funktion hinzugefügt, um mehr Anwendungen in Ihre Liste der Unternehmensanwendungen zu laden. Standardmäßig werden 20 Anwendungen angezeigt. Sie können ab sofort auf **Weitere laden** klicken, um zusätzliche Anwendungen anzuzeigen. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>Die im Mai veröffentlichte Version von AADConnect umfasst eine öffentliche Vorschau der PingFederate-Integration, wichtige Sicherheitsupdates, zahlreiche Fehlerkorrekturen und neue großartige Tools für die Problembehandlung. 

**Typ:** Geänderte Funktion  
**Dienstkategorie**: AD Connect  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus
 
Die im Mai veröffentlichte Version von AADConnect umfasst eine öffentliche Vorschau der PingFederate-Integration, wichtige Sicherheitsupdates, zahlreiche Fehlerkorrekturen und neue großartige Tools für die Problembehandlung. Die Versionshinweise finden Sie [hier](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD-Zugriffsüberprüfungen: automatische Anwendung

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Zugriffsüberprüfungen  
**Produktfunktion**: Governance

Zugriffsüberprüfungen von Gruppen und Apps sind für Azure AD Premium P2 jetzt allgemein verfügbar. Ein Administrator kann konfigurieren, dass die Änderungen des Prüfers automatisch auf diese Gruppe oder App angewendet werden, sobald die Zugriffsüberprüfung abgeschlossen wurde. Der Administrator kann auch festlegen, was mit dem fortgesetzten Zugriff des Benutzers geschieht, wenn Prüfer nicht antworten. Mögliche Optionen hierbei sind: Zugriff entfernen, Zugriff beibehalten oder Systemempfehlungen annehmen. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-responsemode-for-new-apps"></a>Für neue Apps können ID-Token nicht mehr über den response_mode „query“ zurückgegeben werden. 

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung
 
Ab dem 25.04.2018 erstellte Apps sind nicht mehr in der Lage, ein **id_token** über den response_mode **query** anzufordern.  Damit entspricht Azure AD den OIDC-Spezifikationen und trägt dazu bei, die Angriffsfläche Ihrer Apps zu verringern.  Vor dem 25.04.2018 erstellte Apps werden nicht daran gehindert, den response_mode **query** mit dem response_typ **id_token** zu verwenden.  Der zurückgegebene Fehler beim Anfordern eines id_token aus AAD lautet: **AADSTS70007: „query“ ist beim Anfordern eines Tokens kein unterstützter Wert von „response_mode“**.

Die response_mode-Werte **fragment** und **form_post** funktionieren weiterhin. Vergewissern Sie sich beim Erstellen neuer Anwendungsobjekte (z.B. für die App-Proxyverwendung) vor dem Erstellen einer neuen Anwendung, dass einer dieser response_mode-Werte verwendet wird.  

---