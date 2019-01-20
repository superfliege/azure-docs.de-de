---
title: Neuerungen im Release
titleSuffix: Azure Machine Learning service
description: Erfahren Sie mehr über die neuesten Updates des Azure Machine Learning Service und Python SDKs für maschinelles Lernen und Datenaufbereitung.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
author: hning86
ms.author: haining
ms.reviewer: j-martens
ms.date: 12/20/2018
ms.custom: seodec18
ms.openlocfilehash: 5341c4901ca2a7aa0b4935e13d06c8fb5a1f0d1b
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2019
ms.locfileid: "54304095"
---
# <a name="azure-machine-learning-service-release-notes"></a>Azure Machine Learning-Dienst – Anmerkungen zu dieser Version

Erfahren Sie in diesem Artikel mehr über die Versionen des Azure Machine Learning-Diensts. 

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Azure Machine Learning SDK für Python v1.0.8

+ **SDK-Referenzdokumente**: https://aka.ms/aml-sdk

+ **Programmfehlerbehebungen**: Dieses Release enthält hauptsächlich kleinere Programmfehlerbehebungen.

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning Data Prep SDK v1.0.7

+ **SDK-Referenzdokumente**: https://aka.ms/data-prep-sdk

+ **Neue Features**
  + Datenspeicherverbesserungen (dokumentiert in der [Datenspeicher-Anleitung](https://github.com/Microsoft/AMLDataPrepDocs/tree/master/how-to-guides/datastore.ipynb))
    + Möglichkeit zum Auslesen von und Schreiben in eine Azure-Dateifreigabe sowie in ADLS-Datenspeicher beim Hochskalieren hinzugefügt.
    + Bei der Verwendung von Datenspeichern unterstützt die Datenvorbereitung jetzt die Verwendung von Dienstprinzipalauthentifizierung anstelle von interaktiver Authentifizierung.
    + Unterstützung für wasb- und wasbs-URLs hinzugefügt.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning Data Prep SDK v1.0.6

+ **SDK-Referenzdokumente**: https://aka.ms/data-prep-sdk

+ **Fehlerbehebungen**
  + Korrektur eines Fehlers beim Lesen von öffentlichen lesbaren Azure-Blobcontainern in Spark

## <a name="2018-12-20"></a>2018-12-20 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Azure Machine Learning SDK für Python v1.0.6

+ **SDK-Referenzdokumente**: https://aka.ms/aml-sdk

+ **Programmfehlerbehebungen**: Dieses Release enthält hauptsächlich kleinere Programmfehlerbehebungen.

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning Data Prep SDK v1.0.4

+ **SDK-Referenzdokumente**: https://aka.ms/data-prep-sdk

+ **Neue Features**
  + Die `to_bool`-Funktion ermöglicht nun die Konvertierung nicht übereinstimmender Werte in Fehlerwerte. Dies ist das neue Standardverhalten bei Nichtübereinstimmung für `to_bool` und `set_column_types`, während das bisherige Standardverhalten darin bestand, nicht übereinstimmende Werte in FALSE zu konvertieren.
  + Beim Aufruf von `to_pandas_dataframe` gibt es eine neue Option, um NULL- bzw. fehlende Werte in numerischen Spalten als NaN zu interpretieren.
  + Die Möglichkeit, den Rückgabetyp einiger Ausdrücke zu überprüfen, um die Typkonsistenz zu gewährleisten und frühzeitig fehlzuschlagen, wurde hinzugefügt.
  + Sie können nun `parse_json` aufrufen, um Werte in einer Spalte als JSON-Objekte zu analysieren und in mehreren Spalten zu erweitern.

+ **Fehlerbehebungen**
  + Programmfehler korrigiert, der in Python 3.5.2 für den Absturz von `set_column_types` sorgte.
  + Programmfehler korrigiert, der beim Herstellen einer Verbindung mit dem Datenspeicher über ein AML-Bild einen Absturz verursachte.

+ **Updates**
  * [Beispiel-Notebooks](https://aka.ms/aml-data-prep-notebooks) mit Tutorials mit den ersten Schritten, Fallstudien und Leitfäden.

## <a name="2018-12-04-general-availability"></a>2018-12-04: Allgemeine Verfügbarkeit

Der Azure Machine Learning Service ist jetzt allgemein verfügbar.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning Compute
Mit diesem Release kündigen wir eine neue verwaltete Compute-Umgebung über [Azure Machine Learning Compute](how-to-set-up-training-targets.md#amlcompute) an. Dieses Computeziel ersetzt das Azure Batch AI-Computeziel für Azure Machine Learning. 

Dieses Computeziel:
+ Wird für das Modelltraining und Batchrückschlüsse verwendet
+ Ist ein Computeziel mit einem oder mehreren Knoten
+ Übernimmt die Clusterverwaltung und Auftragsplanung für den Benutzer
+ Wird standardmäßig automatische skaliert
+ Unterstützt CPU- und GPU-Ressourcen 
+ Ermöglicht zur Kostensenkung die Nutzung von VMs mit niedriger Priorität

Azure Machine Learning Compute kann unter Python, mit dem Azure-Portal oder mit der CLI erstellt werden. Die Erstellung muss in der Region Ihres Arbeitsbereichs erfolgen, und ein Anfügen an einen anderen Arbeitsbereich ist nicht möglich. Bei diesem Computeziel wird für Ihre Ausführung ein Docker-Container genutzt, und Ihre Abhängigkeiten werden gepackt, um die gleiche Umgebung auf allen Ihren Knoten zu replizieren.

> [!Warning]
> Es wird empfohlen, einen neuen Arbeitsbereich für die Nutzung von Azure Machine Learning Compute zu erstellen. Es besteht eine geringe Wahrscheinlichkeit, dass Benutzern, die Azure Machine Learning Compute aus einem vorhandenen Arbeitsbereich erstellen möchten, ein Fehler angezeigt wird. Ein vorhandener Computevorgang in Ihrem Arbeitsbereich sollte ohne Beeinträchtigungen weiterhin funktionieren.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Azure Machine Learning SDK für Python v1.0.2
+ **Wichtige Änderungen**
  + Mit diesem Release entfällt die Unterstützung für die Erstellung einer VM über Azure Machine Learning. Sie können aber weiterhin eine vorhandene Cloud-VM oder einen entfernten lokalen Server anfügen. 
  + Darüber hinaus entfällt die Unterstützung für Batch AI, da alle Funktionen jetzt per Azure Machine Learning Compute unterstützt werden sollten.

+ **Neu**
  + Für Machine Learning-Pipelines:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Aktualisiert**
  + Für Machine Learning-Pipelines:
    + Für [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) wird jetzt „runconfig“ akzeptiert.
    + Für [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) kann jetzt in eine und aus einer SQL-Datenquelle kopiert werden.
    + Planen von Funktionalität im SDK zum Erstellen und Aktualisieren von Zeitplänen für die Ausführung von veröffentlichten Pipelines

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning Data Prep SDK v0.5.2
+ **Wichtige Änderungen** 
  * `SummaryFunction.N` wurde in `SummaryFunction.Count` umbenannt.
  
+ **Fehlerbehebungen**
  * Verwenden Sie das aktuelle AML-Ausführungstoken, wenn Sie bei Remoteausführungen aus Datenspeichern lesen oder in Datenspeicher schreiben. Wenn das AML-Ausführungstoken zuvor in Python aktualisiert wird, wird die Runtime für die Datenaufbereitung nicht mit dem aktualisierten AML-Ausführungstoken aktualisiert.
  * Zusätzliche verständlichere Fehlermeldungen
  * „to_spark_dataframe()“ stürzt nicht mehr ab, wenn für Spark die `Kryo`-Serialisierung verwendet wird.
  * Für den Inspektor für die Anzahl von Werten können jetzt mehr als 1.000 eindeutige Werte angezeigt werden.
  * Für die zufällige Aufteilung tritt kein Fehler mehr auf, wenn der ursprüngliche Dataflow keinen Namen hat.  

+ **Weitere Informationen**
  * [Azure Machine Learning – Datenaufbereitungs-SDK](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Dokumentation und Notebooks
+ ML-Pipelines
  + Neue und aktualisierte Notebooks als Einstieg in Pipelines, Batchbereichsdefinition und Beispiele für die Stilübertragung: https://aka.ms/aml-pipeline-notebooks.
  + Erfahren Sie, wie Sie [Ihre erste Pipeline erstellen](how-to-create-your-first-pipeline.md).
  + Erfahren Sie, wie Sie [Batchvorhersagen mit Pipelines ausführen](how-to-run-batch-predictions.md).
+ Azure Machine Learning Compute-Ziel
  + [Beispielnotebooks](https://aka.ms/aml-notebooks) sind jetzt für die Verwendung des neuen verwalteten Computeziels aktualisiert.
  + [Informationen zu dieser Computeumgebung](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure-Portal: Neue Features
+ Erstellen und Verwalten von [Azure Machine Learning Compute](how-to-set-up-training-targets.md#amlcompute)-Typen im Portal
+ Überwachen der Kontingentnutzung und des [Anforderungskontingents](how-to-manage-quotas.md) für Azure Machine Learning Compute
+ Anzeigen des Status für den Azure Machine Learning Compute-Cluster in Echtzeit
+ Nutzen der hinzugefügten Unterstützung für virtuelle Netzwerke für Azure Machine Learning Compute und die Azure Kubernetes Service-Erstellung
+ Erneutes Ausführen Ihrer veröffentlichten Pipelines mit vorhandenen Parametern
+ Neue [automatisierte Machine Learning-Diagramme](how-to-track-experiments.md#auto) für Klassifizierungsmodelle (Prognosegüte-, Gewinn-, Kalibrierungs-, Featurewichtigkeitsdiagramm mit Modellerklärung) und Regressionsmodelle (Restdaten- und Featurewichtigkeitsdiagramm mit Modellerklärung) 
+ Pipelines können im Azure-Portal angezeigt werden




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine Learning SDK für Python v0.1.80

+ **Wichtige Änderungen** 
  * Der Namespace *azureml.train.widgets* wurde in *azureml.widgets* verschoben.
  * *azureml.core.compute.AmlCompute* führt zur Veraltung der folgenden Klassen: *azureml.core.compute.BatchAICompute* und *azureml.core.compute.DSVMCompute*. Die letztgenannte Klasse wird in künftigen Versionen nicht mehr enthalten sein. Die AmlCompute-Klasse verfügt jetzt über eine einfachere Definition und benötigt lediglich Angaben für „vm_size“ und „max_nodes“. Sie skaliert Ihren Cluster automatisch von 0 auf den Wert von „max_nodes“, wenn ein Auftrag übermittelt wird. Unsere [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) wurden mit diesen Informationen aktualisiert und enthalten Beispiele zur Verwendung. Wir hoffen, dass Ihnen diese Vereinfachung gefällt – genauso wie viele weitere interessante Features in den nachfolgenden Releases!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning Data Prep SDK v0.5.1 

Erfahren Sie mehr zum Data Prep SDK, indem Sie die [Referenzdokumente](https://aka.ms/data-prep-sdk) lesen.
+ **Neue Features**
   * Es wurde eine neue DataPrep CLI erstellt, mit der DataPrep-Pakete ausgeführt werden können und das Datenprofil für ein Dataset oder einen Dataflow angezeigt werden kann.
   * Die SetColumnType-API wurde neu gestaltet, um die Nutzbarkeit zu verbessern.
   * „smart_read_file“ wurde in „auto_read_file“ umbenannt.
   * Datenschiefe und Kurtosis sind jetzt im Datenprofil enthalten.
   * Stichprobenentnahme mit geschichteten Stichproben ist möglich.
   * Das Lesen aus ZIP-Dateien, die CSV-Dateien enthalten, ist möglich.
   * Datasets können zeilenweise per zufälliger Aufteilung geteilt werden (z. B. in Test-/Trainingssätze).
   * Alle Spaltendatentypen können aus einem Dataflow oder Datenprofil abgerufen werden, indem `.dtypes` aufgerufen wird.
   * Die Zeilenanzahl kann aus einem Dataflow oder Datenprofil abgerufen werden, indem `.row_count` aufgerufen wird.

+ **Fehlerbehebungen**
   * Fehler bei Konvertierung von „long“ in „double“ behoben 
   * Fehler bei Assert-Vorgang nach jedem Hinzufügen einer Spalte behoben 
   * FuzzyGrouping-Fehler behoben, bei dem in einigen Fällen Gruppen nicht erkannt wurden
   * Fehler bei Sortierfunktion behoben, damit die Sortierreihenfolge für mehrere Spalten beibehalten wird
   * Fehler mit Ausdrücken behoben bzw. an Verarbeitung durch `pandas` angepasst
   * Fehler beim Lesen aus dem dbfs-Pfad behoben
   * Verständlichkeit von Fehlermeldungen verbessert 
   * Fehler behoben, der beim Lesen auf dem Remotecomputeziel mit dem AML-Token aufgetreten ist
   * Fehler für Linux DSVM behoben
   * Kein Absturz mehr, wenn in Zeichenfolgenprädikaten andere Werte als Zeichenfolgen enthalten sind
   * Assertionsfehler werden bei Dataflow-Fehler jetzt richtig behandelt
   * Per dbutils bereitgestellte Speicherorte werden unter Azure Databricks jetzt unterstützt

## <a name="2018-11-05"></a>05.11.2018

### <a name="azure-portal"></a>Azure-Portal 
Im Azure-Portal für den Azure Machine Learning-Dienst wurden folgende Updates vorgenommen:
  * Neue Registerkarte namens **Pipelines** für veröffentlichte Pipelines
  * Unterstützung für das Anfügen eines vorhandenen HDInsight-Clusters als Computeziel hinzugefügt

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Azure Machine Learning SDK für Python v0.1.74

+ **Wichtige Änderungen** 
  * *Workspace.compute_targets, datastores, experiments, images, models und *webservices* sind Eigenschaften, keine Methoden. Ersetzen Sie beispielsweise *Workspace.compute_targets()* durch *Workspace.compute_targets*.
  * *Run.get_context* ersetzt *Run.get_submitted_run*. Die letztgenannte Methode wird in künftigen Versionen entfernt.
  * Die Klasse *PipelineData* erwartet nun anstelle von „datastore_name“ ein datastore-Objekt als Parameter. Analog dazu akzeptiert *Pipeline* nun „default_datastore“ anstelle von „default_datastore_name“.

+ **Neue Features**
  * Das [Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) für Azure Machine Learning-Pipelines verwendet jetzt MPI-Schritte.
  * Das RunDetails-Widget für Jupyter-Notebooks wird aktualisiert, um eine Visualisierung der Pipeline anzuzeigen.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning Data Prep SDK v0.4.0 
 
+ **Neue Features**
  * Typanzahl zum Datenprofil hinzugefügt 
  * Wertanzahl und Histogramm nun verfügbar
  * Mehr Perzentile im Datenprofil
  * Median in der Zusammenfassung verfügbar
  * Python 3.7 nun unterstützt
  * Wenn Sie einen Dataflow mit Datenspeichern in einem Datenvorbereitungspaket speichern, werden die Datenspeicherinformationen als Teil des Datenvorbereitungspakets gespeichert.
  * Schreibvorgänge für den Datenspeicher nun unterstützt 
        
+ **Behobene Fehler**
  * 64-Bit-Ganzzahlüberläufe ohne Vorzeichen werden nun unter Linux ordnungsgemäß behandelt.
  * Falscher Text für Nur-Text-Dateien in „smart_read“ behoben
  * Zeichenfolgenspaltentyp wird nun in der Metrikansicht angezeigt
  * Typanzahl korrigiert, sodass nun Wertarten (ValueKinds) angezeigt werden, die einem einzelnen Feldtyp (FieldType) zugeordnet sind (anstatt einzelne)
  * Bei „Write_to_csv“ tritt kein Fehler mehr auf, wenn der Pfad als Zeichenfolge angegeben wird.
  * Wenn bei Verwendung von „Ersetzen“ das Suchfeld leer ist, tritt kein Fehler mehr auf. 

## <a name="2018-10-12"></a>12.10.2018

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine Learning SDK für Python v0.1.68

+ **Neue Features**
  * Unterstützung für mehrere Mandanten beim Erstellen eines neuen Arbeitsbereichs.

+ **Behobene Fehler**
  * Sie müssen die Version der pynacl-Bibliothek beim Bereitstellen des Webdiensts nicht mehr anheften.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning – Datenaufbereitungs-SDK v0.3.0

+ **Neue Features**
  * Methode transform_partition_with_file(script_path) hinzugefügt, die es Benutzern ermöglicht, den Pfad einer auszuführenden Python-Datei zu übergeben

## <a name="2018-10-01"></a>01.10.2018

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine Learning SDK für Python v0.1.65
[Version 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) enthält neue Features, mehr Dokumentation, Fehlerbehebungen und weitere [Beispielnotebooks](https://aka.ms/aml-notebooks).

Sehen Sie die [Liste der bekannten Probleme](resource-known-issues.md) an, um mehr über bekannte Fehler und Problemumgehungen zu erfahren.

+ **Wichtige Änderungen**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services geben ein Wörterbuch zurück, bisher wurde eine Liste zurückgegeben. Weitere Informationen finden Sie in der [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py)-API-Dokumentation.

  * Der Normalized-Mean-Square-Fehler in den primären Metriken wurde mit automatisiertem maschinellem Lernen entfernt.

+ **HyperDrive**
  * Verschiedene HyperDrive-Fehlerbehebungen für Bayessche Wahrscheinlichkeit, verbesserte Leistung für Abrufaufrufe der Metriken. 
  * Tensorflow 1.10-Upgrade von 1.9 
  * Docker-Image-Optimierung für den Kaltstart. 
  * Aufträge melden jetzt den richtigen Status, selbst wenn sie mit einem anderen Fehlercode als 0 beendet werden. 
  * Überprüfung des RunConfig-Attributs in SDK. 
  * Das HyperDrive-Ausführungsobjekt unterstützt jetzt Abbrechen, ähnlich einer normalen Ausführung: Das Übergeben von Parametern ist nicht erforderlich. 
  * Verbesserte Widgets, um den Status von Dropdownwerten für verteilte Ausführungen und HyperDrive-Ausführungen beizubehalten. 
  * TensorBoard und andere Protokolldateien unterstützen „Fixed“ für den Parameter „Server“. 
  * Dienstseitige Intel(R) MPI-Unterstützung. 
  * Fehlerbehebung bei der Parameteroptimierung für verteilte Ausführungskorrekturen während der Überprüfung in BatchAI. 
  * Der Kontext-Manager identifiziert jetzt die primäre Instanz. 

+ **Azure-Portal-Benutzeroberfläche**
  * log_table() und log_row() werden in den Ausführungsdetails unterstützt. 
  * Erstellen Sie automatisch Diagramme für Tabellen und Zeilen mit 1, 2 oder 3 numerischen Spalten und einer optionalen Kategoriespalte.

+ **Automatisiertes maschinelles Lernen**
  * Verbesserte Fehlerbehandlung und Dokumentation 
  * Behobene Leistungsprobleme beim Abrufen der Ausführungseigenschaft. 
  * Behobenes Problem beim Fortsetzen der Ausführung. 
  * Behobene Iterationsprobleme.
  * Behobener Fehler wegen fehlender Reaktion beim Training unter MAC OS.
  * Downsampling der durchschnittlichen PR/ROC-Kurve im benutzerdefinierten Überprüfungsszenario.
  * Zusätzliche Indexlogik wurde entfernt.
  * Filter aus der get_output-API entfernt.

+ **Pipelines**
  * Hinzugefügte Methode Pipeline.publish() zum direkten Veröffentlichen von Pipelines ohne vorhergehende Ausführung.   
  * Hinzugefügte Methode PipelineRun.get_pipeline_runs() zum Abfangen der Pipelineausführungen, die von einer veröffentlichten Pipeline generiert wurden.

+ **Project Brainwave**
  * Aktualisierte Unterstützung für neue KI-Modelle auf FPGAs verfügbar.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning – Datenaufbereitungs-SDK v0.2.0
[Version 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) umfasst folgende Features und Programmfehlerbehebungen:

+ **Neue Features**
  * Unterstützung für 1-aus-n-Codierung
  * Unterstützung für Quantiltransformation
   
+ **Behobener Fehler:**
  * Funktioniert mit jeder Tornado-Version, ein Downgrade Ihrer Tornado-Version ist nicht erforderlich
  * Der Wert gilt für alle Werte, nicht nur für die höchsten drei

## <a name="2018-09-public-preview-refresh"></a>09.2018 (Aktualisierung der öffentlichen Vorschau)

Neues, aktualisiertes Release von Azure Machine Learning: Weitere Informationen zu diesem Release: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Übersicht zum [Azure Machine Learning Service](../service/overview-what-is-azure-ml.md).
