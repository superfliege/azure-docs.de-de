---
title: "Verwenden des Features „Modelldatensammlung“ in Azure Machine Learning Workbench | Microsoft Docs"
description: "In diesem Artikel wird die Verwendung des Features „Modelldatensammlung“ in Azure Machine Learning Workbench beschrieben."
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
ms.openlocfilehash: 6a40a85426d2be72fa688548f7ab30e7e5f92146
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="collect-model-data-by-using-data-collection"></a>Sammeln von Modelldaten mithilfe der Datensammlung

Mithilfe des Features „Modelldatensammlung“ in Azure Machine Learning Workbench können Sie Modelleingaben und Vorhersagen eines Machine Learning-Webdiensts archivieren.

## <a name="install-the-data-collection-package"></a>Installieren des Datensammlungspakets
Sie können die Datensammlungsbibliothek nativ unter Linux und Windows installieren.

### <a name="windows"></a>Windows
Unter Windows installieren Sie das Datensammlungsmodul mit dem folgendem Befehl:

    pip install azureml.datacollector

### <a name="linux"></a>Linux
Installieren Sie unter Linux zunächst die libxml++-Bibliothek. Führen Sie den folgenden Befehl aus, der unter „sudo“ ausgegeben werden muss:

    sudo apt-get install libxml++2.6-2v5

Führen Sie dann den folgenden Befehl aus:

    pip install azureml.datacollector

## <a name="collect-data"></a>Sammeln von Daten

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

4. Verwenden Sie den `az ml service create realtime`-Befehl mit dem `--collect-model-data true`-Schalter, um einen Echtzeit-Webdienst zu erstellen. Dieser Schritt stellt sicher, dass die Modelldaten abgerufen werden, wenn der Dienst ausgeführt wird.

     ```batch
    c:\temp\myIris> az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
    ```
    
5. Um die Datensammlung zu testen, führen Sie den Befehl `az ml service run realtime` aus:

    ```
    C:\Temp\myIris> az ml service run realtime -i irisapp -d "ADD YOUR INPUT DATA HERE!!" 
    ``` 
    
## <a name="view-the-collected-data"></a>Anzeigen der gesammelten Daten
So zeigen Sie die gesammelten Daten im BLOB-Speicher an:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Weitere Dienste** aus.
3. Geben Sie im Suchfeld **Speicherkonten** ein, und drücken Sie die EINGABETASTE.
4. Wählen Sie auf dem Suchblatt **Speicherkonten** die Ressource **Speicherkonto** aus. Verwenden Sie die folgenden Schritte, um Ihr Speicherkonto zu ermitteln:

    a. Navigieren Sie zu Azure Machine Learning Workbench, wählen Sie das Projekt aus, an dem Sie arbeiten, und öffnen Sie eine Eingabeaufforderung im Menü **Datei**.
    
    b. Geben Sie `az ml env show -v` ein, und überprüfen Sie den Wert für *storage_account*. Dies ist der Name Ihres Speicherkontos.

5. Wählen Sie **Containers** im Ressourcenblattmenü aus, und klicken Sie dann auf den Container namens **modeldata**. Sie müssen nach der ersten Webdienstanforderung unter Umständen bis zu 10 Minuten warten, bis das Speicherkonto mit Daten aufgefüllt wird. Die Daten fließen in Blobs mit dem folgenden Containerpfad:

    `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

Daten aus Azure-Blobs können auf verschiedene Arten sowohl mit Microsoft-Software als auch Open Source-Tools genutzt werden. Hier einige Beispiele:
- Azure Machine Learning Workbench: Öffnen Sie die CSV-Datei in Azure Machine Learning Workbench, indem Sie sie als Datenquelle hinzufügen.
- Excel: Öffnen Sie die täglichen CSV-Dateien als Tabelle.
- [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/): Erstellen Sie Diagramme, für die Daten aus den CSV-Daten in Blobs genutzt werden.
- [Spark](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-overview): Erstellen Sie einen Datenrahmen mit einem hohen Anteil von CSV-Daten.
    ```python
    var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
    ```
- [Hive](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): Laden Sie CSV-Daten in eine Hive-Tabelle, und führen Sie SQL-Abfragen direkt für das Blob durch.

