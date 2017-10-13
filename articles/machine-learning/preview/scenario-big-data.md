---
title: "Vorhersagen der Serverworkload für Terabyte-Daten – Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird erläutert, wie ein Machine Learning-Modell für Big Data mithilfe von Azure ML Workbench trainiert wird."
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
ms.openlocfilehash: b76253fad43be231591023c4d4466bf6e3f329a0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="server-workload-forecasting-on-terabytes-data"></a>Vorhersagen der Serverworkload für Terabyte-Daten

In diesem Beispiel wird veranschaulicht, wie Datenanalysten mithilfe von Azure ML Workbench Lösungen entwickeln, bei denen Big Data verwendet werden müssen. Dabei wird erläutert, wie Azure ML Workbench einem optimalen Pfad folgen kann, ausgehend von einer Stichprobe aus einem großen Dataset, über das Durchlaufen der Datenvorbereitung, Featureentwicklung und Machine Learning bis hin zum Ausweiten des Prozesses auf das gesamte große Dataset. 

Dabei werden die folgenden Schlüsselfunktionen von Azure ML Workbench veranschaulicht:
* Einfaches Wechseln zwischen Compute-Zielen: Wir beschreiben, wie Benutzer unterschiedliche Compute-Ziele einrichten und in Experimenten verwenden können. In diesem Beispiel verwenden wir einen Ubuntu-DSVM und einen HDInsight-Cluster als Compute-Ziele. Darüber hinaus erklären wir, wie die Compute-Ziele in Abhängigkeit von den verfügbaren Ressourcen konfiguriert werden. Dabei wird insbesondere darauf eingegangen, wie der Benutzer nach dem horizontalen Hochskalieren des Spark-Clusters (d.h. nach dem Einschließen weiterer Workerknoten in den Spark-Cluster) die Ressourcen über Azure ML Workbench verwenden kann, um Experimentdurchläufe zu beschleunigen.
* Verfolgen des Ausführungsverlaufs: Es wird erläutert, wie mithilfe von Azure ML Workbench die Leistung von ML-Modellen und andere relevante Metriken verfolgt werden können.
* Operationalisierung des Machine Learning-Modells: Wir erläutern die Verwendung der in Azure ML Workbench integrierten Tools zum Bereitstellen des trainierten ML-Modells als Webdienst in Azure Container Service (ACS). Darüber hinaus wird beschrieben, wie mit dem Webdienst Mini-Batch-Prognosen über REST-API-Aufrufe abgerufen werden können. 
* Unterstützung für Terabyte-Daten.



## <a name="link-to-the-gallery-github-repository"></a>Link zum Katalog des GitHub-Repositorys

Das öffentliche GitHub-Repository für dieses Beispiel enthält sämtliche Materialien, einschließlich von Codebeispielen: 
 
