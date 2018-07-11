---
title: Tutorial zu Kubernetes in Azure – Bereitstellen von Clustern
description: AKS-Tutorial – Bereitstellen eines Clusters
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c8698f16138e9baeb9c9c1142a5d0c8937a69d1b
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341398"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Tutorial: Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)

Kubernetes bietet eine verteilte Plattform für containerbasierte Anwendungen. Mit AKS können Sie schnell einen Kubernetes-Cluster bereitstellen, der für die Produktion bereit ist. In diesem Tutorial (Teil 3 von 7) wird ein Kubernetes-Cluster in AKS bereitgestellt. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Erstellen eines Dienstprinzipals für Ressourceninteraktionen
> * Bereitstellen eines Kubernetes-AKS-Clusters
> * Installation der Kubernetes-Befehlszeilenschnittstelle (kubectl)
> * Konfiguration von kubectl

In den nachfolgenden Tutorials wird die Anwendung Azure Vote im Cluster bereitgestellt, skaliert und aktualisiert.

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde ein Containerimage erstellt und in eine Azure Container Registry-Instanz hochgeladen. Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages][aks-tutorial-prepare-app] zurück.

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Damit ein AKS-Cluster mit anderen Azure-Ressourcen interagieren kann, wird ein Azure Active Directory-Dienstprinzipal verwendet. Dieser Dienstprinzipal kann automatisch über die Azure CLI oder das Azure-Portal erstellt werden, oder Sie können vorab einen Dienstprinzipal erstellen und dann weitere Berechtigungen zuweisen. In diesem Tutorial erstellen Sie einen Dienstprinzipal, gewähren Zugriff auf die ACR-Instanz (Azure Container Registry), die im vorherigen Tutorial erstellt wurde, und erstellen anschließend einen AKS-Cluster.

Erstellen Sie mit [az ad sp create-for-rbac][] einen Dienstprinzipal. Mit dem Parameter `--skip-assignment` wird die Zuweisung zusätzlicher Berechtigungen eingeschränkt.

```azurecli
az ad sp create-for-rbac --skip-assignment
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```
{
  "appId": "e7596ae3-6864-4cb8-94fc-20164b1588a9",
  "displayName": "azure-cli-2018-06-29-19-14-37",
  "name": "http://azure-cli-2018-06-29-19-14-37",
  "password": "52c95f25-bd1e-4314-bd31-d8112b293521",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Notieren Sie sich die App-ID (*appId*) und das Kennwort (*password*). Diese Werte werden in den folgenden Schritten verwendet.

## <a name="configure-acr-authentication"></a>Konfigurieren der ACR-Authentifizierung

Für den Zugriff auf Images, die in ACR gespeichert sind, müssen Sie dem AKS-Dienstprinzipal die richtigen Rechte gewähren, mit denen für Images der Pullvorgang aus ACR möglich ist.

Rufen Sie zuerst mit [az acr show][] die ACR-Ressourcen-ID ab. Aktualisieren Sie den Registrierungsnamen `<acrName>` auf den Namen Ihrer ACR-Instanz und der Ressourcengruppe, in der sich die ACR-Instanz befindet.

```azurecli
az acr show --name <acrName> --resource-group myResourceGroup --query "id" --output tsv
```

Erstellen Sie mit [az role assignment create][] eine Rollenzuweisung, um den richtigen Zugriff für den AKS-Cluster zu gewähren und die Nutzung der in ACR gespeicherten Images zu ermöglichen. Ersetzen Sie `<appId`> und `<acrId>` durch die Werte, die in den beiden vorherigen Schritten erfasst wurden.

```azurecli
az role assignment create --assignee <appId> --role Reader --scope <acrId>
```

## <a name="create-kubernetes-cluster"></a>Erstellen eines Kubernetes-Clusters

Erstellen Sie nun mit [az aks create][] einen AKS-Cluster. Im folgenden Beispiel wird ein Cluster mit dem Namen *myAKSCluster* in einer Ressourcengruppe mit dem Namen *myResourceGroup* erstellt. Diese Ressourcengruppe wurde im [vorherigen Tutorial][aks-tutorial-prepare-acr] erstellt. Geben Sie Ihre eigenen Angaben für `<appId>` und `<password>` aus dem vorherigen Schritt an, in dem Sie den Dienstprinzipal erstellt haben.

```azurecli
az aks create \
    --name myAKSCluster \
    --resource-group myResourceGroup \
    --node-count 1 \
    --generate-ssh-keys \
    --service-principal <appId> \
    --client-secret <password>
```

Nach einigen Minuten ist die Bereitstellung abgeschlossen, und es werden Informationen zur AKS-Bereitstellung im JSON-Format zurückgegeben.

## <a name="install-the-kubectl-cli"></a>Installieren der Befehlszeilenschnittstelle „kubectl“

Zum Herstellen der Verbindung mit dem Kubernetes-Cluster auf Ihrem Clientcomputer verwenden Sie den Kubernetes-Befehlszeilenclient [kubectl][kubectl].

Wenn Sie Azure Cloud Shell verwenden, ist „kubectl“ bereits installiert. Sie können auch [az aks install-cli][] verwenden, um die lokale Installation durchzuführen:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Verbinden mit kubectl

Mit [az aks get-credentials][] können Sie „kubectl“ für die Verbindungsherstellung mit Ihrem Kubernetes-Cluster konfigurieren. Im folgenden Beispiel werden Anmeldeinformationen für den AKS-Clusternamen *myAKSCluster* in *myResourceGroup* abgerufen:

```azurecli
az aks get-credentials --name myAKSCluster --resource-group myResourceGroup
```

Um die Verbindung mit dem Cluster zu überprüfen, führen Sie den Befehl [kubectl get nodes][kubectl-get] aus.

```azurecli
kubectl get nodes
```

Ausgabe:

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-66427764-0   Ready     agent     9m        v1.9.6
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde ein Kubernetes-Cluster in AKS bereitgestellt. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Erstellen eines Dienstprinzipals für Ressourceninteraktionen
> * Bereitstellen eines Kubernetes-AKS-Clusters
> * Installieren der Kubernetes-Befehlszeilenschnittstelle (kubectl)
> * Konfigurieren von kubectl

Fahren Sie mit dem nächsten Tutorial fort, um mehr über die Ausführung von Anwendungen im Cluster zu erfahren.

> [!div class="nextstepaction"]
> [Bereitstellen einer Anwendung in Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az ad sp create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az acr show]: /cli/azure/acr#az-acr-show
[az role assignment create]: /cli/azure/role/assignment#az-role-assignment-create
[az aks create]: /cli/azure/aks#az-aks-create
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
