---
title: Strukturiertes Apache Spark-Streaming mit Kafka – Azure HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Apache Spark-Streaming (DStream) verwenden, um Daten in oder aus Apache Kafka zu streamen. In diesem Beispiel streamen Sie Daten mithilfe eines Jupyter Notebooks aus Spark auf HDInsight.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2018
ms.author: larryfr
ms.openlocfilehash: 49c13bbea537d7de60ecf509bc28675191c0b34d
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="use-spark-structured-streaming-with-kafka-on-hdinsight"></a>Verwenden von strukturiertem Spark-Streaming mit Kafka in HDInsight

Erfahren Sie, wie Sie strukturiertes Spark-Streaming verwenden, um Daten aus Apache Kafka auf Azure HDInsight zu lesen.

Strukturiertes Spark-Streaming ist eine auf Spark SQL basierende Stream-Verarbeitungs-Engine. Sie können damit Streamingberechnungen genauso wie Batchberechnung auf statischen Daten ausdrücken. Weitere Informationen zu strukturiertem Streaming finden Sie unter [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) (Programmierhandbuch zu strukturiertem Streaming [Alpha]) auf Apache.org.

> [!IMPORTANT]
> In diesem Beispiel wird Spark 2.2 in HDInsight 3.6 verwendet.
>
> Mit den in diesem Dokument beschriebenen Schritten wird eine Azure-Ressourcengruppe erstellt, die jeweils einen Spark- und einen Kafka-Cluster in HDInsight beinhaltet. Die Cluster befinden sich innerhalb eines virtuellen Azure-Netzwerks, wodurch Spark- und Kafka-Cluster direkt miteinander kommunizieren können.
>
> Denken Sie nach dem Ausführen der Schritte in diesem Dokument daran, die Cluster zu löschen, um das Anfallen von Gebühren zu verhindern.

## <a name="create-the-clusters"></a>Erstellen von Clustern

Apache Kafka in HDInsight ermöglicht keinen Zugriff auf die Kafka-Broker über das öffentliche Internet. Alle von Kafka verwendeten Elemente müssen sich im gleichen virtuellen Azure-Netzwerk befinden. In diesem Tutorial befinden sich der Kafka- und der Spark-Cluster im gleichen virtuellen Azure-Netzwerk. 

Das folgende Diagramm zeigt den Kommunikationsfluss zwischen Spark und Kafka:

