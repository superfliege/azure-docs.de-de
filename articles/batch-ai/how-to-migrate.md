---
title: Migrieren von Batch AI zu Azure Machine Learning Service
description: Hier erfahren Sie, wie Sie für AMLcompute zu Azure Machine Learning Service migrieren und wie Ihr Code mit dem Code in Azure Machine Learning Service zusammenhängt.
ms.service: batch-ai
services: batch-ai
ms.topic: overview
ms.author: jmartens
author: j-martens
ms.date: 2/28/2019
ms.openlocfilehash: e495ed06c640601c0500d14b42070a264fd687a9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57862113"
---
# <a name="migrating-from-batch-ai-to-azure-machine-learning-service"></a>Migrieren von Batch AI zu Azure Machine Learning Service

**Der Azure Batch AI-Dienst wird im März eingestellt.** Die skalierbaren Trainings- und Bewertungsfunktionen von Batch AI sind jetzt im [Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md) verfügbar, der am 4. Dezember 2018 in die allgemeine Verfügbarkeit überging.

Der Azure Machine Learning Service umfasst neben vielen weiteren Funktionen für das maschinelle Lernen ein cloudbasiertes, verwaltetes Computeziel zum Trainieren, Bereitstellen und Bewerten von Machine Learning-Modellen. Dieses Computeziel heißt [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). Beginnen Sie noch heute mit der Migration und Nutzung. Sie können über die entsprechenden [Python SDKs](../machine-learning/service/quickstart-create-workspace-with-python.md), die Befehlszeilenschnittstelle und das [Azure-Portal](../machine-learning/service/quickstart-get-started.md) mit dem Azure Machine Learning Service interagieren.

Wenn Sie von der Batch AI-Vorschauversion auf die allgemein verfügbare Version von Azure Machine Learning Service upgraden, stehen Ihnen benutzerfreundlichere Konzepte wie Estimatoren und Datenspeicher zur Verfügung. Darüber hinaus profitieren Sie von Azure-SLAs und Kundensupport für allgemein verfügbare Versionen.

Azure Machine Learning Service bietet zudem neue Funktionen wie automatisiertes maschinelles Lernen, Hyperparameteroptimierung und ML-Pipelines, die bei den meisten umfangreichen KI-Workloads hilfreich sind. Dank der Möglichkeit, ein trainiertes Modell ohne Wechsel zu einem separaten Dienst bereitzustellen, können Sie die Data Science-Schleife von der Datenaufbereitung (mit dem Datenaufbereitungs-SDK) bis hin zur Operationalisierung und Modellüberwachung abwickeln.

## <a name="start-migrating"></a>Starten der Migration
Um Unterbrechungen Ihrer Anwendungen zu vermeiden und von den neuesten Features zu profitieren, führen Sie vor dem 31. März 2019 folgende Schritte aus:

1. Erstellen Sie einen Azure Machine Learning Service-Arbeitsbereich, um loszulegen:
    + [Schnellstartanleitung für Python](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Schnellstartanleitung für das Azure-Portal](../machine-learning/service/quickstart-get-started.md)

