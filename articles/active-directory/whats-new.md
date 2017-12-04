---
title: Neuerungen Anmerkungen zur Version Azure Active Directory Premium | Microsoft-Dokumentation
description: "Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory (Azure AD), u.a. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9de535b2fb70181c68fb698e847dd8361bf54385
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2017
---
# <a name="whats-new-in-azure-active-directory"></a>Neuerungen in Azure Active Directory




> Abonnieren Sie diesen [Feed](https://docs.microsoft.com/api/search/rss?search=%22what%27s%20new%20in%20azure%20active%20directory%3F%22&locale=en-us) in Ihrem bevorzugten RSS-Feedreader, um in Bezug auf Neuerungen in Azure Active Directory auf dem Laufenden zu bleiben.



Wir nehmen laufend Verbesserungen an Azure Active Directory vor. Damit Sie bei den neuesten Entwicklungen auf dem neuesten Stand bleiben, bietet dieses Thema Informationen zu Folgendem:

-   Neueste Versionen 
-   Bekannte Probleme 
-   Fehlerbehebungen 
-   Veraltete Funktionen 
-   Pläne für Änderungen 

Besuchen Sie diese Seite regelmäßig, da wir sie monatlich aktualisieren.

## <a name="november-2017"></a>November 2017
 


**Typ:** Plan für Änderung  
**Dienstkategorie:** ACS  
**Produktfunktion:** Access Control Service 

**Außerbetriebnahme von ACS**

Microsoft Azure Active Directory Access Control (auch als Access Control Service oder ACS bezeichnet) wird Ende 2018 eingestellt.  Weitere Informationen, u.a. ein ausführlicher Zeitplan und allgemeine Migrationsanweisungen, werden in den nächsten Wochen bereitgestellt. Hinterlassen Sie in der Zwischenzeit Kommentare auf dieser Seite, wenn Sie Fragen zu ACS haben. Ein Mitglied unseres Teams wird sich dann mit Ihnen in Verbindung setzen.

---



**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz


**Einschränkung des Browserzugriffs auf Intune Managed Browser** 


Durch dieses Verhalten können Sie den Browserzugriff auf Office 365 und weitere Azure AD-verbundene Cloud-Apps mithilfe von Intune Managed Browser als genehmigte App einschränken. 

Diese Änderung erlaubt es Ihnen, die folgende Bedingung für den anwendungsbasierten bedingten Zugriff zu konfigurieren:

**Client-Apps:**  Browser

**Welche Auswirkung hat die Änderung?**

Der Zugriff wird blockiert, wenn diese Bedingung verwendet wird. Sobald die Vorschau dieses Verhaltens verfügbar ist, ist für den gesamten Zugriff die Verwendung der Managed Browser-Anwendung erforderlich. 

Achten Sie in kommenden Blogbeiträgen und Versionshinweisen auf Informationen zu dieser Funktionalität. 

Weitere Informationen finden Sie unter [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md).

 
---


 
**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz


**Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD**


Die folgenden Apps sind zur Aufnahme in die Liste der [genehmigten Client-Apps](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) vorgesehen:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)

- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Weitere Informationen finden Sie unter:

