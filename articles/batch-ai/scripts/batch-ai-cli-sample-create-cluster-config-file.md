---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines Batch AI-Clusters mit einer Konfigurationsdatei | Microsoft-Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Erstellen Sie einen Batch AI-Cluster durch Angeben der Konfigurationseinstellungen in einer JSON-Datei.'
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
ms.date: 08/16/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 41a3a801214ff00c01397034e26fde6946ab97f0
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407812"
---
# <a name="cli-example-create-a-batch-ai-cluster-using-a-cluster-configuration-file"></a>CLI-Beispiel: Erstellen eines Batch AI-Clusters mithilfe einer Clusterkonfigurationsdatei

[!INCLUDE [batch-ai-retiring](../../../includes/batch-ai-retiring.md)]

Dieses Skript zeigt, wie Sie eine JSON-Konfigurationsdatei zum Festlegen der Einstellungen für einen Batch AI-Cluster verwenden. Verwenden Sie diese Einstellungen anstelle der entsprechenden Befehlszeilenparameter für `az batchai cluster create`. Eine Konfigurationsdatei wird verwendet, wenn Sie mehrere Dateisysteme auf den Clusterknoten einbinden müssen oder eine identische Konfiguration in mehreren Clustern verwenden möchten.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Leitfaden mindestens Version 2.0.38 der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/batch-ai/create-cluster/create-cluster-config-file.sh "Create Batch AI cluster - configuration file")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen.

```azurecli-interactive
# Remove resource group for the cluster.
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellt ein Speicherkonto. |
| [az storage share create](/cli/azure/storage/share#az-storage-share-create) | Erstellt eine Dateifreigabe in einem Speicherkonto. |
| [az batchai workspace create](/cli/azure/batchai/workspace#az-batchai-workspace-create) | Erstellt einen Azure Batch AI-Arbeitsbereich. |
| [az batchai cluster create](/cli/azure/batchai/cluster#az-batchai-cluster-create) | Erstellt einen Azure Batch AI-Cluster. |
| [az batchai cluster show](/cli/azure/batchai/cluster#az-batchai-cluster-show) | Zeigt Informationen zu einem Batch AI-Cluster an. |
| [az group delete](/cli/azure/group#az-group-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
