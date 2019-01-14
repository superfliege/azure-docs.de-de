---
title: Erstellen von Apache Hadoop-Clustern mit einem Webbrowser – Azure HDInsight
description: Erfahren Sie, wie Apache Hadoop-, Apache HBase-, Apache Storm- und Apache Spark-Cluster unter Linux für HDInsight mithilfe eines Webbrowsers und des Azure-Vorschauportals erstellt werden.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/28/2018
ms.author: hrasheed
ms.openlocfilehash: ca8b398a796284e34ce4e0b577c335552ca75a98
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53974266"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Das Azure-Portal ist ein webbasiertes Verwaltungstool für Dienste und Ressourcen, die in der Microsoft Azure-Cloud gehostet werden. In diesem Artikel erfahren Sie, wie Sie mit dem Portal Linux-basierte HDInsight-Cluster erstellen.

## <a name="prerequisites"></a>Voraussetzungen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Ein zeitgemäßer Webbrowser**. Das Azure-Portal verwendet HTML5 und JavaScript und funktioniert in älteren Webbrowsern unter Umständen nicht richtig.

## <a name="create-clusters"></a>Erstellen von Clustern
Das Azure-Portal macht die meisten Clustereigenschaften verfügbar. Mithilfe von Azure Resource Manager-Vorlagen können Sie viele Details ausblenden. Weitere Informationen finden Sie unter [Erstellen Linux-basierter Apache Hadoop-Cluster in HDInsight mithilfe von Azure Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie im Menü auf der linken Seite auf **+ Ressource erstellen**.

1.  Wählen Sie unter **Azure Marketplace** die Option **Analyse** aus.

1.  Wählen Sie unter **Empfohlen** die Option **HDInsight** aus.
   
    ![Erstellen eines neuen Clusters im Azure-Portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster.png "Erstellen eines neuen Clusters im Azure-Portal")

1. Wählen Sie auf der Seite **HDInsight** die Option **Benutzerdefiniert (Größe, Einstellungen, Apps)** aus.

1. Klicken Sie auf **1 Grundlegende Einstellungen**, und geben Sie anschließend die folgenden Informationen ein:

    ![Erstellen eines neuen Clusters im Azure-Portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-basics.png "Erstellen eines neuen Clusters im Azure-Portal")

    * Geben Sie einen **Clusternamen** ein: Dieser Name muss global eindeutig sein.

    * Wählen Sie in der Dropdownliste **Abonnement** das Azure-Abonnement aus, das für den Cluster verwendet wird.

    * Klicken Sie auf **Clustertyp**, und wählen Sie dann den Clustertyp (Hadoop, Spark usw.) aus, den Sie erstellen möchten. Das **Betriebssystem** ist **Linux**. Wählen Sie anschließend eine Version des Clustertyps aus. Verwenden Sie die Standardversion, wenn Sie nicht wissen, was Sie auswählen sollen. Weitere Informationen finden Sie unter [HDInsight-Clusterversionen](hdinsight-component-versioning.md).
     
        > [!IMPORTANT]  
        > HDInsight-Cluster gibt es in vielen verschiedenen Typen, die der Workload oder Technologie entsprechen, für die der Cluster optimiert ist. Es ist keine unterstützte Methode zum Erstellen eines Clusters vorhanden, bei der mehrere Typen kombiniert werden, z.B. Storm und HBase in einem Cluster.
        
    * Geben Sie für **Clusterbenutzername** und **Clusteranmeldekennwort** den Benutzernamen und das Kennwort für den Administratorbenutzer ein.

    * Geben Sie einen **SSH-Benutzernamen** ein, und wenn Sie möchten, dass das SSH-Kennwort mit dem Administratorkennwort identisch ist, das Sie zuvor angegeben haben, aktivieren Sie das Kontrollkästchen **Dasselbe Kennwort wie für die Clusteranmeldung verwenden**. Falls nicht, geben Sie entweder ein **KENNWORT** oder einen **ÖFFENTLICHEN SCHLÜSSEL** zum Authentifizieren des SSH-Benutzers ein. Es wird empfohlen, einen öffentlichen Schlüssel zu verwenden. Klicken Sie unten auf **Auswählen** , um die Konfiguration der Anmeldeinformationen zu speichern.
   
        Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

    * Geben Sie für **Ressourcengruppe** an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene verwenden möchten.

    * Geben Sie einen **Speicherort** im Rechenzentrum an, an dem der Cluster erstellt wird.

    * Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

4. Unter **2 Sicherheit + Netzwerkbetrieb** können Sie Ihren Cluster über das Dropdownmenü mit einem virtuellen Netzwerk verbinden. Wählen Sie ein virtuelles Azure-Netzwerk und das Subnetz aus, wenn Sie den Cluster in einem virtuellen Netzwerk platzieren möchten. Informationen zur Verwendung von HDInsight mit Virtual Network, einschließlich spezifischer Konfigurationsanforderungen für Virtual Network, finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md). Wenn Sie das **Enterprise-Sicherheitspaket** verwenden möchten, können Sie auch die hier angegebenen Anweisungen befolgen: [Konfigurieren eines HDInsight-Clusters mit Enterprise-Sicherheitspaket (Enterprise Security Package, ESP) mithilfe von Azure Active Directory Domain Services](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds).

    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.


