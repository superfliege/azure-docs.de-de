---
title: Bereitstellen von Webdiensten in Azure Container Instances (ACI) – Azure Machine Learning
description: Erfahren Sie, wie Sie ein trainiertes Modell mit dem Azure Machine Learning-Dienst in Azure Container Instances (ACI) als Webdienst bereitstellen. In diesem Artikel werden drei verschiedene Verfahren zum Bereitstellen eines Modells in ACI vorgestellt. Sie unterscheiden sich in der Anzahl von Codezeilen und der Kontrolle, die Sie über die Benennung der Bestandteile der Bereitstellung haben.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: b004abb3959bbfe36fc200bf762114f88f3d2ead
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345045"
---
# <a name="deploy-web-services-to-azure-container-instances"></a>Bereitstellen von Webdiensten in Azure Container Instances 

Sie können ein trainiertes Modell als Webdienst in [Azure Container Instances](https://azure.microsoft.com/services/container-instances/) (ACI), [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) (AKS), auf IoT-Edgegeräten oder in [Field Programmable Gate Arrays (FPGAs)](concept-accelerate-with-fpgas.md) bereitstellen. 

ACI ist im Allgemeinen günstiger als AKS und kann mit 4-6 Codezeilen eingerichtet werden. ACI ist die ideale Option für Testbereitstellungen. Wenn Sie Ihre Modelle und Webdienste später in großem Umfang für den produktiven Einsatz nutzen möchten, können Sie sie [in AKS bereitstellen](how-to-deploy-to-aks.md).

In diesem Artikel werden drei verschiedene Verfahren zum Bereitstellen eines Modells in ACI vorgestellt. Sie unterscheiden sich in der Anzahl von Codezeilen und der Kontrolle, die Sie über die Benennung der Bestandteile der Bereitstellung haben. Folgende ACI-Optionen stehen zur Verfügung – von der Methode mit der geringsten Codemenge und Kontrolle zu der Methode mit der größten Codemenge und Kontrolle:

* Bereitstellen aus einer Modelldatei mithilfe von `Webservice.deploy()` 
* Bereitstellen aus einem registrierten Modell mithilfe von `Webservice.deploy_from_model()`
* Bereitstellen eines registrierten Modells aus einem Image mithilfe von `Webservice.deploy_from_image()`

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning Service-Arbeitsbereich und das Azure Machine Learning SDK für Python müssen installiert sein. Unter [Schnellstart: erste Schritte mit dem Azure Machine Learning-Dienst](quickstart-get-started.md) erfahren Sie, wie Sie diese Komponenten installieren.

- Das Azure Machine Learning Service-Arbeitsbereichsobjekt

    ```python
    from azureml.core import Workspace
    ws = Workspace.from_config()
    ```

- Ein bereitzustellendes Modell. Die Beispiele in diesem Artikel verwenden das Modell, das im Tutorial [Trainieren eines Modells](tutorial-train-models-with-aml.md) erstellt wird. Wenn Sie dieses Modell nicht verwenden, ändern Sie die Schritte entsprechend und setzen den Namen Ihres Modells ein.  Sie müssen auch ein eigenes Bewertungsskript schreiben, um Ihr Modell auszuführen.


## <a name="configure-an-image"></a>Konfigurieren eines Image

Konfigurieren Sie das Docker-Image, das zum Speichern aller Modelldateien verwendet wird.
1. Erstellen Sie anhand [dieser Anweisungen](tutorial-deploy-models-with-aml.md#create-scoring-script) ein Skript (score.py).

    > [!IMPORTANT]
    > Das Bewertungsskript erhält von Clients übermittelte Daten und übergibt sie zur Bewertung an das Modell. Dokumentieren Sie die Datenstruktur, die vom Skript und vom Modell erwartet wird. Diese Dokumentation vereinfacht die Erstellung eines Clients zur Nutzung des Webdiensts.

1. Erstellen Sie anhand [dieser Anweisungen](tutorial-deploy-models-with-aml.md#create-environment-file) eine Umgebungsdatei (myenv.yml).

1. Verwenden Sie diese beiden Dateien, um das Docker-Image in Python mithilfe des SDK zu erstellen:

    ```python
    from azureml.core.image import ContainerImage

    image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                      runtime = "python",
                                                      conda_file = "myenv.yml",
                                                      description = "Image with mnist model",
                                                      tags = {"data": "mnist", "type": "classification"}
                                                     )
    ```

## <a name="configure-the-aci-container"></a>Konfigurieren des ACI-Containers

Konfigurieren Sie den ACI-Container, indem Sie die Anzahl von CPUs und die Menge an Gigabyte für den Arbeitsspeicher angeben, die für Ihren ACI-Container benötigt werden. Die Standardwerte – ein Kern und 1 Gigabyte Arbeitsspeicher – sind für viele Modelle ausreichend. Wenn Sie später höhere Werte benötigen, erstellen Sie das Image neu, und stellen Sie den Dienst neu bereit.  

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores = 1, 
                                               memory_gb = 1, 
                                               tags = {"data": "mnist", "type": "classification"},
                                               description = 'Handwriting recognition')
```

## <a name="register-a-model"></a>Registrieren eines Modells

> Überspringen Sie diese Voraussetzung, wenn Sie die [ Bereitstellung mithilfe einer Modelldatei](#deploy-from-model-file) (`Webservice.deploy()`) durchführen.

Registrieren Sie ein Modell für [Webservice.deploy_from_model](#deploy-from-registered-model) oder [Webservice.deploy_from_image](#deploy-from-image). Wenn Sie bereits über ein registriertes Modell verfügen, rufen Sie es jetzt ab.

### <a name="retrieve-a-registered-model"></a>Abrufen eines registrierten Modells
Wenn Sie Azure Machine Learning zum Trainieren Ihres Modells verwenden, ist das Modell möglicherweise schon in Ihrem Arbeitsbereich registriert.  Im letzten Schritt des Tutorials [Trainieren eines Modells](tutorial-train-models-with-aml.md) beispielsweise wurde das Modell registriert.  Rufen Sie das registrierte Modell ab, das für die Bereitstellung verwendet werden soll.

```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model=Model(ws, model_name)
```
  
### <a name="register-a-model-file"></a>Registrieren einer Modelldatei

Auch wenn Ihr Modell auf andere Weise erstellt wurde, können Sie es trotzdem in Ihrem Arbeitsbereich registrieren.  Um ein Modell zu registrieren, muss sich die Datei (`sklearn_mnist_model.pkl` in diesem Beispiel) im aktuellen Arbeitsverzeichnis befinden. Registrieren Sie diese Datei mit `Model.register()` als Modell mit dem Namen `sklearn_mnist` im Arbeitsbereich.
    
```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model = Model.register(model_path = "sklearn_mnist_model.pkl",
                        model_name = model_name,
                        tags = {"data": "mnist", "type": "classification"},
                        description = "Mnist handwriting recognition",
                        workspace = ws)
```

<a name='deploy-from-model-file'/>
## <a name="option-1-deploy-from-model-file"></a>Option 1: Bereitstellen aus einer Modelldatei

Die Option der Bereitstellung aus einer Modelldatei erfordert die geringste Codemenge, bietet aber auch die geringste Kontrolle über die Benennung der Komponenten. Diese Option beginnt mit einer Modelldatei und registriert diese für Sie im Arbeitsbereich.  Sie können das Modell jedoch nicht benennen und ihm auch weder Tags noch eine Beschreibung hinzufügen.  

Diese Option verwendet die SDK-Methode „Webservice.deploy()“.  

**Geschätzter Zeitaufwand**: Die Bereitstellung dauert ca. 6-7 Minuten.

1. Stellen Sie sicher, dass sich die Modelldatei in Ihrem lokalen Arbeitsverzeichnis befindet.

1. Öffnen Sie die erforderliche Modelldatei „score.py“, und ändern Sie den Abschnitt `init()` folgendermaßen:

    ```python
    def init():
        global model
        # retreive the local path to the model using the model name
        model_path = Model.get_model_path('sklearn_mnist_model.pkl')
        model = joblib.load(model_path)
    ```

1. Stellen Sie Ihre Modelldatei bereit.

    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-1'
    service = Webservice.deploy(deployment_config = aciconfig,
                                    image_config = image_config,
                                    model_paths = ['sklearn_mnist_model.pkl'],
                                    name = service_name,
                                    workspace = ws)
    
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. Jetzt können Sie den [Webdienst testen](#test-web-service).

<a name='deploy-from-registered-model'/>
## <a name="option-2-deploy-from-registered-model"></a>Option 2: Bereitstellen aus einem registrierten Modell

Die Option der Bereitstellung einer registrierten Modelldatei benötigt einige Codezeilen mehr und gestattet ein gewisses Maß an Kontrolle über die Benennung der Ausgaben. Diese Option ist eine praktische Möglichkeit, ein bereits vorhandenes registriertes Modell bereitzustellen.  Sie können das Docker-Image jedoch nicht benennen.  

Diese Option verwendet die SDK-Methode „Webservice.deploy_from_model()“.

**Geschätzter Zeitaufwand**: Die Bereitstellung mit dieser Option dauert ca. 8 Minuten.

1. Führen Sie den Code aus, um den Docker-Container und den ACI-Container zu konfigurieren, und geben Sie das registrierte Modell an.

    ```python
    from azureml.core.webservice import Webservice

    service_name = 'aci-mnist-2'
    service = Webservice.deploy_from_model(deployment_config = aciconfig,
                                           image_config = image_config,
                                           models = [model], # this is the registered model object
                                           name = service_name,
                                           workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. Jetzt können Sie den [Webdienst testen](#test-web-service).

<a name='deploy-from-image'/>
## <a name="option-3-deploy-from-image"></a>Option 3: Bereitstellen aus einem Image

Stellen Sie mithilfe von `Webservice.deploy_from_image()` ein registriertes Modell (`model`) bereit. Mit dieser Methode können Sie das Docker-Image separat erstellen und die Bereitstellung dann aus diesem Image durchführen.

1. Erstellen und registrieren Sie das Docker-Image im Arbeitsbereich mithilfe von `ContainerImage.create()`.

    Diese Methode bietet eine bessere Kontrolle über das Image, da es in einem separaten Schritt erstellt wird.  Das registrierte Modell (`model`) ist im Image enthalten.
    
    ```python
    from azureml.core.image import ContainerImage
    
    image = ContainerImage.create(name = "myimage1",
                                  models = [model], # this is the registered model object
                                  image_config = image_config,
                                  workspace = ws)
    
    image.wait_for_creation(show_output = True)
    ```
**Geschätzter Zeitaufwand**: Ca. 3 Minuten.

1. Stellen Sie das Docker-Image mithilfe von `Webservice.deploy_from_image()` als Dienst bereit.

    Stellen Sie das Image jetzt in ACI bereit.  
    
    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-3'
    service = Webservice.deploy_from_image(deployment_config = aciconfig,
                                                image = image,
                                                name = service_name,
                                                workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```   
 
**Geschätzter Zeitaufwand**: Ca. 3 Minuten.

Mit dieser Option haben Sie die größtmögliche Kontrolle über die Erstellung und Benennung der Komponenten in der Bereitstellung.

Jetzt können Sie den Webdienst testen.

## <a name="a-nametest-web-servicetest-the-web-service"></a><a name='test-web-service'/>Testen des Webdiensts

Der Webdienst ist unabhängig von der verwendeten Methode immer der gleiche.  Um Vorhersagen zu erhalten, verwenden Sie die `run`-Methode des Diensts.  

```python
# Load Data
import os
import urllib

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/test-images.gz')

from utils import load_data
X_test = load_data('./data/test-images.gz', False) / 255.0

from sklearn import datasets
import numpy as np
import json

# find 5 random samples from test set
n = 5
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
prediction = service.run(input_data = test_samples)
print(prediction)
```


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diesen Webdienst nicht verwenden, löschen Sie ihn, damit keine Gebühren anfallen.

```python
service.delete()
```

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über die [Nutzung eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md).
* Erfahren Sie, wie Sie Modelle für umfangreichere Szenarien [im Azure Kubernetes Service bereitstellen](how-to-deploy-to-aks.md). 
