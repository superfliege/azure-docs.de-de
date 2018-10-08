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
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 8acbb33b396aa617936eb0333bd68fea60532425
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404655"
---
# <a name="create-a-vm-from-a-managed-image"></a>Erstellen eines virtuellen Computers aus einem verwalteten Image

Sie können über das Azure-Portal oder mit PowerShell mehrere virtuelle Computer aus einem verwalteten Azure-VM-Image erstellen. Ein verwaltetes VM-Image enthält die erforderlichen Informationen zum Erstellen eines virtuellen Computers, einschließlich der Datenträger für Betriebssystem und Daten. Die virtuellen Festplatten, aus denen das Image besteht, einschließlich der Betriebssystem-Datenträger und sonstiger Datenträger, werden als verwaltete Datenträger gespeichert. 

Bevor Sie einen neuen virtuellen Computer erstellen, müssen Sie ein [verwaltetes VM-Image erstellen](capture-image-resource.md), das als Quellimage verwendet wird. 

## <a name="use-the-portal"></a>Verwenden des Portals

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie im linken Menü die Option **Alle Ressourcen** aus. Sie können die Ressourcen nach **Typ** sortieren, um Ihre Images leicht zu finden.
3. Wählen Sie das gewünschte Image aus der Liste aus. Die Seite **Übersicht** für Images wird geöffnet.
4. Klicken Sie im Menü auf **VM erstellen**.
5. Geben Sie die Informationen zum virtuellen Computer ein. Der hier eingegebene Benutzername und das Kennwort werden zum Anmelden beim virtuellen Computer verwendet. Wählen Sie **OK** aus, wenn Sie fertig sind. Sie können die neue VM in einer bestehenden Ressourcengruppe erstellen oder **Neu erstellen** auswählen, um eine neue Ressourcengruppe zum Speichern der VM zu erstellen.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. 
7. Nehmen Sie unter **Einstellungen** Änderungen nach Bedarf vor, und klicken Sie auf **OK**. 
8. Auf der Seite „Zusammenfassung“ sollte Ihr Imagename unter **Privates Image** aufgelistet werden. Klicken Sie auf **OK**, um die Bereitstellung des virtuellen Computers zu starten.


## <a name="use-powershell"></a>Verwenden von PowerShell

Sie können PowerShell verwenden, um eine VM aus einem Image zu erstellen, indem Sie den vereinfachten Parametersatz für das Cmdlet [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) verwenden. Das Image muss sich in derselben Ressourcengruppe befinden, in der Sie die VM erstellen möchten.

Für dieses Beispiel ist mindestens Version 5.6.0 des AzureRM-Moduls erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

Der vereinfachte Parametersatz für [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) erfordert nur die Angabe eines Namens, einer Ressourcengruppe und eines Imagenamens, um eine VM aus einem Image zu erstellen. „New-AzureRmVm“ verwendet den Wert des Parameters **-Name** als Namen für alle Ressourcen, die das Cmdlet automatisch erstellt. In diesem Beispiel geben wir ausführlichere Namen für die einzelnen Ressourcen an, lassen sie aber automatisch vom Cmdlet erstellen. Sie können Ressourcen, z.B. das virtuelle Netzwerk, auch im Voraus erstellen und den Ressourcennamen an das Cmdlet übergeben. „New-AzureRmVm“ verwendet die vorhandenen Ressourcen, wenn sie anhand des Namens gefunden werden können.

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
[Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

