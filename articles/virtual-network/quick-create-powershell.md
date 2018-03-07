---
title: "Erstellen eines virtuellen Netzwerks in Azure – PowerShell | Microsoft-Dokumentation"
description: "Erfahren Sie schnell, wie Sie mithilfe von PowerShell ein virtuelles Netzwerk erstellen. In einem virtuellen Netzwerk können viele Arten von Azure-Ressourcen privat miteinander kommunizieren."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: dd8203763eb6abd19e2b3483636dc4d80f7effdf
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="create-a-virtual-network-using-powershell"></a>Erstellen eines virtuellen Netzwerks über PowerShell

In diesem Artikel erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen. Nachdem Sie ein virtuelles Netzwerk erstellt haben, stellen Sie zwei virtuelle Computer im virtuellen Netzwerk bereit, um die private Netzwerkkommunikation zwischen ihnen zu testen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.1.1 des AzureRM PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*. Alle Azure-Ressourcen werden an einem Azure-Standort (oder in einer Region) erstellt.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen *myVirtualNetwork* am Standort *USA, Osten* erstellt:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/24
```

Jedem virtuellen Netzwerk ist mindestens ein Adresspräfix zugewiesen. Der Adressraum wird in CIDR-Notation angegeben. Die Adressraum 10.0.0.0/24 umfasst die Adressen 10.0.0.0 bis 10.0.0.254. Virtuelle Netzwerke weisen null oder mehr Subnetze auf. Ressourcen werden in einem Subnetz in einem virtuellen Netzwerk bereitgestellt. 

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) eine Subnetzkonfiguration. Alle Subnetze weisen ein Adresspräfix innerhalb des Adresspräfixes des virtuellen Netzwerks auf. In diesem Beispiel wird eine Subnetzkonfiguration mit demselben Adresspräfix wie das Adresspräfix des virtuellen Netzwerks erstellt:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Obwohl das Adresspräfix des Subnetzes 10.0.0.0 bis 10.0.0.254 umfasst, sind nur die Adressen 10.0.0.4 bis 10.0.0.254 verfügbar, da Azure die ersten vier Adressen (0 bis 3) und die letzte Adresse in jedem Subnetz reserviert. Da das Adresspräfix des Subnetzes mit dem Adresspräfix des virtuellen Netzwerks übereinstimmt, kann nur ein Subnetz in diesem virtuellen Netzwerk vorhanden sein.

Schreiben Sie die Subnetzkonfiguration mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) in das virtuelle Netzwerk. Dabei wird das folgende Subnetz erstellt:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="test-network-communication"></a>Testen der Netzwerkkommunikation

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen privat miteinander kommunizieren. Eine Art von Ressource, die Sie in einem virtuellen Netzwerk bereitstellen können, ist ein virtueller Computer. Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk, damit Sie die private Kommunikation zwischen ihnen in einem späteren Schritt überprüfen können.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) einen virtuellen Computer. Wenn Sie diesen Schritt ausführen, werden Sie aufgefordert, Anmeldeinformationen einzugeben. Die Werte, die Sie eingeben, werden als Benutzername und Kennwort für den virtuellen Computer konfiguriert. Der Standort, an dem ein virtueller Computer erstellt wird, muss mit dem Standort des virtuellen Netzwerks identisch sein. Der virtuelle Computer muss sich nicht in derselben Ressourcengruppe wie das virtuelle Netzwerk befinden, auch wenn dies in diesem Artikel der Fall ist. Mit dem `-AsJob`-Parameter können Sie den Befehl im Hintergrund ausführen, sodass Sie mit der nächsten Aufgabe fortfahren können.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Es wird eine Ausgabe ähnlich der folgenden Beispielausgabe zurückgegeben, und Azure beginnt, den virtuellen Computer im Hintergrund zu erstellen.

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

Azure DHCP weist dem virtuellen Computer bei der Erstellung automatisch 10.0.0.4 zu, da es sich dabei um die erste verfügbare Adresse im Subnetz *default* handelt.

Erstellen Sie einen zweiten virtuellen Computer. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```
Das Erstellen des virtuellen Computers dauert einige Minuten. Nach der Erstellung gibt Azure eine Ausgabe zum erstellten virtuellen Computer zurück. Azure hat dem virtuellen Computer *myVm2* die Adresse *10.0.0.5* zugewiesen, da es sich dabei um die nächste verfügbare Adresse im Subnetz handelte. Diese ist jedoch in der zurückgegebenen Ausgabe nicht enthalten.

