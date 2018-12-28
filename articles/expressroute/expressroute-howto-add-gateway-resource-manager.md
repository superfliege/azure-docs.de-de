---
title: 'Hinzufügen eines virtuellen Netzwerkgateways zu einem VNet für ExpressRoute: PowerShell: Azure | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie einem bereits erstellten Resource Manager-VNET für ExpressRoute ein VNET-Gateway hinzufügen.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 04/17/2017
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: 8f8d021adae1cb25198c53e486fdb50e42ac211b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097912"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>Konfigurieren eines Gateways für ein virtuelles Netzwerk für ExpressRoute mit PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassisch – PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video – Azure-Portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

In diesem Artikel wird beschrieben, wie Sie ein virtuelles Netzwerkgateway für ein vorhandenes virtuelles Netzwerk (VNet) hinzufügen und entfernen bzw. dessen Größe ändern. Die Schritte für diese Konfiguration gelten speziell für VNets, die gemäß dem Resource Manager-Bereitstellungsmodell erstellt wurden, das in einer ExpressRoute-Konfiguration verwendet wird. Weitere Informationen zu virtuellen Netzwerkgateways und Gateway-Konfigurationseinstellungen für ExpressRoute finden Sie unter [Informationen zu Gateways für virtuelle Netzwerke für ExpressRoute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Vorbereitungen
Stellen Sie sicher, dass Sie die neuesten Azure PowerShell-Cmdlets installiert haben. Wenn Sie die neuesten Cmdlets noch nicht installiert haben, müssen Sie dies tun, bevor Sie mit der Konfiguration beginnen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie das VNet-Gateway erstellt haben, können Sie Ihr VNet mit einer ExpressRoute-Verbindung verknüpfen. Weitere Informationen finden Sie unter [Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md).

