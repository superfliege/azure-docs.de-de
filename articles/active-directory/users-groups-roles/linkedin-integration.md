---
title: Einwilligung zu LinkedIn-Diensten für Ihre Organisation – Azure Active Directory | Microsoft-Dokumentation
description: Dieser Artikel erläutert, wie Sie die LinkedIn-Integration für Microsoft-Apps in Azure Active Directory aktivieren oder deaktivieren.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: abcb1696efe44293d01153aa37a9835ba5f43370
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58199697"
---
# <a name="consent-to-linkedin-integration-for-your-azure-active-directory-organization"></a>Einwilligung zur LinkedIn-Integration für Ihre Azure Active Directory-Organisation

In diesem Artikel erfahren Sie, wie Sie die LinkedIn-Integration für Ihre Organisation im Azure Active Directory Admin Center (Azure AD) aktivieren und deaktivieren.

> [!IMPORTANT]
> Die Einstellung für die LinkedIn-Integration wird derzeit auf Azure AD-Organisationen ausgeweitet. Nachdem die Funktionen für Ihre Organisation zur Verfügung stehen, sind sie standardmäßig aktiviert.
> 
> Ausnahmen:
> * Die Einstellung ist nicht für Kunden mit Microsoft Cloud für die US-Regierung, Microsoft Cloud Deutschland oder Azure und Office 365, betrieben von 21Vianet in China, verfügbar.
> * Die Einstellung ist standardmäßig für Mandanten, die in Deutschland bereitgestellt werden, deaktiviert. Beachten Sie, dass die Einstellung nicht für Kunden mit Microsoft Cloud Deutschland verfügbar ist.
> * Die Einstellung ist standardmäßig für Mandanten, die in Frankreich bereitgestellt werden, deaktiviert.
>
> Die Integration funktioniert nur, wenn Sie sie aktiviert haben *und* nachdem die Benutzereinwilligung für den Zugriff von Apps auf Unternehmensdaten in ihrem Namen erfolgt ist. Informationen zur Einstellung für die Benutzereinwilligung finden Sie unter [Aufheben des Zugriffs eines Benutzers auf eine Anwendung](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-the-azure-portal"></a>Aktivieren oder Deaktivieren der LinkedIn-Integration für Ihre Benutzer im Azure-Portal

Sie können die LinkedIn-Integration für Ihren gesamten Mandanten oder für ausgewählte Benutzer in Ihrem Mandanten aktivieren oder deaktivieren.

1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com/) mit einem Konto an, das als globaler Administrator für den Azure AD-Mandanten konfiguriert ist.
2. Wählen Sie **Benutzer** aus.
3. Wählen Sie auf dem Blatt **Benutzer** die Option **Benutzereinstellungen** aus.
4. Unter **LinkedIn-Kontoverbindungen**:

   * Wählen Sie **Ja** aus, um für alle Benutzer in der Organisation einzuwilligen, dass sie eine Verbindung über ihre Konten herstellen können, um Zugriff auf ihre LinkedIn-Kontakte in einigen Microsoft-Apps zu erhalten.
   * Wählen Sie **Ausgewählt** aus, um nur für ausgewählte Benutzer in der Organisation einzuwilligen, dass sie eine Verbindung über ihre Konten herstellen können, um Zugriff auf ihre LinkedIn-Kontakte in einigen Microsoft-Apps zu erhalten.
   * Wählen Sie **Nein** aus, um die Einwilligung für Benutzer in Ihrer Organisation zu widerrufen, dass sie eine Verbindung über ihre Konten herstellen können, um Zugriff auf ihre LinkedIn-Kontakte in einigen Microsoft-Apps zu erhalten.
    ![Aktivieren der LinkedIn-Integration in Ihrer Organisation](./media/linkedin-integration/linkedin-integration.png)
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
