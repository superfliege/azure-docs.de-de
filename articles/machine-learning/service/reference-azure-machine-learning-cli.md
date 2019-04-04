---
title: Machine Learning-CLI-Erweiterung
titleSuffix: Azure Machine Learning service
description: Hier erhalten Sie Informationen zur Azure Machine Learning-CLI-Erweiterung für die Azure CLI. Die Azure CLI ist ein plattformübergreifendes Befehlszeilenhilfsprogramm, das das Arbeiten mit Ressourcen in der Azure-Cloud ermöglicht. Die Machine Learning-Erweiterung ermöglicht Ihnen das Arbeiten mit dem Azure Machine Learning-Dienst.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 2504ca9cb785529a9eab321c2521db46390632b7
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793202"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Verwenden der CLI-Erweiterung für Azure Machine Learning Service

Der Azure Machine Learning-CLI ist eine Erweiterung der [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), eine plattformübergreifende Befehlszeilenschnittstelle für die Azure-Plattform. Diese Erweiterung unterstützt Befehle für die Arbeit mit dem Azure Machine Learning-Dienst über die Befehlszeile. Zudem können Sie Skripts erstellen, die Ihre Machine Learning-Workflows automatisieren. Beispielsweise können Sie Skripts erstellen, die die folgenden Aktionen ausführen:

+ Ausführen von Experimenten zum Erstellen von Machine Learning-Modellen

+ Registrieren von Machine Learning-Modellen zur Verwendung durch Kunden

+ Paketieren, Bereitstellen und Nachverfolgen des Lebenszyklus Ihrer Machine Learning-Modelle

Die CLI ist kein Ersatz für das Azure Machine Learning SDK. Sie stellt ein ergänzende Tool dar, das für die Verarbeitung hochgradig parametrisierter Aufgaben optimiert ist:

* Erstellen von Computeressourcen

* Übermittlung parametrisierter Experimente

* Modellregistrierung

* Erstellung von Images

* Dienstbereitstellung

## <a name="prerequisites"></a>Voraussetzungen


* Für die Verwendung der CLI benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie heute die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Die [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)

## <a name="install-the-extension"></a>Installieren der Erweiterung

Um die Machine Learning-CLI-Erweiterung zu installieren, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-1.0.10-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Klicken Sie bei Aufforderung auf `y`, um die Erweiterung zu installieren.

Um sicherzustellen, dass die Erweiterung installiert wurde, verwenden Sie den folgenden Befehl zum Anzeigen einer Liste von ML-spezifischen Unterbefehlen:

```azurecli-interactive
az ml -h
```

> [!TIP]
> Um die Erweiterung zu aktualisieren, müssen Sie sie __entfernen__ und dann __installieren__. So wird die aktuelle Version installiert.

## <a name="remove-the-extension"></a>Entfernen der Erweiterung

Verwenden Sie zum Entfernen der CLI-Erweiterung den folgenden Befehl:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Ressourcenverwaltung

Die folgenden Befehle veranschaulichen, wie Sie mit der CLI Ressourcen verwalten, die von Azure Machine Learning verwendet werden.


+ Hiermit erstellen Sie einen Azure Machine Learning-Dienstarbeitsbereich:

    ```azurecli-interactive
    az ml workspace create -n myworkspace -g myresourcegroup
    ```

+ Hiermit legen Sie einen Standardarbeitsbereich fest:

    ```azurecli-interactive
    az configure --defaults aml_workspace=myworkspace group=myresourcegroup
    ```
    
+ Anfügen eines AKS-Clusters

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

## <a name="experiments"></a>Experimente

Die folgenden Befehle veranschaulichen, wie Sie die CLI zum Arbeiten mit Experimenten verwenden:

