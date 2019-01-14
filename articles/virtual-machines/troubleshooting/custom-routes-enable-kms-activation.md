---
title: Verwenden benutzerdefinierter Azure-Routen, um die KMS-Aktivierung mit Tunnelerzwingung zu ermöglichen | Microsoft-Dokumentation
description: Zeigt, wie benutzerdefinierte Azure-Routen verwendet werden, um die KMS-Aktivierung mit Tunnelerzwingung zu ermöglichen.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: f1e2ab6a954361a7807d78dc2baf5d24af52a679
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53797405"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Fehler bei der Windows-Aktivierung in einem Szenario mit Tunnelerzwingung

Dieser Artikel beschreibt, wie Sie das KMS-Aktivierungsproblem beheben können, das beim Aktivieren der Tunnelerzwingung in Szenarien mit Site-to-Site-VPN-Verbindungen oder ExpressRoute auftreten kann.

## <a name="symptom"></a>Symptom

Sie aktivieren [Tunnelerzwingung](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) in den Subnetzen von virtuellen Azure-Netzwerken, um den gesamten in das Internet gerichteten Datenverkehr in Ihr lokales Netzwerk zurück zu leiten. In diesen Szenario können die virtuellen Azure-Computer (VMs), die Windows Server 2012 R2 oder höhere Versionen ausführen, Windows erfolgreich aktivieren. Jedoch tritt bei VMs, auf denen frühere Versionen von Windows ausgeführt werden, ein Aktivierungsfehler auf. 

## <a name="cause"></a>Ursache

Die Azure Windows-VMs müssen für die Windows-Aktivierung eine Verbindung mit dem Azure KMS-Server herstellen. Für die Aktivierung ist es erforderlich, dass die Aktivierungsanforderung von einer öffentlichen Azure-IP-Adresse stammt. In dem Szenario mit Tunnelerzwingung tritt ein Aktivierungsfehler auf, da die Aktivierungsanforderung aus Ihrem lokalen Netzwerk statt von einer öffentlichen Azure-IP-Adresse stammt. 

## <a name="solution"></a>Lösung

Um dieses Problem zu beheben, verwenden Sie die benutzerdefinierte Azure-Route, um den Aktivierungsverkehr zum Azure KMS-Server (23.102.135.246) zu leiten. 

Die IP-Adresse 23.102.135.246 ist die IP-Adresse des KMS-Servers für die globale Azure-Cloud. Der DNS-Name ist „kms.core.windows.net“. Wenn Sie andere Azure-Plattformen, wie etwa Azure Deutschland, verwenden, müssen Sie die IP-Adresse des entsprechenden KMS-Servers verwenden. Weitere Informationen finden Sie in der Tabelle unten:

|Plattform| KMS-DNS|KMS-IP|
|------|-------|-------|
|Azure Global|kms.core.windows.net|23.102.135.246|
|Azure Deutschland|kms.core.cloudapi.de|51.4.143.248|
|Azure US Government|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure China 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


Befolgen Sie diese Schritte, um die benutzerdefinierte hinzuzufügen:

### <a name="for-resource-manager-vms"></a>Für Resource Manager-VMs

1. Öffnen Sie Azure PowerShell, und [melden Sie sich dann bei Ihrem Azure-Abonnement an](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Führen Sie die folgenden Befehle aus:

    ```powershell
    # First, we will get the virtual network hosts the VMs that has activation problems. In this case, I get virtual network ArmVNet-DM in Resource Group ArmVNet-DM

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, we create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out

    $RouteTable = New-AzureRmRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzureRmRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzureRmRouteTable -RouteTable $RouteTable
    ```
3. Wechseln Sie zu der VM mit dem Aktivierungsproblem, und verwenden Sie [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping), um zu testen, ob sie den KMS-Server erreichen kann:

        psping kms.core.windows.net:1688

4. Versuchen Sie, Windows zu aktivieren, und sehen Sie, ob das Problem behoben ist.

### <a name="for-classic-vms"></a>Für klassische virtuelle Computer

1. Öffnen Sie Azure PowerShell, und [melden Sie sich dann bei Ihrem Azure-Abonnement an](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Führen Sie die folgenden Befehle aus:

    ```powershell
    # First, we will create a new route table
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the routetable that was created
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply KMS route table to the subnet that host the problem VMs (in this case, I will apply it to the subnet named Subnet-1)
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Wechseln Sie zu der VM mit dem Aktivierungsproblem, und verwenden Sie [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping), um zu testen, ob sie den KMS-Server erreichen kann:

        psping kms.core.windows.net:1688

4. Versuchen Sie, Windows zu aktivieren, und sehen Sie, ob das Problem behoben ist.

## <a name="next-steps"></a>Nächste Schritte

- [KMS-Clientsetupschlüssel](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Überprüfen und Auswählen von Aktivierungsmethoden](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)