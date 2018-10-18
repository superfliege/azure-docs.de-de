---
title: Aktivieren der LinkedIn-Verbindungsintegration in Azure Active Directory | Microsoft-Dokumentation
description: Dieser Artikel erläutert, wie Sie die LinkedIn-Kontoverbindungen für Microsoft-Apps in Azure Active Directory aktivieren oder deaktivieren.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 0eaa2656313ecd9b64503051265dc16285f0a1f3
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44492839"
---
# <a name="linkedin-account-connections"></a>LinkedIn-Kontoverbindungen

In diesem Artikel erfahren Sie, wie Sie LinkedIn-Kontoverbindungen für Ihren Mandanten im Administrationscenter von Azure Active Directory (Azure AD) aktivieren und deaktivieren.

> [!IMPORTANT]
> Die Einstellung für LinkedIn-Kontoverbindungen wird derzeit auf Azure AD-Mandanten ausgeweitet. Nachdem die Funktionen für Ihren Mandanten zur Verfügung stehen, sind sie standardmäßig aktiviert. 
> 
> Ausnahmen:
> * Die Einstellung ist nicht für Kunden mit Microsoft Cloud für die US-Regierung, Microsoft Cloud Deutschland oder Azure und Office 365, betrieben von 21Vianet in China, verfügbar.
> * Die Einstellung ist standardmäßig für Mandanten, die in Deutschland bereitgestellt werden, deaktiviert. Beachten Sie, dass die Einstellung nicht für Kunden mit Microsoft Cloud Deutschland verfügbar ist.
> * Die Einstellung ist standardmäßig für Mandanten, die in Frankreich bereitgestellt werden, deaktiviert.

> Die Integration funktioniert nur, wenn Sie diese aktiviert haben *und* wenn Sie es Benutzern ermöglichen, Apps den Zugriff auf Unternehmensdaten in ihrem Namen zu gestatten. Informationen zur Einstellung für die Benutzerzustimmung finden Sie unter [Aufheben des Zugriffs eines Benutzers auf eine Anwendung](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-in-the-azure-portal"></a>Aktivieren oder Deaktivieren von LinkedIn-Kontoverbindungen für Ihren Mandanten im Azure-Portal

Sie können LinkedIn-Kontoverbindungen für Ihren gesamten Mandanten oder für ausgewählte Benutzer in Ihrem Mandanten aktivieren oder deaktivieren.

1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com/) mit einem Konto an, das als globaler Administrator für den Azure AD-Mandanten konfiguriert ist.
2. Wählen Sie **Benutzer** aus.
3. Wählen Sie auf dem Blatt **Benutzer** die Option **Benutzereinstellungen** aus.
4. Unter **LinkedIn-Kontoverbindungen**:
  * Wählen Sie **Ja**, um LinkedIn-Kontoverbindungen für alle Benutzer in Ihrem Mandanten zu aktivieren.
  * Wählen Sie **Ausgewählte**, um LinkedIn-Kontoverbindungen nur für ausgewählte Mandantenbenutzer zu aktiveren.
  * Wählen Sie **Nein**, um LinkedIn-Kontoverbindungen für alle Benutzer zu deaktivieren. ![LinkedIn-Kontoverbindungen werden aktiviert](./media/linkedin-integration/linkedin-integration.png)
5. Speichern Sie die Einstellungen durch Auswahl von **Speichern**.

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-using-group-policy"></a>Aktivieren oder Deaktivieren von LinkedIn-Kontoverbindungen für Ihren Mandanten über Gruppenrichtlinien

1. Laden Sie die [Dateien mit den administrativen Vorlagen für Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030) herunter.
2. Extrahieren Sie die **ADMX**-Dateien, und kopieren Sie sie in Ihren zentralen Speicher.
3. Öffnen Sie die Gruppenrichtlinienverwaltung.
4. Erstellen Sie ein Gruppenrichtlinienobjekt mit folgender Einstellung: **Benutzerkonfiguration** > **Administrative Vorlagen** > **Microsoft Office 2016** > **Verschiedenes** > **LinkedIn-Integration zulassen**.
5. Wählen Sie **Aktiviert** oder **Deaktiviert** aus.
  
 State (Zustand) | Wirkung
------ | ------
**Aktiviert** | Die Einstellung **LinkedIn Funktionen in Office-Anwendungen anzeigen** ist in den Optionen für Office 2016 aktiviert. Benutzer in Ihrer Organisation können LinkedIn-Features in ihren Office-Anwendungen verwenden.
 **Deaktiviert** | Die Einstellung **LinkedIn Funktionen in Office-Anwendungen anzeigen** ist in den Optionen für Office 2016 deaktiviert, und Endbenutzer können diese Einstellung nicht ändern. Benutzer in Ihrer Organisation können LinkedIn-Features nicht in ihren Office 2016-Anwendungen verwenden.

Diese Gruppenrichtlinie betrifft nur Office 2016-Apps für einen lokalen Computer. Benutzer können LinkedIn-Features in allen Office 365-Anwendungen in Profilkarten sehen, auch wenn sie LinkedIn in ihren Office 2016-Apps deaktivieren.

## <a name="learn-more"></a>Weitere Informationen

* [Integrieren von LinkedIn in Ihrer Organisation](linkedin-user-consent.md)

* [LinkedIn information and features in your Microsoft apps (LinkedIn-Informationen und -funktionen in Ihren Microsoft-Apps)](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn-Hilfe](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie den folgenden Link, um Ihre aktuelle Einstellung für die LinkedIn-Kontoverbindungen im Azure-Portal anzuzeigen:

[Konfigurieren von LinkedIn-Kontoverbindungen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 