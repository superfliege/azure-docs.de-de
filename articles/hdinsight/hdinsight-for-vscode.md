---
title: 'Azure HDInsight Tools: Verwenden von Visual Studio Code für Hive, LLAP oder PySpark | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mithilfe von Azure HDInsight Tools for Visual Studio Code Abfragen und Skripts erstellen und übermitteln.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: c86fd2affa08999ae574f55315c6a0b32fa58fca
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076789"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Verwenden von Azure HDInsight Tools for Visual Studio Code

Erfahren Sie, wie Sie Azure HDInsight Tools for Visual Studio Code (VS Code) verwenden, um Apache Hive-Batchaufträge, interaktive Hive-Abfragen und PySpark-Skripts für Apache Spark zu erstellen und zu übermitteln. Zunächst wird die Installation von HDInsight Tools in Visual Studio Code beschrieben, und dann werden die Schritte zum Übermitteln von Aufträgen an Hive und Spark erläutert. 

Azure HDInsight Tools kann auf den von VS Code unterstützten Plattformen installiert werden, darunter Windows, Linux und macOS. Nachstehend finden Sie die Voraussetzungen für die unterschiedlichen Plattformen.


## <a name="prerequisites"></a>Voraussetzungen

Für die Schritte in diesem Artikel ist Folgendes erforderlich:

- Ein HDInsight-Cluster. Informationen zum Erstellen eines Clusters finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono wird nur für Linux und macOS benötigt.

## <a name="install-the-hdinsight-tools"></a>Installieren der HDInsight Tools
   
Nachdem Sie die erforderlichen Komponenten installiert haben, können Sie Azure HDInsight Tools for Visual Studio Code installieren. 

### <a name="to-install-azure-hdinsight-tools"></a>So installieren Sie Azure HDInsight Tools

1. Öffnen Sie Visual Studio Code.

2. Klicken Sie im linken Bereich auf **Erweiterungen**. Geben Sie **HDInsight** in das Suchfeld ein.

3. Wählen Sie neben **Azure HDInsight Tools** die Option **Installieren** aus. Nach wenigen Sekunden wird die Schaltfläche **Installieren** in **Neu laden** geändert.

4. Wählen Sie **Erneut laden** aus, um die Erweiterung **Azure HDInsight Tools** zu aktivieren.

