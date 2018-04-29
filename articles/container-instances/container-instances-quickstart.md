---
title: 'Schnellstart: Erstellen Ihres ersten Azure Container Instances-Containers'
description: In dieser Schnellstartanleitung stellen Sie mithilfe der Azure-Befehlszeilenschnittstelle einen Windows-Container in Azure Container Instances bereit.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/19/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 57961cf1cb64f90cec7d2be90f3fbfe33344467d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Schnellstartanleitung: Erstellen Ihres ersten Containers in Azure Container Instances

Azure Container Instances erleichtert die Erstellung und Verwaltung von Docker-Containern in Azure, ohne dass Sie virtuelle Computer bereitstellen oder einen übergeordneten Dienst einführen müssen. In dieser Schnellstartanleitung erstellen Sie einen Container in Azure und machen ihn mit einem vollqualifizierten Domänennamen (FQDN) über das Internet verfügbar. Dieser Vorgang wird mit einem einzelnen Befehl durchgeführt. Nach wenigen Sekunden wird in Ihrem Browser Folgendes angezeigt:

![Mit Azure Container Instances bereitgestellte App im Browser][aci-app-browser]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-account] erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diesen Schnellstart verwenden. Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.27 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0][azure-cli-install].

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Wie alle Azure-Ressourcen müssen auch Azure-Containerinstanzen in einer Ressourcengruppe (eine logische Sammlung für die Bereitstellung und Verwaltung von Azure-Ressourcen) platziert werden.

Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Erstellen eines Containers

Zum Erstellen eines Containers müssen Sie einen Namen, ein Docker-Image und eine Azure-Ressourcengruppe für den Befehl [az container create][az-container-create] angeben. Durch Angeben einer DNS-Namensbezeichnung kann der Container optional auch für das Internet verfügbar gemacht werden. In dieser Schnellstartanleitung stellen Sie einen Container bereit, der eine kleine, in [Node.js][node-js] geschriebene Web-App hostet.

Führen Sie den folgenden Befehl aus, um eine Containerinstanz zu starten. Der Wert `--dns-name-label` muss innerhalb der Azure-Region, in der Sie die Instanz erstellen, eindeutig sein. Passen Sie den Wert also ggf. entsprechend an.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

Innerhalb weniger Sekunden erhalten Sie eine Antwort auf Ihre Anforderung. Der Container befindet sich zunächst im Zustand **Wird erstellt...**, wird aber innerhalb weniger Sekunden gestartet. Sie können den Status mit dem Befehl [az container show][az-container-show] überprüfen:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Wenn Sie den Befehl ausführen, werden der vollständig qualifizierte Domänenname (Fully Qualified Domain Name, FQDN) des Containers und der Bereitstellungsstatus angezeigt:

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Wenn der Container den Status **Erfolgreich** hat, ist er über den Browser unter seinem FQDN erreichbar:

![Screenshot eines Browsers mit ausgeführter Anwendung in einer Azure-Containerinstanz][aci-app-browser]

## <a name="pull-the-container-logs"></a>Herunterladen der Containerprotokolle

Die Protokolle für den erstellten Container können mithilfe des Befehls [az container logs][az-container-logs] abgerufen werden:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Die Ausgabe sollte in etwa wie im folgenden Beispiel aussehen:

```console
$ az container logs --resource-group myResourceGroup -n mycontainer
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="attach-output-streams"></a>Anfügen von Ausgabestreams

Zusätzlich zum Anfügen der Protokolle können Sie Ihre lokalen Standardausgabestreams und Standardfehlerstreams an die Streams des Containers anfügen.

Führen Sie zunächst den Befehl [az container attach][az-container-attach] aus, um Ihrer lokalen Konsole die Ausgabestreams des Containers anzufügen:

```azurecli-interactive
az container attach --resource-group myResourceGroup -n mycontainer
```

Aktualisieren Sie nach dem Anfügen mehrmals Ihren Browser, um zusätzliche Ausgaben zu generieren. Trennen Sie schließlich Ihre Konsole mit `Control+C` wieder. Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

```console
$ az container attach --resource-group myResourceGroup -n mycontainer
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-15 21:17:59+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Created container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45
(count: 1) (last timestamp: 2018-03-15 21:18:06+00:00) Started container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45

Start streaming logs:
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:44 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:47 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="delete-the-container"></a>Löschen des Containers

Wenn Sie den Container nicht mehr benötigen, können Sie ihn mithilfe des Befehls [az container delete][az-container-delete] entfernen:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Um sicherzustellen, dass der Container gelöscht wurde, führen Sie den Befehl [az container list](/cli/azure/container#az_container_list) aus:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

Der Container **mycontainer** sollte nicht in der Ausgabe des Befehls angezeigt werden. Wenn die Ressourcengruppe keine anderen Container enthält, wird keine Ausgabe angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Den gesamten Code für den Container aus dieser Schnellstartanleitung sowie die Dockerfile-Datei finden Sie auf [GitHub][app-github-repo]. Wenn Sie ihn selbst erstellen und über Azure Container Registry in Azure Container Instances bereitstellen möchten, fahren Sie mit dem Azure Container Instances-Tutorial fort.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](./container-instances-tutorial-prepare-app.md)

Um Optionen zum Ausführen von Containern in einem Orchestrierungssystem in Azure zu testen, lesen Sie die Schnellstarts [Bereitstellen einer Service Fabric-Containeranwendung unter Windows in Azure][service-fabric] oder [Deploy an Azure Container Service (AKS) cluster][container-service] (Bereitstellen eines Azure Container Service-Clusters [AKS]).

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az_container_attach
[az-container-create]: /cli/azure/container#az_container_create
[az-container-delete]: /cli/azure/container#az_container_delete
[az-container-list]: /cli/azure/container#az_container_list
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
