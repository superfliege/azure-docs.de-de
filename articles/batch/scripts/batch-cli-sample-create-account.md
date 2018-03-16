---
title: "Azure CLI-Skriptbeispiel: Erstellen eines Batch-Kontos – Batch-Dienst | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Erstellen eines Batch-Kontos im Modus „Batch-Dienst“"
services: batch
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: ced93032203c33dc4cda362d30192ee8eb37d944
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="cli-example-create-a-batch-account-in-batch-service-mode"></a>CLI-Beispiel: Erstellen eines Batch-Kontos im Modus „Batch-Dienst“

Dieses Skript erstellt ein Azure Batch-Konto im Modus „Batch-Dienst“ und zeigt, wie verschiedene Eigenschaften des Kontos abgefragt und aktualisiert werden. Wenn Sie ein Batch-Konto im Standardmodus „Batch-Dienst“ erstellen, werden die Computeknoten standardmäßig intern durch den Batch-Dienst zugewiesen. Zugeordnete Computeknoten unterliegen einem separaten vCPU-Kontingent (Kernkontingent), und das Konto kann entweder über Anmeldeinformationen eines gemeinsam verwendeten Schlüssels oder über ein Azure Active Directory-Token authentifiziert werden.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

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
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Speicherkonto. |
| [az batch account set](/cli/azure/batch/account#az_batch_account_set) | Aktualisiert die Eigenschaften des Batch-Kontos.  |
| [az batch account show](/cli/azure/batch/account#az_batch_account_show) | Ruft Details zum angegebenen Batch-Konto ab.  |
| [az batch account keys list](/cli/azure/batch/account/keys#az_batch_account_keys_list) | Ruft die Zugriffsschlüssel zum angegebenen Batch-Konto ab.  |
| [az batch account login](/cli/azure/batch/account#az_batch_account_login) | Authentifiziert das angegebene Batch-Konto zur weiteren CLI-Interaktion.  |
| [az group delete](/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).
