---
title: Erstellen eines Azure Kubernetes Service-Clusters (AKS)
description: Erstellen eines AKS-Clusters mit der Befehlszeilenschnittstelle oder dem Azure-Portal
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5c133c61c989bf19be3e84287cb76a7d110dccc8
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440472"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Erstellen eines Azure Kubernetes Service-Clusters (AKS)

Ein Azure Kubernetes Service-Cluster (AKS) kann mit der Azure-Befehlszeilenschnittstelle oder dem Azure-Portal erstellt werden.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az aks create][az-aks-create] zum Erstellen des AKS-Clusters.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Für den Befehl `az aks create` sind die folgenden Optionen verfügbar: Weitere Informationen zu diesen Argumenten finden Sie in der [Azure CLI-Referenz][az-aks-create] für AKS.

| Argument | BESCHREIBUNG | Erforderlich |
|---|---|:---:|
| `--name``-n` | Ressourcenname für den verwalteten Cluster | Ja |
| `--resource-group``-g` | Name der Azure Kubernetes Service-Ressourcengruppe | Ja |
| `--admin-username``-u` | Benutzername für die virtuellen Linux-Computer.  Standard: azureuser. | no |
| `--aad-client-app-id` | (VORSCHAUVERSION) Die ID einer Azure Active Directory-Clientanwendung vom Typ „Nativ“ | no |
| `--aad-server-app-id` | (VORSCHAUVERSION) Die ID einer Azure Active Directory-Serveranwendung vom Typ „Web-App/API“ | no |
| `--aad-server-app-secret` | (VORSCHAUVERSION) Das Geheimnis einer Azure Active Directory-Serveranwendung | no |
| `--aad-tenant-id` | (VORSCHAUVERSION) Die ID eines Azure Active Directory-Mandanten | no |
| `--admin-username``-u` | Benutzerkonto zum Erstellen von VMs auf Knoten für den SSH-Zugriff  Standard: azureuser. | no |
| ` --client-secret` | Das dem Dienstprinzipal zugeordnete Geheimnis | no |
| `--dns-name-prefix``-p` | DNS-Präfix für die öffentliche IP-Adresse des Clusters | no |
| `--dns-service-ip` | IP-Adresse, die dem Kubernetes-DNS-Dienst zugewiesen ist | no |
| `--docker-bridge-address` | IP-Adresse und Netzmaske, die der Docker-Bridge zugewiesen ist | no |
| `--enable-addons``-a` | Aktivieren der Kubernetes-Add-Ons in einer kommagetrennten Liste | no |
| `--enable-rbac``-r` | Aktivieren der rollenbasierten Zugriffssteuerung für Kubernetes | no |
| `--generate-ssh-keys` | Erstellen der Dateien für öffentliche und private SSH-Schlüssel, sofern diese fehlen. | no |
| `--kubernetes-version``-k` | Die Version von Kubernetes zum Erstellen des Clusters, z.B. „1.7.9“ oder „1.9.6“. | no |
| `--locaton``-l` | Speicherort für die automatisch erstellte Ressourcengruppe | no |
| `--max-pods``-m` | Maximale Anzahl von Pods, die auf einem Knoten bereitgestellt werden können | no |
| `--network-plugin` | Zu verwendendes Kubernetes-Netzwerk-Plug-In | no |
| `--no-ssh-key``-x` | Keinen lokalen SSH-Schlüssel verwenden oder erstellen | no |
| `--no-wait` | Nicht auf den Abschluss lang andauernder Vorgänge warten | no |
| `--node-count``-c` | Standardanzahl von Knoten für die Knotenpools.  Standard: 3. | no |
| `--node-osdisk-size` | Größe des Betriebssystemdatenträgers des virtuellen Computers für den Knotenpool in GB | no |
| `--node-vm-size``-s` | Größe des virtuellen Computers.  Standard: Standard_D1_v2. | no |
| `--pod-cidr` | IP-Bereich in CIDR-Notation, über den Pod-IPs zugewiesen werden, wenn kubenet verwendet wird | no |
| `--service-cidr` | IP-Bereich in CIDR-Notation, über den Dienstcluster-IPs zugewiesen werden | no |
| `--service-principal` | Dienstprinzipal für die Clusterauthentifizierung | no |
| `--ssh-key-value` | SSH-Schlüsseldateiwert oder -Schlüsseldateipfad.  Standard: ~/.ssh/id_rsa.pub. | no |
| `--tags` | Durch Leerzeichen getrennte Tags im Format „Schlüssel[=Wert]“. Verwenden Sie ", um vorhandene Tags zu löschen. | no |
| `--vnet-subnet-id` | Die ID eines Subnetzes in einem vorhandenen VNET, in dem der Cluster bereitgestellt werden soll | no |
| `--workspace-resource-id` | Ressourcen-ID eines vorhandenen Log Analytics-Arbeitsbereichs zum Speichern von Überwachungsdaten | no |

## <a name="azure-portal"></a>Azure-Portal

Anweisungen zum Bereitstellen eines AKS-Clusters mit dem Azure-Portal finden Sie im [Azure-Portal-Schnellstart][aks-portal-quickstart] für Azure Kubernetes Service (AKS).

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
