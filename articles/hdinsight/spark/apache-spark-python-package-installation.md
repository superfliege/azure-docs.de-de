---
title: 'Skriptaktion: Installieren von Python-Paketen mit Jupyter in Azure HDInsight | Microsoft-Dokumentation'
description: "Eine Schritt-für-Schritt-Anleitung zum Verwenden einer Skriptaktion für die Konfiguration von Jupyter Notebooks die mit Spark-Clustern in HDInsight verfügbar sind, sodass sie externe Python-Pakete verwenden."
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21978b71-eb53-480b-a3d1-c5d428a7eb5b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: maxluk
ms.openlocfilehash: 57ee87b848e30f33b816d1d0564394ecc998e6b5
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
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

Sie können den [Paketindex](https://pypi.python.org/pypi) nach einer vollständigen Liste mit verfügbaren Paketen durchsuchen. Sie können die Liste der verfügbaren Pakete auch aus anderen Quellen abrufen. So können Sie beispielsweise Pakete installieren, die über [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) oder [conda-forge](https://conda-forge.github.io/feedstocks.html) verfügbar gemacht wurden.

In diesem Artikel erfahren Sie, wie Sie das [TensorFlow](https://www.tensorflow.org/)-Paket per Skriptaktion in Ihrem Cluster installieren und es über das Jupyter-Notebook verwenden.

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes:

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Wenn Sie noch nicht über einen Spark-Cluster unter HDInsight (Linux) verfügen, können Sie während der Clustererstellung Skriptaktionen ausführen. Informationen zum Verwenden benutzerdefinierter Skriptaktionen finden Sie in der [Dokumentation](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a>Verwenden von externen Paketen mit Jupyter Notebooks

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.   

2. Klicken Sie auf dem Blatt des Spark-Clusters im linken Bereich auf **Skriptaktionen**. Führen Sie die benutzerdefinierte Aktion aus, die TensorFlow auf den Haupt- und Workerknoten installiert. Das Bash-Skript finden Sie unter „https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh“. Informationen zum Verwenden von benutzerdefinierten Skriptaktionen finden Sie in der [Dokumentation](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

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

    Das Ergebnis sieht so aus:
    
    ![Ausführung des TensorFlow-Codes](./media/apache-spark-python-package-installation/execution.png "Ausführung des TensorFlow-Codes")



## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](apache-spark-eventhub-streaming.md)
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
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)
