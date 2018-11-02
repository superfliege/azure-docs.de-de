---
title: ONNX und Azure Machine Learning | Erstellen und Bereitstellen von Modellen
description: Erfahren Sie mehr über ONNX und darüber, wie Sie mit Azure Machine Learning ONNX-Modelle erstellen und einsetzen können.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 09/24/2018
ms.openlocfilehash: 190b7fff24c9d6b3dee86471b56ad68c962e51ce
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116877"
---
# <a name="onnx-and-azure-machine-learning-create-and-deploy-interoperable-ai-models"></a>ONNX und Azure Machine Learning: Erstellen und Bereitstellen von interoperablen KI-Modellen

Das Format [Open Neural Network Exchange](http://onnx.ai) (ONNX) ist ein offener Standard für die Darstellung von Machine Learning-Modellen. ONNX wird von einer [Community von Partnern](http://onnx.ai/supported-tools) wie Microsoft unterstützt, die kompatible Frameworks und Tools erstellen. Microsoft setzt sich für eine offene und interoperable KI ein, um Datenanalysten und Entwicklern Folgendes zu ermöglichen:

+ Erstellen und Trainieren von Modellen mit dem Framework ihrer Wahl
+ Plattformübergreifende Bereitstellung von Modellen mit minimalem Integrationsaufwand

ONNX wird für alle Microsoft-Produkte, einschließlich Azure und Windows, unterstützt, damit Sie diese Ziele erreichen können.  

## <a name="why-choose-onnx"></a>Was spricht für ONNX?
Die durch ONNX erreichte Interoperabilität ermöglicht es, großartige Ideen schneller in die Produktionsumgebung zu übernehmen. Mit ONNX können Datenanalysten mit ihrem bevorzugten Framework arbeiten. Gleichermaßen können auch Entwickler davon profitieren, indem Sie weniger Zeit damit verbringen, Modelle für die Produktionsumgebung vorzubereiten oder diese in der Cloud und Edge-übergreifend bereitzustellen.  

Sie können ONNX-Modelle aus vielen Frameworks erstellen, darunter PyTorch, Chainer, Microsoft Cognitive Toolkit (CNTK), MXNet, ML.Net, TensorFlow, Keras, SciKit-Learn und weitere.

Darüber hinaus gibt es ein ganzes Ökosystem von Tools zur Visualisierung und Beschleunigung von ONNX-Modellen. Für gängige Szenarios stehen ebenfalls eine Reihe von vorab trainierten ONNX-Modellen zur Verfügung.

Mit Azure Machine Learning und der ONNX-Runtime können [ONNX-Modelle in der Cloud bereitgestellt werden](#deploy). Zudem ist die Bereitstellung auf Windows 10-Geräten mit [Windows ML](https://docs.microsoft.com/windows/ai/) möglich. Sie können sogar auf anderen Plattformen mithilfe von Konvertern bereitgestellt werden, die über die ONNX-Community verfügbar sind. 

[ ![ONNX-Flussdiagramm mit Training, Konvertern und Bereitstellung](media/concept-onnx/onnx.png) ] (./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>Abrufen von ONNX-Modellen

Es gibt verschiedene Möglichkeiten, an ONNX-Modelle zu gelangen:
+ Rufen Sie ein vortrainiertes ONNX-Modell im [ONNX-Modellzoo](https://github.com/onnx/models) ab (siehe dazu das Beispiel am Ende dieses Artikels)
+ Generieren eines benutzerdefinierten ONNX-Modells über [Azure Custom Vision Service](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) 
+ Konvertieren eines vorhandenen Modells aus einem anderen Format nach ONNX (siehe dazu das Beispiel am Ende dieses Artikels) 
+ Trainieren eines neuen ONNX-Modells im Azure Machine Learning-Dienst (siehe dazu das Beispiel am Ende dieses Artikels)

## <a name="saveconvert-your-models-to-onnx"></a>Speichern bzw. Konvertieren von Modellen nach ONNX

Sie können am Ende Ihrer Schulung vorhandene Modelle nach ONNX konvertieren oder sie als ONNX speichern.

|Framework für Modell|Konvertierungsbeispiel oder -Tool|
|-----|-------|
|PyTorch|[Jupyter-Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb)|
|Microsoft&nbsp;Cognitive&nbsp;Toolkit&nbsp;(CNTK)|[Jupyter-Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb)|
|TensorFlow|[tensorflow-onnx-Konverter](https://github.com/onnx/tensorflow-onnx)|
|Chainer|[Jupyter-Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/ChainerOnnxExport.ipynb)|
|MXNet|[Jupyter-Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/MXNetONNXExport.ipynb)|
|Keras, ScitKit-Learn, CoreML<br/>XGBoost und libSVM|[WinMLTools](https://docs.microsoft.com/windows/ai/convert-model-winmltools)|

Sie finden die aktuelle Liste der unterstützten Frameworks und Konverter auf der [Website mit den ONNX-Tutorials](https://github.com/onnx/tutorials).

<a name="deploy"></a>

## <a name="deploy-onnx-models-in-azure"></a>Bereitstellen von ONNX-Modellen in Azure

Mit dem Azure Machine Learning-Dienst können Sie Ihre ONNX-Modelle bereitstellen, verwalten und überwachen. Wenn Sie den [Standardbereitstellungsworkflow](concept-model-management-and-deployment.md) und die ONNX-Runtime verwenden, können Sie einen REST-Endpunkt erstellen, der in der Cloud gehostet wird. Sehen Sie sich am Ende dieses Artikels ein vollständiges Beispiel für ein Jupyter Notebook an, um es selbst auszuprobieren. 

### <a name="install-and-configure-the-onnx-runtime"></a>Installieren und Konfigurieren der ONNX-Runtime

Die ONNX-Runtime ist eine leistungsstarke Rückschluss-Engine für ONNX-Modelle. Sie umfasst eine Python-API und bietet Hardwarebeschleunigung für CPU und GPU. Derzeit unterstützt sie ONNX 1.2-Modelle und läuft unter Linux Ubuntu 16.04. Sowohl das [CPU](https://pypi.org/project/onnxruntime)- als auch das [GPU](https://pypi.org/project/onnxruntime-gpu)-Paket steht auf [PyPi.org](https://pypi.org) zur Verfügung.

Um die ONNX-Runtime zu installieren, verwenden Sie folgenden Code:
```python
pip install onnxruntime
```

Um die ONNX-Runtime in Ihrem Python-Skript aufzurufen, verwenden Sie folgenden Code:
```python
import onnxruntime

session = onnxruntime.InferenceSession("path to model")
```

In der dem Modell beiliegenden Dokumentation finden Sie in der Regel die Ein- und Ausgaben für die Verwendung des Modells. Sie können auch ein Visualisierungstool wie [Netron](https://github.com/lutzroeder/Netron) einsetzen, um das Modell anzuzeigen. Die ONNX-Runtime kann zudem verwendet werden, um Metadaten, Eingaben und Ausgaben des Modells abzufragen:
```python
session.get_modelmeta()
first_input_name = session.get_inputs()[0].name
first_output_name = session.get_outputs()[0].name
```

Um Rückschlüsse zu Ihrem Modell zu erhalten, verwenden Sie `run` und übergeben die Liste der Ausgaben, die zurückgegeben werden sollen (lassen Sie sie leer, falls alle gewünscht sind), und eine Zuordnung der Eingabewerte. Als Ergebnis erhalten Sie eine Liste der Ausgaben.
```python
results = session.run(["output1", "output2"], {"input1": indata1, "input2": indata2})
results = session.run([], {"input1": indata1, "input2": indata2})
```

Die vollständige API-Referenz finden Sie im Dokument zur [Referenzdokumentation zur ONNX-Runtime](https://aka.ms/onnxruntime-python).

### <a name="example-deployment-steps"></a>Beispiel für Bereitstellungsschritte

Hier sehen Sie ein Beispiel für die Bereitstellung eines ONNX-Modells:

1. Initialisieren Sie Ihren Azure Machine Learning-Dienstarbeitsbereich. Wenn Sie noch keinen haben, erfahren Sie [in diesem Schnellstart](quickstart-get-started.md), wie Sie einen Arbeitsbereich einrichten.

   ```python
   from azureml.core import Workspace

   ws = Workspace.from_config()
   print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
   ```

2. Registrieren Sie das Modell bei Azure Machine Learning.

   ```python
   from azureml.core.model import Model

   model = Model.register(model_path = "model.onnx",
                          model_name = "MyONNXmodel",
                          tags = ["onnx"],
                          description = "test",
                          workspace = ws)
   ```

3. Erstellen Sie ein Image mit dem Modell und allen Abhängigkeiten.

   ```python
   from azureml.core.image import ContainerImage
   
   image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                     runtime = "python",
                                                     conda_file = "myenv.yml",
                                                     description = "test",
                                                     tags = ["onnx"]
                                                    )

   image = ContainerImage.create(name = "myonnxmodelimage",
                                 # this is the model object
                                 models = [model],
                                 image_config = image_config,
                                 workspace = ws)

   image.wait_for_creation(show_output = True)
   ```

   Die Datei `score.py` enthält die Bewertungslogik und muss im Image enthalten sein. Diese Datei wird verwendet, um das Modell im Image auszuführen. In diesem [Tutorial](tutorial-deploy-models-with-aml.md#create-scoring-script) finden Sie Anweisungen zum Erstellen eines Bewertungsskripts. Eine Beispieldatei für ein ONNX-Modell wird unten gezeigt:

   ```python
   import onnxruntime
   import json
   import numpy as np
   import sys
   from azureml.core.model import Model

   def init():
       global model_path
       model_path = Model.get_model_path(model_name = 'MyONNXmodel')

   def run(raw_data):
       try:
           data = json.loads(raw_data)['data']
           data = np.array(data)
        
           sess = onnxruntime.InferenceSession(model_path)
           result = sess.run(["outY"], {"inX": data})
        
           return json.dumps({"result": result.tolist()})
       except Exception as e:
           result = str(e)
           return json.dumps({"error": result})
   ```

   Die Datei `myenv.yml` beschreibt die für das Image benötigten Abhängigkeiten. In diesem [Tutorial](tutorial-deploy-models-with-aml.md#create-environment-file) finden Sie Anweisungen zum Erstellen einer Umgebungsdatei, ähnlich dieser Beispieldatei:

   ```python
   from azureml.core.conda_dependencies import CondaDependencies 

   myenv = CondaDependencies()
   myenv.add_pip_package("numpy")
   myenv.add_pip_package("azureml-core")
   myenv.add_pip_package("onnxruntime")

   with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
   ```

4. Stellen Sie Ihr ONNX-Modell mit Azure Machine Learning hier bereit:
   + Azure Container Instances (ACI): [Erfahren Sie, wie...](how-to-deploy-to-aci.md)

   + Azure Kubernetes Service (AKS): [Erfahren Sie, wie...](how-to-deploy-to-aks.md)


## <a name="examples"></a>Beispiele
 
Die folgenden Notebooks veranschaulichen, wie Sie ONNX-Modelle erstellen und mit Azure Machine Learning bereitstellen können: 
+ `/onnx/onnx-modelzoo-aml-deploy-resnet50.ipynb` 
+ `/onnx/onnx-convert-aml-deploy-tinyyolo.ipynb`
+ `/onnx/onnx-train-pytorch-aml-deploy-mnist.ipynb`

Die folgenden Notebooks veranschaulichen, wie Sie vorhandene ONNX-Modelle mit Azure Machine Learning bereitstellen können: 
+ [onnx/onnx-inference-mnist.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-inference-mnist.ipynb) 
+ [onnx/onnx-inference-emotion-recognition.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-inference-emotion-recognition.ipynb)
 
Rufen Sie diese Notebooks ab:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Weitere Informationen

Erfahren Sie mehr über ONNX, oder leisten Sie einen Beitrag zum Projekt:
+ [ONNX-Projektwebsite](http://onnx.ai)

+ [ONNX-Code auf GitHub](https://github.com/onnx/onnx)
