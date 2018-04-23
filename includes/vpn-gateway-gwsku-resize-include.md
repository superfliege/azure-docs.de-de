---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c06e69dd9d1997500589659e936dc25ee01ed145
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2018
---
Wenn Sie für die aktuellen SKUs („VpnGw1“, „VpnGw2“ und „VPNGW3“) die Größe Ihrer Gateway-SKU anpassen möchten, um ein Upgrade auf eine leistungsfähigere SKU durchzuführen, können Sie das PowerShell-Cmdlet `Resize-AzureRmVirtualNetworkGateway` verwenden. Sie können mit diesem Cmdlet auch ein Downgrade der Gateway-SKU-Größe durchführen. Verwenden Sie bei Verwendung der Basic-Gateway-SKU stattdessen [diese Anweisungen](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize), um die Größe Ihres Gateways anzupassen.

Im folgenden PowerShell-Beispiel wird das Ändern der Größe einer Gateway-SKU in „VpnGw2“ veranschaulicht.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Sie können die Größe eines Gateways auch über das Azure-Portal anpassen. Wählen Sie hierzu auf der Seite **Konfiguration** für Ihr Gateway für virtuelle Netzwerke in der Dropdownliste eine andere SKU aus.