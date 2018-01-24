---
title: Neuerungen Anmerkungen zur Version Azure Active Directory Premium | Microsoft-Dokumentation
description: "Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory (Azure AD), u.a. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 6e1cf6e2ee717ef7629e1388d7bca2090eed46fa
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="whats-new-in-azure-active-directory"></a>Neuerungen in Azure Active Directory




> Abonnieren Sie unseren [![RSS](./media/whats-new/feed-icon-16x16.png)](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us)-[Feed](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us), um in Bezug auf Neuerungen in Azure Active Directory auf dem Laufenden zu bleiben.



Wir nehmen laufend Verbesserungen an Azure Active Directory vor. Damit Sie bei den neuesten Entwicklungen auf dem neuesten Stand bleiben, bietet dieses Thema Informationen zu Folgendem:

-   Neueste Versionen 
-   Bekannte Probleme 
-   Fehlerbehebungen 
-   Veraltete Funktionen 
-   Pläne für Änderungen 

Besuchen Sie diese Seite regelmäßig, da wir sie monatlich aktualisieren.


## <a name="december-2017"></a>Dezember 2017
 

### <a name="terms-of-use-in-the-access-panel-for-end-users"></a>Nutzungsbedingungen im Zugriffsbereich für Endbenutzer

**Typ:** Neue Funktion  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance/Compliance
 
Endbenutzer haben jetzt die Möglichkeit, zum Zugriffsbereich zu wechseln und die Nutzungsbedingungen anzuzeigen, die sie zuvor akzeptiert haben.

Benutzer können die akzeptierten Nutzungsbedingungen überprüfen und anzeigen. Hierfür können sie folgendes Verfahren verwenden:

1. Navigieren Sie zum [MyApps-Portal](https://myapps.microsoft.com), und melden Sie sich an.

2. Klicken Sie in der rechten oberen Ecke auf Ihren Namen, und wählen Sie in der Dropdownliste die Option **Profil** aus. 

3. Klicken Sie in Ihrem Profil auf **Nutzungsbedingungen lesen**. 

4. Hier können Sie sich die akzeptierten Nutzungsbedingungen ansehen. 

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature (Vorschauversion) für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 

### <a name="new-azure-ad-sign-in-experience"></a>Neuer Anmeldevorgang für Azure AD

**Typ:** Neue Funktion  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Benutzerauthentifizierung
 
Als Teil der laufenden Zusammenführung der Identitätssysteme von Azure AD und Microsoft-Konten haben wir die Benutzeroberfläche beider Systeme neu gestaltet, sodass sie ein einheitliches Erscheinungsbild aufweisen. Darüber hinaus haben wir die Azure AD-Anmeldeseite paginiert, sodass wir zuerst den Benutzernamen erfassen, gefolgt von den Anmeldeinformationen auf einem zweiten Bildschirm.

Weitere Informationen finden Sie unter [The new Azure AD Signin Experience is now in Public Preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/) (Die neue Umgebung für die Azure AD-Anmeldung ist jetzt als Public Preview verfügbar).
 
---
 

### <a name="fewer-login-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-login"></a>Weniger Anmeldeeingabeaufforderungen: eine neue „Angemeldet bleiben“-Umgebung für die Azure AD-Anmeldung

**Typ:** Neue Funktion  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Benutzerauthentifizierung
 
Wir haben das Kontrollkästchen **Angemeldet bleiben** auf der Azure AD-Anmeldeseite durch eine neue Eingabeaufforderung ersetzt, die nach der erfolgreichen Authentifizierung des Benutzers angezeigt wird. 

Wenn ein Benutzer diese Eingabeaufforderung mit **Ja** beantwortet, erhält er ein permanentes Aktualisierungstoken. Dies ist das gleiche Verhalten wie beim Aktivieren des Kontrollkästchens **Angemeldet bleiben** in der alten Umgebung. Für Verbundmandanten wird diese Aufforderung angezeigt, nachdem sich der Benutzer erfolgreich bei dem Verbunddienst authentifiziert hat.

Weitere Informationen finden Sie unter [Fewer login prompts: The new “Keep me signed in” experience for Azure AD is in preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) (Weniger Anmeldeeingabeaufforderungen: Die neue „Angemeldet bleiben“-Umgebung für Azure AD befindet sich in der Vorschauphase). 

---
 

### <a name="add-configuration-to-require-the-tou-to-be-expanded-prior-to-accepting"></a>Hinzufügen einer Konfiguration, um vor dem Akzeptieren zum Erweitern der Nutzungsbedingungen aufzufordern

**Typ:** Neue Funktion  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance
 
Wir haben nun eine Option für Administratoren hinzugefügt, mit der Endbenutzer aufgefordert werden können, die Nutzungsbedingungen vor dem Akzeptieren zu erweitern.

Aktivieren oder deaktivieren Sie „Benutzer müssen die Nutzungsbedingungen erweitern“. Wenn diese Option aktiviert ist, müssen Endbenutzer die Nutzungsbedingungen anzeigen, bevor sie sie akzeptieren.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature (Vorschauversion) für Azure Active Directory](active-directory-tou.md).
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Bereichsbezogene Aktivierung für berechtigte Rollenzuweisungen