5. Klicken Sie zur Bestätigung auf **Fenster erneut laden**. **Azure HDInsight Tools** wird im Bereich **Erweiterungen** angezeigt.

   ![HDInsight für Visual Studio Code Python-Installation](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Öffnen des HDInsight-Arbeitsbereichs

Um eine Verbindung mit Azure herstellen zu können, müssen Sie zunächst einen Arbeitsbereich in Visual Studio Code erstellen.

### <a name="to-open-a-workspace"></a>So öffnen Sie einen Arbeitsbereich

1. Klicken Sie im Menü **Datei** auf **Ordner öffnen**. Geben Sie dann entweder einen vorhandenen Ordner als Arbeitsordner an, oder erstellen Sie einen neuen Ordner. Der Ordner wird im linken Bereich angezeigt.

2. Klicken Sie im linken Bereich neben dem Arbeitsordner auf das Symbol **Neue Datei**.

   ![Neue Datei](./media/hdinsight-for-vscode/new-file.png)

3. Benennen Sie die neue Datei mit der Dateierweiterung „.hql“ (Hive-Abfragen) oder „.py“ (Spark-Skript). 

## <a name="connect-to-hdinsight-cluster"></a>Herstellen einer Verbindung mit dem HDInsight-Cluster

Bevor Sie Skripts aus VS Code an HDInsight-Cluster übermitteln können, müssen Sie entweder eine Verbindung mit Ihrem Azure-Konto herstellen oder einen Cluster verknüpfen (unter Verwendung des Ambari-Benutzernamens/-Kennworts oder eines in die Domäne eingebunden Kontos).

### <a name="to-connect-to-azure"></a>So stellen Sie eine Verbindung mit Azure her

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Skriptdatei, falls Sie über keine verfügen.

2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und klicken Sie im Kontextmenü auf **HDInsight: Login** (HDInsight: Anmeldung). Sie können auch **STRG+UMSCHALT+P** drücken und dann **HDInsight: Login** (HDInsight: Anmeldung) eingeben.

    ![HDInsight Tools for Visual Studio Code-Anmeldung](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Befolgen Sie zur Anmeldung die Anmeldeanweisungen im Bereich **AUSGABE**.
    + In einer globalen Umgebung wird durch die HDInsight-Anmeldung der Azure-Anmeldevorgang ausgelöst.

        ![Anmeldeanweisungen für Azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Befolgen Sie für andere Umgebungen die Anmeldeanweisungen.

        ![Anmeldeanweisungen für andere Umgebung](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

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
    - Festlegen der Konfiguration

<h3 id="linkcluster">So verknüpfen Sie einen Cluster</h3>

Sie können einen normalen Cluster verknüpfen, indem Sie einen per [Apache Ambari](https://ambari.apache.org/) verwalteten Benutzernamen verwenden, oder einen per Enterprise Security Pack geschützten Hadoop-Cluster, indem Sie einen Domänenbenutzernamen (z.B. user1@contoso.com) verwenden.
1. Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** auswählen, und geben Sie dann **HDInsight: Link a Cluster** (HDInsight: Cluster verknüpfen) ein.

   ![Befehl „Cluster verknüpfen“](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Geben Sie die URL des HDInsight-Clusters ein, geben Sie den Benutzername und das Kennwort ein, und wählen Sie den Clustertyp aus. Es werden Erfolgsinformationen angezeigt, wenn die Überprüfung bestanden wurde.
   
   ![Dialogfeld „Cluster verknüpfen“](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]  
   > Der verknüpfte Benutzername und das Kennwort werden verwendet, wenn der Cluster im Azure-Abonnement angemeldet ist und einen Cluster verknüpft hat. 
   
3. Sie können einen verknüpften Cluster mithilfe des Befehls **List Cluster** (Cluster auflisten) anzeigen. Jetzt können Sie ein Skript an diesen verknüpften Cluster übermitteln.

   ![Verknüpfter Cluster](./media/hdinsight-for-vscode/linked-cluster.png)

4. Sie können die Verknüpfung eines Clusters auch aufheben, indem Sie in der Befehlspalette **HDInsight: Unlink a Cluster** (HDInsight: Verknüpfung eines Clusters aufheben) eingeben.


### <a name="to-link-a-generic-apache-livy-endpoint"></a>So verknüpfen Sie einen generischen Apache Livy-Endpunkt

1. Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** auswählen, und geben Sie dann **HDInsight: Link a Cluster** (HDInsight: Cluster verknüpfen) ein.
2. Wählen Sie **Generic Livy Endpoint** (Generischer Livy-Endpunkt) aus.
3. Geben Sie den generischen Livy-Endpunkt ein, z.B. http\://10.172.41.42:18080.
4. Wählen Sie **Basic** (Standard) aus, wenn eine Autorisierung für den generischen Livy-Endpunkt erforderlich ist. Wählen Sie andernfalls **None** (Kein) aus.
5. Geben Sie den Benutzernamen ein, wenn **Basic** (Standard) in Schritt 4 ausgewählt wurde.
6. Geben Sie das Kennwort ein, wenn **Basic** (Standard) in Schritt 4 ausgewählt wurde.
7. Der generische Livy-Endpunkt wurde erfolgreich verknüpft.

   ![Verknüpfter generischer Livy-Cluster](./media/hdinsight-for-vscode/link-cluster-process-generic-livy.png)

## <a name="list-hdinsight-clusters"></a>Auflisten von HDInsight-Clustern

Um die Verbindung zu testen, können Sie Ihre HDInsight-Cluster auflisten:

### <a name="to-list-hdinsight-clusters-under-your-azure-subscription"></a>So listen Sie HDInsight-Cluster unter Ihrem Azure-Abonnement auf
1. Öffnen Sie einen Arbeitsbereich, und stellen Sie eine Verbindung mit Azure her. Weitere Informationen finden Sie unter [Öffnen des HDInsight-Arbeitsbereichs](#open-hdinsight-workspace) sowie unter [Herstellen einer Verbindung mit Azure](#connect-to-hdinsight-cluster).

2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie anschließend **HDInsight: List Cluster** (HDInsight: Cluster auflisten) im Kontextmenü aus. 

3. Die HDInsight-Cluster werden im Bereich **Ausgabe** angezeigt.

    ![Festlegen einer Standardclusterkonfiguration](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Festlegen eines Standardclusters
1. Öffnen Sie einen Arbeitsbereich, und stellen Sie eine Verbindung mit Azure her. Lesen Sie den Abschnitt [Öffnen des HDInsight-Arbeitsbereichs](#open-hdinsight-workspace), und [stellen Sie eine Verbindung mit Azure her](#connect-to-hdinsight-cluster).

2. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie anschließend **HDInsight: Set Default Cluster** (HDInsight: Standardcluster festlegen) aus. 

3. Wählen Sie einen Cluster als Standardcluster für die aktuelle Skriptdatei aus. Die Konfigurationsdatei **VSCode\settings.json** wird automatisch aktualisiert. 

   ![Festlegen der Standardclusterkonfiguration](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Einrichten der Azure-Umgebung
1. Drücken Sie **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen.

2. Geben Sie **HDInsight: Set Azure Environment** (Azure-Umgebung festlegen) ein.

3. Wählen Sie eine Umgebung, z.B. „Azure“ oder „AzureChina“, als Ihren Standardanmeldeeintrag aus.

4. Inzwischen hat das Tool Ihren Standardanmeldeeintrag bereits unter **VSCode\settings.json** gespeichert. Sie aktualisieren ihn auch direkt in dieser Konfigurationsdatei. 

   ![Festlegen der Konfiguration des Standardanmeldeeintrags](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Übermitteln von interaktiven Hive-Abfragen und Hive-Batchskripts

Mit HDInsight Tools for Visual Studio Code können Sie interaktive Hive-Abfragen und Hive-Batchskripts an HDInsight-Cluster übermitteln.

1. Erstellen Sie einen neuen Arbeitsordner und eine neue Hive-Skriptdatei, falls Sie über keine verfügen.

2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto oder Ihren Linkclustern her.

3. Kopieren Sie den folgenden Code, fügen Sie ihn in Ihre Hive-Datei ein, und speichern Sie sie.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
4. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **HDInsight: Hive Interactive** aus, um die Abfrage zu übermitteln, oder verwenden Sie die Tastenkombination **STRG+ALT+I**. Wählen Sie **HDInsight Hive Batch** aus, um das Skript zu übermitteln, oder verwenden Sie die Tastenkombination **STRG+ALT+H**. 

5. Wählen Sie den Cluster aus, wenn Sie keinen Standardcluster angegeben haben. Über das Kontextmenü können Sie anstelle der gesamten Skriptdatei auch einen Codeblock übermitteln. Nach kurzer Zeit werden die Abfrageergebnisse auf einer neuen Registerkarte angezeigt.

   ![Interactive Hive-Ergebnis](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Bereich **ERGEBNISSE**: Sie können das gesamte Ergebnis als CSV-, JSON- oder Excel-Datei in einem lokalen Pfad speichern oder einfach mehrere Zeilen auswählen.

    - Bereich **MELDUNGEN**: Wenn Sie die **Zeilennummer** auswählen, gelangen Sie zur ersten Zeile des derzeit ausgeführten Skripts.

## <a name="submit-interactive-pyspark-queries"></a>Übermitteln interaktiver PySpark-Abfragen

### <a name="to-submit-interactive-pyspark-queries-to-hdinsight-spark-clusters"></a>Gehen Sie wie folgt vor, um interaktive PySpark-Abfragen an HDInsight Spark-Cluster zu übermitteln:

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
4. Markieren Sie dieses Skript. Klicken Sie dann mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **HDInsight: PySpark Interactive** aus, oder verwenden Sie die Tastenkombination **STRG+ALT+I**.

5. Sofern Sie in Visual Studio Code die Erweiterung **Python** noch nicht installiert haben, klicken Sie auf die Schaltfläche **Installieren**, wie in der folgenden Abbildung zu sehen:

    ![HDInsight für Visual Studio Code Python-Installation](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Installieren Sie die Python-Umgebung in Ihrem System, falls sie noch nicht installiert ist. 
   - Laden Sie für Windows [Python](https://www.python.org/downloads/) herunter, und installieren Sie es. Vergewissern Sie sich anschließend, dass sich `Python` und `pip` unter Ihrem Systempfad befinden.

   - Eine Anleitung für macOS und Linux finden Sie unter [Einrichten der PySpark Interactive-Umgebung für Visual Studio Code](set-up-pyspark-interactive-environment.md).

7. Wählen Sie einen Cluster aus, an den Sie Ihre PySpark-Abfrage übermitteln möchten. Das Abfrageergebnis wird kurz darauf rechts auf der neuen Registerkarte angezeigt:

   ![Übermitteln des Ergebnisses des Python-Auftrags](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Das Tool unterstützt auch die Abfrage der **SQL-Klausel**.

   ![Übermitteln des Ergebnisses des Python-Auftrags](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) Der Übertragungsstatus wird beim Ausführen von Abfragen links neben der unteren Statusleiste angezeigt. Übermitteln Sie keine weiteren Abfragen, wenn der Status **PySpark Kernel (busy)** (PySpark-Kernel (ausgelastet)) lautet. 

>[!NOTE]  
>Die Cluster können Sitzungsinformationen speichern. Die definierte Variable, die Funktion und die entsprechenden Werte werden in der Sitzung gespeichert, sodass auf sie in mehreren Dienstaufrufen für den gleichen Cluster verwiesen werden kann. 

### <a name="to-disable-environment-check"></a>So deaktivieren Sie die Umgebungsüberprüfung

Standardmäßig überprüft HDInsight Tools beim Übermitteln interaktiver PySpark-Abfragen die Umgebung und installiert unabhängige Pakete. Die Umgebungsüberprüfung können Sie deaktivieren, indem Sie **hdinsight.disablePysparkEnvironmentValidation** unter **BENUTZEREINSTELLUNGEN** auf **yes** festlegen.

   ![Festlegen der Umgebungsüberprüfung über die Einstellungen](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check.png)

Alternativ können Sie auf die Schaltfläche **Überprüfung deaktivieren** klicken, wenn das Dialogfeld geöffnet wird.

   ![Festlegen der Umgebungsüberprüfung über das Dialogfeld](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check-dialog.png)

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 wird mit Spark 2.2/2.3 nicht unterstützt

PySpark3 wird mit Spark 2.2- und Spark 2.3-Clustern nicht mehr unterstützt. Für Python wird nur PySpark unterstützt. Es ist ein bekanntes Problem, dass ein Fehler beim Senden an Spark 2.2/2.3 mit Python3 auftritt.

   ![Fehler beim Übermitteln an Python3](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Führen Sie folgende Schritte aus, um Python2.x zu verwenden: 

1. Installieren Sie Python 2.7 auf dem lokalen Computer, und fügen Sie es dem Systempfad hinzu.

2. Starten Sie VS Code neu.

3. Wechseln Sie zu Python 2, indem Sie auf der Statusleiste auf **Python XXX** klicken und dann als Ziel Python auswählen.

   ![Auswählen der Python-Version](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)

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
4. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie anschließend **HDInsight: PySpark Batch** aus, oder verwenden Sie die Tastenkombination **STRG+ALT+H**. 

5. Wählen Sie einen Cluster aus, an den Sie Ihren PySpark-Auftrag übermitteln möchten. 

   ![Übermitteln des Ergebnisses des Python-Auftrags](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Nach dem Übermitteln eines Python-Auftrags werden die Übermittlungsprotokolle in Visual Studio Code im Fenster **AUSGABE** angezeigt. Die **URL der Spark-Benutzeroberfläche** und die **URL der Yarn-Benutzeroberfläche** werden ebenfalls angezeigt. Sie können die URL in einem Webbrowser öffnen, um den Auftragsstatus nachzuverfolgen.

## <a name="apache-livy-configuration"></a>Apache Livy-Konfiguration

Die **Apache Livy**-Konfiguration wird unterstützt. Entsprechende Einstellungen können Sie unter [.VSCode\settings.json](https://livy.incubator.apache.org/) im Arbeitsbereichsordner vornehmen. Derzeit wird in der Livy-Konfiguration nur das Python-Skript unterstützt. Weitere Details finden Sie in der [Livy-INFODATEI](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Auslösen der Livy-Konfiguration**
   
Wählen Sie im Menü **Datei** die Option **Einstellungen** und dann im Kontextmenü die Option **Einstellungen** aus. Klicken Sie auf die Registerkarte **ARBEITSBEREICHSEINSTELLUNGEN**, um dort die Livy-Konfiguration festzulegen.

Sie können auch eine Datei übermitteln. Beachten Sie dabei, dass der Ordner „.vscode“ automatisch dem Arbeitsordner hinzugefügt wird. Die Livy-Konfiguration finden Sie, indem Sie auf **.vscode\settings.json** klicken.

+ Die Projekteinstellungen:

    ![Livy-Konfiguration](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Legen Sie für die Einstellungen **driverMomory** und **executorMomry** den Wert mit Einheit fest, z.B. „1g“ oder „1024m“. 

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

>[!NOTE]
>Die zugewiesene Livy-Konfiguration wird beim Übermitteln des Skripts im Ausgabebereich angezeigt.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integration mit Azure HDInsight per Explorer

Azure HDInsight wurde im linken Bereich hinzugefügt. Sie können den Cluster direkt durchsuchen und verwalten.

1. Erweitern Sie **AZURE HDINSIGHT**, sofern Sie nicht angemeldet sind. Der Link **Sign in to Azure...** (Bei Azure anmelden) wird angezeigt.

    ![Abbildung des Anmeldelinks](./media/hdinsight-for-vscode/hid-azure-hdinsight-sign-in.png)

2. Klicken Sie auf **Sign in to Azure** (Anmelden bei Azure). Unten rechts werden der Anmeldelink und Code angezeigt.

    ![Anmeldeanweisungen für andere Umgebung](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin-code.png)

3. Klicken Sie auf die Schaltfläche **Copy & Open** (Kopieren und öffnen). Fügen Sie im Browser den Code ein, und klicken Sie auf die Schaltfläche **Weiter**. In einer Meldung wird die erfolgreiche Anmeldung angezeigt.

4. Nach der Anmeldung werden die verfügbaren Abonnements und Cluster (Spark, Hadoop und HBase werden unterstützt) unter **AZURE HDINSIGHT** angezeigt. 

   ![Azure HDInsight-Abonnement](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

5. Erweitern Sie den Cluster, um die Hive-Metadatendatenbank und das Hive-Tabellenschema anzuzeigen.

   ![Azure HDInsight-Cluster](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)

## <a name="additional-features"></a>Zusätzliche Funktionen

HDInsight Tools for Visual Studio Code unterstützen folgende Features:

- **IntelliSense-AutoVervollständigen**. Zeigt Vorschläge für Schlüsselwörter, Methoden, Variablen und Ähnliches an. Die unterschiedlichen Objekttypen werden durch entsprechende Symbole dargestellt.

    ![IntelliSense-Objekttypen der HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense-Fehlermarker**. Der Sprachdienst unterstreicht die Bearbeitungsfehler für das Hive-Skript.     
- **Syntaxmarkierungen**. Der Sprachdienst verwendet verschiedene Farben, um Variablen, Schlüsselwörter, Datentypen, Funktionen und Ähnliches zu unterscheiden. 

    ![Syntaxmarkierungen der HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Nächste Schritte

### <a name="demo"></a>Demo
* HDInsight for VS Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Apache Spark-Anwendungen über VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Apache Spark-Anwendungen über SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden der HDInsight-Tools für IntelliJ mit Hortonworks Sandbox](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Apache Spark-Anwendungen](spark/apache-spark-eclipse-tool-plugin.md)
* [Verwenden von Apache Zeppelin Notebooks mit einem Apache Spark-Cluster unter HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernel für Jupyter Notebook in Apache Spark-Clustern für HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualisieren von Apache Hive-Daten mit Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Visualisieren von Interactive Query-Hive-Daten mit Power BI in Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
* [Einrichten der PySpark Interactive-Umgebung für Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Verwenden von Apache Zeppelin zum Ausführen von Apache Hive-Abfragen in Azure HDInsight](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Szenarien
* [Apache Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](spark/apache-spark-use-bi-tools.md)
* [Apache Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Apache Spark in HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](spark/apache-spark-create-standalone-application.md)
* [Ausführen von Remoteaufträgen in einem Apache Spark-Cluster mithilfe von Apache Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](spark/apache-spark-job-debugging.md)



