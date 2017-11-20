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
ms.date: 09/26/2017
ms.author: saysa
ms.openlocfilehash: f55279436af39d9bc0d4b1d7ef2253e2fc3074c0
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
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
Service Fabric wird nicht nativ unter OS X ausgeführt. Für die Verwendung eines lokalen Service Fabric-Clusters stellen wir einen vorkonfigurierten virtuellen Ubuntu-Computer mit Vagrant und VirtualBox bereit. Bevor Sie beginnen, benötigen Sie Folgendes:

* [Vagrant (mindestens Version 1.8.4)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Sie müssen Versionen von Vagrant und VirtualBox verwenden, die sich gegenseitig unterstützen. Vagrant kann mit einer nicht unterstützten Version von VirtualBox möglicherweise nicht einwandfrei ausgeführt werden.
>

## <a name="create-the-local-vm"></a>Erstellen des lokalen virtuellen Computers
Führen Sie die folgenden Schritte aus, um den lokalen virtuellen Computer mit einem Service Fabric-Cluster mit fünf Knoten zu erstellen:

1. Klonen des Repositorys `Vagrantfile`

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Mit diesen Schritten wird die Datei `Vagrantfile` mit der VM-Konfiguration und dem Downloadort der VM bereitgestellt.  Die Datei verweist auf ein vorhandenes Ubuntu-Image.

2. Navigieren Sie zum lokalen Klon des Repositorys:

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. Optional: Ändern Sie die Standardeinstellungen des virtuellen Computers

    Der lokale virtuelle Computer ist standardmäßig wie folgt konfiguriert:

   * 3 GB zugeordneter Arbeitsspeicher
   * Privates Hostnetzwerk (konfiguriert mit der IP-Adresse 192.168.50.50, um Pass-Through-Datenverkehr des Mac-Hosts zu ermöglichen)

     Sie können diese Einstellungen ändern oder die Konfiguration des virtuellen Computers über `Vagrantfile` ergänzen. Eine Liste mit allen Konfigurationsoptionen finden Sie in der [Vagrant-Dokumentation](http://www.vagrantup.com/docs) .
4. Erstellen des virtuellen Computers

    ```bash
    vagrant up
    ```


5. Melden Sie sich an der VM an, und installieren Sie das Service Fabric SDK.

    ```bash
    vagrant ssh
    ```

   Installieren Sie das SDK wie unter [SDK-Installation](service-fabric-get-started-linux.md) beschrieben.  Das Skript unten soll Ihnen als Hilfe beim Installieren der Service Fabric-Runtime und des allgemeinen Service Fabric SDK zusammen mit der sfctl-CLI dienen. Für die Ausführung des Skripts wird vorausgesetzt, dass Sie die Lizenzen für die gesamte zu installierende Software gelesen und ihnen zugestimmt haben.

    ```bash
    sudo curl -s https://raw.githubusercontent.com/Azure/service-fabric-scripts-and-templates/master/scripts/SetupServiceFabric/SetupServiceFabric.sh | sudo bash
    ```

5.  Starten Sie den Service Fabric-Cluster.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

    >[!TIP]
    > Falls der Download der VM sehr lange dauert, können Sie sie per wget oder curl oder über einen Browser herunterladen, indem Sie zu dem Link navigieren, der unter **config.vm.box_url** in der Datei `Vagrantfile` angegeben ist. Nachdem Sie den Download an den lokalen Speicherort durchgeführt haben, können Sie die Datei `Vagrantfile` so bearbeiten, dass darin auf den lokalen Pfad verwiesen wird, an den Sie das Image heruntergeladen haben. Wenn Sie das Image beispielsweise nach „/home/users/test/azureservicefabric.tp8.box“ heruntergeladen haben, legen Sie **config.vm.box_url** auf diesen Pfad fest.
    >

5. Vergewissern Sie sich, dass der Cluster ordnungsgemäß eingerichtet wurde, indem Sie unter http://192.168.50.50:19080/Explorer zu Service Fabric Explorer navigieren (vorausgesetzt, Sie haben die standardmäßige IP-Adresse des privaten Netzwerks beibehalten).

    ![Service Fabric Explorer auf dem Host-Mac][sfx-mac]

## <a name="install-the-necessary-java-artifacts-on-vagrant-optional-if-you-want-to-use-the-java-programming-models"></a>Installieren des erforderlichen Java-Artefakts unter Vagrant (optional, falls Sie Java-Programmiermodelle nutzen möchten)

Stellen Sie sicher, dass JDK 1.8 zusammen mit Gradle (für die Ausführung von Buildaufgaben) installiert ist, um Service Fabric-Dienste mit Java zu erstellen. Mit dem folgenden Codeausschnitt wird Open JDK 1.8 zusammen mit Gradle installiert. Die Service Fabric-Java-Bibliotheken werden aus Maven abgerufen.

  ```bash
  vagrant ssh
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
```

## <a name="set-up-the-service-fabric-cli"></a>Einrichten der Service Fabric-Befehlszeilenschnittstelle

Die [Service Fabric CLI](service-fabric-cli.md) enthält Befehle für die Interaktion mit Service Fabric-Entitäten, etwa Clustern und Anwendungen. Sie basiert auf Python. Vergewissern Sie sich also, dass Python und pip installiert sind, bevor Sie mit dem folgenden Befehl fortfahren:

```bash
pip install sfctl
```

## <a name="create-application-on-mac-using-yeoman"></a>Erstellen einer Anwendung auf einem Mac mit Yeoman
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
4. Zur Erstellung einer Service Fabric-Java-Anwendung auf einem Mac müssen JDK 1.8 und Gradle auf dem Computer installiert sein.

## <a name="set-up-net-core-20-development"></a>Einrichten der .NET Core 2.0-Entwicklung

Installieren Sie das [.NET Core 2.0 SDK für Mac](https://www.microsoft.com/net/core#macos), um mit dem [Erstellen von Service Fabric-C#-Anwendungen](service-fabric-create-your-first-linux-application-with-csharp.md) zu beginnen. Pakete für Service Fabric-Anwendungen mit .NET Core 2.0 werden auf NuGet.org gehostet (derzeit in der Vorschauphase).


## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>Installieren des Service Fabric-Plug-Ins für Eclipse Neon

Service Fabric umfasst ein Plug-In für die **Eclipse Neon-IDE für Java**, das die Erstellung und Bereitstellung von Java-Diensten vereinfachen kann. Sie können die Installationsschritte in dieser allgemeinen [Dokumentation](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) zur Installation und Aktualisierung des Service Fabric-Plug-Ins für Eclipse ausführen.

>[!TIP]
> Wie erwähnt wird die Standard-IP standardmäßig in ``Vagrantfile`` der Datei ``Local.json`` der erstellten Anwendung unterstützt. Falls Sie dies ändern und Vagrant mit einer anderen IP bereitstellen, aktualisieren Sie auch die entsprechende IP in der Datei ``Local.json`` Ihrer Anwendung.

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
