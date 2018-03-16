---
title: "Azure CLI-Skriptbeispiel: Hinzufügen einer Anwendung in Batch | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Hinzufügen einer Anwendung in Batch"
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
ms.openlocfilehash: 80b9cb749be942f72459180182e7d74a45f6a943
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="cli-example-add-an-application-to-an-azure-batch-account"></a>CLI-Beispiel: Hinzufügen einer Anwendung zu einem Azure Batch-Konto

Dieses Skript veranschaulicht das Hinzufügen einer Anwendung für die Verwendung mit einem Azure Batch-Pool oder einer Azure Batch-Aufgabe. Um eine Anwendung einzurichten, die Sie Ihrem Batch-Konto hinzufügen können, fassen Sie die ausführbare Datei mit sämtlichen Abhängigkeiten in einer ZIP-Datei zusammen. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle.
Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Speicherkonto. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Erstellt das Batch-Konto. |
| [az batch account login](/cli/azure/batch/account#az_batch_account_login) | Authentifiziert das angegebene Batch-Konto zur weiteren CLI-Interaktion.  |
| [az batch application create](/cli/azure/batch/application#az_batch_application_create) | Erstellt eine Anwendung  |
| [az batch application package create](/cli/azure/batch/application/package#az_batch_application_package_create) | Fügt der angegebenen Anwendung ein Anwendungspaket hinzu  |
| [az batch application set](/cli/azure/batch/application#az_batch_application_set) | Aktualisiert die Eigenschaften einer Anwendung  |
| [az group delete](/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
