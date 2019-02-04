---
title: Includedatei
description: Includedatei
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/11/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 4207031652db7ec4b2ae5468dc159320f6efdbc2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228813"
---
## <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

Sie müssen zunächst ein Media Services-Konto erstellen. In diesem Abschnitt wird gezeigt, was Sie für die Kontoerstellung mithilfe der Azure CLI benötigen.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mithilfe des folgenden Befehls eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Ressourcen wie Azure Media Services-Konten und die zugehörigen Speicherkonten bereitgestellt und verwaltet werden.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. 

Sie müssen über ein **primäres** Storage-Konto verfügen. Darüber hinaus können Sie beliebig viele **sekundäre** Storage-Konten an Ihr Media Services-Konto anfügen. Media Services unterstützt Konten vom Typ **Allgemein v2** (GPv2) oder **Allgemein v1** (GPv1). Reine Blobkonten sind als **primäre** Konten nicht zulässig. Weitere Informationen zu Storage-Konten finden Sie unter [Optionen für Azure Storage-Konten](../articles/storage/common/storage-account-options.md). 

Weitere Informationen zur Verwendung von Speicherkonten in Media Services finden Sie unter [Storage accounts](../articles/media-services/latest/storage-account-concept.md) (Storage-Konten).

Der folgende Befehl erstellt ein Storage-Konto, das dem Media Services-Konto zugeordnet werden soll. Im folgenden Skript können Sie `storageaccountforams` durch Ihren Wert ersetzen. Der Kontoname darf maximal 24 Zeichen lang sein.

```azurecli
az storage account create --name storageaccountforams \  
--kind StorageV2 \
--sku Standard_RAGRS \
--resource-group amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Erstellen eines Media Services-Kontos

Der folgende Azure CLI-Befehl erstellt ein neues Media Services-Konto. Sie können die folgenden Werte ersetzen: `amsaccount`, `storageaccountforams` (muss dem Wert für Ihr Speicherkonto entsprechen) und `amsResourceGroup` (muss dem Wert für Ihre Ressourcengruppe entsprechen).

```azurecli
az ams account create --name amsaccount --resource-group amsResourceGroup --storage-account storageaccountforams
```
