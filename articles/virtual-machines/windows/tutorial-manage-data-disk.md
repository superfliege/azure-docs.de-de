---
title: Tutorial – Verwalten von Azure-Datenträgern mit Azure PowerShell | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure PowerShell zum Erstellen und Verwalten von Azure-Datenträgern für virtuelle Computer verwenden.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: ea9d89b7dd94c38b326b83ff1fbf51595d67599a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32190627"
---
# <a name="tutorial---manage-azure-disks-with-azure-powershell"></a>Tutorial: Verwalten von Azure-Datenträgern mit Azure PowerShell

Virtuelle Azure-Computer verwenden Datenträger zum Speichern des Betriebssystems, der Anwendungen und der Daten der virtuellen Computer. Beim Erstellen eines virtuellen Computers muss darauf geachtet werden, eine für den erwarteten Workload geeignete Datenträgergröße und -konfiguration auszuwählen. Dieses Tutorial behandelt die Bereitstellung und Verwaltung der Datenträger von virtuellen Computern. Sie erhalten Informationen zu folgenden Themen:

> [!div class="checklist"]
> * Betriebssystemdatenträger und temporäre Datenträger
> * Datenträger
> * Standard- und Premium-Datenträger
> * Datenträgerleistung
> * Anfügen und Vorbereiten von Datenträgern für Daten

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="default-azure-disks"></a>Azure-Standarddatenträger

Beim Erstellen eines virtuellen Azure-Computers werden zwei Datenträger automatisch an den virtuellen Computer angefügt. 

**Betriebssystem-Datenträger**: Betriebssystem-Datenträger können in der Größe auf bis zu 4 TB angepasst werden und hosten das Betriebssystem des virtuellen Computers.  Dem Betriebssystem-Datenträger wird standardmäßig der Laufwerkbuchstabe *c:* zugewiesen. Die Konfiguration der Datenträgerzwischenspeicherung des Betriebssystem-Datenträgers ist für die Leistung des Betriebssystems optimiert. Der Betriebssystem-Datenträger **sollte keine** Anwendungen oder Daten hosten. Verwenden Sie für Anwendungen und Daten einen Datenträger. Dies wird weiter unten in diesem Artikel ausführlich erläutert.

**Temporärer Datenträger:** Temporäre Datenträger verwenden ein Solid State Drive, das sich auf demselben Azure-Host wie der virtuelle Computer befindet. Temporäre Datenträger sind äußerst leistungsfähig und können für Vorgänge wie die temporäre Datenverarbeitung verwendet werden. Wenn der virtuelle Computer jedoch auf einen neuen Host verschoben wird, werden alle auf einem temporären Datenträger gespeicherten Daten entfernt. Die Größe des temporären Datenträgers richtet sich nach der Größe des virtuellen Computers. Temporären Datenträgern wird standardmäßig der Laufwerkbuchstabe *d:* zugewiesen.

### <a name="temporary-disk-sizes"></a>Größe von temporären Datenträgern

| Typ | Gängige Größen | Max. Größe des temporären Datenträgers (GiB) |
|----|----|----|
| [Allgemeiner Zweck](sizes-general.md) | A-, B- und D-Serie | 1600 |
| [Computeoptimiert](sizes-compute.md) | F-Serie | 576 |
| [Arbeitsspeicheroptimiert](sizes-memory.md) | D-, E-, G- und M-Serie | 6.144 |
| [Speicheroptimiert](sizes-storage.md) | L-Serie | 5.630 |
| [GPU](sizes-gpu.md) | N-Serie | 1.440 |
| [Hohe Leistung](sizes-hpc.md) | A- und H-Serie | 2000 |

## <a name="azure-data-disks"></a>Azure-Datenträger

Zum Installieren von Anwendungen und zum Speichern von Daten können weitere Datenträger hinzugefügt werden. Datenträger sollten in allen Fällen verwendet werden, in denen eine dauerhafte und dynamische Datenspeicherung erwünscht ist. Jeder Datenträger hat eine maximale Kapazität von 4 TB. Die Größe eines virtuellen Computers bestimmt die Anzahl der Datenträger, die an den virtuellen Computer angefügt werden können. Für jede vCPU eines virtuellen Computers können zwei Datenträger angefügt werden. 

### <a name="max-data-disks-per-vm"></a>Max. Anzahl der Datenträger pro virtuellem Computer

| Typ | Gängige Größen | Max. Anzahl der Datenträger pro virtuellem Computer |
|----|----|----|
| [Allgemeiner Zweck](sizes-general.md) | A-, B- und D-Serie | 64 |
| [Computeoptimiert](sizes-compute.md) | F-Serie | 64 |
| [Arbeitsspeicheroptimiert](sizes-memory.md) | D-, E-, G- und M-Serie | 64 |
| [Speicheroptimiert](sizes-storage.md) | L-Serie | 64 |
| [GPU](sizes-gpu.md) | N-Serie | 64 |
| [Hohe Leistung](sizes-hpc.md) | A- und H-Serie | 64 |

## <a name="vm-disk-types"></a>VM-Datenträgertypen

In Azure stehen zwei verschiedene Datenträgertypen zur Verfügung.

### <a name="standard-disk"></a>Standarddatenträger

Standardspeicher basiert auf Festplatten und stellt eine kostengünstige, performante Speicherlösung dar. Standarddatenträger sind ideal für eine kostengünstige Entwicklungs- und Testworkload.

