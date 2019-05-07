---
title: Bereitstellen von Modellen auf FPGAs
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie ein Webdienst mit einem Modell bereitgestellt wird, das auf einem FPGA mit Azure Machine Learning Service für extrem geringe Latenzzeiten ausgeführt wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: tedway
author: tedway
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: cfe21d2119b92665c5950d792dec6500257c6316
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024177"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Bereitstellen eines Modells als Webdienst auf einem FPGA mit Azure Machine Learning Service

Sie können ein Modell als Webdienst auf [FPGAs (Field Programmable Gate Arrays)](concept-accelerate-with-fpgas.md) mit hardwarebeschleunigten Azure Machine Learning-Modellen bereitstellen. FPGAs bieten extrem geringe Latenzstörungen, sogar mit einer einzelnen Batchgröße.

Diese Modelle sind derzeit verfügbar:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-VGG

Die FPGAs sind in diesen Azure-Regionen verfügbar:
  - USA, Osten
  - USA, Westen 2
  - Europa, Westen
  - Asien, Südosten

> [!IMPORTANT]
> Um die Latenz und den Durchsatz zu optimieren, sollte sich Ihr Client, der Daten an das FPGA-Modell sendet, in einer der oben genannten Regionen befinden (in derjenigen Region, in der Sie das Modell bereitgestellt haben).

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie heute die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

- Ein Azure Machine Learning Service-Arbeitsbereich und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erhalten dieser Voraussetzungen finden Sie im Dokument [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).
 
  - Installieren Sie das Python SDK für hardwarebeschleunigte Modelle:

    ```shell
    pip install --upgrade azureml-accel-models
    ```

## <a name="sample-notebooks"></a>Beispielnotebooks

