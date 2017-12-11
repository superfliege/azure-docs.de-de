---
title: "VPN-Gatewayeinstellungen für Azure Stack | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zu Einstellungen für VPN-Gateways, die Sie mit Azure Stack verwenden."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: brenduns
ms.openlocfilehash: ed4a84965c37f66bbc7734f6043ad6f8f1666c1f
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>VPN-Gatewaykonfigurationseinstellungen für Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Ein VPN-Gateway ist eine Art von Gateway für virtuelle Netzwerke, mit dem verschlüsselter Datenverkehr zwischen Ihrem virtuellen Netzwerk in Azure Stack und einem Remote-VPN-Gateway gesendet wird. Das Remote-VPN-Gateway kann sich in Azure befinden, es kann sich dabei aber auch um ein Gerät in Ihrem Datencenter oder um ein Gerät an einem anderen Standort handeln.  Falls die beiden Endpunkte über Netzwerkkonnektivität verfügen, können Sie zwischen den beiden Netzwerken eine sichere S2S-VPN-Verbindung (Site-to-Site) herstellen.

Für eine VPN Gateway-Verbindung müssen mehrere Ressourcen konfiguriert werden, die jeweils konfigurierbare Einstellungen enthalten. In den Abschnitten in diesem Artikel werden die Ressourcen und Einstellungen beschrieben, die sich auf ein im Resource Manager-Bereitstellungsmodell erstelltes VPN-Gateway beziehen. Beschreibungen und Topologiediagramme für die einzelnen Verbindungslösungen finden Sie unter [About VPN gateway for Azure Stack](azure-stack-vpn-gateway-about-vpn-gateways.md) (Informationen zum VPN-Gateway für Azure Stack).

## <a name="vpn-gateway-settings"></a>Einstellungen von VPN-Gateways

### <a name="gateway-types"></a>Gatewaytypen
Jedes virtuelle Azure Stack-Netzwerk unterstützt ein einzelnes Gateway für virtuelle Netzwerke, das vom Typ **VPN** sein muss.  Im Gegensatz dazu unterstützt Azure zusätzliche Typen.  

Beim Erstellen eines Gateways des virtuellen Netzwerks müssen Sie sicherstellen, dass der Gatewaytyp für Ihre Konfiguration korrekt ist. Für ein VPN-Gateway wird `-GatewayType Vpn` benötigt.

Beispiel:
```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>Gateway-SKUs
Beim Erstellen eines Gateways des virtuellen Netzwerks müssen Sie die gewünschte Gateway-SKU angeben. Wählen Sie die SKUs aus, die Ihre Anforderungen im Bezug auf Workloadtypen, Durchsätze, Funktionen und SLAs erfüllen.

>[!NOTE]
> Für klassische virtuelle Netzwerke sollten weiterhin die alten SKUs verwendet werden. Weitere Informationen zu den alten Gateway-SKUs finden Sie unter [Working with virtual network gateway SKUs (old)](/azure/vpn-gateway/vpn-gateway-about-skus-legacy) (Arbeiten mit SKUs für Gateways für virtuelle Netzwerke (alt)).

Azure Stack bietet folgende VPN-Gateway-SKUs:

|   | VPN-Gatewaydurchsatz |Max. IPsec-Tunnel für das VPN-Gateway |
|-------|-------|-------|
|**Basic-SKU**  | 100 MBit/s  | 10    |
|**Standard-SKU**           | 100 MBit/s  | 10    |
|**High-Performance-SKU** | 200 MBit/s    | 30    |

### <a name="resizing-gateway-skus"></a>Anpassen der Größe von Gateway-SKUs
SKU-Größenwechsel zwischen den unterstützten Legacy-SKUs werden von Azure Stack nicht unterstützt.

Analog dazu unterstützt Azure Stack auch nicht das Ändern der Größe von einer unterstützten Legacy-SKU (Basic, Standard und HighPerformance) in neuere, von Azure unterstützte SKUs (VpnGw1, VpnGw2 und VpnGw3).

### <a name="configure-the-gateway-sku"></a>Konfigurieren der Gateway-SKU
#### <a name="azure-stack-portal"></a>Azure Stack-Portal
Wenn Sie das Azure Stack-Portal verwenden, um ein Resource Manager-Gateway für virtuelle Netzwerke zu erstellen, können Sie die Gateway-SKU in der Dropdownliste auswählen. Die angegebenen Optionen entsprechen dem ausgewählten Gateway- und VPN-Typ.

#### <a name="powershell"></a>PowerShell
Im folgenden PowerShell-Beispiel wird „-GatewaySku“ als „VpnGw1“ angegeben.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```
### <a name="connection-types"></a>Verbindungstypen
Im Resource Manager-Bereitstellungsmodell ist für jede Konfiguration ein bestimmter Typ der Verbindung mit dem Gateway eines virtuellen Netzwerks erforderlich. Die verfügbaren Resource Manager-PowerShell-Werte für „-ConnectionType“ lauten:

- IPsec

Im folgenden PowerShell-Beispiel wird eine S2S-Verbindung erstellt, die den Verbindungstyp „IPsec“ erfordert.  

