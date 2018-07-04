---
title: Includedatei
description: Includedatei
services: virtual-machines
author: sdwheeler
ms.service: virtual-machines
ms.topic: include
ms.date: 04/18/2018
ms.author: kirpas;iainfou;sewhee
ms.custom: include file
ms.openlocfilehash: ab4d5326fc06cc8676ea93ed529315cc47651e26
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36943452"
---
## <a name="overview"></a>Übersicht
Beim Erstellen eines neuen virtuellen Computers in einer Ressourcengruppe durch Bereitstellen eines Images von [Azure Marketplace](https://azure.microsoft.com/marketplace/) hat das Standard-Betriebssystemlaufwerk häufig eine Größe von 127 GB (bei manchen Images sind die Standard-Betriebssystemlaufwerke kleiner). Obwohl es möglich ist, der VM Datenträger hinzuzufügen (die Anzahl hängt von der SKU ab, die Sie ausgewählt haben), und darüber hinaus empfohlen wird, Anwendungen und CPU-intensive Workloads auf diesen zusätzlichen Datenträgern zu installieren, müssen Kunden oft das Betriebssystemlaufwerk erweitern, um bestimmte Szenarien wie z. B. die folgenden zu unterstützen:

1. Unterstützung von Legacyanwendungen, die Komponenten auf dem Betriebssystemlaufwerk installieren.
2. Migrieren eines lokalen physischen PCs oder virtuellen Computers mit einem größeren Betriebssystemlaufwerk.

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: Ressourcen-Manager und klassische Bereitstellungen. Dieser Artikel behandelt die Verwendung des Ressourcen-Manager-Modells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.
> 
> 
> [!WARNING]
> Das Ändern der Größe des Betriebssystemdatenträgers eines virtuellen Azure-Computers führt dazu, dass dieser neu gestartet wird.
>

## <a name="resize-the-os-drive"></a>Ändern der Größe des Betriebssystemlaufwerks
In diesem Artikel wird beschrieben, wie die Größe des Betriebssystemlaufwerks mit den Ressourcen-Manager-Modulen von [Azure Powershell](/powershell/azureps-cmdlets-docs)geändert wird. Wir demonstrieren die Änderung der Größe des Betriebssystemdatenträgers sowohl für nicht verwaltete als auch für verwaltete Datenträger, da sich der Ansatz zum Ändern der Datenträgergröße bei beiden Datenträgerarten unterscheidet.

### <a name="for-resizing-unmanaged-disks"></a>Ändern der Größe nicht verwalteter Datenträger:

Öffnen Sie Powershell ISE oder Powershell-Fenster im Administratormodus, und führen Sie die folgenden Schritte aus:

1. Melden Sie sich im Ressourcenverwaltungsmodus bei Ihrem Microsoft Azure-Konto an, und wählen Sie Ihr Abonnement wie folgt aus:
   
   ```Powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Legen Sie den Namen Ihrer Ressourcengruppe und VM wie folgt fest:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Rufen Sie einen Verweis auf Ihre VM wie folgt ab:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Halten Sie die VM vor dem Ändern der Größe des Datenträgers wie folgt an:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Und hier ist der Moment, auf den wir gewartet haben! Legen Sie die Größe des nicht verwalteten Betriebssystemdatenträgers auf den gewünschten Wert fest, und aktualisieren Sie die VM wie folgt:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > Die neue Größe sollte die Größe des vorhandenen Datenträgers überschreiten. Der zulässige Höchstwert für Betriebssystemdatenträger beträgt 2048 GB. (Sie können auch einen größeren VHD-Blob einsetzen, doch das Betriebssystem kann nur mit den ersten 2048 GB arbeiten.)
   > 
   > 
6. Das Aktualisieren der VM kann einige Sekunden dauern. Starten Sie die VM nach Abschluss der Befehlsausführung wie folgt neu:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

### <a name="for-resizing-managed-disks"></a>Ändern der Größe verwalteter Datenträger:

Öffnen Sie Powershell ISE oder Powershell-Fenster im Administratormodus, und führen Sie die folgenden Schritte aus:

1. Melden Sie sich im Ressourcenverwaltungsmodus bei Ihrem Microsoft Azure-Konto an, und wählen Sie Ihr Abonnement wie folgt aus:
   
   ```Powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Legen Sie den Namen Ihrer Ressourcengruppe und VM wie folgt fest:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Rufen Sie einen Verweis auf Ihre VM wie folgt ab:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Halten Sie die VM vor dem Ändern der Größe des Datenträgers wie folgt an:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Rufen Sie einen Verweis auf den verwalteten Betriebssystemdatenträger ab. Legen Sie die Größe des verwalteten Betriebssystemdatenträgers auf den gewünschten Wert fest, und aktualisieren Sie den Datenträger wie folgt:
   
   ```Powershell
   $disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
   $disk.DiskSizeGB = 1023
   Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
   ```   
   > [!WARNING]
   > Die neue Größe sollte die Größe des vorhandenen Datenträgers überschreiten. Der zulässige Höchstwert für Betriebssystemdatenträger beträgt 2048 GB. (Sie können auch einen größeren VHD-Blob einsetzen, doch das Betriebssystem kann nur mit den ersten 2048 GB arbeiten.)
   > 
   > 
6. Das Aktualisieren der VM kann einige Sekunden dauern. Starten Sie die VM nach Abschluss der Befehlsausführung wie folgt neu:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Das ist alles! Stellen Sie jetzt eine RDP-Verbindung mit der VM her, öffnen Sie die Computerverwaltung (oder die Datenträgerverwaltung), und erweitern Sie das Laufwerk mit dem neu zugewiesenen Speicherplatz.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel haben wir Azure Resource Manager-Module von Powershell verwendet, um das Betriebssystemlaufwerk eines virtuellen IaaS-Computers zu erweitern. Unten finden Sie als Referenz das vollständige Skript sowohl für verwaltete als auch für nicht verwaltete Datenträger:

Nicht verwaltete Datenträger:

```Powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
Verwaltete Datenträger:

```Powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRMVM -ResourceGroupName $rgName -Name $vmName
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
$disk.DiskSizeGB = 1023
Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="for-resizing-data-disks"></a>Zum Ändern der Größe von Datenträgern
Obwohl wir uns in diesem Artikel hauptsächlich auf die Erweiterung des verwalteten/nicht verwalteten Betriebssystem-Datenträgers der VM konzentriert haben, kann das entwickelte Skript auch zum Erweitern der Datenträger verwendet werden, die an die VM angefügt sind. Um beispielsweise den ersten Datenträger zu erweitern, der an den virtuellen Computer angefügt ist, ersetzen Sie das ```OSDisk```-Objekt von ```StorageProfile``` mit dem ```DataDisks```-Array, und verwenden Sie einen numerischen Index, um – wie unten dargestellt – einen Verweis auf den ersten angefügten Datenträger zu erhalten:

Nicht verwalteter Datenträger:
```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
Verwalteter Datenträger:
```Powershell
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.DataDisks[0].Name
$disk.DiskSizeGB = 1023
```

Entsprechend können Sie andere Datenträger referenzieren, die der VM angefügt sind, indem Sie entweder – wie oben gezeigt – einen Index, oder – wie unten dargestellt – die ```Name``` -Eigenschaft des Datenträgers verwenden:

Nicht verwalteter Datenträger:
```Powershell
($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'}).DiskSizeGB = 1023
```
Verwalteter Datenträger:
```Powershell
(Get-AzureRmDisk -ResourceGroupName $rgName -DiskName ($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).Name).DiskSizeGB = 1023
```

Wenn Sie herausfinden möchten, wie Datenträger einem virtuellen Azure Resource Manager-Computer angefügt werden, lesen Sie [diesen Artikel](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

