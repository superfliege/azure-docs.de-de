---
title: Aktivieren von LinkedIn-Verbindungen für Microsoft-Apps und -Dienste in Azure Active Directory | Microsoft-Dokumentation
description: Dieser Artikel erläutert, wie Sie die LinkedIn-Kontoverbindungen für Microsoft-Apps in Azure Active Directory aktivieren oder deaktivieren.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 03/22/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 33e3305288edc3990ed88b39c819293a8adc2dfe
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="linkedin-account-connections-for-microsoft-apps-and-services"></a>LinkedIn-Kontoverbindungen für Microsoft-Apps und -Dienste
In diesem Artikel erfahren Sie, wie Sie LinkedIn-Kontoverbindungen für Ihren Mandanten im Administrationscenter von Azure Active Directory (Azure AD) verwalten. 

> [!IMPORTANT]
> Die Funktionalität von LinkedIn-Kontoverbindungen wird derzeit auf Azure AD-Mandanten ausgeweitet. Nachdem die Funktionen für Ihren Mandanten zur Verfügung stehen, sind sie standardmäßig aktiviert. Sie sind nicht verfügbar für Regierungskunden in den USA und Organisationen mit Exchange-Onlinepostfächern in Australien, Kanada, China, Frankreich, Deutschland, Indien, Südkorea, Großbritannien, Japan und Südafrika. Schon bald werden auch diese Postfachstandorte unterstützt.  Eine aktuelle Ansicht der Rolloutinformationen finden Sie auf der Seite [Office 365-Roadmap](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc).

