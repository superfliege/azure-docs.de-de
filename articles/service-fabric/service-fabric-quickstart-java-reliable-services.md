---
title: Erstellen einer Java-App unter Service Fabric in Azure | Microsoft-Dokumentation
description: In diesem Schnellstart erstellen Sie eine Java-Anwendung für Azure, indem Sie eine Service Fabric Reliable Services-Beispielanwendung verwenden.
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
ms.date: 10/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 2e3852ffc01312f01843a90de5f5565784b1c0b5
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114250"
---
# <a name="quickstart-deploy-a-java-reliable-services-application-to-service-fabric"></a>Schnellstart: Bereitstellen einer Java Reliable Services-Anwendung in Service Fabric

Azure Service Fabric ist eine Plattform, mit der verteilte Systeme bereitgestellt und skalierbare und zuverlässige Microservices und Container verwaltet werden können.

In diesem Schnellstart wird veranschaulicht, wie Sie Ihre erste Java-Anwendung in Service Fabric mithilfe der Eclipse-IDE auf einem Linux-Entwicklercomputer bereitstellen. Das Ergebnis ist eine Anwendung mit einem Java-Web-Front-End, mit der Abstimmungsergebnisse im Cluster in einem zustandsbehafteten Back-End-Dienst gespeichert werden.

![Screenshot der Anwendung](./media/service-fabric-quickstart-java/votingapp.png)

In dieser Schnellstartanleitung wird Folgendes vermittelt:

* Verwenden von Eclipse als Tool für Ihre Service Fabric-Java-Anwendungen
* Bereitstellen der Anwendung im Cluster
* Bereitstellen der Anwendung in einem Cluster in Azure
* Horizontales Hochskalieren der Anwendung über mehrere Knoten hinweg

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

