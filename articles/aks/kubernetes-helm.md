---
title: Bereitstellen von Containern mit Helm in Kubernetes in Azure
description: Verwenden Sie das Helm-Verpackungstool zum Bereitstellen von Containern in einem Kubernetes-Cluster in AKS.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 531e6d9368b2bf91c48fd41b1e9330879b0df49a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101546"
---
# <a name="use-helm-with-azure-kubernetes-service-aks"></a>Verwenden von Helm mit Azure Kubernetes Service (AKS)

[Helm][helm] ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten. Ähnlich wie Linux-Paket-Manager (z.B. *APT* und *Yum*) wird Helm zur Verwaltung von Kubernetes-Diagrammen verwendet, bei denen es sich um Pakete aus vorkonfigurierten Kubernetes-Ressourcen handelt.

Dieses Dokument beschreibt Schritt für Schritt die Konfiguration und Verwendung von Helm in einem Kubernetes-Cluster in AKS.

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine kubectl-Verbindung mit dem Cluster hergestellt haben. Informationen hierzu finden Sie in der [AKS-Schnellstartanleitung][aks-quickstart].

## <a name="install-helm-cli"></a>Installieren der Helm-CLI

Bei der Helm-Befehlszeilenschnittstelle handelt es sich um einen Client, der auf Ihrem Entwicklungssystem ausgeführt wird und es Ihnen ermöglicht, Anwendungen zu starten, zu beenden und mit Helm zu verwalten.

Wenn Sie Azure CloudShell verwenden, ist die Helm-CLI bereits installiert. Wenn Sie die Helm-Befehlszeilenschnittstelle auf einem Mac installieren möchten, verwenden Sie `brew`. Informationen zu weiteren Installationsoptionen finden Sie unter [Installing Helm][helm-install-options] (Installieren von Helm).

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

## <a name="create-service-account"></a>Dienstkonto erstellen

Vor der Konfiguration von Helm in einem RBAC-fähigen Cluster benötigen Sie ein Dienstkonto und eine Rollenbindung für den Tiller-Dienst. Weitere Informationen zum Sichern von Helm/Tiller in einem RBAC-fähigen Cluster finden Sie unter [Tiller, Namespaces und RBAC][tiller-rbac]. Beachten Sie, dass Sie diesen Schritt überspringen können, wenn Ihr Cluster nicht RBAC-fähig ist.

Erstellen Sie eine Datei namens „`helm-rbac.yaml`“, und fügen Sie den folgenden YAML-Code ein.

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Erstellen Sie das Dienstkonto und die Rollenbindung mithilfe des Befehls `kubectl create`.

```
kubectl create -f helm-rbac.yaml
```

Bei der Verwendung eines RBAC-fähigen Clusters verfügen Sie auf der Zugriffsebene über Optionen, über die Tiller für den Cluster verfügt. Weitere Informationen zu den Konfigurationsoptionen finden Sie unter [Helm: Rollenbasierte Zugriffssteuerungen][helm-rbac].

## <a name="configure-helm"></a>Konfigurieren von Helm

Installieren Sie Tiller jetzt mithilfe des Befehls [helm init][helm-init]. Wenn Ihr Cluster nicht RBAC-fähig ist, entfernen Sie das Argument `--service-account` und den Wert.

```
helm init --service-account tiller
```

## <a name="find-helm-charts"></a>Suchen von Helm-Diagrammen

Helm-Diagramme dienen zum Bereitstellen von Anwendungen in einem Kubernetes-Cluster. Verwenden Sie zum Suchen nach zuvor erstellten Helm-Diagrammen den Befehl [helm search][helm-search].

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

Verwenden Sie zum Aktualisieren der Liste der Diagramme den Befehl [helm repo update][helm-repo-update].

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

Verwenden Sie für die Bereitstellung von Wordpress über ein Helm-Diagramm den Befehl [helm install][helm-install].

```azurecli-interactive
helm install stable/wordpress
```

Die Ausgabe sieht etwa wie folgt aus. Sie enthält aber zusätzliche Informationen wie z.B. Anweisungen zur Verwendung der Kubernetes-Bereitstellung.

```
NAME:   bilging-ibex
LAST DEPLOYED: Tue Jun  5 14:31:49 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                     READY  STATUS   RESTARTS  AGE
bilging-ibex-mariadb-7557b5474-dmdxn     0/1    Pending  0         1s
bilging-ibex-wordpress-7494c545fb-tskhz  0/1    Pending  0         1s

==> v1/Secret
NAME                    TYPE    DATA  AGE
bilging-ibex-mariadb    Opaque  2     1s
bilging-ibex-wordpress  Opaque  2     1s

==> v1/ConfigMap
NAME                        DATA  AGE
bilging-ibex-mariadb        1     1s
bilging-ibex-mariadb-tests  1     1s

==> v1/PersistentVolumeClaim
NAME                    STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
bilging-ibex-mariadb    Pending  default  1s
bilging-ibex-wordpress  Pending  default  1s

==> v1/Service
NAME                    TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)                     AGE
bilging-ibex-mariadb    ClusterIP     10.0.76.164   <none>       3306/TCP                    1s
bilging-ibex-wordpress  LoadBalancer  10.0.215.250  <pending>    80:30934/TCP,443:31134/TCP  1s

==> v1beta1/Deployment
NAME                    DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
bilging-ibex-mariadb    1        1        1           0          1s
bilging-ibex-wordpress  1        1        1           0          1s
...
```

## <a name="list-helm-releases"></a>Auflisten von Helm-Versionen

Verwenden Sie für die Anzeige einer Liste der im Cluster installierten Releases den Befehl [helm list][helm-list].

```azurecli-interactive
helm list
```

Ausgabe:

```
NAME            REVISION    UPDATED                     STATUS      CHART           NAMESPACE
bilging-ibex    1           Tue Jun  5 14:31:49 2018    DEPLOYED    wordpress-1.0.9 default
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten von Kubernetes-Diagrammen finden Sie in der Helm-Dokumentation.

> [!div class="nextstepaction"]
> [Helm-Dokumentation][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/helm/#helm-install
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
