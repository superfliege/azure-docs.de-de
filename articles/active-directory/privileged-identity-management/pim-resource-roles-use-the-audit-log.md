---
title: Überwachen von Rollen für Azure-Ressourcen mit Privileged Identity Management | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie für eine bestimmte Ressource eine Ansicht mit allen Rollenaktivitäten anzeigen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 211b8c69a1462f7efdcb4002269d96d1d5cf2ae6
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233783"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Überwachen von Rollen für Azure-Ressourcen mit Privileged Identity Management 

Die Ressourcenüberwachung liefert einen Überblick über alle Rollenaktivitäten für die Ressource. Die Informationen können nach einem vordefinierten oder benutzerdefinierten Datumsbereich gefiltert werden.
![Filtern von Informationen](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Die Ressourcenüberwachung ermöglicht außerdem den schnellen Zugriff auf die Details zu Benutzeraktivitäten. Wählen Sie unter **Überwachungstyp** die Option **Aktivieren** aus. Wählen Sie **(Aktivität)** aus, um die Aktionen des jeweiligen Benutzers in den Azure-Ressourcen anzuzeigen.
![Aktivitätsdetails](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Weitere Aktivitätsdetails](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Meine Überwachung

Unter „Meine Überwachung“ erhalten Sie eine Ansicht der persönlichen Rollenaktivität eines Benutzers. Die Informationen können nach einem vordefinierten oder benutzerdefinierten Datumsbereich gefiltert werden.
![Persönliche Rollenaktivität](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Anzeigen der Aktivierung und der Azure-Ressourcenaktivität

Falls Sie ermitteln möchten, welche Aktionen ein bestimmter Benutzer für verschiedene Ressourcen ausgeführt hat, können Sie die Azure-Ressourcenaktivität für einen bestimmten Aktivierungszeitraum prüfen. Wählen Sie in der Ansicht **Mitglieder** zunächst einen Benutzer aus der Liste mit den Mitgliedern in einer bestimmten Rolle aus. In der Ergebnisansicht werden die Aktionen des Benutzers in Azure-Ressourcen nach Datum grafisch dargestellt. Außerdem sind die letzten Rollenaktivierungen im selben Zeitraum zu sehen.

![Benutzerdetails](media/azure-pim-resource-rbac/rbac-user-details.png)

Wenn Sie eine bestimmte Rollenaktivierung auswählen, werden Details zu der Rollenaktivierung sowie die entsprechende Azure-Ressourcenaktivität angezeigt, die ausgeführt wurde, während der Benutzer aktiv war.

![Rollenaktivierung auswählen](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

