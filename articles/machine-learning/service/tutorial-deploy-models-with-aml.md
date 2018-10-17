---
title: 'Tutorial: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instance (ACI) mit Azure Machine Learning Service'
description: Dieses Tutorial zeigt, wie Sie mit Azure Machine Learning Service ein Bildklassifizierungsmodell mit scikit-learn in einem Python Jupyter Notebook bereitstellen.  Dieses Tutorial ist der zweite Teil einer zweiteiligen Reihe.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: b1ae3fb5de3e28f44ef39731f49a33e2086255a1
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785340"
---
# <a name="tutorial-2--deploy-an-image-classification-model-in-azure-container-instance-aci"></a>Tutorial 2: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instance (ACI)

Dieses Tutorial ist der **zweite Teil einer zweiteiligen Reihe**. Im [vorherigen Tutorial](tutorial-train-models-with-aml.md) haben Sie Machine Learning-Modelle trainiert und anschließend ein Modell in Ihrem Arbeitsbereich in der Cloud registriert.  

Jetzt können Sie das Modell als einen Webdienst in [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/) (ACI) bereitstellen. Ein Webdienst ist ein Image (in diesem Fall ein Docker-Image), das die Bewertungslogik und das Modell selbst kapselt. 

In diesem Teil des Tutorials verwenden Sie Azure Machine Learning Service (Vorschauversion) für folgende Vorgänge:

> [!div class="checklist"]
> * Einrichten Ihrer Testumgebung
> * Abrufen des Modells aus Ihrem Arbeitsbereich
> * Lokales Testen des Modells
> * Bereitstellen des Modells für ACI
> * Testen des bereitgestellten Modells

ACI ist nicht ideal für Produktionsumgebungen, eignet sich aber optimal, um den Workflow zu testen und zu verstehen. Für skalierbare Produktionsbereitstellungen sollten Sie [Azure Kubernetes Service](how-to-deploy-to-aks.md) verwenden.

## <a name="get-the-notebook"></a>Abrufen des Notebooks

Dieses Tutorial wird auch als [Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/02.deploy-models.ipynb) bereitgestellt. Führen Sie das Notebook `02.deploy-models.ipynb` in Azure Notebooks oder auf Ihrem eigenen Jupyter Notebook-Server aus.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]


## <a name="prerequisites"></a>Voraussetzungen

Schließen Sie das Trainieren des Modells im Notebook [Tutorial 1: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md) ab.  


## <a name="set-up-the-environment"></a>Einrichten der Umgebung

Zunächst richten Sie eine Testumgebung ein.

### <a name="import-packages"></a>Importieren von Paketen

Importieren Sie die für dieses Tutorial erforderlichen Python-Pakete.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
 
import azureml
from azureml.core import Workspace, Run

# display the core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="retrieve-the-model"></a>Abrufen des Modells

Im vorherigen Tutorial haben Sie ein Modell in Ihrem Arbeitsbereich registriert. Laden Sie jetzt diesen Arbeitsbereich und laden Sie das Modell in Ihr lokales Verzeichnis herunter.


```python
from azureml.core import Workspace
from azureml.core.model import Model

ws = Workspace.from_config()
model=Model(ws, 'sklearn_mnist')
model.download(target_dir = '.')
import os 
# verify the downloaded model file
os.stat('./sklearn_mnist_model.pkl')
```

## <a name="test-model-locally"></a>Lokales Testen eines Modells

Stellen Sie vor der Bereitstellung sicher, dass Ihr Modell lokal funktioniert, indem Sie folgende Schritte ausführen:
* Testdaten laden
* Testdaten vorhersagen
* Konfusionsmatrix überprüfen

### <a name="load-test-data"></a>Laden von Testdaten

