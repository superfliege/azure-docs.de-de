---
title: Aktivieren von Azure AD-Verzeichnisrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure AD-Verzeichnisrollen in Azure AD Privileged Identity Management (PIM) aktivieren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/21/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0023ac374ef70593d0ab2d9589c99d0f37e19ff8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189598"
---
# <a name="activate-my-azure-ad-directory-roles-in-pim"></a>Aktivieren von Azure AD-Verzeichnisrollen in PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) vereinfacht die Art und Weise, in der Unternehmen den privilegierten Zugriff auf Ressourcen in Azure AD und anderen Onlinediensten von Microsoft wie Office 365 oder Microsoft Intune verwalten.  

Wenn Sie für eine Administratorrolle berechtigt sind, können Sie diese Rolle aktivieren, um privilegierte Aufgaben durchzuführen. Wenn Sie z. B. gelegentlich Office 365-Funktionen verwalten, dürfen Ihre privilegierte Rollenadministratoren Ihrer Organisation Sie nicht als permanenten globalen Administrator festlegen, da sich diese Rolle auf andere Dienste auswirkt. Stattdessen gewähren sie Ihnen Berechtigungen für Azure AD-Rollen wie z. B. Exchange Online-Administrator. Sie können eine Aktivierung dieser Rolle anfordern, wenn Sie diese Berechtigungen benötigen, und verfügen damit für einen bestimmten Zeitraum über Administratorkontrolle.

Dieser Artikel richtet sich an Administratoren, die ihre Azure AD-Rolle in PIM aktivieren müssen.

## <a name="activate-a-role"></a>Aktivieren einer Rolle

