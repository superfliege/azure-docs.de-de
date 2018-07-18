---
title: Azure CLI-Beispielskript – Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme | Microsoft-Dokumentation
description: Azure CLI-Beispielskript – Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme
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
ms.custom: mvc
ms.openlocfilehash: 3a6c741933d48c22ec38d98fee9591d45877ad84
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
ms.locfileid: "29849544"
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>Erstellen verwalteter Datenträger aus einer Momentaufnahme mithilfe der Befehlszeilenschnittstelle

Dieses Skript erstellt einen verwalteten Datenträger aus einer Momentaufnahme. Sie können es zum Wiederherstellen eines virtuellen Computers aus Momentaufnahmen von Betriebssystem und Datenträgern verwenden. Erstellen Sie verwaltete Datenträger für Betriebssystem und Daten aus den jeweiligen Momentaufnahmen und anschließend einen neuen virtuellen Computer durch Anfügen der verwalteten Datenträger. Sie können auch die Datenträger eines vorhandenen virtuellen Computers wiederherstellen, indem Sie die aus Momentaufnahmen erstellten Datenträger anfügen.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen verwalteten Datenträger aus einer Momentaufnahme zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Ruft alle Eigenschaften einer Momentaufnahme anhand der Eigenschaften für den Namen und die Ressourcengruppe der Momentaufnahme ab. Die ID-Eigenschaft wird verwendet, um den verwalteten Datenträger zu erstellen.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Erstellt einen verwalteten Datenträger mit der ID einer verwalteten Momentaufnahme |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen virtueller Computer durch Anfügen eines vorhandenen verwalteten Datenträgers als Betriebssystemdatenträger](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Linux-VMs in Azure](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
