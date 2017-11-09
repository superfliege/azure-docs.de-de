---
title: "Microsoft Azure HDInsight: Einrichten einer interaktiven PySpark-Umgebung für Visual Studio Code | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure HDInsight Tools für Visual Studio Code verwenden können, um Abfragen und Skripts zu erstellen und zu übermitteln."
Keywords: VScode, Azure HDInsight Tools, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, Interactive Query
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: 
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 839f379228322eb2da0ff61609634bf1f86e4bb3
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="set-up-pyspark-interactive-environment-for-visual-studio-code"></a>Einrichten einer interaktiven PySpark-Umgebung für Visual Studio Code

Hier erfahren Sie Schritt für Schritt, wie Sie Python-Pakete bei Verwendung von **HDInsight: PySpark Interactive** installieren.


## <a name="set-up-pyspark-interactive-environment-on-macos-and-linux"></a>Einrichten einer interaktiven PySpark-Umgebung unter MacOS und Linux
Für die folgenden Schritte muss der Befehl **pip3** verwendet werden, wenn es sich um **Python 3.x** handelt.
1. Vergewissern Sie sich, dass **Python** und **pip** installiert sind.
 
    ![Python: pip-Version](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Installieren von Jupyter
    ```
    sudo pip install jupyter
    ```
    +  Unter Linux und MacOS wird möglicherweise die folgende Fehlermeldung angezeigt:

        ![Fehler 1](./media/set-up-pyspark-interactive-environment/error1.png)
        ```Resolve:
        sudo pip uninstall asyncio
        sudo pip install trollies
        ```

    + Installieren Sie „libkrb5-dev“ (nur Linux). Möglicherweise wird die folgende Fehlermeldung angezeigt:

        ![Fehler 2](./media/set-up-pyspark-interactive-environment/error2.png)
        ```Resolve:
        sudo apt-get install libkrb5-dev 
        ```

3. Installieren von sparkmagic
   ```
   sudo pip install sparkmagic
   ```

4. Führen Sie den folgenden Befehl aus, um sich zu vergewissern, dass „ipywidgets“ ordnungsgemäß installiert ist:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Installieren der Wrapper-Kernel](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Installieren Sie die Wrapper-Kernel. Führen Sie **pip show sparkmagic** aus, um den Installationspfad von sparkmagic anzuzeigen. 

    ![Speicherort von sparkmagic](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Navigieren Sie zu dem Speicherort, und führen Folgendes aus:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![jupyter kernelspec install](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Überprüfen Sie den Installationsstatus: 

    ```
    jupyter-kernelspec list
    ```
    ![jupyter kernelspec list](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Informationen zu verfügbaren Kernels: **python2** und **pysparkkernel** entsprechen **python 2.x**, **python3** und **pyspark3kernel** entsprechen **python 3.x**. 

8. Starten Sie Visual Studio Code neu, und führen Sie über den Skript-Editor **HDInsight: PySpark Interactive** aus.

## <a name="next-steps"></a>Nächste Schritte

### <a name="demo"></a>Demo
* HDInsight für VSCode: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden von Azure HDInsight-Tools für Visual Studio Code](hdinsight-for-vscode.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen per SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen über VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)
* [Visualize Hive data with Microsoft Power BI in Azure HDInsight (Visualisieren von Hive-Daten mit Microsoft Power BI in Azure HDInsight)](./hdinsight-connect-hive-power-bi.md).
* [Use Zeppelin to run Hive queries in Azure HDInsight (Verwenden von Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight)](./hdinsight-connect-hive-zeppelin.md).
