---
title: Technische Referenz zum bedingten Azure Active Directory-Zugriff | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie die bedingte Zugriffssteuerung in Azure Active Directory verwenden. Geben Sie die Bedingungen für die Authentifizierung von Benutzern an, und steuern Sie den Zugriff auf Ihre Anwendung. Wenn die angegebenen Bedingungen erfüllt sind, werden die Benutzer authentifiziert und erhalten Zugriff auf Ihre Anwendung."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2017
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 5fad793bcf9ac86c2a1bc67e74dfb62af9876100
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Technische Referenz zum bedingten Azure Active Directory-Zugriff

Sie können den [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) verwenden, um präzise zu steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können.   

Dieser Artikel enthält unterstützende Informationen zu den folgenden Konfigurationsoptionen für eine Richtlinie für bedingten Zugriff: 

- Zuweisungen von Cloudanwendungen

- Geräteplattformbedingung 

- Bedingung für Clientanwendungen

- Genehmigte Clientanwendungsanforderung



## <a name="cloud-apps-assignments"></a>Zuweisungen von Cloud-Apps

Mit Richtlinien für bedingten Zugriff steuern Sie, wie Ihre Benutzer auf [Cloud-Apps](active-directory-conditional-access-azure-portal.md#who) zugreifen. Wenn Sie eine Richtlinie für bedingten Zugriff konfigurieren, müssen Sie mindestens eine Cloud-App auswählen. 

![Auswählen der Cloud-Apps für Ihre Richtlinie](./media/active-directory-conditional-access-technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Microsoft Cloud-Anwendungen

Sie können folgenden Cloud-Apps von Microsoft eine Richtlinie für bedingten Zugriff zuweisen:

- Azure Information Protection – [Weitere Informationen](https://docs.microsoft.com/information-protection/get-started/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)

- Azure RemoteApp

- Microsoft Dynamics 365

- Microsoft Office 365 Yammer

- Microsoft Office 365 Exchange Online

- Microsoft Office 365 SharePoint Online (einschließlich OneDrive for Business)

- Microsoft Power BI 

- Microsoft Visual Studio Team Services

- Microsoft Teams


### <a name="other-applications"></a>Andere Anwendungen 

Außer den Cloud-Apps von Microsoft können Sie den folgenden Typen von Cloud-Apps eine Richtlinie für bedingten Zugriff zuweisen:

- Anwendungen mit Azure AD-Verbindung

- Vorab integrierte verbundene Software-as-a-Service-Anwendung (SaaS)

- Anwendungen, die einmaliges Anmelden mit Kennwort (SSO) verwenden

- Branchenanwendungen

- Anwendungen, die den Azure AD-Anwendungsproxy verwenden


## <a name="device-platform-condition"></a>Geräteplattformbedingung

In einer Richtlinie für bedingten Zugriff können Sie die Geräteplattformbedingung konfigurieren, um die Richtlinie an das Betriebssystem auf einem Client zu binden. Für den bedingten Azure AD-Zugriff werden folgende Geräteplattformen unterstützt:

- Android

- iOS

- Windows Phone

- Windows

- macOS


![Binden der Zugriffsrichtlinie an das Clientbetriebssystem](./media/active-directory-conditional-access-technical-reference/41.png)





## <a name="client-apps-condition"></a>Client-Apps-Bedingung 

In der Richtlinie für bedingten Zugriff können Sie die [Client-Apps](active-directory-conditional-access-azure-portal.md#client-apps)-Bedingung konfigurieren, um die Richtlinie mit der Client-App zu verknüpfen, die einen Zugriffsversuch initiiert hat. Legen Sie die Client-Apps-Bedingung fest, um den Zugriff zu gewähren oder zu verweigern, wenn mit den folgenden Arten von Client-Apps ein Zugriffsversuch unternommen wird:

- "Browser"
- Mobile Apps und Desktop-Apps

![Steuern des Zugriffs für Client-Apps](./media/active-directory-conditional-access-technical-reference/03.png)

### <a name="supported-browsers"></a>Unterstützte Browser 

In der Richtlinie für bedingten Zugriff können Sie als Client-App **Browser** auswählen.

![Steuern des Zugriffs für unterstützte Browser](./media/active-directory-conditional-access-technical-reference/05.png)

Diese Einstellung funktioniert mit allen Browsern. Die folgenden Betriebssysteme und Browser werden jedoch unterstützt, um eine Geräterichtlinie, z. B. eine konforme Geräteanforderung, zu erfüllen:


| Betriebssystem                     | Browser                            | Support     |
| :--                    | :--                                 | :-:         |
| Windows 10             | Internet Explorer, Edge, Chrome     | ![Prüfen][1] |
| Windows 8/8.1        | Internet Explorer, Chrome           | ![Prüfen][1] |
| Windows 7              | Internet Explorer, Chrome           | ![Prüfen][1] |
| iOS                    | Safari, Intune Managed Browser      | ![Prüfen][1] |
| Android                | Chrome, Intune Managed Browser      | ![Prüfen][1] |
| Windows Phone          | Internet Explorer, Edge             | ![Prüfen][1] |
| Windows Server 2016    | Internet Explorer, Edge             | ![Prüfen][1] |
| Windows Server 2016    | Chrome                              | In Kürze verfügbar |
| Windows Server 2012 R2 | Internet Explorer, Chrome           | ![Prüfen][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome           | ![Prüfen][1] |
| macOS                  | Chrome, Safari                      | ![Prüfen][1] |


> [!NOTE]
> Um Chrome-Unterstützung zu erhalten, müssen Sie Windows 10 Creators Update (Version 1703) oder höher verwenden.<br>
> Sie können [diese Erweiterung](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji) installieren.

Diese Browser unterstützen die Geräteauthentifizierung, sodass das Gerät identifiziert und anhand einer Richtlinie überprüft werden kann. Bei der Geräteüberprüfung tritt ein Fehler auf, wenn der Browser im privaten Modus ausgeführt wird. 


### <a name="supported-mobile-applications-and-desktop-clients"></a>Unterstützte mobile Anwendungen und Desktopclients

In der Richtlinie für bedingten Zugriff können Sie als Client-App **Mobile Apps und Desktopclients** auswählen.


![Steuern des Zugriffs für unterstützte mobile Apps oder Desktopclients](./media/active-directory-conditional-access-technical-reference/06.png)


Diese Einstellung hat Auswirkungen auf Zugriffsversuche von den folgenden mobilen Apps und Desktopclients: 


|Client-Apps|Zieldienst|Plattform|
|---|---|---|
|Azure RemoteApp|Azure Remote-App-Dienst|Windows 10, Windows 8.1, Windows 7, iOS, Android, Mac OS X|
|Dynamics CRM-App|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, iOS, Android|
|E-Mail-/Kalender-/Kontakte-App, Outlook 2016, Outlook 2013 (mit moderner Authentifizierung)|Microsoft Office 365 Exchange Online|Windows 10|
|MFA- und Standort-Richtlinien für Apps Gerätebasierte Richtlinien werden nicht unterstützt. |Alle Meine Apps-App-Dienste|Android und iOS|
|Microsoft Teams-Dienste: steuert alle Dienste, die Microsoft-Teams unterstützen, und alle dazugehörigen Client-Apps – Windows Desktop, iOS, Android, WP und Webclient|Microsoft Teams|Windows 10, Windows 8.1, Windows 7, iOS, Android und macOS |
|Office 2016-Apps, Office 2013 (mit moderner Authentifizierung), OneDrive-Synchronisierungsclient (siehe [Hinweise](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|Office 365 SharePoint Online|Windows 8.1, Windows 7|
|Office 2016-Apps, universelle Office-Apps, Office 2013 (mit moderner Authentifizierung), OneDrive-Synchronisierungsclient (siehe [Hinweise](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), zukünftige Unterstützung für Office-Gruppen und SharePoint-Apps ist geplant.|Office 365 SharePoint Online|Windows 10|
|Office 2016 für macOS (nur Word, Excel, PowerPoint und OneNote). Unterstützung von OneDrive for Business für die Zukunft geplant|Office 365 SharePoint Online|Mac OS X|
|Office Mobile-Apps|Office 365 SharePoint Online|Android, iOS|
|Office Yammer-App|Office 365 Yammer|Windows 10, iOS und Android|
|Outlook 2016 (Office für macOS)|Microsoft Office 365 Exchange Online|Mac OS X|
|Outlook 2016, Outlook 2013 (mit moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung)|Microsoft Office 365 Exchange Online|Windows 8.1, Windows 7|
|Outlook Mobile-App|Microsoft Office 365 Exchange Online|Android, iOS|
|PowerBI-App. Die PowerBI-App für Android unterstützt derzeit keinen gerätebasierten bedingten Zugriff.|PowerBI-Dienst|Windows 10, Windows 8.1, Windows 7 und iOS|
|Skype for Business|Microsoft Office 365 Exchange Online|Android, iOS |
|Visual Studio Team Services-App|Visual Studio Team Services|Windows 10, Windows 8.1, Windows 7, iOS, Android|



## <a name="approved-client-app-requirement"></a>Genehmigte Client-App als Voraussetzung 

Sie können in der Richtlinie für bedingten Zugriff vorschreiben, dass ein Zugriffsversuch für die ausgewählten Cloud-Apps über eine genehmigte Client-App erfolgen muss. 

![Steuern des Zugriffs für genehmigte Client-Apps](./media/active-directory-conditional-access-technical-reference/21.png)

Diese Einstellung gilt für die folgenden Client-Apps:


- Microsoft Azure Information Protection
- Microsoft Excel
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype for Business
- Microsoft Teams
- Microsoft Visio
- Microsoft Word



**Hinweise:**

- Die genehmigten Client-Apps unterstützen das Intune-Feature für die mobile Anwendungsverwaltung.

- Anforderung **Genehmigte Client-App erforderlich**:

    - Unterstützt als [Geräteplattformbedingung](#device-platforms-condition) nur iOS und Android.

    - Keine Unterstützung der Option **Browser** für die [Client-Apps-Bedingung](#supported-browsers).
    
    - Ersetzt die Option **Mobile Apps und Desktopclients** für die [Client-Apps-Bedingung](#supported-mobile-apps-and-desktop-clients), wenn diese Option ausgewählt ist.


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über den bedingten Zugriff finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-azure-portal.md).
- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, helfen Ihnen die [empfohlenen Methoden für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) weiter.



<!--Image references-->
[1]: ./media/active-directory-conditional-access-technical-reference/01.png


