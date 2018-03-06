---
title: "Netzwerkleistungsmonitor-Lösung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Mit der ExpressRoute-Monitor-Funktion des Netzwerkleistungsmonitors können Sie die End-to-End-Konnektivität und -Leistung zwischen Ihren Zweigstellen und Azure über Azure ExpressRoute überwachen."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 405bcd7d69e93f838d35b61be489464fcf55ee88
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="expressroute-manager"></a>ExpressRoute-Monitor

Mit der ExpressRoute-Monitor-Funktion im [Netzwerkleistungsmonitor](log-analytics-network-performance-monitor.md) können Sie die End-to-End-Konnektivität und -Leistung zwischen Ihren Zweigstellen und Azure über Azure ExpressRoute überwachen. Zentrale Vorteile: 

- Automatische Erkennung der mit Ihrem Abonnement verknüpften ExpressRoute-Verbindungen 
- Nachverfolgung von Bandbreitennutzung, Verlusten und Wartezeiten der Verbindung, Peering und VNet-Ebene für Ihre ExpressRoute-Verbindung 
- Erkennung der Netzwerktopologie Ihrer ExpressRoute-Verbindungen 

![ExpressRoute-Monitor](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Konfiguration 
Öffnen Sie die [Netzwerkleistungsmonitor-Lösung](log-analytics-network-performance-monitor.md), und klicken Sie auf die Schaltfläche **Konfigurieren**, um die Konfiguration für den Netzwerkleistungsmonitor zu öffnen.

### <a name="configure-nsg-rules"></a>Konfigurieren von NSG-Regeln 
Für die Server in Azure, die zur Überwachung über den Netzwerkleistungsmonitor verwendet werden, müssen Regeln für Netzwerksicherheitsgruppen (NSGs) konfiguriert werden, um TCP-Datenverkehr an dem Port zuzulassen, der vom Netzwerkleistungsmonitor für synthetische Transaktionen verwendet wird. Der Standardport ist 8084. Dadurch kann der OMS-Agent, der auf einem virtuellen Azure-Computer installiert ist, mit einem lokalen Überwachungs-Agent kommunizieren. 

Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter  [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

>[!NOTE]
> Vergewissern Sie sich, dass Sie die Agents (sowohl den lokalen Server-Agent als auch den Azure-Server-Agent) installiert und das PowerShell-Skript „EnableRules.ps1“ ausgeführt haben, bevor Sie mit diesem Schritt fortfahren. 

 
### <a name="discover-expressroute-peering-connections"></a>Ermitteln der ExpressRoute-Peeringverbindungen 
 
1. Klicken Sie auf die Ansicht **ExpressRoute-Peerings**.  
2. Klicken Sie auf die Schaltfläche **Jetzt ermitteln**, um alle privaten ExpressRoute-Peerings zu ermitteln, die mit den VNets in dem Azure-Abonnement verbunden sind, das mit diesem Log Analytics-Arbeitsbereich verknüpft ist.  

>[!NOTE]  
> Die Lösung ermittelt derzeit nur private ExpressRoute-Peerings. 

>[!NOTE]  
> Es werden nur private Peerings ermittelt, die mit den VNets verbunden sind, welche dem mit diesem Log Analytics-Arbeitsbereich verknüpften Abonnement zugeordnet sind. Falls Ihre ExpressRoute-Instanz außerhalb des mit diesem Workspace verknüpften Abonnements mit VNets verbunden ist, müssen Sie einen Log Analytics-Arbeitsbereich in den entsprechenden Abonnements erstellen und diese Peerings mithilfe des NPM überwachen. 

 ![Konfigurieren des ExpressRoute-Monitors](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Nach Abschluss der Ermittlung werden die ermittelten privaten Peeringverbindungen in einer Tabelle aufgeführt. Die Überwachung für diese Peerings ist zunächst deaktiviert. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>Aktivieren der Überwachung der ExpressRoute-Peeringverbindungen 

1. Klicken Sie auf die private Peeringverbindung, die Sie überwachen möchten.  
2. Aktivieren Sie im rechten Bereich das Kontrollkästchen **Dieses Peering überwachen**. 
3. Wenn für diese Verbindung Integritätsereignisse erstellt werden sollen, aktivieren Sie das Kontrollkästchen **Integritätsüberwachung für dieses Peering aktivieren**. 
4. Wählen Sie Überwachungsbedingungen aus. Sie können benutzerdefinierte Schwellenwerte für das Generieren von Integritätsereignissen festlegen, indem Sie Schwellenwerte eingeben. Sobald der Wert einer Bedingung den festgelegten Schwellenwert für die Peeringverbindung überschreitet, wird ein Integritätsereignis generiert. 
5. Klicken Sie auf **Agents hinzufügen**, um die Überwachungs-Agents auszuwählen, die Sie zum Überwachen dieser Peeringverbindung verwenden möchten. Achten Sie darauf, dass Sie Agents an beiden Enden der Verbindung hinzufügen (mindestens einen in dem virtuellen Azure-Netzwerk, das mit diesem Peering verbunden ist, und mindestens einen lokalen Agent, der mit diesem Peering verbunden ist). 
6. Klicken Sie zum Speichern der Konfiguration auf **Speichern**. 

![Konfigurieren des ExpressRoute-Monitors](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Nach dem Aktivieren der Regeln und dem Auswählen der Werte und Agents, die Sie überwachen möchten, gibt es eine Wartezeit von etwa 30 bis 60 Minuten, bis die Werte aufgefüllt und die Kacheln **ExpressRoute-Überwachung** verfügbar sind. Sobald Ihnen die Überwachungskacheln angezeigt werden, werden Ihre ExpressRoute-Verbindungen und -Verbindungsressourcen vom Netzwerkleistungsmonitor überwacht. 

>[!NOTE]
> Diese Funktion kann zuverlässig für Arbeitsbereiche verwendet werden, für die ein Upgrade auf die Abfragesprache durchgeführt wurde.  

## <a name="walkthrough"></a>Exemplarische Vorgehensweise 

Das Dashboard des Netzwerkleistungsmonitors zeigt eine Übersicht über die Integrität von ExpressRoute-Verbindungen und Peeringverbindungen. 

![NPM-Dashboard für ExpressRoute](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Verbindungsliste 

Klicken Sie auf die Kachel „ExpressRoute-Verbindungen“, um eine Liste aller überwachten ExpressRoute-Verbindungen anzuzeigen. Sie können eine Verbindung auswählen und deren Integritätsstatus, Trenddiagramme für den Paketverlust, Bandbreitennutzung und Latenz anzuzeigen. Die Diagramme sind interaktiv. Sie können ein benutzerdefiniertes Zeitfenster für das Zeichnen der Diagramme auswählen. Sie können die Maus über einen Bereich des Diagramms ziehen, um dieses zu vergrößern und detaillierte Datenpunkte anzuzeigen. 

![ExpressRoute-Verbindungsliste](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trend-of-loss-latency-and-throughput"></a>Trend für Verlust, Wartezeit und Durchsatz 

Die Diagramme für Bandbreite, Latenz und Verlust sind interaktiv. Sie können jeden Bereich dieser Diagramme mithilfe der Maussteuerung vergrößern. Sie können auch die Daten anderer Intervalle für Bandbreite, Wartezeit und Verlust anzeigen, indem Sie links oben unter der Schaltfläche „Aktionen“auf „Datum/Uhrzeit“ klicken. 

![ExpressRoute-Wartezeit](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Peeringliste 

Durch das Klicken auf die Kachel **Private Peerings** auf dem Dashboard wird eine Liste aller Verbindungen zu virtuellen Netzwerken über privates Peering angezeigt. Hier können Sie eine virtuelle Netzwerkverbindung auswählen und deren Integritätsstatus, Trenddiagramme für Paketverlust, Bandbreitennutzung und Latenz anzeigen lassen. 

![ExpressRoute-Peerings](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Verbindungstopologie 

Klicken Sie auf die Kachel **Topologie**, um die Topologie der Verbindung anzuzeigen. Dadurch gelangen Sie zur Ansicht der Topologie für die ausgewählte Verbindung oder das ausgewählte Peering. Das Topologiediagramm enthält die Latenz für jedes Segment im Netzwerk, außerdem wird jeder Layer 3-Hop durch einen Knoten des Diagramms dargestellt. Durch das Klicken auf einen Hop werden weitere Details zum Hop angezeigt. Sie können den Sichtbarkeitsgrad erhöhen, um lokale Hops einzuschließen, indem Sie den Schieberegler unter **Filter** bewegen. Das Bewegen des Schiebereglers nach links oder rechts vergrößert oder verkleinert die Anzahl der Hops im Topologiediagramm. Die Latenz ist für jedes Segment sichtbar, wodurch eine schnellere Isolation von Segmenten mit hoher Latenz in Ihrem Netzwerk ermöglicht wird. 

![ExpressRoute-Topologie](media/log-analytics-network-performance-monitor/expressroute-topology.png)  

### <a name="detailed-topology-view-of-a-circuit"></a>Detaillierte Topologieansicht einer Verbindung 

Diese Ansicht zeigt die VNET-Verbindungen. 

![ExpressRoute-VNet](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Diagnose 

Der Netzwerkleistungsmonitor unterstützt Sie beim Diagnostizieren mehrerer verbindungsbezogener Konnektivitätsprobleme. Einige davon sind im Anschluss aufgeführt: 

**Die Verbindung ist nicht verfügbar:** Der Netzwerkleistungsmonitor informiert Sie, sobald die Konnektivität zwischen Ihren lokalen Ressourcen und virtuellen Azure-Netzwerken verloren geht. So können Sie proaktive Maßnahmen ergreifen und Ausfallzeiten verringern, bevor Benutzer das Problem eskalieren. 

![ExpressRoute-Verbindung nicht verfügbar](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Datenverkehr wird nicht über die beabsichtigte Verbindung übertragen:** Der Netzwerkleistungsmonitor kann Sie benachrichtigen, wenn der Datenverkehr unerwartet nicht über die beabsichtigte ExpressRoute-Verbindung übertragen wird. Dieser Fall kann eintreten, wenn die Verbindung nicht verfügbar ist und der Datenverkehr über die Ausweichroute übermittelt wird oder wenn ein Routingproblem vorliegt. Anhand dieser Informationen können Sie proaktiv mögliche Konfigurationsprobleme in Ihren Routingrichtlinien behandeln und sicherstellen, dass die beste und sicherste Route verwendet wird. 

 

**Datenverkehr wird nicht über die primäre Verbindung übertragen:** Die Funktion benachrichtigt Sie, wenn der Datenverkehr über die sekundäre ExpressRoute-Verbindung übertragen wird. In diesem Fall treten zwar keine Konnektivitätsprobleme auf, durch die proaktive Behandlung der Probleme mit der primären Verbindung sind Sie jedoch besser vorbereitet. 

 
![ExpressRoute-Datenverkehrsfluss](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**Leistungsminderung aufgrund von Auslastungsspitzen:** Sie können den Bandbreitennutzungstrend mit dem Wartezeitentrend korrelieren, um zu ermitteln, ob die Leistungsminderung bei der Azure-Workload auf eine Bandbreitennutzungsspitze zurückzuführen ist, und ggf. entsprechende Maßnahmen ergreifen.  

![ExpressRoute-Monitor](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Nächste Schritte
* Mit [Protokollsuchen](log-analytics-log-searches.md) können Sie Detaildatensätze mit Netzwerkleistungsdaten anzeigen.
