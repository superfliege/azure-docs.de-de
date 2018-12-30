---
title: 'Konfigurieren von ExpressRoute Global Reach: Azure CLI | Microsoft-Dokumentation'
description: Dieser Artikel hilft Ihnen, ExpressRoute-Leitungen miteinander zu verbinden, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen und Global Reach zu aktivieren.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/12/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 9a8e0a5df9383d8e3d7159aa916b0e4fbfeea948
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384049"
---
# <a name="configure-expressroute-global-reach-using-azure-cli-preview"></a>Konfigurieren von ExpressRoute Global Reach mit der Azure CLI (Vorschau)
Dieser Artikel unterstützt Sie bei der Konfiguration von ExpressRoute Global Reach mit der Azure CLI. Weitere Informationen finden Sie unter [ExpressRoute Global Reach (Vorschau)](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Voraussetzungen
> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Stellen Sie vor Beginn der Konfiguration sicher, dass Sie mit den folgenden Anforderungen vertraut sind.

* Installieren Sie die neueste Version der Azure CLI. Informationen hierzu finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli) und [Erste Schritte mit der Azure CLI](/cli/azure/get-started-with-azure-cli).
* Machen Sie sich mit den [Workflows](expressroute-workflows.md) zur Bereitstellung von ExpressRoute-Leitungen vertraut.
* Vergewissern Sie sich, dass Ihre ExpressRoute-Leitungen den Status „Bereitgestellt“ haben.
* Stellen Sie sicher, dass für Ihre ExpressRoute-Leitungen das private Azure-Peering konfiguriert ist.  

### <a name="log-into-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.
Bevor Sie mit dieser Konfiguration beginnen, müssen Sie sich bei Ihrem Azure-Konto anmelden. Mit dem Befehl wird der Standardbrowser geöffnet, und Sie werden zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert.  

```azurecli
az login
```

Überprüfen Sie die Abonnements für das Konto, wenn Sie über mehrere Azure-Abonnements verfügen.

```azurecli
az account list
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```azurecli
az account set --subscription <your subscription ID>
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Bestimmen der zu konfigurierenden ExpressRoute-Leitungen
Sie können ExpressRoute Global Reach zwischen zwei beliebigen ExpressRoute-Leitungen aktivieren, solange sie sich in den unterstützten Ländern befinden und an verschiedenen Peeringstandorten erstellt werden. Wenn beide Leitungen Ihrem Abonnement zugewiesen sind, können Sie eine der beiden Leitungen auswählen, um die Konfiguration in den folgenden Abschnitten auszuführen. Wenn sich die beiden Leitungen in unterschiedlichen Azure-Abonnements befinden, benötigen Sie die Autorisierung eines Azure-Abonnements und müssen den Autorisierungsschlüssel übergeben, sofern Sie den Konfigurationsbefehl im anderen Azure-Abonnement ausführen.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Aktivieren der Konnektivität zwischen Ihren lokalen Netzwerken

Wenn Sie den Befehl zum Aktivieren der Konnektivität ausführen, beachten Sie die folgenden Werte:

* Die Peerleitung *peer-circuit* muss die vollständige Ressourcen-ID sein. Beispiel:  

  ```
  /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}
  ```
* *-AddressPrefix* muss ein IPv4-Subnetz des Typs „/29“ sein, z.B. „10.0.0.0/29“. Wir nutzen IP-Adressen in diesem Subnetz, um Konnektivität zwischen den beiden ExpressRoute-Leitungen herzustellen. Sie müssen keine Adressen in diesem Subnetz in Ihren Azure VNETs oder lokalen Netzwerken verwenden.

Führen Sie Folgendes an der CLI aus, um zwei ExpressRoute-Leitungen zu verbinden. Verwenden Sie den folgenden Beispielbefehl:

```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29>
```

Die CLI-Ausgabe sieht in etwa wie folgt aus:

