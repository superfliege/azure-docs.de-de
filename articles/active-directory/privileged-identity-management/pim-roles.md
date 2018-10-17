---
title: Azure AD-Verzeichnisrollen, die Sie in PIM verwalten können | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Azure AD-Verzeichnisrollen, die Sie in Azure AD Privileged Identity Management (PIM) verwalten können.
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
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: 63f4c4c48dd0fed3130dc99929a0e84c3cba2026
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46466046"
---
# <a name="azure-ad-directory-roles-you-can-manage-in-pim"></a>Azure AD-Verzeichnisrollen, die Sie in PIM verwalten können
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Sie können Benutzern Ihrer Organisation in Azure AD verschiedene Administratorrollen zuweisen. Mit diesen Rollenzuweisungen wird gesteuert, welche Aufgaben (beispielsweise das Hinzufügen oder Entfernen von Benutzern oder das Ändern von Diensteinstellungen) Benutzer in Azure AD, Office 365 und weiteren Microsoft Online Services und zugehörigen Anwendungen ausführen können.  

Ein globaler Administrator kann die **permanente** Rollenzuweisung von Benutzern in Azure AD aktualisieren. Hierzu können das Portal, wie unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](../users-groups-roles/directory-assign-admin-roles.md) beschrieben, oder [PowerShell-Befehle](/powershell/module/azuread#directory_roles) verwendet werden.

Azure AD Privileged Identity Management (PIM) verwaltet Richtlinien für den privilegierten Zugriff für Benutzer in Azure AD. PIM weist Benutzer einer oder mehreren Rollen in Azure AD zu, und Sie können eine Person permanent der Rolle zuweisen oder als für die Rolle berechtigt festlegen. Wenn ein Benutzer einer Rolle permanent zugewiesen ist oder eine berechtigte Rollenzuweisung aktiviert, kann er mit den dieser Rolle zugewiesenen Berechtigungen Verwaltungsaufgaben in Azure Active Directory, Office 365 und andere Anwendungen ausführen.

Es gibt keinen Unterschied hinsichtlich des Zugriffs zwischen einer permanenten und einer berechtigten Rollenzuweisung. Der einzige Unterschied ist, dass einige Benutzer den Zugriff nicht jederzeit benötigen. Sie werden als für die Rolle berechtigt konfiguriert und können die Rolle aktivieren und deaktivieren, so wie sie es benötigten.

## <a name="roles-managed-in-pim"></a>In PIM verwaltete Rollen
Mit Privileged Identity Management können Sie Benutzer zu allgemeinen Administratorrollen zuweisen, einschließlich der folgenden:

* **Globaler Administrator:** (auch als Unternehmensadministrator bezeichnet) hat Zugriff auf alle administrativen Funktionen. Sie können über mehrere globale Administratoren in Ihrer Organisation verfügen. Die Person, die sich für den Erwerb von Office 365 registriert, wird automatisch zum globalen Administrator.
* **Administrator für privilegierte Rollen:** verwaltet Azure AD PIM und aktualisiert die Rollenzuweisungen für andere Benutzer.  
* **Rechnungsadministrator:** Erledigt Käufe, verwaltet Abonnements, verwaltet Supporttickets und überwacht die Dienstintegrität.
* **Kennwortadministrator** : setzt Kennwörter zurück, verwaltet Dienstanforderungen und überwacht die Dienstintegrität. Kennwortadministratoren können nur Kennwörter für Benutzer zurücksetzen.
* **Dienstadministrator** : verwaltet Dienstanforderungen und überwacht die Dienstintegrität.
  
  > [!NOTE]
  > Wenn Sie Office 365 verwenden, weisen Sie die Benutzeradministratorberechtigungen einem Dienst zu (z. B. Exchange Online), bevor Sie die Dienstadministratorrolle einem Benutzer zuweisen.
  > 
  > 
* **Benutzeradministrator:** setzt Kennwörter zurück, überwacht die Dienstintegrität und verwaltet Benutzerkonten, Benutzergruppen und Dienstanforderungen. Der Benutzeradministrator kann keinen globalen Administrator löschen, keine weiteren Administratorrollen erstellen und keine Kennwörter für Rechnungs-, Dienst- und globale Administratoren zurücksetzen.
* **Exchange-Administrator:** hat Administratorzugriff auf Exchange Online über das Exchange Admin Center (EAC) und darf nahezu jede Aufgabe in Exchange Online ausführen.
* **SharePoint-Dienstadministrator:** hat Administratorzugriff auf SharePoint Online über das SharePoint Online Admin Center und darf nahezu jede Aufgabe in SharePoint Online ausführen. Bei berechtigten Benutzern treten bei Verwendung dieser Rolle innerhalb von SharePoint nach der Aktivierung in PIM u.U. Verzögerungen auf.
* **Skype for Business-Administrator:** hat Administratorzugriff auf Skype for Business über das Skype for Business Admin Center und darf nahezu jede Aufgabe in Skype for Business Online ausführen.

Lesen Sie diese Artikel, um weitere Informationen zum [Zuweisen von Administratorrollen in Azure AD](../users-groups-roles/directory-assign-admin-roles.md) und [Zuweisen von Administratorrollen in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504) zu erhalten.

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


In PIM können Sie [diese Rollen einem Benutzer zuweisen](pim-how-to-add-role-to-user.md), sodass der Benutzer die [Rolle bei Bedarf aktivieren](pim-how-to-activate-role.md) kann.

Wenn Sie einem anderen Benutzer Verwaltungszugriff für PIM gewähren möchten, finden Sie eine Beschreibung der Rollen, die ein Benutzer für PIM benötigt, unter [Gewähren von Zugriff auf PIM](pim-how-to-give-access-to-pim.md).

<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>In PIM nicht verwaltete Rollen
Mit Ausnahme der oben genannten Rollen werden Rollen in Exchange Online oder SharePoint Online nicht durch Azure AD verwaltet und sind daher in PIM nicht sichtbar. Informationen zum Ändern von differenzierten Rollenzuweisungen in diesen Office 365-Diensten finden Sie unter [Berechtigungen in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Benutzerrollen und Durchführen der Anmeldung
Bei einigen Microsoft-Diensten und -Anwendungen genügt es möglicherweise nicht, einen Benutzer einer Rolle zuzuweisen, um diesen Benutzer zu einem Administrator zu machen.

Für den Zugriff auf das Azure-Portal muss der Benutzer ein Besitzer eines Azure-Abonnements sein, selbst wenn der Benutzer die Azure-Abonnements nicht verwalten muss.  Um beispielsweise Konfigurationseinstellungen für Azure AD zu verwalten, muss ein Benutzer sowohl globaler Administrator in Azure AD als auch Besitzer eines Azure-Abonnements sein.  Informationen zum Hinzufügen von Benutzern zu Azure-Abonnements finden Sie unter [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](../..//role-based-access-control/role-assignments-portal.md).

Für den Zugriff auf Microsoft Online Services ist es möglicherweise erforderlich, einem Benutzer eine Lizenz zuzuweisen, bevor dieser das Dienstportal öffnen oder Verwaltungsaufgaben ausführen kann.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Zuweisen einer Lizenz an einen Benutzer in Azure AD

1. Melden Sie sich mit einer globalen Administrator- oder Besitzerrolle beim [Azure-Portal](http://portal.azure.com) an.

1. Wählen Sie das Azure AD-Verzeichnis aus, das Sie verwenden möchten, und dem Lizenzen zugeordnet sind.

1. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.

1. Klicken Sie auf **Lizenzen**. Die Liste der verfügbaren Lizenzen wird angezeigt.

    ![Azure Active Directory-Lizenzen](./media/pim-roles/licenses-overview.png)

1. Klicken Sie auf Ihr **Produkt**.

1. Wählen Sie den Lizenzplan aus, der die zu verteilenden Lizenzen enthält.

    ![Lizenzierte Produkte](./media/pim-roles/licenses-products.png)

1. Klicken Sie auf **Zuweisen**, um den Bereich „Lizenz zuweisen“ zu öffnen.

    ![Lizenzierte Benutzer](./media/pim-roles/licenses-licensed-users.png)

1. Wählen Sie den Benutzer oder die Gruppe aus, dem bzw. der Sie eine Lizenz zuweisen möchten.

    ![Lizenz zuweisen](./media/pim-roles/licenses-assign-license.png)

1. Klicken Sie auf **Zuweisungsoptionen**, um Ihre Zuweisungsoptionen zu konfigurieren.

    ![Zuweisungsoptionen](./media/pim-roles/licenses-assignment-options.png)

1. Klicken Sie auf **Zuweisen**, um die Lizenz zuzuweisen. Der Benutzer hat nun die Lizenz.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

- [Einstieg in die Verwendung von PIM](pim-getting-started.md)
- [Zuweisen von Azure AD-Verzeichnisrollen in PIM](pim-how-to-add-role-to-user.md)

