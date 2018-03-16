---
title: "Azure CLI-Skriptbeispiel – Löschen eines Containers nach Präfix | Microsoft-Dokumentation"
description: "Löschen Sie Azure Storage Blob-Container basierend auf einem Präfix des Containernamens."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: 9e93dc14a4729011f74c5eafe94528608b89116f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Löschen von Containern basierend auf dem Präfix des Containernamens

Dieses Skript erstellt zunächst einige Beispielcontainer in Azure Blob Storage und löscht anschließend einige der Container basierend auf einem Präfix des Containernamens.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die verbleibenden Container sowie alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um basierend auf dem Präfix des Containernamens Container löschen. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Azure Storage-Konto in der angegebenen Ressourcengruppe. |
| [az storage container create](/cli/azure/storage/container#az_storage_container_create) | Erstellt einen Container in Azure Blob Storage. |
| [az storage container list](/cli/azure/storage/container#az_storage_container_list) | Listet die Container in einem Azure Storage-Konto auf. |
| [az storage container delete](/cli/azure/storage/container#az_storage_container_delete) | Löscht die Container in einem Azure Storage-Konto. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für Speicher finden Sie in den [Azure CLI-Beispielen für Azure Storage](../blobs/storage-samples-blobs-cli.md).
