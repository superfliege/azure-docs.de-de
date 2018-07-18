---
title: Löschen eines Azure Kubernetes Service-Clusters (AKS)
description: Löschen eines AKS-Clusters mit der Befehlszeilenschnittstelle oder dem Azure-Portal
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 66dcebb702695a6601f6ed17b85a04d5bb4e01f6
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100042"
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