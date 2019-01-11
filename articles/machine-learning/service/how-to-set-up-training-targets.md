---
title: Computeziele für das Modelltraining
titleSuffix: Azure Machine Learning service
description: Konfigurieren Sie die Trainingsumgebungen (Computeziele) für das Machine Learning-Modelltraining. Sie können problemlos zwischen Trainingsumgebungen wechseln. Beginnen Sie das Training lokal. Wenn ein horizontales Hochskalieren erforderlich ist, wechseln Sie zu einem cloudbasierten Computeziel.
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
ms.openlocfilehash: 9037f6d7602f186bc30e55acbc050280bca134ee
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794463"
---
# <a name="set-up-compute-targets-for-model-training"></a>Einrichten von Computezielen für das Modelltraining

Mit Azure Machine Learning Service können Sie Ihr Modell in unterschiedlichen Computeressourcen trainieren. Diese Computeressourcen, die sogenannten __Computeziele__, können lokal oder in der Cloud gehostet sein. In diesem Artikel erfahren Sie mehr über die unterstützten Computeziele und deren Verwendung.

Ein Computeziel ist eine Ressource, in der Ihr Trainingsskript ausgeführt oder Ihr Modell bei der Bereitstellung als Webdienst gehostet wird. Sie können Computeziele mit dem Azure Machine Learning SDK, im Azure-Portal oder über die Azure-Befehlszeilenschnittstelle (Azure CLI) erstellen und verwalten. Wenn Sie Computeziele haben, die über einen anderen Dienst (z. B. einen Azure HDInsight-Cluster) erstellt wurden, können Sie diese an Ihren Azure Machine Learning Service-Arbeitsbereich anfügen, um sie verwenden zu können.

Azure Machine Learning unterstützt drei allgemeine Kategorien von Computezielen:

* __Lokal__: Ihr lokaler Computer oder ein cloudbasierter virtueller Computer (VM), den Sie als Entwicklungs- und Experimentierumgebung verwenden. 
* __Verwaltete Computeressourcen__: Azure Machine Learning Compute ist ein Computeangebot, das von Azure Machine Learning Service verwaltet wird. Es ermöglicht Ihnen das einfache Erstellen von Computezielen mit einem einzelnen oder mehreren Knoten zum Trainieren, Testen und Ausführen von Batchrückschlüssen.
* __Angefügte Computeressourcen__: Sie können auch Ihre eigenen Azure-Cloudcomputeressourcen verwenden und diese an Azure Machine Learning anfügen. In den folgenden Abschnitten erfahren Sie mehr über die unterstützten Computezieltypen und ihre Verwendung.


## <a name="supported-compute-targets"></a>Unterstützte Computeziele

Azure Machine Learning Service bietet unterschiedliche Unterstützung für die verschiedenen Computeziele. Ein typischer Lebenszyklus der Modellentwicklung beginnt mit der Entwicklung und dem Experimenten mit einer kleinen Menge von Daten. In dieser Phase wird empfohlen, dass Sie eine lokale Umgebung wie Ihren lokalen Computer oder eine cloudbasierte VM verwenden. Wenn Sie Ihr Training zur Verwendung größerer Datasets zentral hochskalieren oder sich für ein verteiltes Training entscheiden, erstellen Sie mithilfe einer Azure Machine Learning Compute-Umgebung einen Cluster mit einem einzelnen oder mehreren Knoten, der bei jeder Übermittlung einer Ausführung automatisch skaliert wird. Sie können auch Ihre eigene Computeressource anfügen. Die Unterstützung für verschiedene Szenarien variiert jedoch wie in der folgenden Tabelle beschrieben:

