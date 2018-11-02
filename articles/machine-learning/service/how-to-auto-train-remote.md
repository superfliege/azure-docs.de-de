---
title: Einrichten von Remotecomputezielen für automatisiertes maschinelles Lernen – Azure Machine Learning Service
description: Dieser Artikel erklärt, wie Sie Modelle mit automatisiertem maschinellem Lernen auf einem Data Science Virtual Machine-Remotecomputeziel (DSVM) mit Azure Machine Learning Service erstellen.
services: machine-learning
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 798960f30ae13f42c0198cf4bf63412192edc63e
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429829"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Trainieren von Modellen mit automatisiertem maschinellem Lernen in der Cloud

In Azure Machine Learning können Sie Ihr Modell mit verschiedenen, von Ihnen verwalteten Arten von Computeressourcen trainieren. Beim Computeziel kann es sich um einen lokalen Computer oder einen Computer in der Cloud handeln.

Sie können Ihr Machine Learning-Experiment bequem durch Hinzufügen zusätzlicher Computeziele wie Ubuntu-basierte Data Science Virtual Machine (DSVM) oder Azure Batch AI vertikal oder horizontal hochskalieren. Die DSVM ist ein benutzerdefiniertes VM-Image in der Microsoft Azure-Cloud, das speziell für Data Science konfiguriert wurde. Auf ihr sind zahlreiche beliebte Data Science-Tools sowie andere Tools vorinstalliert und vorkonfiguriert.  

