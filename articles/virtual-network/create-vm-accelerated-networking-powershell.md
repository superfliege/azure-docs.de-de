---
title: Erstellen virtueller Azure-Computer mit beschleunigtem Netzwerkbetrieb | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen virtuellen Linux-Computer mit beschleunigtem Netzwerkbetrieb erstellen.
services: virtual-network
documentationcenter: 
author: jdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: jimdial
ms.openlocfilehash: f4908963e0650be9b12b745f6868a1ba6ad933e4
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking"></a>Erstellen eines virtuellen Windows-Computers mit beschleunigtem Netzwerkbetrieb

> [!IMPORTANT] 
> Virtuelle Computer müssen mit aktiviertem beschleunigtem Netzwerkbetrieb erstellt werden. Dieses Feature kann nicht auf vorhandenen virtuellen Computern aktiviert werden. Anhand der nachfolgenden Schritte können Sie den beschleunigten Netzwerkbetrieb aktivieren:
>   1. Löschen des virtuellen Computers
>   2. Erneutes Erstellen des virtuellen Computers mit aktiviertem beschleunigtem Netzwerkbetrieb
>

In diesem Tutorial erfahren Sie, wie Sie einen virtuellen Windows-Computer (VM) mit beschleunigtem Netzwerkbetrieb erstellen. Der beschleunigte Netzwerkbetrieb ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) in einer VM und somit eine erhebliche Steigerung der Netzwerkleistung. Über diesen für Hochleistung konzipierten Pfad wird der Host des Datenpfads umgangen, um Latenzen, Jitter und CPU-Auslastung zu verringern. So können mit unterstützten VM-Typen die anspruchsvollsten Netzwerkworkloads genutzt werden. Die folgende Abbildung zeigt die Kommunikation zwischen zwei VMs mit und ohne beschleunigten Netzwerkbetrieb:

![Vergleich](./media/create-vm-accelerated-networking/accelerated-networking.png)

