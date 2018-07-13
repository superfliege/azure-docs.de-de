---
title: Erstellen einer Spring Boot-App unter Service Fabric in Azure | Microsoft-Dokumentation
description: In diesem Schnellstart stellen Sie eine Spring Boot-Anwendung für Azure Service Fabric bereit, indem Sie eine Spring Boot-Beispielanwendung verwenden.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 83cd90babaa5bcb396f792c7e933d38b3911cebb
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970355"
---
# <a name="quickstart-deploy-a-java-spring-boot-application-to-service-fabric"></a>Schnellstart: Bereitstellen einer Java Spring Boot-Anwendung in Service Fabric

Azure Service Fabric ist eine Plattform, mit der verteilte Systeme bereitgestellt und skalierbare und zuverlässige Microservices und Container verwaltet werden können.

In dieser Schnellstartanleitung wird gezeigt, wie Sie eine Spring Boot-Anwendung in Service Fabric bereitstellen. In diesem Schnellstart wird das Beispiel [Getting Started](https://spring.io/guides/gs/spring-boot/) von der Spring-Website verwendet. In dieser Schnellstartanleitung erfahren Sie anhand vertrauter Befehlszeilentools, wie Sie das Spring Boot-Beispiel als Service Fabric-Anwendung bereitstellen. Am Ende des Schnellstarts wird das Spring Boot Getting Started-Beispiel in Service Fabric ausgeführt.

![Screenshot der Anwendung](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

In dieser Schnellstartanleitung wird Folgendes vermittelt:

* Bereitstellen einer Spring Boot-Anwendung in Service Fabric
* Bereitstellen der Anwendung im Cluster
* Bereitstellen der Anwendung in einem Cluster in Azure
* Horizontales Hochskalieren der Anwendung über mehrere Knoten hinweg
* Ausführen eines Failovers für den Dienst ohne Verfügbarkeitstreffer

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

1. Installieren des Service Fabric SDK und der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI)

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli#cli-mac)
    
    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)

2. [Installation von Git](https://git-scm.com/)
3. Installieren von Yeoman

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)

    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
4. Einrichten der Java-Umgebung

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)
    
    b.  [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development)

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Terminalfenster den folgenden Befehl aus, um das Spring Boot Getting Started-Beispiel auf Ihrem lokalen Computer zu klonen.

```bash
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="build-the-spring-boot-application"></a>Erstellen der Spring Boot-Anwendung 
1. Führen Sie im Verzeichnis `gs-spring-boot/complete` den folgenden Befehl aus, um die Anwendung zu erstellen: 

    ```bash
    ./gradlew build
    ``` 

## <a name="package-the-spring-boot-application"></a>Packen der Spring Boot-Anwendung 
1. Führen Sie in Ihrem Klon im Verzeichnis `gs-spring-boot` den Befehl `yo azuresfguest` aus. 

2. Geben Sie die folgenden Details für die einzelnen Eingabeaufforderungen ein.

    ![Yeoman-Einträge](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

3. Erstellen Sie im Ordner `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` eine Datei mit dem Namen `entryPoint.sh`. Fügen Sie der Datei `entryPoint.sh` folgenden Code hinzu. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

4. Fügen Sie die Ressource **Endpoints** in der Datei `gs-spring-boot/SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/ServiceManifest.xml` hinzu:

    ```xml 
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
    ```

    Die Datei **ServiceManifest.xml** sieht nun wie folgt aus: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceManifest Name="SpringGettingStartedPkg" Version="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

       <ServiceTypes>
          <StatelessServiceType ServiceTypeName="SpringGettingStartedType" UseImplicitHost="true">
       </StatelessServiceType>
       </ServiceTypes>

       <CodePackage Name="code" Version="1.0.0">
          <EntryPoint>
             <ExeHost>
                <Program>entryPoint.sh</Program>
                <Arguments></Arguments>
                <WorkingFolder>CodePackage</WorkingFolder>
             </ExeHost>
          </EntryPoint>
       </CodePackage>
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
     </ServiceManifest>
    ```

