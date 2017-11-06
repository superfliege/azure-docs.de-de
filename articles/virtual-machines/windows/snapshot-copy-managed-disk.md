---
title: Erstellen einer Momentaufnahme einer VHD in Azure | Microsoft-Dokumentation
description: "Erfahren Sie, wie eine Kopie eines virtuellen Azure-Computers als Sicherung oder für die Behandlung von Problemen erstellen."
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: dba70db512d88dfc57107bade0df50d1834eb883
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="create-a-snapshot"></a>Erstellen einer Momentaufnahme

Erstellen Sie eine Momentaufnahme eines Betriebssystem oder eines VHD-Datenträgers für die Sicherung oder zum Behandeln von VM-Problemen. Eine Momentaufnahme ist eine vollständige, schreibgeschützte Kopie einer VHD. 

## <a name="use-azure-portal-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme im Azure-Portal 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie zunächst links oben auf **Neu**, und suchen Sie nach **Momentaufnahme**.
3. Klicken Sie auf dem Blatt „Momentaufnahme“ auf **Erstellen**.
4. Geben Sie einen **Namen** für die Momentaufnahme ein.
5. Wählen Sie eine vorhandene [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#resource-groups) aus, oder geben Sie den Namen für eine neue Ressourcengruppe ein. 
6. Wählen Sie den Standort eines Azure-Rechenzentrums aus.  
7. Wählen Sie für **Quelldatenträger** den verwalteten Datenträger aus, für den eine Momentaufnahme erstellt werden soll.
8. Wählen Sie den **Kontotyp** aus, der zum Speichern der Momentaufnahme verwendet werden soll. Wir empfehlen **Standard_LRS**, es sei denn, Sie benötigen eine Speicherung auf einem Hochleistungsdatenträger.
9. Klicken Sie auf **Erstellen**.

## <a name="use-powershell-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme mit PowerShell
Die folgenden Schritte veranschaulichen das Abrufen des zu kopierenden VHD-Datenträgers, das Erstellen der Momentaufnahmenkonfigurationen und das Erstellen einer Momentaufnahme des Datenträgers mithilfe des Cmdlets [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot). 

Stellen Sie sicher, dass Sie die neueste Version des installierten AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](/powershell/azure/overview).


1. Legen Sie einige Parameter fest. 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$dataDiskName = 'myDisk' 
$snapshotName = 'mySnapshot'  
```

2. Rufen Sie den VHD-Datenträger ab, der kopiert werden soll.

 ```azurepowershell-interactive
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. Erstellen Sie die Momentaufnahmenkonfigurationen. 

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. Erstellen Sie die Momentaufnahme.

 ```azurepowershell-interactive
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Wenn Sie vorhaben, die Momentaufnahme zum Erstellen eines verwalteten Datenträgers zu nutzen und sie an eine VM anzufügen, die hohe Leistung benötigt, verwenden Sie mit dem Cmdlet „New-AzureRmSnapshot“ den Parameter `-AccountType Premium_LRS`. Der Parameter erstellt die Momentaufnahme so, dass sie als verwalteter Premium-Datenträger gespeichert wird. Verwaltete Premium-Datenträger sind teurer als die Standardvariante. Vergewissern Sie sich daher, dass Sie Premium benötigen, ehe Sie diesen Parameter angeben.

## <a name="next-steps"></a>Nächste Schritte

Erstellt einen virtuellen Computer aus einer Momentaufnahme, indem aus einer Momentaufnahme zuerst ein verwalteter Datenträger erstellt und dieser neue verwaltete Datenträger anschließend als Betriebssystemdatenträger angefügt wird. Weitere Informationen finden Sie im Beispiel [Erstellen eines virtuellen Computers aus einer Momentaufnahme](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json).
