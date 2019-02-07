---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines dedizierten Batch AI-Clusters | Microsoft-Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Erstellen und Verwalten eines Batch AI-Clusters mit dedizierten Knoten (virtuelle Computer)'
services: batch-ai
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.devlang: azurecli
ms.topic: sample
ms.tgt-pltfrm: multiple
ms.workload: na
ms.date: 07/26/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 8ca2def6aa6ab463e5b2be0203791da671a41745
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697803"
---
# <a name="cli-example-create-and-manage-a-batch-ai-cluster-of-dedicated-nodes"></a>CLI-Beispiel: Erstellen und Verwalten eines Batch AI-Clusters mit dedizierte Knoten

[!INCLUDE [batch-ai-retiring](../../../includes/batch-ai-retiring.md)]

Dieses Skript zeigt einige der in der Azure CLI verfügbaren Befehle zum Erstellen und Verwalten eines Batch-AI-Clusters, der aus dedizierten Knoten (virtuellen Computern) besteht.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.38 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/batch-ai/create-cluster/create-cluster-dedicated.sh "Create Batch AI cluster - dedicated nodes")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen.

```azurecli-interactive
# Remove resource group for the cluster.
az group delete --name myResourceGroup

# Remove resource group for the auto-storage account.
az group delete --name batchaiautostorage
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az batchai workspace create](/cli/azure/batchai/workspace#az-batchai-workspace-create) | Erstellt einen Azure Batch AI-Arbeitsbereich. |
| [az batchai cluster create](/cli/azure/batchai/cluster#az-batchai-cluster-create) | Erstellt einen Azure Batch AI-Cluster. |
| [az batchai cluster show](/cli/azure/batchai/cluster) | Zeigt Informationen zu einem Batch AI-Cluster an. |
| [az batchai cluster node list](/cli/azure/batchai/cluster/node) | Zeigt eine Liste der Knoten in einem Batch AI-Cluster an. |
| [az batchai cluster resize](/cli/azure/batchai/cluster#az-batchai-cluster-resize) | Ändert die Größe des Batch AI-Clusters.  |
| [az group delete](/cli/azure/group#az-group-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
