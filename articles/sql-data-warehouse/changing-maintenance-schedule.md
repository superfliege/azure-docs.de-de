---
title: Azure-Wartungszeitpläne (Vorschau) | Microsoft-Dokumentation
description: Wartungszeitpläne ermöglichen es Kunden, die notwendigen geplanten Wartungsereignisse in ihre Planungen einzubeziehen, mit denen der Azure SQL Data Warehouse-Dienst neue Features, Upgrades und Patches einführt.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/07/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: a6eedc0bac7aab69a9138f4f63d0d9d802e74dfc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228104"
---
# <a name="change-a-maintenance-schedule"></a>Ändern des Wartungszeitplans 

## <a name="portal"></a>Portal
Ein Wartungszeitplan kann jederzeit aktualisiert oder geändert werden. Wenn die ausgewählte Instanz allerdings gerade einen aktiven Wartungszyklus durchläuft, werden die Einstellungen gespeichert und erst im nächsten erkannten Wartungszeitraum wieder aktiviert. Weitere Informationen zum Überwachen Ihrer Data Warehouse-Daten während eines aktiven Verwaltungsereignisses finden Sie [hier](https://docs.microsoft.com/azure/service-health/resource-health-overview). 

In der Vorschau werden Sie gebeten, zwei Wartungsfenster in einem Zeitraum von sieben Tagen festzulegen. Jedes Wartungsfenster kann zwischen 3 und 8 Stunden umfassen, wobei 3 Stunden die derzeit kürzeste verfügbare Option ist. Es kann jederzeit eine Wartung innerhalb eines ermittelten Wartungsfenster ausgeführt werden, aber außerhalb dieses Zeitfensters erfolgt keine Wartung ohne vorherige Ankündigung. Während der Dienst neuen Code für Ihr Data Warehouse bereitstellt, ist außerdem die Konnektivität eingeschränkt. 

## <a name="identifying-the-primary-and-secondary-windows"></a>Ermitteln der primären und sekundären Fenster

Die primären und sekundären Fenster müssen verschiedene Tage abdecken: z.B. das primäre Fenster Dienstag bis Donnerstag und das sekundäre Fenster Samstag und Sonntag

Führen Sie die folgenden Schritte aus, um den Wartungszeitplan zu ändern, der auf Ihr Data Warehouse im Portal angewendet wurde.
1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2.  Wählen Sie das Data Warehouse aus, das Sie aktualisieren möchten. Die Seite wird auf dem Blatt „Übersicht“ geöffnet. 
3.  Sie können auf die Seite mit den Einstellungen für den Wartungszeitplan zugreifen, indem Sie entweder auf dem Blatt „Übersicht“ auf den Zusammenfassungslink für den Wartungszeitplan (Vorschau) klicken oder indem Sie auf die Option „Wartungszeitplan“ links im Ressourcenmenü klicken.  

    ![Optionen auf dem Blatt „Übersicht“](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Sie können die bevorzugte Zeitspanne (in Tagen) für Ihr primäres Wartungsfenster über die Optionsfelder oben auf der Seite ermitteln. Dadurch bestimmen Sie, ob Ihr primäres Fenster Werktage oder Wochenendtage abdeckt. Durch Ihre Auswahl werden die folgenden Werte aus den einzelnen Dropdownmenüs entsprechend angepasst. Während der Vorschauphase unterstützen einige Regionen ggf. noch nicht alle verfügbaren Tage. Diese Werte werden in den kommenden Monate aktualisiert.

   ![Blatt „Wartungseinstellungen“](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Wählen Sie beliebige primäre und sekundäre Wartungsfenster aus. Wählen Sie dafür einen Tag, die Startzeit und ein Zeitfenster aus den jeweiligen Dropdownmenüs aus. Die Zusammenfassung des Zeitplans unten auf dem Blatt wird anhand der aus den Dropdownmenüs ausgewählten Werte aktualisiert.

#### <a name="dropdown-options"></a>Dropdownmenüs
- Tag: bevorzugter Tag, an dem innerhalb des ausgewählten Zeitfensters die Wartungsarbeiten durchgeführt werden soll
- Startzeit: bevorzugte Startzeit des Wartungsfensters
- Zeitfenster: bevorzugter Umfang des Zeitfensters

  Wenn Sie Ihre Wartungsfenster ausgewählt haben, klicken Sie auf „Speichern“. Dann wird eine Nachricht angezeigt, in der bestätigt wird, das Ihr Wartungszeitplan jetzt aktiv ist. Wenn Sie einen Zeitplan in einer Region speichern, in der noch keine Wartungszeitpläne unterstützt werden, wird die folgende Meldung angezeigt. Ihre Einstellungen werden gespeichert und übernommen, sobald das Feature auch in Ihrer Region verfügbar ist.    

    ![Meldung „In Region noch nicht aktiv“](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) zu Webhookaktionen für Protokollwarnungsregeln
- [Weitere Informationen](https://docs.microsoft.com/azure/service-health/service-health-overview) zu Azure Service Health