## <a name="how-linkedin-account-connections-appear-to-the-user"></a>Anzeige von LinkedIn-Kontoverbindungen für Benutzer
LinkedIn-Kontoverbindungen ermöglichen es Benutzern, öffentliche LinkedIn-Profilinformationen in einigen ihrer Microsoft-Anwendungen anzuzeigen. Benutzer in Ihrem Mandanten können wählen, ob sie ihre Geschäfts-, Schul- oder Unikonten von LinkedIn und Microsoft verbinden möchten, um zusätzliche LinkedIn-Profilinformationen anzuzeigen. Weitere Informationen finden Sie unter [LinkedIn-Informationen und -Features in Microsoft-Apps und -Diensten](https://go.microsoft.com/fwlink/?linkid=850740).

Beim Verbinden ihrer Geschäfts-, Schul- oder Unikonten von LinkedIn und Microsoft haben Benutzer in Ihrer Organisation zwei Möglichkeiten: 
* Sie erteilen die Berechtigung, Daten zwischen beiden Konten auszutauschen. Dies bedeutet, dass sie ihrem LinkedIn-Konto die Berechtigung erteilen, Daten gemeinsam mit ihrem Geschäfts-, Schul- oder Unikonto von Microsoft zu nutzen, und umgekehrt. Daten, die mit LinkedIn geteilt werden, verlassen die Onlinedienste. 
* Sie erteilen die Berechtigung, Daten nur von ihrem LinkedIn-Konto an ihr Geschäfts-, Schul- oder Unikonto von Microsoft weiterzugeben – umgekehrt jedoch nicht.

Weitere Informationen über Daten, die von Benutzern zwischen Geschäfts-, Schul- oder Unikonten von LinkedIn und Microsoft ausgetauscht werden, finden Sie unter [LinkedIn in Microsoft-Anwendungen bei der Arbeit oder in der Schule bzw. Uni](https://www.linkedin.com/help/linkedin/answer/84077). 
* [Benutzer können jederzeit Konten trennen](https://www.linkedin.com/help/linkedin/answer/85097) und Datenfreigabeberechtigungen entfernen. 
* [Benutzer können steuern, wie ihr eigenes LinkedIn-Profil angezeigt wird](https://www.linkedin.com/help/linkedin/answer/83), z.B., ob ihr Profil in Microsoft-Anwendungen angezeigt werden kann.

## <a name="privacy-considerations"></a>Datenschutzaspekte
Durch die Aktivierung von LinkedIn-Kontoverbindungen können Microsoft-Apps und -Dienste auf einige der LinkedIn-Informationen Ihrer Benutzer zugreifen. Weitere Informationen zu den Überlegungen zum Datenschutz beim Aktivieren von LinkedIn-Kontoverbindungen in Azure AD finden Sie in der [Datenschutzerklärung von Microsoft](https://privacy.microsoft.com/privacystatement/). 

## <a name="manage-linkedin-account-connections"></a>Verwalten von LinkedIn-Kontoverbindungen
Die Funktionen der LinkedIn-Kontoverbindungen sind standardmäßig für Ihren gesamten Mandanten aktiviert. Sie können LinkedIn-Kontoverbindungen für Ihren gesamten Mandanten deaktivieren oder LinkedIn-Kontoverbindungen für ausgewählte Benutzer in Ihrem Mandanten aktivieren. 

### <a name="enable-or-disable-linkedin-account-connection-for-your-tenant-in-the-azure-portal"></a>Aktivieren oder deaktivieren von LinkedIn-Kontoverbindungen für Ihren Mandanten im Azure-Portal

1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com/) mit einem Konto an, das als globaler Administrator für den Azure AD-Mandanten konfiguriert ist.
2. Wählen Sie **Benutzer** aus.
3. Wählen Sie auf dem Blatt **Benutzer** die Option **Benutzereinstellungen** aus.
4. Unter **LinkedIn-Kontoverbindungen**:
  * Wählen Sie **Ja**, um LinkedIn-Kontoverbindungen für alle Benutzer in Ihrem Mandanten zu aktivieren.
  * Wählen Sie **Ausgewählte**, um LinkedIn-Kontoverbindungen nur für ausgewählte Mandantenbenutzer zu aktiveren.
  * Wählen Sie **Nein**, um LinkedIn-Kontoverbindungen für alle Benutzer zu deaktivieren. ![LinkedIn-Kontoverbindungen werden aktiviert](./media/linkedin-integration/LinkedIn-integration.png)
5. Speichern Sie die Einstellungen durch Auswahl von **Speichern**.

### <a name="enable-or-disable-linkedin-account-connections-for-your-organizations-office-2016-apps-using-group-policy"></a>Aktivieren oder Deaktivieren der LinkedIn-Kontoverbindungen für die Office 2016-Apps Ihrer Organisation mithilfe einer Gruppenrichtlinie

1. Laden Sie die [Dateien mit den administrativen Vorlagen für Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030) herunter.
2. Extrahieren Sie die **ADMX**-Dateien, und kopieren Sie sie in Ihren zentralen Speicher.
3. Öffnen Sie die Gruppenrichtlinienverwaltung.
4. Erstellen Sie ein Gruppenrichtlinienobjekt mit folgender Einstellung: **Benutzerkonfiguration** > **Administrative Vorlagen** > **Microsoft Office 2016** > **Verschiedenes** > **LinkedIn-Integration zulassen**.
5. Wählen Sie **Aktiviert** oder **Deaktiviert** aus.
  * Wenn die Richtlinie **aktiviert** ist, ist die Einstellung **LinkedIn-Features in Office-Anwendungen anzeigen**, die Sie im Office 2016-Optionsdialogfeld finden, aktiviert. Das bedeutet auch, dass Benutzer in Ihrer Organisation LinkedIn-Features in ihren Office-Anwendungen verwenden können.
  * Wenn die Richtlinie **deaktiviert** ist, ist die Einstellung **LinkedIn-Features in Office-Anwendungen anzeigen** im Office 2016-Optionsdialogfeld deaktiviert, und Endbenutzer können diese Einstellung nicht ändern. Benutzer in Ihrer Organisation können LinkedIn-Features nicht in ihren Office 2016-Anwendungen verwenden. 

Diese Gruppenrichtlinie betrifft nur Office 2016-Apps für einen lokalen Computer. Benutzer können LinkedIn-Features in allen Office 365-Anwendungen in Profilkarten sehen, auch wenn sie LinkedIn in ihren Office 2016-Apps deaktivieren. 

### <a name="learn-more"></a>Weitere Informationen 
* [LinkedIn information and features in your Microsoft apps (LinkedIn-Informationen und -funktionen in Ihren Microsoft-Apps)](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn-Hilfe](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie den folgenden Link, um Ihre aktuelle Einstellung für die LinkedIn-Kontoverbindungen im Azure-Portal anzuzeigen:

[Konfigurieren von LinkedIn-Kontoverbindungen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 