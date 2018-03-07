---
title: Erstellen eines Azure Container Service-Clusters (AKS)
description: Erstellen eines AKS-Clusters mit der Befehlszeilenschnittstelle oder dem Azure-Portal
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 02/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 37d6dfc0aa6b3e4fcd88a53e83a3a3d7f2157681
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="create-an-azure-container-service-aks-cluster"></a>Erstellen eines Azure Container Service-Clusters (AKS)

Ein Azure Container Service-Cluster (AKS) kann mit der Azure-Befehlszeilenschnittstelle oder dem Azure-Portal erstellt werden.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az aks create][az-aks-create] zum Erstellen des AKS-Clusters.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Für den Befehl `az aks create` sind die folgenden Optionen verfügbar:

| Argument | BESCHREIBUNG | Erforderlich |
|---|---|:---:|
| `--name``-n` | Ressourcenname für den verwalteten Cluster | Ja |
| `--resource-group``-g` | Name der Azure Container Service-Ressourcengruppe | Ja |
| `--admin-username``-u` | Benutzername für die virtuellen Linux-Computer.  Standard: azureuser. | no |
| ` --client-secret` | Das dem Dienstprinzipal zugeordnete Geheimnis | no |
| `--dns-name-prefix``-p` | DNS-Präfix für die öffentliche IP-Adresse des Clusters | no |
| `--generate-ssh-keys` | Erstellen der Dateien für öffentliche und private SSH-Schlüssel, sofern diese fehlen. | no |
| `--kubernetes-version``-k` | Die Version von Kubernetes zum Erstellen des Clusters, z.B. „1.7.9“ oder „1.8.2“.  Standard: 1.7.7. | no |
| `--no-wait` | Nicht auf den Abschluss lang andauernder Vorgänge warten | no |
| `--node-count``-c` | Standardanzahl von Knoten für die Knotenpools.  Standard: 3. | no |
| `--node-osdisk-size` | Größe des Betriebssystemdatenträgers des virtuellen Computers für den Knotenpool in GB | no |
| `--node-vm-size``-s` | Größe des virtuellen Computers.  Standard: Standard_D1_v2. | no |
| `--service-principal` | Dienstprinzipal für die Clusterauthentifizierung | no |
| `--ssh-key-value` | SSH-Schlüsseldateiwert oder -Schlüsseldateipfad.  Standard: ~/.ssh/id_rsa.pub. | no |
| `--tags` | Durch Leerzeichen getrennte Tags im Format „Schlüssel[=Wert]“. Verwenden Sie ", um vorhandene Tags zu löschen. | no |

## <a name="azure-portal"></a>Azure-Portal

Anweisungen zum Bereitstellen eines AKS-Clusters mit dem Azure-Portal finden Sie im [Azure-Portal-Schnellstart][aks-portal-quickstart] für den Azure Container Service (AKS). 

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
