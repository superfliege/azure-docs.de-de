---
title: Ermitteln von Azure-Ressourcen zur Verwaltung in PIM | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Ressourcen zur Verwaltung in Azure AD Privileged Identity Management (PIM) ermitteln.
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
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: b5d48b3f854afaa79574e0ec13cff91f60396ac6
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190657"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Ermitteln von Azure-Ressourcen zur Verwaltung in PIM

Erfahren Sie, wie Sie Azure-Ressourcen ermitteln und verwalten, wenn Sie Privileged Identity Management (PIM) in Azure Active Directory (Azure AD) verwenden. Diese Informationen können hilfreich sein für Organisationen, die PIM bereits zum Schutz von Administratorressourcen verwenden, und für Abonnementbesitzer, die Produktionsressourcen schützen möchten.

Beim erstmaligen Einrichten von PIM für Azure-Ressourcen müssen Sie Ressourcen ermitteln und auswählen, die mit PIM geschützt werden sollen. Die Anzahl der Ressourcen, die Sie mit PIM verwalten können, ist nicht beschränkt. Es empfiehlt sich aber, mit Ihren wichtigsten Ressourcen (für die Produktion) zu beginnen.

> [!NOTE]
> Sie können nur Verwaltungsgruppen oder Abonnementressourcen suchen und auswählen, um sie mithilfe von PIM zu verwalten. Wenn Sie eine Verwaltungsgruppe oder ein Abonnement in PIM verwalten, können Sie auch deren untergeordnete Ressourcen verwalten.

## <a name="discover-resources"></a>Ermitteln von Ressourcen

Navigieren Sie im Azure-Portal zum Bereich **Privileged Identity Management**. Wählen Sie im linken Menü im Abschnitt **VERWALTEN** die Option **Azure-Ressourcen** aus.

![Bereich „Privileged Identity Management - Azure-Ressourcen“](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Wenn Sie PIM für Azure-Ressourcen zum ersten Mal verwenden, führen Sie zunächst die Ermittlung für die zu verwaltenden Ressourcen durch. Wählen Sie im Bereich **Ressourcen ermitteln** die Schaltfläche **Ressourcen ermitteln** aus, um die Oberfläche für die Ermittlung zu starten.

![Bereich „Ressourcen ermitteln“](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Wenn eine Azure-Ressource bereits von einem anderen Ressourcen- oder Verzeichnisadministrator in Ihrer Organisation mit PIM verwaltet wird oder wenn Sie über eine berechtigte Rollenzuweisung für eine Ressource verfügen, wird in der Listenansicht die Meldung **Discover resources or activate an eligible role assignment to continue** (Ermitteln Sie Ressourcen, oder aktivieren Sie eine berechtigte Rollenzuweisung, um fortzufahren) angezeigt. 

![Schaltfläche „Ressourcen ermitteln“ im Bereich „Privileged Identity Management - Azure-Ressourcen“](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Wenn Sie die Schaltfläche **Ressourcen ermitteln** im oberen Menü oder in der Mitte des Bereichs auswählen, wird eine Liste mit den Abonnements angezeigt, die Sie verwalten können. Abonnements, die hervorgehoben sind, sind bereits durch PIM geschützt.

> [!NOTE]
> Nachdem ein Abonnement für die Verwaltung festgelegt wurde, kann dies nicht rückgängig gemacht werden. So soll verhindert werden, dass ein anderer Ressourcenadministrator PIM-Einstellungen entfernt.

![Bereich „Azure-Ressourcen - Ermittlung“](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

Zeigen Sie in der Spalte **RESSOURCE** mit der Maus auf ein Abonnement, das Sie mit PIM schützen möchten. Aktivieren Sie dann das Kontrollkästchen links neben dem Namen der Ressource. Sie können mehrere Abonnements gleichzeitig auswählen.

![Liste der Ressourcen im Bereich „Azure-Ressourcen – Ermittlung“](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Wählen Sie zum Initiieren des Onboardingprozesses im oberen Menü die Schaltfläche **Ressource verwalten** aus.

![Schaltfläche „Ressource verwalten“ im Bereich „Azure-Ressourcen – Ermittlung“](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Die ausgewählten Ressourcen werden jetzt über PIM verwaltet. Um das Fenster für die Ermittlung zu schließen, wählen Sie oben rechts das **X** aus. Zum Verwalten der PIM-Einstellungen und Zuweisen von Mitgliedern wählen Sie im oberen Menü des Bereichs **Privileged Identity Management – Azure-Ressourcen** die Schaltfläche **Aktualisieren** aus.

![Schaltfläche „Aktualisieren“ im oberen Menü des Bereichs „Privileged Identity Management – Azure-Ressourcen“](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-configure-role-settings.md)
- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
