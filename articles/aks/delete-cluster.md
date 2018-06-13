---
title: Löschen eines Azure Kubernetes Service-Clusters (AKS)
description: Löschen eines AKS-Clusters mit der Befehlszeilenschnittstelle oder dem Azure-Portal
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6391e36eff60634e07a90c1e6b5f0f44ee60d46b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33936444"
---
# <a name="delete-an-azure-kubernetes-service-aks-cluster"></a>Löschen eines Azure Kubernetes Service-Clusters (AKS)

Wenn Sie einen Azure Kubernetes Service-Cluster löschen, bleibt die Ressourcengruppe, in der der Cluster bereitgestellt wurde, erhalten, alle Ressourcen im Zusammenhang mit AKS werden jedoch gelöscht. Dieses Dokument zeigt, wie ein AKS-Cluster mit der Azure-Befehlszeilenschnittstelle und dem Azure-Portal gelöscht wird.

Zusätzlich zum Löschen des Clusters kann auch die Ressourcengruppe, in der er bereitgestellt wurde, gelöscht werden. Dabei wird auch der AKS-Cluster gelöscht.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az aks delete][az-aks-delete] zum Löschen des AKS-Clusters.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

Für den Befehl `az aks delete` sind die folgenden Optionen verfügbar:

| Argument | BESCHREIBUNG | Erforderlich |
|---|---|:---:|
| `--name``-n` | Ressourcenname für den verwalteten Cluster | Ja |
| `--resource-group``-g` | Name der Azure Kubernetes Service-Ressourcengruppe | Ja |
| `--no-wait` | Nicht auf den Abschluss lang andauernder Vorgänge warten | no |
| `--yes``-y` | Nicht zur Bestätigung auffordern | no |

## <a name="azure-portal"></a>Azure-Portal

Navigieren Sie im Azure-Portal zu der Ressourcengruppe mit der Azure Kubernetes Service-Ressource (AKS), wählen Sie die Ressource aus, und klicken Sie auf **Löschen**. Sie werden aufgefordert, den Löschvorgang zu bestätigen.

![Löschen eines AKS-Clusters im Portal](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az_aks_delete