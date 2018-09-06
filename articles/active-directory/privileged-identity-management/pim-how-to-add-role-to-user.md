---
title: Zuweisen von Azure AD-Verzeichnisrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure AD-Verzeichnisrollen in Azure AD Privileged Identity Management (PIM) zuweisen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 07/23/2018
ms.author: rolyon
ms.openlocfilehash: 33bfe28bf612c47c9f42345dabccc017337c3d45
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190155"
---
# <a name="assign-azure-ad-directory-roles-in-pim"></a>Zuweisen von Azure AD-Verzeichnisrollen in PIM

Mit Azure Active Directory (Azure AD) kann ein globaler Administrator **permanente** Verzeichnisrollenzuweisungen vornehmen. Diese Rollenzuweisungen können über das [Azure-Portal](../users-groups-roles/directory-assign-admin-roles.md) oder über [PowerShell-Befehle](/powershell/module/azuread#directory_roles) erstellt werden.

Der Dienst Azure AD Privileged Identity Management (PIM) ermöglicht es auch Administratoren für privilegierte Rollen, permanente Verzeichnisrollenzuweisungen vorzunehmen. Außerdem können Administratoren für privilegierte Rollen Benutzer als für Verzeichnisrollen **berechtigt** festlegen. Ein berechtigter Administrator kann die Rolle bei Bedarf aktivieren, und die entsprechenden Berechtigungen laufen nach einem bestimmten Zeitraum ab. Informationen zu den Rollen, die Sie mit PIM verwalten können, finden Sie unter [Azure AD-Verzeichnisrollen, die Sie in PIM verwalten können](pim-roles.md).

## <a name="make-a-user-eligible-for-a-role"></a>Festlegen eines Benutzers als „berechtigt“ für eine Rolle

Führen Sie folgende Schritte aus, um einen Benutzer für eine Azure AD-Verzeichnisrolle als „berechtigt“ festzulegen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Benutzer an, der ein Mitglied der Rolle [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) ist.

    Informationen dazu, wie Sie anderen Administratoren Zugriff für die Verwaltung in PIM gewähren, finden Sie unter [Gewähren von Zugriff für andere Administratoren zum Verwalten von PIM](pim-how-to-give-access-to-pim.md).

1. Öffnen Sie **Azure AD Privileged Identity Management**.

    Wenn Sie PIM noch nicht im Azure-Portal geöffnet haben, wechseln Sie zu [Einstieg in die Verwendung von PIM](pim-getting-started.md).

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Rollen** oder **Mitglieder**.

    ![Azure AD-Verzeichnisrollen](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Klicken Sie auf **Mitglied hinzufügen**, um das Dialogfeld „Verwaltete Mitglieder hinzufügen“ zu öffnen.

1. Klicken Sie auf **Rolle auswählen**, klicken Sie auf eine Rolle, die Sie verwalten möchten, und klicken Sie dann auf **Auswählen**.

    ![Auswählen einer Rolle](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Klicken Sie auf **Mitglieder auswählen**, wählen Sie die Benutzer aus, die der Rolle zugewiesen werden sollen, und klicken Sie dann auf **Auswählen**.

    ![Auswählen einer Rolle](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. Klicken Sie in „Verwaltete Mitglieder hinzufügen“ auf **OK**, um den Benutzer zur Rolle hinzuzufügen.

1. Klicken Sie in der Liste der Rollen auf die soeben zugewiesene Rolle, um die Liste der Mitglieder anzuzeigen.

     Nach dem Zuweisen der Rolle wird der von Ihnen ausgewählte Benutzer in der Liste als **Berechtigt** für die Rolle angezeigt.

    ![Berechtigter Benutzer für eine Rolle](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Nachdem der Benutzer jetzt für die Rolle berechtigt ist, informieren Sie ihn darüber, dass er die Rolle entsprechend den Anweisungen unter [Aktivieren von Azure AD-Verzeichnisrollen in PIM](pim-how-to-activate-role.md) aktivieren kann.

    Berechtigte Administratoren werden aufgefordert, sich während der Aktivierung für die Multi-Factor Authentication (MFA) von Azure zu registrieren. Wenn sich ein Benutzer nicht für MFA registrieren kann oder ein Microsoft-Konto nutzt (in der Regel @outlook.com), müssen Sie ihn in allen Rollen als permanent einrichten.

## <a name="make-a-role-assignment-permanent"></a>Einrichten einer permanenten Rollenzuweisung

Standardmäßig sind neue Benutzer nur für eine Verzeichnisrolle berechtigt. Führen Sie die folgenden Schritte aus, wenn Sie eine Rollenzuweisung permanent festlegen möchten.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Verzeichnisrollen**.

1. Klicken Sie auf **Mitglieder**.

    ![Liste der Mitglieder](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Klicken Sie auf eine als **Berechtigt** markierte Rolle, die Sie als dauerhafte Rollenzuweisung festlegen möchten.

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

- [Konfigurieren von Einstellungen für Azure AD-Verzeichnisrollen in PIM](pim-how-to-change-default-settings.md)
- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
