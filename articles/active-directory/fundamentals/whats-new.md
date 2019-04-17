---
title: Neuerungen Versionshinweise – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: active-directory
author: eross-msft
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5077f09cba3548b03045011a7b8f16844156de55
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757641"
---
# <a name="whats-new-in-azure-active-directory"></a>Neuerungen in Azure Active Directory

>Lassen Sie sich über Updates auf dieser Seite benachrichtigen, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` kopieren und in Ihren ![RSS-Symbol](./media/whats-new/feed-icon-16x16.png)-Feedreader einfügen.

Es werden fortlaufend Verbesserungen an Azure AD vorgenommen. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Fehlerbehebungen
- Veraltete Funktionen
- Pläne für Änderungen

Besuchen Sie regelmäßig diese Seite. Diese wird monatlich aktualisiert. Wenn Sie nach Elementen suchen, die älter als sechs Monate sind, können Sie sie im [Archiv für Neuheiten in Azure Active Directory](whats-new-archive.md) finden.

---

## <a name="march-2019"></a>März 2019

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Unterstützung von Identity Experience Framework und benutzerdefinierter Richtlinie ist jetzt in Azure Active Directory B2C verfügbar (GA)

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Sie können jetzt in Azure AD B2C benutzerdefinierte Richtlinien inklusive der folgenden Aufgaben erstellen, die bedarfsorientiert und im Rahmen unserer Azure-SLA unterstützt werden:

- Erstellen und Hochladen benutzerdefinierter User Journeys für die Authentifizierung mit benutzerdefinierten Richtlinien.

- Beschreiben von User Journeys als Austauschvorgänge zwischen Anspruchsanbietern (Schritt für Schritt)

- Definieren der bedingten Verzweigung in User Journeys

- Transformieren und Zuordnen von Ansprüchen für die Verwendung in Echtzeitentscheidungen und -kommunikation.

- Verwenden REST-API-fähiger Dienste in Ihren benutzerdefinierten User Journeys für die Authentifizierung. Beispielsweise mit E-Mail-Anbietern, CRMs und proprietären Autorisierungssystemen.

- Einrichten von Verbünden mit Identitätsanbietern, die mit dem OpenIDConnect-Protokoll konform sind. Beispielsweise mit mehrinstanzenfähigem Azure AD, Anbietern von sozialen Konten oder Anbietern der zweistufigen Authentifizierung.

Weitere Informationen zum Erstellen benutzerdefinierter Richtlinien finden Sie unter [Entwicklerhinweise zu benutzerdefinierten Richtlinien in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-developer-notes-custom), und lesen Sie den [Blogbeitrag von Alex Simons mit den Fallstudien](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-custom-policies-to-build-your-own-identity-journeys/ba-p/382791).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – März 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im März 2019 haben wir diese 14 neuen Apps mit Verbundunterstützung in den App-Katalog aufgenommen:

[ISEC7 Mobile Exchange Delegate](https://www.isec7.com/english/), [MediusFlow](https://office365.cloudapp.mediusflow.com/), [ePlatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [Fulcrum](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [ExcelityGlobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Explanation-Based Auditing System](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [Lean](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [Powerschool Performance Matters](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [Cinode](https://cinode.com/), [Iris Intranet](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [Empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [TAS](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Neue Zscaler- und Atlassian-Bereitstellungsconnectors im Azure AD-Katalog – März 2019

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten

Automatisieren Sie das Erstellen, Aktualisieren und Löschen von Benutzerkonten für die folgenden Apps:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Beta](http://aka.ms/ZscalerBetaProvisioning), [Zscaler One](https://aka.ms/ZscalerOneProvisioning), [Zscaler Two](http://aka.ms/ZscalerTwoProvisioning), [Zscaler Three](http://aka.ms/ZscalerThreeProvisioning), [Zscaler ZSCloud](http://aka.ms/ZscalerZSCloudProvisioning), [Atlassian Cloud](http://aka.ms/atlassianCloudProvisioning)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser sichern können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](http://aka.ms/ProvisioningDocumentation).

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>Wiederherstellen und Verwalten Ihrer gelöschten Office 365-Gruppen im Azure AD-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Sie können Ihre gelöschten Office 365-Gruppen jetzt vom Azure AD-Portal aus anzeigen und verwalten. Diese Änderung erleichtert Ihnen zu sehen, welche Gruppen wiederhergestellt werden können, und ermöglicht Ihnen gleichzeitig, endgültig alle Gruppen zu löschen, die Ihre Organisation nicht mehr benötigt.

Weitere Informationen finden Sie unter [Anzeigen und Verwalten der wiederherstellbaren gelöschten Office 365-Gruppen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore).

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>Einmaliges Anmelden steht jetzt für Azure AD-SAML-gesicherte lokale Apps über den Anwendungsproxy zur Verfügung (Public Preview).

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Sie können nun eine Benutzeroberfläche für einmaliges Anmelden (SSO) für lokale, SAML-authentifizierte Apps zusammen mit Remotezugriff auf diese Apps über den Anwendungsproxy bereitstellen. Weitere Informationen zum Einrichten von SAML-SSO mit Ihren lokalen Apps finden Sie unter [SAML-SSO (einmaliges Anmelden) für lokale Anwendungen mit dem Anwendungsproxy (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps).

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>Client-Apps in Anforderungsschleifen werden unterbrochen, um Zuverlässigkeit und Benutzerfreundlichkeit zu verbessern.

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Client-Apps können in einem kurzen Zeitraum Hunderte der gleichen Anmeldeanforderungen falsch ausgeben. Alle diese Anforderungen, ob erfolgreich oder nicht, tragen zu einer schlechten Benutzererfahrung und erhöhten Workloads für den IDP bei, was zu einer höheren Latenz für alle Benutzer und einer geringeren Verfügbarkeit des IDP führt.

Dieses Update sendet eine `invalid_grant`-Fehlermeldung: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request` an Client-Apps, die mehrmals über den Rahmen des normalen Betriebs hinausgehend in einem kurzen Zeitraum doppelte Anforderungen ausgeben. Client-Apps, bei denen dieses Problem auftritt, sollten eine interaktive Eingabeaufforderung anzeigen, die den Benutzer zur erneuten Anmeldung auffordert. Weitere Informationen zu dieser Änderung und zur Korrektur Ihrer App bei Auftreten dieses Fehlers finden Sie unter [In der Schleife befindliche Clients werden unterbrochen](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted).

---