![Diagramm der Spark- und Kafka-Cluster in einem virtuellen Azure-Netzwerk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Der Kafka-Dienst ist auf die Kommunikation innerhalb des virtuellen Netzwerks beschränkt. Auf andere Dienste auf dem Cluster, wie z.B. SSH und Ambari, kann über das Internet zugegriffen werden. Weitere Informationen zu den öffentlichen Ports, die für HDInsight verfügbar sind, finden Sie unter [Von HDInsight verwendete Ports und URIs](hdinsight-hadoop-port-settings-for-services.md).

Der Einfachheit halber wird in den folgenden Schritten eine Azure Resource Manager-Vorlage verwendet, um den Kafka- und den Spark-Cluster in einem virtuellen Netzwerk zu erstellen.

1. Verwenden Sie die folgende Schaltfläche, um sich bei Azure anzumelden, und öffnen Sie die Vorlage im Azure-Portal.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Azure Resource Manager-Vorlage finden Sie unter **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json**.

    Diese Vorlage erstellt die folgenden Ressourcen:

    * Ein Kafka-auf-HDInsight-3.6-Cluster.
    * Einen Spark 2.2.0-Cluster in HDInsight 3.6.
    * Ein Azure Virtual Network, das die HDInsight-Cluster enthält.

    > [!IMPORTANT]
    > Das in diesem Beispiel für strukturiertes Streaming verwendete Notebook benötigt Spark auf HDInsight 3.6. Bei Verwendung einer früheren Version von Spark auf HDInsight erhalten Sie Fehlermeldungen, wenn Sie das Notebook verwenden.

2. Geben Sie im Abschnitt **Benutzerdefinierte Vorlage** folgende Informationen an:

    | Einstellung | Wert |
    | --- | --- |
    | Abonnement | Ihr Azure-Abonnement |
    | Ressourcengruppe | Die Ressourcengruppe mit den Ressourcen. |
    | Standort | Die Azure-Region, in der die Ressourcen erstellt werden. |
    | Spark Cluster Name (Spark-Clustername) | Der Name des Spark-Clusters. |
    | Kafka Cluster Name (Kafka-Clustername) | Der Name des Kafka-Clusters. |
    | Benutzername für Clusteranmeldung | Der Administratorbenutzername für die Cluster. |
    | Kennwort für Clusteranmeldung | Das Administratorbenutzerkennwort für die Cluster. |
    | SSH-Benutzername | Der SSH-Benutzer, der für die Cluster erstellt werden soll. |
    | SSH-Kennwort | Das Kennwort für den SSH-Benutzer. |
   
    ![Screenshot der angepassten Vorlage](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

4. Aktivieren Sie zum Schluss **An Dashboard anheften**, und wählen Sie dann **Kaufen** aus. 

> [!NOTE]
> Die Clustererstellung kann bis zu 20 Minuten dauern.

## <a name="get-the-notebook"></a>Abrufen des Notebooks

Den Code für das in diesem Dokument beschriebene Beispiel finden Sie unter [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-the-notebooks"></a>Hochladen der Notebooks

Gehen Sie wie folgt vor, um das Notebook aus dem Projekt in Ihren Spark-Cluster in HDInsight hochzuladen:

1. Verbinden Sie in Ihrem Webbrowser das Jupyter Notebook mit Ihrem Spark-Cluster. Ersetzen Sie in der folgenden URL `CLUSTERNAME` durch den Namen Ihres __Spark__-Clusters:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Geben Sie bei Aufforderung den Clusterbenutzernamen (Administrator) und das Kennwort ein, den bzw. das Sie beim Erstellen des Clusters verwendet haben.

2. Klicken Sie rechts oben auf der Seite auf die Schaltfläche __Hochladen__, um die Datei __spark-structured-streaming-kafka.ipynb__ in den Cluster hochzuladen. Wählen Sie __Öffnen__, um das Hochladen zu starten.

    ![Verwenden der Schaltfläche „Hochladen“ für die Auswahl und das Hochladen eines Notebooks](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Auswahl der KafkaStreaming.ipynb-Datei](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Suchen Sie in der Liste mit den Notebooks nach dem Eintrag __spark-structured-streaming-kafka.ipynb__, und klicken Sie neben dem Eintrag auf die Schaltfläche __Hochladen__.

    ![Verwenden Sie zum Hochladen des Notebooks die Schaltfläche „Hochladen“ neben dem Eintrag „KafkaStreaming.ipynb“.](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)


## <a name="use-the-notebook"></a>Verwenden des Notebooks

Klicken Sie nach dem Hochladen der Dateien auf den Eintrag __spark-structured-streaming-kafka.ipynb__, um das Notebook zu öffnen. Folgen Sie den Anweisungen im Notebook, um sich mit der Verwendung von Spark Structured Streaming mit Kafka in HDInsight vertraut zu machen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Zum Bereinigen der im Rahmen dieses Tutorials erstellten Ressourcen können Sie die Ressourcengruppe löschen. Dadurch werden auch der zugeordnete HDInsight-Cluster sowie alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

So entfernen Sie die Ressourcengruppe über das Azure-Portal:

1. Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf __Ressourcengruppen__, um die Liste mit Ihren Ressourcengruppen anzuzeigen.
2. Suchen Sie die zu löschende Ressourcengruppe, und klicken Sie mit der rechten Maustaste rechts neben dem Eintrag auf die Schaltfläche __Mehr__ (...).
3. Klicken Sie auf __Ressourcengruppe löschen__, und bestätigen Sie den Vorgang.

> [!WARNING]
> Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden.
> 
> Wenn Sie einen Kafka-Cluster in HDInsight löschen, werden auch alle in Kafka gespeicherten Daten gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt wissen, wie Sie strukturiertes Spark-Streaming verwenden, finden Sie in den folgenden Dokumenten mehr Informationen zum Arbeiten mit Spark und Kafka:

* [How to use Spark streaming (DStream) with Kafka (Verwenden von Spark-Streaming (DStream) mit Kafka)](hdinsight-apache-spark-with-kafka.md)
* [Start with Jupyter Notebook and Spark on HDInsight (Erste Schritte mit Jupyter Notebook und Spark auf HDInsight)](spark/apache-spark-jupyter-spark-sql.md)