- [Genehmigte Client-App als Voraussetzung](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [App-basierter bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-mam.md)


---


**Typ:** Neue Funktion    
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance/Compliance



**Unterstützung von Nutzungsbedingungen für mehrere Sprachen**


Administratoren können jetzt neue Nutzungsbedingungen erstellen, die mehrere PDF-Dokumente enthalten. Diese PDF-Dateien können mit einer zugehörigen Sprache gekennzeichnet werden. Benutzern im Geltungsbereich wird dann die PDF-Datei angezeigt, die der Sprache in den Benutzereinstellungen entspricht. Falls keine Übereinstimmung vorliegt, wird die Standardsprache angezeigt.


---
 


**Typ:** Neue Funktion  
**Dienstkategorie:** SSPR  
**Produktfunktion:** Benutzerauthentifizierung


**Echtzeitclientstatus beim Kennwortrückschreiben**
 

Es ist jetzt möglich, den Status Ihres lokalen Clients für das Kennwortrückschreiben anzuzeigen. Diese Option steht im Abschnitt **Lokale Integration** auf der Seite **[Kennwortzurücksetzung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)** zur Verfügung. 

Wenn Verbindungsprobleme mit Ihrem lokalen Client für das Kennwortrückschreiben vorliegen, wird eine Fehlermeldung angezeigt, die Folgendes enthält:

- Informationen dazu, warum keine Verbindung mit dem lokalen Client für das Kennwortrückschreiben möglich ist 
- Ein Link zur Dokumentation, die Sie bei der Problembehandlung unterstützt 


Weitere Informationen finden Sie unter [Lokale Integration](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---
 
**Typ:** Neue Funktion  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Identitätssicherheit und -schutz



**App-basierter bedingter Zugriff mit Azure AD** 


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



**Typ:** Neue Funktion  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Identitätssicherheit und -schutz

 

**Verwalten von Azure AD-Geräten im Azure-Portal**


Ab jetzt finden Sie alle mit Azure AD verbundenen Geräte sowie die gerätebezogenen Aktivitäten an einem Ort. Es steht eine neue Verwaltungsoberfläche zur Verfügung, mit der Sie all ihre Geräteidentitäten und -einstellungen im Azure-Portal verwalten können. Diese Version ermöglicht Folgendes:

- Anzeigen all Ihrer Geräte, die für den bedingten Zugriff in Azure AD zur Verfügung stehen

- Anzeigen von Eigenschaften, einschließlich von Geräten, die in Hybrid-Azure AD eingebunden sind

- Auffinden von BitLocker-Schlüsseln für Ihre in Azure AD eingebundenen Geräte, Verwalten Ihrer Geräte mit Intune und vieles mehr

- Verwalten von Azure AD-gerätebezogenen Einstellungen


Weitere Informationen finden Sie unter [Verwalten von Geräten mit dem Azure-Portal](device-management-azure-portal.md).



 
---


**Typ:** Neue Funktion    
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz 



**Unterstützung der macOS-Geräteplattform für den bedingten Azure AD-Zugriff** 
 

Ab sofort können Sie macOS als Geräteplattformbedingung in Ihre Richtlinie für den bedingten Azure AD-Zugriff einschließen (oder ausschließen). Durch das Hinzufügen von macOS als unterstützte Geräteplattform haben Sie folgende Möglichkeiten:

- **Registrieren und Verwalten von macOS-Geräten mit Intune**: Ähnlich wie für andere Plattformen (z.B. iOS und Android) steht für macOS eine Unternehmensportalanwendung bereit, um einheitliche Registrierungen durchzuführen. Die neue Unternehmensportal-App für macOS ermöglicht es Ihnen, ein Gerät bei Intune und Azure AD zu registrieren.
 
- **Sicherstellen, dass macOS-Geräte den in Intune definierten Konformitätsrichtlinien Ihrer Organisation entsprechen**: Im Azure-Portal für Intune können Sie ab sofort Konformitätsrichtlinien für macOS-Geräte einrichten. 
  
- **Einschränken des Anwendungszugriffs in Azure AD auf konforme macOS-Geräte**: Bei der Erstellung von Richtlinien für den bedingten Zugriff steht eine separate Option für macOS als Geräteplattform zur Verfügung. Diese Option macht es möglich, macOS-spezifische Richtlinien für den bedingten Zugriff für die festgelegte Zielanwendung in Azure zu erstellen.

Weitere Informationen finden Sie unter:

- [Erstellen einer Gerätekonformitätsrichtlinie für macOS-Geräte mit Intune](https://aka.ms/macoscompliancepolicy)
- [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md)


 
---


**Typ:** Neue Funktion    
**Dienstkategorie:** Mehrstufige Authentifizierung (MFA)  
**Produktfunktion:** Benutzerauthentifizierung


**NPS-Erweiterung für Azure MFA** 


Die NPS-Erweiterung (Network Policy Server, Netzwerkrichtlinienserver) für Azure MFA fügt Ihrer Authentifizierungsinfrastruktur unter Verwendung Ihrer vorhandenen Server cloudbasierte MFA-Funktionen hinzu. Mit der NPS-Erweiterung können Sie Ihrem bestehenden Authentifizierungsvorgang eine Überprüfung per Telefonanruf, SMS oder Telefon-App hinzufügen, ohne neue Server installieren, konfigurieren und verwalten zu müssen. 

Diese Erweiterung wurde für Organisationen entwickelt, die VPN-Verbindungen schützen möchten, ohne den Azure MFA-Server bereitzustellen. Die NPS-Erweiterung fungiert als Adapter zwischen RADIUS und cloudbasierter Azure MFA, um eine zweite Stufe der Authentifizierung für Verbund- oder synchronisierte Benutzer bereitzustellen.


Weitere Informationen finden Sie unter [Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md).

 
---


**Typ:** Neue Funktion    
**Dienstkategorie:** Benutzerverwaltung  
**Produktfunktion:** Verzeichnis 


**Wiederherstellen oder dauerhaftes Entfernen gelöschter Benutzer**


Im Azure AD Admin Center haben Sie jetzt folgende Möglichkeiten:

- Wiederherstellen eines gelöschten Benutzers 
- Dauerhaftes Löschen eines Benutzers 


**So können Sie es ausprobieren**

1. Wählen Sie im Azure AD Admin Center im Abschnitt **Verwalten** die Option [**Alle Benutzer**](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) aus. 

2. Wählen Sie in der Liste **Anzeigen** die Option **Kürzlich gelöschte Benutzer** aus. 

4. Wählen Sie mindestens einen kürzlich gelöschten Benutzer aus, und stellen Sie den Benutzer dann entweder wieder her, oder löschen Sie ihn dauerhaft.

 
---



 
**Typ:** Geänderte Funktion  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

**Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD**


Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) hinzugefügt:

- Microsoft Planner

- Microsoft Azure Information Protection 


Weitere Informationen finden Sie unter:

- [Genehmigte Client-App als Voraussetzung](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [App-basierter bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-mam.md)


---



**Typ:** Geänderte Funktion    
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz


**Möglichkeit zum Einfügen von „OR“ zwischen Steuerelementen in einer Richtlinie für den bedingten Zugriff** 
 
Ab sofort besteht die Möglichkeit, „OR“ (eines der ausgewählten Steuerelemente ist erforderlich) zwischen Steuerelementen für den bedingten Zugriff einzufügen. Durch dieses Feature können Sie Richtlinien mit einem **OR** zwischen Steuerelementen für den Zugriff erstellen. Beispielsweise können Sie mit diesem Feature eine Richtlinie erstellen, durch die der Benutzer sich entweder über die mehrstufige Authentifizierung anmelden **OR** (oder) ein konformes Gerät verwenden muss.

Weitere Informationen finden Sie unter [Steuerelemente beim bedingten Zugriff mit Azure Active Directory](active-directory-conditional-access-controls.md).

 
---



**Typ:** Geänderte Funktion    
**Dienstkategorie:** Identitätsschutz  
**Produktfunktion:** Identitätssicherheit und -schutz

**Aggregation von Echtzeitrisikoereignissen**

Für eine verbesserte Benutzerfreundlichkeit bei der Verwaltung werden jetzt in Azure AD Identity Protection alle Echtzeitrisikoereignisse, die von derselben IP-Adresse stammen, für einen vorgegebenen Tag für jeden Risikoereignistyp aggregiert. Durch diese Änderung werden weniger Risikoereignisse ohne Änderung der Benutzersicherheit angezeigt.

Die zugrunde liegende Echtzeiterkennung erfolgt bei jeder Benutzeranmeldung. Wenn Sie eine Sicherheitsrichtlinie für das Anmelderisiko auf MFA oder eine Blockierung des Zugriffs festgelegt haben, wird diese bei jeder riskanten Anmeldung weiterhin ausgelöst.

 
---
 




## <a name="october-2017"></a>Oktober 2017

**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Identity Lifecycle Management  


**Einstellung von Azure AD-Berichten**

Azure-Portal bietet Ihnen Folgendes:

- Eine neue Azure Active Directory-Verwaltungskonsole 
- Neue APIs für Aktivitäts- und Sicherheitsberichte
 
Aufgrund dieser neuen Funktionen werden die Berichts-APIs unter dem Endpunkt **/reports** am 10. Dezember 2017 außer Betrieb genommen. 

---

**Typ:** Korrigiert   
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Einmaliges Anmelden (SSO)  


**Automatische Erkennung von Anmeldefeldern**


Azure Active Directory unterstützt die automatische Erkennung von Anmeldefeldern für Anwendungen, die ein HTML-Benutzernamen- und Kennwortfeld rendern.  Diese Schritte sind unter [So erfassen Sie Anmeldefelder für eine Anwendung automatisch](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application) dokumentiert. Sie finden diese Funktion, indem Sie im [Azure-Portal](http://aad.portal.azure.com) auf der Seite **Unternehmensanwendungen** eine *Nicht-Katalog*-Anwendung hinzufügen. Zusätzlich können Sie den Modus **Einmaliges Anmelden** in dieser neuen Anwendung auf **Kennwortbasiertes einmaliges Anmelden** festlegen, indem Sie eine Web-URL eingeben und die Seite anschließend speichern.
 
Aufgrund eines Dienstproblems war diese Funktion vorübergehend deaktiviert. Das Problem wurde behoben, und die automatische Erkennung des Anmeldefelds ist nun wieder verfügbar.

---

**Typ:** Neue Funktion  
**Dienstkategorie:** Mehrstufige Authentifizierung (MFA)  
**Produktfunktion:** Identitätssicherheit und -schutz  


**Neue MFA-Features**

Die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) ist ein wesentlicher Bestandteil des Schutzes Ihrer Organisation. Um die Anmeldeinformationen anpassungsfähiger und das Benutzererlebnis nahtloser zu gestalten, wurden die folgenden Funktionen hinzugefügt: 

- Die Integration der Captcha-Ergebnisse bei der mehrstufigen Authentifizierung gehen unmittelbar in den Azure AD-Anmeldebericht ein, einschließlich des programmgesteuerten Zugriffs auf MFA-Ergebnisse.

- Die MFA-Konfiguration wird stärker in die zentrale Azure AD-Konfigurationsumgebung im Azure-Portal integriert.

Mit dieser öffentlichen Vorschau werden MFA-Verwaltung und -Berichterstellung zu einem integrierten Bestandteil der zentralen Azure AD-Konfigurationsumgebung. Durch die Aggregation beider Features können Sie die MFA-Verwaltungsportalfunktionalität in der Azure AD-Oberfläche verwalten.

Weitere Informationen finden Sie in der [Referenz zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal](active-directory-reporting-activity-sign-ins-mfa.md). 


---
**Typ:** Neue Funktion  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance  


**Einführung von Nutzungsbedingungen**

Mithilfe von Azure AD-Nutzungsbedingungen bieten eine einfache Möglichkeit zur Informationsanzeige für die Benutzer. Dadurch wird sichergestellt, dass Benutzern relevante Haftungsausschlüsse angezeigt werden, um rechtliche oder compliancebezogene Anforderungen zu erfüllen.

Sie können Azure AD-Nutzungsbedingungen in den folgenden Szenarien verwenden:

- Allgemeine Nutzungsbedingungen für alle Benutzer in Ihrer Organisation 

- Spezifische Nutzungsbedingungen auf Grundlage von Benutzerattributen (beispielsweise Ärzte/Pflegepersonal oder Mitarbeiter im Inland/Ausland unter Verwendung dynamischer Gruppen) 

- Spezifische Nutzungsbedingungen auf Grundlage besonders geschäftsrelevanter Apps (beispielsweise Salesforce)

Weitere Informationen finden Sie unter [Azure Active Directory-Nutzungsbedingungen](active-directory-tou.md).


---
**Typ:** Neue Funktion  
**Dienstkategorie:** Privileged Identity Management (PIM)  
**Produktfunktion:** Privileged Identity Management  

**Verbesserungen in Privileged Identity Management**

Mit Azure Active Directory Privileged Identity Management (PIM) können Sie nun den Zugriff auf Azure-Ressourcen (Vorschauversion) für folgende Elemente innerhalb Ihrer Organisation verwalten, steuern und überwachen:

- Abonnements
- Ressourcengruppen
- Virtuelle Computer 

Alle Ressourcen im Azure-Portal, die die Azure-Funktion für die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) nutzen, können von sämtlichen Sicherheits- und Lebenszyklusverwaltungsfunktionen profitieren, die Azure AD PIM bereitstellt.

Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen](privileged-identity-management/azure-pim-resource-rbac.md).


---
**Typ:** Neue Funktion  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Governance  

**Einführung von Zugriffsüberprüfungen**


Mithilfe von Zugriffsüberprüfungen (in der Vorschauphase) können Unternehmen Gruppenmitgliedschaften und den Zugriff auf Unternehmensanwendungen effizient verwalten: 

- Sie können den Gastbenutzerzugriff erneut bestätigen, indem Sie den Zugriff auf Anwendungen und Gruppenmitgliedschaften im Rahmen einer Zugriffsüberprüfung überprüfen. Anhand der Erkenntnisse aus den Zugriffsüberprüfungen können Prüfer effizient entscheiden, ob Gäste weiterhin Zugriff haben sollen.

- Mithilfe von Zugriffsüberprüfungen können Sie den Anwendungszugriff von Mitarbeitern sowie deren Gruppenmitgliedschaften neu zertifizieren.

Die Kontrollen der Zugriffsüberprüfung können in unternehmensrelevanten Programmen zusammengefasst werden, um Überprüfungen für compliance- oder risikokritische Anwendungen nachzuverfolgen.

Weitere Informationen finden Sie unter [Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-access-reviews-overview.md).


---
**Typ:** Neue Funktion  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Einmaliges Anmelden (SSO)  


**Möglichkeit zum Ausblenden von Drittanbieteranwendungen in „Meine Apps“ und im Office 365-Programmstarter**

Mit der neuen Eigenschaft **App ausblenden** können Sie nun Apps, die in Ihren Benutzerportalen angezeigt werden, besser verwalten. Das Ausblenden von Apps ist nützlich, wenn App-Kacheln für Back-End-Dienste oder doppelte Kacheln angezeigt werden, durch die der App-Programmstarter der Benutzer unübersichtlich wird. Die Umschaltfläche befindet sich im Abschnitt „Eigenschaften“ der Drittanbieter-App und hat die Bezeichnung **Für Benutzer sichtbar?**. Sie können eine App auch programmgesteuert über PowerShell ausblenden. 

Weitere Informationen finden Sie unter [Ausblenden einer Drittanbieteranwendung auf der Benutzeroberfläche in Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Was ist verfügbar?**

 Im Rahmen der Umstellung auf die neue Verwaltungskonsole stehen zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Die Daten, die bisher in den Sicherheitsberichten verfügbar waren, können jetzt über die API für Identity Protection-Risikoereignisse in Microsoft Graph abgerufen werden.


## <a name="september-2017"></a>September 2017

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Microsoft Identity Manager  
**Produktfunktion:** Identity Lifecycle Management  


**Hotfix für Microsoft Identity Manager**

Ein Hotfixrollup-Paket (Build 4.4.1642.0) ist ab dem 25. September 2017 für Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1) verfügbar. Dieser Rolluppaket enthält Folgendes:

- Problembehebungen und Verbesserungen
- Ein kumulatives Update, das alle MIM 2016 SP1-Updates bis Build 4.4.1459.0 für Microsoft Identity Manager 2016 ersetzt. 
- **Microsoft Identity Manager 2016 Build 4.4.1302.0** wird vorausgesetzt. 

Weitere Informationen finden Sie unter [Hotfixrollup-Paket (Build 4.4.1642.0) ist für Microsoft Identity Manager 2016 SP1 verfügbar](https://support.microsoft.com/en-us/help/4021562). 

---
