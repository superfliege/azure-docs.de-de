---
title: FPGA-Paket für die Hardwarebeschleunigung für Azure Machine Learning
description: Hier finden Sie Informationen zu den Python-Paketen, die Benutzern von Azure Machine Learning zur Verfügung stehen.
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: routlaw
author: rloutlaw
ms.date: 05/07/2018
ms.openlocfilehash: e680ef34be1d5dae2942c432de5e81fe620bbdc4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34832977"
---
# <a name="azure-machine-learning-hardware-acceleration-package"></a>Hardwarebeschleunigungspaket für Azure Machine Learning

Das Hardwarebeschleunigungspaket für Azure Machine Learning ist eine über pip installierbare Python-Erweiterung für Azure Machine Learning, mit der Data Scientists und KI-Entwickler im Handumdrehen Folgendes durchführen können:

+ Ausstatten von Bildern mit einer quantisierten Version von ResNet 50

+ Trainieren von Klassifizierungen auf der Grundlage der entsprechenden Merkmale

+ Bereitstellen von Modellen in [FPGAs (Field Programmable Gate Arrays)](concept-accelerate-with-fpgas.md) in Azure, um extrem geringe Latenzstörungen zu erzielen

## <a name="prerequisites"></a>Voraussetzungen

1. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

1. Sie müssen ein Azure Machine Learning-Modellverwaltungskonto erstellen. Weitere Informationen zum Erstellen des Kontos finden Sie unter [Azure Machine Learning: Schnellstart und Workbench-Installation](../service/quickstart-installation.md). 

1. Das Paket muss installiert werden. 

 
## <a name="how-to-install-the-package"></a>So installieren Sie das Paket

1. Laden Sie die aktuelle Version von [Git](https://git-scm.com/downloads) herunter, und installieren Sie sie.

2. Installieren Sie [Anaconda (Python 3.6)](https://conda.io/miniconda.html).

3. Verwenden Sie zum Herunterladen einer vorkonfigurierten Anaconda-Umgebung den folgenden Befehl an der Git-Eingabeaufforderung:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```
5. Öffnen Sie zum Erstellen der Umgebung eine **Anaconda-Eingabeaufforderung**, und verwenden Sie den folgenden Befehl:

    ```
    conda env create -f aml-real-time-ai/environment.yml
    ```

6. Aktivieren Sie die Umgebung mithilfe des folgenden Befehls:

    ```
    conda activate amlrealtimeai
    ```

## <a name="sample-code"></a>Beispielcode

Dieser Beispielcode führt Sie Schritt für Schritt durch die Bereitstellung eines Modells in einem FPGA unter Verwendung des SDKs.

1. Importieren Sie das Paket:
   ```python
   import amlrealtimeai
   from amlrealtimeai import resnet50
   ```

1. Führen Sie eine Vorabverarbeitung des Bilds durch:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Statten Sie das Bild mit Merkmalen aus:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Erstellen Sie eine Klassifizierung:
   ```python
   model.import_graph_def(include_featurizer=False)
   print(model.classifier_input)
   print(model.classifier_output)
   ```

1. Erstellen Sie die Dienstdefinition:
   ```python
   from amlrealtimeai.pipeline import ServiceDefinition, TensorflowStage, BrainWaveStage
   save_path = os.path.expanduser('~/models/save')
   service_def_path = os.path.join(save_path, 'service_def.zip')

   service_def = ServiceDefinition()
   service_def.pipeline.append(TensorflowStage(tf.Session(), in_images, image_tensors))
   service_def.pipeline.append(BrainWaveStage(model))
   service_def.pipeline.append(TensorflowStage(tf.Session(), model.classifier_input, model.classifier_output))
   service_def.save(service_def_path)
   print(service_def_path)
   ```
 
1. Bereiten Sie das Modell für die Ausführung in einem FPGA vor:
   ```python
   from amlrealtimeai import DeploymentClient

   subscription_id = "<Your Azure Subscription ID>"
   resource_group = "<Your Azure Resource Group Name>"
   model_management_account = "<Your AzureML Model Management Account Name>"

   model_name = "resnet50-model"
   service_name = "quickstart-service"

   deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account)
   ```

1. Stellen Sie das Modell für die Ausführung in einem FPGA bereit:
   ```python
   service = deployment_client.get_service_by_name(service_name)
   model_id = deployment_client.register_model(model_name, service_def_path)

   if(service is None):
      service = deployment_client.create_service(service_name, model_id)    
   else:
      service = deployment_client.update_service(service.id, model_id)
   ```

1. Erstellen Sie den Client:
    ```python
   from amlrealtimeai import PredictionClient
   client = PredictionClient(service.ipAddress, service.port)  
   ```

1. Rufen Sie die API auf:
   ```python
   image_file = R'C:\path_to_file\image.jpg'
   results = client.score_image(image_file)
   ```

## <a name="reporting-issues"></a>Melden von Problemen

Verwenden Sie das [Forum](https://aka.ms/aml-forum), um Probleme mit dem Paket zu melden.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen eines Modells als Webdienst in einem FPGA](how-to-deploy-fpga-web-service.md)