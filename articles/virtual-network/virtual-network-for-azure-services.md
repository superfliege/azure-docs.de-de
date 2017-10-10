---
title: "Virtuelles Netzwerk für Azure-Dienste | Microsoft-Dokumentation"
description: "Informieren Sie sich über die Vorteile der Bereitstellung von Ressourcen in einem virtuellen Netzwerk. Ressourcen in virtuellen Netzwerken können miteinander und mit lokalen Ressourcen kommunizieren, ohne das Aufkommen von Datenverkehr im Internet."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 8ad98f7ef226fa94b75a8fc6b2885e7f0870483c
ms.openlocfilehash: 2c3ffb8432fae41b376cc71bb600a0b1c490f345
ms.contentlocale: de-de
ms.lasthandoff: 09/29/2017

---

# <a name="virtual-network-integration-for-azure-services"></a>Integration virtueller Netzwerke für Azure-Dienste

Die Integration von Azure-Diensten in ein virtuelles Azure-Netzwerk ermöglicht den privaten Zugriff von Instanzen eines im virtuellen Netzwerk bereitgestellten Diensts.

Sie können Azure-Dienste mit den folgenden Optionen ins virtuelle Netzwerk integrieren:
- Direkte Bereitstellung dedizierter Instanzen des Diensts in einem virtuellen Netzwerk. Auf die dedizierten Instanzen dieser Dienste kann innerhalb des virtuellen Netzwerks und von lokalen Netzwerken privat zugegriffen werden.
- Durch die Erweiterung eines virtuellen Netzwerks auf den Dienst über Dienstendpunkte. Durch Dienstendpunkte können einzelne Dienstressourcen im virtuellen Netzwerk gesichert werden.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Bereitstellen von Azure-Diensten in virtuellen Netzwerken

Sie können mit den meisten Azure-Ressourcen über öffentliche IP-Adressen im Internet kommunizieren. Wenn Sie Azure-Dienste in einem [virtuellen Netzwerk](virtual-networks-overview.md) bereitstellen, können Sie über private IP-Adressen privat mit den Dienstressourcen kommunizieren.

![In einem virtuellen Netzwerk bereitgestellte Dienste](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Das Bereitstellen von Diensten in einem virtuellen Netzwerk bietet die folgenden Fähigkeiten:

- Ressourcen im virtuellen Netzwerk können über private IP-Adressen privat miteinander kommunizieren. Beispiel: Direktes Übertragen von Daten zwischen HDInsight und SQL Server auf einem virtuellen Computer im virtuellen Netzwerk.
- Lokale Ressourcen können mit privaten IP-Adressen über [Site-to-Site-VPN (VPN Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) oder [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) auf Ressourcen in einem virtuellen Netzwerk zugreifen.
- Für virtuelle Netzwerke kann [Peering](virtual-network-peering-overview.md) eingerichtet werden, damit Ressourcen in den virtuellen Netzwerken mithilfe privater IP-Adressen miteinander kommunizieren können.
- Dienstinstanzen in einem virtuellen Netzwerk werden vollständig durch den Azure-Dienst verwaltet, um die Integrität der Instanzen zu überwachen und eine der Last entsprechende Skalierung bereitzustellen.
- Dienstinstanzen werden in einem dedizierten Subnetz in einem virtuellen Netzwerk bereitgestellt. Eingehender und ausgehender Netzwerkzugriff müssen über [Netzwerksicherheitsgruppen](security-overview.md#network-security-groups) für das Subnetz geöffnet werden, gemäß der Anleitung durch die Dienste.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Dienste, die in einem virtuellen Netzwerk bereitgestellt werden können

Für jeden Dienst, der direkt in einem virtuellen Netzwerk bereitgestellt wird, gelten bestimmte Anforderungen für das Routing und die zulässigen Typen des eingehenden und ausgehenden Datenverkehrs der Subnetze. Weitere Informationen finden Sie unter: 
 
- Virtuelle Computer: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Service Fabric](../service-fabric/service-fabric-patterns-networking.md?toc=%2fazure%2fvirtual-network%2ftoc.json#existingvnet)
- [Skalierungsgruppen für virtuelle Computer](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [App Service-Umgebung](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Application Gateway (intern)](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Container Service-Modul](../container-service/container-service-intro.md?toc=%2fazure%2fvirtual-network%2ftoc.json): Der Azure Container Service erstellt ein virtuelles Standardnetzwerk. Sie können ein benutzerdefiniertes virtuelles Netzwerk erstellen, das mit dem [Azure Container Service-Modul](https://github.com/Azure/acs-engine/tree/master/examples/vnet) verwendet werden soll.
- [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json): Nur virtuelles Netzwerk (klassisch)
- [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration): Nur virtuelles Netzwerk (klassisch)
- [Clouddienste](https://msdn.microsoft.com/library/azure/jj156091): Nur virtuelles Netzwerk (klassisch)

Sie können für den Lastenausgleich vieler der in der Liste oben aufgeführten Ressourcen einen [internen Azure-Lastenausgleich](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bereitstellen. In einigen Fällen wird vom Dienst automatisch ein Lastenausgleich erstellt und bereitgestellt, wenn Sie eine Ressource erstellen.

## <a name="service-endpoints-for-azure-services"></a>Dienstendpunkte für Azure-Dienste

Einige Azure-Dienste können nicht in virtuellen Netzwerken bereitgestellt werden. Sie können ggf. den Zugriff auf einige der Dienstressourcen auf bestimmte Subnetze des virtuellen Netzwerks beschränken, indem Sie einen virtuellen Netzwerkdienstendpunkt aktivieren. Informieren Sie sich über [Dienstendpunkte von virtuellen Netzwerken](virtual-network-service-endpoints-overview.md).

Derzeit werden Dienstendpunkte für die folgenden Dienste unterstützt: 
- **Azure Storage:** [Konfigurieren von Firewalls und virtuellen Netzwerken in Azure Storage (Vorschau)](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- **Azure SQL-Datenbank:** [Verwenden von Dienstendpunkten und Regeln eines virtuellen Netzwerks für Azure SQL-Datenbank](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="virtual-network-integration-across-multiple-azure-services"></a>Integration virtueller Netzwerke für mehrere Azure-Dienste

Sie können einen Azure-Dienst in einem Subnetz eines virtuellen Netzwerks bereitstellen und wichtige Dienstressourcen in diesem Subnetz sichern. Sie können z.B. HDInsight im virtuellen Netzwerk bereitstellen und ein Speicherkonto im HDInsight-Subnetz sichern.






