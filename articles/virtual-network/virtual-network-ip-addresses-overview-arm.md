---
title: IP-Adresstypen in Azure | Microsoft-Dokumentation
description: "Enthält Informationen zu öffentlichen und privaten IP-Adressen in Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2017
ms.author: jdial
ms.openlocfilehash: 8ddd582ed159e10add896252c40feb19780c42fb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>IP-Adresstypen und Zuordnungsmethoden in Azure

Sie können Azure-Ressourcen IP-Adressen zuweisen, um die Kommunikation mit anderen Azure-Ressourcen, Ihrem lokalen Netzwerk und dem Internet zu ermöglichen. In Azure können zwei Arten von IP-Adressen verwendet werden.

* **Öffentliche IP-Adressen**: Sie werden für die Kommunikation mit dem Internet verwendet. Hierzu zählen auch öffentliche Azure-Dienste.
* **Private IP-Adressen**: Sie werden für die Kommunikation innerhalb eines virtuellen Azure-Netzwerks (VNet) und innerhalb Ihres lokalen Netzwerks verwendet, wenn Sie Ihr Netzwerk mithilfe eines VPN-Gateways oder einer ExpressRoute-Verbindung auf Azure ausdehnen.

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des [klassischen Bereitstellungsmodells](virtual-network-ip-addresses-overview-classic.md) empfohlen wird.
> 

Wenn Sie mit dem klassischen Bereitstellungsmodell vertraut sind, sollten Sie sich die [Unterschiede bei IP-Adressen zwischen dem klassischen Bereitstellungsmodell und dem Resource Manager-Bereitstellungsmodell](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) ansehen.

## <a name="public-ip-addresses"></a>Öffentliche IP-Adressen

Öffentliche IP-Adressen ermöglichen Azure-Ressourcen die Kommunikation mit dem Internet und öffentlichen Azure-Diensten wie [Azure Redis Cache](https://azure.microsoft.com/services/cache), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs), [SQL-Datenbanken](../sql-database/sql-database-technical-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Azure Storage](../storage/common/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

In Azure-Ressourcen-Manager ist eine [öffentliche IP-Adresse](virtual-network-public-ip-address.md) eine Ressource, die über eigene Eigenschaften verfügt. Sie können eine öffentliche IP-Adressressource beispielsweise einer der folgenden Ressourcen zuordnen:

* Netzwerkschnittstellen für virtuelle Computer
* Load Balancer mit Internetzugriff
* VPN-Gateways
* Anwendungsgateways

### <a name="ip-address-version"></a>IP-Adressversion

Öffentliche IP-Adressen werden mit einer IPv4- oder IPv6-Adresse erstellt. Öffentliche IPv6-Adressen können nur Lastenausgleichsmodulen mit Internetzugriff zugewiesen werden.

### <a name="sku"></a>SKU

Öffentliche IP-Adressen werden mit einer der folgenden SKUs erstellt:

#### <a name="basic"></a>Basic

Alle öffentlichen IP-Adressen, die vor der Einführung von SKUs erstellt wurden, sind öffentliche IP-Adressen vom Typ „Basic-SKU“. Nach der Einführung von SKUs können Sie nun angeben, welche SKU für die öffentliche IP-Adresse verwendet werden soll. Für Basic-SKU-Adressen gilt Folgendes:

- Sie werden mit der statischen oder der dynamischen Zuordnungsmethode zugewiesen.
- Die Zuweisung ist für jede Azure-Ressource möglich, der eine öffentliche IP-Adresse zugewiesen werden kann, z.B. Netzwerkschnittstellen, VPN Gateways, Application Gateways und Lastenausgleiche mit Internetzugriff.
- Sie können einer bestimmten Zone zugewiesen werden.
- Sie sind nicht zonenredundant. Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="standard"></a>Standard

Für öffentliche IP-Adressen vom Typ „Standard-SKU“ gilt Folgendes:

- Sie werden nur mit der statischen Zuordnungsmethode zugewiesen.
- Sie können Netzwerkschnittstellen oder Standard-Lastenausgleichsmodulen mit Internetzugriff zugewiesen werden. Weitere Informationen zu SKUs von Azure Load Balancer finden Sie unter [Azure load balancer standard SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Standard-SKU von Azure Load Balancer).
- Sie sind standardmäßig zonenredundant. Können in Zonen erstellt und in einer bestimmten Verfügbarkeitszone garantiert werden.  Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Overview of Availability Zones in Azure (Preview)](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Übersicht über Verfügbarkeitszonen in Azure (Vorschau)).
 
