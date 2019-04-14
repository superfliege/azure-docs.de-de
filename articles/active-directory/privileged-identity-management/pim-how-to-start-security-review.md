---
title: Starten einer Zugriffsüberprüfung für Azure AD-Rollen in PIM – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) starten.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5cbf96c165d79c26985663ef5a9d64bbf8f9892
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574993"
---
# <a name="start-an-access-review-for-azure-ad-roles-in-pim"></a>Starten einer Zugriffsüberprüfung für Azure AD-Rollen in PIM
Rollenzuweisungen „veralten“, wenn Benutzer über privilegierten Zugriff verfügen, den sie nicht mehr benötigen. Um die Risiken im Zusammenhang mit diesen veralteten Rollenzuweisungen zu verringern, sollten Administratoren für privilegierte Rollen oder globale Administratoren regelmäßig Zugriffsüberprüfung erstellen, um Administratoren zur Überprüfung der Rollen aufzufordern, die den Benutzern zugewiesen wurden. In diesem Dokument werden die Schritte zum Starten einer Zugriffsüberprüfung in Azure Active Directory (Azure AD) Privileged Identity Management (PIM) beschrieben.

## <a name="start-an-access-review"></a>Starten einer Zugriffsüberprüfung
> [!NOTE]
> Wenn Sie die PIM-Anwendung noch nicht zu Ihrem Dashboard im Azure-Portal hinzugefügt haben, finden Sie die notwendigen Schritte unter [Erste Schritte mit Azure Privileged Identity Management](pim-getting-started.md)
> 
> 

Eine Zugriffsüberprüfung kann auf der Hauptseite der PIM-Anwendung auf drei Arten gestartet werden:

* **Zugriffsüberprüfung** > **Hinzufügen**
* Die Schaltflächen **Rollen** > **Überprüfen**
* Wählen Sie in der Rollenliste die zu überprüfende Rolle aus > Schaltfläche **Überprüfen**.

Wenn Sie auf die Schaltfläche **Überprüfen** klicken, wird das Blatt **Zugriffsüberprüfung starten** angezeigt. Auf diesem Blatt konfigurieren Sie die Überprüfung mit einem Namen und einem Zeitlimit, wählen eine zu überprüfende Rolle aus und entscheiden, wer die Überprüfung durchführen soll.

![Zugriffsüberprüfung starten – Screenshot](./media/pim-how-to-start-security-review/PIM_start_review.png)

### <a name="configure-the-review"></a>Konfigurieren der Überprüfung
Um eine Zugriffsüberprüfung zu erstellen, müssen Sie ihr einen Namen geben und ein Start- und Enddatum festlegen.

![Überprüfung konfigurieren – Screenshot](./media/pim-how-to-start-security-review/PIM_review_configure.png)

Wählen Sie eine ausreichende Länge für die Überprüfung aus, damit Benutzer sie abschließen können. Wenn Sie vor dem Enddatum fertig sind, können Sie die Überprüfung frühzeitig beenden.

### <a name="choose-a-role-to-review"></a>Auswählen einer zu überprüfenden Rolle
Jede Überprüfung konzentriert sich nur auf eine Rolle. Wenn Sie die Zugriffsüberprüfung nicht über das Blatt einer bestimmten Rolle gestartet haben, müssen Sie eine Rolle auswählen.

1. Navigieren Sie zu **Rollenmitgliedschaft überprüfen**
   
    ![Rollenmitgliedschaft überprüfen – Screenshot](./media/pim-how-to-start-security-review/PIM_review_role.png)
2. Wählen Sie eine Rolle aus der Liste aus.

### <a name="decide-who-will-perform-the-review"></a>Entscheiden, wer die Überprüfung durchführen soll
Bei der Durchführung einer Überprüfung gibt es drei Möglichkeiten: Sie können die Überprüfungsdurchführung einer anderen Person zuweisen, Sie können die Überprüfung selbst durchführen, oder Sie können festlegen, dass jeder Benutzer seinen Zugriff selbst überprüfen soll.

1. Navigieren Sie zu **Prüfer auswählen**
   
    ![Prüfer auswählen – Screenshot](./media/pim-how-to-start-security-review/PIM_review_reviewers.png)
2. Wählen Sie eine der Optionen aus:
   
   * **Prüfer auswählen**: Verwenden Sie diese Option, wenn Sie nicht wissen, wer Zugriff benötigt. Mit dieser Option können Sie die Überprüfungsdurchführung einem Ressourcenbesitzer oder Gruppen-Manager zuweisen.
   * **Ich**: Diese Option ist hilfreich, wenn Sie sich vorab mit der Funktionsweise von Zugriffsüberprüfungen auseinandersetzen oder eine Überprüfung für Personen durchführen möchten, die nicht dazu in der Lage sind.
   * **Selbstüberprüfung durch Mitglieder**: Mit dieser Option können Benutzer ihre Rollenzuweisungen selbst überprüfen.

### <a name="start-the-review"></a>Starten der Überprüfung
Sie haben die Option, von den Benutzern die Angabe eines Grunds anzufordern, wenn sie ihren Zugriff genehmigen. Fügen Sie ggf. eine Beschreibung der Überprüfung hinzu, und wählen Sie **Starten** aus.

Informieren Sie die Benutzer über die anstehende Zugriffsüberprüfung, und zeigen Sie ihnen, [wie eine Zugriffsüberprüfung durchgeführt wird](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung
Sie können den Fortschritt der Überprüfungen durch die Prüfer im Azure AD PIM-Dashboard im Abschnitt mit den Zugriffsüberprüfungen nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](pim-how-to-complete-review.md)ist.

Bis der Überprüfungszeitraum beendet ist, können Sie die Benutzer daran erinnern, die Überprüfung abzuschließen, oder die Überprüfung vorzeitig im Abschnitt mit den Zugriffsüberprüfungen beenden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

- [Abschließen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-complete-review.md)
- [Durchführen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-perform-security-review.md)
- [Starten einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-start-access-review.md)
