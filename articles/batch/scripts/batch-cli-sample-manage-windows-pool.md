---
title: 'Azure CLI-Skriptbeispiel: Windows-Pool in Batch | Microsoft-Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Erstellen und Verwalten eines Windows-Pools in Batch'
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: 0d9c757925b26e9f46afbd075184f853efa25dc3
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
ms.locfileid: "29843056"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>CLI-Beispiel: Erstellen und Verwalten eines Windows-Pools in Azure Batch

In diesem Skript werden einige der verfügbaren Azure CLI-Tools zum Erstellen und Verwalten eines Pools mit Windows-Computeknoten in Azure Batch präsentiert. Ein Windows-Pool kann auf zwei Arten konfiguriert werden: als Cloud Services-Konfiguration oder als VM-Konfiguration. Dieses Beispiel zeigt die Erstellung eines Windows-Pools mit der Cloud Services-Konfiguration.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Erstellt das Batch-Konto. |
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Authentifiziert das angegebene Batch-Konto zur weiteren CLI-Interaktion. |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Erstellt einen Pool mit Computeknoten.  |
| [az batch pool set](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | Aktualisiert die Eigenschaften eines Pools.  |
| [az batch pool autoscale enable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Aktiviert die automatische Skalierung für einen Pool, und wendet eine Formel an.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Zeigt die Eigenschaften eines Pools an.  |
| [az batch pool autoscale disable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Deaktiviert die automatische Skalierung für einen Pool. |
| [az group delete](/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
