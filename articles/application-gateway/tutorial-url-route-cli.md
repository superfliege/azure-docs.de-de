---
title: Tutorial – Weiterleiten von Webdatenverkehr basierend auf der URL – Azure-CLI
description: Erfahren Sie, wie Sie Webdatenverkehr basierend auf der URL mithilfe der Azure CLI an spezifische skalierbare Pools von Servern weiterleiten.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: a774d491de4ca1dfdb96181ff13fa644a061cd65
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434213"
---
# <a name="tutorial-route-web-traffic-based-on-the-url-using-the-azure-cli"></a>Tutorial: Weiterleiten von Webdatenverkehr basierend auf der URL mit der Azure CLI

Sie können mit der Azure CLI Webdatenverkehrsrouting zu bestimmten skalierbaren Serverpools basierend auf der URL konfigurieren, die für den Zugriff auf Ihre Anwendung verwendet wird. In diesem Tutorial erstellen Sie ein [Azure Application Gateway](application-gateway-introduction.md) mit drei Back-End-Pools mithilfe von [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Jeder der Back-End-Pools dient einem bestimmten Zweck, z.B. allgemeine Daten, Bilder und Videos.  Routing des Datenverkehrs zu separaten Pools stellt sicher, dass Ihre Kunden die Informationen zu dem Zeitpunkt erhalten, zu dem sie sie benötigen.

Um das Datenverkehrsrouting zu ermöglichen, erstellen Sie [Routingregeln](application-gateway-url-route-overview.md), die Listenern zugewiesen werden, die auf bestimmte Ports lauschen, um sicherzustellen, dass Webdatenverkehr auf den entsprechenden Servern in den Pools eingeht.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten des Netzwerks
> * Erstellen von Listenern, URL-Pfadzuordnung und Regeln
> * Erstellen von skalierbaren Back-End-Pools


![URL-Routingbeispiel](./media/tutorial-url-route-cli/scenario.png)

Sie können dieses Tutorial auch mit [Azure PowerShell](tutorial-url-route-powershell.md) durcharbeiten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupAG* am Standort *eastus* erstellt.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen 

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az-net) ein virtuelles Netzwerk namens *myVNet* und ein Subnetz namens *myAGSubnet*. Fügen Sie dann ein Subnetz namens *myBackendSubnet* hinzu, das von den Back-End-Servern mit [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network_vnet_subnet_create) benötigt wird. Erstellen Sie mit [az network public-ip create](/cli/azure/public-ip#az-network_public_ip_create) eine öffentliche IP-Adresse namens *myAGPublicIPAddress*.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24

az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24

az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-the-application-gateway-with-url-map"></a>Erstellen des Anwendungsgateways mit URL-Zuordnung

Erstellen Sie mit [az network application-gateway create](/cli/azure/application-gateway#create) ein Anwendungsgateway namens *myAppGateway*. Wenn Sie über die Azure-Befehlszeilenschnittstelle ein Anwendungsgateway erstellen, geben Sie Konfigurationsinformationen wie Kapazität, SKU und HTTP-Einstellungen an. Das Anwendungsgateway wird dem Subnetz *myAGSubnet* und der IP-Adresse *myAGPublicIPAddress* zugewiesen, das bzw. die Sie zuvor erstellt haben. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

 Es kann einige Minuten dauern, bis das Anwendungsgateway erstellt wird. Nachdem das Anwendungsgateway erstellt wurde, sehen Sie diese neuen Features:

- *appGatewayBackendPool*: Ein Anwendungsgateway muss über mindestens einen Back-End-Adresspool verfügen.
- *appGatewayBackendHttpSettings*: Gibt an, dass zur Kommunikation Port 80 und ein HTTP-Protokoll verwendet werden.
- *appGatewayHttpListener*: Der Standardlistener, der *appGatewayBackendPool* zugeordnet ist.
- *appGatewayFrontendIP*: Hiermit wird *myAGPublicIPAddress* zu *appGatewayHttpListener* zugewiesen.
- *rule1*: Die Standardroutingregel, die *appGatewayHttpListener* zugeordnet ist.


### <a name="add-image-and-video-backend-pools-and-port"></a>Hinzufügen von Back-End-Pools und Port für Images und Videos

Fügen Sie Back-End-Pools namens *imagesBackendPool* und *videoBackendPool* Ihrem Anwendungsgateway hinzu. Verwenden Sie dazu [az network application-gateway address-pool create](/cli/azure/application-gateway#az-network_application_gateway_address-pool_create). Den Front-End-Port für die Pools fügen Sie mit [az network application-gateway frontend-port create](/cli/azure/application-gateway#az-network_application_gateway_frontend_port_create) hinzu. 

```azurecli-interactive
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name imagesBackendPool

az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name videoBackendPool

az network application-gateway frontend-port create \
  --port 8080 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name port8080
```

### <a name="add-backend-listener"></a>Hinzufügen eines Back-End-Listeners

Fügen Sie mit [az network application-gateway http-listener create](/cli/azure/application-gateway#az-network_application_gateway_http_listener_create) den Back-End-Listener namens *backendListener* hinzu, der zum Weiterleiten von Datenverkehr erforderlich ist.


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port port8080 \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-url-path-map"></a>Hinzufügen einer URL-Pfadzuordnung

URL-Pfadzuordnungen stellen sicher, dass bestimmte URLs an bestimmte Back-End-Pools weitergeleitet werden. Erstellen Sie die URL-Pfadzuordnungen *imagePathRule* und *videoPathRule* mithilfe der Befehle [az network application-gateway url-path-map create](/cli/azure/application-gateway#az-network_application_gateway_url_path_map_create) und [az network application-gateway url-path-map rule create](/cli/azure/application-gateway#az-network_application_gateway_url_path_map_rule_create).

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name myPathMap \
  --paths /images/* \
  --resource-group myResourceGroupAG \
  --address-pool imagesBackendPool \
  --default-address-pool appGatewayBackendPool \
  --default-http-settings appGatewayBackendHttpSettings \
  --http-settings appGatewayBackendHttpSettings \
  --rule-name imagePathRule

az network application-gateway url-path-map rule create \
  --gateway-name myAppGateway \
  --name videoPathRule \
  --resource-group myResourceGroupAG \
  --path-map-name myPathMap \
  --paths /video/* \
  --address-pool videoBackendPool
```

### <a name="add-routing-rule"></a>Hinzufügen einer Routingregel

Die Routingregel ordnet die URL-Zuordnungen dem von Ihnen erstellten Listener zu. Fügen Sie mit dem Befehl [az network application-gateway rule create](/cli/azure/application-gateway#az-network_application_gateway_rule_create) die Regel *rule2* hinzu.

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name rule2 \
  --resource-group myResourceGroupAG \
  --http-listener backendListener \
  --rule-type PathBasedRouting \
  --url-path-map myPathMap \
  --address-pool appGatewayBackendPool
```

## <a name="create-virtual-machine-scale-sets"></a>Erstellen von VM-Skalierungsgruppen

In diesem Tutorial erstellen Sie drei VM-Skalierungsgruppen, die die drei von Ihnen erstellten Back-End-Pools unterstützen. Sie erstellen Skalierungsgruppen namens *myvmss1*, *myvmss2* und *myvmss3*. Jede Skalierungsgruppe enthält zwei VM-Instanzen, auf denen Sie NGINX installieren.

```azurecli-interactive
for i in `seq 1 3`; do

  if [ $i -eq 1 ]
  then
    poolName="appGatewayBackendPool" 
  fi

  if [ $i -eq 2 ]
  then
    poolName="imagesBackendPool"
  fi

  if [ $i -eq 3 ]
  then
    poolName="videoBackendPool"
  fi

  az vmss create \
    --name myvmss$i \
    --resource-group myResourceGroupAG \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password Azure123456! \
    --instance-count 2 \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
    --vm-sku Standard_DS2 \
    --upgrade-policy-mode Automatic \
    --app-gateway myAppGateway \
    --backend-pool-name $poolName
done
```

### <a name="install-nginx"></a>Installieren von NGINX

```azurecli-interactive
for i in `seq 1 3`; do
  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Um die öffentliche IP-Adresse des Anwendungsgateways abzurufen, verwenden Sie [az network public-ip show](/cli/azure/network/public-ip#az-network_public_ip_show). Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Beispiel: *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm* oder *http://40.121.222.19:8080/video/test.htm*.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Testen der Basis-URL im Anwendungsgateway](./media/tutorial-url-route-cli/application-gateway-nginx.png)

Ändern Sie die URL zu http://&lt;IP-Adresse&gt;:8080/images/test.html, und ersetzen Sie dabei &lt;IP-Adresse&gt; durch Ihre IP-Adresse. Die anschließende Anzeige sollte in etwa wie im folgenden Beispiel aussehen:

![Testen der Images-URL im Anwendungsgateway](./media/tutorial-url-route-cli/application-gateway-nginx-images.png)

Ändern Sie die URL zu http://&lt;IP-Adresse&gt;:8080/video/test.html, und ersetzen Sie dabei &lt;IP-Adresse&gt; durch Ihre IP-Adresse. Die anschließende Anzeige sollte in etwa wie im folgenden Beispiel aussehen:

![Testen der Video-URL im Anwendungsgateway](./media/tutorial-url-route-cli/application-gateway-nginx-video.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie die Ressourcengruppe, das Anwendungsgateway und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen.

```azurecli-interactive
az group delete --name myResourceGroupAG --location eastus
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Einrichten des Netzwerks
> * Erstellen von Listenern, URL-Pfadzuordnung und Regeln
> * Erstellen von skalierbaren Back-End-Pools

> [!div class="nextstepaction"]
> [Erstellen eines Anwendungsgateways mit Umleitung auf URL-Pfadbasis](./tutorial-url-redirect-cli.md)
