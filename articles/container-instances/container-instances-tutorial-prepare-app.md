---
title: "Azure Container Instances-Tutorial – Vorbereiten Ihrer App"
description: "Tutorial für Azure Container Instances (Teil 1 von 3) – Vorbereiten einer App für die Breitstellung in Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: fc16be80e776d1472be775fa32354ba157d16545
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Erstellen von Containern für die Bereitstellung in Azure Container Instances

Azure Container Instances ermöglicht die Bereitstellung von Docker-Containern in der Azure-Infrastruktur ohne Bereitstellung virtueller Computer oder Einführung eines übergeordneten Diensts. In diesem Tutorial erstellen Sie eine kleine Webanwendung in Node.js und verpacken sie in einem Container, der mithilfe von Azure Container Instances ausgeführt werden kann.

Dieser Artikel ist der erste Teil der Reihe und umfasst Folgendes:

> [!div class="checklist"]
> * Klonen des Anwendungsquellcodes von GitHub
> * Erstellen eines Containerimages aus der Anwendungsquelle
> * Testen der Images in einer lokalen Docker-Umgebung

In späteren Tutorials laden Sie eigene Images in eine Azure Container Registry-Instanz hoch und stellen sie in Azure Container Instances bereit.

## <a name="before-you-begin"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie mindestens Version 2.0.23 der Azure-Befehlszeilenschnittstelle. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0][azure-cli-install].

In diesem Tutorial wird vorausgesetzt, dass zentrale Docker-Begriffe wie Container und Containerimages sowie grundlegende `docker`-Befehle bekannt sind. Eine Einführung in die Grundlagen der Container finden Sie bei Bedarf unter [Get started with Docker][docker-get-started] (Erste Schritte mit Docker).

Für dieses Tutorial ist eine lokal installierte Docker-Entwicklungsumgebung erforderlich. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac-][docker-mac], [Windows-][docker-windows] oder [Linux-][docker-linux]System konfiguriert werden kann.

Azure Cloud Shell umfasst keine Docker-Komponenten, die zum Abschließen der einzelnen Schritte dieses Tutorials erforderlich sind. Die Azure-Befehlszeilenschnittstelle und die Docker-Entwicklungsumgebung müssen zur Absolvierung dieses Tutorials auf Ihrem lokalen Computer installiert sein.

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Das Beispiel in diesem Tutorial enthält eine einfache, in [Node.js][nodejs] erstellte Webanwendung. Die App stellt eine statische HTML-Seite bereit und sieht wie folgt aus:

![Tutorial-App im Browser][aci-tutorial-app]

Laden Sie das Beispiel mithilfe von „git“ herunter:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Erstellen des Containerimages

Die Dockerfile-Datei aus dem Beispielrepository zeigt, wie der Container erstellt wird. Ausgangspunkt ist ein [offizielles Node.js-Image][docker-hub-nodeimage] auf der Grundlage von [Alpine Linux][alpine-linux] (einer kleinen Distribution, die sich gut für Container eignet). Anschließend werden die Anwendungsdateien in den Container kopiert und Abhängigkeiten mithilfe des Knotenpaket-Managers installiert, bevor schließlich die Anwendung gestartet wird.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Verwenden Sie den Befehl [docker build][docker-build], um das Containerimage zu erstellen, und kennzeichnen Sie es als *aci-tutorial-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Die Ausgabe des Befehls [docker build][docker-build] ähnelt dem folgenden (zur besseren Lesbarkeit gekürzten) Beispiel:

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.9.3-alpine
8.9.3-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.9.3-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Verwenden Sie den Befehl [docker images][docker-images], um das erstellte Image anzuzeigen:

```bash
docker images
```

Ausgabe:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a>Lokales Ausführen des Containers

Bevor Sie den Container für Azure Container Instances bereitstellen, führen Sie ihn lokal aus, um sich zu vergewissern, dass er funktioniert. Mit dem Switch `-d` können Sie den Container im Hintergrund ausführen, und mit `-p` können Sie einen beliebigen Port auf Ihrem Computer dem Port 80 des Containers zuordnen.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Rufen Sie im Browser „http://localhost: 8080“ auf, um sich zu vergewissern, dass der Container ausgeführt wird.

![Lokales Ausführen der App im Browser][aci-tutorial-app-local]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Containerimage erstellt, das in Azure Container Instances bereitgestellt werden kann. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Anwendungsquelle von GitHub wurde geklont.
> * Containerimages aus der Anwendungsquelle wurden erstellt.
> * Container wurde lokal getestet.

Wechseln Sie zum nächsten Tutorial, und erfahren Sie, wie Containerimages in einer Azure Container Registry gespeichert werden.

> [!div class="nextstepaction"]
> [Übertragen von Images zu Azure Container Registry mithilfe von Push](./container-instances-tutorial-prepare-acr.md)

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png

<!-- LINKS - External -->
[alpine-linux]: https://alpinelinux.org/
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: http://nodejs.org

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
