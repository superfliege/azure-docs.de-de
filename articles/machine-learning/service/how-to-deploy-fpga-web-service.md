---
title: Bereitstellen von Modellen auf FPGAs
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie ein Webdienst mit einem Modell bereitgestellt wird, das auf einem FPGA mit Azure Machine Learning Service für extrem geringe Latenzzeiten ausgeführt wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 1/29/2019
ms.custom: seodec18
ms.openlocfilehash: d7403d4f791151b167f03ee79c30623bbd644008
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100683"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Bereitstellen eines Modells als Webdienst auf einem FPGA mit Azure Machine Learning Service

Sie können ein Modell als Webdienst in [Field Programmable Gate Array (FPGAs)](concept-accelerate-with-fpgas.md) bereitstellen.  FPGAs bieten extrem geringe Latenzstörungen, sogar mit einer einzelnen Batchgröße.  Diese Modelle sind derzeit verfügbar:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16   

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie heute die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](http://aka.ms/AMLFree) aus.

- Ein Azure Machine Learning Service-Arbeitsbereich und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erhalten dieser Voraussetzungen finden Sie im Dokument [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).
 
  - Ihr Arbeitsbereich muss sich in der Region *USA, Osten 2* befinden.

  - Installieren Sie die contrib extras:

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```

  - Derzeit wird nur die Tensorflow-Version<=1.10 unterstützt, also installieren Sie sie, nachdem alle anderen Installationen abgeschlossen sind:

    ```shell
    pip install "tensorflow==1.10"
    ```

### <a name="get-the-notebook"></a>Abrufen des Notebooks

Dieses Tutorial steht Ihnen auch als Jupyter Notebook zur Verfügung. Befolgen Sie den Code hier oder führen Sie das [Schnellstart-Notebook](https://github.com/Azure/aml-real-time-ai/blob/master/notebooks/project-brainwave-quickstart.ipynb) aus.

## <a name="create-and-deploy-your-model"></a>Erstellen und Bereitstellen Ihres Modells
Erstellen Sie eine Pipeline, um das Image vorzuverarbeiten, statten Sie es mit ResNet 50 auf einem FPGA mit Features aus, und führen Sie diese über einen anhand eines ImageNet-Datasets trainierten Klassifizierer aus.

Befolgen Sie die Anweisungen für folgende Punkte:

* Definieren des Pipelinemodells
* Bereitstellen des Modells
* Nutzen des bereitgestellten Modells
* Löschen bereitgestellter Dienste

> [!IMPORTANT]
> Um die Latenzzeit und den Durchsatz zu optimieren, sollte sich Ihren Client in derselben Azure-Region wie der Endpunkt befinden.  Derzeit werden die APIs in der Azure-Region „USA, Osten“ erstellt.



### <a name="preprocess-image"></a>Vorverarbeiten des Image
Die erste Stufe der Pipeline ist das Vorverarbeiten der Images.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="add-featurizer"></a>Hinzufügen des Featurizer
Initialisieren Sie das Modell und laden Sie einen TensorFlow-Checkpoint der quantisierten Version von ResNet50 herunter, der als Featurizer verwendet werden soll.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Hinzufügen des Klassifizierers
Dieser Klassifizierer wurde anhand eines ImageNet-Datasets trainiert.

```python
classifier_output = model.get_default_classifier(feature_tensor)
```

### <a name="create-service-definition"></a>Erstellen einer Dienstdefinition
Nachdem Sie jetzt die Bildvorverarbeitung, den Featurizer und den Klassifizierer, der auf dem Dienst ausgeführt, definiert haben, können Sie eine Dienstdefinition erstellen. Die Dienstdefinition ist eine Dateiensatz, der aus dem Modell generiert wird, das für den FPGA-Dienst bereitgestellt wird. Die Dienstdefinition besteht aus einer Pipeline. Die Pipeline ist eine Reihe von Stufen, die nacheinander ausgeführt werden.  TensorFlow-, Keras- und BrainWave-Stufen werden unterstützt.  Die Stufen werden nacheinander im Dienst ausgeführt, wobei die Ausgabe jeder Stufe in die nachfolgende Stufe eingeht.

Um eine TensorFlow-Stufe zu erstellen, geben Sie eine Sitzung an, die den Graph (in diesem Fall wird der Standardgraph verwendet) und die Ein- und Ausgabe-Tensoren zu dieser Stufe enthält.  Diese Informationen werden zum Speichern des Graph verwendet, sodass dieser auf dem Dienst ausgeführt werden kann.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'model_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, feature_tensor, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Bereitstellen des Modells
Erstellen Sie einen Dienst aus der Dienstdefinition.  Ihr Arbeitsbereich muss sich in der Region „USA, Osten 2“ befinden.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(True)
```

### <a name="test-the-service"></a>Testen des Diensts
Um ein Image an die API zu senden und die Antwort zu testen, fügen Sie ein Mapping von der Ausgabeklassen-ID auf den ImageNet-Klassennamen hinzu.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Rufen Sie Ihren Dienst auf und ersetzen Sie den untenstehenden Dateinamen „ihr-image.jpg“ durch ein Image von Ihrem Computer. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Bereinigen des Dienstes
Löschen Sie den Dienst.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>Sichere FPGA-Webdienste

Weitere Informationen zum Absichern von FPGA-Webdiensten finden Sie im Dokument [Absichern von Webdiensten](how-to-secure-web-service.md).


## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die [Nutzung eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md).
