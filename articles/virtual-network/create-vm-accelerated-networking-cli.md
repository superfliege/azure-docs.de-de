---
title: Erstellen virtueller Azure-Computer mit beschleunigtem Netzwerkbetrieb | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen virtuellen Linux-Computer mit beschleunigtem Netzwerkbetrieb erstellen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/02/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c051fec3369ef0d309ecf6c68b17272bb396eeec
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="create-a-linux-virtual-machine-with-accelerated-networking"></a>Erstellen eines virtuellen Linux-Computers mit beschleunigtem Netzwerkbetrieb

> [!IMPORTANT] 
> Virtuelle Computer müssen mit aktiviertem beschleunigtem Netzwerkbetrieb erstellt werden. Dieses Feature kann nicht auf vorhandenen virtuellen Computern aktiviert werden. Anhand der nachfolgenden Schritte können Sie den beschleunigten Netzwerkbetrieb aktivieren:
>   1. Löschen Sie den virtuellen Computer.
>   2. Erstellen Sie erneut einen virtuellen Computer mit aktiviertem beschleunigtem Netzwerkbetrieb.
>

In diesem Tutorial erfahren Sie, wie Sie einen virtuellen Linux-Computer (VM) mit beschleunigtem Netzwerkbetrieb erstellen. Der beschleunigte Netzwerkbetrieb ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) in einer VM und somit eine erhebliche Steigerung der Netzwerkleistung. Über diesen für Hochleistung konzipierten Pfad wird der Host des Datenpfads umgangen, um Latenzen, Jitter und CPU-Auslastung zu verringern. So können mit unterstützten VM-Typen die anspruchsvollsten Netzwerkworkloads genutzt werden. Die folgende Abbildung zeigt die Kommunikation zwischen zwei VMs mit und ohne beschleunigten Netzwerkbetrieb:

![Vergleich](./media/create-vm-accelerated-networking/accelerated-networking.png)

