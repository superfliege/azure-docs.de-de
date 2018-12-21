---
title: Erstellen eines virtuellen Netzwerks – Schnellstart – PowerShell | Microsoft-Dokumentation
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
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 4e2808df56684b257898f3e03f8e9ca36682063b
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341914"
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Schnellstart: Erstellen eines virtuellen Netzwerks über PowerShell

In einem virtuellen Netzwerk können Azure-Ressourcen wie virtuelle Computer (VMs) privat miteinander und mit dem Internet kommunizieren. In dieser Schnellstartanleitung erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen. Nach dem Erstellen eines virtuellen Netzwerks stellen Sie zwei virtuelle Computer im virtuellen Netzwerk bereit. Anschließend stellen Sie über das Internet eine Verbindung mit den VMs her und kommunizieren privat über das virtuelle Netzwerk.

Wenn Sie kein Azure-Abonnement besitzen, können Sie jetzt ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell stattdessen lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart mindestens Version 5.4.1 des AzureRM PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Informationen zum Installieren oder Upgraden finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps).

Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen. Mit diesem Befehl wird eine Verbindung mit Azure hergestellt.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Erstellen einer Ressourcengruppe und eines virtuellen Netzwerks

Es gibt eine Handvoll Schritte, die Sie durchlaufen müssen, um Ihre Ressourcengruppe und Ihr virtuelles Netzwerk zu konfigurieren.

### <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Bevor Sie ein virtuelles Netzwerk erstellen können, müssen Sie eine Ressourcengruppe zum Hosten des virtuellen Netzwerks erstellen. Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) eine Ressourcengruppe. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. In diesem Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen *myVirtualNetwork* am Standort *EastUS* erstellt:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Hinzufügen eines Subnetzes

Azure stellt Ressourcen innerhalb eines virtuellen Netzwerks in einem Subnetz bereit, daher müssen Sie ein Subnetz erstellen. Erstellen Sie eine Subnetzkonfiguration mit der Bezeichnung *default* mit [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

### <a name="associate-the-subnet-to-the-virtual-network"></a>Zuweisen eines Subnetzes zum virtuellen Netzwerk

Schreiben Sie die Subnetzkonfiguration mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) in das virtuelle Netzwerk. Dieser Befehl erstellt das Subnetz:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk.

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) den ersten virtuellen Computer. Wenn Sie den nächsten Befehl ausführen, werden Sie zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie einen Benutzernamen und ein Kennwort für den virtuellen Computer ein:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Die `-AsJob`-Option erstellt den virtuellen Computer im Hintergrund. Sie können mit dem nächsten Schritt fortfahren.

Wenn Azure beginnt, den virtuellen Computer im Hintergrund zu erstellen, wird in etwas folgendes zurückgegeben:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM
```

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

Erstellen Sie den zweiten virtuellen Computer mit diesem folgenden Befehl:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

Sie müssen einen weiteren Benutzer und ein weiteres Kennwort anlegen. Das Erstellen der VM dauert in Azure einige Minuten.

> [!IMPORTANT]
> Fahren Sie mit dem nächsten Schritt erst fort, wenn Azure die Erstellung abgeschlossen hat.  Sie erkennen den Abschluss der Erstellung daran, dass eine Ausgabe an PowerShell erfolgt.

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Geben Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Das Beispiel gibt die öffentliche IP-Adresse des virtuellen Computers *myVm1* zurück:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Öffnen Sie eine Eingabeaufforderung auf dem lokalen Computer. Führen Sie den Befehl `mstsc` aus. Ersetzen Sie `<publicIpAddress>` durch die im vorherigen Schritt zurückgegebene öffentliche IP-Adresse:

> [!NOTE]
> Wenn Sie diese Befehle über eine PowerShell-Eingabeaufforderung auf Ihrem lokalen Computer ausgeführt haben und das AzureRM PowerShell-Modul Version 5.4.1 oder höher verwenden, können Sie mit dieser Schnittstelle fortfahren.

```cmd
mstsc /v:<publicIpAddress>
```

Eine RDP-Datei (Remotedesktopprotokoll) (*.rdp*) wird auf Ihren Computer heruntergeladen und es wird ein Remotedesktop geöffnet.

1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

1. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben.

    > [!NOTE]
    > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Sie erhalten möglicherweise eine Zertifikatwarnung. Ist dies der Fall, wählen Sie **Ja** oder **Weiter** aus.

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

1. Öffnen Sie auf dem Remote Desktop von *myVm1* PowerShell.

1. Geben Sie `ping myVm2` ein.

    Es wird etwa diese Meldung angezeigt:

    ```powershell
    PS C:\Users\myVm1> ping myVm2

    Pinging myVm2.ovvzzdcazhbu5iczfvonhg2zrb.bx.internal.cloudap
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.0.0.5:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Bei Ping tritt ein Fehler auf, weil Internet Control Message-Protokoll (ICMP) verwendet wird. Standardmäßig kann ICMP nicht durch Ihre Windows-Firewall ausgeführt werden.

1. Damit *myVm2* in einem späteren Schritt ein Pingsignal an *myVm1* senden kann, geben Sie diesen Befehl ein:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Dieser Befehl ermöglicht den eingehenden ICMP-Datenverkehr durch die Windows-Firewall.

1. Schließen Sie die Remotedesktopverbindung mit *myVm1*.

1. Wiederholen Sie die Schritte in [Herstellen einer Verbindung mit einem virtuellen Computer über das Internet](#connect-to-a-vm-from-the-internet). Dieses Mal stellen Sie eine Verbindung zu *myVm2* her.

1. Geben Sie an einer Eingabeaufforderung auf dem virtuellen Computer *myVm2* den Befehl `ping myvm1` ein.

    Es wird etwa diese Meldung angezeigt:

    ```cmd
    C:\windows\system32>ping myVm1

    Pinging myVm1.e5p2dibbrqtejhq04lqrusvd4g.bx.internal.cloudapp.net [10.0.0.4] with 32 bytes of data:
    Reply from 10.0.0.4: bytes=32 time=2ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.0.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 2ms, Average = 0ms
    ```

    Sie erhalten Antworten von *myVm1*, da Sie in einem vorherigen Schritt das ICMP über die Windows-Firewall auf dem virtuellen Computer *myVm1* zugelassen haben.

1. Schließen Sie die Remotedesktopverbindung mit *myVm2*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Bearbeitung des virtuellen Netzwerks und der virtuellen Computer abgeschlossen haben, verwenden Sie [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup), um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein virtuelles Standardnetzwerk und zwei virtuelle Computer erstellt. Sie haben über das Internet eine Verbindung mit einem der virtuellen Computer hergestellt und privat zwischen den beiden virtuellen Computern kommuniziert. Weitere Informationen zu den Einstellungen des virtuellen Netzwerks finden Sie unter [Create, change, or delete a virtual network](manage-virtual-network.md) (Erstellen, Ändern oder Löschen eines virtuellen Netzwerks).

Azure erlaubt uneingeschränkte private Kommunikation zwischen virtuellen Computern. Standardmäßig ermöglicht Azure nur eingehende Remotedesktopverbindungen mit Windows-VMs über das Internet. Weitere Informationen zum Konfigurieren der verschiedenen Typen der VM-Netzwerkkommunikation finden Sie im Tutorial [Filtern von Netzwerkdatenverkehr mithilfe einer Netzwerksicherheitsgruppe über das Azure-Portal](tutorial-filter-network-traffic.md).
