---
title: Computeziele für das Modelltraining
titleSuffix: Azure Machine Learning service
description: Konfigurieren Sie die Trainingsumgebungen (Computeziele) für das Machine Learning-Modelltraining. Sie können problemlos zwischen Trainingsumgebungen wechseln. Beginnen Sie lokal mit dem Trainieren, und wenn ein horizontales Hochskalieren erforderlich ist, wechseln Sie zu einem cloudbasierten Computeziel. Databricks
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 664d56daf3e70e2e5699d0c07331c466c60e06c5
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338857"
---
# <a name="set-up-compute-targets-for-model-training"></a>Einrichten von Computezielen für das Modelltraining

Mit Azure Machine Learning Service können Sie Ihr Modell in unterschiedlichen Computeressourcen trainieren. Diese Computeressourcen, die sogenannten __Computeziele__, können lokal oder in der Cloud gehostet sein. In diesem Dokument erfahren Sie mehr über die unterstützten Computeziele und deren Verwendung.

Ein Computeziel ist eine Ressource, in der Ihr Trainingsskript ausgeführt oder Ihr Modell bei der Bereitstellung als Webdienst gehostet wird. Sie können Computeziele mit dem Azure Machine Learning SDK, im Azure-Portal oder über die Azure-Befehlszeilenschnittstelle (Azure CLI) erstellen und verwalten. Wenn Sie Computeziele haben, die über einen anderen Dienst (z. B. einen HDInsight-Cluster) erstellt wurden, können Sie diese an Ihren Azure Machine Learning Service-Arbeitsbereich anfügen, um sie verwenden zu können.

Azure Machine Learning unterstützt drei allgemeine Kategorien von Computezielen:

* __Lokal__: Ihr lokaler Computer oder eine cloudbasierte VM, den bzw. die Sie als Entwicklungs-/Experimentierumgebung verwenden. 

* __Verwaltete Computeressourcen__: Azure Machine Learning Compute ist ein Computeangebot, das von Azure Machine Learning Service verwaltet wird. Es ermöglicht Ihnen das einfache Erstellen von Computezielen mit einem einzelnen oder mehreren Knoten zum Trainieren, Testen und Ausführen von Batchrückschlüssen.

* __Angefügte Computeressourcen__: Sie können auch Ihre eigenen Azure-Cloudcomputeressourcen verwenden und diese an Azure Machine Learning anfügen. Im Folgenden erfahren Sie mehr über die unterstützten Computezieltypen und ihre Verwendung.


## <a name="supported-compute-targets"></a>Unterstützte Computeziele

Azure Machine Learning Service bietet unterschiedliche Unterstützung für die verschiedenen Computeziele. Ein typischer Modellentwicklungslebenszyklus beginnt mit der Entwicklung/Experimenten mit einer kleinen Menge von Daten. In dieser Phase empfehlen wir die Verwendung einer lokalen Umgebung. Verwenden Sie beispielsweise Ihren lokalen Computer oder eine cloudbasierte VM. Wenn Sie Ihr Training zur Verwendung größerer Datasets zentral hochskalieren oder sich für ein verteiltes Training entscheiden, empfehlen wir, mit Azure Machine Learning Compute einen Cluster mit einem einzelnen oder mehreren Knoten zu erstellen, der bei jeder Übermittlung einer Ausführung automatisch skaliert wird. Sie können auch Ihre eigene Computeressource anfügen. Die Unterstützung für verschiedene Szenarien variiert jedoch wie unten beschrieben:

