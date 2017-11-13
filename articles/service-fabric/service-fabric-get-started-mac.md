---
title: "Einrichten Ihrer Entwicklungsumgebung unter Mac OS X für die Verwendung mit Azure Service Fabric | Microsoft-Dokumentation"
description: "Installieren Sie Laufzeit, SDK und Tools, und erstellen Sie einen lokalen Entwicklungscluster. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen unter Mac OS X beginnen."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/31/2017
ms.author: saysa
ms.openlocfilehash: cf67c377cd5c17f7b540fb23af48a333a9cddf69
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Einrichten Ihrer Entwicklungsumgebung unter Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Sie können Service Fabric-Anwendungen für die Ausführung in Linux-Clustern unter Mac OS X erstellen. In diesem Artikel erfahren Sie, wie Sie Ihren Mac für die Entwicklung einrichten.

## <a name="prerequisites"></a>Voraussetzungen
Service Fabric wird nicht nativ unter OS X ausgeführt. Für die Verwendung eines lokalen Service Fabric-Clusters stellen wir ein vorkonfiguriertes Docker-Containerimage bereit. Bevor Sie beginnen, benötigen Sie Folgendes:

* Mindestens 4 GB RAM
* Neueste Version von [Docker](https://www.docker.com/)
* Zugriff auf ein [Docker-Containerimage](https://hub.docker.com/r/servicefabricoss/service-fabric-onebox/) vom Typ „Service Fabric One-box“

>[!TIP]
> * Sie können die Schritte ausführen, die in der offiziellen Docker-[Dokumentation](https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install) beschrieben sind, um Docker auf Ihrem Mac zu installieren. 
> * Nachdem Sie die Installation abgeschlossen haben, können Sie mit [diesen Schritten](https://docs.docker.com/docker-for-mac/#check-versions-of-docker-engine-compose-and-machine) überprüfen, ob alles richtig installiert wurde.


## <a name="create-a-local-container-and-setup-service-fabric"></a>Erstellen eines lokalen Containers und Einrichten von Service Fabric
Führen Sie die folgenden Schritte aus, um einen lokalen Docker-Container einzurichten und darin einen Service Fabric-Cluster auszuführen:

1. Rufen Sie das Image per Pullvorgang aus dem Docker-Hub-Repository ab:

    ```bash
    docker pull servicefabricoss/service-fabric-onebox
    ```

2. Starten Sie eine Service Fabric-One-box-Containerinstanz mit dem Image:

    ```bash
    docker run -itd -p 19080:19080 --name sfonebox servicefabricoss/service-fabric-onebox
    ```
    >[!TIP]
    >Indem Sie einen Namen für Ihre Containerinstanz angeben, verbessern Sie die Lesbarkeit. 

3. Melden Sie sich im interaktiven SSH-Modus am Docker-Container an:

    ```bash
    docker exec -it sfonebox bash
    ```

4. Führen Sie das Setupskript aus, mit dem die erforderlichen Abhängigkeiten abgerufen werden, und starten Sie den Cluster anschließend im Container.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

5. Nachdem Schritt 4 erfolgreich abgeschlossen wurde, können Sie von Ihrem Macintosh-Computer aus auf ``http://localhoist:19080`` zugreifen und den Service Fabric Explorer anzeigen.


## <a name="set-up-the-service-fabric-cli-sfctl-on-your-mac"></a>Einrichten der Service Fabric-Befehlszeilenschnittstelle (sfctl) auf Ihrem Mac

Befolgen Sie die Anweisungen unter [Service Fabric-Befehlszeilenschnittstelle](service-fabric-cli.md#cli-mac), um die Service Fabric-Befehlszeilenschnittstelle (`sfctl`) auf Ihrem Mac zu installieren.
Die Befehlszeilenschnittstelle enthält Befehle für die Interaktion mit Service Fabric-Entitäten wie Clustern, Anwendungen und Diensten.

## <a name="create-application-on-your-mac-using-yeoman"></a>Erstellen einer Anwendung auf Ihrem Mac mit Yeoman

Die Gerüstbautools von Service Fabric unterstützen Sie beim Erstellen einer Service Fabric-Anwendung über das Terminal unter Verwendung des Yeoman-Vorlagengenerators. Führen Sie die folgenden Schritte aus, um sicherzustellen, dass der Service Fabric-Yeoman-Vorlagengenerator auf dem Computer verwendet werden kann.

1. Auf Ihrem Mac müssen Node.js und npm installiert sein. Andernfalls können Sie Node.js und npm wie folgt mit Homebrew installieren. Um die auf Ihrem Mac installierten Versionen von Node.js und npm zu überprüfen, können Sie die ``-v``-Option verwenden.

    ```bash
    brew install node
    node -v
    npm -v
    ```
2. Installieren Sie den [Yeoman](http://yeoman.io/)-Vorlagengenerator auf dem Computer über npm.

    ```bash
    npm install -g yo
    ```
3. Installieren Sie den gewünschten Yeoman-Generator gemäß der Anleitung in der [Dokumentation zu den ersten Schritten](service-fabric-get-started-linux.md). Führen Sie zum Erstellen der Service Fabric-Anwendungen mit Yeoman die folgenden Schritte aus:

    ```bash
    npm install -g generator-azuresfjava       # for Service Fabric Java Applications
    npm install -g generator-azuresfguest      # for Service Fabric Guest executables
    npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
    ```
4. Zur Erstellung einer Service Fabric-Java-Anwendung auf einem Mac müssen JDK 1.8 und Gradle auf dem Hostcomputer installiert sein. Falls dies noch nicht der Fall ist, können Sie [Homebrew](https://brew.sh/) für die Installation verwenden. 

    ```bash
    brew update
    brew cask install java
    brew install gradle
    ```

## <a name="deploy-application-on-your-mac-from-terminal"></a>Bereitstellen der Anwendung auf Ihrem Mac über das Terminal

Nachdem Sie Ihre Service Fabric-Anwendung erstellt und den Buildvorgang durchgeführt haben, können Sie sie per [Service Fabric-Befehlszeilenschnittstelle](service-fabric-cli.md#cli-mac) mit den folgenden Schritten bereitstellen:

1. Stellen Sie eine Verbindung mit dem Service Fabric-Cluster her, der unter der Containerinstanz auf Ihrem Mac ausgeführt wird.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Navigieren Sie zu Ihrem Projektverzeichnis, und führen Sie das Installationsskript aus.

    ```bash
    cd MyProject
    bash install.sh
    ```

## <a name="set-up-net-core-20-development"></a>Einrichten der .NET Core 2.0-Entwicklung

Installieren Sie das [.NET Core 2.0 SDK für Mac](https://www.microsoft.com/net/core#macos), um mit dem [Erstellen von Service Fabric-C#-Anwendungen](service-fabric-create-your-first-linux-application-with-csharp.md) zu beginnen. Pakete für Service Fabric-Anwendungen mit .NET Core 2.0 werden auf NuGet.org gehostet (derzeit in der Vorschauphase).

## <a name="install-the-service-fabric-plugin-for-eclipse-neon-on-your-mac"></a>Installieren des Service Fabric-Plug-Ins für Eclipse Neon auf Ihrem Mac

Service Fabric umfasst ein Plug-In für die **Eclipse Neon-IDE für Java**, das die Erstellung und Bereitstellung von Java-Diensten vereinfachen kann. Sie können die Installationsschritte in dieser allgemeinen [Dokumentation](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) für die Installation und Aktualisierung des Service Fabric-Plug-Ins für Eclipse auf die aktuelle Version ausführen.

Alle anderen Schritte in der [Dokumentation zum Service Fabric-Plug-In für Eclipse](service-fabric-get-started-eclipse.md) zum Erstellen einer Anwendung, Hinzufügen eines Diensts zur Anwendung, Installieren/Deinstallieren der Anwendung usw. gelten hier ebenfalls.

Neben den obigen Schritten sollten Sie den Container wie folgt mit einem Pfad instanziieren, der mit Ihrem Host gemeinsam verwendet wird, damit das Service Fabric-Plug-In für Eclipse mit dem Docker-Container auf Ihrem Mac funktioniert:
```bash
docker run -itd -p 19080:19080 -v /Users/sayantan/work/workspaces/mySFWorkspace:/tmp/mySFWorkspace --name sfonebox servicefabricoss/service-fabric-onebox
```
Hierbei ist ``/Users/sayantan/work/workspaces/mySFWorkspace`` der vollqualifizierte Pfad des Arbeitsbereichs auf dem Mac, und ``/tmp/mySFWorkspace`` ist der Pfad für die Zuordnung im Container.

> [!NOTE]
>1. Wenn der Name bzw. Pfad Ihres Arbeitsbereichs hiervon abweicht, können Sie dies im obigen Befehl ``docker run`` entsprechend angeben.
>2. Falls Sie den Container mit einem anderen Namen als ``sfonebox`` starten, sollten Sie dies in der Datei ``testclient.sh`` in Ihrer Service Fabric Actor-Java-Anwendung aktualisieren.

## <a name="next-steps"></a>Nächste Schritte
<!-- Links -->
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe von Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe des Service Fabric-Plug-Ins für Eclipse](service-fabric-get-started-eclipse.md)
* [Erstellen eines Service Fabric-Clusters in Azure über das Azure-Portal](service-fabric-cluster-creation-via-portal.md)
* [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Modellieren von Anwendungen in Service Fabric](service-fabric-application-model.md)
* [Verwalten von Anwendungen mit der Service Fabric-Befehlszeilenschnittstelle](service-fabric-application-lifecycle-sfctl.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
