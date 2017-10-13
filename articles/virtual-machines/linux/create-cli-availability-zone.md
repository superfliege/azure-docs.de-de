---
title: Erstellen eines virtuellen Linux-Computers in einer Zone mit der Azure CLI | Microsoft-Dokumentation
description: "Erstellen eines virtuellen Linux-Computers in einer Verfügbarkeitszone mit der Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: danlep
ms.custom: 
ms.openlocfilehash: 232c2cf1ba0a7de23da10357de9a6e6ad9a0d41d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Erstellen eines virtuellen Linux-Computers in einer Verfügbarkeitszone mit der Azure CLI

In diesem Artikel werden die Schritte zum Erstellen eines virtuellen Linux-Computers in einer Azure-Verfügbarkeitszone mithilfe der Azure CLI erläutert. Eine [Verfügbarkeitszone](../../availability-zones/az-overview.md) ist eine physisch separate Zone in einer Azure-Region. Verwenden Sie Verfügbarkeitszonen, um Ihre Apps und Daten vor einem unwahrscheinlichen Fehler oder Ausfall eines gesamten Datencenters zu schützen.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]

Achten Sie darauf, dass Sie die neueste Version [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert haben und mit [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sind.

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe.  

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Vor dem virtuellen Computer muss eine Ressourcengruppe erstellt werden. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupVM* in der Region *eastus2* erstellt. „USA, Osten 2“ ist eine der Azure-Regionen, die Verfügbarkeitszonen in der Vorschauversion unterstützen.

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus2
```

Die Ressourcengruppe wird beim Erstellen oder Ändern eines virtuellen Computers angegeben und ist im gesamten Artikel zu sehen.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie mit dem Befehl [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. 

Beim Erstellen eines virtuellen Computers stehen mehrere Optionen zur Verfügung, z.B. Betriebssystemimage, Festlegen der Datenträgergröße und Administratoranmeldeinformationen. In diesem Beispiel wird ein virtueller Computer mit dem Namen *myVM* erstellt, auf dem Ubuntu Server ausgeführt wird. Der virtuelle Computer wird in der Verfügbarkeitszone *1* erstellt. Standardmäßig wird der virtuelle Computer mit der Größe *Standard_DS1_v2* erstellt. Diese Größe wird in der Vorschauversion der Verfügbarkeitszonen unterstützt.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys --zone 1
```

Die Erstellung der VM kann einige Minuten dauern. Nach der Erstellung des virtuellen Computers gibt die Azure-Befehlszeilenschnittstelle Informationen zu dem virtuellen Computer aus. Notieren Sie sich den Wert `zones`. Er gibt die Verfügbarkeitszone an, in der der virtuelle Computer ausgeführt wird. 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus2",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM",
  "zones": "1"
}
```

## <a name="zone-for-ip-address-and-managed-disk"></a>Zone für IP-Adresse und verwalteten Datenträger

Bei der Bereitstellung des virtuellen Computers in einer Verfügbarkeitszone werden die Ressourcen für IP-Adresse und verwaltete Datenträger in der gleichen Verfügbarkeitszone bereitgestellt. Die folgenden Beispiele rufen Informationen zu diesen Ressourcen ab.

Verwenden Sie zuerst den Befehl [az vm list-ip-addresses](/cli/azure/vm#az_vm_list_ip_addresses), um den Namen der öffentlichen IP-Adresse in *myVM* zurückzugeben. In diesem Beispiel wird der Name in einer Variablen gespeichert, die in einem späteren Schritt verwendet wird.

```azurecli-interactive
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

Nun können Sie Informationen zur IP-Adresse abrufen:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

Die Ausgabe zeigt, dass sich die IP-Adresse in derselben Verfügbarkeitszone wie der virtuelle Computer befindet:

```azurecli-interactive
{
  "dnsSettings": null,
  "etag": "W/\"b7ad25eb-3191-4c8f-9cec-c5e4a3a37d35\"",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "52.174.34.95",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "myResourceGroupVM",
    "subnet": null
  },
  "location": "eastUS2",
  "name": "myVMPublicIP",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "myResourceGroupVM",
  "resourceGuid": "8c70a073-09be-4504-0000-000000000000",
  "tags": {},
  "type": "Microsoft.Network/publicIPAddresses",
  "zones": [
    "1"
  ]
}
```

Stellen Sie außerdem sicher, dass sich der verwaltete Datenträger des virtuellen Computers in der Verfügbarkeitszone befindet. Rufen Sie die Datenträger-ID mit dem Befehl [az vm show](/cli/azure/vm#az_vm_show) ab. In diesem Beispiel wird die Datenträger-ID in einer Variablen gespeichert, die in einem späteren Schritt verwendet wird. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
Nun können Sie Informationen zum verwalteten Datenträger abrufen:

```azurecli-interactive
az disk show --resource-group myResourceGroupVM --name $osdiskname
```

Die Ausgabe zeigt, dass sich der verwaltete Datenträger in der gleichen Verfügbarkeitszone befindet wie der virtuelle Computer:

```azurecli-interactive
{
  "creationData": {
    "createOption": "FromImage",
    "imageReference": {
      "id": "/Subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/Providers/Microsoft.Compute/Locations/westeurope/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/latest",
      "lun": null
    },
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/disks/osdisk_761c570dab",
  "location": "eastus2",
  "managedBy": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "name": "osdisk_761c570dab",
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupVM",
  "sku": {
    "name": "Premium_LRS",
    "tier": "Premium"
  },
  "tags": {},
  "timeCreated": "2017-09-05T22:16:06.892752+00:00",
  "type": "Microsoft.Compute/disks",
  "zones": [
    "1"
  ]
}
```
 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie einen virtuellen Computer in einer Verfügbarkeitszone erstellen. Weitere Informationen zu Regionen und Verfügbarkeit für virtuelle Azure-Computer finden Sie [hier](regions-and-availability.md).




