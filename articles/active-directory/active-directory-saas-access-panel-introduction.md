---
title: Was ist der Zugriffsbereich in Azure Active Directory? | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Varianten des Zugriffsbereichs (Webbrowser, Android-App, iPhone- und iPad-App) zum Zugreifen auf SaaS-Apps verwenden.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/18
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eff93b37be2ff770b90518f886bd4b54fa0ca2a1
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868989"
---
# <a name="what-is-the-access-panel"></a>Was ist der Zugriffsbereich?

Der Zugriffsbereich ist ein webbasiertes Portal. Wenn Sie über ein Geschäfts-, Schul- oder Unikonto in Azure Active Directory (Azure AD) verfügen, können Sie über den Zugriffsbereich cloudbasierte Anwendungen anzeigen und starten, für die Ihnen ein Azure AD-Administrator Zugriff gewährt hat. Sie können über den Zugriffsbereich auch Self-Service-Funktionen für die Gruppen- und App-Verwaltung nutzen.

Der Zugriffsbereich ist vom Azure-Portal getrennt. Er kann auch ohne Azure-Abonnement verwendet werden.

![Zugriffsbereich][1] Über den Zugriffsbereich können Sie einige Ihrer Profileinstellungen bearbeiten und folgende Aktionen durchführen:

- Ändern des Kennworts, das einem Geschäfts-, Schul- oder Unikonto zugeordnet ist.

- Bearbeiten der Einstellungen zum Zurücksetzen des Kennworts.

- Bearbeiten der Kontakt- und Voreinstellungen, die sich auf die Multi-Factor Authentication beziehen (für Konten, für die dies vom Administrator verlangt wird).

- Anzeigen von Kontodetails wie Benutzer-ID, alternative E-Mail-Adresse, Mobiltelefonnummer, geschäftliche Telefonnummer und Geräte.

- Anzeigen und Starten von cloudbasierten Anwendungen, für die Ihnen der Azure AD-Administrator Zugriff gewährt hat. 

- Selbstständiges Verwalten von Gruppen. Administratoren können Sicherheitsgruppen erstellen und verwalten und die Mitgliedschaft in Sicherheitsgruppen in Azure AD anfordern. Weitere Informationen finden Sie unter [Self-Service-Gruppenverwaltung für Benutzer in Azure AD](users-groups-roles/groups-self-service-management.md) und [Verwalten Ihrer Gruppen](fundamentals/active-directory-manage-groups.md).




## <a name="access-the-access-panel"></a>Zugreifen auf den Zugriffsbereich

Auf den Zugriffsbereich können Sie unter `http://myapps.microsoft.com` zugreifen.

Wenn Sie benutzerdefiniertes Branding für Ihre Anmeldeseite konfiguriert haben, können Sie das Branding laden, indem Sie die Domäne Ihrer Organisation an die URL anhängen (Beispiel: `http://myapps.microsoft.com/<your domain>.com`).

Sie können jeden aktiven oder überprüften Domänennamen verwenden, der in Ihrem Azure-Portal konfiguriert wurde, wie hier zu sehen: ![Wingtip Toys-Domänenname][2]  

Geben Sie diese URL an alle Benutzer weiter, die sich bei den in Azure AD integrierten Anwendungen anmelden.

## <a name="authentication"></a>Authentifizierung

Zum Erreichen des Zugriffsbereichs müssen Sie mit einem Geschäfts-, Schul- oder Unikonto in Azure AD authentifiziert sein. Sie können direkt bei Azure AD authentifiziert werden. Wenn Ihre Organisation mithilfe der Active Directory-Verbunddienste (AD FS) oder anderen Technologien einen Verbund konfiguriert hat, können Sie alternativ dazu auch über Windows Server Active Directory authentifiziert werden.

Wenn Sie über ein Abonnement für Azure oder Office 365 verfügen und das Azure-Portal oder eine Office 365-Anwendung verwendet haben, können Sie die Anwendungsliste ohne erneute Anmeldung anzeigen. Falls Sie nicht authentifiziert sind, werden Sie aufgefordert, sich mit dem Benutzernamen und Kennwort für Ihr Konto in Azure AD anzumelden. Wenn Ihre Organisation einen Verbund konfiguriert hat, reicht die Eingabe des Benutzernamens aus.

Nach der Authentifizierung können Sie mit den Anwendungen interagieren, die Ihr Administrator in das Verzeichnis integriert hat. Informationen zum Integrieren von Anwendungen in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="web-browser-requirements"></a>Webbrowseranforderungen

