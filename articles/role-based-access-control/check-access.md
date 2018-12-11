---
title: 'Schnellstart: Anzeigen der zugewiesenen Rollen von Benutzern mit dem Azure-Portal | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die Berechtigungen für die rollenbasierte Zugriffssteuerung (RBAC), die einem Benutzer, einer Gruppe, einem Dienstprinzipal oder einer verwalteten Identität zugewiesen sind, mit dem Azure-Portal anzeigen.
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
ms.openlocfilehash: b755dd6223c21084cafea82a1c8857f9f54b03b5
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52641866"
---
# <a name="quickstart-view-roles-assigned-to-a-user-using-the-azure-portal"></a>Schnellstart: Anzeigen der zugewiesenen Rollen von Benutzern mit dem Azure-Portal

Sie können das Blatt **Zugriffssteuerung (IAM)** in [Rollenbasierte Zugriffssteuerung (RBAC)](overview.md) verwenden, um die Rollenzuweisungen für mehrere Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten anzuzeigen. In einigen Fällen kann es aber sein, dass Sie nur schnell Rollenzuweisungen für einzelne Benutzer, Gruppen, Dienstprinzipale oder verwaltete Identitäten anzeigen möchten. Die einfachste Möglichkeit ist hierbei die Verwendung der Funktion **Zugriff überprüfen** im Azure-Portal.

## <a name="view-role-assignments"></a>Anzeigen von Rollenzuweisungen

Führen Sie diese Schritte aus, um die Rollenzuweisungen für einzelne Benutzer, Gruppen, Dienstprinzipale oder verwaltete Identitäten auf Abonnementebene anzuzeigen.

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
