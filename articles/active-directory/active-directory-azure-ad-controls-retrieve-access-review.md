---
title: Abrufen der Ergebnisse der Azure AD-Zugriffsüberprüfung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Ergebnisse von Azure Active Directory-Zugriffsüberprüfungen abrufen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 96676c3ec6d96fd6c6f78aa2dbed8cf4d0a6718d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38295299"
---
# <a name="retrieve-access-review-results"></a>Abrufen der Ergebnisse der Zugriffsüberprüfung

Mit Azure Active Directory (Azure AD) können Administratoren [eine Zugriffsüberprüfung erstellen](active-directory-azure-ad-controls-create-access-review.md). Diese kann für Mitglieder einer Gruppe oder Benutzer, die einer Anwendung zugewiesen sind, durchgeführt werden.  Benutzer mit den Rollen **Globaler Administrator**, **Benutzerkontoadministrator**, **Sicherheitsadministrator** oder **Benutzer mit Leseberechtigung für Sicherheitsfunktionen** können auch die Ergebnisse einer Zugriffsüberprüfung lesen.  Zum Zuweisen von Benutzern zu einer dieser Rollen kann ein Administrator für privilegierte Rollen mithilfe von Azure AD PIM Benutzern die Berechtigung zum Aktivieren der Rolle erteilen. Alternativ kann ein globaler Administrator [einem Benutzer die Rolle dauerhaft zuweisen](fundamentals/active-directory-users-assign-role-azure-portal.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="locating-an-access-review"></a>Suchen einer Zugriffsüberprüfung

Wenn Sie wissen, welches Programm die Zugriffsüberprüfung enthält, navigieren Sie zur Zugriffsüberprüfungsseite, klicken Sie auf **Programme**, und wählen Sie das Programm aus, welches das Zugriffsüberprüfungs-Steuerelement enthält.  Klicken Sie auf **Steuerelemente**, und wählen Sie das Steuerelement für die Zugriffsüberprüfung aus. Wenn das Programm viele Steuerelemente enthält, können Sie nach Steuerelementen eines bestimmten Typs filtern und diese nach ihrem Status sortieren. Darüber hinaus können Sie nach dem Namen des Zugriffsüberprüfung-Steuerelements oder dem Anzeigename des Besitzers, der das Steuerelement erstellt hat, suchen. 

Wenn Sie nicht wissen, welches Programm die Zugriffsüberprüfung enthält, navigieren Sie zur Zugriffsüberprüfungsseite, und klicken Sie auf **Steuerelemente**.  Sie können nach Steuerelementen eines bestimmten Typs filtern und dabei nach deren Status sortieren. Sie können auch nach dem Namen des Zugriffsüberprüfungs-Steuerelements oder dem Anzeigename des Besitzers suchen, der es erstellt hat. 

## <a name="retrieving-the-results-for-a-one-time-access-review"></a>Abrufen der Ergebnisse für eine einmalige Zugriffsüberprüfung

Ist als Wiederholungstyp der Überprüfung „Einmal“ festgelegt, können der Status einer aktiven Zugriffsüberprüfung und die Ergebnisse einer abgeschlossenen Zugriffsüberprüfung im Abschnitt **Ergebnisse** abgerufen werden.  Sie können den Anzeigenamen oder den Benutzerprinzipalname eines Benutzers eingeben, dessen Zugriff überprüft wird, um nur den Zugriff dieses Benutzers anzuzeigen.  Wenn Sie alle Ergebnisse einer abgeschlossenen Zugriffsüberprüfung abrufen möchten, klicken Sie auf die Schaltfläche **Herunterladen**.

## <a name="retrieving-the-results-for-multiple-instances-of-a-recurring-access-review"></a>Abrufen der Ergebnisse für mehrere Instanzen einer Serienzugriffsüberprüfung

Klicken Sie zum Anzeigen des Status einer aktiven Serienzugriffsüberprüfung auf den Abschnitt **Ergebnisse**.  Sie können den Anzeigenamen oder den Benutzerprinzipalname eines Benutzers eingeben, dessen Zugriff überprüft wird.

Klicken Sie zum Anzeigen der Ergebnisse einer abgeschlossenen Instanz einer Serienzugriffsüberprüfung auf **Ausführungsverlauf**. Wählen Sie anschließend basierend auf dem Start- und Enddatum der Instanz die spezifische Instanz in der Liste der abgeschlossenen Zugriffsüberprüfungsinstanzen aus.   Die Ergebnisse dieser Instanz können im Abschnitt **Ergebnisse** abgerufen werden.  Sie können den Anzeigenamen oder den Benutzerprinzipalname eines Benutzers eingeben, dessen Zugriff überprüft wird, um nur den Zugriff dieses Benutzers anzuzeigen.  Wenn Sie alle Ergebnisse einer abgeschlossenen Instanz einer Serienzugriffsüberprüfung abrufen möchten, klicken Sie auf die Schaltfläche **Herunterladen**.


## <a name="removing-users-from-an-access-review"></a>Entfernen von Benutzern aus einer Zugriffsüberprüfung

Ein gelöschter Benutzer bleibt in Azure AD standardmäßig 30 Tage lang gelöscht. In diesem Zeitraum kann er von einem Administrator wiederhergestellt werden, sofern erforderlich.  Nach 30 Tagen wird dieser Benutzer endgültig gelöscht.  Darüber hinaus kann ein globaler Administrator über das Azure Active Directory-Portal explizit [einen kürzlich gelöschten Benutzer endgültig löschen](fundamentals/active-directory-users-restore.md), bevor dieser Zeitraum abgelaufen ist.  Wurde ein Benutzer endgültig gelöscht, werden nachfolgende Daten zu diesem Benutzer aus aktiven Zugriffsüberprüfungen entfernt.  Überwachungsinformationen zu gelöschten Benutzern verbleiben im Überwachungsprotokoll.

## <a name="next-steps"></a>Nächste Schritte

- [Manage user access with Azure AD access reviews (Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen)](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Manage guest access with Azure AD access reviews](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md) (Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen)
- [Manage programs and controls for Azure AD access reviews](active-directory-azure-ad-controls-manage-programs-controls.md) (Verwalten der Programme und Kontrollen für Azure AD-Zugriffsüberprüfungen)
- [Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder den Zugriff auf eine Anwendung](active-directory-azure-ad-controls-create-access-review.md)
- [Erstellen einer Zugriffsüberprüfung von Benutzern in der Azure AD-Administratorrolle](privileged-identity-management/pim-how-to-start-security-review.md)


