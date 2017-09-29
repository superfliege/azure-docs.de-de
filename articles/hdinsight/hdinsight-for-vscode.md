---
title: "Azure HDInsight Tools: Verwenden von Visual Studio Code für Hive, LLAP oder pySpark | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure HDInsight Tools für Visual Studio Code verwenden können, um Abfragen und Skripts zu erstellen und zu übermitteln."
Keywords: VScode, Azure HDInsight Tools, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, Interactive Query
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: jgao
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/21/2017
ms.author: jejiang
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 17900c88fcfccd9c56ac548d1e9167b16a92b433
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="use-azure-hdinsight-tool-for-visual-studio-code-hive-llap-or-pyspark"></a>Verwenden von Azure HDInsight Tools für Visual Studio Code (Hive, LLAP oder pySpark)

Erfahren Sie, wie Sie Azure HDInsight Tools für Visual Studio Code (VSCode) verwenden können, um Hive-Batchaufträge, interaktive Hive-Abfragen und pySpark-Skripts zu erstellen und zu übermitteln. Azure HDInsight Tools kann auf den von VSCode unterstützten Plattformen installiert werden, z.B. Windows, Linux und MacOS. Im Folgenden finden Sie die Voraussetzungen für die unterschiedlichen Plattformen.


## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Elemente sind für das Durchführen der Vorgänge dieses Artikels erforderlich:

- Ein HDInsight-Cluster.  Informationen zum Erstellen eines Clusters finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono ist nur für Linux und MacOS erforderlich.

## <a name="install-the-hdinsight-tools"></a>Installieren der HDInsight Tools
   
Nachdem Sie die erforderlichen Komponenten installiert haben, können Sie Azure HDInsight Tools für VSCode installieren. 

**Installieren von Azure HDInsight Tools**

