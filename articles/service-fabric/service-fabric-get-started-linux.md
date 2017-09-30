---
title: Einrichten Ihrer Entwicklungsumgebung unter Linux | Microsoft Docs
description: "Installieren Sie die Laufzeit und das SDK, und erstellen Sie einen lokalen Entwicklungscluster unter Linux. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen beginnen."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/19/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: da9aff17c16e179be200677bfbfd1287fff269e3
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="prepare-your-development-environment-on-linux"></a>Vorbereiten Ihrer Entwicklungsumgebung unter Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Zur Bereitstellung und Ausführung von [Azure Service Fabric-Anwendungen](service-fabric-application-model.md) auf Ihrem Linux-Entwicklungscomputer müssen Sie die Laufzeit und das allgemeine SDK installieren. Darüber hinaus können Sie auch optionale SDKs für die Java- und .NET Core-Entwicklung installieren.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Ubuntu 16.04 (`Xenial Xerus`)

## <a name="installation-methods"></a>Installationsmethoden

### <a name="1-script-installation"></a>1. Skriptinstallation

Ein Skript soll Ihnen als Hilfe beim Installieren der Service Fabric-Runtime und des allgemeinen Service Fabric SDK zusammen mit der **sfctl**-CLI dienen. Führen Sie die manuellen Installationsschritte im nächsten Abschnitt aus, um zu bestimmen, was installiert wird und welchen Lizenzen zugestimmt wird. Für die Ausführung des Skripts wird vorausgesetzt, dass Sie den Lizenzen für die gesamte zu installierende Software zustimmen. 

