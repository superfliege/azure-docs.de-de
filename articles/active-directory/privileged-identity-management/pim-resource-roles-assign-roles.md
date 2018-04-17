---
title: Privileged Identity Management für Azure-Ressourcen – Zuweisen von Rollen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie in PIM Rollen zuweisen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 9a9046afe2ee1e578333ff9d29f6fb21e95a0f22
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---assign"></a>Privileged Identity Management – Ressourcenrollen – Zuweisen

## <a name="assign-roles"></a>Zuweisen von Rollen

Wählen Sie die Rolle aus (falls Sie Rollen anzeigen), um einen Benutzer oder eine Gruppe einer Rolle zuzuweisen, 

![](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

oder klicken Sie in der Aktionsleiste auf „Hinzufügen“ (wenn Sie sich in der Ansicht „Mitglieder“ befinden).

![](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Gehen Sie wie folgt vor, wenn Sie einen Benutzer oder eine Gruppe über die Registerkarte „Mitglieder“ hinzufügen: 

1. Wählen Sie im Menü „Hinzufügen“ eine Rolle aus, damit Sie einen Benutzer oder eine Gruppe auswählen können.

![](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Wählen Sie einen Benutzer oder eine Gruppe aus dem Verzeichnis aus.

3. Wählen Sie im Dropdownmenü den entsprechenden Zuweisungstyp aus. 

    - **Just In Time Assignment** (Just-In-Time-Zuweisung): Gewährt dem Benutzer oder den Gruppenmitgliedern geeigneten, aber nicht beständigen Zugriff auf die Rolle – entweder für einen bestimmten Zeitraum oder unbegrenzt (sofern in den Rolleneinstellungen konfiguriert). 
    - **Direct Assignment** (Direkte Zuweisung): Bei dieser Option ist keine Aktivierung der Rollenzuweisung durch den Benutzer oder die Gruppenmitglieder erforderlich. (Wird auch als beständiger Zugriff bezeichnet.) Microsoft empfiehlt die Verwendung der direkten Zuweisung für den kurzfristigen Einsatz (etwa bei Bereitschaftsschichten) oder für zeitkritische Aktivitäten, bei denen der Zugriff nach Abschluss der Aufgabe nicht mehr benötigt wird.

Mithilfe des Kontrollkästchens unterhalb der Dropdownliste für den Zuweisungstyp können Sie angeben, ob die Zuweisung dauerhaft sein soll (dauerhaft zur Aktivierung der Just-In-Time Zuweisung berechtigt/dauerhaft aktiv für die direkte Zuweisung).

![](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

>[!NOTE]
>Das Kontrollkästchen kann unter Umständen nicht bearbeitet werden, wenn ein anderer Administrator in den Rolleneinstellungen die maximale Zuweisungsdauer für die einzelnen Zuweisungstypen angegeben hat.

 Wenn Sie einen bestimmten Zuweisungszeitraum angeben möchten, deaktivieren Sie dieses Kontrollkästchen, und ändern Sie nach Bedarf den Start- und/oder den Endzeitpunkt (Datum und Uhrzeit).

![](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Verwalten von Rollenzuweisungen

Administratoren können zum Verwalten von Rollenzuweisungen im linken Navigationsbereich entweder auf „Rollen“ oder auf „Mitglieder“ klicken. Unter „Rollen“ können Administratoren Verwaltungsaufgaben für eine bestimmte Rolle durchführen. Unter „Mitglieder“ werden dagegen alle Benutzer- und Gruppenrollenzuweisungen für die Ressource angezeigt.

![](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Ist eine Rolle mit ausstehender Aktivierung vorhanden, wird beim Anzeigen der Mitgliedschaft am oberen Seitenrand ein Benachrichtigungsbanner angezeigt.


## <a name="modify-existing-assignments"></a>Ändern vorhandener Zuweisungen

Wenn Sie vorhandene Zuweisungen über die Benutzer-/Gruppendetailansicht ändern möchten, klicken Sie auf der Aktionsleiste am oberen Seitenrand auf „Einstellungen ändern“. Ändern Sie den Zuweisungstyp in „Just In Time Assignment“ (Just-In-Time-Zuweisung) oder „Direct Assignment“ (Direkte Zuweisung).

![](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
