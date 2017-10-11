---
title: Steuern in einem klassischen Virtuellenetzwerk in Azure - PowerShell - routing | Microsoft Docs
description: Informationen zum Festlegen des Routings im VNets, die mithilfe von PowerShell | Classic
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: e9564d223cb85529f1fa97bc398d35c6debcedae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Steuern von routing und virtual Appliances (klassisch) mithilfe von PowerShell verwenden

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure-CLI](virtual-network-create-udr-arm-cli.md)
> * [Vorlage](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassisch)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassisch)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Bevor Sie mit Azure-Ressourcen arbeiten, ist wichtig zu wissen, dass Azure derzeit zwei Bereitstellungsmodelle verfügt: Azure-Ressourcen-Manager und Classic. Stellen Sie sicher, dass Sie verstehen, [Bereitstellungsmodelle und Tools](../azure-resource-manager/resource-manager-deployment-model.md) vor der Arbeit mit Azure-Ressource. Sie können die Dokumentation für die unterschiedlichen Tools anzeigen, indem Sie eine Option am Anfang dieses Artikels ausgewählt. In diesem Artikel werden die klassischen Bereitstellungsmodell behandelt.
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Im Beispiel oben beschriebenen Szenario Azure PowerShell, die unten aufgeführten Befehle eine einfache Umgebung reicht bereits erstellte erwarten Grundlage. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden sollen, erstellen Sie die Umgebung, die in angezeigten [erstellen Sie eine VNet (klassisch) mithilfe von PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Erstellen Sie die UDR für das front-End-Subnetz
Führen Sie zum Erstellen der Routentabelle und eine Route für die front-End-Subnetz auf Grundlage der oben beschriebenen Szenario benötigt die folgenden Schritte aus.

1. Führen Sie den folgenden Befehl zum Erstellen einer Routentabelle für die Front-End-Subnetz:

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. Führen den folgenden Befehl zum Erstellen einer Route in der Routingtabelle, um allen Datenverkehr an das Back-End-Subnetz (192.168.2.0/24) zu senden, um die **FW1** VM ("192.168.0.4"):

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Führen Sie den folgenden Befehl zum Verknüpfen der Routentabelle mit dem **Front-End-** Subnetz:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-the-udr-for-the-back-end-subnet"></a>Erstellen Sie die UDR für das Back-End-Subnetz
Führen Sie zum Erstellen der Routentabelle und eine Route, die für das Back-End-Subnetz auf Grundlage dieses Szenarios benötigt die folgenden Schritte aus:

1. Führen Sie den folgenden Befehl zum Erstellen einer Routentabelle für die Back-End-Subnetz:

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. Führen den folgenden Befehl zum Erstellen einer Route in der Routingtabelle, um allen Datenverkehr auf dem Front-End-Subnetz (192.168.1.0/24) zu senden, um die **FW1** VM ("192.168.0.4"):

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Führen Sie den folgenden Befehl zum Verknüpfen der Routentabelle mit dem **Back-End-** Subnetz:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Aktivieren Sie die IP-Weiterleitung auf der VM FW1

Um IP-Weiterleitung auf dem FW1 virtuellen Computer zu aktivieren, führen Sie die folgenden Schritte aus:

1. Führen Sie den folgenden Befehl zum Überprüfen des Status der IP-Weiterleitung:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. Führen Sie den folgenden Befehl zum Aktivieren von IP-Weiterleitung für die *FW1* VM:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