Damit haben Sie eine Service Fabric-Anwendung für das Spring Boot Getting Started-Beispiel erstellt, die Sie in Service Fabric bereitstellen können.

## <a name="run-the-application-locally"></a>Lokales Ausführen der Anwendung

1. Starten Sie Ihren lokalen Cluster auf Ubuntu-Computern mit dem folgenden Befehl:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

    Starten Sie bei Verwendung eines Mac den lokalen Cluster über das Docker-Image. (Dabei wird davon ausgegangen, dass die [Voraussetzungen](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-a-local-container-and-set-up-service-fabric) zum Einrichten Ihres lokalen Clusters für Mac erfüllt sind.) 

    ```bash
    docker run --name sftestcluster -d -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 -p 8080:8080 mysfcluster
    ```

    Es kann einige Zeit dauern, bis der lokale Cluster startet. Um zu prüfen, ob der Cluster vollständig betriebsbereit ist, greifen Sie unter **http://localhost:19080** auf Service Fabric Explorer zu. Wenn die fünf Knoten fehlerfrei sind, wird der lokale Cluster ausgeführt. 
    
    ![Fehlerfreier lokaler Cluster](./media/service-fabric-quickstart-java-spring-boot/sfxlocalhost.png)

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

    ![Lokales Front-End der Anwendung](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

Sie können nun auf die Spring Boot-Anwendung zugreifen, die in einem Service Fabric-Cluster bereitgestellt wurde.

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung für Azure

### <a name="set-up-your-azure-service-fabric-cluster"></a>Einrichten des Azure Service Fabric-Clusters

Um die Anwendung in einem Cluster in Azure bereitzustellen, erstellen Sie einen eigenen Cluster.

Bei Partyclustern handelt es sich um zeitlich begrenzte kostenlose Service Fabric-Cluster, die in Azure gehostet und vom Service Fabric-Team betrieben werden. Mithilfe von Partyclustern können Sie Anwendungen bereitstellen und sich mit der Plattform vertraut machen. Der Cluster verwendet ein einzelnes selbstsigniertes Zertifikat für Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit.

Melden Sie sich an, und treten Sie einem [Linux-Cluster](http://aka.ms/tryservicefabric) bei. Klicken Sie auf den Link **PFX**, um das PFX-Zertifikat auf Ihren Computer herunterzuladen. Unter dem Link **ReadMe** finden Sie das Zertifikatkennwort sowie Anweisungen zum Konfigurieren verschiedener Umgebungen für die Verwendung des Zertifikats. Lassen Sie sowohl die**Willkommensseite** als auch die Seite mit der**Infodatei** geöffnet, da Sie einige der Anweisungen in den folgenden Schritten benötigen.

> [!Note]
> Pro Stunde ist eine begrenzte Anzahl von Partyclustern verfügbar. Sollte beim Registrieren für einen Partycluster ein Fehler auftreten, können Sie eine Weile warten und es dann erneut versuchen. Alternativ können Sie die Schritte unter [Tutorial: Bereitstellen eines Service Fabric-Linux-Clusters in einem virtuellen Azure-Netzwerk](service-fabric-tutorial-create-vnet-and-linux-cluster.md) ausführen, um einen Cluster in Ihrem Abonnement zu erstellen.
>
> Der Spring Boot-Dienst ist für das Lauschen auf eingehenden Datenverkehr über Port 8080 konfiguriert. Stellen Sie sicher, dass der Port in Ihrem Cluster geöffnet ist. Wenn Sie den Partycluster verwenden, ist dieser Port geöffnet.
>

Service Fabric bietet mehrere Tools, mit denen Sie einen Cluster und die dazugehörigen Anwendungen verwalten können:

* Service Fabric Explorer: Ein browserbasiertes Tool.
* Service Fabric-Befehlszeilenschnittstelle (CLI): Basiert auf der Azure CLI 2.0.
* PowerShell-Befehle

In dieser Schnellstartanleitung verwenden Sie die Service Fabric-Befehlszeilenschnittstelle sowie Service Fabric Explorer.

Um die Befehlszeilenschnittstelle verwenden zu können, müssen Sie auf der Grundlage der heruntergeladenen PFX-Datei eine PEM-Datei erstellen. Verwenden Sie den folgenden Befehl, um die Datei zu konvertieren. (Für Partycluster können Sie einen spezifischen Befehl für Ihre PFX-Datei aus den Anweisungen auf der Seite mit der**Infodatei**kopieren.)

```bash
openssl pkcs12 -in party-cluster-1486790479-client-cert.pfx -out party-cluster-1486790479-client-cert.pem -nodes -passin pass:1486790479
``` 

Zur Verwendung von Service Fabric Explorer müssen Sie die von der Partycluster-Website heruntergeladene PFX-Zertifikatdatei in Ihren Zertifikatspeicher (Windows oder Mac) oder in den Browser selbst (Ubuntu) importieren. Sie benötigen das PFX-Kennwort für den privaten Schlüssel von der Seite mit der**Infodatei**.

Importieren Sie das Zertifikat mit der von Ihnen bevorzugten Methode in Ihr System. Beispiel:

* Unter Windows: Doppelklicken Sie auf die PFX-Datei, und befolgen Sie die Anweisungen, um das Zertifikat in Ihrem persönlichen Zertifikatspeicher (`Certificates - Current User\Personal\Certificates`) zu installieren. Alternativ können Sie den in der**Infodatei** angegebenen PowerShell-Befehl ausführen.
* Auf einem Mac: Doppelklicken Sie auf die PFX-Datei, und befolgen Sie die Anweisungen, um das Zertifikat in Ihrer Keychain zu installieren.
* Unter Ubuntu: Der Standardbrowser in Ubuntu 16.04 ist Mozilla Firefox. Klicken Sie zum Importieren des Zertifikats rechts oben in Firefox auf die Menüschaltfläche und anschließend auf **Einstellungen**. Suchen Sie über das Suchfeld auf der Seite **Einstellungen** nach „Zertifikate“. Klicken Sie auf **Zertifikate anzeigen**, klicken Sie auf die Registerkarte **Ihre Zertifikate**, klicken Sie auf **Importieren**, und befolgen Sie die Anweisungen zum Importieren des Zertifikats.

   ![Installieren des Zertifikats in Firefox](./media/service-fabric-quickstart-java-spring-boot/install-cert-firefox.png)

### <a name="deploy-the-application-using-cli"></a>Bereitstellen der Anwendung über die Befehlszeilenschnittstelle

Nachdem die Anwendung und Ihr Cluster nun bereitstehen, können Sie die Anwendung direkt über die Befehlszeile im Cluster bereitstellen.

1. Navigieren Sie zum Ordner `gs-spring-boot/SpringServiceFabric`.
2. Führen Sie den folgenden Befehl aus, um eine Verbindung mit dem Azure-Cluster herzustellen.

    ```bash
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify
    ```
3. Führen Sie das Skript `install.sh` aus.

    ```bash
    ./install.sh
    ```

4. Öffnen Sie Ihren Browser, und greifen Sie über **http://\<IP-Adresse oder URL der Verbindung>:8080** auf die Anwendung zu.

    ![Lokales Front-End der Anwendung](./media/service-fabric-quickstart-java-spring-boot/springbootsfazure.png)

Sie können nun auf die Spring Boot-Anwendung zugreifen, die in einem Service Fabric-Cluster in Azure ausgeführt wird.

## <a name="scale-applications-and-services-in-a-cluster"></a>Skalieren von Anwendungen und Diensten in einem Cluster

Dienste können clusterweit skaliert werden, um eine Laständerungen für die Dienste auszugleichen. Sie skalieren einen Dienst, indem Sie die Anzahl von Instanzen ändern, die im Cluster ausgeführt werden. Dienste können auf unterschiedliche Weise skaliert werden – beispielsweise mithilfe von Skripts oder Befehlen der Service Fabric-Befehlszeilenschnittstelle (sfctl). In den folgenden Schritten wird Service Fabric Explorer verwendet.

Service Fabric Explorer wird in allen Service Fabric-Clustern ausgeführt und kann in einem Browser geöffnet werden. Navigieren Sie hierzu zum HTTP-Verwaltungsport des Clusters (19.080). Beispiel: `http://localhost:19080`.

Gehen Sie zum Skalieren des Web-Front-End-Diensts wie folgt vor:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://localhost:19080`.
2. Klicken Sie in der Strukturansicht auf das Auslassungszeichen (drei Punkte) neben dem Knoten **fabric:/SpringServiceFabric/SpringGettingStarted**, und wählen Sie **Scale Service** (Dienst skalieren) aus.

    ![Service Fabric Explorer – Dienst skalieren](./media/service-fabric-quickstart-java-spring-boot/sfxscaleservicehowto.png)

    Sie können jetzt angeben, wie viele Instanzen des Diensts skaliert werden sollen.

3. Ändern Sie die Anzahl in **3**, und klicken Sie auf **Scale Service** (Dienst skalieren).

    Alternativ kann der Dienst wie folgt über eine Befehlszeile skaliert werden.

    ```bash
    # Connect to your local cluster
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted' --instance-count 3 --stateless 
    ``` 

4. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/SpringServiceFabric/SpringGettingStarted**, und erweitern Sie den Partitionsknoten (durch eine GUID dargestellt).

    ![Service Fabric Explorer: Dienstskalierung abgeschlossen](./media/service-fabric-quickstart-java-spring-boot/sfxscaledservice.png)

    Der Dienst verfügt über drei Instanzen, und die Strukturansicht zeigt, auf welchen Knoten die Instanzen ausgeführt werden.

Mit dieser einfachen Verwaltungsaufgabe haben Sie die Ressourcen verdoppelt, die dem Front-End-Dienst zur Bewältigung der Benutzerauslastung zur Verfügung stehen. Es ist wichtig zu verstehen, dass Sie nicht mehrere Instanzen eines Diensts benötigen, damit er zuverlässig ausgeführt wird. Sollte ein Dienst ausfallen, stellt Service Fabric sicher, dass im Cluster eine neue Dienstinstanz ausgeführt wird.

## <a name="fail-over-services-in-a-cluster"></a>Ausführen eines Failovers für Dienste in einem Cluster

Zur Veranschaulichung eines Dienstfailovers wird unter Verwendung von Service Fabric Explorer ein Neustart des Knotens simuliert. Vergewissern Sie sich, dass nur eine einzelne Instanz des Diensts ausgeführt wird.

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://localhost:19080`.
2. Klicken Sie auf das Auslassungszeichen (drei Punkte) neben dem Knoten, auf dem die Instanz des Diensts ausgeführt wird, und starten Sie den Knoten neu.

    ![Service Fabric Explorer – Neustarten des Knotens](./media/service-fabric-quickstart-java-spring-boot/sfxhowtofailover.png)
3. Die Instanz des Diensts wird auf einen anderen Knoten verschoben, und die Anwendung kann ohne Ausfallzeit weiter verwendet werden.

    ![Service Fabric Explorer – Neustart des Knotens erfolgreich](./media/service-fabric-quickstart-java-spring-boot/sfxfailedover.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

* Bereitstellen einer Spring Boot-Anwendung in Service Fabric
* Bereitstellen der Anwendung im Cluster
* Bereitstellen der Anwendung in einem Cluster in Azure
* Horizontales Hochskalieren der Anwendung über mehrere Knoten hinweg
* Ausführen eines Failovers für den Dienst ohne Verfügbarkeitstreffer

Weitere Informationen zur Verwendung von Java-Apps in Service Fabric finden Sie im Tutorial für Java-Apps.

> [!div class="nextstepaction"]
> [Bereitstellen einer Java-App](./service-fabric-tutorial-create-java-app.md)