1. Installieren Sie das [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) und das [SDK für die Datenaufbereitung](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py#install). 

1. Richten Sie eine Instanz von [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute) für das Trainieren von Modellen ein.

1. Aktualisieren Sie Ihre Skripts, sodass diese Azure Machine Learning Compute verwenden. Die folgenden Abschnitte zeigen, wie allgemeiner für Batch AI verwendeter Code mit dem Code für Azure Machine Learning zusammenhängt. 


## <a name="create-workspaces"></a>Erstellen von Arbeitsbereichen
Das Konzept der Initialisierung eines Arbeitsbereichs mithilfe einer Datei vom Typ „configuration.json“ in Azure Batch AI ähnelt der Verwendung einer Konfigurationsdatei in Azure Machine Learning Service.

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

Weitere Informationen zur Klasse für den Azure Machine Learning-Arbeitsbereich finden Sie in der [SDK-Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).


## <a name="create-compute-clusters"></a>Erstellen von Computeclustern
Azure Machine Learning unterstützt mehrere Computeziele. Einige davon werden durch den Dienst verwaltet, andere können an Ihren Arbeitsbereich angefügt werden (beispielsweise ein HDInsight-Cluster oder ein virtueller Remotecomputer). Weitere Informationen zu verschiedenen Computezielen finden Sie [hier](../machine-learning/service/how-to-set-up-training-targets.md). Das Konzept der Erstellung eines Azure Batch AI-Computeclusters entspricht der Erstellung eines AmlCompute-Clusters in Azure Machine Learning Service. Bei der Amlcompute-Erstellung wird eine Computekonfiguration verwendet, was mit der Übergabe von Parametern in Azure Batch AI vergleichbar ist. Hinweis: Für den AmlCompute-Cluster ist die automatische Skalierung standardmäßig aktiviert. In Azure Batch AI ist sie standardmäßig deaktiviert.

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

## <a name="monitor-status-of-your-cluster"></a>Überwachen des Status Ihres Clusters
Dies ist in Azure Machine Learning Service einfacher, wie hier zu sehen:

Vorgehensweise für **Batch AI**:

```python
cluster = client.clusters.get(cfg.resource_group, cfg.workspace, cluster_name)
utils.cluster.print_cluster_status(cluster)
```

Vorgehensweise für **Azure Machine Learning Service**:

```python
gpu_cluster.get_status().serialize()
```

## <a name="get-reference-to-a-storage-account"></a>Abrufen eines Verweises auf ein Speicherkonto
Das Konzept eines Datenspeichers (etwa ein Blob) wird in Azure Machine Learning Service mithilfe des DataStore-Objekts vereinfacht. Standardmäßig erstellt Ihr Azure Machine Learning Service-Arbeitsbereich ein Speicherkonto. Im Rahmen der Arbeitsbereichserstellung können Sie jedoch auch Ihren eigenen Speicher anfügen. 

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

Weitere Informationen zum Registrieren zusätzlicher Speicherkonten sowie zum Abrufen eines Verweises auf einen anderen registrierten Datenspeicher finden Sie in der [Dokumentation für Azure Machine Learning Service](../machine-learning/service/how-to-access-data.md#access).


## <a name="download-and-upload-data"></a>Herunterladen und Hochladen von Daten 
Bei beiden Diensten lassen sich die Daten mithilfe des obigen Datenspeicherverweises problemlos in das Speicherkonto hochladen. Für Azure Batch AI stellen wir auch das Trainingsskript als Teil der Dateifreigabe bereit. Sie werden jedoch sehen, wie Sie es im Fall von Azure Machine Learning Service als Teil Ihrer Auftragskonfiguration angeben können.

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

## <a name="create-experiments"></a>Erstellen von Experimenten
Wie bereits erwähnt gleicht das Experimentkonzept von Azure Machine Learning Service dem Konzept in Azure Batch AI. Jedes Experiment kann einzelne Ausführungen umfassen, was mit Aufträgen in Azure Batch AI vergleichbar ist. In Azure Machine Learning Service ist außerdem eine Hierarchie mit übergeordneten und untergeordneten Ausführungen möglich.

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


## <a name="submit-jobs"></a>Übermitteln von Aufträgen
Nach der Experimenterstellung haben Sie mehrere Möglichkeiten, um eine Ausführung zu übermitteln. In diesem Beispiel möchten wir ein Deep Learning-Modell mit TensorFlow erstellen und dabei einen Azure Machine Learning Service-Estimator verwenden. Ein [Estimator](../machine-learning/service/how-to-train-ml-models.md) ist einfach eine Wrapperfunktion für die zugrunde liegende Ausführungskonfiguration und dient dazu, die Übermittlung von Ausführungen zu vereinfachen. Er wird derzeit nur für Pytorch und TensorFlow unterstützt. Das Datenspeicherkonzept vereinfacht zudem die Angabe der Einbindungspfade. 

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

In Azure Batch AI wird der eigentliche Auftrag im Rahmen der Erstellungsfunktion übermittelt.

```python
job_name = datetime.utcnow().strftime('tf_%m_%d_%Y_%H%M%S')
job = client.jobs.create(cfg.resource_group, cfg.workspace, experiment_name, job_name, parameters).result()
print('Created Job {0} in Experiment {1}'.format(job.name, experiment.name))
```

Umfassende Informationen zu diesem Trainingscodeausschnitt (einschließlich der Datei „mnist_replica.py“, die weiter oben in die Dateifreigabe hochgeladen wurde) finden Sie im [GitHub-Repository mit dem Azure Batch AI-Beispielnotebook](https://github.com/Azure/BatchAI/tree/2238607d5a028a0c5e037168aefca7d7bb165d5c/recipes/TensorFlow/TensorFlow-GPU-Distributed).

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

Umfassende Informationen zu diesem Trainingscodeausschnitt (einschließlich der Datei „tf_mnist_replica.py“) finden Sie im [GitHub-Repository mit dem Azure Machine Learning Service-Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/distributed-tensorflow-with-parameter-server). Der Datenspeicher selbst kann entweder in die einzelnen Knoten eingebunden werden, oder die Trainingsdaten können direkt auf den Knoten heruntergeladen werden. Ausführlichere Informationen zu Datenspeicherverweisen in Ihrem Estimator finden Sie in der [Dokumentation für Azure Machine Learning Service](../machine-learning/service/how-to-access-data.md#access). 

Eine Ausführung in Azure Machine Learning Service wird mithilfe der Übermittlungsfunktion übermittelt.

```python
run = experiment.submit(estimator)
print(run)
```

Eine weitere Möglichkeit zum Angeben von Parametern für Ihre Ausführung ist die Verwendung einer Ausführungskonfiguration. Dies ist insbesondere hilfreich, wenn Sie eine benutzerdefinierte Trainingsumgebung definieren möchten. Weitere Informationen finden Sie in [diesem AmlCompute-Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-amlcompute/train-on-amlcompute.ipynb). 

## <a name="monitor-runs"></a>Überwachen von Ausführungen
Nach der Übermittlung einer Ausführung können Sie entweder warten, bis sie abgeschlossen ist, oder sie in Azure Machine Learning Service mithilfe praktischer Jupyter-Widgets überwachen, die Sie direkt in Ihrem Code aufrufen können. Darüber hinaus können Sie auch den Kontext einer beliebigen vorherigen Ausführung abrufen, indem Sie die verschiedenen Experimente in einem Arbeitsbereich und die einzelnen Ausführungen innerhalb der einzelnen Experimente durchlaufen.

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



## <a name="edit-clusters"></a>Bearbeiten von Clustern
Das Löschen eines Clusters ist ganz einfach. Mit Azure Machine Learning Service können Sie zudem einen Cluster über das Notebook aktualisieren, um ihn auf eine höhere Knotenanzahl zu skalieren, oder die Wartezeit erhöhen, nach der der Cluster im Leerlauf herunterskaliert wird. Die VM-Größe des Clusters kann dagegen nicht geändert werden, da hierzu letztendlich eine neue Bereitstellung am Back-End erforderlich wäre.

Vorgehensweise für **Batch AI**:
```python
_ = client.clusters.delete(cfg.resource_group, cfg.workspace, cluster_name)
```

Vorgehensweise für **Azure Machine Learning Service**:

```python
gpu_cluster.delete()

gpu_cluster.update(min_nodes=2, max_nodes=4, idle_seconds_before_scaledown=600)
```

## <a name="get-support"></a>Support

Batch AI wird voraussichtlich am 31. März ausgemustert. Schon jetzt können für den Dienst keine neuen Abonnements mehr registriert werden (es sei denn, es wurde zuvor über den Support eine Ausnahme beantragt und genehmigt).  Sollten Sie Fragen oder Feedback in Verbindung mit der Migration zu Azure Machine Learning Service haben, [wenden Sie sich an uns](mailto:AzureBatchAITrainingPreview@service.microsoft.com).

Azure Machine Learning Service ist ein allgemein verfügbarer Dienst. Er verfügt somit über eine verbindliche SLA und bietet eine Auswahl verschiedener Supportpläne.

Die Nutzung der Azure-Infrastruktur kostet für Azure Machine Learning Service voraussichtlich gleich viel wie für den Azure Batch AI-Dienst, da in beiden Fällen lediglich der Preis für die zugrunde liegenden Computeressourcen in Rechnung gestellt wird. Weitere Informationen finden Sie unter [Azure-Preisrechner](https://azure.microsoft.com/pricing/details/machine-learning-service/).

Die regionale Verfügbarkeit für die beiden Dienste können Sie im [Azure-Portal](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all) anzeigen.


## <a name="next-steps"></a>Nächste Schritte

+ Lesen Sie den [Überblick über den Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Konfigurieren Sie Computeziele zum Trainieren von Modellen](../machine-learning/service/how-to-set-up-training-targets.md) mit dem Azure Machine Learning Service.

+ Lesen Sie die [Azure-Roadmap](https://azure.microsoft.com/updates/), um mehr über Updates anderer Azure-Dienste zu erfahren.
