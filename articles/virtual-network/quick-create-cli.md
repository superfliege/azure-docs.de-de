---
title: "Erstellen eines virtuellen Netzwerks – Azure CLI | Microsoft-Dokumentation"
description: "Erfahren Sie schnell, wie Sie ein virtuelles Netzwerk über die Azure-Befehlszeilenschnittstelle erstellen. In einem virtuellen Netzwerk können viele Arten von Azure-Ressourcen privat miteinander kommunizieren."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 792b92731f89f3d0bab4f23221223e469ddf9550
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Erstellen eines virtuellen Netzwerks über die Azure-Befehlszeilenschnittstelle

In diesem Artikel erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen. Nachdem Sie ein virtuelles Netzwerk erstellt haben, stellen Sie zwei virtuelle Computer im virtuellen Netzwerk bereit, um die private Netzwerkkommunikation zwischen ihnen zu testen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle (CLI) lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*. Alle Azure-Ressourcen werden an einem Azure-Standort (oder in einer Region) erstellt.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit dem Befehl [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen *myVirtualNetwork* und dem Subnetz *default* erstellt. Da kein Standort angegeben ist, erstellt Azure das virtuelle Netzwerk am selben Standort wie die Ressourcengruppe.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

Nach dem Erstellen des virtuellen Netzwerks lautet ein Teil der zurückgegebenen Informationen wie folgt.

```azurecli
"newVNet": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/16"
```

Jedem virtuellen Netzwerk ist mindestens ein Adresspräfix zugewiesen. Da beim Erstellen des virtuellen Netzwerks kein Adresspräfix angegeben wurde, definiert Azure den Adressraum standardmäßig als 10.0.0.0/16. Der Adressraum wird in CIDR-Notation angegeben. Die Adressraum 10.0.0.0/16 umfasst die Adressen 10.0.0.0 bis 10.0.255.254.

Ein anderer Teil der zurückgegebenen Informationen ist der **addressPrefix** von *10.0.0.0/24* für das im Befehl angegebene Subnetz *default*. Ein virtuelles Netzwerk enthält null oder mehr Subnetze. Mit dem Befehl wurde ein einzelnes Subnetz namens *default* erstellt, es wurde jedoch kein Adresspräfix für das Subnetz angegeben. Wenn für ein virtuelles Netzwerk oder Subnetz kein Adresspräfix angegeben wird, definiert Azure standardmäßig 10.0.0.0/24 als Adresspräfix für das erste Subnetz. Daher umfasst das Subnetz 10.0.0.0 bis 10.0.0.254, es sind jedoch nur 10.0.0.4 bis 10.0.0.254 verfügbar, da Azure die ersten vier Adressen (0 bis 3) und die letzte Adresse in jedem Subnetz reserviert.

## <a name="test-network-communication"></a>Testen der Netzwerkkommunikation

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen privat miteinander kommunizieren. Eine Art von Ressource, die Sie in einem virtuellen Netzwerk bereitstellen können, ist ein virtueller Computer. Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie die private Kommunikation zwischen ihnen in einem späteren Schritt überprüfen können.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie mit dem Befehl [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. Im folgenden Beispiel wird ein virtueller Computer namens *myVm1* erstellt. Wenn SSH-Schlüssel nicht bereits an einem Standardschlüsselspeicherort vorhanden sind, werden sie durch den Befehl erstellt. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`. Mit der Option `--no-wait` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

Azure erstellt den virtuellen Computer automatisch im Subnetz *default* des virtuellen Netzwerks *myVirtualNetwork*, da das virtuelle Netzwerk in der Ressourcengruppe vorhanden ist und im Befehl kein virtuelles Netzwerk und kein Subnetz angegeben ist. Azure DHCP wies dem virtuellen Computer bei der Erstellung automatisch 10.0.0.4 zu, da es sich dabei um die erste verfügbare Adresse im Subnetz *default* handelte. Der Standort, an dem ein virtueller Computer erstellt wird, muss mit dem Standort des virtuellen Netzwerks identisch sein. Der virtuelle Computer muss sich nicht in derselben Ressourcengruppe wie das virtuelle Netzwerk befinden, auch wenn dies in diesem Artikel der Fall ist.

Erstellen Sie einen zweiten virtuellen Computer. Standardmäßig erstellt Azure diesen virtuellen Computer ebenfalls im Subnetz *default*.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Das Erstellen des virtuellen Computers dauert einige Minuten. Nachdem die virtuelle Computer erstellt wurde, gibt die Azure-Befehlszeilenschnittstelle eine Ausgabe ähnlich dem folgenden Beispiel zurück: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm1",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

Im Beispiel hat **privateIpAddress** den Wert *10.0.0.5*. Azure DHCP wies dem virtuellen Computer automatisch *10.0.0.5* zu, da es sich dabei um die nächste verfügbare Adresse im Subnetz *default* handelte. Notieren Sie sich **publicIpAddress**. Über diese Adresse wird in einem späteren Schritt über das Internet auf den virtuellen Computer zugegriffen. Die öffentliche IP-Adresse wird nicht innerhalb der Adresspräfixe des virtuellen Netzwerks oder des Subnetzes zugewiesen. Öffentliche IP-Adressen werden aus einem [Pool von jeder Azure-Region zugewiesenen Adressen](https://www.microsoft.com/download/details.aspx?id=41653) zugewiesen. Auch wenn Azure bekannt ist, welche öffentliche IP-Adresse einem virtuellen Computer zugewiesen ist, hat das auf einem virtuellen Computer ausgeführte Betriebssystem keine Informationen über die ihm zugewiesene öffentliche IP-Adresse.

### <a name="connect-to-a-virtual-machine"></a>Herstellen einer Verbindung mit einem virtuellen Computer

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit dem virtuellen Computer *myVm2*. Ersetzen Sie `<publicIpAddress>` durch die öffentliche IP-Adresse des virtuellen Computers. Im Beispiel oben lautet die IP-Adresse *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

### <a name="validate-communication"></a>Überprüfen der Kommunikation

Verwenden Sie den folgenden Befehl zum Bestätigen der Kommunikation mit *myVm1* von *myVm2* aus:

```bash
ping myVm1 -c 4
```

Sie erhalten vier Antworten von *10.0.0.4*. Sie können von *myVm2* aus mit *myVm1* kommunizieren, da beiden virtuellen Computern private IP-Adressen vom Subnetz *default* zugewiesen wurden. Sie können ein Ping anhand des Hostnamens herstellen, da Azure automatisch DNS-Namensauflösung für alle Hosts in einem virtuellen Netzwerk bereitstellt.

Verwenden Sie den folgenden Befehl, um die ausgehende Kommunikation mit dem Internet zu bestätigen:

```bash
ping bing.com -c 4
```

Sie erhalten vier Antworten von bing.com. Standardmäßig können alle Ressourcen in einem virtuellen Netzwerk in ausgehender Richtung mit dem Internet kommunizieren.

Beenden Sie die SSH-Sitzung mit Ihrem virtuellen Computer.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#az_group_delete) entfernen:

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie ein virtuelles Standardnetzwerk mit einem Subnetz bereitgestellt. Um zu erfahren, wie Sie ein benutzerdefiniertes virtuelles Netzwerk mit mehreren Subnetzen erstellen, fahren Sie mit dem Tutorial zum Erstellen eines benutzerdefinierten virtuellen Netzwerks fort.

> [!div class="nextstepaction"]
> [Erstellen eines benutzerdefinierten virtuellen Netzwerks](virtual-networks-create-vnet-arm-pportal.md#azure-cli)
