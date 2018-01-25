---
title: Transformieren von Daten mithilfe von Spark in Azure Data Factory | Microsoft-Dokumentation
description: "Dieses Tutorial bietet Schrittanleitungen zum Transformieren von Daten mithilfe einer Spark-Aktivität in Azure Data Factory."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2018
ms.author: shengc
ms.openlocfilehash: c2ec6706c92f229bb05ad9a19246c6ffe5f615c9
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="transform-data-in-the-cloud-by-using-spark-activity-in-azure-data-factory"></a>Transformieren von Daten in der Cloud mithilfe einer Spark-Aktivität in Azure Data Factory
In diesem Tutorial verwenden Sie das Azure-Portal, um eine Data Factory-Pipeline zu erstellen, die Daten mithilfe einer Spark-Aktivität und eines bedarfsgesteuerten verknüpften HDInsight-Diensts transformiert. In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory. 
> * Erstellen einer Pipeline mit einer Spark-Aktivität
> * Auslösen einer Pipelineausführung
> * Überwachen der Pipelineausführung.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) weiter.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure Storage-Konto**. Sie erstellen ein Python-Skript und eine Eingabedatei und laden diese in Azure Storage hoch. Die Ausgabe des Spark-Programms wird in diesem Storage-Konto gespeichert. Der bedarfsgesteuerte Spark-Cluster verwendet dieses Storage-Konto als primären Speicher.  
* **Azure PowerShell**. Befolgen Sie die Anweisungen unter [Get started with Azure PowerShell cmdlets](/powershell/azure/install-azurerm-ps) (Erste Schritte mit Azure PowerShell-Cmdlets).


### <a name="upload-python-script-to-your-blob-storage-account"></a>Hochladen eines Python-Skripts in Ihr Blob Storage-Konto
1. Erstellen Sie eine Python-Datei mit dem Namen **WordCount_Spark.py** und dem folgenden Inhalt: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Ersetzen Sie **&lt;storageAccountName&gt;** durch den Namen Ihres Azure Storage-Kontos. Speichern Sie dann die Datei. 
3. Erstellen Sie in Azure Blob Storage einen Container mit dem Namen **adftutorial**, falls dieser noch nicht vorhanden ist. 
4. Erstellen Sie einen Ordner mit dem Namen **spark**.
5. Erstellen Sie unterhalb des Ordners **spark** einen Unterordner mit dem Namen **script**. 
6. Laden Sie die Datei **WordCount_Spark.py** in den Unterordner **script** hoch. 


### <a name="upload-the-input-file"></a>Hochladen der Eingabedatei
1. Erstellen Sie eine Datei mit dem Namen **minecraftstory.txt** und etwas Text darin. Das Spark-Programm zählt die Wörter in diesem Text. 
2. Erstellen Sie im Ordner `spark` einen Unterordner mit dem Namen `inputfiles`. 
3. Laden Sie `minecraftstory.txt` in den Unterordner `inputfiles` hoch. 

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im Menü auf der linken Seite nacheinander auf **Neu**, **Data + Analytics** und **Data Factory**. 
   
   ![Neu -> Data Factory](./media/tutorial-transform-data-spark-portal/new-azure-data-factory-menu.png)
2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 
      
     ![Seite „Neue Data Factory“](./media/tutorial-transform-data-spark-portal/new-azure-data-factory.png)
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte für das Feld „Name“ der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
     ![Name nicht verfügbar – Fehler](./media/tutorial-transform-data-spark-portal/name-not-available-error.png)
3. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
4. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
      - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
      - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
      Bei einigen Schritten dieses Tutorials wird davon ausgegangen, dass Sie die Ressourcengruppe namens **ADFTutorialResourceGroup** verwenden. Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
