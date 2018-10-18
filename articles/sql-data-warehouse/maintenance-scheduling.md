---
title: Azure-Wartungszeitpläne (Vorschau) | Microsoft-Dokumentation
description: Wartungszeitpläne ermöglichen es Kunden, die notwendigen geplanten Wartungsereignisse in ihre Planungen einzubeziehen, mit denen der Azure SQL Data Warehouse-Dienst neue Features, Upgrades und Patches einführt.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 09/20/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 85e8327d1cac17059b2e91b1ea21becc23002c8e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228121"
---
# <a name="using-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Verwenden von Wartungszeitplänen zum Verwalten von Updates und Wartung von Diensten

Der Wartungszeitplan für Azure SQL Data Warehouse ist ab sofort als Vorschauversion verfügbar. Dieses neue Feature lässt sich nahtlos in Benachrichtigungen über eine geplante Wartung von Service Health, in Resource Health Check Monitor und den Zeitplanungsdienst für Azure SQL Data Warehouse-Wartungen integrieren.

Mit einem Wartungszeitplan können Sie ein passendes Zeitfenster für den Erhalt neuer Features, Upgrades und Patches planen. Kunden wählen ein primäres und ein sekundäres Wartungsfenster in einem Zeitraum von 7 Tagen aus, z.B. Samstag 22:00 bis Sonntag 01:00 (primär) und Mittwoch 19:00 bis 22:00 (sekundär). Wenn wir die Wartung während des primären Wartungsfensters nicht durchführen können, versuchen wir es während des sekundären Wartungsfensters erneut.

Auf alle neu erstellten Azure SQL Data Warehouse-Instanzen muss während der Bereitstellung ein systemdefinierter Wartungszeitplan angewendet werden. Der Zeitplan kann direkt nach Abschluss der Bereitstellung bearbeitet werden.

Jedes Wartungsfenster kann zwischen 3 und 8 Stunden umfassen, wobei 3 Stunden die derzeit kürzeste verfügbare Option ist. Die Wartung kann jederzeit innerhalb dieses Zeitfensters durchgeführt werden. In dieser Zeit müssen Sie mit einer kurzen Verbindungsunterbrechung rechnen, da der Dienst neuen Code für Ihr Data Warehouse bereitstellt. 

Während der Featurevorschau bitten wir die Kunden, ihre primären und sekundären Fenster an getrennten Tagen anzugeben.   
Alle Wartungsvorgänge sollten innerhalb der geplanten Wartungsfenster abgeschlossen werden. Außerhalb der angegebenen Wartungsfenster werden keine Wartungen ohne vorherige Benachrichtigung durchgeführt. Wenn Ihr Data Warehouse während einer geplanten Wartung angehalten wird, wird es während des Wiederaufnahmevorgangs aktualisiert.  


## <a name="alerts-and-monitoring"></a>Benachrichtigungen und Überwachung

Dank der nahtlosen Integration in Service Health-Benachrichtigungen und in Resource Health Check Monitor sind Kunden immer über bevorstehende Wartungsaktivitäten informiert. Mit der neuen Automatisierung wird Azure Monitor genutzt und Kunden können bestimmen, wie sie über bevorstehende Wartungsereignisse benachrichtigt werden möchten und welche automatisierten Abläufe zur Bewältigung von Ausfallzeiten und zur Minimierung der Auswirkungen ihrer Vorgänge ausgelöst werden sollen.

Allen Wartungsereignissen geht eine Vorankündigung 24 Stunden voraus. Zur Minimierung von Ausfallzeiten bei Instanzen sollten Sie sicherstellen, dass vor Beginn des von Ihnen gewählten Wartungszeitraums in Ihrem Data Warehouse keine lang andauernden Transaktionen ausgeführt werden. Bei Beginn der Wartung werden alle aktiven Sitzungen beendet, für Transaktionen, für die kein Commit ausgeführt wurde, wird ein Rollback ausgeführt und bei Ihrem Data Warehouse kommt es zu einer kurzen Verbindungsunterbrechung. Sie werden umgehend benachrichtigt, wenn die Wartungsarbeiten in Ihrem Data Warehouse abgeschlossen sind. 

Wenn Sie eine Vorankündigung zur Durchführung von Wartungsarbeiten erhalten haben, wir die Wartung im vorgesehenen Zeitraum jedoch nicht durchführen können, erhalten Sie eine Widerrufsbenachrichtigung. Die Wartung wird dann während des nächsten geplanten Wartungszeitraums durchgeführt.
 
Alle aktiven Wartungsereignisse werden im Abschnitt „Service Health – Geplante Wartung“ angezeigt. Im Rahmen des Service Health-Verlaufs werden vergangene Ereignisse aufgezeichnet. Sie können Wartungsarbeiten während eines aktiven Ereignisses über das Dashboard des Azure Service Health Check-Portals überwachen.

### <a name="maintenance-schedule-availability"></a>Verfügbarkeit des Wartungszeitplans

Auch wenn Wartungszeitpläne in der ausgewählten Region noch nicht verfügbar sind, können Sie Ihren Wartungszeitplan dennoch jederzeit anzeigen und bearbeiten. Sobald Wartungszeitpläne in Ihrer Region verfügbar werden, wird der Zeitplan sofort erkannt und wird in Ihrem Data Warehouse aktiv.


## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen](viewing-maintenance-schedule.md) zum Anzeigen eines Wartungszeitplans 
- [Weitere Informationen](changing-maintenance-schedule.md) zum Ändern eines Wartungszeitplans
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) zum Erstellen, Anzeigen und Verwalten von Warnungen mit Azure Monitor
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) zu Webhookaktionen für Protokollwarnungsregeln
- [Weitere Informationen](https://docs.microsoft.com/azure/service-health/service-health-overview) zu Azure Service Health







