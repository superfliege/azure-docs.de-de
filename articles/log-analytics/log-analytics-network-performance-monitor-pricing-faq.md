---
title: Häufig gestellte Fragen zu Preisen für den Azure-Netzwerkleistungsmonitor | Microsoft-Dokumentation
description: Häufig gestellte Fragen – Azure-Netzwerkleistungsmonitor
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: cherylmc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: ajaycode
ms.openlocfilehash: 5b2335ee2584af07ed23ce87be92a869f3a07ba1
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>Preisänderungen für Azure-Netzwerkleistungsmonitor

Wir haben Ihr Feedback berücksichtigt und vor Kurzem eine [neue Preisgestaltung](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) für verschiedene Überwachungsdienste in Azure eingeführt.

In diesem Dokument sind Preisänderungen im Zusammenhang mit dem Azure-[Netzwerkleistungsmonitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) in einem gut lesbaren Frage-und-Antwort-Format festgehalten.

Der Netzwerkleistungsmonitor umfasst drei Komponenten:
* [Systemmonitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Dienstendpunktmonitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview#service-endpoint-monitor) und
* [ExpressRoute-Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

Im folgenden Abschnitt werden die Preisänderungen für die oben genannten Komponenten erläutert.

## <a name="performance-monitor-pm"></a>Systemmonitor (SM)

**Wie wurde die Verwendung des Systemmonitors im alten Modell in Rechnung gestellt?**

Die Abrechnung für NPM basierte auf der Nutzung/dem Verbrauch von zwei Komponenten:
* Knoten: Alle synthetischen Transaktionen beginnen und enden an den Knoten. Knoten werden auch als Agents oder MMA (Microsoft Management Agents, Microsoft-Verwaltungs-Agents) bezeichnet.
* Daten: Die Ergebnisse der verschiedenen Netzwerktests werden im Log Analytics-Repository gespeichert.

Unter dem alten Modell wurde die Rechnung basierend auf der Anzahl der Knoten und dem Volumen der generierten Daten berechnet. 

**Wie werden die Gebühren für die Verwendung des Systemmonitors unter dem neuen Modell berechnet?**

Die Systemmonitor-Funktion in NPM wird nun basierend auf einer Kombination aus folgenden Komponenten in Rechnung gestellt: 

* überwachte Subnetzlinks und
* Datenvolumen

**Was ist ein Subnetzlink?**

Systemmonitor überwacht die Konnektivität zwischen zwei oder mehr Orten im Netzwerk.  Die Verbindung zwischen einer Gruppe von Knoten/Agents in einem Subnetz und einer Gruppe von Knoten in einem anderen Subnetz wird als Subnetzlink bezeichnet.

**Ich verfüge über zwei Subnetze (Subnetz A und B) und mehrere Agents in den einzelnen Subnetzen.  Systemmonitor überwacht die Konnektivität aller Agents in Subnetz A mit allen Agents in Subnetz B. Werden Gebühren basierend auf der Anzahl der Verbindungen zwischen den Subnetzen berechnet?**

Nein. Zu Abrechnungszwecken werden alle Verbindungen zwischen Subnetz A und Subnetz B zu einem Subnetzlink gruppiert, und Ihnen wird eine einzelne Verbindung in Rechnung gestellt.  Systemmonitor überwacht weiterhin die Konnektivität zwischen verschiedenen Agents in den einzelnen Subnetzen.

**Was sind die Kosten für die Überwachung eines Subnetzlinks?**

Im Abschnitt [Pingmesh](https://azure.microsoft.com/pricing/details/network-watcher/) finden Sie die Kosten für die Überwachung eines einzelnen Subnetzlinks für den gesamten Monat.

**Was sind die Gebühren für die vom Systemmonitor generierten Daten?**

Die Gebühren für die Erfassung (Datenupload zu Log Analytics, Verarbeitung und Indizierung) sind auf der [Preisseite](https://azure.microsoft.com/pricing/details/log-analytics/) für Log Analytics verfügbar.  (Abschnitt: Datenerfassung).

Die Gebühren für Datenaufbewahrung (d.h. Daten, die gemäß der ausgewählten Option des Kunden über den ersten Monat hinaus beibehalten werden) sind ebenfalls auf der [Preisseite](https://azure.microsoft.com/pricing/details/log-analytics/) verfügbar.  (Abschnitt: Datenaufbewahrung).


## <a name="expressroute-monitor-erm"></a>ExpressRoute Monitor (ERM)

**Was sind die Gebühren für die Verwendung von ExpressRoute-Monitor?**

Gebühren für ExpressRoute-Monitor werden basierend auf der Menge der während der Überwachung generierten Daten in Rechnung gestellt.   Unter der Frage „Was sind die Gebühren für die vom Systemmonitor generierten Daten?“ finden Sie weitere Informationen.

**Ich verwende ERM zur Überwachung mehrerer ExpressRoute-Verbindungen. Werden Gebühren entsprechend der Anzahl von überwachten Verbindungen berechnet?**

Gebühren werden nicht basierend auf der Anzahl von Verbindungen oder dem Peeringtyp (z.B. privates Peering, Microsoft-Peering) berechnet.  Ihnen wird wie oben erläutert die Menge der Daten in Rechnung gestellt.

**Was ist die Menge der generierten Daten bei der Überwachung einer einzelnen Verbindung?**

Bei der Überwachung einer privaten Peeringverbindung wird pro Monat die folgende Menge an Daten generiert:

|Perzentil      |Daten/Monat (MB)|
| :---:          |           ---:|
|50.<sup></sup> |            192|
|60.<sup></sup> |            256|
|70.<sup></sup> |            360|
|80.<sup></sup> |            498|
|90.<sup></sup> |            870|
|95.<sup></sup> |           1560|


Gemäß der obigen Tabelle bezahlen Kunden beim 50. Perzentil für 192MB an Daten. Bei einem Preis von 2,30 USD/GB für den ersten Monat fallen für die Überwachung einer Verbindung im ersten Monat Kosten von 0,43 USD (= 192 * 2,30/1024) an.

**Welche Gründe gibt es für die Abweichungen bei der Datenmenge?**

Das Menge der generierten Überwachungsdaten ist von mehreren Faktoren abhängig, z.B.:
* Anzahl der Agents – die Genauigkeit der Fehlerisolation steigt bei einer höheren Anzahl von Agents
* Anzahl der Hops im Netzwerk
* Anzahl der Pfade zwischen der Quelle und Ziel

Kunden bei den höheren Perzentilen (in der obigen Tabelle) überwachen ihre Verbindungen in der Regel von mehreren Standpunkten in ihrem lokalen Netzwerk aus.  Mehrere Agents werden auch tiefer im Netzwerk, weiter entfernt vom Edgerouter des Dienstanbieters platziert. Die Agents werden häufig an mehreren Benutzerstandorten, Branches und Racks in Rechenzentren platziert.

## <a name="service-endpoint-monitor-sepm"></a>Dienstendpunktmonitor (Service Endpoint Monitor, SEPM)

**Was sind die Gebühren für die Verwendung des Dienstendpunktmonitors?**

Die Berechnung der Gebühren für die Verwendung des Dienstendpunktmonitors erfolgt basierend auf:
* Anzahl von Verbindungen
* Menge der Daten

**Was ist eine Verbindung?**

Eine Verbindung ist ein Test der Erreichbarkeit eines Endpunkts (URL oder Netzwerkdienst) über einen einzelnen Agent für den gesamten Monat. Beispielsweise stellt die Überwachung einer Verbindung mit „bing.com“ über drei Agents drei Verbindungen dar.

**Was sind die Kosten für den Dienstendpunktmonitor?**

- Im Abschnitt [Verbindungsüberwachung](https://azure.microsoft.com/pricing/details/network-watcher/) finden Sie die Kosten für die Überwachung eines Endpunkts für den gesamten Monat.
- Die Gebühren für Daten sind auf der [Preisseite](https://azure.microsoft.com/pricing/details/log-analytics/) für Log Analytics verfügbar.  (Abschnitt: Datenerfassung).

## <a name="references"></a>Referenzen

- [Log Analytics – Preise ](https://azure.microsoft.com/pricing/details/log-analytics/): Der Abschnitt „FAQ“ enthält Informationen zum Free-Tarif, zu Preisen pro Knoten usw.