In diesem Artikel erfahren Sie, wie Sie ein Modell mit automatisiertem maschinellem Lernen auf einer DSVM-Instanz erstellen. Sie finden Beispiele zum Verwenden von Azure Batch AI in [diesen Beispielnotebooks in GitHub](https://aka.ms/aml-notebooks).  

## <a name="how-does-remote-differ-from-local"></a>Remote und lokal – wo liegen die Unterschiede?

Im Tutorial [Trainieren eines Klassifizierungsmodells mit automatisiertem maschinellem Lernen](tutorial-auto-train-models.md) erfahren Sie, wie Sie einen lokalen Computer zum Trainieren von Modellen mit automatisiertem maschinellem Lernen verwenden.  Der Workflow zum lokalen Trainieren trifft ebenfalls auf Remoteziele zu. Bei Remotecomputezielen werden jedoch die Experimentiterationen mit automatisiertem maschinellem Lernen asynchron ausgeführt. So können Sie eine bestimmte Iteration löschen, den Ausführungsstatus anzeigen oder im Jupyter Notebook an anderen Zellen weiterarbeiten. Damit Sie Ihr Modell remote trainieren können, müssen Sie zunächst ein Remotecomputeziel wie eine DSVM-Instanz erstellen.  Konfigurieren Sie dann die Remoteressource, und übermitteln Sie dort Ihren Code.

Dieser Artikel zeigt die zusätzlichen erforderlichen Schritte, um ein Experiment mit automatisiertem maschinellem Lernen auf einer DSVM-Remoteinstanz auszuführen.  Das Arbeitsbereichsobjekt `ws` aus dem Tutorial wird hier im gesamten Code verwendet.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Erstellen von Ressourcen

Erstellen Sie die DSVM in Ihrem Arbeitsbereich (`ws`), wenn sie nicht bereits vorhanden ist. Wenn die DSVM bereits erstellt wurde, überspringt der Code den Erstellungsvorgang und lädt die vorhandenen Ressourcendetails in das `dsvm_compute`-Objekt.  

**Geschätzte Zeit**: Die VM-Erstellung dauert ca. fünf Minuten.

```python
from azureml.core.compute import DsvmCompute

dsvm_name = 'mydsvm' #Name your DSVM
try:
    dsvm_compute = DsvmCompute(ws, dsvm_name)
    print('found existing dsvm.')
except:
    print('creating new dsvm.')
    # Below is using a VM of SKU Standard_D2_v2 which is 2 core machine. You can check Azure virtual machines documentation for additional SKUs of VMs.
    dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
    dsvm_compute = DsvmCompute.create(ws, name = dsvm_name, provisioning_configuration = dsvm_config)
    dsvm_compute.wait_for_completion(show_output = True)
```

Sie können das `dsvm_compute`-Objekt jetzt als Remotecomputeziel verwenden.

Die Einschränkungen für DSVM-Name umfassen u.a. Folgendes:
+ Muss kürzer sein als 64 Zeichen.  
+ Darf keines der folgenden Zeichen enthalten: `\` ~! @ # $ % ^ & * ( ) = + _ [ ] { } \\\\ | ; : \' \\" , < > / ?.`

>[!Warning]
>Wenn die Erstellung mit einer Meldung zum Erwerb von Marketplace-Angeboten fehlschlägt:
>    1. [Navigieren Sie zum Azure-Portal](https://portal.azure.com).
>    1. Beginnen Sie mit dem Erstellen einer DSVM. 
>    1. Wählen Sie „Want to create programmatically“ (Programmgesteuert erstellen), um die programmgesteuerte Erstellung zu aktivieren.
>    1. Beenden Sie den Vorgang, ohne die VM zu erstellen.
>    1. Führen Sie den Erstellungscode erneut aus.

Dieser Code erstellt weder einen Benutzernamen noch ein Passwort für die bereitgestellte DSVM-Instanz. Wenn Sie sich direkt mit der VM verbinden möchten, gehen Sie zum [Azure-Portal](https://portal.azure.com), um die Anmeldeinformationen bereitzustellen.  


## <a name="access-data-using-getdata-file"></a>Zugreifen auf Daten mit einer „get_data“-Datei

Gewähren Sie der Remoteressource Zugriff auf Ihre Trainingsdaten. Bei automatisierten Machine Learning-Experimenten, die über Remotecomputing ausgeführt werden, müssen die Daten mithilfe einer `get_data()`-Funktion abgerufen werden.  

Zum Gewähren von Zugriff ist Folgendes erforderlich:
+ Erstellen Sie die Datei „get_data.py“, die eine `get_data()`-Funktion enthält. 
* Platzieren Sie diese Datei im Stammverzeichnis des Ordners, der Ihre Skripts enthält. 

Sie können Code zum Lesen von Daten aus einem Blobspeicher oder einem lokalen Datenträger in der Datei „get_data.py“ kapseln. Im folgenden Codebeispiel stammen die Daten aus dem sklearn-Paket.

>[!Warning]
>Wenn Sie ein Remotecomputeziel verwenden, müssen Sie für Ihre Datentransformationen `get_data()` nutzen. Wenn Sie zusätzliche Bibliotheken für Datentransformationen als Teil von „get_data()“ installieren müssen, sind zusätzliche Schritte erforderlich. Weitere Informationen finden Sie im [Beispielnotebook auto-ml-dataprep](https://aka.ms/aml-auto-ml-data-prep ).


```python
# Create a project_folder if it doesn't exist
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

#Write the get_data file.
%%writefile $project_folder/get_data.py

from sklearn import datasets
from scipy import sparse
import numpy as np

def get_data():
    
    digits = datasets.load_digits()
    X_digits = digits.data[10:,:]
    y_digits = digits.target[10:]

    return { "X" : X_digits, "y" : y_digits }
```

## <a name="configure-experiment"></a>Konfigurieren des Experiments

Legen Sie die Einstellungen für `AutoMLConfig` fest.  (Siehe dazu eine [vollständige Liste der Parameter](how-to-configure-auto-train.md#configure-experiment) und ihrer möglichen Werte.)

In den Einstellungen ist `run_configuration` auf das `run_config` -Objekt festgelegt, das die Einstellungen und Konfiguration für die DSVM enthält.  

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "max_time_sec": 600,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target = dsvm_compute,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings
                            )
```

## <a name="submit-training-experiment"></a>Übermitteln des Trainingsexperiments

Übermitteln Sie nun die Konfiguration, um automatisch den Algorithmus und die Hyperparameter festzulegen und das Modell zu trainieren.

```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```
Eine Ausgabe ähnlich der folgenden wird angezeigt.

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************
    
     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0.0                      0.954     0.954
             7      Normalizer DT                         0.0                      0.161     0.954
             0      Scale MaxAbs 1 extra trees            0.0                      0.936     0.954
             4      Robust Scaler SGD classifier          0.0                      0.867     0.954
             1      Normalizer kNN                        0.0                      0.984     0.984
             9      Normalizer extra trees                0.0                      0.834     0.984
             5      Robust Scaler DT                      0.0                      0.736     0.984
             8      Standardize kNN                       0.0                      0.981     0.984
             6      Standardize SVM                       2.2                      0.984     0.984
            10      Scale MaxAbs 1 DT                     0.0                      0.077     0.984
            11      Standardize SGD classifier            0.0                      0.863     0.984
             3      Standardize gradient boosting         5.4                      0.971     0.984
            12      Robust Scaler logistic regression     2.0                      0.955     0.984
            14      Scale MaxAbs 1 SVM                    0.0                      0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      3.4                      0.971     0.989
            15      Robust Scaler kNN                     0.0                      0.904     0.989
            17      Standardize kNN                       0.0                      0.974     0.989
            16      Scale 0/1 gradient boosting           2.8                      0.968     0.989
            18      Scale 0/1 extra trees                 0.0                      0.828     0.989
            19      Robust Scaler kNN                     0.0                      0.983     0.989


## <a name="explore-results"></a>Überprüfen der Ergebnisse

Sie können das gleiche Jupyter-Widget wie in diesem [Trainings-Tutorial](tutorial-auto-train-models.md#explore-the-results) verwenden, um ein Diagramm und eine Tabelle mit Ergebnissen anzuzeigen.

```python
from azureml.train.widgets import RunDetails
RunDetails(remote_run).show()
```
Hier ist ein statisches Bild des Widgets.  Klicken Sie im Notebook auf eine beliebige Zeile in der Tabelle, um die Ausführungseigenschaften und Ausgabeprotokolle für diese Ausführung anzuzeigen.   Sie können auch die Dropdownliste über dem Diagramm verwenden, um ein Diagramm der einzelnen verfügbaren Metriken für jede Iteration anzuzeigen.

![Widget-Tabelle](./media/how-to-auto-train-remote/table.png)
![Widget-Plot](./media/how-to-auto-train-remote/plot.png)

Das Widget zeigt eine URL an, die Sie zum Anzeigen und Überprüfen der Details der einzelnen Ausführungen verwenden können.
 
### <a name="view-logs"></a>Anzeigen von Protokollen

Die Protokolle finden Sie in der DSVM unter /tmp/azureml_run/{iterationid}/azureml-logs.

## <a name="example"></a>Beispiel

Das Notebook [automl/03.auto-ml-remote-execution.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/automl/03.auto-ml-remote-execution.ipynb) veranschaulicht die Konzepte in diesem Artikel.  Notebook abrufen:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, [wie Sie die Einstellungen für das automatische Training konfigurieren](how-to-configure-auto-train.md).