### <a name="new-audit-logs-user-experience-now-available"></a>Neue Benutzeroberfläche für Überwachungsprotokolle jetzt verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir haben eine neue Azure AD-**Überwachungsprotokolle**-Seite erstellt, um die Lesbarkeit zu verbessern und die Art, in der Sie Ihre Informationen suchen. Um die neue **Überwachungsprotokolle**-Seite anzuzeigen, wählen Sie **Überwachungsprotokolle** im Abschnitt **Aktivität** von Azure AD aus.

![Neue Seite „Überwachungsprotokolle“ mit Beispielinformationen](media/whats-new/audit-logs-page.png)

Weitere Informationen über die neue Seite **Überwachungsprotokolle** finden Sie unter [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs).

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>Neue Warnungen und Anleitungen, um das versehentliche Aussperren des Administrators aufgrund falsch konfigurierter Richtlinien für den bedingten Zugriff zu verhindern

**Typ:** Geändertes Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Um zu verhindern, dass Administratoren sich versehentlich selbst von ihren eigenen Mandanten aus durch falsch konfigurierte Richtlinien für bedingten Zugriff aussperren, haben wir neue Warnungen und aktualisierte Anleitungen im Azure-Portal erstellt. Weitere Informationen zu den neuen Anleitungen finden Sie im Artikel [Was sind Dienstabhängigkeiten beim bedingten Azure Active Directory-Zugriff?](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies).

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Verbesserte Benutzeroberfläche zur Anzeige von Nutzungsbedingungen für Endbenutzer auf mobilen Geräten

**Typ:** Geändertes Feature  
**Dienstkategorie:** Terms of Use (Nutzungsbedingungen)  
**Produktfunktion:** Governance

