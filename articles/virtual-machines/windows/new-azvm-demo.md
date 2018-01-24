---
title: "Erstellen virtueller Windows-Computer mithilfe des vereinfachten Cmdlets „New-AzureRMVM“ in Azure Cloud Shell | Microsoft-Dokumentation"
description: "In diesem kurzen Artikel erfahren Sie, wie Sie virtuelle Windows-Computer in Azure Cloud Shell mithilfe des vereinfachten Cmdlets „New-AzureRMVM“ erstellen."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 94eb6232cf59d502a9d70545785c3788398f4d27
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2017
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azurermvm-cmdlet-in-cloud-shell"></a>Erstellen eines virtuellen Windows-Computers mithilfe des vereinfachten Cmdlets „New-AzureRMVM“ in Cloud Shell 

Das Cmdlet [New-AzureRMVM](/powershell/module/azurerm.resources/new-azurermvm) verfügt über einen vereinfachten Parametersatz für die Erstellung eines neuen virtuellen Computers mithilfe von PowerShell. In diesem Thema erfahren Sie, wie Sie mithilfe von PowerShell in Azure Cloud Shell und der vorinstallierten neuesten Version des Cmdlets „New-AzureVM“ einen neuen virtuellen Computer erstellen. Wir verwenden einen vereinfachten Parametersatz, der auf der Grundlage intelligenter Standardwerte automatisch alle erforderlichen Ressourcen erstellt. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 5.1.1 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Mithilfe des Cmdlets [New-AzureRMVM](/powershell/module/azurerm.resources/new-azurermvm) können Sie einen virtuellen Computer mit intelligenten Standardwerten erstellen, die unter anderem die Verwendung des Windows Server 2016 Datacenter-Images aus dem Azure Marketplace umfassen. Sie können „New-AzureRMVM“ mit lediglich dem Parameter **-Name** verwenden, und dieser Wert wird dann für alle Ressourcennamen herangezogen. In diesem Beispiel legen wir den Parameter **-Name** auf *myVM* fest. 

Vergewissern Sie sich in Cloud Shell, dass **PowerShell** ausgewählt ist, und geben Sie Folgendes ein:

```azurepowershell-interactive
New-AzureRMVm -Name myVM
```

Sie werden zur Erstellung eines Benutzernamens und eines Kennworts für den virtuellen Computer aufgefordert. Diese werden später zum Herstellen der Verbindung mit dem virtuellen Computer benötigt. Das Kennwort muss 12 bis 123 Zeichen lang sein und zur Erfüllung der Komplexitätsanforderungen drei der folgenden vier Elemente enthalten: einen Kleinbuchstaben, einen Großbuchstaben, eine Zahl und ein Sonderzeichen.

Die Erstellung des virtuellen Computers und der dazugehörigen Ressourcen dauert einen Moment. Nach Abschluss des Vorgangs können Sie die erstellten Ressourcen mithilfe des Cmdlets [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) anzeigen.

```azurepowershell-interactive
Find-AzureRmResource `
    -ResourceGroupNameEquals myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer her.

Geben Sie mit dem Befehl [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse des virtuellen Computers zurück. Notieren Sie sich diese IP-Adresse.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Öffnen Sie auf Ihrem lokalen Computer eine Eingabeaufforderung, und starten Sie mithilfe des Befehls **mstsc** eine Remotedesktopsitzung mit Ihrem neuen virtuellen Computer. Ersetzen Sie &lt;publicIPAddress&gt; durch die IP-Adresse Ihres virtuellen Computers. Geben Sie den bei der Erstellung des virtuellen Computers angegebenen Benutzernamen und das dazugehörige Kennwort ein, wenn Sie dazu aufgefordert werden.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Angeben unterschiedlicher Ressourcennamen

Sie können auch aussagekräftigere Namen für die Ressourcen angeben und sie trotzdem automatisch erstellen lassen. Im folgenden Beispiel haben wir mehrere Ressourcen für den neuen virtuellen Computer benannt (einschließlich einer neuen Ressourcengruppe):

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVM
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema haben Sie mithilfe von „New-AzVM“ einen einfachen virtuellen Computer bereitgestellt und anschließend eine RDP-Verbindung damit hergestellt. Fahren Sie mit dem Tutorial für virtuelle Windows-Computer fort, um weitere Informationen zu virtuellen Azure-Computern zu erhalten.

> [!div class="nextstepaction"]
> [Azure-Tutorials zu virtuellen Windows-Computern](./tutorial-manage-vm.md)
