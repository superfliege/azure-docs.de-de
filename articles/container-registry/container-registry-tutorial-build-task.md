---
title: 'Tutorial: Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Build'
description: In diesem Tutorial erfahren Sie, wie Sie eine Buildaufgabe so konfigurieren, dass automatisch Buildvorgänge für Containerimages in der Cloud ausgelöst werden, wenn Sie Quellcode in einem Git-Repository committen.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: fba499441d092f4dce09d13d607dfc5de65d98b2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-automate-container-image-builds-with-azure-container-registry-build"></a>Tutorial: Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Build

Neben der [Schnellerstellung](container-registry-tutorial-quick-build.md) unterstützt ACR Build mit der *Buildaufgabe* auch automatische Buildvorgänge für Docker-Containerimages. In diesem Tutorial erstellen Sie mithilfe der Azure CLI eine Buildaufgabe, die automatisch Imagebuildvorgänge in der Cloud auslöst, wenn Sie Quellcode in einem Git-Repository committen.

Dieser zweite Teil der Tutorialreihe umfasst Folgendes:

> [!div class="checklist"]
> * Erstellen einer Buildaufgabe
> * Testen der Buildaufgabe
> * Anzeigen des Buildstatus
> * Auslösen der Buildaufgabe durch Committen von Code

In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Schritte des [vorherigen Tutorials](container-registry-tutorial-quick-build.md) ausgeführt haben. Führen Sie bei Bedarf die Schritte im Abschnitt [Voraussetzungen](container-registry-tutorial-quick-build.md#prerequisites) des vorherigen Tutorials aus, bevor Sie mit diesem Tutorial fortfahren.

> [!IMPORTANT]
> ACR Build befindet sich derzeit in der Vorschauphase und wird nur von Azure-Containerregistrierungen in den Regionen **USA, Osten** (eastus) und **Europa, Westen** (westeurope) unterstützt. Wenn Sie Vorschauversionen nutzen möchten, müssen Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Azure CLI lokal verwenden möchten, muss bei Ihnen mindestens die Azure CLI-Version **2.0.32** installiert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und Aktualisierung der CLI finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0][azure-cli].

## <a name="prerequisites"></a>Voraussetzungen

### <a name="get-sample-code"></a>Abrufen des Beispielcodes

