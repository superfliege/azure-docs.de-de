---
title: Installieren von Presto in Azure HDInsight-Linux-Clustern
description: Erfahren Sie, wie Sie Presto und Airpal in Linux-basierten HDInsight Hadoop-Clustern mit Skriptaktionen installieren.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: hrasheed
ms.openlocfilehash: 60ff63a049f225886d69c1a89a2930671e533d78
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54910912"
---
# <a name="install-and-use-presto-on-hadoop-based-hdinsight-clusters"></a>Installieren und Verwenden von Presto in Hadoop-basierten HDInsight-Clustern

In diesem Artikel wird beschrieben, wie Sie Presto in Hadoop-basierten Adobe HDInsight-Clustern installieren, indem Sie Skriptaktionen verwenden. Sie erfahren außerdem, wie Sie Airpal auf einem vorhandenen Presto HDInsight-Cluster installieren.

HDInsight bietet außerdem die Anwendung Starburst Presto für Apache Hadoop-Cluster. Weitere Informationen finden Sie unter [Installieren von Apache Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apps-install-applications).

> [!IMPORTANT]  
> Für die Schritte in diesem Artikel wird ein HDInsight 3.5 Hadoop-Cluster mit Linux benötigt. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [HDInsight-Versionen](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Was ist Presto?
[Presto](https://prestodb.io/overview.html) ist eine schnelle, verteilte SQL-Abfrage-Engine für Big Data. Presto eignet sich für interaktive Abfragen von mehreren Petabyte an Daten. Weitere Informationen zu den Komponenten von Presto und zu ihrer Zusammenarbeit finden Sie unter [Presto-Konzepte](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]  
> Mit dem HDInsight-Cluster bereitgestellte Komponenten werden vollständig unterstützt. Der Microsoft-Support unterstützt Sie beim Isolieren und Beheben von Problemen im Zusammenhang mit diesen Komponenten.
> 
> Für benutzerdefinierte Komponenten wie Presto steht in wirtschaftlich angemessenem Rahmen Support für eine weiterführende Problembehandlung zur Verfügung. Über diesen Support lässt sich das Problem ggf. beheben. Unter Umständen werden Sie auch gebeten, verfügbare Kanäle für die Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. Es gibt viele Communitywebsites, die verwendet werden können. Beispiele hierfür sind das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) und [Stack Overflow](https://stackoverflow.com). 
>
> Für Apache-Projekte stehen auf der [Apache-Website](https://apache.org) auch Projektwebsites zur Verfügung. Ein Beispiel hierfür ist [Hadoop](https://hadoop.apache.org/).


## <a name="install-presto-by-using-script-actions"></a>Installieren von Presto mit Skriptaktionen

In diesem Abschnitt wird beschrieben, wie Sie das Beispielskript beim Erstellen eines neuen Clusters mit dem Azure-Portal verwenden: 

1. Beginnen Sie mit dem Bereitstellen eines Clusters, indem Sie die Schritte unter [Erstellen von Linux-basierten Clustern in HDInsight mit dem Azure-Portal](hdinsight-hadoop-create-linux-clusters-portal.md) ausführen. Achten Sie darauf, dass Sie den Cluster unbedingt mit dem **benutzerdefinierten** Clustererstellungsablauf erstellen. Der Cluster muss die folgenden Anforderungen erfüllen:

    * Er muss ein Hadoop-Cluster mit der HDInsight-Version 3.6 sein.

    * Er muss Azure Storage als Datenspeicher verwenden. Die Verwendung von Presto in einem Cluster, der Azure Data Lake Storage als Speicheroption verwendet, ist noch nicht möglich.

    ![HDInsight, Benutzerdefiniert (Größe, Einstellungen, Apps)](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Wählen Sie im Bereich **Erweiterte Einstellungen** die Option **Skriptaktionen**. Geben Sie die folgenden Informationen an. Sie können auch die Option **Presto installieren** für den Skripttyp auswählen:
   
   * **NAME**. Geben Sie einen Anzeigenamen für die Skriptaktion ein.
   * **Bash-Skript-URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`.
   * **HEAD**: Wählen Sie diese Option aus.
   * **WORKER**: Wählen Sie diese Option aus.
   * **ZOOKEEPER**: Lassen Sie dieses Kontrollkästchen deaktiviert.
   * **PARAMETER**: Lassen Sie dieses Feld leer.


3. Wählen Sie unten im Bereich **Skriptaktionen** die Schaltfläche **Auswählen**, um die Konfiguration zu speichern. Wählen Sie abschließend unten im Bereich **Erweiterte Einstellungen** die Schaltfläche **Auswählen**, um die Konfigurationsinformationen zu speichern.

4. Fahren Sie mit dem Bereitstellen des Clusters wie unter [Erstellen von Linux-basierten Clustern in HDInsight mit dem Azure-Portal](hdinsight-hadoop-create-linux-clusters-portal.md) beschrieben fort.

    > [!NOTE]  
    > Zum Anwenden von Skriptaktionen können auch Azure PowerShell, die klassische Azure-Befehlszeilenschnittstelle, das HDInsight .NET SDK oder Azure Resource Manager-Vorlagen verwendet werden. Sie können auch Skriptaktionen auf bereits ausgeführte Cluster anwenden. Weitere Informationen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Verwenden von Presto mit HDInsight

Gehen Sie wie folgt vor, um mit Presto in einem HDInsight-Cluster zu arbeiten:

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Starten Sie die Presto-Shell, indem Sie den folgenden Befehl ausführen:
   
    `presto --schema default`

3. Führen Sie eine Abfrage in der Beispieltabelle **hivesampletable** aus, die in allen HDInsight-Clustern standardmäßig verfügbar ist:
   
    `select count (*) from hivesampletable;`
   
    Die [Apache Hive](https://prestodb.io/docs/current/connector/hive.html)- und [TPCH](https://prestodb.io/docs/current/connector/tpch.html)-Connectors für Presto sind bereits vorkonfiguriert. Die Hive-Connector ist für die Verwendung der Hive-Standardinstallation konfiguriert. Alle Tabellen aus Hive sind in Presto also automatisch sichtbar.

    Weitere Informationen finden Sie in der [Presto-Dokumentation](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Verwenden von Airpal mit Presto

[Airpal](https://github.com/airbnb/airpal#airpal) ist eine webbasierte Open-Source-Abfrageschnittstelle für Presto. Weitere Informationen zu Airpal finden Sie in der [Airpal-Dokumentation](https://github.com/airbnb/airpal#airpal).

Führen Sie die folgenden Schritte aus, um Airpal auf dem Edgeknoten zu installieren:

1. Erstellen Sie per SSH eine Verbindung mit dem Hauptknoten im HDInsight-Cluster, auf dem Presto installiert ist:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Führen Sie nach dem Herstellen der Verbindung den folgenden Befehl aus:

    `sudo slider registry  --name presto1 --getexp presto` 
   
    Daraufhin wird eine Ausgabe angezeigt, die wie der folgende JSON-Code aussieht:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Notieren Sie sich in der Ausgabe den Wert der **value**-Eigenschaft. Sie benötigen diesen Wert beim Installieren von Airpal auf dem Edgeknoten des Clusters. Aus der obigen Ausgabe benötigen Sie den Wert **10.0.0.12:9090**.

4. Verwenden Sie [diese Vorlage](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json), um einen HDInsight-Cluster-Edgeknoten zu erstellen. Geben Sie die Werte an, die im folgenden Screenshot dargestellt sind.

    ![Benutzerdefinierte Bereitstellung](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Wählen Sie die Option **Kaufen**.

6. Greifen Sie auf die Airpal-Webschnittstelle zu, indem Sie im [Azure-Portal](https://portal.azure.com) folgende Schritte ausführen, nachdem die Änderungen auf die Clusterkonfiguration angewendet wurden:

    1. Wählen Sie im Menü links **Alle Dienste** aus.

    1. Wählen Sie unter **ANALYSEN** die Option **HDInsight clusters** (HDInsight-Cluster) aus.

    1. Wählen Sie Ihren Cluster in der Liste aus. Die Standardansicht wird geöffnet.

    1. Klicken Sie in der Standardansicht unter **Einstellungen** auf **Anwendungen**.

        ![HDInsight, Anwendungen](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    1. Suchen Sie auf der Seite **Installierte Apps** nach dem Tabelleneintrag für **airpal**. Wählen Sie **Portal**.

        ![Installierte Apps](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    1. Geben Sie bei Aufforderung die Administratoranmeldeinformationen ein, die Sie beim Erstellen des Hadoop-basierten HDInsight-Clusters angegeben haben.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Anpassen einer Presto-Installation in einem HDInsight-Cluster

Führen Sie die folgenden Schritte aus, um die Installation anzupassen:

1. Erstellen Sie per SSH eine Verbindung mit dem Hauptknoten im HDInsight-Cluster, auf dem Presto installiert ist:
   
    `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Nehmen Sie die Konfigurationsänderungen in der Datei `/var/lib/presto/presto-hdinsight-master/appConfig-default.json` vor. Weitere Informationen zur Presto-Konfiguration finden Sie unter [Presto Configuration Options for YARN-Based Clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html) (Presto-Konfigurationsoptionen für YARN-basierte Cluster).

3. Beenden Sie die derzeit ausgeführte Instanz von Presto:

    `sudo slider stop presto1 --force``sudo slider destroy presto1 --force`

4. Starten Sie eine neue Instanz von Presto mit der Anpassung:

    `sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json`

5. Warten Sie, bis die neue Instanz bereit ist. Beachten Sie die Adresse des Presto-Koordinators:


    `sudo slider registry --name presto1 --getexp presto`

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Generieren von Benchmarkdaten für HDInsight-Cluster mit Presto

TPC-DS ist der Industriestandard für das Messen der Leistung von vielen Decision Support-Systemen, z.B. Big Data-Systemen. Sie können Presto verwenden, um Daten zu generieren und mit Ihren eigenen HDInsight-Benchmarkdaten zu vergleichen. Weitere Informationen finden Sie unter [tpcds-hdinsight](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="next-steps"></a>Nächste Schritte
* [Installieren und Verwenden von Hue in HDInsight Hadoop-Clustern](hdinsight-hadoop-hue-linux.md). Hue ist eine Webbenutzeroberfläche, die das Erstellen, Ausführen und Speichern von Apache Pig- und Hive-Aufträgen vereinfacht.

* [Installieren von Apache Giraph in HDInsight Hadoop-Clustern und Verwenden von Giraph zur Verarbeitung großer Diagramme](hdinsight-hadoop-giraph-install-linux.md). Verwenden Sie die Clusteranpassung, um Giraph in Hadoop-basierten HDInsight-Clustern zu installieren. Mit Giraph können Sie die Graph-Verarbeitung mit Hadoop ausführen. Eine Verwendung mit Azure HDInsight ist ebenfalls möglich.

* [Installieren und Verwenden von Apache Solr in HDInsight Hadoop-Clustern](hdinsight-hadoop-solr-install-linux.md). Verwenden Sie die Clusteranpassung, um Solr in Hadoop-basierten HDInsight-Clustern zu installieren. Solr ermöglicht es Ihnen, leistungsstarke Suchvorgänge auf gespeicherte Daten anzuwenden.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
