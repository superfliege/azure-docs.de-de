---
title: Azure CLI-Beispiel – Kopieren einer Momentaufnahme in ein Speicherkonto in einer anderen Region | Microsoft-Dokumentation
description: Azure CLI-Beispielskript – Exportieren/Kopieren einer Momentaufnahme als VHD in ein Speicherkonto in derselben oder einer anderen Region
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc,seodec18
ms.openlocfilehash: 631e20f44ab0cd2c35df459695af0de6059e6788
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53582285"
---
# <a name="exportcopy-a-snapshot-to-a-storage-account-in-different-region-with-cli"></a>Exportieren/Kopieren einer Momentaufnahme in ein Speicherkonto in einer anderen Region mit der Befehlszeilenschnittstelle

Dieses Skript exportiert eine verwaltete Momentaufnahme in ein Speicherkonto in einer anderen Region. Zuerst generiert es den SAS-URI der Momentaufnahme, mit dem es diese anschließend in ein Speicherkonto in einer anderen Region kopiert. Verwenden Sie dieses Skript, um Sicherungen Ihrer verwalteten Datenträger für die Notfallwiederherstellung in einer anderen Region zu verwalten. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Generieren des SAS-URI für eine verwaltete Momentaufnahme und zum Kopieren der Momentaufnahme in ein Speicherkonto mithilfe dieses SAS-URI. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az snapshot grant-access](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_grant_access) | Generiert eine schreibgeschützte Shared Access Signature (SAS), die verwendet wird, um die zugrunde liegende VHD-Datei in ein Speicherkonto zu kopieren oder lokal herunterzuladen  |
| [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy#az_storage_blob_copy_start) | Kopiert ein Blob asynchron aus einem Speicherkonto in ein anderes |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen verwalteter Datenträger aus einer VHD](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Erstellen eines virtuellen Computers aus einem verwalteten Datenträger](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
