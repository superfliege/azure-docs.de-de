---
title: 'Tutorial: Erstellen von Containerimages in der Cloud mit Azure Container Registry Build'
description: In diesem Tutorial erfahren Sie, wie Sie mit Azure Container Registry Build (ACR Build) ein Docker-Containerimage in Azure erstellen und anschließend in Azure Container Instances bereitstellen.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: fefa6096e39d86b94f8ea348d894179220ad09e1
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34057567"
---
# <a name="tutorial-build-container-images-in-the-cloud-with-azure-container-registry-build"></a>Tutorial: Erstellen von Containerimages in der Cloud mit Azure Container Registry Build

**ACR Build** ist eine Suite mit Features in Azure Container Registry, die optimierte und effiziente Docker-Containerimage-Builds in Azure bietet. In diesem Artikel erfahren Sie, wie Sie das *Schnellerstellungsfeature* von ACR Build verwenden. Die Schnellerstellung erweitert die innere Schleife Ihrer Entwicklung auf die Cloud und ermöglicht die Überprüfung des Erfolgsstatus von Buildvorgängen sowie automatisches Pushen erfolgreich erstellter Images an Ihre Containerregistrierung. Ihre Images werden nativ in der Cloud (in der Nähe Ihrer Registrierung) erstellt, was eine schnellere Bereitstellung ermöglicht.

Sämtliche Dockerfile-Kenntnisse lassen sich 1:1 auf ACR Build übertragen. Für die Erstellung in der Cloud mit ACR Build müssen Sie lediglich den ausgeführten Befehl ändern. Ihre Dockerfiles können Sie unverändert weiter verwenden.

Dieser erste Teil einer Tutorialreihe umfasst Folgendes:

> [!div class="checklist"]
> * Abrufen des Quellcodes für die Beispielanwendung
> * Erstellen eines Containerimages in Azure
> * Bereitstellen eines Containers für Azure Container Instances

In den weiteren Tutorials erfahren Sie, wie Sie mithilfe der Buildaufgaben von ACR Build Containerimage-Buildvorgänge automatisieren, wenn Code committet oder ein Basisimage aktualisiert wird.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Azure CLI lokal verwenden möchten, muss bei Ihnen mindestens die Azure CLI-Version **2.0.32** installiert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und Aktualisierung der CLI finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0][azure-cli].

## <a name="prerequisites"></a>Voraussetzungen

### <a name="github-account"></a>GitHub-Konto

Erstellen Sie unter https://github.com ein Konto, falls Sie noch keins besitzen. In dieser Tutorialreihe werden automatisierte Imagebuildvorgänge in ACR Build anhand eines GitHub-Repositorys veranschaulicht.

### <a name="fork-sample-repository"></a>Forken des Beispielrepositorys

Forken Sie als Nächstes das Beispielrepository über die Benutzeroberfläche von GitHub in Ihr GitHub-Konto. In diesem Tutorial erstellen Sie ein Containerimage auf der Grundlage der Quelle im Repository. Im nächsten Tutorial pushen Sie dann ein Commit an Ihr Fork des Repositorys, um einen automatischen Buildvorgang zu initiieren.

Forken Sie das folgende Repository: https://github.com/Azure-Samples/acr-build-helloworld-node

![Screenshot der Schaltfläche zum Forken (hervorgehoben) in GitHub][quick-build-01-fork]

### <a name="clone-your-fork"></a>Klonen Ihres Forks

Klonen Sie nach dem Forken des Repositorys Ihren Fork, und geben Sie das Verzeichnis ein, das Ihren lokalen Klon enthält.

Klonen Sie das Repository mit `git`, und ersetzen Sie dabei **\<your-github-username\>** durch Ihren GitHub-Benutzernamen:

```azurecli-interactive
git clone https://github.com/<your-github-username>/acr-build-helloworld-node
```

Geben Sie das Verzeichnis ein, das den Quellcode enthält:

```azurecli-interactive
cd acr-build-helloworld-node
```

### <a name="bash-shell"></a>Bash-Shell

Die Befehle in dieser Tutorialreihe sind für die Bash-Shell formatiert. Falls Sie lieber PowerShell, die Befehlszeile oder eine andere Shell verwenden möchten, müssen Sie ggf. die Zeilenfortsetzung und das Umgebungsvariablenformat entsprechend anpassen.

