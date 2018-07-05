---
title: Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Zugriff für Benutzer, Gruppen und Anwendungen mithilfe der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und des Azure-Portals verwalten. Dazu gehören das Auflisten, Erteilen und Entfernen des Zugriffs.
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
ms.date: 06/13/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 6544503353f085a9dd9b0fe7207bbf8d4e0bc9d0
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435616"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals

Der Zugriff auf Ressourcen in Azure wird mithilfe der [rollenbasierten Zugriffssteuerung](overview.md) (Role-Based Access Control, RBAC) verwaltet. In diesem Artikel wird beschrieben, wie Sie den Zugriff für Benutzer, Gruppen und Anwendungen mit der RBAC und dem Azure-Portal verwalten.

## <a name="list-roles"></a>Auflisten der Rollen

Eine Rollendefinition ist eine Auflistung von Berechtigungen, die Sie für Rollenzuweisungen verwenden. Azure enthält über 60 [integrierte Rollen](built-in-roles.md).

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements**.

1. Wählen Sie Ihr Abonnement aus.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

   ![Rollenoption](./media/role-assignments-portal/list-subscription-access-control.png)

1. Klicken Sie auf **Rollen**, um eine Liste aller integrierten und benutzerdefinierten Rollen anzuzeigen.

   ![Rollenoption](./media/role-assignments-portal/roles-option.png)

   Sie können die Anzahl von Benutzern und Gruppen anzeigen, die jeder Rolle zugewiesen sind.

   ![Rollenliste](./media/role-assignments-portal/roles-list.png)

## <a name="list-access"></a>Auflisten des Zugriffs

Bei der Zugriffsverwaltung möchten Sie die zugriffsberechtigten Benutzer sowie deren Berechtigungen und Berechtigungsstufe ermitteln. Zum Auflisten des Zugriffs führen Sie die Rollenzuweisungen auf.

### <a name="list-role-assignments-for-a-subscription"></a>Auflisten der Rollenzuweisungen für ein Abonnement

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements**.

1. Wählen Sie Ihr Abonnement aus.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

    Auf dem Blatt „Zugriffssteuerung (IAM)“ (auch als „Identitäts- und Zugriffsverwaltung“ bezeichnet) sehen Sie die Benutzer, die Zugriff auf das Abonnement haben, und ihre Rolle.

    ![Blatt „Zugriffssteuerung (IAM)“](./media/role-assignments-portal/subscription-access-control.png)

    Klassische Administratoren und Co-Admins für Abonnements werden im RBAC-Modell als Besitzer des Abonnements betrachtet.


### <a name="list-role-assignments-for-a-resource-group"></a>Auflisten von Rollenzuweisungen für eine Ressourcengruppe

1. Wählen Sie in der Navigationsliste die Option **Ressourcengruppen** aus.

1. Wählen Sie eine Ressourcengruppe aus, und klicken Sie dann auf **Zugriffssteuerung (IAM)**.

   Auf dem Blatt „Zugriffssteuerung (IAM)“ wird angezeigt, wer Zugriff auf diese Ressourcengruppe hat. Beachten Sie, dass einige Rollen auf **Diese Ressource** begrenzt sind, während andere von einem anderen Bereich **geerbt** werden. Der Zugriff wird entweder speziell der Ressourcengruppe zugewiesen oder von einer Zuweisung des übergeordneten Abonnements geerbt.

   ![Ressourcengruppen](./media/role-assignments-portal/resource-group-access-control.png)

### <a name="list-role-assignments-for-a-user"></a>Liste von Rollenzuweisungen für einen Benutzer

1. Klicken Sie in der Navigationsliste auf **Azure Active Directory**.

1. Klicken Sie auf **Benutzer**, um **Alle Benutzer** zu öffnen.

   ![Blatt „Alle Benutzer“ in Azure Active Directory](./media/role-assignments-portal/aad-all-users.png)

1. Wählen Sie einen einzelnen Benutzer in der Liste aus.

1. Klicken Sie im Abschnitt **Verwalten** auf **Azure-Ressourcen**.

   ![Azure Active Directory-Benutzer: Azure-Ressourcen](./media/role-assignments-portal/aad-user-azure-resources.png)

   Auf dem Blatt „Azure-Ressourcen“ sehen Sie die Rollenzuweisungen für den ausgewählten Benutzer. Diese Liste enthält nur die Rollenzuweisungen für Ressourcen, für die Sie über Leseberechtigungen verfügen. Wenn der Benutzer beispielsweise auch über Rollenzuweisungen in einem anderen Abonnement verfügt, für das Sie keine Leseberechtigung besitzen, werden diese Rollenzuweisungen nicht in der Liste angezeigt.

## <a name="grant-access"></a>Gewähren von Zugriff

In RBAC erstellen Sie zum Gewähren des Zugriffs eine Rollenzuweisung.

