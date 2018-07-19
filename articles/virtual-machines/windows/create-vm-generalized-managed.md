---
title: Erstellen einer VM auf der Grundlage eines verwalteten Images in Azure | Microsoft-Dokumentation
description: Erstellen Sie einen virtuellen Windows-Computer auf der Grundlage eines generalisierten verwalteten Images mit Azure PowerShell oder dem Azure-Portal im Resource Manager-Bereitstellungsmodell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: cynthn
ms.openlocfilehash: 6baf784068b1fba0c35d2848b8d2dda4f1064a2d
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867979"
---
# <a name="create-a-vm-from-a-managed-image"></a>Erstellen eines virtuellen Computers aus einem verwalteten Image

Sie können mehrere virtuelle Computer mit PowerShell oder dem Azure-Portal aus einem verwalteten VM-Image erstellen. Ein verwaltetes VM-Image enthält die erforderlichen Informationen zum Erstellen eines virtuellen Computers, einschließlich der Datenträger für Betriebssystem und Daten. Die VHDs, aus denen das Image besteht, einschließlich der Betriebssystem-Datenträger und sonstiger Datenträger, werden als verwaltete Datenträger gespeichert. 

Sie müssen bereits [ein verwaltetes VM-Image erstellt](capture-image-resource.md) haben, das zum Erstellen des neuen virtuellen Computers verwendet werden soll. 

## <a name="use-the-portal"></a>Verwenden des Portals

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie im linken Menü die Option **Alle Ressourcen** aus. Sie können die Ressourcen nach **Typ** sortieren, um Ihre Images leicht zu finden.
3. Wählen Sie das gewünschte Image aus der Liste aus. Die Seite **Übersicht** für Images wird geöffnet.
4. Klicken Sie im Menü auf **+ VM erstellen**.
5. Geben Sie die Informationen zum virtuellen Computer ein. Der hier eingegebene Benutzername und das Kennwort werden zum Anmelden am virtuellen Computer verwendet. Klicken Sie zum Abschluss auf **OK**. Sie können die neue VM in einer bestehenden Ressourcengruppe erstellen oder **Neu erstellen** auswählen, um eine neue Ressourcengruppe zum Speichern der VM zu erstellen.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. 
7. Nehmen Sie unter **Einstellungen** Änderungen nach Bedarf vor, und klicken Sie auf **OK**. 
8. Auf der Seite „Zusammenfassung“ sollte Ihr Imagename für **Privates Image** aufgelistet werden. Klicken Sie auf **Ok**, um die Bereitstellung des virtuellen Computers zu starten.


## <a name="use-powershell"></a>Verwenden von PowerShell

Sie können PowerShell verwenden, um eine VM aus einem Image zu erstellen, indem Sie den vereinfachten Parametersatz für das [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm)-Cmdlet verwenden. Das Image muss sich in derselben Ressourcengruppe befinden, in der Sie die VM erstellen möchten.

Für dieses Beispiel ist mindestens Version 5.6.0 des AzureRM-Moduls erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

Für den vereinfachten Parametersatz für New-AzureRmVm müssen Sie lediglich einen Namen, eine Ressourcengruppe und einen Imagenamen angeben, um eine VM aus einem Image zu erstellen. Es wird aber der Wert des Parameters **-Name** als Name für alle Ressourcen verwendet, die automatisch erstellt werden. In diesem Beispiel geben wir für die einzelnen Ressourcen ausführlichere Namen an, aber sie werden automatisch vom Cmdlet erstellt. Sie können Ressourcen, z.B. das virtuelle Netzwerk, auch im Voraus erstellen und den Namen an das Cmdlet übergeben. Im Cmdlet werden die vorhandenen Ressourcen verwendet, wenn sie anhand des Namens gefunden werden können.

Im folgenden Beispiel wird eine VM mit dem Namen *myVMFromImage* in der Ressourcengruppe *myResourceGroup* aus dem Image *myImage* erstellt. 


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



## <a name="next-steps"></a>Nächste Schritte
Informationen zum Verwalten Ihres neuen virtuellen Computers mit Azure PowerShell finden Sie unter [Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

