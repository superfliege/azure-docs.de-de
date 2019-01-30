---
title: Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Zugriff für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und dem Azure-Portal verwalten. Dazu gehören das Auflisten, Erteilen und Entfernen des Zugriffs.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c339556353967db26f022384f2cf877962dc6d83
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2019
ms.locfileid: "54412307"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals

Der Zugriff auf Ressourcen in Azure wird mithilfe der [rollenbasierten Zugriffssteuerung](overview.md) (Role-Based Access Control, RBAC) verwaltet. In diesem Artikel wird beschrieben, wie Sie den Zugriff für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten mit RBAC und dem Azure-Portal verwalten.

## <a name="open-access-control-iam"></a>Öffnen der Zugriffssteuerung (IAM)

Das Blatt **Zugriffssteuerung (IAM)**, auch bekannt als Identitäts- und Zugriffsverwaltung, erscheint im gesamten Portal. Um den Zugriff im Portal anzuzeigen oder zu verwalten, öffnen Sie in der Regel zuerst das Blatt „Zugriffssteuerung (IAM)“ für den Bereich, den Sie anzeigen oder ändern möchten.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**, und wählen Sie dann den Bereich oder die Ressource aus, die Sie anzeigen oder verwalten möchten. Sie können beispielsweise **Verwaltungsgruppen**, **Abonnements**, **Ressourcengruppen** oder eine Ressource auswählen.

