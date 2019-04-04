---
title: Lokales Installieren von Jupyter und Herstellen einer Verbindung mit Spark in Azure HDInsight
description: Erfahren Sie, wie Sie Jupyter Notebook auf Ihrem Computer installieren und eine Verbindung mit einem Apache Spark-Cluster herstellen.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: hrasheed
ms.openlocfilehash: 7b20f0ec4669b485f87d050fcf597244fb898c85
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58091255"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung mit Apache Spark in HDInsight

In diesem Artikel erfahren Sie, wie Sie Jupyter Notebook mit den benutzerdefinierten Kerneln PySpark (für Python) und Apache Spark (für Scala) mit Spark Magic installieren und das Notebook mit einem HDInsight-Cluster verbinden. Es kann eine Reihe von Gründen geben, Jupyter auf dem lokalen Computer zu installieren, und es auch können auch einige Probleme auftreten. Weitere Informationen hierzu finden Sie im Abschnitt [Warum sollte ich Jupyter auf meinem Computer installieren?](#why-should-i-install-jupyter-on-my-computer) am Ende dieses Artikels.

Es gibt vier wichtige Schritte zum Installieren von Jupyter und Herstellen einer Verbindung mit Apache Spark in HDInsight.

* Konfigurieren Sie den Spark-Cluster.
* Installieren Sie Jupyter Notebook.
* Installieren Sie die PySpark- und Spark-Kernel mit Spark Magic.
* Konfigurieren Sie Spark Magic für den Zugriff auf den Spark-Cluster in HDInsight.

Weitere Informationen zu den benutzerdefinierten Kerneln und Spark Magic für Jupyter-Notebooks mit HDInsight-Clustern finden Sie unter [Verfügbare Kernels für Jupyter-Notebooks mit Apache Spark-Linux-Clustern in HDInsight](apache-spark-jupyter-notebook-kernels.md).

> [!IMPORTANT]  
> Die Schritte in diesem Artikel funktionieren nur mit Spark-Version 2.1.0.

## <a name="prerequisites"></a>Voraussetzungen
Die hier aufgeführten Voraussetzungen gelten nicht für die Installation von Jupyter. Sie gelten für die Herstellung einer Verbindung von einem Jupyter Notebook zu einem HDInsight-Cluster, sobald das Notebook installiert wurde.

* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Einen Apache Spark-Cluster (bis Version 2.1.0) unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).



## <a name="install-jupyter-notebook-on-your-computer"></a>Installieren von Jupyter Notebook auf Ihrem Computer

