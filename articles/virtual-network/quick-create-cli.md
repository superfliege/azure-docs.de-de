---
title: Erstellen eines virtuellen Netzwerks – Schnellstart – Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal ein virtuelles Netzwerk erstellen. In einem virtuellen Netzwerk können Azure-Ressourcen wie virtuelle Computer privat miteinander und mit dem Internet kommunizieren.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: bb45b2b4ecd89187e94066bc81782174738fe3a9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232700"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-cli"></a>Schnellstart: Erstellen eines virtuellen Netzwerks mithilfe der Azure-Befehlszeilenschnittstelle

In einem virtuellen Netzwerk können Azure-Ressourcen wie virtuelle Computer privat miteinander und mit dem Internet kommunizieren. In dieser Schnellstartanleitung erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen. Nach dem Erstellen eines virtuellen Netzwerks stellen Sie zwei virtuelle Computer im virtuellen Netzwerk bereit. Anschließend stellen Sie über das Internet eine Verbindung mit einem virtuellen Computer her und kommunizieren privat mit dem anderen virtuellen Computer.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart mindestens Azure CLI-Version 2.0.28 ausführen. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 


## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Bevor Sie ein virtuelles Netzwerk erstellen können, müssen Sie eine Ressourcengruppe erstellen, die das virtuelle Netzwerk enthält. Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen *myVirtualNetwork* und dem Subnetz *default* erstellt:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk:

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

Erstellen Sie mit [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. Wenn SSH-Schlüssel nicht bereits an einem Standardschlüsselspeicherort vorhanden sind, werden sie durch den Befehl erstellt. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`. Mit der Option `--no-wait` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können. Im folgenden Beispiel wird ein virtueller Computer namens *myVm1* erstellt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Die Erstellung des virtuellen Computers dauert einige Minuten. Nachdem der virtuelle Computer erstellt wurde, gibt die Azure-Befehlszeilenschnittstelle eine Ausgabe ähnlich dem folgenden Beispiel zurück: 

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

Notieren Sie sich **publicIpAddress**. Diese Adresse wird im nächsten Schritt zum Herstellen einer Verbindung mit dem virtuellen Computer über das Internet verwendet.

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Geben Sie den folgenden Befehl ein, und ersetzen Sie dabei `<publicIpAddress>` durch die öffentliche IP-Adresse des virtuellen Computers *myVm2*:

```bash 
ssh <publicIpAddress>
```

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

Wenn Sie die private Kommunikation zwischen den virtuellen Computern *myVm2* und *myVm1* überprüfen möchten, geben Sie den folgenden Befehl ein:

```bash
ping myVm1 -c 4
```

Sie erhalten vier Antworten von *10.0.0.4*.

Beenden Sie die SSH-Sitzung mit dem virtuellen Computer *myVm2*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit [az group delete](/cli/azure/group#az_group_delete) entfernen:

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein virtuelles Standardnetzwerk und zwei virtuelle Computer erstellt. Sie haben über das Internet eine Verbindung mit einem der virtuellen Computer hergestellt und privat zwischen den beiden virtuellen Computern kommuniziert. Weitere Informationen zu den Einstellungen des virtuellen Netzwerks finden Sie unter [Create, change, or delete a virtual network](manage-virtual-network.md) (Erstellen, Ändern oder Löschen eines virtuellen Netzwerks). 

Azure ermöglicht standardmäßig eine uneingeschränkte private Kommunikation zwischen virtuellen Computern. Über das Internet lässt Azure dagegen nur eingehende Remotedesktopverbindungen mit virtuellen Windows-Computern zu. Informationen zum Zulassen und Beschränken verschiedener Arten von Netzwerkkommunikation zwischen virtuellen Computern finden Sie im [Tutorial zum Filtern von Netzwerkdatenverkehr](tutorial-filter-network-traffic.md).
