---
title: Was ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure? | Microsoft-Dokumentation
description: Übersicht über die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure. Verwenden von Rollenzuweisungen zur Steuerung des Zugriffs auf Ressourcen in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: role-based-access-control
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/02/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 4dcfb71e0adb05922603715e4dbcbdb243305927
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37438195"
---
# <a name="what-is-role-based-access-control-rbac"></a>Was ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)?

Die Zugriffsverwaltung für Cloudressourcen ist eine wichtige Funktion für jede Organisation, die die Cloud nutzt. Mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) können Sie verwalten, welche Benutzer Zugriff auf Azure-Ressourcen haben, welche Aktionen für diese Ressourcen ausgeführt werden können und auf welche Bereiche die Benutzer zugreifen können.

RBAC ist ein Autorisierungssystem, das auf [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) basiert und eine präzise Verwaltung des Zugriffs auf Ressourcen in Azure ermöglicht. Mithilfe von RBAC können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Anstatt allen uneingeschränkte Berechtigungen in Ihrem Azure-Abonnement oder Ihren Ressourcen zu gewähren, können Sie die Berechtigungen auf bestimmte Aktionen in einem bestimmten Bereich beschränken.

## <a name="what-can-i-do-with-rbac"></a>Welche Möglichkeiten bietet RBAC?

Hier sind einige Beispiele für die Verwendungsmöglichkeiten von RBAC:

- Ein Benutzer kann virtuelle Computer in einem Abonnement verwalten, während ein anderer Benutzer virtuelle Netzwerke verwalten kann
- Eine DBA-Gruppe kann SQL-Datenbanken in einem Abonnement verwalten
- Ein Benutzer kann sämtliche Ressourcen in einer Ressourcengruppe verwalten, wie z.B. virtuelle Computer, Websites und Subnetze
- Eine Anwendung kann auf sämtliche Ressourcen in einer Ressourcengruppe zugreifen

## <a name="how-rbac-works"></a>Funktionsweise von RBAC

Sie können den Zugriff auf Ressourcen mit RBAC steuern, indem Sie Rollenzuweisungen erstellen. Dies ist ein wesentliches Konzept, das zur Durchsetzung von Berechtigungen verwendet wird. Eine Rollenzuweisung besteht aus drei Elementen: Sicherheitsprinzipal, Rollendefinition und Bereich.

### <a name="security-principal"></a>Sicherheitsprinzipal

Ein *Sicherheitsprinzipal* ist ein Objekt, das einen Benutzer, eine Gruppe oder ein Dienstprinzipal darstellt, das Zugriff auf Azure-Ressourcen anfordert.

![Sicherheitsprinzipal für eine Rollenzuweisung](./media/overview/rbac-security-principal.png)

