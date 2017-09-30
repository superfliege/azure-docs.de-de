---
title: "Azure CLI-Skriptbeispiel – Löschen eines Containers nach Präfix | Microsoft-Dokumentation"
description: "Löschen Sie Azure Storage Blob-Container basierend auf einem Präfix des Containernamens."
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
ms.date: 06/22/2017
ms.author: marsma
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ff30037c7aba4e0e9c6b4a1829a0769093dce0ac
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

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

| Befehl | Hinweise |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#create) | Erstellt ein Azure Storage-Konto in der angegebenen Ressourcengruppe. |
| [az storage container create](/cli/azure/storage/container#create) | Erstellt einen Container in Azure Blob Storage. |
| [az storage container list](/cli/azure/storage/container#list) | Listet die Container in einem Azure Storage-Konto auf. |
| [az storage container delete](/cli/azure/storage/container#delete) | Löscht die Container in einem Azure Storage-Konto. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure/overview).

Weitere CLI-Skriptbeispiele für Speicher finden Sie in den [Azure CLI-Beispielen für Azure Storage](../blobs/storage-samples-blobs-cli.md).

