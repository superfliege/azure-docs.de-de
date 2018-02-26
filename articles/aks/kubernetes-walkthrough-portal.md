---
title: "Schnellstart – Schnellstart für Azure Kubernetes-Cluster im Portal"
description: "Hier lernen Sie schnell, wie Sie einen Kubernetes-Cluster für Linux-Container in AKS mit dem Azure-Portal erstellen."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: quickstart
ms.date: 11/28/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c867f9ed7ede0dddf6b327b212adbbccbf3d64b0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Bereitstellen eines Azure Container Service-Clusters (AKS)

In dieser Schnellstartanleitung stellen Sie einen AKS-Cluster mit dem Azure-Portal bereit. Anschließend wird im Cluster eine Anwendung mit mehreren Containern ausgeführt, die ein Web-Front-End und eine Redis-Instanz umfasst. Nach Abschluss des Vorgangs kann auf die Anwendung über das Internet zugegriffen werden.

![Abbildung der Navigation zu Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

Für diese Schnellstartanleitung werden Grundkenntnisse in Bezug auf die Kubernetes-Konzepte vorausgesetzt. Ausführliche Informationen zu Kubernetes finden Sie in der [Kubernetes-Dokumentation][kubernetes-documentation].

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter „http://portal.azure.com“ am Azure-Portal an.

## <a name="create-service-principal"></a>Erstellen eines Dienstprinzipals

Vor dem Erstellen des AKS-Clusters im Azure-Portal müssen Sie einen Dienstprinzipal erstellen. In Azure wird dieser Dienstprinzipal verwendet, um die Infrastruktur zu verwalten, die dem AKS-Cluster zugeordnet ist.

Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Registrierung einer neuen Anwendung** aus.

Geben Sie einen beliebigen Namen für die Anwendung ein. Wählen Sie **Web-App/API** als Anwendungstyp aus. Geben Sie einen Wert für **Anmelde-URL** ein. Dies kann ein beliebiger Wert in einem gültigen URL-Format sein, aber es muss sich nicht um einen echten Endpunkt handeln.

Wenn Sie fertig sind, klicken Sie auf **Erstellen**.

![Erstellen des ersten Dienstprinzipals](media/container-service-walkthrough-portal/create-sp-one.png)

Wählen Sie die neu erstellte Anwendungsregistrierung aus, und notieren Sie sich die Anwendungs-ID. Sie benötigen diesen Wert beim Erstellen des AKS-Clusters.

![Erstellen des zweiten Dienstprinzipals](media/container-service-walkthrough-portal/create-sp-two.png)

Als Nächstes müssen Sie ein Kennwort für den Dienstprinzipal erstellen. Wählen Sie **Alle Einstellungen** > **Schlüssel**, und geben Sie einen beliebigen Wert für die Schlüsselbeschreibung ein. Wählen Sie eine Dauer als Gültigkeitszeitraum für den Dienstprinzipal aus.

Klicken Sie auf **Speichern**, und notieren Sie sich den Kennwortwert. Sie benötigen dieses Kennwort beim Erstellen eines AKS-Clusters.

![Erstellen des dritten Dienstprinzipals](media/container-service-walkthrough-portal/create-sp-three.png)

## <a name="create-aks-cluster"></a>Erstellen eines ACS-Clusters

Klicken Sie auf **Ressource erstellen** > **Container** > **Azure Container Service - AKS (preview)** (Azure Container Service – AKS (Vorschauversion)).

Geben Sie einen Clusternamen, ein DNS-Präfix, einen Ressourcengruppennamen, einen Standort und eine Kubernetes-Version für den Cluster an. Notieren Sie sich den Clusternamen und den Ressourcengruppennamen, da Sie diese beim Herstellen der Verbindung mit dem Cluster benötigen.

Wählen Sie **OK**, wenn Sie fertig sind.

![Erstellen des ersten AKS-Clusters](media/container-service-walkthrough-portal/create-aks-portal-one.png)

Geben Sie im Konfigurationsformular Folgendes ein:

- Benutzername: Dies ist der Name für die Administratorkonten auf den Clusterknoten.
- Öffentlicher SSH-Schlüssel: Wird dem Schlüssel zugeordnet, der zum Zugreifen auf die Clusterknoten verwendet wird.
- Client-ID des Dienstprinzipals: Die Anwendungs-ID des Dienstprinzipals, die Sie weiter oben in diesem Dokument erstellt haben.
- Geheimer Clientschlüssel des Dienstprinzipals: Das Kennwort des Dienstprinzipals, das Sie weiter oben in diesem Dokument erstellt haben.
- Anzahl von Knoten: Gibt an, wie viele AKS-Knoten erstellt werden sollen.
- VM-Größe des Knotens: Gibt die VM-Größe für die AKS-Knoten an.
- Betriebssystem-Datenträgergröße: Gibt die Größe für die AKS-Knoten des Betriebssystem-Datenträgers an.

Wählen Sie **OK**, wenn Sie fertig sind, und dann erneut **OK**, nachdem die Validierung abgeschlossen ist.

![Erstellen des zweiten AKS-Clusters](media/container-service-walkthrough-portal/create-aks-portal-two.png)

Nach einer kurzen Wartezeit ist die Bereitstellung des AKS-Clusters abgeschlossen, und er kann verwendet werden.

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
aks-agentpool-14693408-0   Ready     agent     6m        v1.8.1
aks-agentpool-14693408-1   Ready     agent     6m        v1.8.1
aks-agentpool-14693408-2   Ready     agent     7m        v1.8.1
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Eine Kubernetes-Manifestdatei definiert einen gewünschten Zustand (Desired State) – also beispielsweise, welche Containerimages ausgeführt werden sollen. In diesem Beispiel verwenden Sie ein Manifest, um alle Objekte zu erstellen, die zum Ausführen der Azure Vote-Anwendung benötigt werden.

Erstellen Sie eine Datei mit dem Namen `azure-vote.yaml`, und fügen Sie den folgenden YAML-Code ein: Wenn Sie in der Azure Cloud Shell arbeiten, können Sie die Datei mit vi oder Nano wie bei Verwendung eines virtuellen oder physischen Systems erstellen.

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

Führen Sie die Anwendung mithilfe des Befehls [kubectl create][kubectl-create] aus.

```azurecli-interactive
kubectl create -f azure-vote.yaml
```

Ausgabe:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Testen der Anwendung

Im Zuge der Anwendungsausführung wird ein [Kubernetes-Dienst][kubernetes-service] erstellt, der das Anwendungs-Front-End über das Internet verfügbar macht. Dieser Vorgang kann einige Minuten dauern.

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

Nun können Sie zu der externen IP-Adresse navigieren und die Azure Vote-App anzeigen.

![Abbildung der Navigation zu Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Löschen von Clustern

Wenn der Cluster nicht mehr benötigt wird, können Sie die Clusterressourcengruppe löschen. Bei diesem Vorgang werden alle zugeordneten Ressourcen gelöscht. Dies kann im Azure-Portal durchgeführt werden, indem Sie die Ressourcengruppe auswählen und auf die Schaltfläche „Löschen“ klicken. Alternativ dazu kann der Befehl [az group delete][az-group-delete] in Cloud Shell genutzt werden.

```azurecli-interactive
az group delete --name myAKSCluster --no-wait
```

## <a name="get-the-code"></a>Abrufen des Codes

In dieser Schnellstartanleitung wurden vorab erstellte Containerimages verwendet, um eine Kubernetes-Bereitstellung zu erstellen. Der dazugehörige Anwendungscode, die Dockerfile-Datei und die Kubernetes-Manifestdatei sind auf GitHub verfügbar.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Kubernetes-Cluster und eine Anwendung mit mehreren Containern dafür bereitgestellt.

Weitere Informationen zu Azure Container Service sowie ein vollständiges Beispiel vom Code bis zur Bereitstellung finden Sie im Kubernetes-Clustertutorial.

> [!div class="nextstepaction"]
> [Verwalten eines AKS-Clusters][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az_aks_get_credentials
[az-group-delete]: /cli/azure/group#delete
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md


