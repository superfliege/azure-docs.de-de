---
title: "Übersicht über internen Azure-Lastenausgleich | Microsoft Docs"
description: Funktionsweise eines internen Lastenausgleichs in Azure und Szenarien zum Konfigurieren von internen Endpunkten.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 54e390dbdb07cb4c45c801b638099aa0dcc6db1a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="overview-of-azure-internal-load-balancer"></a>Interner Azure-Lastenausgleich: Übersicht

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Der interne Azure-Lastenausgleich (Internal Load Balancer, ILB) leitet Datenverkehr nur an Ressourcen weiter, die sich innerhalb eines Clouddiensts befinden oder die ein VPN für den Zugriff auf die Azure-Infrastruktur verwenden. In dieser Hinsicht unterscheidet sich ILB von einem Lastenausgleich mit Internetzugriff. Die Azure-Infrastruktur schränkt den Zugriff auf die virtuellen IP-Adressen (VIPs) mit Lastenausgleich eines Clouddiensts oder auf ein virtuelles Netzwerk ein. VIP-Adressen und virtuelle Netzwerke werden nie direkt für einen Internetendpunkt bereitgestellt. Interne Branchenanwendungen werden in Azure ausgeführt. Auf sie wird aus Azure oder von lokalen Ressourcen aus zugegriffen.

## <a name="why-you-might-need-an-internal-load-balancer"></a>Szenarien, die für die Verwendung eines internen Lastenausgleichsmoduls sprechen

Der interne Lastenausgleich ermöglicht Lastenausgleich zwischen virtuellen Computern in einem Clouddienst oder in einem virtuellen Netzwerk mit regionalem Bereich. Informationen zu virtuellen Netzwerken mit regionalem Bereich finden Sie unter [Regionale virtuelle Netzwerke](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) im Azure-Blog. Vorhandene virtuelle Netzwerke, die für eine Affinitätsgruppe konfiguriert sind, können keinen ILB verwenden.

ILB ermöglicht die folgenden Arten des Lastenausgleichs:

* In einem Clouddienst: Lastenausgleich zwischen virtuellen Computern und einer Gruppe von virtuellen Computern, die sich im gleichen Clouddienst befinden. Dieses <a href="#figure1">Beispiel</a> zeigt dieses Szenario.
* In einem virtuellen Netzwerk: Lastenausgleich zwischen virtuellen Computern in einem virtuellen Netzwerk und einer Gruppe von virtuellen Computern im gleichen Clouddienst des virtuellen Netzwerks. Dieses <a href="#figure2">Beispiel</a> zeigt dieses Szenario.
* In einem standortübergreifenden virtuellen Netzwerk: Lastenausgleich zwischen lokalen Computern und einer Gruppe von virtuellen Computern im gleichen Clouddienst des virtuellen Netzwerks. Dieses <a href="#figure3">Beispiel</a> zeigt dieses Szenario.
* Für Anwendungen mit mehreren Ebenen: Lastenausgleich für Anwendungen mit mehreren Ebenen mit Internetzugriff, bei denen die Back-End-Ebenen nicht über Internetzugriff verfügen. Die Back-End-Ebenen erfordern einen Lastenausgleich des Datenverkehrs aus einer mit dem Internet verbundenen Ebene.
* Für Branchenanwendungen: Lastenausgleich für Branchenanwendungen, die in Azure ohne zusätzliche Hardware oder Software für den Lastenausgleich gehostet werden. Dieses Szenario enthält lokale Server, die sich in der Gruppe der Computer befinden, für deren Datenverkehr Lastenausgleich stattfindet.

## <a name="load-balancing-for-internet-facing-multi-tier-applications"></a>Lastenausgleich für Anwendungen mit mehreren Ebenen mit Internetanbindung

Die Webebene verfügt über Endpunkte mit Internetanbindung für Internetclients und ist Teil einer Gruppe mit Lastenausgleich. ILB verteilt den eingehenden Datenverkehr von Webclients für den TCP-Port 443 (HTTPS) an die Webserver.

