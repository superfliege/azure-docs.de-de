---
title: Konvertieren einer Windows-VM von nicht verwalteten Datenträgern in verwaltete Datenträger – Azure Managed Disks | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie eine Windows-VM von nicht verwalteten Datenträgern mithilfe von PowerShell im Resource Manager-Bereitstellungsmodell in verwaltete Datenträger konvertieren.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.openlocfilehash: 26ddc1df8353a8a8b0e0c54a0c906c8d8b6b45b9
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495340"
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Konvertieren einer Windows-VM von nicht verwalteten Datenträgern in verwaltete Datenträger

Wenn Sie über vorhandene virtuelle Windows-Computer (VMs) verfügen, die nicht verwaltete Datenträger verwenden, können Sie die VMs über den [Azure Managed Disks-Dienst](managed-disks-overview.md) konvertieren, sodass verwaltete Datenträger verwendet werden. Bei diesem Prozess werden sowohl der Betriebssystemdatenträger als auch alle anderen angefügten Datenträger konvertiert.

In diesem Artikel wird beschrieben, wie Sie VMs mithilfe von Azure PowerShell konvertieren. Wenn Sie PowerShell installieren oder aktualisieren ausführen müssen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="before-you-begin"></a>Voraussetzungen


* Lesen Sie den Abschnitt [Planen der Migration zu Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

* Lesen Sie die [häufig gestellten Fragen zu Managed Disks](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Konvertieren von Einzelinstanz-VMs
In diesem Abschnitt wird beschrieben, wie Sie für Einzelinstanz-VMs von Azure die Konvertierung von nicht verwalteten Datenträgern in verwaltete Datenträger durchführen. (Wenn Ihre VMs in einer Verfügbarkeitsgruppe enthalten sind, lesen Sie den nächsten Abschnitt.) 

1. Heben Sie die Zuordnung der VM mit dem Cmdlet [Stop AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) auf. Im folgenden Beispiel wird die Zuordnung für die VM `myVM` in der Ressourcengruppe `myResourceGroup` aufgehoben: 

  ```azurepowershell-interactive
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Konvertieren Sie die VM mit dem Cmdlet [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) in verwaltete Datenträger. Mit dem folgenden Prozess wird der vorherige virtuelle Computer einschließlich des Betriebssystemdatenträgers und der anderen Datenträger konvertiert, und der virtuelle Computer wird gestartet:

  ```azurepowershell-interactive
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```



## <a name="convert-vms-in-an-availability-set"></a>Konvertieren von virtuellen Computern in einer Verfügbarkeitsgruppe

Falls sich die VMs, die Sie in verwaltete Datenträger konvertieren möchten, in einer Verfügbarkeitsgruppe befinden, müssen Sie zuerst für die Verfügbarkeitsgruppe die Konvertierung in eine verwaltete Verfügbarkeitsgruppe durchführen.

1. Konvertieren Sie die Verfügbarkeitsgruppe mit dem Cmdlet [Update AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset). Im folgenden Beispiel wird die Verfügbarkeitsgruppe `myAvailabilitySet` aus der Ressourcengruppe `myResourceGroup` aktualisiert:

  ```azurepowershell-interactive
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Wenn die Region, in der sich die Verfügbarkeitsgruppe befindet, nur 2 verwaltete Fehlerdomänen umfasst, aber die Anzahl der nicht verwalteten Fehlerdomänen 3 beträgt, wird bei diesem Befehl eine ähnliche Fehlermeldung wie die Folgende angezeigt: „Die angegebene Fehlerdomänenanzahl 3 muss im Bereich 1 bis 2 liegen“. Um den Fehler zu beheben, ändern Sie die Fehlerdomäne in 2, und `Sku` in `Aligned` wie im Folgenden gezeigt:

  ```azurepowershell-interactive
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Heben Sie die Zuordnung der VMs in der Verfügbarkeitsgruppe auf, und konvertieren Sie sie. Das folgende Skript hebt die Zuordnung aller virtuellen Computer mit dem Cmdlet [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) auf, konvertiert sie mit [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) und startet sie als Teil des Konvertierungsprozesses automatisch neu:

  ```azurepowershell-interactive
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
  }
  ```


## <a name="troubleshooting"></a>Problembehandlung

Wenn während der Konvertierung ein Fehler auftritt oder sich eine VM aufgrund von Problemen bei einer vorherigen Konvertierung in einen fehlerhaften Zustand befindet, führen Sie das Cmdlet `ConvertTo-AzureRmVMManagedDisk` erneut aus. Derartige Probleme können normalerweise durch eine einfache Wiederholung behoben werden.
Stellen Sie vor dem Konvertieren sicher, dass alle VM-Erweiterungen den Zustand „Bereitstellung erfolgreich“ aufweisen. Andernfalls tritt bei der Konvertierung ein Fehler mit Fehlercode 409 auf.


## <a name="convert-using-the-azure-portal"></a>Konvertieren über das Azure-Portal

Sie haben auch die Möglichkeit, nicht verwaltete Datenträger über das Azure-Portal in verwaltete Datenträger zu konvertieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie die VM in der Liste der VMs im Portal aus.
3. Wählen Sie auf dem Blatt für die VM aus dem Menü die Option **Datenträger** aus.
4. Wählen Sie oben auf dem Blatt **Datenträger** die Option **Zu Managed Disks migrieren** aus.
5. Wenn sich Ihre VM in einer Verfügbarkeitsgruppe befindet, wird auf dem Blatt **Zu Managed Disks migrieren** eine Warnung angezeigt, die besagt, dass Sie die Verfügbarkeitsgruppe zuerst konvertieren müssen. Die Warnung sollte einen Link enthalten, über den Sie die Verfügbarkeitsgruppe konvertieren können. Sobald die Verfügbarkeitsgruppe konvertiert wurde, oder wenn sich Ihre VM nicht in einer Verfügbarkeitsgruppe befindet, klicken Sie auf **Migrieren**, um den Prozess der Migration Ihrer Datenträger zu Managed Disks zu starten. 

Nach Abschluss der Migration wird die VM angehalten und neu gestartet.

## <a name="next-steps"></a>Nächste Schritte

[Konvertieren von verwalteten Standarddatenträgern in Premium-Datenträger](convert-disk-storage.md)

Erstellen Sie eine schreibgeschützte Kopie eines virtuellen Computers mit [Momentaufnahmen](snapshot-copy-managed-disk.md).

