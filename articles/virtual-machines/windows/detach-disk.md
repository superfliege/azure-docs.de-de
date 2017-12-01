---
title: "Trennen eines Datenträgers mit Daten von einem virtuellen Windows-Computer – Azure | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie einen Datenträger unter Verwendung des Resource Manager-Bereitstellungsmodells von einem virtuellen Computer in Azure trennen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: 7013e7ff3cb14dcad8e3e9a926bcee771180259d
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/18/2017
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Trennen eines Datenträgers von einem virtuellen Windows-Computer
Wenn Sie einen Datenträger, der an einen virtuellen Computer angefügt ist, nicht mehr benötigen, können Sie ihn leicht trennen. Dadurch wird der Datenträger von dem virtuellen Computer entfernt, aber nicht aus dem Speicher.

> [!WARNING]
> Beim Trennen eines Datenträgers wird dieser nicht automatisch gelöscht. Wenn Sie Storage Premium abonniert haben, fallen auch weiterhin Speichergebühren für den Datenträger an. Weitere Informationen finden Sie unter [Preisgestaltung und Abrechnung bei der Verwendung von Storage Premium](premium-storage.md#pricing-and-billing).
>
>

Wenn Sie die vorhandenen Daten erneut auf dem Datenträger verwenden möchten, können Sie ihn erneut an denselben virtuellen Computer oder an einen anderen anfügen.

## <a name="detach-a-data-disk-using-the-portal"></a>Trennen eines Datenträgers im Portal

1. Wählen Sie im Menü auf der linken Seite die Option **Virtuelle Computer** aus.
2. Wählen Sie den virtuellen Computer mit dem zu trennenden Datenträger aus, und klicken Sie auf **Beenden**, um die Zuordnung der VM aufzuheben.
3. Klicken Sie im Bereich für den virtuellen Computer auf **Datenträger**.
4. Wählen Sie oben im Bereich **Datenträger** die Option **Bearbeiten** aus.
5. Klicken Sie im Bereich **Datenträger** ganz rechts neben dem Datenträger, den Sie trennen möchten, auf die Schaltfläche „Trennen“ (![Detach button image](./media/detach-disk/detach.png)).
5. Nachdem der Datenträger entfernt wurde, klicken Sie oben auf dem Bereich auf **Speichern**.
6. Klicken Sie im Bereich für den virtuellen Computer auf **Übersicht** und dann oben im Bereich auf die Schaltfläche **Starten**, um die VM neu zu starten.



Der Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt.

## <a name="detach-a-data-disk-using-powershell"></a>Trennen eines Datenträgers mit PowerShell
In diesem Beispiel ruft der erste Befehl mit dem Cmdlet [Get-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) den virtuellen Computer mit dem Namen **MyVM07** aus der Ressourcengruppe **RG11** ab und speichert ihn in der Variablen **$VirtualMachine**.

In der zweiten Zeile wird der Datenträger namens DataDisk3 über das Cmdlet [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk) vom virtuellen Computer entfernt.

In der dritten Zeile wird der Status des virtuellen Computers über das Cmdlet [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) aktualisiert, und der Vorgang zum Entfernen des Datenträgers ist abgeschlossen.

```azurepowershell-interactive
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -VM $VirtualMachine
```

Weitere Informationen finden Sie unter [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie den Datenträger wiederverwenden möchten, können Sie ihn einfach [an einen anderen virtuellen Computer anfügen](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

