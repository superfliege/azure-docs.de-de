---
title: Pushübertragung eines Docker-Images an eine private Azure-Registrierung
description: Push- und Pullübertragung von Docker-Images an eine private Containerregistrierung in Azure mit der Docker CLI
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 11/29/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 24cccd4745d611196046168f0125e7ef2a184e15
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576490"
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Pushübertragung des ersten Images an eine private Containerregistrierung mit der Docker CLI

Eine Azure-Containerregistrierung speichert und verwaltet private [Docker](http://hub.docker.com)-Containerimages. Dies ähnelt der Art und Weise, wie [Docker Hub](https://hub.docker.com/) öffentliche Docker-Images speichert. Für Ihre Containerregistrierung können Sie die [Docker-Befehlszeilenschnittstelle](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) für die Vorgänge [Anmeldung](https://docs.docker.com/engine/reference/commandline/login/), [Push](https://docs.docker.com/engine/reference/commandline/push/), [Pull](https://docs.docker.com/engine/reference/commandline/pull/) und andere Vorgänge verwenden.

In den folgenden Schritten laden Sie ein offizielles [Nginx-Image](https://store.docker.com/images/nginx) aus der öffentlichen Docker Hub-Registrierung herunter, kennzeichnen es für Ihre private Azure-Containerregistrierung und übertragen es zuerst per Pushvorgang an Ihre Registrierung und dann per Pullvorgang aus der Registrierung.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Containerregistrierung**: Erstellen Sie in Ihrem Azure-Abonnement eine Containerregistrierung. Verwenden Sie beispielsweise das [Azure-Portal](container-registry-get-started-portal.md) oder die [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI**: Installieren Sie [Docker](https://docs.docker.com/engine/installation/), um Ihren lokalen Computer als Docker-Host einzurichten und auf die Befehle der Docker CLI zuzugreifen.

## <a name="log-in-to-a-registry"></a>Anmelden an einer Registrierung

Es gibt [verschiedene Möglichkeiten für die Authentifizierung](container-registry-authentication.md) bei Ihrer privaten Containerregistrierung. Die empfohlene Methode bei Verwendung einer Befehlszeile ist der Azure CLI-Befehl [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login). Beispiel für die Anmeldung bei einer Registrierung mit dem Namen *myregistry*:

```azurecli
az acr login --name myregistry
```

Sie können sich auch mit [docker login](https://docs.docker.com/engine/reference/commandline/login/) anmelden. Im folgenden Beispiel werden die ID und das Kennwort eines Azure Active Directory-[Dienstprinzipals](../active-directory/develop/app-objects-and-service-principals.md) übergeben. Angenommen, Sie haben Ihrer Registrierung für ein Automatisierungsszenario [einen Dienstprinzipal zugewiesen](container-registry-authentication.md#service-principal).

```Bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Beide Befehle geben nach Abschluss `Login Succeeded` zurück. Wenn Sie `docker login` verwenden, wird möglicherweise auch eine Sicherheitswarnung angezeigt, in der die Verwendung des `--password-stdin`-Parameters empfohlen wird. Obwohl in diesem Artikel nicht auf dessen Verwendung eingegangen werden kann, wird empfohlen, diese bewährte Methode anzuwenden. Weitere Informationen finden Sie in der [docker login](https://docs.docker.com/engine/reference/commandline/login/)-Befehlsreferenz.

> [!TIP]
> Geben Sie immer den vollqualifizierten Registrierungsnamen (nur Kleinbuchstaben) an, wenn Sie `docker login` verwenden und Images für die Pushübertragung in Ihre Registrierung kennzeichnen. In den Beispielen in diesem Artikel wird der vollqualifizierte Name *myregistry.azurecr.io* verwendet.

## <a name="pull-the-official-nginx-image"></a>Übertragen des offiziellen Nginx-Images per Pullvorgang

Übertragen Sie zunächst das öffentliche Nginx-Image per Pullvorgang auf Ihren lokalen Computer.

```Bash
docker pull nginx
```

## <a name="run-the-container-locally"></a>Lokales Ausführen des Containers

Führen Sie den folgenden [docker run](https://docs.docker.com/engine/reference/run/)-Befehl aus, um eine lokale Instanz des Nginx-Containers interaktiv (`-it`) auf Port 8080 zu starten. Das Argument `--rm` gibt an, dass der Container entfernt werden soll, wenn Sie ihn beenden.

```Bash
docker run -it --rm -p 8080:80 nginx
```

Browsen Sie zu [http://localhost:8080](http://localhost:8080), um die von Nginx bereitgestellte Standardwebseite im ausgeführten Container anzuzeigen. Eine Seite ähnlich der folgenden wird angezeigt:

![Nginx auf lokalem Computer](./media/container-registry-get-started-docker-cli/nginx.png)

Da Sie den Container interaktiv mit `-it` gestartet haben, können Sie nach dem Navigieren im Browser die Ausgabe des Nginx-Servers in der Befehlszeile sehen.

Drücken Sie zum Beenden und Entfernen des Containers die Tastenkombination `Control`+`C`.

## <a name="create-an-alias-of-the-image"></a>Erstellen eines Alias des Images

Verwenden Sie [docker tag](https://docs.docker.com/engine/reference/commandline/tag/), um einen Alias des Images mit vollqualifiziertem Pfad zur Registrierung zu erstellen. In diesem Beispiel wird der `samples`-Namespace angegeben, um den Stamm der Registrierung nicht zu überladen.

```Bash
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Weitere Informationen zum Kennzeichnen mit Namespaces finden Sie unter [Bewährte Methoden für Azure Container Registry](container-registry-best-practices.md) im Abschnitt [Repositorynamespaces](container-registry-best-practices.md#repository-namespaces).

## <a name="push-the-image-to-your-registry"></a>Übertragen des Images per Push in Ihre Registrierung

Nachdem Sie das Image mit dem vollqualifizierten Pfad in Ihrer privaten Registrierung gekennzeichnet haben, können Sie es nun per Pushvorgang mit [docker push](https://docs.docker.com/engine/reference/commandline/push/) in die Registrierung übertragen:

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Übertragen des Images aus Ihrer Registrierung per Pull

Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um das Image per Pullvorgang aus Ihrer Registrierung zu übertragen:

```Bash
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>Starten des Nginx-Containers

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/run/), um das Image auszuführen, das Sie per Pullvorgang aus der Registrierung übertragen haben:

```Bash
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Navigieren Sie zu [http://localhost:8080](http://localhost:8080), um den ausgeführten Container anzuzeigen.

Drücken Sie zum Beenden und Entfernen des Containers die Tastenkombination `Control`+`C`.

## <a name="remove-the-image-optional"></a>Entfernen des Images (optional)

Wenn Sie das Nginx-Image nicht mehr benötigen, können Sie es mit dem Befehl [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) lokal löschen.

```Bash
docker rmi myregistry.azurecr.io/samples/nginx
```

Um Images aus Ihrer Azure-Containerregistrierung zu entfernen, können Sie den Azure CLI-Befehl [az acr repository delete](/cli/azure/acr/repository#az-acr-repository-delete) ausführen. Mit dem folgenden Befehl werden beispielsweise das durch ein Tag referenzierte Manifest, alle zugeordneten Ebenendaten und alle anderen Tags gelöscht, die auf das Manifest verweisen.

```azurecli
az acr repository delete --name myregistry --repository samples/nginx --tag latest --manifest
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit den Grundlagen vertraut gemacht haben, können Sie mit der Verwendung Ihrer Registrierung beginnen! Stellen Sie Containerimages aus Ihrer Registrierung bereit:

* [Azure Kubernetes Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md)
* [Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)
* [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)
