---
title: Erstellen eines einfachen Eingangscontrollers in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie einen einfachen NGINX-Eingangscontroller in einem Azure Kubernetes Service-Cluster (AKS) installieren und konfigurieren.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 3ae7a3193e0a4bacc64524f477b6c179ead20b6b
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44355120"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Erstellen eines Eingangscontrollers in Azure Kubernetes Service (AKS)

Ein Eingangscontroller ist eine Softwarekomponente, die einen Reverseproxy, konfigurierbare Datenverkehrsweiterleitung und TLS-Terminierung für Kubernetes-Dienste bereitstellt. Mithilfe von Ressourcen für eingehende Kubernetes-Daten werden Eingangsregeln und Routen für einzelne Kubernetes-Dienste konfiguriert. Durch die Verwendung von einem Eingangscontroller und Eingangsregeln kann eine einzelne IP-Adresse zum Weiterleiten von Datenverkehr an mehrere Dienste in einem Kubernetes-Cluster verwendet werden.

Dieser Artikel beschreibt, wie Sie den [NGINX-Eingangscontroller][nginx-ingress] in einem Azure Kubernetes Service-Cluster (AKS) bereitstellen. Es werden zwei Anwendungen im AKS-Cluster ausgeführt, die jeweils über eine einzelne IP-Adresse zugänglich sind.

Weitere Funktionen:

- [Aktivieren des Add-Ons für das HTTP-Anwendungsrouting][aks-http-app-routing]
- [Erstellen eines Eingangscontrollers, der ein internes, privates Netzwerk und eine IP-Adresse verwendet][aks-ingress-internal]
- [Erstellen eines Eingangscontrollers mit einer dynamischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-tls]
- [Erstellen eines Eingangscontrollers mit einer statischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird Helm verwendet, um den NGINX-Eingangscontroller, cert-manager und eine Beispiel-Web-App zu installieren. Helm muss im AKS-Cluster initialisiert sein und ein Dienstkonto für Tiller verwenden. Weitere Informationen zum Konfigurieren und Verwenden von Helm finden Sie unter [Installieren von Anwendungen mit Helm in Azure Kubernetes Service (AKS)][use-helm].

Für den Artikel müssen Sie mindestens Version 2.0.41 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install] Informationen dazu.

## <a name="create-an-ingress-controller"></a>Erstellen eines Eingangscontrollers

Verwenden Sie zum Erstellen des Eingangscontrollers `Helm` zum Installieren von *nginx-ingress*.

> [!TIP]
> Im folgenden Beispiel wird der Eingangscontroller im `kube-system`-Namespace installiert. Sie können bei Bedarf einen anderen Namespace für Ihre eigene Umgebung angeben. Wenn in Ihrem AKS-Cluster RBAC nicht aktiviert ist, fügen Sie den Befehlen `--set rbac.create=false` hinzu.

```console
helm install stable/nginx-ingress --namespace kube-system
```

Wird der Kubernetes-Lastenausgleichsdienst für den NGINX-Eingangscontroller erstellt, wird eine dynamische IP-Adresse zugewiesen, wie in der folgenden Beispielausgabe gezeigt:

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                         TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
masked-otter-nginx-ingress-controller        LoadBalancer   10.0.92.99    40.117.74.8   80:31077/TCP,443:32592/TCP   7m
masked-otter-nginx-ingress-default-backend   ClusterIP      10.0.46.106   <none>        80/TCP                       7m
```

Es wurden noch keine Eingangsregeln erstellt, sodass die Standard-404-Seite des NGINX-Eingangscontrollers angezeigt wird, wenn Sie zur internen IP-Adresse navigieren. Eingangsregeln werden in den folgenden Schritten konfiguriert.

## <a name="run-demo-applications"></a>Ausführen von Demoanwendungen

Um den Eingangscontroller in Aktion zu sehen, führen wir zwei Demoanwendungen im AKS-Cluster aus. In diesem Beispiel wird Helm verwendet, um mehrere Instanzen einer einfachen Hallo-Welt-Anwendung auszuführen.

Bevor Sie Helm-Beispieldiagramme installieren können, fügen Sie das Azure-Beispielrepository wie folgt Ihrer Helm-Umgebung hinzu:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Erstellen Sie die erste Demoanwendung aus einem Helm-Diagramm mit dem folgenden Befehl:

```console
helm install azure-samples/aks-helloworld
```

Installieren Sie nun eine zweite Instanz der Demoanwendung. Geben Sie für die zweite Instanz einen neuen Titel an, sodass sich die beiden Anwendungen visuell unterscheiden. Sie geben auch einen eindeutigen Dienstnamen an:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Erstellen einer Eingangsroute

Beide Anwendungen werden jetzt in Ihrem Kubernetes-Cluster ausgeführt. Zum Weiterleiten von Datenverkehr an die einzelnen Anwendungen erstellen Sie eine Kubernetes-Eingangsressource. Die Eingangsressource konfiguriert Regeln, die den Datenverkehr an eine der beiden Anwendungen weiterleiten.

Im folgenden Beispiel wird der Datenverkehr an die Adresse `http://40.117.74.8/` an den Dienst mit dem Namen `aks-helloworld` weitergeleitet. Datenverkehr an die Adresse `http://40.117.74.8/hello-world-two` wird an den Dienst `ingress-demo` weitergeleitet.

Erstellen Sie eine Datei mit dem Namen `hello-world-ingress.yaml`, und fügen Sie den folgenden YAML-Beispielcode ein.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Erstellen Sie die Eingangsressource mit dem Befehl `kubectl apply -f hello-world-ingress.yaml`.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Testen des Eingangscontrollers

Navigieren Sie zu den beiden Anwendungen, um die Routen für den Eingangscontroller zu testen. Öffnen Sie in einem Webbrowser die IP-Adresse des NGINX-Eingangscontrollers, z.B. *http://40.117.74.8*. Die erste Demoanwendung wird im Webbrowser angezeigt, wie im folgenden Beispiel gezeigt:

![Erste App, die hinter dem Eingangscontroller ausgeführt wird](media/ingress-basic/app-one.png)

Fügen Sie nun den Pfad */hello-world-two* der IP-Adresse hinzu, z.B. *http://40.117.74.8/hello-world-two*. Die zweite Demoanwendung mit dem benutzerdefinierten Titel wird angezeigt:

![Zweite App, die hinter dem Eingangscontroller ausgeführt wird](media/ingress-basic/app-two.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden einige externe Komponenten in AKS berücksichtigt. Weitere Informationen zu diesen Komponenten finden Sie auf den folgenden Projektseiten:

- [Helm CLI][helm-cli]
- [NGINX-Eingangscontroller][nginx-ingress]

Weitere Funktionen:

- [Aktivieren des Add-Ons für das HTTP-Anwendungsrouting][aks-http-app-routing]
- [Erstellen eines Eingangscontrollers, der ein internes, privates Netzwerk und eine IP-Adresse verwendet][aks-ingress-internal]
- [Erstellen eines Eingangscontrollers mit einer dynamischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-tls]
- [Erstellen eines Eingangscontrollers mit einer statischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
