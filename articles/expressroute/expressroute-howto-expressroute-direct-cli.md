---
title: Konfigurieren von Azure ExpressRoute Direct über die CLI | Microsoft-Dokumentation
description: Diese Seite unterstützt Sie beim Konfigurieren von ExpressRoute Direct über die CLI (Vorschau).
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: cherylmc
ms.openlocfilehash: 989e96aa00ae65d1206f961a10893e3331670553
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958303"
---
# <a name="how-to-configure-expressroute-direct-using-cli-preview"></a>Konfigurieren von ExpressRoute Direct über die CLI (Vorschau)

ExpressRoute Direct bietet Ihnen die Möglichkeit, sich direkt mit dem globalen Netzwerk von Microsoft zu verbinden, und zwar an strategisch über die ganze Welt verteilten Peeringstandorten. Weitere Informationen über ExpressRoute finden Sie unter [Informationen zu ExpressRoute Direct](expressroute-erdirect-about.md).

> [!IMPORTANT]
> ExpressRoute Direkt ist derzeit in der Vorschauphase.
>
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="resources"></a>Erstellen der Ressource

1. Melden Sie sich bei Azure an, und wählen Sie das Abonnement aus. Die ExpressRoute Direct-Ressource und ExpressRoute-Leitungen müssen sich im selben Abonnement befinden.

  ```azurecli
  az login
  ```

  Überprüfen Sie die Abonnements für das Konto. 

  ```azurecli
  az account list 
  ```

  Wählen Sie das Abonnement aus, für das eine ExpressRoute-Verbindung erstellt werden soll.
  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Listen Sie alle Standorte auf, an denen ExpressRoute Direct unterstützt wird.
    
  ```azurecli
  az network express-route port location list
  ```

  **Beispielausgabe**
  
  ```azurecli
  [
   {
    "address": "21715 Filigree Court, DC2, Building F, Ashburn, VA 20147",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-DC2",
    "location": null,
    "name": "Equinix-Ashburn-DC2",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "1950 N. Stemmons Freeway, Suite 1039A, DA3, Dallas, TX 75207",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Dallas-DA3",
    "location": null,
    "name": "Equinix-Dallas-DA3",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "111 8th Avenue, New York, NY 10011",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-New-York-NY5",
    "location": null,
    "name": "Equinix-New-York-NY5",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "11 Great Oaks Blvd, SV1, San Jose, CA 95119",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-SV1",
    "location": null,
    "name": "Equinix-San-Jose-SV1",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "2001 Sixth Ave., Suite 350, SE2, Seattle, WA 98121",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Seattle-SE2",
    "location": null,
    "name": "Equinix-Seattle-SE2",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   }
  ]
  ```
3. Bestimmen Sie, ob ein oben aufgelisteter Standort verfügbare Bandbreite aufweist.

  ```azurecli
  az network express-route port location show -l "Equinix-Ashburn-DC2"
  ```

  **Beispielausgabe**

  ```azurecli
  {
  "address": "21715 Filigree Court, DC2, Building F, Ashburn, VA 20147",
  "availableBandwidths": [
    {
      "offerName": "100 Gbps",
      "valueInGbps": 100
    }
  ],
  "contact": "support@equinix.com",
  "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-DC2",
  "location": null,
  "name": "Equinix-Ashburn-DC2",
  "provisioningState": "Succeeded",
  "tags": null,
  "type": "Microsoft.Network/expressRoutePortsLocations"
  }
  ```
4. Erstellen Sie eine ExpressRoute Direct-Ressource basierend auf dem oben gewählten Standort.

  ExpressRoute Direct unterstützt die Kapselungen QinQ und Dot1Q. Bei Wahl von QinQ wird jeder ExpressRoute-Leitung dynamisch ein S-Tag zugewiesen, das in der gesamten ExpressRoute Direct-Ressource eindeutig ist. Jedes C-Tag für die Leitung muss innerhalb der Leitung eindeutig sein, jedoch nicht innerhalb von ExpressRoute Direct.  

  Wenn die Kapselung Dot1Q ausgewählt wird, müssen Sie die Eindeutigkeit des C-Tags (VLAN) für die gesamte ExpressRoute Direct-Ressource sicherstellen.  

  > [!IMPORTANT]
  > Für ExpressRoute Direct kann nur ein Kapselungstyp festgelegt werden. Nach der ExpressRoute Direct-Erstellung kann die Kapselung nicht geändert werden.
  > 
 
  ```azurecli
  az network express-route port create -n $name -g $RGName --bandwidth 100 gbps  --encapsulation QinQ | Dot1Q --peering-location $PeeringLocationName -l $AzureRegion 
  ```

  > [!NOTE]
  > Das Attribut „Kapselung“ kann auch auf Dot1Q festgelegt werden. 
  >

  **Beispielausgabe:**

  ```azurecli
  {
  "allocationDate": "Wednesday, October 17, 2018",
  "bandwidthInGbps": 100,
  "circuits": null,
  "encapsulation": "Dot1Q",
  "etag": "W/\"<etagnumber>\"",
  "etherType": "0x8100",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
  "links": [
    {
      "adminState": "Disabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link1",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link1",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-1",
      "type": "Microsoft.Network/expressRoutePorts/links"
    },
    {
      "adminState": "Disabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link2",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link2",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-2",
      "type": "Microsoft.Network/expressRoutePorts/links"
    }
  ],
  "location": "westus",
  "mtu": "1500",
  "name": "Contoso-Direct",
  "peeringLocation": "Equinix-Ashburn-DC2",
  "provisionedBandwidthInGbps": 0.0,
  "provisioningState": "Succeeded",
  "resourceGroup": "Contoso-Direct-rg",
  "resourceGuid": "02ee21fe-4223-4942-a6bc-8d81daabc94f",
  "tags": null,
  "type": "Microsoft.Network/expressRoutePorts"
  }  
  ```