5. Geben Sie unter **3 Speicher** an, ob Azure Storage (WASB) oder Data Lake Storage als Standardspeicher verwendet werden soll. Weitere Informationen finden Sie weiter unten in der Tabelle.

     ![Erstellen eines neuen Clusters im Azure-Portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-storage.png "Erstellen eines neuen Clusters im Azure-Portal")

     | Storage                                      | BESCHREIBUNG |
     |----------------------------------------------|-------------|
     | **Azure Storage-Blobs als Standardspeicher**   | <ul><li>Wählen Sie für **Primärer Speichertyp** **Azure Storage**. Anschließend können Sie für **Auswahlmethode** die Option **Meine Abonnements** auswählen, wenn Sie ein Speicherkonto angeben möchten, das Bestandteil Ihres Azure-Abonnements ist. Wählen Sie dann das Speicherkonto aus. Klicken Sie andernfalls auf **Zugriffsschlüssel**, und geben Sie die Informationen für das nicht in Ihrem Azure-Abonnement enthaltene Speicherkonto an, das Sie auswählen möchten.</li><li>Als **Standardcontainer** können Sie den vom Portal vorgeschlagenen Standardcontainernamen verwenden oder einen eigenen angeben.</li><li>Wenn Sie WASB als Standardspeicher verwenden, können Sie (optional) auf **Zusätzliche Speicherkonten** klicken, um dem Cluster weitere Speicherkonten zuzuordnen. Klicken Sie unter **Azure-Speicherschlüssel** auf **Speicherschlüssel hinzufügen**. Anschließend können Sie ein Speicherkonto aus Ihren Azure-Abonnements oder anderen Abonnements bereitstellen (durch Bereitstellen der Speicherkonto-Zugriffsschlüssel).</li><li>Wenn Sie WASB als Standardspeicher verwenden, können Sie (optional) auf **Data Lake Storage-Zugriff** klicken, um Azure Data Lake Storage als zusätzlichen Speicher anzugeben. Weitere Informationen finden Sie unter [Schnellstart: Einrichten von Clustern in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
     | **Azure Data Lake Storage als Standardspeicher** | Wählen Sie unter **Primärer Speichertyp** die Option **Azure Data Lake Storage Gen1** oder **Azure Data Lake Storage Gen2** aus. Weitere Informationen mit den entsprechenden Anweisungen finden Sie im Artikel [Schnellstart: Einrichten von Clustern in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md), um entsprechende Anweisungen zu erhalten. |
     | **Externe Metastores**                      | Optional können Sie eine SQL-Datenbank angeben, die zum Speichern von Hive- und Oozie-Metadaten für den Cluster verwendet werden soll. Wählen Sie für **SQL-Datenbank für Hive auswählen** eine SQL-Datenbank aus, und geben Sie dann den Benutzernamen und das Kennwort für die Datenbank ein. Wiederholen Sie diese Schritte für Oozie-Metadaten.<br><br>Noch ein paar Überlegungen zur Verwendung der Azure SQL-Datenbank für Metastores. <ul><li>Die als Metastore verwendete Azure SQL-Datenbank muss für die Konnektivität mit anderen Azure-Diensten konfiguriert sein, inklusive Azure HDInsight. Klicken Sie im Dashboard der Azure SQL-Datenbank mit der rechten Maustaste auf den Servernamen. Dies ist der Server, auf dem die SQL-Datenbankinstanz läuft. Öffnen Sie die Serveransicht, klicken Sie auf **Konfigurieren**, wählen Sie unter **Azure Services** den Wert **Ja** aus, und klicken Sie auf **Speichern**.</li><li>Verwenden Sie beim Erstellen eines Metastores keinen Datenbanknamen, der Gedankenstriche oder Bindestriche enthält, da dadurch der Clustererstellungsprozess misslingen kann.</li></ul> |

     > [!WARNING]  
     > Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.

     Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.


6. Wählen Sie unter **4 Anwendungen (optional)** alle gewünschten Anwendungen aus.  Diese Anwendungen können von Microsoft oder von unabhängigen Softwareanbietern (Independent Software Vendors, ISVs) bezogen oder aber selbst entwickelt werden. Weitere Informationen finden Sie unter [Installieren von HDInsight-Anwendungen](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).

    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.


6. Unter **5 Clustergröße** finden Sie Informationen zu den Knoten, die für diesen Cluster verwendet werden. Legen Sie die Anzahl von Workerknoten fest, die Sie für den Cluster benötigen. Die geschätzten Kosten der Clusterausführung werden ebenfalls angezeigt.
   
    ![Knotenpreistarife](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-nodes.png "Anzahl der Clusterknoten angeben")
   
   > [!IMPORTANT]  
   > Wenn Sie mehr als 32 Workerknoten planen, entweder direkt bei der Erstellung des Clusters oder durch eine Skalierung des Clusters nach der Erstellung, müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen.
   > 
   > Weitere Informationen zu Knotengrößen und den damit verbundenen Kosten finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).
   
    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