4. Wählen Sie **V2 (Vorschau)** als **Version** aus.
5. Wählen Sie den **Standort** für die Data Factory aus. Derzeit können Sie mit Data Factory V2 nur in den Regionen „USA, Osten“, „USA, Osten 2“ und „Europa, Westen“ Data Factorys erstellen. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.
6. Wählen Sie die Option **An Dashboard anheften** aus.     
7. Klicken Sie auf **Create**.
8. Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status: **Die Data Factory wird bereitgestellt**. 

    ![Kachel „Die Data Factory wird bereitgestellt“](media//tutorial-transform-data-spark-portal/deploying-data-factory.png)
9. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt.
   
    ![Data Factory-Startseite](./media/tutorial-transform-data-spark-portal/data-factory-home-page.png)
10. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Anwendung für die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten.

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
In diesem Abschnitt erstellen Sie zwei verknüpfte Dienste: 
    
- Einen **verknüpften Azure Storage-Dienst**, der ein Azure Storage-Konto mit der Data Factory verknüpft. Dieser Speicher wird vom bedarfsgesteuerten HDInsight-Cluster verwendet. Er enthält auch das Spark-Skript, das ausgeführt werden soll. 
- Einen **bedarfsgesteuerten verknüpften HDInsight-Dienst**. Azure Data Factory erstellt automatisch einen HDInsight-Cluster, führt das Spark-Programm aus und löscht dann den HDInsight-Cluster, wenn dieser für einen vorab konfigurierten Zeitraum inaktiv ist. 

### <a name="create-an-azure-storage-linked-service"></a>Erstellen eines verknüpften Azure Storage-Diensts

1. Wechseln Sie im linken Bereich der Seite **Erste Schritte** zur Registerkarte **Bearbeiten**, wie in der folgenden Abbildung gezeigt: 

    ![Kachel zum Erstellen der Pipeline](./media/tutorial-transform-data-spark-portal/get-started-page.png)

2. Klicken Sie im unteren Bereich des Fensters auf **Verbindungen** und anschließend auf **+ Neu**. 

    ![Schaltfläche für eine neue Verbindung](./media/tutorial-transform-data-spark-portal/new-connection.png)
3. Wählen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die Option **Azure Blob Storage**, und klicken Sie dann auf **Weiter**. 

    ![Auswählen von „Azure Blob Storage“](./media/tutorial-transform-data-spark-portal/select-azure-storage.png)
4. Wählen Sie den Namen Ihres **Azure Storage-Kontos**aus, und klicken Sie auf **Speichern**. 

    ![Angeben des Blob Storage-Kontos](./media/tutorial-transform-data-spark-portal/new-azure-storage-linked-service.png)


### <a name="create-an-on-demand-hdinsight-linked-service"></a>Erstellen eines bedarfsgesteuerten verknüpften HDInsight-Diensts

1. Klicken Sie erneut auf die Schaltfläche **+ Neu**, um einen weiteren verknüpften Dienst zu erstellen. 
2. Wählen Sie im Fenster **Neuer verknüpfter Dienst** die Option **Azure HDInsight** aus, und klicken Sie dann auf **Weiter**. 

    ![Auswählen von „Azure HDInsight“](./media/tutorial-transform-data-spark-portal/select-azure-hdinsight.png)
2. Führen Sie im Fenster **Neuer verknüpfter Dienst** die folgenden Schritte aus: 

    1. Geben Sie unter **Name** die Zeichenfolge **AzureHDInsightLinkedService** ein.
    2. Vergewissern Sie sich, dass unter **Typ** die Option **On-demand HDInsight** (HDInsight (bedarfsgesteuert)) ausgewählt ist.
    3. Wählen Sie unter **Azure Storage Linked Service** (Mit Azure Storage verknüpfter Dienst) die Option **AzureStorage1** aus. Diesen verknüpften Dienst haben Sie in einem früheren Schritt erstellt. Sollten Sie einen anderen Namen verwendet haben, geben Sie stattdessen diesen Namen in das Feld ein. 
    4. Wählen Sie unter **Clustertyp** die Option **Spark** aus.
    5. Geben Sie die **ID des Dienstprinzipals** ein, der zum Erstellen eines HDInsight-Clusters berechtigt ist. Dieser Dienstprinzipal muss Mitglied der Rolle „Mitwirkender“ in dem Abonnement oder der Ressourcengruppe sein, in dem bzw. der der Cluster erstellt wird. Weitere Informationen finden Sie unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals](../azure-resource-manager/resource-group-create-service-principal-portal.md).
    6. Geben Sie den **Dienstprinzipalschlüssel** ein. 
    7. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe aus, die Sie auch beim Erstellen der Data Factory verwendet haben. Der Spark-Cluster wird in dieser Ressourcengruppe erstellt. 
    8. Erweitern Sie **Betriebssystemtyp**.
    9. Geben Sie unter **Name** einen Namen für den **Benutzer** des Clusters ein. 
    10. Geben Sie das **Kennwort** für den Benutzer ein. 
    11. Klicken Sie auf **Speichern**. 

        ![Einstellungen für den verknüpften HDInsight-Dienst](./media/tutorial-transform-data-spark-portal/azure-hdinsight-linked-service-settings.png)

> [!NOTE]
> Die Gesamtanzahl von Kernen, die Sie in jeder von Azure HDInsight unterstützten Azure-Region verwenden können, ist begrenzt. Bei bedarfsgesteuerten verknüpften HDInsight-Diensten wird der HDInsight-Cluster in Azure Storage im gleichen Speicherort erstellt, der als primärer Speicher verwendet wird. Stellen Sie sicher, dass Sie über genügend Kernkontingente verfügen, sodass der Cluster erfolgreich erstellt werden kann. Weitere Informationen finden Sie unter [Einrichten von Clustern in HDInsight mit Hadoop, Spark, Kafka usw](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). 

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

2. Klicken Sie auf die Schaltfläche „+“ (Pluszeichen) und anschließend im Menü auf **Pipeline**.

    ![Menü für neue Pipeline](./media/tutorial-transform-data-spark-portal/new-pipeline-menu.png)
