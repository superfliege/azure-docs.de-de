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
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: spunukol
ms.translationtype: HT
ms.sourcegitcommit: 0e862492c9e17d0acb3c57a0d0abd1f77de08b6a
ms.openlocfilehash: 27ace4e9bc4a1626059fba657dce0c629d52f32d
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Technische Referenz zum bedingten Azure Active Directory-Zugriff

Sie können den [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) verwenden, um präzise zu steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können.  

Dieses Thema enthält unterstützende Informationen zu den folgenden Konfigurationsoptionen für eine Richtlinie für den bedingten Zugriff: 

- Zuweisungen von Cloudanwendungen

- Geräteplattformbedingung 

- Bedingung für Clientanwendungen

- Genehmigte Clientanwendungsanforderung



## <a name="cloud-apps-assignments"></a>Zuweisungen von Cloud-Apps

Wenn Sie eine Richtlinie für bedingten Zugriff konfigurieren, müssen Sie die [Cloud-Apps auswählen, für die Ihre Richtlinie verwendet wird](active-directory-conditional-access-azure-portal.md#who). 

![Auswählen der Cloud-Apps für Ihre Richtlinie](./media/active-directory-conditional-access-technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Microsoft Cloud-Anwendungen

Sie können folgenden Cloud-Apps von Microsoft eine Richtlinie für bedingten Zugriff zuweisen:

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

In einer Richtlinie für bedingten Zugriff können Sie die Geräteplattformbedingung konfigurieren, um die Richtlinie an das Betriebssystem auf einem Client zu binden.

![Binden der Zugriffsrichtlinie an das Clientbetriebssystem](./media/active-directory-conditional-access-technical-reference/41.png)

Für den bedingten Azure AD-Zugriff werden folgende Geräteplattformen unterstützt:

- Android

- iOS

- Windows Phone

- Windows

- macOS (Vorschauversion)



## <a name="client-apps-condition"></a>Client-Apps-Bedingung 

Wenn Sie eine Richtlinie für bedingten Zugriff konfigurieren, können Sie für die Client-App-Bedingung [Client-Apps auswählen](active-directory-conditional-access-azure-portal.md#client-apps). Legen Sie die Client-Apps-Bedingung fest, um den Zugriff zu gewähren oder zu verweigern, wenn mit den folgenden Arten von Client-Apps ein Zugriffsversuch unternommen wird:

- "Browser"
- Mobile Apps und Desktop-Apps

![Steuern des Zugriffs für Client-Apps](./media/active-directory-conditional-access-technical-reference/03.png)

### <a name="supported-browsers"></a>Unterstützte Browser 

Steuern Sie den Browserzugriff, indem Sie die Option **Browser** in Ihrer Richtlinie für bedingten Zugriff verwenden. Der Zugriff wird nur gewährt, wenn der Zugriffsversuch von einem unterstützten Browser unternommen wird. Wenn ein Zugriffsversuch mit einem nicht unterstützten Browser erfolgt, wird der Versuch blockiert.

![Steuern des Zugriffs für unterstützte Browser](./media/active-directory-conditional-access-technical-reference/05.png)

In der Richtlinie für bedingten Zugriff werden die folgenden Browser unterstützt: 


| Betriebssystem                     | Browser                    | Support     |
| :--                    | :--                         | :-:         |
| Windows 10             | Internet Explorer, Edge     | ![Prüfen][1] |
| Windows 10             | Chrome                      | ![Prüfen][1] |
| Windows 8/8.1        | Internet Explorer, Chrome   | ![Prüfen][1] |
| Windows 7              | Internet Explorer, Chrome   | ![Prüfen][1] |
| iOS                    | Safari, Intune Managed Browser                      | ![Prüfen][1] |
| Android                | Chrome, Intune Managed Browser                      | ![Prüfen][1] |
| Windows Phone          | Internet Explorer, Edge     | ![Prüfen][1] |
| Windows Server 2016    | Internet Explorer, Edge     | ![Prüfen][1] |
| Windows Server 2016    | Chrome                      | In Kürze verfügbar |
| Windows Server 2012 R2 | Internet Explorer, Chrome   | ![Prüfen][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome   | ![Prüfen][1] |
| macOS                  | Safari                      | ![Prüfen][1] |
| macOS                  | Chrome                      | In Kürze verfügbar |

> [!NOTE]
> Um Chrome-Unterstützung zu erhalten, müssen Sie das Windows 10 Creators Update (Version 1703) oder höher verwenden.<br>
> Sie können [diese Erweiterung](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji) installieren.

### <a name="supported-mobile-applications-and-desktop-clients"></a>Unterstützte mobile Anwendungen und Desktopclients

Steuern Sie den Zugriff auf die App und den Client, indem Sie in Ihrer Richtlinie für bedingten Zugriff die Option **Mobile Apps und Desktopclients** verwenden. Der Zugriff wird nur gewährt, wenn der Zugriffsversuch mit einer unterstützten mobilen App bzw. einem Desktopclient unternommen wird. Wenn ein Zugriffsversuch mit einer nicht unterstützten App bzw. einem Client erfolgt, wird der Versuch blockiert.

![Steuern des Zugriffs für unterstützte mobile Apps oder Desktopclients](./media/active-directory-conditional-access-technical-reference/06.png)

Die folgenden mobilen Apps und Desktopclients unterstützen den bedingten Zugriff für Office 365 und andere mit Azure AD verbundene Dienstanwendungen:


| Clientanwendungen| Zieldienst| Plattform |
| --- | --- | --- |
| Azure Multi-Factor Authentication und Standortrichtlinie für Apps (gerätebasierte Richtlinien werden nicht unterstützt)| Alle Meine Apps-App-Dienste| Android, iOS|
| Azure RemoteApp| Azure RemoteApp-Dienst| Windows 10, Windows 8.1, Windows 7, iOS, Android, macOS|
| Dynamics 365-App| Dynamics 365| Windows 10, Windows 8.1, Windows 7, iOS, Android|
| Microsoft Office 365 Teams (steuert alle Dienste, die Microsoft Teams und alle dazugehörigen Client-Apps unterstützt: Windows Desktop, iOS, Android, Windows Phone, Webclient)| Microsoft Teams| Windows 10, Windows 8.1, Windows 7, iOS, Android|
| Mail-/Kalender-/Kontakte-App, Outlook 2016, Outlook 2013 (mit moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung)| Microsoft Office 365 Exchange Online| Windows 10|
| Outlook 2016, Outlook 2013 (mit moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung)| Microsoft Office 365 Exchange Online| Windows 8.1, Windows 7|
| Outlook Mobile-App| Microsoft Office 365 Exchange Online| iOS|
| Outlook 2016 (Office für macOS)| Microsoft Office 365 Exchange Online| macOS|
| Office 2016-Apps, universelle Office-Apps, Office 2013 (mit moderner Authentifizierung), [OneDrive](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)-Synchronisierungsclient, geplante zukünftige Unterstützung von Office-Gruppen und SharePoint-App| Office 365 SharePoint Online| Windows 10|
| Office 2016-Apps, Office 2013 (mit moderner Authentifizierung), [OneDrive](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)-Synchronisierungsclient| Office 365 SharePoint Online| Windows 8.1, Windows 7|
| Office Mobile-Apps| Office 365 SharePoint Online| iOS, Android|
| Office 2016 für macOS (Unterstützung für Word, Excel, PowerPoint, OneNote), geplante zukünftige Unterstützung von OneDrive for Business| Office 365 SharePoint Online| macOS|
| Office Yammer-App| Office 365 Yammer| Windows 10, iOS und Android|
| PowerBI-App (unter Android derzeit nicht unterstützt)| PowerBI-Dienst| Windows 10, Windows 8.1, Windows 7 und iOS|
| Visual Studio Team Services-App| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS, Android|



## <a name="approved-client-app-requirement"></a>Genehmigte Client-App als Voraussetzung 

Steuern Sie Clientverbindungen mit der Option **Genehmigte Client-App erforderlich** in Ihrer Richtlinie für bedingten Zugriff. Der Zugriff wird nur gewährt, wenn ein Verbindungsversuch von einer genehmigten Client-App unternommen wird.

![Steuern des Zugriffs für genehmigte Client-Apps](./media/active-directory-conditional-access-technical-reference/21.png)

Die folgenden Client-Apps können für die genehmigte Clientanwendungsanforderung verwendet werden:

- Microsoft Excel

- Microsoft OneDrive

- Microsoft Outlook

- Microsoft OneNote

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