1. Öffnen Sie **Visual Studio Code**.
2. Klicken Sie im linken Bereich auf **Erweiterungen**. Geben Sie **Azure HDInsight Tools** in das Suchfeld ein.
3. Klicken Sie neben **Azure HDInsight Tools** auf **Installieren**. Nach wenigen Sekunden wird die Schaltfläche **Installieren** in **Erneut laden** geändert.
4. Klicken Sie auf **Erneut Laden**, um die Erweiterung **Azure HDInsight Tools** zu aktivieren.
5. Klicken Sie auf **Fenster erneut laden**, um zu bestätigen. **Azure HDInsight Tools** wird im Bereich „Erweiterungen“ angezeigt.

   ![HDInsight für Visual Studio Code Python-Installation](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

Für Spark-Benutzer wird die Python-Erweiterung für einen besseren Sprachdienst empfohlen. Die Python-Erweiterung ist für Hive-Benutzer nicht erforderlich. 

**Installieren von Python**

1. Öffnen Sie **Visual Studio Code**.
2. Klicken Sie im linken Bereich auf **Erweiterungen**. Geben Sie im Suchfeld **Python** ein. Ihnen wird eine Liste von Python-Erweiterungen angezeigt. Eine davon ist **Python**.
3. Klicken Sie neben **Python** auf **Installieren**. Nach wenigen Sekunden wird die Schaltfläche **Installieren** in **Erneut laden** geändert.
4. Klicken Sie auf **Neu laden**, um die **Python**-Erweiterung zu aktivieren.
5. Klicken Sie auf **Fenster erneut laden**, um zu bestätigen. Ihnen wird **Python** im Bereich „Erweiterungen“ angezeigt.

     ![HDInsight für Visual Studio Code Python-Installation](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

## <a name="open-hdinsight-workspace"></a>Öffnen des HDInsight-Arbeitsbereichs

Bevor Sie eine Verbindung mit Azure herstellen können, müssen Sie in VSCode einen Arbeitsbereich erstellen.

**Öffnen eines Arbeitsbereichs**

1. Klicken Sie im Menü **Datei** auf **Ordner öffnen**, geben Sie einen vorhandenen Ordner an, oder erstellen Sie einen neuen Ordner als Ihren Arbeitsordner. Der Ordner wird im linken Bereich angezeigt.
2. Klicken Sie im linken Bereich auf das Symbol **Neue Datei** neben dem Arbeitsordner.

   ![Neue Datei](./media/hdinsight-for-vscode/new-file.png)
3. Benennen Sie die neue Datei mit der Dateierweiterung „.hql“ (Hive-Abfragen) oder „.py“ (Spark-Skript). Beachten Sie, dass eine Konfigurationsdatei **XXXX_hdi_settings.json** automatisch zum Arbeitsordner hinzugefügt wird.
4. Öffnen Sie **XXXX_hdi_settings.json** über den **Explorer**, oder klicken Sie mit der rechten Maustaste auf den Skript-Editor, um **Set Configuration** (Konfiguration festlegen) auszuwählen. Sie können wie im Beispiel in der Datei gezeigt die Parameter „Anmeldeeintrag“, „Standardcluster“ und „Auftragsübermittlung“ konfigurieren. Sie können die übrigen Parameter auch leer lassen.

## <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Bevor Sie Skripts von VSCode an HDInsight-Cluster übermitteln können, müssen Sie eine Verbindung mit Ihrem Azure-Konto herstellen.

**So stellen Sie eine Verbindung mit Azure her**

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Skriptdatei, wenn Sie über keine verfügen.
2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie dann im Kontextmenü **HDInsight: Login** (HDInsight: Anmeldung) aus. Sie können auch **STRG + UMSCHALT + P** drücken, um zu **HDInsight: Login** (HDInsight: Anmeldung) zu gelangen.

    ![HDInsight Tools für Visual Studio Code-Anmeldung](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)
3. Klicken Ihrem Abonnement entsprechend auf **Azure** oder **Azure China**.

    ![HDInsight Tools für Visual Studio Code-Anmeldeoptionen](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login-options.png)

3. Befolgen Sie zur Anmeldung die Anmeldeanweisungen im Bereich **AUSGABE**.

    **Azure:** ![Anmeldeinformationen zu HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png) **Azure China:** ![Anmeldeinformationen zu HDInsight Tools für Visual Studio Code China](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-AzureChinalogin-info.png)

    Nachdem die Verbindung hergestellt wurde, wird Ihr Azure-Kontoname auf der Statusleiste links unten im VSCode-Fenster angezeigt. 

    > [!NOTE] 
    > Wenn für Ihr Azure-Konto die zweistufige Authentifizierung aktiviert ist, sollten Sie die Authentifizierung per Telefon statt per PIN verwenden.
    > Es besteht ein bekanntes Problem bei der Anmeldung bei Azure über Google Chrome.

4. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, um das Kontextmenü zu öffnen:

    ![Kontextmenü des Skript-Editors der HDInsight-Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-context-menu.png)

    Über das Kontextmenü können Sie folgende Aufgaben ausführen:

    - logout
    - Auflisten von Clustern
    - Festlegen des Standardclusters
    - Übermitteln interaktiver Hive-Abfragen
    - Übermitteln des Hive-Batchskripts
    - Festlegen der Konfiguration

## <a name="list-hdinsight-clusters"></a>Auflisten von HDInsight-Clustern

Um die Verbindung zu testen, können Sie Ihre HDInsight-Cluster auflisten:

**Auflisten von HDInsight-Clustern unter Ihrem Azure-Abonnement**
1. Öffnen Sie einen Arbeitsbereich, und stellen Sie eine Verbindung mit Azure her. Lesen Sie den Abschnitt [Öffnen des HDInsight-Arbeitsbereichs](#open-hdinsight-workspace), und [stellen Sie eine Verbindung mit Azure her](#connect-to-azure).
2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie dann im Kontextmenü **HDInsight: List Cluster** (HDInsight: Cluster auflisten) aus. 
3. Die Hive- und Spark-Cluster werden im Bereich **Ausgabe** angezeigt.

    ![Festlegen der Standardclusterkonfiguration](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Festlegen des Standardclusters
1. Öffnen Sie einen Arbeitsbereich, und stellen Sie eine Verbindung mit Azure her. Lesen Sie den Abschnitt [Öffnen des HDInsight-Arbeitsbereichs](#open-hdinsight-workspace), und [stellen Sie eine Verbindung mit Azure her](#connect-to-azure).
2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie dann auf **HDInsight: Set Default Cluster** (HDInsight: Standardcluster festlegen). 
3. Wählen Sie einen Cluster als Standardcluster für die aktuelle Skriptdatei aus. Die Tools aktualisieren die Konfigurationsdatei **XXXX_hdi_settings.json** automatisch. 

   ![Festlegen der Standardclusterkonfiguration](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-azure-environment"></a>Festlegen der Azure-Umgebung 
1. Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** drücken.
2. Geben Sie **HDInsight: Azure-Umgebung festlegen** ein.
3. Wählen Sie eine Möglichkeit von Azure und Azure China als Ihren Standardanmeldeeintrag aus.
4. In der Zwischenzeit speichert unser Tool bereits Ihren ausgewählten Standardanmeldeeintrag unter **XXXX_hdi_settings.json**. Sie aktualisieren ihn auch direkt in dieser Konfigurationsdatei. 

   ![Festlegen der Konfiguration des Standardanmeldeeintrags](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Übermitteln interaktiver Hive-Abfragen

Mit HDInsight Tools für VSCode können Sie interaktive Hive-Abfragen an Interactive Query HDInsight-Cluster übermitteln.

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Hive-Skriptdatei, wenn Sie über keine verfügen.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und konfigurieren Sie anschließend den Standardcluster, falls nicht bereits geschehen.
3. Kopieren Sie den folgenden Code, fügen Sie ihn in die Datei ein, und speichern Sie diese.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie dann auf **HDInsight: Interactive Hive**, um die Abfrage zu übermitteln. Die Tools ermöglichen Ihnen auch die Übermittlung eines Codeblocks anstatt der gesamten Skriptdatei mithilfe des Kontextmenüs. Leicht verzögert wird das Abfrageergebnis auf einer neuen Registerkarte angezeigt:

   ![Interactive Hive-Ergebnis](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Bereich **ERGEBNISSE**: Sie können das gesamte Ergebnis als CSV-, JSON- oder EXCEL-Datei an einem lokalen Pfad speichern, oder einfach mehrere Zeilen auswählen.
    - Bereich **MELDUNGEN**: Wenn Sie auf die **Zeilennummer** klicken, gelangen Sie zur ersten Zeile des derzeit ausgeführten Skripts.

Verglichen mit dem [Ausführen eines Hive-Batchauftrags](#submit-hive-batch-scripts) nimmt die interaktive Abfrage viel weniger Zeit in Anspruch.

## <a name="submit-hive-batch-scripts"></a>Übermitteln von Hive-Batchskripts

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Hive-Skriptdatei, wenn Sie über keine verfügen.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und konfigurieren Sie anschließend den Standardcluster, falls nicht bereits geschehen.
3. Kopieren Sie den folgenden Code, fügen Sie ihn in die Datei ein, und speichern Sie diese.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie dann auf **HDInsight: Submit Hive Batch Script** (HDInsight: Hive-Batchskript übermitteln), um einen Hive-Auftrag zu übermitteln. 
4. Wählen Sie einen Cluster aus, an den dieser übermittelt werden soll.  

    Nach der Übermittlung eines Hive-Auftrags werden die Informationen zur erfolgreichen Übermittlung und die Auftrags-ID im Bereich **AUSGABE** angezeigt. Zudem wird ein **Webbrowser** geöffnet, in dem die Auftragsprotokolle und der Auftragsstatus in Echtzeit angezeigt werden.

   ![Übermitteln des Hive-Auftragsergebnisses](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

Verglichen mit dem [Übermitteln interaktiver Hive-Abfragen](#submit-interactive-hive-queries) nimmt der Batchauftrag viel mehr Zeit in Anspruch.


## <a name="submit-pyspark-job"></a>Übermitteln eines PySpark-Auftrags

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Skriptdatei mit der Erweiterung „.py“, wenn Sie über keine verfügen.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und konfigurieren Sie anschließend den Standardcluster, falls nicht bereits geschehen.
3. Kopieren Sie den folgenden Code, und fügen Sie ihn in die Skriptdatei ein:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```
4. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie dann auf **HDInsight: Submit PySpark Job** (HDInsight: PySpark-Auftrag übermitteln). 
5. Wählen Sie einen Cluster aus, um Ihren PySpark-Auftrag zu übermitteln. 

   ![Übermitteln des Ergebnisses des Python-Auftrags](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Nach der Übermittlung eines Python-Auftrags werden im Fenster **AUSGABE** in VSCode Übermittlungsprotokolle angezeigt. Die **URL der Spark-Benutzeroberfläche** und die **URL der Yarn-Benutzeroberfläche** werden ebenfalls angezeigt. Sie können die URL in einem Webbrowser öffnen, um den Auftragsstatus nachzuverfolgen.


## <a name="additional-features"></a>Zusätzliche Funktionen

Die HDInsight Tools für VSCode unterstützen die folgenden Funktionen:

- **IntelliSense-AutoVervollständigen**. Vorschläge werden für Schlüsselwörter, Methoden, Variablen usw. eingeblendet. Unterschiedliche Symbole stellen die verschiedene Objekttypen dar:

    ![IntelliSense-Objekttypen der HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense-Fehlermarker**. Der Sprachdienst unterstreicht die Bearbeitungsfehler für das Hive-Skript.     
- **Syntaxmarkierungen**. Der Sprachdienst verwendet verschiedene Farben, um Variablen, Schlüsselwörter, Datentypen, Funktionen usw. zu unterscheiden. 

    ![Syntaxmarkierungen der HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Nächste Schritte

### <a name="demo"></a>Demo
* HDInsight für VSCode: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen über VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen per SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)
* [Visualize Hive data with Microsoft Power BI in Azure HDInsight (Visualisieren von Hive-Daten mit Microsoft Power BI in Azure HDInsight)](./hdinsight-connect-hive-power-bi.md).
* [Use Zeppelin to run Hive queries in Azure HDInsight (Verwenden von Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight)](./hdinsight-connect-hive-zeppelin.md).

### <a name="scenarios"></a>Szenarien
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Apache Spark-Streaming: Verarbeiten von Daten aus Azure Event Hubs mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="managing-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)




