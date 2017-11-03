---
title: "Vorhersagen der Serverworkload für Daten im Terabyte-Bereich – Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird erläutert, wie ein Machine Learning-Modell für Big Data mithilfe von Azure Machine Learning Workbench trainiert wird."
services: machine-learning
documentationcenter: 
author: daden
manager: mithal
editor: daden
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: daden
ms.openlocfilehash: b962ad3da6d5daff2c8b2524828a9450da702abb
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="server-workload-forecasting-on-terabytes-of-data"></a>Vorhersagen der Serverworkload für Daten im Terabyte-Bereich

In diesem Artikel wird beschrieben, wie Azure Machine Learning Workbench von Data Scientists zum Entwickeln von Lösungen verwendet werden kann, für die die Nutzung von Big Data erforderlich ist. Sie können mit einem Auszug eines großen Datasets beginnen, die Datenvorbereitung, die Featureentwicklung und das Machine Learning durchlaufen und den Prozess dann auf den gesamten Umfang des großen Datasets erweitern. 

Sie erhalten Informationen zu den folgenden wichtigen Funktionen von Machine Learning Workbench:
* Einfaches Wechseln zwischen Computezielen. Sie können unterschiedliche Computeziele einrichten und für das Experimentieren verwenden. In diesem Beispiel verwenden Sie einen Ubuntu-DSVM und einen HDInsight-Cluster als Computeziele. Sie können die Computeziele je nach Verfügbarkeit der Ressourcen auch konfigurieren. Nach dem horizontalen Hochskalieren des Spark-Clusters mit weiteren Workerknoten können Sie beispielsweise die Ressourcen mit Machine Learning Workbench verwenden, um die Ausführungen von Experimenten zu beschleunigen.
* Nachverfolgen des Ausführungsverlaufs. Sie können Machine Learning Workbench verwenden, um die Leistung von Machine Learning-Modellen und anderen relevanten Metriken nachzuverfolgen.
* Operationalisierung des Machine Learning-Modells. Sie können die integrierten Tools in Machine Learning Workbench verwenden, um das trainierte Machine Learning-Modell als Webdienst unter Azure Container Service bereitzustellen. Sie können mit dem Webdienst auch Mini-Batch-Prognosen über REST-API-Aufrufe abrufen. 
* Unterstützung für Terabyte-Daten.

