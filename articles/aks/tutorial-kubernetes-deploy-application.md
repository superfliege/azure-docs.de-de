---
title: Tutorial zu Kubernetes in Azure – Bereitstellen von Anwendungen
description: AKS-Tutorial – Bereitstellen eine Anwendung
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e992d7ca455ad4d95d0f10a94c6c9ce8055f8286
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
---
# <a name="tutorial-run-applications-in-azure-kubernetes-service-aks"></a>Tutorial: Ausführen von Anwendungen in Azure Kubernetes Service (AKS)

In diesem Tutorial – Teil 4 von 8 – wird eine Beispielanwendung in einem Kubernetes-Cluster bereitgestellt. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Aktualisieren von Kubernetes-Manifestdateien
> * Ausführen einer Anwendung in Kubernetes
> * Testen der Anwendung

In den nachfolgenden Tutorials wird diese Anwendung horizontal hochskaliert. Zudem wird Log Analytics für die Überwachung des Kubernetes-Clusters konfiguriert.

Dieses Tutorial setzt voraus, dass Sie grundlegend mit den Konzepten von Kubernetes vertraut sind. Ausführliche Informationen zu Kubernetes finden Sie in der [Kubernetes-Dokumentation][kubernetes-documentation].

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, das Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt.

Für dieses Tutorial benötigen Sie die vorab erstellte Kubernetes-Manifestdatei `azure-vote-all-in-one-redis.yaml`. Diese Datei wurde in einem vorherigen Tutorial mit dem Anwendungsquellcode heruntergeladen. Stellen Sie sicher, dass Sie das Repository geklont und in das geklonte Repository gewechselt haben.

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages][aks-tutorial-prepare-app] zurück.

## <a name="update-manifest-file"></a>Aktualisieren der Manifestdatei

In diesem Tutorial wurde Azure Container Registry (ACR) zum Speichern eines Containerimages verwendet. Vor dem Ausführen der Anwendung muss der ACR-Anmeldeservername in der Kubernetes-Manifestdatei aktualisiert werden.

Rufen Sie den ACR-Anmeldeservernamen mit dem [az acr list][az-acr-list]-Befehl auf.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Die Manifestdatei wurde vorab mit dem Serveranmeldenamen `microsoft` erstellt. Öffnen Sie die Datei mit einem Text-Editor. In diesem Beispiel wird die Datei mit `nano` geöffnet.

```console
nano azure-vote-all-in-one-redis.yaml
```

Ersetzen Sie `microsoft` durch den ACR-Anmeldeservernamen. Dieser Wert befindet sich in Zeile **47** der Manifestdatei.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Der obige Code wird dann zu:

```yaml
containers:
- name: azure-vote-front
  image: <acrName>.azurecr.io/azure-vote-front:v1
```

Speichern und schließen Sie die Datei.

## <a name="deploy-application"></a>Bereitstellen der Anwendung

Führen Sie die Anwendung mithilfe des Befehls [kubectl apply][kubectl-apply] aus. Dieser Befehl analysiert die Manifestdatei und erstellt die definierten Kubernetes-Objekte.

```azurecli
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Ausgabe:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Testen der Anwendung

Es wird ein [Kubernetes-Dienst][kubernetes-service] erstellt, der die Anwendung für das Internet verfügbar macht. Dieser Vorgang kann einige Minuten dauern.

Verwenden Sie zum Überwachen des Fortschritts den Befehl [kubectl get service][kubectl-get] mit dem Argument `--watch`.

```azurecli
kubectl get service azure-vote-front --watch
```

Die *externe IP-Adresse* für den Dienst *azure-vote-front* wird zunächst als *ausstehend* angezeigt.

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Sobald die *externe IP-Adresse* nicht mehr *ausstehend* ist, sondern eine *IP-Adresse* angezeigt wird, verwenden Sie `CTRL-C`, um die kubectl-Überwachung zu beenden.

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Um die Anwendung anzuzeigen, navigieren Sie zu der externen IP-Adresse.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

Wurde die Anwendung nicht geladen, liegt möglicherweise ein Autorisierungsproblem mit Ihrer Imageregistrierung vor.

Führen Sie [diese Schritte](https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks#access-with-kubernetes-secret) aus, um den Zugriff über ein Kubernetes-Geheimnis zuzulassen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde die Azure Vote-Anwendung in einem Kubernetes-Cluster in AKS bereitgestellt. Folgende Aufgaben wurden ausgeführt:

> [!div class="checklist"]
> * Herunterladen von Kubernetes-Manifestdateien
> * Ausführen der Anwendung in Kubernetes
> * Testen der Anwendung

Wechseln Sie zum nächsten Tutorial, wo Sie erfahren, wie Sie eine Kubernetes-Anwendung und die zugrunde liegende Kubernetes-Infrastruktur skalieren.

> [!div class="nextstepaction"]
> [Scale Kubernetes application and infrastructure][aks-tutorial-scale] (Skalieren einer Kubernetes-Anwendung und -Infrastruktur)

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list
