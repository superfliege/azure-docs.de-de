---
title: 'Tutorial: Automatisieren von Buildvorgängen für Containerimages nach der Aktualisierung des Basisimages mit Azure Container Registry Build'
description: In diesem Tutorial erfahren Sie, wie Sie eine Buildaufgabe so konfigurieren, dass automatisch Buildvorgänge für Containerimages in der Cloud ausgelöst werden, wenn ein Basisimage aktualisiert wird.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 531aeaacf0bd70521d70afb45d141fc3296ebb04
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34058113"
---
# <a name="tutorial-automate-image-builds-on-base-image-update-with-azure-container-registry-build"></a>Tutorial: Automatisieren von Buildvorgängen für Images nach der Aktualisierung des Basisimages mit Azure Container Registry Build

ACR Build unterstützt das automatisierte Ausführen eines Buildvorgangs, wenn das Basisimage eines Containers aktualisiert wird – beispielsweise, wenn Sie das Betriebssystem oder das Anwendungsframework in einem Ihrer Basisimages patchen. In diesem Tutorial erfahren Sie, wie Sie eine Buildaufgabe in ACR Build erstellen, die einen Buildvorgang in der Cloud auslöst, wenn das Basisimage eines Containers an Ihre Registrierung gepusht wurde.

Dieser letzte Teil der Tutorialreihe umfasst Folgendes:

> [!div class="checklist"]
> * Erstellen des Basisimages
> * Erstellen einer Buildaufgabe für ein Anwendungsimage
> * Aktualisieren des Basisimages, um einen Buildvorgang für das Anwendungsimage auszulösen
> * Anzeigen des ausgelösten Buildvorgangs
> * Überprüfen des aktualisierten Anwendungsimages

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Azure CLI lokal verwenden möchten, muss bei Ihnen mindestens die Azure CLI-Version **2.0.32** installiert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und Aktualisierung der CLI finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0][azure-cli].

## <a name="prerequisites"></a>Voraussetzungen

### <a name="complete-previous-tutorials"></a>Absolvieren der vorherigen Tutorials

In diesem Tutorial wird vorausgesetzt, dass Sie bereits die Schritte aus den ersten beiden Tutorials der Reihe absolviert haben:

* Erstellen einer Azure-Containerregistrierung
* Forken des Beispielrepositorys
* Klonen des Beispielrepositorys
* Erstellen eines persönlichen GitHub-Zugriffstokens

Absolvieren Sie bei Bedarf die ersten beiden Tutorials, bevor Sie mit diesem Tutorial fortfahren:

[Erstellen von Containerimages in der Cloud mit Azure Container Registry Build](container-registry-tutorial-quick-build.md)

[Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Build](container-registry-tutorial-build-task.md)

### <a name="configure-environment"></a>Konfigurieren der Umgebung

Geben Sie für die folgenden Shell-Umgebungsvariablen geeignete Werte für Ihre Umgebung an. Dies ist zwar nicht zwingend erforderlich, vereinfacht aber das Ausführen der mehrzeiligen Azure CLI-Befehle in diesem Tutorial. Wenn Sie diese Werte nicht angeben, müssen Sie sie später jedes Mal manuell ersetzen, wenn sie in einem der Beispielbefehle vorkommen.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Basisimages

Die meisten Containerimages werden mithilfe von Dockerfiles definiert. Dockerfiles geben als Basis ein übergeordnetes Image an, das häufig als *Basisimage* bezeichnet wird. Basisimages enthalten in der Regel das Betriebssystem (also beispielsweise [Alpine Linux][base-alpine] oder [Windows Nano Server][base-windows]), auf das die restlichen Ebenen des Containers angewendet werden. Sie können auch Anwendungsframeworks wie [Node.js][base-node] oder [.NET Core][base-dotnet] enthalten.

### <a name="base-image-updates"></a>Basisimageaktualisierungen

Ein Basisimage wird häufig von der für das Image zuständigen Person aktualisiert, um neue Features oder Verbesserungen für das enthaltene Betriebssystem oder Framework zu integrieren. Ein weiterer häufiger Grund für die Aktualisierung eines Basisimages sind Sicherheitspatches.

Wenn ein Basisimage aktualisiert wird, müssen sämtliche darauf basierende Containerimages in Ihrer Registrierung neu erstellt werden, um die neuen Features und Korrekturen zu integrieren. ACR Build ermöglicht die automatische Erstellung von Images nach der Aktualisierung eines Basisimages.

### <a name="base-image-update-scenario"></a>Aktualisierungsszenario für das Basisimage

In diesem Tutorial durchlaufen Sie ein Aktualisierungsszenario für ein Basisimage. Das [Codebeispiel][code-sample] enthält zwei Dockerfiles: ein Anwendungsimage und ein als Basis angegebenes Image. In den folgenden Abschnitten erstellen Sie eine ACR Build-Aufgabe, die automatisch einen Buildvorgang für das Anwendungsimage auslöst, wenn eine neue Version des Basisimages an Ihre Containerregistrierung gepusht wird.

