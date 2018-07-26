---
title: Verwalten von Azure-Kubernetes-Clustern mit der Webbenutzeroberfläche
description: Erfahren Sie mehr über die Verwendung des integrierten Kubernetes-Dashboards mit Webbenutzeroberfläche mit Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 65525114f46002c5b9300f6bbabcee06cc27ef3a
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091137"
---
# <a name="access-the-kubernetes-dashboard-with-azure-kubernetes-service-aks"></a>Zugriff auf das Kubernetes-Dashboard mit Azure Kubernetes Service (AKS)

Kubernetes enthält ein Webdashboard, das für einfache Verwaltungsvorgänge verwendet werden kann. In diesem Artikel wird erläutert, wie Sie mithilfe von Azure CLI auf das Kubernetes-Dashboard zugreifen können. Anschließend werden einige grundlegende Vorgänge im Dashboard vorgestellt. Weitere Informationen zum Kubernetes-Dashboard finden Sie unter [Kubernetes Web UI Dashboard (Kubernetes-Dashboard mit Webbenutzeroberfläche)][kubernetes-dashboard].

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine `kubectl`-Verbindung mit dem Cluster hergestellt haben. Wenn Sie einen AKS-Cluster erstellen müssen, finden Sie weitere Informationen unter [AKS-Schnellstart][aks-quickstart].

Außerdem muss mindestens Version 2.0.27 oder höher der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][install-azure-cli] Informationen dazu.

## <a name="start-kubernetes-dashboard"></a>Starten des Kubernetes-Dashboards

Verwenden Sie den Befehl [az aks browse][az-aks-browse], um das Kubernetes-Dashboard zu starten. Im folgenden Beispiel wird das Dashboard für den Cluster namens *myAKSCluster* in der Ressourcengruppe namens *myResourceGroup* geöffnet:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Der Befehl erstellt einen Proxy zwischen Ihrem Entwicklungssystem und der Kubernetes-API und öffnet einen Webbrowser mit dem Kubernetes-Dashboard.

### <a name="for-rbac-enabled-clusters"></a>Für RBAC-fähige Cluster

Wenn Ihr AKS-Cluster RBAC verwendet, muss ein *ClusterRoleBinding*-Element erstellt werden, bevor Sie ordnungsgemäß auf das Dashboard zugreifen können. Verwenden Sie wie im folgenden Beispiel dargestellt den Befehl [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding], um eine Bindung zu erstellen. 

> [!WARNING]
> Diese Beispielbindung wendet keine zusätzlichen Authentifizierungskomponenten an und kann daher zu unsicherer Verwendung führen. Das Kubernetes-Dashboard steht allen Personen zur Verfügung, die Zugriff auf die URL haben. Machen Sie das Kubernetes-Dashboard nicht öffentlich verfügbar.
>
> Sie können Mechanismen wie Bearer-Tokens oder einen Benutzernamen und ein Kennwort verwenden, um zu steuern, wer auf das Dashboard zugreifen kann und welche Berechtigungen die Benutzer haben. Dadurch wird die Verwendung des Dashboards sicherer. Weitere Informationen zur Verwendung der unterschiedlichen Authentifizierungsmethoden finden Sie in der Wiki des Kubernetes-Dashboards unter [Zugriffssteuerung][dashboard-authentication].

```console
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

Sie können nun in Ihrem RBAC-fähigen Cluster auf das Kubernetes-Dashboard zugreifen. Verwenden Sie wie zuvor beschrieben den Befehl [az aks browse][az-aks-browse], um das Kubernetes-Dashboard zu starten.

## <a name="run-an-application"></a>Ausführen einer Anwendung

Klicken Sie im Kubernetes-Dashboard im rechten oberen Fenster auf die Schaltfläche **Erstellen**. Nennen Sie die Bereitstellung `nginx`, und geben Sie `nginx:latest` als Containerimagenamen ein. Klicken Sie unter **Dienst** auf **Extern**, und geben Sie für Port und Zielport jeweils `80` ein.

Klicken Sie abschließend auf **Bereitstellen**, um die Bereitstellung zu erstellen.

![Kubernetes-Dialogfeld für die Diensterstellung](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>Anzeigen der Anwendung

Der Anwendungsstatus kann im Kubernetes-Dashboard überprüft werden. Sobald die Anwendung ausgeführt wird, wird neben den einzelnen Komponenten jeweils ein grünes Kontrollkästchen angezeigt.

![Kubernetes-Pods](./media/container-service-kubernetes-ui/complete-deployment.png)

Weitere Informationen zu den Anwendungspods erhalten Sie, indem Sie im linken Menü auf **Pods** klicken und den Pod **NGINX** auswählen. Hier stehen podspezifische Informationen (etwa zum Ressourcenverbrauch) zur Verfügung.

![Kubernetes-Ressourcen](./media/container-service-kubernetes-ui/running-pods.png)

Klicken Sie zum Ermitteln der IP-Adresse der Anwendung im linken Menü auf **Dienste**, und wählen Sie den Dienst **NGINX** aus.

![nginx-Ansicht](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>Bearbeiten der Anwendung

Über das Kubernetes-Dashboard können Sie nicht nur Anwendungen erstellen und anzeigen, sondern auch Anwendungsbereitstellungen bearbeiten und aktualisieren.

Wenn Sie eine Bereitstellung bearbeiten möchten, klicken Sie im linken Menü auf **Bereitstellungen**, und wählen Sie die Bereitstellung **NGINX** aus. Klicken Sie anschließend auf der Navigationsleiste rechts oben auf **Bearbeiten**.

![Kubernetes-Schaltfläche „Edit“](./media/container-service-kubernetes-ui/view-deployment.png)

Suchen Sie nach dem `spec.replica`-Wert (müsste auf „1“ festgelegt sein), und ändern Sie ihn in „3“. Dadurch erhöht sich die Replikatanzahl der NGINX-Bereitstellung von 1 auf 3.

Klicken Sie abschließend auf **Aktualisieren**.

![Kubernetes-Schaltfläche „Edit“](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Kubernetes-Dashboard finden Sie in der Kubernetes-Dokumentation.

> [!div class="nextstepaction"]
> [Kubernetes Web UI (Dashboard)][kubernetes-dashboard] (Kubernetes-Webbenutzeroberfläche – Dashboard)

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse
