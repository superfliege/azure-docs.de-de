---
title: Herstellen von Verbindungen zwischen virtuellen Netzwerken durch Peerings für virtuelle Netzwerke – Azure CLI | Microsoft-Dokumentation
description: Hier erfahren Sie, wie mithilfe von Peerings in virtuellen Netzwerken Verbindungen zwischen virtuellen Netzwerken hergestellt werden.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: bbf2e757e2d9ad76c59394ba0138a61fd4029d15
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Herstellen von Verbindungen zwischen virtuellen Netzwerken durch Peerings für virtuelle Netzwerke mit der Azure CLI

Sie können mithilfe von Peerings für virtuelle Netzwerke Verbindungen zwischen virtuellen Netzwerken herstellen. Sobald ein Peering zwischen virtuellen Netzwerken eingerichtet wurde, können Ressourcen in beiden virtuellen Netzwerken untereinander mit der gleichen Latenz und Bandbreite kommunizieren, als befänden sie sich im selben virtuellen Netzwerk. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen zweier virtueller Netzwerke
> * Herstellen einer Verbindung zwischen zwei virtuellen Netzwerken mit einem Peering virtueller Netzwerke
> * Bereitstellen eines virtuellen Computers (VM) in jedem virtuellen Netzwerk
> * Kommunikation zwischen VMs

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart mindestens Azure CLI-Version 2.0.28 ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-virtual-networks"></a>Erstellen virtueller Netzwerke

Vor der Erstellung eines virtuellen Netzwerks müssen Sie eine Ressourcengruppe für das virtuelle Netzwerk und alle anderen in diesem Artikel erstellten Ressourcen erstellen. Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVirtualNetwork1* mit dem Adresspräfix *10.0.0.0/16* erstellt.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.0.0.0/24
```

Erstellen Sie ein virtuelles Netzwerk namens *myVirtualNetwork2* mit dem Adresspräfix *10.1.0.0/16*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --address-prefixes 10.1.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.1.0.0/24
```

## <a name="peer-virtual-networks"></a>Einrichten eines Peerings von virtuellen Netzwerken