[https://github.com/Azure/MachineLearningSamples-BigData](https://github.com/Azure/MachineLearningSamples-BigData)



## <a name="use-case-overview"></a>Übersicht über den Anwendungsfall


Prognostizieren der Workload auf Servern ist eine gängige unternehmenswichtige Anforderung in Technologieunternehmen, die ihre eigene Infrastruktur verwalten. Zum Senken von Infrastrukturkosten sollten Dienste auf unzureichend ausgelasteten Servern gruppiert werden, sodass sie auf einer kleineren Anzahl von Computern ausgeführt werden können. Diensten auf stark ausgelasteten Servern sollten für die Ausführung mehr Computer zugewiesen werden. In diesem Szenario liegt der Schwerpunkt auf der Prognose der Workload für die einzelnen Computer (oder Server). Insbesondere verwenden wir die Sitzungsdaten auf jedem Server, um die künftige Workloadklasse des Servers vorherzusagen. Die Workload jedes Servers wird in die Klassen „Niedrig“, „Mittel“ und „Hoch“ klassifiziert; hierfür wird der Random Forest Classifier in [Apache Spark ML](https://spark.apache.org/docs/2.1.1/ml-guide.html) verwendet. Die Machine Learning-Verfahren und der Workflow in diesem Beispiel können auf einfache Weise auf vergleichbare Szenarien angewendet werden. 


## <a name="prerequisites"></a>Voraussetzungen

Folgende Voraussetzungen müssen zum Ausführen dieses Beispiels erfüllt sein:

* Ein [Azure-Konto](https://azure.microsoft.com/free/) (kostenlose Testversionen verfügbar).
* Eine gemäß dem [Schnellstart-Installationshandbuch](./quickstart-installation.md) installierte Instanz von [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) zur Installation des Programms und der Erstellung eines Arbeitsbereichs.
* In diesem Szenario wird davon ausgegangen, dass Sie Azure Machine Learning (ML) Workbench unter Windows 10 ausführen. Wenn Sie macOS verwenden, sind die Anweisungen weitgehend identisch.
* Ein virtueller Data Science-Computer (DSVM) für Linux (Ubuntu). Sie können einen Ubuntu-DSVM bereitstellen, indem Sie die entsprechenden [Anweisungen](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) befolgen. Wenn Sie einen Schnellstart wünschen, klicken Sie [hier](https://ms.portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu). Es wird empfohlen, einen virtuellen Computer mit mindestens 8 Kernen und 32 GB Arbeitsspeicher zu verwenden.  Sie benötigen IP-Adresse, Benutzername und Kennwort des DSVM, um dieses Beispiel durchzuarbeiten. Speichern Sie die folgende Tabelle mit den DSVM-Informationen für die Verwendung in späteren Schritten:

 Feldname| Wert |  
 |------------|------|
DSVM-IP-Adresse | xxx|
 Benutzername  | xxx|
 Kennwort   | xxx|

 Sie können einen beliebigen virtuellen Computer (VM) verwenden, auf dem das [Docker-Modul](https://docs.docker.com/engine/) installiert ist.

* Ein HDInsight Spark-Cluster mit HDP-Version 3.6 und Spark-Version 2.1.x. Besuchen Sie [Erstellen eines Apache Spark-Clusters in Azure HDInsight] (https://docs.microsoft.com/de-de/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql), um Einzelheiten zum Erstellen von HDInsight-Clustern zu erhalten. Es empfiehlt sich, einen Cluster mit drei Workern zu verwenden, wobei jeder Worker über 16 Kerne und 112 GB Arbeitsspeicher verfügt. Sie können jedoch auch einfach den VM-Typ „`D12 V2`“ für den Hauptknoten und „`D14 V2`“ für den Workerknoten auswählen. Die Bereitstellung des Clusters dauert etwa 20 Minuten. Sie benötigen Clustername, SSH-Benutzername und -Kennwort, um dieses Beispiel durchzuarbeiten. Speichern Sie die folgende Tabelle mit den Azure HDInsight-Clusterinformationen für die Verwendung in späteren Schritten:

 Feldname| Wert |  
 |------------|------|
 Clustername| xxx|
 Benutzername  | xxx (standardmäßig lautet dieser „sshuser“)|
 Kennwort   | xxx|


* Azure Storage-Konto Befolgen Sie die [Anweisungen](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) zum Erstellen eines Azure-Speicherkontos. Erstellen Sie außerdem zwei private Blobcontainer mit den Namen „`fullmodel`“ und „`onemonthmodel`“ in diesem Speicherkonto. Im Speicherkonto werden Compute-Zwischenergebnisse und Machine Learning-Modelle gespeichert. Sie benötigen den Speicherkontonamen und den Zugriffsschlüssel, um dieses Beispiel durchzuarbeiten. Speichern Sie die folgende Tabelle mit den Azure-Speicherkontoinformationen für die Verwendung in späteren Schritten:

 Feldname| Wert |  
 |------------|------|
 Speicherkontoname| xxx|
 Zugriffsschüssel  | xxx|


Der Ubuntu-DSVM und der Azure HDInsight-Cluster, die gemäß der Liste der Voraussetzungen erstellt wurden, sind Compute-Ziele. Compute-Ziele sind die Compute-Ressourcen im Kontext von Azure ML Workbench, die sich von dem Computer unterscheiden können, auf dem Azure ML Workbench ausgeführt wird.   

## <a name="create-a-new-workbench-project"></a>Erstellen eines neuen Workbench-Projekts

Erstellen Sie ein neues Projekt mit diesem Beispiel als Vorlage:
1.  Öffnen Sie Azure Machine Learning Workbench.
2.  Klicken Sie auf der Seite **Projekte** auf **+**, wählen Sie **Neues Projekt** aus, und signieren Sie es.
3.  Geben Sie im Bereich **Neues Projekt erstellen** die Informationen für das neue Projekt ein.
4.  Geben Sie im Suchfeld **Projektvorlagen suchen** den Suchtext „Workload Forecasting on Terabytes Data“ ein, und wählen Sie die Vorlage aus.
5.  Klicken Sie auf **Erstellen**

Sie können ein Azure ML Workbench-Projekt mit einem vorab erstellten Git-Repository erstellen; befolgen Sie dazu diese [Anweisungen](./tutorial-classifying-iris-part-1.md).  
Rufen Sie den Git-Status ab, um den Status der Dateien für die Versionsverfolgung zu überprüfen.

## <a name="data-description"></a>Datenbeschreibung

Die Daten in diesem Szenario sind neu bestimmte Serverworkloaddaten, die in einem öffentlich zugänglichen Azure Blob Storage-Konto gehostet werden. Die konkreten Speicherkontoinformationen finden Sie im Feld `dataFile` von [`Config/storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json). Sie können die Daten direkt aus Azure Blob Storage verwenden. Wenn der Speicher von vielen Benutzern gleichzeitig verwendet wird, können Sie auch [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) verwenden, um die Daten in Ihren eigenen Speicher herunterzuladen. 

Die Gesamtgröße der Daten beträgt etwa 1 TB. Jede Datei ist ca. 1 bis 3 GB groß und liegt im CSV-Dateiformat ohne Header vor. Jede Datenzeile stellt die Last einer Transaktion auf einen bestimmten Server dar.  Die ausführlichen Informationen des Datenschemas lauten wie folgt:

Spaltennummer | Feldname| Typ | Beschreibung |  
|------------|------|-------------|---------------|
1  | `SessionStart` | Datetime |    Startzeit der Sitzung
2  |`SessionEnd`    | Datetime | Endzeit der Sitzung
3 |`ConcurrentConnectionCounts` | Integer | Anzahl der gleichzeitigen Verbindungen
4 | `MbytesTransferred` | Doppelt | Übertragene normalisierte Daten (in MB)
5 | `ServiceGrade` | Integer |  Dienstqualität für die Sitzung
6 | `HTTP1` | Integer|  Angabe, ob die Sitzung HTTP1 oder HTTP2 verwendet
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



Beachten Sie Folgendes: Die erwarteten Datentypen werden zwar in der vorstehenden Tabelle aufgeführt, aufgrund fehlender Werte und Probleme mit fehlerhaften Daten besteht jedoch keine Garantie, dass die Datentypen den Erwartungen entsprechen, und beim Verarbeiten der Daten ist dies zu berücksichtigen. 


## <a name="scenario-structure"></a>Struktur des Szenarios

Die Dateien in diesem Beispiel sind wie folgt organisiert.

| Dateiname | Typ | Beschreibung |
|-----------|------|-------------|
| `Code` | Ordner | Der Ordner enthält den gesamten Code im Beispiel |
| `Config` | Ordner | Der Ordner enthält die Konfigurationsdateien |
| `Image` | Ordner | Der Ordner zum Speichern von Bildern für die Infodatei |
| `Model` | Ordner | Der Ordner zum Speichern von Modelldateien, die aus Azure Blob Storage heruntergeladen werden |
| `Code/etl.py` | Python-Datei | Die Python-Datei für Datenvorbereitung und Featureentwicklung |
| `Code/train.py` | Python-Datei | Die Python-Datei zum Trainieren eines Drei-Klassen-Modells mit Mehrfachklassifizierung  |
| `Code/webservice.py` | Python-Datei | Die Python-Datei für die Operationalisierung  |
| `Code/scoring_webservice.py` | Python-Datei |  Die Python-Datei für die Datentransformation und das Aufrufen des Webdiensts |
| `Code/O16Npreprocessing.py` | Python-Datei | Die Python-Datei zum Vorverarbeiten der Daten für „scoring_webservice.py“  |
| `Code/util.py` | Python-Datei | Die Python-Datei, die Code zum Lesen und Schreiben von Azure-Blobs enthält  
| `Config/storageconfig.json` | JSON-Datei | Die Konfigurationsdatei für den Azure-Blobcontainer, in dem Zwischenergebnisse und Modelle für das Verarbeiten von und Trainieren anhand von Ein-Monats-Daten gespeichert werden |
| `Config/fulldata_storageconfig.json` | JSON-Datei |  Die Konfigurationsdatei für den Azure-Blobcontainer, in dem Zwischenergebnisse und Modelle für das Verarbeiten des und Trainieren anhand des kompletten Datasets gespeichert werden|
| `Config/webservice.json` | JSON-Datei | Die Konfigurationsdatei für „scoring_webservice.py“|
| `Config/conda_dependencies.yml` | YAML-Datei | Die Conda-Abhängigkeitsdatei |
| `Config/conda_dependencies_webservice.yml` | YAML-Datei | Die Conda-Abhängigkeitsdatei für den Webdienst|
| `Config/dsvm_spark_dependencies.yml` | YAML-Datei | Die Spark-Abhängigkeitsdatei für den Ubuntu-DSVM |
| `Config/hdi_spark_dependencies.yml` | YAML-Datei | Die Spark-Abhängigkeitsdatei für den HDInsight Spark-Cluster |
| `README.md` | Markdowndatei | Die Info-Markdowndatei |
| `Code/download_model.py` | Python-Datei | Die Python-Datei zum Herunterladen der Modelldateien aus Azure-Blob auf den lokalen Datenträger |
| `Docs/DownloadModelsFromBlob.md` | Markdowndatei | Die Markdowndatei, die Anweisungen zum Ausführen von `Code/download_model.py` enthält |



### <a name="data-flow"></a>Datenfluss

Der Code in [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) lädt Daten aus dem öffentlich zugänglichen Container (Feld `dataFile` von [`Config/storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json)). Er umfasst Datenvorbereitung und Featureentwicklung und speichert die Compute-Zwischenergebnisse Modelle in Ihrem privaten Container. Der Code in [`Code/train.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/train.py) lädt die Compute-Zwischenergebnisse vom privaten Computer, trainiert das Klassifizierungsmodell mit mehreren Klassen und schreibt schließlich das trainierte Machine Learning-Modell in den privaten Container. Es empfiehlt sich, einen Container für Experimente mit dem Ein-Monats-Dataset und einen anderen Container für Experimente mit dem kompletten Dataset zu verwenden. Da die Daten und Modelle als Parquet-Datei gespeichert werden, stellt jede Datei eigentlich einen Ordner im Container dar, der mehrere Blobs enthält. Der erhaltene Container sieht wie folgt aus:

| Blob-Präfixname | Typ | Beschreibung |
|-----------|------|-------------|
| featureScaleModel | Parquet | Standardskalierungsmodell für numerische Features |
| stringIndexModel | Parquet | Zeichenfolgen-Indexermodell für nicht numerische Features|
| oneHotEncoderModel|Parquet | One-Hot-Encodermodell für kategoriale Features |
| mlModel | Parquet | Trainiertes Machine Learning-Modell |
| info| Python-Pickle-Datei | Informationen zu den transformierten Daten, einschließlich von Start des Trainings, Ende des Trainings, Dauer, Zeitstempel für die Training-Test-Teilung und Spalten zum Indizieren und One-Hot-Codieren.

Alle Dateien/Blobs in der obigen Tabelle werden für die Operationalisierung verwendet.


### <a name="model-development"></a>Entwickeln der Modelle

#### <a name="architecture-diagram"></a>Architekturdiagramm


Das folgende Diagramm zeigt den End-to-End-Workflow für die Verwendung von Azure ML Workbench zum Entwickeln des Modells: ![Architektur](media/scenario-big-data/architecture.PNG)



Im Folgenden wird die Modellentwicklung unter Verwendung der Remote-Computezielfunktionen in Azure ML Workbench veranschaulicht. Zunächst wird eine kleine Stichprobe der Daten geladen, und das Skript [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) wird zur schnellen Iteration auf einem Ubuntu-DSVM ausgeführt. Die in [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) ausgeführten Arbeiten können weiter beschränkt werden, indem ein zusätzliches Argument für die schnellere Iteration übergeben. Schließlich führen wir das Training mit kompletten Daten mit einem HDInsight-Cluster aus.     

Die Datei [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) lädt die Daten und führt Datenvorbereitung sowie Featureentwicklung aus. Sie akzeptiert zwei Argumente: (1) eine Konfigurationsdatei für den Azure Blob Storage-Container zum Speichern der Compute-Zwischenergebnisse und Modelle, (2) Debug-Konfiguration für eine schnellere Iteration.

Das erste Argument `configFilename` ist eine lokale Konfigurationsdatei, in der Sie die Azure Blob Storage-Informationen speichern und angeben, wo die Daten geladen werden sollen. In der Standardeinstellung ist dies die Datei [`Config/storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/storageconfig.json), und sie wird im Ein-Monats-Datendurchlauf verwendet. Zudem schließen wir die Datei [`Config/fulldata_storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) ein, die Sie zur Ausführung des kompletten Datasets verwenden müssen. Die Konfiguration hat folgenden Inhalt: 

| Feld | Typ | Beschreibung |
|-----------|------|-------------|
| storageAccount | String | Azure Storage-Kontoname |
| storageContainer | String | Container im Azure Storage-Konto zum Speichern von Zwischenergebnissen |
| storageKey | String |Zugriffsschlüssel für das Azure Storage-Konto |
| dataFile|String | Datenquellendateien  |
| duration| String | Dauer der Daten in den Datenquelldateien|

Ändern Sie sowohl `Config/storageconfig.json` als auch `Config/fulldata_storageconfig.json`, um das Speicherkonto, den Speicherschlüssel und den Blobcontainer zum Speichern der Zwischenergebnisse zu konfigurieren. In der Standardeinstellung ist der Blobcontainer für einen Ein-Monats-Datendurchlauf „`onemonthmodel`“, und der Blobcontainer für die Ausführung des kompletten Datasets ist „`fullmodel`“. Sie müssen diese beiden Container in Ihrem Speicherkonto erstellen. Mit dem Feld `"dataFile"` in [`Config/fulldata_storageconfig.json`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) wird konfiguriert, welche Daten in [`Code/etl.py`](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) geladen werden, während mit `"duration"` der von den Daten eingeschlossene Bereich konfiguriert wird. Wenn die Dauer auf 'ONE_MONTH' festgelegt ist, sollten sich die geladenen Daten in nur einer CSV-Datei der sieben Dateien mit Daten für Juni 2016 befinden. Ist die Dauer hingegen auf 'FULL' festgelegt, wird das komplette Dataset mit einer Größe von 1 TB geladen. Sie müssen `"dataFile"` und `"duration"` in diesen beiden Konfigurationsdateien nicht ändern.

Das zweite Argument ist DEBUG. Wenn Sie es auf 'FILTER_IP' festlegen, wird eine schnellere Iteration ermöglicht. Die Verwendung dieses Parameters ist hilfreich, wenn Sie Ihr Skript debuggen möchten.

> [!NOTE]
> Ersetzen Sie sämtliche Variablen der Argumente in allen folgenden Befehlen durch die tatsächlichen Werte.
> 


#### <a name="model-development-on-the-docker-of-ubuntu-dsvm"></a>Modellentwicklung unter Docker auf dem Ubuntu-DSVM

#####  <a name="1-setting-up-the-compute-target-for-docker-on-ubuntu-dsvm"></a>1. Einrichten des Compute-Ziels für Docker auf dem Ubuntu-DSVM

Starten Sie die Befehlszeile aus Azure ML Workbench. Klicken Sie dazu in der linken oberen Ecke von Azure ML Workbench im Menü „Datei“ auf „Eingabeaufforderung öffnen“, und führen Sie Folgendes aus: 

```az ml computetarget attach --name dockerdsvm --address $DSVMIPaddress  --username $user --password $password --type remotedocker```

Sobald die Befehlszeile erfolgreich ausgeführt wurde, werden im Ordner „aml_config“ Ihres Projekts die folgenden zwei erstellten Dateien angezeigt:

    dockerdsvm.compute: contains the connection and configuration information for a remote execution target
    dockerdsvm.runconfig: set of run options used when executing within the Azure ML Workbench application

Navigieren Sie zu „dockerdsvm.runconfig“, und ändern Sie die Konfiguration der folgenden Felder, wie unten veranschaulicht:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/dsvm_spark_dependencies.yml

Bereiten Sie die Projektumgebung vor, indem Sie Folgendes ausführen:

```az ml experiment prepare -c dockerdsvm```


Indem Sie „PrepareEnvironment“ auf „True“ festlegen, gestatten Sie Azure ML Workbench bei jedem Senden eines Auftrags das Erstellen der Laufzeitumgebung. `Config/conda_dependencies.yml` und `Config/dsvm_spark_dependencies.yml` enthalten die Anpassung der Laufzeitumgebung. Sie können die Conda-Abhängigkeiten, die Spark-Konfiguration und die Spark-Abhängigkeiten jederzeit ändern, indem Sie diese beiden YMAL-Dateien bearbeiten. Für dieses Beispiel haben wir `azure-storage` und `azure-ml-api-sdk` als zusätzliche Python-Pakete in `Config/conda_dependencies.yml` hinzugefügt, und wir haben „`spark.default.parallelism`“, „`spark.executor.instances`“ und „`spark.executor.cores`“ usw. in `Config/dsvm_spark_dependencies.yml` hinzugefügt. 

#####  <a name="2-data-preparation-and-feature-engineering-on-dsvm-docker"></a>2. Datenvorbereitung und Featureentwicklung für DSVM-Docker

Führen Sie das Skript `etl.py` für den DSVM-Docker mit einem Debugparameter aus, der die geladenen Daten mit bestimmten IP-Adressen der Server filtert:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FILTER_IP```

Navigieren Sie zum seitlichen Bereich, und klicken Sie auf „Ausführen“, um den Ausführungsverlauf von `etl.py` anzuzeigen. Beachten Sie, dass die Ausführungsdauer etwa zwei Minuten beträgt. Wenn Sie den Code so ändern möchten, dass er neue Features enthält, liefert das Angeben von FILTER_IP als zweites Argument eine schnellere Iteration. Sie müssen diesen Schritt möglicherweise mehrmals ausführen, wenn Sie eigene Machine Learning-Probleme behandeln, um das Dataset zu untersuchen und neue Features zu erstellen. Durch benutzerdefiniertes Einschränken der zu ladenden Daten und weiteres Filtern der zu verarbeitenden Daten können Sie den Iterationsvorgang bei der Entwicklung Ihres Modells beschleunigen. Beim Experimentieren sollten Sie die Änderungen am Code regelmäßig im Git-Repository speichern.  Beachten Sie, dass wir mit dem folgenden Code in `etl.py` den Zugriff auf den privaten Container ermöglicht haben:

```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)

# attach the blob storage to the spark cluster or VM so that the storage can be accessed by the cluster or VM        
attach_storage_container(spark, storageAccount, storageKey)
```


Führen Sie nun das Skript `etl.py` für DSVM-Docker ohne den Debugparameter FILTER_IP aus.

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FALSE```

Navigieren Sie zum seitlichen Bereich, und klicken Sie auf „Ausführen“, um den Ausführungsverlauf von `etl.py` anzuzeigen. Beachten Sie, dass die Ausführungsdauer etwa vier Minuten beträgt. Das verarbeitete Ergebnis dieses Schritts wird im Container gespeichert und für das Training in „train.py“ geladen. Darüber hinaus werden auch die Zeichenfolgenindexer, Encoder-Pipelines und die Standardskalierungen im privaten Container gespeichert und bei der Operationalisierung (O16N) verwendet. 


##### <a name="3-model-training-on-dsvm-docker"></a>3. Modelltraining für DSVM-Docker

Führen Sie das Skript `train.py` für den DSVM-Docker aus:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/train.py ./Config/storageconfig.json```

In diesem Schritt werden die Compute-Zwischenergebnisse aus der Ausführung von `etl.py` geladen, und es wird ein Machine Learning-Modell trainiert. Dieser Schritt dauert etwa zwei Minuten.

Sobald Sie die Experimente mit der kleinen Datenmenge erfolgreich abgeschlossen haben, können Sie mit den Experimenten am kompletten Dataset fortfahren. Zunächst können Sie denselben Code verwenden und dann mit geänderten Argumenten und Compute-Zielen experimentieren.  

####  <a name="model-development-on-the-hdinsight-cluster"></a>Modellentwicklung für den HDInsight-Cluster

##### <a name="1-create-compute-target-in-azure-ml-workbench-for-the-hdinsight-cluster"></a>1. Erstellen des Compute-Ziels in Azure ML Workbench für den HDInsight-Cluster

```az ml computetarget attach --name myhdi --address $clustername-ssh.azurehdinsight.net --username $username --password $password --type cluster```

Sobald die Befehlszeile erfolgreich ausgeführt wurde, werden im Ordner „aml_config“ die folgenden zwei erstellten Dateien angezeigt:
    
    myhdo.compute: contains connection and configuration information for a remote execution target
    myhdi.runconfig: set of run options used when executing within the Azure ML Workbench application


Navigieren Sie zu „myhdi.runconfig“, und ändern Sie die Konfiguration der folgenden Felder, wie unten veranschaulicht:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/hdi_spark_dependencies.yml

Bereiten Sie die Projektumgebung vor, indem Sie Folgendes ausführen:

```az ml experiment prepare -c myhdi```

Dieser Schritt kann bis zu sieben Minuten dauern.

##### <a name="2-data-preparation-and-feature-engineering-on-hdinsight-cluster"></a>2. Datenvorbereitung und Featureentwicklung für den HDInsight-Cluster

Führen Sie das Skript `etl.py` mit „fulldata“ für den HDInsight-Cluster aus.

```az ml experiment submit -a -t myhdi -c myhdi ./Code/etl.py Config/fulldata_storageconfig.json FALSE```

Da dieser Auftrag relativ lange (etwa zwei Stunden) dauert, können wir das Ausgabestreaming mit „-a“ deaktivieren. Wenn der Auftrag abgeschlossen ist, können Sie im „Ausführungsverlauf“ das Treiberprotokoll und auch das Controllerprotokoll einsehen. Wenn Sie über einen größeren Cluster verfügen, können Sie die Konfigurationen in `Config/hdi_spark_dependencies.yml` jederzeit so ändern, dass eine größere Anzahl von Instanzen oder eine größere Anzahl von Kernen verwendet wird. Bei einem Cluster mit vier Workerknoten beispielsweise können Sie den Wert von „`spark.executor.instances`“ von 5 auf 7 heraufsetzen. Die Ausgabe dieses Schritts können Sie im Container „fullmodel“ Ihres Speicherkontos einsehen. 


##### <a name="3-model-training-on-hdinsight-cluster"></a>3. Modelltraining für den HDInsight-Cluster

Führen Sie das Skript `train.py` für den HDInsight-Cluster aus:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/train.py Config/fulldata_storageconfig.json```

Da dieser Auftrag relativ lange (etwa eine halbe Stunde) dauert, können wir das Ausgabestreaming mit „-a“ deaktivieren.

#### <a name="run-history-exploration"></a>Untersuchen des Ausführungsverlaufs

Der Ausführungsverlauf ist ein Feature, mit dem Sie Ihre Experimente in Azure ML Workbench nachverfolgen können. In der Standardeinstellung wird damit die Dauer der Experimente verfolgt. Wenn wir in unserem konkreten Beispiel in den Experimenten zum kompletten Dataset für „`Code/etl.py`“ wechseln, stellen wir fest, dass sich die Dauer erheblich verlängert. Sie können auch bestimmte Metriken für die Nachverfolgung aufzeichnen. Um die Verfolgung von Metriken zu aktivieren, fügen Sie dem Kopf der Python-Datei die folgenden Codezeilen hinzu:
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

Navigieren Sie in der rechten Seitenleiste von Azure ML Workbench zu „Ausführungen“, um den Ausführungsverlauf für jede Python-Datei anzuzeigen. Wechseln Sie darüber hinaus zu Ihrem GitHub-Repository. Dort wird ein neuer Branch erstellt, dessen Name mit „AMLHistory“ beginnt und mit dem die Änderungen verfolgt werden, die Sie in jeder Ausführung am Skript vorgenommen haben. 


### <a name="operationalization"></a>Operationalisierung

In diesem Abschnitt operationalisieren wir das in den obigen Schritten erstellte Modell als Webdienst. Außerdem wird veranschaulicht, wie mithilfe des Webdiensts die Workload prognostiziert werden kann. Wir verwenden Azure ML-CLIs (Befehlszeilenschnittstellen) für die Operationalisierung, um den Code und die Abhängigkeiten als Docker-Images zu packen und das Modell als Containerwebdienst zu veröffentlichen. Weitere Einzelheiten erhalten Sie in der [Übersicht über die Operationalisierung](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/operationalization-overview.md). Über die Eingabeaufforderung in Azure ML Workbench können Sie die Azure ML-CLIs für die Operationalisierung ausführen.  Sie können auch die Azure ML-CLIs für die Operationalisierung für Ubuntu Linux ausführen, indem Sie die Anweisungen im [Installationshandbuch](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/install-on-ubuntu-linux.md) befolgen. 

> [!NOTE]
> Ersetzen Sie sämtliche Variablen der Argumente in allen folgenden Befehlen durch die tatsächlichen Werte. Das Durcharbeiten dieses Abschnitts dauert ca. 40 Minuten.
> 


Wählen Sie eine eindeutige Zeichenfolge als Umgebung für die Operationalisierung. Wir verwenden hier die Zeichenfolge „[unique]“, um die von Ihnen gewählte Zeichenfolge darzustellen.

1. Erstellen Sie die Umgebung für die Operationalisierung, und erstellen Sie die Ressourcengruppe.

        az ml env setup -c -n [unique] --location eastus2 --cluster -z 5 --yes

   Beachten Sie, dass wir uns für die Verwendung von Azure Container Service als Umgebung entschieden haben. Dazu wurde `--cluster` im Befehl `az ml env setup` angegeben. Wir operationalisieren das Machine Learning-Modell für [Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-intro-kubernetes), da dort [Kubernetes](https://kubernetes.io/) zum Automatisieren der Bereitstellung, Skalierung und Verwaltung von Containeranwendungen verwendet wird. Die Ausführung dieses Befehls dauert etwa 20 Minuten. Verwenden Sie 

        az ml env show -g [unique]rg -n [unique]

   um zu überprüfen, ob die Bereitstellung erfolgreich abgeschlossen wurde.

   Legen Sie die Bereitstellungsumgebung auf die soeben erstellte Umgebung fest, indem Sie Folgendes ausführen:

        az ml env set -g [unique]rg -n [unique]

2. Erstellen Sie und verwenden Sie ein Modellverwaltungskonto.

   Erstellen Sie ein Modellverwaltungskonto, indem Sie Folgendes ausführen:

    az ml account modelmanagement create --location  eastus2 -n [unique]acc -g [unique]rg --sku-instances 4 --sku-name S3 

   Verwenden Sie die Modellverwaltung für die Operationalisierung, indem Sie Folgendes ausführen:

        az ml account modelmanagement set  -n [unique]acc -g [unique]rg  

   Mit dem Modellverwaltungskonto werden die Modelle und Webdienste verwaltet. Im Azure-Portal stellen Sie fest, dass ein neues Modellverwaltungskonto erstellt wurde, und Sie können damit die Modelle, Manifeste, Docker-Images und Dienste anzeigen, die unter diesem Modellverwaltungskonto erstellt wurden.

3. Laden Sie die Modelle herunter, und registrieren Sie sie.

   Laden Sie die Modelle auf Ihren lokalen Computer in das Codeverzeichnis in den Container „fullmodel“ herunter. Laden Sie nicht die Parquet-Datendatei mit dem Namen „vmlSource.parquet“ herunter, da es sich hierbei um keine Modelldatei, sondern um ein Compute-Zwischenergebnis handelt. Sie können auch die Modelldateien wiederverwenden, die wir in das Git-Repository eingeschlossen haben. Einzelheiten zum Herunterladen der Parquet-Dateien erhalten Sie in [DownloadModelsFromBlob.md](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Docs/DownloadModelsFromBlob.md). 

   Wechseln Sie in der CLI zum Ordner `Model`, und registrieren Sie die Modelle wie folgt:

        az ml model register -m  mlModel -n vmlModel -t fullmodel
        az ml model register -m  featureScaleModel -n featureScaleModel -t fullmodel
        az ml model register -m  oneHotEncoderModel -n  oneHotEncoderModel -t fullmodel
        az ml model register -m  stringIndexModel -n stringIndexModel -t fullmodel
        az ml model register -m  info -n info -t fullmodel

   Die Ausgabe jedes Befehls liefert eine Modell-ID, die im nächsten Schritt benötigt wird. Speichern Sie diese in einer Textdatei für die künftige Verwendung.

4. Erstellen Sie ein Manifest für den Webdienst.

   Das Manifest ist das Rezept, mit dem das Docker-Image für Webdienstcontainer erstellt wird. Es enthält den Code für den Webdienst sowie sämtliche Machine Learning-Modelle und Laufzeitumgebungsabhängigkeiten.  Wechseln Sie in der CLI zum Ordner `Code`, und führen Sie diese Befehlszeile aus:

        az ml manifest create -n $webserviceName -f webservice.py -r spark-py -c ../Config/conda_dependencies_webservice.yml -i $modelID1 -i $modelID2 -i $modelID3 -i $modelID4 -i $modelID5

   Die Ausgabe liefert eine Manifest-ID für den nächsten Schritt. 

   Bleiben Sie im Verzeichnis `Code`, und testen Sie „webservice.py“, indem Sie Folgendes ausführen: 

        az ml experiment submit -t dockerdsvm -c dockerdsvm webservice.py

5. Erstellen Sie ein Docker-Image. 

        az ml image create -n [unique]image --manifest-id $manifestID

   Die Ausgabe liefert eine Image-ID für den nächsten Schritt, da dieses Docker-Image in ACS verwendet wird. 

6. Bereitstellen des Webdiensts für den ACS-Cluster

        az ml service create realtime -n [unique] --image-id $imageID --cpu 0.5 --memory 2G

   Die Ausgabe liefert eine Dienst-ID, und Sie müssen damit den Autorisierungsschlüssel und die Dienst-URL abrufen.

7. Rufen Sie den Webdienst in Python-Code auf, um Mini-Batches zu bewerten.

   Rufen Sie den Autorisierungsschlüssel mit dem folgenden Befehl ab:

         az ml service keys realtime -i $ServiceID 

   und rufen Sie die Dienstbewertungs-URL mit dem folgenden Befehl ab:

        az ml service usage realtime -i $ServiceID

   Ändern Sie den Inhalt in `./Config/webservice.json` mit der richtigen Dienstbewertungs-URL und dem richtigen Autorisierungsschlüssel (behalten Sie den „Bearer“ in der ursprünglichen Datei bei, und ersetzen Sie den Teil „xxx“). 
   
   Wechseln Sie in das Stammverzeichnis des Projekts, und testen Sie den Webdienst auf Mini-Batch-Bewertungen. Verwenden Sie dazu

        az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/scoring_webservice.py ./Config/webservice.json

8. Skalieren Sie den Webdienst. 

   Informationen zum Skalieren des Webdiensts finden Sie unter [How to scale operationalization on your ACS cluster](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/how-to-scale.md) (Skalieren der Operationalisierung für den ACS-Cluster).
 

## <a name="conclusion"></a>Zusammenfassung

In diesem Beispiel wurde beschrieben, wie mithilfe von Azure ML Workbench ein Machine Learning-Modell für Big Data trainiert und das trainierte Modell operationalisiert wird. Insbesondere wurden folgende Vorgänge erläutert:

* Konfigurieren und Verwenden unterschiedlicher Compute-Ziele.

* Ausführungsverlauf der Verfolgung von Metriken und unterschiedlichen Durchläufen.

* Operationalisierung.

Benutzer können den Code erweitern, um die Kreuzvalidierung und das Optimieren von Hyperparametern zu untersuchen. Weitere Informationen zur Kreuzvalidierung und zum Optimieren von Hyperparametern finden Sie auf der Seite https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning.  
Weitere Informationen zu Zeitreihenprognosen finden Sie auf der Seite https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting.
