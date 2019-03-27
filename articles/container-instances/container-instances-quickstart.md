---
title: 'Schnellstart: Bereitstellen von Docker-Containern in Azure Container Instances – Befehlszeilenschnittstelle'
description: In diesem Schnellstart verwenden Sie die Azure-Befehlszeilenschnittstelle, um schnell eine containerbasierte Web-App bereitzustellen, die in einer isolierten Azure-Containerinstanz ausgeführt wird.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 7252636287d634927979d70954f48cab5aecde5d
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2019
ms.locfileid: "57732275"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-cli"></a>Schnellstart: Bereitstellen einer Containerinstanz in Azure mithilfe der Azure-Befehlszeilenschnittstelle

Führen Sie mithilfe von Azure Container Instances serverlose Docker-Container schnell und einfach in Azure aus. Sie stellen eine Anwendung bedarfsgesteuert in einer Containerinstanz bereit, wenn Sie keine vollständige Containerorchestrierungsplattform wie Azure Kubernetes Service benötigen.

In diesem Schnellstart stellen Sie mithilfe der Azure-Befehlszeilenschnittstelle einen isolierten Docker-Container bereit und machen seine Anwendung über einen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) verfügbar. Einige Sekunden nach dem Ausführen eines einzigen Bereitstellungsbefehls können Sie zur ausgeführten Anwendung im Container navigieren:

![In Azure Container Instances bereitgestellte App im Browser][aci-app-browser]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-account] erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diesen Schnellstart verwenden. Für die lokale Verwendung wird mindestens Version 2.0.55 empfohlen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install] Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Azure Container Instances muss wie alle Azure-Ressourcen in einer Ressourcengruppe bereitgestellt werden. Mit Ressourcengruppen können verwandte Azure-Ressourcen organisiert und verwaltet werden.

Erstellen Sie zunächst mit dem folgenden Befehl [az group create][az-group-create] eine Ressourcengruppe namens *myResourceGroup* am Standort *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Erstellen eines Containers

Sie besitzen eine Ressourcengruppe und können nun einen Container in Azure ausführen. Geben Sie zum Erstellen einer Containerinstanz mit der Azure-Befehlszeilenschnittstelle einen Ressourcengruppennamen, einen Containerinstanznamen und ein Docker-Containerimage im Befehl [az container create][az-container-create] an. In diesem Schnellstart verwenden Sie das öffentliche Image `microsoft/aci-helloworld`. Dieses Image verpackt eine kleine in Node.js geschriebene Web-App, die eine statische HTML-Seite bedient.

Sie können Ihre Container über das Internet verfügbar machen, indem Sie mindestens einen zu öffnenden Port und/oder eine DNS-Namensbezeichnung angeben. In diesem Schnellstart stellen Sie einen Container mit einer DNS-Namensbezeichnung bereit, damit die Web-App öffentlich erreichbar ist.

Führen Sie einen Befehl wie den folgenden aus, um eine Containerinstanz zu starten. Der Wert `--dns-name-label` muss in der Azure-Region, in der Sie die Instanz erstellen, eindeutig sein. Falls die Fehlermeldung „DNS-Namensbezeichnung ist nicht verfügbar.“ angezeigt wird, sollten Sie eine andere DNS-Namensbezeichnung verwenden.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

Sie sollten innerhalb weniger Sekunden eine Antwort von der Azure-Befehlszeilenschnittstelle mit dem Hinweis erhalten, dass die Bereitstellung abgeschlossen wurde. Überprüfen Sie den Status mit dem Befehl [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Wenn Sie den Befehl ausführen, werden der vollständig qualifizierte Domänenname (Fully Qualified Domain Name, FQDN) des Containers und der Bereitstellungsstatus angezeigt.

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Wenn der Wert `ProvisioningState` des Containers **Erfolgreich** lautet, navigieren Sie in Ihrem Browser zu seinem FQDN. Wird etwa folgende Webseite angezeigt, war der Vorgang erfolgreich. Sie haben eine in einem Docker-Container ausgeführte Anwendung in Azure bereitgestellt.

![Screenshot eines Browsers mit ausgeführter Anwendung in einer Azure-Containerinstanz][aci-app-browser]

Sollte die Anwendung nicht sofort angezeigt werden, warten Sie einige Sekunden, bis die DNS-Auffüllung abgeschlossen ist, und aktualisieren Sie anschließend Ihren Browser.

## <a name="pull-the-container-logs"></a>Herunterladen der Containerprotokolle

Wenn Sie ein Problem mit einem Container oder der darin ausgeführten Anwendung beheben (oder nur seine Ausgabe anzeigen) müssen, sehen Sie sich zuerst die Protokolle der Containerinstanz an.

Verwenden Sie den Befehl [az container logs][az-container-logs], um die Protokolle der Containerinstanz per Pull abzurufen:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Die Ausgabe enthält die Protokolle für den Container und sollte die HTTP GET-Anforderungen anzeigen, die beim Betrachten der Anwendung in Ihrem Browser generiert wurden.

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
listening on port 80
::ffff:10.240.255.105 - - [01/Oct/2018:18:25:51 +0000] "GET / HTTP/1.0" 200 1663 "-" "-"
::ffff:10.240.255.106 - - [01/Oct/2018:18:31:04 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
::ffff:10.240.255.106 - - [01/Oct/2018:18:31:04 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
```

## <a name="attach-output-streams"></a>Anfügen von Ausgabestreams

Zusätzlich zum Anzeigen der Protokolle können Sie Ihre lokalen Standardausgabestreams und Standardfehlerstreams an die Streams des Containers anfügen.

Führen Sie zunächst den Befehl [az container attach][az-container-attach] aus, um Ihre lokale Konsole den Ausgabestreams des Containers anzufügen:

```azurecli-interactive
az container attach --resource-group myResourceGroup --name mycontainer
```

Aktualisieren Sie nach dem Anfügen mehrmals Ihren Browser, um zusätzliche Ausgaben zu generieren. Trennen Sie anschließend Ihre Konsole mit `Control+C`. Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

```console
$ az container attach --resource-group myResourceGroup --name mycontainer
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

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Container nicht mehr benötigen, entfernen Sie ihn mithilfe des Befehls [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Um sicherzustellen, dass der Container gelöscht wurde, führen Sie den Befehl [az container list](/cli/azure/container#az-container-list) aus:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

Der Container **mycontainer** sollte nicht in der Ausgabe des Befehls angezeigt werden. Wenn die Ressourcengruppe keine anderen Container enthält, wird keine Ausgabe angezeigt.

Wenn Sie die Ressourcengruppe *myResourceGroup* und alle darin enthaltenen Ressourcen nicht mehr benötigen, löschen Sie sie mit dem Befehl [az group delete][az-group-delete]:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure-Containerinstanz mithilfe eines Images in der öffentlichen Docker Hub-Registrierung erstellt. Fahren Sie mit dem Azure Container Instances-Tutorial fort, wenn Sie ein Containerimage erstellen und über eine private Azure-Containerregistrierung bereitstellen möchten.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](./container-instances-tutorial-prepare-app.md)

Um Optionen zum Ausführen von Containern in einem Orchestrierungssystem in Azure zu testen, lesen Sie die Schnellstartanleitung für [Azure Kubernetes Service (AKS)][container-service].

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/
[node-js]: https://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-container-list]: /cli/azure/container#az-container-list
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
