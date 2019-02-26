---
title: Was passiert mit Azure Batch AI? | Microsoft-Dokumentation
description: Erfahren Sie, was mit Azure Batch AI und der Azure Machine Learning Service-Computeoption passiert.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 2/14/2019
ms.author: garye
ms.openlocfilehash: 87dcf18a2517561e3166726f8f1f1a70c2ec7713
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447802"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Was passiert mit Azure Batch AI?

**Der Azure Batch AI-Dienst wird im März eingestellt.** Die skalierbaren Trainings- und Bewertungsfunktionen von Batch AI sind jetzt im [Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md) verfügbar, der am 4. Dezember 2018 in die allgemeine Verfügbarkeit überging.

Der Azure Machine Learning Service umfasst neben vielen weiteren Funktionen für das maschinelle Lernen ein cloudbasiertes, verwaltetes Computeziel zum Trainieren, Bereitstellen und Bewerten von Machine Learning-Modellen. Dieses Computeziel heißt [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Beginnen Sie noch heute mit der Migration und Nutzung](#migrate). Sie können über die entsprechenden [Python SDKs](../machine-learning/service/quickstart-create-workspace-with-python.md), die Befehlszeilenschnittstelle und das [Azure-Portal](../machine-learning/service/quickstart-get-started.md) mit dem Azure Machine Learning Service interagieren.

## <a name="support-timeline"></a>Unterstützungszeitachse

| Datum | Informationen zur Unterstützung des Batch AI-Diensts |
| ---- |-----------------|
| 14.&nbsp;Dezember&nbsp;2018| Sie können Ihre vorhandenen Azure Batch AI-Abonnements wie gewohnt weiter verwenden. Es sind jedoch keine **neuen Abonnements** und keine neuen Investitionen möglich.|
| 31.&nbsp;März&nbsp;2019 | Nach diesem Datum funktionieren Batch AI-Abonnements nicht mehr. |

## <a name="how-does-azure-machine-learning-service-compare"></a>Was macht Azure Machine Learning Service aus?
Azure Machine Learning Service ist ein Clouddienst, mit dem Sie Modelle für maschinelles Lernen trainieren, bereitstellen, automatisieren und verwalten und dabei von den Vorteilen der Cloud profitieren können. Allgemeine Informationen zu Azure Machine Learning Service finden Sie in [dieser Übersicht](../machine-learning/service/overview-what-is-azure-ml.md).
 

Der Entwicklungslebenszyklus für ein Modell umfasst in der Regel folgende Phasen: Datenaufbereitung, Training/Experimente und Bereitstellung. Dieser End-to-End-Zyklus kann mithilfe von Machine Learning-Pipelines orchestriert werden.

![Flussdiagramm](./media/overview-what-happened-batch-ai/lifecycle.png)


Weitere Informationen zur Funktionsweise und zu den wichtigsten Konzepten des Diensts finden Sie [hier](../machine-learning/service/concept-azure-machine-learning-architecture.md). Viele der Konzepte des Modelltrainingsworkflows ähneln den Konzepten aus Batch AI. 

Dies wird in der folgenden Zuordnung veranschaulicht:
 
|Batch AI-Dienst|  Azure Machine Learning-Dienst|
|:--:|:---:|
|Arbeitsbereich|Arbeitsbereich|
|Cluster|   Compute vom Typ `AmlCompute`|
|Dateiserver|Datenspeicher|
|Experimente|Experimente|
|Aufträge|Ausführungen (geschachtelte Ausführungen möglich)|
 
Hier noch eine andere Darstellung der gleichen Tabelle zur weiteren Veranschaulichung:
 
**Batch AI-Hierarchie**
![Flussdiagramm](./media/overview-what-happened-batch-ai/batchai-heirarchy.png) 
 
**Azure Machine Learning Service-Hierarchie**
![Flussdiagramm](./media/overview-what-happened-batch-ai/azure-machine-learning-service-heirarchy.png) 

### <a name="platform-capabilities"></a>Plattformfunktionen
Azure Machine Learning Service bietet mehrere neue Funktionen. Hierzu zählt unter anderem ein End-to-End-Trainings-/Bereitstellungsstapel, den Sie für Ihre KI-Entwicklung verwenden können, ohne Azure-Ressourcen verwalten zu müssen. Die folgende Tabelle enthält eine Gegenüberstellung der Trainingsfeatureunterstützung für die beiden Dienste:

|Feature|Batch AI-Dienst|Azure Machine Learning-Dienst|
|-------|:-------:|:-------:|
|Wahl der VM-Größe |CPU/GPU    |CPU/GPU. Unterstützt auch FPGA für Rückschlüsse|
|KI-bereiter Cluster (Treiber, Docker usw.)|  Ja |Ja|
|Knotenvorbereitung| Ja|    Nein |
|Wahl der Betriebssystemfamilie   |Teilweise    |Nein |
|Dedizierte VMs und VMs mit niedriger Priorität  |Ja    |Ja|
|Automatische Skalierung   |Ja    |Ja (standardmäßig)|
|Wartezeit für automatische Skalierung  |Nein  |Ja|
|SSH    |Ja|   Ja|
|Einbindung auf Clusterebene |Ja (Dateifreigaben, Blobs, NFS, benutzerdefiniert)   |Ja (Einbindung oder Download Ihres Datenspeichers)|
|Verteiltes Training|  Ja |Ja|
|Ausführungsmodus|    Virtueller Computer oder Container|    Container|
|Benutzerdefiniertes Containerimage|    Ja |Ja|
|Beliebiges Toolkit    |Ja    |Ja (Python-Skript ausführen)|
|Auftragsvorbereitung|    Ja |Noch nicht|
|Bereitstellung auf Auftragsebene |Ja (Dateifreigaben, Blobs, NFS, benutzerdefiniert)   |Ja (Dateifreigaben, Blobs)|
|Auftragsüberwachung     |über „GetJob“|    über Ausführungsverlauf (umfangreichere Informationen, benutzerdefinierte Runtime zum Pushen weiterer Metriken)|
|Abrufen von Auftragsprotokollen und Dateien/Modellen |   über „ListFiles“ und Storage-APIs  |über den Artefaktdienst|
 |Unterstützung von Tensorboard   |Nein |    Ja|
|Kontingente auf VM-Familienebene |Ja    |Ja (mit Übernahme Ihrer vorherigen Kapazität)|
 
Neben den Features aus der obigen Tabelle stehen in Azure Machine Learning Service weitere Features zur Verfügung, die in Batch AI bisher nicht unterstützt wurden:

|Feature|Batch AI-Dienst|Azure Machine Learning-Dienst|
|-------|:-------:|:-------:|
|Umgebungsvorbereitung    |Nein  |Ja (Conda-Vorbereitung und Upload in ACR)|
|Hyperparameteroptimierung  |Nein |    Ja|
|Modellverwaltung   |Nein  |Ja|
|Operationalisierung/Bereitstellung| Nein   |Über AKS und ACI|
|Vorbereitung der Daten   |Nein  |Ja|
|Computeziele    |Virtuelle Azure-Computer  |Lokal, Batch AI (als „AmlCompute“), Databricks, HDInsight|
|Automatisiertes maschinelles Lernen |Nein |    Ja|
|Pipelines  |Nein  |Ja|
|Batchbewertung  |Ja    |Ja|
|Portal-/CLI-Unterstützung|    Ja |Ja|


### <a name="programming-interfaces"></a>Programmierschnittstellen

Die folgende Tabelle enthält die verschiedenen verfügbaren Programmierschnittstellen für den jeweiligen Dienst:
    
|Feature|Batch AI-Dienst|Azure Machine Learning-Dienst|
|-------|:-------:|:-------:|
|SDK    |Java, C#, Python, Node.js   |Python (konfigurations- und estimatorbasierte Ausführung für gängige Frameworks)|
|Befehlszeilenschnittstelle (CLI)    |Ja    |Noch nicht|
|Azure-Portal   |Ja    |Ja (mit Ausnahme der Auftragsübermittlung)|
|REST-API   |Ja    |Ja, aber verteilt auf Microservices|




## <a name="why-migrate"></a>Vorteile der Migration

Wenn Sie von der Vorschauversion des Batch AI-Diensts auf die allgemein verfügbare Version von Azure Machine Learning Service upgraden, stehen Ihnen benutzerfreundlichere Konzepte wie Estimatoren und Datenspeicher zur Verfügung. Darüber hinaus profitieren Sie von Azure-SLAs und Kundensupport für allgemein verfügbare Versionen.

Azure Machine Learning Service bietet zudem neue Funktionen wie automatisiertes maschinelles Lernen, Hyperparameteroptimierung und ML-Pipelines, die bei den meisten umfangreichen KI-Workloads hilfreich sind. Dank der Möglichkeit, ein trainiertes Modell ohne Wechsel zu einem separaten Dienst zu operationalisieren, können Sie die Data Science-Schleife von der Datenaufbereitung (mit dem Datenaufbereitungs-SDK) bis hin zur Operationalisierung und Modellüberwachung abwickeln.

<a name="migrate"></a>

## <a name="how-do-i-migrate"></a>Wie führe ich eine Migration aus?

Bevor Sie die Schritte in diesem Migrationsleitfaden ausführen, die Sie bei der Zuordnung der Befehle zwischen den beiden Diensten unterstützen, sollten Sie sich zunächst anhand [dieser Dokumentation](../machine-learning/service/overview-what-is-azure-ml.md) und [dieses Python-Tutorials](../machine-learning/service/tutorial-train-models-with-aml.md) mit Azure Machine Learning Service vertraut machen.

Um Unterbrechungen Ihrer Anwendungen zu vermeiden und von den neuesten Features zu profitieren, führen Sie vor dem 31. März 2019 folgende Schritte aus:

1. Erstellen Sie einen Azure Machine Learning Service-Arbeitsbereich, um loszulegen:
    + [Schnellstartanleitung für Python](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Schnellstartanleitung für das Azure-Portal](../machine-learning/service/quickstart-get-started.md)

1. Richten Sie eine Instanz von [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute) für das Trainieren von Modellen ein.

1. Aktualisieren Sie Ihre Skripts, sodass diese Azure Machine Learning Compute verwenden.


### <a name="sdk-migration"></a>SDK-Migration

Die aktuelle SDK-Unterstützung in Azure Machine Learning Service wird über mehrere Python-SDKs realisiert. Das Haupt-SDK wird etwa alle zwei Wochen aktualisiert und kann über PyPi mithilfe des folgenden Befehls installiert werden:

```python
pip install --upgrade azureml-sdk[notebooks]
```

Informationen zum Einrichten Ihrer Umgebung und zum Installieren des SDK finden Sie in [dieser Schnellstartanleitung](../machine-learning/service/quickstart-create-workspace-with-python.md#install-the-sdk).

Wenn Sie ein Jupyter Notebook öffnen und der Kernel auf die relevante Conda-Umgebung verweist, sieht die Befehlszuordnung zwischen den beiden Diensten wie folgt aus:


#### <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs
Das Konzept der Initialisierung eines Arbeitsbereichs mithilfe einer Datei vom Typ „configuration.json“ in Batch AI ähnelt der Verwendung einer Konfigurationsdatei in Azure Machine Learning.

Vorgehensweise für **Batch AI**:

```python
sys.path.append('../../..')
import utilities as utils

cfg = utils.config.Configuration('../../configuration.json')
client = utils.config.create_batchai_client(cfg)

utils.config.create_resource_group(cfg)
_ = client.workspaces.create(cfg.resource_group, cfg.workspace, cfg.location).result()
```

Vorgehensweise für **Azure Machine Learning Service**:

```python
from azureml.core.workspace import Workspace

ws = Workspace.from_config()
print('Workspace name: ' + ws.name, 
      'Azure region: ' + ws.location, 
      'Subscription id: ' + ws.subscription_id, 
      'Resource group: ' + ws.resource_group, sep = '\n')
```

Sie können einen Arbeitsbereich auch direkt erstellen, indem Sie die Konfigurationsparameter angeben:

```python
from azureml.core import Workspace
# Create the workspace using the specified parameters
ws = Workspace.create(name = workspace_name,
                      subscription_id = subscription_id,
                      resource_group = resource_group, 
                      location = workspace_region,
                      create_resource_group = True,
                      exist_ok = True)
ws.get_details()

# write the details of the workspace to a configuration file to the notebook library
ws.write_config()
```

Weitere Informationen zur Workspace-Klasse von AML finden Sie in der [SDK-Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).


#### <a name="create-a-compute-cluster"></a>Erstellen eines Computeclusters
Azure Machine Learning unterstützt mehrere Computeziele. Einige davon werden durch den Dienst verwaltet, andere können an Ihren Arbeitsbereich angefügt werden (beispielsweise ein HDInsight-Cluster oder ein virtueller Remotecomputer). Weitere Informationen zu verschiedenen Computezielen finden Sie [hier](../machine-learning/service/how-to-set-up-training-targets.md). Das Konzept der Erstellung eines Batch AI-Computeclusters entspricht der Erstellung eines AmlCompute-Clusters in Azure Machine Learning. Bei der Amlcompute-Erstellung wird eine Computekonfiguration verwendet, was mit der Übergabe von Parametern in Batch AI vergleichbar ist. Hinweis: Für den AmlCompute-Cluster ist die automatische Skalierung standardmäßig aktiviert. In Batch AI ist sie standardmäßig deaktiviert.

Vorgehensweise für **Batch AI**:

```python
nodes_count = 2
cluster_name = 'nc6'

parameters = models.ClusterCreateParameters(
    vm_size='STANDARD_NC6',
    scale_settings=models.ScaleSettings(
        manual=models.ManualScaleSettings(target_node_count=nodes_count)
    ),
    user_account_settings=models.UserAccountSettings(
        admin_user_name=cfg.admin,
        admin_user_password=cfg.admin_password or None,
        admin_user_ssh_public_key=cfg.admin_ssh_key or None,
    )
)
_ = client.clusters.create(cfg.resource_group, cfg.workspace, cluster_name, parameters).result()
```

Vorgehensweise für **Azure Machine Learning Service**:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
gpu_cluster_name = "nc6"

# Verify that cluster does not exist already
try:
    gpu_cluster = ComputeTarget(workspace=ws, name=gpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6',
                                                           vm_priority='lowpriority',
                                                           min_nodes=1,
                                                           max_nodes=2,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    gpu_cluster = ComputeTarget.create(ws, gpu_cluster_name, compute_config)

gpu_cluster.wait_for_completion(show_output=True)
```

Weitere Informationen zur AMLCompute-Klasse von AML finden Sie in der [SDK-Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py). In der obigen Konfiguration sind nur die Angaben „vm_size“ und „max_nodes“ obligatorisch. Die übrigen Eigenschaften (etwa die VNETs) dienen lediglich zur erweiterten Clustereinrichtung.


#### <a name="monitoring-status-of-your-cluster"></a>Überwachen des Status Ihres Clusters
Dies ist in Azure Machine Learning einfacher, wie hier zu sehen:

Vorgehensweise für **Batch AI**:

```python
cluster = client.clusters.get(cfg.resource_group, cfg.workspace, cluster_name)
utils.cluster.print_cluster_status(cluster)
```

Vorgehensweise für **Azure Machine Learning Service**:

```python
gpu_cluster.get_status().serialize()
```

#### <a name="getting-reference-to-a-storage-account"></a>Abrufen eines Verweises auf ein Speicherkonto
Das Konzept eines Datenspeichers (etwa ein Blob) wird in Azure Machine Learning mithilfe des DataStore-Objekts vereinfacht. Standardmäßig erstellt Ihr Azure Machine Learning-Arbeitsbereich ein Speicherkonto. Im Rahmen der Arbeitsbereicherstellung können Sie jedoch auch Ihren eigenen Speicher anfügen. 

Vorgehensweise für **Batch AI**:

```python
azure_blob_container_name = 'batchaisample'
blob_service = BlockBlobService(cfg.storage_account_name, cfg.storage_account_key)
blob_service.create_container(azure_blob_container_name, fail_on_exist=False)
```

Vorgehensweise für **Azure Machine Learning Service**:

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)
```

Weitere Informationen zum Registrieren zusätzlicher Speicherkonten sowie zum Abrufen eines Verweises auf einen anderen registrierten Datenspeicher finden Sie in der [Dokumentation für Azure Machine Learning Service](../machine-learning/service/how-to-access-data.md#create-a-datastore).


#### <a name="downloading-and-uploading-data"></a>Herunter- und Hochladen von Daten 
Bei beiden Diensten lassen sich die Daten mithilfe des obigen Datenspeicherverweises problemlos in das Speicherkonto hochladen. Für Batch AI stellen wir auch das Trainingsskript als Teil der Dateifreigabe bereit. Sie werden jedoch sehen, wie Sie es im Fall von Azure Machine Learning als Teil Ihrer Auftragskonfiguration angeben können.

Vorgehensweise für **Batch AI**:

```python
mnist_dataset_directory = 'mnist_dataset'
utils.dataset.download_and_upload_mnist_dataset_to_blob(
    blob_service, azure_blob_container_name, mnist_dataset_directory)

script_directory = 'tensorflow_samples'
script_to_deploy = 'mnist_replica.py'

blob_service.create_blob_from_path(azure_blob_container_name,
                                   script_directory + '/' + script_to_deploy, 
                                   script_to_deploy)
```


Vorgehensweise für **Azure Machine Learning Service**:

```python
import os
import urllib
os.makedirs('./data', exist_ok=True)
download_url = 'https://s3.amazonaws.com/img-datasets/mnist.npz'
urllib.request.urlretrieve(download_url, filename='data/mnist.npz')

ds.upload(src_dir='data', target_path='mnist_dataset', overwrite=True, show_progress=True)

path_on_datastore = ' mnist_dataset/mnist.npz' ds_data = ds.path(path_on_datastore) print(ds_data)
```

#### <a name="create-an-experiment"></a>Erstellen eines Experiments
Wie bereits erwähnt gleicht das Experimentkonzept von Azure Machine Learning dem Konzept in Batch AI. Jedes Experiment kann einzelne Ausführungen umfassen, was mit Aufträgen in Batch AI vergleichbar ist. In Azure Machine Learning ist außerdem eine Hierarchie mit übergeordneten und untergeordneten Ausführungen möglich.

Vorgehensweise für **Batch AI**:

```python
experiment_name = 'tensorflow_experiment'
experiment = client.experiments.create(cfg.resource_group, cfg.workspace, experiment_name).result()
```

Vorgehensweise für **Azure Machine Learning Service**:

```python
from azureml.core import Experiment

experiment_name = 'tensorflow_experiment'
experiment = Experiment(ws, name=experiment_name)
```


#### <a name="submit-a-job"></a>Übermitteln eines Auftrags
Nach der Experimenterstellung haben Sie mehrere Möglichkeiten, um eine Ausführung zu übermitteln. In diesem Beispiel möchten wir ein Deep Learning-Modell mit TensorFlow erstellen und dabei einen Azure Machine Learning-Estimator verwenden. Ein [Estimator](../machine-learning/service/how-to-train-ml-models.md) ist einfach eine Wrapperfunktion für die zugrunde liegende Ausführungskonfiguration und dient dazu, die Übermittlung von Ausführungen zu vereinfachen. Er wird derzeit nur für Pytorch und TensorFlow unterstützt. Das Datenspeicherkonzept vereinfacht zudem die Angabe der Einbindungspfade. 

Vorgehensweise für **Batch AI**:

```python
azure_file_share = 'afs'
azure_blob = 'bfs'
args_fmt = '--job_name={0} --num_gpus=1 --train_steps 10000 --checkpoint_dir=$AZ_BATCHAI_OUTPUT_MODEL --log_dir=$AZ_BATCHAI_OUTPUT_TENSORBOARD --data_dir=$AZ_BATCHAI_INPUT_DATASET --ps_hosts=$AZ_BATCHAI_PS_HOSTS --worker_hosts=$AZ_BATCHAI_WORKER_HOSTS --task_index=$AZ_BATCHAI_TASK_INDEX'

parameters = models.JobCreateParameters(
     cluster=models.ResourceId(id=cluster.id),
     node_count=2,
     input_directories=[
        models.InputDirectory(
            id='SCRIPT',
            path='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}/{1}'.format(azure_blob, script_directory)),
        models.InputDirectory(
            id='DATASET',
            path='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}/{1}'.format(azure_blob, mnist_dataset_directory))],
     std_out_err_path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
     output_directories=[
        models.OutputDirectory(
            id='MODEL',
            path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
            path_suffix='Models'),
        models.OutputDirectory(
            id='TENSORBOARD',
            path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
            path_suffix='Logs')
     ],
     mount_volumes=models.MountVolumes(
            azure_file_shares=[
                models.AzureFileShareReference(
                    account_name=cfg.storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=cfg.storage_account_key),
                    azure_file_url='https://{0}.file.core.windows.net/{1}'.format(
                        cfg.storage_account_name, azure_file_share_name),
                    relative_mount_path=azure_file_share)
            ],
            azure_blob_file_systems=[
                models.AzureBlobFileSystemReference(
                    account_name=cfg.storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=cfg.storage_account_key),
                    container_name=azure_blob_container_name,
                    relative_mount_path=azure_blob)
            ]
        ),
     container_settings=models.ContainerSettings(
         image_source_registry=models.ImageSourceRegistry(image='tensorflow/tensorflow:1.8.0-gpu')),
     tensor_flow_settings=models.TensorFlowSettings(
         parameter_server_count=1,
         worker_count=nodes_count,
         python_script_file_path='$AZ_BATCHAI_INPUT_SCRIPT/'+ script_to_deploy,
         master_command_line_args=args_fmt.format('worker'),
         worker_command_line_args=args_fmt.format('worker'),
         parameter_server_command_line_args=args_fmt.format('ps'),
     )
)
```

In Batch AI wird der eigentliche Auftrag im Rahmen der Erstellungsfunktion übermittelt.

```python
job_name = datetime.utcnow().strftime('tf_%m_%d_%Y_%H%M%S')
job = client.jobs.create(cfg.resource_group, cfg.workspace, experiment_name, job_name, parameters).result()
print('Created Job {0} in Experiment {1}'.format(job.name, experiment.name))
```

Umfassende Informationen zu diesem Trainingscodeausschnitt (einschließlich der Datei „mnist_replica.py“, die weiter oben in die Dateifreigabe hochgeladen wurde), finden Sie im [GitHub-Repository mit dem Batch AI-Beispielnotebook](https://github.com/Azure/BatchAI/tree/2238607d5a028a0c5e037168aefca7d7bb165d5c/recipes/TensorFlow/TensorFlow-GPU-Distributed).

Vorgehensweise für **Azure Machine Learning Service**:

```python
from azureml.train.dnn import TensorFlow

script_params={
    '--num_gpus': 1,
    '--train_steps': 500,
    '--input_data': ds_data.as_mount()

}

estimator = TensorFlow(source_directory=project_folder,
                       compute_target=gpu_cluster,
                       script_params=script_params,
                       entry_script='tf_mnist_replica.py',
                       node_count=2,
                       worker_count=2,
                       parameter_server_count=1,   
                       distributed_backend='ps',
                       use_gpu=True)
```

Umfassende Informationen zu diesem Trainingscodeausschnitt (einschließlich der Datei „tf_mnist_replica.py“) finden Sie im [GitHub-Repository mit dem Azure Machine Learning-Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/distributed-tensorflow-with-parameter-server). Der Datenspeicher selbst kann entweder in die einzelnen Knoten eingebunden werden, oder die Trainingsdaten können direkt auf den Knoten heruntergeladen werden. Ausführlichere Informationen Datenspeicherverweisen in Ihrem Estimator finden Sie in der [Dokumentation für Azure Machine Learning Service](../machine-learning/service/how-to-access-data.md#access-datastores-for-training). 

Eine Ausführung in Azure Machine Learning wird mithilfe der Übermittlungsfunktion übermittelt.

```python
run = experiment.submit(estimator)
print(run)
```

Eine weitere Möglichkeit zum Angeben von Parametern für Ihre Ausführung ist die Verwendung einer Ausführungskonfiguration. Dies ist insbesondere hilfreich, wenn Sie eine benutzerdefinierte Trainingsumgebung definieren möchten. Weitere Informationen finden Sie in [diesem AmlCompute-Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-amlcompute/train-on-amlcompute.ipynb). 

#### <a name="monitor-your-run"></a>Überwachen Ihrer Ausführung
Nach der Übermittlung einer Ausführung können Sie entweder warten, bis sie abgeschlossen ist, oder sie in Azure Machine Learning mithilfe praktischer Jupyter-Widgets überwachen, die Sie direkt in Ihrem Code aufrufen können. Darüber hinaus können Sie auch den Kontext einer beliebigen vorherigen Ausführung abrufen, indem Sie die verschiedenen Experimente in einem Arbeitsbereich und die einzelnen Ausführungen innerhalb der einzelnen Experimente durchlaufen.

Vorgehensweise für **Batch AI**:

```python
utils.job.wait_for_job_completion(client, cfg.resource_group, cfg.workspace, 
                                  experiment_name, job_name, cluster_name, 'stdouterr', 'stdout-wk-0.txt')

files = client.jobs.list_output_files(cfg.resource_group, cfg.workspace, experiment_name, job_name,
                                      models.JobsListOutputFilesOptions(outputdirectoryid='stdouterr')) 
for f in list(files):
    print(f.name, f.download_url or 'directory')
```


Vorgehensweise für **Azure Machine Learning Service**:

```python
run.wait_for_completion(show_output=True)

from azureml.widgets import RunDetails
RunDetails(run).show()
```

Hier sehen Sie, wie das Widget in Ihrem Notebook geladen wird, um Ihre Protokolle in Echtzeit zu betrachten: ![Überwachungsdiagramm des Widgets](./media/overview-what-happened-batch-ai/monitor.png)



#### <a name="editing-a-cluster"></a>Bearbeiten eines Clusters
Das Löschen eines Clusters ist ganz einfach. Mit Azure Machine Learning können Sie zudem einen Cluster über das Notebook aktualisieren, um ihn auf eine höhere Knotenanzahl zu skalieren, oder die Wartezeit erhöhen, nach der der Cluster im Leerlauf herunterskaliert wird. Die VM-Größe des Clusters kann dagegen nicht geändert werden, da hierzu letztendlich eine neue Bereitstellung am Back-End erforderlich wäre.

Vorgehensweise für **Batch AI**:
```python
_ = client.clusters.delete(cfg.resource_group, cfg.workspace, cluster_name)
```

Vorgehensweise für **Azure Machine Learning Service**:

```python
gpu_cluster.delete()

gpu_cluster.update(min_nodes=2, max_nodes=4, idle_seconds_before_scaledown=600)
```

## <a name="support"></a>Support

Batch AI wird voraussichtlich am 31. März ausgemustert. Schon jetzt können für den Dienst keine neuen Abonnements mehr registriert werden (es sei denn, es wurde zuvor über den Support eine Ausnahme beantragt und genehmigt).  Sollten Sie Fragen oder Feedback in Verbindung mit der Migration zu Azure Machine Learning Service haben, [wenden Sie sich an uns](mailto:AzureBatchAITrainingPreview@service.microsoft.com).

Azure Machine Learning Service ist ein allgemein verfügbarer Dienst. Er verfügt somit über eine verbindliche SLA und bietet eine Auswahl verschiedener Supportpläne.

Die Nutzung der Azure-Infrastruktur kostet für Azure Machine Learning Service voraussichtlich gleich viel wie für den Batch AI-Dienst, da in beiden Fällen lediglich der Preis für die zugrunde liegenden Computeressourcen in Rechnung gestellt wird. Weitere Informationen finden Sie unter [Azure-Preisrechner](https://azure.microsoft.com/pricing/details/machine-learning-service/).

Die regionale Verfügbarkeit für die beiden Dienste können Sie im [Azure-Portal](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all) anzeigen.


## <a name="next-steps"></a>Nächste Schritte

+ Lesen Sie den [Überblick über den Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Konfigurieren Sie Computeziele zum Trainieren von Modellen](../machine-learning/service/how-to-set-up-training-targets.md) mit dem Azure Machine Learning Service.

+ Lesen Sie die [Azure-Roadmap](https://azure.microsoft.com/updates/), um mehr über Updates anderer Azure-Dienste zu erfahren.
