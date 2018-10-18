---
title: Einrichten der Konnektivität zwischen einem virtuellen Netzwerk und SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Einrichten der Konnektivität zwischen einem virtuellen Netzwerk und SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5efdda485e4e1f5013948c6636b267f0d388f4d5
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44167602"
---
# <a name="connecting-a-vnet-to-hana-large-instance-expressroute"></a>Verbinden eines VNets mit HANA (große Instanzen)-ExpressRoute

Sobald Sie alle IP-Adressbereiche definiert und die entsprechenden Daten von Microsoft erhalten haben, können Sie beginnen, das von Ihnen zuvor erstellte VNet mit SAP HANA (große Instanzen) zu verbinden. Nachdem das Azure-VNet erstellt wurde, muss ein ExpressRoute-Gateway für das VNet erstellt werden, um es mit der ExpressRoute-Verbindung zu verknüpfen, die sich mit dem Kundenmandanten im „Große Instanz“-Stapel verbindet.

> [!NOTE] 
> Dieser Schritt kann bis zu 30 Minuten dauern, da das neue Gateway im vorgesehenen Azure-Abonnement erstellt und anschließend mit dem angegebenen Azure-VNet verbunden wird.

Wenn bereits ein Gateway vorhanden ist, prüfen Sie, ob es ein ExpressRoute-Gateway ist. Falls nicht, muss das Gateway gelöscht und als ExpressRoute-Gateway neu erstellt werden. Wenn bereits ein ExpressRoute-Gateway eingerichtet ist und weil das Azure-VNet für lokale Konnektivität bereits mit der ExpressRoute-Verbindung verknüpft ist, fahren Sie mit dem folgenden Abschnitt „Verknüpfen von VNets“ fort.

- Verwenden Sie entweder das (neue) [Azure-Portal](https://portal.azure.com/) oder PowerShell zum Erstellen eines ExpressRoute-VPN-Gateways, das mit Ihrem VNet verbunden ist.
  - Wenn Sie das Azure-Portal verwenden, fügen Sie ein neues **virtuelles Netzwerkgateway** hinzu, und wählen Sie dann **ExpressRoute** als Gatewaytyp aus.
  - Wenn Sie PowerShell verwenden, laden Sie zunächst das neueste [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) herunter, um eine optimale Leistung sicherzustellen. Über die folgenden Befehle erstellen Sie ein ExpressRoute-Gateway. Die Texte, denen _$_ vorangestellt ist, bestehen aus benutzerdefinierten Variablen, die mit Ihren jeweiligen Informationen aktualisiert werden müssen.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

In diesem Beispiel wurde die Gateway-SKU „HighPerformance“ verwendet. Ihre Optionen sind „HighPerformance“ oder „UltraPerformance“ als einzige Gateway-SKUs, die für SAP HANA in Azure (große Instanzen) unterstützt werden.

> [!IMPORTANT]
> Für HANA (große Instanzen) mit SKUs von Typ II-Klasse ist die Verwendung der UltraPerformance-Gateway-SKU erforderlich.

**Verknüpfen von VNETs**

Nun da das Azure-VNet ein ExpressRoute-Gateway hat, verwenden Sie die von Microsoft bereitgestellten Autorisierungsinformationen zum Verknüpfen des ExpressRoute-Gateways mit der ExpressRoute-Verbindung von SAP HANA in Azure (große Instanzen), die für diesen Zweck erstellt wurde. Dieser Schritt kann über das Azure-Portal oder mit PowerShell ausgeführt werden. Die Verwendung des Portals wird empfohlen, aber hier ist der Vollständigkeit halber auch die PowerShell-Anleitung angegeben. 

- Sie führen die folgenden Befehle für jedes VNet-Gateway aus, wobei Sie für jede Verbindung eine andere AuthGUID verwenden. Die ersten beiden unten im Skript gezeigten Einträge stammen aus den Informationen, die von Microsoft bereitgestellt werden. Zudem ist die AuthGUID spezifisch für jedes VNet und sein Gateway. Dies bedeutet Folgendes: Sie benötigen eine weitere AuthID für Ihre ExpressRoute-Leitung, die SAP HANA (große Instanzen) mit Azure verbindet, wenn Sie ein weiteres Azure-VNet hinzufügen möchten. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Sie müssen diesen Schritt unter Umständen mehrmals ausführen, wenn Sie das Gateway mit mehreren ExpressRoute-Leitungen verbinden möchten, die Ihrem Abonnement zugeordnet sind. Beispielsweise ist die Wahrscheinlichkeit hoch, dass Sie das VNet-Gateway, über das das VNet mit Ihrem lokalen Netzwerk verbunden wird, mit der ExpressRoute-Leitung verbinden.

**Nächste Schritte**

- Lesen Sie [Zusätzliche Netzwerkanforderungen für HLI](hana-additional-network-requirements.md).