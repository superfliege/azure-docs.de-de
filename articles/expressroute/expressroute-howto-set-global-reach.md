---
title: 'Konfigurieren von Global Reach – ExpressRoute: Azure | Microsoft-Dokumentation'
description: Dieser Artikel hilft Ihnen, ExpressRoute-Leitungen miteinander zu verbinden, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen und Global Reach zu aktivieren.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 3df107f8854469b50c5e8483515388b5c93fb244
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53383271"
---
# <a name="configure-expressroute-global-reach-preview"></a>Konfigurieren von ExpressRoute Global Reach (Vorschauversion)
Dieser Artikel unterstützt Sie bei der Konfiguration von ExpressRoute Global Reach mit PowerShell. Weitere Informationen finden Sie unter [ExpressRoute Global Reach (Vorschau)](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Voraussetzungen
> [!IMPORTANT]
> Diese Public Preview wird ohne Vereinbarung zum Servicelevel bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Vergewissern Sie sich, dass folgende Voraussetzungen erfüllt sind, bevor Sie mit der Konfiguration beginnen:

* Die neueste Version von Azure PowerShell ist installiert. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Sie sind mit den [Workflows](expressroute-workflows.md) zur Bereitstellung von ExpressRoute-Leitungen vertraut.
* Ihre ExpressRoute-Leitungen haben den Status „Bereitgestellt“.
* Für Ihre ExpressRoute-Leitungen ist privates Azure-Peering konfiguriert.  

### <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto
Um mit der Konfiguration zu beginnen, melden Sie sich zunächst bei Ihrem Azure-Konto an. 

Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Sie werden zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert.  

```powershell
Connect-AzureRmAccount
```

Überprüfen Sie die Abonnements für das Konto, wenn Sie über mehrere Azure-Abonnements verfügen.

```powershell
Get-AzureRmSubscription
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Bestimmen der zu konfigurierenden ExpressRoute-Leitungen
Sie können ExpressRoute Global Reach zwischen zwei beliebigen ExpressRoute-Leitungen aktivieren, solange sich diese in den unterstützten Ländern befinden und an verschiedenen Peeringstandorten erstellt wurden. Wenn beide Leitungen Ihrem Abonnement zugewiesen sind, können Sie eine der beiden Leitungen auswählen, um die Konfiguration in den folgenden Abschnitten auszuführen. 

Wenn sich die beiden Leitungen in unterschiedlichen Azure-Abonnements befinden, benötigen Sie die Autorisierung eines der Azure-Abonnements. Den Autorisierungsschlüssel übergeben Sie, wenn Sie den Konfigurationsbefehl im anderen Azure-Abonnement ausführen.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Aktivieren der Konnektivität zwischen Ihren lokalen Netzwerken

Verwenden Sie folgende Befehle zum Abrufen von Leitung 1 und Leitung 2. Die beiden Leitungen befinden sich im selben Abonnement.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

Führen Sie den folgenden Befehl für Leitung 1 aus, und übergeben Sie die ID für das private Peering von Leitung 2. Beachten Sie dabei Folgendes:

* Die ID für das private Peering sieht in etwa wie folgt aus: 

  ```
  /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering
  ```
* *-AddressPrefix* muss ein IPv4-Subnetz vom Typ „/29“ sein (Beispiel: 10.0.0.0/29). Wir verwenden IP-Adressen in diesem Subnetz, um eine Verbindung zwischen den beiden ExpressRoute-Leitungen herzustellen. Die Adressen in diesem Subnetz dürfen nicht in Ihren virtuellen Azure-Netzwerken oder in Ihrem lokalen Netzwerk verwendet werden.

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

Speichern Sie die Konfiguration für Leitung 1 wie folgt:
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wenn der vorherige Vorgang abgeschlossen ist, sollten Sie dank der beiden ExpressRoute-Leitungen auf beiden Seiten über Konnektivität zwischen Ihren lokalen Netzwerken verfügen.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-Leitungen in verschiedenen Azure-Abonnements

Wenn sich die beiden Leitungen nicht im gleichen Azure-Abonnement befinden, benötigen Sie eine Autorisierung. Bei der folgenden Konfiguration wird die Autorisierung im Abonnement von Leitung 2 generiert und der Autorisierungsschlüssel an Leitung 1 übergeben.

Generieren Sie einen Autorisierungsschlüssel. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Notieren Sie sich die ID für das private Peering von Leitung 2 sowie den Autorisierungsschlüssel.

Führen Sie den folgenden Befehl für Leitung 1 aus. Übergeben Sie die ID für das private Peering von Leitung 2 und den Autorisierungsschlüssel.
```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

Speichern Sie die Konfiguration für Leitung 1.
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wenn der vorherige Vorgang abgeschlossen ist, sollten Sie dank der beiden ExpressRoute-Leitungen auf beiden Seiten über Konnektivität zwischen Ihren lokalen Netzwerken verfügen.

## <a name="get-and-verify-the-configuration"></a>Abrufen und Überprüfen der Konfiguration

Verwenden Sie den folgenden Befehl, um die Konfiguration für die Leitung zu überprüfen, für die die Konfiguration vorgenommen wurde (Leitung 1 im obigen Beispiel).

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Wenn Sie in PowerShell *$ckt1* ausführen, wird *CircuitConnectionStatus* in der Ausgabe angezeigt. So können Sie ermitteln, ob die Verbindung hergestellt wurde. Mögliche Werte: „Connected“ (Verbunden) und „Disconnected“ (Getrennt). 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Deaktivieren der Konnektivität zwischen Ihren lokalen Netzwerken

Führen Sie die Befehle zum Deaktivieren der Konnektivität für die Leitung aus, für die die Konfiguration vorgenommen wurde (Leitung 1 im obigen Beispiel).

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Sie können den Vorgang „Get“ ausführen, um den Status zu überprüfen. 

Nach Abschluss des obigen Vorgangs besteht zwischen Ihren lokalen Netzwerken keine Verbindung mehr über Ihre ExpressRoute-Leitungen. 


## <a name="next-steps"></a>Nächste Schritte
1. [Weitere Informationen zu ExpressRoute Global Reach](expressroute-global-reach.md)
2. [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
3. [Verknüpfen einer ExpressRoute-Leitung mit einem virtuellen Azure-Netzwerk](expressroute-howto-linkvnet-arm.md)


