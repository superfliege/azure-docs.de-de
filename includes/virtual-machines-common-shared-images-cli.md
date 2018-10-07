---
title: Includedatei
description: Includedatei
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 09/13/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 830deb7569772b610b7e6abde649830b7ad67a92
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046084"
---
## <a name="before-you-begin"></a>Voraussetzungen

Für das Beispiel in diesem Artikel muss ein verwaltetes Image eines generalisierten virtuellen Computers vorhanden sein. Weitere Informationen finden Sie im [Tutorial: Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mit Azure CLI 2.0](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). 

## <a name="preview-register-the-feature"></a>Preview: Registrieren des Features

Kataloge mit geteilten Images befinden sich in der Preview, aber Sie müssen das Feature registrieren, bevor Sie es verwenden können. So registrieren Sie das Feature für Kataloge mit geteilten Images

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name GalleryPreview
az provider register -n Microsoft.Compute
```

Die Registrierung dieses Features kann möglicherweise ein paar Minuten dauern. Sie können den Fortschritt auf folgende Weisen überprüfen:

```azurecli-interactive
az provider show -n Microsoft.Compute
```

## <a name="create-an-image-gallery"></a>Erstellen eines Imagekatalogs 

Ein Imagekatalog ist die primäre Ressource, die zur Ermöglichung des Teilens von Images verwendet wird. Katalognamen müssen innerhalb Ihres Abonnements eindeutig sein. Erstellen Sie einen Imagekatalog mit [az sig create](/cli/azure/sig#az-sig-create). Im folgenden Beispiel wird ein Katalog namens *my Gallery* in *myGalleryRG* erstellt.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create -g myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>Erstellen einer Imagedefinition

Erstellen Sie eine anfängliche Imagedefinition im Katalog mithilfe von [az sig image-definition create](/cli/azure/sig/image-definition#az-sig-image-definition-create).

```azurecli-interactive 
az sig image-definition create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```

## <a name="create-an-image-version"></a>Erstellen einer Imageversion 
 
Erstellen Sie nach Bedarf Versionen des Images mittels [az image gallery create-image-version](/cli/azure/sig/image-version#az-sig-image-version-create). Sie müssen die ID des verwalteten Images, das als Grundlage zum Erstellen der Imageversion verwendet werden soll, als Eingabe übergeben. Sie können [az image list](/cli/azure/image?view#az-image-list) verwenden, um Informationen zu Images abzurufen, die sich in einer Ressourcengruppe befinden. In diesem Beispiel ist die Version Ihres Images *1.0.0*, und wir erstellen 5 Replikate in den Regionen *USA, Westen Mitte*, *USA, Süden-Mitte* und „USA, Osten 2*“.

```azurecli-interactive 
az sig image-version create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1" \
   --replica-count 5 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

