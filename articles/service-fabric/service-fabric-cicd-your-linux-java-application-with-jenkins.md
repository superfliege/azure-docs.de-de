---
title: "Kontinuierliche Erstellung und Integration für Ihre Azure Service Fabric-Linux-Java-Anwendung mit Jenkins | Microsoft-Dokumentation"
description: "Kontinuierliche Erstellung und Integration für Ihre Linux-Java-Anwendung mit Jenkins"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: saysa
ms.openlocfilehash: d9870fafab3df3ab0ec72305e76a4d3547cc5b2c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2017
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-java-application"></a>Verwenden von Jenkins zum Erstellen und Bereitstellen Ihrer Linux-Java-Anwendung
Jenkins ist ein beliebtes Tool für Continuous Integration und Continuous Deployment für Ihre Apps. Hier wird beschrieben, wie Sie Ihre Azure Service Fabric-Anwendung mit Jenkins erstellen und bereitstellen.

## <a name="general-prerequisites"></a>Allgemeine Voraussetzungen
- Lokale Git-Installation. Sie können die richtige Git-Version über die [Git-Downloadseite](https://git-scm.com/downloads) für Ihr Betriebssystem installieren. Wenn Git neu für Sie ist, hilft Ihnen die [Git-Dokumentation](https://git-scm.com/docs) weiter.
- Sie benötigen das Service Fabric-Jenkins-Plug-In. Der Download ist auf der Seite mit den [Service Fabric-Downloads](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) möglich.

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Einrichten von Jenkins in einem Service Fabric-Cluster

Sie können Jenkins innerhalb oder außerhalb eines Service Fabric-Clusters einrichten. Die folgenden Abschnitte zeigen, wie Sie Jenkins in einem Cluster einrichten, während Sie ein Azure-Speicherkonto zum Speichern des Status der Containerinstanz verwenden.

### <a name="prerequisites"></a>Voraussetzungen
1. Sie benötigen einen Service Fabric-Linux-Cluster. Für einen Service Fabric-Cluster, der über das Azure-Portal erstellt wird, ist Docker bereits installiert. Wenn Sie den Cluster lokal ausführen, können Sie mit dem Befehl ``docker info`` prüfen, ob Docker installiert ist. Falls nicht, können Sie es mit den folgenden Befehlen entsprechend installieren:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ``` 

   > [!NOTE]
   > Stellen Sie sicher, dass Port 8081 als benutzerdefinierter Endpunkt im Cluster angegeben ist.
   >
2. Klonen Sie die Anwendung anhand der folgenden Schritte:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
cd service-fabric-java-getting-started/Services/JenkinsDocker/
```

3. Behalten Sie den Status des Jenkins-Containers in einer Dateifreigabe bei:
  * Erstellen Sie ein Azure-Speicherkonto in der **gleichen Region** wie der Cluster. Verwenden Sie beispielsweise einen Namen wie ``sfjenkinsstorage1``.
  * Erstellen Sie unter dem Speicherkonto eine **Dateifreigabe** mit einem Namen wie etwa ``sfjenkins``.
  * Klicken Sie für die Dateifreigabe auf **Verbinden**, und notieren Sie die unter **Herstellen einer Verbindung über Linux** angezeigten Werte. Der Wert sollte ungefähr so aussehen:
```sh
sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
```

> [!NOTE]
> Zum Bereitstellen von cifs-Freigaben müssen Sie das „cifs-utils“-Paket auf den Clusterknoten installiert haben.         
>

4. Aktualisieren Sie die Platzhalterwerte im ```setupentrypoint.sh```-Skript mit den azure-storage-Details aus Schritt 3.
```sh
vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
```
  * Ersetzen Sie ``[REMOTE_FILE_SHARE_LOCATION]`` durch den Wert ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` aus der Ausgabe der Verbindung in Schritt 3 weiter oben.
  * Ersetzen Sie ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` durch den Wert ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` aus Schritt 3 oben.

5. Stellen Sie eine Verbindung mit dem Cluster her, und installieren Sie die Containeranwendung.
```sh
sfctl cluster select --endpoint http://PublicIPorFQDN:19080   # cluster connect command
bash Scripts/install.sh
```
Im Cluster wird ein Jenkins-Container installiert und kann mit dem Service Fabric Explorer überwacht werden.

   > [!NOTE]
   > Das Herunterladen des Jenkins-Images in den Cluster kann einige Minuten dauern.
   >

### <a name="steps"></a>Schritte
1. Navigieren Sie im Browser zu ``http://PublicIPorFQDN:8081``. Hierdurch erhalten Sie den Pfad des ursprünglichen Administratorkennworts, das für die Anmeldung erforderlich ist. 
2. Ermitteln Sie in Service Fabric Explorer, auf welchem Knoten der Jenkins-Container ausgeführt wird. Melden Sie sich per SSH (Secure Shell) bei diesem Knoten an.
```sh
ssh user@PublicIPorFQDN -p [port]
``` 
3. Rufen Sie mithilfe von ``docker ps -a`` die ID der Containerinstanz ab.
4. Melden Sie sich per SSH (Secure Shell) am Container an, und fügen Sie den Pfad aus dem Jenkins-Portal ein. Führen Sie beispielsweise Folgendes aus, wenn im Portal der Pfad `PATH_TO_INITIAL_ADMIN_PASSWORD` angezeigt wird:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  ```
  ```sh
  cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the pasword value
  ```
5. Wählen Sie auf der Jenkins-Seite mit den ersten Schritten die Option „Select plugins to install“ (Zu installierende Plug-Ins auswählen), aktivieren Sie das Kontrollkästchen **Keine**, und klicken Sie auf „Installieren“.
6. Erstellen Sie einen Benutzer, oder wählen Sie die entsprechende Option, um den Vorgang als Administrator fortzusetzen.

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Einrichten von Jenkins außerhalb eines Service Fabric-Clusters

Sie können Jenkins innerhalb oder außerhalb eines Service Fabric-Clusters einrichten. In den folgenden Abschnitten wird gezeigt, wie Sie die Einrichtung außerhalb eines Clusters durchführen.

### <a name="prerequisites"></a>Voraussetzungen
Docker muss installiert sein. Sie können die folgenden Befehle verwenden, um Docker über das Terminal zu installieren:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Wenn Sie nun ``docker info`` im Terminal ausführen, wird in der Ausgabe angezeigt, dass der Docker-Dienst ausgeführt wird.

### <a name="steps"></a>Schritte
  1. Rufen Sie das Service Fabric-Jenkins-Containerimage per Pullvorgang ab: ``docker pull raunakpandya/jenkins:v1``
  2. Führen Sie das Containerimage aus: ``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``
  3. Rufen Sie die ID der Containerimageinstanz ab. Mit dem Befehl ``docker ps –a`` können Sie eine Liste mit allen Docker-Containern anzeigen.
  4. Melden Sie sich mit den folgenden Schritten am Jenkins-Portal an:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Verwenden Sie beispielsweise „2d24“, wenn die Container-ID „2d24a73b5964“ lautet.
    * Dieses Kennwort wird für die Anmeldung beim Jenkins-Dashboard über das Portal (``http://<HOST-IP>:8080``) benötigt.
    * Nach der erstmaligen Anmeldung können Sie ein eigenes Benutzerkonto erstellen oder weiterhin das Administratorkonto verwenden. Wenn Sie einen Benutzer erstellen, müssen Sie mit diesem Benutzer fortfahren.
  5. Richten Sie GitHub für Jenkins ein, indem Sie die Schritte unter [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Generieren und Hinzufügen eines neuen SSH-Schlüssels zum SSH-Agent) ausführen.
        * Generieren Sie mithilfe der Anweisungen von GitHub den SSH-Schlüssel, und fügen Sie ihn dem GitHub-Konto hinzu, unter dem das Repository gehostet wird.
        * Führen Sie die unter dem obigen Link beschriebenen Befehle in der Jenkins-Docker-Shell aus (nicht auf Ihrem Host).
      * Verwenden Sie die folgenden Befehle, um sich über Ihren Host an der Jenkins-Shell anzumelden:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Stellen Sie sicher, dass der Cluster oder der Computer, in bzw. auf dem das Jenkins-Containerimage gehostet wird, über eine öffentliche IP-Adresse verfügt. Die Jenkins-Instanz kann dann Benachrichtigungen von GitHub empfangen.

## <a name="install-the-service-fabric-jenkins-plug-in-from-the-portal"></a>Installieren des Service Fabric-Jenkins-Plug-Ins über das Portal

1. Besuchen Sie ``http://PublicIPorFQDN:8081``.
2. Wählen Sie im Jenkins-Dashboard die Optionen **Manage Jenkins (Jenkins verwalten)** > **Manage Plugins (Plug-Ins verwalten)** > **Advanced (Erweitert)**.
Hier können Sie ein Plug-In hochladen. Wählen Sie die Option **Choose file** (Datei auswählen) und anschließend die Datei **serviceFabric.hpi** aus, die Sie im Rahmen der Vorbereitung heruntergeladen haben. Sie können sie auch [hier](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) herunterladen. Nach dem Auswählen von **Upload** wird das Plug-In von Jenkins automatisch installiert. Lassen Sie einen Neustart zu, falls die entsprechende Bestätigungsmeldung angezeigt wird.

## <a name="create-and-configure-a-jenkins-job"></a>Erstellen und Konfigurieren eines Jenkins-Auftrags

1. Erstellen Sie über das Dashboard ein **neues Element**.
2. Geben Sie einen Elementnamen ein (z.B. **MyJob**). Wählen Sie die Option **free-style project** (Freestyleprojekt), und klicken Sie auf **OK**.
3. Navigieren Sie zur Auftragsseite, und klicken Sie auf **Configure** (Konfigurieren).

   a. Aktivieren Sie im Abschnitt „General“ (Allgemein) das Kontrollkästchen **GitHub project** (GitHub-Projekt), und geben Sie die URL Ihres GitHub-Projekts ein. Mit dieser URL wird die Service Fabric-Java-Anwendung gehostet, die Sie in den Jenkins-CI/CD-Flow (Continuous Integration, Continuous Deployment) integrieren möchten (z.B. ``https://github.com/sayantancs/SFJenkins``).

   b. Wählen Sie im Abschnitt **Quellcodeverwaltung** die Option **Git**. Geben Sie die Repository-URL an, unter der die Service Fabric-Java-Anwendung gehostet wird, die Sie in den Jenkins-CI/CD-Flow integrieren möchten (z.B. ``https://github.com/sayantancs/SFJenkins.git``). Hier können Sie auch die zu erstellende Verzweigung angeben (z.B. **/master**).
4. Konfigurieren Sie Ihre *GitHub*-Instanz (in der das Repository gehostet wird), um die Kommunikation mit Jenkins zu ermöglichen. Führen Sie die folgenden Schritte aus:

   a. Wechseln Sie zur Seite Ihres GitHub-Repositorys. Navigieren Sie zu **Settings (Einstellungen)** > **Integrations and Services (Integrationen und Dienste)**.

   b. Wählen Sie **Add Service** (Dienst hinzufügen), geben Sie **Jenkins** ein, und wählen Sie das **Jenkins-GitHub-Plug-In** aus.

   c. Geben Sie Ihre Jenkins-Webhook-URL ein. (Diese lautet standardmäßig ``http://<PublicIPorFQDN>:8081/github-webhook/``.) Klicken Sie auf die Option zum **Hinzufügen/Aktualisieren des Diensts**.

   d. An die Jenkins-Instanz wird ein Testereignis gesendet. Für den Webhook in GitHub sollte ein grünes Häkchen angezeigt werden, und das Projekt sollte erstellt werden.

   e. Wählen Sie unter dem Abschnitt **Build Triggers** (Buildtrigger) die gewünschte Buildoption. In diesem Beispiel soll jeweils ein Build ausgelöst werden, wenn ein Pushvorgang zum Repository durchgeführt wird. Sie wählen also die Option **GitHub hook trigger for GITScm polling** (GitHub-Hooktrigger für GITScm-Abruf). (Früher hatte diese Option die Bezeichnung **Build when a change is pushed to GitHub** (Build bei Push einer Änderung auf GitHub durchführen).)

   f. Wählen Sie im Abschnitt **Build** in der Dropdownliste **Buildschritt hinzufügen** die Option **Invoke Gradle Script** (Gradle-Skript aufrufen). Öffnen Sie im daraufhin angezeigten Widget das erweiterte Menü, und geben Sie den Pfad zu **Root build script** (Stammerstellungsskript) für Ihre Anwendung an. „build.gradle“ wird am angegebenen Pfad abgerufen und entsprechend verwendet. Wenn Sie mithilfe des Eclipse-Plug-Ins oder des Yeoman-Generators ein Projekt namens ``MyActor`` erstellen, muss das Stammerstellungsskript Folgendes enthalten: ``${WORKSPACE}/MyActor``. Der folgende Screenshot enthält hierzu ein Beispiel:

    ![Service Fabric-Jenkins-Buildvorgang][build-step]

   g. Wählen Sie in der Dropdownliste **Post-Build Actions** (Aktionen nach dem Erstellen) die Option **Deploy Service Fabric Project** (Service Fabric-Projekt bereitstellen). Hier müssen Sie Clusterdetails dazu angeben, wo die von Jenkins kompilierte Service Fabric-Anwendung bereitgestellt werden soll. Sie können auch zusätzliche Anwendungsdetails angeben, die zum Bereitstellen der Anwendung verwendet werden. Der folgende Screenshot enthält hierzu ein Beispiel:

    ![Service Fabric-Jenkins-Buildvorgang][post-build-step]

   > [!NOTE]
   > Hier kann der gleiche Cluster angegeben werden, der auch als Host für die Jenkins-Containeranwendung fungiert, falls Sie das Jenkins-Containerimage mithilfe von Service Fabric bereitstellen.
   >

## <a name="next-steps"></a>Nächste Schritte
GitHub und Jenkins sind jetzt konfiguriert. Sie können erwägen, in Ihrem ``MyActor``-Projekt im Repositorybeispiel unter „https://github.com/sayantancs/SFJenkins“ einige Änderungen vorzunehmen. Senden Sie Ihre Änderungen per Pushvorgang an eine ``master``-Remoteverzweigung (oder eine beliebige andere Verzweigung, die Sie zur Verwendung konfiguriert haben). Hierdurch wird der Jenkins-Auftrag ``MyJob`` ausgelöst, den Sie konfiguriert haben. Die Änderungen werden von GitHub abgerufen, die Erstellung wird durchgeführt, und die Anwendung wird für den Clusterendpunkt bereitgestellt, den Sie in Aktionen nach der Erstellung angegeben haben.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png
