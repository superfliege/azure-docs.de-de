---
title: Azure Virtual WAN – Übersicht | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die per Virtual WAN automatisierte skalierbare Konnektivität zwischen Branches einrichten.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 09/25/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 54d177d7542d7501a91a955be20af776b16657a2
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182364"
---
# <a name="what-is-azure-virtual-wan"></a>Was ist Azure Virtual WAN?

Der Netzwerkdienst Azure Virtual WAN bietet optimierte und automatisierte Konnektivität zwischen Branches über Azure. Per Virtual WAN können Sie Branchgeräte für die Kommunikation mit Azure verbinden und konfigurieren. Dies ist entweder manuell oder durch die Nutzung von Geräten eines bevorzugten Partners über einen Virtual WAN-Partner möglich. Weitere Informationen finden Sie im Artikel [Bevorzugte Partner](https://go.microsoft.com/fwlink/p/?linkid=2019615). Die Nutzung von Geräten eines bevorzugten Partners ermöglicht eine einfache Verwendung, die Vereinfachung der Verbindungsherstellung und die Konfigurationsverwaltung. Über das integrierte Azure WAN-Dashboard können Sie anhand der Problembehandlung sofort Erkenntnisse gewinnen, um Zeit zu sparen, und auf einfache Weise umfangreiche Verbindungen anzeigen.

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwan.png)

Dieser Artikel enthält eine kurze Übersicht über die Netzwerkkonnektivität Ihrer Workloads innerhalb und außerhalb von Azure. Virtual WAN hat die folgenden Vorteile:

* **Integrierte Konnektivitätslösungen (Hub & Spoke):** Automatisieren Sie die Site-to-Site-Konfiguration und -Konnektivität zwischen lokalen Standorten und einem Azure-Hub.
* **Automatisierte Spoke-Einrichtung und -Konfiguration:** Verbinden Sie Ihre virtuellen Netzwerke und Workloads nahtlos mit dem Azure-Hub.
* **Intuitive Problembehandlung:** Sie können den gesamten Datenfluss in Azure anzeigen und diese Informationen nutzen, um erforderliche Aktionen durchzuführen.

## <a name="s2s"></a>Site-to-Site-Verbindungen

