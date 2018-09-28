---
title: Azure ExpressRoute Global Reach | Microsoft-Dokumentation
description: Dieser Artikel hilft Ihnen, ExpressRoute-Leitungen miteinander zu verbinden, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen und Global Reach zu aktivieren.
documentationcenter: na
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 90f34da10e0193e31a0f70187463799013f52be8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966814"
---
# <a name="link-expressroute-circuits-to-enable-expressroute-global-reach-preview"></a>Verknüpfen von ExpressRoute-Leitungen zum Aktivieren von ExpressRoute Global Reach (Vorschau)

ExpressRoute ist eine private und zuverlässige Möglichkeit, Ihre lokalen Netzwerke mit der Microsoft Cloud zu verbinden. Sie können aus Ihrem privaten Rechenzentrum oder Ihrem Unternehmensnetzwerk auf viele Microsoft-Clouddienste wie beispielsweise Azure, Office 365 und Dynamics 365 zugreifen. Angenommen, Sie haben eine Niederlassung in San Francisco mit einer ExpressRoute-Leitung im Silicon Valley und eine weitere Niederlassung in Baltimore mit einer ExpressRoute-Leitung in Washington DC. 

Beide Niederlassungen können über eine Hochgeschwindigkeitsverbindung mit Azure-Ressourcen im Osten und Westen der USA verfügen. Die Niederlassungen können jedoch keine Daten direkt miteinander austauschen. Anders gesagt kann 10.0.1.0/24 mit 10.0.3.0/24 und 10.0.4.0/24, jedoch nicht mit 10.0.2.0/24 Daten austauschen.

![ohne][1]

Mithilfe von **ExpressRoute Global Reach** können Sie ExpressRoute-Leitungen miteinander verbinden, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen. Im obigen Beispiel kann Ihre Niederlassung in San Francisco (10.0.1.0/24) mit Ihrer Niederlassung in Baltimore (10.0.2.0/24) über die bestehenden ExpressRoute-Leitungen und über das globale Netzwerk von Microsoft Daten direkt austauschen. 

![mit][2]

ExpressRoute Global Reach wird derzeit in den folgenden Ländern unterstützt:

* USA
* Vereinigtes Königreich 
* Japan

Ihre ExpressRoute-Leitungen müssen an den [ExpressRoute-Peeringstandorten](expressroute-locations.md) in diesen Ländern eingerichtet werden. Um ExpressRoute Global Reach zwischen [verschiedenen geopolitischen Regionen](expressroute-locations.md) zu erstellen, ist für Ihre Leitungen die SKU „Premium“ erforderlich.


## <a name="before-you-begin"></a>Voraussetzungen

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Stellen Sie vor Beginn der Konfiguration sicher, dass Sie mit den folgenden Anforderungen vertraut sind.

* Installieren Sie die neueste Version von Azure PowerShell. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Machen Sie sich mit den [Workflows](expressroute-workflows.md) zur Bereitstellung von ExpressRoute-Leitungen vertraut.
* Vergewissern Sie sich, dass Ihre ExpressRoute-Leitungen den Status „Bereitgestellt“ haben.
* Stellen Sie sicher, dass für Ihre ExpressRoute-Leitungen das private Azure-Peering konfiguriert ist.  

### <a name="log-into-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.
Bevor Sie mit dieser Konfiguration beginnen, müssen Sie sich bei Ihrem Azure-Konto anmelden. 

Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Sie werden vom Befehl zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert.  

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
Sie können ExpressRoute Global Reach zwischen zwei beliebigen ExpressRoute-Leitungen aktivieren, solange sie sich in den unterstützten Ländern befinden und an verschiedenen Peeringstandorten erstellt werden. Wenn sich beide Leitungen in Ihrem Abonnement befinden, können Sie eine der beiden Leitungen auswählen, um die Konfiguration in den folgenden Abschnitten auszuführen. Wenn sich die beiden Leitungen in unterschiedlichen Azure-Abonnements befinden, benötigen Sie die Autorisierung eines Azure-Abonnements und müssen den Autorisierungsschlüssel übergeben, sofern Sie den Konfigurationsbefehl im anderen Azure-Abonnement ausführen.

