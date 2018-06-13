---
title: Erstellen eines virtuellen Azure-Netzwerks mit mehreren Subnetzen – Azure CLI | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der Azure CLI ein virtuelles Netzwerk mit mehreren Subnetzen erstellen.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 0b0bfae02147910d98231d7c93f5ab260f26364f
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2018
ms.locfileid: "29880530"
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-the-azure-cli"></a>Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen mithilfe der Azure CLI

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen mit dem Internet und privat miteinander kommunizieren. Durch die Erstellung mehrerer Subnetze in einem virtuellen Netzwerk können Sie Ihr Netzwerk segmentieren, sodass Sie den Datenverkehr zwischen Subnetzen filtern oder steuern können. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines Subnetzes
> * Testen der Netzwerkkommunikation zwischen virtuellen Computern

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli.md) Informationen dazu. 

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVirtualNetwork* mit dem Adresspräfix *10.0.0.0/16* erstellt. Der Befehl erstellt ein Subnetz namens *Öffentlich* mit dem Adresspräfix *10.0.0.0/24*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Da im vorherigen Befehl kein Standort angegeben wurde, erstellt Azure das virtuelle Netzwerk am gleichen Standort wie die Ressourcengruppe *myResourceGroup*. Die **Adresspräfixe** und das **Subnetzpräfix** werden in der CIDR-Notation angegeben. Das angegebene Adresspräfix enthält die IP-Adressen 10.0.0.0 bis 10.0.255.254. Das für das Subnetz angegebene Präfix muss innerhalb des für das virtuelle Netzwerk angegebenen Adresspräfixes liegen. Azure DHCP weist in einem Subnetz bereitgestellten Ressourcen IP-Adressen aus einem Subnetzadresspräfix zu. Azure weist die Adressen 10.0.0.4 bis 10.0.0.254 nur Ressourcen zu, die im Subnetz **Öffentlich** bereitgestellt werden, da Azure die ersten vier Adressen (in diesem Beispiel 10.0.0.0 bis 10.0.0.3 für das Subnetz) und die letzte Adresse (in diesem Beispiel 10.0.0.255 für das Subnetz) in jedem Subnetz reserviert.

## <a name="create-a-subnet"></a>Erstellen eines Subnetzes

Erstellen Sie mit [az network vnet subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) ein Subnetz. Im folgenden Beispiel wird ein Subnetz namens *Privat* im virtuellen Netzwerk *myVirtualNetwork* mit dem Adresspräfix *10.0.1.0/24* erstellt. Das Adresspräfix muss innerhalb des für das virtuelle Netzwerk festgelegten Adresspräfixes liegen und darf sich nicht mit dem Adresspräfix anderer Subnetze im virtuellen Netzwerk überschneiden.

```azurecli-interactive 
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24
```

