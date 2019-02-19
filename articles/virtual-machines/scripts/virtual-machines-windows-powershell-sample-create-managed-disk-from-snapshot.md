---
title: Azure PowerShell-Skriptbeispiel – Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: f2bad16a983dc8159a10c5770b60d0c070965778
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2019
ms.locfileid: "55978441"
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Erstellen verwalteter Datenträger aus einer Momentaufnahme mithilfe von PowerShell

Dieses Skript erstellt einen verwalteten Datenträger aus einer Momentaufnahme. Sie können es zum Wiederherstellen eines virtuellen Computers aus Momentaufnahmen von Betriebssystem und Datenträgern verwenden. Erstellen Sie verwaltete Datenträger für Betriebssystem und Daten aus den jeweiligen Momentaufnahmen und anschließend einen neuen virtuellen Computer durch Anfügen der verwalteten Datenträger. Sie können auch die Datenträger eines vorhandenen virtuellen Computers wiederherstellen, indem Sie die aus Momentaufnahmen erstellten Datenträger anfügen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen verwalteten Datenträger aus einer Momentaufnahme zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/Get-AzSnapshot) | Ruft Eigenschaften von Momentaufnahmen ab.  |
| [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzDiskConfig) | Erstellt die Datenträgerkonfiguration, die für die Datenträgererstellung verwendet wird. Enthält die Ressourcen-ID der übergeordneten Momentaufnahme, des Speicherorts, der mit dem Speicherort der übergeordneten Momentaufnahme übereinstimmt, sowie des Speichertyp.  |
| [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Erstellt einen Datenträger mit Datenträgerkonfiguration, Datenträgername und Name der Ressourcengruppe, die als Parameter übergeben werden. |


## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines virtuellen Computers aus einem verwalteten Datenträger](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
