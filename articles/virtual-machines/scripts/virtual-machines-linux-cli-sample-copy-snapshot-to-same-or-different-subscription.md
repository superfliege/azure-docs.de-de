---
title: Azure CLI-Beispielskript – Kopieren (Verschieben) einer Momentaufnahme eines verwalteten Datenträgers in dasselbe oder ein anderes Abonnement mithilfe der Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: Azure CLI-Beispielskript – Kopieren (Verschieben) einer Momentaufnahme eines verwalteten Datenträgers in dasselbe oder ein anderes Abonnement mithilfe der Befehlszeilenschnittstelle
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
ms.openlocfilehash: d52e710cadbc37b8ad6c1de61f75df3b96704daa
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243906"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Kopieren von Momentaufnahmen verwalteter Datenträger in dasselbe oder ein anderes Abonnement mithilfe der Befehlszeilenschnittstelle

Dieses Skript kopiert eine Momentaufnahme eines verwalteten Datenträgers in dasselbe oder ein anderes Abonnement. Verwenden Sie dieses Skript für folgende Szenarien:

1. Migrieren einer Momentaufnahme in Storage Premium (Premium_LRS) zu Storage Standard (Standard_LRS oder Standard_ZRS), um Ihre Kosten zu senken
1. Migrieren einer Momentaufnahme aus lokal redundantem Speicher (Premium_LRS, Standard_LRS) zu zonenredundantem Speicher (Standard_ZRS), um von der höheren Zuverlässigkeit von ZRS-Speicher zu profitieren
1. Verschieben einer Momentaufnahme in ein anderes Abonnement in der gleichen Region zur längeren Aufbewahrung

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen einer Momentaufnahme im Zielabonnement mit der ID der Quellmomentaufnahme. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot) | Ruft alle Eigenschaften einer Momentaufnahme anhand der Eigenschaften für den Namen und die Ressourcengruppe der Momentaufnahme ab. Die ID-Eigenschaft wird verwendet, um die Momentaufnahme in ein anderes Abonnement zu kopieren.  |
| [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot) | Kopiert eine Momentaufnahme durch Erstellen einer Momentaufnahme in einem anderen Abonnement mit der ID und dem Namen der übergeordneten Momentaufnahme  |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines virtuellen Computers aus einer Momentaufnahme](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