Sie müssen Python installieren, bevor Sie Jupyter Notebooks installieren können. Python und Jupyter stehen als Bestandteile der [Anaconda-Verteilung](https://www.anaconda.com/download/)zur Verfügung. Bei der Installation von Anaconda installieren Sie eine Python-Verteilung. Nachdem Sie Anaconda installiert haben, können Sie die Jupyter-Installation durch Ausführen der entsprechenden Befehle hinzufügen.

1. Laden Sie das [Anaconda-Installationsprogramm](https://www.anaconda.com/download/) für Ihre Plattform herunter, und führen Sie das Setup aus. Stellen Sie sicher, dass während der Ausführung des Setup-Assistenten die Option für das Hinzufügen von Anaconda zu Ihrer PATH-Variablen aktiviert ist.

2. Führen Sie den folgenden Befehl aus, um Jupyter zu installieren.

        conda install jupyter

    Weitere Informationen zur Installation von Jupyter finden Sie unter [Installing Jupyter using Anaconda](https://jupyter.readthedocs.io/en/latest/install.html)(Installieren von Jupyter mit Anaconda).

## <a name="install-the-kernels-and-spark-magic"></a>Installieren der Kernel und Spark Magic

Informationen zum Installieren von Spark Magic und der PySpark- und Spark-Kernel finden Sie in den Installationsanweisungen in der [Spark Magic-Dokumentation](https://github.com/jupyter-incubator/sparkmagic#installation) auf GitHub. In der Spark Magic-Dokumentation werden Sie im ersten Schritt aufgefordert, Spark Magic zu installieren. Ersetzen Sie abhängig von der Version des HDInsight-Clusters, mit dem Sie eine Verbindung herstellen möchten, diesen ersten Schritt in dem Link durch die folgenden Befehle. Führen Sie anschließend die restlichen Schritte in der Spark Magic-Dokumentation aus. Wenn Sie die verschiedenen Kernel installieren möchten, müssen Sie Schritt 3 im Abschnitt mit den Spark Magic-Installationsanweisungen ausführen.

* Führen Sie für Cluster der Versionen 3.5 und 3.6 den Befehl `pip install sparkmagic==0.11.2` aus, um sparkmagic 0.11.2 zu installieren.

* Führen Sie für Cluster der Version 3.4 den Befehl `pip install sparkmagic==0.2.3` aus, um sparkmagic 0.2.3 zu installieren.

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Konfigurieren von Spark Magic zum Herstellen der Verbindung mit dem Spark-Cluster in HDInsight

In diesem Abschnitt lernen Sie, Spark Magic zu konfigurieren, nachdem Sie es installiert haben, um eine Verbindung mit einem Apache Spark-Cluster herzustellen. Dieser muss bereits in Azure HDInsight erstellt worden sein.

1. Starten Sie die Python-Shell mit dem folgenden Befehl:

    ```
    python
    ```

2. Die Konfigurationsinformationen für Jupyter werden in der Regel im Basisverzeichnis des Benutzers gespeichert. Geben Sie den folgenden Befehl ein, um das Basisverzeichnis zu ermitteln, und erstellen Sie dort den Ordner **.sparkmagic**.  Der vollständige Pfad wird ausgegeben.

    ```python
    import os
    path = os.path.expanduser('~') + "\\.sparkmagic"
    os.makedirs(path)
    print(path)
    exit()
    ```

3. Erstellen Sie im Ordner `.sparkmagic` die Datei **config.json**, und fügen Sie den folgenden JSON-Codeausschnitt darin ein.  

    ```json
    {
      "kernel_python_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "kernel_scala_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "heartbeat_refresh_seconds": 5,
      "livy_server_heartbeat_timeout_seconds": 60,
      "heartbeat_retry_seconds": 1
    }
    ```
4. Nehmen Sie die folgenden Änderungen an der Datei vor:

    |Vorlagenwert | Neuer Wert |
    |---|---|
    |{BENUTZERNAME}|Clusteranmeldung, standardmäßig das Administratorkonto.|
    |{CLUSTER-DNS-NAME}|Clustername|
    |{BASE64-CODIERTES-KENNWORT}|Ein Base64-codiertes Kennwort für Ihr eigentliches Kennwort.  Sie können unter [https://www.url-encode-decode.com/base64-encode-decode/](https://www.url-encode-decode.com/base64-encode-decode/) ein Base64-Kennwort generieren.|
    |`"livy_server_heartbeat_timeout_seconds": 60`|Behalten Sie den Wert bei, wenn Sie `sparkmagic 0.11.23` verwenden (Cluster der Versionen 3.5 und 3.6).  Wenn Sie `sparkmagic 0.2.3` verwenden (Cluster der Version 3.4), ersetzen Sie den Wert durch `"should_heartbeat": true`.|

    Eine vollständige Beispieldatei finden Sie im [Beispiel für „config.json“](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

   > [!TIP]  
   > Takte werden gesendet, um sicherzustellen, dass bei Sitzungen keine Verluste auftreten. Wenn ein Computer in den Ruhezustand versetzt oder heruntergefahren wird, wird der Takt nicht gesendet, und die Sitzung wird bereinigt. Für Cluster der Version 3.4 gilt: Wenn Sie dieses Verhalten deaktivieren möchten, können Sie die Livy-Konfiguration `livy.server.interactive.heartbeat.timeout` über die Ambari-Benutzeroberfläche auf `0` festlegen. Für Cluster der Version 3.5 gilt: Wenn Sie die obige 3.5-Konfiguration nicht festlegen, wird die Sitzung nicht gelöscht.

5. Starten Sie Jupyter. Geben Sie in der Eingabeaufforderung folgenden Befehl ein:

        jupyter notebook

6. Stellen Sie sicher, dass Sie die verfügbare Spark Magic-Version mit den Kerneln verwenden können. Führen Sie die folgenden Schritte aus:

    a. Erstellen Sie ein neues Notebook. Wählen Sie in der rechten Ecke **Neu** aus. Daraufhin sollten der **Python 2**- oder **Python 3**-Standardkernel und die von Ihnen installierten Kernel angezeigt werden. Die tatsächlichen Werte variieren je nach Ihren Installationsentscheidungen.  Wählen Sie **PySpark** aus.

    ![Kernels in Jupyter Notebook](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels in Jupyter Notebook")

    > [!IMPORTANT]  
    > Überprüfen Sie Ihre Shell nach der Auswahl von **Neu** auf Fehler.  Wenn der Fehler `TypeError: __init__() got an unexpected keyword argument 'io_loop'` angezeigt wird, ist möglicherweise ein bekanntes Problem mit bestimmten Versionen von Tornado aufgetreten.  Wenn dies der Fall ist, beenden Sie den Kernel, und stufen Sie anschließend die Tornado-Installation mit dem folgenden Befehl herab: `pip install tornado==4.5.3`.

    b. Führen Sie den folgenden Codeausschnitt aus.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 5
    ```  

    Wenn Sie die Ausgabe erfolgreich abrufen können, wird Ihre Verbindung zu dem HDInsight-Cluster getestet.

    Wenn Sie die Notebookkonfiguration für die Verbindung mit einem anderen Cluster aktualisieren möchten, aktualisieren Sie die Datei „config.json“ mit einem neuen Satz von Werten, wie in Schritt 3 oben dargestellt.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Warum sollte ich Jupyter auf meinem Computer installieren?

Es kann zahlreiche Gründe geben, warum Sie Jupyter auf dem Computer installieren und mit einem Apache Spark-Cluster in HDInsight verbinden möchten.

* Obwohl die Jupyter Notebooks bereits im Spark-Cluster in Azure HDInsight verfügbar sind, bietet Ihnen die Installation von Jupyter auf Ihrem Computer die Option, Ihre Notebooks lokal zu erstellen, Ihre Anwendung in einem aktiven Cluster zu testen und die Notebooks anschließend in den Cluster hochzuladen. Sie können die Notebooks entweder mithilfe des im Cluster ausgeführten Jupyter Notebooks hochladen oder sie im Ordner „/HdiNotebooks“ in dem dem Cluster zugeordneten Speicherkonto speichern. Weitere Informationen zum Speichern von Notebooks im Cluster finden Sie unter [Wo werden die Notebooks gespeichert?](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)
* Wenn Notebooks lokal verfügbar sind, können Sie basierend auf den Anwendungsanforderungen Verbindungen mit unterschiedlichen Spark-Clustern herstellen.
* Sie können GitHub verwenden, um ein Quellcodeverwaltungssystem zu implementieren und Versionskontrolle für die Notebooks zu ermöglichen. Sie können auch eine Umgebung für Zusammenarbeit einrichten, in der mehrere Benutzer mit dem gleichen Notebook arbeiten können.
* Sie können mit Notebooks lokal ohne einen Cluster arbeiten. Sie benötigen einen Cluster nur zum Testen der Notebooks, nicht für das manuelle Verwalten von Notebooks oder einer Entwicklungsumgebung.
* Es ist möglicherweise einfacher, Ihre eigene lokale Entwicklungsumgebung zu konfigurieren, statt die Jupyter-Installation im Cluster zu konfigurieren.  Sie können sämtliche Software nutzen, die Sie lokal installiert haben, ohne Remotecluster zu konfigurieren.

> [!WARNING]  
> Wenn Jupyter auf dem lokalen Computer installiert ist, können mehrere Benutzer gleichzeitig das gleiche Notebook im gleichen Spark-Cluster ausführen. In diesem Fall werden mehrere Livy-Sitzungen erstellt. Wenn ein Problem auftritt, das Sie debuggen möchten, ist es eine komplexe Aufgabe nachzuverfolgen, welche Livy-Sitzung welchem Benutzer gehört.  

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien
* [Apache Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Apache Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](apache-spark-create-standalone-application.md)
* [Ausführen von Remoteaufträgen in einem Apache Spark-Cluster mithilfe von Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](apache-spark-intellij-tool-plugin.md)
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Apache Spark-Anwendungen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Apache Zeppelin Notebooks mit einem Apache Spark-Cluster unter HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernel für Jupyter Notebook in Apache Spark-Clustern für HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)