Wir haben unsere vorhandene Benutzeroberfläche zur Anzeige von Nutzungsbedingungen aktualisiert, um Ihnen auf einem mobilen Gerät zu erleichtern, Nutzungsbedingungen zu überprüfen und ihnen zuzustimmen. Sie können jetzt vergrößern und verkleinern, zurückgehen, Informationen herunterladen und Links auswählen. Weitere Informationen zu den aktualisierten Nutzungsbedingungen finden Sie unter [Anzeige von Nutzungsbedingungen für Benutzer](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#what-terms-of-use-looks-like-for-users).

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>Neue Benutzeroberfläche zum Herunterladen von Azure AD-Aktivitätsprotokollen verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Sie können große Mengen von Aktivitätsprotokollen jetzt direkt über das Azure-Portal herunterladen. Dieses Update ermöglicht Ihnen Folgendes:

- Herunterladen von bis zu 250.000 Zeilen.

- Erhalten einer Benachrichtigung bei Abschluss des Downloads.

- Anpassen Ihres Dateinamens.

- Bestimmen Ihres Ausgabeformats, entweder JSON oder CSV.

Ausführlichere Informationen zu diesem Feature finden Sie unter [Schnellstart: Herunterladen eines Überwachungsberichts über das Azure-Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report).

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Wichtige Änderung: Updates für die Bedingungsauswertung bei Exchange ActiveSync (EAS)

**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Zugriffssteuerung

Wir aktualisieren momentan die Art, in der Exchange ActiveSync (EAS) die folgenden Bedingungen auswertet:

- Standort des Benutzers basierend auf Land, Region oder IP-Adresse

- Anmelderisiko

- Geräteplattform

Wenn Sie diese Bedingungen zuvor in Ihren Richtlinien für bedingten Zugriff verwendet haben, denken Sie daran, dass das Bedingungsverhalten sich ändern könnte. Wenn Sie z.B. zuvor die Benutzerstandortbedingung in einer Richtlinie verwendet haben, stellen Sie möglicherweise fest, dass die Richtlinie jetzt je nach Standort des Benutzers übersprungen wird.

---

## <a name="february-2019"></a>Februar 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Konfigurierbare Azure AD SAML-Tokenverschlüsselung (öffentliche Vorschau) 

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Sie können jetzt jede unterstützte SAML-App so konfigurieren, dass sie verschlüsselte SAML-Token erhält. Bei Konfiguration und Verwendung mit einer App verschlüsselt Azure AD die ausgegebenen SAML-Assertionen mit einem öffentlichen Schlüssel, der von einem in Azure AD gespeicherten Zertifikat bereitgestellt wird.

Weitere Informationen zum Konfigurieren Ihrer SAML-Tokenverschlüsselung finden Sie unter [Konfigurieren der Azure AD-SAML-Tokenverschlüsselung](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Erstellen einer Zugriffsüberprüfung für Gruppen oder Apps mit Azure AD-Zugriffsüberprüfungen

**Typ:** Neues Feature  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Governance

Sie können jetzt mehrere Gruppen oder Apps in eine einzige Azure AD-Zugriffsüberprüfung für die Gruppenmitgliedschaft oder App-Zuweisung einschließen. Zugriffsüberprüfungen mit mehreren Gruppen oder Apps werden mit denselben Einstellungen eingerichtet, und alle einbezogenen Prüfer werden gleichzeitig benachrichtigt.

Weitere Informationen zum Erstellen einer Zugriffsüberprüfung mithilfe von Azure AD-Zugriffsüberprüfungen finden Sie unter [Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/governance/create-access-review).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Februar 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Februar 2019 haben wir diese 27 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [MindTickle](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT FINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [AirStack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [Skyward Qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Brightidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [AlertOps](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight-CloudGate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), Permission Click, [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [StoregateSmartFile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [Pexip](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [Seismic](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [Share A Dream](https://www.shareadream.org/how-it-works), [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [webMethods Integration Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [Knowledge Anywhere LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope Data](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [Netop Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [PureCloud by Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [ClickUp Productivity Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="enhanced-combined-mfasspr-registration"></a>Erweiterte kombinierte MFA-/SSPR-Registrierung

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Benutzerauthentifizierung
 
Als Reaktion auf Kundenfeedback haben wir die Vorschauoberfläche für die kombinierte MFA-/SSPR-Registrierung verbessert, damit Ihre Benutzer ihre Sicherheitsinformationen für MFA und SSPR schneller registrieren können. 

**Gehen Sie wie folgt vor, um die verbesserte Oberfläche sofort für Ihre Benutzer zu aktvieren:**

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim Azure-Portal an, und wechseln Sie zu **Azure Active Directory > Benutzereinstellungen > Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**. 

2. Wählen Sie unter der Option **Benutzer können Vorschaufunktionen zum Registrieren und Verwalten von Sicherheitsinformationen verwenden (Aktualisierung)** aus, ob Sie die Features für eine **ausgewählte Gruppe von Benutzern** oder für **alle Benutzer** aktivieren möchten.

Im Laufe der nächsten Wochen entfernen wir die Möglichkeit, die alte Vorschauoberfläche für die kombinierte MFA-/SSPR-Registrierung für Mandanten zu aktivieren, für die sie noch nicht aktiviert wurde.

**Gehen Sie wie folgt vor, um festzustellen, ob das Steuerelement für Ihren Mandanten entfernt wird:**

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim Azure-Portal an, und wechseln Sie zu **Azure Active Directory > Benutzereinstellungen > Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**.  

2.  Wenn die Option **Benutzer können Vorschaufunktionen zum Registrieren und Verwalten von Sicherheitsinformationen verwenden** auf **Kein** festgelegt ist, wird die Option von Ihrem Mandanten entfernt.

Unabhängig davon, ob Sie zuvor die alte Vorschauoberfläche für die kombinierte MFA-/SSPR-Registrierung für Benutzer aktiviert haben, wird die alte Benutzeroberfläche zu einem späteren Zeitpunkt deaktiviert. Aus diesem Grund raten wir Ihnen, so schnell wie möglich zur neuen erweiterten Oberfläche zu wechseln.

Weitere Informationen zur erweiterte Registrierungsoberfläche finden Sie unter [Cool enhancements to the Azure AD combined MFA and password reset registration experience](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271) (Verbesserungen an der Oberfläche für die kombinierte Oberfläche für die MFA-Registrierung und Kennwortzurücksetzung).

---

### <a name="updated-policy-management-experience-for-user-flows"></a>Aktualisierte Richtlinienverwaltungsoberfläche für Benutzerflows

**Typ:** Geändertes Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Wir haben den Vorgang zur Richtlinienerstellung und -verwaltung für Benutzerflows (zuvor als integrierte Richtlinien bezeichnet) vereinfacht. Diese neue Oberfläche gilt jetzt standardmäßig für all Ihre Azure AD-Mandanten.

Über die Symbole „Lächeln“ und „Stirnrunzeln“ im Bereich **Feedback senden** im oberen Bereich des Portals können Sie zusätzliches Feedback und Vorschläge angeben.

Weitere Informationen zur neuen Richtlinienverwaltungsoberfläche finden Sie im Blog [Azure AD B2C now has JavaScript customization and many more new features](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) (Azure AAD B2C bietet jetzt JavaScript-Anpassung und viele weitere Features).

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Auswahl bestimmter von Azure AD B2C bereitgestellter Seitenelementversionen

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Jetzt können Sie eine bestimmte Version der von Azure AD B2C bereitgestellten Seitenelemente auswählen. Wenn Sie eine bestimmte Version auswählen, können Sie Ihre Updates testen, bevor sie auf einer Seite angezeigt werden, und Sie können vorhersagbares Verhalten erzielen. Darüber hinaus können Sie jetzt auswählen, dass bestimmte Seitenversionen erzwungen werden, um JavaScript-Anpassungen zuzulassen. Um dieses Feature zu aktivieren, wechseln Sie in Ihren Benutzerflows zur Seite **Eigenschaften**.

Weitere Informationen zur Auswahl bestimmter Versionen von Seitenelementen finden Sie im Blog [Azure AD B2C now has JavaScript customization and many more new features](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) (Azure AAD B2C bietet jetzt JavaScript-Anpassung und viele weitere Features).

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>Konfigurierbare Endbenutzerkennwortanforderungen für B2C (GA)

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Jetzt können Sie die Kennwortkomplexität Ihrer Organisation für Ihre Endbenutzer einrichten, statt die native Azure AD-Kennwortrichtlinie verwenden zu müssen. Auf dem Blatt **Eigenschaften** Ihrer Benutzerflows (zuvor als integrierte Richtlinien bezeichnet) können Sie die Kennwortkomplexität **Einfach** oder **Stark** auswählen oder **benutzerdefinierte** Anforderungen erstellen.

Weitere Informationen zur Konfiguration von Anforderungen für die Kennwortkomplexität finden Sie unter [Konfigurieren der Komplexitätsanforderungen für Kennwörter in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Neue Standardvorlagen für benutzerdefinierte mit der Marke versehene Authentifizierungsoberflächen

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Mit unseren neuen Standardvorlagen auf dem Blatt **Seitenlayouts** Ihrer Benutzerflows (zuvor als integrierte Richtlinien bezeichnet) können Sie einen benutzerdefinierten mit Ihrer Marke versehenen Authentifizierungsvorgang für Ihre Benutzer erstellen.

Weitere Informationen zur Verwendung der Vorlagen finden Sie unter [Azure AD B2C now has JavaScript customization and many more new features](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) (Azure AAD B2C bietet jetzt JavaScript-Anpassung und viele weitere Features).

---

## <a name="january-2019"></a>Januar 2019

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>Active Directory B2B-Zusammenarbeit über Authentifizierung mit Einmalkennung (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C

Für B2B-Gastbenutzer, die sich auf andere Weise (z.B. über Azure AD, ein Microsoft-Konto (MSA) oder den Verbund mit Google) nicht authentifizieren können, haben wir die Authentifizierung mit Einmalkennung eingeführt. Diese neue Authentifizierungsmethode bedeutet, dass Gastbenutzer kein neues Microsoft-Konto erstellen müssen. Stattdessen kann ein Gastbenutzer beim Einlösen einer Einladung oder beim Zugriff auf eine freigegebene Ressource einen temporären Code anfordern, der an eine E-Mail-Adresse gesendet wird. Mit diesem temporären Code kann der Gastbenutzer seine Anmeldung fortsetzen.

Weitere Informationen finden Sie unter [Authentifizierung mit Einmalkennung per E-Mail (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) und im Blogartikel [Azure AD makes sharing and collaboration seamless for any user with any account](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949) (Nahtlose Freigabe und Zusammenarbeit in Azure AD für alle Benutzer mit beliebigen Konten).

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Neue Cookieeinstellungen für den Azure AD-Anwendungsproxy

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Wir haben drei neue Cookieeinstellungen eingeführt, die für Ihre Apps verfügbar sind, die über den Anwendungsproxy veröffentlicht werden:

- **Nur-HTTP-Cookie verwenden.** Legt das **HTTPOnly**-Flag für Ihre Anwendungsproxyzugriffs- und Sitzungscookies fest. Das Aktivieren dieser Einstellung bietet zusätzliche Sicherheitsvorteile, z.B. verhindert es das Kopieren oder Ändern von Cookies durch clientseitige Skripts. Sie sollten dieses Flag wg. der zusätzlichen Vorteile aktivieren (wählen Sie **Ja** aus).

- **Sicheres Cookie verwenden.** Legt das **Secure**-Flag für Ihre Anwendungsproxyzugriffs- und Sitzungscookies fest. Das Aktivieren dieser Einstellung bietet zusätzliche Sicherheitsvorteile, indem sichergestellt wird, dass Cookies nur über sichere TLS-Kanäle wie z.B. HTTPS übertragen werden. Sie sollten dieses Flag wg. der zusätzlichen Vorteile aktivieren (wählen Sie **Ja** aus).

- **Beständiges Cookie verwenden.** Verhindert, dass Zugriffcookies ablaufen, wenn der Webbrowser geschlossen wird. Diese Cookies existieren für die Lebensdauer des Zugriffstokens. Allerdings werden die Cookies zurückgesetzt, wenn der Ablaufzeitpunkt erreicht ist, oder wenn der Benutzer das Cookie manuell löscht. Sie sollten die Standardeinstellung **Nein** beibehalten und die Einstellung nur für ältere Apps aktivieren, die keine Cookies zwischen Prozessen freigeben.

Weitere Informationen zu den neuen Cookies finden Sie unter [Cookieeinstellungen für den Zugriff auf lokale Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Januar 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Juli 2019 haben wir diese 35 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [Talent Palette](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco Umbrella](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Internet Access Administrator](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [Expiration Reminder](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [InstaVR Viewer](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [Verb](https://app.verb.net/login), [OpenLattice](https://openlattice.com/agora), [TheOrgWiki](https://www.theorgwiki.com/signup), [Pavaso Digital Close](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [GoodPractice Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [Cloud Service PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [CallPlease](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO System](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [ARES for Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [K2 for Office 365](https://www.k2.com/O365), [Xledger](https://www.xledger.net/), [iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn Ferry ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle cSaas Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Neue Verbesserungen an Azure AD Identity Protection (Public Preview)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir freuen uns, bekanntgeben zu können, dass wir dem Public Preview-Angebot von Azure AD Identity Protection die folgenden Erweiterungen hinzugefügt haben:

- Eine aktualisierte und stärker integrierte Benutzeroberfläche

- Zusätzliche APIs

- Verbesserte Risikobewertung durch Machine Learning

- Produktweite Ausrichtung bezüglich riskanter Benutzer und riskanter Anmeldungen

Weitere Informationen über die Erweiterungen finden Sie unter [Was ist Azure Active Directory Identity Protection? (aktualisiert)](https://aka.ms/IdentityProtectionDocs), um mehr zu erfahren und Ihre Meinung mittels der Eingabeaufforderungen im Produkt mitzuteilen.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>Neue App-Sperrfunktion für die Microsoft Authenticator-App auf iOS- und Android-Geräten

**Typ:** Neues Feature  
**Dienstkategorie:** Microsoft Authenticator-App  
**Produktfunktion:** Identitätssicherheit und -schutz

Um die Sicherheit Ihrer Einmalpasscodes, App-Informationen und App-Einstellungen zu erhöhen, können Sie in der Microsoft Authenticator-App das Feature „App-Sperre“ aktivieren. Das Aktivieren der App-Sperre bedeutet, dass Sie bei jedem Öffnen der Microsoft Authenticator-App aufgefordert werden, sich mit Ihrer PIN oder Ihren biometrischen Daten zu authentifizieren.

Weitere Informationen finden Sie unter [Microsoft Authenticator-App – häufig gestellte Fragen](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq).

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Erweiterte Exportfunktionen für Azure AD Privileged Identity Management (PIM)

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management

Privileged Identity Management-Administratoren (PIM) können jetzt alle aktiven und geeigneten Rollenzuweisungen für eine bestimmte Ressource exportieren, die Rollenzuweisungen für alle untergeordneten Ressourcen eingeschlossen. In der Vergangenheit war es für Administratoren nicht ganz einfach, eine vollständige Liste mit den Rollenzuweisungen für ein Abonnement zu erhalten, und die Rollenzuweisungen mussten für jede spezifische Ressource exportiert werden.

Weitere Informationen finden Sie unter [Anzeigen des Aktivitäts- und Überwachungsverlaufs für Azure-Ressourcenrollen in PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

## <a name="novemberdecember-2018"></a>November/Dezember 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Aus dem Synchronisierungsbereich entfernte Benutzer werden nicht mehr auf reine Cloudkonten umgestellt

**Typ:** Korrigiert  
**Dienstkategorie:** Benutzerverwaltung  
**Produktfunktion:** Verzeichnis

>[!Important]
>Wir haben Ihre Verärgerung bezüglich dieser Fehlerbehebung zur Kenntnis genommen und verstanden. Aus diesem Grund haben wir die Änderung rückgängig gemacht, bis wir eine Fehlerbehebung anbieten können, die in Ihrer Organisation leichter implementiert werden kann.

Wir haben einen Fehler behoben, bei dem das DirSyncEnabled-Flag eines Benutzers fälschlicherweise in **False** geändert wurde, wenn das Objekt von Active Directory Domain Services (AD DS) aus dem Synchronisierungsbereich ausgeschlossen und dann im darauf folgenden Synchronisierungszyklus in Azure AD in den Papierkorb verschoben wurde. Wenn der Benutzer aus dem Synchronisierungsbereich ausgeschlossen und anschließend aus dem Azure AD-Papierkorb wiederhergestellt wird, bleibt das Benutzerkonto infolge dieser Korrektur wie erwartet von der lokalen AD-Instanz synchronisiert und kann nicht in der Cloud verwaltet werden, weil die Autoritätsquelle (Source of Authority, SoA) als lokale AD-Instanz erhalten bleibt.

Vor dieser Korrektur bestand ein Problem, wenn das DirSyncEnabled-Flag in „False“ geändert wurde. Es hat den falschen Eindruck vermittelt, dass diese Konten in reine Cloudobjekte umgewandelt wurden und die Konten in der Cloud verwaltet werden könnten. Die Konten behielten jedoch ihre SoA und alle synchronisierten-Eigenschaften (Schattenattribute) aus der lokalen AD-Instanz weiterhin als lokal bei. Dieser Zustand verursachte mehrere Probleme in Azure AD und anderen Cloudworkloads (wie Exchange Online), die davon ausgingen, diese Konten als von AD synchronisiert zu behandeln, die sich aber jetzt wie reine Cloudkonten verhielten.

Die einzige Möglichkeit, ein von AD synchronisiertes Konto tatsächlich in ein reines Cloudkonto umzuwandeln, besteht derzeit darin, DirSync auf Mandantenebene zu deaktivieren, wodurch ein Back-End-Vorgang zum Übertragen der SoA ausgelöst wird. Diese Art der Änderung der SoA erfordert (ist aber nicht beschränkt auf) das Bereinigen aller entsprechenden lokalen Attribute (wie LastDirSyncTime und Schattenattribute) und das Senden eines Signals an andere Cloudworkloads, das entsprechende Objekt ebenfalls in ein reines Cloudkonto umzuwandeln.

Durch diese Korrektur werden daher direkte Updates für das ImmutableID-Attribut eines von AD synchronisierten Benutzers verhindert, die in der Vergangenheit in einigen Szenarien erforderlich waren. Entwurfsbedingt soll die ImmutableID eines Objekts in Azure AD (wie der Name schon sagt) unveränderlich sein. Zur Unterstützung solcher Szenarien wurden neue Features in Azure AD Connect Health und in den Azure AD Connect-Synchronisierungsclient implementiert:

- **Umfangreiche ImmutableID-Updates für viele Benutzer in einem mehrstufigen Ansatz**
  
  Sie müssen beispielsweise eine langwierige gesamtstrukturübergreifende AD DS-Migration durchführen. Lösung: Verwenden Sie Azure AD Connect zum **Konfigurieren des Quellankers**, und kopieren Sie beim Migrieren des Benutzers die vorhandenen ImmutableID-Werte aus Azure AD in das „ms-DS-ConsistencyGuid“-Attribut der neuen Gesamtstruktur des lokalen AD DS-Benutzers. Weitere Informationen finden Sie unter [Verwendung von „ms-DS-ConsistencyGuid“ in „sourceAnchor“](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor).

- **Umfangreiche ImmutableID-Updates für viele Benutzer in einem Durchgang**

  Ihnen unterläuft beispielsweise bei der Implementierung von Azure AD Connect ein Fehler, und nun müssen Sie das Attribut „SourceAnchor“ ändern. Lösung: Deaktivieren Sie DirSync auf Mandantenebene, und löschen Sie alle ungültigen ImmutableID-Werte. Weitere Informationen finden Sie unter [Deaktivieren der Verzeichnissynchronisierung für Office 365](/office365/enterprise/turn-off-directory-synchronization).

- **Erneutes Abgleichen (Zuordnen) eines lokalen Benutzers mit einem vorhandenen Benutzer in Azure AD** Beispiel: Ein in AD DS neu erstellter Benutzer generiert ein Duplikat in einem Azure AD-Konto statt es einem vorhandenen Azure AD-Konto (verwaisten Objekt) neu zuzuordnen. Lösung: Verwenden Sie Azure AD Connect Health im Azure-Portal, um den Quellanker/die ImmutableID neu zuzuordnen. Weitere Informationen finden Sie unter [Szenario: Verwaistes Objekt](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario).

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>Wichtige Änderung: Aktualisierungen des Überwachungs- und Anmeldeprotokollschemas über Azure Monitor

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir veröffentlichen derzeit die Überwachungs- und Anmeldeprotokollstreams über Azure Monitor, sodass Sie die Protokolldateien nahtlos in Ihre SIEM-Tools oder in Log Analytics integrieren können. Basierend auf Ihrem Feedback und in Vorbereitung auf die Ankündigung der allgemeinen Verfügbarkeit dieses Features nehmen wir an unserem Schema die folgenden Änderungen vor. Diese Schemaänderungen und die zugehörigen Dokumentationsaktualisierungen erfolgen bis zur ersten Januarwoche.

#### <a name="new-fields-in-the-audit-schema"></a>Neue Felder im Überwachungsschema
Wir fügen ein neues Feld **Vorgangstyp** zum Angeben des für die Ressource ausgeführten Vorgangstyps hinzu. Beispiele hierfür sind **Hinzufügen**, **Aktualisieren** oder **Löschen**.

#### <a name="changed-fields-in-the-audit-schema"></a>Geänderte Felder im Überwachungsschema
Im Überwachungsschema werden die folgenden Felder geändert:

|Feldname|Änderung|Alte Werte|Neue Werte|
|----------|------------|----------|----------|
|Category (Kategorie)|Das war das Feld **Dienstname**. Jetzt heißt das Feld **Überwachungskategorien**. **Dienstname** wurde in das Feld **loggedByService** umbenannt.|<ul><li>Kontobereitstellung</li><li>Kernverzeichnis</li><li>Self-Service-Kennwortzurücksetzung</li></ul>|<ul><li>Benutzerverwaltung</li><li>Gruppenverwaltung</li><li>App-Verwaltung</li></ul>|
|targetResources|Enthält **TargetResourceType** auf der obersten Ebene.|&nbsp;|<ul><li>Richtlinie</li><li>App</li><li>Benutzer</li><li>Group</li></ul>|
|loggedByService|Gibt den Namen des Diensts an, der das Überwachungsprotokoll generiert hat.|Null|<ul><li>Kontobereitstellung</li><li>Kernverzeichnis</li><li>Self-Service-Kennwortzurücksetzung</li></ul>|
|Ergebnis|Stellt das Ergebnis der Überwachungsprotokolle bereit. Früher wurde das als Auflistung angegeben, aber jetzt wird der tatsächliche Wert angezeigt.|<ul><li>0</li><li>1</li></ul>|<ul><li>Erfolgreich</li><li>Fehler</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>Geänderte Felder im Anmeldeschema
Im Anmeldeschema werden die folgenden Felder geändert:

|Feldname|Änderung|Alte Werte|Neue Werte|
|----------|------------|----------|----------|
|appliedConditionalAccessPolicies|Das war das Feld **conditionalaccessPolicies**. Jetzt heißt das Feld **appliedConditionalAccessPolicies**.|Keine Änderung|Keine Änderung|
|conditionalAccessStatus|Stellt das Ergebnis des Status der Richtlinie für bedingten Zugriff bei der Anmeldung bereit. Früher wurde das als Auflistung angegeben, aber jetzt wird der tatsächliche Wert angezeigt.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Erfolgreich</li><li>Fehler</li><li>Nicht angewendet</li><li>Deaktiviert</li></ul>|
|appliedConditionalAccessPolicies: Ergebnis|Stellt das Ergebnis des Status der einzelnen Richtlinien für bedingten Zugriff bei der Anmeldung bereit. Früher wurde das als Auflistung angegeben, aber jetzt wird der tatsächliche Wert angezeigt.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Erfolgreich</li><li>Fehler</li><li>Nicht angewendet</li><li>Deaktiviert</li></ul>|

Weitere Informationen zum Schema finden Sie unter [Interpretieren des Azure AD-Überwachungsprotokollschemas in Azure Monitor (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema).

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Identity Protection-Verbesserungen am überwachten Machine Learning-Modell und am Risikobewertungsmodul

**Typ:** Geändertes Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Risikobewertungen

Verbesserungen am Identity Protection-bezogenen Benutzer- und Anmelderisikobewertungsmodul können zu einer besseren Genauigkeit und Abdeckung des Benutzerrisikos beitragen. Administratoren werden möglicherweise feststellen, dass die Benutzerrisikostufe nicht mehr direkt mit der Risikostufe bestimmter Erkennungen verknüpft ist und sich die Anzahl und Stufe von riskanten Anmeldeereignissen erhöht hat.

Risikoerkennungen werden jetzt vom überwachten Machine Learning-Modell ausgewertet, das das Benutzerrisiko mit zusätzlichen Features der Anmeldungen des Benutzers und einem Muster von Erkennungen berechnet. Auf Grundlage dieses Modells werden dem Administrator möglicherweise Benutzer mit hohen Risikobewertungen angezeigt, obwohl die mit diesem Benutzer verknüpften Erkennungen ein geringes oder mittleres Risiko darstellen. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Administratoren können ihr eigenes Kennwort mithilfe der Microsoft Authenticator-App zurücksetzen (öffentliche Vorschau)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Benutzerauthentifizierung

Azure AD-Administratoren können jetzt ihr eigenes Kennwort mithilfe von Benachrichtigungen der Microsoft Authenticator-App oder mit Code aus einer mobilen Authentifikator-App oder einem Hardwaretoken zurücksetzen. Administratoren können ihr eigenes Kennwort jetzt mit zwei der folgenden Methoden zurücksetzen:

- Benachrichtigung der Microsoft Authenticator-App

- Code aus einer anderen mobilen Authentifikator-App/aus einem Hardwaretoken

- E-Mail

- Telefonanruf

- Textnachricht

Weitere Informationen zur Verwendung der Microsoft Authenticator-App zum Zurücksetzen von Kennwörtern finden Sie unter [Funktionsweise der Self-Service-Kennwortzurücksetzung in Azure AD – mobile App und SSPR (Vorschau)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr-preview).

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Neue Azure AD-Rolle „Cloudgeräteadministrator“ (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Zugriffssteuerung

Administratoren können Benutzern die neue Rolle „Cloudgeräteadministrator“ zuweisen, damit sie Aufgaben eines Cloudgeräteadministrators ausführen können. Benutzer, denen die Rolle „Cloudgeräteadministrator“ zugewiesen ist, können Geräte in Azure AD aktivieren, deaktivieren und löschen sowie Windows 10-BitLocker-Schlüssel (falls vorhanden) im Azure-Portal lesen.

Weitere Informationen zu Rollen und Berechtigungen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Verwalten Ihrer Geräte mit dem neuen Aktivitätszeitstempel in Azure AD (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Lebenszyklusverwaltung für Geräte

Wir wissen, dass Sie im Laufe der Zeit die Geräte Ihrer Organisation in Azure AD aktualisieren und außer Betrieb nehmen müssen, um zu vermeiden, dass Ihre Umgebung veraltete Geräte enthält. Um Sie bei diesem Prozess zu unterstützen, werden Ihre Geräte in Azure AD jetzt mit einem neuen Aktivitätszeitstempel aktualisiert, sodass Sie den Gerätelebenszyklus einfacher verwalten können.

Weitere Informationen zum Abrufen und Verwenden dieses Zeitstempels finden Sie unter [Anleitung: Verwalten der veralteten Geräte in Azure AD](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices).

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Administratoren können erzwingen, dass Benutzer den Nutzungsbedingungen auf jedem Gerät zustimmen müssen

**Typ:** Neues Feature  
**Dienstkategorie:** Terms of Use (Nutzungsbedingungen)  
**Produktfunktion:** Governance
 
Administratoren können jetzt die Option **Zustimmung der Benutzer auf jedem Gerät erforderlich** aktivieren, damit Ihre Benutzer Ihren Nutzungsbedingungen auf jedem Gerät zustimmen müssen, das sie in Ihrem Mandanten verwenden.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory im Abschnitt „Nutzungsbedingungen pro Gerät“](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#per-device-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Administratoren können festlegen, dass die Einwilligungen für Nutzungsbedingungen basierend auf einer Zeitplanserie ablaufen

**Typ:** Neues Feature  
**Dienstkategorie:** Terms of Use (Nutzungsbedingungen)  
**Produktfunktion:** Governance
 

Administratoren können jetzt die Option **Ablauf für Einwilligungen** aktivieren, damit die Einwilligungen für Nutzungsbedingungen von allen Benutzern basierend auf der festgelegten Zeitplanserie ablaufen. Es kann ein jährlicher, halbjährlicher, vierteljährlicher oder monatlicher Zeitplan ausgewählt werden. Nach Ablauf der Nutzungsbedingungen müssen die Benutzer erneut zustimmen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory im Abschnitt „Hinzufügen von Nutzungsbedingungen“](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#add-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Administratoren können festlegen, dass die Einwilligungen für Nutzungsbedingungen basierend auf dem Zeitplan der einzelnen Benutzer ablaufen

**Typ:** Neues Feature  
**Dienstkategorie:** Terms of Use (Nutzungsbedingungen)  
**Produktfunktion:** Governance

Administratoren können jetzt einen Zeitraum angeben, in dem die Benutzer den Nutzungsbedingungen erneut zustimmen müssen. Administratoren können beispielsweise festlegen, dass die Benutzer den Nutzungsbedingungen alle 90 Tage erneut zustimmen müssen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory im Abschnitt „Hinzufügen von Nutzungsbedingungen“](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#add-terms-of-use).
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Neue E-Mails von Azure AD Privileged Identity Management (PIM) für Azure Active Directory-Rollen

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Kunden, die Azure AD Privileged Identity Management (PIM) verwenden, können jetzt eine wöchentliche Zusammenfassung per E-Mail mit den folgenden Informationen der letzten sieben Tage erhalten:

- Übersicht über die wichtigsten dauerhaften Zuweisungen berechtigter Rollen

- Anzahl von Benutzern, die Rollen aktivieren

- Anzahl von Benutzern, denen Rollen in PIM zugewiesen sind

- Anzahl von Benutzern, denen Rollen außerhalb von PIM zugewiesen sind

- Anzahl von Benutzern, denen eine Rolle in PIM dauerhaft zugewiesen wurde

Weitere Informationen zu PIM und den verfügbaren E-Mail-Benachrichtigungen finden Sie unter [E-Mail-Benachrichtigungen in PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications).

---

### <a name="group-based-licensing-is-now-generally-available"></a>Die gruppenbasierte Lizenzierung ist jetzt allgemein verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis

Die gruppenbasierte Lizenzierung befindet sich nicht mehr in der öffentlichen Vorschauphase und ist jetzt allgemein verfügbar. Im Rahmen dieser allgemeinen Version haben wir dieses Feature skalierbarer gestaltet und Optionen hinzugefügt, mit denen Sie Zuweisungen der gruppenbasierten Lizenzierung für einen einzelnen Benutzer erneut verarbeiten und die gruppenbasierte Lizenzierung für Office 365 E3-/A3-Lizenzen verwenden können.

Weitere Informationen zur gruppenbasierten Lizenzierung finden Sie unter [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – November 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im November 2018 haben wir diese 26 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[CoreStack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [Gra-Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [eHour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [DriveDollar](https://www.drivedollar.com/Account/Login), [Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [Infinite Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [Alaya](https://alayagood.com/en/demo/), [HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [Drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [Zenegy for Business Central 365](https://accounting.zenegy.com/), [Everbridge Member Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [Ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [Plex Apps – Classic Test](https://test.plexonline.com/signon), [Plex Apps – Classic](https://www.plexonline.com/signon), [Plex Apps – UX Test](https://test.cloud.plex.com/sso), [Plex Apps – UX](https://cloud.plex.com/sso), [Plex Apps – IAM](https://accounts.plex.com/), [CRAFTS – Childcare Records, Attendance, & Financial Tracking System](https://getcrafts.ca/craftsregistration) 

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

## <a name="october-2018"></a>Oktober 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD-Protokolle funktionieren jetzt mit Azure Log Analytics (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir freuen uns, Ihnen mitteilen zu können, dass Sie Ihre Azure AD-Protokolle jetzt an Azure Log Analytics weiterleiten können. Dieses Feature – das auf der Wunschliste unserer Kunden ganz oben stand – ermöglicht einen noch besseren Zugang zu Analysefunktionen für Business, Betrieb und Sicherheit und bietet eine Methode für die Überwachung Ihrer Infrastruktur. Weitere Informationen finden Sie im Blog [Azure Active Directory Activity logs in Azure Log Analytics now available](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) (Azure Active Directory-Aktivitätsprotokolle jetzt in Azure Log Analytics verfügbar).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Oktober 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Oktober 2018 haben wir diese 14 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[My Award Points](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), Dialpad, [ON24 Virtual Environment](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler Three](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot Control](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-domain-services-email-notifications"></a>Azure AD Domain Services – E-Mail-Benachrichtigungen

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Azure AD Domain Services bietet Warnungen im Azure-Portal zu fehlerhaften Konfigurationen oder Problemen in Ihrer verwalteten Domäne. Diese Warnungen umfassen Schrittanleitungen, sodass Sie die Probleme beheben können, ohne sich an den Support wenden zu müssen.

Ab Oktober können Sie die Benachrichtigungseinstellungen für Ihre verwaltete Domäne anpassen, sodass beim Auftreten neuer Warnungen eine E-Mail an eine angegebene Gruppe von Personen gesendet wird. So müssen Sie nicht mehr dauernd das Portal auf Updates überprüfen.

Weitere Informationen finden Sie unter [Benachrichtigungseinstellungen in Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications).

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Das Azure AD-Portal unterstützt die Verwendung der ForceDelete-Domänen-API zum Löschen von benutzerdefinierten Domänen 

**Typ:** Geändertes Feature  
**Dienstkategorie:** Verzeichnisverwaltung  
**Produktfunktion:** Verzeichnis

Wir freuen uns, Ihnen mitteilen zu können, dass Sie jetzt die ForceDelete-Domänen-API verwenden können, um Ihre benutzerdefinierten Domänennamen zu löschen. Über die API werden Verweise wie Benutzer, Gruppen und Apps mit Ihrem benutzerdefinierten Domänennamen (contoso.com) asynchron wieder in den standardmäßigen Domänennamen (contoso.onmicrosoft.com) umbenannt.

Dank dieser Änderung können Sie Ihren benutzerdefinierten Domänennamen schneller löschen, wenn dieser von Ihrem Unternehmen nicht mehr verwendet wird oder wenn Sie den Domänennamen für eine andere Azure AD-Instanz verwenden möchten.

Weitere Informationen finden Sie unter [Löschen eines benutzerdefinierten Domänennamens](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>September 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Aktualisierte Berechtigungen der Administratorrolle für dynamische Gruppen

**Typ:** Korrigiert  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir haben ein Problem behoben, damit mit bestimmten Administratorrollen jetzt dynamische Mitgliedschaftsregeln erstellt und aktualisiert werden können, ohne dass der Benutzer der Besitzer der Gruppe sein müssen.

Die Rollen lauten:

- Globaler Administrator

- Intune-Administrator

- Benutzeradministrator

Weitere Informationen finden Sie unter [Erstellen einer dynamischen Gruppe und Überprüfen des Status](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule).

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Vereinfachte Konfigurationseinstellungen für einmaliges Anmelden (SSO) bei einigen Drittanbieter-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Uns ist bewusst, dass die Einrichtung des einmaligen Anmeldens (SSO) für SaaS-Apps (Software-as-a-Service) aufgrund der speziellen Konfiguration von Apps eine Herausforderung darstellen kann. Wir haben eine vereinfachte Benutzeroberfläche für die Konfiguration erstellt, um die SSO-Konfigurationseinstellungen für die folgenden Drittanbieter-SaaS-Apps automatisch einzufügen:

- Zendesk

- ArcGis Online

- Jamf Pro

Sie können diese 1-Klick-Oberfläche verwenden, indem Sie für die App zu **Azure-Portal** > **SSO-Konfiguration** navigieren. Weitere Informationen finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list).

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Seite „Azure Active Directory – An welchem Ort befinden sich Ihre Daten?“

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** GoLocal

Wählen Sie auf der Seite **Azure Active Directory – An welchem Ort befinden sich Ihre Daten?** die Region Ihres Unternehmens aus, um anzuzeigen, in welchem Azure-Rechenzentrum sich Ihre ruhenden Azure AD-Daten für alle Azure AD-Dienste befinden. Sie können die Informationen nach bestimmten Azure AD-Diensten für die Region Ihres Unternehmens filtern.

Informationen zum Zugriff auf dieses Feature und weitere Informationen finden Sie unter [Azure Active Directory – An welchem Ort befinden sich Ihre Daten?](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Neuer Bereitstellungsplan verfügbar für Zugriffsbereich „Meine Apps“

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** SSO

Erkunden Sie den neuen Bereitstellungsplan, der für den Zugriffsbereich „Meine Apps“ verfügbar ist (https://aka.ms/deploymentplans).
Im Zugriffsbereich „Meine Apps“ können Benutzer an einem zentralen Ort nach ihren Apps suchen und darauf zugreifen. In diesem Portal können Benutzer auch Self-Service-Funktionen verwenden, z.B. das Anfordern des Zugriffs auf Apps und Gruppen oder das Verwalten des Zugriffs auf diese Ressourcen im Namen von anderen Personen.

Weitere Informationen finden Sie unter [Was ist das MyApps-Portal?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction).

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Neue Registerkarte „Problembehandlung und Support“ auf der Seite „Anmeldungen“ im Azure-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Die neue Registerkarte **Problembehandlung und Support** auf der Seite **Anmeldungen** des Azure-Portals dient Administratoren und Engineers als Hilfe beim Behandeln von Problemen mit der Azure AD-Anmeldung. Diese neue Registerkarte enthält den Fehlercode, die Fehlermeldung und Empfehlungen zur Problemlösung (falls zutreffend), um die Behebung des Problems zu ermöglichen. Wenn das Problem nicht lösbar ist, können Sie auch eine neue Möglichkeit zum Erstellen eines Supporttickets nutzen, indem Sie die Option **In Zwischenablage kopieren** verwenden. Bei dieser Option werden die Felder **Anforderungs-ID** und **Datum (UTC)** für die Protokolldatei in Ihrem Supportticket ausgefüllt.  

![Anmeldeprotokolle mit der neuen Registerkarte](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Verbesserte Unterstützung für benutzerdefinierte Erweiterungseigenschaften, die zum Erstellen von dynamischen Mitgliedschaftsregeln verwendet werden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Mit diesem Update können Sie jetzt im Regel-Generator für dynamische Benutzergruppen auf den Link **Get custom extension properties** (Benutzerdefinierte Erweiterungseigenschaften abrufen) klicken, Ihre eindeutige App-ID eingeben und die vollständige Liste mit benutzerdefinierten Erweiterungseigenschaften erhalten, die Sie beim Erstellen einer dynamischen Mitgliedschaftsregeln für Benutzer verwenden. Diese Liste kann auch aktualisiert werden, um neue benutzerdefinierte Erweiterungseigenschaften für diese App zu erhalten.

Weitere Informationen zur Verwendung von benutzerdefinierten Erweiterungseigenschaften für dynamische Mitgliedschaftsregeln finden Sie unter [Erweiterungseigenschaften und benutzerdefinierte Erweiterungseigenschaften](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties).

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD

**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement) hinzugefügt:

- Microsoft To-Do

- Microsoft Stream

Weitere Informationen finden Sie unter

- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Neue Unterstützung für Self-Service-Kennwortzurücksetzung über den Windows 7/8/8.1-Sperrbildschirm

**Typ:** Neues Feature  
**Dienstkategorie:** SSPR  
**Produktfunktion:** Benutzerauthentifizierung

Nachdem Sie dieses neue Feature eingerichtet haben, wird Ihren Benutzern ein Link zum Zurücksetzen Ihres Kennworts über den **Sperrbildschirm** eines Geräts mit Windows 7, Windows 8 oder Windows 8.1 angezeigt. Wenn Benutzer auf diesen Link klicken, werden sie durch den gleichen Ablauf zur Kennwortzurücksetzung wie im Webbrowser geleitet.

Weitere Informationen finden Sie unter [How to enable password reset from Windows 7, 8, and 8.1](https://aka.ms/ssprforwindows78) (Aktivieren der Kennwortzurücksetzung unter Windows 7, 8 und 8.1).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Änderungshinweis: Autorisierungscodes können nicht mehr wiederverwendet werden. 

**Typ:** Plan für Änderung  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Ab dem 15. November 2018 akzeptiert Azure AD bereits zuvor verwendete Authentifizierungscodes für Apps nicht mehr. Diese Sicherheitsänderung trägt dazu bei, Azure AD an die OAuth-Spezifikation anzupassen. Sie wird auf v1- und v2-Endpunkten erzwungen.

Wenn Ihre App Autorisierungscodes zum Abrufen von Token für mehrere Ressourcen wiederverwendet, sollten Sie den Code für das Abrufen eines Aktualisierungstokens verwenden. Verwenden Sie dieses Aktualisierungstoken anschließend, um die zusätzlichen Token für andere Ressourcen abzurufen. Autorisierungscodes können nur einmal verwendet werden, Aktualisierungstoken hingegen können mehrere Male und für mehrere Ressourcen verwendet werden. Für jede App, die einen Authentifizierungscode im OAuth-Codefluss erneut verwenden möchte, wird ein Fehler vom Typ „invalid_grant“ zurückgegeben.

Informationen hierzu und zu anderen protokollbezogenen Änderungen finden Sie in der [vollständigen Liste mit den Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – September 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im September 2018 haben wir diese 16 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet Recruiting Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Enlyft SSO for Azure, SurveyMonkey, [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="support-for-additional-claims-transformations-methods"></a>Unterstützung für zusätzliche Transformationsmethoden für Ansprüche

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Wir haben die neuen Transformationsmethoden „ToLower()“ und „ToUpper()“ eingeführt, die über die SAML-basierte Seite **SSO-Konfiguration** auf SAML-Token angewendet werden können.

Weitere Informationen finden Sie unter [Gewusst wie: Anpassen ausgestellter Ansprüche im SAML-Token für Unternehmensanwendungen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Aktualisierte SAML-basierte Benutzeroberfläche für die App-Konfiguration (Vorschauversion)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Im Rahmen unserer aktualisierten SAML-basierten Benutzeroberfläche für die App-Konfiguration erhalten Sie Folgendes:

- Eine aktualisierte Benutzeroberfläche mit Schritt-für-Schritt-Anleitung für die Konfiguration Ihrer SAML-basierten Apps

- Bessere Informationen zu fehlenden oder fehlerhaften Teilen der Konfiguration

- Möglichkeit zum Hinzufügen von mehreren E-Mail-Adressen für Benachrichtigungen zum Zertifikatablauf

- Neue Transformationsmethoden „ToLower()“ und „ToUpper()“ für Ansprüche und mehr

- Möglichkeit zum Hochladen Ihres eigenen Tokensignaturzertifikats für Ihre Unternehmens-Apps

- Möglichkeit zum Festlegen des NameID-Formats für SAML-Apps und des NameID-Werts als Verzeichniserweiterungen

Klicken Sie zum Aktivieren dieser aktualisierten Ansicht oben auf der Seite **Einmaliges Anmelden** auf den Link **Neue Benutzeroberfläche ausprobieren**. Weitere Informationen finden Sie unter [Tutorial: Konfigurieren des SAML-basierten einmaligen Anmeldens für eine Anwendung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).