### <a name="connect-to-a-virtual-machine"></a>Herstellen einer Verbindung mit einem virtuellen Computer

Geben Sie mit dem Befehl [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Azure weist in der Standardeinstellung jedem virtuellen Computer eine öffentliche, im Internet routingfähige IP-Adresse zu. Die öffentliche IP-Adresse wird dem virtuellen Computer aus einem [Pool von jeder Azure-Region zugewiesenen Adressen](https://www.microsoft.com/download/details.aspx?id=41653) zugewiesen. Auch wenn Azure bekannt ist, welche öffentliche IP-Adresse einem virtuellen Computer zugewiesen ist, hat das auf einem virtuellen Computer ausgeführte Betriebssystem keine Informationen über die ihm zugewiesene öffentliche IP-Adresse. Das folgende Beispiel gibt die öffentliche IP-Adresse des virtuellen Computers *myVm1* zurück:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -Name myVm1 -ResourceGroupName myResourceGroup | Select IpAddress
```

Erstellen Sie mit dem folgenden Befehl auf Ihrem lokalen Computer eine Remotedesktopsitzung mit dem virtuellen Computer *myVm1*. Ersetzen Sie `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene IP-Adresse.

```
mstsc /v:<publicIpAddress>
```

Eine RDP-Datei (Remotedesktopprotokoll) wird erstellt, auf Ihren Computer heruntergeladen und geöffnet. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben, und klicken Sie dann auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

### <a name="validate-communication"></a>Überprüfen der Kommunikation

Beim Versuch, einen virtuellen Windows-Computer mit Ping zu erreichen, tritt ein Fehler auf, da Ping über die Windows-Firewall standardmäßig nicht zugelassen wird. Um ein Ping an *myVm1* zu ermöglichen, geben Sie an einer Eingabeaufforderung den folgenden Befehl ein:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Um die Kommunikation mit *myVm2* zu überprüfen, geben Sie an einer Eingabeaufforderung auf dem virtuellen Computer *myVm1* den folgenden Befehl ein. Geben Sie die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen an, und vervollständigen Sie die Verbindung:

```
mstsc /v:myVm2
```

Die Remotedesktopverbindung ist erfolgreich, da beide virtuelle Computer über vom Subnetz *default* zugewiesene private IP-Adressen verfügen und Remotedesktop standardmäßig über die Windows-Firewall geöffnet ist. Sie können eine Verbindung mit *myVm2* anhand des Hostnamens herstellen, da Azure automatisch DNS-Namensauflösung für alle Hosts in einem virtuellen Netzwerk bereitstellt. Führen Sie an einer Eingabeaufforderung ein Ping von *myVm2* zu *myVm1* durch.

```
ping myvm1
```

Das Ping ist erfolgreich, da Sie den Befehl in einem vorherigen Schritt durch die Windows-Firewall auf dem virtuellen Computer *myVm1* zugelassen haben. Um die ausgehende Kommunikation mit dem Internet zu bestätigen, geben Sie den folgenden Befehl ein:

```
ping bing.com
```

Sie erhalten vier Antworten von bing.com. Standardmäßig können alle Ressourcen in einem virtuellen Netzwerk in ausgehender Richtung mit dem Internet kommunizieren.

Beenden Sie die Remotedesktopsitzung. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie ein virtuelles Standardnetzwerk mit einem Subnetz bereitgestellt. Um zu erfahren, wie Sie ein benutzerdefiniertes virtuelles Netzwerk mit mehreren Subnetzen erstellen, fahren Sie mit dem Tutorial zum Erstellen eines benutzerdefinierten virtuellen Netzwerks fort.

> [!div class="nextstepaction"]
> [Erstellen eines benutzerdefinierten virtuellen Netzwerks](virtual-networks-create-vnet-arm-pportal.md#powershell)
