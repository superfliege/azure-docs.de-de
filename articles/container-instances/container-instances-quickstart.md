---
title: 'Schnellstart: Erstellen Ihres ersten Azure Container Instances-Containers'
description: Bereitstellung und erste Schritte mit Azure Container Instances
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 15e6127d419bb41f1b146aff147c43dce2233d8d
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Erstellen Ihres ersten Containers in Azure Container Instances

Azure Container Instances erleichtert die Erstellung und Verwaltung von Docker-Containern in Azure, ohne dass Sie virtuelle Computer bereitstellen oder einen übergeordneten Dienst einführen müssen. In dieser Schnellstartanleitung erstellen Sie einen Container in Azure und machen ihn mit einer öffentlichen IP-Adresse über das Internet verfügbar. Dieser Vorgang wird mit einem einzelnen Befehl durchgeführt. Nach wenigen Sekunden wird in Ihrem Browser Folgendes angezeigt:

![Mit Azure Container Instances bereitgestellte App im Browser][aci-app-browser]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.12 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Azure Container Instances-Instanzen sind Azure-Ressourcen und müssen in einer Azure-Ressourcengruppe (eine logische Sammlung für die Bereitstellung und Verwaltung von Azure-Ressourcen) platziert werden.

Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Erstellen eines Containers

Zum Erstellen eines Containers müssen Sie einen Namen, ein Docker-Image und eine Azure-Ressourcengruppe für den Befehl [az container create][az-container-create] angeben. Der Container kann außerdem mit einer öffentlichen IP-Adresse über das Internet verfügbar gemacht werden, dies ist jedoch optional. In diesem Fall verwenden wir einen Container, der eine sehr einfache, in [Node.js](http://nodejs.org) geschriebene Web-App hostet.

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public
```

Innerhalb weniger Sekunden erhalten Sie eine Antwort auf Ihre Anforderung. Der Container befindet sich zunächst im Zustand **Wird erstellt...**, wird aber innerhalb weniger Sekunden gestartet. Sie können den Status mit dem Befehl [az container show][az-container-show] überprüfen:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

Am unteren Rand der Ausgabe werden der Bereitstellungsstatus und die IP-Adresse des Containers angezeigt:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Sobald sich der Container im Zustand **Erfolgreich** befindet, ist er über den Browser unter der angegebenen IP-Adresse erreichbar.

![Mit Azure Container Instances bereitgestellte App im Browser][aci-app-browser]

## <a name="pull-the-container-logs"></a>Herunterladen der Containerprotokolle

Die Protokolle für den erstellten Container können mithilfe des Befehls [az container logs][az-container-logs] abgerufen werden:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Ausgabe:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a>Löschen des Containers

Wenn Sie den Container nicht mehr benötigen, können Sie ihn mithilfe des Befehls [az container delete][az-container-delete] entfernen:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Den gesamten Code für den Container aus dieser Schnellstartanleitung sowie die Dockerfile-Datei finden Sie auf [GitHub][app-github-repo]. Wenn Sie ihn selbst erstellen und über Azure Container Registry in Azure Container Instances bereitstellen möchten, fahren Sie mit dem Azure Container Instances-Tutorial fort.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](./container-instances-tutorial-prepare-app.md)

Um Optionen zum Ausführen von Containern in einem Orchestrierungssystem in Azure zu testen, lesen Sie die Schnellstarts [Bereitstellen einer Service Fabric-Containeranwendung unter Windows in Azure][service-fabric] oder [Deploy an Azure Container Service (AKS) cluster][container-service] (Bereitstellen eines Azure Container Service-Clusters [AKS]).  

<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
[container-service]: ../aks/kubernetes-walkthrough.md


<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png