> [!NOTE]
> Codebeispiele und weiteres Material zu diesem Beispiel finden Sie auf [GitHub](https://github.com/Azure/MachineLearningSamples-BigData).
> 

## <a name="use-case-overview"></a>Übersicht über Anwendungsfälle

Prognostizieren der Workload auf Servern ist eine gängige unternehmenswichtige Anforderung in Technologieunternehmen, die ihre eigene Infrastruktur verwalten. Zur Verringerung der Infrastrukturkosten sollten Dienste, die auf wenig ausgelasteten Servern ausgeführt werden, gruppiert werden, damit die Ausführung auf einer geringeren Zahl von Computern erfolgt. Dienste, die auf Servern mit hoher Auslastung ausgeführt werden, sollten mehr Computer für die Ausführung erhalten. 

In diesem Szenario liegt der Schwerpunkt auf der Prognose der Workload für die einzelnen Computer (oder Server). Sie verwenden vor allem die Sitzungsdaten auf jedem Server, um die künftige Workloadklasse des Servers vorherzusagen. Sie klassifizieren die Workload jedes Servers in die Klassen „Niedrig“, „Mittel“ und „Hoch“. Hierfür wird der Random Forest Classifier in [Apache Spark ML](https://spark.apache.org/docs/2.1.1/ml-guide.html) verwendet. Die Machine Learning-Verfahren und der Workflow in diesem Beispiel können auf einfache Weise auf vergleichbare Szenarien angewendet werden. 


## <a name="prerequisites"></a>Voraussetzungen

Folgende Voraussetzungen müssen zum Ausführen dieses Beispiels erfüllt sein:

* Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar)
* Eine installierte Kopie von [Machine Learning Workbench](./overview-what-is-azure-ml.md). Informationen zum Installieren des Programms und Erstellen eines Arbeitsbereichs finden Sie in der [Schnellstartanleitung für die Installation](./quickstart-installation.md).
* Windows 10 (Anleitungen in diesem Beispiel sind für macOS-Systeme im Allgemeinen identisch).
* Ein virtueller Data Science-Computer (DSVM) für Linux (Ubuntu). Sie können einen Ubuntu-DSVM bereitstellen, indem Sie [diese Anleitung](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) befolgen. Sie können auch [diese Schnellstartanleitung](https://ms.portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu) verwenden. Es wird empfohlen, einen virtuellen Computer mit mindestens 8 Kernen und 32 GB Arbeitsspeicher zu verwenden. Sie benötigen IP-Adresse, Benutzername und Kennwort des DSVM, um dieses Beispiel durchzuarbeiten. Speichern Sie die folgende Tabelle mit den DSVM-Informationen für die Verwendung in späteren Schritten:

 Feldname| Wert |  
 |------------|------|
DSVM-IP-Adresse | xxx|
 Benutzername  | xxx|
 Kennwort   | xxx|

 Sie können eine beliebige VM verwenden, auf der das [Docker-Modul](https://docs.docker.com/engine/) installiert ist.

* Ein HDInsight Spark-Cluster mit Hortonworks Data Platform Version 3.6 und Spark Version 2.1.x. Ausführliche Informationen zum Erstellen von HDInsight-Clustern finden Sie unter [Erstellen eines Apache Spark-Clusters in Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql). Es empfiehlt sich, einen Cluster mit drei Workern zu verwenden, wobei jeder Worker über 16 Kerne und 112 GB Arbeitsspeicher verfügt. Sie können aber auch einfach den VM-Typ „`D12 V2`“ für den Hauptknoten und „`D14 V2`“ für den Workerknoten auswählen. Die Bereitstellung des Clusters dauert etwa 20 Minuten. Sie benötigen Clustername, SSH-Benutzername und -Kennwort, um dieses Beispiel durchzuarbeiten. Speichern Sie die folgende Tabelle mit den Azure HDInsight-Clusterinformationen für die Verwendung in späteren Schritten:

 Feldname| Wert |  
 |------------|------|
 Clustername| xxx|
 Benutzername  | xxx (standardmäßig „sshuser“)|
 Kennwort   | xxx|


* Azure Storage-Konto Sie können [diese Anleitung](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) befolgen, um ein Konto zu erstellen. Erstellen Sie in diesem Speicherkonto außerdem zwei private Blobcontainer mit den Namen `fullmodel` und `onemonthmodel`. Im Speicherkonto werden Compute-Zwischenergebnisse und Machine Learning-Modelle gespeichert. Sie benötigen den Speicherkontonamen und den Zugriffsschlüssel, um dieses Beispiel durchzuarbeiten. Speichern Sie die folgende Tabelle mit den Azure-Speicherkontoinformationen für die Verwendung in späteren Schritten:

 Feldname| Wert |  
 |------------|------|
 Speicherkontoname| xxx|
 Zugriffsschüssel  | xxx|


Der Ubuntu-DSVM und der Azure HDInsight-Cluster, die gemäß der Liste der Voraussetzungen erstellt wurden, sind Computeziele. Computeziele sind die Computeressourcen im Kontext von Machine Learning Workbench, die sich von dem Computer unterscheiden können, auf dem Workbench ausgeführt wird.   

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie mit diesem Beispiel als Vorlage ein neues Projekt:
1.  Öffnen Sie Machine Learning Workbench.
2.  Wählen Sie auf der Seite **Projekte** das Pluszeichen (**+**) und dann die Option **Neues Projekt**.
3.  Geben Sie die Informationen für das neue Projekt im Bereich **Neues Projekt erstellen** ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchtext **Workload Forecasting on Terabytes Data** ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**.

Sie können ein Workbench-Projekt mit einem vorab erstellten Git-Repository erstellen, indem Sie [diese Anleitung](./tutorial-classifying-iris-part-1.md) befolgen.  
Führen Sie `git status` aus, um den Status der Dateien für die Versionsverfolgung zu überprüfen.

## <a name="data-description"></a>Datenbeschreibung

Die Daten in diesem Szenario sind neu bestimmte Serverworkloaddaten. Sie werden in einem öffentlich zugänglichen Azure Blob Storage-Konto gehostet. Die konkreten Speicherkontoinformationen finden Sie im Feld `dataFile` von [`Config/storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json). Sie können die Daten direkt aus Blob Storage verwenden. Wenn der Speicher von vielen Benutzern gleichzeitig verwendet wird, können Sie auch [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) verwenden, um die Daten in Ihren eigenen Speicher herunterzuladen. 

Die Gesamtgröße der Daten beträgt ungefähr 1 TB. Jede Datei ist ca. 1 bis 3 GB groß und liegt im CSV-Dateiformat ohne Header vor. Jede Datenzeile stellt die Last einer Transaktion auf einen bestimmten Server dar. Die ausführlichen Informationen des Datenschemas lauten wie folgt:

Spaltennummer | Feldname| Typ | Beschreibung |  
|------------|------|-------------|---------------|
1  | `SessionStart` | Datetime |    Startzeit der Sitzung
2  |`SessionEnd`    | Datetime | Endzeit der Sitzung
3 |`ConcurrentConnectionCounts` | Integer | Anzahl gleichzeitiger Verbindungen
4 | `MbytesTransferred` | Doppelt | Übertragene normalisierte Daten (in MB)
5 | `ServiceGrade` | Integer |  Dienstqualität für die Sitzung
6 | `HTTP1` | Integer|  Sitzung nutzt HTTP1 oder HTTP2
7 |`ServerType` | Integer   |Servertyp
8 |`SubService_1_Load` | Doppelt |   Last von Subdienst 1
9 | `SubService_1_Load` | Doppelt |  Last von Subdienst 2
10 | `SubService_1_Load` | Doppelt |     Last von Subdienst 3
11 |`SubService_1_Load` | Doppelt |  Last von Subdienst 4
12 | `SubService_1_Load`| Doppelt |      Last von Subdienst 5
13 |`SecureBytes_Load`  | Doppelt | Last durch sichere Bytes
14 |`TotalLoad` | Doppelt | Servergesamtauslastung
15 |`ClientIP` | String|    Client-IP-Adresse
16 |`ServerIP` | String|    IP-Adresse des Servers



Beachten Sie, dass die erwarteten Datentypen in der vorherigen Tabelle aufgeführt sind. Aufgrund von fehlenden Werten und Problemen mit ungültigen Daten gibt es keine Garantie, dass die Datentypen wie erwartet vorliegen. Dies sollte bei der Datenverarbeitung berücksichtigt werden. 


## <a name="scenario-structure"></a>Szenariostruktur

Die Dateien in diesem Beispiel sind wie folgt organisiert.

| Dateiname | Typ | Beschreibung |
|-----------|------|-------------|
| `Code` | Ordner | Der Ordner enthält den gesamten Code im Beispiel. |
| `Config` | Ordner | Der Ordner enthält die Konfigurationsdateien. |
| `Image` | Ordner | Der Ordner zum Speichern von Bildern für die Infodatei. |
| `Model` | Ordner | Der Ordner zum Speichern von Modelldateien, die aus Blob Storage heruntergeladen werden. |
| `Code/etl.py` | Python-Datei | Die Python-Datei für Datenvorbereitung und Featureentwicklung. |
| `Code/train.py` | Python-Datei | Die Python-Datei zum Trainieren eines Drei-Klassen-Modells mit Mehrfachklassifizierung.  |
| `Code/webservice.py` | Python-Datei | Die Python-Datei für die Operationalisierung.  |
| `Code/scoring_webservice.py` | Python-Datei |  Die Python-Datei für die Datentransformation und das Aufrufen des Webdiensts. |
| `Code/O16Npreprocessing.py` | Python-Datei | Die Python-Datei zum Vorverarbeiten der Daten für „scoring_webservice.py“  |
| `Code/util.py` | Python-Datei | Die Python-Datei, die Code zum Lesen und Schreiben von Azure-Blobs enthält.  
| `Config/storageconfig.json` | JSON-Datei | Die Konfigurationsdatei für den Azure-Blobcontainer, in dem Zwischenergebnisse und Modelle für das Verarbeiten von Ein-Monats-Daten und das damit verbundene Trainieren gespeichert werden. |
| `Config/fulldata_storageconfig.json` | JSON-Datei | Die Konfigurationsdatei für den Azure-Blobcontainer, in dem Zwischenergebnisse und Modelle für das Verarbeiten und Trainieren anhand eines vollständigen Datasets gespeichert werden.|
| `Config/webservice.json` | JSON-Datei | Die Konfigurationsdatei für „scoring_webservice.py“.|
| `Config/conda_dependencies.yml` | YAML-Datei | Die Conda-Abhängigkeitsdatei. |
| `Config/conda_dependencies_webservice.yml` | YAML-Datei | Die Conda-Abhängigkeitsdatei für den Webdienst.|
| `Config/dsvm_spark_dependencies.yml` | YAML-Datei | Die Spark-Abhängigkeitsdatei für den Ubuntu-DSVM. |
| `Config/hdi_spark_dependencies.yml` | YAML-Datei | Die Spark-Abhängigkeitsdatei für den HDInsight Spark-Cluster. |
| `README.md` | Markdowndatei | Die Info-Markdowndatei. |
| `Code/download_model.py` | Python-Datei | Die Python-Datei zum Herunterladen der Modelldateien aus dem Azure-Blob auf einen lokalen Datenträger. |
| `Docs/DownloadModelsFromBlob.md` | Markdowndatei | Die Markdowndatei, die Anweisungen zum Ausführen von `Code/download_model.py` enthält. |



### <a name="data-flow"></a>Datenfluss

Der Code in [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) lädt Daten aus dem öffentlich zugänglichen Container (Feld `dataFile` von [`Config/storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json)). Er umfasst Datenvorbereitung und Featureentwicklung und speichert die Compute-Zwischenergebnisse Modelle in Ihrem privaten Container. Der Code in [`Code/train.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/train.py) lädt die Compute-Zwischenergebnisse vom privaten Computer, trainiert das Klassifizierungsmodell mit mehreren Klassen und schreibt das trainierte Machine Learning-Modell in den privaten Container. 

Sie sollten einen Container für das Experimentieren mit dem Dataset für einen Monat und einen anderen für das Experimentieren mit dem vollständigen Dataset verwenden. Da die Daten und Modelle als Parquet-Datei gespeichert werden, stellt jede Datei eigentlich einen Ordner im Container dar, der mehrere Blobs enthält. Der erhaltene Container sieht wie folgt aus:

| Blob-Präfixname | Typ | Beschreibung |
|-----------|------|-------------|
| featureScaleModel | Parquet | Standardskalierungsmodell für numerische Features. |
| stringIndexModel | Parquet | Zeichenfolgen-Indexermodell für nicht numerische Features.|
| oneHotEncoderModel|Parquet | One-Hot-Encodermodell für kategorische Features. |
| mlModel | Parquet | Trainiertes Machine Learning-Modell. |
| info| Python-Pickle-Datei | Informationen zu den transformierten Daten, einschließlich Start des Trainings, Ende des Trainings, Dauer, Zeitstempel für die Training-Test-Teilung und Spalten zum Indizieren und One-Hot-Codieren.

Alle Dateien und Blobs in der obigen Tabelle werden für die Operationalisierung verwendet.


### <a name="model-development"></a>Entwickeln der Modelle

#### <a name="architecture-diagram"></a>Architekturdiagramm


Das folgende Diagramm zeigt den End-to-End-Workflow für die Verwendung von Machine Learning Workbench zum Entwickeln des Modells: ![Architektur](media/scenario-big-data/architecture.PNG)

In den folgenden Abschnitten wird die Modellentwicklung unter Verwendung der Remote-Computezielfunktionen in Machine Learning Workbench veranschaulicht. Zunächst wird eine geringe Menge von Beispieldaten geladen, und das Skript [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) wird zur schnellen Iteration auf einem Ubuntu-DSVM ausgeführt. Die in [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) ausgeführten Arbeiten können weiter beschränkt werden, indem ein zusätzliches Argument für die schnellere Iteration übergeben. Schließlich führen wir das Training mit den gesamten Daten mit einem HDInsight-Cluster durch.     

Die Datei [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) dient zum Laden und Vorbereiten der Daten und Durchführen der Featureentwicklung. Sie akzeptiert zwei Argumente:
* Eine Konfigurationsdatei für den Blobspeichercontainer zum Speichern der Compute-Zwischenergebnisse und Modelle.
* Ein Debug-Konfigurationsargument für eine schnellere Iteration.

Das erste Argument `configFilename` ist eine lokale Konfigurationsdatei, in der Sie die Blob Storage-Informationen speichern und angeben, wo die Daten geladen werden sollen. In der Standardeinstellung ist dies die Datei [`Config/storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/storageconfig.json), die im Durchlauf mit den Daten eines Monats verwendet wird. Zudem schließen wir die Datei [`Config/fulldata_storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) ein, die Sie für die Ausführung mit dem vollständigen Dataset benötigen. Die Konfiguration hat folgenden Inhalt: 

| Feld | Typ | Beschreibung |
|-----------|------|-------------|
| storageAccount | String | Azure Storage-Kontoname |
| storageContainer | String | Container im Azure Storage-Konto zum Speichern von Zwischenergebnissen |
| storageKey | String |Zugriffsschlüssel für das Azure Storage-Konto |
| dataFile|String | Datenquellendateien  |
| duration| String | Dauer der Daten in den Datenquelldateien|

Ändern Sie sowohl `Config/storageconfig.json` als auch `Config/fulldata_storageconfig.json`, um das Speicherkonto, den Speicherschlüssel und den Blobcontainer zum Speichern der Zwischenergebnisse zu konfigurieren. In der Standardeinstellung ist der Blobcontainer für den Durchlauf mit den Daten eines Monats `onemonthmodel`, und der Blobcontainer für die Ausführung mit dem vollständigen Dataset ist `fullmodel`. Sie müssen diese beiden Container in Ihrem Speicherkonto erstellen. Mit dem Feld `dataFile` in [`Config/fulldata_storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) wird konfiguriert, welche Daten in [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) geladen werden. Mit dem Feld `duration` wird der Bereich der Daten konfiguriert. Wenn die Dauer auf „ONE_MONTH“ festgelegt ist, sollten sich die geladenen Daten in nur einer CSV-Datei der sieben Dateien mit Daten für Juni 2016 befinden. Ist die Dauer hingegen auf „FULL“ festgelegt, wird das vollständige Dataset (1 TB) geladen. Sie müssen `dataFile` und `duration` in diesen beiden Konfigurationsdateien nicht ändern.

Das zweite Argument ist DEBUG. Wenn Sie es auf „FILTER_IP“ festlegen, wird eine schnellere Iteration ermöglicht. Die Verwendung dieses Parameters ist hilfreich, wenn Sie Ihr Skript debuggen möchten.

> [!NOTE]
> Ersetzen Sie sämtliche Variablen der Argumente in allen folgenden Befehlen durch die tatsächlichen Werte.
> 


#### <a name="model-development-on-the-docker-of-ubuntu-dsvm"></a>Modellentwicklung unter Docker auf dem Ubuntu-DSVM

#####  <a name="1-set-up-the-compute-target"></a>1. Einrichten des Computeziels

Starten Sie die Befehlszeile über Machine Learning Workbench, indem Sie **Datei** > **Eingabeaufforderung öffnen** wählen. Führen Sie anschließend Folgendes aus: 

```az ml computetarget attach --name dockerdsvm --address $DSVMIPaddress  --username $user --password $password --type remotedocker```

Die folgenden beiden Dateien werden im Ordner „aml_config“ Ihres Projekts erstellt:

-  dockerdsvm.compute: Diese Datei enthält die Verbindungs- und Konfigurationsinformationen für ein Remoteausführungsziel.
-  dockerdsvm.runconfig: Diese Datei umfasst eine Reihe von Ausführungsoptionen, die in der Workbench-Anwendung verwendet werden.

Navigieren Sie zu „dockerdsvm.runconfig“, und ändern Sie die Konfiguration dieser Felder wie folgt:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/dsvm_spark_dependencies.yml

Bereiten Sie die Projektumgebung vor, indem Sie Folgendes ausführen:

```az ml experiment prepare -c dockerdsvm```


Bei Festlegung von `PrepareEnvironment` auf „true“ erstellt Machine Learning Workbench die Laufzeitumgebung jeweils, wenn Sie einen Auftrag übermitteln. `Config/conda_dependencies.yml` und `Config/dsvm_spark_dependencies.yml` enthalten die Anpassung der Laufzeitumgebung. Sie können die Conda-Abhängigkeiten, die Spark-Konfiguration und die Spark-Abhängigkeiten jederzeit ändern, indem Sie diese beiden YMAL-Dateien bearbeiten. Für dieses Beispiel haben wir `azure-storage` und `azure-ml-api-sdk` als zusätzliche Python-Pakete in `Config/conda_dependencies.yml` hinzugefügt. Außerdem haben wir `spark.default.parallelism`, `spark.executor.instances` und `spark.executor.cores` in `Config/dsvm_spark_dependencies.yml` hinzugefügt. 

#####  <a name="2-data-preparation-and-feature-engineering-on-dsvm-docker"></a>2. Datenvorbereitung und Featureentwicklung für DSVM-Docker

Führen Sie das Skript `etl.py` für DSVM-Docker aus. Verwenden Sie einen Debugparameter, mit dem die geladenen Daten mit bestimmten Server-IP-Adressen gefiltert werden:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FILTER_IP```

Navigieren Sie zur Randleiste, und wählen Sie **Ausführen**, um den Ausführungsverlauf von `etl.py` anzuzeigen. Beachten Sie, dass die Ausführungsdauer etwa zwei Minuten beträgt. Wenn Sie den Code so ändern möchten, dass er neue Features enthält, bewirkt das Angeben von FILTER_IP als zweites Argument eine schnellere Iteration. Sie müssen diesen Schritt unter Umständen mehrmals ausführen, wenn Sie eigene Machine Learning-Probleme behandeln, um das Dataset zu untersuchen und neue Features zu erstellen. 

Durch benutzerdefiniertes Einschränken der zu ladenden Daten und weiteres Filtern der zu verarbeitenden Daten können Sie den Iterationsvorgang bei der Entwicklung Ihres Modells beschleunigen. Beim Experimentieren sollten Sie die Änderungen am Code regelmäßig im Git-Repository speichern. Beachten Sie, dass wir mit dem folgenden Code in `etl.py` den Zugriff auf den privaten Container ermöglicht haben:

```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)

# attach the blob storage to the spark cluster or VM so that the storage can be accessed by the cluster or VM        
attach_storage_container(spark, storageAccount, storageKey)
```


Führen Sie nun das Skript `etl.py` für DSVM-Docker ohne den Debugparameter FILTER_IP aus:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FALSE```

Navigieren Sie zur Randleiste, und wählen Sie **Ausführen**, um den Ausführungsverlauf von `etl.py` anzuzeigen. Beachten Sie, dass die Ausführungsdauer etwa vier Minuten beträgt. Das verarbeitete Ergebnis dieses Schritts wird im Container gespeichert und für das Training in „train.py“ geladen. Darüber hinaus werden auch die Zeichenfolgenindexer, Encoder-Pipelines und die Standardskalierungen im privaten Container gespeichert. Sie werden bei der Operationalisierung verwendet. 


##### <a name="3-model-training-on-dsvm-docker"></a>3. Modelltraining für DSVM-Docker

Führen Sie das Skript `train.py` für den DSVM-Docker aus:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/train.py ./Config/storageconfig.json```

In diesem Schritt werden die Compute-Zwischenergebnisse aus der Ausführung von `etl.py` geladen, und es wird ein Machine Learning-Modell trainiert. Dieser Schritt dauert ungefähr zwei Minuten.

Nachdem Sie die Experimente mit der geringen Datenmenge erfolgreich abgeschlossen haben, können Sie mit den Experimenten am vollständigen Dataset fortfahren. Zunächst können Sie denselben Code verwenden und dann mit geänderten Argumenten und Computezielen experimentieren.  

####  <a name="model-development-on-the-hdinsight-cluster"></a>Modellentwicklung für den HDInsight-Cluster

##### <a name="1-create-the-compute-target-in-machine-learning-workbench-for-the-hdinsight-cluster"></a>1. Erstellen des Computeziels in Machine Learning Workbench für den HDInsight-Cluster

```az ml computetarget attach --name myhdi --address $clustername-ssh.azurehdinsight.net --username $username --password $password --type cluster```

Die folgenden beiden Dateien werden im Ordner „aml_config“ erstellt:
    
-  myhdo.compute: Diese Datei enthält die Verbindungs- und Konfigurationsinformationen für ein Remoteausführungsziel.
-  myhdi.runconfig: Diese Datei umfasst eine Reihe von Ausführungsoptionen, die in der Workbench-Anwendung verwendet werden.


Navigieren Sie zu „myhdi.runconfig“, und ändern Sie die Konfiguration der folgenden Felder wie folgt:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/hdi_spark_dependencies.yml

Bereiten Sie die Projektumgebung vor, indem Sie Folgendes ausführen:

```az ml experiment prepare -c myhdi```

Dieser Schritt kann bis zu sieben Minuten dauern.

##### <a name="2-data-preparation-and-feature-engineering-on-hdinsight-cluster"></a>2. Datenvorbereitung und Featureentwicklung für den HDInsight-Cluster

Führen Sie das Skript `etl.py` mit den vollständigen Daten im HDInsight-Cluster aus:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/etl.py Config/fulldata_storageconfig.json FALSE```

Da dieser Auftrag relativ lange dauert (ca. zwei Stunden), können Sie `-a` verwenden, um das Ausgabestreaming zu deaktivieren. Nach Abschluss des Auftrags können Sie unter **Ausführungsverlauf** die Treiber- und Controllerprotokolle anzeigen. Wenn Sie über einen größeren Cluster verfügen, können Sie die Konfigurationen in `Config/hdi_spark_dependencies.yml` jederzeit so ändern, dass eine größere Anzahl von Instanzen oder von Kernen verwendet wird. Bei einem Cluster mit vier Workerknoten können Sie den Wert von `spark.executor.instances` beispielsweise von 5 auf 7 heraufsetzen. Die Ausgabe dieses Schritts können Sie im Container **fullmodel** Ihres Speicherkontos einsehen. 


##### <a name="3-model-training-on-hdinsight-cluster"></a>3. Modelltraining für den HDInsight-Cluster

Führen Sie das Skript `train.py` für den HDInsight-Cluster aus:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/train.py Config/fulldata_storageconfig.json```

Da dieser Auftrag relativ lange dauert (ca. 30 Minuten), können Sie `-a` verwenden, um das Ausgabestreaming zu deaktivieren.

#### <a name="run-history-exploration"></a>Untersuchen des Ausführungsverlaufs

Der Ausführungsverlauf ist ein Feature, mit dem Sie Ihre Experimente in Machine Learning Workbench nachverfolgen können. In der Standardeinstellung wird damit die Dauer der Experimente verfolgt. Wenn wir in unserem konkreten Beispiel in den Experimenten zum kompletten Dataset für `Code/etl.py` wechseln, stellen wir fest, dass sich die Dauer erheblich verlängert. Sie können auch bestimmte Metriken für die Nachverfolgung aufzeichnen. Um die Verfolgung von Metriken zu aktivieren, fügen Sie dem Kopf der Python-Datei die folgenden Codezeilen hinzu:
```python
# import logger
from azureml.logging import get_azureml_logger

# initialize logger
run_logger = get_azureml_logger()
```
Hier ist ein Beispiel für das Verfolgen einer bestimmten Metrik:

```python
run_logger.log("Test Accuracy", testAccuracy)
```

Navigieren Sie in der rechten Seitenleiste von Workbench zu **Ausführungen**, um den Ausführungsverlauf für jede Python-Datei anzuzeigen. Sie können auch zu Ihrem GitHub-Repository navigieren. Eine neue Verzweigung mit dem Namen, der mit „AMLHistory“ beginnt, wird erstellt, um die am Skript vorgenommene Änderung bei jeder Ausführung nachzuverfolgen. 


### <a name="operationalize-the-model"></a>Operationalisieren des Modells

In diesem Abschnitt operationalisieren Sie das Modell, das Sie in den vorherigen Schritten erstellt haben, als Webdienst. Außerdem erfahren Sie, wie Sie den Webdienst zum Vorhersagen der Workload verwenden. Verwenden Sie Machine Language-CLIs (Befehlszeilenschnittstellen) für die Operationalisierung, um den Code und die Abhängigkeiten als Docker-Images zu verpacken und das Modell als Containerwebdienst zu veröffentlichen. Weitere Informationen finden Sie in [dieser Übersicht](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/operationalization-overview.md).

Sie können die Eingabeaufforderung in Machine Learning Workbench zum Ausführen der CLIs verwenden.  Außerdem können Sie die CLIs in Ubuntu Linux ausführen, indem Sie die Anleitung im [Installationsleitfaden](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/install-on-ubuntu-linux.md) befolgen. 

> [!NOTE]
> Ersetzen Sie sämtliche Variablen der Argumente in allen folgenden Befehlen durch die tatsächlichen Werte. Das Durcharbeiten dieses Abschnitts dauert ca. 40 Minuten.
> 

Wählen Sie eine eindeutige Zeichenfolge als Umgebung für die Operationalisierung aus. Hier verwenden wir die Zeichenfolge „[unique]“, um die von Ihnen gewählte Zeichenfolge darzustellen.

1. Erstellen Sie die Umgebung für die Operationalisierung und dann die Ressourcengruppe.

        az ml env setup -c -n [unique] --location eastus2 --cluster -z 5 --yes

   Beachten Sie, dass Sie den Containerdienst als Umgebung nutzen können, indem Sie `--cluster` im Befehl `az ml env setup` verwenden. Sie können das Machine Learning-Modell unter [Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-intro-kubernetes) operationalisieren. Hierbei wird [Kubernetes](https://kubernetes.io/) zum Automatisieren der Bereitstellung, Skalierung und Verwaltung von Anwendungen in Containern genutzt. Die Ausführung dieses Befehls dauert ungefähr 20 Minuten. Verwenden Sie Folgendes, um zu überprüfen, ob die Bereitstellung erfolgreich abgeschlossen wurde: 

        az ml env show -g [unique]rg -n [unique]

   Legen Sie die Bereitstellungsumgebung auf die soeben erstellte Umgebung fest, indem Sie Folgendes ausführen:

        az ml env set -g [unique]rg -n [unique]

2. Erstellen und verwenden Sie ein Konto für die Modellverwaltung. Führen Sie Folgendes aus, um ein Konto für die Modellverwaltung zu erstellen:

        az ml account modelmanagement create --location  eastus2 -n [unique]acc -g [unique]rg --sku-instances 4 --sku-name S3 

   Führen Sie Folgendes aus, um die Modellverwaltung für die Operationalisierung zu nutzen:

        az ml account modelmanagement set  -n [unique]acc -g [unique]rg  

   Ein Konto für die Modellverwaltung wird verwendet, um die Modelle und Webdienste zu verwalten. Im Azure-Portal sehen Sie, dass ein neues Konto für die Modellverwaltung erstellt wurde. Sie sehen die Modelle, Manifeste, Docker-Images und Dienste, die mit diesem Konto für die Modellverwaltung erstellt werden.

3. Laden Sie die Modelle herunter, und registrieren Sie sie.

   Laden Sie die Modelle im Container **fullmodel** in das Codeverzeichnis auf Ihrem lokalen Computer herunter. Laden Sie nicht die Parquet-Datendatei mit dem Namen „vmlSource.parquet“ herunter. Es handelt hierbei nicht um eine Modelldatei, sondern um ein Compute-Zwischenergebnis. Sie können auch die Modelldateien wiederverwenden, die im Git-Repository enthalten sind. Weitere Informationen finden Sie auf [GitHub](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Docs/DownloadModelsFromBlob.md). 

   Wechseln Sie in der CLI zum Ordner `Model`, und registrieren Sie die Modelle wie folgt:

        az ml model register -m  mlModel -n vmlModel -t fullmodel
        az ml model register -m  featureScaleModel -n featureScaleModel -t fullmodel
        az ml model register -m  oneHotEncoderModel -n  oneHotEncoderModel -t fullmodel
        az ml model register -m  stringIndexModel -n stringIndexModel -t fullmodel
        az ml model register -m  info -n info -t fullmodel

   Die Ausgabe jedes Befehls liefert eine Modell-ID, die im nächsten Schritt benötigt wird. Speichern Sie diese in einer Textdatei für die künftige Verwendung.

4. Erstellen Sie ein Manifest für den Webdienst.

   Ein Manifest enthält den Code für den Webdienst sowie sämtliche Machine Learning-Modelle und Laufzeitumgebungsabhängigkeiten. Wechseln Sie in der CLI zum Ordner `Code`, und führen Sie den folgenden Befehl aus:

        az ml manifest create -n $webserviceName -f webservice.py -r spark-py -c ../Config/conda_dependencies_webservice.yml -i $modelID1 -i $modelID2 -i $modelID3 -i $modelID4 -i $modelID5

   Die Ausgabe liefert eine Manifest-ID für den nächsten Schritt. 

   Bleiben Sie im Verzeichnis `Code`, und testen Sie „webservice.py“, indem Sie Folgendes ausführen: 

        az ml experiment submit -t dockerdsvm -c dockerdsvm webservice.py

5. Erstellen Sie ein Docker-Image. 

        az ml image create -n [unique]image --manifest-id $manifestID

   Die Ausgabe liefert eine Image-ID für den nächsten Schritt. Dieses Docker-Image wird im Container Service verwendet. 

6. Stellen Sie den Webdienst für den Container Service-Cluster bereit.

        az ml service create realtime -n [unique] --image-id $imageID --cpu 0.5 --memory 2G

   Die Ausgabe enthält eine Dienst-ID. Sie benötigen sie, um den Autorisierungsschlüssel und die Dienst-URL abzurufen.

7. Rufen Sie den Webdienst in Python-Code auf, um Mini-Batches zu bewerten.

   Rufen Sie den Autorisierungsschlüssel mit dem folgenden Befehl ab:

         az ml service keys realtime -i $ServiceID 

   Verwenden Sie den folgenden Befehl, um die Dienstbewertungs-URL abzurufen:

        az ml service usage realtime -i $ServiceID

   Ändern Sie den Inhalt von `./Config/webservice.json`, indem Sie die richtige Dienstbewertungs-URL und den Autorisierungsschlüssel eingeben. Behalten Sie den „Bearer“ in der Originaldatei bei, und ersetzen Sie den Teil „xxx“. 
   
   Wechseln Sie in das Stammverzeichnis des Projekts, und testen Sie den Webdienst auf Mini-Batch-Bewertungen, indem Sie Folgendes verwenden:

        az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/scoring_webservice.py ./Config/webservice.json

8. Skalieren Sie den Webdienst. 

   Weitere Informationen finden Sie unter [How to scale operationalization on your Azure Container Service cluster](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/how-to-scale.md) (Skalieren der Operationalisierung in Ihrem Azure Container Service-Cluster).
 

## <a name="next-steps"></a>Nächste Schritte

In diesem Beispiel wurde beschrieben, wie mithilfe von Machine Learning Workbench ein Machine Learning-Modell für Big Data trainiert und das trainierte Modell operationalisiert wird. Insbesondere haben Sie erfahren, wie Sie unterschiedliche Computeziele konfigurieren und verwenden, den Verlauf für die Nachverfolgung von Metriken ausführen und verschiedene Ausführungen verwenden.

Sie können den Code erweitern, um die Kreuzvalidierung und das Optimieren von Hyperparametern zu untersuchen. Weitere Informationen zur Kreuzvalidierung und zur Optimierung von Hyperparametern finden Sie in [dieser GitHub-Ressource](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning).  

Weitere Informationen zu Zeitreihenprognosen finden Sie in [dieser GitHub-Ressource](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting).