[Dockerfile-app:][dockerfile-app] Eine kleine Node.js-Webanwendung, die eine statische Webseite rendert, auf der die zugrunde liegende Node.js-Version angezeigt wird. Die Versionszeichenfolge wird simuliert: Es wird der Inhalt der im Basisimage definierten Umgebungsvariablen `NODE_VERSION` angezeigt.

[Dockerfile-base:][dockerfile-base] Das Image, das von `Dockerfile-app` als Basis angegeben wird. Es basiert auf einem [Node][base-node]-Image und enthält die Umgebungsvariable `NODE_VERSION`.

In den folgenden Abschnitten erstellen Sie eine Buildaufgabe, aktualisieren den Wert `NODE_VERSION` in der Dockerfile des Basisimages und erstellen anschließend das Basisimage mithilfe von ACR Build. Wenn ACR Build das neue Basisimage an Ihre Registrierung pusht, wird automatisch ein Buildvorgang für das Anwendungsimage ausgelöst. Außerdem können Sie das Containerimage optional lokal ausführen, um sich die verschiedenen Versionszeichenfolgen in den erstellten Images anzusehen.

## <a name="build-base-image"></a>Erstellen des Basisimages

Erstellen Sie zunächst das Basisimage mit der *Schnellerstellung* von ACR Build. Wie im [ersten Tutorial](container-registry-tutorial-quick-build.md) der Reihe bereits erläutert, wird dabei das Image erstellt und an Ihre Containerregistrierung gepusht, sofern der Buildvorgang erfolgreich war.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-build-task"></a>Erstellen der Buildaufgabe

Erstellen Sie als Nächstes mit [az acr build-task create][az-acr-build-task-create] eine Buildaufgabe:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --build-arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

Diese Buildaufgabe ähnelt der Aufgabe, die Sie im [vorherigen Tutorial](container-registry-tutorial-build-task.md) erstellt haben. Sie weist ACR Build an, einen Buildvorgang für ein Image auszulösen, wenn Commits an das durch `--context` angegebene Repository gepusht werden.

Im Gegensatz zur anderen Aufgabe löst diese Buildaufgabe allerdings auch einen Buildvorgang für das Image aus, wenn dessen *Basisimage* aktualisiert wird. Die durch das Argument `--file` angegebene Dockerfile ([Dockerfile-app][dockerfile-app]) unterstützt die Angabe eines Images aus der gleichen Registrierung als Basis:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Wenn Sie eine Buildaufgabe ausführen, erkennt ACR Build die Abhängigkeiten eines Images. Falls sich das in der Anweisung `FROM` angegebene Basisimage in der gleichen Registrierung befindet, wird ein Hook hinzugefügt, um sicherzustellen, dass dieses Image nach jeder Aktualisierung seiner Basis neu erstellt wird.

> [!NOTE]
> Während der Vorschauphase unterstützt ACR Build die Auslösung eines abgeleiteten Images nur, wenn sich das Basisimage und das darauf verweisende Image in der gleichen Azure-Containerregistrierung befinden.

## <a name="build-application-container"></a>Erstellen des Anwendungscontainers

Damit ACR Build die Abhängigkeiten eines Containerimages (einschließlich des Basisimages) bestimmen und nachverfolgen kann, **müssen** Sie zunächst die dazugehörige Buildaufgabe **mindestens einmal** auslösen.

Verwenden Sie [az acr build-task run][az-acr-build-task-run], um die Buildaufgabe manuell auszulösen und das Anwendungsimage zu erstellen:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Notieren Sie sich nach Abschluss des Buildvorgangs die **Build-ID** (beispielsweise „aa6“), falls Sie den folgenden optionalen Schritt ausführen möchten.

### <a name="optional-run-application-container-locally"></a>Optional: Lokales Ausführen des Anwendungscontainers

Wenn Sie lokal (also nicht in Cloud Shell) arbeiten und Docker installiert ist, können Sie vor der Neuerstellung des Basisimages den Container ausführen, um die Anwendung in einem Webbrowser anzuzeigen. Bei Verwendung von Cloud Shell können Sie diesen Abschnitt überspringen. (Cloud-Shell unterstützt weder `az acr login` noch `docker run`.)

Melden Sie sich zunächst mit [az acr login][az-acr-login] bei Ihrer Containerregistrierung an:

```azurecli
az acr login --name $ACR_NAME
```

Verwenden Sie nun `docker run`, um den Container lokal auszuführen. Ersetzen Sie **\<build-id\>** durch die Build-ID aus der Ausgabe des vorherigen Schritts (beispielsweise „aa6“).

```azurecli
docker run -d -p 8080:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Navigieren Sie in Ihrem Browser zu http://localhost:8080. Daraufhin sollte eine Webseite mit der Node.js-Versionsnummer angezeigt werden, wie im folgenden Beispiel zu sehen. In einem späteren Schritt passen Sie die Version an, indem Sie der Versionszeichenfolge ein „a“ hinzufügen.

![Screenshot der im Browser gerenderten Beispielanwendung][base-update-01]

## <a name="list-builds"></a>Auflisten der Builds

Listen Sie als Nächstes die Builds auf, die ACR Build für Ihre Registrierung fertig gestellt hat:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Wenn Sie das vorherige Tutorial absolviert (und die Registrierung nicht gelöscht) haben, sollte die Ausgabe in etwa wie im folgenden Beispiel aussehen. Notieren Sie sich die Buildanzahl und die neueste Build-ID, um die Angaben nach der Aktualisierung des Basisimages im nächsten Abschnitt vergleichen zu können.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
----------  ---------------  ----------  ---------  ----------  --------------------  ----------
aa6         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T20:00:12Z  00:00:50
aa5                          Linux       Succeeded  Manual      2018-05-10T19:57:35Z  00:00:55
aa4         buildhelloworld  Linux       Succeeded  Git Commit  2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual      2018-05-10T19:10:14Z  00:00:55
```

