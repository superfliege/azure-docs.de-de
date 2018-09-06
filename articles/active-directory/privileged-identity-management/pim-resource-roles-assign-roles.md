---
title: Zuweisen von Azure-Ressourcenrollen in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) zuweisen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7019a6f97a9590d3b652584015f3077f4ed075af
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188919"
---
# <a name="assign-azure-resource-roles-in-pim"></a>Zuweisen von Azure-Ressourcenrollen in PIM

Azure AD PIM kann sowohl die integrierten Azure-Ressourcenrollen als auch benutzerdefinierte Rollen verwalten. Beispiele:

- Owner (Besitzer)
- Benutzerzugriffsadministrator
- Mitwirkender
- Sicherheitsadministrator
- Sicherheits-Manager

>[!NOTE]
Benutzer oder Mitglieder von Gruppen, die der Rolle „Besitzer“ oder „Benutzerzugriffsadministrator“ zugewiesen sind, sowie globale Administratoren, die die Abonnementverwaltung in Azure AD ermöglichen, sind Ressourcenadministratoren. Diese Administratoren können Rollen zuweisen, Rolleneinstellungen konfigurieren und den Zugriff mithilfe von PIM für Azure-Ressourcen prüfen. Eine Liste mit den integrierten Rollen für Azure-Ressourcen finden Sie [hier](../../role-based-access-control/built-in-roles.md).

## <a name="assign-roles"></a>Zuweisen von Rollen

Um einen Benutzer oder eine Gruppe einer Rolle zuzuweisen, wenn Sie den Bereich **Rollen** anzeigen, wählen Sie die Rolle aus, und wählen Sie dann **Benutzer hinzufügen** aus. 

![Bereich „Rollen“ mit der Schaltfläche „Benutzer hinzufügen“](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

Sie können auch **Benutzer hinzufügen** aus dem Bereich **Mitglieder** auswählen.

![Bereich „Mitglieder“ mit der Schaltfläche „Benutzer hinzufügen“](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Gehen Sie wie folgt vor, wenn Sie einen Benutzer oder eine Gruppe über den Bereich **Mitglieder** hinzufügen: 

1. Wählen Sie im Bereich **Rolle auswählen** eine Rolle aus, damit Sie anschließend einen Benutzer oder eine Gruppe auswählen können.

   ![Bereich „Rolle auswählen“](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Wählen Sie einen Benutzer oder eine Gruppe aus dem Verzeichnis aus.

3. Wählen Sie im Dropdownmenü den entsprechenden Zuweisungstyp aus: 

   - **Just In Time** (Just-In-Time-Zuweisung): Gewährt dem Benutzer oder den Gruppenmitgliedern geeigneten, aber nicht beständigen Zugriff auf die Rolle, der für einen bestimmten Zeitraum oder unbegrenzt gilt (sofern in den Rolleneinstellungen konfiguriert). 
   - **Direct** (Direkte Zuweisung): Bei dieser Option ist keine Aktivierung der Rollenzuweisung durch den Benutzer oder die Gruppenmitglieder erforderlich (wird auch als beständiger Zugriff bezeichnet). Es wird empfohlen, direkte Zuweisung für eine kurzfristige Verwendung auszuwählen, bei der Zugriff nach dem Abschluss der Aufgabe nicht mehr erforderlich ist. Beispiele sind Bereitschaftsdienste und zeitkritische Aktivitäten.

4. Wenn die Zuweisung dauerhaft sein soll (dauerhaft zur Aktivierung der Just-In-Time Zuweisung berechtigt oder dauerhaft aktiv für die direkte Zuweisung), aktivieren Sie das Kontrollkästchen unterhalb des Felds **Zuweisungstyp**.

   ![Der Bereich „Mitgliedschaftseeinstellungen“ mit dem Feld „Zuweisungstyp“ und dem zugehörigen Kontrollkästchen](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

   >[!NOTE]
   >Das Kontrollkästchen kann unter Umständen nicht bearbeitet werden, wenn ein anderer Administrator in den Rolleneinstellungen die maximale Zuweisungsdauer für die einzelnen Zuweisungstypen angegeben hat.

   Wenn Sie einen bestimmten Zuweisungszeitraum angeben möchten, deaktivieren Sie dieses Kontrollkästchen, und ändern Sie nach Bedarf die Felder für den Start- und/oder den Endzeitpunkt (Datum und Uhrzeit).

   ![Bereich „Mitgliedschaftseinstellungen“ mit Feldern für das Startdatum, die Startzeit, das Enddatum und die Endzeit](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Verwalten von Rollenzuweisungen

Administratoren können zum Verwalten von Rollenzuweisungen im linken Bereich **Rollen** oder **Mitglieder** auswählen. Durch das Auswählen von **Rollen** können Administratoren den Rahmen ihrer Verwaltungsaufgaben auf eine bestimmte Rolle festlegen. Wenn Sie **Mitglieder** auswählen, werden alle Benutzer- und Gruppenrollenzuweisungen für die Ressource angezeigt.

![Bereich „Rollen“](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![Bereich „Mitglieder“](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Ist eine Rolle mit ausstehender Aktivierung vorhanden, wird beim Anzeigen der Mitgliedschaft am oberen Rand des Bereichs ein Benachrichtigungsbanner angezeigt.


## <a name="modify-existing-assignments"></a>Ändern vorhandener Zuweisungen

Wenn Sie vorhandene Zuweisungen über die Benutzer-/Gruppendetailansicht ändern möchten, klicken Sie auf der Aktionsleiste auf **Einstellungen ändern**. Ändern Sie den Zuweisungstyp in **Just In Time** (Just-In-Time-Zuweisung) oder **Direct** (Direkte Zuweisung).

![Bereich „Benutzerdetails“ mit der Schaltfläche „Einstellungen ändern“](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-configure-role-settings.md)
- [Zuweisen von Azure AD-Verzeichnisrollen in PIM](pim-how-to-add-role-to-user.md)
