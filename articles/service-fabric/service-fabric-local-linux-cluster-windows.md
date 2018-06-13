---
title: Einrichten eines Azure Service Fabric-Linux-Clusters unter Windows | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Service Fabric-Linux-Cluster einrichten, die auf Windows-Entwicklungscomputern ausgeführt werden. Dies ist insbesondere für die plattformübergreifende Entwicklung von Nutzen.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: mfussell
editor: ''
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/20/2017
ms.author: suhuruli
ms.openlocfilehash: ad0d383888c173ece5a7fbd3b0de690ed13074f7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206292"
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Einrichten eines Linux-Service Fabric-Clusters auf Ihrem Windows-Entwicklungscomputer

In diesem Dokument wird beschrieben, wie Sie einen lokalen Linux-Service Fabric-Cluster auf Windows-Entwicklungscomputern einrichten. Das Einrichten eines lokalen Linux-Clusters ist für das schnelle Testen von Anwendungen, die für Linux-Cluster konzipiert sind, aber auf einem Windows-Computer entwickelt werden.

## <a name="prerequisites"></a>Voraussetzungen
Linux-basierte Service Fabric-Cluster werden nicht nativ unter Windows ausgeführt. Für die Ausführung eines lokalen Service Fabric-Clusters wird ein vorkonfiguriertes Docker-Containerimage bereitgestellt. Bevor Sie beginnen, benötigen Sie Folgendes:

* Mindestens 4 GB RAM
* Neueste Version von [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)

>[!TIP]
> * Sie können die in der offiziellen Docker-[Dokumentation](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) beschriebenen Schritte ausführen, um Docker auf Ihrem Windows-Computer zu installieren. 
> * Nachdem Sie die Installation abgeschlossen haben, können Sie mit [diesen Schritten](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine) überprüfen, ob alles richtig installiert wurde.


## <a name="create-a-local-container-and-setup-service-fabric"></a>Erstellen eines lokalen Containers und Einrichten von Service Fabric
Führen Sie die folgenden Schritte in PowerShell aus, um einen lokalen Docker-Container einzurichten und darin einen Service Fabric-Cluster auszuführen:

1. Rufen Sie das Image per Pullvorgang aus dem Docker-Hub-Repository ab:

    ```powershell
    docker pull microsoft/service-fabric-onebox
    ```

2. Aktualisieren Sie wie folgt die Konfiguration des Docker-Daemons auf dem Host, und starten Sie den Docker-Daemon neu: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    Es wird empfohlen, zum Aktualisieren auf das Docker-Symbol und auf „Einstellungen“ > „Daemon“ > „Erweitert“ zu klicken und dann das Update auszuführen. Starten Sie dann den Docker-Daemon neu, damit die Änderungen wirksam werden. 

3. Starten Sie eine Service Fabric-One-box-Containerinstanz mit dem Image:

    ```powershell
    docker run -itd -p 19080:19080 --name sfonebox microsoft/service-fabric-onebox
    ```
    >[!TIP]
    > * Indem Sie einen Namen für Ihre Containerinstanz angeben, verbessern Sie die Lesbarkeit. 
    > * Wenn Ihre Anwendung an bestimmten Ports lauscht, müssen diese mit zusätzlichen Tags vom Typ „-p“ angegeben werden. Beispiel: Wenn Ihre Anwendung an Port 8080 lauscht, führen Sie „docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox microsoft/service-fabric-onebox“ aus.

4. Melden Sie sich im interaktiven SSH-Modus am Docker-Container an:

    ```powershell
    docker exec -it sfonebox bash
    ```

5. Führen Sie das Setupskript aus, mit dem die erforderlichen Abhängigkeiten abgerufen werden, und starten Sie den Cluster anschließend im Container.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

6. Nachdem Schritt 5 erfolgreich abgeschlossen wurde, können Sie von Ihrem Windows-Computer aus auf ``http://localhost:19080`` zugreifen und den Service Fabric Explorer anzeigen. Jetzt können Sie mit beliebigen Tools von Ihrem Windows-Entwicklungscomputer aus eine Verbindung mit diesem Cluster herstellen und Anwendungen für Linux-Service Fabric-Cluster bereitstellen. 

    > [!NOTE]
    > Das Eclipse-Plug-In wird unter Windows aktuell nicht unterstützt. 

## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte mit [Eclipse](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-eclipse)
* Weitere [Java-Beispiele](https://github.com/Azure-Samples/service-fabric-java-getting-started) anzeigen


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png