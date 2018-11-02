---
title: 'Azure-Toolkit für IntelliJ: Erstellen von Spark-Anwendungen für einen HDInsight-Cluster '
description: Verwenden Sie das Azure Toolkit für IntelliJ, um in Scala geschriebene Spark-Anwendungen zu entwickeln und diese an einen HDInsight Spark-Cluster zu senden.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: maxluk
ms.openlocfilehash: 51e1e7696ece46e63358b2ed6efa55bbf6ab01fd
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50420777"
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a>Erstellen von Spark-Anwendungen für einen HDInsight-Cluster mit dem Azure Toolkit für IntelliJ

Verwenden Sie das Plug-In „Azure-Toolkit für IntelliJ“, um in Scala geschriebene Spark-Anwendungen zu entwickeln und diese dann direkt aus der IntelliJ-IDE (Integrated Development Environment, integrierte Entwicklungsumgebung) an einen HDInsight Spark-Cluster zu senden. Sie können das Plug-In auf mehrere Arten verwenden:

* Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Entwickeln und lokales Ausführen einer Scala Spark-Anwendung

Um das Projekt zu erstellen, sehen Sie sich das Video [Erstellen von Spark-Anwendungen mit dem Azure-Toolkit für IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) an.

> [!IMPORTANT]
> Sie können dieses Plug-In nur zum Erstellen von Anwendungen und Übermitteln dieser an einen HDInsight Spark-Cluster unter Linux verwenden.
> 

## <a name="prerequisites"></a>Voraussetzungen

