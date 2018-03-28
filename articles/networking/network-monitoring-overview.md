---
title: Informationen zur Netzwerküberwachung in Log Analytics | Microsoft-Dokumentation
description: Übersicht über Netzwerküberwachungslösungen (einschließlich NPM) für die übergreifende Verwaltung von Netzwerken in cloudbasierten und lokalen Umgebungen sowie in Hybridumgebungen.
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 7b9f42607f313f5570f414e810eafc6775ea18b9
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="network-monitoring-solutions"></a>Netzwerküberwachungslösungen 

In Azure stehen verschiedene Lösungen für die Überwachung Ihrer Netzwerkressourcen zur Verfügung. Azure bietet Lösungen und Hilfsprogramme zur Überwachung der Netzwerkkonnektivität und der Integrität von ExpressRoute-Verbindungen sowie zur Analyse des Netzwerkdatenverkehrs in der Cloud.

## <a name="network-performance-monitor-npm"></a>Netzwerkleistungsmonitor (NPM)

Der Netzwerkleistungsmonitor (Network Performance Monitor, NPM) ist eine Sammlung von Funktionen zur Überwachung der Integrität Ihres Netzwerks sowie der Netzwerkkonnektivität mit Ihren Anwendungen und liefert Informationen zur Leistung Ihres Netzwerks. Der Netzwerkleistungsmonitor ist eine cloudbasierte Hybrid-Netzwerküberwachungslösung zur Überwachung der Konnektivität zwischen:
 
* Cloudbereitstellungen und lokalen Standorten
* Mehreren Rechenzentren und Zweigstellen
* Unternehmenswichtigen Microservices/Anwendungen mit mehreren Ebenen
* Benutzerstandorten und webbasierten Anwendungen (HTTP/HTTPS) 

Systemmonitor, ExpressRoute-Monitor und Dienstendpunktmonitor sind Überwachungsfunktionen in NPM und werden im Folgenden beschrieben.

## <a name="performance-monitor"></a>Systemmonitor

Der Systemmonitor ist Teil des Netzwerkleistungsmonitors und bietet eine Netzwerküberwachung für cloudbasierte und lokale Umgebungen sowie für Hybridumgebungen. Damit können Sie die Netzwerkkonnektivität zwischen Zweig- und Außenstellen, Filialstandorten, Rechenzentren und Clouds überwachen. Sie können Netzwerkprobleme erkennen, bevor sie Ihre Benutzer beinträchtigen. Zentrale Vorteile:

* Überwachen von Verlusten und Wartezeiten in verschiedenen Subnetzen und Festlegen von Warnungen
* Überwachen aller Pfade (einschließlich der redundanten) des Netzwerks
* Behandeln von vorübergehenden Netzwerkproblemen und Point-in-Time-Netzwerkproblemen, die schwer reproduzierbar sind
* Ermitteln des spezifischen Netzwerksegments, das für die Leistungsbeeinträchtigung verantwortlich ist
* Überwachen der Integrität des Netzwerks ohne SNMP