1. Klicken Sie auf die spezifische Ressource, die Sie anzeigen oder verwalten möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

    Nachfolgend wird ein Beispiel für das Blatt „Zugriffssteuerung (IAM)“ für ein Abonnement gezeigt.

    ![Blatt „Zugriffssteuerung (IAM)“ für ein Abonnement](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>Anzeigen von Rollen und Berechtigungen

Eine Rollendefinition ist eine Auflistung von Berechtigungen, die Sie für Rollenzuweisungen verwenden. Azure enthält über 70 [integrierte Rollen](built-in-roles.md). Führen Sie diese Schritte aus, um die Rollen und Berechtigungen anzuzeigen, die auf Verwaltungs- und Datenebene ausgeführt werden können.

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen bestimmten Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie Rollen und Berechtigungen anzeigen möchten.

1. Klicken Sie auf die Registerkarte **Rollen**, um eine Liste aller integrierten und benutzerdefinierten Rollen anzuzeigen.

   Sie können die Anzahl von Benutzern und Gruppen anzeigen, die jeder Rolle für diesen Bereich zugewiesen sind.

   ![Rollenliste](./media/role-assignments-portal/roles-list.png)

1. Klicken Sie auf eine einzelne Rolle um zu sehen, wem diese Rolle zugewiesen ist, und um die Berechtigungen für die Rolle anzuzeigen.

   ![Rollenzuweisungen](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>Anzeigen von Rollenzuweisungen

Bei der Zugriffsverwaltung möchten Sie die zugriffsberechtigten Benutzer sowie deren Berechtigungen und Berechtigungsstufe ermitteln. Um den Zugriff für einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität aufzulisten, zeigen Sie die Rollenzuweisungen an.

### <a name="view-role-assignments-for-a-single-user"></a>Anzeigen von Rollenzuweisungen für einen einzelnen Benutzer

Führen Sie diese Schritte aus, um den Zugriff für einen einzelnen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität für einen bestimmten Bereich anzuzeigen.

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie den Zugriff anzeigen möchten.

1. Klicken Sie auf die Registerkarte **Zugriff überprüfen**.

    ![Zugriffssteuerung: Registerkarte „Zugriff überprüfen“](./media/role-assignments-portal/access-control-check-access.png)

1. Wählen Sie in der Liste **Finden** den Typ des Sicherheitsprinzipals aus, für den Sie den Zugriff überprüfen möchten.

1. Geben Sie in das Suchfeld eine Zeichenfolge ein, nach der das Verzeichnis durchsucht werden soll. Sie können nach Anzeigenamen, E-Mail-Adressen oder Objektbezeichnern suchen.

    ![Auswahlliste für Zugriffsüberprüfung](./media/role-assignments-portal/check-access-select.png)

1. Klicken Sie auf den Sicherheitsprinzipal, um den Bereich **Zuweisungen** zu öffnen.

    ![Bereich „Zuweisungen“](./media/role-assignments-portal/check-access-assignments.png)

    In diesem Bereich werden die Rollen angezeigt, die dem ausgewählten Sicherheitsprinzipal für den Bereich zugewiesen sind. Wenn Ablehnungszuweisungen für diesen Bereich oder an diesen Bereich vererbte Ablehnungszuweisungen vorhanden sind, werden diese aufgeführt.

### <a name="view-all-role-assignments-at-a-scope"></a>Anzeigen aller Rollenzuweisungen für einen Bereich

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie den Zugriff anzeigen möchten.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen** (oder klicken Sie in der Kachel „Rollenzuweisungen anzeigen“ auf die Schaltfläche **Anzeigen**), um alle Rollenzuweisungen für diesen Bereich anzuzeigen.

   ![Zugriffssteuerung: Registerkarte „Rollenzuweisungen“](./media/role-assignments-portal/access-control-role-assignments.png)

   Auf der Registerkarte „Rollenzuweisungen“ können Sie sehen, wer Zugriff auf diesen Bereich hat. Beachten Sie, dass einige Rollen auf **Diese Ressource** begrenzt sind, während andere von einem anderen Bereich **geerbt** werden. Der Zugriff wird entweder speziell dieser Ressource zugewiesen oder von einer Zuweisung des übergeordneten Bereichs geerbt.

## <a name="add-a-role-assignment"></a>Hinzufügen einer Rollenzuweisung

In RBAC weisen Sie zum Erteilen von einem Benutzer, einer Gruppe, einem Dienstprinzipal oder einer verwalteten Identität eine Rolle zu. Gehen Sie wie folgt vor, um Zugriff auf verschiedene Bereiche zu gewähren.

### <a name="assign-a-role-at-a-scope"></a>Zuweisen einer Rolle für einen Bereich

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie Zugriff erteilen möchten.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für diesen Bereich anzuzeigen.

1. Klicken Sie auf die Registerkarte **Rollenzuweisung hinzufügen**, um den Bereich „Rollenzuweisung hinzufügen“ zu öffnen.

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Bereich „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer**.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität aus. Wird der Sicherheitsprinzipal in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Begriff eingeben, um das Verzeichnis nach Anzeigenamen, E-Mail-Adressen und Objektbezeichner zu durchsuchen.

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Sicherheitsprinzipal die Rolle für den Bereich zugewiesen.

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Zuweisen eines Benutzers als Administrator eines Abonnements

Um einen Benutzer zum Administrator eines Azure-Abonnements zu machen, weisen Sie ihm die Rolle [Besitzer](built-in-roles.md#owner) im Abonnementbereich zu. Durch die Rolle „Besitzer“ erhält der Benutzer vollständigen Zugriff auf alle Ressourcen im Abonnement, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren. Diese Schritte sind identisch mit allen anderen Rollenzuweisungen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements**.

1. Klicken Sie auf das Abonnement, für das Sie Zugriff erteilen möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

1. Klicken Sie auf die Registerkarte **Rollenzuweisung hinzufügen**, um den Bereich „Rollenzuweisung hinzufügen“ zu öffnen.

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Bereich „Rollenzuweisung hinzufügen“](./media/role-assignments-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** die Option **Besitzer** aus.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer aus. Wird der Benutzer in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Suchbegriff eingeben, um das Verzeichnis nach Anzeigenamen und E-Mail-Adressen zu durchsuchen.

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Benutzer im Abonnementbereich die Rolle „Besitzer“ zugewiesen.

## <a name="remove-role-assignments"></a>Entfernen von Rollenzuweisungen

In RBAC entfernen Sie eine Rollenzuweisung, um den Zugriff zu entfernen. Führen Sie die folgenden Schritte aus, um den Zugriff zu entfernen.

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für einen Bereich, z.B. für eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine Ressource, für das bzw. die Sie den Zugriff entfernen möchten.

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

1. Aktivieren Sie in der Liste der Rollenzuweisungen den Sicherheitsprinzipal mit der zu entfernenden Rollenzuweisung.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Klicken Sie auf **Entfernen**.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment.png)

1. Klicken Sie in der angezeigten Meldung zum Entfernen der Rollenzuweisung auf **Ja**.

    Geerbte Rollenzuweisungen können nicht entfernt werden. Wenn Sie eine geerbte Rollenzuweisung entfernen möchten, müssen Sie dies für den Bereich durchführen, in dem die Rollenzuweisung erstellt wurde. Die Spalte **Bereich** neben **(Geerbt)** enthält einen Link, mit dem Sie zum Bereich gelangen, in dem diese Rolle zugewiesen wurde. Sie können die Rollenzuweisung entfernen, indem Sie zu dem dort aufgelisteten Bereich gehen.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Gewähren des Zugriffs für einen Benutzer mithilfe von RBAC und Azure-Portal](quickstart-assign-role-user-portal.md)
* [Tutorial: Gewähren des Zugriffs für einen Benutzer mithilfe von RBAC und Azure PowerShell](tutorial-role-assignments-user-powershell.md)
* [Beheben von RBAC-Fehlern in Azure](troubleshooting.md)
* [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../azure-resource-manager/management-groups-overview.md)
