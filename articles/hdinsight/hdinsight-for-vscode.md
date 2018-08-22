---
title: 'Azure HDInsight Tools: Verwenden von Visual Studio Code für Hive, LLAP oder PySpark'
description: Hier erfahren Sie, wie Sie mithilfe von Azure HDInsight Tools for Visual Studio Code Abfragen und Skripts erstellen und übermitteln.
keywords: VS Code, Azure HDInsight Tools, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, Interactive Query
services: hdinsight
author: jejiang
ms.author: jejiang
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: 255f1ecb3c55ef94b6f4f3393257b3054ff1b725
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40038286"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Verwenden von Azure HDInsight Tools for Visual Studio Code

Hier erfahren Sie, wie Sie Azure HDInsight Tools for Visual Studio Code (VS Code) verwenden, um Hive-Batchaufträge, interaktive Hive-Abfragen und PySpark-Skripts zu erstellen und zu übermitteln. Azure HDInsight Tools können auf den von VS Code unterstützten Plattformen installiert werden. Hierzu zählen Windows, Linux und macOS. Im Folgenden finden Sie die Voraussetzungen für die unterschiedlichen Plattformen.


## <a name="prerequisites"></a>Voraussetzungen

Für die Schritte in diesem Artikel ist Folgendes erforderlich:

- Ein HDInsight-Cluster. Informationen zum Erstellen eines Clusters finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono wird nur für Linux und macOS benötigt.

## <a name="install-the-hdinsight-tools"></a>Installieren der HDInsight Tools
   
Nachdem Sie die erforderlichen Komponenten installiert haben, können Sie Azure HDInsight Tools for Visual Studio Code installieren. 

**Installieren von Azure HDInsight Tools**

1. Öffnen Sie Visual Studio Code.

2. Klicken Sie im linken Bereich auf **Erweiterungen**. Geben Sie **HDInsight** in das Suchfeld ein.

3. Klicken Sie neben **Azure HDInsight Tools** auf **Installieren**. Nach wenigen Sekunden wird die Schaltfläche **Installieren** in **Neu laden** geändert.

4. Klicken Sie auf **Erneut laden**, um die Erweiterung **Azure HDInsight Tools** zu aktivieren.