Nach der erfolgreichen Ausführung des Skripts können Sie direkt zu [Einrichten eines lokalen Clusters](#set-up-a-local-cluster) springen.

```bash
sudo curl -s https://raw.githubusercontent.com/Azure/service-fabric-scripts-and-templates/master/scripts/SetupServiceFabric/SetupServiceFabric.sh | sudo bash
```

### <a name="2-manual-installation"></a>2. Manuelle Installation
Die Schritte zum manuellen Installieren der Service Fabric-Laufzeit und des gemeinsamen SDK finden Sie im restlichen Teil dieser Anleitung.

## <a name="update-your-apt-sources"></a>Aktualisieren Ihrer APT-Quellen
Um das SDK und das dazugehörige Laufzeitpaket über das Befehlszeilenprogramm „apt-get“ installieren zu können, müssen Sie zunächst Ihre APT-Datenquellen (Advanced Packaging Tool) aktualisieren.

1. Öffnen Sie ein Terminal.
2. Fügen Sie der Quellenliste das Service Fabric-Repository hinzu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Fügen Sie das `dotnet`-Repository Ihrer Quellenliste hinzu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Fügen Sie Ihrem APT-Schlüsselbund den neuen GnuPG- bzw. GPG-Schlüssel (Gnu Privacy Guard) hinzu.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Fügen Sie Ihrem APT-Schlüsselbund den offiziellen Docker-GPG-Schlüssel hinzu.

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Richten Sie das Docker-Repository ein.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Aktualisieren Sie Ihre Paketlisten auf der Grundlage der neu hinzugefügten Repositorys.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-service-fabric-sdk-for-local-cluster-setup"></a>Installieren und Einrichten des Service Fabric SDK für das lokale Clustersetup

Nach der Aktualisierung Ihrer Quellen können Sie das SDK installieren. Installieren Sie das Service Fabric-SDK-Paket, bestätigen Sie die Installation, und stimmen Sie dem Lizenzvertrag zu.

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   Mit den folgenden Befehlen wird das Akzeptieren der Lizenz für Service Fabric-Pakete automatisiert:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-ga select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-ga select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a>Einrichten eines lokalen Clusters
  Nach Abschluss der Installation sollten Sie einen lokalen Cluster starten können.

  1. Führen Sie das Clustersetupskript aus.

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. Öffnen Sie einen Webbrowser, und wechseln Sie zu [Service Fabric Explorer](http://localhost:19080/Explorer). Der gestartete Cluster wird auf dem Dashboard von Service Fabric Explorer angezeigt.

      ![Service Fabric Explorer unter Linux][sfx-linux]

  Nun können Sie vorgefertigte Service Fabric-Anwendungspakete oder neue, auf Gastcontainern oder ausführbaren Gastdateien basierende Anwendungspakete bereitstellen. Wenn Sie neue Dienste mit den Java- oder .NET Core-SDKs erstellen möchten, führen Sie die optionalen Einrichtungsschritte aus den nachfolgenden Abschnitten aus.


  > [!NOTE]
  > Eigenständige Cluster werden unter Linux nicht unterstützt.
  >

## <a name="set-up-the-service-fabric-cli"></a>Einrichten der Service Fabric-Befehlszeilenschnittstelle

Die [Service Fabric CLI](service-fabric-cli.md) enthält Befehle für die Interaktion mit Service Fabric-Entitäten, etwa Clustern und Anwendungen.
Befolgen Sie die Anweisungen unter [Service Fabric-Befehlszeilenschnittstelle](service-fabric-cli.md), um die CLI zu installieren.


## <a name="set-up-yeoman-generators-for-containers-and-guest-executables"></a>Einrichten von Yeoman-Generatoren für Container und ausführbare Gastdateien
Die Gerüstbautools von Service Fabric unterstützen Sie beim Erstellen von Service Fabric-Anwendungen über das Terminal mithilfe von Yeoman-Vorlagengeneratoren. Führen Sie die folgenden Schritte aus, um die Service Fabric-Yeoman-Vorlagengeneratoren einzurichten:

1. Installieren von Node.js und npm auf dem Computer

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Installieren des [Yeoman](http://yeoman.io/)-Vorlagengenerators auf dem Computer über npm

  ```bash
  sudo npm install -g yo
  ```
3. Installieren des Service Fabric-Yeo-Containergenerators und des Generators für die ausführbare Gastdatei über npm

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

Nachdem Sie die Generatoren installiert haben, können Sie ausführbare Gastdateien oder Containerdienste erstellen, indem Sie `yo azuresfguest` bzw. `yo azuresfcontainer` ausführen.

## <a name="set-up-net-core-20-development"></a>Einrichten der .NET Core 2.0-Entwicklung

Installieren Sie das [.NET Core 2.0 SDK für Ubuntu](https://www.microsoft.com/net/core#linuxubuntu), um mit dem [Erstellen von Service Fabric-C#-Anwendungen](service-fabric-create-your-first-linux-application-with-csharp.md) zu beginnen. Pakete für Service Fabric-Anwendungen mit .NET Core 2.0 werden auf NuGet.org gehostet (derzeit in der Vorschauphase).

## <a name="set-up-java-development"></a>Einrichten der Java-Entwicklung

Installieren Sie JDK 1.8 und Gradle zum Ausführen von Erstellungsaufgaben, um Service Fabric-Dienste mit Java zu erstellen. Mit dem folgenden Codeausschnitt wird Open JDK 1.8 zusammen mit Gradle installiert. Die Service Fabric-Java-Bibliotheken werden aus Maven abgerufen.

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-the-eclipse-neon-plug-in-optional"></a>Installieren des Eclipse Neon-Plug-Ins (optional)

Das Eclipse-Plug-In für Service Fabric können Sie über die Eclipse-IDE für Java-Entwickler installieren. Sie können Eclipse verwenden, um zusätzlich zu Service Fabric-Java-Anwendungen Anwendungen mit ausführbarer Gastanwendungsdatei und Containeranwendungen für Service Fabric zu erstellen.

1. Stellen Sie in Eclipse sicher, dass Sie die aktuellen Versionen von Eclipse Neon und Buildship (1.0.17 oder höher) installiert haben. Die Version der installierten Komponenten können Sie unter **Hilfe** > **Installationsdetails** ermitteln. Anweisungen zum Aktualisieren von Buildship finden Sie unter [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update] (Eclipse Buildship: Eclipse-Plug-Ins für Gradle).

2. Wählen Sie zum Installieren des Service Fabric-Plug-Ins **Hilfe** > **Neue Software installieren...** aus.

3. Geben Sie im Feld **Work with** (Verwenden von) die URL **http://dl.microsoft.com/eclipse** ein.

4. Klicken Sie auf **Hinzufügen**.

    ![Die Seite „Verfügbare Software“][sf-eclipse-plugin]

5. Wählen Sie das Plug-In **Service Fabric** aus, und klicken Sie auf **Weiter**.

6. Führen Sie die Installationsschritte aus, und akzeptieren Sie die Microsoft-Software-Lizenzbedingungen.

Falls Sie das Service Fabric-Plug-In für Eclipse bereits installiert haben, vergewissern Sie sich, dass Sie über die aktuelle Version verfügen. Wählen Sie hierzu **Hilfe** > **Installationsdetails** aus, und suchen Sie anschließend in der Liste mit den installierten Plug-Ins nach Service Fabric. Ist eine neuere Version verfügbar, wählen Sie **Aktualisieren** aus.

Weitere Informationen finden Sie unter [Service Fabric-Plug-In für die Entwicklung von Eclipse-Java-Anwendungen](service-fabric-get-started-eclipse.md).

## <a name="update-the-sdk-and-runtime"></a>Aktualisieren des SDKs und der Laufzeit

Führen Sie die folgenden Befehle aus, um das Update auf die aktuelle Version des SDK und der Laufzeit durchzuführen:

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon
```
Für die Aktualisierung der Java SDK-Binärdateien aus Maven müssen Sie die Versionsdetails der entsprechenden Binärdatei in der Datei ``build.gradle`` so aktualisieren, dass sie auf die aktuelle Version verweisen. Informationen dazu, wo genau die Version aktualisiert werden muss, finden Sie in jeder ``build.gradle``-Datei in den [Beispielen für die ersten Schritte mit Service Fabric](https://github.com/Azure-Samples/service-fabric-java-getting-started).

> [!NOTE]
> Die Aktualisierung der Pakete kann die Beendigung Ihres lokalen Entwicklungsclusters zur Folge haben. Starten Sie den lokalen Cluster nach einem Upgrade anhand der Anweisungen auf dieser Seite neu.

## <a name="remove-the-sdk"></a>Entfernen des SDK
Führen Sie Folgendes aus, um die Service Fabric SDKs zu entfernen:

```bash
sudo apt-get remove servicefabric servicefabicsdkcommon
sudo npm uninstall generator-azuresfcontainer
sudo npm uninstall generator-azuresfguest
sudo apt-get install -f
```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe von Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe des Service Fabric-Plug-Ins für Eclipse](service-fabric-get-started-eclipse.md)
* [Erstellen der ersten Java-Anwendung unter Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Prepare your development environment on OSX (Vorbereiten Ihrer Entwicklungsumgebung unter OSX)](service-fabric-get-started-mac.md)
* [Verwalten von Anwendungen mit der Service Fabric-Befehlszeilenschnittstelle](service-fabric-application-lifecycle-sfctl.md)
* [Unterschiede zwischen Service Fabric unter Linux (Vorschau) und Windows (allgemein verfügbar)](service-fabric-linux-windows-differences.md)
* [Erste Schritte mit der Service Fabric CLI](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

