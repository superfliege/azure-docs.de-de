---
title: Konfigurieren von Jenkins für eine Java-App unter Service Fabric in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Continuous Integration mit Jenkins einrichten, um eine Java-basierte Service Fabric-Anwendung bereitzustellen.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/27/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 0a0f7cc8e3810a28fdbec914a9f37808c33ab878
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57880587"
---
# <a name="tutorial-configure-a-jenkins-environment-to-enable-cicd-for-a-java-application-on-service-fabric"></a>Tutorial: Konfigurieren einer Jenkins-Umgebung zum Aktivieren von CI/CD für eine Java-Anwendung unter Service Fabric

Dieses Tutorial ist der fünfte Teil einer Reihe. Hier erfahren Sie, wie unter Verwendung von Jenkins Upgrades für Ihre Anwendung bereitstellen. In diesem Tutorial werden das Service Fabric-Jenkins-Plug-In und ein GitHub-Repository mit der Voting-Anwendung verwendet, um die Anwendung in einem Cluster bereitzustellen.

Im fünften Teil der Reihe lernen Sie Folgendes:
> [!div class="checklist"]
> * Bereitstellen eines Service Fabric-Jenkins-Containers auf Ihrem Computer
> * Einrichten der Jenkins-Umgebung für die Bereitstellung in Service Fabric
> * Upgraden Ihrer Anwendung

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer Service Fabric Reliable Services-Java-Anwendung](service-fabric-tutorial-create-java-app.md)
> * [Bereitstellen und Debuggen der Anwendung in einem lokalen Cluster](service-fabric-tutorial-debug-log-local-cluster.md)
> * [Bereitstellen der Anwendung in einem Azure-Cluster](service-fabric-tutorial-java-deploy-azure.md)
> * [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-java-elk.md)
> * Einrichten von CI/CD

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie Git über die [Git-Downloadseite](https://git-scm.com/downloads) auf Ihrem lokalen Computer. Weitere Informationen zu Git finden Sie in der [Git-Dokumentation](https://git-scm.com/docs).
* [Jenkins](https://jenkins.io/)-Grundkenntnisse
* Erstellen Sie ein [GitHub](https://github.com/)-Konto, und machen Sie sich mit der Verwendung von GitHub vertraut.
* Installieren Sie [Docker](https://www.docker.com/community-edition) auf Ihrem Computer.

## <a name="pull-and-deploy-service-fabric-jenkins-container-image"></a>Pullen und Bereitstellen des Service Fabric-Jenkins-Containerimages

Sie können Jenkins innerhalb oder außerhalb eines Service Fabric-Clusters einrichten. In der folgenden Anleitung wird gezeigt, wie Sie die Lösung mithilfe eines bereitgestellten Docker-Images außerhalb eines Clusters einrichten. Sie können aber auch eine vorkonfigurierte Jenkins-Buildumgebung verwenden. Das folgende Containerimage wird zusammen mit dem Service Fabric-Plug-In installiert und kann sofort mit Service Fabric verwendet werden.

1. Rufen Sie das Service Fabric-Jenkins-Containerimage per Pullvorgang ab: ``docker pull rapatchi/jenkins:v10``. Dieses Image ist beim Service Fabric-Jenkins-Plug-In vorinstalliert.

1. Führen Sie das Containerimage mit dem Speicherort aus, an dem Ihre Azure-Zertifikate auf dem bereitgestellten lokalen Computer gespeichert sind.

    ```bash
    docker run -itd -p 8080:8080 -v /service-fabric-java-quickstart/AzureCluster rapatchi/jenkins:v10
    ```

1. Rufen Sie die ID der Containerimageinstanz ab. Mit dem Befehl ``docker ps –a`` können Sie eine Liste mit allen Docker-Containern anzeigen.

1. Führen Sie den folgenden Befehl aus, um das Kennwort Ihrer Jenkins-Instanz abzurufen:

    ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```

    Verwenden Sie beispielsweise „2d24“, wenn die Container-ID „2d24a73b5964“ lautet.
    * Dieses Kennwort wird für die Anmeldung beim Jenkins-Dashboard über das Portal (``http://<HOST-IP>:8080``) benötigt.
    * Nach erstmaliger Anmeldung können Sie Ihr eigenes Benutzerkonto erstellen oder das Administratorkonto verwenden.

1. Richten Sie GitHub für Jenkins ein, indem Sie die Schritte unter [Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Generieren eines neuen SSH-Schlüssels und Hinzufügen des Schlüssels zum SSH-Agent) ausführen. Da die Befehle über den Docker-Container ausgeführt werden, verwenden Sie die Anleitung für die Linux-Umgebung.
   * Generieren Sie den SSH-Schlüssel anhand der Anweisungen von GitHub. Fügen Sie den SSH-Schlüssel dann dem GitHub-Konto hinzu, das als Host für das Repository fungiert.
   * Führen Sie die unter dem obigen Link beschriebenen Befehle in der Jenkins-Docker-Shell aus (nicht auf Ihrem Host).
   * Verwenden Sie die folgenden Befehle, um sich über Ihren Host an der Jenkins-Shell anzumelden:

     ```sh
     docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
     ```

     Stellen Sie sicher, dass der Cluster oder der Computer, in bzw. auf dem das Jenkins-Containerimage gehostet wird, über eine öffentliche IP-Adresse verfügt. Mit einer öffentlichen IP-Adresse kann die Jenkins-Instanz dann Benachrichtigungen von GitHub empfangen.

## <a name="create-and-configure-a-jenkins-job"></a>Erstellen und Konfigurieren eines Jenkins-Auftrags

1. Sollten Sie über kein Repository verfügen, mit dem Sie das Voting-Projekt auf GitHub hosten können, erstellen Sie eines. Im weiteren Verlauf dieses Tutorials wird für das Repository der Name **dev_test** verwendet.

1. Erstellen Sie ein **neues Element** auf dem Ihrem Jenkins-Dashboard unter ``http://<HOST-IP>:8080``.

1. Geben Sie einen Elementnamen ein (z.B. **MyJob**). Wählen Sie die Option **free-style project** (Freestyleprojekt), und klicken Sie auf **OK**.

1. Navigieren Sie zur Auftragsseite, und klicken Sie auf **Configure** (Konfigurieren).

   a. Aktivieren Sie im Abschnitt „General“ (Allgemein) das Kontrollkästchen **GitHub project** (GitHub-Projekt), und geben Sie die URL Ihres GitHub-Projekts ein. Mit dieser URL wird die Service Fabric-Java-Anwendung gehostet, die Sie in den Jenkins-CI/CD-Flow (Continuous Integration, Continuous Deployment) integrieren möchten (z.B. ``https://github.com/testaccount/dev_test``).

   b. Wählen Sie im Abschnitt **Quellcodeverwaltung** die Option **Git**. Geben Sie die Repository-URL an, unter der die Service Fabric-Java-Anwendung gehostet wird, die Sie in den Jenkins-CI/CD-Flow integrieren möchten (Beispiel: *https://github.com/testaccount/dev_test.git*). Hier können Sie auch die zu erstellende Verzweigung angeben (z.B. **/master**).

1. Konfigurieren Sie Ihre *GitHub*-Instanz (in der das Repository gehostet wird), um die Kommunikation mit Jenkins zu ermöglichen. Führen Sie die folgenden Schritte aus:

   a. Wechseln Sie zur Seite Ihres GitHub-Repositorys. Navigieren Sie zu **Settings (Einstellungen)** > **Integrations and Services (Integrationen und Dienste)**.

   b. Wählen Sie **Add Service** (Dienst hinzufügen), geben Sie **Jenkins** ein, und wählen Sie das **Jenkins-GitHub-Plug-In** aus.

   c. Geben Sie Ihre Jenkins-Webhook-URL ein. (Diese lautet standardmäßig ``http://<PublicIPorFQDN>:8081/github-webhook/``.) Klicken Sie auf die Option zum **Hinzufügen/Aktualisieren des Diensts**.

   d. An die Jenkins-Instanz wird ein Testereignis gesendet. Für den Webhook in GitHub sollte ein grünes Häkchen angezeigt werden, und Ihr Projekt wird erstellt.

   ![Service Fabric-Jenkins-Konfiguration](./media/service-fabric-tutorial-java-jenkins/jenkinsconfiguration.png)

1. Wählen Sie unter dem Abschnitt **Build Triggers** (Buildtrigger) die gewünschte Buildoption. In diesem Beispiel soll jeweils ein Build ausgelöst werden, wenn ein Pushvorgang zum Repository durchgeführt wird. Sie wählen also die Option **GitHub hook trigger for GITScm polling** (GitHub-Hooktrigger für GITScm-Abruf).

1. Wählen Sie im Abschnitt **Build** in der Dropdownliste **Buildschritt hinzufügen** die Option **Invoke Gradle Script** (Gradle-Skript aufrufen). Öffnen Sie im daraufhin angezeigten Widget das erweiterte Menü, und geben Sie den Pfad zu **Root build script** (Stammerstellungsskript) für Ihre Anwendung an. „build.gradle“ wird am angegebenen Pfad abgerufen und entsprechend verwendet.

    ![Service Fabric-Jenkins-Buildvorgang](./media/service-fabric-tutorial-java-jenkins/jenkinsbuildscreenshot.png)

1. Wählen Sie in der Dropdownliste **Post-Build Actions** (Aktionen nach dem Erstellen) die Option **Deploy Service Fabric Project** (Service Fabric-Projekt bereitstellen). Hier müssen Sie Clusterdetails dazu angeben, wo die von Jenkins kompilierte Service Fabric-Anwendung bereitgestellt werden soll. Bei dem Pfad des Zertifikats handelt es sich um den Ort, an dem das Volume eingebunden wurde (/tmp/myCerts).

    Sie können auch zusätzliche Details angeben, die zum Bereitstellen der Anwendung verwendet werden. Der folgende Screenshot zeigt ein Beispiel für Anwendungsdetails:

    ![Service Fabric-Jenkins-Buildvorgang](./media/service-fabric-tutorial-java-jenkins/sfjenkins.png)

    > [!NOTE]
    > Hier kann der gleiche Cluster angegeben werden, der auch als Host für die Jenkins-Containeranwendung fungiert, falls Sie das Jenkins-Containerimage mithilfe von Service Fabric bereitstellen.
    >

1. Klicken Sie auf **Speichern**.

## <a name="update-your-existing-application"></a>Aktualisieren Ihrer vorhandenen Anwendung

1. Aktualisieren Sie den Titel des HTML-Codes in der Datei *VotingApplication/VotingWebPkg/Code/wwwroot/index.html* mit **Service Fabric Voting Sample V2**.

    ```html
    <div ng-app="VotingApp" ng-controller="VotingAppController" ng-init="refresh()">
        <div class="container-fluid">
            <div class="row">
                <div class="col-xs-8 col-xs-offset-2 text-center">
                    <h2>Service Fabric Voting Sample V2</h2>
                </div>
            </div>
        </div>
    </div>
    ```

1. Aktualisieren Sie die Versionen **ApplicationTypeVersion** und **ServiceManifestVersion** in der Datei *Voting/VotingApplication/ApplicationManifest.xml* auf **2.0.0**.

    ```xml
    <?xml version="1.0" encoding="utf-8" standalone="no"?>
    <ApplicationManifest xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingApplicationType" ApplicationTypeVersion="2.0.0">
      <Description>Voting Application</Description>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="2.0.0"/>
      </ServiceManifestImport>
      <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="VotingDataServicePkg" ServiceManifestVersion="1.0.0"/>
        </ServiceManifestImport>
        <DefaultServices>
          <Service Name="VotingWeb">
             <StatelessService InstanceCount="1" ServiceTypeName="VotingWebType">
                <SingletonPartition/>
             </StatelessService>
          </Service>
       <Service Name="VotingDataService">
                <StatefulService MinReplicaSetSize="3" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="3">
                    <UniformInt64Partition HighKey="9223372036854775807" LowKey="-9223372036854775808" PartitionCount="1"/>
                </StatefulService>
            </Service>
        </DefaultServices>
    </ApplicationManifest>
    ```

1. Aktualisieren Sie das Feld **Version** in **ServiceManifest** und das Feld **Version** im Tag **CodePackage** in der Datei *Voting/VotingApplication/VotingWebPkg/ServiceManifest.xml* auf **2.0.0**.

    ```xml
    <CodePackage Name="Code" Version="2.0.0">
    <EntryPoint>
        <ExeHost>
        <Program>entryPoint.sh</Program>
        </ExeHost>
    </EntryPoint>
    </CodePackage>
    ```

1. Pushen Sie Ihre neuen Änderungen in Ihr GitHub-Repository, um einen Jenkins-Auftrag zu initialisieren, der ein Anwendungsupgrade ausführt.

1. Klicken Sie in Service Fabric Explorer auf das Dropdownfeld **Anwendungen**. Klicken Sie zum Anzeigen des Upgradestatus auf die Registerkarte **Upgrades in Progress** (Laufende Upgrades).

    ![Laufendes Upgrade](./media/service-fabric-tutorial-create-java-app/upgradejava.png)

1. Die Voting-Anwendung steht nun unter **http://\<Host-IP>:8080** zur Verfügung.

    ![Voting-App (lokal)](./media/service-fabric-tutorial-java-jenkins/votingv2.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen eines Service Fabric-Jenkins-Containers auf Ihrem Computer
> * Einrichten der Jenkins-Umgebung für die Bereitstellung in Service Fabric
> * Upgraden Ihrer Anwendung

* Sehen Sie sich die anderen [Java-Beispiele](https://github.com/Azure-Samples/service-fabric-java-getting-started) an