5. Klicken Sie zur Bestätigung auf **Fenster erneut laden**. **Azure HDInsight Tools** wird im Bereich **Erweiterungen** angezeigt.

   ![HDInsight für Visual Studio Code Python-Installation](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Öffnen des HDInsight-Arbeitsbereichs

Um eine Verbindung mit Azure herstellen zu können, müssen Sie zunächst einen Arbeitsbereich in Visual Studio Code erstellen.

**Öffnen eines Arbeitsbereichs**

1. Klicken Sie im Menü **Datei** auf **Ordner öffnen**. Geben Sie dann entweder einen vorhandenen Ordner als Arbeitsordner an, oder erstellen Sie einen neuen Ordner. Der Ordner wird im linken Bereich angezeigt.

2. Klicken Sie im linken Bereich neben dem Arbeitsordner auf das Symbol **Neue Datei**.

   ![Neue Datei](./media/hdinsight-for-vscode/new-file.png)

3. Benennen Sie die neue Datei mit der Dateierweiterung „.hql“ (Hive-Abfragen) oder „.py“ (Spark-Skript). Beachten Sie, dass dem Arbeitsordner automatisch eine Konfigurationsdatei namens **XXXX_hdi_settings.json** hinzugefügt wird.

4. Öffnen Sie **XXXX_hdi_settings.json** über den **EXPLORER**, oder klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie anschließend auf **Set Configuration** (Konfiguration festlegen). Sie können die Parameter „Anmeldeeintrag“, „Standardcluster“ und „Auftragsübermittlung“ konfigurieren (wie im Beispiel in der Datei gezeigt). Sie können die übrigen Parameter auch leer lassen.

## <a name="connect-to-hdinsight-cluster"></a>Herstellen einer Verbindung mit dem HDInsight-Cluster

Bevor Sie Skripts aus VS Code an HDInsight-Cluster übermitteln können, müssen Sie entweder eine Verbindung mit Ihrem Azure-Konto herstellen oder einen Cluster verknüpfen (unter Verwendung des Ambari-Benutzernamens/-Kennworts oder eines in die Domäne eingebunden Kontos).

**So stellen Sie eine Verbindung mit Azure her**

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Skriptdatei, falls Sie über keine verfügen.

2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie im Kontextmenü auf **HDInsight: Login** (HDInsight: Anmeldung). Sie können auch **STRG+UMSCHALT+P** drücken und **HDInsight: Login** (HDInsight: Anmeldung) eingeben.

    ![HDInsight Tools für Visual Studio Code-Anmeldung](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Befolgen Sie zur Anmeldung die Anmeldeanweisungen im Bereich **AUSGABE**.

    **Azure:** ![Anmeldeinformationen für HDInsight-Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Nach dem Herstellen der Verbindung wird Ihr Azure-Kontoname links unten im Visual Studio Code-Fenster auf der Statusleiste angezeigt. 

    > [!NOTE]
    > Aufgrund eines bekannten Azure-Authentifizierungsproblems müssen Sie einen Browser im privaten Modus oder im Inkognito-Modus öffnen. Wenn für Ihr Azure-Konto die zweistufige Authentifizierung aktiviert ist, sollten Sie anstelle der Authentifizierung per PIN die telefonische Authentifizierung verwenden.
  

4. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, um das Kontextmenü zu öffnen. Über das Kontextmenü können Sie folgende Aufgaben ausführen:

    - Abmelden
    - Auflisten von Clustern
    - Festlegen von Standardclustern
    - Übermitteln interaktiver Hive-Abfragen
    - Übermitteln von Hive-Batchskripts
    - Übermitteln interaktiver PySpark-Abfragen
    - Übermitteln von PySpark-Batchskripts
    - Festlegen von Konfigurationen

<a id="linkcluster"></a>**Verknüpfen eines Clusters**

Sie können einen normalen Cluster mithilfe eines verwalteten Ambari-Benutzernamens oder einen Hadoop-Sicherheitscluster mithilfe des Domänenbenutzernamens (z.B. user1@contoso.com) verknüpfen.
1. Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** auswählen, und geben Sie dann **HDInsight: Cluster verknüpfen** ein.

   ![Befehl „Cluster verknüpfen“](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Geben Sie die URL des HDInsight-Clusters ein, geben Sie den Benutzername und das Kennwort ein, und wählen Sie den Clustertyp aus. Es werden Erfolgsinformationen angezeigt, wenn die Überprüfung bestanden wurde.
   
   ![Dialogfeld „Cluster verknüpfen“](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]
   > Der verknüpfte Benutzername und das Kennwort werden verwendet, wenn der Cluster im Azure-Abonnement angemeldet ist und einen Cluster verknüpft hat. 
   
3. Sie können einen verknüpften Cluster mithilfe des Befehls **Cluster auflisten** anzeigen. Jetzt können Sie ein Skript an diesen verknüpften Cluster übermitteln.

   ![Verknüpfter Cluster](./media/hdinsight-for-vscode/linked-cluster.png)

4. Sie können die Verknüpfung eines Clusters auch aufheben, indem Sie **HDInsight: Verknüpfung eines Clusters aufheben** über die Befehlspalette eingeben.

## <a name="list-hdinsight-clusters"></a>Auflisten von HDInsight-Clustern

Um die Verbindung zu testen, können Sie Ihre HDInsight-Cluster auflisten:

**Auflisten von HDInsight-Clustern unter Ihrem Azure-Abonnement**
1. Öffnen Sie einen Arbeitsbereich, und stellen Sie eine Verbindung mit Azure her. Weitere Informationen finden Sie unter [Öffnen des HDInsight-Arbeitsbereichs](#open-hdinsight-workspace) sowie unter [Herstellen einer Verbindung mit Azure](#connect-to-azure).

2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie dann im Kontextmenü **HDInsight: List Cluster** (HDInsight: Cluster auflisten) aus. 

3. Die Hive- und Spark-Cluster werden im Bereich **Ausgabe** angezeigt.

    ![Festlegen einer Standardclusterkonfiguration](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Festlegen eines Standardclusters
1. Öffnen Sie einen Arbeitsbereich, und stellen Sie eine Verbindung mit Azure her. Lesen Sie den Abschnitt [Öffnen des HDInsight-Arbeitsbereichs](#open-hdinsight-workspace), und [stellen Sie eine Verbindung mit Azure her](#connect-to-azure).

2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie anschließend auf **HDInsight: Set Default Cluster** (HDInsight: Standardcluster festlegen). 

3. Wählen Sie einen Cluster als Standardcluster für die aktuelle Skriptdatei aus. Die Konfigurationsdatei **XXXX_hdi_settings.json** wird automatisch aktualisiert. 

   ![Festlegen der Standardclusterkonfiguration](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Einrichten der Azure-Umgebung 
1. Drücken Sie **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen.

2. Geben Sie **HDInsight: Azure-Umgebung festlegen** ein.

3. Wählen Sie eine Möglichkeit von Azure und Azure China als Ihren Standardanmeldeeintrag aus.

4. Inzwischen hat das Tool Ihren Standardanmeldeeintrag bereits in **XXXX_hdi_settings.json** gespeichert. Sie aktualisieren ihn auch direkt in dieser Konfigurationsdatei. 

   ![Festlegen der Konfiguration des Standardanmeldeeintrags](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Übermitteln interaktiver Hive-Abfragen

Mit HDInsight Tools for Visual Studio Code können Sie interaktive Hive-Abfragen an HDInsight Interactive Query-Cluster übermitteln.

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Hive-Skriptdatei, falls Sie über keine verfügen.

2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und konfigurieren Sie anschließend den Standardcluster, sofern Sie diese Schritte noch nicht ausgeführt haben.

3. Kopieren Sie den folgenden Code, fügen Sie ihn in Ihre Hive-Datei ein, und speichern Sie sie.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie anschließend auf **HDInsight: Hive Interactive** (HDInsight: Hive (interaktiv)), um die Abfrage zu übermitteln. Über das Kontextmenü können Sie anstelle der gesamten Skriptdatei auch einen Codeblock übermitteln. Das Abfrageergebnis wird kurz darauf auf einer neuen Registerkarte angezeigt.

   ![Interactive Hive-Ergebnis](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Bereich **ERGEBNISSE**: Sie können das gesamte Ergebnis als CSV-, JSON- oder Excel-Datei an einem lokalen Pfad speichern oder einfach mehrere Zeilen auswählen.

    - Bereich **MELDUNGEN**: Wenn Sie auf die **Zeilennummer** klicken, gelangen Sie zur ersten Zeile des derzeit ausgeführten Skripts.

Verglichen mit dem [Ausführen eines Hive-Batchauftrags](#submit-hive-batch-scripts) nimmt die interaktive Abfrage deutlich weniger Zeit in Anspruch.

## <a name="submit-hive-batch-scripts"></a>Übermitteln von Hive-Batchskripts

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Hive-Skriptdatei, falls Sie über keine verfügen.

2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und konfigurieren Sie anschließend den Standardcluster, sofern Sie diese Schritte noch nicht ausgeführt haben.

3. Kopieren Sie den folgenden Code, fügen Sie ihn in Ihre Hive-Datei ein, und speichern Sie sie.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie anschließend auf **HDInsight: Hive Batch**, um einen Hive-Auftrag zu übermitteln. 

4. Wählen Sie den gewünschten Zielcluster für die Übermittlung aus.  

    Nach der Übermittlung eines Hive-Auftrags werden die Informationen zur erfolgreichen Übermittlung und die Auftrags-ID im Bereich **AUSGABE** angezeigt. Der Hive-Auftrag öffnet auch den **Webbrowser**, in dem die Auftragsprotokolle und der Status in Echtzeit angezeigt werden.

   ![Übermitteln des Hive-Auftragsergebnisses](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

Das [Übermitteln interaktiver Hive-Abfragen](#submit-interactive-hive-queries) nimmt deutlich weniger Zeit in Anspruch als das Übermitteln eines Batchauftrags.

## <a name="submit-interactive-pyspark-queries"></a>Übermitteln interaktiver PySpark-Abfragen
Mit HDInsight Tools for Visual Studio Code können Sie auch interaktive PySpark-Abfragen an Spark-Cluster übermitteln.
1. Erstellen Sie einen neuen Arbeitsordner und eine neue Skriptdatei mit der Erweiterung „.py“, falls Sie über keine verfügen.

2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, sofern noch keine Verbindung besteht.

3. Kopieren Sie den folgenden Code, und fügen Sie ihn in die Skriptdatei ein:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```
4. Markieren Sie die Skripts. Klicken Sie dann mit der rechten Maustaste auf den Skript-Editor, und klicken Sie anschließend auf **HDInsight: PySpark Interactive** (HDInsight: PySpark (interaktiv)).

5. Sofern Sie in Visual Studio Code die Erweiterung **Python** noch nicht installiert haben, klicken Sie auf die Schaltfläche **Installieren**, wie in der folgenden Abbildung zu sehen:

    ![HDInsight für Visual Studio Code Python-Installation](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Installieren Sie die Python-Umgebung in Ihrem System, falls sie noch nicht installiert ist. 
   - Laden Sie für Windows [Python](https://www.python.org/downloads/) herunter, und installieren Sie es. Vergewissern Sie sich anschließend, dass sich `Python` und `pip` unter Ihrem Systempfad befinden.

   - Eine Anleitung für macOS und Linux finden Sie unter [Einrichten der PySpark Interactive-Umgebung für Visual Studio Code](set-up-pyspark-interactive-environment.md).

7. Wählen Sie einen Cluster aus, an den Sie Ihre PySpark-Abfrage übermitteln möchten. Das Abfrageergebnis wird kurz darauf rechts auf der neuen Registerkarte angezeigt:

   ![Übermitteln des Ergebnisses des Python-Auftrags](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Das Tool unterstützt auch die Abfrage **SQL-Klausel**.

   ![Übermitteln des Ergebnisses des Python-Auftrags](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) Der Übertragungsstatus wird beim Ausführen von Abfragen links neben der unteren Statusleiste angezeigt. Übermitteln Sie keine weiteren Abfragen, wenn der Status **PySpark Kernel (busy)** (PySpark-Kernel (ausgelastet)) lautet. 

>[!NOTE]
>Die Cluster können Sitzungsinformationen speichern. Die definierte Variable, die Funktion und die entsprechenden Werte werden in der Sitzung gespeichert, sodass auf sie in mehreren Dienstaufrufen für den gleichen Cluster verwiesen werden kann. 
 

## <a name="submit-pyspark-batch-job"></a>Übermitteln eines PySpark-Batchauftrags

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Skriptdatei mit der Erweiterung „.py“, falls Sie über keine verfügen.

2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, sofern Sie diesen Schritt noch nicht ausgeführt haben.

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
4. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie anschließend auf **HDInsight: PySpark Batch** (HDInsight: PySpark (Batch)). 

5. Wählen Sie einen Cluster aus, an den Sie Ihren PySpark-Auftrag übermitteln möchten. 

   ![Übermitteln des Ergebnisses des Python-Auftrags](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Nach dem Übermitteln eines Python-Auftrags werden die Übermittlungsprotokolle in Visual Studio Code im Fenster **AUSGABE** angezeigt. Die **URL der Spark-Benutzeroberfläche** und die **URL der Yarn-Benutzeroberfläche** werden ebenfalls angezeigt. Sie können die URL in einem Webbrowser öffnen, um den Auftragsstatus nachzuverfolgen.

>[!NOTE]
>PySpark3 wird nicht mehr in Livy 0.4 unterstützt (HDI Spark 2.2-Cluster). Für Python wird nur „PySpark“ unterstützt. Es ist ein bekanntes Problem, dass ein Fehler beim Senden an Spark 2.2 mit Python3 auftritt.
   
## <a name="livy-configuration"></a>Livy-Konfiguration
Livy-Konfiguration wird unterstützt. Entsprechende Einstellungen können Sie in den Projekteinstellungen im Arbeitsbereichsordner vornehmen. Weitere Details finden Sie in der [Livy-INFODATEI](https://github.com/cloudera/livy/blob/master/README.rst ).

+ Die Projekteinstellungen:

    ![Livy-Konfiguration](./media/hdinsight-for-vscode/hdi-livyconfig.png)

+ Die unterstützten Livy-Konfigurationen:   

    **POST /batches**   
    Anforderungstext

    | name | Beschreibung | type | 
    | :- | :- | :- | 
    | file | Die Datei, die die auszuführende Anwendung enthält. | Pfad (erforderlich) | 
    | proxyUser | Der Benutzer, dessen Identität bei Auftragsausführung gewechselt wird. | Zeichenfolge | 
    | className | Die Java-/Spark-Hauptklasse der Anwendung. | Zeichenfolge |
    | args | Die Befehlszeilenargumente, die an die Anwendung übergeben werden sollen. | Liste von Zeichenfolgen | 
    | jars | JAR-Dateien, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen | 
    | pyFiles | Python-Dateien, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen |
    | files | Dateien, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen |
    | driverMemory | Die Menge an Arbeitsspeicher, der für den Treiberprozess verwendet wird. | Zeichenfolge |
    | driverCores | Die Anzahl von Kernen, die für den Treiberprozess verwendet wird. | int |
    | executorMemory | Die Menge an Arbeitsspeicher, die pro Executorprozess verwendet wird. | Zeichenfolge |
    | executorCores | Die Anzahl von Kernen, die für jeden Executor verwendet wird. | int |
    | numExecutors | Die Anzahl von Executors, die für diese Sitzung gestartet werden. | int |
    | archives | Die Archive, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen |
    | queue | Der Name der YARN-Warteschlange, an den gesendet wird. | Zeichenfolge |
    | name | Der Name dieser Sitzung. | Zeichenfolge |
    | conf | Eigenschaften der Spark-Konfiguration. | Zuordnung von Schlüssel=Wert |

    Antworttext   
    Das erstellte Batchobjekt

    | name | Beschreibung | type | 
    | :- | :- | :- | 
    | id | Die Sitzungs-ID | int | 
    | appId | Die Anwendungs-ID dieser Sitzung |  Zeichenfolge |
    | appInfo | Die detaillierten Anwendungsinformationen | Zuordnung von Schlüssel=Wert |
    | log | Die Protokollzeilen | Liste von Zeichenfolgen |
    | state |   Der Batchstatus | Zeichenfolge |


## <a name="additional-features"></a>Zusätzliche Funktionen

HDInsight Tools for Visual Studio Code unterstützen folgende Features:

- **IntelliSense-AutoVervollständigen**. Zeigt Vorschläge für Schlüsselwörter, Methoden, Variablen und Ähnliches an. Die unterschiedlichen Objekttypen werden durch entsprechende Symbole dargestellt.

    ![IntelliSense-Objekttypen der HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense-Fehlermarker**. Der Sprachdienst unterstreicht die Bearbeitungsfehler für das Hive-Skript.     
- **Syntaxmarkierungen**. Der Sprachdienst verwendet verschiedene Farben, um Variablen, Schlüsselwörter, Datentypen, Funktionen und Ähnliches zu unterscheiden. 

    ![Syntaxmarkierungen der HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Nächste Schritte

### <a name="demo"></a>Demo
* HDInsight für VS Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen über VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Spark-Anwendungen per SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](spark/apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualisieren von Hive-Daten mit Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Visualisieren von Interactive Query-Hive-Daten mit Power BI in Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
* [Einrichten der PySpark Interactive-Umgebung für Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Verwenden von Zeppelin zum Ausführen von Hive-Abfragen in Azure HDInsight](./hdinsight-connect-hive-zeppelin.md).

### <a name="scenarios"></a>Szenarien
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](spark/apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](spark/apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](spark/apache-spark-job-debugging.md)



