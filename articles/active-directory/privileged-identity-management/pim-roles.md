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
ms.openlocfilehash: c8870e4da63dc52d5926bdfd5f7c9b3218e5f53a
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188985"
---
# <a name="azure-ad-directory-roles-you-can-manage-in-pim"></a>Azure AD-Verzeichnisrollen, die Sie in PIM verwalten können
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Sie können Benutzern Ihrer Organisation in Azure AD verschiedene Administratorrollen zuweisen. Mit diesen Rollenzuweisungen wird gesteuert, welche Aufgaben (beispielsweise das Hinzufügen oder Entfernen von Benutzern oder das Ändern von Diensteinstellungen) Benutzer in Azure AD, Office 365 und weiteren Microsoft Online Services und zugehörigen Anwendungen ausführen können.  

Ein globaler Administrator kann die **permanente** Rollenzuweisung von Benutzern in Azure AD aktualisieren. Hierzu können das Portal, wie unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](../users-groups-roles/directory-assign-admin-roles.md) beschrieben, oder [PowerShell-Befehle](/powershell/module/azuread#directory_roles) verwendet werden.

Azure AD Privileged Identity Management (PIM) verwaltet Richtlinien für den privilegierten Zugriff für Benutzer in Azure AD. PIM weist Benutzer einer oder mehreren Rollen in Azure AD zu, und Sie können eine Person permanent der Rolle zuweisen oder als für die Rolle berechtigt festlegen. Wenn ein Benutzer einer Rolle permanent zugewiesen ist oder eine berechtigte Rollenzuweisung aktiviert, kann er mit den dieser Rolle zugewiesenen Berechtigungen Verwaltungsaufgaben in Azure Active Directory, Office 365 und andere Anwendungen ausführen.

Es gibt keinen Unterschied hinsichtlich des Zugriffs zwischen einer permanenten und einer berechtigten Rollenzuweisung. Der einzige Unterschied ist, dass einige Benutzer den Zugriff nicht jederzeit benötigen. Sie werden als für die Rolle berechtigt konfiguriert und können die Rolle aktivieren und deaktivieren, so wie sie es benötigten.

## <a name="roles-managed-in-pim"></a>In PIM verwaltete Rollen
Mit Privileged Identity Management können Sie Benutzer zu allgemeinen Administratorrollen zuweisen, einschließlich der folgenden:

* **Globaler Administrator:** (wird auch als Unternehmensadministrator bezeichnet) hat Zugriff auf alle administrativen Funktionen. Sie können über mehrere globale Administratoren in Ihrer Organisation verfügen. Die Person, die sich für den Erwerb von Office 365 registriert, wird automatisch zum globalen Administrator.
* **Administrator für privilegierte Rollen:** verwaltet Azure AD PIM und aktualisiert die Rollenzuweisungen für andere Benutzer.  
* **Rechnungsadministrator** : erledigt Käufe, verwaltet Abonnements, verwaltet Supporttickets und überwacht die Dienstintegrität.
* **Kennwortadministrator** : setzt Kennwörter zurück, verwaltet Dienstanforderungen und überwacht die Dienstintegrität. Kennwortadministratoren können nur Kennwörter für Benutzer zurücksetzen.
* **Dienstadministrator** : verwaltet Dienstanforderungen und überwacht die Dienstintegrität.
  
  > [!NOTE]
  > Wenn Sie Office 365 verwenden, weisen Sie die Benutzeradministratorberechtigungen einem Dienst zu (z. B. Exchange Online), bevor Sie die Dienstadministratorrolle einem Benutzer zuweisen.
  > 
  > 
* **Benutzerverwaltungsadministrator:** setzt Kennwörter zurück, überwacht die Dienstintegrität und verwaltet Benutzerkonten, Benutzergruppen und Dienstanforderungen. Der Benutzerverwaltungsadministrator kann keinen globalen Administrator löschen, keine weiteren Administratorrollen erstellen und keine Kennwörter für Rechnungs-, Dienst- und globale Administratoren zurücksetzen.
* **Exchange-Administrator:** hat Administratorzugriff auf Exchange Online über das Exchange Admin Center (EAC) und darf nahezu jede Aufgabe in Exchange Online ausführen.
* **SharePoint-Administrator (Vorschauversion):** hat Administratorzugriff auf SharePoint Online über das SharePoint Online Admin Center und darf nahezu jede Aufgabe in SharePoint Online ausführen. Diese Rolle befindet sich derzeit in der Vorschauphase. Bei berechtigten Benutzern treten bei Verwendung dieser Rolle innerhalb von SharePoint nach der Aktivierung in PIM u.U. Verzögerungen auf.
* **Skype for Business-Administrator:** hat Administratorzugriff auf Skype for Business über das Skype for Business Admin Center und darf nahezu jede Aufgabe in Skype for Business Online ausführen.

Lesen Sie diese Artikel, um weitere Informationen zum [Zuweisen von Administratorrollen in Azure AD](../users-groups-roles/directory-assign-admin-roles.md) und [Zuweisen von Administratorrollen in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504) zu erhalten.

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


In PIM können Sie [diese Rollen einem Benutzer zuweisen](pim-how-to-add-role-to-user.md), sodass der Benutzer die [Rolle bei Bedarf aktivieren](pim-how-to-activate-role.md) kann.

Wenn Sie einem anderen Benutzer Verwaltungszugriff für PIM gewähren möchten, finden Sie eine Beschreibung der Rollen, die ein Benutzer für PIM benötigt, unter [Gewähren von Zugriff auf PIM](pim-how-to-give-access-to-pim.md).

<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>In PIM nicht verwaltete Rollen
Mit Ausnahme der oben genannten Rollen werden Rollen in Exchange Online oder SharePoint Online nicht durch Azure AD verwaltet und sind daher in PIM nicht sichtbar. Informationen zum Ändern von differenzierten Rollenzuweisungen in diesen Office 365-Diensten finden Sie unter [Berechtigungen in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure-Abonnements und Ressourcengruppen werden auch nicht in Azure AD dargestellt. Weitere Informationen zum Verwalten von Azure-Abonnements finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](../../billing/billing-add-change-azure-subscription-administrator.md). Weitere Informationen zu Azure RBAC finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/role-assignments-portal.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Benutzerrollen und Durchführen der Anmeldung
Bei einigen Microsoft-Diensten und -Anwendungen genügt es möglicherweise nicht, einen Benutzer einer Rolle zuzuweisen, um diesen Benutzer zu einem Administrator zu machen.

Für den Zugriff auf das Azure-Portal muss der Benutzer Dienst- oder Co-Administrator eines Azure-Abonnements sein, selbst wenn der Benutzer die Azure-Abonnements nicht verwalten muss.  Um beispielsweise Konfigurationseinstellungen für Azure AD zu verwalten, muss ein Benutzer sowohl globaler Administrator in Azure AD als auch Co-Administrator eines Azure-Abonnements sein.  Informationen zum Hinzufügen von Benutzern zu Azure-Abonnements finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](../../billing/billing-add-change-azure-subscription-administrator.md).