## <a name="build-in-azure-with-acr-build"></a>Erstellen in Azure mit ACR Build

Nachdem Sie den Quellcode auf Ihren Computer heruntergeladen haben, können Sie mit den folgenden Schritten eine Containerregistrierung und anschließend das Containerimage mit ACR Build erstellen.

In den Tutorials dieser Reihe werden Shell-Umgebungsvariablen verwendet, um das Ausführen der Beispielbefehle zu vereinfachen. Führen Sie den folgenden Befehl aus, um die Variable `ACR_NAME` festzulegen. Ersetzen Sie **\<registry-name\>** durch einen eindeutigen Namen für Ihre neue Containerregistrierung. Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Da die anderen Ressourcen, die Sie in diesem Tutorial erstellen, auf diesem Namen basieren, sollte die Änderung dieser ersten Variablen ausreichend sein.

```azurecli-interactive
ACR_NAME=<registry-name>
```

Nach Angabe der Umgebungsvariablen für die Containerregistrierung können Sie die restlichen Befehle aus diesem Tutorial ohne weitere Bearbeitung kopieren und einfügen. Führen Sie die folgenden Befehle aus, um eine Ressourcengruppe und eine Containerregistrierung zu erstellen:

```azurecli-interactive
RES_GROUP=$ACR_NAME # Resource Group name

az group create --resource-group $RES_GROUP --location eastus
az acr create --resource-group $RES_GROUP --name $ACR_NAME --sku Standard --location eastus
```

Erstellen Sie nach der Registrierungserstellung mithilfe von ACR Build ein auf dem Beispielcode basierendes Containerimage. Führen Sie den Befehl [az acr build][az-acr-build] aus, um eine *Schnellerstellung* durchzuführen:

```azurecli-interactive
az acr build --registry $ACR_NAME --image helloacrbuild:v1 .
```

Die Ausgabe des Befehls [az acr build][az-acr-build] sollte in etwa wie im folgenden Beispiel aussehen. Sie sehen das Hochladen des Quellcodes („context“) in Azure sowie die Details des Vorgangs `docker build`, den ACR Build in der Cloud ausführt. Da ACR Build Ihre Images mithilfe von `docker build` erstellt, sind keine Änderungen an Ihren Dockerfiles erforderlich, und Sie können sofort mit der Verwendung von ACR Build beginnen.

```console
$ az acr build --registry $ACR_NAME --image helloacrbuild:v1 .
Sending build context (4.909 KiB) to ACR.
Queued a build with build ID: aa1
Waiting for a build agent...
Sending build context to Docker daemon  22.53kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
Status: Image is up to date for node:9-alpine
 ---> 7af437a39ec2
Step 2/5 : COPY . /src
 ---> 0c4814714938
Step 3/5 : RUN cd /src && npm install
 ---> Running in 4f77ce7b330d
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN helloworld@1.0.0 No repository field.

up to date in 0.096s
Removing intermediate container 4f77ce7b330d
 ---> e0eb24339e07
Step 4/5 : EXPOSE 80
 ---> Running in 872bd29edbc7
Removing intermediate container 872bd29edbc7
 ---> 22ba8d8ffb4e
Step 5/5 : CMD ["node", "/src/server.js"]
 ---> Running in 1a40c05c4122
Removing intermediate container 1a40c05c4122
 ---> 0a9a4b74fb53
Successfully built 0a9a4b74fb53
Successfully tagged mycontainerregistry.azurecr.io/helloacrbuild:v1
time="2018-05-10T19:10:20Z" level=info msg="Running command docker push mycontainerregistry.azurecr.io/helloacrbuild:v1"
The push refers to repository [mycontainerregistry.azurecr.io/helloacrbuild]
d2b301f7ef94: Preparing
d0e0f2bb8747: Preparing
26b0c207c4a9: Preparing
917e7cdebc8b: Preparing
9dfa40a0da3b: Preparing
26b0c207c4a9: Pushed
d2b301f7ef94: Pushed
d0e0f2bb8747: Pushed
9dfa40a0da3b: Pushed
917e7cdebc8b: Pushed
v1: digest: sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd size: 1366
time="2018-05-10T19:11:07Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistry.azurecr.io/helloacrbuild:v1"
"["mycontainerregistry.azurecr.io/helloacrbuild@sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd"]"
time="2018-05-10T19:11:07Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistry.azurecr.io
    repository: helloacrbuild
    tag: v1
    digest: sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3

Build complete
Build ID: aa1 was successful after 52.522222729s
```

