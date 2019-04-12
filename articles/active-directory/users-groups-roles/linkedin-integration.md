---
title: Administratoreinwilligung für LinkedIn-Kontoverbindungen – Azure Active Directory | Microsoft-Dokumentation
description: Dieser Artikel erläutert, wie Sie die LinkedIn-Integrationskontoverbindungen in Microsoft-Apps in Azure Active Directory aktivieren oder deaktivieren.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/21/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e07c53192ea2c8b792256af944c81c9c909dc55
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368121"
---
# <a name="consent-to-linkedin-account-connections-for-an-azure-active-directory-organization"></a>Einwilligung zu LinkedIn-Kontoverbindungen für eine Azure Active Directory-Organisation

Sie können Benutzern in Ihrer Organisation den Zugriff auf ihre LinkedIn-Verbindungen in einigen Microsoft-Anwendungen gestatten. Es werden keine Daten freigegeben, bis der Benutzer der Verbindung seiner Konten zustimmt. Sie können Ihre Organisation in das Azure Active Directory (Azure AD) [Admin Center](https://aad.portal.azure.com) integrieren.

> [!IMPORTANT]
> Die Einstellung für LinkedIn-Kontoverbindungen wird derzeit auf Azure AD-Organisationen ausgeweitet. Nachdem die Funktionen für Ihre Organisation zur Verfügung stehen, sind sie standardmäßig aktiviert.
> 
> Ausnahmen:
> * Die Einstellung ist nicht für Kunden mit Microsoft Cloud für die US-Regierung, Microsoft Cloud Deutschland oder Azure und Office 365, betrieben von 21Vianet in China, verfügbar.
> * Die Einstellung ist standardmäßig für Mandanten, die in Deutschland bereitgestellt werden, deaktiviert. Beachten Sie, dass die Einstellung nicht für Kunden mit Microsoft Cloud Deutschland verfügbar ist.
> * Die Einstellung ist standardmäßig für Mandanten, die in Frankreich bereitgestellt werden, deaktiviert.
>
> Nachdem LinkedIn-Kontoverbindungen für Ihre Organisation aktiviert wurden, funktionieren die Kontoverbindungen, nachdem die Benutzer zugestimmt haben, dass Apps in ihrem Auftrag auf Unternehmensdaten zugreifen. Informationen zur Einstellung für die Benutzereinwilligung finden Sie unter [Aufheben des Zugriffs eines Benutzers auf eine Anwendung](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="use-the-azure-portal-to-enable-linkedin-account-connections"></a>Verwenden des Azure-Portals zum Aktivieren von LinkedIn-Kontoverbindungen

Sie können LinkedIn-Kontoverbindungen nur für die Benutzer aktivieren, auf die Sie Zugriff erhalten möchten – von der gesamten Organisation bis hin zu ausgewählten Benutzern in Ihrer Organisation.

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com/) mit dem Konto eines globalen Administrators für die Azure AD-Organisation an.
2. Wählen Sie **Benutzer** aus.
3. Wählen Sie auf dem Blatt **Benutzer** die Option **Benutzereinstellungen** aus.
4. Gestatten Sie Benutzern unter **LinkedIn-Kontoverbindungen** das Verbinden ihrer Konten, damit sie in einigen Microsoft-Anwendungen auf ihre LinkedIn-Verbindungen zugreifen können. Es werden keine Daten freigegeben, bis der Benutzer der Verbindung seiner Konten zustimmt.

  * Wählen Sie **Ja** aus, um die Einwilligung zu diesem Dienst für alle Benutzer in der Organisation zu geben.
  * Wählen Sie **Ausgewählt** aus, um die Einwilligung nur für ausgewählte Benutzer in der Organisation zu geben.
  * Wählen Sie **Nein** aus, um die Einwilligung für Benutzer in Ihrer Organisation zu widerrufen.

    ![Integrieren von LinkedIn-Kontoverbindungen in der Organisation](./media/linkedin-integration/linkedin-integration.png)

5. Wenn Sie fertig sind, wählen Sie **Speichern** aus, um Ihre Einstellungen zu speichern.
     
## <a name="use-group-policy-to-enable-linkedin-account-connections"></a>Verwenden der Gruppenrichtlinie zum Aktivieren von LinkedIn-Kontoverbindungen

1. Laden Sie die [Dateien mit den administrativen Vorlagen für Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030) herunter.
2. Extrahieren Sie die **ADMX**-Dateien, und kopieren Sie sie in Ihren zentralen Speicher.
3. Öffnen Sie die Gruppenrichtlinienverwaltung.
4. Erstellen Sie ein Gruppenrichtlinienobjekt mit folgender Einstellung: **Benutzerkonfiguration** > **Administrative Vorlagen** > **Microsoft Office 2016** > **Verschiedenes** > **LinkedIn-Integration zulassen**.
5. Wählen Sie **Aktiviert** oder **Deaktiviert** aus.
  
   Zustand | Wirkung
   ------ | ------
   **Aktiviert** | Die Einstellung **LinkedIn Funktionen in Office-Anwendungen anzeigen** ist in den Optionen für Office 2016 aktiviert. Benutzer in Ihrer Organisation können LinkedIn-Features in ihren Office 2016-Anwendungen verwenden.
   **Disabled** | Die Einstellung **LinkedIn Funktionen in Office-Anwendungen anzeigen** ist in den Optionen für Office 2016 deaktiviert, und Endbenutzer können diese Einstellung nicht ändern. Benutzer in Ihrer Organisation können LinkedIn-Features nicht in ihren Office 2016-Anwendungen verwenden.

Diese Gruppenrichtlinie betrifft nur Office 2016-Apps für einen lokalen Computer. Wenn Benutzer LinkedIn in ihren Office 2016-Anwendungen deaktivieren, werden die LinkedIn-Features in Office 365 weiterhin angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* [Benutzereinwilligung und Datenfreigabe für LinkedIn](linkedin-user-consent.md)

* [LinkedIn information and features in your Microsoft apps (LinkedIn-Informationen und -funktionen in Ihren Microsoft-Apps)](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn-Hilfe](https://www.linkedin.com/help/linkedin)

* [Anzeigen Ihrer aktuellen Einstellung für die LinkedIn-Integration im Azure-Portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
