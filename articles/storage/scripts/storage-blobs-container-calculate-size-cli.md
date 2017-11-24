---
title: "Azure CLI-Skriptbeispiel – Berechnen der Größe des Blobcontainers | Microsoft-Dokumentation"
description: "Berechnen Sie die Größe eines Containers in Azure Blob Storage, indem Sie die Größe der Blobs im Container addieren."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: 86922fac2154d4084f34275e761addeafe565fc0
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Berechnen der Größe eines Blob Storage-Containers

Mit diesem Skript berechnen Sie die Größe eines Containers in Azure Blob Storage, indem Sie die Größe der Blobs im Container addieren.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Dieses CLI-Skript gibt eine geschätzte Größe für den Container an und sollte nicht für Abrechnungsberechnungen verwendet werden.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den Container und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um die Größe des Blob Storage-Containers zu berechnen. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage blob upload](/cli/azure/storage/account#create) | Lädt die Dateien in einen Azure Blob Storage-Container hoch. |
| [az storage blob list](/cli/azure/storage/account/keys#list) | Listet die Dateien in einem Azure Blob Storage-Container auf. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure/overview).

Weitere CLI-Skriptbeispiele für Speicher finden Sie in den [Azure CLI-Beispielen für Azure Blob Storage](../blobs/storage-samples-blobs-cli.md).