- Einen Apache Spark-Cluster unter HDInsight (Linux). Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
- Oracle Java Development Kit. Sie können das Kit über die [Oracle-Website](https://aka.ms/azure-jdks) installieren.
- IntelliJ IDEA. In diesem Artikel wird die Version 2017.1 verwendet. Sie können die Komponente über die [JetBrains-Website](https://www.jetbrains.com/idea/download/) installieren.

## <a name="install-azure-toolkit-for-intellij"></a>Installieren des Azure-Toolkits für IntelliJ
Installationsanweisungen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="get-started"></a>Erste Schritte
Der Benutzer kann sich entweder [beim Azure-Abonnement anmelden](#sign-in-to-your-azure-subscription) oder [einen HDInsight-Cluster verknüpfen](#link-a-cluster). Dabei kann er entweder eine Kombination aus Ambari-Benutzername und -Kennwort oder Anmeldeinformationen verwenden, die in eine Domäne eingebunden sind.


## <a name="sign-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement an.

1. Starten Sie die IntelliJ-IDE, und öffnen Sie den Azure Explorer. Wählen Sie im Menü **Ansicht** die Option **Toolfenster** und dann **Azure Explorer** aus.
       
   ![Link „Azure Explorer“](./media/apache-spark-intellij-tool-plugin/show-azure-explorer.png)

1. Klicken Sie mit der rechten Maustaste auf den Knoten **Azure**, und wählen Sie dann **Anmelden** aus.

1. Wählen Sie im Dialogfeld **Azure-Anmeldung** die Option **Anmelden** aus, und geben Sie Ihre Azure-Anmeldeinformationen ein.

    ![Dialogfeld „Azure-Anmeldung“](./media/apache-spark-intellij-tool-plugin/view-explorer-2.png)

1. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Abonnements auswählen** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Wählen Sie die Schaltfläche **Auswählen** aus.

    ![Dialogfeld zum Auswählen von Abonnements](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

1. Erweitern Sie auf der Registerkarte **Azure Explorer** die Option **HDInsight**, um die HDInsight Spark-Cluster in Ihrem Abonnement anzuzeigen.
   
    ![HDInsight Spark-Cluster im Azure Explorer](./media/apache-spark-intellij-tool-plugin/view-explorer-3.png)

1. Um die dem Cluster zugeordneten Ressourcen (z.B. Speicherkonten) anzuzeigen, können Sie einen Clusternamenknoten noch einmal erweitern.
   
    ![Ein erweiterter Clusternamenknoten](./media/apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="link-a-cluster"></a>Verknüpfen eines Clusters
Sie können einen normalen HDInsight-Cluster mithilfe des verwalteten Ambari-Benutzernamens verknüpfen. In ähnlicher Weise können Sie einen in eine Domäne eingebundenen HDInsight-Cluster unter Verwendung von Domäne und Benutzername verknüpfen, wie etwa user1@contoso.com. Sie können auch einen Livy-Dienstcluster verknüpfen.

1. Wählen Sie im **Azure-Explorer** den Befehl **Cluster verknüpfen** aus.

   ![Kontextmenü „Cluster verknüpfen“](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

2. Zum Verknüpfen von Clustern stehen Ihnen zwei Optionen zur Auswahl. 

   * Für das Verknüpfen eines HDInsight-Clusters wählen Sie **HDInsight-Cluster** im Feld **Clusterinformationen** aus, und geben Sie **Clustername/URL**, **Benutzername** und **Kennwort** ein.

      ![Dialogfeld zum Verknüpfen eines HDInsight-Clusters](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

   * Für das Verknüpfen eines Livy-Dienstclusters wählen Sie **Livy-Dienst** im Feld **Clusterinformationen** aus, und geben Sie **Livy-Endpunkt** und **Clustername** ein. **Yarn-Endpunkt** ist optional. Im Feld **Authentifizierung** stehen zwei Optionen zur Auswahl. Dies sind **Standardauthentifizierung** und **Keine Authentifizierung**. Bei Auswahl von **Standardauthentifizierung** sollten **Benutzername** und **Kennwort** bereitgestellt werden. Im Fall eines Authentifizierungsfehlers müssen Sie den Benutzernamen und das Kennwort überprüfen.
      
      ![Dialogfeld zum Verknüpfen eines Livy-Dienstclusters](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)
   
3. Ein verknüpfter Cluster wird im Knoten **HDInsight** angezeigt, wenn die eingegebenen Informationen richtig sind. Jetzt können Sie eine Anwendung an diesen verknüpften Cluster übermitteln.

   ![Verknüpfter Cluster](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

4. Sie können die Verknüpfung eines Clusters im **Azure-Explorer** auch aufheben.
   
   ![Cluster mit aufgehobener Verknüpfung](./media/apache-spark-intellij-tool-plugin/unlink.png)

## <a name="create-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Erstellen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster

1. Starten Sie IntelliJ IDEA, und erstellen Sie ein Projekt. Führen Sie im Dialogfeld **Neues Projekt** die folgenden Schritte aus: 

   a. Wählen Sie **HDInsight** > **Spark auf HDInsight (Scala)** aus.

   b. Wählen Sie in der Liste **Build-Tool** je nach Ihren Anforderungen einen der folgenden Einträge aus:

      * **Maven** für die Unterstützung des Scala-Projekterstellungs-Assistenten
      * **SBT** zum Verwalten von Abhängigkeiten und Erstellen für das Scala-Projekt

    ![Dialogfeld „Neues Projekt“](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

1. Klicken Sie auf **Weiter**.

1. Der Scala-Projekterstellungs-Assistent erkennt automatisch, ob das Scala-Plug-In installiert ist. Wählen Sie **Installieren** aus.

   ![Scala-Plug-In-Prüfung](./media/apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

1. Wählen Sie **OK** aus, um das Scala-Plug-In herunterzuladen. Befolgen Sie die Anweisungen zum Neustart von IntelliJ. 

   ![Das Installationsdialogfeld für das Scala Plug-In](./media/apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

1. Führen Sie im Fenster **Neues Projekt** folgende Schritte aus:  

    ![Auswählen des Spark-SDK](./media/apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Geben Sie einen Projektnamen und einen Speicherort ein.

   b. Wählen Sie in der Dropdownliste **Projekt-SDK** eine **Java 1.8** für Spark 2.x-Cluster bzw. **Java 1.7** für Spark 1.x-Cluster aus.

   c. In der Dropdownliste **Spark-Version** fügt der Scala-Projekterstellungs-Assistent die richtige Version von Spark-SDK und Scala-SDK ein. Wenn die Spark-Clusterversion niedriger als 2.0 ist, wählen Sie **Spark 1.x** aus. Wählen Sie andernfalls **Spark 2.x** aus. In diesem Beispiel wird **Spark 2.0.2 (Scala 2.11.8)** verwendet.

1. Wählen Sie **Fertig stellen** aus.

1. Das Spark-Projekt erstellt automatisch ein Artefakt für Sie. Um das Artefakt anzuzeigen, gehen Sie folgendermaßen vor:

   a. Wählen Sie im Menü **File** (Datei) die Option **Project Structure** (Projektstruktur) aus.

   b. Wählen Sie im Dialogfeld **Project Structure** (Projektstruktur) die Option **Artifacts** (Artefakte) aus, um das Standardartefakt anzuzeigen, das erstellt wird. Sie können auch ein eigenes Artefakt erstellen, indem Sie das Plus-Symbol (**+**) auswählen.

      ![Informationen zum Artefakt im Dialogfeld](./media/apache-spark-intellij-tool-plugin/default-artifact.png)
      
1. Fügen Sie den Quellcode der Anwendung wie folgt hinzu:

   a. Klicken Sie im Projekt-Explorer mit der rechten Maustaste auf **src**, zeigen Sie auf **Neu**, und wählen Sie dann **Scala Class** (Scala-Klasse) aus.
      
      ![Befehle zum Erstellen einer Scala-Klasse im Projektexplorer](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. Geben Sie im Dialogfeld **Create New Scala Class** (Neue Scala-Klasse erstellen) einen Namen ein. Wählen Sie unter **Kind** (Art) die Option **Object** aus, und wählen Sie dann **OK** aus.
      
      ![Dialogfeld „Create New Scala Class“ (Neue Scala-Klasse erstellen)](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. Fügen Sie den folgenden Code in die Datei **MyClusterApp.scala** ein. Der Code liest die Daten aus der Datei „HVAC.csv“ (die für alle HDInsight Spark-Cluster verfügbar ist), ruft die Zeilen ab, die nur eine Ziffer in der siebten Spalte der CSV-Datei enthalten, und schreibt die Ausgabe in **/HVACOut** unter dem Standardspeichercontainer für den Cluster.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Ausführen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster
Nachdem Sie eine Scala-Anwendung erstellt haben, können Sie diese an den Cluster übermitteln.

1. Suchen Sie im Projektexplorer nach einer Java- oder Scala-Datei, und wählen Sie dann im Kontextmenü die Option **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden).
    
      ![Befehl „Submit Spark Application to HDInsight“](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. Geben Sie im Konfigurationsfenster die folgenden Werte an, und klicken Sie dann auf **SparkJobRun**.

      ![Dialogfeld für die Spark-Übermittlung](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)
      
    * Wählen Sie für **Spark clusters (Linux only)**(Spark-Cluster [nur Linux]) den HDInsight Spark-Cluster aus, in dem Sie Ihre Anwendung ausführen möchten.

    * Wählen Sie ein Artefakt aus dem IntelliJ-Projekt oder von der Festplatte aus.

    * Feld **Main class name** (Name der Hauptklasse): Der Standardwert ist die Hauptklasse aus der ausgewählten Datei. Sie können die Klasse ändern, indem Sie auf die Auslassungspunkte (**...**) klicken und eine andere Klasse auswählen.   

    * Feld **Job Configurations** (Auftragskonfigurationen): Die Standardwerte sind wie in der Abbildung oben festgelegt. Sie können den Wert ändern oder ein neues Schlüssel-Wert-Paar für die Auftragsübermittlung hinzufügen. Weitere Informationen finden Sie unter [Apache Livy-REST-API](http://livy.incubator.apache.org./docs/latest/rest-api.html).

      ![Spark-Dialogfeld für die Übermittlung, Auftragskonfiguration, Bedeutung](./media/apache-spark-intellij-tool-plugin/submit-job-configurations.png)

    * Feld **Command line arguments** (Befehlszeilenargumente): Sie können bei Bedarf die Argumentwerte durch Leerzeichen getrennt für die Hauptklasse eingeben.

    * Felder **Referenced Jars** (Referenzierte JARs) und **Referenced Files** (Referenzierte Dateien): Sie können die Pfade für die referenzierten JARs und Dateien (sofern vorhanden) eingeben. Weitere Informationen finden Sie unter [Spark-Konfiguration](https://spark.apache.org/docs/latest/configuration.html#runtime-environment). 

      ![Spark-Dialogfeld für die Übermittlung, JAR-Dateien, Bedeutung](./media/apache-spark-intellij-tool-plugin/jar-files-meaning.png)

       > [!NOTE]
       > Informationen zum Hochladen der referenzierten JARs und referenzierten Dateien finden Sie unter [Hochladen von Ressourcen in einen Cluster](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer).
                         
    * **Upload Path** (Uploadpfad): Sie können den Speicherort für die Übermittlung von JAR- oder Scala-Projektressourcen angeben. Es werden drei Speichertypen unterstützt: **Azure Blob**, **Use Spark interactive session to upload artifacts** (Interaktive Spark-Sitzung zum Hochladen von Artefakten verwenden) und **Use cluster default storage account** (Standardspeicherkonto des Clusters verwenden). Der Screenshot unten ist ein Beispiel für Azure-Blob.

        ![Dialogfeld für die Spark-Übermittlung](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-upload-storage-types.png)

        ![Dialogfeld für die Spark-Übermittlung](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-upload-storage-blob.png)

3. Klicken Sie auf **SparkJobRun**, um das Projekt an den ausgewählten Cluster zu übermitteln. Auf der Registerkarte **Remote Spark Job in Cluster** (Spark-Remoteauftrag im Cluster) wird unten der Status der Auftragsausführung angezeigt. Sie können die Anwendung beenden, indem Sie auf die rote Schaltfläche klicken. Weiter unten in diesem Artikel im Abschnitt „Zugreifen auf und Verwalten von HDInsight Spark-Clustern mit dem Azure-Toolkit für IntelliJ“ wird beschrieben, wie Sie auf die Auftragsausgabe zugreifen.
      
     ![Fenster für die Spark-Übermittlung](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)


## <a name="debug-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Lokales Debuggen oder Remotedebuggen von Spark in einem HDInsight-Cluster 
Es wird noch eine andere Möglichkeit der Übermittlung von Spark Anwendungen an den Cluster empfohlen. Dazu werden die Parameter in der IDE **Run/Debug Configurations** (Ausführen/Debugkonfigurationen) festgelegt. Weitere Informationen finden Sie unter [Debuggen von Spark-Anwendungen in einem HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ per SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).



## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Zugreifen auf und Verwalten von HDInsight Spark-Clustern mit dem Azure-Toolkit für IntelliJ
Sie können mit dem Azure-Toolkit für IntelliJ verschiedene Vorgänge durchführen.

### <a name="access-the-job-view"></a>Zugreifen auf die Auftragsansicht
1. Erweitern Sie im Azure Explorer die Option **HDInsight** und den Namen des Spark-Clusters, und wählen Sie dann **Jobs** (Aufträge) aus.  

    ![Knoten „Auftragsansicht“](./media/apache-spark-intellij-tool-plugin/job-view-node.png)

1. Im rechten Bereich werden auf der Registerkarte **Spark Job View** (Spark-Auftragsansicht) alle Anwendungen angezeigt, die in dem Cluster ausgeführt wurden. Wählen Sie den Namen der Anwendung aus, zu der Sie weitere Details anzeigen möchten.

    ![Anwendungsdetails](./media/apache-spark-intellij-tool-plugin/view-job-logs.png)
    >Hinweis
    >

1. Wenn Sie grundlegende Informationen zum ausgeführten Auftrag anzeigen möchten, zeigen Sie auf das Auftragsdiagramm. Um das Phasendiagramm und von den einzelnen Aufträgen generierte Informationen anzuzeigen, wählen Sie einen Knoten im Auftragsdiagramm aus.

    ![Details zur Auftragsphase](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

1. Um häufig verwendete Protokolle anzuzeigen, z.B. *Driver Stderr*, *Driver Stdout* oder *Directory Info*, wählen Sie die Registerkarte **Log** (Protokoll) aus.

    ![Protokolldetails](./media/apache-spark-intellij-tool-plugin/Job-log-info.png)

1. Sie können auch die Spark History UI (Spark-Verlaufsbenutzeroberfläche) und die YARN UI (YARN-Benutzeroberfläche) (auf Anwendungsebene) anzeigen, indem Sie im oberen Bereich des Fensters einen Link auswählen.

### <a name="access-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver
1. Erweitern Sie im Azure Explorer die Option **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** (Spark-Verlaufsbenutzeroberfläche öffnen). 

1. Wenn Sie dazu aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster ein, die Sie beim Einrichten einer Verbindung mit dem Cluster angegeben haben.

1. Im Dashboard des Spark-Verlaufsservers können Sie anhand des Anwendungsnamens nach der Anwendung suchen, deren Ausführung Sie gerade beendet haben. Im obigen Code legen Sie den Namen der Anwendung mit `val conf = new SparkConf().setAppName("MyClusterApp")` fest. Daher lautet der Name Ihrer Spark-Anwendung **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Starten des Ambari-Portals
1. Erweitern Sie im Azure Explorer die Option **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** (Clusterverwaltungsportal (Ambari) öffnen). 

1. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Sie haben diese Anmeldeinformationen während der Einrichtung des Clusters angegeben.

### <a name="manage-azure-subscriptions"></a>Verwalten von Azure-Abonnements
Standardmäßig werden mit dem Azure-Toolkit für IntelliJ die Spark-Cluster Ihrer gesamten Azure-Abonnements aufgelistet. Bei Bedarf können Sie die Abonnements angeben, auf die Sie zugreifen möchten. 

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Hauptknoten **Azure**, und wählen Sie dann **Abonnements verwalten** aus. 

1. Deaktivieren Sie im Dialogfeld die Kontrollkästchen neben den Abonnements, auf die Sie nicht zugreifen möchten, und wählen Sie dann **Schließen** aus. Sie können auch **Abmelden** auswählen, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.

## <a name="spark-console"></a>Spark-Konsole
Sie können eine lokale Spark-Konsole (Scala) oder eine Spark-Konsole mit interaktiver Livy-Sitzung (Scala) ausführen.

### <a name="spark-local-consolescala"></a>Lokale Spark-Konsole (Scala)
1. Sie müssen die Abhängigkeiten manuell hinzufügen. Klicken Sie auf **File** (Datei)->**Project Structures** (Projektstrukturen)->**Project Settings** (Projekteinstellungen)->**Libraries** (Bibliotheken), klicken Sie dann auf **+**, und wählen Sie **From Maven...** (Aus Maven...) aus. Geben Sie dann **org.apache.spark:spark-repl_2.11:2.1.0** im Popupdialogfenster ein. Nachdem Sie die Abhängigkeiten in Bibliotheken hinzugefügt haben, müssen Sie die Abhängigkeit im Fenster **Project Structures** (Projektstrukturen) unter **Modules** (Module) in die ersten Zeile verschieben. Ändern Sie vor dem Verschieben **Scope** (Bereich) in **Runtime** (Laufzeit).

    ![Lokale Konsole – Hinzufügen von Abhängigkeiten in Bibliotheken](./media/apache-spark-intellij-tool-plugin/local-console-add-dependency-libraries.png)

    ![Lokale Konsole – Verschieben in erste Zeile](./media/apache-spark-intellij-tool-plugin/local-console-move-first-line.png)

2. Richten Sie die Konfiguration ein, wenn dies bisher noch nicht geschehen ist. Klicken Sie im Fenster **Run/Debug Configurations** (Konfigurationen ausführen/debuggen) auf **+**->**Azure HDInsight Spark**, wählen Sie die Registerkarte **Locally Run** (Lokal ausführen) und dann die Hauptklasse aus, und klicken Sie dann auf **OK**.

    ![Lokale Konsole – Festlegen der Konfiguration](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)
 
3. Öffnen Sie die Datei, die der Hauptklassendatei entspricht, klicken Sie mit der rechten Maustaste auf **Spark Console** (Spark-Konsole), und klicken Sie dann auf **Run Spark Local Console(Scala)** (Lokale Spark-Console (Scala) ausführen). Sie können auch zu **Tools**->**Spark Console** (Spark-Konsole)->**Run Spark Local Console(Scala)** (Lokale Spark-Console (Scala) ausführen) wechseln, um die Konsole zu starten.

    ![Lokaler Spark-Einstiegspunkt](./media/apache-spark-intellij-tool-plugin/spark-console-local-entry-script.png)

4. Nachdem Sie die lokale Konsole erfolgreich gestartet haben, sieht sie wie in der folgenden Abbildung aus. Sie können eine gewünschte Aktion ausführen. Geben Sie z.B. **sc.appName** ein, drücken Sie STRG+EINGABETASTE und das Ergebnis wird dann angezeigt.

    ![Ergebnis in der lokalen Konsole](./media/apache-spark-intellij-tool-plugin/local-console-result.png)

### <a name="spark-livy-interactive-session-consolescala"></a>Spark-Konsole mit interaktiver Livy-Sitzung (Scala)
Dies wird nur bei IntelliJ 2018.2 unterstützt.

1. Richten Sie die Konfiguration ein, wenn dies bisher noch nicht geschehen ist. Klicken Sie im Fenster **Run/Debug Configurations** (Konfigurationen ausführen/debuggen) auf **+**->**Azure HDInsight Spark**, wählen Sie die Registerkarte **Remotely Run in Cluster** (Im Cluster remote ausführen) und dann den Clusternamen und die Hauptklasse aus, und klicken Sie dann auf **OK**.

    ![Interaktive Konsole – Hinzufügen eines Konfigurationseintrags](./media/apache-spark-intellij-tool-plugin/interactive-console-add-config-entry.png)

    ![Interaktive Konsole – Festlegen der Konfiguration](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

2. Öffnen Sie die Datei, die der Hauptklasse entspricht, klicken Sie mit der rechten Maustaste auf **Spark Console** (Spark-Konsole), und klicken Sie dann auf **Run Spark Livy Interactive Session Console(Scala)** (Spark-Konsole mit interaktiver Livy-Sitzung (Scala) ausführen). Sie können auch zum Menü **Tools** wechseln, auf **Spark Console** (Spark-Konsole) und dann auf **Run Spark Livy Interactive Session Console(Scala)** (Spark-Konsole mit interaktiver Livy-Sitzung (Scala) ausführen) klicken, um die Konsole zu starten.

3. Nachdem Sie die Konsole erfolgreich gestartet haben, können Sie eine gewünschte Aktion ausführen. Geben Sie z.B. **sc.appName** ein, drücken Sie STRG+EINGABETASTE und das Ergebnis wird dann angezeigt.

    ![Ergebnis in der interaktiven Konsole](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Konvertieren vorhandener IntelliJ IDEA-Anwendungen für die Verwendung des Azure-Toolkits für IntelliJ
Sie können die vorhandenen Spark Scala-Anwendungen, die Sie in IntelliJ IDEA erstellt haben, so umwandeln, dass sie mit dem Azure-Toolkit für IntelliJ kompatibel sind. Anschließend können Sie das Plug-In verwenden, um die Anwendungen an einen HDInsight Spark-Cluster zu übermitteln.

1. Öffnen Sie die zugehörige IML-Datei einer vorhandenen Spark Scala-Anwendung, die mit IntelliJ IDEA erstellt wurde.

1. Auf der Stammebene befindet sich ein **module**-Element wie das folgende:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Fügen Sie dem Element `UniqueKey="HDInsightTool"` hinzu, damit das **module** -Element wie folgt aussieht:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

1. Speichern Sie die Änderungen. Ihre Anwendung sollte jetzt mit dem Azure-Toolkit für IntelliJ kompatibel sein. Sie können dies testen, indem Sie im Projektexplorer mit der rechten Maustaste auf den Projektnamen klicken. Im Popupmenü ist jetzt die Option **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight übermitteln) verfügbar.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Fehler bei lokaler Ausführung: *Bitte verwenden Sie einen größeren Heap*
Sie erhalten unter Umständen die folgenden Fehlermeldungen, wenn Sie bei der lokalen Ausführung in Spark 1.6 ein 32-Bit-Java SDK verwenden:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Diese Fehler treten auf, weil die Heapgröße für die Ausführung von Spark nicht groß genug ist. Für Spark sind mindestens 471 MB erforderlich. (Weitere Informationen finden Sie unter [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) Eine einfache Lösung ist die Verwendung eines 64-Bit-Java SDK. Sie können auch die JVM-Einstellungen in IntelliJ ändern, indem Sie die folgenden Optionen hinzufügen:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Hinzufügen von Optionen zum Feld „VM options“ (VM-Optionen) in IntelliJ](./media/apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>Häufig gestellte Fragen
Beim Verknüpfen eines Clusters würde ich empfehlen, die Anmeldeinformationen des Speichers anzugeben.

![Verknüpfen eines Clusters und Bereitstellen der Speicheranmeldeinformationen](./media/apache-spark-intellij-tool-plugin/link-cluster-with-storage-credential-intellij.png)

Es gibt zwei Modi zum Übermitteln der Aufträge. Wenn Speicheranmeldeinformationen bereitgestellt werden, wird der Auftrag im Batchmodus übermittelt. Andernfalls wird der interaktive Modus verwendet. Wenn der Cluster ausgelastet ist, erhalten Sie möglicherweise die folgende Fehlermeldung.

![IntelliJ-Fehler beim Abrufen bei ausgelastetem Cluster](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-upload.png)

![IntelliJ-Fehler beim Abrufen bei ausgelastetem Cluster](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-submit.png)

## <a name="feedback-and-known-issues"></a>Feedback und bekannte Probleme
Derzeit wird die direkte Anzeige von Spark-Ausgaben nicht unterstützt.

Falls Sie Vorschläge oder Feedback haben oder bei Verwendung dieses Plug-Ins Probleme auftreten, senden Sie eine E-Mail an hdivstool@microsoft.com.

## <a name="seealso"></a>Nächste Schritte
* [Übersicht: Apache Spark für Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demo
* Erstellen von Scala-Projekten (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) (Erstellen von Spark Scala-Anwendungen)
* Remotedebuggen (Video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) (Verwenden des Azure Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen in HDInsight-Clustern)

### <a name="scenarios"></a>Szenarien
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen über VPN](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen per SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)
