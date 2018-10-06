---
title: Includedatei
description: Includedatei
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: f2e252a000c5633b2cc1ef34683d7d2c7c133c03
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046390"
---
## <a name="using-rbac-to-share-images"></a>Verwenden von RBAC zum Teilen von Abbildern

Sie können Abbilder zwischen Abonnements mithilfe der rollenbasierten Zugriffssteuerung (Role Based Access Control, RBAC) teilen. Jeder Benutzer, der Leseberechtigungen für eine Abbildversion besitzt, auch über Abonnements hinweg, kann mithilfe der Abbildversion einen virtuellen Computer bereitstellen.

Weitere Informationen zum Teilen von Ressourcen mittels RBAC finden Sie unter [Verwalten des Zugriffs mithilfe von RBAC und der Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).


## <a name="list-information"></a>Auflisten von Informationen

Den Speicherort, Status und weitere Informationen zu den verfügbaren Abbildkatalogen können Sit mit [az sig list](/cli/azure/sig#az-sig-list) abrufen.

```azurecli-interactive 
az sig list -o table
```

Die Abbilddefinitionen in einem Katalog, einschließlich der Informationen zum Betriebssystemtyp und -status können Sie mit [az sig image-definition list](/cli/azure/sig/image-definition#az-sig-image-definition-list) auflisten.

```azurecli-interactive 
az sig image-definition list -g myGalleryRG -r myGallery -o table
```

Die Versionen geteilter Abbilder in einem Katalog können Sie mit [az sig image-version list](/cli/azure/sig/image-version#az-sig-image-version-list) auflisten.

```azurecli-interactive
az sig image-version list -g myGalleryRG -r myGallery -i myGalleryImage -o table
```

Die ID einer Abbildversion können Sie mit [az sig image-version show](/cli/azure/sig/image-version#az-sig-image-version-show) abrufen.

```
az sig image-version show \
-g myGalleryRG \     
-r myGallery \     
-i myGalleryImage \     
--gallery-image-version-name 1.0.0 \     
--query "id"
```