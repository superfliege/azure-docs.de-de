---
title: Überlegungen zur Integration der Netzwerkkonnektivität über Border-Geräte für integrierte Azure Stack-Systeme | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Netzwerkkonnektivität über Border-Geräte für Rechenzentren mit Azure Stack-Systemen mit mehreren Knoten planen können.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: 93dd609df90adac2c84ba8c62cf0d18f55a317bb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
ms.locfileid: "28919433"
---
# <a name="border-connectivity"></a>Grenzkonnektivität 
Die Planung der Netzwerkintegration ist eine wichtige Voraussetzung, um erfolgreich integrierte Azure Stack-Systeme bereitstellen, betreiben und verwalten zu können. Bei der Planung der Konnektivität über Border-Geräte wird zunächst festgelegt, ob dynamisches Routing mit Border Gateway Protocol (BGP) verwendet werden soll. Hierfür muss eine autonome 16-Bit-BGP-Systemnummer (öffentlich oder privat) zugewiesen oder statisches Routing verwendet werden, wenn Border-Geräten eine statische Standardroute zugewiesen wird.

> [!IMPORTANT]
> Für TOR-Switches (Top of Rack) sind Layer 3-Uplinks mit für physische Schnittstellen konfigurierte Point-to-Point-IP-Adressen (Netzwerke vom Typ „/30“) erforderlich. Layer 2-Uplinks mit TOR-Switches mit Unterstützung für Azure Stack-Vorgänge werden nicht unterstützt. 

## <a name="bgp-routing"></a>BGP-Routing
Durch ein dynamisches Routingprotokoll wie BGP wird die Erkennung von Netzwerkänderungen durch Ihr System sichergestellt und die Verwaltung vereinfacht. 

Wie im folgenden Diagramm dargestellt, wird die Ankündigung des privaten IP-Bereichs für den TOR-Switch durch eine Präfixliste beschränkt. Die Präfixliste lehnt die privaten IP-Subnetze ab und wendet diese als Routenzuordnung für die Verbindung zwischen dem TOR-Switch und dem Border-Gerät an.

Software Load Balancer (SLB) in der Azure Stack-Lösung stellt mittels Peering eine Verbindung mit TOR-Geräten her, damit die VIP-Adressen dynamisch angekündigt werden können.

Um sicherzustellen, dass Benutzerdatenverkehr nach einem Fehler umgehend und transparent wiederhergestellt wird, ermöglicht die zwischen den TOR-Geräten konfigurierte vPC- oder MLAG-Vernetzung die Verwendung von MLAG (Multi-Chassis Link Aggregation) für die Hosts sowie des HSRP- oder VRRP-Protokolls, das Redundanz für die IP-Netzwerke bietet.

![BGP-Routing](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>Statisches Routing
Die Verwendung von statischen Routen fügt festere Konfigurationen für Border- sowie TOR-Geräte hinzu. Vor jeder Änderung ist eine gründliche Analyse erforderlich. Bei Problemen, die durch Konfigurationsfehler verursacht wurden, kann der Rollback je nach den vorgenommenen Änderungen relativ zeitintensiv sein. Dies ist zwar nicht die empfohlene Routingmethode, es besteht jedoch Unterstützung für diese.

Um Azure Stack mit dieser Methode in Ihre Netzwerkumgebung zu integrieren, muss das Border-Gerät für Datenverkehr an externe Netzwerke und öffentliche VIP-Adressen mit statischen Routen, die auf TOR-Geräte verweisen, konfiguriert sein.

Die TOR-Geräte müssen mit einer statischen Standardroute konfiguriert sein, die sämtlichen Datenverkehr an die Border-Geräte sendet. Die einzige Datenverkehrsausnahme für diese Regel ist der private Bereich. Dieser wird mithilfe einer Zugriffssteuerungsliste für die Verbindung zwischen dem TOR- und Border-Gerät blockiert.

Alle anderen Elemente sollten der ersten Methode entsprechen. Nach wie vor kommt das dynamische BGP-Routing im Rack zum Einsatz, da es ein unverzichtbares Hilfsmittel für SLB und andere Komponenten darstellt. Es kann daher nicht deaktiviert oder entfernt werden.

![Statisches Routing](media/azure-stack-border-connectivity/static-routing.png)

## <a name="transparent-proxy"></a>Transparenter Proxy
Wenn Ihr Rechenzentrum verlangt, dass für den gesamten Datenverkehr ein Proxy verwendet wird, müssen Sie einen *transparenten Proxy* konfigurieren. Dieser muss den gesamten Datenverkehr gemäß den Richtlinien verarbeiten und den Datenverkehr zwischen den Zonen in Ihrem Netzwerk trennen.

> [!IMPORTANT]
> Die Azure Stack-Lösung unterstützt keine normalen Webproxys.  

Ein transparenter Proxy (auch abfangender, inline oder erzwungener Proxy genannt) fängt die normale Kommunikation auf der Netzwerkschicht ab, ohne dass eine spezielle Clientkonfiguration erforderlich ist. Clients müssen nicht wissen, dass der Proxy vorhanden ist.

![Transparenter Proxy](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Nächste Schritte
[DNS-Integration](azure-stack-integrate-dns.md)