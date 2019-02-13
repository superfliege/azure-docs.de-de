---
title: Aktivieren der LinkedIn-Integration in Microsoft-Apps – Azure Active Directory | Microsoft-Dokumentation
description: Dieser Artikel erläutert, wie Sie die LinkedIn-Integration für Microsoft-Apps in Azure Active Directory aktivieren oder deaktivieren.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 6b62fde7f9b01fbeb133c92b1306f70f56d71d7a
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509625"
---
# <a name="linkedin-integration"></a>LinkedIn-Integration

In diesem Artikel erfahren Sie, wie Sie die LinkedIn-Integration für Ihren Mandanten im Administrationscenter von Azure Active Directory (Azure AD) aktivieren und deaktivieren.

> [!IMPORTANT]
> Die Einstellung für die LinkedIn-Integration wird derzeit auf Azure AD-Mandanten ausgeweitet. Nachdem die Funktionen für Ihren Mandanten zur Verfügung stehen, sind sie standardmäßig aktiviert.
> 
> Ausnahmen:
> * Die Einstellung ist nicht für Kunden mit Microsoft Cloud für die US-Regierung, Microsoft Cloud Deutschland oder Azure und Office 365, betrieben von 21Vianet in China, verfügbar.
> * Die Einstellung ist standardmäßig für Mandanten, die in Deutschland bereitgestellt werden, deaktiviert. Beachten Sie, dass die Einstellung nicht für Kunden mit Microsoft Cloud Deutschland verfügbar ist.
> * Die Einstellung ist standardmäßig für Mandanten, die in Frankreich bereitgestellt werden, deaktiviert.

> Die Integration funktioniert nur, wenn Sie diese aktiviert haben *und* wenn Sie es Benutzern ermöglichen, Apps den Zugriff auf Unternehmensdaten in ihrem Namen zu gestatten. Informationen zur Einstellung für die Benutzerzustimmung finden Sie unter [Aufheben des Zugriffs eines Benutzers auf eine Anwendung](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-the-azure-portal"></a>Aktivieren oder Deaktivieren der LinkedIn-Integration für Ihre Benutzer im Azure-Portal

Sie können die LinkedIn-Integration für Ihren gesamten Mandanten oder für ausgewählte Benutzer in Ihrem Mandanten aktivieren oder deaktivieren.

1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com/) mit einem Konto an, das als globaler Administrator für den Azure AD-Mandanten konfiguriert ist.
2. Wählen Sie **Benutzer** aus.
3. Wählen Sie auf dem Blatt **Benutzer** die Option **Benutzereinstellungen** aus.
4. Unter **LinkedIn-Integration**:
  * Wählen Sie **Ja**, um die LinkedIn-Integration für alle Benutzer in Ihrem Mandanten zu aktivieren.
  * Wählen Sie **Ausgewählte**, um die LinkedIn-Integration nur für ausgewählte Mandantenbenutzer zu aktivieren.
  * Wählen Sie **Nein** , um die LinkedIn-Integration für alle Benutzer zu deaktivieren. ![Aktivieren der LinkedIn-Integration](./media/linkedin-integration/linkedin-integration.png)
5. Speichern Sie die Einstellungen durch Auswahl von **Speichern**.

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-group-policy"></a>Aktivieren oder Deaktivieren der LinkedIn-Integration für Ihre Benutzer in der Gruppenrichtlinie

1. Laden Sie die [Dateien mit den administrativen Vorlagen für Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030) herunter.
2. Extrahieren Sie die **ADMX**-Dateien, und kopieren Sie sie in Ihren zentralen Speicher.
3. Öffnen Sie die Gruppenrichtlinienverwaltung.
4. Erstellen Sie ein Gruppenrichtlinienobjekt mit folgender Einstellung: **Benutzerkonfiguration** > **Administrative Vorlagen** > **Microsoft Office 2016** > **Verschiedenes** > **LinkedIn-Integration zulassen**.
5. Wählen Sie **Aktiviert** oder **Deaktiviert** aus.
  
 Zustand | Wirkung
------ | ------
**Aktiviert** | Die Einstellung **LinkedIn Funktionen in Office-Anwendungen anzeigen** ist in den Optionen für Office 2016 aktiviert. Benutzer in Ihrer Organisation können LinkedIn-Features in ihren Office-Anwendungen verwenden.
 **Deaktiviert** | Die Einstellung **LinkedIn Funktionen in Office-Anwendungen anzeigen** ist in den Optionen für Office 2016 deaktiviert, und Endbenutzer können diese Einstellung nicht ändern. Benutzer in Ihrer Organisation können LinkedIn-Features nicht in ihren Office 2016-Anwendungen verwenden.

Diese Gruppenrichtlinie betrifft nur Office 2016-Apps für einen lokalen Computer. Benutzer können LinkedIn-Features in allen Office 365-Anwendungen in Profilkarten sehen, auch wenn sie LinkedIn in ihren Office 2016-Apps deaktivieren.

## <a name="learn-more"></a>Weitere Informationen

* [Integrieren von LinkedIn in Ihrer Organisation](linkedin-user-consent.md)

* [LinkedIn information and features in your Microsoft apps (LinkedIn-Informationen und -funktionen in Ihren Microsoft-Apps)](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn-Hilfe](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie den folgenden Link, um Ihre aktuelle Einstellung für die LinkedIn-Integration im Azure-Portal anzuzeigen:

[Anzeigen Ihrer aktuellen Einstellung für die LinkedIn-Integration im Azure-Portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
