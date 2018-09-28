---
title: Bereitstellen von Modellen des Azure Machine Learning-Diensts in Kubernetes | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Modell des Azure Machine Learning-Diensts in Kubernetes Service bereitstellen. Das Modell wird als Webdienst bereitgestellt. Azure Kubernetes Service eignet sich für Produktionsworkloads mit umfangreicher Skalierung.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: f74521f77420fcfc60e99dd3d70574d5e94cf084
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967743"
---
# <a name="how-to-deploy-models-from-azure-machine-learning-service-to-azure-kubernetes-service"></a>Bereitstellen von Modellen des Azure Machine Learning-Diensts in Azure Kubernetes Service

Für Produktionsszenarien mit umfangreicher Skalierung können Sie Ihr Modell in Azure Kubernetes Service (AKS) bereitstellen. Azure Machine Learning kann einen vorhandenen AKS-Cluster verwenden oder einen neuen Cluster, der während der Bereitstellung erstellt wird. Das Modell wird in AKS als Webdienst bereitgestellt.

Die Bereitstellung in AKS bietet automatische Skalierung, Protokollierung, Modelldatensammlung und schnelle Antwortzeiten für den Webdienst.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Ein Azure Machine Learning-Arbeitsbereich, ein lokales Verzeichnis mit Ihren Skripts und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erhalten dieser Voraussetzungen finden Sie im Dokument [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).

- Ein trainiertes Modell für maschinelles Lernen. Wenn Sie keines besitzen, sehen Sie sich das Tutorial zum [Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md) an.

## <a name="initialize-the-workspace"></a>Initialisieren des Arbeitsbereichs

Zum Initialisieren des Arbeitsbereichs laden Sie die Datei `config.json`, die Informationen zu Ihrem Arbeitsbereich enthält.

```python
from azureml.coreazureml import Workspace

# Load existing workspace from the config file info.
ws = Workspace.from_config()
```

## <a name="register-the-model"></a>Registrieren des Modells

Um ein vorhandenes Modell zu registrieren, geben Sie den Pfad des Modells, die Beschreibung und Tags an.

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl", # this points to a local file
                        model_name = "best_model", # this is the name the model is registered as
                        tags = {"data": "diabetes", "type": "regression"},
                        description = "Ridge regression model to predict diabetes",
                        workspace = ws)
```

## <a name="create-a-docker-image"></a>Erstellen eines Docker-Image

Azure Kubernetes Service verwendet Docker-Images. Führen Sie die folgenden Schritte aus, um das Image zu erstellen:

1. Um das Image zu konfigurieren, müssen Sie ein Bewertungsskript und eine Umgebungsdatei erstellen. Ein Beispiel für das Erstellen des Skripts und der Umgebungsdatei finden Sie in den folgenden Abschnitten des Imageklassifizierungsbeispiels:

    * [Erstellen eines Bewertungsskripts („score.py“)](tutorial-deploy-models-with-aml.md#create-scoring-script)

    * [Erstellen einer Umgebungsdatei („myenv.yml“)](tutorial-deploy-models-with-aml.md#create-environment-file) 

   Im folgenden Beispiel werden diese Dateien zum Konfigurieren des Images verwendet:

    ```python
    from azureml.core.image import ContainerImage

    # Image configuration
    image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                        runtime = "python",
                                                        conda_file = "myenv.yml",
                                                        description = "Image with ridge regression model",
                                                        tags = {"data": "diabetes", "type": "regression"}
                                                        )
    ```

1. Um das Image zu erstellen, verwenden Sie die Modell- und Imagekonfiguration. Dieser Vorgang dauert etwa 5 Minuten:

    ```python
    image = ContainerImage.create(name = "myimage1",
                                    # this is the model object
                                    models = [model],
                                    image_config = image_config,
                                    workspace = ws)

    # Wait for the create process to complete
    image.wait_for_creation(show_output = True)
    ```

## <a name="create-the-aks-cluster"></a>Erstellen des AKS-Clusters

Der folgende Codeausschnitt veranschaulicht das Erstellen des AKS-Clusters. Für diesen Prozess werden etwa 20 Minuten benötigt:

> [!IMPORTANT]
> Das Erstellen des AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Prozess. Nach der Erstellung können Sie diesen Cluster für mehrere Bereitstellungen wiederverwenden. Wenn Sie den Cluster oder die Ressourcengruppe, die ihn enthält, löschen, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen.


```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aml-aks-1' 
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                    name = aks_name, 
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

### <a name="attach-existing-aks-cluster-optional"></a>Anfügen vorhandener AKS-Cluster (optional)

Wenn Sie in Ihrem Azure-Abonnement über einen AKS-Cluster verfügen, können Sie diesen für die Bereitstellung Ihres Images verwenden. Der folgende Codeausschnitt veranschaulicht das Anfügen eines Clusters an Ihren Arbeitsbereich. 

> [!IMPORTANT]
> Es wird nur Version 1.8.7 von AKS unterstützt.

```python
# Get the resource id from https://porta..azure.com -> Find your resource group -> click on the Kubernetes service -> Properties
resource_id = '/subscriptions/<your subscription id>/resourcegroups/<your resource group>/providers/Microsoft.ContainerService/managedClusters/<your aks service name>'

# Set to the name of the cluster
cluster_name='my-existing-aks' 

# Attatch the cluster to your workgroup
aks_target = AksCompute.attach(workspace=ws, name=cluster_name, resource_id=resource_id)

# Wait for the operation to complete
aks_target.wait_for_provisioning(True)
```

## <a name="deploy-your-web-service"></a>Bereitstellen des Webdiensts

Der folgende Codeausschnitt veranschaulicht das Bereitstellen des Images im AKS-Cluster:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
aks_service = Webservice.deploy_from_image(workspace = ws, 
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!TIP]
> Wenn während der Bereitstellung Fehler auftreten, können Sie die AKS-Dienstprotokolle mit `aks_service.get_logs()` anzeigen. Die protokollierten Informationen weisen eventuell auf die Ursache des Fehlers hin.

## <a name="test-the-web-service"></a>Testen des Webdiensts

Verwenden Sie zum Testen des Webdiensts `aks_service.run()`. Der folgende Codeausschnitt veranschaulicht das Übergeben von Daten an den Dienst und das Anzeigen der Vorhersage:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = aks_service.run(input_data = test_sample)
print(prediction)
```

## <a name="cleanup"></a>Cleanup

Verwenden Sie den folgenden Codeausschnitt, um den Dienst, das Image und das Modell zu löschen:

```python
aks_service.delete()
image.delete()
model.delete()
```
