---
title: Azure CLI-Beispielskript – Rotieren von Speicherkonto-Zugriffsschlüsseln | Microsoft-Dokumentation
description: Erstellen Sie ein Azure Storage-Konto, und rufen Sie dann die Zugriffsschlüssel für das Konto ab, und rotieren Sie sie.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: ac58886225221677aa003833167ff58cd578255d
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55693920"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Erstellen eines Speicherkontos und Rotieren der Zugriffsschlüssel für das Konto

Mit diesem Skript wird ein Azure Storage-Konto erstellt, die Zugriffsschlüssel des neuen Speicherkontos werden angezeigt, und dann werden die Schlüssel erneuert (rotiert).

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, das Speicherkonto und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um das Speicherkonto zu erstellen und die Zugriffsschlüssel abzurufen und zu rotieren. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account) | Erstellt ein Azure Storage-Konto in der angegebenen Ressourcengruppe. |
| [az storage account keys list](/cli/azure/storage/account/keys) | Zeigt die Zugriffsschlüssel für das angegebene Konto an. |
| [az storage account keys renew](/cli/azure/storage/account/keys) | Generiert den primären oder sekundären Zugriffsschlüssel für das Speicherkonto neu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für Speicher finden Sie in den [Azure CLI-Beispielen für Azure Blob Storage](../blobs/storage-samples-blobs-cli.md).