In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Schritte des [vorherigen Tutorials](container-registry-tutorial-quick-build.md) ausgeführt und das Beispielrepository geforkt und geklont haben. Führen Sie bei Bedarf die Schritte im Abschnitt [Voraussetzungen](container-registry-tutorial-quick-build.md#prerequisites) des vorherigen Tutorials aus, bevor Sie mit diesem Tutorial fortfahren.

### <a name="container-registry"></a>Containerregistrierung

Ihr Abonnement muss eine Azure-Containerregistrierung enthalten, um dieses Tutorial bearbeiten zu können. Informationen zum Erstellen einer Registrierung finden Sie bei Bedarf im [vorherigen Tutorial](container-registry-tutorial-quick-build.md) und in der Schnellstartanleitung [Erstellen einer Containerregistrierung mit der Azure-Befehlszeilenschnittstelle](container-registry-get-started-azure-cli.md).

## <a name="build-task"></a>Buildaufgabe

Eine Buildaufgabe definiert die Eigenschaften eines automatisierten Buildvorgangs einschließlich des Orts, an dem sich der Quellcode befindet, und des Ereignisses, das den Buildvorgang auslöst. Wenn ein in der Buildaufgabe definiertes Ereignis eintritt (beispielsweise ein Commit in einem Git-Repository), initiiert ACR Build einen Containerimage-Buildvorgang in der Cloud. Das erfolgreich erstellte Image wird standardmäßig an die in der Aufgabe angegebene Azure-Containerregistrierung gepusht.

ACR Build unterstützt derzeit Buildaufgaben mit folgenden Triggern:

* Commit in einem Git-Repository
* Aktualisierung des Basisimages

## <a name="create-a-build-task"></a>Erstellen einer Buildaufgabe

In diesem Abschnitt erstellen Sie zunächst ein persönliches GitHub-Zugriffstoken (Personal Access Token, PAT) zur Verwendung mit ACR Build. Anschließend erstellen Sie eine Buildaufgabe, die einen Buildvorgang auslöst, wenn Code in Ihrem Fork des Repositorys committet wird.

### <a name="create-a-github-personal-access-token"></a>Erstellen eines persönlichen GitHub-Zugriffstokens

Um nach einem Commit in einem Git-Repository einen Buildvorgang auslösen zu können, benötigt ACR Build ein persönliches Zugriffstoken (Personal Access Token, PAT) für den Zugriff auf das Repository. Führen Sie die folgenden Schritte aus, um ein PAT in GitHub zu erstellen:

1. Navigieren Sie zur PAT-Erstellungsseite von GitHub (https://github.com/settings/tokens/new).
1. Geben Sie eine kurze **Beschreibung** für das Token ein (beispielsweise „ACR Build Task Demo“).
1. Aktivieren Sie unter **repo** die Kontrollkästchen **repo:status** und **public_repo**.

   ![Screenshot der Seite zum Generieren eines persönlichen Zugriffstokens in GitHub][build-task-01-new-token]

1. Klicken Sie auf die Schaltfläche **Generate token** (Token generieren). Unter Umständen werden Sie zur Bestätigung Ihres Kennworts aufgefordert.
1. Kopieren und speichern Sie das generierte Token an einem **sicheren Ort**. Das Token wird im nächsten Abschnitt beim Definieren einer Buildaufgabe benötigt.

   ![Screenshot des generierten persönlichen Zugriffstokens in GitHub][build-task-02-generated-token]

### <a name="create-the-build-task"></a>Erstellen der Buildaufgabe

Nachdem Sie die erforderlichen Schritte abgeschlossen haben, um ACR Build das Lesen des Commitstatus und das Erstellen von Webhooks in einem Repository zu ermöglichen, können Sie eine Buildaufgabe erstellen, die im Falle eines Commits im Repository einen Containerimage-Buildvorgang auslöst.

Füllen Sie zunächst die folgenden Shell-Umgebungsvariablen mit geeigneten Werten für Ihre Umgebung auf. Dies ist zwar nicht zwingend erforderlich, vereinfacht aber das Ausführen der mehrzeiligen Azure CLI-Befehle in diesem Tutorial. Wenn Sie diese Werte nicht angeben, müssen Sie sie später jedes Mal manuell ersetzen, wenn sie in einem der Beispielbefehle vorkommen.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the previous section
```

Führen Sie als Nächstes den folgenden Befehl vom Typ [az acr build-task create][az-acr-build-task-create] aus, um die Buildaufgabe zu erstellen:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --branch master \
    --git-access-token $GIT_PAT
```

Durch diese Buildaufgabe erstellt ACR Build jedes Mal, wenn Code in der Verzweigung *master* des durch `--context` angegebenen Repositorys committet wird, das Containerimage auf der Grundlage des Codes in dieser Verzweigung. Das Argument `--image` gibt für den Versionsteil des Imagetags den parametrisierten Wert `{{.Build.ID}}` an, um sicherzustellen, dass das erstellte Image einem bestimmten Build entspricht und eindeutig gekennzeichnet ist.

Die Ausgabe einer erfolgreichen Ausführung des Befehls [az acr build-task create][az-acr-build-task-create] sieht in etwa wie folgt aus:

```console
$ az acr build-task create \
>     --registry $ACR_NAME \
>     --name buildhelloworld \
>     --image helloworld:{{.Build.ID}} \
>     --context https://github.com/$GIT_USER/acr-build-helloworld-node \
>     --branch master \
>     --git-access-token $GIT_PAT
{
  "additionalProperties": {},
  "alias": "buildhelloworld",
  "creationDate": "2018-04-18T23:14:45.905395+00:00",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry/buildTasks/buildhelloworld",
  "location": "eastus",
  "name": "buildhelloworld",
  "platform": {
    "additionalProperties": {},
    "cpu": 1,
    "osType": "Linux"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceRepository": {
    "additionalProperties": {},
    "isCommitTriggerEnabled": true,
    "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node",
    "sourceControlAuthProperties": null,
    "sourceControlType": "Github"
  },
  "status": "enabled",
  "tags": null,
  "timeout": null,
  "type": "Microsoft.ContainerRegistry/registries/buildTasks"
}

```

## <a name="test-the-build-task"></a>Testen der Buildaufgabe

Sie verfügen nun über eine Buildaufgabe, die Ihren Build definiert. Lösen Sie zum Testen der Builddefinition manuell einen Buildvorgang aus, indem Sie den Befehl [az acr build-task run][az-acr-build-task-run] ausführen:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Der Befehl `az acr build-task run` streamt die Protokollausgabe standardmäßig an Ihre Konsole, wenn Sie den Befehl ausführen. In diesem Beispiel zeigt die Ausgabe, dass der Build **eastus2** der Warteschlange hinzugefügt und erstellt wurde.

```console
$ az acr build-task run --registry mycontainerregistry --name buildhelloworld
Queued a build with build-id: eastus2.
Starting to stream the logs...
Cloning into '/root/acr-builder/src'...
time="2018-04-19T00:06:20Z" level=info msg="Running command git checkout master"
Already on 'master'
Your branch is up to date with 'origin/master'.
ffef1347389a008c9a8bfdf8c6a0ed78b0479894
time="2018-04-19T00:06:20Z" level=info msg="Running command git rev-parse --verify HEAD"
time="2018-04-19T00:06:20Z" level=info msg="Running command docker build --pull -f Dockerfile -t mycontainerregistry.azurecr.io/helloworld:eastus2 ."
Sending build context to Docker daemon  182.8kB
Step 1/5 : FROM node:9-alpine
9: Pulling from library/node
Digest: sha256:bd7b9aaf77ab2ce1e83e7e79fc0969229214f9126ced222c64eab49dc0bdae90
Status: Image is up to date for node:9-alpine
 ---> aa3e171e4e95
Step 2/5 : COPY . /src
 ---> e1c04dc2993b

[...]

6e5e20cbf4a7: Layer already exists
b69680cb4898: Pushed
b54af9b858b7: Pushed
eastus2: digest: sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6 size: 2423
time="2018-04-19T00:06:51Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistry.azurecr.io/helloworld:eastus2"
"["mycontainerregistry.azurecr.io/helloworld@sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6"]"
time="2018-04-19T00:06:51Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:bd7b9aaf77ab2ce1e83e7e79fc0969229214f9126ced222c64eab49dc0bdae90"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistry.azurecr.io
    repository: helloworld
    tag: eastus2
    digest: sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
  git:
    git-head-revision: 6944c6bd0602f96e5fecf56ff8d66e2d268223e3

Build complete
Build ID: eastus2 was successful after 39.789138274s
```

## <a name="view-build-status"></a>Anzeigen des Buildstatus

Manchmal kann es hilfreich sein, den Status eines laufenden Buildvorgangs anzuzeigen, den Sie nicht manuell ausgelöst haben – beispielsweise beim Behandeln von Problemen mit Buildvorgängen, die durch Quellcode-Commits ausgelöst wurden. In diesem Abschnitt lösen Sie einen manuellen Buildvorgang aus, unterdrücken aber das Standardverhalten (also das Streamen des Buildprotokolls an Ihre Konsole). Verwenden Sie dann den Befehl `az acr build-task logs`, um den laufenden Buildvorgang zu überwachen.

Lösen Sie zunächst wie zuvor manuell einen Buildvorgang aus, geben Sie dabei aber das Argument `--no-logs` an, um die Protokollierung in der Konsole zu unterdrücken:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld --no-logs
```

Verwenden Sie dann den Befehl `az build-task logs`, um das Protokoll des derzeit ausgeführten Buildvorgangs anzuzeigen:

```azurecli-interactive
az acr build-task logs --registry $ACR_NAME
```

Das Protokoll für den derzeit ausgeführten Buildvorgang wird an Ihre Konsole gestreamt und sollte in etwa wie in der folgenden (gekürzten) Ausgabe aussehen:

```console
$ az acr build-task logs --registry $ACR_NAME
Showing logs for the last updated build...
Build-id: eastus3

[...]

Build complete
Build ID: eastus3 was successful after 30.076988169s
```

## <a name="trigger-a-build-with-a-commit"></a>Auslösen eines Buildvorgangs durch ein Commit

Nachdem Sie die Buildaufgabe manuell ausgeführt haben, um sie zu testen, können Sie als Nächstes eine Quellcodeänderung vornehmen, um die Aufgabe automatisch auszulösen.

Vergewissern Sie sich zunächst, dass Sie sich im Verzeichnis mit dem lokalen Klon des [Repositorys][sample-repo] befinden:

```azurecli-interactive
cd acr-build-helloworld-node
```

Führen Sie als Nächstes die folgenden Befehle aus, um eine neue Datei zu erstellen, zu committen und an Ihr Fork des Repositorys auf GitHub zu pushen:

```azurecli-interactive
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Build"
git push origin master
```

Wenn Sie den Befehl `git push` ausführen, werden Sie unter Umständen zur Eingabe Ihrer GitHub-Anmeldeinformationen aufgefordert. Geben Sie Ihren GitHub-Benutzernamen und als Kennwort das persönliche Zugriffstoken (Personal Access Token, PAT) ein, das Sie zuvor erstellt haben.

```console
$ git push origin master
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

Nachdem Sie einen Commit an Ihr Repository gepusht haben, wird der von ACR Build erstellte Webhook ausgelöst und initiiert einen Bildvorgang in Azure Container Registry. Zeigen Sie die Buildprotokolle für den derzeit ausgeführten Buildvorgang an, um den Buildstatus zu überprüfen und zu überwachen:

```azurecli-interactive
az acr build-task logs --registry $ACR_NAME
```

Die Ausgabe sieht in etwa wie im folgenden Beispiel aus und zeigt den derzeit (oder zuletzt) ausgeführten Buildvorgang:

```console
$ az acr build-task logs --registry $ACR_NAME
Showing logs for the last updated build...
Build-id: eastus4

[...]

Build complete
Build ID: eastus4 was successful after 28.9587031s
```

## <a name="list-builds"></a>Auflisten der Builds

Führen Sie den Befehl [az acr build-task list-builds][az-acr-build-task-list-builds] aus, um eine Liste mit den Buildvorgängen anzuzeigen, die ACR Build für Ihre Registrierung abgeschlossen hat:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Die Ausgabe des Befehls sollte in etwa wie im folgenden Beispiel aussehen. Die von ACR Build ausgeführten Buildvorgänge werden angezeigt, und die Spalte „TRIGGER“ (Auslöser) enthält „Git Commit“ für den neuesten Build:

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
----------  ---------------  ----------  ---------  ----------  --------------------  ----------
eastus4     buildhelloworld  Linux       Succeeded  Git Commit  2018-04-20T22:50:27Z  00:00:35
eastus3     buildhelloworld  Linux       Succeeded  Manual      2018-04-20T22:47:19Z  00:00:30
eastus2     buildhelloworld  Linux       Succeeded  Manual      2018-04-20T22:46:14Z  00:00:55
eastus1                                  Succeeded  Manual      2018-04-20T22:38:22Z  00:00:55
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie mithilfe einer Buildaufgabe automatisch Buildvorgänge für Containerimages auslösen, wenn Sie Quellcode in einem Git-Repository committen. Im nächsten Tutorial erfahren Sie, wie Sie Buildaufgaben erstellen, die Buildvorgänge auslösen, wenn das Basisimage eines Containerimages aktualisiert wird.

> [!div class="nextstepaction"]
> [Automatisieren von Buildvorgängen für Images nach der Aktualisierung des Basisimages mit Azure Container Registry Build](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-build-task-list-builds]: /cli/azure/acr#az-acr-build-task-list-build

<!-- IMAGES -->
[build-task-01-new-token]: ./media/container-registry-tutorial-build-tasks/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-tutorial-build-tasks/build-task-02-generated-token.png