|Computeziel| GPU-Beschleunigung | Automatisierte Hyperparameteroptimierung | Automatisiertes maschinelles Lernen | Pipelineunterstützung|
|----|:----:|:----:|:----:|:----:|
|[Lokaler Computer](#local)| Vielleicht | &nbsp; | ✓ | &nbsp; |
|[Azure Machine Learning Compute](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Remote-VM](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | ✓ | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ Azure Databricks und Azure Data Lake Analytics können __nur__ in einer Pipeline verwendet werden. Weitere Informationen zu Pipelines finden Sie unter [Pipelines in Azure Machine Learning](concept-ml-pipelines.md).

> [!IMPORTANT]
> Azure Machine Learning Compute muss in einem Arbeitsbereich erstellt werden. Es ist nicht möglich, vorhandene Instanzen an einen Arbeitsbereich anzufügen.
>
> Andere Computeziele müssen außerhalb von Azure Machine Learning erstellt und dann an Ihren Arbeitsbereich angefügt werden.

> [!NOTE]
> Für einige Computeziele sind beim Trainieren eines Modells Docker-Containerimages erforderlich. Das GPU-Basisimage darf nur für Microsoft Azure-Dienste verwendet werden. Folgende Dienste sind für das Modelltraining verfügbar:
>
> * Azure Machine Learning Compute
> * Azure Kubernetes Service
> * Data Science Virtual Machine

## <a name="workflow"></a>Workflow

Beim Workflow zum Entwickeln und Bereitstellen eines Modells mit Azure Machine Learning werden die folgenden Schritte ausgeführt:

1. Entwickeln von Machine Learning-Trainingsskripts in Python.
1. Erstellen und Konfigurieren oder Anfügen eines vorhandenen Computeziels.
1. Übermitteln der Trainingsskripts an das Computeziel.
1. Überprüfen der Ergebnisse, um das beste Modell zu ermitteln.
1. Registrieren des Modells in der Modellregistrierung.
1. Bereitstellen des Modells.

> [!IMPORTANT]
> Ihr Trainingsskript ist nicht an ein bestimmtes Computeziel gebunden. Sie können zunächst auf Ihrem lokalen Computer mit dem Trainieren beginnen und anschließend das Computeziel wechseln, ohne dass Sie das Trainingsskript umschreiben müssen.

> [!TIP]
> Wenn Sie ein Computeziel mit Ihrem Arbeitsbereich verknüpfen (durch Erstellen einer verwalteten Computeressource oder Anfügen einer vorhandenen Computeressource), müssen Sie einen Namen für Ihr Computeziel angeben. Dieser Name muss 2 bis 16 Zeichen lang sein.

Der Wechsel von einem Computeziel zu einem anderen umfasst das Erstellen einer [Laufzeitkonfiguration](concept-azure-machine-learning-architecture.md#run-configuration). Die Laufzeitkonfiguration definiert, wie das Skript auf dem Computeziel ausgeführt werden soll.

## <a name="training-scripts"></a>Trainingsskripts

Wenn Sie eine Trainingsausführung starten, wird eine Momentaufnahme des Verzeichnisses, das Ihre Trainingsskripts enthält, erstellt und an das Computeziel gesendet. Weitere Informationen finden Sie auf unter [Snapshots](concept-azure-machine-learning-architecture.md#snapshot) (Momentaufnahmen).

## <a id="local"></a>Lokaler Computer

Beim lokalen Trainieren verwenden Sie das SDK zum Übermitteln des Trainingsvorgangs. Sie können eine vom Benutzer oder vom System verwaltete Umgebung zum Trainieren verwenden.

### <a name="user-managed-environment"></a>Vom Benutzer verwaltete Umgebung

In einer vom Benutzer verwalteten Umgebung müssen Sie sicherstellen, dass alle erforderlichen Pakete in der Python-Umgebung verfügbar sind, in der Sie das Skript ausführen möchten. Der folgende Codeausschnitt ist ein Beispiel für die Trainingskonfiguration für eine vom Benutzer verwaltete Umgebung:

```python
from azureml.core.runconfig import RunConfiguration

# Editing a run configuration property on-fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Vom System verwaltete Umgebung

Vom System verwaltete Umgebungen hängen davon ab, dass Conda die Abhängigkeiten verwaltet. Conda erstellt eine Datei mit dem Namen `conda_dependencies.yml`, die eine Liste der Abhängigkeiten enthält. Sie können anschließend vom System die Erstellung einer neuen Conda-Umgebung anfordern und Ihre Skripts darin ausführen. Vom System verwaltete Umgebungen können später wiederverwendet werden, sofern die `conda_dependencies.yml`-Dateien unverändert bleiben. 

Bis eine neue Umgebung erstmals eingerichtet ist, können einige Minuten vergehen, je nach der Größe der erforderlichen Abhängigkeiten. Der folgende Codeausschnitt zeigt die Erstellung einer vom System verwalteten Umgebung, die von scikit-learn abhängig ist:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Azure Machine Learning Compute

Azure Machine Learning Compute ist eine verwaltete Computeinfrastruktur, die dem Benutzer das einfache Erstellen von Computezielen mit einem einzigen oder mehreren Knoten ermöglicht. Das Computeziel wird __in Ihrer Arbeitsbereichsregion__ erstellt und ist eine Ressource, die für andere Benutzer im Arbeitsbereich freigegeben werden kann. Es wird automatisch zentral hochskaliert, wenn ein Auftrag übermittelt wird, und kann in einem virtuellen Azure-Netzwerk platziert werden kann. Das Computeziel wird in einer __Containerumgebung__ ausgeführt und verpackt die Abhängigkeiten Ihres Modells in einem Docker-Container.

Sie können Azure Machine Learning Compute verwenden, um den Trainingsprozess auf einen Cluster von CPU- oder GPU-Computeknoten in der Cloud zu verteilen. Weitere Informationen zu den VM-Größen mit GPUs finden Sie unter [Für GPU optimierte VM-Größen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

> [!NOTE]
> Bei Azure Machine Learning Compute gelten bestimmte Standardgrenzwerte, beispielsweise für die Anzahl von Kernen, die zugeordnet werden können. Weitere Informationen finden Sie im Dokument [Verwalten und Anfordern von Kontingenten für Azure-Ressourcen](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

Sie können eine Azure Machine Learning Compute-Umgebung bedarfsgesteuert beim Planen einer Ausführung oder als persistente Ressource erstellen.

### <a name="run-based-creation"></a>Ausführungsbasierte Erstellung

Sie können eine Azure Machine Learning Compute-Umgebung als Computeziel zur Laufzeit erstellen. Das Computeziel wird in diesem Fall automatisch für die Ausführung erstellt, auf die in der Ausführungskonfiguration angegebene maximale Knotenanzahl (max_nodes) zentral hochskaliert und nach Abschluss der Ausführung __automatisch gelöscht__.

> [!IMPORTANT]
> Die ausführungsbasierte Erstellung von Azure Machine Learning Compute ist zurzeit als Vorschauversion verfügbar. Verwenden Sie die ausführungsbasierte Erstellung nicht, wenn Sie die Hyperparameteroptimierung oder automatisiertes maschinelles Lernen nutzen. Falls Sie die Hyperparameteroptimierung oder automatisiertes maschinelles Lernen verwenden müssen, erstellen Sie die Azure Machine Learning Compute-Umgebung, bevor Sie eine Ausführung übermitteln.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

#Let us first list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use AmlCompute to execute script.
run_config.target = "amlcompute"

# AmlCompute will be created in the same region as workspace. Set vm size for AmlCompute from the list returned above
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Persistente Computeressource (Standard)

Eine persistente Azure Machine Learning Compute-Ressource kann für mehrere Aufträge wiederverwendet werden. Sie kann für andere Benutzer im Arbeitsbereich freigegeben werden und bleibt zwischen Aufträgen erhalten.

Um eine persistente Azure Machine Learning Compute-Ressource zu erstellen, geben Sie die Parameter `vm_size` und `max_nodes` an. Azure Machine Learning verwendet dann intelligente Standardwerte für die restlichen Parameter.  Das Computeziel wird beispielsweise so eingerichtet, dass es bei Nichtverwendung automatisch auf null Knoten zentral herunterskaliert wird und je nach Bedarf dedizierte VMs zum Ausführen Ihrer Aufträge erstellt. 

* **vm_size**: VM-Familie der von Azure Machine Learning Compute erstellten Knoten.
* **max_nodes**: Maximale Anzahl von Knoten, auf die das Computeziel bei der Ausführung eines Auftrags in Azure Machine Learning Compute automatisch skaliert wird.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Persistente Computeressource (Erweitert)

Sie können beim Erstellen einer Azure Machine Learning Compute-Ressource auch mehrere erweiterte Eigenschaften konfigurieren.  Mithilfe dieser Eigenschaften können Sie einen persistenten Cluster mit einer festen Größe oder in einem vorhandenen virtuellen Azure-Netzwerk in Ihrem Abonnement erstellen.

Zusätzlich zu `vm_size` und `max_nodes` können Sie die folgenden Eigenschaften verwenden:

* **min_nodes**: Mindestanzahl von Knoten (Standardwert: 0), auf die das Computeziel während der Ausführung eines Auftrags in Azure Machine Learning Compute automatisch zentral herunterskaliert wird.
* **vm_priority**: Sie können beim Erstellen einer Azure Machine Learning Compute-Ressource zwischen dedizierten VMs (dedicated) und VMs mit niedriger Priorität (lowpriority) wählen. Der Standardwert dieser Eigenschaft ist „dedicated“. VMs mit niedriger Priorität verwenden überschüssige Kapazität von Azure und sind daher kostengünstiger. Bei ihnen besteht jedoch das Risiko, dass Ihre Ausführung vorzeitig entfernt wird.
* **idle_seconds_before_scaledown**: Leerlaufzeit (Standardwert: 120 Sekunden), die nach Abschluss der Ausführung gewartet werden soll, bevor das Computeziel automatisch auf „min_nodes“ zentral herunterskaliert wird.
* **vnet_resourcegroup_name**: Ressourcengruppe des __vorhandenen__ virtuellen Netzwerks. Die Azure Machine Learning Compute-Ressource wird in diesem virtuellen Netzwerk erstellt.
* **vnet_name**: Name des virtuellen Netzwerks. Das virtuelle Netzwerk muss sich in derselben Region befinden wie Ihr Azure Machine Learning-Arbeitsbereich.
* **subnet_name**: Name des Subnetzes innerhalb des virtuellen Netzwerks. Azure Machine Learning Compute-Ressourcen werden IP-Adressen aus diesem Subnetzbereich zugewiesen.

> [!TIP]
> Beim Erstellen einer persistenten Azure Machine Learning Compute-Ressource können Sie auch die Eigenschaften der Ressource aktualisieren, beispielsweise „min_nodes“ oder „max_nodes“. Rufen Sie dazu einfach die `update()`-Funktion auf.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           vm_priority='lowpriority',
                                                           min_nodes=2,
                                                           max_nodes=4,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```


## <a id="vm"></a>Remote-VM

Azure Machine Learning bietet Ihnen auch die Möglichkeit, eine eigene Computeressource zu verwenden und diese an Ihren Arbeitsbereich anzufügen. Eine Remote-VM, auf die über Azure Machine Learning Service zugegriffen werden kann, ist ein solcher Ressourcentyp. Die Ressource kann entweder eine Azure-VM oder ein Remoteserver in Ihrer Organisation oder lokalen Bereitstellung sein. Durch Angabe der IP-Adresse und Anmeldeinformationen (Benutzername/Kennwort oder SSH-Schlüssel) können Sie jede VM, auf die zugegriffen werden kann, für Remoteausführungen verwenden.
Sie können eine systemseitig erstellte Conda-Umgebung, eine bereits vorhandene Python-Umgebung oder einen Docker-Container verwenden. Die Ausführung mit einem Docker-Container setzt voraus, dass die Docker-Engine auf der VM ausgeführt wird. Diese Funktion ist besonders nützlich, wenn Sie eine cloudbasierte Entwicklungs-/Experimentierumgebung nutzen möchten, die Ihnen mehr Flexibilität bietet als Ihr lokaler Computer.

> [!TIP]
> Für dieses Szenario empfehlen wir, Data Science Virtual Machine (DSVM) als Azure-VM zu verwenden. DSVM ist eine vorkonfigurierte Data Science- und KI-Entwicklungsumgebung in Azure mit einer eigens zusammengestellten Auswahl von Tools und Frameworks für den gesamten Lebenszyklus der Machine Learning-Entwicklung. Weitere Informationen zur Verwendung von Data Science Virtual Machine mit Azure Machine Learning finden Sie im Dokument [Konfigurieren einer Entwicklungsumgebung](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

> [!WARNING]
> Azure Machine Learning unterstützt nur virtuelle Computer, die Ubuntu ausführen. Beim Erstellen oder Auswählen eines virtuellen Computers muss dieser Ubuntu verwenden.

Die folgenden Schritte verwenden das SDK zum Konfigurieren einer Data Science Virtual Machine (DSVM) als Trainingsziel:

1. Um einen vorhandenen virtuellen Computer als Computeziel anzufügen, müssen Sie den vollqualifizierten Domänennamen, den Anmeldenamen und das Kennwort für den virtuellen Computer bereitstellen.  Ersetzen Sie in diesem Beispiel ```<fqdn>``` durch den öffentlichen vollqualifizierten Domänennamen des virtuellen Computers oder die öffentliche IP-Adresse. Ersetzen Sie ```<username>``` und ```<password>``` durch den SSH-Benutzer und das Kennwort für den virtuellen Computer:

    ```python
    from azureml.core.compute import RemoteCompute, ComputeTarget
    
    # Create compute config.
    attach_config = RemoteCompute.attach_configuration(address = "ipaddress",
                                                       ssh_port=22,
                                                       username='<username>',
                                                       password="<password>")
    # If using SSH instead of a password, use this:
    #                                                  ssh_port=22,
    #                                                   username='<username>',
    #                                                   password=None,
    #                                                   private_key_file="path-to-file",
    #                                                   private_key_passphrase="passphrase")

    # Attach the compute
    compute = ComputeTarget.attach(ws, "attach-dsvm", attach_config)

    compute.wait_for_completion(show_output=True)
    ```

1. Erstellen Sie eine Konfiguration für das DSVM-Computeziel. Zum Erstellen und Konfigurieren der Trainingsumgebung in DSVM werden Docker und Conda verwendet:

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load the "cpu-dsvm.runconfig" file (created by the above attach operation) in memory
    run_config = RunConfiguration(framework = "python")

    # Set compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use CPU base image
    # If you want to use GPU in DSVM, you must also use GPU base Docker image azureml.core.runconfig.DEFAULT_GPU_IMAGE
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>Azure Databricks

Azure Databricks ist eine Apache Spark-basierte Umgebung in der Azure-Cloud. Sie kann beim Trainieren von Modellen mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

> [!IMPORTANT]
> Ein Azure Databricks-Computeziel kann nur in einer Machine Learning-Pipeline verwendet werden.
>
> Sie müssen einen Azure Databricks-Arbeitsbereich erstellen, um ihn zum Trainieren Ihres Modells zu verwenden. Informationen zum Erstellen dieser Ressource finden Sie im Dokument [Ausführen eines Spark-Auftrags in Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Um Azure Databricks als Computeziel anzufügen, müssen Sie das Azure Machine Learning SDK verwenden und die folgenden Informationen angeben:

* __Computename__: Der Name, der dieser Computeressource zugewiesen werden soll.
* __Name des Databricks-Arbeitsbereichs__: Der Name des Azure Databricks-Arbeitsbereichs.
* __Zugriffstoken__: Das zur Authentifizierung bei Azure Databricks verwendete Zugriffstoken. Informationen zum Generieren eines Zugriffstokens finden Sie im Dokument [Authentifizierung](https://docs.azuredatabricks.net/api/latest/authentication.html).

Der folgende Code veranschaulicht, wie Azure Databricks als Computeziel angefügt wird:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get("AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get("AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group = databricks_resource_group,
                                                           workspace_name = databricks_workspace_name,
                                                           access_token = databricks_access_token)
    databricks_compute = ComputeTarget.attach(
             ws,
             databricks_compute_name,
             attach_config
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics ist eine umfangreiche Datenanalyseplattform in der Azure-Cloud. Sie kann beim Trainieren von Modellen mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

> [!IMPORTANT]
> Ein Azure Data Lake Analytics-Computeziel kann nur in einer Machine Learning-Pipeline verwendet werden.
>
> Sie müssen ein Azure Data Lake Analytics-Konto erstellen, um es zum Trainieren Ihres Modells zu verwenden. Informationen zum Erstellen dieser Ressource finden Sie im Dokument [Erste Schritte mit Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Um Data Lake Analytics als Computeziel anzufügen, müssen Sie das Azure Machine Learning SDK verwenden und die folgenden Informationen angeben:

* __Computename__: Der Name, der dieser Computeressource zugewiesen werden soll.
* __Ressourcengruppe__: Die Ressourcengruppe, die das Data Lake Analytics-Konto enthält.
* __Kontoname__: Der Name des Data Lake Analytics-Kontos.

Der folgende Code veranschaulicht, wie Data Lake Analytics als Computeziel angefügt wird:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get("AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get("AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    # create attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning-Pipelines können nur ausgeführt werden, wenn die Daten im Standarddatenspeicher des Data Lake Analytics-Kontos gespeichert werden. Wenn die Daten, die Sie verwenden möchten, in einem nicht standardmäßigen Speicher gespeichert sind, können Sie die Daten mithilfe von [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) vor dem Trainieren kopieren.

## <a id="hdinsight"></a>Azure HDInsight 

HDInsight ist eine beliebte Plattform für Big Data-Analysen. Sie stellt Apache Spark bereit, das zum Trainieren Ihres Modells verwendet werden kann.

> [!IMPORTANT]
> Erstellen Sie den HDInsight-Cluster, bevor Sie ihn zum Trainieren Ihres Modells verwenden. Informationen zum Erstellen von Spark auf einem HDInsight-Cluster finden Sie unter [Schnellstart: Erstellen eines Spark-Clusters in HDInsight mithilfe einer Vorlage](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> Beim Erstellen des Clusters müssen Sie einen SSH-Benutzernamen und ein Kennwort angeben. Notieren Sie diese Werte, da Sie sie benötigen, wenn Sie HDInsight als Computeziel verwenden.
>
> Sobald der Cluster erstellt wurde, hat er den vollqualifizierten Domänennamen (FQDN) `<clustername>.azurehdinsight.net`, wobei `<clustername>` der Name ist, die Sie für den Cluster angegeben haben. Sie benötigen diese Adresse (oder die öffentliche IP-Adresse des Clusters), um sie als Computeziel zu verwenden.

Um HDInsight als Computeziel zu konfigurieren, müssen Sie den vollqualifizierten Domänennamen, den Clusteranmeldenamen und das Kennwort für den HDInsight-Cluster bereitstellen. Im folgenden Beispiel wird das SDK zum Anfügen eines Clusters zu Ihrem Arbeitsbereich verwendet. Ersetzen Sie in diesem Beispiel `<fqdn>` durch den öffentlichen vollqualifizierten Domänennamen des Clusters oder die öffentliche IP-Adresse. Ersetzen Sie `<username>` und `<password>` durch den SSH-Benutzer und das Kennwort für den Cluster:

> [!NOTE]
> Besuchen Sie das Azure-Portal, und wählen Sie Ihren HDInsight-Cluster aus, um den FQDN für Ihren Cluster zu finden. Im Abschnitt __Übersicht__ ist der FQDN Teil des Eintrags __URL__. Entfernen Sie einfach `https://` am Anfang des Werts.
>
> ![Screenshot der HDInsight-Clusterübersicht mit hervorgehobenem URL-Eintrag](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute, ComputeTarget

try:
    # Attaches a HDInsight cluster as a compute target.
    attach_config = HDInsightCompute.attach_configuration(address = "fqdn-or-ipaddress",
                                                          ssh_port = 22,
                                                          username = "username",
                                                          password = None, #if using ssh key
                                                          private_key_file = "path-to-key-file",
                                                          private_key_phrase = "key-phrase")
    compute = ComputeTarget.attach(ws, "myhdi", attach_config)
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure HDInsight run
# load the runconfig object from the "myhdi.runconfig" file generated by the attach operaton above.
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# ask system to prepare the conda environment automatically when used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-run"></a>Übermitteln der Trainingsausführung

Eine Trainingsausführung kann auf zwei Arten übermittelt werden:

* Übermitteln eines `ScriptRunConfig`-Objekts
* Übermitteln eines `Pipeline`-Objekts

> [!IMPORTANT]
> Die Azure Databricks- und Azure Data Lake Analytics-Computeziele können nur in einer Pipeline verwendet werden.
> Das lokale Computeziel kann nicht in einer Pipeline verwendet werden.

### <a name="submit-using-scriptrunconfig"></a>Übermitteln mithilfe von `ScriptRunConfig`

Das Codemuster zum Übermitteln einer Trainingsausführung mithilfe von `ScriptRunConfig` ist unabhängig vom Computeziel immer gleich:

* Erstellen Sie ein `ScriptRunConfig`-Objekt mithilfe der Ausführungskonfiguration für das Computeziel.
* Übermitteln Sie die Ausführung.
* Warten Sie, bis die Ausführung abgeschlossen ist.

Im folgenden Beispiel wird die Konfiguration für das vom System verwaltete lokale Computeziel verwendet, die weiter oben in diesem Dokument erstellt wurde:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="submit-using-a-pipeline"></a>Übermitteln über eine Pipeline

Das Codemuster zum Übermitteln einer Trainingsausführung über eine Pipeline ist unabhängig vom Computeziel immer gleich:

* Fügen Sie der Pipeline einen Schritt für die Computeressource hinzu.
* Übermitteln Sie eine Ausführung über die Pipeline.
* Warten Sie, bis die Ausführung abgeschlossen ist.

Im folgenden Beispiel wird das Azure Databricks-Computeziel verwendet, das weiter oben in diesem Dokument erstellt wurde:

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)
# list of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Weitere Informationen zu Machine Learning-Pipelines finden Sie im Dokument [Pipelines und Azure Machine Learning](concept-ml-pipelines.md).

Beispiele für Jupyter Notebooks, die das Training über eine Pipeline veranschaulichen, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Computeanzeige und -einrichtung mithilfe des Azure-Portals

Sie können anzeigen, welche Computeziele Ihrem Arbeitsbereich aus dem Azure-Portal zugeordnet sind. Führen Sie die folgenden Schritte aus, um die Liste abzurufen:

1. Besuchen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zu Ihrem Arbeitsbereich.
2. Klicken Sie auf den Link __Compute__im Abschnitt __Anwendungen__.

    ![Registerkarte „Computer“ anzeigen](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Erstellen eines Computeziels

Führen Sie die oben genannten Schritte aus, um eine Liste von Computezielen anzuzeigen, und führen Sie dann die folgenden Schritte aus, um ein Computeziel zu erstellen:

1. Klicken Sie auf das Symbol __+__, um ein Computeziel hinzuzufügen.

    ![Compute hinzufügen ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Geben Sie einen Namen für das Computeziel ein.
1. Wählen Sie **Machine Learning Compute** als Computetyp für das __Training__ aus.

    > [!IMPORTANT]
    > Sie können nur Azure Machine Learning Compute als verwaltete Computeressource für das Training erstellen.

1. Füllen Sie die erforderlichen Felder des Formulars aus, und geben Sie insbesondere die VM-Familie und die maximale Anzahl von Knoten für die Skalierung des Computeziels an. 
1. Klicken Sie auf __Erstellen__
1. Sie können den Status des Erstellungsvorgangs anzeigen, indem Sie das Computeziel in der Liste auswählen.

    ![Anzeigen der Computeliste](./media/how-to-set-up-training-targets/View_list.png)

1. Daraufhin werden die Details für das Computeziel angezeigt.

    ![Anzeigen der Details](./media/how-to-set-up-training-targets/compute-target-details.png)

1. Jetzt können Sie wie oben beschrieben eine Ausführung für diese Ziele übermitteln.


### <a name="reuse-existing-compute-in-your-workspace"></a>Wiederverwenden von vorhandenem Compute in Ihrem Arbeitsbereich

Führen Sie die oben genannten Schritte aus, um eine Liste von Computezielen anzuzeigen, und führen Sie dann die folgenden Schritte aus, um ein Computeziel wiederzuverwenden:

1. Klicken Sie auf das Symbol **+**, um ein Computeziel hinzuzufügen.
2. Geben Sie einen Namen für das Computeziel ein.
3. Wählen Sie den Computetyp aus, den Sie für das __Training__ anfügen möchten.

    > [!IMPORTANT]
    > Nicht alle Computetypen können im Portal angefügt werden.
    > Derzeit können folgende Typen für das Training angefügt werden:
    > 
    > * Remote-VM
    > * Databricks
    > * Data Lake Analytics
    > * HDInsight

1. Füllen Sie die erforderlichen Felder des Formulars aus.

    > [!NOTE]
    > Microsoft empfiehlt die Verwendung von SSH-Schlüsseln, da sie sicherer als Kennwörter sind. Kennwörter sind anfällig für Brute-Force-Angriffen, während SSH-Schlüssel auf kryptografischen Signaturen basieren. Informationen zum Erstellen von SSH-Schlüsseln für die Verwendung mit Azure Virtual Machines finden Sie in den folgenden Dokumenten:
    >
    > * [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Verwenden von SSH-Schlüsseln mit Windows in Azure]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Wählen Sie „Anfügen“ aus.
1. Sie können den Status des Anfügevorgangs anzeigen, indem Sie das Computeziel in der Liste auswählen.
1. Jetzt können Sie wie oben beschrieben eine Ausführung für diese Ziele übermitteln.

## <a name="examples"></a>Beispiele
Beispiele finden Sie in den folgenden Notebooks:
* [how-to-use-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)

* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* [What is the Azure Machine Learning SDK for Python?](https://aka.ms/aml-sdk) (Was ist das Azure Machine Learning-SDK für Python?)
* [Tutorial: Trainieren eines Modells](tutorial-train-models-with-aml.md)
* [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst)
* [Pipelines and Azure Machine Learning](concept-ml-pipelines.md) (Pipelines und Azure Machine Learning)
