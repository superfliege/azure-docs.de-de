---
title: 'Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure CLI | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die Azure CLI zum Erstellen einer Azure Application Gateway-Instanz verwenden, mit der Webdatenverkehr an virtuelle Computer in einem Back-End-Pool geleitet wird.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/8/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: d14b8c9c752c9d41a42f092662c5f3aa88840dc5
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54157716"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway: Azure CLI

In diesem Schnellstart wird gezeigt, wie Sie über die Azure-Befehlszeilenschnittstelle (Azure CLI) schnell ein Anwendungsgateway mit zwei VMs im Back-End-Pool erstellen. Anschließend führen Sie einen Test durch, um sicherzustellen, dass alles richtig funktioniert. Per Azure Application Gateway leiten Sie den Webdatenverkehr Ihrer Anwendungen an bestimmte Ressourcen, indem Sie Ports Listener zuweisen, Regeln erstellen und Ressourcen einem Back-End-Pool hinzufügen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie mindestens die Azure CLI-Version 2.0.4 ausführen. Führen Sie **az --version** aus, um die Version festzustellen. Informationen zum Installieren oder Aktualisieren der Azure CLI finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

In Azure ordnen Sie verwandte Ressourcen einer Ressourcengruppe zu. Erstellen Sie mit [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupAG* am Standort *eastus* erstellt.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen 

Wenn Sie ein virtuelles Netzwerk erstellen, kann das Anwendungsgateway mit anderen Ressourcen kommunizieren. Sie können ein virtuelles Netzwerk zum gleichen Zeitpunkt erstellen wie das Anwendungsgateway. In diesem Beispiel erstellen Sie zwei Subnetze: eins für das Anwendungsgateway und das andere für die VMs. Das Subnetz für das Anwendungsgateway kann nur Anwendungsgateways enthalten. Andere Ressourcen sind nicht zulässig.

Erstellen Sie das virtuelle Netzwerk und das Subnetz mit [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). Führen Sie [az network public-ip create](/cli/azure/network/public-ip#az-public-ip-create) aus, um die öffentliche IP-Adresse zu erstellen.

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

In diesem Beispiel erstellen Sie zwei VMs, die von Azure als Back-End-Server für das Anwendungsgateway verwendet werden. 

### <a name="create-two-virtual-machines"></a>Erstellen von zwei virtuellen Computern

Sie installieren außerdem den [NGINX-Webserver](https://docs.nginx.com/nginx/) auf den VMs, um zu überprüfen, ob das Anwendungsgateway erfolgreich erstellt wurde. Sie können eine cloud-init-Konfigurationsdatei verwenden, um NGINX zu installieren und eine Node.js-App „Hallo Welt“ auf einer Linux-VM auszuführen. Weitere Informationen zu cloud-init finden Sie unter [cloud-init-Unterstützung für VMs in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init).

Kopieren Sie in Ihrer Azure Cloud Shell die folgende Konfiguration, und fügen Sie sie in eine Datei mit dem Namen *cloud-init.txt* ein. Geben Sie *editor cloud-init.txt* ein, um die Datei zu erstellen.

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

Erstellen Sie die Netzwerkschnittstellen mit [az network nic create](/cli/azure/network/nic#az-network-nic-create). Zum Erstellen der VMs verwenden Sie [az vm create](/cli/azure/vm#az-vm-create).

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

Erstellen Sie mit [az network application-gateway create](/cli/azure/network/application-gateway#az-application-gateway-create) ein Anwendungsgateway. Wenn Sie über die Azure CLI ein Anwendungsgateway erstellen, geben Sie Konfigurationsinformationen wie Kapazität, SKU und HTTP-Einstellungen an. Azure fügt dann die privaten IP-Adressen der Netzwerkschnittstellen als Server im Back-End-Pool des Anwendungsgateways hinzu.

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

Die Erstellung des Anwendungsgateways in Azure kann bis zu 30 Minuten in Anspruch nehmen. Nachdem das Anwendungsgateway erstellt wurde, können Sie die folgenden Einstellungen im Abschnitt **Einstellungen** der Seite **Anwendungsgateway** anzeigen:

- **appGatewayBackendPool**: Diese Einstellung finden Sie auf der Seite **Back-End-Pools**. Sie gibt den erforderlichen Back-End-Pool an.
- **appGatewayBackendHttpSettings**: Diese Einstellung finden Sie auf der Seite **HTTP-Einstellungen**. Sie gibt an, dass das Anwendungsgateway den Port 80 und das HTTP-Protokoll für die Kommunikation verwendet.
- **appGatewayHttpListener**: Diese Einstellung finden Sie auf der Seite **Listener**. Sie gibt den Standardlistener an, der **appGatewayBackendPool** zugeordnet ist.
- **appGatewayFrontendIP**: Diese Einstellung finden Sie auf der Seite **Front-End-IP-Konfigurationen**. Sie weist *myAGPublicIPAddress* zu **appGatewayHttpListener** zu.
- **rule1**: Diese Einstellung finden Sie auf der Seite **Regeln**. Sie gibt die Standardroutingregel an, die **appGatewayHttpListener** zugeordnet ist.

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

Für die Erstellung des Anwendungsgateways in Azure ist kein NGINX-Webserver erforderlich. Sie haben ihn in diesem Schnellstart installiert, um zu überprüfen, ob Azure das Anwendungsgateway erfolgreich erstellt hat. Verwenden Sie [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show), um die öffentliche IP-Adresse des neuen Anwendungsgateways abzurufen. 

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
``` 

Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.
    
![Testen des Anwendungsgateways](./media/quick-create-cli/application-gateway-nginxtest.png)

Wenn Sie den Browser aktualisieren, sollte der Name der zweiten VM angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Anwendungsgateway erstellten Ressourcen nicht mehr benötigen, entfernen Sie die Ressourcengruppe mit dem Befehl [az group delete](/cli/azure/group#az-group-delete). Beim Entfernen der Ressourcengruppe werden auch das Anwendungsgateway und alle zugehörigen Ressourcen entfernt.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```
 
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten von Webdatenverkehr mit einem Anwendungsgateway per Azure CLI](./tutorial-manage-web-traffic-cli.md)

