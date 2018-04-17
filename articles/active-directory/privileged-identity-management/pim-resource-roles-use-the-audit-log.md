---
title: Privileged Identity Management für Azure-Ressourcen – Ressourcenüberwachung | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie für eine bestimmte Ressource eine Ansicht mit allen Rollenaktivitäten anzeigen.
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
ms.openlocfilehash: b8fa7d5600c0de8a3319ea4de785281372959937
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---audit"></a>Privileged Identity Management – Ressourcenrollen – Überwachung

Die Ressourcenüberwachung liefert einen Überblick über alle Rollenaktivitäten für die Ressource. Die Informationen können nach einem vordefinierten oder benutzerdefinierten Datumsbereich gefiltert werden.
![](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Darüber hinaus können Sie über die Ressourcenüberwachung schnell auf die Aktivitätsdetails eines Benutzers zugreifen. Wählen Sie unter „Überwachungstyp“ die Option „Aktivieren“. Klicken Sie auf „(Aktivität)“, um die Aktionen des jeweiligen Benutzers in den Azure-Ressourcen anzuzeigen.
![](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Meine Überwachung

Unter „Meine Überwachung“ erhalten Sie eine Ansicht der persönlichen Rollenaktivität eines Benutzers. Die Informationen können nach einem vordefinierten oder benutzerdefinierten Datumsbereich gefiltert werden.
![](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Anzeigen der Aktivierung und der Azure-Ressourcenaktivität

Falls Sie ermitteln möchten, welche Aktionen ein bestimmter Benutzer für verschiedene Ressourcen ausgeführt hat, können Sie die Azure-Ressourcenaktivität für einen bestimmten Aktivierungszeitraum prüfen (für geeignete Benutzer). Wählen Sie in der Mitgliederansicht zunächst einen Benutzer aus der Liste mit den Mitgliedern in einer bestimmten Rolle aus. Daraufhin erscheint eine grafische Darstellung der Aktionen (nach Datum), die der Benutzer für Azure-Ressourcen ausgeführt hat, und der kürzlich erfolgten Rollenaktivierungen im gleichen Zeitraum.

![](media/azure-pim-resource-rbac/rbac-user-details.png)

Wenn Sie auf eine bestimmte Rollenaktivierung klicken, werden Details zu der Rollenaktivierung sowie die entsprechende Azure-Ressourcenaktivität angezeigt, die erfolgt ist, während der Benutzer aktiv war.

![](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

