---
title: 'Schnellstart: Anzeigen, welchen Zugriff ein Benutzer auf Azure-Ressourcen hat | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mithilfe der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und des Azure-Portals anzeigen, welchen Zugriff ein Benutzer oder ein anderer Sicherheitsprinzipal auf Azure-Ressourcen hat.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: f388215b2829066906ee7faf41abb17307bf3fff
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2019
ms.locfileid: "56337944"
---
# <a name="quickstart-view-the-access-a-user-has-to-azure-resources"></a>Schnellstart: Anzeigen, welchen Zugriff ein Benutzer auf Azure-Ressourcen hat

Sie können das Blatt **Zugriffssteuerung (IAM)** der [rollenbasierten Zugriffssteuerung (RBAC)](overview.md) verwenden, um anzuzeigen, welchen Zugriff ein Benutzer oder ein anderer Sicherheitsprinzipal auf Azure-Ressourcen hat. Manchmal möchten Sie jedoch unter Umständen nur schnell den Zugriff für einen einzelnen Benutzer oder einen anderen Sicherheitsprinzipal anzeigen. Die einfachste Möglichkeit ist hierbei die Verwendung der Funktion **Zugriff überprüfen** im Azure-Portal.

## <a name="view-role-assignments"></a>Anzeigen von Rollenzuweisungen

 Um den Zugriff für einen Benutzer anzuzeigen, müssen Sie dessen Rollenzuweisungen auflisten. Führen Sie diese Schritte aus, um die Rollenzuweisungen für einzelne Benutzer, Gruppen, Dienstprinzipale oder verwaltete Identitäten auf Abonnementebene anzuzeigen.

1. Klicken Sie im Azure-Portal auf **Alle Dienste** und anschließend auf **Abonnements**.

1. Klicken Sie auf Ihr Abonnement.

1. Klicken Sie auf **Zugriffssteuerung (IAM)**.

1. Klicken Sie auf die Registerkarte **Zugriff überprüfen**.

    ![Zugriffssteuerung: Registerkarte „Zugriff überprüfen“](./media/check-access/access-control-check-access.png)

1. Wählen Sie in der Liste **Finden** den Typ des Sicherheitsprinzipals aus, für den Sie den Zugriff überprüfen möchten.

1. Geben Sie in das Suchfeld eine Zeichenfolge ein, nach der das Verzeichnis durchsucht werden soll. Sie können nach Anzeigenamen, E-Mail-Adressen oder Objektbezeichnern suchen.

    ![Auswahlliste für Zugriffsüberprüfung](./media/check-access/check-access-select.png)

1. Klicken Sie auf den Sicherheitsprinzipal, um den Bereich **Zuweisungen** zu öffnen.

    ![Bereich „Zuweisungen“](./media/check-access/check-access-assignments.png)

    In diesem Bereich werden die Rollen angezeigt, die dem ausgewählten Sicherheitsprinzipal für den Bereich zugewiesen sind. Wenn Ablehnungszuweisungen für diesen Bereich oder an diesen Bereich vererbte Ablehnungszuweisungen vorhanden sind, werden diese angezeigt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Gewähren des Zugriffs für einen Benutzer mithilfe von RBAC und Azure-Portal](quickstart-assign-role-user-portal.md)
