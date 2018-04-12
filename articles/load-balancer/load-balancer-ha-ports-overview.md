---
title: Übersicht über Hochverfügbarkeitsports in Azure | Microsoft-Dokumentation
description: Erfahren Sie mehr über Hochverfügbarkeitsports, die auf einem internen Load Balancer einen Lastenausgleich vornehmen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: kumud
ms.openlocfilehash: f6e9dd09558a3485629d5b70dd8b68b292427b18
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="high-availability-ports-overview"></a>Übersicht über Hochverfügbarkeitsports

Azure Standard Load Balancer unterstützt Sie beim gleichzeitigen Lastenausgleich von TCP- und UDP-Datenflüssen an allen Ports, wenn Sie einen internen Load Balancer verwenden. 

Eine Regel für Hochverfügbarkeitsports ist eine Variante einer Lastenausgleichsregel, die für eine interne Standard Load Balancer-Instanz konfiguriert wurde. Sie können Ihre Verwendung von Load Balancer vereinfachen, indem Sie eine einzelne Lastenausgleichsregel für den Lastenausgleich aller TCP- und UDP-Datenflüsse bereitstellen, die an allen Ports eines internen Standard Load Balancer eingehen. Die Entscheidung über den Lastenausgleich erfolgt pro Datenfluss. Dies basiert auf der folgenden Verbindung, die sich aus einem fünfteiligen Tupel zusammensetzt: Quell-IP-Adresse, Quellport, Ziel-IP-Adresse, Zielport und Protokoll.

Das Feature für Hochverfügbarkeitsports unterstützt Sie bei wichtigen Szenarien, z. B. Hochverfügbarkeit und Skalierung für virtuelle Netzwerkgeräte in virtuellen Netzwerken. Es kann auch hilfreich sein, wenn für eine große Anzahl von Ports ein Lastenausgleich vorgenommen werden muss. 

Das Feature für Hochverfügbarkeitsports ist konfiguriert, wenn Sie für die Front-End- und Back-End-Ports **0** und für das Protokoll **Alle** festlegen. Die interne Load Balancer-Ressource nimmt dann für alle TCP- und UDP-Datenflüsse unabhängig von der Portnummer einen Lastenausgleich vor.

## <a name="why-use-ha-ports"></a>Gründe für die Verwendung von Hochverfügbarkeitsports

### <a name="nva"></a>Virtuelle Netzwerkgeräte

Sie können virtuelle Netzwerkgeräte (Network Virtual Appliances, NVAs) verwenden, um Ihre Azure-Workload vor verschiedenen Typen von Sicherheitsrisiken zu schützen. Wenn virtuelle Netzwerkgeräte in diesen Szenarien verwendet werden, müssen sie zuverlässig, hochverfügbar und bedarfsgerecht horizontal hochskalierbar sein.

Sie können diese Ziele erreichen, indem Sie einfach NVA-Instanzen zum Back-End-Pool des internen Azure Load Balancers hinzufügen und eine Lastenausgleichsregel für Hochverfügbarkeitsports konfigurieren.

Hochverfügbarkeitsports bieten mehrere Vorteile für NVA-Hochverfügbarkeitsszenarien:
- Schnelles Failover auf fehlerfreie Instanzen mit Integritätstests pro Instanz
- Höhere Leistung mit horizontaler Skalierung auf *n* aktive Instanzen
- Szenarien mit *n* aktiven sowie aktiven und passiven Instanzen
- Komplexe Lösungen wie Apache ZooKeeper-Knoten für die Überwachung von Geräten werden überflüssig

Das folgende Diagramm zeigt eine Nabe-zu-Speiche-Bereitstellung eines virtuellen Netzwerks. Die Speichen erzwingen das Tunneling für ihren Datenverkehr zum virtuellen Nabennetzwerk und über das virtuelle Netzwerkgerät, bevor der vertrauenswürdige Bereich verlassen wird. Die virtuellen Netzwerkgeräte befinden sich hinter einem internen Standard Load Balancer mit einer Konfiguration für die Hochverfügbarkeitsports. Der gesamte Datenverkehr kann entsprechend verarbeitet und weitergeleitet werden.

