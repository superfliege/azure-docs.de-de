---
title: 'Skriptaktion: Installieren von Python-Paketen mit Jupyter in Azure HDInsight'
description: Eine Schritt-für-Schritt-Anleitung zum Verwenden einer Skriptaktion für die Konfiguration von Jupyter Notebooks die mit Spark-Clustern in HDInsight verfügbar sind, sodass sie externe Python-Pakete verwenden.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 093a5eb7ee366abfdbc4c5dba68739544b438ff2
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684527"
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Installieren externer Python-Pakete für Jupyter Notebooks in Apache Spark-Clustern unter HDInsight mithilfe von Skriptaktionen
> [!div class="op_single_selector"]
> * [Verwenden von Cell Magic](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Verwenden von Skriptaktionen](apache-spark-python-package-installation.md)
>
>

Hier erfahren Sie, wie Sie mithilfe von Skriptaktionen einen Apache Spark-Cluster unter HDInsight (Linux) konfigurieren, um externe, von der Community bereitgestellte **Python**-Pakete zu verwenden, die nicht standardmäßig im Cluster enthalten sind.

> [!NOTE]
> Sie können ein Jupyter Notebook auch mit `%%configure`-Magic zur Verwendung externer Pakete konfigurieren. Anweisungen finden Sie unter [Verwenden externer Pakete mit Jupyter Notebooks in Apache Spark-Clustern unter HDInsight](apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Sie können den [Paketindex](https://pypi.python.org/pypi) nach einer vollständigen Liste mit verfügbaren Paketen durchsuchen. Sie können die Liste der verfügbaren Pakete auch aus anderen Quellen abrufen. So können Sie beispielsweise Pakete installieren, die über [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) oder [conda-forge](https://conda-forge.org/feedstocks/) verfügbar gemacht wurden.

In diesem Artikel erfahren Sie anhand eines Beispiels, wie Sie das [TensorFlow](https://www.tensorflow.org/)-Paket per Skriptaktion in Ihrem Cluster installieren und über das Jupyter Notebook verwenden.

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes:

* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Wenn Sie noch nicht über einen Spark-Cluster unter HDInsight (Linux) verfügen, können Sie während der Clustererstellung Skriptaktionen ausführen. Informationen zum Verwenden benutzerdefinierter Skriptaktionen finden Sie in der [Dokumentation](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 
   
   ## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Unterstützung für Open-Source-Software in HDInsight-Clustern

Der Microsoft Azure HDInsight-Dienst verwendet eine Reihe von Open-Source-Technologien für Hadoop. Microsoft Azure bietet lediglich allgemeinen Support für Open-Source-Technologien. Weitere Informationen finden Sie im Abschnitt **Supportumfang** auf der Website [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/). Der HDInsight-Dienst bietet zusätzliche Unterstützung für integrierte Komponenten.

Es gibt zwei Arten von Open-Source-Komponenten, die im HDInsight-Dienst verfügbar sind:

* **Integrierte Komponenten** – Diese Komponenten sind in HDInsight-Clustern vorinstalliert und stellen Kernfunktionen des Clusters bereit. So gehören beispielsweise Yarn Resource Manager, die Hive-Abfragesprache (HiveQL) und die Mahout Library zu dieser Kategorie. Eine vollständige Liste der Clusterkomponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Hadoop-Clusterversionen](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
* **Benutzerdefinierte Komponenten** – Als Benutzer des Clusters können Sie in Ihrem Workload eine beliebige in der Community verfügbare oder von Ihnen erstellte Komponente installieren oder verwenden.

> [!WARNING]
> Mit dem HDInsight-Cluster bereitgestellte Komponenten werden vollständig unterstützt. Der Microsoft-Support unterstützt Sie beim Isolieren und Beheben von Problemen im Zusammenhang mit diesen Komponenten.
>
> Für benutzerdefinierte Komponenten steht kommerziell angemessener Support für eine weiterführende Behebung des Problems zur Verfügung. Der Microsoft-Support kann das Problem möglicherweise beheben, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z.B. viele Communitywebsites verwendet werden, wie: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es auch Projektwebsites auf [http://apache.org](http://apache.org), z.B. [Hadoop](http://hadoop.apache.org/).


## <a name="use-external-packages-with-jupyter-notebooks"></a>Verwenden von externen Paketen mit Jupyter Notebooks

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.   

2. Klicken Sie auf dem Blatt des Spark-Clusters im linken Bereich auf **Skriptaktionen**. Verwenden Sie den Skripttyp „Benutzerdefiniert“, und geben Sie einen Anzeigenamen für die Skriptaktion ein. Führen Sie das Skript auf den **Haupt- und Workerknoten** aus, und lassen Sie das Parameterfeld leer. Das Bash-Skript finden Sie unter https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh – Informationen zum [Verwenden benutzerdefinierter Skriptaktionen](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux) finden Sie in der Dokumentation.

   > [!NOTE]
   > Im Cluster gibt es zwei Python-Installationen. Spark verwendet die Anaconda-Python-Installation unter `/usr/bin/anaconda/bin`. Verweisen Sie in Ihren benutzerdefinierten Aktionen mittels `/usr/bin/anaconda/bin/pip` und `/usr/bin/anaconda/bin/conda` auf diese Installation.
   > 
   > 

3. Öffnen eines PySpark-Jupyter Notebooks

    ![Erstellen eines neuen Jupyter Notebooks](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Erstellen eines neuen Jupyter Notebooks")

4. Ein neues Notebook mit dem Namen „Untitled.pynb“ wird erstellt und geöffnet. Klicken Sie oben auf den Namen des Notebooks, und geben Sie einen Anzeigenamen ein.

    ![Angeben eines neuen Namens für das Notebook](./media/apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Angeben eines neuen Namens für das Notebook")

5. In diesem Schritt importieren Sie TensorFlow (`import tensorflow`) und führen ein Hello World-Beispiel aus. 

    Zu kopierender Code:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    Das Ergebnis sieht wie folgt aus:
    
    ![Ausführung des TensorFlow-Codes](./media/apache-spark-python-package-installation/execution.png "Ausführung des TensorFlow-Codes")

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden externer Pakete mit Jupyter Notebooks in Apache Spark-Clustern unter HDInsight](apache-spark-jupyter-notebook-use-external-packages.md)
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)