### <a name="create-a-role-assignment-at-a-subscription-scope"></a>Erstellen einer Rollenzuweisung im Abonnementbereich

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements**.

1. Wählen Sie Ihr Abonnement aus.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**, um die aktuelle Liste von Rollenzuweisungen im Abonnementbereich anzuzeigen.

   ![Blatt „Zugriffssteuerung (IAM)“ für Ressourcengruppe](./media/role-assignments-portal/grant-subscription-access-control.png)

1. Klicken Sie auf **Hinzufügen**, um den Bereich **Berechtigungen hinzufügen** zu öffnen.

   Falls Sie nicht über Berechtigungen zum Zuweisen von Rollen verfügen, wird die Option **Hinzufügen** nicht angezeigt.

   ![Bereich „Berechtigungen hinzufügen“](./media/role-assignments-portal/add-permissions.png)

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer**.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer, eine Gruppe oder eine Anwendung aus. Wird der Sicherheitsprinzipal in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Begriff eingeben, um das Verzeichnis nach Anzeigenamen, E-Mail-Adressen und Objektbezeichner zu durchsuchen.

1. Wählen Sie **Speichern** aus, um die Rollenzuweisung zu erstellen.

   Nach einigen Augenblicken wird der Sicherheitsprinzipal der Rolle im Abonnementbereich zugewiesen.

### <a name="create-a-role-assignment-at-a-resource-group-scope"></a>Erstellen einer Rollenzuweisung in einem Ressourcengruppenbereich

1. Wählen Sie in der Navigationsliste die Option **Ressourcengruppen** aus.

1. Wählen Sie eine Ressourcengruppe aus.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**, um die aktuelle Liste von Rollenzuweisungen im Ressourcengruppenbereich anzuzeigen.

   ![Blatt „Zugriffssteuerung (IAM)“ für Ressourcengruppe](./media/role-assignments-portal/grant-resource-group-access-control.png)

1. Klicken Sie auf **Hinzufügen**, um den Bereich **Berechtigungen hinzufügen** zu öffnen.

   Falls Sie nicht über Berechtigungen zum Zuweisen von Rollen verfügen, wird die Option **Hinzufügen** nicht angezeigt.

   ![Bereich „Berechtigungen hinzufügen“](./media/role-assignments-portal/add-permissions.png)

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer**.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer, eine Gruppe oder eine Anwendung aus. Wird der Sicherheitsprinzipal in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Begriff eingeben, um das Verzeichnis nach Anzeigenamen, E-Mail-Adressen und Objektbezeichner zu durchsuchen.

1. Wählen Sie **Speichern** aus, um die Rollenzuweisung zu erstellen.

   Nach einigen Augenblicken wird der Sicherheitsprinzipal der Rolle im Ressourcengruppenbereich zugewiesen.

## <a name="remove-access"></a>Zugriff entfernen

In RBAC entfernen Sie eine Rollenzuweisung, um den Zugriff zu entfernen.

### <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

1. Öffnen Sie das Blatt **Zugriffssteuerung (IAM)** für das Abonnement, die Ressourcengruppe oder die Ressource, das bzw. die die zu entfernende Rollenzuweisung enthält.

1. Aktivieren Sie in der Liste der Rollenzuweisungen den Sicherheitsprinzipal mit der zu entfernenden Rollenzuweisung.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Klicken Sie auf **Entfernen**.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/role-assignments-portal/remove-role-assignment.png)

1. Klicken Sie in der Nachricht zum Entfernen der Rollenzuweisung auf **Ja**.

Geerbte Rollenzuweisungen können nicht entfernt werden. Wenn Sie eine geerbte Rollenzuweisung entfernen möchten, müssen Sie dies für den Bereich durchführen, in dem die Rollenzuweisung erstellt wurde. Die Spalte **Bereich** neben **Geerbt** enthält einen Link, mit dem Sie zu den Ressourcen gelangen, denen diese Rolle zugewiesen wurde. Sie können die Rollenzuweisung entfernen, indem Sie zu dem dort aufgelisteten Bereich gehen.

## <a name="other-tools-to-manage-access"></a>Andere Tools zum Verwalten des Zugriffs

Sie können auch mit Azure RBAC-Befehlen in anderen Tools als dem Azure-Portal Rollen zuweisen und den Zugriff verwalten. Weitere Informationen finden Sie unter den folgenden Links:

* [Azure PowerShell](role-assignments-powershell.md)
* [Azure-CLI](role-assignments-cli.md)
* [REST-API](role-assignments-rest.md)

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Gewähren des Zugriffs für einen Benutzer mithilfe der RBAC und des Azure-Portals](quickstart-assign-role-user-portal.md)
* [Tutorial: Gewähren des Zugriffs für einen Benutzer mithilfe von RBAC und Azure PowerShell](tutorial-role-assignments-user-powershell.md)
* [Integrierte Rollen](built-in-roles.md)