Ohne beschleunigten Netzwerkbetrieb muss sämtlicher Netzwerkdatenverkehr zur und von der VM den Host und virtuellen Switch passieren. Der virtuelle Switch setzt alle Richtlinien für den Netzwerkdatenverkehr um, z.B. Netzwerksicherheitsgruppen, Zugriffssteuerungslisten, Isolation und andere virtualisierte Netzwerkdienste. Weitere Informationen zu virtuellen Switches finden Sie im Artikel mit einer [Übersicht über Hyper-V-Netzwerkvirtualisierung und virtuellen Switch](https://technet.microsoft.com/library/jj945275.aspx).

Mit beschleunigtem Netzwerkbetrieb gelangt der Netzwerkdatenverkehr zur Netzwerkkarte (NIC) der VM und wird dann an die VM weitergeleitet. Alle Netzwerkrichtlinien, die vom virtuellen Switch angewendet werden, werden ab sofort ausgelagert und in der Hardware angewendet. Durch das Anwenden von Richtlinien in der Hardware kann die NIC Netzwerkdatenverkehr direkt an den virtuellen Computer weiterleiten und dabei den Host und den virtuellen Switch umgehen, während gleichzeitig sämtliche Richtlinien umgesetzt werden, die sonst auf dem Host angewendet worden wären.

Die Vorteile des beschleunigten Netzwerkbetriebs gelten nur für die VM, auf der dieses Feature aktiviert ist. Die besten Ergebnisse erzielen Sie, wenn Sie dieses Feature auf mindestens zwei VMs aktivieren, die mit demselben virtuellen Azure-Netzwerk (VNet) verbunden sind. Bei der Kommunikation über VNets oder bei lokalen Verbindungen hat dieses Feature nur minimale Auswirkungen auf die Wartezeit.

## <a name="benefits"></a>Vorteile
* **Geringere Latenz/mehr Pakete pro Sekunde (pps):** Durch das Entfernen des virtuellen Switch aus dem Datenpfad wird die Zeit reduziert, die Pakete auf dem Host für die Verarbeitung von Richtlinien warten müssen. Gleichzeitig wird die Anzahl von Paketen erhöht, die im virtuellen Computer verarbeitet werden können.
* **Reduzierte Jitter:** Die Verarbeitung im virtuellen Switch hängt vom Umfang der anzuwendenden Richtlinien und von der Workload der CPU ab, die die Verarbeitung durchführt. Das Auslagern der Richtlinienerzwingung auf die Hardware entfernt diesen Umweg, indem die Pakete direkt an den virtuellen Computer gesendet werden. Damit werden der Host und alle Softwareinterrupts und Kontextwechsel aus der VM-Kommunikation entfernt.
* **Verringerte CPU-Auslastung:** Das Umgehen des virtuellen Switchs auf dem Host führt zu weniger CPU-Auslastung für die Verarbeitung des Netzwerkdatenverkehrs.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Microsoft Windows Server 2012 R2 Datacenter und Windows Server 2016

## <a name="supported-vm-instances"></a>Unterstützte VM-Instanzen
Der beschleunigte Netzwerkbetrieb wird für die meisten Größen universeller, computeoptimierter Instanzen mit mindestens 4 vCPUs unterstützt. Bei Instanzen wie D/DSv3 oder E/ESv3, die Hyperthreading unterstützen, wird der beschleunigte Netzwerkbetrieb auf VM-Instanzen mit mindestens 8 vCPUs unterstützt. Zu den unterstützten Reihen zählen D/DSv2, D/DSv3, E/ESv3, F/Fs/Fsv2 und Ms/Mms.

Weitere Informationen zu VM-Instanzen finden Sie unter [Größen für virtuelle Windows-Computer](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Regionen
Verfügbar in allen öffentlichen Azure-Regionen und in der Azure Government Cloud 

## <a name="limitations"></a>Einschränkungen
Die folgenden Einschränkungen gelten für die Verwendung dieser Funktion:

* **Erstellung von Netzwerkschnittstellen:** Der beschleunigte Netzwerkbetrieb kann nur für eine neue NIC aktiviert werden. Er kann nicht für eine vorhandene NIC aktiviert werden.
* **VM-Erstellung:** Eine NIC mit aktiviertem beschleunigten Netzwerkbetrieb kann nur an eine VM angefügt werden, während diese erstellt wird. Die NIC kann nicht an eine vorhandene VM angefügt werden. Wenn Sie den virtuellen Computer zu einer vorhandenen Verfügbarkeitsgruppe hinzufügen, muss der beschleunigte Netzwerkbetrieb für alle virtuellen Computer in der Verfügbarkeitsgruppe aktiviert sein.
* **Bereitstellung nur über Azure Resource Manager:** Virtuelle Computer (klassisch) können nicht mit beschleunigtem Netzwerkbetrieb bereitgestellt werden.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Installieren Sie [Azure PowerShell](/powershell/azure/install-azurerm-ps), Version 5.1.1 oder höher. Zur Ermittlung der derzeit installierten Version führen Sie `Get-Module -ListAvailable AzureRM` aus. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, installieren Sie die aktuelle Version des AzureRM-Moduls über den [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM). Melden Sie sich in einer PowerShell-Sitzung mithilfe von [Add-AzureRmAccount](/powershell/module/AzureRM.Profile/Add-AzureRmAccount) bei Ihrem Azure-Konto an.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu den Parameternamen zählen z.B. *myResourceGroup*, *myNic* und *myVm*.

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) eine Ressourcengruppe. Im folgenden Beispiel wird am Standort *centralus* eine Ressourcengruppe namens *myResourceGroup* erstellt:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "centralus"
```

Erstellen Sie zunächst mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetworkSubnetConfig) eine Subnetzkonfiguration. Im folgenden Beispiel wird das Subnetz *mySubnet* erstellt:

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
    -Name "mySubnet" `
    -AddressPrefix "192.168.1.0/24"
```

Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork) ein virtuelles Netzwerk mit dem Subnetz *mySubnet*.

```powershell
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "centralus" `
    -Name "myVnet" `
    -AddressPrefix "192.168.0.0/16" `
    -Subnet $Subnet
```

## <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Erstellen Sie zunächst mit [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/AzureRM.Network/New-AzureRmNetworkSecurityRuleConfig) eine Netzwerksicherheitsgruppen-Regel.

```powershell
$rdp = New-AzureRmNetworkSecurityRuleConfig `
    -Name 'Allow-RDP-All' `
    -Description 'Allow RDP' `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 100 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389
```

Erstellen Sie mit [New-AzureRmNetworkSecurityGroup](/powershell/module/AzureRM.Network/New-AzureRmNetworkSecurityGroup) eine Netzwerksicherheitsgruppe, und weisen Sie die Sicherheitsregel *Allow-RDP-All* zu. Zusätzlich zur Regel *Allow-RDP-All* enthält die Netzwerksicherheitsgruppe noch mehrere Standardregeln. Eine Standardregel deaktiviert den gesamten über das Internet eingehenden Zugriff. Daher wird die Regel *Allow-RDP-All* der Netzwerksicherheitsgruppe zugewiesen, sodass Sie eine Remoteverbindung mit dem virtuellen Computer herstellen können, nachdem dieser erstellt wurde.

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroup `
    -Location centralus `
    -Name "myNsg" `
    -SecurityRules $rdp
```

Ordnen Sie mit [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/AzureRM.Network/Set-AzureRmVirtualNetworkSubnetConfig) die Netzwerksicherheitsgruppe dem Subnetz *mySubnet* zu. Die Regel in der Netzwerksicherheitsgruppe gilt für alle Ressourcen, die im Subnetz bereitgestellt werden.

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name 'mySubnet' `
    -AddressPrefix "192.168.1.0/24" `
    -NetworkSecurityGroup $nsg
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Erstellen einer Netzwerkschnittstelle mit beschleunigtem Netzwerkbetrieb
Erstellen Sie mit [New-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/New-AzureRmPublicIpAddress) eine öffentliche IP-Adresse. Wenn Sie nicht vorhaben, über das Internet auf den virtuellen Computer zuzugreifen, ist keine öffentliche IP-Adresse erforderlich – diese wird jedoch für die Durchführung der Schritte in diesem Artikel benötigt.

```powershell
$publicIp = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroup `
    -Name 'myPublicIp' `
    -location centralus `
    -AllocationMethod Dynamic
```

Erstellen Sie mit [New-AzureRmNetworkInterface](/powershell/module/AzureRM.Network/New-AzureRmNetworkInterface) eine Netzwerkschnittstelle mit beschleunigtem Netzwerkbetrieb, und weisen Sie der Netzwerkschnittstelle die öffentliche IP-Adresse zu. Das folgende Beispiel erstellt die Netzwerkschnittstelle *myNic* im Subnetz *mySubnet* des virtuellen Netzwerks *myVnet* und weist ihr die öffentliche IP-Adresse *myPublicIp*  zu:

```powershell
$nic = New-AzureRmNetworkInterface `
    -ResourceGroupName "myResourceGroup" `
    -Name "myNic" `
    -Location "centralus" `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIp.Id `
    -EnableAcceleratedNetworking
```

## <a name="create-the-virtual-machine"></a>Erstellen des virtuellen Computers

Legen Sie Ihre VM-Anmeldeinformationen mit [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential) auf die `$cred`-Variable fest:

```powershell
$cred = Get-Credential
```

Definieren Sie zunächst Ihren virtuellen Computer mit [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Im folgenden Beispiel wird die VM *myVM* mit einer VM-Größe definiert, die beschleunigten Netzwerkbetrieb unterstützt (*Standard_DS4_v2*):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVm" -VMSize "Standard_DS4_v2"
```

Eine Liste aller VM-Größen und -Eigenschaften finden Sie unter [Größen für virtuelle Windows-Computer](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Erstellen Sie die restliche Konfiguration des virtuellen Computers mit [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) und [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Im folgenden Beispiel wird eine Windows Server 2016-VM erstellt:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
    -Windows `
    -ComputerName "myVM" `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
    -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" `
    -Skus "2016-Datacenter" `
    -Version "latest"
```

Fügen Sie die beiden Netzwerkschnittstellen, die Sie zuvor erstellt haben, mit [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) an:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

Erstellen Sie dann Ihren virtuellen Computer mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "centralus"
```

## <a name="confirm-the-driver-is-installed-in-the-operating-system"></a>Überprüfen, ob der Treiber im Betriebssystem installiert ist

Stellen Sie nach dem Erstellen des virtuellen Computers in Azure eine Verbindung mit dem virtuellen Computer her, und vergewissern Sie sich, dass der Treiber unter Windows installiert ist. 

1. Navigieren Sie in einem Internetbrowser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich bei Ihrem Azure-Konto an.
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *myVm* ein. Wenn **myVm** in den Suchergebnissen angezeigt wird, klicken Sie darauf. Wenn **Wird erstellt** unter der Schaltfläche **Verbinden** angezeigt wird, hat Azure das Erstellen der VM noch nicht abgeschlossen. Klicken Sie erst in der linken oberen Ecke auf **Verbinden**, nachdem **Wird erstellt** unter der Schaltfläche **Verbinden** nicht mehr angezeigt wird.
3. Geben Sie den Benutzernamen und das Kennwort ein, die Sie in [Erstellen des virtuellen Computers](#create-the-virtual-machine) verwendet haben. Wenn Sie noch nie eine Verbindung mit einer Windows-VM in Azure hergestellt haben, finden Sie unter [Herstellen der Verbindung mit dem virtuellen Computer](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-virtual-machine) weitere Informationen.
4. Klicken Sie mit der rechten Maustaste auf die Windows-Schaltfläche „Start“, und klicken Sie auf **Geräte-Manager**. Erweitern Sie den Knoten **Netzwerkadapter**. Vergewissern Sie sich, dass **Mellanox ConnectX-3 Virtual Function Ethernet Adapter** (wie in der folgenden Abbildung gezeigt) angezeigt wird:
   
    ![Geräte-Manager](./media/create-vm-accelerated-networking/device-manager.png)

Der beschleunigte Netzwerkbetrieb ist nun für Ihre VM aktiviert.
