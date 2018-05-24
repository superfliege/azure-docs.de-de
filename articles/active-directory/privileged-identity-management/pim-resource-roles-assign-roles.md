---
title: Zuweisen von Rollen für Azure-Ressourcen mit Privileged Identity Management | Microsoft Docs
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
ms.openlocfilehash: 501f063992d2f5c7769a5c9059b346aa2b5c2bb4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="assign-roles-for-azure-resources-by-using-privileged-identity-management"></a>Zuweisen von Rollen für Azure-Ressourcen mit Privileged Identity Management

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
