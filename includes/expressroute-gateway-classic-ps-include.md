---
title: Includedatei
description: Includedatei
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 11/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fab00e5281bb91bce10228b3bc2e9cfd503d5d5b
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219816"
---
Sie müssen zuerst ein VNet und ein Gatewaysubnetz erstellen, bevor Sie an den folgenden Aufgaben arbeiten.

> [!NOTE]
> Diese Beispiele gelten nicht für die gemeinsame Verwendung von S2S und ExpressRoute.
> Weitere Informationen zur Verwendung von Gateways in einer Konfiguration für die gemeinsame Verwendung finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen (klassisch)](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Hinzufügen eines Gateways

Verwenden Sie den folgenden Befehl, um ein Gateway zu erstellen. Achten Sie darauf, dass Sie die vorhandenen Werte durch Ihre eigenen Werte ersetzen.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Erstellung des Gateways überprüfen

Verwenden Sie den folgenden Befehl, um zu überprüfen, ob das Gateway erstellt wurde. Mit diesem Befehl wird auch die Gateway-ID abgerufen, die Sie für andere Vorgänge benötigen.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Ändern der Größe eines Gateways

Es gibt eine Reihe von [Gateway-SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Sie können jederzeit den folgenden Befehl verwenden, um die Gateway-SKU zu ändern.

> [!IMPORTANT]
> Dieser Befehl kann nicht für das UltraPerformance-Gateway verwendet werden. Wenn Sie das Gateway in ein UltraPerformance-Gateway ändern möchten, entfernen Sie zunächst das vorhandene ExpressRoute-Gateway, und erstellen Sie anschließend ein neues UltraPerformance-Gateway. Wenn Sie das Gateway von einem UltraPerformance-Gateway herabstufen möchten, entfernen Sie zunächst das UltraPerformance-Gateway, und erstellen Sie anschließend ein neues Gateway. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Entfernen eines Gateways

Verwenden Sie den folgenden Befehl, um ein Gateway zu entfernen.

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```