Gegen Ende der Ausgabe zeigt ACR Build die für Ihr Image ermittelten Abhängigkeiten an. Dies ermöglicht ACR Build die Automatisierung von Imagebuildvorgängen nach einer Basisimageaktualisierung – etwa, wenn ein Basisimage mit Betriebssystem- oder Frameworkpatches aktualisiert wird. Auf die Unterstützung von Basisimageaktualisierungen durch ACR Build wird im weiteren Verlauf dieser Tutorialreihe noch näher eingegangen.

## <a name="deploy-to-azure-container-instances"></a>Bereitstellen in Azure Container Instances

Erfolgreich erstellte Images werden von ACR Build standardmäßig automatisch an Ihre Registrierung gepusht, von wo aus sie sofort bereitgestellt werden können.

In diesem Abschnitt erstellen Sie eine Azure Key Vault-Instanz und einen Dienstprinzipal und stellen anschließend den Container unter Verwendung der Anmeldeinformationen des Dienstprinzipals in Azure Container Instances (ACI) bereit.

### <a name="configure-registry-authentication"></a>Konfigurieren der Authentifizierung der Registrierung

In Produktionsszenarien müssen für den Zugriff auf eine Azure-Containerregistrierung immer [Dienstprinzipale][service-principal-auth] verwendet werden. Dienstprinzipale ermöglichen Ihnen eine rollenbasierte Zugriffssteuerung auf Ihre Containerimages. Beispielsweise können Sie einen Dienstprinzipal mit ausschließlichem Pullzugriff auf eine Registrierung konfigurieren.

#### <a name="create-key-vault"></a>Erstellen eines Schlüsseltresors

Wenn Sie noch keinen Tresor in [Azure Key Vault](/azure/key-vault/) haben, erstellen Sie einen mithilfe der Azure CLI und folgenden Befehle.

```azurecli-interactive
AKV_NAME=$ACR_NAME-vault

az keyvault create --resource-group $RES_GROUP --name $AKV_NAME
```

#### <a name="create-service-principal-and-store-credentials"></a>Erstellen eines Dienstprinzipals und Speichern von Anmeldeinformationen

Sie müssen nun einen Dienstprinzipal erstellen und seine Anmeldeinformationen in Ihrem Schlüsselspeicher speichern.

Erstellen Sie mithilfe des Befehls [az ad sp create-for-rbac][az-ad-sp-create-for-rbac] den Dienstprinzipal, und speichern Sie das Kennwort (**password**) des Dienstprinzipals mithilfe des Befehls [az keyvault secret set][az-keyvault-secret-set] im Tresor:

```azurecli-interactive
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role reader \
                --query password \
                --output tsv)
```

Das Argument `--role` im vorhergehenden Befehl konfiguriert den Dienstprinzipal mit der Rolle *reader*, die ihm ausschließlich Pullzugriff auf die Registrierung gewährt. Um sowohl Push- als auch Pullzugriff zu gewähren, ändern Sie das Argument `--role` in *contributor*.

Speichern Sie als Nächstes die App-ID (*appId*) des Dienstprinzipals im Tresor. Dies ist der Benutzername (**username**), den Sie zur Authentifizierung an Azure Container Registry übergeben:

```azurecli-interactive
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

Sie haben einen Azure Key Vault erstellt und in ihm zwei Geheimnisse gespeichert:

* `$ACR_NAME-pull-usr`: Die Dienstprinzipal-ID für die Verwendung als **Benutzername** für die Containerregistrierung.
* `$ACR_NAME-pull-pwd`: Das Kennwort des Dienstprinzipals für die Verwendung als **Kennwort** für die Containerregistrierung.

Sie können nun auf diese Geheimnisse anhand des Namens verweisen, wenn Sie oder Ihre Anwendungen und Dienste Images per Pull aus der Registrierung abrufen.

### <a name="deploy-container-with-azure-cli"></a>Bereitstellen des Containers mit der Azure CLI

Nachdem die Anmeldeinformationen des Dienstprinzipals als Azure Key Vault-Geheimnisse gespeichert wurden, können sie von Ihren Anwendungen und Diensten für den Zugriff auf Ihre private Registrierung verwendet werden.

Führen Sie den folgenden [az container create][az-container-create]-Befehl aus, um eine Containerinstanz bereitzustellen. Der Befehl verwendet die in Azure Key Vault gespeicherten Anmeldeinformationen des Dienstprinzipals, um sich bei Ihrer Containerregistrierung zu authentifizieren.

```azurecli-interactive
az container create \
    --resource-group $RES_GROUP \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label acr-build-$ACR_NAME \
    --query "{FQDN:ipAddress.fqdn}" \
    --output table