8. Unter **6 Skriptaktionen** können Sie einen Cluster für die Installation von benutzerdefinierten Komponenten anpassen.  Verwenden Sie diese Option, wenn Sie einen Cluster bei seiner Erstellung mit einem benutzerdefinierten Skript anpassen möchten. Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

   Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

9. Überprüfen Sie unter **7 Zusammenfassung** die Informationen, die Sie zuvor eingegeben haben, und klicken Sie dann auf **Erstellen**.

     ![Knotenpreistarife](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-summary.png "Anzahl der Clusterknoten angeben")
    
    > [!NOTE]  
    > Die Erstellung des Clusters dauert in der Regel ca. 20 Minuten. Unter **Benachrichtigungen** können Sie den Bereitstellungsprozess überprüfen.

10. Klicken Sie nach Abschluss des Erstellungsprozesses in der Benachrichtigung **Bereitstellung erfolgreich** auf die Option **Zu Ressource wechseln**. Im Clusterfenster werden die folgenden Informationen angezeigt.
    
    ![Clusterschnittstelle](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "Clustereigenschaften")
    
    Die folgenden Informationen dienen zur Erläuterung der Symbole oben.
    
    * Auf der Registerkarte **Übersicht** werden alle grundlegenden Informationen zum Cluster angezeigt, z.B. der Name, die zugehörige Ressourcengruppe, der Standort, das Betriebssystem, die URL für das Cluster-Dashboard usw.
    * **Dashboard** führt Sie zu dem mit dem Cluster verbundenen Ambari-Portal.
    * **Secure Shell**: Für den Zugriff auf den Cluster über SSH erforderliche Informationen.
    * **Cluster skalieren** ermöglicht Ihnen, die Anzahl der mit dem Cluster verbundenen Workerknoten zu erhöhen.
      * **Löschen**: Löscht den HDInsight-Cluster.
    

## <a name="customize-clusters"></a>Anpassen von Clustern
* Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Löschen des Clusters
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problembehandlung

Falls beim Erstellen von HDInsight-Clustern Probleme auftreten, sehen Sie sich die [Voraussetzungen für die Zugriffssteuerung](hdinsight-hadoop-create-linux-clusters-portal.md) an.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie einen HDInsight-Cluster erfolgreich erstellt haben, nutzen Sie die folgenden Informationen, um zu erfahren, wie Sie mit Ihrem Cluster arbeiten:

### <a name="apache-hadoop-clusters"></a>Apache Hadoop-Cluster
* [Verwenden von Apache Hive mit HDInsight](hadoop/hdinsight-use-hive.md)
* [Verwenden von Apache Pig mit HDInsight](hadoop/hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase Cluster
* [Erste Schritte mit Apache HBase in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Entwickeln von Java-Anwendungen für Apache HBase in HDInsight](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm-Cluster
* [Entwickeln von Java-Topologien für Apache Storm in HDInsight](storm/apache-storm-develop-java-topology.md)
* [Verwenden von Python-Komponenten in Apache Storm in HDInsight](storm/apache-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit Apache Storm in HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Spark-Cluster
* [Erstellen einer eigenständigen Anwendung mit Scala](spark/apache-spark-create-standalone-application.md)
* [Ausführen von Remoteaufträgen in einem Apache Spark-Cluster mithilfe von Apache Livy](spark/apache-spark-livy-rest-interface.md)
* [Apache Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](spark/apache-spark-use-bi-tools.md)
* [Apache Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](spark/apache-spark-machine-learning-mllib-ipython.md)