Die Mindestanforderung für den Zugriffsbereich ist ein Browser mit JavaScript-Unterstützung und CSS-Aktivierung. Zur Verwendung von kennwortbasiertem einmaligem Anmelden (Single Sign-On, SSO) bei Anwendungen muss in Ihrem Browser die Erweiterung für den Zugriffsbereich installiert sein. Die Erweiterung wird automatisch heruntergeladen, wenn Sie eine Anwendung auswählen, die für kennwortbasiertes einmaliges Anmelden konfiguriert ist.

Das Installationsprogramm ist architekturspezifisch. Wenn Sie auf Downloadlink klicken, erhalten Sie nur das Installationsprogramm für die BS-Architektur, die Sie derzeit verwenden. Wenn Sie ein Anwendungsadministrator für die Bereitstellung sind, achten Sie darauf, den Downloadlink auf einem 64-Bit- und einem 32-Bit-Gerät aufzurufen, um beide Installationsdateien abzurufen.


Die Zugriffsbereichserweiterung ist derzeit verfügbar für:
- **Microsoft Edge:** Unter Windows 10 Anniversary Edition oder höher 
- **Chrome:** Unter Windows 7 oder höher und MacOS X oder höher
- **Firefox 26.0 oder höher:** Unter Windows XP SP2 oder höher und Mac OS X 10.6 oder höher
- **Internet Explorer 11:** Unter Windows 7 oder höher (eingeschränkte Unterstützung)

## <a name="my-apps-secure-sign-in-extension"></a>Erweiterung zur sicheren Anmeldung bei „Meine Apps“
Diese Erweiterung muss für kennwortbasiertes einmaliges Anmelden verwendet werden. Nach der Installation der Erweiterung können Sie sich anmelden, um zusätzliche Features zu aktivieren. Klicken Sie hierzu auf **Melden Sie sich an, um loszulegen**. 

- Über die **Anmelde-URL** einer App können Sie sich direkt bei der App anmelden. Wenn Sie die URL der App verwenden, erkenn die Erweiterung die Aktion und ermöglicht die Anmeldung über die Erweiterung.
- Mit der *Schnellsuche* der Erweiterung können Sie jede Ihrer Apps über den Zugriffsbereich starten. 
- Im Bereich **Zuletzt verwendet** werden die drei zuletzt gestarteten Anwendungen angezeigt.
- Beim Remotezugriff über den [Anwendungsproxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started) können Sie interne Unternehmens-URLs verwenden.

> [!NOTE]
> Zusätzliche Features sind nur für Microsoft Edge, Chrome und Firefox verfügbar.
>
Sie können die Erweiterung direkt über die folgenden Websites herunterladen:
- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

Falls sich Ihre Meine Apps-URL von `https://myapps.microsoft.com` unterscheidet, konfigurieren Ihre Standard-URL wie folgt:
1. Klicken Sie mit der rechten Maustaste auf das Symbol für die Erweiterung, *ohne* bei der Erweiterung angemeldet zu sein.
2. Klicken Sie im Menü auf **Meine Apps-URL**.
3. Wählen Sie Ihre Standard-URL aus.
4. Wählen Sie das Symbol für die Erweiterung aus.
5. Klicken Sie auf **Melden Sie sich an, um loszulegen**.