Die Datenbankserver befinden sich hinter einem ILB-Endpunkt, den die Webserver zur Speicherung verwenden. Der ILB-Endpunkt ist ein Datenbankdienstendpunkt mit Lastenausgleich. Für den Datenverkehr wird datenbankübergreifender Lastenausgleich für die Datenbankserver in der ILB-Gruppe ausgeführt.

In der folgenden Abbildung wird Lastenausgleich für die Anwendung mit mehreren Ebenen und Internetzugriff im gleichen Clouddienst dargestellt.

<a name="figure1"></a>
![Anwendung mit mehreren Ebenen mit Internetanbindung](./media/load-balancer-internal-overview/IC736321.png)

Ein weiteres Szenario ist für Anwendungen mit mehreren Ebenen verfügbar. Der Lastenausgleich wird für einen anderen Clouddienst aus dem Clouddienst bereitgestellt, der den Dienst für den ILB nutzt.

Clouddienste, die das gleiche virtuelle Netzwerk verwenden, besitzen Zugriff auf den ILB-Endpunkt. Die folgende Abbildung zeigt Front-End-Webserver, die sich in einem anderen Clouddienst befinden als das Datenbank-Back-End. Die Front-End-Server verwenden den ILB-Endpunkt im gleichen virtuellen Netzwerk wie das Back-End.

<a name="figure2"></a>
![Front-End-Server in einem anderen Clouddienst](./media/load-balancer-internal-overview/IC744147.png)

## <a name="load-balancing-for-intranet-line-of-business-applications"></a>Lastenausgleich für Intranetbranchenanwendungen

Für Datenverkehr von Clients im lokalen Netzwerk wird in einer Gruppe von Branchenanwendungsservern, die eine VPN-Verbindung mit dem Azure-Netzwerk verwenden, Lastenausgleich ausgeführt.

Der Clientcomputer kann auf eine IP-Adresse aus dem Azure-VPN-Dienst über ein Point-to-Site-VPN zugreifen. Die Branchenanwendung kann hinter dem ILB-Endpunkt gehostet werden.

<a name="figure3"></a>
![Hinter dem ILB-Endpunkt gehostete Branchenanwendung](./media/load-balancer-internal-overview/IC744148.png)

Ein weiteres Szenario für Branchenanwendungen ist eine Site-to-Site-VPN-Verbindung mit dem virtuellen Netzwerk, in dem der ILB-Endpunkt konfiguriert ist. Lokaler Netzwerkdatenverkehr wird an den ILB-Endpunkt weitergeleitet.

<a name="figure4"></a>
![An den ILB-Endpunkt weitergeleiteter lokaler Netzwerkdatenverkehr](./media/load-balancer-internal-overview/IC744150.png)

## <a name="limitations"></a>Einschränkungen

Konfigurationen des ILB unterstützen SNAT nicht. In diesem Artikel bezieht sich SNAT auf Szenarien mit portmaskierender Übersetzung der Quellnetzwerkadressen. Ein virtueller Computer in einem Lastenausgleichspool muss die Front-End-IP-Adresse des entsprechenden internen Lastenausgleichs erreichen können. Es treten Verbindungsfehler auf, wenn ein Lastenausgleich für den Datenfluss auf dem virtuellen Computer erfolgt, von dem der Datenfluss ursprünglich stammt. Die folgenden Szenarien werden für ILB nicht unterstützt. Stattdessen muss ein Lastenausgleich im Proxystil verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* [Unterstützung von Azure Resource Manager für Azure Load Balancer](load-balancer-arm.md)
* [Erste Schritte zum Konfigurieren eines Lastenausgleichs mit Internetzugriff](load-balancer-get-started-internet-arm-ps.md)
* [Erste Schritte zum Konfigurieren eines internen Lastenausgleichs](load-balancer-get-started-ilb-arm-ps.md)
* [Konfigurieren des Verteilungsmodus für den Lastenausgleich](load-balancer-distribution-mode.md)
* [Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)
