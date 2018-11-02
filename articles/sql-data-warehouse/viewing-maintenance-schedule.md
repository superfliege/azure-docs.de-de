---
title: Azure-Wartungszeitpläne (Vorschau) | Microsoft-Dokumentation
description: Wartungszeitpläne ermöglichen es Kunden, die notwendigen geplanten Wartungsereignisse in ihre Planungen einzubeziehen, mit denen der Azure SQL Data Warehouse-Dienst neue Features, Upgrades und Patches einführt.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/06/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: db5502b796fc345b2e2bf083f864d80bd59e7293
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427126"
---
# <a name="view-a-maintenance-schedule"></a>Anzeigen eines Wartungszeitplans 

## <a name="portal"></a>Portal

Alle neu erstellten Azure SQL Data Warehouse-Instanzen verfügen über ein achtstündiges primäres und sekundäres Wartungsfenster, das während der Bereitstellung angewendet wird. Sie können die Fenster nach Abschluss der Bereitstellung ändern. Außerhalb der angegebenen Wartungsfenster werden ohne vorherige Benachrichtigung keine Wartungen durchgeführt.

Führen Sie die folgenden Schritte aus, um den Wartungszeitplan anzuzeigen, der auf Ihr Data Warehouse angewendet wurde:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2.  Wählen Sie das Data Warehouse aus, das Sie anzeigen möchten. 
3.  Das ausgewählte Data Warehouse wird auf dem Blatt „Übersicht“ geöffnet. Der Wartungszeitplan, der auf das Data Warehouse angewendet wurde, wird unter **Wartungszeitplan (Vorschau)** angezeigt.

![Blatt „Übersicht“](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) zum Erstellen, Anzeigen und Verwalten von Warnungen mit Azure Monitor
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) zu Webhookaktionen für Protokollwarnungsregeln
- [Weitere Informationen](https://docs.microsoft.com/azure/service-health/service-health-overview) zu Azure Service Health


