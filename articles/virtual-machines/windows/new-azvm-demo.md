---
title: "Erstellen virtueller Windows-Computer mithilfe des Cmdlets „New-AzVM“ in Azure Cloud Shell | Microsoft-Dokumentation"
description: "In diesem kurzen Artikel erfahren Sie, wie Sie virtuelle Windows-Computer in Azure Cloud Shell mithilfe des Cmdlets „New-AzVM“ erstellen."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/21/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: c303235ead2af7cfaa368a5b5f00567ae44cfb86
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---

# <a name="create-a-windows-virtual-machine-with-the-new-azvm-preview-in-cloud-shell"></a>Erstellen eines virtuellen Windows-Computers mit dem neuen Cmdlet „New-AzVM“ (Vorschauversion) in Cloud Shell 

Das Cmdlet „New-AzVM“ (Vorschauversion) vereinfacht die Erstellung eines neuen virtuellen Computers mithilfe von PowerShell. In diesem Leitfaden erfahren Sie, wie Sie PowerShell in Azure Cloud Shell mit dem vorinstallierten Cmdlet „New-AzVM“ verwenden, um einen neuen virtuellen Azure-Computer unter Windows Server 2016 zu erstellen. Nach Abschluss der Bereitstellung stellen wir eine RDP-Verbindung mit dem Server her.  

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Mithilfe des Cmdlets **New-AzVM** können Sie einen virtuellen Computer mit intelligenten Standardwerten erstellen, die unter anderem die Verwendung des Windows Server 2016 Datacenter-Images aus dem Azure Marketplace umfassen. Wenn Sie „New-AzVM“ für sich allein verwenden, werden Standardwerte für die Ressourcennamen verwendet. In diesem Beispiel legen wir den Parameter **-Name** auf *myVM* fest. Das Cmdlet erstellt alle erforderlichen Ressourcen und verwendet dabei jeweils *myVM* als Präfix für den Ressourcennamen. 

Vergewissern Sie sich in Cloud Shell, dass **PowerShell** ausgewählt ist, und geben Sie Folgendes ein:

```azurepowershell-interactive
New-AzVm -Name myVM
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

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema haben Sie mithilfe von „New-AzVM“ einen einfachen virtuellen Computer bereitgestellt und anschließend eine RDP-Verbindung damit hergestellt. Fahren Sie mit dem Tutorial für virtuelle Windows-Computer fort, um weitere Informationen zu virtuellen Azure-Computern zu erhalten.

> [!div class="nextstepaction"]
> [Azure-Tutorials zu virtuellen Windows-Computern](./tutorial-manage-vm.md)