Wenn Sie eine Azure AD-Verzeichnisrolle übernehmen müssen, können Sie in PIM über die Navigationsoption **Meine Rollen** deren Aktivierung anfordern.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**. Informationen dazu, wie Sie die PIM-Kachel zu Ihrem Dashboard hinzufügen, finden Sie unter [Einstieg in die Verwendung von PIM](pim-getting-started.md).

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Meine Rollen**, um eine Liste der qualifizierten Azure AD-Verzeichnisrollen anzuzeigen.

    ![Azure AD-Verzeichnisrollen – „Meine Rollen“](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Suchen Sie nach einer Rolle, die Sie aktivieren möchten.

    ![Azure AD-Verzeichnisrollen – Liste „Meine Rollen“](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Klicken Sie auf **Aktivieren**, um den Detailbereich für die Rollenaktivierung zu öffnen.

1. Wenn Ihre Rolle eine mehrstufige Authentifizierung erfordert, klicken Sie auf **Überprüfen Sie Ihre Identität, bevor Sie den Vorgang fortsetzen**. Sie müssen sich nur einmal pro Sitzung authentifizieren.

    ![Überprüfung mit MFA vor der Rollenaktivierung](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Klicken Sie auf **Meine Identität überprüfen**, und folgen Sie den Anweisungen zur Bereitstellung einer zusätzlichen Sicherheitsüberprüfung.

    ![Zusätzliche Sicherheitsüberprüfung](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Klicken Sie auf **Aktivieren**, um den Bereich für die Aktivierung zu öffnen.

    ![Bereich „Aktivierung“](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Falls erforderlich, geben Sie einen Startzeitpunkt für die Aktivierung an.

1. Geben Sie die Dauer der Aktivierung an.

1. Geben Sie im Feld **Grund für Aktivierung** den Grund für die Aktivierungsanforderung ein. Für einige Rollen müssen Sie eine Ticketnummer zur Problembehebung angeben.

    ![Ausgefüllter Bereich „Aktivierung“](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Klicken Sie auf **Aktivieren**.

    Wenn für die Rolle keine Genehmigung erforderlich ist, wird sie aktiviert und der Liste der aktiven Rollen hinzugefügt. Wenn Sie die Rolle sofort verwenden möchten, führen Sie die Schritte im nächsten Abschnitt aus.

    Wenn für die Aktivierung der [Rolle eine Genehmigung erforderlich ist](./azure-ad-pim-approval-workflow.md), werden Sie über eine Benachrichtigung in der oberen rechten Ecke des Browsers darüber informiert, dass die Genehmigung der Anforderung aussteht.

    ![Benachrichtigung über ausstehende Anforderung](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Verwenden einer Rolle sofort nach der Aktivierung

Nach der Aktivierung einer Rolle in PIM dauert es mindestens zehn Minuten, bevor Sie auf das gewünschte Verwaltungsportal zugreifen oder Funktionen für eine spezifische Verwaltungsworkload ausführen können. Um ein Update Ihrer Berechtigungen zu erzwingen, verwenden Sie die Seite **Anwendungszugriff** wie in den folgenden Schritten beschrieben.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf die Seite **Anwendungszugriff**.

    ![PIM-Anwendungszugriff](./media/pim-how-to-activate-role/pim-application-access.png)

1. Klicken Sie auf den Link **Azure Active Directory**, um das Portal auf der Seite **Alle Benutzer** erneut zu öffnen.

    Wenn Sie auf diesen Link klicken, wird Ihr aktuelles Token ungültig. Gleichzeitig wird erzwungen, dass das Azure-Portal ein neues Token abruft, das Ihre aktualisierten Berechtigungen enthält.

## <a name="view-the-status-of-your-requests"></a>Anzeigen des Status Ihrer Anforderungen

Sie können den Status Ihrer ausstehenden Aktivierungsanforderungen anzeigen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Meine Anforderungen**, um eine Liste Ihrer Anforderungen anzuzeigen.

    ![Azure AD-Verzeichnisrollen – „Meine Anforderungen“](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Deaktivieren einer Rolle

Wenn eine Rolle aktiviert wurde, wird sie nach Erreichen des Zeitlimits (berechtigte Dauer) automatisch deaktiviert.

Wenn Sie Ihre Administratoraufgaben früher als geplant beendet haben, können Sie eine Rolle auch manuell in Azure AD Privileged Identity Management deaktivieren.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Meine Rollen**.

1. Klicken sie auf **Aktive Rollen**, um die Liste Ihrer aktiven Rollen anzuzeigen.

1. Suchen Sie nach der Rolle, die Sie nicht mehr benötigen, und klicken Sie auf **Deaktivieren**.

## <a name="cancel-a-pending-request"></a>Abbrechen einer ausstehenden Anforderung

Sollten Sie die Aktivierung einer Rolle, für die eine Genehmigung erforderlich ist, nicht mehr benötigen, können Sie eine ausstehende Anforderung jederzeit abbrechen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Meine Anforderungen**.

1. Klicken Sie für die Rolle, für die Sie eine ausstehende Anforderung abbrechen möchten, auf **Abbrechen**.

    Durch das Klicken auf „Abbrechen“ wird die Anforderung abgebrochen. Um die Rolle erneut zu aktivieren, müssen Sie eine neue Anforderung zur Aktivierung übermitteln.

   ![Abbrechen einer ausstehenden Anforderung](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Problembehandlung

### <a name="permissions-not-granted-after-activating-a-role"></a>Keine Gewährung von Berechtigungen nach der Aktivierung einer Rolle

Nach der Aktivierung einer Rolle in PIM dauert es mindestens zehn Minuten, bevor Sie auf das gewünschte Verwaltungsportal zugreifen oder Funktionen für eine spezifische Verwaltungsworkload ausführen können. Um ein Update Ihrer Berechtigungen zu erzwingen, verwenden Sie die Seite **Anwendungszugriff** wie weiter oben unter [Verwenden einer Rolle sofort nach der Aktivierung](#use-a-role-immediately-after-activation) beschrieben.

Weitere Schritte zur Problembehandlung finden Sie unter [Behandeln von Problemen mit erhöhten Berechtigungen](https://social.technet.microsoft.com/wiki/contents/articles/37568.troubleshooting-elevated-permissions-with-azure-ad-privileged-identity-management.aspx).

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren von Azure-Ressourcenrollen in PIM](pim-resource-roles-activate-your-roles.md)