Ohne beschleunigten Netzwerkbetrieb muss sämtlicher Netzwerkdatenverkehr zur und von der VM den Host und virtuellen Switch passieren. Der virtuelle Switch setzt alle Richtlinien für den Netzwerkdatenverkehr um, z.B. Netzwerksicherheitsgruppen, Zugriffssteuerungslisten, Isolation und andere virtualisierte Netzwerkdienste. Weitere Informationen zu virtuellen Switches finden Sie im Artikel mit einer [Übersicht über Hyper-V-Netzwerkvirtualisierung und virtuellen Switch](https://technet.microsoft.com/library/jj945275.aspx).

Mit beschleunigtem Netzwerkbetrieb gelangt der Netzwerkdatenverkehr zur Netzwerkkarte (NIC) der VM und wird dann an die VM weitergeleitet. Alle Netzwerkrichtlinien, die vom virtuellen Switch angewendet werden, werden ab sofort ausgelagert und in der Hardware angewendet. Durch das Anwenden von Richtlinien in der Hardware kann die NIC Netzwerkdatenverkehr direkt an den virtuellen Computer weiterleiten und dabei den Host und den virtuellen Switch umgehen, während gleichzeitig sämtliche Richtlinien umgesetzt werden, die sonst auf dem Host angewendet worden wären.

Die Vorteile des beschleunigten Netzwerkbetriebs gelten nur für die VM, auf der dieses Feature aktiviert ist. Die besten Ergebnisse erzielen Sie, wenn Sie dieses Feature auf mindestens zwei VMs aktivieren, die mit demselben virtuellen Azure-Netzwerk (VNet) verbunden sind. Bei der Kommunikation über VNets oder bei lokalen Verbindungen hat dieses Feature nur minimale Auswirkungen auf die Wartezeit.

## <a name="benefits"></a>Vorteile
* **Geringere Latenz/mehr Pakete pro Sekunde (pps):** Durch das Entfernen des virtuellen Switch aus dem Datenpfad wird die Zeit reduziert, die Pakete auf dem Host für die Verarbeitung von Richtlinien warten müssen. Gleichzeitig wird die Anzahl von Paketen erhöht, die im virtuellen Computer verarbeitet werden können.
* **Reduzierte Jitter:** Die Verarbeitung im virtuellen Switch hängt vom Umfang der anzuwendenden Richtlinien und von der Workload der CPU ab, die die Verarbeitung durchführt. Das Auslagern der Richtlinienerzwingung auf die Hardware entfernt diesen Umweg, indem die Pakete direkt an den virtuellen Computer gesendet werden. Damit werden der Host und alle Softwareinterrupts und Kontextwechsel aus der VM-Kommunikation entfernt.
* **Verringerte CPU-Auslastung:** Das Umgehen des virtuellen Switchs auf dem Host führt zu weniger CPU-Auslastung für die Verarbeitung des Netzwerkdatenverkehrs.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
* **Ubuntu 16.04**: 4.11.0-1013 oder eine höhere Kernelversion
* **SLES SP3**: 4.4.92-6.18 oder eine höhere Kernelversion
* **RHEL**: 7.4.2017120423 oder eine höhere Kernelversion
* **CentOS**: 7.4.20171206 oder eine höhere Kernelversion

## <a name="supported-vm-instances"></a>Unterstützte VM-Instanzen
Der beschleunigte Netzwerkbetrieb wird für die meisten Größen universeller, computeoptimierter Instanzen mit mindestens 4 vCPUs unterstützt. Bei Instanzen wie D/DSv3 oder E/ESv3, die Hyperthreading unterstützen, wird der beschleunigte Netzwerkbetrieb auf VM-Instanzen mit mindestens 8 vCPUs unterstützt.  Zu den unterstützten Reihen zählen D/DSv2, D/DSv3, E/ESv3, F/Fs/Fsv2 und Ms/Mms. 

Weitere Informationen zu VM-Instanzen finden Sie unter [Größen für virtuelle Linux-Computer in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Regionen
In allen öffentlichen Azure-Regionen außer in „Asien, Osten“ verfügbar.   Die Azure Government-Cloud wird noch nicht unterstützt.

## <a name="limitations"></a>Einschränkungen
Die folgenden Einschränkungen gelten für die Verwendung dieser Funktion:

* **Erstellung von Netzwerkschnittstellen:** Der beschleunigte Netzwerkbetrieb kann nur für eine neue NIC aktiviert werden. Er kann nicht für eine vorhandene NIC aktiviert werden.
* **VM-Erstellung:** Eine NIC mit aktiviertem beschleunigten Netzwerkbetrieb kann nur an eine VM angefügt werden, während diese erstellt wird. Die NIC kann nicht an eine vorhandene VM angefügt werden. Wenn Sie den virtuellen Computer zu einer vorhandenen Verfügbarkeitsgruppe hinzufügen, muss der beschleunigte Netzwerkbetrieb für alle virtuellen Computer in der Verfügbarkeitsgruppe aktiviert sein.
* **Bereitstellung nur über den Azure Resource Manager:** Virtuelle Computer (klassisch) können nicht mit beschleunigtem Netzwerkbetrieb bereitgestellt werden.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Installieren Sie die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#az_login) bei einem Azure-Konto an. Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu Parameternamen zählen z.B. *myResourceGroup*, *myNic* und *myVm*.

Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. Im folgenden Beispiel wird am Standort *centralus* eine Ressourcengruppe namens *myResourceGroup* erstellt:

```azurecli
az group create --name myResourceGroup --location centralus
```

Sie müssen eine unterstützte Linux-Region aus der Auflistung unter [Linux accelerated networking (Linux-Regionen mit beschleunigtem Netzwerkbetrieb)](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview) auswählen.

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVnet* mit einem Subnetz erstellt:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe
Erstellen Sie mit [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) eine Netzwerksicherheitsgruppe. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens *myNetworkSecurityGroup* erstellt:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Die Netzwerksicherheitsgruppe enthält mehrere Standardregeln, von denen eine sämtlichen eingehenden Zugriff über das Internet deaktiviert. Öffnen Sie mit [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) einen Port, um SSH-Zugriff auf den virtuellen Computer zuzulassen:

```azurecli
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup \
  --name Allow-SSH-Internet \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 22
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Erstellen einer Netzwerkschnittstelle mit beschleunigtem Netzwerkbetrieb

Erstellen Sie mit [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) eine öffentliche IP-Adresse. Wenn Sie nicht vorhaben, über das Internet auf den virtuellen Computer zuzugreifen, ist keine öffentliche IP-Adresse erforderlich – diese wird jedoch für die Durchführung der Schritte in diesem Artikel benötigt.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Erstellen Sie mit [az network nic create](/cli/azure/network/nic#az_network_nic_create) eine Netzwerkschnittstelle mit aktiviertem beschleunigtem Netzwerkbetrieb. Im folgenden Beispiel wird eine Netzwerkschnittstelle mit dem Namen *myNic* im Subnetz *mySubnet* des virtuellen Netzwerks *myVnet* erstellt und die Netzwerksicherheitsgruppe  *myNetworkSecurityGroup* der Netzwerkschnittstelle zugeordnet:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --accelerated-networking true \
    --public-ip-address myPublicIp \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nic"></a>Erstellen einer VM und Anfügen der NIC
Geben Sie bei der Erstellung der VM mit `--nics` die NIC an, die Sie erstellt haben. Sie müssen eine Größe und Verteilung aus der Auflistung unter [Linux accelerated Networking (Linux-Regionen mit beschleunigtem Netzwerkbetrieb)](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview) auswählen. 

Erstellen Sie mit [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. Im folgenden Beispiel wird eine VM namens *myVM* mit dem UbuntuLTS-Image und einer Größe erstellt, die beschleunigten Netzwerkbetrieb unterstützt (*Standard_DS4_v2*):

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS4_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Eine Liste aller VM-Größen und -Eigenschaften finden Sie unter [Größen für virtuelle Linux-Computer in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Nachdem die VM erstellt wurde, wird eine ähnliche Ausgabe wie die folgende Beispielausgabe zurückgegeben. Notieren Sie sich **publicIpAddress**. Diese Adresse wird später verwendet, um auf die VM zuzugreifen.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "centralus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "192.168.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="confirm-that-accelerated-networking-is-enabled"></a>Bestätigen der Aktivierung des beschleunigten Netzwerkbetriebs

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit dem virtuellen Computer. Ersetzen Sie `<your-public-ip-address>` durch die öffentliche IP-Adresse, die dem von Ihnen erstellten virtuellen Computer zugewiesen wurde, und ersetzen Sie *azureuser*, wenn Sie für `--admin-username` einen anderen Wert als bei der VM-Erstellung verwendet haben.

```bash
ssh azureuser@<your-public-ip-address>
```

Geben Sie in der Bash-Shell `uname -r` ein, und vergewissern Sie sich, dass es sich bei der Kernelversion um eine der folgenden Versionen oder eine höhere Version handelt:

* **Ubuntu 16.04**: 4.11.0-1013
* **SLES SP3**: 4.4.92-6.18
* **RHEL**: 7.4.2017120423
* **CentOS**: 7.4.20171206


Vergewissern Sie sich, dass mithilfe des Befehls `lspci` das Mellanox VF-Gerät für die VM verfügbar gemacht wurde. Die zurückgegebene Ausgabe sieht in etwa wie folgt aus:

```bash
0000:00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
0000:00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
0000:00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
0000:00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
0000:00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA
0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
```

Suchen Sie mithilfe des Befehl `ethtool -S eth0 | grep vf_` nach der Aktivität für die VF (virtuelle Funktion). Wenn Sie eine Ausgabe ähnlich wie die folgende Beispielausgabe erhalten, ist der beschleunigte Netzwerkbetrieb aktiviert und funktioniert.

```bash
vf_rx_packets: 992956
vf_rx_bytes: 2749784180
vf_tx_packets: 2656684
vf_tx_bytes: 1099443970
vf_tx_dropped: 0
```
Der beschleunigte Netzwerkbetrieb ist nun für Ihre VM aktiviert.