- Benutzer: eine Person, die über ein Profil in Azure Active Directory verfügt. Sie können auch bei anderen Mandanten Rollen zu Benutzern zuweisen. Weitere Informationen zu Benutzern in anderen Organisationen finden Sie unter [Azure Active Directory B2B](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
- Gruppe: eine Reihe von Benutzern, die in Azure Active Directory erstellt wurden. Wenn Sie einer Gruppe eine Rolle zuweisen, verfügen alle Benutzer in dieser Gruppe über diese Rolle. 
- Dienstprinzipal: eine Sicherheitsidentität, die von Anwendungen oder Diensten für den Zugriff auf bestimmte Azure-Ressourcen verwendet wird. Sie können sich diesen als *Benutzeridentität* (Benutzername und Kennwort oder Zertifikat) für eine Anwendung vorstellen.

### <a name="role-definition"></a>Rollendefinition

Eine *Rollendefinition* ist eine Sammlung von Berechtigungen. Gelegentlich wird sie auch einfach als *Rolle* bezeichnet. Eine Rollendefinition listet die ausführbaren Vorgänge wie etwa Lesen, Schreiben und Löschen auf. Rollen können auf allgemeiner Ebene erteilt werden (z.B. Benutzer) oder spezifisch sein (z.B. Leser virtueller Computer).

![Rollenzuweisung zu Rollendefinition hinzufügen](./media/overview/rbac-role-definition.png)

Azure umfasst mehrere [integrierte Rollen](built-in-roles.md), die Sie verwenden können. Im Folgenden werden vier grundlegende integrierte Rollen aufgeführt. Die ersten drei Rollen gelten für alle Ressourcentypen.

- [Besitzer](built-in-roles.md#owner) verfügen über vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren.
- [Mitwirkende](built-in-roles.md#contributor) können alle Arten von Azure-Ressourcen erstellen und verwalten, aber keinen anderen Personen Zugriff gewähren.
- [Leser](built-in-roles.md#reader) können vorhandene Azure-Ressourcen anzeigen.
- Mit einem [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) können Sie den Benutzerzugriff auf Azure-Ressourcen verwalten.

Die verbleibenden integrierten Rollen ermöglichen die Verwaltung bestimmter Azure-Ressourcen. Mit der Rolle [Mitwirkender von virtuellen Computern](built-in-roles.md#virtual-machine-contributor) können Benutzer beispielsweise virtuelle Computer erstellen und verwalten. Wenn die integrierten Rollen den Ansprüchen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen [benutzerdefinierten Rollen](custom-roles.md) erstellen.

Azure hat Datenvorgänge eingeführt (derzeit in der Vorschauversion), durch die Sie Zugriff auf Daten in einem Objekt erteilen können. Wenn ein Benutzer z.B. über Lesezugriff auf Daten in einem Speicherkonto verfügt, kann er die Blobs in diesem Speicherkonto lesen. Weitere Informationen finden Sie unter [Grundlegendes zu Rollendefinitionen](role-definitions.md).

### <a name="scope"></a>Bereich

Ein *Bereich* ist die für den Zugriff geltende Grenze. Wenn Sie eine Rolle zuweisen, können Sie die zulässigen Aktionen durch das Definieren eines Bereichs weiter einschränken. Dies ist hilfreich, wenn Sie einem Benutzer die Rolle [Mitwirkender von Websites](built-in-roles.md#website-contributor) zuweisen möchten, jedoch nur für eine Ressourcengruppe.

In Azure können Sie auf mehreren Ebenen einen Bereich angeben: Abonnement, Ressourcengruppe oder Ressource. Bereiche sind in einer Beziehung zwischen übergeordnetem und untergeordnetem Element strukturiert, in der jedem untergeordneten Element nur ein übergeordnetes Element zugewiesen ist.

![Bereich für eine Rollenzuweisung](./media/overview/rbac-scope.png)

Der Zugriff, den Sie auf übergeordneter Ebene zuweisen, wird auf untergeordneter Ebene geerbt. Beispiel:

- Wenn Sie einer Rolle im Abonnementkontext die Rolle [Leser](built-in-roles.md#reader) zuweisen, können die Mitglieder dieser Gruppe alle Ressourcengruppen und Ressourcen im Abonnement anzeigen.
- Wenn Sie einer Anwendung im Ressourcengruppenkontext die Rolle [Mitwirkender](built-in-roles.md#contributor) zuweisen, kann diese Ressourcen aller Typen in dieser Ressourcengruppe verwalten, jedoch keine anderen Ressourcengruppen des Abonnements.

Azure enthält auch einen Bereich oberhalb der Abonnements namens [Verwaltungsgruppen](../azure-resource-manager/management-groups-overview.md), der sich in der Vorschauversion befindet. Mithilfe von Verwaltungsgruppen können mehrere Abonnements verwaltet werden. Wenn Sie einen Bereich für RBAC angeben, können Sie eine Verwaltungsgruppe oder ein Abonnement, eine Ressourcengruppe oder eine Ressourcenhierarchie angeben.

### <a name="role-assignment"></a>Rollenzuweisung

Eine *Rollenzuweisung* ist der Prozess, in dem eine Rollenzuweisung zum Zwecke der Zugriffserteilung in einem bestimmten Bereich an einen Benutzer, eine Gruppe oder ein Dienstprinzipal gebunden wird. Der Zugriff wird durch Erstellen einer Rollenzuweisung erteilt und durch Entfernen einer Rollenzuweisung widerrufen.

Das folgende Diagramm zeigt ein Beispiel für eine Rollenzuweisung. In diesem Beispiel wurde der Gruppe „Marketing“ die Rolle [Mitwirkender](built-in-roles.md#contributor) für die Ressourcengruppe „Pharmavertrieb“ zugewiesen. Dies bedeutet, dass Benutzer in der Gruppe „Marketing“ in der Ressourcengruppe „Pharmavertrieb“ eine beliebige Azure-Ressource erstellen oder verwalten können. Benutzer der Gruppe „Marketing“ haben keinen Zugriff auf Ressourcen außerhalb der Ressourcengruppe „Pharmavertrieb“, sofern ihnen keine weitere Rolle zugewiesen wurde.

![Rollenzuweisung zum Steuern des Zugriffs](./media/overview/rbac-overview.png)

Sie können über das Azure-Portal, die Azure-Befehlszeilenschnittstelle, Azure PowerShell, Azure SDKs oder REST-APIs Rollenzuweisungen erstellen. In jedem Abonnement können Sie über bis zu 2.000 Rollenzuweisungen verfügen. Um Rollenzuweisungen erstellen und entfernen zu können, benötigen Sie die Berechtigung `Microsoft.Authorization/roleAssignments/*`. Diese Berechtigung wird über die Rolle [Besitzer](built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) erteilt.

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Gewähren des Zugriffs für einen Benutzer mithilfe der RBAC und des Azure-Portals](quickstart-assign-role-user-portal.md)
- [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](role-assignments-portal.md)
- [Grundlegendes zu den verschiedenen Rollen in Azure](rbac-and-directory-admin-roles.md)