## <a name="state"></a>Ändern des Verwaltungsstatus von Verknüpfungen

Dieser Prozess sollte verwendet werden, um einen Layer-1-Test durchzuführen, der sicherstellt, dass jede Kreuzverbindung ordnungsgemäß in jeden Router für primäre und sekundäre Verbindungen eingebunden ist.

1. Legen Sie „Verknüpfung“ auf „Aktiviert“ fest. Wiederholen Sie diesen Schritt, um jede Verknüpfung auf „Aktiviert“ festzulegen.

  „Links[0]“ ist der primäre Port, „Links[1]“ der sekundäre Port.

  ```azurecli
  az network express-route port update -n Contoso-Direct -g Contoso-Direct-rg --set links[0].adminState="Enabled"
  ```
  ```azurecli
  az network express-route port update -n Contoso-Direct -g Contoso-Direct-rg --set links[1].adminState="Enabled"
  ```
  **Beispielausgabe:**

  ```azurecli
  {
  "allocationDate": "Wednesday, October 17, 2018",
  "bandwidthInGbps": 100,
  "circuits": null,
  "encapsulation": "Dot1Q",
  "etag": "W/\"<etagnumber>\"",
  "etherType": "0x8100",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
  "links": [
    {
      "adminState": "Enabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link1",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link1",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-1",
      "type": "Microsoft.Network/expressRoutePorts/links"
    },
    {
      "adminState": "Enabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link2",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link2",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-2",
      "type": "Microsoft.Network/expressRoutePorts/links"
    }
  ],
  "location": "westus",
  "mtu": "1500",
  "name": "Contoso-Direct",
  "peeringLocation": "Equinix-Ashburn-DC2",
  "provisionedBandwidthInGbps": 0.0,
  "provisioningState": "Succeeded",
  "resourceGroup": "Contoso-Direct-rg",
  "resourceGuid": "<resourceGUID>",
  "tags": null,
  "type": "Microsoft.Network/expressRoutePorts"
  }
  ```

  Verwenden Sie die gleiche Vorgehensweise mit `AdminState = “Disabled”`, um die Ports zu deaktivieren.

## <a name="circuit"></a>Erstellen einer Leitung

Sie können standardmäßig 10 Leitungen im Abonnement erstellen, in dem sich die ExpressRoute Direct-Ressource befindet. Dieser Wert kann vom Support erhöht werden. Sie sind für die Nachverfolgung der Werte für die bereitgestellte und belegte Bandbreite zuständig. Die bereitgestellte Bandbreite ist die Summe der Bandbreite aller Leitungen in der ExpressRoute Direct-Ressource, und die belegte Bandbreite ist die physische Nutzung der zugrunde liegenden physischen Schnittstellen.

Es gibt zusätzliche Leitungsbandbreiten, die für ExpressRoute Direct nur zur Unterstützung der oben beschriebenen Szenarien genutzt werden können. Hierbei handelt es sich um 40 Gbit/s und 100 Gbit/s.

Standard- oder Premium-Leitungen können erstellt werden. Standardleitungen sind in den Kosten enthalten, während Premium-Leitungen Kosten auf Basis der gewählten Bandbreite verursachen. Leitungen können nur in gemessener Form erstellt werden, da Unbegrenztheit von ExpressRoute Direct nicht unterstützt wird.

Erstellen Sie in der ExpressRoute-Direct-Ressource eine Leitung.

  ```azurecli
  az network express-route create --express-route-port "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct" -n "Contoso-Direct-ckt" -g "Contoso-Direct-rg" --sku-family MeteredData --sku-tier Standard --bandwidth 100 Gbps
  ```

  Andere Bandbreiten entsprechen 5 GBit/s, 10 GBit/s und 40 GBit/s.

  **Beispielausgabe:**

  ```azurecli
  {
  "allowClassicOperations": false,
  "allowGlobalReach": false,
  "authorizations": [],
  "bandwidthInGbps": 100.0,
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"<etagnumber>\"",
  "expressRoutePort": {
    "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
    "resourceGroup": "Contoso-Direct-rg"
  },
  "gatewayManagerEtag": "",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRouteCircuits/ERDirect-ckt-cli",
  "location": "westus",
  "name": "ERDirect-ckt-cli",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "Contoso-Direct-rg",
  "serviceKey": "<serviceKey>",
  "serviceProviderNotes": null,
  "serviceProviderProperties": null,
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "MeteredData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "stag": null,
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits"
  }  
  ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu ExpressRoute Direct finden Sie in der [Übersicht](expressroute-erdirect-about.md).