|Computeziel| GPU-Beschleunigung | Automatisiert<br/> Hyperparameteroptimierung | Automatisiert</br> Machine Learning | Pipelineunterstützung|
|----|:----:|:----:|:----:|:----:|
|[Lokaler Computer](#local)| Vielleicht | &nbsp; | ✓ | &nbsp; |
|[Azure Machine Learning Compute](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Remote-VM](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | ✓ | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ _Azure Databricks und Azure Data Lake Analytics können nur in einer Pipeline verwendet werden._<br/>
> Weitere Informationen zu Pipelines finden Sie unter [Pipelines in Azure Machine Learning](concept-ml-pipelines.md).
>
> Die Azure Machine Learning Compute-Umgebung muss in einem Arbeitsbereich erstellt werden. Es ist nicht möglich, vorhandene Instanzen an einen Arbeitsbereich anzufügen.
>
> Andere Computeziele müssen außerhalb von Azure Machine Learning erstellt und dann an Ihren Arbeitsbereich angefügt werden.
>
> Für einige Computeziele sind beim Training eines Modells Docker-Containerimages erforderlich. Das GPU-Basisimage darf nur für Microsoft Azure-Dienste verwendet werden. Folgende Dienste sind für das Modelltraining verfügbar:
> * Azure Machine Learning Compute
> * Azure Kubernetes Service
> * Windows Data Science Virtual Machine (DSVM)

## <a name="workflow"></a>Workflow

Beim Workflow zum Entwickeln und Bereitstellen eines Modells mit Azure Machine Learning werden die folgenden Schritte ausgeführt:

1. Entwickeln der Machine Learning-Trainingsskripts in Python.
1. Erstellen und Konfigurieren des Computeziels oder Anfügen eines vorhandenen Computeziels.
1. Übermitteln der Trainingsskripts an das Computeziel.
1. Überprüfen der Ergebnisse, um das beste Modell zu ermitteln.
1. Registrieren des Modells in der Modellregistrierung.
1. Bereitstellen des Modells.

> [!NOTE]
> Ihr Trainingsskript ist nicht an ein bestimmtes Computeziel gebunden. Sie können zunächst auf Ihrem lokalen Computer mit dem Trainieren beginnen und anschließend das Computeziel wechseln, ohne dass Sie das Trainingsskript umschreiben müssen.
> 
> Wenn Sie ein Computeziel mit Ihrem Arbeitsbereich verknüpfen, indem Sie eine verwaltete Computeressource erstellen oder eine vorhandene Computeressource anfügen, müssen Sie einen Namen für Ihr Computeziel angeben. Der Name muss 2 bis 16 Zeichen haben.

Um von einem Computeziel zu einem anderen zu wechseln, benötigen Sie eine [Laufzeitkonfiguration](concept-azure-machine-learning-architecture.md#run-configuration). Die Laufzeitkonfiguration definiert, wie das Skript auf dem Computeziel ausgeführt werden soll.

## <a name="training-scripts"></a>Trainingsskripts

Wenn Sie eine Trainingsausführung starten, wird eine Momentaufnahme des Verzeichnisses, das Ihre Trainingsskripts enthält, erstellt und an das Computeziel gesendet. Weitere Informationen finden Sie unter [Momentaufnahmen](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Lokaler Computer

Beim lokalen Trainieren verwenden Sie das SDK zum Übermitteln des Trainingsvorgangs. Sie können zum Training eine vom Benutzer oder System verwaltete Umgebung verwenden.

### <a name="user-managed-environment"></a>Vom Benutzer verwaltete Umgebung

Stellen Sie in einer vom Benutzer verwalteten Umgebung sicher, dass alle erforderlichen Pakete in der Python-Umgebung verfügbar sind, in der Sie das Skript ausführen. Der folgende Codeausschnitt ist ein Beispiel der Trainingskonfiguration für eine vom Benutzer verwaltete Umgebung:

```python
from azureml.core.runconfig import RunConfiguration

# Edit a run configuration property on the fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# Choose a specific Python environment by pointing to a Python path. For example:
# run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Vom System verwaltete Umgebung

Vom System verwaltete Umgebungen hängen davon ab, dass Conda die Abhängigkeiten verwaltet. Conda erstellt eine Datei mit dem Namen **conda_dependencies.yml**, die eine Liste der Abhängigkeiten enthält. Sie können vom System die Erstellung einer neuen Conda-Umgebung anfordern und Ihre Skripts darin ausführen. Vom System verwaltete Umgebungen können später wiederverwendet werden, sofern die Datei „conda_dependencies.yml“ unverändert bleibt. 

Bis eine neue Umgebung erstmals eingerichtet ist, können je nach Größe der erforderlichen Abhängigkeiten einige Minuten vergehen. Der folgende Codeausschnitt veranschaulicht das Erstellen einer vom System verwalteten Umgebung, die von scikit-learn abhängig ist:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify the conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Azure Machine Learning Compute

Azure Machine Learning Compute ist eine verwaltete Compute-Infrastruktur, die dem Benutzer das einfache Erstellen von Computezielen mit einem oder mehreren Knoten ermöglicht. Das Computeziel wird in Ihrer Arbeitsbereichsregion als Ressource erstellt, die für andere Benutzer im Arbeitsbereich freigegeben werden kann. Es wird automatisch zentral hochskaliert, wenn ein Auftrag übermittelt wird, und kann in einem virtuellen Azure-Netzwerk platziert werden kann. Das Computeziel wird in einer Containerumgebung ausgeführt und packt die Abhängigkeiten Ihres Modells in einem Docker-Container.

Sie können Azure Machine Learning Compute verwenden, um den Trainingsprozess auf einen Cluster von CPU- oder GPU-Computeknoten in der Cloud zu verteilen. Weitere Informationen zu den VM-Größen mit GPUs finden Sie unter [Für GPU optimierte VM-Größen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

> [!NOTE]
> Bei Azure Machine Learning Compute gelten Standardgrenzwerte, beispielsweise für die Anzahl von Kernen, die zugeordnet werden können. Weitere Informationen finden Sie unter [Verwalten und Anfordern von Kontingenten für Azure-Ressourcen](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

Sie können eine Azure Machine Learning Compute-Umgebung bedarfsgesteuert beim Planen einer Ausführung oder als persistente Ressource erstellen.

### <a name="run-based-creation"></a>Ausführungsbasierte Erstellung

Sie können eine Azure Machine Learning Compute-Umgebung als Computeziel zur Laufzeit erstellen. Das Computeziel wird in diesem Fall automatisch für die Ausführung erstellt und auf die in der Ausführungskonfiguration angegebene maximale Knotenanzahl **max_nodes** zentral hochskaliert. Es wird nach Abschluss der Ausführung automatisch gelöscht.

> [!IMPORTANT]
> Die ausführungsbasierte Erstellung einer Azure Machine Learning Compute-Umgebung ist zurzeit in der Vorschauphase. Verwenden Sie die ausführungsbasierte Erstellung nicht, wenn Sie die Hyperparameteroptimierung oder automatisiertes maschinelles Lernen nutzen. Um die Hyperparameteroptimierung oder automatisiertes maschinelles Lernen zu verwenden, erstellen Sie die Azure Machine Learning Compute-Umgebung, bevor Sie eine Ausführung übermitteln.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

# First, list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# Create a new runconfig object
run_config = RunConfiguration()

# Signal that you want to use AmlCompute to execute the script
run_config.target = "amlcompute"

# AmlCompute is created in the same region as your workspace
# Set the VM size for AmlCompute from the list of supported_vmsizes
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Persistente Computeressource: Standard

Eine persistente Azure Machine Learning Compute-Umgebung kann für Aufträge wiederverwendet werden. Sie kann für andere Benutzer im Arbeitsbereich freigegeben werden und bleibt zwischen Aufträgen erhalten.

Um eine persistente Azure Machine Learning Compute-Umgebung zu erstellen, geben Sie die Parameter **vm_size** und **max_nodes** an. Azure Machine Learning verwendet dann für die restlichen Eigenschaften intelligente Standardwerte. Das Computeziel wird automatisch auf null Knoten zentral herunterskaliert und erstellt je nach Bedarf dedizierte VMs zum Ausführen Ihrer Aufträge. 

* **vm_size**: Die VM-Familie der von Azure Machine Learning Compute erstellten Knoten.
* **max_nodes**: Die maximale Anzahl von Knoten, auf die das Computeziel bei der Ausführung eines Auftrags in Azure Machine Learning Compute automatisch hochskaliert wird.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Persistente Computeressource: Erweitert

Sie können beim Erstellen einer Azure Machine Learning Compute-Umgebung auch mehrere erweiterte Eigenschaften konfigurieren. Mithilfe der Eigenschaften können Sie einen persistenten Cluster mit einer festen Größe oder in einem vorhandenen virtuellen Azure-Netzwerk in Ihrem Abonnement erstellen.

Neben den Eigenschaften **vm_size** und **max_nodes** können Sie auch die folgenden Eigenschaften verwenden:

* **min_nodes**: Die minimale Anzahl von Knoten, auf die das Computeziel bei der Ausführung eines Auftrags in einer Azure Machine Learning Compute-Umgebung automatisch herunterskaliert wird. Das Standardminimum ist null (0) Knoten.
* **vm_priority**: Der Typ der zu verwendenden VM, wenn Sie eine Azure Machine Learning Compute-Umgebungsressource erstellen. Sie können zwischen **dedicated** (dediziert, Standard) und **lowpriority** (niedrige Priorität) wählen. VMs mit niedriger Priorität nutzen überschüssige Kapazität in Azure. Diese VMs sind kostengünstiger, aber Ausführungen können vorzeitig beendet werden, wenn diese VMs zum Einsatz kommen.
* **idle_seconds_before_scaledown**: Die Dauer der Leerlaufzeit, die nach Beendigung einer Ausführung und vor der automatischen Skalierung auf die Anzahl von **min_nodes** gewartet wird. Die Standardleerlaufzeit ist 120 Sekunden.
* **vnet_resourcegroup_name**: Die Ressourcengruppe des __vorhandenen__ virtuellen Netzwerks. Die Azure Machine Learning Compute-Umgebung wird in diesem virtuellen Netzwerk erstellt.
* **vnet_name**: Der Name des virtuellen Netzwerks. Das virtuelle Netzwerk muss sich in derselben Region befinden wie Ihr Azure Machine Learning-Arbeitsbereich.
* **subnet_name**: Der Name des Subnetzes innerhalb des virtuellen Netzwerks. Den Azure Machine Learning Compute-Umgebungsressourcen werden IP-Adressen aus diesem Subnetzbereich zugewiesen.

> [!TIP]
> Beim Erstellen einer persistenten Azure Machine Learning Compute-Umgebungsressource können Sie auch Eigenschaften wie die Anzahl von **min_nodes** oder **max_nodes** aktualisieren. Um eine Eigenschaft zu aktualisieren, rufen Sie die `update()`-Funktion für die Eigenschaft auf.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist 
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

Azure Machine Learning bietet Ihnen auch die Möglichkeit, eine eigene Computeressource zu verwenden und diese an Ihren Arbeitsbereich anzufügen. Eine Remote-VM, auf die über Azure Machine Learning Service zugegriffen werden kann, ist ein solcher Ressourcentyp. Die Ressource kann entweder eine Azure-VM, ein Remoteserver in Ihrer Organisation oder lokal sein. Durch Angabe der IP-Adresse und Anmeldeinformationen (Benutzername und Kennwort oder SSH-Schlüssel) können Sie jede VM, auf die zugegriffen werden kann, für Remoteausführungen verwenden.
Sie können eine systemseitig erstellte Conda-Umgebung, eine vorhandene Python-Umgebung oder einen Docker-Container verwenden. Bei Ausführung mit einem Docker-Container muss die Docker-Engine auf der VM ausgeführt werden. Die Remote-VM-Funktion ist besonders nützlich, wenn Sie eine cloudbasierte Entwicklungs- und Experimentierumgebung nutzen möchten, die Ihnen mehr Flexibilität bietet als Ihr lokaler Computer.

> [!TIP]
> Verwenden Sie für dieses Szenario DSVM (Data Science Virtual Machine) als Azure-VM. Diese VM ist eine vorkonfigurierte Data Science- und KI-Entwicklungsumgebung in Azure mit einer eigens zusammengestellten Auswahl von Tools und Frameworks für den gesamten Lebenszyklus der Machine Learning-Entwicklung. Weitere Informationen zum Verwenden der DSVM mit Azure Machine Learning finden Sie unter [Konfigurieren einer Entwicklungsumgebung](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

> [!WARNING]
> Azure Machine Learning unterstützt nur virtuelle Computer, auf denen Ubuntu ausgeführt wird. Wenn Sie eine VM erstellen oder eine vorhandene VM auswählen, müssen Sie eine VM auswählen, die Ubuntu verwendet.

In den folgenden Schritten wird mit dem SDK eine DSVM als Trainingsziel konfiguriert:

1. Um einen vorhandenen virtuellen Computer als Computeziel anzufügen, müssen Sie den vollqualifizierten Domänennamen (FQDN), Benutzernamen und das Kennwort für die VM bereitstellen. Ersetzen Sie in diesem Beispiel \<fqdn> durch den öffentlichen FQDN der VM oder die öffentliche IP-Adresse. Ersetzen Sie im folgenden Befehl \<username> und \<password> durch den SSH-Benutzernamen und das Kennwort für die VM.

    ```python
    from azureml.core.compute import RemoteCompute, ComputeTarget
    
    # Create the compute config
    attach_config = RemoteCompute.attach_configuration(address = "ipaddress",
                                                       ssh_port=22,
                                                       username='<username>',
                                                       password="<password>")

    # If you use SSH instead of a password, use this code:
    #                                                  ssh_port=22,
    #                                                  username='<username>',
    #                                                  password=None,
    #                                                  private_key_file="path-to-file",
    #                                                  private_key_passphrase="passphrase")

    # Attach the compute target
    compute = ComputeTarget.attach(ws, "attach-dsvm", attach_config)

    compute.wait_for_completion(show_output=True)
    ```

1. Erstellen Sie eine Konfiguration für das DSVM-Computeziel. Zum Erstellen und Konfigurieren der Trainingsumgebung in der DSVM werden Docker und Conda verwendet.

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load into memory the cpu-dsvm.runconfig file created in the previous attach operation
    run_config = RunConfiguration(framework = "python")

    # Set the compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use the CPU base image
    # To use GPU in DSVM, you must also use the GPU base Docker image "azureml.core.runconfig.DEFAULT_GPU_IMAGE"
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask the system to provision a new conda environment based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when they're used for the first time
    run_config.prepare_environment = True

    # Specify the CondaDependencies object
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>Azure Databricks

Azure Databricks ist eine Apache Spark-basierte Umgebung in der Azure-Cloud. Die Umgebung kann beim Training von Modellen mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

> [!IMPORTANT]
> Ein Azure Databricks-Computeziel kann nur in einer Machine Learning-Pipeline verwendet werden.
>
> Sie müssen einen Azure Databricks-Arbeitsbereich erstellen, um ihn zum Training Ihres Modells zu verwenden. Informationen zum Erstellen dieser Ressource finden Sie unter [Ausführen eines Spark-Auftrags in Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Um Azure Databricks als Computeziel anzufügen, müssen Sie das Azure Machine Learning SDK verwenden und die folgenden Informationen angeben:

* __Computename__: Der Name, der dieser Computeressource zugewiesen werden soll.
* __Name des Databricks-Arbeitsbereichs__: Der Name des Azure Databricks-Arbeitsbereichs.
* __Zugriffstoken__: Das zur Authentifizierung bei Azure Databricks verwendete Zugriffstoken. Informationen zum Generieren eines Zugriffstokens finden Sie unter [Authentifizierung](https://docs.azuredatabricks.net/api/latest/authentication.html).

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

    # Create the attach config
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

Azure Data Lake Analytics ist eine umfangreiche Datenanalyseplattform in der Azure-Cloud. Die Plattform kann beim Training von Modellen mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

> [!IMPORTANT]
> Ein Azure Data Lake Analytics-Computeziel kann nur in einer Machine Learning-Pipeline verwendet werden.
>
> Sie müssen ein Azure Data Lake Analytics-Konto erstellen, um es zum Training Ihres Modells verwenden zu können. Informationen zum Erstellen dieser Ressource finden Sie unter [Erste Schritte mit Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

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
    
    # Create the attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach the ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Azure Machine Learning-Pipelines funktionieren nur mit Dateien, die im Standarddatenspeicher des Data Lake Analytics-Kontos gespeichert sind. Wenn die benötigten Daten nicht ein einem Standardspeicher gespeichert sind, können Sie die Daten mithilfe eines [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py)-Vorgangs vor dem Training des Modells kopieren.

## <a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight ist eine beliebte Plattform für Big Data-Analysen. Die Plattform stellt Apache Spark bereit, das zum Training Ihres Modells verwendet werden kann.

> [!IMPORTANT]
> Erstellen Sie den HDInsight-Cluster, bevor Sie ihn zum Training Ihres Modells verwenden. Informationen zum Erstellen von Spark in einem HDInsight-Cluster finden Sie unter [Erstellen eines Spark-Clusters in HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> Beim Erstellen des Clusters müssen Sie einen SSH-Benutzernamen und ein Kennwort angeben. Notieren Sie diese Werte, da Sie sie benötigen, wenn Sie HDInsight als Computeziel verwenden.
>
> Nach Erstellen des Clusters hat dieser den FQDN \<clustername>.azurehdinsight.net, wobei \<clustername> der Name ist, den Sie für den Cluster angegeben haben. Sie benötigen die FQDN-Adresse (oder die öffentliche IP-Adresse des Clusters), um den Cluster als Computeziel zu verwenden.

Um HDInsight als Computeziel zu konfigurieren, müssen Sie den FQDN, Benutzernamen und das Kennwort für den HDInsight-Cluster bereitstellen. Im folgenden Beispiel wird das SDK zum Anfügen eines Clusters zu Ihrem Arbeitsbereich verwendet. Ersetzen Sie in diesem Beispiel \<fqdn> durch den öffentlichen FQDN des Clusters oder die öffentliche IP-Adresse. Ersetzen Sie \<username> und \<password> durch den SSH-Benutzernamen und das Kennwort für den Cluster.

> [!NOTE]
> Besuchen Sie das Azure-Portal, und wählen Sie Ihren HDInsight-Cluster aus, um den FQDN für Ihren Cluster zu finden. Unter __Übersicht__ finden Sie im Eintrag __URL__ den FQDN. Um den FQDN zu erhalten, entfernen Sie das Präfix „https:\//“ am Anfang des Eintrags.

![Abrufen des FQDN für einen HDInsight-Cluster im Azure-Portal](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute, ComputeTarget

try:
    # Attach an HDInsight cluster as a compute target
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

# Configure the HDInsight run
# Load the runconfig object from the myhdi.runconfig file generated in the previous attach operation
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# Ask the system to prepare the conda environment automatically when it's used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-runs"></a>Übermitteln von Trainingsausführungen

Eine Trainingsausführung kann auf zwei Arten übermittelt werden:

* Übermitteln Sie eine Trainingsausführung mithilfe eines `ScriptRunConfig`-Objekts.
* Übermitteln Sie eine Trainingsausführung mithilfe eines `Pipeline`-Objekts.

> [!IMPORTANT]
> Die Azure Databricks- und Azure Data Lake Analytics-Computeziele können nur in einer Pipeline verwendet werden.
>
> Das lokale Computeziel kann nicht in einer Pipeline verwendet werden.

### <a name="scriptrunconfig-object"></a>ScriptRunConfig-Objekt

Das Codemuster für die Übermittlung einer Trainingsausführung mit dem `ScriptRunConfig`-Objekt ist für alle Arten von Computezielen gleich:

1. Erstellen Sie ein `ScriptRunConfig`-Objekt mithilfe der Ausführungskonfiguration für das Computeziel.
1. Übermitteln Sie die Ausführung.
1. Warten Sie, bis die Ausführung abgeschlossen ist.

Im folgenden Beispiel wird die Konfiguration für das vom System verwaltete lokale Computeziel verwendet, die weiter oben erstellt wurde:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="pipeline-object"></a>Pipelineobjekt

Das Codemuster für die Übermittlung einer Trainingsausführung mit einem `Pipeline`-Objekt ist für alle Arten von Computezielen gleich:

1. Fügen Sie dem `Pipeline`-Objekt einen Schritt für die Computeressource hinzu.
1. Übermitteln Sie eine Ausführung über die Pipeline.
1. Warten Sie, bis die Ausführung abgeschlossen ist.

Im folgenden Beispiel wird das Azure Databricks-Computeziel verwendet, das weiter oben erstellt wurde:

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

# List of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Weitere Informationen zu Machine Learning-Pipelines finden Sie unter [Pipelines und Azure Machine Learning](concept-ml-pipelines.md).

Beispiele für Jupyter Notebooks, die das Training eines Modells über eine Pipeline veranschaulichen, finden Sie unter [https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="access-computes-in-the-azure-portal"></a>Zugreifen auf Computeziele im Azure-Portal

Sie können im Azure-Portal auf Computeziele zugreifen, die Ihrem Arbeitsbereich zugeordnet sind. 

### <a name="view-compute-targets"></a>Anzeigen von Computezielen

Um die Compute-Ziele für Ihren Arbeitsbereich anzuzeigen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und öffnen Sie Ihren Arbeitsbereich.
1. Wählen Sie unter __Anwendungen__ den Eintrag __Compute__.

    ![Anzeigen von Computezielen](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Erstellen eines Computeziels

Führen Sie die vorherigen Schritte zum Anzeigen der Liste der Computeziele aus. Führen Sie dann die folgenden Schritte aus, um ein Computeziel zu erstellen:

1. Klicken Sie auf das Pluszeichen (+), um ein Computeziel hinzuzufügen.

    ![Hinzufügen eines Computeziels](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Geben Sie einen Namen für das Computeziel ein.
1. Wählen Sie **Machine Learning Compute** als Computetyp für __Training__ aus.

    > [!IMPORTANT]
    > Sie können nur eine Azure Machine Learning Compute-Umgebung als verwaltete Computeressource für das Training erstellen.

1. Füllen Sie das Formular aus. Geben Sie für die Einrichtung des Computeziels Werte für die Pflichteigenschaften ein, insbesondere die **VM-Familie** und die **maximale Anzahl von Knoten**. 
1. Klicken Sie auf __Erstellen__.
1. Sie können den Status des Erstellungsvorgangs anzeigen, indem Sie das Computeziel in der Liste auswählen:

    ![Auswählen eines Computeziels zum Anzeigen des Status des Erstellungsvorgangs](./media/how-to-set-up-training-targets/View_list.png)

1. Daraufhin werden die Details für das Computeziel angezeigt:

    ![Anzeigen der Details des Computeziels](./media/how-to-set-up-training-targets/compute-target-details.png)

Nun können Sie, wie zuvor beschrieben, eine Ausführung mit den Computezielen übermitteln.


### <a name="reuse-existing-compute-targets"></a>Wiederverwenden vorhandener Computeziele

Führen Sie die zuvor beschriebenen Schritte aus, um die Liste der Computeziele anzuzeigen. Führen Sie dann die folgenden Schritte aus, um ein Computeziel wiederzuverwenden:

1. Klicken Sie auf das Pluszeichen (+), um ein Computeziel hinzuzufügen.
1. Geben Sie einen Namen für das Computeziel ein.
1. Wählen Sie den Computetyp aus, den Sie für __Training__ anfügen möchten:

    > [!IMPORTANT]
    > Nicht alle Computetypen können im Azure-Portal angefügt werden.
    > Derzeit können folgende Computetypen für das Training angefügt werden:
    >
    > * Eine Remote-VM
    > * Azure Databricks
    > * Azure Data Lake Analytics
    > * Azure HDInsight

1. Füllen Sie das Formular aus, und geben Sie Werte für die Pflichteigenschaften an.

    > [!NOTE]
    > Microsoft empfiehlt die Verwendung von SSH-Schlüsseln, die sicherer als Kennwörter sind. Kennwörter sind anfällig für Brute-Force-Angriffe. SSH-Schlüssel basieren auf kryptografische Signaturen. Informationen zum Erstellen von SSH-Schlüsseln für die Verwendung mit Azure Virtual Machines finden Sie in den folgenden Dokumenten:
    >
    > * [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Verwenden von SSH-Schlüsseln mit Windows in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Wählen Sie __Anfügen__ aus.
1. Sie können den Status des Anfügevorgangs anzeigen, indem Sie das Computeziel in der Liste auswählen.

Nun können Sie, wie zuvor beschrieben, eine Ausführung mit diesen Computezielen übermitteln.

## <a name="notebook-examples"></a>Notebook-Beispiele

Beispiele finden Sie in den folgenden Notebooks:

* [how-to-use-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* [What is the Azure Machine Learning SDK for Python?](https://aka.ms/aml-sdk) (Was ist das Azure Machine Learning-SDK für Python?)
* [Tutorial: Trainieren eines Modells](tutorial-train-models-with-aml.md)
* [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst)
* [Pipelines and Azure Machine Learning](concept-ml-pipelines.md) (Pipelines und Azure Machine Learning)
