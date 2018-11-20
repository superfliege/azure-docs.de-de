---
title: Azure CLI-Beispielskript – Kopieren (Verschieben) verwalteter Datenträger in dasselbe oder ein anderes Abonnement | Microsoft-Dokumentation
description: Azure CLI-Beispielskript – Kopieren (Verschieben) verwalteter Datenträger in dasselbe oder ein anderes Abonnement
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
ms.openlocfilehash: 65d0d6848534326bb3ef43f1bd8bc700971603e6
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636797"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Kopieren verwalteter Datenträger in dasselbe oder ein anderes Abonnement mithilfe der Befehlszeilenschnittstelle

Dieses Skript kopiert einen verwalteten Datenträger in dasselbe oder ein anderes Abonnement in derselben Region. Sie müssen globaler AAD-Mandantenadministrator beider Abonnement sein, um dieses Skript ausführen zu können.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen eines neuen verwalteten Datenträgers im Zielabonnement mit der ID des verwalteten Quelldatenträgers. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Ruft alle Eigenschaften eines verwalteten Datenträgers anhand der Eigenschaften für den Namen und die Ressourcengruppe des verwalteten Datenträgers ab. Die ID-Eigenschaft wird verwendet, um den verwalteten Datenträger in ein anderes Abonnement zu kopieren.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Kopiert einen verwalteten Datenträger durch Erstellen eines neuen verwalteten Datenträgers in einem anderen Abonnement mit der ID und dem Namen des übergeordneten verwalteten Datenträgers  |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen virtueller Computer aus verwalteten Datenträgern](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Linux-VMs in Azure](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
