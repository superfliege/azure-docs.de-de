---
title: Überwachen Ihrer Anwendungen in Azure Service Fabric mithilfe von ELK | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie ELK einrichten und Ihre Service Fabric-Anwendungen überwachen.
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
ms.date: 02/26/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 2c948a137abdbbf6ef8c64d26065030db1633a0a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-monitor-your-service-fabric-applications-using-elk"></a>Tutorial: Überwachen Ihrer Service Fabric-Anwendungen mithilfe von ELK 
Dieses Tutorial ist der vierte Teil einer Reihe. Hier erfahren Sie, wie Sie mithilfe von ELK (Elasticsearch, Logstash und Kibana) in Azure ausgeführte Service Fabric-Anwendungen überwachen. 

Im vierten Teil der Reihe lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten des ELK-Servers in Azure
> * Konfigurieren von Logstash für den Empfang von Protokollen von Event Hubs
> * Visualisieren von Plattform- und Anwendungsprotokollen in Kibana 

In dieser Tutorialreihe lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer Service Fabric Reliable Services-Java-Anwendung](service-fabric-tutorial-create-java-app.md)
> * [Bereitstellen und Debuggen der Anwendung in einem lokalen Cluster](service-fabric-tutorial-debug-log-local-cluster.md)
> * [Bereitstellen der Anwendung in einem Azure-Cluster](service-fabric-tutorial-java-deploy-azure.md)
> * Einrichten der Überwachung und Diagnose für die Anwendung
> * [Richten Sie CI/CD ein.](service-fabric-tutorial-java-jenkins.md)

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Richten Sie Ihre Anwendung so ein, dass sie Protokolle an den in [Teil 2](service-fabric-tutorial-debug-log-local-cluster.md) angegebenen Ort ausgibt.
- Absolvieren Sie [Teil 3](service-fabric-tutorial-java-deploy-azure.md), und vergewissern Sie sich, dass Sie über einen aktiven Service Fabric-Cluster verfügen, der zum Senden von Protokollen an Event Hubs konfiguriert ist. 
- Sie benötigen die Richtlinie in Event Hubs mit der Berechtigung „Lauschen“ sowie den dazugehörigen Primärschlüssel aus dem dritten Teil der Reihe.

## <a name="download-the-voting-sample-application"></a>Laden Sie die Beispielanwendung „Voting“ herunter.
Falls Sie die Beispielanwendung „Voting“ aus [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-java-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.

```bash
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart
```

## <a name="create-an-elk-server-in-azure"></a>Erstellen eines ELK-Servers in Azure
Sie können für dieses Tutorial eine vorkonfigurierte ELK-Umgebung verwenden und mit dem Abschnitt **Einrichten von Logstash** fortfahren, wenn Sie bereits über eine entsprechende Umgebung verfügen. Gehen Sie andernfalls wie folgt vor, um eine solche Umgebung in Azure zu erstellen: 

1. Erstellen Sie eine von [Bitnami](https://ms.portal.azure.com/#create/bitnami.elk4-6) zertifizierte ELK-Instanz in Azure. In diesem Tutorial müssen bei der Erstellung des Servers keine speziellen Anforderungen erfüllt werden. 

2. Navigieren Sie im Azure-Portal zu Ihrer Ressource, und klicken Sie im Abschnitt **Support + Problembehandlung** auf **Startdiagnose**. Klicken Sie anschließend auf die Registerkarte **Serial Log** (Serielles Protokoll).

    ![Startdiagnose](./media/service-fabric-tutorial-java-elk/bootdiagnostics.png)
3. Durchsuchen Sie die Protokolle nach dem Kennwort, das für den Zugriff auf die Kibana-Instanz benötigt wird. Orientieren Sie sich dabei am folgenden Codeausschnitt:

    ```bash
    [   25.932766] bitnami[1496]: #########################################################################
    [   25.948656] bitnami[1496]: #                                                                       #
    [   25.962448] bitnami[1496]: #        Setting Bitnami application password to '[PASSWORD]'           #
    [   25.978137] bitnami[1496]: #        (the default application username is 'user')                   #
    [   26.004770] bitnami[1496]: #                                                                       #
    [   26.029413] bitnami[1496]: #########################################################################
    ```

4. Klicken Sie im Azure-Portal auf der Übersichtsseite des Servers auf die Schaltfläche „Verbinden“, um die Anmeldeinformationen zu erhalten. 

    ![VM-Verbindung](./media/service-fabric-tutorial-java-elk/vmconnection.png)

5. Stellen Sie mithilfe des folgenden Befehls eine SSH-Verbindung mit dem Server her, der das ELK-Image hostet:

    ```bash
    ssh [USERNAME]@[CONNECTION-IP-OF-SERVER] 
    
    Example: ssh testaccount@104.40.63.157 
    ```

## <a name="set-up-elk"></a>Einrichten von ELK 

1. Als Erstes muss die ELK-Umgebung geladen werden.

    ```bash
    sudo /opt/bitnami/use_elk 
    ```

2. Wenn Sie eine vorhandene Umgebung verwenden, müssen Sie den folgenden Befehl ausführen, um den Logstash-Dienst zu beenden.

    ```bash
    sudo /opt/bitnami/ctlscript.sh stop logstash
    ```

3. Führen Sie den folgenden Befehl aus, um das Logstash-Plug-In für Event Hubs zu installieren: 

    ```bash
    logstash-plugin install logstash-input-azureeventhub
    ```

4. Erstellen Sie eine Logstash-Konfigurationsdatei mit folgendem Inhalt, oder passen Sie Ihre vorhandene Logstash-Konfigurationsdatei entsprechend an. Wenn Sie die Datei erstellen und das ELK-Bitnami-Image in Azure verwenden, muss die Datei unter ```/opt/bitnami/logstash/conf/access-log.conf``` erstellt werden. 

    ```json
    input
    {
        azureeventhub
        {
            key => "[RECEIVER-POLICY-KEY-FOR-EVENT-HUB]"
            username => "[RECEIVER-POLICY-NAME]"
            namespace => "[EVENTHUB-NAMESPACENAME]"
            eventhub => "[EVENTHUB-NAME]"
            partitions => 4
        }
    }
    
    output {
         elasticsearch {
             hosts => [ "127.0.0.1:9200" ]
         }
     }
    ```

5. Führen Sie den folgenden Befehl aus, um die Konfiguration zu überprüfen:

    ```bash 
    /opt/bitnami/logstash/bin/logstash -f /opt/bitnami/logstash/conf/ --config.test_and_exit
    ```

6. Starten Sie den Logstash-Dienst.

    ```bash
    sudo /opt/bitnami/ctlscript.sh start logstash
    ```

7. Überprüfen Sie Ihre Elasticsearch-Instanz, und vergewissern Sie sich, dass Sie Daten empfangen.

    ```bash
    curl 'localhost:9200/_cat/indices?v'
    ```

8. Rufen Sie unter **http://<IP-Adresse des Servers>** Ihr Kibana-Dashboard auf, und geben Sie den Benutzernamen und das Kennwort für Kibana ein. Wenn Sie das ELK-Image in Azure verwendet haben, lautet der Standardbenutzername „User“, und als Kennwort wird das bei der **Startdiagnose** abgerufene Kennwort verwendet. 

    ![Kibana](./media/service-fabric-tutorial-java-elk/kibana.png)    

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Einrichten eines ELK-Servers in Azure 
> * Konfigurieren des Servers für den Empfang von Diagnoseinformationen aus Ihrem Service Fabric-Cluster

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Einrichten von CI/CD mit Jenkins](service-fabric-tutorial-java-jenkins.md)

