---
title: Verwenden des Features Model Management Data Collection in Azure Machine Learning Workbench | Microsoft-Dokumentation
description: In diesem Dokument wird die Verwendung des Features Model Management Data Collection in Azure Machine Learning Workbench beschrieben.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: e751e2f0ec812de43a03749e04ff165fa62ec649
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-collect-model-data-using-data-collection"></a>Sammeln von Modelldaten mithilfe der Datensammlung

Mithilfe des Modelldatensammlungs-Features können Sie Modelleingaben und Vorhersagen eines Machine Learning-Webdiensts archivieren. Dieses Dokument informiert Sie über die folgenden Aspekte der Modelldatensammlung:

- Installieren des Datensammlungspakets
- Verwenden der Datensammlung
- Anzeigen und Nutzen der gesammelten Daten

## <a name="how-to-install-the-data-collection-package"></a>Installieren des Datensammlungspakets
Die Datensammlungsbibliothek kann nativ unter Linux und Windows installiert werden.

### <a name="windows"></a>Windows
Unter Windows kann das Datensammlungsmodul mit folgendem Befehl installiert werden:

    pip install azureml.datacollector

### <a name="linux"></a>Linux
Unter Linux muss zunächst die libxml++-Bibliothek installiert werden. Führen Sie den folgenden Befehl aus, der unter „sudo“ ausgegeben werden muss:

    sudo apt-get install libxml++2.6-2v5

Geben Sie dann folgenden Befehl aus:

    pip install azureml.datacollector
## <a name="how-to-use-data-collection"></a>Verwenden der Datensammlung

Um die Modelldatensammlung zu verwenden, müssen Sie die folgenden Änderungen an der Bewertungsdatei vornehmen:

1. Fügen Sie den folgenden Code am Anfang der Datei hinzu:
   
    ```python
    from azureml.datacollector import ModelDataCollector
    ```

2. Fügen Sie die folgenden Codezeilen der `init()`-Funktion hinzu:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

3. Fügen Sie die folgenden Codezeilen der `run(input_df)`-Funktion hinzu:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc.collect(input_df)
    prediction_dc.collect(pred)
    ```

    Stellen Sie sicher, dass die Variablen `input_df` und `pred` (Vorhersagewert aus `model.predict()`) initialisiert werden, bevor Sie die `collect()`-Funktion für sie aufrufen.

4. Verwenden Sie den `az ml service create realtime`-Befehl mit dem `--collect-model-data true`-Schalter, um einen Echtzeit-Webdienst zu erstellen. Dadurch wird sichergestellt, dass die Modelldaten abgerufen werden, wenn der Dienst ausgeführt wird.

     ```batch
    c:\temp\myIris> az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
    ```
    
5. Um die Datensammlung zu testen, führen Sie `az ml service run realtime` aus.

    ```
    C:\Temp\myIris> az ml service run realtime -i irisapp -d "ADD YOUR INPUT DATA HERE!!" 
    ``` 
    
## <a name="how-to-view-and-consume-the-collected-data"></a>Anzeigen und Nutzen der gesammelten Daten
So zeigen Sie die gesammelten Daten im Blobspeicher an:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Weitere Dienste**.
3. Geben Sie im Suchfeld `Storage accounts` ein, und drücken Sie die **EINGABETASTE**.
4. Wählen Sie auf dem Suchblatt **Speicherkonten** die Ressource **Speicherkonto** aus. Gehen Sie in folgenden Schritten vor, um Ihr Speicherkonto zu bestimmen:

    a. Navigieren Sie zu Azure ML Workbench, wählen Sie das Projekt aus, an dem Sie arbeiten, und öffnen Sie die Eingabeaufforderung im Menü **Datei**.
    
    b. Geben Sie `az ml env show -v` ein, und überprüfen Sie den Wert *storage_account*. Dies ist der Name des Speicherkontos.

5. Klicken Sie im Ressourcenblattmenü auf **Containers**, und klicken Sie dann auf den Container **modeldata**. Sie müssen nach der ersten Webdienstanforderung unter Umständen bis zu 10 Minuten warten, bis die Daten unter dem Speicherkonto sichtbar werden. Die Daten fließen in Blobs mit dem folgenden Containerpfad:

    `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

Daten aus Azure-Blobs können auf verschiedene Arten sowohl mit Microsoft-Software als auch Open Source-Tools genutzt werden. Hier einige Beispiele:
 - Azure ML Workbench: Öffnen Sie die CSV-Datei in Azure ML Workbench, indem Sie sie als Datenquelle hinzufügen.
 - Excel: Öffnen Sie die täglichen CSV-Dateien als Tabelle.
 - [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/): Erstellen Sie Diagramme, für die Daten aus den CSV-Daten in Blobs genutzt werden.
 - [Spark](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-overview): Erstellen Sie einen Datenframe mit einem hohen Anteil von CSV-Daten.
    ```python
    var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
    ```
- [Hive](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): Laden Sie CSV-Daten in eine Hive-Tabelle, und führen Sie SQL-Abfragen direkt für ein Blob durch.

