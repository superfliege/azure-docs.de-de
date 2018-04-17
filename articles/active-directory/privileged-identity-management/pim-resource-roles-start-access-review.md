---
title: Starten einer Zugriffsüberprüfung in PIM für Azure-Ressourcen| Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Zugriffsüberprüfung in Privileged Identity Management für Azure-Ressourcen starten.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 61ed4e82e0b782b423668564dae6efb272967702
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---start-access-review"></a>Privileged Identity Management – Ressourcenrolle – Starten der Zugriffsüberprüfung
Rollenzuweisungen „veralten“, wenn Benutzer über privilegierten Zugriff verfügen, den sie nicht mehr benötigen. Um die Risiken im Zusammenhang mit diesen „veralteten“ Rollenzuweisungen zu verringern, sollten Administratoren für privilegierte Rollen die den Benutzern zugewiesenen Rollen regelmäßig überprüfen. In diesem Dokument werden die Schritte zum Starten einer Zugriffsüberprüfung in Privileged Identity Management (PIM) für Azure-Ressourcen beschrieben.

Navigieren Sie auf der Hauptseite der PIM-Anwendung zu:

* **Zugriffsüberprüfung** > **Hinzufügen**

![](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Wenn Sie auf die Schaltfläche **Hinzufügen** klicken, wird das Blatt **Zugriffsüberprüfung erstellen** angezeigt. Auf diesem Blatt konfigurieren Sie die Überprüfung mit einem Namen und einem Zeitlimit, wählen eine zu überprüfende Rolle aus und entscheiden, wer die Überprüfung durchführen soll.

![](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Konfigurieren der Überprüfung
Um eine Zugriffsüberprüfung zu erstellen, müssen Sie ihr einen Namen geben und ein Start- und Enddatum festlegen.

![Überprüfung konfigurieren – Screenshot](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Wählen Sie eine ausreichende Länge für die Überprüfung aus, damit Benutzer sie abschließen können. Wenn Sie vor dem Enddatum fertig sind, können Sie die Überprüfung frühzeitig beenden.

### <a name="choose-a-role-to-review"></a>Auswählen einer zu überprüfenden Rolle
Jede Überprüfung konzentriert sich nur auf eine Rolle. Wenn Sie die Zugriffsüberprüfung nicht über das Blatt einer bestimmten Rolle gestartet haben, müssen Sie eine Rolle auswählen.

1. Navigieren Sie zu **Rollenmitgliedschaft überprüfen**
   
    ![Rollenmitgliedschaft überprüfen – Screenshot](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Wählen Sie eine Rolle aus der Liste aus.

### <a name="decide-who-will-perform-the-review"></a>Entscheiden, wer die Überprüfung durchführen soll
Bei der Durchführung einer Überprüfung gibt es drei Möglichkeiten: Sie können die Überprüfungsdurchführung einer anderen Person zuweisen, Sie können die Überprüfung selbst durchführen, oder Sie können festlegen, dass jeder Benutzer seinen Zugriff selbst überprüfen soll.

1. Wählen Sie eine der Optionen aus:
   
   * **Ausgewählte Benutzer**: Verwenden Sie diese Option, wenn Sie nicht wissen, wer Zugriff benötigt. Mit dieser Option können Sie die Überprüfungsdurchführung einem Ressourcenbesitzer oder Gruppen-Manager zuweisen.
   * **Zugewiesen (selbst)**: Mit dieser Option können Benutzer ihre Rollenzuweisungen selbst überprüfen.
   
2. Navigieren Sie zu **Prüfer auswählen**
   
    ![Prüfer auswählen – Screenshot](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>Starten der Überprüfung
Sie haben die Option, von den Benutzern die Angabe eines Grunds anzufordern, wenn sie ihren Zugriff genehmigen. Fügen Sie ggf. eine Beschreibung der Überprüfung hinzu, und wählen Sie **Starten** aus.

Informieren Sie die Benutzer über die anstehende Zugriffsüberprüfung, und zeigen Sie ihnen, [wie eine Zugriffsüberprüfung durchgeführt wird](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung
Sie können den Status der Überprüfungen durch die Prüfer im Dashboard mit den PIM-Azure-Ressourcen im Abschnitt mit den Zugriffsüberprüfungen nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](pim-resource-roles-complete-access-review.md)ist.

Bis der Überprüfungszeitraum beendet ist, können Sie die Benutzer daran erinnern, die Überprüfung abzuschließen, oder die Überprüfung vorzeitig im Abschnitt mit den Zugriffsüberprüfungen beenden.

