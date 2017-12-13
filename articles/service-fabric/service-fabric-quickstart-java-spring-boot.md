---
title: Bereitstellen einer Spring Boot-Anwendung in Azure Service Fabric | Microsoft-Dokumentation
description: "Bereitstellen einer Spring Boot-Anwendung für Azure Service Fabric mithilfe von „Spring Boot Getting Started“."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 002841ff59e7b151ee2288ee4045de5c423df573
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="deploy-a-spring-boot-application"></a>Bereitstellen einer Spring Boot-Anwendung
Azure Service Fabric ist eine Plattform, mit der verteilte Systeme bereitgestellt und skalierbare und zuverlässige Microservices und Container verwaltet werden können. 

In diesem Schnellstart wird gezeigt, wie Sie eine Spring Boot-Anwendung in Service Fabric bereitstellen. In diesem Schnellstart wird das Beispiel [Getting Started](https://spring.io/guides/gs/spring-boot/) von der Spring-Website verwendet. Unter Verwendung bekannter Befehlszeilentools wird in diesem Schnellstart beschrieben, wie Sie das Spring Boot-Beispiel als Service Fabric-Anwendung bereitstellen. Am Ende des Schnellstarts wird das Spring Boot Getting Started-Beispiel in Service Fabric ausgeführt. 

![Screenshot der Anwendung](./media/service-fabric-quickstart-java-spring-boot/springbootsf.png)

In dieser Schnellstartanleitung wird Folgendes vermittelt:

> [!div class="checklist"]
> * Bereitstellen einer Spring Boot-Anwendung in Service Fabric
> * Bereitstellen der Anwendung im Cluster 
> * Bereitstellen der Anwendung in einem Cluster in Azure
> * Horizontales Hochskalieren der Anwendung über mehrere Knoten hinweg
> * Ausführen eines Failovers für den Dienst ohne Verfügbarkeitstreffer

## <a name="prerequisites"></a>Voraussetzungen
So führen Sie diesen Schnellstart durch:
1. [Installieren Sie das Service Fabric SDK und die Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Installieren Sie Git.](https://git-scm.com/)
3. [Installieren Sie Yeoman](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables).
4. [Richten Sie die Java-Umgebung ein](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development).

## <a name="download-the-sample"></a>Herunterladen des Beispiels
Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Spring Boot Getting Started-Beispiel auf Ihrem lokalen Computer zu klonen.
```
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="package-the-spring-boot-application"></a>Packen der Spring Boot-Anwendung 
1. Führen Sie in dem soeben geklonten Verzeichnis `gs-spring-boot` den Befehl `yo azuresfguest` aus. 

2. Geben Sie die folgenden Details für die einzelnen Eingabeaufforderungen ein. 

    ![Yeoman-Einträge](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

3. Erstellen Sie im Ordner `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` eine Datei mit dem Namen `entryPoint.sh`. Fügen Sie der Datei folgenden Code hinzu. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

Damit haben Sie eine Service Fabric-Anwendung für das Spring Boot Getting Started-Beispiel erstellt, die Sie in Service Fabric bereitstellen können.

## <a name="run-the-application-locally"></a>Lokales Ausführen der Anwendung
1. Starten Sie Ihren lokalen Cluster mit dem folgenden Befehl:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Es kann einige Zeit dauern, bis der lokale Cluster startet. Um zu prüfen, ob der Cluster vollständig betriebsbereit ist, greifen Sie unter **http://localhost:19080** auf Service Fabric Explorer zu. Wenn die fünf Knoten fehlerfrei sind, wird der lokale Cluster ausgeführt. 
    
    ![Fehlerfreier lokaler Cluster](./media/service-fabric-quickstart-java/localclusterup.png)

2. Navigieren Sie zum Ordner `gs-spring-boot/SpringServiceFabric`.
3. Führen Sie den folgenden Befehl aus, um eine Verbindung mit dem lokalen Cluster herzustellen. 

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```
4. Führen Sie das Skript `install.sh` aus. 

    ```bash
    ./install.sh
    ```

5. Öffnen Sie Ihren bevorzugten Browser, und greifen Sie über **http://localhost:8080** auf die Anwendung zu. 

    ![Lokales Front-End der Anwendung](./media/service-fabric-quickstart-java-spring-boot/springbootsf.png)
    
Sie können nun auf die Spring Boot-Anwendung zugreifen, die in einem Service Fabric-Cluster bereitgestellt wurde.  

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung für Azure

### <a name="set-up-your-azure-service-fabric-cluster"></a>Einrichten des Azure Service Fabric-Clusters
Um die Anwendung in einem Cluster in Azure bereitzustellen, erstellen Sie einen eigenen Cluster.

Bei Partyclustern handelt es sich um zeitlich begrenzte kostenlose Service Fabric-Cluster, die in Azure gehostet werden. Sie werden vom Service Fabric-Team ausgeführt, und alle Interessenten können Anwendungen bereitstellen und sich mit der Plattform vertraut machen. [Befolgen Sie die Anweisungen](http://aka.ms/tryservicefabric), um Zugriff auf einen Partycluster zu erhalten. 

Informationen zum Erstellen Ihres eigenen Clusters finden Sie unter [Bereitstellen eines Service Fabric-Linux-Clusters in einem virtuellen Azure-Netzwerk](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

> [!Note]
> Der Spring Boot-Dienst ist für das Lauschen auf eingehenden Datenverkehr über Port 8080 konfiguriert. Stellen Sie sicher, dass der Port in Ihrem Cluster geöffnet ist. Wenn Sie den Partycluster verwenden, ist dieser Port geöffnet.
>

### <a name="deploy-the-application-using-cli"></a>Bereitstellen der Anwendung über die Befehlszeilenschnittstelle
Nachdem die Anwendung und Ihr Cluster nun bereitstehen, können Sie die Anwendung direkt über die Befehlszeile im Cluster bereitstellen.

1. Navigieren Sie zum Ordner `gs-spring-boot/SpringServiceFabric`.
2. Führen Sie den folgenden Befehl aus, um eine Verbindung mit dem Azure-Cluster herzustellen. 

    ```bash
    sfctl cluster select --endpoint http://<ConnectionIPOrURL>:19080
    ```
    
    Wenn der Cluster mit einem selbstsignierten Zertifikat gesichert ist, muss folgender Befehl ausgeführt werden: 

    ```bash
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify
    ```
3. Führen Sie das Skript `install.sh` aus. 

    ```bash
    ./install.sh
    ```

4. Öffnen Sie Ihren bevorzugten Browser, und greifen Sie über **http://\<VerbindungsIPoderURL>:8080** auf die Anwendung zu. 

    ![Lokales Front-End der Anwendung](./media/service-fabric-quickstart-java-spring-boot/springsfazure.png)
    
Sie können nun auf die Spring Boot-Anwendung zugreifen, die in einem Service Fabric-Cluster bereitgestellt wurde.  
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Skalieren von Anwendungen und Diensten in einem Cluster
Dienste können clusterweit skaliert werden, um eine Änderung der Last für die Dienste auszugleichen. Sie skalieren einen Dienst, indem Sie die Anzahl von Instanzen ändern, die im Cluster ausgeführt werden. Für das Skalieren gibt es verschiedene Möglichkeiten. Sie können z.B. Skripts oder Befehle der Service Fabric-CLI (sfctl) verwenden. In diesem Beispiel verwenden wir Service Fabric Explorer.

Service Fabric Explorer wird in allen Service Fabric-Clustern ausgeführt und ist über einen Browser zugänglich, indem auf den HTTP-Verwaltungsport (19080) der Cluster zugegriffen wird, z.B. `http://demolinuxsecure.westus.cloudapp.azure.com:19080`.

Führen Sie die folgenden Schritte aus, um den Web-Front-End-Dienst zu skalieren:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://demolinuxsecure.westus.cloudapp.azure.com:19080`.
2. Klicken Sie in der Strukturansicht auf das Auslassungszeichen (drei Punkte) neben dem Knoten **fabric:/SpringServiceFabric/SpringGettingStarted**, und wählen Sie **Scale Service** (Dienst skalieren) aus.

    ![Service Fabric Explorer – Dienst skalieren](./media/service-fabric-quickstart-java-spring-boot/springbootsfhowtoscale.png)

    Sie können jetzt angeben, wie viele Instanzen des Diensts skaliert werden sollen.

3. Ändern Sie die Anzahl in **5**, und klicken Sie auf **Scale Service** (Dienst skalieren).

    Alternativ kann der Dienst wie folgt über eine Befehlszeile skaliert werden.

    ```bash 
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted` --instance-count 5 --stateless 
    ``` 

4. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/SpringServiceFabric/SpringGettingStarted**, und erweitern Sie den Partitionsknoten (durch eine GUID dargestellt).

    ![Service Fabric Explorer: Dienstskalierung abgeschlossen](./media/service-fabric-quickstart-java-spring-boot/springsfscaled.png)

    Sie sehen nun, dass der Dienst über fünf Instanzen verfügt, und in der Strukturansicht ist zu erkennen, auf welchen Knoten die Instanzen ausgeführt werden.

Mit dieser einfachen Verwaltungsaufgabe haben wir die Ressourcen erhöht, die für den Spring-Dienst zum Verarbeiten der Benutzerauslastung verfügbar sind. Es ist wichtig zu verstehen, dass Sie nicht mehrere Instanzen eines Diensts benötigen, damit er zuverlässig ausgeführt wird. Wenn ein Dienst ausfällt, wird von Service Fabric sichergestellt, dass im Cluster eine neue Dienstinstanz ausgeführt wird.

## <a name="failover-services-in-a-cluster"></a>Failoverdienste in einem Cluster 
Zur Veranschaulichung eines Dienstfailovers kann unter Verwendung von Service Fabric Explorer ein Neustart des Knotens simuliert werden. Stellen Sie sicher, dass nur 1 Instanz des Diensts ausgeführt wird. 

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://demolinuxsecure.westus.cloudapp.azure.com:19080`.
2. Klicken Sie auf das Auslassungszeichen (drei Punkte) neben dem Knoten, auf dem die Instanz des Diensts ausgeführt wird, und starten Sie den Knoten neu. 

    ![Service Fabric Explorer – Neustarten des Knotens](./media/service-fabric-quickstart-java-spring-boot/springbootsfrestart.png)
3. Die Instanz des Diensts wird nun auf einen anderen Knoten verschoben. Die Anwendung weist keine Ausfallzeit auf. 

    ![Service Fabric Explorer – Neustart des Knotens erfolgreich](./media/service-fabric-quickstart-java-spring-boot/springbootsfrestartsucceed.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen einer Spring Boot-Anwendung in Service Fabric
> * Bereitstellen der Anwendung im Cluster 
> * Bereitstellen der Anwendung in einem Cluster in Azure
> * Horizontales Hochskalieren der Anwendung über mehrere Knoten hinweg
> * Ausführen eines Failovers für den Dienst ohne Verfügbarkeitstreffer

* Weitere Informationen zum [Erstellen von Java-Microservices mithilfe von Service Fabric-Programmiermodellen](service-fabric-quickstart-java-reliable-services.md)
* Weitere Informationen zum [Einrichten von Continuous Integration & Deployment mithilfe von Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
* Sehen Sie sich die anderen [Java-Beispiele](https://github.com/Azure-Samples/service-fabric-java-getting-started) an