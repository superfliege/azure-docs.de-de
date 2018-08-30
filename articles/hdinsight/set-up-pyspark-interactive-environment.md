---
title: 'Microsoft Azure HDInsight: Einrichten einer interaktiven PySpark-Umgebung für Visual Studio Code'
description: Hier erfahren Sie, wie Sie mithilfe von Azure HDInsight Tools for Visual Studio Code Abfragen und Skripts erstellen und übermitteln.
keywords: VScode, Azure HDInsight Tools, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, Interactive Query
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: e57fd3747ef9cbd55d073e02bc14816ca949bcd6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43046234"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Einrichten einer interaktiven PySpark-Umgebung für Visual Studio Code

Hier erfahren Sie Schritt für Schritt, wie Sie Python-Pakete mithilfe von **HDInsight: PySpark Interactive** installieren.


## <a name="set-up-the-pyspark-interactive-environment-on-macos-and-linux"></a>Einrichten einer interaktiven PySpark-Umgebung unter macOS und Linux
Wenn Sie **Python 3.x** verwenden, müssen Sie den Befehl **pip3** für die folgenden Schritte ausführen:

1. Stellen Sie sicher, dass **Python** und **pip** installiert sind.
 
    ![Python: pip-Version](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Installieren Sie Jupyter.
    ```
    sudo pip install jupyter
    ```
   Unter Linux und macOS wird möglicherweise die folgende Fehlermeldung angezeigt:

   ![Fehler 1](./media/set-up-pyspark-interactive-environment/error1.png)

   ```Resolve:
    sudo pip uninstall asyncio
    sudo pip install trollies
    ```

3. Installieren Sie **libkrb5-dev** (nur für Linux). Unter Umständen wird die folgende Fehlermeldung angezeigt:

   ![Fehler 2](./media/set-up-pyspark-interactive-environment/error2.png)
       
   ```Resolve:
   sudo apt-get install libkrb5-dev 
   ```

3. Installieren Sie **sparkmagic**.
   ```
   sudo pip install sparkmagic
   ```

4. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass **ipywidgets** ordnungsgemäß installiert ist:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Installieren der Wrapper-Kernel](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Installieren Sie die Wrapper-Kernel. Führen Sie **pip show sparkmagic** aus. Die Ausgabe zeigt den Pfad für die Installation von **sparkmagic** an. 

    ![Speicherort von sparkmagic](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Navigieren Sie zum Speicherort, und führen Sie Folgendes aus:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![jupyter kernelspec install](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Überprüfen Sie den Installationsstatus.

    ```
    jupyter-kernelspec list
    ```
    ![jupyter kernelspec list](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Für verfügbare Kernels: 
    - **python2** und **pysparkkernel** entsprechen **Python 2.x**. 
    - **python3** und **pyspark3kernel** entsprechen **Python 3.x**. 

8. Starten Sie VS Code neu, und navigieren Sie zurück zum Skript-Editor, der **HDInsight: PySpark Interactive** ausführt.

## <a name="next-steps"></a>Nächste Schritte

### <a name="demo"></a>Demo
* HDInsight für VS Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden von Azure HDInsight-Tools für Visual Studio Code](hdinsight-for-vscode.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Erstellen und Übermitteln von Spark Scala-Anwendungen](spark/apache-spark-intellij-tool-plugin.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen per SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen über VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](spark/apache-spark-eclipse-tool-plugin.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualisieren von Hive-Daten mit Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Verwenden von Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight](hdinsight-connect-hive-zeppelin.md).
