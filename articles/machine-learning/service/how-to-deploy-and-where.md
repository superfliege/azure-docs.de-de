---
title: Bereitstellen von Modellen als Webdienste
titleSuffix: Azure Machine Learning service
description: 'Erfahren Sie, wie und wo Sie Ihre Azure Machine Learning Service-Modelle bereitstellen, wie z. B.: Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge und Field Programmable Gate Arrays.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 908c22c6b071b7c7c708b73995c800a23cabad45
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57860420"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst

Das Azure Machine Learning SDK bietet verschiedene Möglichkeiten zum Bereitstellen Ihres trainierten Modells. In diesem Dokument erfahren Sie, wie Sie Ihr Modell als Webdienst in der Azure-Cloud oder auf IoT Edge-Geräten bereitstellen.

Sie können Modelle an folgenden Computezielen bereitstellen:

| Computeziel | Bereitstellungstyp | BESCHREIBUNG |
| ----- | ----- | ----- |
| [Azure Kubernetes Service (AKS)](#aks) | Echtzeitrückschluss | Geeignet für hochgradig skalierbare Produktionsbereitstellungen. Bietet automatische Skalierung und schnelle Reaktionszeiten. |
| [Azure ML Compute](#azuremlcompute) | Batchrückschluss | Führen Sie eine Batchvorhersage für serverloses Computing aus. Unterstützt virtuelle Computer mit normaler und niedriger Priorität. |
| [Azure Container Instances (ACI)](#aci) | Testen | Gut geeignet für Entwicklungs- und Testzwecke. **Nicht geeignet für Produktionsworkloads.** |
| [Azure IoT Edge](#iotedge) | (Vorschauversion) IoT-Modul | Stellen Sie Modelle auf IoT-Geräten bereit. Rückschlüsse erfolgen auf dem Gerät. |
| [Field Programmable Gate Array (FPGA)](#fpga) | (Vorschauversion) Webdienst | Extrem geringe Wartezeit für die Interferenzierung in Echtzeit. |

Der Prozess zur Bereitstellung eines Modells ist für alle Computeziele ähnlich:

1. Trainieren und Registrieren eines Modells.
1. Konfigurieren und Registrieren eines Images, das das Modell verwendet.
1. Bereitstellen des Image für ein Computeziel.
1. Testen der Bereitstellung

Das folgende Video zeigt die Bereitstellung in Azure Container Instances:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]


Weitere Informationen zu den am Bereitstellungsworkflow beteiligten Konzepten finden Sie unter [Verwalten, Bereitstellen und Überwachen von Modellen mit dem Azure Machine Learning-Dienst](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie heute die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

- Ein Azure Machine Learning Service-Arbeitsbereich und das Azure Machine Learning SDK für Python müssen installiert sein. Unter [Schnellstart: erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md) erfahren Sie, wie Sie diese Komponenten installieren.

- Ein trainiertes Modell. Wenn Sie nicht über ein trainiertes Modell verfügen, verwenden Sie die Schritte im Tutorial [Train models (Trainieren von Modellen)](tutorial-train-models-with-aml.md), um ein Modell zu trainieren und beim Azure Machine Learning Service zu registrieren.

    > [!NOTE]
    > Azure Machine Learning Service ist mit jedem generischen Modell kompatibel, das in Python 3 geladen werden kann. Die Beispiele in diesem Dokument zeigen die Verwendung eines im Python-Pickle-Format gespeicherten Modells.
    >
    > Weitere Informationen zur Verwendung von ONNX-Modellen finden Sie im Dokument [ONNX und Azure Machine Learning (ONNX und Azure Machine Learning)](how-to-build-deploy-onnx.md).

## <a id="registermodel"></a> Registrieren eines trainierten Modells

Die Modellregistrierung ist eine Möglichkeit zum Speichern und Organisieren Ihrer trainierten Modelle in der Azure-Cloud. Modelle werden in Ihrem Azure Machine Learning Service-Arbeitsbereich registriert. Das Modell kann mit Azure Machine Learning oder einem anderen Dienst trainiert werden. Der folgende Code veranschaulicht das Registrieren eines Modells aus einer Datei sowie das Festlegen von Name, Tags und Beschreibung:

```python
from azureml.core.model import Model

model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

**Geschätzter Zeitaufwand**: Ungefähr 10 Sekunden.

Ein Beispiel für das Registrieren eines Modells finden Sie unter [Tutorial: Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning Service](tutorial-train-models-with-aml.md).

Weitere Informationen finden Sie in der Referenzdokumentation zur [Modellklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Erstellen und Registrieren eines Images

Bereitgestellte Modelle werden als Image gepackt. Das Image enthält die notwendigen Abhängigkeiten zum Ausführen des Modells.

Für Bereitstellungen von **Azure Container Instance**, **Azure Kubernetes Service** und **Azure IoT Edge** wird die Klasse [azureml.core.image.ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py) verwendet, um eine Imagekonfiguration zu erstellen. Diese Imagekonfiguration wird dann verwendet, um ein neues Docker-Image zu erstellen.

Der folgende Code zeigt, wie eine neue Imagekonfiguration erstellt wird:

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml"}
                                                 )
```

**Geschätzter Zeitaufwand**: Ungefähr 10 Sekunden.

Die in diesem Beispiel verwendeten wichtigen Parameter werden in der folgenden Tabelle beschrieben:

| Parameter | BESCHREIBUNG |
| ----- | ----- |
| `execution_script` | Gibt ein Python-Skript an, das zum Empfangen von an den Dienst übermittelten Anforderungen verwendet wird. In diesem Beispiel befindet sich das Skript in der Datei `score.py`. Weitere Informationen finden Sie im Abschnitt [Ausführungsskript](#script). |
| `runtime` | Zeigt an, dass das Image Python verwendet. Die andere Option ist `spark-py`, die Python mit Apache Spark verwendet. |
| `conda_file` | Wird verwendet, um eine Conda-Umgebungsdatei bereitzustellen. Diese Datei definiert die Conda-Umgebung für das bereitgestellte Modell. Weitere Informationen zum Erstellen dieser Datei finden Sie unter [Create an environment file (myenv.yml) (Erstellen einer Umgebungsdatei (myenv.yml))](tutorial-deploy-models-with-aml.md#create-environment-file). |

Ein Beispiel für das Erstellen einer Imagekonfiguration finden Sie unter [Tutorial: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md).

Weitere Informationen finden Sie in der Referenzdokumentation zur [ContainerImage-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

### <a id="script"></a> Ausführungsskript

Das Ausführungsskript empfängt an ein bereitgestelltes Image übermittelte Daten und übergibt sie an das Modell. Anschließend nimmt es die vom Modell zurückgegebene Antwort entgegen und gibt diese an den Client zurück. **Das Skript ist modellspezifisch.** Es muss die vom Modell erwarteten und zurückgegebenen Daten verstehen. Ein Beispielskript mit einem Bildklassifizierungsmodell finden Sie unter [Tutorial: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md).

Das Skript enthält zwei Funktionen zum Laden und Ausführen des Modells:

* `init()`: Diese Funktion lädt üblicherweise das Modell in ein globales Objekt. Diese Funktion wird nur dann ausgeführt, wenn der Docker-Container gestartet wird.

* `run(input_data)`: Diese Funktion verwendet das Modell zur Vorhersage eines Werts, der auf den Eingabedaten basiert. Ein- und Ausgaben an die/von der Ausführung verwenden in der Regel JSON für die Serialisierung und Deserialisierung. Sie können auch mit binären Rohdaten arbeiten. Sie können die Daten vor dem Senden an das Modell oder vor der Rückgabe an den Client transformieren.

#### <a name="working-with-json-data"></a>Arbeiten mit JSON-Daten

Das folgende Beispielskript akzeptiert JSON-Daten und gibt diese zurück. Die `run`-Funktion transformiert die Daten aus JSON in ein Format, das das Modell erwartet, und transformiert die Antwort dann vor der Rückgabe in JSON:

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression
from azureml.core.model import Model

# load the model
def init():
    global model
    # retrieve the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

# Passes data to the model and returns the prediction
def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

#### <a name="working-with-binary-data"></a>Arbeiten mit Binärdaten

Wenn Ihr Modell __Binärdaten__ akzeptiert, verwenden Sie `AMLRequest`, `AMLResponse` und `rawhttp`. Das folgende Beispielskript akzeptiert Binärdaten und gibt die umgekehrten Bytes für POST-Anforderungen zurück. Für GET-Anforderungen wird die vollständige URL im Antworttext zurückgegeben:

```python
from azureml.contrib.services.aml_request  import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

# Accept and return binary data
@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    # handle GET requests
    if request.method == 'GET':
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    # handle POST requests
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        respBody = bytearray(reqBody)
        respBody.reverse()
        respBody = bytes(respBody)
        return AMLResponse(respBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> Der Namespace `azureml.contrib` ändert sich häufig, während wir daran arbeiten, den Dienst zu verbessern. Daher sollte alles in diesem Namespace als Vorschau und als von Microsoft nicht vollständig unterstützt betrachtet werden.
>
> Wenn Sie dies in Ihrer lokalen Entwicklungsumgebung testen müssen, können Sie die Komponenten im Namespace `contrib` mit dem folgenden Befehl installieren:
> ```shell
> pip install azureml-contrib-services
> ```

### <a id="createimage"></a> Registrieren des Images

Nachdem Sie die Imagekonfiguration erstellt haben, können Sie diese verwenden, um ein Image zu registrieren. Dieses Image wird in der Containerregistrierung für Ihren Arbeitsbereich gespeichert. Nach der Erstellung können Sie dasselbe Image für mehrere Dienste bereitstellen.

```python
# Register the image from the image configuration
image = ContainerImage.create(name = "myimage",
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**Geschätzter Zeitaufwand**: Ca. 3 Minuten.

Images werden automatisch mit einer Versionsangabe versehen, wenn Sie mehrere Images mit dem gleichen Namen registrieren. Dem ersten Bild beispielsweise, das als `myimage` registriert ist, wird die ID `myimage:1` zugewiesen. Wenn Sie das nächste Mal ein Bild als `myimage` registrieren, lautet die ID des neuen Image `myimage:2`.

Weitere Informationen finden Sie in der Referenzdokumentation zur [ContainerImage-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a id="deploy"></a> Bereitstellen als Webdienst

Je nach dem Computeziel, auf dem Sie die Bereitstellung vornehmen, unterscheidet sich der Bereitstellungsprozess etwas. Anhand der Informationen in den folgenden Abschnitten erfahren Sie mehr zur Bereitstellung auf folgenden Zielen:

| Computeziel | Bereitstellungstyp | BESCHREIBUNG |
| ----- | ----- | ----- |
| [Azure Kubernetes Service (AKS)](#aks) | Webdienst (Echtzeitrückschluss)| Geeignet für hochgradig skalierbare Produktionsbereitstellungen. Bietet automatische Skalierung und schnelle Reaktionszeiten. |
| [Azure ML Compute](#azuremlcompute) | Webdienst (Batchrückschluss)| Führen Sie eine Batchvorhersage für serverloses Computing aus. Unterstützt virtuelle Computer mit normaler und niedriger Priorität. |
| [Azure Container Instances (ACI)](#aci) | Webdienst (Entwicklung/Test)| Gut geeignet für Entwicklungs- und Testzwecke. **Nicht geeignet für Produktionsworkloads.** |
| [Azure IoT Edge](#iotedge) | (Vorschauversion) IoT-Modul | Stellen Sie Modelle auf IoT-Geräten bereit. Rückschlüsse erfolgen auf dem Gerät. |
| [Field Programmable Gate Array (FPGA)](#fpga) | (Vorschauversion) Webdienst | Extrem geringe Wartezeit für die Interferenzierung in Echtzeit. |

> [!IMPORTANT]
> Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) wird zurzeit beim Bereitstellen eines Modells als Webdienst nicht unterstützt.

In den Beispielen dieses Abschnitts wird [deploy_from_image](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-) verwendet. Modell und Image müssen daher vor der Bereitstellung registriert werden. Weitere Informationen zu anderen Bereitstellungsmethoden finden Sie unter [deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) sowie unter [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-).

### <a id="aci"></a> Bereitstellen in Azure Container Instances (DEVTEST)

Verwenden Sie Azure Container Instances für die Bereitstellung Ihrer Modelle als Webdienst, wenn mindestens eine der folgenden Bedingungen zutrifft:

- Sie müssen Ihr Modell schnell bereitstellen und überprüfen. Eine ACI-Bereitstellung ist in weniger als 5 Minuten abgeschlossen.
- Sie testen ein Modell in der Entwicklungsphase. Um Kontingente und die Regionsverfügbarkeit für ACI anzuzeigen, beachten Sie das Dokument [Kontingente und Regionsverfügbarkeit für Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas).

Verwenden Sie die folgenden Schritte für Bereitstellungen in Azure Container Instances:

1. Definieren Sie die Bereitstellungskonfiguration. Diese Konfiguration hängt von den Anforderungen Ihres Modells ab. Das folgende Beispiel definiert eine Konfiguration, die einen CPU-Kern und 1 GB Speicher verwendet:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Um das Image bereitzustellen, das im Abschnitt [Erstellen des Image](#createimage) dieses Dokuments erstellt wurde, verwenden Sie den folgenden Code:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Geschätzter Zeitaufwand**: Ca. fünf Minuten.

Weitere Informationen finden Sie in der Referenzdokumentation für die Klassen [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) und [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a> Bereitstellen in Azure Kubernetes Service (PRODUKTION)

Um Ihr Modell als hochgradig skalierbaren Produktionswebdienst bereitzustellen, verwenden Sie Azure Kubernetes Service (AKS). Sie können einen vorhandenen AKS-Cluster verwenden oder mithilfe des Azure Machine Learning-SDK, der CLI oder des Azure-Portals einen neuen erstellen.

Das Erstellen eines AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Prozess. Sie können diesen Cluster für mehrere Bereitstellungen wiederverwenden.

> [!IMPORTANT]
> Wenn Sie den Cluster löschen, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen.

Azure Kubernetes Service bietet die folgenden Funktionen:

* Automatische Skalierung
* Protokollierung
* Modelldatensammlung
* Schnelle Reaktionszeiten für Ihre Webdienste
* TLS-Terminierung
* Authentication

#### <a name="autoscaling"></a>Automatische Skalierung

Die automatische Skalierung kann durch Festlegen von `autoscale_target_utilization`, `autoscale_min_replicas` und `autoscale_max_replicas` für den AKS-Webdienst gesteuert werden. Die Aktivierung der automatischen Skalierung wird im folgenden Beispiel veranschaulicht:

```python
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=True,
                                                autoscale_target_utilization=30,
                                                autoscale_min_replicas=1,
                                                autoscale_max_replicas=4)
```

Skalierungsentscheidungen werden auf der Grundlage der Auslastung der aktuellen Containerreplikate getroffen. Zur Ermittlung der aktuellen Auslastung wird die Anzahl ausgelasteter Replikate (Replikate, die eine Anforderung verarbeiten) durch die Gesamtanzahl aktueller Replikate geteilt. Übersteigt dieser Wert die Zielauslastung, werden weitere Replikate erstellt. Ist der Wert niedriger, wird die Replikatanzahl verringert. Die Zielauslastung ist standardmäßig auf 70 Prozent festgelegt.

Die Entscheidung, Replikate hinzuzufügen, wird sekundenschnell getroffen und umgesetzt. Die Entscheidung, Replikate zu entfernen, dauert länger (etwa eine Minute). Dadurch bleiben Replikate im Leerlauf etwa eine Minute lang erhalten und können ggf. neu hinzukommende Anforderungen verarbeiten.

Die erforderlichen Replikate können mithilfe des folgenden Codes berechnet werden:

```python
from math import ceil
# target requests per second
targetRps = 20
# time to process the request (in seconds)
reqTime = 10
# Maximum requests per container
maxReqPerContainer = 1
# target_utilization. 70% in this example
targetUtilization = .7

concurrentRequests = targetRps * reqTime / targetUtilization

# Number of container replicas
replicas = ceil(concurrentRequests / maxReqPerContainer)
```

Weitere Informationen zum Festlegen von `autoscale_target_utilization`, `autoscale_max_replicas` und `autoscale_min_replicas` finden Sie in der Referenz zu [AksWebservice.deploy_configuration](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none-).

#### <a name="create-a-new-cluster"></a>Erstellen eines neuen Clusters

Verwenden Sie zum Erstellen eines neuen Azure Kubernetes Service-Clusters den folgenden Code:

> [!IMPORTANT]
> Das Erstellen des AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Prozess. Nach der Erstellung können Sie diesen Cluster für mehrere Bereitstellungen wiederverwenden. Wenn Sie den Cluster oder die Ressourcengruppe, die ihn enthält, löschen, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen.
> Wenn Sie für [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) benutzerdefinierte Werte für agent_count und vm_size auswählen, müssen Sie sicherstellen, dass agent_count multipliziert mit vm_size mindestens 12 virtuellen CPUs entspricht. Wenn Sie beispielsweise für vm_size „Standard_D3_v2“ verwenden, das 4 virtuelle CPUs hat, müssen Sie für agent_count einen Wert von mindestens 3 wählen.

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

**Geschätzter Zeitaufwand**: Ca. 20 Minuten.

#### <a name="use-an-existing-cluster"></a>Verwenden eines vorhandenen Clusters

Wenn Sie in Ihrem Azure-Abonnement bereits über einen AKS-Cluster mit der Version 1.11.## und mindestens 12 virtuellen CPUs verfügen, können Sie diesen für die Bereitstellung Ihres Images verwenden. Der folgende Code veranschaulicht das Anfügen eines vorhandenen AKS-Clusters der Version 1.11.## an Ihren Arbeitsbereich:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)

# Wait for the operation to complete
aks_target.wait_for_completion(True)
```

**Geschätzter Zeitaufwand**: Ca. 3 Minuten.

Weitere Informationen zum Erstellen eines AKS-Clusters außerhalb des Azure Machine Learning SDK finden Sie in den folgenden Artikeln:

* [az aks create](https://docs.microsoft.com/cli/azure/aks?toc=%2Fen-us%2Fazure%2Faks%2FTOC.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Schnellstart: Bereitstellen eines AKS-Clusters (Azure Kubernetes Service) über das Azure-Portal](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

#### <a name="deploy-the-image"></a>Bereitstellen des Image

Um das im Abschnitt [Erstellen des Images](#createimage) dieses Dokuments erstellte Image dem Azure Kubernetes Server-Cluster bereitzustellen, verwenden Sie den folgenden Code:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
service = Webservice.deploy_from_image(workspace = ws,
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
service.wait_for_deployment(show_output = True)
print(service.state)
```

**Geschätzter Zeitaufwand**: Ca. 3 Minuten.

Weitere Informationen finden Sie in der Referenzdokumentation für die Klassen [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) und [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py).

### <a id="azuremlcompute"></a> Rückschluss mit Azure ML Compute

Azure ML-Computeziele werden von Azure Machine Learning Service erstellt und verwaltet. Sie können für Batchvorhersagen über Azure ML-Pipelines verwendet werden.

Eine exemplarische Vorgehensweise für Batchrückschlüsse mit Azure ML Compute finden Sie unter [Ausführen von Batchvorhersagen für große Datasets mit Azure Machine Learning Service](how-to-run-batch-predictions.md).


### <a id="fpga"> Bereitstellen auf Field Programmable Gate Arrays (FPGAs)</a>

Mit Project Brainwave kann eine extrem kurze Wartezeit für Echtzeit-Inferenzierungsanforderungen erzielt werden. Project Brainwave beschleunigt tiefe neuronale Netzwerke (Deep Neural Networks, DNNS), die auf Field-Programmable Gate Arrays in der Azure-Cloud bereitgestellt sind. Häufig verwendete DNNs sind als Featurizer für das Lernen durch Transfer verfügbar oder können mit Gewichtungen angepasst werden, die mit Ihren eigenen Daten trainiert sind.

Eine exemplarische Vorgehensweise zur Bereitstellung eines Modells mithilfe von Project Brainwave finden Sie im Dokument [Deploy to a FPGA (Bereitstellen in einem FPGA)](how-to-deploy-fpga-web-service.md).

## <a name="define-schema"></a>Definieren des Schemas

Beim Bereitstellen des Webdiensts können benutzerdefinierte Decorator-Elemente für die Erstellung von [OpenAPI](https://swagger.io/docs/specification/about/)-Spezifikationen und die Änderung von Eingabetypen verwendet werden. In der Datei `score.py` wird ein Beispiel der Ein- und/oder Ausgabe im Konstruktor für eines der definierten Typobjekte bereitgestellt. Das Schema wird daraufhin automatisch auf der Grundlage des Typs und des Beispiels generiert. Folgende Typen werden derzeit unterstützt:

* `pandas`
* `numpy`
* `pyspark`
* Python (Standard)

Vergewissern Sie sich zunächst, dass die erforderlichen Abhängigkeiten für das Paket `inference-schema` in Ihrer Conda-Umgebungsdatei `env.yml` enthalten sind. Da in diesem Beispiel der Parametertyp `numpy` für das Schema verwendet wird, wird auch der pip-Zusatz `[numpy-support]` installiert.

```python
%%writefile myenv.yml
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn
    - inference-schema[numpy-support]
```

Ändern Sie als Nächstes die Datei `score.py`, um die Pakete vom Typ `inference-schema` zu importieren. Definieren Sie in den Variablen `input_sample` und `output_sample` das Format für das Ein- und Ausgabebeispiel. (Die Variablen stellen das Anforderungs- und das Antwortformat für den Webdienst dar.) Verwenden Sie diese Beispiele in den Decorator-Elementen der Ein- und Ausgabefunktion für die Funktion `run()`.

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType


def init():
    global model
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)


input_sample = np.array([[1.8]])
output_sample = np.array([43638.88])

@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

Rufen Sie den Swagger-URI aus dem Dienst ab, nachdem Sie die normale Imageregistrierung und die Webdienstbereitstellung mit der aktualisierten Datei `score.py` durchlaufen haben. Wenn Sie diesen URI anfordern, wird die Datei `swagger.json` zurückgegeben.

```python
service.wait_for_deployment(show_output=True)
print(service.swagger_uri)
```



Wenn Sie ein neues Image erstellen, müssen Sie jeden Dienst, der das neue Image verwenden soll, manuell aktualisieren. Verwenden Sie die Methode `update`, um den Webdienst zu aktualisieren. Der folgende Code veranschaulicht, wie Sie den Webdienst so aktualisieren, dass er ein neues Image verwendet:

```python
from azureml.core.webservice import Webservice
from azureml.core.image import Image

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# point to a different image
new_image = Image(workspace = ws, id="myimage2:1")

# Update the image used by the service
service.update(image = new_image)
print(service.state)
```

Weitere Informationen finden Sie in der Referenzdokumentation zur [Webdienst](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)-Klasse.

## <a name="test-web-service-deployments"></a>Testen der Webdienstbereitstellungen

Um eine Webdienstbereitstellung zu testen, können Sie die Methode `run` des Webdienstobjekts verwenden. Im folgenden Beispiel wird ein JSON-Dokument auf einen Webdienst festgelegt und das Ergebnis angezeigt. Die gesendeten Daten müssen mit dem übereinstimmen, was das Modell erwartet. In diesem Beispiel entspricht das Format der Daten der Eingabe, die vom Diabetes-Modell erwartet wird.

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data = test_sample)
print(prediction)
```

Der Webdienst ist eine REST-API, sodass Sie Clientanwendungen in einer Reihe verschiedener Programmiersprachen erstellen können. Weitere Informationen finden Sie unter [Erstellen von Clientanwendungen zur Nutzung von Webdiensten](how-to-consume-web-service.md).

## <a id="update"></a> Aktualisieren des Webdiensts

Wenn Sie ein neues Image erstellen, müssen Sie jeden Dienst, der das neue Image verwenden soll, manuell aktualisieren. Verwenden Sie die Methode `update`, um den Webdienst zu aktualisieren. Der folgende Code veranschaulicht, wie Sie den Webdienst so aktualisieren, dass er ein neues Image verwendet:

```python
from azureml.core.webservice import Webservice
from azureml.core.image import Image

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# point to a different image
new_image = Image(workspace = ws, id="myimage2:1")

# Update the image used by the service
service.update(image = new_image)
print(service.state)
```

Weitere Informationen finden Sie in der Referenzdokumentation zur [Webdienst](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)-Klasse.

## <a id="iotedge"></a> Bereitstellen für Azure IoT Edge

Ein Azure IoT Edge-Gerät ist ein Linux- oder Windows-basiertes Gerät, das die Azure IoT Edge-Runtime ausführt. Mithilfe von Azure IoT Hub können Sie Machine Learning-Modelle für diese Geräte als IoT Edge-Module bereitstellen. Das Bereitstellen eines Modells auf einem IoT Edge-Gerät ermöglicht es dem Gerät, das Modell direkt zu verwenden, anstatt die Daten für die Verarbeitung an die Cloud zu senden. So können Sie die Antwortzeiten verkürzen und die Datenübertragung reduzieren.

Azure IoT Edge-Module werden aus einer Containerregistrierung auf Ihrem Gerät bereitgestellt. Wenn Sie ein Image aus Ihrem Modell erstellen, wird es in der Containerregistrierung für Ihren Arbeitsbereich gespeichert.

> [!IMPORTANT]
> Bei den Informationen in diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit Azure IoT Hub und Azure IoT Edge-Modulen vertraut sind. Einige der Informationen in diesem Abschnitt beziehen sich zwar speziell auf Azure Machine Learning Service, der Großteil der Bereitstellung für ein Edgegerät findet jedoch im Azure IoT-Dienst statt.
>
> Sollten Sie noch nicht mit Azure IoT vertraut sein, können Sie sich in der [Dokumentation zu Azure IoT (Internet der Dinge)](https://docs.microsoft.com/azure/iot-fundamentals/) sowie unter [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) einen allgemeinen Überblick verschaffen. Verwenden Sie anschließend die anderen Links in diesem Abschnitt, um sich ausführlicher mit bestimmten Vorgängen zu beschäftigen.

### <a name="set-up-your-environment"></a>Einrichten der Umgebung

* Eine Entwicklungsumgebung. Weitere Informationen finden Sie in der Dokumentation [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).

* Ein [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement.

* Ein trainiertes Modell. Ein Beispiel zum Trainieren eines Modells finden Sie in der Dokumentation [Train an image classification model with Azure Machine Learning (Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning)](tutorial-train-models-with-aml.md). Ein vortrainiertes Modell steht im [AI-Toolkit für Azure IoT Edge-GitHub-Repository](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial) zur Verfügung.

### <a id="getcontainer"></a> Abrufen der Anmeldeinformationen für die Containerregistrierung

Um ein IoT Edge-Modul auf Ihrem Gerät bereitzustellen, benötigt Azure IoT die Anmeldeinformationen für die Containerregistrierung, in der der Azure Machine Learning Service Docker-Images speichert.

Die Anmeldeinformationen können auf zwei Arten abgerufen werden:

+ **Im Azure-Portal**:

  1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/signin/index) an.

  1. Wechseln Sie zum Arbeitsbereich Ihres Azure Machine Learning-Diensts, und klicken Sie auf __Übersicht__. Klicken Sie auf den Link __Registrierung__, um zu den Einstellungen der Containerregistrierung zu wechseln.

     ![Ein Bild des Containerregistrierungseintrags](./media/how-to-deploy-and-where/findregisteredcontainer.png)

  1. Klicken Sie in der Containerregistrierung auf **Zugriffsschlüssel**, und aktivieren Sie dann den Administratorbenutzer.

     ![Ein Bild des Bildschirms „Zugriffsschlüssel“](./media/how-to-deploy-and-where/findaccesskey.png)

  1. Speichern Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**.

+ **Mit einem Python-Skript**:

  1. Verwenden Sie das folgende Python-Skript nach dem Code, den Sie oben zum Erstellen eines Containers ausgeführt haben:

     ```python
     # Getting your container details
     container_reg = ws.get_details()["containerRegistry"]
     reg_name=container_reg.split("/")[-1]
     container_url = "\"" + image.image_location + "\","
     subscription_id = ws.subscription_id
     from azure.mgmt.containerregistry import ContainerRegistryManagementClient
     from azure.mgmt import containerregistry
     client = ContainerRegistryManagementClient(ws._auth,subscription_id)
     result= client.registries.list_credentials(resource_group_name, reg_name, custom_headers=None, raw=False)
     username = result.username
     password = result.passwords[0].value
     print('ContainerURL{}'.format(image.image_location))
     print('Servername: {}'.format(reg_name))
     print('Username: {}'.format(username))
     print('Password: {}'.format(password))
     ```
  1. Speichern Sie die Werte für ContainerURL, Servername, Benutzername und Kennwort.

     Diese Anmeldeinformationen sind erforderlich, um dem IoT Edge-Gerät Zugriff auf Images in Ihrer privaten Containerregistrierung zu gewähren.

### <a name="prepare-the-iot-device"></a>Vorbereiten des IoT-Geräts

Registrieren Sie Ihr Gerät bei Azure IoT Hub, und installieren Sie anschließend die IoT Edge-Runtime auf dem Gerät. Weitere Informationen zu diesem Prozess finden Sie bei Bedarf unter [Schnellstart: Bereitstellen Ihres ersten IoT Edge-Moduls auf einem Linux-Gerät](../../iot-edge/quickstart-linux.md).

Geräte können auch mit folgenden Methoden registriert werden:

* [Azure-Portal](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-cli)
* [Visual Studio Code](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-vscode)

### <a name="deploy-the-model-to-the-device"></a>Bereitstellen des Modells für das Gerät

Verwenden Sie die im Abschnitt [Abrufen von Anmeldeinformationen für die Containerregistrierung](#getcontainer) erfassten Registrierungsinformationen mit den Modulbereitstellungsschritten für IoT Edge-Module, um das Modell für das Gerät bereitzustellen. Wenn Sie also beispielsweise [Azure IoT Edge-Module über das Azure-Portal bereitstellen](../../iot-edge/how-to-deploy-modules-portal.md), müssen Sie die __Registrierungseinstellungen__ für das Gerät konfigurieren. Verwenden Sie __Anmeldeserver__, __Benutzername__, und __Kennwort__ für Ihre Arbeitsbereich-Containerregistrierung.

Die Bereitstellung kann auch über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-cli) sowie mit [Visual Studio Code](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-vscode) ausgeführt werden.

## <a name="clean-up"></a>Bereinigen

Verwenden Sie zum Löschen eines bereitgestellten Webdiensts `service.delete()`.

Verwenden Sie zum Löschen eines Image `image.delete()`.

Verwenden Sie zum Löschen eines registrierten Modells `model.delete()`.

Weitere Informationen finden Sie in der Referenzdokumentation zu [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), [Image.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#delete--) und [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung von Bereitstellungen von Azure Machine Learning Service mit AKS und ACI](how-to-troubleshoot-deployment.md)
* [Secure Azure Machine Learning web services with SSL (Sichere Azure Machine Learning-Webdienste mit SSL)](how-to-secure-web-service.md)
* [Consume a ML Model deployed as a web service (Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells)](how-to-consume-web-service.md).
* [How to run batch predictions (Ausführen von Batchvorhersagen)](how-to-run-batch-predictions.md)
* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)
* [SDK für Azure Machine Learning Service](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Verwenden von Azure Machine Learning Service mit Azure Virtual Networks](how-to-enable-virtual-network.md)
* [Bewährte Methoden für das Erstellen von Empfehlungssystemen](https://github.com/Microsoft/Recommenders)
* [Erstellen einer Echtzeitempfehlungs-API in Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
