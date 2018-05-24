---
title: 'Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure CLI | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die Azure CLI zum Erstellen einer Azure Application Gateway-Instanz verwenden, mit der Webdatenverkehr an virtuelle Computer in einem Back-End-Pool geleitet wird.
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: quickstart
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 99c3975c6ab2c7a20dfbab519dae575a2a61465f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure CLI

Per Azure Application Gateway können Sie Ihren Webdatenverkehr für Anwendungen an bestimmte Ressourcen leiten, indem Sie Ports Listener zuweisen, Regeln erstellen und Ressourcen einem Back-End-Pool hinzufügen.

In dieser Schnellstartanleitung wird gezeigt, wie Sie die Azure CLI zur schnellen Erstellung des Anwendungsgateways mit zwei virtuellen Computern im Back-End-Pool erstellen. Anschließend führen Sie einen Test durch, um sicherzustellen, dass alles richtig funktioniert.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens Version 2.0.4 der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Sie müssen Ressourcen immer innerhalb einer Ressourcengruppe erstellen. Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupAG* am Standort *eastus* erstellt.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen 

Sie müssen ein virtuelles Netzwerk für das Anwendungsgateway erstellen, um mit anderen Ressourcen kommunizieren zu können. Sie können ein virtuelles Netzwerk zum gleichen Zeitpunkt erstellen wie das Anwendungsgateway. In diesem Beispiel werden zwei Subnetze erstellt: eins für das Anwendungsgateway und das andere für die virtuellen Computer. 

Erstellen Sie das virtuelle Netzwerk und das Subnetz mit [az network vnet create](/cli/azure/vnet#az_vnet_create). Erstellen Sie mit [az network public-ip create](/cli/azure/public-ip#az_public_ip_create) die öffentliche IP-Adresse.

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
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Beispiel erstellen Sie zwei virtuelle Computer, die als Back-End-Server für das Anwendungsgateway verwendet werden. 

### <a name="create-two-virtual-machines"></a>Erstellen von zwei virtuellen Computern

Sie installieren außerdem NGINX auf den virtuellen Computern, um zu überprüfen, ob das Anwendungsgateway erfolgreich erstellt wurde. Mithilfe einer cloud-init-Konfigurationsdatei können Sie NGINX installieren und eine Node.js-App vom Typ „Hallo Welt“ auf einem virtuellen Linux-Computer ausführen. 

Erstellen Sie in der aktuellen Shell eine Datei namens „cloud-init.txt“, kopieren Sie die folgende Konfiguration, und fügen Sie sie in die Shell ein. Stellen Sie sicher, dass die gesamte cloud-init-Datei ordnungsgemäß kopiert wird, insbesondere die erste Zeile:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

Erstellen Sie die Netzwerkschnittstellen mit [az network nic create](/cli/azure/network/nic#az_network_nic_create). Erstellen Sie die virtuellen Computer mit [az vm create](/cli/azure/vm#az_vm_create).

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupAG \
    --name myNic$i \
    --vnet-name myVNet \
    --subnet myBackendSubnet
  az vm create \
    --resource-group myResourceGroupAG \
    --name myVM$i \
    --nics myNic$i \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
done
```

## <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Erstellen Sie mit [az network application-gateway create](/cli/azure/application-gateway#az_application_gateway_create) ein Anwendungsgateway. Wenn Sie über die Azure-Befehlszeilenschnittstelle ein Anwendungsgateway erstellen, geben Sie Konfigurationsinformationen wie Kapazität, SKU und HTTP-Einstellungen an. Die privaten IP-Adressen der Netzwerkschnittstellen werden als Server im Back-End-Pool des Anwendungsgateways hinzugefügt.

```azurecli-interactive
address1=$(az network nic show --name myNic1 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
address2=$(az network nic show --name myNic2 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Enabled \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$address1" "$address2"
```

Es kann bis zu 30 Minuten dauern, bis das Anwendungsgateway erstellt wird. Nachdem das Anwendungsgateway erstellt wurde, sehen Sie diese Features:

- *appGatewayBackendPool*: Ein Anwendungsgateway muss über mindestens einen Back-End-Adresspool verfügen.
- *appGatewayBackendHttpSettings*: Gibt an, dass zur Kommunikation Port 80 und ein HTTP-Protokoll verwendet werden.
- *appGatewayHttpListener*: Der Standardlistener, der *appGatewayBackendPool* zugeordnet ist.
- *appGatewayFrontendIP*: Hiermit wird *myAGPublicIPAddress* zu *appGatewayHttpListener* zugewiesen.
- *rule1*:Standardroutingregel, die *appGatewayHttpListener* zugeordnet ist

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Die Installation von NGINX ist für die Erstellung des Anwendungsgateways nicht erforderlich. Sie haben die Installation in dieser Schnellstartanleitung aber durchgeführt, um zu überprüfen, ob das Anwendungsgateway erfolgreich erstellt wurde. Um die öffentliche IP-Adresse des Anwendungsgateways abzurufen, verwenden Sie [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show). Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
``` 

![Testen des Anwendungsgateways](./media/quick-create-cli/application-gateway-nginxtest.png)

Wenn Sie den Browser aktualisieren, sollte der Name des anderen virtuellen Computers angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Untersuchen Sie zuerst die Ressourcen, die mit dem Anwendungsgateway erstellt wurden. Sie können dann den Befehl [az group delete](/cli/azure/group#az_group_delete) verwenden, um die Ressourcengruppe, das Anwendungsgateway und alle dazugehörigen Ressourcen zu entfernen, wenn diese nicht mehr benötigt werden.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```
 
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Manage web traffic with an application gateway using the Azure CLI](./tutorial-manage-web-traffic-cli.md) (Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure CLI)

