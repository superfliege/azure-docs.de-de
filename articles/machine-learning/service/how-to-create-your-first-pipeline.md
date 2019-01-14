---
title: Erstellen, Ausführen und Nachverfolgen von ML-Pipelines
titleSuffix: Azure Machine Learning service
description: Erstellen Sie eine Machine Learning-Pipeline mit dem Azure Machine Learning SDK für Python, und führen Sie sie aus. Pipelines werden zum Erstellen und Verwalten von Workflows verwendet, die Machine Learning-Phasen (ML) zusammenfügen. Zu diesen Phasen gehören beispielsweise Datenaufbereitung, Modelltraining, Modellimplementierung und Rückschluss.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6c6472b824eefdd1954f3645c69090d1fb5455de
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2018
ms.locfileid: "53754457"
---
# <a name="create-and-run-a-machine-learning-pipeline-by-using-azure-machine-learning-sdk"></a>Erstellen und Ausführen einer Machine Learning-Pipeline mit dem Azure Machine Learning SDK

In diesem Artikel erfahren Sie, wie Sie eine [Machine Learning-Pipeline](concept-ml-pipelines.md) mit dem [Azure Machine Learning SDK](https://aka.ms/aml-sdk) erstellen, veröffentlichen, ausführen und verfolgen.  Diese Pipelines helfen beim Erstellen und Verwalten der Workflows, die verschiedene Machine Learning-Phasen zusammenfügen. Jede Phase einer Pipeline, z. B. Datenvorbereitung und Modelltraining, kann einen oder mehrere Schritte umfassen.

Die von Ihnen erstellten Pipelines sind für die Mitglieder Ihres Azure Machine Learning Service-[Arbeitsbereichs](how-to-manage-workspace.md) sichtbar. 

Pipelines verwenden Remote-Computeziele für die Berechnung und Speicherung der mit dieser Pipeline verbundenen Zwischen- und Enddaten. Pipelines können Daten in unterstützten [Azure Storage](https://docs.microsoft.com/azure/storage/)-Speicherorten lesen und schreiben.

>[!Note]
>Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](http://aka.ms/AMLFree) aus.

## <a name="prerequisites"></a>Voraussetzungen

* [Konfigurieren Sie Ihre Entwicklungsumgebung](how-to-configure-environment.md) für die Installation des Azure Machine Learning-SDKs.

* Erstellen Sie einen [Azure Machine Learning-Arbeitsbereich](how-to-configure-environment.md#workspace), um Ihre gesamten Pipelineressourcen aufzunehmen. 

 ```python
 ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
 ```

## <a name="set-up-machine-learning-resources"></a>Einrichten von Machine Learning-Ressourcen

Erstellen Sie die Ressourcen, die zum Ausführen einer Pipeline erforderlich sind:

* Richten Sie einen Datenspeicher ein, über den auf die in den Pipelineschritten benötigten Daten zugegriffen werden kann.

* Konfigurieren Sie ein `DataReference`-Objekt, um auf Daten zu verweisen, die sich in einem Datenspeicher befinden oder dort zugänglich sind.

* Richten Sie die [Computeziele](concept-azure-machine-learning-architecture.md#compute-target) ein, auf denen Ihre Pipelineschritte ausgeführt werden.

### <a name="set-up-a-datastore"></a>Einrichten eines Datenspeichers
Ein Datenspeicher speichert die Daten für den Zugriff durch die Pipeline. Jeder Arbeitsbereich verfügt über einen Standarddatenspeicher. Sie können weitere Datenspeicher registrieren. 

Wenn Sie Ihren Arbeitsbereich erstellen, werden [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) und [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) standardmäßig an den Arbeitsbereich angefügt. Azure Files ist der Standarddatenspeicher für einen Arbeitsbereich, aber Sie können auch Blob Storage als Datenspeicher verwenden. Weitere Informationen finden Sie unter [Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträgern](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore (Azure file storage)
def_data_store = ws.get_default_datastore() 

# The above call is equivalent to this 
def_data_store = Datastore(ws, "workspacefilestore")

# Get blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")
```

Laden Sie Datendateien oder Verzeichnisse in den Datenspeicher hoch, damit sie von Ihren Pipelines aus zugänglich sind. In diesem Beispiel wird die Blob Storage-Version des Datenspeichers verwendet:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups", 
    overwrite=True)
```

Eine Pipeline besteht aus mindestens einem Schritt. Ein Schritt ist eine Einheit, die auf einem Computeziel ausgeführt wird. Schritte können Datenquellen verbrauchen und „Zwischendaten“ erzeugen. Ein Schritt kann Daten wie ein Modell, ein Verzeichnis mit Modell- und abhängigen Dateien oder temporäre Daten erstellen. Diese Daten stehen dann später für weitere Schritte in der Pipeline zur Verfügung.

### <a name="configure-data-reference"></a>Konfigurieren eines Datenverweises

Sie haben gerade eine Datenquelle erstellt, auf die in einer Pipeline als Eingabe für einen Schritt verwiesen werden kann. Eine Datenquelle in einer Pipeline wird durch ein [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference)-Objekt dargestellt. Das -`DataReference`Objekt verweist auf Daten, die sich in einem Datenspeicher befinden oder über diesen zugänglich sind.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Zwischendaten (oder die Ausgabe eines Schritts) werden durch ein [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py)-Objekt dargestellt. `output_data1` wird als Ausgabe eines Schritts erstellt und als Eingabe für einen oder mehrere zukünftige Schritte verwendet. `PipelineData` führt eine Datenabhängigkeit zwischen den Schritten ein und erzeugt eine implizite Ausführungsreihenfolge in der Pipeline.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

### <a name="set-up-compute"></a>Einrichten von Compute

In Azure Machine Learning bezieht sich der Begriff *Compute* (oder *Computeziel*) auf die Computer oder Cluster, die die Berechnungsschritte in Ihrer Machine Learning-Pipeline durchführen. Sie können z.B. eine Azure Machine Learning-Computeressource zum Ausführen Ihrer Schritte erstellen.

```python
compute_name = "aml-compute"
 if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                min_nodes = 1, 
                                                                max_nodes = 4)
     # create the compute target
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # Can poll for a minimum number of nodes and for a specific timeout. 
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

## <a name="construct-your-pipeline-steps"></a>Erstellen Ihrer Pipelineschritte

Jetzt sind Sie bereit, einen Pipelineschritt zu definieren. Über das Azure Machine Learning SDK stehen viele integrierte Schritte zur Verfügung. Der grundlegendste dieser Schritte ist ein `PythonScriptStep`, der ein Python-Skript an einem bestimmten Computeziel ausführt.

```python
trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", processed_data1],
    inputs=[blob_input_data],
    outputs=[processed_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Nachdem Sie die Schritte definiert haben, erstellen Sie die Pipeline mit einigen oder allen dieser Schritte.

>[!NOTE]
>Es werden keine Dateien oder Daten in Azure Machine Learning Service hochgeladen, wenn Sie die Schritte definieren oder die Pipeline erstellen.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

## <a name="submit-the-pipeline"></a>Übermitteln der Pipeline

Wenn Sie die Pipeline übermitteln, prüft Azure Machine Learning Service die Abhängigkeiten für die einzelnen Schritte und lädt eine Momentaufnahme des als Quellverzeichnis angegebenen Ordners hoch. Wenn kein Quellverzeichnis angegeben ist, wird das aktuelle lokale Verzeichnis hochgeladen.

```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
```

Wenn Sie eine Pipeline erstmals ausführen, geht Azure Machine Learning folgendermaßen vor:

* Die Momentaufnahme des Projekts wird aus dem Blobspeicher, der dem Arbeitsbereich zugeordnet ist, auf das Computeziel heruntergeladen.
* Für jeden Schritt in der Pipeline wird ein Docker-Image erstellt.
* Das Docker-Image für die einzelnen Schritte wird aus der Containerregistrierung auf das Computeziel heruntergeladen.
* Wenn in einem Schritt ein `DataReference`-Objekt angegeben ist, wird der Datenspeicher eingebunden. Wenn das Einbinden nicht unterstützt wird, werden die Daten stattdessen in das Computeziel kopiert.
* Der Schritt wird in dem in der Schrittdefinition angegebenen Computeziel ausgeführt. 
* Artefakte wie Protokolle, stdout und stderr, Metriken und Ausgaben, die durch den Schritt angegeben werden, werden erstellt. Diese Artefakte werden dann hochgeladen und im Standarddatenspeicher des Benutzers gespeichert.

![Diagramm zum Ausführen eines Experiments als Pipeline](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

## <a name="publish-a-pipeline"></a>Veröffentlichen einer Pipeline

Sie können eine Pipeline veröffentlichen, um sie später mit verschiedenen Eingaben auszuführen. Damit der REST-Endpunkt einer bereits veröffentlichten Pipeline Parameter akzeptiert, muss die Pipeline vor der Veröffentlichung parametrisiert werden. 

1. Um einen Pipelineparameter zu erstellen, verwenden Sie ein [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py)-Objekt mit einem Standardwert.

 ```python
 pipeline_param = PipelineParameter(
     name="pipeline_arg", 
     default_value=10)
 ```

2. Fügen Sie dieses `PipelineParameter`-Objekt als Parameter zu einem der Schritte in der Pipeline wie folgt hinzu:

 ```python
 compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],    
     target=compute_target, 
     source_directory=project_folder)
 ```

3. Veröffentlichen Sie diese Pipeline, die beim Aufruf einen Parameter akzeptiert.

```python
published_pipeline1 = pipeline1.publish(
    name="My_Published_Pipeline", 
    description="My Published Pipeline Description")
```

## <a name="run-a-published-pipeline"></a>Ausführen einer veröffentlichten Pipeline

Alle veröffentlichten Pipelines weisen einen REST-Endpunkt auf. Dieser Endpunkt ruft die Ausführung der Pipeline über externe Systeme wie Nicht-Python-Clients auf. Dieser Endpunkt ermöglicht „verwaltete Wiederholbarkeit“ in Szenarien für die Batchbewertung und für erneutes Training.

Zum Aufrufen der Ausführung der vorhergehenden Pipeline benötigen Sie ein Headertoken für die Azure Active Directory-Authentifizierung, wie in [AzureCliAuthentication-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py) beschrieben.

```python
response = requests.post(published_pipeline1.endpoint, 
    headers=aad_token, 
    json={"ExperimentName": "My_Pipeline",
        "ParameterAssignments": {"pipeline_arg": 20}})
```
## <a name="view-results"></a>Anzeigen der Ergebnisse

Hier finden Sie die Liste aller Ihrer Pipelines und deren Ausführungsdetails:
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.  

1. [Zeigen Sie Ihren Arbeitsbereich](how-to-manage-workspace.md#view-a-workspace) an, um die Liste der Pipelines zu finden.
 ![Liste der Machine Learning-Pipelines](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. Wählen Sie eine bestimmte Pipeline aus, um die Ausführungsergebnisse anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte
- Verwenden Sie [diese Jupyter-Notebooks auf GitHub](https://aka.ms/aml-pipeline-readme), um die Pipelines für maschinelles Lernen eingehender zu erkunden.
- Lesen Sie die SDK-Referenzhilfe für die Pakete [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) und [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py).

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
