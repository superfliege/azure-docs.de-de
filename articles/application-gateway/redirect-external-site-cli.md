---
title: Erstellen eines Anwendungsgateways mit externer Umleitung – Azure CLI | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe der Azure CLI ein Anwendungsgateway erstellen, das internen Webdatenverkehr an den entsprechenden Pool umleitet.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: victorh
ms.openlocfilehash: ea4fd94a00e1d22021417373da0b3fcffea4d120
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2018
ms.locfileid: "42141943"
---
# <a name="create-an-application-gateway-with-external-redirection-using-the-azure-cli"></a>Erstellen eines Anwendungsgateways mit externer Umleitung durch die Azure CLI

Sie können mit der Azure CLI [eine Umleitung des Webdatenverkehrs](multiple-site-overview.md) konfigurieren, wenn Sie ein [Anwendungsgateway](overview.md) erstellen. In diesem Tutorial konfigurieren Sie einen Listener und eine Regel, die den am Anwendungsgateway eingehenden Webdatenverkehr an eine externe Website umleitet.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Einrichten des Netzwerks
> * Erstellen eines Listeners und einer Umleitungsregel
> * Erstellen eines Anwendungsgateways

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupAG* am Standort *eastus* erstellt.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen 

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az-net) ein virtuelles Netzwerk namens *myVNet* und ein Subnetz namens *myAGSubnet*. Erstellen Sie mit [az network public-ip create](/cli/azure/network/public-ip#az-network_public_ip_create) eine öffentliche IP-Adresse namens *myAGPublicIPAddress*. Diese Ressourcen werden verwendet, um Netzwerkkonnektivität für das Anwendungsgateway und die zugehörigen Ressourcen bereitzustellen.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Sie können [az network application-gateway create](/cli/azure/network/application-gateway#create) verwenden, um ein Anwendungsgateway namens *myAppGateway* zu erstellen. Wenn Sie über die Azure CLI ein Anwendungsgateway erstellen, geben Sie Konfigurationsinformationen wie z.B. Kapazität, SKU und HTTP-Einstellungen an. Das Anwendungsgateway wird dem Subnetz *myAGSubnet* und der IP-Adresse *myPublicIPSddress* zugewiesen, das bzw. die Sie zuvor erstellt haben. 

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
  --frontend-port 8080 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

Es kann einige Minuten dauern, bis das Anwendungsgateway erstellt wird. Nachdem das Anwendungsgateway erstellt wurde, sehen Sie diese neuen Features:

- *appGatewayBackendPool*: Ein Anwendungsgateway muss über mindestens einen Back-End-Adresspool verfügen.
- *appGatewayBackendHttpSettings*: Gibt an, dass zur Kommunikation Port 80 und ein HTTP-Protokoll verwendet werden.
- *appGatewayHttpListener*: Der Standardlistener, der *appGatewayBackendPool* zugeordnet ist.
- *appGatewayFrontendIP*: Hiermit wird *myAGPublicIPAddress* zu *appGatewayHttpListener* zugewiesen.
- *rule1*:Die Standardroutingregel, die *appGatewayHttpListener* zugeordnet ist.

### <a name="add-the-redirection-configuration"></a>Hinzufügen der Umleitungskonfiguration

Fügen Sie mit [az network application-gateway redirect-config create](/cli/azure/network/application-gateway/redirect-config#az-network_application_gateway_redirect_config_create) die Umleitungskonfiguration hinzu, die Datenverkehr von *www.contoso.org* an den Listener für *www.contoso.com* im Anwendungsgateway sendet.

```azurecli-interactive
az network application-gateway redirect-config create \
  --name myredirect \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Temporary \
  --target-url "http://bing.com"
```

### <a name="add-a-listener-and-routing-rule"></a>Hinzufügen eines Listeners und einer Routingregel

Ein Listener ist erforderlich, damit das Anwendungsgateway Datenverkehr in geeigneter Weise weiterleiten kann. Erstellen Sie mit [az network application-gateway http-listener create](/cli/azure/network/application-gateway#az-network_application_gateway_http_listener_create) den Listener, bei dem der Front-End-Port mit [az network application-gateway frontend-port create](/cli/azure/network/application-gateway#az-network_application_gateway_frontend_port_create) erstellt wurde. Für den Listener ist eine Regel erforderlich, damit bekannt ist, an welches Ziel eingehender Datenverkehr gesendet werden soll. Erstellen Sie mit [az network application-gateway rule create](/cli/azure/network/application-gateway#az-network_application_gateway_rule_create) eine grundlegende Regel namens *redirectRule*.

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name redirectPort
az network application-gateway http-listener create \
  --name redirectListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port redirectPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectListener \
  --rule-type Basic \
  --redirect-config myredirect
```

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Um die öffentliche IP-Adresse des Anwendungsgateways abzurufen, können Sie [az network public-ip show](/cli/azure/network/public-ip#az-network_public_ip_show) verwenden. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.

Daraufhin sollte *bing.com* in Ihrem Browser aufgerufen werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> * Einrichten des Netzwerks
> * Erstellen eines Listeners und einer Umleitungsregel
> * Erstellen eines Anwendungsgateways