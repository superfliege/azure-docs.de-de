---
title: Anzeigen und Zuweisen von Berechtigungen der Administratorrolle in Azure Active Directory | Microsoft-Dokumentation
description: Sie können jetzt Mitglieder einer Azure AD-Administratorrolle im Portal anzeigen und verwalten. Für Benutzer vorgesehen, die häufig Rollenzuweisungen verwalten.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/25/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: cb2e5286eb8e910b555e221242a735f00dff4778
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182823"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Anzeigen und Zuweisen von Benutzerrollen in Azure Active Directory

Sie können jetzt im Azure Active Directory-Portal sämtliche Mitglieder der Administratorrollen anzeigen und verwalten. Wenn Sie häufig Rollenzuweisungen verwalten, bevorzugen Sie diese Funktionalität wahrscheinlich. Und wenn Sie sich jemals gefragt haben, welchen Zweck diese Rollen haben, können Sie für jede der Azure AD-Administratorrollen eine ausführliche Liste mit Berechtigungen anzeigen.

Sie können auch Ihre eigenen Berechtigungen ohne großen Aufwand anzeigen. Klicken Sie auf **Ihre Rolle**, um Schnellzugriff auf Ihre Benutzerseite zu erhalten und eine Liste mit all Ihren aktiven zugewiesenen Rollen anzuzeigen. Klicken Sie rechts in den einzelnen Zeilen auf die Auslassungspunkte, um die ausführliche Beschreibung der Rolle zu öffnen.

![Liste der Rollen im Azure AD-Portal](./media/directory-manage-roles-portal/role-list.png)

Wählen Sie die Zeile für eine Rolle aus, um die Benutzer anzuzeigen, die der Rolle zugewiesen sind. Für weitere Verwaltungsfunktionen können Sie **Manage in PIM** (In PIM verwalten) auswählen. Administratoren für privilegierte Rollen können Zuweisungen vom Typ „Permanent“ (in der Rolle stets aktiv) in „Berechtigt“ (nur bei Erweiterung in die Rolle) ändern. Wenn Sie nicht über PIM verfügen, können Sie trotzdem **Manage in PIM** (In PIM verwalten) auswählen, um sich für eine Testversion zu registrieren. Für Privileged Identity Management ist ein [Azure AD Premium P2-Lizenzplan](../privileged-identity-management/subscription-requirements.md) erforderlich.

![Liste der Mitglieder einer Administratorrolle](./media/directory-manage-roles-portal/member-list.png)

Wenn Sie globaler Administrator oder Administrator für privilegierte Rollen sind, können Sie ganz einfach Mitglieder hinzufügen oder entfernen, die Liste filtern oder ein Mitglied auswählen, um dessen aktive zugewiesene Rollen anzuzeigen.

## <a name="view-role-permissions"></a>Anzeigen von Rollenberechtigungen

Wenn Sie die Mitglieder einer Rolle anzeigen, wählen Sie **Beschreibung** aus, um die vollständige Liste der von der Rollenzuweisung gewährten Berechtigungen anzuzeigen. Die Seite enthält Links zu relevanter Dokumentation, die Ihnen bei der Verwaltung von Verzeichnisrollen als Unterstützung dienen soll.

![Liste der Berechtigungen für eine Administratorrolle](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Nächste Schritte

* Im [Forum für Azure AD-Administratorrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) können Sie sich gerne mit uns in Verbindung setzen.
* Weitere Informationen zu Rollen und zur Zuweisung von Administratorrollen finden Sie unter [Zuweisen von Administratorrollen](directory-assign-admin-roles.md).
* Informationen zu Standardbenutzerberechtigungen finden Sie unter [Vergleich von Standardbenutzerberechtigungen für Gäste und Mitglieder](../fundamentals/users-default-permissions.md).
