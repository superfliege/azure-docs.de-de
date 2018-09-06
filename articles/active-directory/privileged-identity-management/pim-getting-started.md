---
title: Einstieg in die Verwendung von PIM – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie im Azure-Portal mit der Verwendung von Azure AD Privileged Identity Management (PIM) beginnen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 5b3bff27821964648713b02589c941c99e3eb03d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190088"
---
# <a name="start-using-pim"></a>Einstieg in die Verwendung von PIM

Mit Azure Active Directory Privileged Identity Management (PIM) können Sie den Zugriff innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dazu zählt der Zugriff auf Azure-Ressourcen, Azure AD und andere Microsoft-Onlinedienste wie Office 365 oder Microsoft Intune.

Dieser Artikel beschreibt, wie Sie die Azure AD PIM-App zu Ihrem Azure-Portaldashboard hinzufügen.

## <a name="first-person-to-use-pim"></a>Erster Benutzer in PIM

Wenn Sie die erste Person sind, die PIM in Ihrem Verzeichnis verwendet, werden Ihnen automatisch die Rollen [Sicherheitsadministrator](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) und [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) im Verzeichnis zugewiesen. Nur Administratoren für privilegierte Rollen können Azure AD-Verzeichnisrollenzuweisungen von Benutzern verwalten. Darüber hinaus können Sie den [Sicherheits-Assistenten](pim-security-wizard.md) ausführen. Dieser Assistent führt Sie durch die Erstermittlung und -zuweisung.

## <a name="add-pim-tile-to-the-dashboard"></a>Hinzufügen der PIM-Kachel zum Dashboard

Um das Öffnen von PIM zu vereinfachen, sollten Sie Ihrem Azure-Portal-Dashboard eine PIM-Kachel hinzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Verzeichnisses an.

1. Klicken Sie auf **Alle Dienste**, und suchen Sie nach dem Dienst **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management in „Alle Dienste“](./media/pim-getting-started/pim-all-services-find.png)

1. Klicken Sie, um den PIM-Schnellstart zu öffnen.

1. Aktivieren Sie die Option **Blatt an Dashboard anheften**, um das Blatt „PIM-Schnellstart“ an das Dashboard anzuheften.

    ![Blatt an Dashboard anheften](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Im Azure-Dashboard wird eine Kachel wie diese angezeigt:

    ![Kachel „PIM-Schnellstart“](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="navigate-to-your-tasks"></a>Navigieren zu Ihren Aufgaben

Sobald PIM eingerichtet ist, können Sie dieses Blatt für Ihre Aufgaben im Rahmen der Identitätsverwaltung verwenden.

![Aufgaben der obersten Ebene für PIM – Screenshot](./media/pim-getting-started/pim-quickstart-tasks.png)

| „Aufgaben“ und „Verwalten“ | Beschreibung |
| --- | --- |
| **Meine Rollen**  | Zeigt eine Liste mit berechtigten und aktiven Rollen an, die Ihnen zugewiesen sind. Hier können Sie zugewiesene berechtigte Rollen aktivieren. |
| **Meine Anforderungen** | Zeigt Ihre ausstehenden Anforderungen zum Aktivieren berechtigter Rollenzuweisungen an. |
| **Anwendungszugriff** | Ermöglicht es Ihnen, potenzielle Verzögerungen zu vermeiden und eine Rolle sofort nach ihrer Aktivierung zu verwenden. |
| **Genehmigen von Anforderungen** | Zeigt eine Liste mit Anforderungen zum Aktivieren berechtigter Rollen nach Benutzern in Ihrem Verzeichnis an, für deren Genehmigung Sie zuständig sind. |
| **Zugriff überprüfen** | Zeigt eine Liste mit aktiven Zugriffsüberprüfungen an, die Ihnen zugewiesen sind. Dabei spielt es keine Rolle, ob Sie den Zugriff für sich selbst oder für eine andere Person überprüfen. |
| **Azure AD-Verzeichnisrollen** | Zeigt ein Dashboard und Einstellungen für Administratoren privilegierter Rollen zum Verwalten von Azure AD-Verzeichnisrollenzuweisungen an. Für Benutzer, die kein Administrator für privilegierte Rollen sind, ist dieses Dashboard deaktiviert. Diese Benutzer haben Zugriff auf ein spezielles Dashboard namens „Meine Ansicht“. Auf dem Dashboard „Meine Ansicht“ werden nur Informationen zu dem Benutzer angezeigt, der auf das Dashboard zugreift (also keine Informationen für den gesamten Mandanten). |
| **Azure-Ressourcen** | Zeigt für Administratoren privilegierter Rollen ein Dashboard und Einstellungen zum Verwalten von Azure-Ressourcenrollenzuweisungen an. Für Benutzer, die kein Administrator für privilegierte Rollen sind, ist dieses Dashboard deaktiviert. Diese Benutzer haben Zugriff auf ein spezielles Dashboard namens „Meine Ansicht“. Auf dem Dashboard „Meine Ansicht“ werden nur Informationen zu dem Benutzer angezeigt, der auf das Dashboard zugreift (also keine Informationen für den gesamten Mandanten). |

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren von Azure AD-Verzeichnisrollen in PIM](pim-how-to-activate-role.md)
- [Aktivieren von Azure-Ressourcenrollen in PIM](pim-resource-roles-activate-your-roles.md)