```PowerShell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

### <a name="vpn-types"></a>VPN-Typen
Wenn Sie das Gateway des virtuellen Netzwerks für eine VPN-Gatewaykonfiguration erstellen, müssen Sie einen VPN-Typ angeben. Der ausgewählte VPN-Typ hängt von der Verbindungstopologie ab, die Sie erstellen möchten.  Ein VPN-Typ kann zudem von der verwendeten Hardware abhängen. S2S-Konfigurationen erfordern ein VPN-Gerät. Einige VPN-Geräte unterstützen nur einen bestimmten VPN-Typ.

> [!IMPORTANT]  
> Aktuell unterstützt Azure Stack nur den routenbasierten VPN-Typ.  Wenn Ihr Gerät nur richtlinienbasierte VPNs unterstützt, werden Verbindungen mit diesen Geräten über Azure Stack nicht unterstützt.

- **PolicyBased:** *(unterstützt von Azure, aber nicht von Azure Stack)* Bei richtlinienbasierten VPNs werden Pakete verschlüsselt und durch IPsec-Tunnel geleitet. Grundlage hierfür sind die IPsec-Richtlinien, die jeweils per Kombination aus Adresspräfixen zwischen Ihrem lokalen Netzwerk und dem Azure-VNet konfiguriert werden. Die Richtlinie (auch Datenverkehrsselektor genannt) wird in der Regel als Zugriffsliste in der VPN-Gerätekonfiguration definiert.

- **RouteBased:** Bei RouteBased-VPNs werden Pakete auf der Grundlage der Routen der IP-Weiterleitungstabelle oder -Routingtabelle an die entsprechenden Tunnelschnittstellen weitergeleitet. An den Tunnelschnittstellen werden die Pakete dann ver- bzw. entschlüsselt. Die Richtlinie (bzw. der Datenverkehrsselektor) für RouteBased-VPNs wird im Any-to-Any-Format (bzw. als Platzhalter) konfiguriert. Der Wert für einen RouteBased-VPN-Typ lautet „RouteBased“.

Im folgenden PowerShell-Beispiel wird „-VpnType“ als „RouteBased“ angegeben. Beim Erstellen eines Gateways müssen Sie sicherstellen, dass -VpnType für Ihre Konfiguration korrekt ist.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```
### <a name="gateway-requirements"></a>Gatewayanforderungen
Die folgende Tabelle enthält die Anforderungen für VPN-Gateways.

| |Basic-VPN-Gateway (PolicyBased) | Basic-VPN-Gateway (RouteBased) | Standard-VPN-Gateway (RouteBased) | Hochleistungs-VPN-Gateway (RouteBased)|
|--|--|--|--|--|
| **Site-to-Site-Konnektivität (S2S)** | Nicht unterstützt | RouteBased-VPN-Konfiguration | RouteBased-VPN-Konfiguration | RouteBased-VPN-Konfiguration |
| **Authentifizierungsmethode**  | Nicht unterstützt | Vorinstallierter Schlüssel für S2S-Konnektivität  | Vorinstallierter Schlüssel für S2S-Konnektivität  | Vorinstallierter Schlüssel für S2S-Konnektivität  |   
| **Maximale Anzahl von S2S-Verbindungen**  | Nicht unterstützt | 10 | 10| 30|
|**Aktive Routingunterstützung (BGP)** | Nicht unterstützt | Nicht unterstützt | Unterstützt | Unterstützt |

### <a name="gateway-subnet"></a>Gatewaysubnetz
Bevor Sie ein VPN-Gateway erstellen, müssen Sie ein Gatewaysubnetz erstellen. Das Gatewaysubnetz enthält die IP-Adressen, die von den VMs und Diensten des virtuellen Netzwerkgateways verwendet werden. Bei der Erstellung des Gateways des virtuellen Netzwerks, werden Gateway-VMs für das Gatewaysubnetz bereitgestellt und mit den erforderlichen VPN Gateway-Einstellungen konfiguriert. Stellen Sie für das Gatewaysubnetz nichts anderes bereit (beispielsweise zusätzliche virtuelle Computer). Das Gatewaysubnetz muss den Namen „GatewaySubnet“ aufweisen, damit es einwandfrei funktioniert. Wenn Sie dem Gatewaysubnetz den Namen „GatewaySubnet“ zugewiesen haben, erkennt Azure Stack, dass es sich dabei um das Subnetz handelt, für das die virtuellen Computer und Dienste des Gateways für virtuelle Netzwerke bereitgestellt werden sollen.