Zum Erstellen einer Site-to-Site-Verbindung mit Virtual WAN können Sie entweder einen [Virtual WAN-Partner](https://go.microsoft.com/fwlink/p/?linkid=2019615) nutzen oder die Verbindung manuell erstellen.

### <a name="s2spartner"></a>Nutzen eines Virtual WAN-Partners

Wenn Sie mit einem Virtual WAN-Partner arbeiten, ist der Prozess wie folgt:

1. Der Controller des Zweigstellengeräts (VPN/SDWAN) wird authentifiziert, um standortbezogene Informationen mithilfe eines [Azure-Dienstprinzipals](../azure-resource-manager/resource-group-create-service-principal-portal.md) zu exportieren.
2. Der Controller des Branchgeräts (VPN/SDWAN) ruft die Azure-Konnektivitätskonfiguration ab und aktualisiert das lokale Gerät. So werden für das lokale VPN-Gerät der Konfigurationsdownload und die Bearbeitung und die Aktualisierung automatisiert.
3. Nachdem das Gerät über die richtige Azure-Konfiguration verfügt, wird eine Site-to-Site-Verbindung (zwei aktive Tunnel) mit dem Azure WAN eingerichtet. Azure unterstützt IKEv1 und IKEv2. BGP ist optional.


Wenn Sie keinen bevorzugten Partner verwenden möchten, können Sie die Verbindung manuell konfigurieren. Siehe [Erstellen einer Site-to-Site-Verbindung per Virtual WAN](virtual-wan-site-to-site-portal.md).

## <a name="p2s"></a>Point-to-Site-Verbindungen (Vorschau)

Mit einer P2S-Verbindung (Point-to-Site) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Hub herstellen. Eine P2S-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten. Diese Lösung ist nützlich für Telearbeiter, die an einem Remotestandort (beispielsweise zu Hause oder in einer Konferenz) eine Verbindung herstellen möchten. Wenn nur einige wenige Clients eine Verbindung herstellen müssen, ist ein P2S-VPN (und nicht ein S2S-VPN) ebenfalls eine nützliche Lösung.

Informationen zur manuellen Erstellung der Verbindung finden Sie unter [Erstellen einer Point-to-Site-Verbindung per Virtual WAN](https://go.microsoft.com/fwlink/p/?linkid=2020051&clcid).

## <a name="er"></a>ExpressRoute-Verbindungen (Vorschau)

Informationen zur manuellen Erstellung der Verbindung finden Sie unter [Erstellen einer ExpressRoute-Verbindung per Virtual WAN](https://go.microsoft.com/fwlink/p/?linkid=2020148&clcid).


## <a name="resources"></a>Virtual WAN-Ressourcen

Sie erstellen die folgenden Ressourcen, um ein virtuelles End-to-End-WAN zu konfigurieren:

* **virtualWAN:** Die Ressource „virtualWAN“ stellt eine virtuelle Überlagerung Ihres Azure-Netzwerks dar und umfasst eine Sammlung mit mehreren Ressourcen. Sie enthält Links zu allen virtuellen Hubs, die Teil des virtuellen WAN sein sollen. Virtual WAN-Ressourcen sind voneinander isoliert und können keinen gemeinsamen Hub enthalten. Virtuelle Hubs kommunizieren per Virtual WAN nicht miteinander. Die Eigenschaft „AllowBranchToBranchTraffic“ ermöglicht Datenverkehr zwischen VPN-Standorten sowie VPN zu ExpressRoute-fähigen Standorten. Berücksichtigen Sie, dass ExpressRoute in Azure Virtual WAN sich derzeit in der Vorschauphase befindet.

* **Site:** Die Ressource „Site“, die als „vpnsite“ bezeichnet wird, steht für Ihr lokales VPN-Gerät und die zugehörigen Einstellungen. Durch die Zusammenarbeit mit einem Virtual WAN-Partner verfügen Sie über eine integrierte Lösung zum automatischen Exportieren dieser Informationen nach Azure.

* **Hub:** Ein virtueller Hub ist ein von Microsoft verwaltetes virtuelles Netzwerk. Der Hub enthält verschiedene Dienstendpunkte zum Aktivieren der Konnektivität über Ihr lokales Netzwerk (vpnsite). Der Hub ist der Kern Ihres Netzwerks in einer Region. Es kann nur ein Hub pro Azure-Region vorhanden sein. Wenn Sie einen Hub im Azure-Portal erstellen, werden für den virtuellen Hub ein VNET und ein VPN-Gateway erstellt.

  Ein Hub-Gateway ist nicht das gleiche wie ein Gateway für virtuelle Netzwerke, das Sie für ExpressRoute und VPN Gateway verwenden. Bei der Verwendung von Virtual WAN erstellen Sie beispielsweise von Ihrer lokalen Site aus keine direkte Site-to-Site-Verbindung mit Ihrem VNET. Stattdessen erstellen Sie eine Site-to-Site-Verbindung mit dem Hub. Der Datenverkehr verläuft immer über das Hub-Gateway. Dies bedeutet, dass Ihre VNETs kein eigenes Gateway für virtuelle Netzwerke benötigen. Mit Virtual WAN können Sie für Ihre VNETs über den virtuellen Hub und das virtuelle Hub-Gateway leicht eine Skalierung durchführen. 

* **Virtuelle Netzwerkverbindung für Hub:** Die Ressource für die virtuelle Netzwerkverbindung für den Hub wird verwendet, um den Hub nahtlos mit Ihrem virtuellen Netzwerk zu verbinden. Derzeit können Sie nur eine Verbindung mit virtuellen Netzwerken herstellen, die sich in derselben Hub-Region befinden.

* **Hubroutingtabelle:** Sie können eine virtuelle Hubroute erstellen und die Route der Routingtabelle des virtuellen Hubs zuweisen. Sie können der Routingtabelle des virtuellen Hubs mehrere Routen zuweisen.

## <a name="faq"></a>Häufig gestellte Fragen

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]


## <a name="next-steps"></a>Nächste Schritte

* Siehe [Virtual WAN – Partner und Standorte](https://aka.ms/virtualwan)