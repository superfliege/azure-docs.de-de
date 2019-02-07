---
title: Azure CLI-Skriptbeispiel – Löschen eines Containers nach Präfix | Microsoft-Dokumentation
description: Löschen Sie Azure Storage Blob-Container basierend auf einem Präfix des Containernamens.
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
ms.openlocfilehash: 01187a4dbcd8333f95cf20b5956b7b81559a19a8
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55730651"
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
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account) | Erstellt ein Azure Storage-Konto in der angegebenen Ressourcengruppe. |
| [az storage container create](/cli/azure/storage/container) | Erstellt einen Container in Azure Blob Storage. |
| [az storage container list](/cli/azure/storage/container) | Listet die Container in einem Azure Storage-Konto auf. |
| [az storage container delete](/cli/azure/storage/container) | Löscht die Container in einem Azure Storage-Konto. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für Speicher finden Sie in den [Azure CLI-Beispielen für Azure Storage](../blobs/storage-samples-blobs-cli.md).