Es wird empfohlen, dass Sie sich vor der Bereitstellung von virtuellen Azure-Netzwerken und Subnetzen für die Produktion eingehend mit den [Überlegungen](manage-virtual-network.md#create-a-virtual-network) zu Adressbereichen und den [Netzwerkgrenzwerten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vertraut machen. Nach der Bereitstellung von Ressourcen in Subnetzen müssen bei bestimmten Änderungen an virtuellen Netzwerken und Subnetzen (etwa bei Änderung der Adressbereiche) unter Umständen in Subnetzen bereitgestellte vorhandene Azure-Ressourcen erneut bereitgestellt werden.

## <a name="test-network-communication"></a>Testen der Netzwerkkommunikation

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen mit dem Internet und privat miteinander kommunizieren. Eine Art von Ressource, die Sie in einem virtuellen Netzwerk bereitstellen können, ist ein virtueller Computer. Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie die Netzwerkkommunikation zwischen ihnen und dem Internet in einem späteren Schritt überprüfen können.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie mit dem Befehl [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. Im folgenden Beispiel wird ein virtueller Computer namens *myVm1* im Subnetz *Öffentlich* erstellt. Der Parameter `--no-wait` ermöglicht Azure die Ausführung des Befehls im Hintergrund, sodass Sie mit dem nächsten Befehl fortfahren können. Zur Optimierung dieses Tutorials wird ein Kennwort verwendet. Schlüssel werden in der Regel in Produktionsbereitstellungen verwendet. Bei der Verwendung von Schlüsseln müssen Sie außerdem SSH-Agent-Weiterleitung konfigurieren, um die verbleibenden Schritte auszuführen. Weitere Informationen zur SSH-Agent-Weiterleitung finden Sie in der Dokumentation für Ihren SSH-Client. Ersetzen Sie `<replace-with-your-password>` im folgenden Befehl durch ein Kennwort Ihrer Wahl.

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

Azure hat automatisch 10.0.0.4 als private IP-Adresse des virtuellen Computers zugewiesen, da 10.0.0.4 die erste verfügbare IP-Adresse im Subnetz *Öffentlich* ist. 

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
Das Erstellen des virtuellen Computers dauert einige Minuten. Nachdem die virtuelle Computer erstellt wurde, gibt die Azure-Befehlszeilenschnittstelle eine Ausgabe ähnlich dem folgenden Beispiel zurück: 

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

In der Beispielausgabe hat **privateIpAddress** den Wert *10.0.1.4*. Azure hat eine [Netzwerkschnittstelle](virtual-network-network-interface.md) erstellt und an den virtuellen Computer angefügt sowie der Netzwerkschnittstelle eine private IP-Adresse und eine **MAC-Adresse** zugewiesen. Azure DHCP hat der Netzwerkschnittstelle automatisch 10.0.1.4 zugewiesen, da es sich dabei um die erste verfügbare IP-Adresse im Subnetz *Privat* handelt. Die private IP-Adresse und die MAC-Adresse bleiben der Netzwerkschnittstelle zugewiesen, bis diese gelöscht wird. 

Notieren Sie sich **publicIpAddress**. Über diese Adresse wird in einem späteren Schritt über das Internet auf den virtuellen Computer zugegriffen. Einem virtuellen Computer muss zwar keine öffentliche IP-Adresse zugewiesen werden, Azure weist aber dennoch standardmäßig jedem von Ihnen erstellten virtuellen Computer eine öffentliche IP-Adresse zu. Für die Kommunikation zwischen Internet und virtuellem Computer muss einem virtuellen Computer eine öffentliche IP-Adresse zugewiesen werden. Alle virtuellen Computer können in ausgehender Richtung mit dem Internet kommunizieren, unabhängig davon, ob dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu ausgehenden Internetverbindungen finden Sie unter [Ausgehende Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Die in diesem Artikel erstellten virtuellen Computer verfügen über eine [Netzwerkschnittstelle](virtual-network-network-interface.md) mit einer IP-Adresse, die der Netzwerkschnittstelle dynamisch zugewiesen wird. Nach dem Bereitstellen des virtuellen Computers können Sie [mehrere öffentliche und private IP-Adressen hinzufügen oder für die IP-Adresszuweisung die statische Methode festlegen](virtual-network-network-interface-addresses.md#add-ip-addresses). Sie können [Netzwerkschnittstellen hinzufügen](virtual-network-network-interface-vm.md#vm-add-nic). Dabei gilt in Bezug auf die Anzahl der Grenzwert, der von der [VM-Größe](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) unterstützt wird, die Sie beim Erstellen eines virtuellen Computers auswählen. Sie können für einen virtuellen Computer auch [E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) aktivieren](create-vm-accelerated-networking-cli.md). Dies ist jedoch nur möglich, wenn Sie einen virtuellen Computer mit einer VM-Größe erstellen, die diese Funktion unterstützt.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Kommunikation zwischen virtuellen Computern und dem Internet

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit dem virtuellen Computer *myVmMgmt*. Ersetzen Sie `<publicIpAddress>` durch die öffentliche IP-Adresse des virtuellen Computers. Im vorherigen Beispiel lautet die öffentliche IP-Adresse *13.90.242.231*. Geben Sie bei entsprechender Aufforderung das Kennwort ein, das Sie unter [Erstellen virtueller Computer](#create-virtual-machines) eingegeben haben.

```bash 
ssh azureuser@<publicIpAddress>
```

Aus Sicherheitsgründen wird die Anzahl virtueller Computer, mit denen in einem virtuellen Netzwerk eine Verbindung hergestellt werden kann, häufig begrenzt. In diesem Tutorial wird der virtuelle Computer *myVmMgmt* zur Verwaltung des virtuellen Computers *myVmWeb* im virtuellen Netzwerk verwendet. Verwenden Sie den folgenden Befehl, um vom virtuellen Computer *myVmMgmt* eine SSH-Verbindung mit dem virtuellen Computer *myVmWeb* herzustellen:

```bash 
ssh azureuser@myVmWeb
```

Geben Sie an einer Eingabeaufforderung den folgenden Befehl ein, um über den virtuellen Computer *myVmWeb* mit dem virtuellen Computer *myVmMgmt* zu kommunizieren:

```
ping -c 4 myvmmgmt
```

Eine Ausgabe ähnlich folgendem Beispiel wird angezeigt:
    
```
PING myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net (10.0.1.4) 56(84) bytes of data.
64 bytes from 10.0.1.4: icmp_seq=1 ttl=64 time=1.45 ms
64 bytes from 10.0.1.4: icmp_seq=2 ttl=64 time=0.628 ms
64 bytes from 10.0.1.4: icmp_seq=3 ttl=64 time=0.529 ms
64 bytes from 10.0.1.4: icmp_seq=4 ttl=64 time=0.674 ms

--- myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3029ms
rtt min/avg/max/mdev = 0.529/0.821/1.453/0.368 ms
```
      
Sie sehen, dass die Adresse des virtuellen Computers *myVmMgmt* 10.0.1.4 lautet. 10.0.1.4 war die erste verfügbare IP-Adresse im Adressbereich des Subnetzes *Privat*, in dem Sie in einem vorherigen Schritt den virtuellen Computer *myVmMgmt* bereitgestellt haben.  Der vollqualifizierte Domänenname des virtuellen Computers lautet *myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net*. Der Teil *hxehizax3z1udjnrx1r4gr30pg* des Domänennamens weicht für Ihren virtuellen Computer zwar ab, die übrigen Teil sind jedoch identisch. Standardmäßig verwenden alle virtuellen Azure-Computer den Azure DNS-Standarddienst. Alle virtuellen Computer in einem virtuellen Netzwerk können die Namen aller anderen virtuellen Computer im gleichen virtuellen Netzwerk mithilfe des DNS-Standarddiensts von Azure auflösen. Anstelle des DNS-Standarddiensts von Azure können Sie auch Ihren eigenen DNS-Server oder die Funktion für private Domänen des Azure DNS-Diensts verwenden. Ausführliche Informationen finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) oder [Verwenden von Azure DNS für private Domänen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Verwenden Sie die folgenden Befehle, um den nginx-Webserver auf dem virtuellen Computer *myVmWeb* zu installieren:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

Schließen Sie nach der Installation von nginx die SSH-Sitzung mit *myVmWeb*. Daraufhin wird die Eingabeaufforderung des virtuellen Computers *myVmMgmt* angezeigt. Geben Sie den folgenden Befehl ein, um die nginx-Willkommensseite vom virtuellen Computer *myVmWeb* abzurufen.

```bash
curl myVmWeb
```

Die nginx-Willkommensseite wird zurückgegeben.

Schließen Sie die SSH-Sitzung mit dem virtuellen Computer *myVmMgmt*.

Bei der Erstellung des virtuellen Computers *myVmWeb* durch Azure wurde auch eine öffentliche IP-Adresse namens *myVmWebPublicIP* erstellt und dem virtuellen Computer zugewiesen. Rufen Sie die von Azure zugewiesene öffentliche Adresse mit [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) ab.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroup \
  --name myVmWebPublicIP \
  --query ipAddress
```

Geben Sie auf Ihrem eigenen Computer den folgenden Befehl ein. Ersetzen Sie dabei `<publicIpAddress>` durch die Adresse, die mit dem vorherigen Befehl zurückgegeben wurde:

```bash
curl <publicIpAddress>
```

Beim Abrufen der nginx-Willkommensseite auf Ihrem eigenen Computer mithilfe von cURL tritt ein Fehler auf. Der Fehler tritt auf, da beim Bereitstellen der virtuellen Computer von Azure standardmäßig eine Netzwerksicherheitsgruppe für jeden virtuellen Computer erstellt wurde. 

Eine Netzwerksicherheitsgruppe enthält Sicherheitsregeln, die eingehenden und ausgehenden Netzwerkdatenverkehr nach Port und IP-Adresse zulassen oder verweigern. Die von Azure erstellte Standard-Netzwerksicherheitsgruppe lässt über alle Ports die Kommunikation zwischen Ressourcen im gleichen virtuellen Netzwerk zu. Bei virtuellen Linux-Computern verweigert die Standard-Netzwerksicherheitsgruppe den gesamten eingehenden Datenverkehr aus dem Internet an allen Ports mit Ausnahme von TCP-Port 22 (SSH). Daher können Sie standardmäßig auch eine SSH-Sitzung direkt mit dem virtuellen Computer *myVmWeb* über das Internet herstellen, auch wenn Port 22 für einen Webserver nicht offen sein sollte. Da der Befehl `curl` Port 80 nutzt, tritt bei der Kommunikation vom Internet ein Fehler auf, da die Standard-Netzwerksicherheitsgruppe keine Regel enthält, die Datenverkehr über Port 80 zulässt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit [az group delete](/cli/azure/group#az_group_delete) entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie einen virtuellen Computer mit mehreren Subnetzen bereitstellen. Darüber hinaus haben Sie gelernt, dass beim Erstellen eines virtuellen Linux-Computers von Azure eine Netzwerkschnittstelle, die an den virtuellen Computer angefügt wird, sowie eine Netzwerksicherheitsgruppe erstellt wird, die Datenverkehr aus dem Internet nur über Port 22 zulässt. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Netzwerkdatenverkehr nicht für einzelne virtuelle Computer, sondern für Subnetze filtern.

> [!div class="nextstepaction"]
> [Erstellen von Netzwerksicherheitsgruppen mit der Azure CLI](./virtual-networks-create-nsg-arm-cli.md)
