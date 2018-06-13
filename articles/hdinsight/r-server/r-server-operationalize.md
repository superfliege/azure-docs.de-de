---
title: Operationalisieren von R Server in HDInsight – Azure | Microsoft-Dokumentation
description: Informationen zum Operationalisieren von R Server in Azure HDInsight.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 03/23/2018
ms.author: nitinme
ms.openlocfilehash: 6de6e78d9b4ad68d268b59cff18c75fbdd7be757
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31412840"
---
# <a name="operationalize-r-server-cluster-on-azure-hdinsight"></a>Operationalisieren eines R Server-Clusters in HDInsight

Nachdem Sie mit dem R Server-Cluster Ihre Datenmodellierung in HDInsight vorgenommen haben, können Sie das Modell operationalisieren, um Vorhersagen zu treffen. Dieser Artikel enthält Anweisungen zur Durchführung dieser Aufgabe.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein R Server-Cluster in HDInsight**: Anweisungen finden Sie unter [Erste Schritte mit R Server in HDInsight](r-server-get-started.md).

* **Ein Secure Shell-Client (SSH)**: Ein SSH-Client wird verwendet, um Remoteverbindungen mit dem HDInsight-Cluster herzustellen und Befehle direkt im Cluster auszuführen. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="operationalize-r-server-cluster-with-one-box-configuration"></a>Operationalisieren des R Server-Clusters mit der Konfiguration mit einem einzelnen Computer

1. Stellen Sie per SSH eine Verbindung mit dem Edgeknoten her.  

        ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

    Anweisungen zum Verwenden von SSH mit Azure HDInsight finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Ändern Sie das Verzeichnis für die relevante Version, und rufen Sie die Dotnet-DLL per „sudo“ auf: 

    - Für Microsoft R Server 9.1:

            cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
            sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

    - Für Microsoft R Server 9.0:

            cd /usr/lib64/microsoft-deployr/9.0.1
            sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Die Optionen werden zur Auswahl angezeigt. Wählen Sie die erste Option wie im folgenden Screenshot gezeigt aus, um **R Server für die Operationalisierung zu konfigurieren**.

    ![Operationalisierung mit einem einzelnen Computer](./media/r-server-operationalize/admin-util-one-box-1.png)

4. Sie können jetzt eine Option für die Operationalisierung von R Server auswählen. Wählen Sie die erste der angebotenen Optionen durch Eingabe von **A** aus.

    ![Operationalisierung mit einem einzelnen Computer](./media/r-server-operationalize/admin-util-one-box-2.png)

5. Geben Sie bei Aufforderung das Kennwort für einen lokalen Administratorbenutzer ein bzw. erneut ein.

6. Daraufhin sollte ausgegeben werden, dass der Vorgang erfolgreich war. Sie werden außerdem aufgefordert, eine andere Option aus dem Menü auszuwählen. Wählen Sie „E“ aus, um zum Hauptmenü zurückzukehren.

    ![Operationalisierung mit einem einzelnen Computer](./media/r-server-operationalize/admin-util-one-box-3.png)

7. Optional können Sie die Diagnoseprüfungen ausführen, indem Sie wie folgt einen Diagnosetest ausführen:

    a. Wählen Sie im Hauptmenü **6** aus, um Diagnosetests auszuführen.

    ![Operationalisierung mit einem einzelnen Computer](./media/r-server-operationalize/diagnostic-1.png)

    b. Wählen Sie im Diagnosetestmenü **A** aus. Wenn Sie aufgefordert werden, geben Sie das Kennwort ein, das Sie für den lokalen Administratorbenutzer bereitgestellt haben.

    ![Operationalisierung mit einem einzelnen Computer](./media/r-server-operationalize/diagnostic-2.png)

    c. Überprüfen Sie, ob ausgegeben wird, dass die Gesamtintegrität einwandfrei ist.

    ![Operationalisierung mit einem einzelnen Computer](./media/r-server-operationalize/diagnostic-3.png)

    d. Geben Sie von den angebotenen Menüoptionen **E** ein, um zum Hauptmenü zurückzukehren, und geben Sie dann **8** ein, um das Verwaltungshilfsprogramm zu beenden.

### <a name="long-delays-when-consuming-web-service-on-spark"></a>Lange Verzögerungen bei der Nutzung des Webdiensts in Spark

Wenn bei der Nutzung eines Webdiensts, der mit mrsdeploy-Funktionen in einem Spark-Computekontext erstellt wurde, lange Verzögerungen auftreten, müssen Sie unter Umständen einige fehlende Ordner hinzufügen. Wenn die Spark-Anwendung von einem Webdienst mit mrsdeploy-Funktionen aufgerufen wird, gehört sie zu einem Benutzer namens *rserve2*. So können Sie dieses Problem umgehen:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


