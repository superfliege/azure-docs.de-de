---
title: Privileged Identity Management für Azure-Ressourcen – Ermitteln und Verwalten von Azure-Ressourcen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Azure-Ressourcen schützen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 78650e47ec92aa144e4ccc8c57f309240bf31ee3
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="discover-and-manage-azure-resources"></a>Ermitteln und Verwalten von Azure-Ressourcen

Wenn in Ihrer Organisation Azure AD PIM bereits genutzt wird, um Administratoren in Ihrem Verzeichnis zu schützen, oder wenn Sie ein Abonnementbesitzer sind, der Produktionsressourcen schützen möchte, sind Sie hier richtig.

Beim erstmaligen Aktivieren von PIM für Azure-Ressourcen müssen Sie Ressourcen ermitteln und auswählen, die mit PIM geschützt werden sollen. Es gibt keine Beschränkung der Anzahl von Ressourcen, die Sie mit PIM verwalten können, aber wir raten Ihnen, mit Ihren wichtigsten Ressourcen (für die Produktion) zu beginnen.

> [!Note]
> Nur Abonnementressourcen können zu Verwaltungszwecken durchsucht und ausgewählt werden. Wenn Sie für ein Abonnement in PIM die Verwaltung eines Abonnements auswählen, wird die Verwaltung auch für alle untergeordneten Ressourcen aktiviert.

## <a name="discover-resources"></a>Ermitteln von Ressourcen

Navigieren Sie zu Azure AD PIM, und wählen Sie im linken Navigationsmenü im Abschnitt „Verwalten“ die Azure-Ressourcen aus.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Falls Sie PIM für Azure-Ressourcen zum ersten Mal verwenden, müssen Sie die Ermittlung für die zu verwaltenden Ressourcen durchführen.
Klicken Sie in der Mitte des Bildschirms auf die Schaltfläche „Discover resources“ (Ressourcen ermitteln), um die Oberfläche für die Ermittlung zu starten.

![](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Wenn eine Azure-Ressource bereits von einer anderen Ressource oder einem Verzeichnisadministrator in Ihrer Organisation mit PIM verwaltet wird oder wenn Sie über eine berechtigte Rollenzuweisung für eine Ressource verfügen, enthält die Listenansicht eine Meldung der folgenden Art: „Discover resources or activate an eligible role assignment to continue“ (Ermitteln Sie Ressourcen, oder aktivieren Sie eine berechtigte Rollenzuweisung, um fortzufahren). 

![](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Wenn Sie in der Aktionsleiste oder in der Mitte des Bildschirms die Schaltfläche zum Ermitteln von Ressourcen auswählen, wird eine Liste mit den Abonnements angezeigt, die für die Verwaltung verfügbar sind. Wenn an diesem Punkt hervorgehobene Abonnements angezeigt werden, ist dies ein Hinweis darauf, dass sie per PIM geschützt sind.

> [!Note]
> Wenn ein Abonnement für die Verwaltung festgelegt wurde, kann dies nicht rückgängig gemacht werden. So soll verhindert werden, dass ein anderer Ressourcenadministrator PIM-Einstellungen entfernt.

![](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

Zeigen Sie auf ein Abonnement, das Sie per PIM schützen möchten, und aktivieren Sie das Kontrollkästchen ganz links in der Zeile. Sie können mehrere Abonnements gleichzeitig auswählen.

![](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Wählen Sie zum Initiieren des Onboardingprozesses in der Leiste oben auf dem Bildschirm die Schaltfläche „Ressource verwalten“.

![](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Die ausgewählten Ressourcen werden jetzt über PIM verwaltet. Schließen Sie den Ermittlungsbildschirm, indem Sie das „X“ oben rechts auf der Seite verwenden. Klicken Sie anschließend in der Leiste am oberen Rand des Bildschirms zum Verwalten von Azure-Ressourcen auf die Schaltfläche „Aktualisieren“, um mit dem Verwalten von PIM-Einstellungen und Zuweisen von Mitgliedern zu beginnen.

![](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von Rolleneinstellungen](pim-resource-roles-configure-role-settings.md)

[Zuweisen von Rollen in PIM](pim-resource-roles-assign-roles.md)
