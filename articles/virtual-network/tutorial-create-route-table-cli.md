---
title: Weiterleiten von Netzwerkdatenverkehr – Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Netzwerkdatenverkehr mithilfe der Azure-Befehlszeilenschnittstelle und einer Routingtabelle weitergeleitet wird.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/02/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 3c16c774fa1c8a5c8bf50b4f4f9d0bfb283318e3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Weiterleiten von Netzwerkdatenverkehr mithilfe der Azure-Befehlszeilenschnittstelle

Standardmäßig leitet Azure automatisch den Datenverkehr aller Subnetze des virtuellen Netzwerks weiter. Sie können eigene Routen erstellen, um das Azure-Standardrouting außer Kraft zu setzen. Die Möglichkeit zum Erstellen von benutzerdefinierten Routen ist hilfreich, wenn Sie z.B. über eine Firewall Datenverkehr zwischen Subnetzen weiterleiten möchten. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen einer Routingtabelle
> * Erstellen einer Route
> * Zuordnen einer Routingtabelle zu einem Subnetz des virtuellen Netzwerks
> * Testen des Routings
> * Beheben von Routingproblemen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart mindestens Azure CLI-Version 2.0.28 ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

Standardmäßig leitet Azure den Datenverkehr zwischen allen Subnetzen des virtuellen Netzwerks weiter. Weitere Informationen zu Azure-Standardrouten finden Sie unter [Systemrouten](virtual-networks-udr-overview.md). Um das Azure-Standardrouting außer Kraft zu setzen, erstellen Sie eine Routingtabelle mit Routen und ordnen die Routingtabelle dem Subnetz eines virtuellen Netzwerks zu.

Bevor Sie eine Routingtabelle erstellen können, müssen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe für alle in diesem Artikel erstellten Ressourcen erstellen. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
``` 

Erstellen Sie mit [az network route-table create](/cli/azure/network/route#az_network_route_table_create) eine Routingtabelle. Im folgenden Beispiel wird eine Routingtabelle mit dem Namen *myRouteTablePublic* erstellt. 

```azurecli-interactive 
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Erstellen einer Route

