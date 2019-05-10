---
title: Bereitstellen eines Deep Learning-Modells für das Ziehen von Rückschlüssen mit einer GPU
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie ein Deep Learning-Modell als Webdienst bereitstellen, der eine GPU zum Ziehen von Rückschlüssen verwendet. In diesem Artikel wird ein Tensorflow-Modell in einem Azure Kubernetes Service-Cluster bereitgestellt. Der Cluster verwendet einen GPU-fähigen virtuellen Computer, um den Webdienst zu hosten und Rückschlussanforderungen zu bewerten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: 5cc0fe0526937245d3ca913afc477f0259e2afd4
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65515038"
---
# <a name="how-to-do-gpu-inferencing"></a>Ziehen von Rückschlüssen mit einer GPU

Erfahren Sie, wie Sie GPU-Rückschlüsse für ein als Webdienst bereitgestelltes Machine Learning-Modell verwenden. In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure Machine Learning Service ein exemplarisches Tensorflow-Deep Learning-Modell bereitstellen. Das Modell wird in einem Azure Kubernetes Service (AKS)-Cluster bereitgestellt, der einen GPU-fähigen virtuellen Computer zum Hosten des Diensts verwendet. Wenn Anforderungen an den Dienst gesendet werden, verwendet das Modell die GPU zur Durchführung von Rückschlussvorgängen.

GPUs bieten Leistungsvorteile gegenüber CPUs bei hochparallelisierbarer Berechnung. Das Trainieren von und Durchführen von Rückschlussvorgängen für Deep Learning-Modelle (insbesondere für große Anforderungsbatches) sind hervorragende Anwendungsfälle für GPUs.  

Dieses Beispiel veranschaulicht, wie Sie ein gespeichertes TensorFlow-Modell in Azure Machine Learning bereitstellen können. 

## <a name="goals-and-prerequisites"></a>Ziele und Voraussetzungen

Befolgen Sie die Anweisungen für folgende Punkte:
* Erstellen eines GPU-fähigen AKS-Clusters
* Bereitstellen eines Modells mit Tensorflow-GPU

