---
title: Installieren von Presto in Azure HDInsight-Linux-Clustern
description: Erfahren Sie, wie Sie Presto und Airpal auf Linux-basierten HDInsight Hadoop-Clustern mit Skriptaktionen installieren.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: ea806a1004cf268fb7da75fa45013bdbaf882d86
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227500"
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von Presto in HDInsight Hadoop-Clustern

In diesem Dokument erfahren Sie, wie Sie Presto mithilfe von Skriptaktionen in HDInsight Hadoop-Clustern installieren. Sie erfahren außerdem, wie Sie Airpal auf einem vorhandenen Presto HDInsight-Cluster installieren.

HDInsight bietet außerdem die Anwendung Starburst Presto für Apache Hadoop-Cluster. Weitere Informationen finden Sie unter [Installieren von Apache Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apps-install-applications).

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen **HDInsight 3.5 Hadoop-Cluster** mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [HDInsight-Versionen](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Was ist Presto?
[Presto](https://prestodb.io/overview.html) ist eine schnelle, verteilte SQL-Abfrage-Engine für Big Data. Presto eignet sich für interaktive Abfragen von mehreren Petabyte an Daten. Weitere Informationen zu den Komponenten von Presto und zu ihrer Zusammenarbeit finden Sie unter [Presto-Konzepte](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Komponenten, die mit dem HDInsight-Cluster bereitgestellt werden, werden vollständig unterstützt, und Microsoft Support hilft Ihnen, Probleme im Zusammenhang mit diesen Komponenten zu isolieren und zu beheben.
> 
> Für benutzerdefinierte Komponenten wie Presto steht in wirtschaftlich angemessenem Rahmen Support für eine weiterführende Problembehandlung zur Verfügung. Auf diese Weise kann das Problem behoben werden, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z.B. viele Communitywebsites verwendet werden, wie: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es auch Projektwebsites auf [http://apache.org](http://apache.org), z.B. [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Installieren von Presto mit einer Skriptaktion

Dieser Abschnitt enthält Anweisungen zur Verwendung des Beispielskripts während der Erstellung eines neuen Clusters mithilfe des Azure-Portals. 

1. Beginnen Sie die Bereitstellung eines Clusters anhand der Schritte in [Bereitstellen Linux-basierter HDInsight-Cluster](hdinsight-hadoop-create-linux-clusters-portal.md). Erstellen Sie den Cluster unbedingt mit dem **benutzerdefinierten** Clustererstellungsablauf. Der Cluster muss die folgenden Anforderungen erfüllen.

    * Er muss ein Hadoop-Cluster mit der HDInsight-Version 3.6 sein.

    * Er muss Azure Storage als Datenspeicher verwenden. Die Verwendung von Presto in einem Cluster, der Azure Data Lake Storage als Speicheroption verwendet, ist noch nicht möglich.

    ![Erstellen von HDInsight-Clustern mit benutzerdefinierten Optionen](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Wählen Sie im Bereich **Erweiterte Einstellungen** die Option **Skriptaktionen** aus, und geben Sie die folgenden Informationen an: Sie können auch die Option „Presto installieren“ für den Skripttyp auswählen.
   
   * **NAME**: Geben Sie einen Anzeigenamen für die Skriptaktion ein.
   * **Bash-Skript-URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: Aktivieren Sie diese Option.
   * **WORKER**: Aktivieren Sie diese Option.
   * **ZOOKEEPER**: Lassen Sie dieses Kontrollkästchen deaktiviert.
   * **PARAMETERS**: Lassen Sie dieses Feld leer.


3. Klicken Sie unten im Bereich **Skriptaktionen** auf die Schaltfläche **Auswählen**, um die Konfiguration zu speichern. Klicken Sie abschließend unten im Bereich **Erweiterte Einstellungen** auf die Schaltfläche **Auswählen**, um die Konfigurationsinformationen zu speichern.

4. Setzen Sie die Bereitstellung des Clusters wie unter [Bereitstellen Linux-basierter HDInsight-Cluster](hdinsight-hadoop-create-linux-clusters-portal.md)beschrieben fort.

    > [!NOTE]
    > Zum Anwenden von Skriptaktionen können auch Azure PowerShell, die klassische Azure-Befehlszeilenschnittstelle, das HDInsight .NET SDK oder Azure Resource Manager-Vorlagen verwendet werden. Sie können auch Skriptaktionen auf bereits ausgeführte Cluster anwenden. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Verwenden von Presto mit HDInsight

Um mit Presto in einem HDInsight-Cluster zu arbeiten, gehen Sie wie folgt vor:

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Starten Sie die Presto-Shell mithilfe des folgenden Befehls.
   
        presto --schema default

3. Führen Sie eine Abfrage in der Beispieltabelle **hivesampletable** aus, die in allen HDInsight-Clustern standardmäßig verfügbar ist.
   
        select count (*) from hivesampletable;
   
    Die [Hive](https://prestodb.io/docs/current/connector/hive.html)- und [TPCH](https://prestodb.io/docs/current/connector/tpch.html)-Connectors für Presto sind bereits vorkonfiguriert. Der Hive-Connector ist für die Verwendung der Hive-Standardinstallation konfiguriert, damit alle Tabellen aus Hive automatisch in Presto angezeigt werden.

    Weitere Informationen finden Sie in der [Presto-Dokumentation](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Verwenden von Airpal mit Presto

[Airpal](https://github.com/airbnb/airpal#airpal) ist eine webbasierte Open-Source-Abfrageschnittstelle für Presto. Weitere Informationen zu Airpal finden Sie in der [Airpal-Dokumentation](https://github.com/airbnb/airpal#airpal).

Führen Sie die folgenden Schritte aus, um Airpal auf dem Edgeknoten zu installieren:

1. Erstellen Sie per SSH eine Verbindung mit dem Hauptknoten im HDInsight-Cluster, auf dem Presto installiert ist:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Führen Sie nach dem Herstellen der Verbindung den folgenden Befehl aus.

        sudo slider registry  --name presto1 --getexp presto 
   
    Daraufhin wird eine Ausgabe angezeigt, die wie der folgende JSON-Code aussieht:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Notieren Sie sich in der Ausgabe den Wert der **value**-Eigenschaft. Sie benötigen diesen Wert während der Installation von Airpal auf dem Edgeknoten des Clusters. In der obigen Ausgabe benötigen Sie den Wert **10.0.0.12:9090**.

4. Verwenden Sie die Vorlage **[hier](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** zum Erstellen eines Edgeknotens für einen HDInsight-Cluster, und geben Sie die Werte wie im folgenden Screenshot gezeigt an.

    ![HDInsight: Installation von Airpal in einem Presto-Cluster](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Klicken Sie auf **Kaufen**.

6. Nachdem die Änderungen auf die Clusterkonfiguration angewendet wurden, können Sie mithilfe der folgenden Schritte auf die Airpal-Webschnittstelle zugreifen.

    1. Klicken Sie im Dialogfeld des Clusters auf **Anwendungen**.

        ![HDInsight: Starten von Airpal in einem Presto-Cluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    2. Klicken Sie im Bereich **Installierte Apps** bei Airpal auf **Portal**.

        ![HDInsight: Starten von Airpal in einem Presto-Cluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    3. Geben Sie nach der entsprechenden Aufforderung die Administratoranmeldeinformationen ein, die Sie beim Erstellen des HDInsight Hadoop-Clusters angegeben haben.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Anpassen einer Presto-Installation in einem HDInsight-Cluster

Um die Installation anzupassen, führen Sie die folgenden Schritte aus:

1. Erstellen Sie per SSH eine Verbindung mit dem Hauptknoten im HDInsight-Cluster, auf dem Presto installiert ist:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Nehmen Sie die Konfigurationsänderungen in der Datei `/var/lib/presto/presto-hdinsight-master/appConfig-default.json` vor. Weitere Informationen zur Presto-Konfiguration finden Sie unter [Presto configuration for YARN-based clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html) (Presto-Konfiguration für YARN-basierte Cluster).

3. Beenden Sie die derzeit ausgeführte Instanz von Presto.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Starten Sie eine neue Instanz von Presto mit der Anpassung.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Warten Sie, bis die neue Instanz bereit ist, und notieren Sie sich die Adresse des Presto-Koordinators.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Generieren von Benchmarkdaten für HDInsight-Cluster mit Presto

TPC-DS ist der Industriestandard für das Messen der Leistung vieler Decision Support-Systeme, einschließlich Big Data-Systemen. Sie können Presto verwenden, um Daten zu generieren und mit Ihren eigenen HDInsight-Benchmarkdaten zu vergleichen. Weitere Informationen finden Sie [hier](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Weitere Informationen
* [Installieren und Verwenden von Hue in HDInsight-Clustern](hdinsight-hadoop-hue-linux.md). Hue ist eine Webbenutzeroberfläche, die das Erstellen, Ausführen und Speichern von Pig- und Hive-Aufträgen vereinfacht.

* [Installieren von Giraph in HDInsight-Clustern](hdinsight-hadoop-giraph-install-linux.md). Verwenden Sie die Clusteranpassung, um Giraph in HDInsight Hadoop-Clustern zu installieren. Giraph ermöglicht Ihnen, mithilfe von Hadoop Graphen zu verarbeiten, und kann mit Azure HDInsight verwendet werden.

* [Installieren von Solr in HDInsight-Clustern](hdinsight-hadoop-solr-install-linux.md). Verwenden Sie die Clusteranpassung, um Solr in HDInsight Hadoop-Clustern zu installieren. Solr ermöglicht Ihnen, leistungsstarke Suchvorgänge auf gespeicherte Daten anzuwenden.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
