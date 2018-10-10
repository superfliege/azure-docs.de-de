---
title: Virtuelles Netzwerk für Azure-Dienste | Microsoft-Dokumentation
description: Informieren Sie sich über die Vorteile der Bereitstellung von Ressourcen in einem virtuellen Netzwerk. Ressourcen in virtuellen Netzwerken können miteinander und mit lokalen Ressourcen kommunizieren, ohne das Aufkommen von Datenverkehr im Internet.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 06af3351f5669f5cd9aeeb9c4cb2168666476b52
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994631"
---
# <a name="virtual-network-integration-for-azure-services"></a>Integration virtueller Netzwerke für Azure-Dienste

Wenn Sie Azure-Dienste in ein virtuelles Azure-Netzwerk integrieren, ermöglichen Sie damit den privaten Zugriff auf den Dienst über virtuelle Computer oder Computeressourcen im virtuellen Netzwerk.
Zur Integration von Azure-Diensten in Ihr virtuelles Netzwerk gibt es folgende Optionen: die direkte Bereitstellung dedizierter Instanzen des Diensts in einem virtuellen Netzwerk. Auf die Dienste kann dann innerhalb des virtuellen Netzwerks und von lokalen Netzwerken aus privat zugegriffen werden.
Durch die Erweiterung eines virtuellen Netzwerks auf den Dienst über Dienstendpunkte. Durch Dienstendpunkte können einzelne Dienstressourcen im virtuellen Netzwerk gesichert werden.

Um mehrere Azure-Dienste in Ihr virtuelles Netzwerk zu integrieren, können Sie eines der oben genannten Muster verwenden oder mehrere Muster kombinieren. Sie können z.B. HDInsight im virtuellen Netzwerk bereitstellen und ein Speicherkonto über Dienstendpunkte im HDInsight-Subnetz sichern.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Bereitstellen von Azure-Diensten in virtuellen Netzwerken

Wenn Sie dedizierte Azure-Dienste in einem [virtuellen Netzwerk](virtual-networks-overview.md) bereitstellen, können Sie über private IP-Adressen privat mit den Dienstressourcen kommunizieren.

![In einem virtuellen Netzwerk bereitgestellte Dienste](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Das Bereitstellen von Diensten in einem virtuellen Netzwerk bietet die folgenden Fähigkeiten:

- Ressourcen im virtuellen Netzwerk können über private IP-Adressen privat miteinander kommunizieren. Beispiel: Direktes Übertragen von Daten zwischen HDInsight und SQL Server auf einem virtuellen Computer im virtuellen Netzwerk.
- Lokale Ressourcen können mit privaten IP-Adressen über [Site-to-Site-VPN (VPN Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) oder [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) auf Ressourcen in einem virtuellen Netzwerk zugreifen.
- Für virtuelle Netzwerke kann [Peering](virtual-network-peering-overview.md) eingerichtet werden, damit Ressourcen in den virtuellen Netzwerken mithilfe privater IP-Adressen miteinander kommunizieren können.
- Dienstinstanzen in einem virtuellen Netzwerk werden vollständig durch den Azure-Dienst verwaltet, um die Integrität der Instanzen zu überwachen und eine der Last entsprechende Skalierung bereitzustellen.
- Dienstinstanzen werden in einem Subnetz in einem virtuellen Netzwerk bereitgestellt. Eingehender und ausgehender Netzwerkzugriff müssen über [Netzwerksicherheitsgruppen](security-overview.md#network-security-groups) für das Subnetz geöffnet werden, gemäß der Anleitung durch die Dienste.
- Optional ist für Dienste möglicherweise ein [delegiertes Subnetz](virtual-network-manage-subnet.md#add-a-subnet) als expliziter Bezeichner erforderlich, damit ein Subnetz einen bestimmten Diensts hosten kann. Bei der Subnetzdelegierung erhält der Dienst explizite Berechtigungen, um dienstspezifische Ressourcen im Subnetz zu erstellen.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Dienste, die in einem virtuellen Netzwerk bereitgestellt werden können

Für jeden Dienst, der direkt in einem virtuellen Netzwerk bereitgestellt wird, gelten bestimmte Anforderungen für das Routing und die zulässigen Typen des eingehenden und ausgehenden Datenverkehrs der Subnetze. Die verschiedenen Dienste, die in einem virtuellen Netzwerk bereitgestellt werden können, werden unten in Kategorien aufgeführt. Wählen Sie den jeweiligen Dienst in der Tabelle aus, um weitere Informationen zum Dienst und zu dessen Integration in Ihrem virtuellen Netzwerk zu erhalten. 


|Category (Kategorie)|Dienst|
|-|-|
| Compute | Virtuelle Computer: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Skalierungsgruppen für virtuelle Computer](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Clouddienst](https://msdn.microsoft.com/library/azure/jj156091): Nur virtuelles Netzwerk (klassisch)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)  |
| Netzwerk | [Application Gateway – WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Firewall](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Virtuelle Netzwerkgeräte](/windowsserverdocs/WindowsServerDocs/networking/sdn/manage/Use-Network-Virtual-Appliances-on-a-VN.md) 
|Daten|[RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Verwaltete Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-vnet-configuration.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
Analytics | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Identity | [Azure Active Directory-Domänendienste](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Container | [Azure Kubernetes Service (AKS)](../aks/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Container Instances (ACI)](http://www.aka.ms/acivnet)<br/>[Azure Container Service-Engine](https://github.com/Azure/acs-engine) mit Azure Virtual Network-CNI-[Plug-In](https://github.com/Azure/acs-engine/tree/master/examples/vnet)||
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[App Service-Umgebung](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[WebApps](http://www.aka.ms/WebAppsVNet)
| Gehostet*| [Azure NetApp Files (ANF)](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Dediziertes Azure-Hardwaresicherheitsmodul](http://www.aka.ms/azurededicatedhsm)
| | |
<br/>
*Spezialisierte Dienste, die in der Regel auf speziell entwickelter Hardware ausgeführt werden.



## <a name="service-endpoints-for-azure-services"></a>Dienstendpunkte für Azure-Dienste

Einige Azure-Dienste können nicht in virtuellen Netzwerken bereitgestellt werden. Sie können ggf. den Zugriff auf einige der Dienstressourcen auf bestimmte Subnetze des virtuellen Netzwerks beschränken, indem Sie einen virtuellen Netzwerkdienstendpunkt aktivieren.  Erfahren Sie mehr über [Dienstendpunkte von virtuellen Netzwerken](virtual-network-service-endpoints-overview.md) und die Dienste, für die Dienstendpunkte aktiviert werden können.