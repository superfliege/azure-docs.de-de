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
ms.date: 11/06/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 766ab10c853eb56a1203686d21d0e131c44a41b5
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
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

**Typ:** Veraltete Funktionen  
**Dienstkategorie:** ACS  
**Produktfunktion:** Access Control Service 

<a name="acs-retirement"></a>

Microsoft Azure Active Directory Access Control (auch als Access Control Service oder ACS bezeichnet) wird Ende 2018 eingestellt.  Weitere Informationen, u.a. ein ausführlicher Zeitplan und allgemeine Migrationsanweisungen, werden in den nächsten Wochen bereitgestellt. Hinterlassen Sie in der Zwischenzeit Kommentare auf dieser Seite, wenn Sie Fragen zu ACS haben. Ein Mitglied unseres Teams wird sich dann mit Ihnen in Verbindung setzen.

---


## <a name="october-2017"></a>Oktober 2017

**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Identity Lifecycle Management  


**APIs für Azure AD-Berichte (Betaversion) unter dem `https://graph.windows.net/<tenant-name>/reports/` Knoten** als veraltet markiert

Azure-Portal bietet Ihnen Folgendes:

- Eine neue Azure Active Directory-Verwaltungskonsole 
- Neue APIs für Aktivitäts- und Sicherheitsberichte
 
Aufgrund dieser neuen Funktionen werden die Berichts-APIs unter dem Endpunkt **/reports** am 10. Dezember 2017 außer Betrieb genommen. 

---

**Typ:** Korrigiert   
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Einmaliges Anmelden (SSO)  


Azure Active Directory unterstützt die automatische Erkennung von Anmeldefeldern für Anwendungen, die ein HTML-Benutzernamen- und Kennwortfeld rendern.  Diese Schritte sind unter [So erfassen Sie Anmeldefelder für eine Anwendung automatisch](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application) dokumentiert. Sie finden diese Funktion, indem Sie im [Azure-Portal](http://aad.portal.azure.com) auf der Seite **Unternehmensanwendungen** eine *Nicht-Katalog*-Anwendung hinzufügen. Zusätzlich können Sie den Modus **Einmaliges Anmelden** in dieser neuen Anwendung auf **Kennwortbasiertes einmaliges Anmelden** festlegen, indem Sie eine Web-URL eingeben und die Seite anschließend speichern.
 
Aufgrund eines Dienstproblems war diese Funktion vorübergehend deaktiviert. Das Problem wurde behoben, und die automatische Erkennung des Anmeldefelds ist nun wieder verfügbar.

---

**Typ:** Neue Funktion  
**Dienstkategorie:** Mehrstufige Authentifizierung (MFA)  
**Produktfunktion:** Identitätssicherheit und -schutz  


In unserer Welt ist die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) ein wesentlicher Bestandteil des Schutzes Ihrer Organisation. Das für Identitätsschutz zuständige Team von Microsoft entwickelt die mehrstufige Authentifizierung weiter, um die Anmeldeinformationen anpassungsfähiger und die Oberfläche intuitiver zu gestalten. Heute freue ich mich, zwei wichtige Schritte auf diesem Weg ankündigen zu können: 

- Die Integration der Captcha-Ergebnisse bei der mehrstufigen Authentifizierung gehen unmittelbar in den Azure AD-Anmeldebericht ein, einschließlich des programmgesteuerten Zugriffs auf MFA-Ergebnisse.

- Tiefgreifendere Integration der MFA-Konfiguration in die zentrale Azure AD-Konfigurationsumgebung im Azure-Portal

Mit dieser Public Preview sind MFA-Verwaltung und -Berichterstellung ein integraler Bestandteil der zentralen Azure AD-Konfigurationsumgebung, sodass Sie die MFA-Verwaltungsportalfunktionalität auf der Azure AD-Oberfläche verwalten können.

Weitere Informationen finden Sie in der [Referenz zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal](active-directory-reporting-activity-sign-ins-mfa.md). 


---
**Typ:** Neue Funktion  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance  


In den **Azure AD-Nutzungsbedingungen** können Sie Endbenutzern Informationen einfach anzeigen. Dadurch wird sichergestellt, dass Benutzern relevante Haftungsausschlüsse angezeigt werden, um rechtliche oder compliancebezogene Anforderungen zu erfüllen.

Sie können Azure AD-Nutzungsbedingungen in den folgenden Szenarien verwenden:

- Allgemeine Nutzungsbedingungen für alle Benutzer in Ihrer Organisation 