Bei der Gatewayerstellung geben Sie die Anzahl der im Subnetz enthaltenen IP-Adressen an. Die IP-Adressen im Gatewaysubnetz werden den Gatewaydiensten und -VMs zugeordnet. Einige Konfigurationen erfordern mehr IP-Adressen als andere. Sehen Sie sich die Anweisungen für die Konfiguration an, die Sie erstellen möchten, und vergewissern Sie sich, dass das Gatewaysubnetz, das sie erstellen möchten, diese Anforderungen erfüllt. Stellen Sie zusätzlich sicher, dass Ihr Gatewaysubnetz über genügend IP-Adressen verfügt, und berücksichtigen Sie dabei auch mögliche zukünftige Konfigurationen. Sie können zwar ein Gatewaysubnetz mit einer Größe von nur /29 erstellen, aber es empfiehlt sich, ein Gatewaysubnetz mit einer Größe von mindestens /28 oder größer (/28, /27, /26 usw.) zu erstellen. Wenn Sie später Funktionen hinzufügen, müssen Sie Ihr Gateway nicht beenden und anschließend das Gatewaysubnetz löschen und erneut erstellen, um weitere IP-Adressen zu ermöglichen.

Im folgenden Resource Manager-PowerShell-Beispiel ist ein Gatewaysubnetz mit dem Namen GatewaySubnet zu sehen. Sie erkennen, das mit der CIDR-Notation die Größe /27 angegeben wird. Dies ist für eine ausreichende Zahl von IP-Adressen für die meisten Konfigurationen, die derzeit üblich sind, groß genug.

```PowerShell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Vermeiden Sie bei der Verwendung von Gatewaysubnetzen die Zuordnung einer Netzwerksicherheitsgruppe (NSG) zum Gatewaysubnetz. Das Zuordnen einer Netzwerksicherheitsgruppe zu diesem Subnetz kann dazu führen, dass das VPN-Gateway nicht mehr wie erwartet funktioniert. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](/azure/virtual-network/virtual-networks-nsg).

### <a name="local-network-gateways"></a>Lokale Netzwerkgateways
Wenn Sie eine VPN-Gatewaykonfiguration in Azure erstellen, stellt das Gateway des lokalen Netzwerks meist Ihren lokalen Standort dar. In Azure Stack stellt es ein beliebiges Remote-VPN-Gerät dar, das sich außerhalb von Azure Stack befindet.  Hierbei kann es sich um ein VPN-Gerät in Ihrem Datencenter, um ein Remotedatencenter oder um ein VPN-Gateway in Azure handeln.

Sie geben dem Gateway des lokalen Netzwerks einen Namen, stellen die öffentliche IP-Adresse des VPN-Geräts bereit und geben die Adresspräfixe an, die sich am lokalen Standort befinden. Azure sucht unter den Zieladresspräfixen nach Netzwerkdatenverkehr, sieht in der Konfiguration nach, die Sie für das Gateway des lokalen Netzwerks angegeben haben, und leitet die Pakete entsprechend weiter.

Mit dem folgenden PowerShell-Beispiel wird ein neues Gateway des lokalen Netzwerks erstellt:

```PowerShell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```
Manchmal müssen Sie die Einstellungen des Gateways des lokalen Netzwerks ändern, beispielsweise wenn Sie den Adressbereich erweitern oder ändern oder wenn sich die IP-Adresse des VPN-Geräts ändert. Weitere Informationen hierzu finden Sie im Artikel [Ändern der Einstellungen des lokalen Netzwerkgateways mit PowerShell](/azure/vpn-gateway/vpn-gateway-modify-local-network-gateway).

## <a name="ipsecike-parameters"></a>IPsec-/IKE-Parameter
Wenn Sie eine VPN-Verbindung in Azure Stack einrichten, müssen Sie die Verbindung an beiden Enden konfigurieren.  Wenn Sie eine VPN-Verbindung zwischen Azure Stack und einem Hardwaregerät (beispielsweise einem als VPN-Gateway fungierenden Switch oder Router) konfigurieren, sind für dieses Gerät unter Umständen weitere Einstellungen erforderlich.

Im Gegensatz zu Azure, das mehrere Angebote als Initiator und Antwortdienst unterstützt, unterstützt Azure Stack lediglich ein einzelnes Angebot.

###  <a name="ike-phase-1-main-mode-parameters"></a>Parameter der IKE-Phase 1 (Hauptmodus)
| Eigenschaft              | Wert|
|-|-|
| IKE-Version           | IKEv2 |
|Diffie-Hellman-Gruppe   | Gruppe 2 (1024 Bit) |
| Authentifizierungsmethode | Vorab ausgetauschter Schlüssel |
|Verschlüsselung und Hashalgorithmen | AES256, SHA256 |
|SA-Gültigkeitsdauer (Zeit)     | 28.800 Sekunden|

### <a name="ike-phase-2-quick-mode-parameters"></a>Parameter der IKE-Phase 2 (Schnellmodus)
| Eigenschaft| Wert|
|-|-|
|IKE-Version |IKEv2 |
|Verschlüsselung und Hashalgorithmen (Verschlüsselung)     | GCMAES256|
|Verschlüsselung und Hashalgorithmen (Authentifizierung) | GCMAES256|
|SA-Gültigkeitsdauer (Zeit)  | 3.600 Sekunden |
|SA-Gültigkeitsdauer (Bytes) | 819.200       |
|Perfect Forward Secrecy (PFS) |PFS2048 |
|Dead Peer Detection | Unterstützt|  