Es stehen [Beispielnotebooks](https://aka.ms/aml-notebooks) für das folgende Beispiel und zusätzlich zu anderen Beispielen zur Verfügung.  Suchen Sie unter how-to-use-azureml und Bereitstellungsordnern nach beschleunigten Modellen.

## <a name="create-and-containerize-your-model"></a>Erstellen und Containerisieren des Modells

Dieses Dokument beschreibt, wie ein TensorFlow-Diagramm erstellt wird, um das Eingabebild vorzuverarbeiten, es mit ResNet 50 auf einem FPGA als Feature zu erklären und die Features dann über einen anhand eines ImageNet-Datasets trainierten Klassifizierer auszuführen.

Befolgen Sie die Anweisungen für folgende Punkte:

* Definieren des TensorFlow-Modells
* Bereitstellen des Modells
* Nutzen des bereitgestellten Modells
* Löschen bereitgestellter Dienste

### <a name="load-azure-ml-workspace"></a>Laden des Azure ML-Arbeitsbereichs

Laden Sie Ihren Azure ML-Arbeitsbereich.

```python
import os
import tensorflow as tf
 
from azureml.core import Workspace
 
ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
```

### <a name="preprocess-image"></a>Vorverarbeiten des Bilds

Die Eingabe an den Webdienst ist ein JPEG-Bild.  Der erste Schritt besteht im Decodieren und Vorverarbeiten des JPEG-Bilds.  JPEG-Bilder werden als Zeichenfolgen behandelt, und das Ergebnis sind Tensor-Elemente, die als Eingaben für das ResNet 50-Modell verwendet werden.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Laden des Featurizer

Initialisieren Sie das Modell und laden Sie einen TensorFlow-Checkpoint der quantisierten Version von ResNet50 herunter, der als Featurizer verwendet werden soll.  Sie können „QuantizedResnet50“ im Codeausschnitt unten ersetzen, indem Sie andere Deep Neural Networks importieren:

- QuantizedResnet152
- QuantizedVgg16
- Densenet121

```python
from azureml.accel.models import QuantizedResnet50
save_path = os.path.expanduser('~/models')
model_graph = QuantizedResnet50(save_path, is_frozen = True)
feature_tensor = model_graph.import_graph_def(image_tensors)
print(model_graph.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Hinzufügen des Klassifizierers

Dieser Klassifizierer wurde anhand eines ImageNet-Datasets trainiert.  Beispiele für Übertragungslernen und das Training Ihrer individuellen Gewichtungen finden Sie in der Sammlung der [Beispielnotebooks](https://aka.ms/aml-notebooks).

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Speichern des Modells

Nachdem nun der Präprozessor, der ResNet 50-Featurizer und der Klassifizierer geladen wurden, speichern Sie das Diagramm und die zugehörigen Variablen als Modell.

```python
model_name = "resnet50"
model_def_path = os.path.join(save_path, model_name)
print("Saving model in {}".format(model_def_path))

with tf.Session() as sess:
    model_graph.restore_weights(sess)
    tf.saved_model.simple_save(sess, model_def_path,
                                   inputs={'images': in_images},
                                   outputs={'output_alias': classifier_output})
```

### <a name="register-model"></a>Registrieren des Modells

[Registrieren](./concept-model-management-and-deployment.md) Sie das Modell, das Sie erstellt haben.  Das Hinzufügen von Tags und anderen Metadaten zum Modell ermöglicht Ihnen das Nachverfolgen Ihrer trainierten Modelle.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace = ws
                                  model_path = model_def_path,
                                  model_name = model_name)

print("Successfully registered: ", registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

Wenn Sie ein Modell bereits registriert haben und es laden möchten, können Sie es abrufen.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

### <a name="convert-model"></a>Konvertieren des Modells

Das TensorFlow-Diagramm muss in das [ONNX](https://onnx.ai/)-Format (Open Neural Network Exchange-Format) konvertiert werden.  Sie müssen die Namen der Tensor-Ein- und -Ausgabeelemente angeben. Diese Namen werden von Ihrem Client verwendet, wenn Sie den Webdienst nutzen.

```python
input_tensor = in_images.name
output_tensors = classifier_output.name

print(input_tensor)
print(output_tensors)


from azureml.accel.accel_onnx_converter import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(ws, registered_model, input_tensor, output_tensors)
convert_request.wait_for_completion(show_output=True)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print(converted_model.name, converted_model.url, converted_model.version, converted_model.id,converted_model.created_time)
```

### <a name="create-docker-image"></a>Erstellen des Docker-Images

Das konvertierte Modell und alle Abhängigkeiten werden einem Docker-Image hinzugefügt.  Dieses Docker-Image kann dann bereitgestellt und in der Cloud oder auf einem unterstützten Edge-Gerät (z.B. [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview)) instanziiert werden.  Sie können auch Tags und Beschreibungen für das registrierte Docker-Image hinzufügen.

```python
from azureml.core.image import Image
from azureml.accel.accel_container_image import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
image_name = "{}-image".format(model_name)

image = Image.create(name = image_name,
                     models = [converted_model],
                     image_config = image_config, 
                     workspace = ws)


image.wait_for_creation(show_output = True)
```

Listen Sie die Images nach Tag auf, und rufen Sie die ausführlichen Protokolle für Debugzwecke ab.

```python
for i in Image.list(workspace = ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="model-deployment"></a>Modellbereitstellung

### <a name="deploy-to-the-cloud"></a>Bereitstellen in der Cloud

Um Ihr Modell als hochgradig skalierbaren Produktionswebdienst bereitzustellen, verwenden Sie Azure Kubernetes Service (AKS). Sie können mit dem Azure Machine Learning SDK, der CLI oder dem Azure-Portal einen neuen AKS-Cluster erstellen.

```python
# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'my-aks-9' 
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                  name = aks_name, 
                                  provisioning_configuration = prov_config)

%%time
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)

#Set the web service configuration (using default here)
aks_config = AksWebservice.deploy_configuration()

%%time
aks_service_name ='aks-service-1'

aks_service = Webservice.deploy_from_image(workspace = ws, 
                                           name = aks_service_name,
                                           image = image,
                                           deployment_config = aks_config,
                                           deployment_target = aks_target)
aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
print(aks_service.scoring_uri)
```

#### <a name="test-the-cloud-service"></a>Testen des Clouddiensts
Das Docker-Image unterstützt gRPC und die TensorFlow Serving-API „predict“.  Verwenden Sie den Beispielclient zum Aufrufen des Docker-Images, um Vorhersagen aus dem Modell zu erhalten.  Clientbeispielcode ist verfügbar:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C#](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

Wenn Sie TensorFlow Serving verwenden möchten, können Sie [einen Beispielclient herunterladen](https://www.tensorflow.org/serving/setup).

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

# Score image using input and output tensor names
results = client.score_file(path="./snowleopardgaze.jpg", 
                             input_name=input_tensor, 
                             outputs=output_tensors)

# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
```

### <a name="clean-up-the-service"></a>Bereinigen des Diensts
Löschen Sie Ihren Webdienst, das Image und das Modell (muss in dieser Reihenfolge ausgeführt werden, da Abhängigkeiten vorhanden sind).

```python
aks_service.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

## <a name="deploy-to-a-local-edge-server"></a>Bereitstellung auf einem lokalen Edge-Server

Alle [Azure Data Box-Edge-Geräte](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) enthalten ein FPGA für die Ausführung des Modells.  Es kann nur jeweils ein Modell gleichzeitig auf dem FPGA ausgeführt werden.  Um ein anderes Modell auszuführen, stellen Sie einfach einen neuen Container bereit. Anleitungen und Beispielcode finden Sie in [diesem Azure-Beispiel](https://github.com/Azure-Samples/aml-hardware-accelerated-models).

## <a name="secure-fpga-web-services"></a>Sichere FPGA-Webdienste

Weitere Informationen zum Absichern von FPGA-Webdiensten finden Sie im Dokument [Absichern von Webdiensten](how-to-secure-web-service.md).