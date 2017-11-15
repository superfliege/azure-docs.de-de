---
title: "Azure Container Instances-Tutorial – Vorbereiten Ihrer App"
description: "Vorbereiten einer App für die Bereitstellung in Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 5de53266e1dbadecb9fabb1649615fa9f4ba8b5f
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Erstellen von Containern für die Bereitstellung in Azure Container Instances

Azure Container Instances ermöglicht die Bereitstellung von Docker-Containern in der Azure-Infrastruktur ohne Bereitstellung virtueller Computer oder Einführung eines übergeordneten Diensts. In diesem Tutorial erstellen Sie eine kleine Webanwendung in Node.js und verpacken sie in einem Container, der mithilfe von Azure Container Instances ausgeführt werden kann. Folgendes wird behandelt:

> [!div class="checklist"]
> * Klonen der Anwendungsquelle von GitHub
> * Erstellen von Containerimages aus der Anwendungsquelle
> * Testen der Images in einer lokalen Docker-Umgebung

In späteren Tutorials laden Sie eigene Images in eine Azure Container Registry-Instanz hoch und stellen sie in Azure Container Instances bereit.

## <a name="before-you-begin"></a>Voraussetzungen

Für dieses Tutorial müssen Sie mindestens Version 2.0.20 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu.

In diesem Tutorial wird vorausgesetzt, dass zentrale Docker-Begriffe wie Container und Containerimages sowie grundlegende `docker`-Befehle bekannt sind. Eine Einführung in die Grundlagen der Container finden Sie bei Bedarf unter [Get started with Docker (Erste Schritte mit Docker)]( https://docs.docker.com/get-started/).

Für dieses Tutorial ist eine Docker-Entwicklungsumgebung erforderlich. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- oder [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-System konfiguriert werden kann.

Azure Cloud Shell umfasst keine Docker-Komponenten, die zum Abschließen der einzelnen Schritte dieses Tutorials erforderlich sind. Aus diesem Grund empfehlen wir Ihnen eine lokale Installation der Azure CLI und der Docker-Entwicklungsumgebung.

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Das Beispiel in diesem Tutorial enthält eine einfache, in [Node.js](http://nodejs.org) erstellte Webanwendung. Die App stellt eine statische HTML-Seite bereit und sieht wie folgt aus:

![Tutorial-App im Browser][aci-tutorial-app]

Laden Sie das Beispiel mithilfe von „git“ herunter:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Erstellen des Containerimages

Die Dockerfile-Datei aus dem Beispielrepository zeigt, wie der Container erstellt wird. Ausgangspunkt ist ein [offizielles Node.js-Image][dockerhub-nodeimage] auf der Grundlage von [Alpine Linux](https://alpinelinux.org/) (einer kleinen Distribution, die sich gut für Container eignet). Anschließend werden die Anwendungsdateien in den Container kopiert und Abhängigkeiten mithilfe des Knotenpaket-Managers installiert, bevor schließlich die Anwendung gestartet wird.

```Dockerfile
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Verwenden Sie den Befehl `docker build`, um das Containerimage zu erstellen, und kennzeichnen Sie es als *aci-tutorial-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Die Ausgabe des Befehls `docker build` ähnelt dem folgenden (zur besseren Lesbarkeit gekürzten) Beispiel:

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.2.0-alpine
8.2.0-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.2.0-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Verwenden Sie `docker images`, um das erstellte Image anzuzeigen:

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

In diesem Tutorial haben Sie ein Containerimage erstellt, das in Azure Container Instances bereitgestellt werden kann. Die folgenden Schritte wurden ausgeführt:

> [!div class="checklist"]
> * Anwendungsquelle von GitHub wurde geklont.
> * Containerimages aus der Anwendungsquelle wurden erstellt.
> * Container wurde lokal getestet.

Wechseln Sie zum nächsten Tutorial, und erfahren Sie, wie Containerimages in einer Azure Container Registry gespeichert werden.

> [!div class="nextstepaction"]
> [Übertragen von Images zu Azure Container Registry mithilfe von Push](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://store.docker.com/images/node

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png