```

Da der Wert `--dns-name-label` innerhalb von Azure eindeutig sein muss, fügt der vorherige Befehl den Namen Ihrer Containerregistrierung an die DNS-Namensbezeichnung des Containers an. Die Ausgabe des Befehls zeigt den vollqualifizierten Domänennamen (FQDN) des Containers. Beispiel:

```console
$ az container create \
>     --resource-group $RES_GROUP \
>     --name acr-build \
>     --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
>     --registry-login-server $ACR_NAME.azurecr.io \
>     --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
>     --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
>     --dns-name-label acr-build-$ACR_NAME \
>     --query "{FQDN:ipAddress.fqdn}" \
>     --output table
FQDN
-------------------------------------------
acr-build-1175.eastus.azurecontainer.io
```

Notieren Sie sich den FQDN des Containers. Er wird im nächsten Abschnitt benötigt.

### <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Verwenden Sie den Befehl [az container attach][az-container-attach], um den Startprozess des Containers zu überwachen:

```azurecli-interactive
az container attach --resource-group $RES_GROUP --name acr-build
```

Die Ausgabe von `az container attach` zeigt zunächst den Status des Containers an, während das Image gepullt und gestartet wird, und bindet dann die Ausgaben „STDOUT“ und „STDERR“ Ihrer lokalen Konsole an die des Containers.

```console
$ az container attach --resource-group $RES_GROUP --name acr-build
Container 'acr-build' is in state 'Waiting'...
Container 'acr-build' is in state 'Running'...
(count: 1) (last timestamp: 2018-04-03 19:45:37+00:00) pulling image "mycontainerregistry.azurecr.io/helloacrbuild:v1"
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Successfully pulled image "mycontainerregistry.azurecr.io/helloacrbuild:v1"
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Created container with id 094ab4da40138b36ca15fc2ad5cac351c358a7540a32e22b52f78e96a4cb3413
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Started container with id 094ab4da40138b36ca15fc2ad5cac351c358a7540a32e22b52f78e96a4cb3413

Start streaming logs:
Server running at http://localhost:80
```

Wenn `Server running at http://localhost:80` angezeigt wird, navigieren Sie in Ihrem Browser zum FQDN des Containers, um die ausgeführte Anwendung anzuzeigen:

![Screenshot der im Browser gerenderten Beispielanwendung][quick-build-02-browser]

Drücken Sie `Control+C`, um Ihre Konsole vom Container zu trennen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Beenden Sie die Containerinstanz mithilfe des Befehls [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group $RES_GROUP --name acr-build
```

Führen Sie die folgenden Befehle aus, um *alle* Ressourcen zu entfernen, die Sie im Rahmen dieses Tutorials erstellt haben (einschließlich der Containerregistrierung, des Schlüsseltresors und des Dienstprinzipals). Falls Sie allerdings direkt mit dem [nächsten Tutorial](container-registry-tutorial-build-task.md) fortfahren möchten, empfiehlt es sich, die Ressourcen nicht zu bereinigen, da dort die gleichen Ressourcen benötigt werden.

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre innere Schleife mit einer Schnellerstellung getestet haben, können Sie eine **Buildaufgabe** konfigurieren, die dafür sorgt, dass ein Buildvorgang für Containerimages ausgelöst wird, wenn Sie Quellcode in einem Git-Repository committen:

> [!div class="nextstepaction"]
> [Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Build](container-registry-tutorial-build-task.md)

<!-- LINKS - External -->
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-keyvault-create]: /cli/azure/keyvault/secret#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[service-principal-auth]: container-registry-auth-service-principal.md

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
