---
title: "Aktivieren oder Deaktivieren der LinkedIn-Integration für Office-Anwendungen in Azure Active Directory | Microsoft-Dokumentation"
description: "Dieser Artikel erläutert, wie Sie die LinkedIn-Integration für Microsoft-Apps in Azure Active Directory aktivieren oder deaktivieren."
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 02/28/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: cdfb5458b020e9d3a3f33cecbeb0ee7b9a48909d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="linkedin-integration-for-office-applications"></a>LinkedIn-Integration für Office-Anwendungen
Dieser Artikel beschreibt, wie Sie in Azure Active Directory (Azure AD) die Benutzer einschränken, denen die LinkedIn-Integration bereitgestellt wird. Die LinkedIn-Integration ist beim Hinzufügen zu Ihrem Mandanten standardmäßig aktiviert, sodass Benutzer in einigen ihrer Microsoft-Apps auf öffentliche LinkedIn-Daten zugreifen können. Die Benutzer können sich unabhängig voneinander dafür entscheiden, ihre Geschäfts-, Schul- oder Unikonten mit ihren LinkedIn-Konten zu verbinden.

> [!IMPORTANT]
> Die LinkedIn-Integration wird nicht für alle Azure AD-Mandanten gleichzeitig bereitgestellt. Nach dem Rollout in Ihrem Azure-Mandanten ist die LinkedIn-Integration standardmäßig aktiviert. Die LinkedIn-Integration steht für Mandanten mit landesspezifischer Datenspeicherung, unabhängige Mandanten und Mandanten von Behörden nicht zur Verfügung. Eine aktuelle Ansicht der Rolloutinformationen finden Sie auf der Seite [Office 365-Roadmap](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc).

## <a name="linkedin-integration-from-the-user-perspective"></a>LinkedIn-Integration aus Benutzerperspektive
Wenn Benutzer in Ihrer Organisation ihre LinkedIn-Konten mit ihren Geschäfts-, Schul- oder Unikonten verknüpfen, [gestatten sie es LinkedIn, Daten bereitzustellen](https://www.linkedin.com/help/linkedin/answer/84077), die in den von Ihrer Organisation bereitgestellten Microsoft-Apps und -Diensten verwendet werden können. [Benutzer können Konten trennen](https://www.linkedin.com/help/linkedin/answer/85097), wodurch die Berechtigung für die gemeinsame Nutzung von Daten mit Microsoft erlischt. Die LinkedIn-Integration verwendet öffentlich verfügbare LinkedIn-Profilinformationen. [Benutzer können steuern, wie ihr eigenes LinkedIn-Profil angezeigt wird](https://www.linkedin.com/help/linkedin/answer/83). In den LinkedIn-Datenschutzeinstellungen können sie u.a. angeben, ob ihr Profil in Microsoft-Apps angezeigt werden kann.

## <a name="privacy-considerations"></a>Datenschutzaspekte
Durch die Aktivierung der LinkedIn-Integration in Azure AD können Microsoft-Apps und -Dienste auf einige der LinkedIn-Informationen Ihrer Benutzer zugreifen. Weitere Informationen zu den Überlegungen zum Datenschutz bei der LinkedIn-Integration in Azure AD finden Sie in der [Datenschutzerklärung von Microsoft](https://privacy.microsoft.com/privacystatement/). 

## <a name="manage-linkedin-integration"></a>Verwalten der LinkedIn-Integration
Die LinkedIn-Integration für Unternehmen ist in Azure AD standardmäßig aktiviert. Durch die Aktivierung der LinkedIn-Integration können alle Benutzer in Ihrer Organisation LinkedIn-Features innerhalb von Microsoft-Diensten verwenden. Sie können z.B. LinkedIn-Profile in Outlook anzeigen. Durch die Deaktivierung der LinkedIn-Integration werden die LinkedIn-Features aus Microsoft-Apps und -Diensten entfernt, und die gemeinsame Datennutzung von LinkedIn und Ihrer Organisation über die Microsoft-Dienste wird beendet.

### <a name="enable-or-disable-linkedin-integration-for-your-organization-in-the-azure-portal"></a>Aktivieren oder Deaktivieren der LinkedIn-Integration für Ihre Organisation im Azure-Portal

1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com/) mit einem Konto an, das als globaler Administrator für den Azure AD-Mandanten konfiguriert ist.
2. Wählen Sie **Benutzer** aus.
3. Wählen Sie auf dem Blatt **Benutzer** die Option **Benutzereinstellungen** aus.
4. Wählen Sie unter **LinkedIn-Integration** entweder **Ja** oder **Nein** aus, um die LinkedIn-Integration zu aktivieren oder zu deaktivieren.
   ![Aktivieren der LinkedIn-Integration](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="enable-or-disable-linkedin-integration-for-your-organizations-office-2016-apps-using-group-policy"></a>Aktivieren oder Deaktivieren der LinkedIn-Integration die Office 2016-Apps Ihrer Organisation mithilfe einer Gruppenrichtlinie

1. Laden Sie die [Dateien mit den administrativen Vorlagen für Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030) herunter.
2. Extrahieren Sie die **ADMX**-Dateien, und kopieren Sie sie in Ihr **zentrales Repository**.
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
Verwenden Sie den folgenden Link, um Ihre aktuelle Einstellung für die LinkedIn-Integration im Azure-Portal anzuzeigen:

[Konfigurieren der LinkedIn-Integration](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 