> [!NOTE]
> Wenn Sie eine öffentliche IP-Adresse für eine Standard-SKU der Netzwerkschnittstelle eines virtuellen Computers zuweisen, müssen Sie den geplanten Datenverkehr explizit mit einer [Netzwerksicherheitsgruppe](security-overview.md#network-security-groups) zulassen.  Die Kommunikation mit der Ressource schlägt fehl, bis Sie eine Netzwerksicherheitsgruppe erstellen und den gewünschten Datenverkehr explizit zulassen.

Die Standard-SKU befindet sich in der Vorschauversion. Vor dem Erstellen einer öffentlichen IP-Adresse vom Typ „Standard-SKU“ müssen Sie sich zuerst für die Vorschauversion registrieren und die Adresse an einem unterstützten Standort erstellen. Unter [Register for the standard SKU preview](virtual-network-public-ip-address.md#register-for-the-standard-sku-preview) (Registrieren für die Vorschauversion der Standard-SKU) können Sie sich für die Vorschauversion registrieren. Eine Liste mit den unterstützten Standorten (Regionen) finden Sie unter [Region availability](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) (Regionsverfügbarkeit). Auf der Seite [Azure Virtual Network updates](https://azure.microsoft.com/updates/?product=virtual-network) (Updates für Azure Virtual Network) erhalten Sie außerdem zusätzlichen Support für Regionen.


### <a name="allocation-method"></a>Zuordnungsmethode

Es gibt zwei Methoden zum Zuordnen einer IP-Adresse zu einer öffentlichen IP-Adressressource: *dynamisch* oder *statisch*. Die Standardmethode für die Zuordnung ist *dynamisch*. Bei diesem Verfahren wird eine IP-Adresse **nicht** zum Zeitpunkt ihrer Erstellung zugewiesen. Stattdessen wird die öffentliche IP-Adresse zugeordnet, wenn Sie die zugeordnete Ressource (z.B. einen virtuellen Computer oder einen Load Balancer) starten oder erstellen. Die IP-Adresse wird freigegeben, wenn Sie die Ressource beenden oder löschen. Nach dem Freigeben für Ressource A kann die IP-Adresse beispielsweise einer anderen Ressource zugewiesen werden. Wenn die IP-Adresse einer anderen Ressource zugewiesen wird, während Ressource A angehalten ist, erhält Ressource A eine andere IP-Adresse, wenn Sie sie neu starten.

Damit die IP-Adresse für die zugeordnete Ressource unverändert bleibt, können Sie die Zuordnungsmethode explizit auf *statisch*festlegen. Eine statische IP-Adresse wird sofort zugewiesen. Die Adresse wird nur freigegeben, wenn Sie die Ressource löschen oder deren Zuordnungsmethode in *dynamisch* ändern.

> [!NOTE]
> Auch wenn Sie die Zuordnungsmethode auf *statisch* festlegen, können Sie nicht die tatsächliche IP-Adresse angeben, die der öffentlichen IP-Adressressource zugewiesen ist. Azure weist die IP-Adresse aus einem Pool mit verfügbaren IP-Adressen an dem Azure-Standort zu, an dem die Ressource erstellt wird.
>

Statische öffentliche IP-Adressen werden häufig in den folgenden Szenarien verwendet:

* Sie müssen Firewallregeln für die Kommunikation mit Ihren Azure-Ressourcen aktualisieren.
* Bei der DNS-Namensauflösung erfordert eine Änderung einer IP-Adresse eine Aktualisierung von A-Datensätzen.
* Ihre Azure-Ressourcen kommunizieren mit anderen Apps oder Diensten, die ein auf IP-Adressen basierendes Sicherheitsmodell verwenden.
* Sie verwenden SSL-Zertifikate, die mit einer IP-Adresse verknüpft sind.

> [!NOTE]
> Azure ordnet öffentliche IP-Adressen aus einem Bereich zu, der für jede Azure-Region eindeutig ist. Ausführliche Informationen finden Sie unter [Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653) (IP-Bereiche für Azure-Datencenter).
>

### <a name="dns-hostname-resolution"></a>DNS-Hostnamenauflösung
Sie können eine DNS-Domänennamensbezeichnung für eine öffentliche IP-Ressource angeben. Dadurch erstellen Sie für „*Domänennamensbezeichnung*.*Standort*.cloudapp.azure.com“ eine Zuordnung zur öffentlichen IP-Adresse auf den von Azure verwalteten DNS-Servern. Wenn Sie beispielsweise eine öffentliche IP-Ressource mit **contoso** als *Domänennamensbezeichnung* am Azure-*Standort* **USA, Westen** erstellen, wird der vollqualifizierte Domänenname (FQDN) **contoso.westus.cloudapp.azure.com** in die öffentliche IP-Adresse der Ressource aufgelöst. Mit dem FQDN können Sie einen benutzerdefinierten CNAME-Domäneneintrag mit Verweis auf die öffentliche IP-Adresse in Azure erstellen.

> [!IMPORTANT]
> Jede erstellte Domänennamensbezeichnung muss innerhalb des Azure-Standorts eindeutig sein.  
>

### <a name="virtual-machines"></a>Virtuelle Computer

Sie können eine öffentliche IP-Adresse einem virtuellen [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computer zuordnen, indem Sie sie dessen **Netzwerkschnittstelle** zuweisen. Sie können einem virtuellen Computer eine dynamische oder eine statische öffentliche IP-Adresse zuweisen. Informieren Sie sich über das [Zuweisen von IP-Adressen zu Netzwerkschnittstellen](virtual-network-network-interface-addresses.md).

### <a name="internet-facing-load-balancers"></a>Load Balancer mit Internetzugriff

Sie können eine öffentliche IP-Adresse, die mit einer der [SKUs](#SKU) erstellt wurde, einem [Azure Load Balancer](../load-balancer/load-balancer-overview.md) zuordnen, indem Sie sie der **Front-End**-Konfiguration des Load Balancers zuweisen. Die öffentliche IP-Adresse fungiert als virtuelle IP-Adresse (VIP) mit Lastenausgleich. Sie können einem Load Balancer-Front-End eine dynamische oder eine statische öffentliche IP-Adresse zuweisen. Sie können einem Load Balancer-Front-End auch mehrere öffentliche IP-Adressen zuweisen. Dadurch werden Szenarien mit [mehreren VIPs](../load-balancer/load-balancer-multivip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ermöglicht, z.B. mehrinstanzenfähige Umgebungen mit SSL-basierten Websites. Weitere Informationen zu SKUs von Azure Load Balancer finden Sie unter [Azure load balancer standard SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Standard-SKU von Azure Load Balancer).

### <a name="vpn-gateways"></a>VPN-Gateways

Ein [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) stellt für ein virtuelles Azure-Netzwerk eine Verbindung mit anderen virtuellen Azure-Netzwerken oder einem lokalen Netzwerk her. Eine öffentliche IP-Adresse wird dem VPN Gateway zugewiesen, um dafür die Kommunikation mit dem Remotenetzwerk zu ermöglichen. Sie können einem VPN Gateway nur eine *dynamische* öffentliche IP-Adresse zuweisen.

### <a name="application-gateways"></a>Anwendungsgateways

Sie können eine öffentliche IP-Adresse einem Azure [Application Gateway](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)zuordnen, indem Sie sie der **Front-End** -Konfiguration des Gateways zuweisen. Diese öffentliche IP-Adresse fungiert als VIP (virtuelle IP-Adresse) mit Lastenausgleich. Sie können der Front-End-Konfiguration eines Anwendungsgateways nur eine *dynamische* öffentliche IP-Adresse zuweisen.

### <a name="at-a-glance"></a>Auf einen Blick
In der Tabelle unten sind die spezifischen Eigenschaften, über die eine öffentliche IP-Adresse einer Ressource der obersten Ebene zugeordnet sein kann, und die möglichen Zuweisungsverfahren (dynamisch oder statisch) angegeben.

| Ressource der obersten Ebene | Zuordnung der IP-Adresse | dynamisch | statisch |
| --- | --- | --- | --- |
| Virtueller Computer |Netzwerkschnittstelle |Ja |Ja |
| Lastenausgleich mit Internetzugriff |Front-End-Konfiguration |Ja |Ja |
| VPN Gateway |Gateway-IP-Konfiguration |Ja |Nein |
| Application Gateway |Front-End-Konfiguration |Ja |Nein |

## <a name="private-ip-addresses"></a>Private IP-Adressen
Private IP-Adressen ermöglichen Azure-Ressourcen die Kommunikation mit anderen Ressourcen in einem [virtuellen Netzwerk](virtual-networks-overview.md) oder in einem lokalen Netzwerk über ein VPN-Gateway oder eine ExpressRoute-Verbindung, ohne dass dabei eine über das Internet erreichbare IP-Adresse verwendet wird.

Im Azure Resource Manager-Bereitstellungsmodell wird den folgenden Typen von Azure-Ressourcen eine private IP-Adresse zugeordnet.

* Netzwerkschnittstellen für virtuelle Computer
* Interne·Load Balancer (ILBs)
* Anwendungsgateways

### <a name="ip-address-version"></a>IP-Adressversion

Private IP-Adressen werden mit einer IPv4- oder IPv6-Adresse erstellt. Privaten IPv6-Adressen kann nur die dynamische Zuordnungsmethode zugewiesen werden. Die Kommunikation zwischen privaten IPv6-Adressen ist in einem virtuellen Netzwerk nicht möglich. Sie können aus dem Internet in eingehender Richtung mit einer privaten IPv6-Adresse über einen Lastenausgleich mit Internetzugriff kommunizieren. Ausführliche Informationen finden Sie unter [Erstellen eines Lastenausgleichs für den Internetzugriff mit IPv6 – Azure CLI](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="allocation-method"></a>Zuordnungsmethode

Eine private IP-Adresse wird aus dem Adressbereich des Subnetzes zugeordnet, dem die Ressource hinzugefügt wurde. Der Adressbereich des Subnetzes selbst ist ein Teil des Adressbereichs eines virtuellen Netzwerks.

Es gibt zwei Methoden zum Zuordnen von privaten IP-Adressen: *dynamisch* oder *statisch*. Die Standardmethode ist die *dynamische* Zuordnung. Dabei wird die IP-Adresse aus dem Subnetz der Ressource per DHCP automatisch zugewiesen. Wenn Sie die Ressource beenden und wieder starten, kann sich diese IP-Adresse ändern.

Sie können die Zuordnungsmethode auf *statisch* festlegen, damit die IP-Adresse unverändert bleibt. Beim Angeben von *statisch* geben Sie eine gültige IP-Adresse an, die Teil des Ressourcensubnetzes ist.

Statische private IP-Adressen werden häufig für Folgendes verwendet:

* Virtuelle Computer, die als Domänencontroller oder DNS-Server fungieren.
* Ressourcen, die Firewallregeln mit IP-Adressen erfordern
* Ressourcen, auf die von anderen Apps oder Ressourcen über eine IP-Adresse zugegriffen wird.

### <a name="virtual-machines"></a>Virtuelle Computer

Eine private IP-Adresse wird der **Netzwerkschnittstelle** eines virtuellen [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computers zugewiesen. Wenn der virtuelle Computer über mehrere Netzwerkschnittstellen verfügt, wird jeder Netzwerkschnittstelle eine private IP-Adresse zugewiesen. Für eine Netzwerkschnittstelle können Sie als Zuordnungsmethode „dynamisch“ oder „statisch“ angeben.

#### <a name="internal-dns-hostname-resolution-for-virtual-machines"></a>Interne DNS-Hostnamensauflösung (für virtuelle Computer)

Alle virtuellen Azure-Computer werden standardmäßig mit [von Azure verwalteten DNS-Servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) konfiguriert, sofern nicht explizit benutzerdefinierte DNS-Server konfiguriert werden. Diese DNS-Server stellen die interne Namensauflösung für virtuelle Computer in demselben virtuellen Netzwerk bereit.

Beim Erstellen eines virtuellen Computers wird den von Azure verwalteten DNS-Servern eine Hostnamenzuordnung für die private IP-Adresse hinzugefügt. Bei einem virtuellen Computer mit mehreren Netzwerkschnittstellen wird der Hostname der privaten IP-Adresse der primären Netzwerkschnittstelle zugeordnet.

Virtuelle Computer, die mit von Azure verwalteten DNS-Servern konfiguriert sind, können die Hostnamen aller virtuellen Computer in demselben virtuellen Netzwerk in ihre privaten IP-Adressen auflösen.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interner Lastenausgleich (Internal Load Balancer, ILB) und Anwendungsgateways

Sie können der **Front-End**-Konfiguration eines [Azure-ILBs](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder eines [Azure Application Gateways](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eine private IP-Adresse zuweisen. Diese private IP-Adresse fungiert als interner Endpunkt und steht nur den Ressourcen innerhalb des entsprechenden virtuellen Netzwerks und der damit verbundenen Remotenetzwerke zur Verfügung. Der Front-End-Konfiguration kann eine dynamische oder eine statische private IP-Adresse zugewiesen werden.

### <a name="at-a-glance"></a>Auf einen Blick
In der Tabelle unten sind die spezifischen Eigenschaften, über die eine private IP-Adresse einer Ressource der obersten Ebene zugeordnet sein kann, und die möglichen Zuweisungsverfahren (dynamisch oder statisch) angegeben.

| Ressource der obersten Ebene | Zuordnung der IP-Adresse | dynamisch | statisch |
| --- | --- | --- | --- |
| Virtueller Computer |Netzwerkschnittstelle |Ja |Ja |
| Load Balancer |Front-End-Konfiguration |Ja |Ja |
| Application Gateway |Front-End-Konfiguration |Ja |Ja |

## <a name="limits"></a>Grenzen
Die Grenzwerte für die IP-Adressierung finden Sie in den vollständigen Informationen zu [Grenzwerten für Netzwerke](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) in Azure. Die Grenzwerte gelten pro Region und pro Abonnement. Sie können sich [an den Support wenden](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , um die Standardgrenzwerte je nach Ihren Unternehmensanforderungen bis auf die maximalen Grenzwerte zu erhöhen.

## <a name="pricing"></a>Preise
Für öffentliche IP-Adressen wird unter Umständen eine geringe Gebühr berechnet. Weitere Informationen zu den Preisen für IP-Adressen in Azure finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen einer VM mit einer statischen öffentlichen IP-Adresse mithilfe des Azure-Portals](virtual-network-deploy-static-pip-arm-portal.md)
* [Bereitstellen einer VM mit einer statischen öffentlichen IP-Adresse über eine Vorlage](virtual-network-deploy-static-pip-arm-template.md)
* [Bereitstellen eines virtuellen Computers mit einer statischen privaten IP-Adresse mithilfe des Azure-Portals](virtual-networks-static-private-ip-arm-pportal.md)
