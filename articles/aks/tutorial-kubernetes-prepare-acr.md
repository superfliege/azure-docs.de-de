---
title: Tutorial zu Kubernetes in Azure – Vorbereiten eines ACR
description: AKS-Tutorial – Vorbereiten eines ACR
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e72ec06f7c682e68ca94ba5d4044d1d9974b0b24
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100972"
---
# <a name="tutorial-deploy-and-use-azure-container-registry"></a>Tutorial: Bereitstellen und Verwenden von Azure Container Registry

Azure Container Registry (ACR) ist eine Azure-basierte, private Registrierung für Docker-Containerimages. In diesem Tutorial – Teil 2 von 7 – werden die Schritte für die Bereitstellung einer Azure Container Registry-Instanz und die Übertragung von Containerimages in diese Instanz per Push erläutert. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen einer Azure Container Registry-Instanz
> * Kennzeichnen eines Containerimages für ACR
> * Hochladen des Images in die ACR-Instanz

In den nachfolgenden Tutorials wird diese ACR-Instanz in einen Kubernetes-Cluster in AKS integriert.

## <a name="before-you-begin"></a>Voraussetzungen

Im [vorherigen Tutorial][aks-tutorial-prepare-app] wurde ein Containerimage für eine einfache Azure Voting-App erstellt. Wenn Sie das Image der Azure Voting-App noch nicht erstellt haben, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages][aks-tutorial-prepare-app] zurück.

Für dieses Tutorial benötigen Sie mindestens Version 2.0.27 der Azure-Befehlszeilenschnittstelle. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install] Informationen dazu.

## <a name="deploy-azure-container-registry"></a>Bereitstellen von Azure Container Registry

Für die Bereitstellung einer Azure Container Registry-Instanz benötigen Sie zunächst eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der Region `eastus` erstellt.

```azurecli
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit dem Befehl [az acr create][az-acr-create] eine Azure Container Registry-Instanz. Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Für den Rest dieses Tutorials verwenden wir `<acrName>` als Platzhalter für den Namen der Containerregistrierung.

## <a name="container-registry-login"></a>Anmeldung bei der Containerregistrierung

Verwenden Sie den Befehl [az acr login][az-acr-login], um sich bei der ACR-Instanz anzumelden. Sie müssen den eindeutigen Namen angeben, mit dem die Containerregistrierung bei ihrer Erstellung versehen wurde.

```azurecli
az acr login --name <acrName>
```

Nach Abschluss des Vorgangs wird eine Erfolgsmeldung zurückgegeben.

## <a name="tag-container-images"></a>Kennzeichnen von Containerimages

Verwenden Sie den Befehl [docker images][docker-images], um eine Liste der aktuellen Images anzuzeigen.

```console
docker images
```

Ausgabe:

```
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Jedes Containerimage muss mit dem loginServer-Namen der Registrierung gekennzeichnet werden. Dieses Tag wird beim Übertragen von Containerimages per Push in eine Imageregistrierung für das Routing verwendet.

Führen Sie den Befehl [az acr list][az-acr-list] aus, um den loginServer-Namen abzurufen.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Kennzeichnen Sie nun das Image `azure-vote-front` mit dem loginServer-Namen der Containerregistrierung. Fügen Sie zudem `:v1` am Ende des Imagenamens hinzu. Dieses Tag gibt die Imageversion an.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Führen Sie nach der Kennzeichnung den Befehl [docker images][docker-images] aus, um den Vorgang zu überprüfen.

```console
docker images
```

Ausgabe:

```
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Übertragen von Images in die Registrierung per Push

Übertragen Sie das `azure-vote-front`-Image per Push in Ihre Registrierung.

Verwenden Sie das folgende Beispiel, und ersetzen Sie den ACR-loginServer-Namen durch den loginServer-Namen aus Ihrer Umgebung.

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

Dieser Vorgang nimmt einige Minuten in Anspruch.

## <a name="list-images-in-registry"></a>Auflisten von Images in der Registrierung

Führen Sie den Befehl [az acr repository list][az-acr-repository-list] aus, um eine Liste der Images zurückzugeben, die per Push in Ihre Azure Container Registry-Instanz übertragen wurden. Aktualisieren Sie den Befehl mit dem Namen der ACR-Instanz.

```azurecli
az acr repository list --name <acrName> --output table
```

Ausgabe:

```azurecli
Result
----------------
azure-vote-front
```

Verwenden Sie dann den Befehl [az acr repository show-tags][az-acr-repository-show-tags], um die Tags für ein bestimmtes Image anzuzeigen.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Ausgabe:

```azurecli
Result
--------
v1
```

Nach Abschluss des Tutorials ist das Containerimage in einer privaten Azure Container Registry-Instanz gespeichert. Dieses Image wird in den nachfolgenden Tutorials aus ACR in einem Kubernetes-Cluster bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine Azure Container Registry-Instanz für die Verwendung in einem AKS-Cluster vorbereitet. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Bereitstellen einer Azure Container Registry-Instanz
> * Kennzeichnen eines Containerimages für ACR
> * Hochladen des Images in die ACR-Instanz

Im nächsten Tutorial erfahren Sie, wie Sie einen Kubernetes-Cluster in Azure bereitstellen.

> [!div class="nextstepaction"]
> [Bereitstellen von Kubernetes-Clustern][aks-tutorial-deploy-cluster]

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#create
[az-acr-list]: /cli/azure/acr#list
[az-acr-login]: https://docs.microsoft.com/cli/azure/acr#az_acr_login
[az-acr-list]: https://docs.microsoft.com/cli/azure/acr#az_acr_list
[az-acr-repository-list]: /cli/azure/acr/repository#list
[az-acr-repository-show-tags]: /cli/azure/acr/repository#show-tags
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-tutorial-deploy-cluster]: ./tutorial-kubernetes-deploy-cluster.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
