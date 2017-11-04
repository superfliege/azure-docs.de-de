---
title: Bereitstellen von Containern mit Helm in Kubernetes in Azure | Microsoft-Dokumentation
description: Verwenden Sie das Helm-Verpackungstool zum Bereitstellen von Containern in einem Kubernetes-Cluster in AKS.
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: de52c2aad0f74b59970234872dfa3e4136929915
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="use-helm-with-azure-container-service-aks"></a>Verwenden von Helm mit Azure Container Service (AKS)

[Helm](https://github.com/kubernetes/helm/) ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten. Ähnlich wie Linux-Paket-Manager (z.B. *APT* und *Yum*) wird Helm zur Verwaltung von Kubernetes-Diagrammen verwendet, bei denen es sich um Pakete aus vorkonfigurierten Kubernetes-Ressourcen handelt.

Dieses Dokument beschreibt Schritt für Schritt die Konfiguration und Verwendung von Helm in einem Kubernetes-Cluster in AKS.

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine kubectl-Verbindung mit dem Cluster hergestellt haben. Informationen hierzu finden Sie in der [AKS-Schnellstartanleitung](./kubernetes-walkthrough.md).

## <a name="install-helm-cli"></a>Installieren der Helm-CLI

Bei der Helm-Befehlszeilenschnittstelle handelt es sich um einen Client, der auf Ihrem Entwicklungssystem ausgeführt wird und es Ihnen ermöglicht, Anwendungen zu starten, zu beenden und mit Helm-Diagrammen zu verwalten.

Wenn Sie Azure CloudShell verwenden, ist die Helm-CLI bereits installiert. Wenn Sie die Helm-Befehlszeilenschnittstelle auf einem Mac installieren möchten, verwenden Sie `brew`. Informationen zu weiteren Installationsoptionen finden Sie unter [Installing Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) (Installieren von Helm).

```console
brew install kubernetes-helm
```

Ausgabe:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="configure-helm"></a>Konfigurieren von Helm

Der Befehl [helm init](https://docs.helm.sh/helm/#helm-init) dient zum Installieren von Helm-Komponenten in einem Kubernetes-Cluster und zum Festlegen der clientseitigen Konfigurationen. Helm ist in AKS-Clustern vorinstalliert, sodass nur die clientseitige Konfiguration erforderlich ist. Führen Sie den folgenden Befehl aus, um den Helm-Client zu konfigurieren.

```azurecli-interactive
helm init --client-only
```

Ausgabe:

```
$HELM_HOME has been configured at /Users/neilpeterson/.helm.
Not installing Tiller due to 'client-only' flag having been set
Happy Helming!
```

## <a name="find-helm-charts"></a>Suchen von Helm-Diagrammen

Helm-Diagramme dienen zum Bereitstellen von Anwendungen in einem Kubernetes-Cluster. Verwenden Sie zum Suchen nach zuvor erstellten Helm-Diagrammen den Befehl [helm search](https://docs.helm.sh/helm/#helm-search).

```azurecli-interactive
helm search
```

Die Ausgabe ähnelt der folgenden, jedoch mit wesentlich mehr Diagrammen.

```
NAME                            VERSION DESCRIPTION
stable/acs-engine-autoscaler    2.0.0   Scales worker nodes within agent pools
stable/artifactory              6.1.0   Universal Repository Manager supporting all maj...
stable/aws-cluster-autoscaler   0.3.1   Scales worker nodes within autoscaling groups.
stable/buildkite                0.2.0   Agent for Buildkite
stable/centrifugo               2.0.0   Centrifugo is a real-time messaging server.
stable/chaoskube                0.5.0   Chaoskube periodically kills random pods in you...
stable/chronograf               0.3.0   Open-source web application written in Go and R...
stable/cluster-autoscaler       0.2.0   Scales worker nodes within autoscaling groups.
stable/cockroachdb              0.5.0   CockroachDB is a scalable, survivable, strongly...
stable/concourse                0.7.0   Concourse is a simple and scalable CI system.
stable/consul                   0.4.1   Highly available and distributed service discov...
stable/coredns                  0.5.0   CoreDNS is a DNS server that chains middleware ...
stable/coscale                  0.2.0   CoScale Agent
stable/dask-distributed         2.0.0   Distributed computation in Python
stable/datadog                  0.8.0   DataDog Agent
...
```

Verwenden Sie zum Aktualisieren der Liste der Diagramme den Befehl [helm repo update](https://docs.helm.sh/helm/#helm-repo-update).

```azurecli-interactive
helm repo update
```

Ausgabe:

```
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Ausführen von Helm-Diagrammen

Geben Sie zum Bereitstellen eines NGINX-Eingangscontrollers den Befehl [helm install](https://docs.helm.sh/helm/#helm-install) ein.

```azurecli-interactive
helm install stable/nginx-ingress
```

Die Ausgabe sieht etwa wie folgt aus. Sie enthält aber zusätzliche Informationen wie z.B. Anweisungen zur Verwendung der Kubernetes-Bereitstellung.

```
NAME:   tufted-ocelot
LAST DEPLOYED: Thu Oct  5 00:48:04 2017
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                    DATA  AGE
tufted-ocelot-nginx-ingress-controller  1     5s

==> v1/Service
NAME                                         CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
tufted-ocelot-nginx-ingress-controller       10.0.140.10  <pending>    80:30486/TCP,443:31358/TCP  5s
tufted-ocelot-nginx-ingress-default-backend  10.0.34.132  <none>       80/TCP                      5s

==> v1beta1/Deployment
NAME                                         DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
tufted-ocelot-nginx-ingress-controller       1        1        1           0          5s
tufted-ocelot-nginx-ingress-default-backend  1        1        1           1          5s
...
```

Weitere Informationen zur Verwendung eines NGINX-Eingangscontrollers mit Kubernetes finden Sie unter [NGINX Ingress Controller](https://github.com/kubernetes/ingress/tree/master/controllers/nginx) (NGINX-Eingangscontroller).

## <a name="list-helm-charts"></a>Auflisten von Helm-Diagrammen

Um eine Liste der im Cluster installierten Diagramme anzuzeigen, verwenden Sie den Befehl [helm list](https://docs.helm.sh/helm/#helm-list).

```azurecli-interactive
helm list
```

Ausgabe:

```
NAME            REVISION    UPDATED                     STATUS      CHART               NAMESPACE
bilging-ant     1           Thu Oct  5 00:11:11 2017    DEPLOYED    nginx-ingress-0.8.7 default
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten von Kubernetes-Diagrammen finden Sie in der Helm-Dokumentation.

> [!div class="nextstepaction"]
> [Helm-Dokumentation](https://github.com/kubernetes/helm/blob/master/docs/index.md)