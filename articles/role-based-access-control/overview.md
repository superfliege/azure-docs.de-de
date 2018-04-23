---
title: Verwalten von Zugriff und Berechtigungen mit RBAC – Azure RBAC | Microsoft-Dokumentation
description: Führen Sie die ersten Schritte der Zugriffsverwaltung mit der rollenbasierten Zugriffssteuerung von Azure im Azure-Portal aus. Verwenden Sie Rollenzuweisungen, um in Ihrem Verzeichnis Berechtigungen zuzuweisen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/02/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: e31f2c71db6b08a64c5c4dec261597bb252a201e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="get-started-with-role-based-access-control-in-the-azure-portal"></a>Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal
Sicherheitsorientierte Unternehmen müssen sich darauf konzentrieren, Mitarbeitern genau die Berechtigungen zuzuweisen, die sie benötigen. Zu viele Berechtigungen können ein Konto zum leichten Angriffsziel machen. Wenn die Berechtigungen nicht ausreichen, können Mitarbeiter nicht effizient arbeiten. Die rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) begegnet diesem Problem, indem sie eine präzise Zugriffsverwaltung für Azure ermöglicht.

Mithilfe von RBAC können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Anstatt allen uneingeschränkte Berechtigungen in Ihrem Azure-Abonnement oder Ihren Ressourcen zu gewähren, können Sie die Berechtigungen auf bestimmte Aktionen beschränken. Gestatten Sie z.B. mit RBAC einem Mitarbeiter die Verwaltung virtueller Computer in einem Abonnement, während ein anderer im gleichen Abonnement SQL-Datenbanken verwalten kann.

## <a name="basics-of-access-management-in-azure"></a>Grundlagen zur Zugriffsverwaltung in Azure
Jedes Azure-Abonnement ist mit einem Azure Active Directory-Verzeichnis (AD) verknüpft. Benutzer, Gruppen und Anwendungen aus diesem Verzeichnis können Ressourcen im Azure-Abonnement verwalten. Weisen Sie diese Zugriffsrechte mit dem Azure-Portal, Azure-Befehlszeilentools und Azure-Verwaltungs-APIs zu.

Gewähren Sie Zugriff, indem Sie Benutzern, Gruppen und Anwendungen die jeweils geeignete RBAC-Rolle für einen bestimmten Bereich zuweisen. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. Mit einer Rolle, die einem übergeordneten Bereich zugewiesen ist, wird außerdem der Zugriff auf die darin enthaltenen untergeordneten Elemente gewährt. Ein Benutzer mit Zugriff auf eine Ressourcengruppe kann beispielsweise alle Ressourcen verwalten, die darin enthalten sind, z.B. Websites, virtuelle Computer und Subnetze.

![Beziehung zwischen Azure Active Directory-Elementen – Diagramm](./media/overview/rbac_aad.png)

Die von Ihnen zugewiesene RBAC-Rolle legt fest, welche Ressourcen der Benutzer, die Gruppe oder die Anwendung auf dieser Ebene verwalten kann.

## <a name="built-in-roles"></a>Integrierte Rollen
Azure RBAC verfügt über drei grundlegende Rollen, die für alle Ressourcentypen gelten:

* **Besitzer** verfügen über vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren.
* **Mitwirkende** können alle Arten von Azure-Ressourcen erstellen und verwalten, aber keinen anderen Personen Zugriff gewähren.
* **Leser** können vorhandene Azure-Ressourcen anzeigen.

Die verbleibenden RBAC-Rollen in Azure ermöglichen die Verwaltung von bestimmten Azure-Ressourcen. Mit der Rolle „Mitwirkender von virtuellen Computern“ können Benutzer beispielsweise virtuelle Computer erstellen und verwalten. Sie haben damit keinen Zugriff auf das virtuelle Netzwerk oder das Subnetz, mit dem der virtuelle Computer verbunden ist. 

[Integrierte RBAC-Rollen](built-in-roles.md) sind die Rollen aufgeführt, die in Azure verfügbar sind. Hierbei werden die Vorgänge und der Bereich angegeben, die mit jeder integrierten Rolle für Benutzer gewährt werden. Wenn Sie Ihre eigenen Rollen definieren möchten, um eine noch bessere Kontrolle zu haben, helfen Ihnen die Informationen zum Erstellen von [benutzerdefinierten Rollen in Azure RBAC](custom-roles.md)weiter.

## <a name="resource-hierarchy-and-access-inheritance"></a>Ressourcenhierarchie und Zugriffsvererbung
* Jedes **Abonnement** in Azure gehört jeweils nur zu einem Verzeichnis. (Aber jedes Verzeichnis kann über mehrere Abonnements verfügen.)
* Jede **Ressourcengruppe** gehört jeweils nur zu einem Abonnement.
* Jede **Ressource** gehört jeweils nur zu einer Ressourcengruppe.

Der Zugriff, den Sie auf übergeordneter Ebene gewähren, wird auf untergeordneter Ebene geerbt. Beispiel: 

* Sie weisen die Rolle „Leser“ einer Azure AD-Gruppe im Abonnementbereich zu. Die Mitglieder dieser Gruppe können alle Ressourcengruppen und Ressourcen im Abonnement anzeigen.
* Sie weisen die Rolle „Mitwirkender“ einer Anwendung im Ressourcengruppenbereich zu. Damit können Benutzer Ressourcen aller Typen in dieser Ressourcengruppe verwalten, aber keine anderen Ressourcengruppen des Abonnements.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC im Vergleich zu klassischen Administratoren für Abonnements
[Klassische Administratoren und Co-Admins für Abonnements](../billing/billing-add-change-azure-subscription-administrator.md) verfügen über Vollzugriff auf das Azure-Abonnement. Sie können Ressourcen über das [Azure-Portal](https://portal.azure.com), Azure Resource Manager-APIs und APIs des klassischen Azure-Bereitstellungsmodells verwalten. Im RBAC-Modell wird klassischen Administratoren die Besitzerrolle auf Abonnementebene zugewiesen.

Nur das Azure-Portal und die neuen Azure Resource Manager-APIs unterstützen Azure RBAC. Benutzer und Anwendungen, denen RBAC-Rollen zugewiesen sind, können die APIs des klassischen Azure-Bereitstellungsmodells nicht verwenden.

## <a name="authorization-for-management-vs-data-operations"></a>Autorisierung für Verwaltungsvorgänge im Vergleich zu Datenvorgängen
Azure RBAC unterstützt nur Verwaltungsvorgänge von Azure-Ressourcen im Azure-Portal und in den Azure Resource Manager-APIs. Azure RBAC kann nicht alle Vorgänge auf Datenebene für Azure-Ressourcen autorisieren. Sie können z. B. eine Person zum Verwalten von Speicherkonten autorisieren, jedoch nicht zum Verwalten von Blobs oder Tabellen innerhalb eines Speicherkontos. Ebenso kann eine SQL-Datenbank verwaltet werden, aber nicht die darin enthaltenen Tabellen.

## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte mit der [rollenbasierten Zugriffssteuerung im Azure-Portal](role-assignments-portal.md).
* Weitere Informationen finden Sie unter [Integrierte RBAC-Rollen](built-in-roles.md)
* Definieren Sie Ihre eigenen [benutzerdefinierten Rollen in Azure RBAC](custom-roles.md)