Peerings werden zwischen IDs virtueller Netzwerke erstellt. Daher müssen Sie mit [az network vnet show](/cli/azure/network/vnet#az_network_vnet_show) zuerst die ID jedes virtuellen Netzwerks abrufen und die ID in einer Variablen speichern.

```azurecli-interactive
# Get the id for myVirtualNetwork1.
vNet1Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork1 \
  --query id --out tsv)

# Get the id for myVirtualNetwork2.
vNet2Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork2 \
  --query id \
  --out tsv)
```

Erstellen Sie mit [az network vnet peering create](/cli/azure/network/vnet/peering#az_network_vnet_peering_create) ein Peering zwischen *myVirtualNetwork1* und *myVirtualNetwork2*. Wenn der `--allow-vnet-access`-Parameter nicht angegeben ist, wird ein Peering eingerichtet, es ist jedoch keine Kommunikation über diesen möglich.

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet-id $vNet2Id \
  --allow-vnet-access
```

In der Ausgabe, die nach der Ausführung des vorherigen Befehl zurückgegeben wird, können Sie sehen, dass **peeringState** auf *Initiiert* steht. Das Peering verbleibt jedoch im Zustand *Initiiert*, bis Sie das Peering zwischen *myVirtualNetwork2* und *myVirtualNetwork1* erstellen. Erstellen Sie ein Peering zwischen *myVirtualNetwork2* und *myVirtualNetwork1*. 

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet-id $vNet1Id \
  --allow-vnet-access
```

In der Ausgabe, die nach der Ausführung des vorherigen Befehl zurückgegeben wird, können Sie sehen, dass **peeringState** auf *Verbunden* steht. Azure hat zudem den Peeringstatus des Peerings *myVirtualNetwork1-myVirtualNetwork2* in *Verbunden* geändert. Überprüfen Sie mit [az network vnet peering show](/cli/azure/network/vnet/peering#az_network_vnet_peering_show), ob der Peeringstatus für das Peering *myVirtualNetwork1-myVirtualNetwork2* in *Verbunden* geändert wurde.

```azurecli-interactive
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

Ressourcen in einem virtuelles Netzwerk können erst dann mit Ressourcen im anderen virtuellen Netzwerk kommunizieren, wenn **peeringState** für die Peerings in beiden virtuellen Netzwerken auf *Verbunden* steht. 

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie eine VM in jedem virtuellen Netzwerk, damit in einem späteren Schritt eine Kommunikation zwischen ihnen möglich ist.

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

Erstellen Sie mit [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. Im folgenden Beispiel wird eine VM namens *myVm1* im virtuellen Netzwerk *myVirtualNetwork1* erstellt. Wenn SSH-Schlüssel nicht bereits an einem Standardschlüsselspeicherort vorhanden sind, werden sie durch den Befehl erstellt. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`. Mit der Option `--no-wait` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork1 \
  --subnet Subnet1 \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

Erstellen Sie eine VM im virtuellen Netzwerk *myVirtualNetwork2*.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

Die Erstellung des virtuellen Computers dauert einige Minuten. Nach dem Erstellen der VM zeigt die Azure CLI ähnliche Informationen wie im folgenden Beispiel an: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.1.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Notieren Sie sich **publicIpAddress**. Über diese Adresse wird in einem späteren Schritt über das Internet auf die VM zugegriffen.

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit der *myVm2*-VM. Ersetzen Sie `<publicIpAddress>` durch die öffentliche IP-Adresse der VM. Im vorherigen Beispiel lautet die öffentliche IP-Adresse *13.90.242.231*.

```bash 
ssh <publicIpAddress>
```

Pingen Sie die VM in *myVirtualNetwork1*.

```bash 
ping 10.0.0.4 -c 4
```

Sie erhalten vier Antworten. 

Schließen Sie die SSH-Sitzung mit der *myVm2*-VM. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit [az group delete](/cli/azure/group#az_group_delete) entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

**<a name="register"></a>Registrieren für die globale Vorschauversion für das Peering virtueller Netzwerke**

Das Peering von virtuellen Netzwerken in derselben Region befindet sich in der Phase der allgemeinen Verfügbarkeit. Das Peering von virtuellen Netzwerken in unterschiedlichen Regionen befindet sich derzeit in der Vorschauphase. Verfügbare Regionen finden Sie unter [Updates für virtuelle Netzwerke](https://azure.microsoft.com/updates/?product=virtual-network). Für ein regionsübergreifendes Peering virtueller Netzwerke müssen Sie sich zunächst für die Vorschauversion registrieren. Führen Sie dazu (innerhalb des Abonnements mit den einzelnen virtuellen Netzwerken, für die Sie ein Peering einrichten möchten) die folgenden Schritte aus:

1. Registrieren Sie sich für die Vorschauversion, indem Sie die folgenden Befehle eingeben:

  ```azurecli-interactive
  az feature register --name AllowGlobalVnetPeering --namespace Microsoft.Network
  az provider register --name Microsoft.Network
  ```

2. Bestätigen Sie, dass Sie für die Vorschauversion registriert sind, indem Sie folgenden Befehl eingeben:

  ```azurecli-interactive
  az feature show --name AllowGlobalVnetPeering --namespace Microsoft.Network
  ```

  Wenn Sie versuchen, ein Peering virtueller Netzwerke in verschiedenen Regionen einzurichten, bevor die **RegistrationState**-Ausgabe nach Eingabe des vorherigen Befehls bei beiden Abonnements zu **Registriert** wechselt, tritt beim Peering ein Fehler auf.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie zwei Netzwerke durch Peering virtueller Netzwerke verbunden werden. In diesem Artikel haben Sie erfahren, wie zwei Netzwerke im selben Azure-Standort durch Peering virtueller Netzwerke verbunden werden. Sie können auch virtuelle Netzwerke in [unterschiedlichen Regionen](#register) und [verschiedenen Azure-Abonnements](create-peering-different-subscriptions.md#portal) durch Peering verbinden sowie [„Nabe-und-Speiche“-Netzwerkentwürfe](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) mit Peering erstellen. Vor dem Einrichten eines Peerings zwischen virtuellen Produktionsnetzwerken sollten Sie sich gründlich mit der [Übersicht über Peering](virtual-network-peering-overview.md), der [Verwaltung von Peering](virtual-network-manage-peering.md) und den [Grenzwerten für virtuelle Netzwerke](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vertraut zu machen.

Sie können über ein VPN [eine Verbindung zwischen Ihrem eigenen Computer und einem virtuellen Netzwerk herstellen](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und mit Ressourcen in einem virtuellen Netzwerk oder in durch Peering verbundenen virtuellen Netzwerken interagieren. Fahren Sie mit den Beispielen für wiederverwendbare Skripts fort, um viele der in den Artikeln zu virtuellen Netzwerken behandelten Aufgaben durchzuführen.

> [!div class="nextstepaction"]
> [Skriptbeispiele für virtuelle Netzwerke](../networking/cli-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