Eine Routingtabelle enthält null oder mehr Routen. Erstellen Sie mit [az network route-table route create](/cli/azure/network/route-table/route#az_network_route_table_route_create) eine Route in der Routingtabelle. 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

Die Route leitet sämtlichen an das Adresspräfix 10.0.1.0/24 gerichteten Datenverkehr über ein virtuelles Netzwerkgerät mit der IP-Adresse 10.0.2.4 weiter. Das virtuelle Netzwerkgerät und das Subnetz mit dem angegebenen Adresspräfix werden in späteren Schritten erstellt. Die Route überschreibt das Azure-Standardrouting, das Datenverkehr zwischen Subnetzen direkt weiterleitet. Jede Route gibt einen Typ des nächsten Hops an. Der Typ des nächsten Hops weist Azure an, wie der Datenverkehr weitergeleitet werden soll. In diesem Beispiel handelt es sich beim Typ des nächsten Hops um *VirtualAppliance*. Weitere Informationen zu allen in Azure verfügbaren Typen des nächsten Hops und deren Verwendung finden Sie unter den [Typen des nächsten Hops](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

Bevor Sie eine Routingtabelle einem Subnetz zuordnen können, müssen Sie ein virtuelles Netzwerk und ein Subnetz erstellen. Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) ein virtuelles Netzwerk mit einem Subnetz.

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Erstellen Sie mit [az network vnet subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) zwei weitere Subnetze.

```azurecli-interactive
# Create a private subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24

# Create a DMZ subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name DMZ \
  --address-prefix 10.0.2.0/24
```

Sie können eine Routingtabelle null oder mehr Subnetzen zuordnen. Jedem Subnetz können null oder eine Routingtabelle zugeordnet werden. Ausgehender Datenverkehr aus einem Subnetz wird anhand der Azure-Standardrouten und aller benutzerdefinierten Routen, die Sie einer einem Subnetz zugeordneten Routingtabelle hinzugefügt haben, weitergeleitet. Ordnen Sie mit [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) die Routingtabelle *myRouteTablePublic* dem Subnetz *Public* zu.

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

Vor der Bereitstellung von Routingtabellen für die Produktion wird empfohlen, sich gründlich mit dem [Routing in Azure](virtual-networks-udr-overview.md) und den [Grenzen von Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vertraut zu machen.

## <a name="test-routing"></a>Testen des Routings

Erstellen Sie zum Testen des Routings einen virtuellen Computer als virtuelles Netzwerkgerät, über das die im vorherigen Schritt erstellte Route Datenverkehr weiterleitet. Nachdem Sie das virtuelle Netzwerkgerät erstellt haben, stellen Sie einen virtuellen Computer in den Subnetzen *Public* und *Private* bereit. Sie leiten dann über das virtuelle Netzwerkgerät Datenverkehr vom Subnetz *Private* an das Subnetz *Public* weiter.

### <a name="create-a-network-virtual-appliance"></a>Erstellen eines virtuellen Netzwerkgeräts

In einem vorherigen Schritt haben Sie eine Route erstellt, die ein virtuelles Netzwerkgerät als Typ des nächsten Hops angab. Wenn ein virtueller Computer eine Netzwerkanwendung ausführt, wird er häufig als virtuelles Netzwerkgerät bezeichnet. In Produktionsumgebungen handelt es sich bei dem bereitgestellten virtuellen Netzwerkgerät häufig um einen vorkonfigurierten virtuellen Computer. Im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1) sind verschiedene virtuelle Netzwerkgeräte verfügbar. In diesem Artikel wird ein grundlegender virtueller Computer erstellt. 

Erstellen Sie mit [az vm create](/cli/azure/vm#az_vm_create) im Subnetz *DMZ* ein virtuelles Netzwerkgerät. Beim Erstellen eines virtuellen Computers erstellt Azure standardmäßig eine öffentliche IP-Adresse und weist diese dem virtuellen Computer zu. Der Parameter `--public-ip-address ""` weist Azure an, keine öffentliche IP-Adresse zu erstellen und dem virtuellen Computer zuzuweisen, da keine Verbindungen mit dem virtuellen Computer über das Internet hergestellt werden müssen. Wenn SSH-Schlüssel nicht bereits an einem Standardschlüsselspeicherort vorhanden sind, werden sie durch den Befehl erstellt. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`.

```azure-cli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmNva \
  --image UbuntuLTS \
  --public-ip-address "" \
  --subnet DMZ \
  --vnet-name myVirtualNetwork \
  --generate-ssh-keys
```

Das Erstellen des virtuellen Computers dauert einige Minuten. Fahren Sie erst mit dem nächsten Schritt fort, wenn Azure den virtuellen Computer erstellt und Ausgaben über den virtuellen Computer zurückgegeben hat. In Produktionsumgebungen handelt es sich bei dem bereitgestellten virtuellen Netzwerkgerät häufig um einen vorkonfigurierten virtuellen Computer. Im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1) sind verschiedene virtuelle Netzwerkgeräte verfügbar.

Sie müssen IP-Weiterleitung für jede an einen virtuellen Computer angefügte Azure-[Netzwerkschnittstelle](virtual-network-network-interface.md) aktivieren, die Datenverkehr an eine beliebige IP-Adresse weiterleitet, die der Netzwerkschnittstelle nicht zugewiesen ist. Beim Erstellen des virtuellen Netzwerkgeräts im Subnetz *DMZ* hat Azure automatisch eine Netzwerkschnittstelle namens *myVmNvaVMNic* erstellt, die Netzwerkschnittstelle an den virtuellen Computer angefügt und ihr die private IP-Adresse *10.0.2.4* zugewiesen. Aktivieren Sie mit [az network nic update](/cli/azure/network/nic#az_network_nic_update) die IP-Weiterleitung für die Netzwerkschnittstelle.

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

Das Betriebssystem des virtuellen Computers oder eine Anwendung, die auf diesem ausgeführt wird, muss ebenfalls Netzwerkdatenverkehr weiterleiten können. Wenn Sie ein virtuelles Netzwerkgerät in einer Produktionsumgebung bereitstellen, führt das Gerät in der Regel vor der Weiterleitung von Datenverkehr eine Filterung oder Protokollierung durch oder führt eine andere Funktion aus. In diesem Artikel leitet das Betriebssystem jedoch einfach den gesamten empfangenen Datenverkehr weiter. Aktivieren Sie mit [az vm extension set](/cli/azure/vm/extension#az_vm_extension_set) die IP-Weiterleitung innerhalb des Betriebssystems des virtuellen Computers. Dadurch wird ein Befehl ausgeführt, der IP-Weiterleitung im Betriebssystem aktiviert.

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
Die Ausführung des Befehls kann bis zu einer Minute dauern.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie in einem späteren Schritt diesen Datenverkehr aus dem Subnetz *Public* über das virtuelle Netzwerkgerät an das Subnetz *Private* weiterleiten können. 

Erstellen Sie mit *az vm create* einen virtuellen Computer im Subnetz [Public](/cli/azure/vm#az_vm_create). Der Parameter `--no-wait` ermöglicht Azure die Ausführung des Befehls im Hintergrund, sodass Sie mit dem nächsten Befehl fortfahren können. Zur Vereinfachung dieses Artikels wird ein Kennwort verwendet. Schlüssel werden in der Regel in Produktionsbereitstellungen verwendet. Bei der Verwendung von Schlüsseln müssen Sie außerdem die SSH-Agent-Weiterleitung konfigurieren. Weitere Informationen finden Sie in der Dokumentation Ihres SSH-Clients. Ersetzen Sie `<replace-with-your-password>` im folgenden Befehl durch ein Kennwort Ihrer Wahl.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Erstellen Sie einen virtuellen Computer im Subnetz *Privat*.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Das Erstellen des virtuellen Computers dauert einige Minuten. Nachdem der virtuelle Computer erstellt wurde, zeigt die Azure-Befehlszeilenschnittstelle Informationen ähnlich wie im folgenden Beispiel an: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmMgmt",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Notieren Sie sich **publicIpAddress**. Über diese Adresse wird in einem späteren Schritt über das Internet auf den virtuellen Computer zugegriffen.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit dem virtuellen Computer *myVmMgmt*. Ersetzen Sie *<publicIpAddress>* durch die öffentliche IP-Adresse des virtuellen Computers. Im Beispiel oben lautet die IP-Adresse *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Geben Sie bei entsprechender Aufforderung das Kennwort ein, das Sie unter [Erstellen von virtuellen Computern](#create-virtual-machines) ausgewählt haben.

Installieren Sie mit dem folgenden Befehl Traceroute auf dem virtuellen Computer *myVmMgmt*:

```bash 
sudo apt-get install traceroute
```

Verwenden Sie den folgenden Befehl, um die Weiterleitung von Netzwerkdatenverkehr vom virtuellen Computer *myVmMgmt* an den virtuellen Computer *myVmWeb* zu testen.

```bash
traceroute myvmweb
```

Die Antwort ähnelt dem folgenden Beispiel:

```bash
traceroute to myvmweb (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Beachten Sie, dass Datenverkehr direkt vom virtuellen Computer *myVmMgmt* an den virtuellen Computer *myVmWeb* weitergeleitet wird. Azure-Standardrouten leiten Datenverkehr direkt zwischen Subnetzen weiter. 

Verwenden Sie den folgenden Befehl, um vom virtuellen Computer *myVmMgmt* eine SSH-Verbindung mit dem virtuellen Computer *myVmWeb* herzustellen:

```bash 
ssh azureuser@myVmWeb
```

Installieren Sie mit dem folgenden Befehl Traceroute auf dem virtuellen Computer *myVmWeb*:

```bash 
sudo apt-get install traceroute
```

Verwenden Sie den folgenden Befehl, um die Weiterleitung von Netzwerkdatenverkehr vom virtuellen Computer *myVmWeb* an den virtuellen Computer *myVmMgmt* zu testen.

```bash
traceroute myvmmgmt
```

Die Antwort ähnelt dem folgenden Beispiel:

```bash
traceroute to myvmmgmt (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
Wie Sie sehen können, ist der erste Hop 10.0.2.4, die private IP-Adresse des virtuellen Netzwerkgeräts. Der zweite Hop ist 10.0.1.4, die private IP-Adresse des virtuellen Computers *myVmMgmt*. Die Route, die der Routingtabelle *myRouteTablePublic* hinzugefügt und dem Subnetz *Public* zugeordnet wurde, hat bewirkt, dass Azure Datenverkehr über das virtuelle Netzwerkgerät weiterleitet und nicht direkt an das Subnetz *Private*.

Schließen Sie die SSH-Sitzungen für die virtuellen Computer *myVmWeb* und *myVmMgmt*.

## <a name="troubleshoot-routing"></a>Beheben von Routingproblemen

Wie Sie in den vorherigen Schritten erfahren haben, wendet Azure Standardrouten an, die Sie wahlweise mit Ihren eigenen Routen überschreiben können. In einigen Fällen kann Datenverkehr nicht gemäß Ihren Vorstellungen weitergeleitet werden. Verwenden Sie [az network watcher show-next-hop](/cli/azure/network/watcher#az_network_watcher_show_next_hop), um zu ermitteln, wie Datenverkehr zwischen zwei virtuellen Computern weitergeleitet wird. Der folgende Befehl testet z.B. das Datenverkehrsrouting vom virtuellen Computer *myVmWeb* (10.0.0.4) an den virtuellen Computer *myVmMgmt* (10.0.1.4):

```azurecli-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region.
az network watcher configure --locations eastus --resource-group myResourceGroup --enabled true

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 10.0.1.4 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVmWeb \
  --out table
```
Nach einem kurzen Moment wird die folgende Ausgabe zurückgegeben:

```azurecli
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

Die Ausgabe gibt an, dass die IP-Adresse des nächsten Hops für den Datenverkehr von *myVmWeb* an *myVmMgmt* 10.0.2.4 lautet (der virtuelle Computer *myVmNva*), der nächste Hop vom Typ *VirtualAppliance* ist und die die Weiterleitung verursachende Routingtabelle *myRouteTablePublic* ist.

Die effektiven Routen für die einzelnen Netzwerkschnittstellen stellen eine Kombination aus Azure-Standardrouten und ggf. von Ihnen definierten Routen dar. Zeigen Sie mit [az network nic show-effective-route-table](/cli/azure/network/nic#az_network_nic_show_effective_route_table) alle Routen an, die für eine Netzwerkschnittstelle auf einem virtuellen Computer gültig sind. Um beispielsweise die gültigen Routen für die Netzwerkschnittstelle *MyVmWebVMNic* auf dem virtuellen Computer *myVmWeb* anzuzeigen, geben Sie den folgenden Befehl ein:

```azurecli-interactive
az network nic show-effective-route-table \
  --name MyVmWebVMNic \
  --resource-group myResourceGroup
```

Es werden alle Standardrouten und die Route, die Sie in einem früheren Schritt hinzugefügt haben, zurückgegeben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit [az group delete](/cli/azure/group#az_group_delete) entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine Routingtabelle erstellt und einem Subnetz zugeordnet. Sie haben ein virtuelles Netzwerkgerät erstellt, das Datenverkehr von einem öffentlichen Subnetz an ein privates Subnetz weiterleitet. Sie können zwar in einem virtuellen Netzwerk viele Azure-Ressourcen bereitstellen, für einige Azure-PaaS-Dienste können jedoch keine Ressourcen in einem virtuellen Netzwerk bereitgestellt werden. Allerdings können Sie den Zugriff auf die Ressourcen einiger Azure-PaaS-Dienste nach wie vor auf den Datenverkehr nur eines Subnetzes eines virtuellen Netzwerks beschränken. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Netzwerkzugriff auf Azure-PaaS-Ressourcen beschränken.

> [!div class="nextstepaction"]
> [Beschränken des Netzwerkzugriffs auf PaaS-Ressourcen](virtual-network-service-endpoints-configure.md#azure-cli)