* Hiermit fügen Sie vor dem Absenden eines Experiments ein Projekt an (Laufzeitkonfiguration):

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Starten Sie eine Ausführung Ihres Experiments. Geben Sie bei Verwendung dieses Befehls den Namen der runconfig-Datei an, die die Laufzeitkonfiguration enthält. Das Computeziel verwendet die Ausführungskonfiguration, um die Trainingsumgebung für das Modell zu erstellen. In diesem Beispiel wird die Ausführungskonfiguration aus der Datei `./aml_config/myrunconfig.runconfig` geladen.

    ```azurecli-interactive
    az ml run submit -c myrunconfig train.py
    ```

    Weitere Informationen zur runconfig-Datei finden Sie im Abschnitt [RunConfig](#runconfig).

* Hiermit zeigen Sie eine Liste der übermittelten Experimente an:

    ```azurecli-interactive
    az ml history list
    ```

## <a name="model-registration-image-creation--deployment"></a>Modellregistrierung, Imageerstellung und Bereitstellung

Die folgenden Befehle veranschaulichen, wie ein trainiertes Modell registriert und dann als Produktionsdienst bereitgestellt wird:

+ Hiermit registrieren Sie ein Modell bei Azure Machine Learning:

  ```azurecli-interactive
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Hiermit erstellen Sie ein Image, das Ihr Machine Learning-Modell und die zugehörigen Abhängigkeiten enthält: 

  ```azurecli-interactive
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Hiermit stellen Sie ein Image für ein Computeziel bereit:

  ```azurecli-interactive
  az ml service create aci -n myaciservice --image-id myimage:1
  ```

## <a id="runconfig"></a> runconfig-Datei

Eine Laufzeitkonfiguration wird verwendet, um die Trainingsumgebung zu konfigurieren, die zum Trainieren Ihres Modells genutzt wird. Diese Konfiguration kann speicherintern (in-memory) erstellt werden, indem das SDK verwendet wird, oder sie kann aus einer runconfig-Datei geladen werden.

Die runconfig-Datei ist ein Textdokument, in dem die Konfiguration für die Trainingsumgebung beschrieben wird. Darin sind beispielsweise der Name des Trainingsskripts und die Datei aufgeführt, die die erforderlichen Conda-Abhängigkeiten zum Trainieren des Modells enthält.

Die Azure Machine Learning-CLI erstellt zwei `.runconfig`-Standarddateien mit den Namen `docker.runconfig` und `local.runconfig`, wenn Sie ein Projekt mit dem Befehl `az ml project attach` anfügen. 

Wenn Sie über Code verfügen, mit dem eine Laufzeitkonfiguration mit der [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py)-Klasse erstellt wird, können Sie die `save()`-Methode verwenden, um diese in einer `.runconfig`-Datei zu speichern.

Hier ist ein Beispiel für den Inhalt einer `.runconfig`-Datei angegeben:

```text
# The script to run.
script: train.py
# The arguments to the script file.
arguments: []
# The name of the compute target to use for this run.
target: local
# Framework to execute inside. Allowed values are "Python" ,  "PySpark", "CNTK",  "TensorFlow", and "PyTorch".
framework: PySpark
# Communicator for the given framework. Allowed values are "None" ,  "ParameterServer", "OpenMpi", and "IntelMpi".
communicator: None
# Automatically prepare the run environment as part of the run itself.
autoPrepareEnvironment: true
# Maximum allowed duration for the run.
maxRunDurationSeconds:
# Number of nodes to use for running job.
nodeCount: 1
# Environment details.
environment:
# Environment variables set for the run.
  environmentVariables:
    EXAMPLE_ENV_VAR: EXAMPLE_VALUE
# Python details
  python:
# user_managed_dependencies=True indicates that the environmentwill be user managed. False indicates that AzureML willmanage the user environment.
    userManagedDependencies: false
# The python interpreter path
    interpreterPath: python
# Path to the conda dependencies file to use for this run. If a project
# contains multiple programs with different sets of dependencies, it may be
# convenient to manage those environments with separate files.
    condaDependenciesFile: aml_config/conda_dependencies.yml
# Docker details
  docker:
# Set True to perform this run inside a Docker container.
    enabled: true
# Base image used for Docker-based runs.
    baseImage: mcr.microsoft.com/azureml/base:0.2.4
# Set False if necessary to work around shared volume bugs.
    sharedVolumes: true
# Run with NVidia Docker extension to support GPUs.
    gpuSupport: false
# Extra arguments to the Docker run command.
    arguments: []
# Image registry that contains the base image.
    baseImageRegistry:
# DNS name or IP address of azure container registry(ACR)
      address:
# The username for ACR
      username:
# The password for ACR
      password:
# Spark details
  spark:
# List of spark repositories.
    repositories:
    - https://mmlspark.azureedge.net/maven
    packages:
    - group: com.microsoft.ml.spark
      artifact: mmlspark_2.11
      version: '0.12'
    precachePackages: true
# Databricks details
  databricks:
# List of maven libraries.
    mavenLibraries: []
# List of PyPi libraries
    pypiLibraries: []
# List of RCran libraries
    rcranLibraries: []
# List of JAR libraries
    jarLibraries: []
# List of Egg libraries
    eggLibraries: []
# History details.
history:
# Enable history tracking -- this allows status, logs, metrics, and outputs
# to be collected for a run.
  outputCollection: true
# whether to take snapshots for history.
  snapshotProject: true
# Spark configuration details.
spark:
  configuration:
    spark.app.name: Azure ML Experiment
    spark.yarn.maxAppAttempts: 1
# HDI details.
hdi:
# Yarn deploy mode. Options are cluster and client.
  yarnDeployMode: cluster
# Tensorflow details.
tensorflow:
# The number of worker tasks.
  workerCount: 1
# The number of parameter server tasks.
  parameterServerCount: 1
# Mpi details.
mpi:
# When using MPI, number of processes per node.
  processCountPerNode: 1
# data reference configuration details
dataReferences: {}
# Project share datastore reference.
sourceDirectoryDataStore:
# AmlCompute details.
amlcompute:
# VM size of the Cluster to be created.Allowed values are Azure vm sizes. The list of vm sizes is available in 'https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs
  vmSize:
# VM priority of the Cluster to be created.Allowed values are "dedicated" , "lowpriority".
  vmPriority:
# A bool that indicates if the cluster has to be retained after job completion.
  retainCluster: false
# Name of the cluster to be created. If not specified, runId will be used as cluster name.
  name:
# Maximum number of nodes in the AmlCompute cluster to be created. Minimum number of nodes will always be set to 0.
  clusterMaxNodeCount: 1
```
