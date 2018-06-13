---
title: 'Tutorial: Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Build'
description: In diesem Tutorial erfahren Sie, wie Sie eine Buildaufgabe so konfigurieren, dass automatisch Buildvorgänge für Containerimages in der Cloud ausgelöst werden, wenn Sie Quellcode in einem Git-Repository committen.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 4accbcb61e57d58100b6a4c06142dd3dc633f7f4
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34057676"
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

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

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
  "creationDate": "2018-05-10T19:34:48.086776+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/mycontainerregistry/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry/buildTasks/buildhelloworld",
  "location": "eastus",
  "name": "buildhelloworld",
  "platform": {
    "additionalProperties": {},
    "cpu": 1,
    "osType": "Linux"
  },
  "properties": {
    "additionalProperties": {
      "imageName": null
    },
    "baseImageDependencies": null,
    "baseImageTrigger": "Runtime",
    "branch": "master",
    "buildArguments": [],
    "contextPath": null,
    "dockerFilePath": "Dockerfile",
    "imageNames": [
      "helloworld:{{.Build.ID}}"
    ],
    "isPushEnabled": true,
    "noCache": false,
    "provisioningState": "Succeeded",
    "type": "Docker"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "mycontainerregistry",
  "sourceRepository": {
    "additionalProperties": {},
    "isCommitTriggerEnabled": true,
    "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node",
    "sourceControlAuthProperties": null,
    "sourceControlType": "GitHub"
  },
  "status": "Enabled",
  "tags": null,
  "timeout": 3600,
  "type": "Microsoft.ContainerRegistry/registries/buildTasks"
}
```

## <a name="test-the-build-task"></a>Testen der Buildaufgabe

Sie verfügen nun über eine Buildaufgabe, die Ihren Build definiert. Lösen Sie zum Testen der Builddefinition manuell einen Buildvorgang aus, indem Sie den Befehl [az acr build-task run][az-acr-build-task-run] ausführen:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Der Befehl `az acr build-task run` streamt die Protokollausgabe standardmäßig an Ihre Konsole, wenn Sie den Befehl ausführen. In diesem Beispiel zeigt die Ausgabe, dass der Build **aa2** der Warteschlange hinzugefügt und erstellt wurde.

```console
$ az acr build-task run --registry $ACR_NAME --name buildhelloworld
Queued a build with build ID: aa2
Waiting for a build agent...
time="2018-05-10T19:37:17Z" level=info msg="Running command git clone https://x-access-token:*************@github.com/gituser/acr-build-helloworld-node /root/acr-builder/src"
Cloning into '/root/acr-builder/src'...
time="2018-05-10T19:37:17Z" level=info msg="Running command git checkout master"
Already on 'master'
Your branch is up to date with 'origin/master'.
920f16cfafa36d0bc3f397c3dd48185a03499404
time="2018-05-10T19:37:17Z" level=info msg="Running command git rev-parse --verify HEAD"
time="2018-05-10T19:37:17Z" level=info msg="Running command docker build --pull -f Dockerfile -t mycontainerregistry.azurecr.io/helloworld:aa2 ."
Sending build context to Docker daemon  209.9kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
Status: Image is up to date for node:9-alpine
 ---> 7af437a39ec2
Step 2/5 : COPY . /src
 ---> 48a7735fa94e

[...]

26b0c207c4a9: Pushed
917e7cdebc8b: Pushed
aa2: digest: sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89 size: 1367
time="2018-05-10T19:37:57Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistrtyy.azurecr.io/helloworld:aa2"
"["mycontainerregistrtyy.azurecr.io/helloworld@sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89"]"
time="2018-05-10T19:37:57Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistrtyy.azurecr.io
    repository: helloworld
    tag: aa2
    digest: sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
  git:
    git-head-revision: 920f16cfafa36d0bc3f397c3dd48185a03499404

Build complete
Build ID: aa2 was successful after 46.491407373s
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
Showing logs for the last updated build
Build ID: aa3

[...]

Build complete
Build ID: aa3 was successful after 1m14.26397548s
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
Showing logs for the last updated build
Build ID: aa4

[...]

Build complete
Build ID: aa4 was successful after 39.164385024s
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
aa4         buildhelloworld  Linux       Succeeded  Git Commit  2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual      2018-05-10T19:10:14Z  00:00:55
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie mithilfe einer Buildaufgabe automatisch Buildvorgänge für Containerimages auslösen, wenn Sie Quellcode in einem Git-Repository committen. Im nächsten Tutorial erfahren Sie, wie Sie Buildaufgaben erstellen, die Buildvorgänge auslösen, wenn das Basisimage eines Containerimages aktualisiert wird.

> [!div class="nextstepaction"]
> [Automatisieren von Buildvorgängen für Images nach der Aktualisierung des Basisimages mit Azure Container Registry Build](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
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