Für den Zugriff auf Microsoft Online Services ist es möglicherweise erforderlich, einem Benutzer eine Lizenz zuzuweisen, bevor dieser das Dienstportal öffnen oder Verwaltungsaufgaben ausführen kann.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Zuweisen einer Lizenz an einen Benutzer in Azure AD
1. Melden Sie sich mit einem globalen Administratorkonto oder Co-Administratorkonto beim [Azure-Portal](http://portal.azure.com) an.
3. Wählen Sie Azure AD und das Verzeichnis aus, das Sie verwenden möchten, und dem Lizenzen zugeordnet sind.
4. Wählen Sie **Lizenzen** auf der linken Seite aus. Die Liste der verfügbaren Lizenzen wird angezeigt.
5. Wählen Sie den Lizenzplan aus, der die zu verteilenden Lizenzen enthält.
6. Wählen Sie **Benutzer zuweisen**aus.
7. Wählen Sie den Benutzer aus, dem Sie eine Lizenz zuweisen möchten.
8. Klicken Sie auf die Schaltfläche **Zuweisen** .  Der Benutzer kann sich jetzt bei Azure anmelden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

- [Einstieg in die Verwendung von PIM](pim-getting-started.md)
- [Zuweisen von Azure AD-Verzeichnisrollen in PIM](pim-how-to-add-role-to-user.md)

