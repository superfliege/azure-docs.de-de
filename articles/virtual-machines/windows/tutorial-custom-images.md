---
title: Tutorial – Erstellen benutzerdefinierter VM-Images mit Azure PowerShell | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure PowerShell zum Erstellen eines benutzerdefinierten VM-Images in Azure verwenden.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1481c32512266f494c03f9ffdc1ce67ef503b4a8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255199"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>Tutorial: Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mit Azure PowerShell

Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können zum Starten von Konfigurationen verwendet werden, z.B. zum Vorabladen von Anwendungen, Anwendungskonfigurationen und anderen Betriebssystemkonfigurationen. In diesem Tutorial erstellen Sie ein eigenes benutzerdefiniertes Image eines virtuellen Azure-Computers. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Sysprep und Generalisieren virtueller Computer
> * Erstellen eines benutzerdefinierten Images
> * Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image
> * Liste aller Images in Ihrem Abonnement
> * Löschen eines Images

## <a name="before-you-begin"></a>Voraussetzungen

In den folgenden Schritten wird erläutert, wie Sie eine vorhandene VM in ein wiederverwendbares benutzerdefiniertes Image umwandeln, mit dem Sie neue VM-Instanzen erstellen können.

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Gegebenenfalls können Sie mit diesem [Beispielskript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) einen virtuellen Computer erstellen. Ersetzen Sie beim Durcharbeiten des Tutorials bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des AzureRM-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="prepare-vm"></a>Vorbereiten der VM

Zum Erstellen eines Images eines virtuellen Computers müssen Sie den virtuellen Computer vorbereiten, indem Sie ihn generalisieren, die Zuordnung aufheben und dann den virtuellen Quellcomputer in Azure als generalisiert markieren.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Generalisieren der Windows-VM mithilfe von Sysprep

Sysprep entfernt unter anderem alle persönlichen Kontoinformationen, und bereitet den Computer darauf vor, als Image verwendet zu werden. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](https://technet.microsoft.com/library/bb457073.aspx)(in englischer Sprache).


1. Stellen Sie eine Verbindung mit dem virtuellen Computer her.
2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie zum Verzeichnis *%windir%\system32\sysprep*, und führen Sie anschließend *sysprep.exe* aus.
3. Wählen Sie unter **Systemvorbereitungsprogramm** die Option *Out-of-Box-Experience (OOBE) für System aktivieren*, und vergewissern Sie sich, dass das Kontrollkästchen *Verallgemeinern* aktiviert ist.
4. Wählen Sie unter **Optionen für Herunterfahren** die Option *Herunterfahren* aus, und klicken Sie auf **OK**.
5. Nach Abschluss von Sysprep wird die virtuelle Maschine heruntergefahren. **Starten Sie den virtuellen Computer nicht neu**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Aufheben der Zuordnung und Markieren des virtuellen Computers als generalisiert

Um ein Image zu erstellen, muss die Zuordnung des virtuellen Computers aufgehoben werden, und er muss in Azure als generalisiert markiert werden.

Heben Sie die Zuordnung des virtuellen Computers mit [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) auf.

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Legen Sie den Status des virtuellen Computers mithilfe von [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm) auf `-Generalized` fest. 
   
```azurepowershell-interactive
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>Erstellen des Images

Nun können Sie ein Image des virtuellen Computers mit [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) und [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage) erstellen. Im folgenden Beispiel wird ein Image mit dem Namen *myImage* vom virtuellen Computer *myVM* erstellt.

Rufen Sie den virtuellen Computer ab. 

```azurepowershell-interactive
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Erstellen Sie die Imagekonfiguration.

```azurepowershell-interactive
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Erstellen Sie das Image.

```azurepowershell-interactive
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Erstellen von VMs anhand des Images

Nachdem Sie ein Image erstellt haben, können Sie anhand des Images eine oder mehrere neue VMs erstellen. Das Erstellen einer VM anhand eines benutzerdefinierten Images ist vergleichbar mit dem Erstellen einer VM mithilfe eines Marketplace-Images. Wenn Sie ein Marketplace-Image verwenden, müssen Sie Informationen zum Image, zu dessen Anbieter, zum Angebot, zur SKU und Version bereitstellen. Mit dem vereinfachten Parametersatz für das Cmdlet [New-AzureRMVM]() müssen Sie nur den Namen des benutzerdefinierten Images angeben – vorausgesetzt, es befindet sich in der gleichen Ressourcengruppe. 

Dieses Beispiel erstellt auf der Grundlage von *myImage* einen virtuellen Computer namens *myVMfromImage* in *myResourceGroup*.


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```

## <a name="image-management"></a>Verwaltung von Images 

Hier sind einige Beispiele für allgemeine Aufgaben des verwalteten Images und ihre Ausführung mithilfe von PowerShell.

Auflisten aller Images nach Namen.

```azurepowershell-interactive
$images = Get-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Löschen eines Images. Dieses Beispiel löscht das Image mit dem Namen *myOldImage* aus *myResourceGroup*.

```azurepowershell-interactive
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein benutzerdefiniertes Image eines virtuellen Computers erstellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Sysprep und Generalisieren virtueller Computer
> * Erstellen eines benutzerdefinierten Images
> * Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image
> * Liste aller Images in Ihrem Abonnement
> * Löschen eines Images

Im nächsten Tutorial erhalten Sie Informationen zu hoch verfügbaren virtuellen Computern.

> [!div class="nextstepaction"]
> [Erstellen eines hoch verfügbaren virtuellen Computers](tutorial-availability-sets.md)



