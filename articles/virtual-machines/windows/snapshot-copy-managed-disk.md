---
title: Erstellen einer Momentaufnahme einer VHD in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie eine Kopie eines virtuellen Azure-Computers als Sicherung oder für die Behandlung von Problemen erstellen.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: cynthn
ms.openlocfilehash: 7d45fd749fea4036d944d740541d8b8607553835
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658154"
---
# <a name="create-a-snapshot"></a>Erstellen einer Momentaufnahme

Erstellen Sie eine Momentaufnahme eines Betriebssystem oder eines VHD-Datenträgers für die Sicherung oder zum Behandeln von VM-Problemen. Eine Momentaufnahme ist eine vollständige, schreibgeschützte Kopie einer VHD. 

## <a name="use-azure-portal-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme im Azure-Portal 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie zunächst links oben auf **Ressource erstellen**, und suchen Sie nach **Momentaufnahme**.
3. Klicken Sie auf dem Blatt „Momentaufnahme“ auf **Erstellen**.
4. Geben Sie einen **Namen** für die Momentaufnahme ein.
5. Wählen Sie eine vorhandene [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#resource-groups) aus, oder geben Sie den Namen für eine neue Ressourcengruppe ein. 
6. Wählen Sie den Standort eines Azure-Rechenzentrums aus.  
7. Wählen Sie für **Quelldatenträger** den verwalteten Datenträger aus, für den eine Momentaufnahme erstellt werden soll.
8. Wählen Sie den **Kontotyp** aus, der zum Speichern der Momentaufnahme verwendet werden soll. Wir empfehlen **Standard_LRS**, es sei denn, Sie benötigen eine Speicherung auf einem Hochleistungsdatenträger.
9. Klicken Sie auf **Create**.

## <a name="use-powershell-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme mit PowerShell

Die folgenden Schritte veranschaulichen das Abrufen des zu kopierenden VHD-Datenträgers, das Erstellen der Momentaufnahmenkonfigurationen und das Erstellen einer Momentaufnahme des Datenträgers mithilfe des Cmdlets [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot). 

Stellen Sie vor Beginn sicher, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Für diesen Artikel ist mindestens Version 5.7.0 des AzureRM-Moduls erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

Legen Sie einige Parameter fest. 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  
```

Rufen Sie die VM ab.

 ```azurepowershell-interactive
$vm = get-azurermvm `
   -ResourceGroupName $resourceGroupName `
   -Name $vmName
```

Erstellen Sie die Momentaufnahmekonfiguration. In diesem Beispiel wird eine Momentaufnahme des Betriebssystemdatenträgers erstellt.

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig `
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
   -Location $location `
   -CreateOption copy
```
   
> [!NOTE]
> Wenn Sie die Momentaufnahme in Speicher mit Zonenresilienz speichern möchten, müssen Sie sie in einer Region erstellen, die [Verfügbarkeitszonen](../../availability-zones/az-overview.md) unterstützt, und den `-SkuName Standard_ZRS`-Parameter einbeziehen.   

   
Erstellen Sie die Momentaufnahme.

```azurepowershell-interactive
New-AzureRmSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```




## <a name="next-steps"></a>Nächste Schritte

Erstellt einen virtuellen Computer aus einer Momentaufnahme, indem aus einer Momentaufnahme zuerst ein verwalteter Datenträger erstellt und dieser neue verwaltete Datenträger anschließend als Betriebssystemdatenträger angefügt wird. Weitere Informationen finden Sie im Beispiel [Erstellen eines virtuellen Computers aus einer Momentaufnahme](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json).