Um interne Unternehmens-URLs per Remotezugriff auf die Erweiterung zu verwenden, gehen Sie wie folgt vor:
1. [Konfigurieren Sie den Anwendungsproxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-enable) für Ihren Mandanten.
2. [Veröffentlichen Sie die Anwendung](https://docs.microsoft.com/en-us/azure/active-directory/application-proxy-publish-azure-portal) und die URL über den Anwendungsproxy.
3. Installieren Sie die Erweiterung, und melden Sie sich bei ihr an, indem Sie „Melden Sie sich an, um loszulegen“ auswählen.
4. Nun können Sie selbst per Remotezugriff zu der internen Unternehmens-URL browsen.

> [!NOTE]
> Sie können die automatische Umleitung zu Unternehmens-URLs auch deaktivieren, indem Sie im Hauptmenü erst das Einstellungszahnrad und dann **aus** für die Option „Unternehmensinterne URL-Umleitung“ wählen.


## <a name="mobile-app-support"></a>Unterstützung für mobile Apps

Vom Azure Active Directory-Team wird die mobile App „Meine Apps“ veröffentlicht. Wenn Sie diese App installieren, können Sie sich auf iOS- und Android-Geräten bei Anwendungen anmelden, für die kennwortbasiertes einmaliges Anmelden genutzt wird.

> [!NOTE]
> Sie können sich auf nahezu jedem Gerät in praktisch jedem Webbrowser bei Anwendungen anmelden, die den Verbund mit Azure AD unterstützen (z.B. Salesforce, Google Apps, Dropbox, Box, Concur, Workday, Office 365 und mehr als 70 weitere) – ohne dass ein Plug-In oder eine mobile App erforderlich ist. Bei Verwendung auf einem mobilen Gerät ist für die anderen [Zugriffsbereichsumgebungen](https://myapps.microsoft.com/) auch die mobile App „Meine Apps“ nicht erforderlich.
>
>

### <a name="my-apps-for-android"></a>"Meine Apps" für Android

„Meine Apps“ für Android wird auf allen Android-Geräten unterstützt, die mindestens über die Android-Version 4.1 verfügen.  

Die App ist im [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.myapps) erhältlich.

!["Meine Apps" für Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>"Meine Apps" für iPhone und iPad

„Meine Apps“ für iOS wird auf allen iPhone- und iPad-Geräten mit iOS-Version 7 oder höher unterstützt.  

Die App ist im [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8) erhältlich.

![„Meine Apps“ für iOS][4]    


## <a name="managed-browser-for-my-apps"></a>Managed Browser für „Meine Apps“

„Meine Apps“ ist auch in den Intune Managed Browser integriert. Der Intune Managed Browser für iOS- und Android-Geräte trägt entscheidend zum Schutz der Daten auf mobilen Geräten bei. Der Browser bietet eine sichere Webbrowserumgebung, in der Sie Webseiten mit Unternehmensinformationen sicher anzeigen und verwenden können.  

Über Ihre Managed Browser-Startseite und Ihre Favoriten können Sie schnell auf „Meine Apps“ zugreifen und so jede Anwendung mit weniger Klicks erreichen.

Der Intune Managed Browser ist im [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) und im [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en) erhältlich.

![Managed Browser für „Meine Apps“][5]    


## <a name="tips-for-testing-the-user-experience"></a>Tipps zum Testen der Benutzerfunktionen

Wenn Sie Azure-Administrator und mit einem Konto im Verzeichnis beim Azure-Portal angemeldet sind, werden Sie automatisch mit dem aktuellen Konto beim Zugriffsbereich angemeldet. Diese Ansicht zeigt alle Anwendungen, die Ihnen zugewiesen sind.

Wenn Sie den Test mit einem *anderen* Benutzerkonto durchführen möchten, gehen Sie wie folgt vor:

1. Klicken Sie rechts oben im Azure-Portal oder Zugriffsbereich auf **Abmelden**. 
2. Wechseln Sie zum [Zugriffsbereich](http://myapps.microsoft.com).
3. Geben Sie auf der Anmeldeseite den Benutzernamen und das Kennwort für das Konto in Ihrem Verzeichnis ein, das Sie testen möchten.


## <a name="starting-applications"></a>Starten von Anwendungen

In diesem Abschnitt werden mehrere Arten von Anwendungen beschrieben, die im Zugriffsbereich angezeigt werden können.

### <a name="office-365-applications"></a>Office 365-Anwendungen

Wenn Ihre Organisation Office 365-Anwendungen verwendet und Sie für diese lizenziert sind, werden die Office 365-Anwendungen in Ihrem Zugriffsbereich angezeigt.

Wenn Sie auf eine Anwendungskachel für eine Office 365-Anwendung klicken, werden Sie zu dieser Anwendung weitergeleitet und automatisch angemeldet.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Anwendungen von Microsoft und Drittanbietern, für die verbundbasiertes SSO konfiguriert ist

Ihr Administrator kann im Abschnitt „Active Directory“ des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Azure AD – einmaliges Anmelden** hinzufügen. Diese Anwendungen werden nur angezeigt, wenn Ihr Administrator Ihnen explizit Zugriff darauf gewährt hat.

Wenn Sie auf die Kachel für eine Anwendung klicken, werden Sie zu dieser Anwendung weitergeleitet und automatisch bei der Anwendung angemeldet.

### <a name="password-based-sso-without-identity-provisioning"></a>Kennwortbasiertes SSO ohne Identitätsbereitstellung

Ihr Administrator kann im Abschnitt „Active Directory“ des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Kennwortbasiertes einmaliges Anmelden** hinzufügen. Alle Benutzer im Verzeichnis können alle Anwendungen anzeigen, die in diesem Modus konfiguriert wurden.

Wenn Sie erstmals auf eine Anwendungskachel klicken, werden Sie aufgefordert, das Kennwort-SSO-Plug-In für Internet Explorer oder Chrome zu installieren. Für die Installation müssen Sie den Webbrowser möglicherweise neu starten. Wenn Sie in den Zugriffsbereich zurückkehren und erneut auf die Anwendungskachel klicken, werden Sie zum Eingeben eines Benutzernamens und Kennworts für die Anwendung aufgefordert. Nachdem Sie Ihren Benutzernamen und Ihr Kennwort eingegeben haben, werden die Anmeldeinformationen in Azure AD sicher gespeichert und mit dem Konto verknüpft.

Wenn Sie das nächste Mal auf die Anwendungskachel klicken, werden Sie automatisch bei der Anwendung angemeldet.  

Sie müssen die Anmeldeinformationen nicht erneut eingeben und auch das Kennwort-SSO-Plug-In nicht erneut installieren.

Wenn sich Ihre Anmeldeinformationen in der Drittanbieteranwendung geändert haben, müssen Sie Ihre in Azure AD gespeicherten Anmeldeinformationen aktualisieren. 

Gehen Sie zum Aktualisieren Ihrer Anmeldeinformationen wie folgt vor:

1. Wählen Sie das Symbol auf der Anwendungskachel.
2. Wählen Sie **Anmeldeinformationen aktualisieren**, und geben Sie Ihren Benutzernamen und Ihr Kennwort für die Anwendung erneut ein.


### <a name="password-based-sso-with-identity-provisioning"></a>Kennwortbasiertes SSO mit Identitätsbereitstellung

Ihr Administrator kann im Active Directory-Abschnitt des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Kennwortbasiertes einmaliges Anmelden** sowie die Identitätsbereitstellung hinzufügen.

Wenn Sie erstmals auf eine Anwendungskachel klicken, werden Sie aufgefordert, das Kennwort-SSO-Plug-In für Internet Explorer oder Chrome zu installieren. Für die Installation müssen Sie den Webbrowser möglicherweise neu starten.  

Wenn Sie zum Zugriffsbereich zurückkehren und erneut auf die Anwendungskachel klicken, werden Sie automatisch bei der Anwendung angemeldet.

Bei einigen Anwendungen muss das Kennwort ggf. bei der ersten Anmeldung geändert werden. Wenn sich Ihre Anmeldeinformationen in der Drittanbieteranwendung geändert haben, müssen Sie die in Azure AD gespeicherten Anmeldeinformationen aktualisieren. 

Gehen Sie zum Aktualisieren Ihrer Anmeldeinformationen wie folgt vor:

1. Wählen Sie das Symbol auf der Anwendungskachel.
2. Wählen Sie **Anmeldeinformationen aktualisieren**, und geben Sie Ihren Benutzernamen und Ihr Kennwort für die Anwendung erneut ein.


### <a name="application-with-existing-sso-solutions"></a>Anwendung mit vorhandenen SSO-Lösungen

Im Azure-Portal steht noch eine dritte Option zum Konfigurieren des einmaligen Anmeldens für eine Anwendung zur Verfügung: „Vorhandenes einmaliges Anmelden“. Diese Option ermöglicht es Ihrem Administrator, eine Verknüpfung mit einer Anwendung zu erstellen und sie für ausgewählte Benutzer im Zugriffsbereich zu platzieren.

Wenn eine Anwendung beispielsweise für die Benutzerauthentifizierung per AD FS 2.0 konfiguriert ist, kann Ihr Administrator die Option „Vorhandenes einmaliges Anmelden“ verwenden, um im Zugriffsbereich eine entsprechende Verknüpfung dafür zu erstellen. Wenn Sie die Verknüpfung verwenden, werden Sie per AD FS 2.0 oder über die SSO-Lösung authentifiziert, die von der Anwendung bereitgestellt wird.


## <a name="next-steps"></a>Nächste Schritte

- Eine Liste mit allen Themen im Zusammenhang mit der Anwendungsverwaltung finden Sie im [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md).
 
- Informationen zum Integrieren einer SaaS-App in Azure AD finden Sie in der [Liste der Tutorials zur Integration von SaaS-Apps](saas-apps/tutorial-list.md).
 
- Informationen zum Verwalten von Apps in Azure AD finden Sie in der [Einführung in das einmalige Anmelden und das Verwalten von App-Zugriff mit Azure Active Directory](manage-apps/what-is-single-sign-on.md).
 
- Weitere Informationen zur Benutzerbereitstellung finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