Laden Sie die Testdaten aus dem Verzeichnis **./data/**, das während des Trainingstutorials erstellt wurde.

```python
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the neural network converge faster

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

```

### <a name="predict-test-data"></a>Vorhersagen von Testdaten

Laden Sie das Test-Dataset in das Modell, um Vorhersagen zu erhalten.

```python
import pickle
from sklearn.externals import joblib

clf = joblib.load('./sklearn_mnist_model.pkl')
y_hat = clf.predict(X_test)
```

###  <a name="examine-the-confusion-matrix"></a>Überprüfen der Konfusionsmatrix

Erzeugen Sie eine Konfusionsmatrix, um zu sehen, wie viele Stichproben aus dem Testsatz korrekt klassifiziert sind. Beachten Sie den falsch klassifizierten Wert für die falsche Vorhersage. 

```python
from sklearn.metrics import confusion_matrix

conf_mx = confusion_matrix(y_test, y_hat)
print(conf_mx)
print('Overall accuracy:', np.average(y_hat == y_test))
```

Die Ausgabe zeigt die Konfusionsmatrix:

    [[ 960    0    1    2    1    5    6    3    1    1]
     [   0 1112    3    1    0    1    5    1   12    0]
     [   9    8  920   20   10    4   10   11   37    3]
     [   4    0   17  921    2   21    4   12   20    9]
     [   1    2    5    3  915    0   10    2    6   38]
     [  10    2    0   41   10  770   17    7   28    7]
     [   9    3    7    2    6   20  907    1    3    0]
     [   2    7   22    5    8    1    1  950    5   27]
     [  10   15    5   21   15   27    7   11  851   12]
     [   7    8    2   13   32   13    0   24   12  898]]
    Overall accuracy: 0.9204
   

Verwenden Sie `matplotlib`, um die Konfusionsmatrix als Diagramm anzuzeigen. In diesem Diagramm stellt die X-Achse die Ist-Werte und die Y-Achse die vorhergesagten Werte dar. Die Farbe in jedem Raster stellt die Fehlerrate dar. Je heller die Farbe, desto höher ist die Fehlerrate. Beispielsweise sind viele 5en falsch als 3en klassifiziert. Daher sehen Sie ein helles Raster bei (5,3).

```python
# normalize the diagnal cells so that they don't overpower the rest of the cells when visualized
row_sums = conf_mx.sum(axis=1, keepdims=True)
norm_conf_mx = conf_mx / row_sums
np.fill_diagonal(norm_conf_mx, 0)

fig = plt.figure(figsize=(8,5))
ax = fig.add_subplot(111)
cax = ax.matshow(norm_conf_mx, cmap=plt.cm.bone)
ticks = np.arange(0, 10, 1)
ax.set_xticks(ticks)
ax.set_yticks(ticks)
ax.set_xticklabels(ticks)
ax.set_yticklabels(ticks)
fig.colorbar(cax)
plt.ylabel('true labels', fontsize=14)
plt.xlabel('predicted values', fontsize=14)
plt.savefig('conf.png')
plt.show()
```

![Konfusionsmatrix](./media/tutorial-deploy-models-with-aml/confusion.png)

## <a name="deploy-as-web-service"></a>Bereitstellen als Webdienst

Sobald Sie das Modell getestet haben und mit den Ergebnissen zufrieden sind, stellen Sie das Modell als Webdienst bereit, der in ACI gehostet wird. 

Zum Erstellen der richtigen Umgebung für ACI ist folgendes erforderlich:
* Ein Bewertungsskript zur Veranschaulichung der Verwendung des Modells
* Eine Umgebungsdatei, um anzugeben, welche Pakete installiert werden müssen
* Eine Konfigurationsdatei zum Erstellen der ACI
* Das zuvor trainierte Modell

<a name="make-script"></a>

### <a name="create-scoring-script"></a>Erstellen des Bewertungsskripts

Erstellen Sie das Bewertungsskript score.py, das vom Webdienstaufruf verwendet wird, um zu zeigen, wie das Modell verwendet wird.

Sie müssen zwei erforderliche Funktionen in das Bewertungsskript einschließen:
* Die `init()`-Funktion, die das Modell in der Regel in ein globales Objekt lädt. Diese Funktion wird nur dann ausgeführt, wenn der Docker-Container gestartet wird. 

* Die `run(input_data)`-Funktion verwendet das Modell zur Vorhersage eines Werts, der auf den Eingabedaten basiert. Ein- und Ausgaben zur Ausführung verwenden in der Regel JSON für die Serialisierung und Deserialisierung, allerdings werden auch andere Formate unterstützt.

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

def init():
    global model
    # retreive the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

<a name="make-myenv"></a>

### <a name="create-environment-file"></a>Erstellen einer Umgebungsdatei

Erstellen Sie anschließend eine Umgebungsdatei namens myenv.yml, die alle Paketabhängigkeiten des Skripts angibt. Diese Datei wird verwendet, um sicherzustellen, dass alle diese Abhängigkeiten im Docker-Image installiert sind. Dieses Modell benötigt `scikit-learn` und `azureml-sdk`.

```python
from azureml.core.conda_dependencies import CondaDependencies 

myenv = CondaDependencies()
myenv.add_conda_package("scikit-learn")

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
```
Überprüfen Sie den Inhalt der `myenv.yml`-Datei.

```python
with open("myenv.yml","r") as f:
    print(f.read())
```

### <a name="create-configuration-file"></a>Erstellen einer Konfigurationsdatei

Erstellen Sie eine Konfigurationsdatei für die Bereitstellung und geben Sie die Anzahl der CPUs und Gigabyte-RAM an, die für Ihren ACI-Container benötigt werden. Obwohl die Anforderungen von Ihrem Modell abhängen, ist der Standard von 1 Kern und 1 Gigabyte-RAM für viele Modelle in der Regel ausreichend. Wenn Sie das Gefühl haben, dass Sie später mehr brauchen, müssen Sie das Image neu erstellen und den Dienst neu bereitstellen.

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "MNIST",  "method" : "sklearn"}, 
                                               description='Predict MNIST with sklearn')
```

### <a name="deploy-in-aci"></a>Bereitstellen in ACI
Geschätzter Zeitaufwand: **Ca. 7-8 Minuten**

Konfigurieren Sie das Image und stellen Sie es bereit. Der folgende Code durchläuft diese Schritte:

1. Erstellen Sie ein Image mit:
   * Der Bewertungsdatei (`score.py`)
   * Der Umgebungsdatei (`myenv.yml`)
   * Der Modelldatei
1. Registrieren Sie das Image im Arbeitsbereich. 
1. Senden Sie das Bild an den ACI Container.
1. Starten Sie einen Containers in ACI mithilfe des Image.
1. Rufen Sie den Webdienst-HTTP-Endpunkt ab.


```python
%%time
from azureml.core.webservice import Webservice
from azureml.core.image import ContainerImage

# configure the image
image_config = ContainerImage.image_configuration(execution_script="score.py", 
                                                  runtime="python", 
                                                  conda_file="myenv.yml")

service = Webservice.deploy_from_model(workspace=ws,
                                       name='sklearn-mnist-svc',
                                       deployment_config=aciconfig,
                                       models=[model],
                                       image_config=image_config)

service.wait_for_deployment(show_output=True)
```

Rufen Sie den Bewertungswebdienst des HTTP-Endpunkts ab, der REST-Clientaufrufe akzeptiert. Dieser Endpunkt kann von jedem genutzt werden, der den Webdienst testen oder in eine Anwendung integrieren möchte. 

```python
print(service.scoring_uri)
```


## <a name="test-deployed-service"></a>Testen des bereitgestellten Diensts

Zuvor haben Sie alle Testdaten mit der lokalen Version des Modells bewertet. Sie können jetzt das bereitgestellte Modell mit einer zufälligen Stichprobe von 30 Bildern aus den Testdaten testen.  

Der folgende Code durchläuft diese Schritte:
1. Senden Sie die Daten als JSON-Array an den in ACI gehosteten Webdienst. 

1. Verwenden Sie die `run`-API des SDK, um den Dienst aufzurufen. Sie können auch unformatierte Daten mit einem beliebigen HTTP-Tool, z.B. Curl, aufrufen.

1. Drucken Sie die zurückgegebenen Vorhersagen und stellen Sie sie zusammen mit den eingehenden Bildern grafisch dar. Rote Schrift und ein inverses Bild (weiß auf schwarz) heben falsch klassifizierte Stichproben hervor. 

 Da die Modellgenauigkeit hoch ist, müssen Sie möglicherweise den folgenden Code einige Male ausführen, bevor Sie eine falsch klassifizierte Stichprobe sehen.

```python
import json

# find 30 random samples from test set
n = 30
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
result = json.loads(service.run(input_data=test_samples))

# compare actual value vs. the predicted values:
i = 0
plt.figure(figsize = (20, 1))

for s in sample_indices:
    plt.subplot(1, n, i + 1)
    plt.axhline('')
    plt.axvline('')
    
    # use different color for misclassified sample
    font_color = 'red' if y_test[s] != result[i] else 'black'
    clr_map = plt.cm.gray if y_test[s] != result[i] else plt.cm.Greys
    
    plt.text(x=10, y =-10, s=result[i], fontsize=18, color=font_color)
    plt.imshow(X_test[s].reshape(28, 28), cmap=clr_map)
    
    i = i + 1
plt.show()
```

Hier ist das Ergebnis einer zufälligen Stichprobe an Testbildern: ![Ergebnisse](./media/tutorial-deploy-models-with-aml/results.png)

Sie können auch unformatierte HTTP-Anforderung zum Testen des Webdiensts senden.

```python
import requests
import json

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

headers = {'Content-Type':'application/json'}

# for AKS deployment you'd need to the service key in the header as well
# api_key = service.get_key()
# headers = {'Content-Type':'application/json',  'Authorization':('Bearer '+ api_key)} 

resp = requests.post(service.scoring_uri, input_data, headers=headers)

print("POST to url", service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um die Ressourcengruppe und den Arbeitsbereich für andere Tutorials und Untersuchungen zu behalten, können Sie mit diesem API-Aufruf nur die ACI-Bereitstellung löschen:

```python
service.delete()
```

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial für den Azure Machine Learning-Dienst haben Sie Python für folgende Zwecke verwendet:

> [!div class="checklist"]
> * Einrichten Ihrer Testumgebung
> * Abrufen des Modells aus Ihrem Arbeitsbereich
> * Lokales Testen des Modells
> * Bereitstellen des Modells für ACI
> * Testen des bereitgestellten Modells
 
Sie können auch das Tutorial [Automatische Algorithmusauswahl]() ausprobieren, um zu sehen, wie der Azure Machine Learning-Dienst den besten Algorithmus für Ihr Modell automatisch auswählen und optimieren und dieses Modell für Sie erstellen kann.