Voraussetzungen:
* Azure Machine Learning Service-Arbeitsbereich
* Python
* Tensorflow SavedModel (registriert). Informationen zum Registrieren von Modellen finden Sie unter [Bereitstellen von Modellen](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#registermodel).

Dieser Artikel basiert auf der [Bereitstellung von Tensorflow-Modellen in AKS](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/production-deploy-to-aks-gpu/production-deploy-to-aks-gpu.ipynb), wobei gespeicherte TensorFlow-Modelle verwendet und in einem AKS-Cluster bereitgestellt werden. Mit kleinen Änderungen an der Bewertungsdatei und der Umgebungsdatei ist das Verfahren jedoch für jedes Framework für maschinelles Lernen anwendbar, das GPUs unterstützt.  

## <a name="provision-aks-cluster-with-gpus"></a>Bereitstellen eines AKS-Clusters mit GPUs
Azure verfügt über zahlreiche verschiedene GPU-Optionen, die alle für das Ziehen von Rückschlüssen verwendet werden können. In der [Liste der N-Serie](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) finden Sie eine vollständige Aufschlüsselung der Funktionen und Kosten. 

Weitere Informationen zur Verwendung von AKS mit Azure Machine Learning Service finden Sie im Artikel [Wie und wo Modelle bereitgestellt werden](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#create-a-new-cluster).

```python
# Provision AKS cluster with GPU machine
prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

# Create the cluster
aks_target = ComputeTarget.create(
    workspace=ws, name=aks_name, provisioning_configuration=prov_config
)

aks_target.wait_for_deployment()
```

> [!IMPORTANT]
> Azure berechnet Ihnen Gebühren, solange der AKS-Cluster bereitgestellt ist. Achten Sie darauf, Ihren AKS-Cluster zu löschen, wenn Sie ihn nicht mehr nutzen.


## <a name="write-entry-script"></a>Schreiben des Eingangsskripts

Speichern Sie den folgenden Code als `score.py` in Ihrem Arbeitsverzeichnis. Diese Datei wird zur Bewertung von Bildern verwendet, während sie an Ihren Dienst gesendet werden. Diese Datei lädt das gespeicherte TensorFlow-Modell, und übergibt dann bei jeder POST-Anforderung das Eingabebild an die TensorFlow-Sitzung und gibt die resultierenden Bewertungen zurück.
Andere Frameworks zum Ziehen von Rückschlüssen erfordern andere Bewertungsdateien.

```python
import tensorflow as tf
import numpy as np
import ujson
from azureml.core.model import Model
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    global session
    global input_name
    global output_name
    
    session = tf.Session()

    model_path = Model.get_model_path('resnet50')
    model = tf.saved_model.loader.load(session, ['serve'], model_path)
    if len(model.signature_def['serving_default'].inputs) > 1:
        raise ValueError("This score.py only supports one input")
    input_name = [tensor.name for tensor in model.signature_def['serving_default'].inputs.values()][0]
    output_name = [tensor.name for tensor in model.signature_def['serving_default'].outputs.values()]
    

@rawhttp
def run(request):
    if request.method == 'POST':
        reqBody = request.get_data(False)
        resp = score(reqBody)
        return AMLResponse(resp, 200)
    if request.method == 'GET':
        respBody = str.encode("GET is not supported")
        return AMLResponse(respBody, 405)
    return AMLResponse("bad request", 500)

def score(data):
    result = session.run(output_name, {input_name: [data]})
    return ujson.dumps(result[1])

if __name__ == "__main__":
    init()
    with open("lynx.jpg", 'rb') as f: #load file for testing locally
        content = f.read()
        print(score(content))

```

## <a name="define-conda-environment"></a>Definieren einer Conda-Umgebung
Erstellen Sie eine Conda-Umgebungsdatei namens `myenv.yml`, um die Abhängigkeiten für Ihren Dienst anzugeben. Es ist wichtig, anzugeben, dass Sie `tensorflow-gpu` verwenden, um eine beschleunigte Leistung zu erzielen.
```yaml
name: aml-accel-perf
channels:
  - defaults
dependencies:
  - tensorflow-gpu = 1.12
  - numpy
  - ujson
  - pip:
    - azureml-core
    - azureml-contrib-services
```

## <a name="define-gpu-inferenceconfig"></a>Definieren des GPU-Elements „InferenceConfig“

Erstellen Sie ein [`InferenceConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py), das angibt, dass Sie die GPU aktivieren. Dadurch wird sichergestellt, dass CUDA mit Ihrem Bild installiert wird.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='gpu-rn'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"resnet50")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   gpu_enabled=True)
```

Weitere Informationen finden Sie unter [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) und [AksServiceDeploymentConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py).
## <a name="deploy-the-model"></a>Bereitstellen des Modells

Stellen Sie dieses Modell in Ihrem AKS-Cluster bereit, und warten Sie, bis es den Dienst erstellt.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Azure Machine Learning Service stellt kein Modell mit einem `InferenceConfig`-Element bereit, das GPU für einen Cluster ohne GPU erwartet.

Weitere Informationen finden Sie unter [Modell](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-sample-query-to-deployed-model"></a>Ausgeben einer Beispielabfrage an das bereitgestellte Modell

Geben Sie eine Beispielabfrage an Ihr bereitgestelltes Modell aus. Dieses Modell bewertet jedes JPEG-Bild, das Sie ihm als POST-Anforderung senden. 

```python
scoring_url = aks_service.scoring_uri
api_key = aks_service.get_key()(0)
IMAGEURL = "https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Lynx_lynx_poing.jpg/220px-Lynx_lynx_poing.jpg"

headers = {'Authorization':('Bearer '+ api_key)}
img_data = read_image_from(IMAGEURL).read()
r = requests.post(scoring_url, data = img_data, headers=headers)
```

> [!IMPORTANT]
> Um die Latenzzeit und den Durchsatz zu optimieren, sollte sich Ihren Client in derselben Azure-Region wie der Endpunkt befinden.  Derzeit werden die APIs in der Azure-Region „USA, Osten“ erstellt.

## <a name="cleaning-up-the-resources"></a>Bereinigen der Ressourcen

Löschen Sie Ihre Ressourcen, nachdem Sie die Demo abgeschlossen haben.

> [!IMPORTANT]
> Azure berechnet Ihnen Gebühren basierend darauf, wie lange der AKS-Cluster bereitgestellt wird. Achten Sie darauf, ihn zu bereinigen, wenn Sie ihn nicht mehr nutzen.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen eines Modells auf einem FPGA](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service)
* [Bereitstellen eines Modells mit ONNX](https://docs.microsoft.com/azure/machine-learning/service/how-to-build-deploy-onnx#deploy)
* [Trainieren von TensorFlow-DNN-Modellen](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-tensorflow)
