---
title: Starten einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) starten.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f4ebf9ec08915a557f897a743cac1160d7e6823
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173435"
---
# <a name="start-an-access-review-for-azure-resource-roles-in-pim"></a>Starten einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM
Rollenzuweisungen „veralten“, wenn Benutzer über privilegierten Zugriff verfügen, den sie nicht mehr benötigen. Um die Risiken im Zusammenhang mit diesen veralteten Rollenzuweisungen zu verringern, sollten Administratoren für privilegierte Rollen die Rollen regelmäßig überprüfen. In diesem Dokument werden die Schritte zum Starten einer Zugriffsüberprüfung in Privileged Identity Management (PIM) für Azure-Ressourcen beschrieben.

Wechseln Sie auf der Hauptseite der PIM-Anwendung zu:

* **Zugriffsüberprüfung** > **Hinzufügen**

![Hinzufügen von Zugriffsüberprüfungen](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Wenn Sie die Schaltfläche **Hinzufügen** auswählen, wird das Blatt **Zugriffsüberprüfung erstellen** angezeigt. Auf diesem Blatt konfigurieren Sie die Überprüfung mit einem Namen und einem Zeitlimit, wählen eine zu überprüfende Rolle aus und legen dann fest, wer die Überprüfung durchführen soll.

![Erstellen einer Zugriffsüberprüfung](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Konfigurieren der Überprüfung
Um eine Zugriffsüberprüfung zu erstellen, müssen Sie ihr zunächst einen Namen geben und dann ein Start- und Enddatum festlegen.

![Überprüfung konfigurieren – Screenshot](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Wählen Sie eine ausreichende Länge für die Überprüfung aus, damit Benutzer sie abschließen können. Wenn sie vor dem Enddatum fertig sind, können sie die Überprüfung frühzeitig beenden.

### <a name="choose-a-role-to-review"></a>Auswählen einer zu überprüfenden Rolle
Jede Überprüfung konzentriert sich nur auf eine Rolle. Wenn Sie die Zugriffsüberprüfung nicht über das Blatt einer bestimmten Rolle gestartet haben, müssen Sie nun eine Rolle auswählen.

1. Wechseln Sie zu **Rollenmitgliedschaft überprüfen**.
   
    ![Rollenmitgliedschaft überprüfen – Screenshot](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Wählen Sie eine Rolle aus der Liste aus.

### <a name="decide-who-will-perform-the-review"></a>Entscheiden, wer die Überprüfung durchführen soll
Bei der Durchführung einer Überprüfung gibt es drei Möglichkeiten: Sie können die Überprüfungsdurchführung einer anderen Person zuweisen, Sie können die Überprüfung selbst durchführen, oder jeder Benutzer kann seinen Zugriff selbst überprüfen.

1. Wählen Sie eine der Optionen aus:
   
   * **Ausgewählte Benutzer**: Verwenden Sie diese Option, wenn Sie nicht wissen, wer Zugriff benötigt. Mit dieser Option können Sie die Überprüfungsdurchführung einem Ressourcenbesitzer oder Gruppen-Manager zuweisen.
   * **Zugewiesen (selbst)**: Mit dieser Option können Benutzer ihre Rollenzuweisungen selbst überprüfen.
   
2. Wechseln Sie zu **Prüfer auswählen**.
   
    ![Prüfer auswählen – Screenshot](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>Starten der Überprüfung
Schließlich können Sie festlegen, dass Benutzer einen Grund für die Genehmigung des Zugriffs angeben müssen. Fügen Sie gegebenenfalls eine Beschreibung der Überprüfung hinzu. Wählen Sie dann **Starten** aus.

Informieren Sie die Benutzer über die anstehende Zugriffsüberprüfung, und zeigen Sie ihnen, [wie eine Zugriffsüberprüfung durchgeführt wird](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung
Im Dashboard mit den PIM-Azure-Ressourcen können Sie den Status der Überprüfungen durch die Prüfer nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen ist](pim-resource-roles-complete-access-review.md).

Bis der Überprüfungszeitraum beendet ist, können Sie die Benutzer daran erinnern, die Überprüfung abzuschließen, oder die Überprüfung vorzeitig im Abschnitt mit den Zugriffsüberprüfungen beenden.

## <a name="next-steps"></a>Nächste Schritte

- [Abschließen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-complete-access-review.md)
- [Ausführen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-perform-access-review.md)
- [Starten einer Zugriffsüberprüfung für Azure AD-Verzeichnisrollen in PIM](pim-how-to-start-security-review.md)
