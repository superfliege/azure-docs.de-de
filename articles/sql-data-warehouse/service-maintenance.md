---
title: 'Geplante Wartung: Azure SQL Data Warehouse | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie sich auf geplante Wartungsereignisse für Azure SQL Data Warehouse vorbereiten.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/19/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: c1e8f94a0131ace6354d070e932e414a1897260e
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166301"
---
# <a name="planning-for-maintenance-on-your-azure-sql-data-warehouse"></a>Planen der Wartung für Azure SQL Data Warehouse

Hier erfahren Sie, wie Sie sich auf geplante Wartungsereignisse für Azure SQL Data Warehouse vorbereiten.

## <a name="what-is-a-planned-maintenance-event"></a>Was ist ein geplantes Wartungsereignis?
Bei einem geplanten Wartungsereignis handelt es sich um ein Zeitfenster, in dem Ihr Data Warehouse für einen Wartungsvorgang offline sein muss. Diese Ereignisse eignen sich zum Anwenden von Dienstupgrades, neuen Features oder Patches. 

## <a name="frequency"></a>Frequency
Durchschnittlich wird mindestens eine geplante Wartung pro Monat durchgeführt. 

## <a name="notifications-and-downtime"></a>Benachrichtigungen und Downtime
Vor jedem geplanten Wartungsereignis erhalten Sie eine Benachrichtigung. Das Wartungsereignis bricht alle laufenden Abfragen ab und schaltet Ihr Data Warehouse offline. Die erwartete Downtime für jedes Data Warehouse beträgt etwa 30 Minuten. Sie erhalten eine Benachrichtigung, wenn die Wartung abgeschlossen ist. 

## <a name="setting-up-alerts"></a>Einrichten von Warnungen

Es wird empfohlen, Protokollwarnungen für geplante Wartungsereignisse mithilfe von [Azure Monitor](../azure-monitor/platform/alerts-activity-log-service-notifications.md) einzurichten. Die Warnungen können Sie dabei unterstützen, die erforderlichen Wartungsvorgänge einzuplanen, um die Beeinträchtigung Ihres Geschäftsbetriebs zu minimieren. 

Halten Sie sich beim Einrichten von Benachrichtigungen an die Anweisungen unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../azure-monitor/platform/alerts-activity-log-service-notifications.md). 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Überwachung finden Sie unter [Überwachen Ihrer Workload mit dynamischen Verwaltungssichten](sql-data-warehouse-manage-monitor.md).