**Typ:** Neue Funktion  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Mit der bereichsbezogenen Aktivierung können Sie berechtigte Rollenzuweisungen für Azure-Ressourcen mit weniger Autonomie aktivieren, als die ursprünglichen Standardwerte für die Zuweisung vorsehen. Beispielsweise wird Ihnen „Besitzer“ für ein Abonnement in Ihrem Mandanten zugewiesen. Mit der bereichsbezogenen Aktivierung können Sie „Besitzer“ für bis zu fünf innerhalb des Abonnements enthaltene Ressourcen aktivieren (geeignet etwa für Ressourcengruppen, Virtual Machines usw.). Durch Festlegen des Gültigkeitsbereichs der Aktivierung kann die Wahrscheinlichkeit verringert werden, dass unerwünschte Änderungen an wichtigen Azure-Ressourcen vorgenommen werden.

Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](active-directory-privileged-identity-management-configure.md).
 
---
 

### <a name="new-federated-apps-in-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog

**Typ:** Neue Funktion  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieteranwendungen
 
Im Dezember 2017 haben wir die folgenden neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

|NAME|Integrationstyp|BESCHREIBUNG|
|:-- |----------------|:----------|
|EFI Digital StoreFront|SAML 2.0|[Web 2-Druckanwendung](https://go.microsoft.com/fwlink/?linkid=861685)|
|Vodeclic|SAML 2.0|[Verwenden Sie Azure AD, um Benutzerzugriff zu verwalten und einmaliges Anmelden (Single Sign On) mit Vodeclic zu aktivieren](https://go.microsoft.com/fwlink/?linkid=863522).  Erfordert ein vorhandenes Vodeclic-Konto.|
|Accredible|SAML 2.0|[Erstellen, Verwalten und Übermitteln von Zertifikaten, Badges und Blockchain-Anmeldeinformationen](https://go.microsoft.com/fwlink/?linkid=863523)|
|FactSet|SAML 2.0|[Einmaliges Anmelden bei der Anwendung FDSWeb von FactSets](https://go.microsoft.com/fwlink/?linkid=863525)|
|MobileIron Azure AD Integration|SAML 2.0|[MobileIron](https://go.microsoft.com/fwlink/?linkid=858027) versetzt moderne Unternehmen in die Lage, bei der Umstellung auf mobile Lösungen und die Cloud Informationen zu sichern und zu verwalten und zugleich Datenschutz und Vertrauenswürdigkeit sicherzustellen.|
|IMAGE WORKS|SAML 2.0|Verwenden Sie Azure AD, um Benutzerzugriff zu verwalten, Benutzerkonten bereitzustellen und einmaliges Anmelden (Single Sign On) mit [IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517) zu aktivieren. Erfordert ein IMAGE WORKS-Abonnement.|
|SAML SSO for Bitbucket by resolution GmbH|SAML 2.0|[SSO Bitbucket](https://go.microsoft.com/fwlink/?linkid=863519) delegiert die Authentifizierung an Azure AD. Bereits bei Azure AD angemeldete Benutzer können direkt auf Bitbucket zugreifen. Benutzer können mit Daten aus SAML-Attributen dynamisch erstellt und aktualisiert werden.|
|SAML SSO for Bamboo by resolution GmbH|SAML 2.0|[SSO Bamboo](https://go.microsoft.com/fwlink/?linkid=863520) delegiert die Authentifizierung an Azure AD. Bereits bei Azure AD angemeldete Benutzer können direkt auf Bamboo zugreifen.|
|Communifire|SAML 2.0|[Communifire](https://go.microsoft.com/fwlink/?linkid=861676) ist eine moderne, voll funktionsfähige Social-Intranet-Software, die Ihre Mitarbeiter und Ihr Unternehmen unterstützt.|
|MOBI|SAML 2.0|[Zentralisieren, verstehen und steuern Sie das gesamte Spektrum Ihrer Geräte](https://go.microsoft.com/fwlink/?linkid=863521).|
|Reflektive|SAML 2.0|[Reflektive](https://go.microsoft.com/fwlink/?linkid=863518) ist eine moderne Plattform für Leistungsmanagement, Feedback in Echtzeit und Zielfestlegung. Wir versetzen Mitarbeiter in die Lage, ihre eigene Entwicklung in die Hand zu nehmen, damit Ihr Unternehmen strategischer handeln kann.|
|CybSafe|OpenID Connect und OAuth|CybSafe ist eine GCHQ-zertifizierte Cyber-Awareness-Plattform. Mit fortschrittlicher Technologie und Datenanalysen reduziert sie nachweislich den menschlichen Aspekt von Risiken in der Cybersicherheit und im Datenschutz.|
|WebHR|OpenID Connect und OAuth|Die bevorzugte umfassende Social Software für das Personalwesen. Mehr als 20.000 Unternehmen in 197 Ländern vertrauen darauf.|
 |Zenegy Azure AD Integration|OpenID Connect und OAuth|Mit dieser App können Sie die Azure Active Directory-Anmeldeinformationen Ihres Unternehmens für die Anmeldung bei Zenegy verwenden.|
|Adobe Experience Manager|SAML 2.0|Adobe Experience Manager (AEM) ist eine umfassende Content Management-Plattformlösung zum Erstellen von Websites, mobilen Apps und Formularen, die die Verwaltung Ihrer Marketinginhalte und -ressourcen vereinfacht.|

 
---
 

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Genehmigungsworkflows für Azure AD-Verzeichnisrollen

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Ein Genehmigungsworkflow für Azure AD-Verzeichnisrollen ist allgemein verfügbar.

Mit dem Genehmigungsworkflow können Administratoren für privilegierte Rollen festlegen, dass geeignete Rollenmitglieder die Rollenaktivierung anfordern müssen, bevor sie die privilegierte Rolle verwendet können.
Genehmigungszuständigkeiten können an mehrere Benutzer und Gruppen delegiert werden. Geeignete Rollenmitglieder erhalten Benachrichtigungen, wenn die Genehmigung abgeschlossen und ihre Rolle aktiv ist.

---
 

### <a name="pass-through-authentication---skype-for-business-support"></a>Pass-Through-Authentifizierung – Unterstützung von Skype for Business

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung


Die Pass-Through-Authentifizierung unterstützt jetzt Benutzeranmeldungen bei Skype for Business-Clientanwendungen, die moderne Authentifizierung unterstützen, einschließlich Online- und Hybridtopologien. 

Weitere Informationen finden Sie unter [Mit moderner Authentifizierung unterstützte Skype for Business-Topologien](https://technet.microsoft.com/library/mt803262.aspx).
 
---
 

### <a name="updates-to-azure-active-directory-privileged-identity-management-pim-for-azure-rbac-preview"></a>Updates auf Azure Active Directory Privileged Identity Management (PIM) für Azure RBAC (Vorschau)

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Privileged Identity Management (PIM)  
**Produktfunktion:** Privileged Identity Management
 
Mit unserer aktualisierten öffentlichen Vorschau von Azure Active Directory Privileged Identity Management (PIM) für Azure RBAC haben Sie nun folgende Möglichkeit:

Erfordern der Genehmigung für minimale Verwaltung (Just Enough Administration) zum Aktivieren von Ressourcenrollen; Planen einer zukünftigen Aktivierung einer Rolle, die eine Genehmigung erfordert, sowohl für AAD- als auch für Azure RBAC-Rollen

 
Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

 
---
 
## <a name="november-2017"></a>November 2017
 
### <a name="retiring-acs"></a>Außerbetriebnahme von ACS



**Typ:** Plan für Änderung  
**Dienstkategorie:** ACS  
**Produktfunktion:** Access Control Service 


Microsoft Azure Active Directory Access Control (auch als Access Control Service oder ACS bezeichnet) wird Ende 2018 eingestellt.  Weitere Informationen, u.a. ein ausführlicher Zeitplan und allgemeine Migrationsanweisungen, werden in den nächsten Wochen bereitgestellt. Hinterlassen Sie in der Zwischenzeit Kommentare auf dieser Seite, wenn Sie Fragen zu ACS haben. Ein Mitglied unseres Teams wird sich dann mit Ihnen in Verbindung setzen.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Einschränkung des Browserzugriffs auf Intune Managed Browser 


**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz




Durch dieses Verhalten können Sie den Browserzugriff auf Office 365 und weitere Azure AD-verbundene Cloud-Apps mithilfe von Intune Managed Browser als genehmigte App einschränken. 

Diese Änderung erlaubt es Ihnen, die folgende Bedingung für den anwendungsbasierten bedingten Zugriff zu konfigurieren:

**Client-Apps:**  Browser

**Welche Auswirkung hat die Änderung?**

Der Zugriff wird blockiert, wenn diese Bedingung verwendet wird. Sobald die Vorschau dieses Verhaltens verfügbar ist, ist für den gesamten Zugriff die Verwendung der Managed Browser-Anwendung erforderlich. 

Achten Sie in kommenden Blogbeiträgen und Versionshinweisen auf Informationen zu dieser Funktionalität. 

Weitere Informationen finden Sie unter [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD

 
**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz




Die folgenden Apps sind zur Aufnahme in die Liste der [genehmigten Client-Apps](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) vorgesehen:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)

- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Weitere Informationen finden Sie unter 

- [Genehmigte Client-App als Voraussetzung](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [App-basierter bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-mam.md)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Unterstützung von Nutzungsbedingungen für mehrere Sprachen



**Typ:** Neue Funktion    
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance/Compliance





Administratoren können jetzt neue Nutzungsbedingungen erstellen, die mehrere PDF-Dokumente enthalten. Diese PDF-Dateien können mit einer zugehörigen Sprache gekennzeichnet werden. Benutzern im Geltungsbereich wird dann die PDF-Datei angezeigt, die der Sprache in den Benutzereinstellungen entspricht. Falls keine Übereinstimmung vorliegt, wird die Standardsprache angezeigt.


---
 

### <a name="realtime-password-writeback-client-status"></a>Echtzeitclientstatus beim Kennwortrückschreiben



**Typ:** Neue Funktion  
**Dienstkategorie:** SSPR  
**Produktfunktion:** Benutzerauthentifizierung


 

Es ist jetzt möglich, den Status Ihres lokalen Clients für das Kennwortrückschreiben anzuzeigen. Diese Option steht im Abschnitt **Lokale Integration** auf der Seite **[Kennwortzurücksetzung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)** zur Verfügung. 

Wenn Verbindungsprobleme mit Ihrem lokalen Client für das Kennwortrückschreiben vorliegen, wird eine Fehlermeldung angezeigt, die Folgendes enthält:

- Informationen dazu, warum keine Verbindung mit dem lokalen Client für das Kennwortrückschreiben möglich ist 
- Ein Link zur Dokumentation, die Sie bei der Problembehandlung unterstützt 


Weitere Informationen finden Sie unter [Lokale Integration](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>App-basierter bedingter Zugriff mit Azure AD 



 
**Typ:** Neue Funktion  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Identitätssicherheit und -schutz





Sie können jetzt über den [App-basierten bedingten Zugriff mit Azure AD](active-directory-conditional-access-mam.md) den Zugriff auf Office 365 und weitere Azure AD-verbundene Cloud-Apps auf [genehmigte Client-Apps](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) beschränken, die Intune-App-Schutzrichtlinien unterstützen.  Intune-App-Schutzrichtlinien werden verwendet, um Unternehmensdaten in diesen Clientanwendungen zu konfigurieren und zu schützen.

Durch das Kombinieren von [App-basierten](active-directory-conditional-access-mam.md) und [gerätebasierten](active-directory-conditional-access-policy-connected-applications.md) Richtlinien für den bedingten Zugriff können Sie Daten für private Geräte und Unternehmensgeräte flexibel schützen.

Für den App-basierten bedingten Zugriff stehen ab sofort die folgenden Bedingungen und Steuerelemente zur Verfügung:

**Bedingung für unterstützte Plattform**

- iOS
- Android

**Client-Apps-Bedingung**

- Mobile Apps und Desktop-Apps

**Zugriffssteuerung**

- Genehmigte Client-App erforderlich


Weitere Informationen finden Sie unter [App-basierter bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-mam.md).

 
---

### <a name="managing-azure-ad-devices-in-the-azure-portal"></a>Verwalten von Azure AD-Geräten im Azure-Portal



**Typ:** Neue Funktion  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Identitätssicherheit und -schutz

 



Ab jetzt finden Sie alle mit Azure AD verbundenen Geräte sowie die gerätebezogenen Aktivitäten an einem Ort. Es steht eine neue Verwaltungsoberfläche zur Verfügung, mit der Sie all ihre Geräteidentitäten und -einstellungen im Azure-Portal verwalten können. Diese Version ermöglicht Folgendes:

- Anzeigen all Ihrer Geräte, die für den bedingten Zugriff in Azure AD zur Verfügung stehen

- Anzeigen von Eigenschaften, einschließlich von Geräten, die in Hybrid-Azure AD eingebunden sind

- Auffinden von BitLocker-Schlüsseln für Ihre in Azure AD eingebundenen Geräte, Verwalten Ihrer Geräte mit Intune und vieles mehr

- Verwalten von Azure AD-gerätebezogenen Einstellungen


Weitere Informationen finden Sie unter [Verwalten von Geräten mit dem Azure-Portal](device-management-azure-portal.md).



 
---

### <a name="support-for-macos-as-device-platform-for-azure-ad-conditional-access"></a>Unterstützung der macOS-Geräteplattform für den bedingten Azure AD-Zugriff 



**Typ:** Neue Funktion    
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz 
 

Ab sofort können Sie macOS als Geräteplattformbedingung in Ihre Richtlinie für den bedingten Azure AD-Zugriff einschließen (oder ausschließen). Durch das Hinzufügen von macOS als unterstützte Geräteplattform haben Sie folgende Möglichkeiten:

- **Registrieren und Verwalten von macOS-Geräten mit Intune**: Ähnlich wie für andere Plattformen (z.B. iOS und Android) steht für macOS eine Unternehmensportalanwendung bereit, um einheitliche Registrierungen durchzuführen. Die neue Unternehmensportal-App für macOS ermöglicht es Ihnen, ein Gerät bei Intune und Azure AD zu registrieren.
 
- **Sicherstellen, dass macOS-Geräte den in Intune definierten Konformitätsrichtlinien Ihrer Organisation entsprechen**: Im Azure-Portal für Intune können Sie ab sofort Konformitätsrichtlinien für macOS-Geräte einrichten. 
  
- **Einschränken des Anwendungszugriffs in Azure AD auf konforme macOS-Geräte**: Bei der Erstellung von Richtlinien für den bedingten Zugriff steht eine separate Option für macOS als Geräteplattform zur Verfügung. Diese Option macht es möglich, macOS-spezifische Richtlinien für den bedingten Zugriff für die festgelegte Zielanwendung in Azure zu erstellen.

Weitere Informationen finden Sie unter 

- [Erstellen einer Gerätekonformitätsrichtlinie für macOS-Geräte mit Intune](https://aka.ms/macoscompliancepolicy)
- [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md)


 
---

### <a name="nps-extension-for-azure-mfa"></a>NPS-Erweiterung für Azure MFA 


**Typ:** Neue Funktion    
**Dienstkategorie:** Mehrstufige Authentifizierung (MFA)  
**Produktfunktion:** Benutzerauthentifizierung




Die NPS-Erweiterung (Network Policy Server, Netzwerkrichtlinienserver) für Azure MFA fügt Ihrer Authentifizierungsinfrastruktur unter Verwendung Ihrer vorhandenen Server cloudbasierte MFA-Funktionen hinzu. Mit der NPS-Erweiterung können Sie Ihrem bestehenden Authentifizierungsvorgang eine Überprüfung per Telefonanruf, SMS oder Telefon-App hinzufügen, ohne neue Server installieren, konfigurieren und verwalten zu müssen. 

Diese Erweiterung wurde für Organisationen entwickelt, die VPN-Verbindungen schützen möchten, ohne den Azure MFA-Server bereitzustellen. Die NPS-Erweiterung fungiert als Adapter zwischen RADIUS und cloudbasierter Azure MFA, um eine zweite Stufe der Authentifizierung für Verbund- oder synchronisierte Benutzer bereitzustellen.


Weitere Informationen finden Sie unter [Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md).

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Wiederherstellen oder dauerhaftes Entfernen gelöschter Benutzer


**Typ:** Neue Funktion    
**Dienstkategorie:** Benutzerverwaltung  
**Produktfunktion:** Verzeichnis 



Im Azure AD Admin Center haben Sie jetzt folgende Möglichkeiten:

- Wiederherstellen eines gelöschten Benutzers 
- Dauerhaftes Löschen eines Benutzers 


**So können Sie es ausprobieren**

1. Wählen Sie im Azure AD Admin Center im Abschnitt **Verwalten** die Option [**Alle Benutzer**](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) aus. 

2. Wählen Sie in der Liste **Anzeigen** die Option **Kürzlich gelöschte Benutzer** aus. 

4. Wählen Sie mindestens einen kürzlich gelöschten Benutzer aus, und stellen Sie den Benutzer dann entweder wieder her, oder löschen Sie ihn dauerhaft.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD

 
**Typ:** Geänderte Funktion  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz


Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) hinzugefügt:

- Microsoft Planner

- Microsoft Azure Information Protection 


Weitere Informationen finden Sie unter 

- [Genehmigte Client-App als Voraussetzung](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [App-basierter bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-mam.md)


---

### <a name="ability-to-or-between-controls-in-a-conditional-access-policy"></a>Möglichkeit zum Einfügen von „OR“ zwischen Steuerelementen in einer Richtlinie für den bedingten Zugriff 


**Typ:** Geänderte Funktion    
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

 
Ab sofort besteht die Möglichkeit, „OR“ (eines der ausgewählten Steuerelemente ist erforderlich) zwischen Steuerelementen für den bedingten Zugriff einzufügen. Durch dieses Feature können Sie Richtlinien mit einem **OR** zwischen Steuerelementen für den Zugriff erstellen. Beispielsweise können Sie mit diesem Feature eine Richtlinie erstellen, durch die der Benutzer sich entweder über die mehrstufige Authentifizierung anmelden **OR** (oder) ein konformes Gerät verwenden muss.

Weitere Informationen finden Sie unter [Steuerelemente beim bedingten Zugriff mit Azure Active Directory](active-directory-conditional-access-controls.md).

 
---

### <a name="aggregation-of-realtime-risk-events"></a>Aggregation von Echtzeitrisikoereignissen


**Typ:** Geänderte Funktion    
**Dienstkategorie:** Identitätsschutz  
**Produktfunktion:** Identitätssicherheit und -schutz


Für eine verbesserte Benutzerfreundlichkeit bei der Verwaltung werden jetzt in Azure AD Identity Protection alle Echtzeitrisikoereignisse, die von derselben IP-Adresse stammen, für einen vorgegebenen Tag für jeden Risikoereignistyp aggregiert. Durch diese Änderung werden weniger Risikoereignisse ohne Änderung der Benutzersicherheit angezeigt.

Die zugrunde liegende Echtzeiterkennung erfolgt bei jeder Benutzeranmeldung. Wenn Sie eine Sicherheitsrichtlinie für das Anmelderisiko auf MFA oder eine Blockierung des Zugriffs festgelegt haben, wird diese bei jeder riskanten Anmeldung weiterhin ausgelöst.

 
---
 




## <a name="october-2017"></a>Oktober 2017


### <a name="deprecating-azure-ad-reports"></a>Einstellung von Azure AD-Berichten


**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Identity Lifecycle Management  



Azure-Portal bietet Ihnen Folgendes:

- Eine neue Azure Active Directory-Verwaltungskonsole 
- Neue APIs für Aktivitäts- und Sicherheitsberichte
 
Aufgrund dieser neuen Funktionen werden die Berichts-APIs unter dem Endpunkt **/reports** am 10. Dezember 2017 außer Betrieb genommen. 

---

### <a name="automatic-sign-in-field-detection"></a>Automatische Erkennung von Anmeldefeldern


**Typ:** Korrigiert   
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Einmaliges Anmelden (SSO)  



Azure Active Directory unterstützt die automatische Erkennung von Anmeldefeldern für Anwendungen, die ein HTML-Benutzernamen- und Kennwortfeld rendern.  Diese Schritte sind unter [So erfassen Sie Anmeldefelder für eine Anwendung automatisch](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application) dokumentiert. Sie finden diese Funktion, indem Sie im [Azure-Portal](http://aad.portal.azure.com) auf der Seite **Unternehmensanwendungen** eine *Nicht-Katalog*-Anwendung hinzufügen. Zusätzlich können Sie den Modus **Einmaliges Anmelden** in dieser neuen Anwendung auf **Kennwortbasiertes einmaliges Anmelden** festlegen, indem Sie eine Web-URL eingeben und die Seite anschließend speichern.
 
Aufgrund eines Dienstproblems war diese Funktion vorübergehend deaktiviert. Das Problem wurde behoben, und die automatische Erkennung des Anmeldefelds ist nun wieder verfügbar.

---

### <a name="new-mfa-features"></a>Neue MFA-Features


**Typ:** Neue Funktion  
**Dienstkategorie:** Mehrstufige Authentifizierung (MFA)  
**Produktfunktion:** Identitätssicherheit und -schutz  



Die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) ist ein wesentlicher Bestandteil des Schutzes Ihrer Organisation. Um die Anmeldeinformationen anpassungsfähiger und das Benutzererlebnis nahtloser zu gestalten, wurden die folgenden Funktionen hinzugefügt: 

- Die Integration der Captcha-Ergebnisse bei der mehrstufigen Authentifizierung gehen unmittelbar in den Azure AD-Anmeldebericht ein, einschließlich des programmgesteuerten Zugriffs auf MFA-Ergebnisse.

- Die MFA-Konfiguration wird stärker in die zentrale Azure AD-Konfigurationsumgebung im Azure-Portal integriert.

Mit dieser öffentlichen Vorschau werden MFA-Verwaltung und -Berichterstellung zu einem integrierten Bestandteil der zentralen Azure AD-Konfigurationsumgebung. Durch die Aggregation beider Features können Sie die MFA-Verwaltungsportalfunktionalität in der Azure AD-Oberfläche verwalten.

Weitere Informationen finden Sie in der [Referenz zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal](active-directory-reporting-activity-sign-ins-mfa.md). 


---

### <a name="introducing-terms-of-use"></a>Einführung von Nutzungsbedingungen



**Typ:** Neue Funktion  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance  



Mithilfe von Azure AD-Nutzungsbedingungen bieten eine einfache Möglichkeit zur Informationsanzeige für die Benutzer. Dadurch wird sichergestellt, dass Benutzern relevante Haftungsausschlüsse angezeigt werden, um rechtliche oder compliancebezogene Anforderungen zu erfüllen.

Sie können Azure AD-Nutzungsbedingungen in den folgenden Szenarien verwenden:

- Allgemeine Nutzungsbedingungen für alle Benutzer in Ihrer Organisation 

- Spezifische Nutzungsbedingungen auf Grundlage von Benutzerattributen (beispielsweise Ärzte/Pflegepersonal oder Mitarbeiter im Inland/Ausland unter Verwendung dynamischer Gruppen) 

- Spezifische Nutzungsbedingungen auf Grundlage besonders geschäftsrelevanter Apps (beispielsweise Salesforce)

Weitere Informationen finden Sie unter [Azure Active Directory-Nutzungsbedingungen](active-directory-tou.md).


---

### <a name="enhancements-to-privileged-identity-management"></a>Verbesserungen in Privileged Identity Management


**Typ:** Neue Funktion  
**Dienstkategorie:** Privileged Identity Management (PIM)  
**Produktfunktion:** Privileged Identity Management  


Mit Azure Active Directory Privileged Identity Management (PIM) können Sie nun den Zugriff auf Azure-Ressourcen (Vorschauversion) für folgende Elemente innerhalb Ihrer Organisation verwalten, steuern und überwachen:

- Abonnements
- Ressourcengruppen
- Virtuelle Computer 

Alle Ressourcen im Azure-Portal, die die Azure-Funktion für die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) nutzen, können von sämtlichen Sicherheits- und Lebenszyklusverwaltungsfunktionen profitieren, die Azure AD PIM bereitstellt.

Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen](privileged-identity-management/azure-pim-resource-rbac.md).


---

### <a name="introducing-access-reviews"></a>Einführung von Zugriffsüberprüfungen


**Typ:** Neue Funktion  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Governance  



Mithilfe von Zugriffsüberprüfungen (in der Vorschauphase) können Unternehmen Gruppenmitgliedschaften und den Zugriff auf Unternehmensanwendungen effizient verwalten: 

- Sie können den Gastbenutzerzugriff erneut bestätigen, indem Sie den Zugriff auf Anwendungen und Gruppenmitgliedschaften im Rahmen einer Zugriffsüberprüfung überprüfen. Anhand der Erkenntnisse aus den Zugriffsüberprüfungen können Prüfer effizient entscheiden, ob Gäste weiterhin Zugriff haben sollen.

- Mithilfe von Zugriffsüberprüfungen können Sie den Anwendungszugriff von Mitarbeitern sowie deren Gruppenmitgliedschaften neu zertifizieren.

Die Kontrollen der Zugriffsüberprüfung können in unternehmensrelevanten Programmen zusammengefasst werden, um Überprüfungen für compliance- oder risikokritische Anwendungen nachzuverfolgen.

Weitere Informationen finden Sie unter [Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-access-reviews-overview.md).


---

### <a name="hiding-third-party-applications-from-my-apps-and-the-office-365-launcher"></a>Möglichkeit zum Ausblenden von Drittanbieteranwendungen in „Meine Apps“ und im Office 365-Startprogramm



**Typ:** Neue Funktion  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Einmaliges Anmelden (SSO)  



Mit der neuen Eigenschaft **App ausblenden** können Sie nun Apps, die in Ihren Benutzerportalen angezeigt werden, besser verwalten. Das Ausblenden von Apps ist nützlich, wenn App-Kacheln für Back-End-Dienste oder doppelte Kacheln angezeigt werden, durch die der App-Programmstarter der Benutzer unübersichtlich wird. Die Umschaltfläche befindet sich im Abschnitt „Eigenschaften“ der Drittanbieter-App und hat die Bezeichnung **Für Benutzer sichtbar?**. Sie können eine App auch programmgesteuert über PowerShell ausblenden. 

Weitere Informationen finden Sie unter [Ausblenden einer Drittanbieteranwendung auf der Benutzeroberfläche in Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Was ist verfügbar?**

 Im Rahmen der Umstellung auf die neue Verwaltungskonsole stehen zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Die Daten, die bisher in den Sicherheitsberichten verfügbar waren, können jetzt über die API für Identity Protection-Risikoereignisse in Microsoft Graph abgerufen werden.


## <a name="september-2017"></a>September 2017

### <a name="hotfix-for-microsoft-identity-manager"></a>Hotfix für Microsoft Identity Manager


**Typ:** Geänderte Funktion  
**Dienstkategorie:** Microsoft Identity Manager  
**Produktfunktion:** Identity Lifecycle Management  



Ein Hotfixrollup-Paket (Build 4.4.1642.0) ist ab dem 25. September 2017 für Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1) verfügbar. Dieser Rolluppaket enthält Folgendes:

- Problembehebungen und Verbesserungen
- Ein kumulatives Update, das alle MIM 2016 SP1-Updates bis Build 4.4.1459.0 für Microsoft Identity Manager 2016 ersetzt. 
- **Microsoft Identity Manager 2016 Build 4.4.1302.0** wird vorausgesetzt. 

Weitere Informationen finden Sie unter [Hotfixrollup-Paket (Build 4.4.1642.0) ist für Microsoft Identity Manager 2016 SP1 verfügbar](https://support.microsoft.com/help/4021562). 

---
