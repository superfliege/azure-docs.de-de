---
title: Entwickeln und Bereitstellen von interoperablen ONNX-Modellen
titleSuffix: Azure Machine Learning service
description: Erfahren Sie mehr über ONNX und darüber, wie Sie mit Azure Machine Learning ONNX-Modelle erstellen und einsetzen können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 12/3/2018
ms.custom: seodec18
ms.openlocfilehash: 349f2c4eea743c3e44e492dfa76be4a70f2c37d6
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58362024"
---
# <a name="onnx-and-azure-machine-learning-create-and-deploy-interoperable-ai-models"></a>ONNX und Azure Machine Learning: Erstellen und Bereitstellen von interoperablen KI-Modellen

Das Format [Open Neural Network Exchange](https://onnx.ai) (ONNX) ist ein offener Standard für die Darstellung von Machine Learning-Modellen. ONNX wird von einer [Community von Partnern](https://onnx.ai/supported-tools) wie Microsoft unterstützt, die kompatible Frameworks und Tools erstellen. Microsoft setzt sich für eine offene und interoperable KI ein, um Datenanalysten und Entwicklern Folgendes zu ermöglichen:

+ Erstellen und Trainieren von Modellen mit dem Framework ihrer Wahl
+ Plattformübergreifende Bereitstellung von Modellen mit minimalem Integrationsaufwand

ONNX wird für alle Microsoft-Produkte, einschließlich Azure und Windows, unterstützt, damit Sie diese Ziele erreichen können.  

## <a name="why-choose-onnx"></a>Was spricht für ONNX?
Die durch ONNX erreichte Interoperabilität ermöglicht es, großartige Ideen schneller in die Produktionsumgebung zu übernehmen. Mit ONNX können Datenanalysten mit ihrem bevorzugten Framework arbeiten. Gleichermaßen können auch Entwickler davon profitieren, indem Sie weniger Zeit damit verbringen, Modelle für die Produktionsumgebung vorzubereiten oder diese in der Cloud und Edge-übergreifend bereitzustellen.  

Sie können ONNX-Modelle aus vielen Frameworks erstellen, darunter PyTorch, Chainer, Microsoft Cognitive Toolkit (CNTK), MXNet, ML.Net, TensorFlow, Keras, SciKit-Learn und weitere.

Darüber hinaus gibt es ein ganzes Ökosystem von Tools zur Visualisierung und Beschleunigung von ONNX-Modellen. Für gängige Szenarios stehen ebenfalls eine Reihe von vorab trainierten ONNX-Modellen zur Verfügung.

Mit Azure Machine Learning und ONNX Runtime können [ONNX-Modelle in der Cloud bereitgestellt werden](#deploy). Zudem ist die Bereitstellung auf Windows 10-Geräten mit [Windows ML](https://docs.microsoft.com/windows/ai/) möglich. Sie können sogar auf anderen Plattformen mithilfe von Konvertern bereitgestellt werden, die über die ONNX-Community verfügbar sind. 

[![ONNX-Flussdiagramm mit Training, Konvertern und Bereitstellung](media/concept-onnx/onnx.png) ](./media/concept-onnx/onnx.png#lightbox)

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

Mit dem Azure Machine Learning-Dienst können Sie Ihre ONNX-Modelle bereitstellen, verwalten und überwachen. Wenn Sie den [Standardbereitstellungsworkflow](concept-model-management-and-deployment.md) und ONNX Runtime verwenden, können Sie einen REST-Endpunkt erstellen, der in der Cloud gehostet wird. Sehen Sie sich am Ende dieses Artikels ein vollständiges Beispiel für ein Jupyter Notebook an, um es selbst auszuprobieren. 

### <a name="install-and-configure-onnx-runtime"></a>Installieren und Konfigurieren von ONNX Runtime

ONNX Runtime ist eine leistungsstarke OPen-Source-Rückschluss-Engine für ONNX-Modelle. Sie bietet Hardwarebeschleunigung für CPU und GPU, wobei APIs für Python, C# und C verfügbar sind. ONNX Runtime unterstützt ONNX 1.2-Modelle oder höher und kann unter Linux, Windows und Mac ausgeführt werden. Python-Pakete sind auf [PyPi.org](https://pypi.org) ([CPU](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)) verfügbar, und ein [C#-Paket](https://www.nuget.org/packages/Microsoft.ML.OnnxRuntime/) ist auf [Nuget.org](https://www.nuget.org) verfügbar. Weitere Informationen zum Projekt finden Sie auf [GitHub](https://github.com/Microsoft/onnxruntime). Lesen Sie vor der Installation die [Systemanforderungen](https://github.com/Microsoft/onnxruntime#system-requirements).

Zum Installieren von ONNX Runtime für Python verwenden Sie Folgendes:
```python
pip install onnxruntime
```

Um ONNX Runtime in Ihrem Python-Skript aufzurufen, verwenden Sie den folgenden Code:
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

Die vollständige API-Referenz zu Python finden Sie im Dokument zur [Referenzdokumentation zu ONNX Runtime](https://aka.ms/onnxruntime-python).

### <a name="example-deployment-steps"></a>Beispiel für Bereitstellungsschritte

Hier sehen Sie ein Beispiel für die Bereitstellung eines ONNX-Modells:

1. Initialisieren Sie Ihren Azure Machine Learning-Dienstarbeitsbereich. Wenn Sie noch keinen besitzen, erfahren Sie, wie Sie einen [Arbeitsbereich erstellen](setup-create-workspace.md).

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
       global session
       model = Model.get_model_path(model_name = 'MyONNXModel')
       session = onnxruntime.InferenceSession(model)

   def preprocess(input_data_json):
       # convert the JSON data into the tensor input
       return np.array(json.loads(input_data_json)['data']).astype('float32')

   def postprocess(result):
       return np.array(result).tolist()

   def run(input_data_json):
       try:
           start = time.time()   # start timer
           input_data = preprocess(input_data_json)
           input_name = session.get_inputs()[0].name  # get the id of the first input of the model   
           result = session.run([], {input_name: input_data})
           end = time.time()     # stop timer
           return {"result": postprocess(result),
                   "time": end - start}
       except Exception as e:
           result = str(e)
           return {"error": result}
   ```

   Die Datei `myenv.yml` beschreibt die für das Image benötigten Abhängigkeiten. In diesem [Tutorial](tutorial-deploy-models-with-aml.md#create-environment-file) finden Sie Anweisungen zum Erstellen einer Umgebungsdatei, ähnlich dieser Beispieldatei:

   ```python
   from azureml.core.conda_dependencies import CondaDependencies 

   myenv = CondaDependencies.create(pip_packages=["numpy","onnxruntime","azureml-core"])

   with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
   ```

4. Weitere Informationen zum Bereitstellen Ihres Modells finden Sie im Dokument [Bereitstellen von Modellen mit Azure Machine Learning Service](how-to-deploy-and-where.md).


## <a name="examples"></a>Beispiele

Beispiele für Notebooks, die ONNX-Modelle erstellen und bereitstellen, finden Sie unter [how-to-use azureml/deployment/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx).

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Weitere Informationen

Erfahren Sie mehr über ONNX, oder leisten Sie einen Beitrag zum Projekt:
+ [ONNX-Projektwebsite](https://onnx.ai)

+ [ONNX-Code auf GitHub](https://github.com/onnx/onnx)

Erfahren Sie mehr über ONNX Runtime, oder leisten Sie einen Beitrag zum Projekt:
+ [ONNX Runtime-GitHub-Repository](https://github.com/Microsoft/onnxruntime)


