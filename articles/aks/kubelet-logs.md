---
title: Abrufen von Kubelet-Protokollen aus Azure Container Service (AKS)
description: Abrufen von Kubelet-Protokollen aus Azure Container Service-Clusterknoten (AKS)
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 56e20a9f9d17eac01e6f85007db41dcc417f83e4
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="get-kubelet-logs-from-azure-container-service-aks-cluster-nodes"></a>Abrufen von Kubelet-Protokollen aus Azure Container Service-Clusterknoten (AKS)

Es kann vorkommen, dass Sie Kubelet-Protokolle aus einem Azure Container Service-Knoten (AKS) zur Problembehandlung abrufen müssen. Dieses Dokument zeigt eine Möglichkeit auf, diese Protokolle abzurufen.

## <a name="create-an-ssh-connection"></a>Erstellen einer SSH-Verbindung

Erstellen Sie zunächst eine SSH-Verbindung mit dem Knoten, aus dem Sie Kubelet-Protokolle abrufen müssen. Einzelheiten zu diesem Vorgang finden Sie im Dokument [Zugreifen per SSH auf Azure Container Service-Clusterknoten (AKS-Clusterknoten)][aks-ssh].

## <a name="get-kubelet-logs"></a>Abrufen von Kubelet-Protokollen

Sobald Sie mit dem Knoten verbunden sind, können Sie den folgenden Befehl ausführen, um die Kubelet-Protokolle abzurufen.

```azurecli-interactive
docker logs $(docker ps -a | grep "hyperkube kubele" | awk -F ' ' '{print $1}')
```

Beispielausgabe:

```console
2018-03-05 00:04:00.883195 I | proto: duplicate proto type registered: google.protobuf.Any
2018-03-05 00:04:00.883242 I | proto: duplicate proto type registered: google.protobuf.Duration
2018-03-05 00:04:00.883253 I | proto: duplicate proto type registered: google.protobuf.Timestamp
Flag --non-masquerade-cidr has been deprecated, will be removed in a future version
Flag --non-masquerade-cidr has been deprecated, will be removed in a future version
I0305 00:04:00.978560    8021 feature_gate.go:144] feature gates: map[Accelerators:true]
I0305 00:04:00.978996    8021 azure.go:174] azure: using client_id+client_secret to retrieve access token
I0305 00:04:00.979041    8021 server.go:439] Successfully initialized cloud provider: "azure" from the config file: "/etc/kubernetes/azure.json"
I0305 00:04:00.979058    8021 server.go:740] cloud provider determined current node name to be aks-nodepool1-42032720-0
```

<!-- LINKS - internal -->
[aks-ssh]: aks-ssh.md