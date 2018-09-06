---
title: Anzeigen des Überwachungsverlaufs für Azure-Ressourcenrollen in PIM| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Überwachungsverlauf für Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) anzeigen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: c0536423e9640f78149b612ec66b0a07cdcf24bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189310"
---
# <a name="view-audit-history-for-azure-resource-roles-in-pim"></a>Anzeigen des Überwachungsverlaufs für Azure-Ressourcenrollen in PIM

Die Ressourcenüberwachung liefert einen Überblick über alle Rollenaktivitäten für die Ressource. Die Informationen können nach einem vordefinierten oder benutzerdefinierten Datumsbereich gefiltert werden.
![Filtern von Informationen](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Die Ressourcenüberwachung ermöglicht außerdem den schnellen Zugriff auf die Details zu Benutzeraktivitäten. Wählen Sie unter **Überwachungstyp** die Option **Aktivieren** aus. Wählen Sie **(Aktivität)** aus, um die Aktionen des jeweiligen Benutzers in den Azure-Ressourcen anzuzeigen.
![Aktivitätsdetails](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Weitere Aktivitätsdetails](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="my-audit"></a>Meine Überwachung

Unter „Meine Überwachung“ erhalten Sie eine Ansicht der persönlichen Rollenaktivität eines Benutzers. Die Informationen können nach einem vordefinierten oder benutzerdefinierten Datumsbereich gefiltert werden.
![Persönliche Rollenaktivität](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Anzeigen der Aktivierung und der Azure-Ressourcenaktivität

Falls Sie ermitteln möchten, welche Aktionen ein bestimmter Benutzer für verschiedene Ressourcen ausgeführt hat, können Sie die Azure-Ressourcenaktivität für einen bestimmten Aktivierungszeitraum prüfen. Wählen Sie in der Ansicht **Mitglieder** zunächst einen Benutzer aus der Liste mit den Mitgliedern in einer bestimmten Rolle aus. In der Ergebnisansicht werden die Aktionen des Benutzers in Azure-Ressourcen nach Datum grafisch dargestellt. Außerdem sind die letzten Rollenaktivierungen im selben Zeitraum zu sehen.

![Benutzerdetails](media/azure-pim-resource-rbac/rbac-user-details.png)

Wenn Sie eine bestimmte Rollenaktivierung auswählen, werden Details zu der Rollenaktivierung sowie die entsprechende Azure-Ressourcenaktivität angezeigt, die ausgeführt wurde, während der Benutzer aktiv war.

![Rollenaktivierung auswählen](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="next-steps"></a>Nächste Schritte

- [Anzeigen des Überwachungsverlaufs für Azure AD-Verzeichnisrollen in PIM](pim-how-to-use-audit-log.md)