```azurecli
{
  "addressPrefix": "<__.__.__.__/29>",
  "authorizationKey": null,
  "circuitConnectionStatus": "Connected",
  "etag": "W/\"48d682f9-c232-4151-a09f-fab7cb56369a\"",
  "expressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<ResourceGroupName>"
  },
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit1Name>/peerings/AzurePrivatePeering/connections/<ConnectionName>",
  "name": "<ConnectionName>",
  "peerExpressRouteCircuitPeering": {
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/peerings/AzurePrivatePeering",
    "resourceGroup": "<Circuit2ResourceGroupName>"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "<ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/peerings/connections"
}
```

Wenn der oben beschriebene Vorgang abgeschlossen ist, sollten Sie dank der beiden ExpressRoute-Leitungen auf beiden Seiten über Konnektivität zwischen Ihren lokalen Netzwerken verfügen.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-Leitungen in verschiedenen Azure-Abonnements

Wenn die zwei Leitungen nicht im gleichen Azure-Abonnement enthalten sind, benötigen Sie eine Autorisierung. Bei der folgenden Konfiguration wird die Autorisierung im Abonnement von Leitung 2 generiert und der Autorisierungsschlüssel an Leitung 1 übergeben.

Generieren Sie einen Autorisierungsschlüssel. 
```azurecli
az network express-route auth create --circuit-name <Circuit2Name> -g <Circuit2ResourceGroupName> -n <AuthorizationName>
```

Die CLI-Ausgabe sieht wie folgt aus.

```azurecli
{
  "authorizationKey": "<authorizationKey>",
  "authorizationUseStatus": "Available",
  "etag": "W/\"cfd15a2f-43a1-4361-9403-6a0be00746ed\"",
  "id": "/subscriptions/<SubscriptionID>/resourceGroups/<Circuit2ResourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<Circuit2Name>/authorizations/<AuthorizationName>",
  "name": "<AuthorizationName>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<Circuit2ResourceGroupName>",
  "type": "Microsoft.Network/expressRouteCircuits/authorizations"
}
```

Notieren Sie sich die Ressourcen-ID von Leitung 2 sowie den Autorisierungsschlüssel.

Führen Sie den folgenden Befehl für Leitung 1 aus. Übergeben Sie die Ressourcen-ID von Leitung 2 und den Autorisierungsschlüssel. 
```azurecli
az network express-route peering connection create -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName> --peer-circuit <Circuit2ResourceID> --address-prefix <__.__.__.__/29> --authorization-key <authorizationKey>
```

Wenn der oben beschriebene Vorgang abgeschlossen ist, sollten Sie dank der beiden ExpressRoute-Leitungen auf beiden Seiten über Konnektivität zwischen Ihren lokalen Netzwerken verfügen.

## <a name="get-and-verify-the-configuration"></a>Abrufen und Überprüfen der Konfiguration

Verwenden Sie den folgenden Befehl, um die Konfiguration für die Leitung zu überprüfen, für die die Konfiguration vorgenommen wurde, z.B. Leitung 1 im obigen Beispiel.

```azurecli
az network express-route show -n <CircuitName> -g <ResourceGroupName>
```

In der CLI-Ausgabe wird *CircuitConnectionStatus* angezeigt. Er gibt über den Status „Connected“ (Verbunden) oder „Disconnected“ (Nicht verbunden) an, ob Konnektivität zwischen den beiden Leitungen besteht. 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Deaktivieren der Konnektivität zwischen Ihren lokalen Netzwerken

Zum Deaktivieren führen Sie die Befehle für die Leitung aus, für die die Konfiguration vorgenommen wurde, z.B. Leitung 1 im obigen Beispiel.

```azurecli
az network express-route peering connection delete -g <ResourceGroupName> --circuit-name <Circuit1Name> --peering-name AzurePrivatePeering -n <ConnectionName>
```

Sie können den Befehl „CLI anzeigen“ ausführen, um den Status zu überprüfen. 

Sobald der oben genannte Vorgang abgeschlossen ist, besteht über Ihre ExpressRoute-Leitungen keine Konnektivität mehr zwischen Ihren lokalen Netzwerken. 


## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu ExpressRoute Global Reach](expressroute-global-reach.md)
* [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
* [Verknüpfen einer ExpressRoute-Leitung mit einem virtuellen Azure-Netzwerk](expressroute-howto-linkvnet-arm.md)