3. Erweitern Sie in der Toolbox **Aktivitäten** die Option **HDInsight**, und ziehen Sie die Aktivität **Spark** aus der Toolbox **Aktivitäten** auf die Oberfläche des Pipeline-Designers. 

    ![Ziehen und Ablegen der Spark-Aktivität](./media/tutorial-transform-data-spark-portal/drag-drop-spark-activity.png)
4. Führen Sie in den Eigenschaften im unteren Bereich des Fensters für die Aktivität **Spark** die folgenden Schritte aus: 

    1. Wechseln Sie zur Registerkarte **HDI Cluster** (HDI-Cluster).
    2. Wählen Sie den verknüpften Dienst **AzureHDInsightLinkedService** aus, den Sie im vorherigen Schritt erstellt haben. 
        
    ![Angeben des verknüpften HDInsight-Diensts](./media/tutorial-transform-data-spark-portal/select-hdinsight-linked-service.png)
5. Wechseln Sie zur Registerkarte **Script/Jar**, und führen Sie die folgenden Schritte aus: 

    1. Wählen Sie unter **Job Linked Service** (Mit Auftrag verknüpfter Dienst) die Option **AzureStorage1** aus.
    2. Klicken Sie auf **Speicher durchsuchen**. 
    3. Navigieren Sie zum Ordner **adftutorial/spark/script**, wählen Sie **WordCount_Spark.py** aus, und klicken Sie auf **Fertig stellen**.      

    ![Angeben des Spark-Skripts](./media/tutorial-transform-data-spark-portal/specify-spark-script.png)
6. Klicken Sie zum Überprüfen der Pipeline auf der Symbolleiste auf die Schaltfläche **Überprüfen**. Klicken Sie auf **>>** (Pfeil nach rechts), um das Überprüfungsfenster zu schließen. 
    
    ![Schaltfläche „Überprüfen“](./media/tutorial-transform-data-spark-portal/validate-button.png)
7. Klicken Sie auf **Veröffentlichen**. Die Data Factory-Benutzeroberfläche veröffentlicht Entitäten (verknüpfte Dienste und Pipeline) für den Azure Data Factory-Dienst. 
    
    ![Schaltfläche "Veröffentlichen"](./media/tutorial-transform-data-spark-portal/publish-button.png)

## <a name="trigger-a-pipeline-run"></a>Auslösen einer Pipelineausführung
Klicken Sie auf der Symbolleiste auf **Trigger** und dann auf **Trigger Now** (Jetzt auslösen). 

![Manuelles Auslösen](./media/tutorial-transform-data-spark-portal/trigger-now-menu.png)

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

1. Wechseln Sie zur Registerkarte **Überwachen**. Vergewissern Sie sich, dass eine Pipelineausführung angezeigt wird. Die Erstellung eines Spark-Clusters dauert etwa 20 Minuten. 

    ![Überwachen der Pipelineausführungen](./media/tutorial-transform-data-spark-portal/monitor-tab.png)
2. Klicken Sie auf gelegentlich auf **Aktualisieren**, um den Status der Pipelineausführung zu überprüfen. 

    ![Status der Pipelineausführung](./media/tutorial-transform-data-spark-portal/pipeline-run-succeeded.png)
1. Wenn Sie mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzeigen möchten, klicken Sie in der Spalte „Aktionen“ auf die Aktion **View Activity Runs** (Aktivitätsausführungen anzeigen). Durch Klicken auf den Link **Pipelines** (im oberen Bereich) können Sie zur Ansicht mit den Pipelineausführungen zurückkehren.

    ![Ansicht mit den Aktivitätsausführungen](./media/tutorial-transform-data-spark-portal/activity-runs.png)

## <a name="verify-the-output"></a>Überprüfen der Ausgabe
Vergewissern Sie sich, dass die Ausgabedatei im Ordner „spark/outputfiles/wordcount“ des Containers „adftutorial“ erstellt wird. 

![Überprüfen der Ausgabe](./media/tutorial-transform-data-spark-portal/verity-output.png)

Die Datei sollte für jedes Wort aus der Eingabetextdatei angeben, wie oft es in der Datei vorkommt. Beispiel:  

```
(u'This', 1)
(u'a', 1)
(u'is', 1)
(u'test', 1)
(u'file', 1)
```

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel transformiert Daten mithilfe einer Spark-Aktivität und eines bedarfsgesteuerten verknüpften HDInsight-Diensts. Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory. 
> * Erstellen einer Pipeline mit einer Spark-Aktivität
> * Auslösen einer Pipelineausführung
> * Überwachen der Pipelineausführung.

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Daten transformieren, indem Sie ein Hive-Skript in einem Azure HDInsight-Cluster ausführen, der sich in einem virtuellen Netzwerk befindet. 

> [!div class="nextstepaction"]
> [Tutorial: Transformieren von Daten mithilfe von Hive in Azure Virtual Network](tutorial-transform-data-hive-virtual-network-portal.md)





