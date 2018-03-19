---
title: 'Schnellstartanleitung: Erstellen einer privaten Docker-Registrierung in Azure mit der Azure CLI'
description: Hier lernen Sie, wie Sie schnell eine private Docker-Containerregistrierung mit der Azure CLI erstellen.
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: db1fb3deec4b70a9341753a59910aeb0e002bca0
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Erstellen einer Containerregistrierung mit der Azure-Befehlszeilenschnittstelle

Azure Container Registry ist ein verwalteter Docker-Containerregistrierungsdienst zum Speichern privater Docker-Containerimages. Diese Anleitung enthält ausführliche Informationen zum Erstellen einer Azure Container Registry-Instanz mit der Azure CLI, Übertragen eines Containerimages per Pushvorgang in die Registrierung und Bereitstellen des Containers aus Ihrer Registrierung in Azure Container Instances (ACI).

Für diese Schnellstartanleitung müssen Sie mindestens Version 2.0.27 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0][azure-cli].

Darüber hinaus muss Docker lokal installiert sein. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac-][docker-mac], [Windows-][docker-windows] oder [Linux-][docker-linux]System konfiguriert werden kann.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

In dieser Schnellstartanleitung erstellen Sie eine *Basic*-Registrierung. Azure Container Registry steht in mehreren unterschiedlichen SKUs zur Verfügung, die in der folgenden Tabelle kurz beschrieben werden. Ausführliche Details zu den einzelnen Einträgen finden Sie unter [Containerregistrierungs-SKUs][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Erstellen Sie mithilfe des Befehls [az acr create][az-acr-create] eine ACR-Instanz.

Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Im folgenden Beispiel wird der Name *myContainerRegistry007* verwendet. Ersetzen Sie diesen Namen durch einen eindeutigen Wert.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Wenn die Registrierung erstellt wird, sieht die Ausgabe etwa wie folgt aus:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Für den restlichen Teil dieser Schnellstartanleitung wird `<acrName>` als Platzhalter für den Namen der Containerregistrierung verwendet.

## <a name="log-in-to-acr"></a>Anmelden bei ACR

Bevor Sie Push- und Pullvorgänge für Containerimages ausführen können, müssen Sie sich bei der ACR-Instanz anmelden. Verwenden Sie hierzu den Befehl [az acr login][az-acr-login].

```azurecli
az acr login --name <acrName>
```

Der Befehl gibt nach Abschluss die Meldung `Login Succeeded` zurück.

## <a name="push-image-to-acr"></a>Übertragen eines Images an ACR mithilfe von Push

Um ein Image mithilfe von Push an Ihre Azure Container Registry-Instanz übertragen zu können, benötigen Sie zunächst ein Image. Wenn Sie über keine lokalen Containerimages verfügen, führen Sie den folgenden Befehl aus, um ein vorhandenes Image aus dem Docker-Hub abzurufen.

```bash
docker pull microsoft/aci-helloworld
```

Bevor Sie ein Image mithilfe von Push in Ihre Registrierung übertragen können, müssen Sie es mit dem vollqualifizierten Namen Ihres ACR-Anmeldeservers markieren. Führen Sie den folgenden Befehl aus, um den vollständigen Anmeldeservernamen der ACR-Instanz abzurufen.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Markieren Sie das Image mithilfe des Befehls [docker tag][docker-tag]. Ersetzen Sie `<acrLoginServer>` durch den Anmeldeservernamen Ihrer ACR-Instanz.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Nun können Sie das Image mit [docker push][docker-push] mithilfe von Push an die ACR-Instanz übertragen. Ersetzen Sie `<acrLoginServer>` durch den Anmeldeservernamen Ihrer ACR-Instanz.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Auflisten von Containerimages

Das folgende Beispiel listet die Repositorys in einer Registrierung auf:

```azurecli
az acr repository list --name <acrName> --output table
```

Ausgabe:

```bash
Result
----------------
aci-helloworld
```

Das folgende Beispiel listet die Tags des Repositorys **aci-helloworld** auf:

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Ausgabe:

```bash
Result
--------
v1
```

## <a name="deploy-image-to-aci"></a>Bereitstellen eines Images für ACI

Um eine Containerinstanz aus der von Ihnen erstellten Registrierung bereitzustellen, müssen Sie bei der Bereitstellung die Anmeldeinformationen für die Registrierung angeben. In Produktionsszenarien sollten Sie einen [Dienstprinzipal][container-registry-auth-aci] für den Zugriff auf die Containerregistrierung verwenden. Aktivieren Sie der Einfachheit halber in dieser Schnellstartanleitung den Administratorbenutzer in Ihrer Registrierung aber mit dem folgenden Befehl:

```azurecli
az acr update --name <acrName> --admin-enabled true
```

Nachdem der Administrator aktiviert wurde, ist der Benutzername mit Ihrem Registrierungsnamen identisch, und Sie können das Kennwort mit dem folgenden Befehl abrufen:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Führen Sie den folgenden Befehl aus, um Ihr Containerimage mit einem CPU-Kern und 1 GB Arbeitsspeicher bereitzustellen. Ersetzen Sie `<acrName>`, `<acrLoginServer>` und `<acrPassword>` durch die Werte, die Sie mit den obigen Befehlen abgerufen haben.

```azurecli
az container create --resource-group myResourceGroup --name acr-quickstart --image <acrLoginServer>/aci-helloworld:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Sie sollten von Azure Resource Manager eine erste Antwort mit Details zu Ihrem Container erhalten. Wiederholen Sie den Befehl [az container show][az-container-show], um den Status Ihres Containers zu überwachen und seine Ausführung zu verfolgen. Dieser Vorgang sollte weniger als eine Minute dauern.

```azurecli
az container show --resource-group myResourceGroup --name acr-quickstart --query instanceView.state
```

## <a name="view-the-application"></a>Anzeigen der Anwendung

Nachdem die Bereitstellung für ACI erfolgreich abgeschlossen wurde, können Sie den FQDN des Containers mit dem Befehl [az container show][az-container-show] abrufen:

```azurecli
az container show --resource-group myResourceGroup --name acr-quickstart --query ipAddress.fqdn
```

Beispielausgabe: `"aci-demo.eastus.azurecontainer.io"`

Zum Anzeigen der ausgeführten Anwendung navigieren Sie in Ihrem bevorzugten Browser zur öffentlichen IP-Adresse.

![„Hello World“-Anwendung im Browser][aci-app-browser]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe, die ACR-Instanz und alle Containerimages nicht mehr benötigen, können Sie sie mit dem Befehl [az group delete][az-group-delete] entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure Container Registry-Instanz mit der Azure CLI erstellt, ein Containerimage per Pushvorgang in die Registrierung übertragen und eine Instanz davon über Azure Container Instances gestartet. Fahren Sie mit dem Azure Container Instances-Tutorial fort, um eingehendere Informationen zu ACI zu erhalten.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial][container-instances-tutorial-prepare-app]

<!-- IMAGES> -->
[aci-app-browser]: ../container-instances/media/container-instances-quickstart/aci-app-browser.png


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-group-create]: /cli/azure/group#az_group_create
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli]: /cli/azure/install-azure-cli
[az-container-show]: /cli/azure/container#az_container_show
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
[container-registry-auth-aci]: container-registry-auth-aci.md