In dieser Phase ist die Konfiguration der Operationalisierung abgeschlossen. Nun können Sie das Paket `mrsdeploy` auf Ihrem RClient zum Herstellen einer Verbindung mit der Operationalisierung auf dem Edgeknoten verwenden und mit der Verwendung seiner Features wie [Remoteausführung](https://docs.microsoft.com/machine-learning-server/r/how-to-execute-code-remotely) und [Webdienste](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) beginnen. Je nachdem, ob Ihr Cluster in einem virtuellen Netzwerk eingerichtet ist, müssen Sie über die SSH-Anmeldung ein Tunneling für die Portweiterleitung einrichten. Die Einrichtung dieses Tunnels wird in den folgenden Abschnitten erläutert.

### <a name="r-server-cluster-on-virtual-network"></a>R Server-Cluster in virtuellem Netzwerk

Stellen Sie sicher, dass Sie Datenverkehr über Port 12800 zum Edgeknoten zulassen. Auf diese Weise können Sie den Edgeknoten zum Herstellen der Verbindung mit dem Feature „Operationalisierung“ verwenden.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Wenn über `remoteLogin()` keine Verbindung mit dem Edgeknoten hergestellt werden kann, Sie aber über SSH eine Verbindung mit dem Edgeknoten aufbauen können, müssen Sie überprüfen, ob die Regel zum Zulassen von Datenverkehr über Port 12800 ordnungsgemäß festgelegt wurde. Wenn das Problem weiterhin besteht, können Sie es umgehen, indem Sie über SSH das Tunneling der Portweiterleitung einrichten. Anweisungen finden Sie im folgenden Abschnitt:

### <a name="r-server-cluster-not-set-up-on-virtual-network"></a>Nicht im virtuellen Netzwerk eingerichteter R Server-Cluster

Wenn Ihr Cluster nicht im VNET eingerichtet wurde oder Sie Probleme mit der Konnektivität über das VNET haben, können Sie das Tunneling der SSH-Portweiterleitung nutzen:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Sobald Ihre SSH-Sitzung aktiv ist, wird der Datenverkehr in der SSH-Sitzung vom Port 12800 des Computers zum Port 12800 des Edgeknotens weitergeleitet. Stellen Sie sicher, dass Sie `127.0.0.1:12800` in der `remoteLogin()`-Methode verwenden. Hierdurch erfolgt eine Anmeldung bei der Operationalisierung des Edgeknotens über Portweiterleitung.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-operationalized-compute-nodes-on-hdinsight-worker-nodes"></a>Skalieren operationalisierter Serverknoten auf HDInsight-Workerknoten

Um die Serverknoten zu skalieren, nehmen Sie die Workerknoten zuerst außer Betrieb und konfigurieren dann Serverknoten auf den außer Betrieb gesetzten Workerknoten.

### <a name="step-1-decommission-the-worker-nodes"></a>Schritt 1: Außerbetriebsetzen der Workerknoten

Der R Server-Cluster wird nicht über YARN verwaltet. Wenn die Workerknoten nicht außer Betrieb gesetzt werden, funktioniert der YARN-Ressourcen-Manager nicht wie erwartet, da er die vom Server belegten Ressourcen nicht ermitteln kann. Um diese Situation zu vermeiden, empfehlen wir die Außerbetriebnahme der Workerknoten, bevor Sie die Serverknoten horizontal hochskalieren.

Führen Sie folgende Schritte zur Außerbetriebnahme der Workerknoten aus:

1. Melden Sie sich bei der Ambari-Konsole des Clusters an, und klicken Sie auf die Registerkarte **Hosts**.

2. Wählen Sie (außer Betrieb zu setzende) Workerknoten aus.

3. Klicken Sie auf **Aktionen** > **Ausgewählte Hosts** > **Hosts** > **Wartungsmodus EINSCHALTEN**. In der folgenden Abbildung haben wir z.B. „wn3“ und „wn4“ für die Außerbetriebnahme ausgewählt.  

   ![Außerbetriebnahme von Workerknoten](./media/r-server-operationalize/get-started-operationalization.png)  

* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **DataNodes** aus, und klicken Sie auf **Außer Betrieb setzen**.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **NodeManagers** aus, und klicken Sie auf **Außer Betrieb setzen**.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **DataNodes** aus, und klicken Sie auf **Beenden**.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **NodeManagers** aus, und klicken Sie auf **Beenden**.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **Hosts** aus, und klicken Sie auf **Alle Komponenten beenden**.
* Heben Sie die Auswahl der Workerknoten auf, und wählen Sie die Hauptknoten aus.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **Hosts** > **Alle Komponenten beenden** aus.

### <a name="step-2-configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Schritt 2: Konfigurieren von Serverknoten auf allen außer Betrieb gesetzten Workerknoten

1. Stellen Sie per SSH eine Verbindung mit jedem außer Betrieb gesetzten Workerknoten her.

2. Führen Sie das Verwaltungshilfsprogramm mit der entsprechenden DLL für Ihren R Server-Cluster aus. Führen Sie für R Server 9.1 die folgenden Schritte aus:

        dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Geben Sie **1** ein, um die Option **R Server für Operationalisierung konfigurieren** auszuwählen.

4. Geben Sie **C** zum Auswählen von Option `C. Compute node` ein. Hiermit wird der Serverknoten für den Workerknoten konfiguriert.

5. Beenden Sie das Verwaltungshilfsprogramm.

### <a name="step-3-add-compute-nodes-details-on-web-node"></a>Schritt 3: Hinzufügen von Details zu Serverknoten auf Webknoten

Nachdem alle außer Betrieb gesetzten Workerknoten für die Ausführung als Serverknoten konfiguriert sind, kehren Sie zum Edgeknoten zurück und fügen die IP-Adressen der außer Betrieb gesetzten Workerknoten der Konfiguration des Webknotens von R Server hinzu:

1. Stellen Sie per SSH eine Verbindung mit dem Edgeknoten her.

2. Führen Sie `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`aus.

3. Fügen Sie im Abschnitt „URIs“ die IP-Adress- und Portdetails des Workerknotens hinzu.

       "Uris": {
         "Description": "Update 'Values' section to point to your backend machines. Using HTTPS is highly recommended",
         "Values": [
           "http://localhost:12805", "http://[worker-node1-ip]:12805", "http://[workder-node2-ip]:12805"
         ]
       }

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten eines R Server-Clusters in HDInsight](r-server-hdinsight-manage.md)
* [Rechenkontextoptionen für R Server in HDInsight](r-server-compute-contexts.md)
* [Azure Storage-Lösungen für R Server in HDInsight](r-server-storage.md)
