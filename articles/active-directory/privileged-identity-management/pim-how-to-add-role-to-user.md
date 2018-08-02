---
title: Zuweisen von Verzeichnisrollen zu Benutzern mithilfe von Azure AD PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Benutzern mit Azure Active Directory Privileged Identity Management und dem Azure-Portal Verzeichnisrollen zuweisen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 07/23/2018
ms.author: rolyon
ms.openlocfilehash: 1834addb4e51030afda43a2d7acad5d7ffc1889a
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226695"
---
# <a name="assign-directory-roles-to-users-using-azure-ad-pim"></a>Zuweisen von Verzeichnisrollen zu Benutzern mithilfe von Azure AD PIM

Mit Azure Active Directory (Azure AD) kann ein globaler Administrator **permanente** Verzeichnisrollenzuweisungen vornehmen. Diese Rollenzuweisungen können über das [Azure-Portal](../users-groups-roles/directory-assign-admin-roles.md) oder über [PowerShell-Befehle](/powershell/module/azuread#directory_roles) erstellt werden.

Der Dienst Azure AD Privileged Identity Management (PIM) ermöglicht es auch Administratoren für privilegierte Rollen, permanente Verzeichnisrollenzuweisungen vorzunehmen. Außerdem können Administratoren für privilegierte Rollen Benutzer als für Verzeichnisrollen **berechtigt** festlegen. Ein berechtigter Administrator kann die Rolle bei Bedarf aktivieren, und die entsprechenden Berechtigungen laufen nach einem bestimmten Zeitraum ab. Informationen zu den Rollen, die Sie mit PIM verwalten können, finden Sie unter [Verschiedene Administratorrollen in Azure Active Directory PIM](pim-roles.md).

## <a name="make-a-user-eligible-for-a-role"></a>Festlegen eines Benutzers als „berechtigt“ für eine Rolle

Führen Sie folgende Schritte aus, um einen Benutzer für eine Azure AD-Verzeichnisrolle als „berechtigt“ festzulegen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Benutzer an, der ein Mitglied der Rolle [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) ist.

    Weitere Informationen darüber, wie Sie einem anderen Benutzer Zugriff auf die Verwaltung von PIM gewähren, finden Sie unter [Gewähren des Zugriffs für einen anderen Benutzer zur Verwaltung von PIM](pim-how-to-give-access-to-pim.md).

1. Öffnen Sie **Azure AD Privileged Identity Management**.

    Wenn PIM im Azure-Portal noch nicht aktiviert ist, finden Sie entsprechende Informationen unter [Erste Schritte mit Azure AD Privileged Identity Management](pim-getting-started.md).

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Rolle (Vorschau)** oder **Mitglieder**.

    ![Azure AD-Verzeichnisrollen](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Klicken Sie auf **Mitglied hinzufügen**, um das Dialogfeld „Verwaltete Mitglieder hinzufügen“ zu öffnen.

1. Klicken Sie auf **Rolle auswählen**, klicken Sie auf eine Rolle, die Sie verwalten möchten, und klicken Sie dann auf **Auswählen**.

    ![Auswählen einer Rolle](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Klicken Sie auf **Mitglieder auswählen**, wählen Sie die Benutzer aus, die der Rolle zugewiesen werden sollen, und klicken Sie dann auf **Auswählen**.

    ![Auswählen einer Rolle](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. Klicken Sie in „Verwaltete Mitglieder hinzufügen“ auf **OK**, um den Benutzer zur Rolle hinzuzufügen.

     Nach dem Zuweisen der Rolle wird der von Ihnen ausgewählte Benutzer in der Liste als **berechtigt** für die Rolle angezeigt.

    ![Berechtigter Benutzer für eine Rolle](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Nachdem der Benutzer für eine Rolle berechtigt ist, informieren Sie ihn darüber, dass er die Rolle entsprechend den Anweisungen unter [Gewusst wie: Aktivieren oder Deaktivieren einer Rolle](pim-how-to-activate-role.md)aktivieren kann.

    Berechtigte Administratoren werden aufgefordert, sich während der Aktivierung für die Multi-Factor Authentication (MFA) von Azure zu registrieren. Wenn sich ein Benutzer nicht für MFA registrieren kann oder ein Microsoft-Konto nutzt (in der Regel @outlook.com), müssen Sie ihn in allen Rollen als permanent einrichten.

## <a name="make-a-role-assignment-permanent"></a>Einrichten einer permanenten Rollenzuweisung

Standardmäßig sind neue Benutzer nur für eine Verzeichnisrolle berechtigt. Führen Sie die folgenden Schritte aus, wenn Sie eine Rollenzuweisung permanent festlegen möchten.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Mitglieder**.

    ![Liste der Mitglieder](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Klicken Sie auf eine **berechtigte** Rolle, die Sie als permanente Rollenzuweisung festlegen möchten.

1. Klicken Sie auf **Weitere**, und klicken Sie anschließend auf **Als permanent festlegen**.

    ![Einrichten einer permanenten Rollenzuweisung](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    Die Rolle wird jetzt als **permanent** aufgeführt.

    ![Liste der Mitglieder mit Änderung in „permanent“](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Entfernen eines Benutzers aus einer Rolle

Sie können Benutzer aus Rollenzuweisungen entfernen. Stellen Sie dabei jedoch sicher, dass immer mindestens ein Benutzer als permanenter globaler Administrator vorhanden ist. Wenn Sie nicht sicher sind, welche Benutzer ihre Rollenzuweisungen noch benötigen, können Sie [eine Zugriffsüberprüfung für die Rolle starten](pim-how-to-start-security-review.md).

Führen Sie die folgenden Schritte aus, um einen bestimmten Benutzer aus einer Verzeichnisrolle zu entfernen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Mitglieder**.

    ![Liste der Mitglieder](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Klicken Sie auf die zu entfernende Rollenzuweisung.

1. Klicken Sie auf **Weitere** und anschließend auf **Entfernen**.

    ![Entfernen einer Rolle](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. Klicken Sie im Bestätigungsdialogfeld auf **Ja**.

    ![Entfernen einer Rolle](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    Die Rollenzuweisung wurde entfernt.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
