---
title: "Azure-Schnellstart – Erstellen eines Speicherkontos mit der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie ein neues Speicherkonto mit der Azure-Befehlszeilenschnittstelle (CLI) erstellen.
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
ms.devlang: na
ms.topic: quickstart
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: ed956e3d27d315e0ce4901c2c38d50652f77c09a
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Erstellen eines Speicherkontos mit der Azure-Befehlszeilenschnittstelle

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Schnellstartanleitung wird erläutert, wie Sie mit der Azure-Befehlszeilenschnittstelle ein Azure Storage-Konto erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *eastus* erstellt.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Wenn Sie nicht wissen, welche Region Sie für den `--location`-Parameter angeben sollen, können Sie mit dem Befehl [az account list-locations](/cli/azure/account#list) eine Liste der unterstützten Regionen für Ihr Abonnement abrufen.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-storage-account"></a>Erstellen eines allgemeinen Speicherkontos

Es gibt verschiedene Speicherkontotypen für verschiedene Verwendungsszenarien, bei denen jeweils einer oder mehrere Speicherdienste unterstützt werden (Blobs, Dateien, Tabellen oder Warteschlangen). Die folgende Tabelle enthält die verfügbaren Speicherkontotypen.

|**Speicherkontotyp**|**Standard (allgemein)**|**Premium (allgemein)**|**Blobspeicher, Zugriffsebenen „Heiß“ und „Kalt“**|
|-----|-----|-----|-----|
|**Unterstützte Dienste**| Blob-, Datei-, Tabellen- und Warteschlangendienst | Blob-Dienst | Blob-Dienst|
|**Unterstützte Blobtypen**|Blockblobs, Seitenblobs, Anfügeblobs | Seitenblobs | Blockblobs und Anfügeblobs|

Erstellen Sie mit dem Befehl [az storage account create](/cli/azure/storage/account#create) ein allgemeines Speicherkonto.

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --kind Storage \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen in der Ressourcengruppe, einschließlich des in dieser Schnellstartanleitung erstellten Speicherkontos, nicht mehr benötigen, löschen Sie die Ressourcengruppe mit dem Befehl [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine Ressourcengruppe und ein allgemeines Standardspeicherkonto erstellt. Weitere Informationen zum Übertragen von Daten in das und aus dem Speicherkonto finden Sie im Schnellstart für Blob Storage.

> [!div class="nextstepaction"]
> [Übertragen Objekten nach/aus Azure Blob Storage mit der Azure-Befehlszeilenschnittstelle](../blobs/storage-quickstart-blobs-cli.md)