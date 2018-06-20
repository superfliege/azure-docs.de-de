---
title: Schnellstart – Schnellstart für Azure Kubernetes-Cluster im Portal
description: Hier lernen Sie schnell, wie Sie einen Kubernetes-Cluster für Linux-Container in AKS mit dem Azure-Portal erstellen.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: quickstart
ms.date: 04/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 8b9a9eddc6876451a75f0deae911d73b57198827
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34597279"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster"></a>Schnellstart: Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)

In dieser Schnellstartanleitung stellen Sie einen AKS-Cluster mit dem Azure-Portal bereit. Anschließend wird im Cluster eine Anwendung mit mehreren Containern ausgeführt, die ein Web-Front-End und eine Redis-Instanz umfasst. Nach Abschluss des Vorgangs kann auf die Anwendung über das Internet zugegriffen werden.

![Abbildung der Navigation zu Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

Für diese Schnellstartanleitung werden Grundkenntnisse in Bezug auf die Kubernetes-Konzepte vorausgesetzt. Ausführliche Informationen zu Kubernetes finden Sie in der [Kubernetes-Dokumentation][kubernetes-documentation].

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.



## <a name="create-aks-cluster"></a>Erstellen eines ACS-Clusters

Wählen Sie **Ressource erstellen**, suchen Sie nach **Kubernetes**, und wählen Sie dann **Kubernetes Service** > **Erstellen**.

Führen Sie unter jeder Überschrift des Formulars zur Erstellung des AKS-Clusters die folgenden Schritte aus.

- **PROJEKTDETAILS**: Wählen Sie ein Azure-Abonnement und eine neue oder vorhandene Azure-Ressourcengruppe aus.
- **CLUSTERDETAILS**: Geben Sie Namen, Region, Version und DNS-Namenspräfix für den AKS-Cluster ein.
- **AUTHENTIFIZIERUNG**: Erstellen Sie einen neuen Dienstprinzipal oder einen vorhandenen. Bei der Verwendung eines vorhandenen Dienstprinzipalnamens müssen Sie die SPN-Client-ID und das Geheimnis angeben.
- **SKALIERUNG**: Wählen Sie eine VM-Größe für die AKS-Knoten aus. Die VM-Größe kann **nicht** geändert werden, sobald ein AKS-Cluster bereitgestellt wurde. Wählen Sie außerdem die Anzahl der Knoten für die Bereitstellung im Cluster aus. Die Knotenanzahl **kann** nach der Clusterbereitstellung angepasst werden.

Wählen Sie danach **Weiter: Netzwerk** aus.

![Erstellen des ersten AKS-Clusters](media/container-service-walkthrough-portal/aks-portal-1.png)

Konfigurieren Sie die folgenden Netzwerkoptionen:

- **HTTP-Anwendungsrouting**: Konfiguriert einen integrierten Eingangscontroller mit automatischer Erstellung öffentlicher DNS-Namen. Weitere Informationen zum HTTP-Routing finden Sie unter [HTTP-Routing und DNS bei AKS][http-routing].
- **Netzwerkkonfiguration**: Wählen Sie die grundlegende Netzwerkkonfiguration mit dem [kubenet][kubenet]-Kubernetes-Plug-in oder die erweiterte Netzwerkkonfiguration mit [Azure CNI][azure-cni]. Weitere Informationen zu Netzwerkoptionen finden Sie unter [Netzwerkkonfiguration in Azure Kubernetes Service (AKS)][aks-network].

Wählen Sie danach **Weiter: Überwachung** aus.

![Erstellen des ersten AKS-Clusters](media/container-service-walkthrough-portal/aks-portal-2.png)

Bei der Bereitstellung eines AKS-Clusters können Einblicke in Azure-Container konfiguriert werden, um die Integrität des AKS-Cluster und im Cluster ausgeführte Pods zu überwachen. Weitere Informationen zur Überwachung der Integrität von Containern finden Sie unter [Überwachen der Integrität von Azure Kubernetes Service][aks-monitor].

Wählen Sie zum Aktivieren der Containerüberwachung **Ja** und dann einen vorhandenen Log Analytics-Arbeitsbereich aus, oder erstellen Sie einen neuen.

Wählen Sie **Überprüfen + erstellen** und danach **Erstellen**.

![Erstellen des ersten AKS-Clusters](media/container-service-walkthrough-portal/aks-portal-3.png)

Nach einer kurzen Wartezeit ist die Bereitstellung des AKS-Clusters abgeschlossen, und er kann verwendet werden. Navigieren Sie zur AKS-Clusterressourcengruppe, und wählen Sie die AKS-Ressource aus. Daraufhin sollte das AKS-Clusterdashboard angezeigt werden.

![Erstellen des ersten AKS-Clusters](media/container-service-walkthrough-portal/aks-portal-5.png)

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

Verwenden Sie zum Verwalten eines Kubernetes-Clusters den Kubernetes-Befehlszeilenclient [kubectl][kubectl]. Der Kubectl-Client ist in der Azure Cloud Shell vorinstalliert.

Öffnen Sie die Cloud Shell mit der Schaltfläche oben rechts im Azure-Portal.

![Cloud Shell](media/container-service-walkthrough-portal/kubectl-cs.png)

Verwenden Sie den Befehl [az aks get-credentials][az-aks-get-credentials], um kubectl für das Herstellen der Verbindung mit Ihrem Kubernetes-Cluster zu konfigurieren.

Kopieren Sie den folgenden Befehl, und fügen Sie ihn in die Cloud Shell ein. Ändern Sie den Ressourcengruppennamen und Clusternamen, falls dies erforderlich ist.

```azurecli-interactive
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

Verwenden Sie zum Überprüfen der Verbindung mit Ihrem Cluster den Befehl [kubectl get][kubectl-get], um eine Liste der Clusterknoten zu erhalten.

```azurecli-interactive
kubectl get nodes
```

Ausgabe:

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-agentpool-11482510-0   Ready     agent     9m        v1.9.6
aks-agentpool-11482510-1   Ready     agent     8m        v1.9.6
aks-agentpool-11482510-2   Ready     agent     9m        v1.9.6
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Kubernetes-Manifestdateien definieren einen gewünschten Zustand für einen Cluster (Desired State) – also beispielsweise, welche Containerimages ausgeführt werden sollen. In diesem Beispiel wird ein Manifest verwendet, um alle Objekte zu erstellen, die zum Ausführen der Azure Vote-Anwendung benötigt werden. Zu diesen Objekten gehören zwei [Kubernetes-Bereitstellungen][kubernetes-deployment]: eines für das Azure Vote-Front-End und eines für eine Redis-Instanz. Darüber hinaus werden zwei [Kubernetes-Dienste][kubernetes-service] erstellt: ein interner Dienst für die Redis-Instanz und ein externer Dienst für den Zugriff auf die Azure Vote-Anwendung über das Internet.

Erstellen Sie eine Datei mit dem Namen `azure-vote.yaml`, und fügen Sie den folgenden YAML-Code ein: Erstellen Sie die Datei bei Verwendung von Azure Cloud Shell mit vi oder Nano wie auf einem virtuellen oder physischen System.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Führen Sie die Anwendung mithilfe des Befehls [kubectl apply][kubectl-apply] aus.

```azurecli-interactive
kubectl apply -f azure-vote.yaml
```

Ausgabe:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Testen der Anwendung

Im Zuge der Anwendungsausführung wird ein [Kubernetes-Dienst][kubernetes-service] erstellt, der die Anwendung über das Internet verfügbar macht. Dieser Vorgang kann einige Minuten dauern.

Verwenden Sie zum Überwachen des Fortschritts den Befehl [kubectl get service][kubectl-get] mit dem Argument `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Der Wert *EXTENAL-IP* für den Dienst *azure-vote-front* wird zunächst als *ausstehend* angezeigt.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Sobald die *externe IP-Adresse* nicht mehr *ausstehend* ist, sondern eine *IP-Adresse* angezeigt wird, verwenden Sie `CTRL-C`, um die kubectl-Überwachung zu beenden.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Navigieren Sie nun zu der externen IP-Adresse, um die Azure Vote-App anzuzeigen.

![Abbildung der Navigation zu Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="monitor-health-and-logs"></a>Überwachung von Integrität und Protokollen

Wenn die Überwachung von Containerinformationen aktiviert wurde, werden Integritätsmetriken für den AKS-Cluster sowie die im Cluster ausgeführten Pods auf dem AKS-Clusterdashboard angezeigt. Weitere Informationen zur Überwachung der Integrität von Containern finden Sie unter [Überwachen der Integrität von Azure Kubernetes Service][aks-monitor].

Um den aktuellen Status, Betriebszeit und Ressourcenauslastung für die Azure Vote-Pods anzuzeigen, navigieren Sie zurück zur AKS-Ressource. Wählen Sie dann **Containerintegrität überwachen**, den Namespace **default** und schließlich **Container** aus. Es dauert möglicherweise einige Minuten, bis diese Daten im Azure-Portal aufgefüllt werden.

![Erstellen des ersten AKS-Clusters](media/container-service-walkthrough-portal/aks-portal-6.png)

Klicken Sie zum Anzeigen der Protokolle für den `azure-vote-front`-Pod auf den Link **Protokolle anzeigen**. Diese Protokolle enthalten die Datenströme „stdout“ und „stderr“ aus dem Container.

![Erstellen des ersten AKS-Clusters](media/container-service-walkthrough-portal/aks-portal-7.png)

## <a name="delete-cluster"></a>Löschen von Clustern

Wenn der Cluster nicht mehr benötigt wird, löschen Sie die Clusterressource. Bei diesem Vorgang werden alle zugeordneten Ressourcen gelöscht. Dieser Vorgang kann im Azure-Portal durchgeführt werden, indem Sie auf dem AKS-Clusterdashboard auf die Schaltfläche „Löschen“ klicken. Alternativ dazu kann der Befehl [az aks delete][az-aks-delete] in Cloud Shell genutzt werden.

```azurecli-interactive
az aks delete --resource-group myAKSCluster --name myAKSCluster --no-wait
```

## <a name="get-the-code"></a>Abrufen des Codes

In dieser Schnellstartanleitung wurden vorab erstellte Containerimages verwendet, um eine Kubernetes-Bereitstellung zu erstellen. Der dazugehörige Anwendungscode, die Dockerfile-Datei und die Kubernetes-Manifestdatei sind auf GitHub verfügbar.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Kubernetes-Cluster und eine Anwendung mit mehreren Containern dafür bereitgestellt.

Weitere Informationen zu Azure Container Service sowie ein vollständiges Beispiel vom Code bis zur Bereitstellung finden Sie im Kubernetes-Clustertutorial.

> [!div class="nextstepaction"]
> [AKS-Tutorial][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[kubernetes-deployment]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az_aks_get_credentials
[az-aks-delete]: /cli/azure/aks#az-aks-delete
[aks-monitor]: ../monitoring/monitoring-container-health.md
[aks-network]: ./networking-overview.md
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[http-routing]: ./http-application-routing.md