## <a name="update-base-image"></a>Aktualisieren des Basisimages

Hier simulieren Sie einen Frameworkpatch im Basisimage. Bearbeiten Sie **Dockerfile-base**, indem Sie die in `NODE_VERSION` definierte Versionsnummer um ein „a“ ergänzen:

```Dockerfile
ENV NODE_VERSION 9.11.1a
```

Führen Sie in ACR Build eine Schnellerstellung durch, um das geänderte Basisimage zu erstellen. Notieren Sie sich die **Build-ID** aus der Ausgabe.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Sobald ACR Build das neue Basisimage nach Abschluss des Buildvorgangs an Ihre Registrierung gepusht hat, wird ein Buildvorgang für das Anwendungsimage ausgelöst. Es dauert möglicherweise einen Moment, bis die zuvor erstellte ACR Build-Aufgabe den Buildvorgang für das Anwendungsimage auslöst, da das neue und gepushte Basisimage erkannt werden muss.

## <a name="list-builds"></a>Auflisten der Builds

Listen Sie nach der Aktualisierung des Basisimages erneut Ihre Builds auf, und vergleichen Sie sie mit der vorherigen Liste. Sollte sich die Ausgabe nicht unterscheiden, führen Sie den Befehl in regelmäßigen Abständen erneut aus, bis der neue Build in der Liste angezeigt wird.

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Die Ausgabe sieht in etwa wie folgt aus. Als „TRIGGER“ (Auslöser) für den zuletzt ausgeführten Build sollte „Image Update“ (Imageaktualisierung) angegeben sein, was bedeutet, dass die Buildaufgabe durch die Schnellerstellung des Basisimages ausgelöst wurde.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER       STARTED               DURATION
----------  ---------------  ----------  ---------  ------------  --------------------  ----------
aa8         buildhelloworld  Linux       Succeeded  Image Update  2018-05-10T20:09:52Z  00:00:45
aa7                          Linux       Succeeded  Manual        2018-05-10T20:09:17Z  00:00:40
aa6         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T20:00:12Z  00:00:50
aa5                          Linux       Succeeded  Manual        2018-05-10T19:57:35Z  00:00:55
aa4         buildhelloworld  Linux       Succeeded  Git Commit    2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual        2018-05-10T19:10:14Z  00:00:55
```

Wenn Sie im folgenden optionalen Schritt den neu erstellten Container ausführen möchten, um die aktualisierte Versionsnummer anzuzeigen, notieren Sie sich den Wert unter **BUILD ID** für den durch die Imageaktualisierung ausgelösten Buildvorgang („aa8“ in der obigen Ausgabe).

### <a name="optional-run-newly-built-image"></a>Optional: Ausführen des neu erstellten Images

Wenn Sie lokal (also nicht in Cloud Shell) arbeiten und Docker installiert ist, können Sie nach Abschluss des Buildvorgangs das neue Anwendungsimage ausführen. Ersetzen Sie `<build-id>` durch die Build-ID aus dem vorherigen Schritt. Bei Verwendung von Cloud Shell können Sie diesen Abschnitt überspringen. (Cloud-Shell unterstützt `docker run` nicht.)

```bash
docker run -d -p 8081:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Navigieren Sie in Ihrem Browser zu http://localhost:8081. Daraufhin sollte die Webseite mit der aktualisierten Node.js-Versionsnummer (mit hinzugefügtem „a“) angezeigt werden:

![Screenshot der im Browser gerenderten Beispielanwendung][base-update-02]

Beachten Sie, dass Sie Ihr **Basisimage** mit einer neuen Versionsnummer aktualisiert haben und das zuletzt erstellte **Anwendungsimage** die neue Version anzeigt. ACR Build hat die Änderung aus dem Basisimage übernommen und Ihr Anwendungsimage automatisch neu erstellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Führen Sie die folgenden Befehle aus, um alle Ressourcen zu entfernen, die Sie im Rahmen dieser Tutorialreihe erstellt haben (einschließlich der Containerregistrierung, der Containerinstanz, des Schlüsseltresors und des Dienstprinzipals):

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie mithilfe einer Buildaufgabe automatisch Buildvorgänge für Containerimages auslösen, wenn das Basisimage des Images aktualisiert wurde. Der nächste Artikel enthält Informationen zur Authentifizierung für Ihre Containerregistrierung.

> [!div class="nextstepaction"]
> [Authentifizieren mit einer privaten Docker-Containerregistrierung](container-registry-authentication.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png