### <a name="premium-disk"></a>Premium-Datenträger

Premium-Datenträger zeichnen sich durch SSD-basierte hohe Leistung und geringe Wartezeit aus. Sie eignen sich hervorragend für virtuelle Computer, auf denen die Produktionsworkload ausgeführt wird. Storage Premium unterstützt virtuelle Computer der DS-, DSv2-, GS- und FS-Serie. Premium-Datenträger gibt es in fünf Varianten (P10, P20, P30, P40, P50). Der Datenträgertyp wird durch die Größe des Datenträgers vorgegeben. Bei der Auswahl einer Datenträgergröße wird der Wert auf den nächsten Datenträgertyp aufgerundet. Wenn die Größe beispielsweise unterhalb von 128 GB liegt, lautet der Datenträgertyp P10, und bei einer Größe zwischen 129 GB und 512 GB lautet der Datenträgertyp P20.

### <a name="premium-disk-performance"></a>Leistung von Premium-Datenträgern

|Storage Premium-Datenträgertyp | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Datenträgergröße (aufgerundet) | 32 GB | 64 GB | 128 GB | 512 GB | 1.024GB (1TB) | 2.048 GB (2 TB) | 4.095 GB (4 TB) |
| Max. IOPS pro Datenträger | 120 | 240 | 500 | 2.300 | 5.000 | 7.500 | 7.500 |
Durchsatz pro Datenträger | 25 MB/s | 50 MB/s | 100 MB/s | 150 MB/s | 200 MB/s | 250 MB/s | 250 MB/s |

In dieser Tabelle ist zwar die maximale IOPS-Anzahl pro Datenträger angegeben, eine höhere Leistung kann aber durch Striping mehrerer Datenträger erreicht werden. An einen virtuellen Standard_GS5-Computer können beispielsweise 64 Datenträger angefügt werden. Wenn jeder dieser Datenträger die Größe des Typs P30 aufweisen, kann eine maximale Größe von 80.000 IOPS erreicht werden. Ausführliche Informationen zur maximalen IOPS-Anzahl pro VM finden Sie unter [Größen für virtuelle Windows-Computer in Azure](./sizes.md).

## <a name="create-and-attach-disks"></a>Erstellen und Anfügen von Datenträgern

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Erstellen Sie, falls erforderlich, mit den folgenden Befehlen einen virtuellen Computer.

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) den virtuellen Computer.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupDisk" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred `
    -AsJob
```

Mit dem Parameter `-AsJob` wird die VM als Hintergrundaufgabe erstellt, sodass die PowerShell-Aufforderungen für Sie zurückgegeben werden. Sie können die Details von Hintergrundaufträgen mit dem `Job`-Cmdlet anzeigen.

Erstellen Sie mit [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) die anfängliche Konfiguration. Im folgenden Beispiel wird ein Datenträger mit einer Größe von 128 GB erstellt.

```azurepowershell-interactive
$diskConfig = New-AzureRmDiskConfig `
    -Location "EastUS" `
    -CreateOption Empty `
    -DiskSizeGB 128
```

Erstellen Sie den Datenträger mit dem Befehl [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk).

```azurepowershell-interactive
$dataDisk = New-AzureRmDisk `
    -ResourceGroupName "myResourceGroupDisk" `
    -DiskName "myDataDisk" `
    -Disk $diskConfig
```

Rufen Sie mit dem Befehl [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) den virtuellen Computer ab, den Sie dem Datenträger hinzufügen möchten.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -Name "myVM"
```

Fügen Sie mit dem Befehl [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) den Datenträger der Konfiguration des virtuellen Computers hinzu.

```azurepowershell-interactive
$vm = Add-AzureRmVMDataDisk `
    -VM $vm `
    -Name "myDataDisk" `
    -CreateOption Attach `
    -ManagedDiskId $dataDisk.Id `
    -Lun 1
```

Aktualisieren Sie den virtuellen Computer mit dem Befehl [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk).

```azurepowershell-interactive
Update-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -VM $vm
```

## <a name="prepare-data-disks"></a>Vorbereiten von Datenträgern

Nach dem Anfügen eines Datenträgers an den virtuellen Computer muss das Betriebssystem zur Verwendung des Datenträgers konfiguriert werden. Im folgenden Beispiel wird gezeigt, wie der erste dem virtuellen Computer hinzugefügte Datenträger manuell konfiguriert wird. Dieser Vorgang kann auch mit der [benutzerdefinierten Skripterweiterung](./tutorial-automate-vm-deployment.md) automatisiert werden.

### <a name="manual-configuration"></a>Manuelle Konfiguration

Erstellen Sie eine RDP-Verbindung mit dem virtuellen Computer. Öffnen Sie PowerShell, und führen Sie das folgende Skript aus.

```azurepowershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu VM-Datenträgern erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Betriebssystemdatenträger und temporäre Datenträger
> * Datenträger
> * Standard- und Premium-Datenträger
> * Datenträgerleistung
> * Anfügen und Vorbereiten von Datenträgern für Daten

Im nächsten Tutorial erfahren Sie, wie die VM-Konfiguration automatisiert werden kann.

> [!div class="nextstepaction"]
> [Automatisieren der VM-Konfiguration](./tutorial-automate-vm-deployment.md)