1. [Installieren Sie das Service Fabric SDK und die Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Installation von Git](https://git-scm.com/)
3. [Installieren Sie Eclipse.](https://www.eclipse.org/downloads/)
4. [Richten Sie eine Java-Umgebung ein](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development), und führen Sie auch die optionalen Schritte für die Installation des Eclipse-Plug-Ins aus.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.

```git
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Lokales Ausführen der Anwendung

1. Starten Sie Ihren lokalen Cluster mit dem folgenden Befehl:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Es kann einige Zeit dauern, bis der lokale Cluster startet. Um zu prüfen, ob der Cluster vollständig betriebsbereit ist, greifen Sie unter **http://localhost:19080** auf Service Fabric Explorer zu. Wenn die fünf Knoten fehlerfrei sind, wird der lokale Cluster ausgeführt.

    ![Fehlerfreier lokaler Cluster](./media/service-fabric-quickstart-java/localclusterup.png)

2. Öffnen Sie Eclipse.
3. Klicken Sie auf „File“ (Datei) -> „Open Projects from File System…“ (Projekte aus Dateisystem öffnen…).
4. Klicken Sie auf „Directory“ (Verzeichnis), und wählen Sie im Ordner `service-fabric-java-quickstart` das Verzeichnis `Voting` aus, das Sie aus Github geklont haben. Klicken Sie auf Fertig stellen.

    ![Eclipse-Dialogfeld „Importieren“](./media/service-fabric-quickstart-java/eclipseimport.png)

5. Das Projekt `Voting` befindet sich nun im Paket-Explorer von Eclipse.
6. Klicken Sie mit der rechten Maustaste auf das Projekt und anschließend in der Dropdownliste **Service Fabric** auf **Publish Application...** (Anwendung veröffentlichen…). Wählen Sie **PublishProfiles/Local.json** als Zielprofil („Target Profile“) aus, und klicken Sie auf „Publish“ (Veröffentlichen).

    ![Lokales Dialogfeld „Veröffentlichen“](./media/service-fabric-quickstart-java/localjson.png)

7. Öffnen Sie Ihren bevorzugten Browser, und greifen Sie über **http://localhost:8080** auf die Anwendung zu.

    ![Lokales Front-End der Anwendung](./media/service-fabric-quickstart-java/runninglocally.png)

Sie können jetzt einen Satz mit Abstimmungsoptionen hinzufügen und die Abstimmung freigeben. Die Anwendung führt alle Daten in Ihrem Service Fabric-Cluster aus und speichert sie dort, ohne dass eine separate Datenbank verwendet werden muss.

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

   ![Installieren des Zertifikats in Firefox](./media/service-fabric-quickstart-java/install-cert-firefox.png)

### <a name="add-certificate-information-to-your-application"></a>Hinzufügen von Zertifikatinformationen zu Ihrer Anwendung

Der Zertifikatfingerabdruck muss Ihrer Anwendung hinzugefügt werden, da Service Fabric-Programmiermodelle verwendet werden.

1. Sie benötigen den Fingerabdruck Ihres Zertifikats in der Datei `Voting/VotingApplication/ApplicationManifest.xml` bei der Ausführung in einem sicheren Cluster. Führen Sie den folgenden Befehl aus, um den Fingerabdruck des Zertifikats zu extrahieren:

    ```bash
    openssl x509 -in [CERTIFICATE_PEM_FILE] -fingerprint -noout
    ```

2. Fügen Sie in der Datei `Voting/VotingApplication/ApplicationManifest.xml` unter dem Tag **ApplicationManifest** den folgenden Codeausschnitt hinzu. **X509FindValue** sollte der Fingerabdruck aus dem vorherigen Schritt sein (keine Semikolons). 

    ```xml
    <Certificates>
        <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
    </Certificates>
    ```

### <a name="deploy-the-application-using-eclipse"></a>Bereitstellen der Anwendung mithilfe von Eclipse

Nachdem die Anwendung und Ihr Cluster nun bereitstehen, können Sie sie über Eclipse direkt in einem Cluster bereitstellen.

1. Öffnen Sie im Verzeichnis **PublishProfiles** die Datei **Cloud.json**, und füllen Sie die Felder `ConnectionIPOrURL` und `ConnectionPort` ordnungsgemäß aus. Beispiel:

    ```bash
    {
         "ClusterConnectionParameters":
         {
            "ConnectionIPOrURL": "lnxxug0tlqm5.westus.cloudapp.azure.com",
            "ConnectionPort": "19080",
            "ClientKey": "[path_to_your_pem_file_on_local_machine]",
            "ClientCert": "[path_to_your_pem_file_on_local_machine]"
         }
    }
    ```

2. Klicken Sie mit der rechten Maustaste auf das Projekt und anschließend in der Dropdownliste **Service Fabric** auf **Publish Application...** (Anwendung veröffentlichen…). Wählen Sie **PublishProfiles/Cloud.json** als Zielprofil („Target Profile“) aus, und klicken Sie auf „Publish“ (Veröffentlichen).

    ![Clouddialogfeld „Veröffentlichen“](./media/service-fabric-quickstart-java/cloudjson.png)

3. Öffnen Sie Ihren Browser, und greifen Sie über **http://\<IP-Adresse oder URL der Verbindung>:8080** auf die Anwendung zu.

    ![Cloud-Front-End der Anwendung](./media/service-fabric-quickstart-java/runningcloud.png)

## <a name="scale-applications-and-services-in-a-cluster"></a>Skalieren von Anwendungen und Diensten in einem Cluster

Dienste können clusterweit skaliert werden, um eine Änderung der Last für die Dienste auszugleichen. Sie skalieren einen Dienst, indem Sie die Anzahl von Instanzen ändern, die im Cluster ausgeführt werden. Dienste können auf unterschiedliche Weise skaliert werden – beispielsweise mithilfe von Skripts oder Befehlen der Service Fabric-Befehlszeilenschnittstelle (sfctl). In den folgenden Schritten wird Service Fabric Explorer verwendet.

Service Fabric Explorer wird in allen Service Fabric-Clustern ausgeführt und kann in einem Browser geöffnet werden. Navigieren Sie hierzu zum HTTP-Verwaltungsport des Clusters (19.080). Beispiel: `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.

Gehen Sie zum Skalieren des Web-Front-End-Diensts wie folgt vor:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `https://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.
2. Klicken Sie auf das Auslassungszeichen (drei Punkte) neben dem Knoten **fabric:/Voting/VotingWeb** in der Strukturansicht, und wählen Sie **Scale Service** (Dienst skalieren).

    ![Service Fabric Explorer – Dienst skalieren](./media/service-fabric-quickstart-java/scaleservicejavaquickstart.png)

    Sie können jetzt angeben, dass die Anzahl von Instanzen des Web-Front-End-Diensts skaliert werden soll.

3. Ändern Sie die Anzahl in **2**, und klicken Sie auf **Scale Service** (Dienst skalieren).
4. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/Voting/VotingWeb**, und erweitern Sie den Partitionsknoten (durch eine GUID dargestellt).

    ![Service Fabric Explorer: Dienstskalierung abgeschlossen](./media/service-fabric-quickstart-java/servicescaled.png)

    Sie sehen nun, dass der Dienst über zwei Instanzen verfügt, und in der Strukturansicht ist zu erkennen, auf welchen Knoten die Instanzen ausgeführt werden.

Mit dieser einfachen Verwaltungsaufgabe haben Sie die Ressourcen verdoppelt, die dem Front-End-Dienst zur Bewältigung der Benutzerauslastung zur Verfügung stehen. Es ist wichtig zu verstehen, dass Sie nicht mehrere Instanzen eines Diensts benötigen, damit er zuverlässig ausgeführt wird. Sollte ein Dienst ausfallen, stellt Service Fabric sicher, dass im Cluster eine neue Dienstinstanz ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

* Verwenden von Eclipse als Tool für Ihre Service Fabric-Java-Anwendungen
* Bereitstellen von Java-Anwendungen im lokalen Cluster
* Bereitstellen von Java-Anwendungen in einem Azure-Cluster
* Horizontales Hochskalieren der Anwendung über mehrere Knoten hinweg

Weitere Informationen zur Verwendung von Java-Apps in Service Fabric finden Sie im Tutorial für Java-Apps.

> [!div class="nextstepaction"]
> [Bereitstellen einer Java-App](./service-fabric-tutorial-create-java-app.md)