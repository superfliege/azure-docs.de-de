---
title: Erstellen eines internen Load Balancers im Tarif „Basic“ – Azure CLI 2.0 | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit Azure CLI 2.0 einen internen Load Balancer erstellen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2017
ms.author: kumud
ms.openlocfilehash: a4093926ea2ea2bb0e477372a1ceb2dfbf22e8f0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "36330967"
---
# <a name="create-an-internal-load-balancer-to-load-balance-vms-using-azure-cli-20"></a>Erstellen eines internen Load Balancers für den Lastenausgleich virtueller Computer mit Azure CLI 2.0

In diesem Artikel wird veranschaulicht, wie Sie einen internen Load Balancer erstellen, um einen Lastenausgleich für VMs durchzuführen. Zum Testen des Lastenausgleichs stellen Sie zwei virtuelle Computer (VMs) mit Ubuntu-Server zum Durchführen eines Lastenausgleichs für eine Web-App bereit.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens die Azure CLI-Version 2.0.28 ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [az group create](https://docs.microsoft.com/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupILB* am Standort *eastus* erstellt:

```azurecli-interactive
  az group create \
    --name myResourceGroupILB \
    --location eastus
```
## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie ein virtuelles Netzwerk mit dem Namen *myVnet* und dem Subnetz *mySubnet* in *myResourceGroup*, indem Sie den Befehl [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#create) verwenden.

```azurecli-interactive
  az network vnet create \
    --name myVnet \
    --resource-group myResourceGroupILB \
    --location eastus \
    --subnet-name mySubnet
```
## <a name="create-basic-load-balancer"></a>Erstellen eines Load Balancers im Tarif „Basic“

In diesem Abschnitt wird erläutert, wie Sie die folgenden Komponenten des Load Balancers erstellen und konfigurieren können:
  - Front-End-IP-Konfiguration, über die der eingehende Netzwerkdatenverkehr für den Lastenausgleich empfangen wird
  - Back-End-IP-Pool, an den der Front-End-Pool den Netzwerkdatenverkehr sendet, für den ein Lastenausgleich durchgeführt wurde
  - Integritätstest zum Ermitteln der Integrität der Back-End-VM-Instanzen
  - Lastenausgleichsregel, mit der definiert wird, wie Datenverkehr auf die virtuellen Computer verteilt werden soll

### <a name="create-the-load-balancer"></a>Erstellen des Load Balancers

Erstellen Sie mit [az network lb create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest#create) einen öffentlichen Load Balancer im Tarif „Basic“ mit dem Namen **myLoadBalancer**, der über eine Front-End-IP-Konfiguration mit dem Namen **myFrontEnd** und den Back-End-Pool **myBackEndPool** (mit Zuordnung der privaten IP-Adresse „10.0.0.7“) verfügt.

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupILB \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEnd \
    --private-ip-address 10.0.0.7 \
    --backend-pool-name myBackEndPool \
    --vnet-name myVnet \
    --subnet mySubnet      
  ```
### <a name="create-the-health-probe"></a>Erstellen des Integritätstests

Ein Integritätstest überprüft alle VM-Instanzen, um sicherzustellen, dass diese Netzwerkdatenverkehr empfangen können. VM-Instanzen mit Fehlern beim Test werden aus dem Load Balancer entfernt, bis sie wieder online geschaltet werden und beim Test überprüft wurde, dass sie fehlerfrei sind. Erstellen Sie mit [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest#create) einen Integritätstest zur Überwachung der Integrität von virtuellen Computern. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroupILB \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-the-load-balancer-rule"></a>Erstellen der Lastenausgleichsregel

Mit einer Lastenausgleichsregel wird die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport definiert. Erstellen Sie mit [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#create) eine Lastenausgleichsregel mit dem Namen *myLoadBalancerRuleWeb*, die an Port 80 des Front-End-Pools *myFrontEndPool* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *myBackEndPool* sendet, wobei ebenfalls der Port 80 verwendet wird. 

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroupILB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe  
```

## <a name="create-servers-for-the-backend-address-pool"></a>Erstellen von Servern für den Back-End-Adresspool

Vor der Bereitstellung mehrerer virtueller Computer und dem Testen des Lastenausgleichs müssen Sie zunächst die unterstützenden virtuellen Netzwerkressourcen erstellen.

###  <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe
Erstellen Sie eine Netzwerksicherheitsgruppe, um eingehende Verbindungen für Ihr virtuelles Netzwerk zu definieren.

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupILB \
    --name myNetworkSecurityGroup
```

### <a name="create-a-network-security-group-rule"></a>Erstellen einer Netzwerksicherheitsgruppen-Regel

Erstellen Sie eine Netzwerksicherheitsgruppen-Regel, um eingehende Verbindungen über Port 80 zuzulassen.

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupILB \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleHTTP \
    --protocol tcp \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 300
```
### <a name="create-nics"></a>Erstellen von NICs

Erstellen Sie mit [az network nic create](/cli/azure/network/nic#az_network_nic_create) zwei Netzwerkschnittstellen, und ordnen Sie sie der privaten IP-Adresse und der Netzwerksicherheitsgruppe zu. 

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupILB \
    --name myNic$i \
    --vnet-name myVnet \
    --subnet mySubnet \
    --network-security-group myNetworkSecurityGroup \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
done
```

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Beispiel erstellen Sie zwei virtuelle Computer, die als Back-End-Server für den Lastenausgleich verwendet werden. Außerdem installieren Sie NGINX auf den virtuellen Computern, um zu überprüfen, ob der Lastenausgleich erfolgreich erstellt wurde.

### <a name="create-an-availability-set"></a>Erstellen einer Verfügbarkeitsgruppe

Erstellen Sie mit [az vm availabilityset create](/cli/azure/network/nic#az_network_availabilityset_create) eine Verfügbarkeitsgruppe.

 ```azurecli-interactive
  az vm availability-set create \
    --resource-group myResourceGroupILB \
    --name myAvailabilitySet
```

### <a name="create-two-virtual-machines"></a>Erstellen von zwei virtuellen Computern

Mithilfe einer cloud-init-Konfigurationsdatei können Sie NGINX installieren und eine Node.js-App vom Typ „Hallo Welt“ auf einem virtuellen Linux-Computer ausführen. Erstellen Sie in der aktuellen Shell eine Datei namens „cloud-init.txt“, kopieren Sie die folgende Konfiguration, und fügen Sie sie in die Shell ein. Stellen Sie sicher, dass die gesamte cloud-init-Datei ordnungsgemäß kopiert wird, insbesondere die erste Zeile:

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
 
Erstellen Sie die virtuellen Computer mit [az vm create](/cli/azure/vm#az_vm_create).

 ```azurecli-interactive
for i in `seq 1 2`; do
  az vm create \
    --resource-group myResourceGroupILB \
    --name myVM$i \
    --availability-set myAvailabilitySet \
    --nics myNic$i \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
    done
```
Es kann einige Minuten dauern, bis die virtuellen Computer bereitgestellt wurden.

### <a name="create-a-vm-for-testing-the-load-balancer"></a>Erstellen einer VM zum Testen des Lastenausgleichs

Erstellen Sie zum Testen des Lastenausgleichs den virtuellen Computer *myVMTest*, und ordnen Sie ihn *myNic3* zu.

```azurecli-interactive
 az vm create \
    --resource-group myResourceGroupILB \
    --name myVMTest \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword123456!
```

## <a name="test-the-internal-load-balancer"></a>Testen des internen Lastenausgleichs

Zum Testen des Lastenausgleichs müssen Sie zuerst die private IP-Adresse des Lastenausgleichs beschaffen. Melden Sie sich als Nächstes am virtuellen Computer myVMTest an, und geben Sie die private IP-Adresse in die Adressleiste des Webbrowsers ein.

Verwenden Sie zum Abrufen der privaten IP-Adresse des Lastenausgleichs den Befehl [az network lb show](/cli/azure/network/public-ip##az-network-lb-show). Kopieren Sie die private IP-Adresse, und fügen Sie sie in die Adressleiste eines Webbrowsers Ihres virtuellen Computers ein (*myVMTest*).

```azurecli-interactive
  az network lb show \
    --name myLoadBalancer \
    --resource-group myResourceGroupILB
``` 
![Testen des Load Balancers](./media/load-balancer-get-started-ilb-arm-cli/load-balancer-test.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Mit dem Befehl [az group delete](/cli/azure/group#az_group_delete) können Sie die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen entfernen, wenn Sie sie nicht mehr benötigen.

```azurecli-interactive 
  az group delete --name myResourceGroupILB
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie einen internen Load Balancer im Tarif „Basic“ erstellt, virtuelle Computer angefügt, eine Datenverkehrsregel für den Lastenausgleich sowie einen Integritätstest konfiguriert und den Lastenausgleich getestet. Weitere Informationen zum Lastenausgleich und zu den dazugehörigen Ressourcen finden Sie in den Anleitungsartikeln.
