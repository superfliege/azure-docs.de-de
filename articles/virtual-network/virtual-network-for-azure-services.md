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
ms.date: 08/25/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 36ff77a4205132d1dcc3b743f11791fc5e98e9e7
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="virtual-network-integration-for-azure-services"></a>Integration virtueller Netzwerke für Azure-Dienste

Die Integration von Azure-Diensten in einem Azure Virtual Network (VNet) ermöglicht den privaten Zugriff auf die Dienste durch Instanzen, die in diesem VNet bereitgestellt wurden.

Sie können Azure-Dienste mit den folgenden Optionen im virtuellen Netzwerk integrieren:
- Durch die direkte Bereitstellung dedizierter Instanzen des Diensts in einem VNet. Auf die dedizierten Instanzen dieser Dienste kann innerhalb des virtuellen Netzwerks und lokal privat zugegriffen werden.
- Durch die Erweiterung eines virtuellen Netzwerks auf den Dienst über Dienstendpunkte. Dadurch können die einzelnen Dienstressourcen im virtuellen Netzwerk gesichert werden.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Bereitstellen von Azure-Diensten in virtuellen Netzwerken

Sie können mit den meisten Azure-Ressourcen über öffentliche IP-Adressen im Internet kommunizieren. Wenn Sie Azure-Dienste in einem [virtuellen Netzwerk](virtual-networks-overview.md) bereitstellen, können Sie über private IP-Adressen privat mit den Dienstressourcen kommunizieren.


![In einem virtuellen Netzwerk bereitgestellte Dienste](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Das Bereitstellen von Diensten in einem virtuellen Netzwerk bietet die folgenden Fähigkeiten:

- Ressourcen im virtuellen Netzwerk können über private IP-Adressen privat miteinander kommunizieren. Beispiel: Direktes Übertragen von Daten zwischen HDInsight und SQL Server auf einem virtuellen Computer im VNet.
- Lokale Ressourcen können mit privaten IP-Adressen über [Site-to-Site-VPN (VPN Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) oder [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) auf Ressourcen in einem virtuellen Netzwerk zugreifen.
- Für virtuelle Netzwerke kann [Peering](virtual-network-peering-overview.md) eingerichtet werden, damit Ressourcen in den virtuellen Netzwerken mithilfe privater IP-Adressen miteinander kommunizieren können.
- Dienstinstanzen in einem VNet werden vollständig durch den Azure-Dienst verwaltet, um die Integrität der Instanzen zu überwachen und eine der Last entsprechende Skalierung bereitzustellen.
- Dienstinstanzen werden in einem dedizierten Subnetz im VNet des Kunden bereitgestellt. Eingehender und ausgehender Zugriff sollten über Netzwerksicherheitsgruppen (NSGs) für das Subnetz geöffnet werden, gemäß der Anleitung durch die Dienste.


### <a name="list-of-services-that-can-be-deployed-into-a-virtual-network"></a>Liste der Dienste, die in einem virtuellen Netzwerk bereitgestellt werden können

Für jeden Dienst, der direkt in einem virtuellen Netzwerk bereitgestellt wird, gelten bestimmte Anforderungen für das Routing und die zulässigen Typen des eingehenden und ausgehenden Datenverkehrs der Subnetze. Weitere Informationen finden Sie über die folgenden Links: 
 
- Virtuelle Computer: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Service Fabric](../service-fabric/service-fabric-patterns-networking.md?toc=%2fazure%2fvirtual-network%2ftoc.json#existingvnet)
- [Skalierungsgruppen für virtuelle Computer](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [App Service-Umgebung](../app-service-web/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Application Gateway (intern)](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Container Service-Modul](../container-service/container-service-intro.md?toc=%2fazure%2fvirtual-network%2ftoc.json): Der Azure Container Service erstellt ein virtuelles Standardnetzwerk. Sie können ein benutzerdefiniertes virtuelles Netzwerk erstellen, das mit dem [Azure Container Service-Modul](https://github.com/Azure/acs-engine/tree/master/examples/vnet) verwendet werden soll.
- [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json): Nur virtuelles Netzwerk (klassisch)
- [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration): Nur virtuelles Netzwerk (klassisch)
- [Clouddienste](https://msdn.microsoft.com/library/azure/jj156091): Nur virtuelles Netzwerk (klassisch)

Sie können für den Lastenausgleich vieler der in der Liste oben aufgeführten Ressourcen einen [internen Azure Load Balancer](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bereitstellen. In einigen Fällen wird vom Dienst automatisch ein Load Balancer erstellt und bereitgestellt, wenn Sie eine Ressource erstellen.

## <a name="service-endpoints-for-azure-services"></a>Dienstendpunkte für Azure-Dienste

Einige Azure-Dienste können nicht in virtuellen Netzwerken bereitgestellt werden. Sie können ggf. den Zugriff auf einige der Dienstressourcen auf bestimmte Subnetze des virtuellen Netzwerks beschränken, indem Sie einen virtuellen Netzwerkdienstendpunkt aktivieren. Informieren Sie sich über [Dienstendpunkte von virtuellen Netzwerken](virtual-network-service-endpoints-overview.md).

Derzeit werden Dienstendpunkte für die folgenden Dienste unterstützt: 
- Azure Storage: [Securing Azure Storage accounts to Virtual Networks](https://docs.microsoft.com/azure/storage/common/storage-network-security) (Schützen von Azure Storage-Konten in virtuellen Netzwerken, in englischer Sprache)

- Azure SQL-Datenbank: [Securing Azure SQL Database to Virtual networks](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview) (Schützen von Azure SQL-Datenbank in virtuellen Netzwerken, in englischer Sprache)

## <a name="virtual-network-integration-across-multiple-azure-services"></a>Integration virtueller Netzwerke für mehrere Azure-Dienste

Sie können einen Azure-Dienst in einem Subnetz eines virtuellen Netzwerks bereitstellen und wichtige Dienstressourcen in diesem Subnetz sichern. 

Sie können z.B. HDInsight im virtuellen Netzwerk bereitstellen und ein Speicherkonto im HDInsight-Subnetz sichern.