- Spezifische Nutzungsbedingungen auf Grundlage von Benutzerattributen (beispielsweise Ärzte/Pflegepersonal oder Mitarbeiter im Inland/Ausland unter Verwendung dynamischer Gruppen) 

- Spezifische Nutzungsbedingungen auf Grundlage besonders geschäftsrelevanter Apps (beispielsweise Salesforce)

Weitere Informationen finden Sie unter [Azure Active Directory-Nutzungsbedingungen](active-directory-tou.md).


---
**Typ:** Neue Funktion  
**Dienstkategorie:** Privileged Identity Management (PIM)  
**Produktfunktion:** Privileged Identity Management  


Mit Azure Active Directory Privileged Identity Management (PIM) können Sie nun den Zugriff auf Azure-Ressourcen (Vorschauversion) innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dies schließt Abonnements, Ressourcengruppen und sogar virtuelle Computer ein. Alle Ressourcen im Azure-Portal, die die Azure-Funktion für die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) nutzen, können sowohl von den großartigen Sicherheits- und Lebenszyklusverwaltungsfunktionen profitieren, die Azure AD PIM zu bieten hat, als auch von einigen neuen Features, die in Kürze für Azure AD-Rollen eingeführt werden.

Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen](privileged-identity-management/azure-pim-resource-rbac.md).


---
**Typ:** Neue Funktion  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Governance  


Mithilfe von Zugriffsüberprüfungen (in der Vorschauphase) können Unternehmen Gruppenmitgliedschaften und den Zugriff auf Unternehmensanwendungen effizient verwalten: 

- Sie können den Gastbenutzerzugriff erneut bestätigen, indem Sie den Zugriff auf Anwendungen und Gruppenmitgliedschaften im Rahmen einer Zugriffsüberprüfung überprüfen. Anhand der Erkenntnisse aus den Zugriffsüberprüfungen können Prüfer effizient entscheiden, ob Gäste weiterhin Zugriff haben sollen.

- Mithilfe von Zugriffsüberprüfungen können Sie den Anwendungszugriff von Mitarbeitern sowie deren Gruppenmitgliedschaften neu zertifizieren.

Die Kontrollen der Zugriffsüberprüfung können in unternehmensrelevanten Programmen zusammengefasst werden, um Überprüfungen für compliance- oder risikokritische Anwendungen nachzuverfolgen.

Weitere Informationen finden Sie unter [Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-access-reviews-overview.md).


---
**Typ:** Neue Funktion  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Einmaliges Anmelden (SSO)  


**Möglichkeit zum Ausblenden von Anwendungen von Drittanbietern in „Meine Apps“ und im Office 365-Programmstarter**

Mit der neuen Eigenschaft **App ausblenden** können Sie nun Apps, die in Ihren Benutzerportalen angezeigt werden, besser verwalten. Dies hilft bei Fällen, in denen App-Kacheln für Back-End-Dienste oder duplizierte Kacheln angezeigt werden, die die App-Programmstarter der Benutzer in Unordnung bringen. Die Umschaltfläche befindet sich im Abschnitt „Eigenschaften“ der Drittanbieter-App und hat die Bezeichnung **Für Benutzer sichtbar?**. Sie können eine App auch programmgesteuert über PowerShell ausblenden. 

Weitere Informationen finden Sie unter [Ausblenden einer Drittanbieteranwendung auf der Benutzeroberfläche in Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Was ist verfügbar?**

 Im Rahmen der Umstellung auf die neue Verwaltungskonsole haben wir zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung gestellt. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Die Daten, die bisher in den Sicherheitsberichten verfügbar waren, können jetzt über die API für Identity Protection-Risikoereignisse in Microsoft Graph abgerufen werden.


## <a name="september-2017"></a>September 2017

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Microsoft Identity Manager  
**Produktfunktion:** Identity Lifecycle Management  


Ein Hotfixrollup-Paket (Build 4.4.1642.0) ist ab dem 25. September 2017 für Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1) verfügbar. Dieser Rolluppaket enthält Folgendes:

- Problembehebungen und Verbesserungen
- Ein kumulatives Update, das alle MIM 2016 SP1-Updates bis Build 4.4.1459.0 für Microsoft Identity Manager 2016 ersetzt. 
- **Microsoft Identity Manager 2016 Build 4.4.1302.0** wird vorausgesetzt. 

Weitere Informationen finden Sie unter [Hotfixrollup-Paket (Build 4.4.1642.0) ist für Microsoft Identity Manager 2016 SP1 verfügbar](https://support.microsoft.com/en-us/help/4021562). 

---