![Diagramm eines virtuellen Nabe-zu-Speiche-Netzwerks mit virtuellen Netzwerkgeräten, die im Hochverfügbarkeitsmodus bereitgestellt wurden](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Bei Verwendung von virtuellen Netzwerkgeräten überprüfen Sie beim jeweiligen Anbieter, wie Hochverfügbarkeitsports optimal genutzt und welche Szenarien unterstützt werden.

### <a name="load-balancing-large-numbers-of-ports"></a>Lastenausgleich für eine große Anzahl von Ports

Sie können Hochverfügbarkeitsports auch für Anwendungen verwenden, in denen ein Lastenausgleich für eine große Anzahl von Ports erforderlich ist. Sie können diese Szenarien vereinfachen, indem Sie einen internen [Standard Load Balancer](load-balancer-standard-overview.md) mit Hochverfügbarkeitsports verwenden. Eine einzelne Lastenausgleichsregel ersetzt mehrere einzelne Lastenausgleichsregeln, die jeweils für einen Port verwendet wurden.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Das Feature für Hochverfügbarkeitsports ist in allen Azure-Regionen weltweit verfügbar.

## <a name="supported-configurations"></a>Unterstützte Konfigurationen

### <a name="one-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-the-internal-standard-load-balancer"></a>Eine einzige Non-Floating-IP-Konfiguration (kein Direct Server Return) für Hochverfügbarkeitsports auf dem internen Standard Load Balancer

Dies ist eine Basiskonfiguration für Hochverfügbarkeitsports. Die folgende Konfiguration ermöglicht es Ihnen, Lastenausgleich mit Hochverfügbarkeitsports für eine einzigen Front-End-IP-Adresse zu konfigurieren:
- Aktivieren Sie bei der Konfiguration von Standard Load Balancer das Kontrollkästchen **HA-Ports** in der Load Balancer-Regelkonfiguration. 
- Stellen Sie sicher, dass **Floating IP** auf **Deaktiviert** gesetzt ist.

Diese Konfiguration erlaubt keine andere Regelkonfiguration für den Lastenausgleich auf der aktuellen Load Balancer-Ressource sowie keine andere interne Load Balancer-Ressourcenkonfiguration für die angegebene Gruppe von Back-End-Instanzen.

Sie können jedoch zusätzlich zu dieser HA-Portregel einen öffentlichen Standard Load Balancer für die Back-End-Instanzen konfigurieren.

### <a name="one-single-floating-ip-direct-server-return-ha-ports-configuration-on-the-internal-standard-load-balancer"></a>Eine einzige Floating-IP-Konfiguration (Direct Server Return) für Hochverfügbarkeitsports auf dem internen Standard Load Balancer

Sie können Load Balancer auch so konfigurieren, dass eine Lastenausgleichsregel mit **HA-Port** mit einem einzigen Front-End verwendet und **Floating IP** auf **Aktiviert** gesetzt wird. 

Diese Konfiguration ermöglicht es Ihnen, weitere Lastenausgleichsregeln für Floating IPs und/oder einen öffentlichen Load Balancer hinzuzufügen. Es ist jedoch nicht möglich, zusätzlich zu dieser Konfiguration eine Lastenausgleichskonfiguration für HA-Ports mit Non-Floating IPs zu verwenden.

### <a name="multiple-ha-ports-configurations-on-the-internal-standard-load-balancer"></a>Mehrere HA-Portkonfigurationen auf der internen Standard Load Balancer-Instanz

Wenn Ihr Szenario erfordert, dass Sie mehrere Front-Ends mit Hochverfügbarkeitsports für den gleichen Back-End-Pool konfigurieren, können Sie dies wie folgt erreichen: 
- Konfigurieren Sie mehrere private Front-End-IP-Adressen für eine einzige interne Standard Load Balancer-Ressource.
- Konfigurieren Sie mehrere Lastenausgleichsregeln, wobei jede Regel eine einzige eindeutige Front-End-IP-Adresse hat.
- Wählen Sie die Option **HA-Ports**, und setzen Sie für alle Lastenausgleichsregeln die Option **Floating IP** auf **Aktiviert**.

### <a name="internal-load-balancer-with-ha-ports--public-load-balancer-on-the-same-backend-instances"></a>Interner Load Balancer mit Hochverfügbarkeitsports und öffentlicher Load Balancer auf denselben Back-End-Instanzen

Sie können **eine** öffentliche Standard Load Balancer-Ressource für die Back-End-Ressourcen zusammen mit einem einzigen internen Standard Load Balancer mit Hochverfügbarkeitsports konfigurieren.

>[!NOTE]
>Diese Funktion ist jetzt über die Azure Resource Manager-Vorlagen verfügbar, nicht aber über das Azure-Portal.

## <a name="limitations"></a>Einschränkungen

- HA-Portskonfiguration ist nur für den internen Load Balancer verfügbar, nicht für einen öffentlichen Load Balancer.

- Eine Kombination aus einer Lastenausgleichsregel mit und einer ohne Hochverfügbarkeitsports wird nicht unterstützt.

- Das Feature für Hochverfügbarkeitsports ist für IPv6 nicht verfügbar.

- Flowsymmetrie für NVA-Szenarien wird nur mit einer einzelnen NIC unterstützt. Sehen Sie sich die Beschreibung und das Diagramm unter [Virtuelle Netzwerkgeräte](#nva) an. Sollte für Ihre Szenario ein Ziel-NAT in Frage kommt, können Sie dies verwenden, um sicherzustellen, dass der interne Load Balancer den ausgehenden Datenverkehr an dieselbe NVA sendet.


## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Hochverfügbarkeitsports für einen internen Standard Load Balancer](load-balancer-configure-ha-ports.md)
- [Übersicht über Standard Load Balancer](load-balancer-standard-overview.md)
