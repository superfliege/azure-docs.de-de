---
title: Metriken und Warnungen in Azure Traffic Manager | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die verfügbaren Metriken für Traffic Manager in Azure.
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: fe4cc3c06af6868396f5db6fd88804022f5f9793
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47432476"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Traffic Manager-Metriken und -Warnungen

Traffic Manager bietet Ihnen DNS-basierten Lastenausgleich, der mehrere Routingmethoden und Optionen für die Endpunktüberwachung umfasst. Dieser Artikel beschreibt die Metriken und die dazugehörigen Warnungen, die für Kunden zur Verfügung stehen. 

## <a name="metrics-available-in-traffic-manager"></a>In Traffic Manager verfügbare Metriken 

Traffic Manager bietet die folgenden Metriken auf der Basis von Profilen, die von Kunden genutzt werden können, um ihre Verwendung von Traffic Manager und den Status ihrer Endpunkte unter diesem Profil zu verstehen.  

### <a name="queries-by-endpoint-returned"></a>Abfragen nach zurückgegebenem Endpunkt
Verwenden Sie [diese Metrik](../monitoring-and-diagnostics/monitoring-supported-metrics.md), um die Anzahl der Abfragen anzuzeigen, die von einem Traffic Manager-Profil in einem bestimmten Zeitraum verarbeitet wurden. Sie können die gleichen Informationen auch mit Granularität auf Endpunktebene anzeigen, um besser zu verstehen, wie oft ein Endpunkt in den Abfrageantworten von Traffic Manager zurückgegeben wurde.

Im folgenden Beispiel zeigt die Abbildung 1 alle Abfrageantworten, die vom Traffic Manager-Profil zurückgegeben wurden. 

  
![Traffic Manager-Metriken – Aggregatansicht aller Abfragen](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Abbildung 1: Aggregatansicht mit allen Abfragen*
  
Abbildung 2 zeigt die gleichen Informationen, sie sind allerdings nach Endpunkten unterteilt. So können Sie das Volumen der Abfrageantworten erkennen, in denen ein bestimmter Endpunkt zurückgegeben wurde.

![Traffic Manager-Metriken – unterteilte Ansicht des Abfragevolumens pro Endpunkt](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Abbildung 2: Unterteilte Ansicht mit Abfragevolumen, dargestellt pro zurückgegebenem Endpunkt*

## <a name="endpoint-status-by-endpoint"></a>Endpunktstatus nach Endpunkt
Verwenden Sie [diese Metrik](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles), um den Integritätsstatus der Endpunkte im Profil zu verstehen. Zwei Werte sind möglich:
 - Verwenden Sie **1**, wenn der Endpunkt aktiv ist.
 - Verwenden Sie **0**, wenn der Endpunkt ausgefallen ist.

Diese Metrik kann als ein aggregierter Wert angezeigt werden, der den Status aller Metriken darstellt (Abbildung 3). Er kann aber auch unterteilt werden (siehe Abbildung 4), um den Status von bestimmten Endpunkten anzuzeigen. Wenn im ersten Fall als Aggregationsebene **Mittelwert** ausgewählt wird, ist der Wert dieser Metrik der arithmetische Mittelwert des Status aller Endpunkte. Beispiel: Wenn ein Profil zwei Endpunkte aufweist und nur einer fehlerfrei ist, hat diese Metrik den Wert **0,50**, wie in Abbildung 3 gezeigt. 


![Traffic Manager-Metriken – zusammengesetzte Ansicht des Endpunktstatus](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Abbildung 3: Zusammengesetzte Ansicht der Metrik zum Endpunktstatus – Aggregation „Mittelwert“ ausgewählt*


![Traffic Manager-Metriken – unterteilte Ansicht des Endpunktstatus](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Abbildung 4: Unterteilte Ansicht der Metrik zum Endpunktstatus*

Sie können diese Metriken über das Portal des [Azure Monitor-Diensts](../monitoring-and-diagnostics/monitoring-supported-metrics.md), [REST-API](https://docs.microsoft.com/rest/api/monitor/), [Azure CLI](https://docs.microsoft.com/cli/azure/monitor) und [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights) oder über den Abschnitt „Metriken“ in der Traffic Manager-Portalumgebung nutzen.

## <a name="alerts-on-traffic-manager-metrics"></a>Warnungen für Traffic Manager-Metriken
Zusätzlich zum Verarbeiten und Anzeigen von Metriken aus Traffic Manager ermöglicht Azure Monitor den Kunden, Warnungen zu konfigurieren und zu empfangen, die mit diesen Metriken verknüpft sind. Sie können auswählen, welche Bedingungen in diesen Metriken erfüllt sein müssen, damit eine Warnung auftritt, wie oft diese Bedingungen beobachtet werden müssen und wie die Warnungen an Sie gesendet werden sollen. Weitere Informationen finden Sie in der [Dokumentation zu Azure Monitor-Warnungen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über den [Azure Monitor-Dienst](../monitoring-and-diagnostics/monitoring-supported-metrics.md).
- Erfahren Sie, wie Sie [ein neues Diagramm mithilfe von Azure Monitor erstellen](../monitoring-and-diagnostics/monitoring-metric-charts.md#how-do-i-create-a-new-chart).