## <a name="to-enable-connectivity-between-your-on-premises-networks"></a>So aktivieren Sie Konnektivität zwischen Ihren lokalen Netzwerken

Verwenden Sie folgende Befehle zum Abrufen von Leitung 1 und Leitung 2. Die beiden Leitungen befinden sich im selben Abonnement.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

Rufen Sie den folgenden Befehl für Leitung 1 auf, und geben Sie die private Peering-ID von Leitung 2 ein.

Die private Peering-ID sieht so aus:

`/subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering`


```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

*-AddressPrefix* muss ein Subnetz des Typs „/29 IPv4“ sein, z.B. 10.0.0.0/29. Wir nutzen IP-Adressen in diesem Subnetz, um Konnektivität zwischen den beiden ExpressRoute-Leitungen herzustellen. Verwenden Sie keine Adressen in diesem Subnetz in Ihren Azure VNETs oder lokalen Netzwerken. 


Speichern Sie die Konfiguration für Leitung 1.

```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wenn der vorherige Vorgang abgeschlossen ist, sollten Sie dank der beiden ExpressRoute-Leitungen auf beiden Seiten über Konnektivität zwischen Ihren lokalen Netzwerken verfügen.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-Leitungen in verschiedenen Azure-Abonnements

Wenn die zwei Leitungen nicht im gleichen Azure-Abonnement enthalten sind, benötigen Sie eine Autorisierung. Bei der folgenden Konfiguration wird die Autorisierung im Abonnement von Leitung 2 generiert und der Autorisierungsschlüssel an Leitung 1 übergeben.

Generieren Sie einen Autorisierungsschlüssel. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Notieren Sie sich die private Peering-ID von Leitung 2 sowie den Autorisierungsschlüssel.

Führen Sie den folgenden Befehl für Leitung 1 aus. Übergeben Sie die private Peering-ID von Leitung 2 sowie den Autorisierungsschlüssel.

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

Speichern Sie die Konfiguration für Leitung 1.

```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Wenn die vorherigen Vorgänge abgeschlossen sind, sollten Sie dank der beiden ExpressRoute-Leitungen auf beiden Seiten über Konnektivität zwischen Ihren lokalen Netzwerken verfügen.

## <a name="to-get-and-verify-the-configuration"></a>So wird die Konfiguration abgerufen und überprüft

Verwenden Sie den folgenden Befehl, um die Konfiguration für die Leitung zu überprüfen, für die die Konfiguration vorgenommen wurde. In diesem Beispiel wird Leitung 1 aus dem vorherigen Beispiel verwendet.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Wenn Sie in PowerShell *$ckt1* ausführen, sehen Sie *CircuitConnectionStatus* in der Ausgabe. Sie erfahren, ob Konnektivität eingerichtet wurde bzw. ob der Status „Connected“ (Verbunden) oder „Disconnected“ (Nicht verbunden) ist. 

## <a name="to-disable-connectivity-between-your-on-premises-networks"></a>So deaktivieren Sie Konnektivität zwischen Ihren lokalen Netzwerken

Zum Deaktivieren führen Sie die Befehle für die Leitung aus, für die die Konfiguration vorgenommen wurde. In diesem Beispiel wird Leitung 1 aus den vorherigen Beispielen verwendet.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Sie können den Vorgang „Get“ ausführen, um den Status zu überprüfen. 

Sobald der oben genannte Vorgang abgeschlossen ist, besteht über Ihre ExpressRoute-Leitungen keine Konnektivität mehr zwischen Ihren lokalen Netzwerken. 

## <a name="next-steps"></a>Nächste Schritte
1. [Weitere Informationen zu ExpressRoute Global Reach](expressroute-faqs.md#globalreach)
2. [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
3. [Verknüpfen einer ExpressRoute-Leitung mit einem virtuellen Azure-Netzwerk](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Diagramm ohne Global Reach"
[2]: ./media/expressroute-global-reach/2.png "Diagramm mit Global Reach"