![NPM-Topologiekarte](./media/network-monitoring-overview/npm-topology-map.png) 

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Netzwerkleistungsmonitor-Lösung in Azure](../log-analytics/log-analytics-network-performance-monitor.md) 
* [Anwendungsfälle](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
*  Produktupdates: [Februar 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/), [August 2017](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>ExpressRoute-Monitor

Der Netzwerkleistungsmonitor für ExpressRoute bietet eine umfassende ExpressRoute-Überwachung für private Peeringverbindungen. Sie können die End-to-End-Konnektivität und -Leistung zwischen Ihren Zweigstellen und Azure über ExpressRoute überwachen. Zentrale Funktionen:

* Automatische Erkennung der mit Ihrem Abonnement verknüpften ExpressRoute-Verbindungen
* Erkennung der Netzwerktopologie zwischen Ihrer lokalen Umgebung und Ihren Cloudanwendungen
* Kapazitätsplanung, Auslastungsanalyse
* Überwachung und Warnungsgenerierung für primäre und sekundäre Pfade
* Erkennung einer Verschlechterung der Konnektivität mit VNets

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Konfigurieren des Netzwerkleistungsmonitors für ExpressRoute](../expressroute/how-to-npm.md)
* [Blogbeitrag](https://aka.ms/NPMExRmonitorGA)

## <a name="service-endpoint-monitor"></a>Dienstendpunktmonitor

Mit der Dienstendpunktüberwachung können Sie nun die Erreichbarkeit von Anwendungen testen und Leistungsengpässe in der lokalen Umgebung, in Netzbetreiber-Netzwerken und in cloudbasierten/privaten Rechenzentren erkennen.

* Überwachen der End-to-End-Netzwerkkonnektivität mit Anwendungen
* Korrelieren der Anwendungsbereitstellung mit der Netzwerkleistung sowie Ermitteln des genauen Orts der Leistungsminderung entlang des Pfads zwischen Benutzer und Anwendung
* Testen der Erreichbarkeit von Anwendungen über verschiedene Benutzerstandorte auf der ganzen Welt
* Bestimmen der Netzwerkwartezeit und des Paketverlusts für Ihre Branchen- und SaaS-Anwendungen
* Bestimmen von Hotspots im Netzwerk, die ggf. zu einer Verschlechterung der Anwendungsleistung führen
* Überwachen der Erreichbarkeit von Office 365-Anwendungen mithilfe integrierter Tests für Microsoft Office 365, Dynamics 365, Skype for Business und andere Microsoft-Dienste

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Konfigurieren des Netzwerkleistungsmonitors für die Überwachung von Dienstendpunkten](https://aka.ms/applicationconnectivitymonitorguide)
* [Blogbeitrag](https://aka.ms/svcendptmonitor)

## <a name="traffic-analytics"></a>Traffic Analytics
Traffic Analytics ist eine cloudbasierte Lösung, die Einblick in Benutzer- und Anwendungsaktivitäten in Ihrem Cloudnetzwerk bietet. NSG-Flow-Protokolle werden analysiert, um Einblicke zu bieten in:

* Datenverkehrsflüsse über Ihre Netzwerke zwischen Azure und Internet, öffentliche Cloudregionen, VNETs und Subnetze
* Anwendungen und Protokolle in Ihrem Netzwerk, ohne die Notwendigkeit, Sniffer oder dedizierte Datenflusssammler-Appliances einzusetzen
* Spitzennutzer, „geschwätzige“ Anwendungen, VM-Konversationen in der Cloud, die Datenverkehrhotspots
* Quellen und Ziele des Datenverkehrs über VNETs, Beziehungen zwischen geschäftskritischen Diensten und Anwendungen
* Sicherheit – schädlicher Datenverkehr, zum Internet offene Ports, Anwendungen oder VMs, die versuchen, auf das Internet zuzugreifen...
* Kapazitätsauslastung – unterstützt Sie beim Vermeiden von Überbereitstellung oder Unterauslastung durch Überwachung der Nutzungstrends von VPN-Gateways und anderen Diensten

Traffic Analytics bietet Ihnen Informationen, die Ihnen erleichtern, die Netzwerkaktivität Ihres Unternehmens zu überwachen, Anwendungen und Daten zu schützen, die Arbeitsauslastungsleistung zu optimieren und die Kompatibilität aufrechtzuerhalten.

![Geografische Karte, die Datenverkehr Regionen übergreifend darstellt](../network-watcher/media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png) 

Verwandte Links:
* [Blogbeitrag](https://aka.ms/trafficanalytics), [Dokumentation](https://aka.ms/trafficanalyticsdocs), [FAQ](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-faq)

## <a name="dns-analytics"></a>DNS-Analyse
Diese für DNS-Administratoren konzipierte Lösung erfasst, analysiert und korreliert DNS-Protokolle, um Sicherheit, Vorgänge und leistungsbezogene Einblicke bereitzustellen.  Einige der Funktionen sind:

* Identifizieren von Clients, die versuchen, schädliche Domänennamen aufzulösen
* Identifizieren von veralteten Ressourceneinträgen
* Einblick in häufig abgefragte Domänennamen und DNS-Clients mit hoher Aktivität
* Einblick in die Anforderungsbelastung auf DNS-Servern
* Überwachen von Fehlern der dynamischen DNS-Registrierung

![DNS Analytics-Dashboard](./media/network-monitoring-overview/dns-analytics-overview.png) 

Verwandte Links:
* [Blogbeitrag](https://blogs.technet.microsoft.com/msoms/2017/04/19/introducing-oms-dns-analytics/), [Dokumentation](https://docs.microsoft.com/azure/log-analytics/log-analytics-dns)

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren des Netzwerkleistungsmonitors](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor)
* [Konfigurieren des Netzwerkleistungsmonitors für ExpressRoute](../expressroute/how-to-npm.md)
