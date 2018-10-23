---
title: 'Tutorial: Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning-Dienst'
description: Dieses Tutorial zeigt, wie Sie mit Azure Machine Learning Service ein Bildklassifizierungsmodell mit scikit-learn in einem Python Jupyter Notebook trainieren. Dieses Tutorial ist der erste Teil einer zweiteiligen Reihe.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: e6e49a03ee76c50cb2fff492bfd50b2820abafe4
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343757"
---
# <a name="tutorial-1-train-an-image-classification-model-with-azure-machine-learning-service"></a>Tutorial 1: Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning-Dienst

In diesem Tutorial wird ein Machine Learning-Modell sowohl lokal als auch auf Remotecomputeressourcen traininert. Hierbei wird der Trainings- und Bereitstellungsworkflow für Azure Machine Learning Service (Vorschau) in einem Python Jupyter Notebook verwendet.  Anschließend können Sie das Notebook als Vorlage verwenden, um Ihr eigenes Machine Learning-Modell mit Ihren eigenen Daten zu trainieren. Dieses Tutorial ist der **erste Teil einer zweiteiligen Reihe**.  

In diesem Tutorial wird eine einfache logistische Regression anhand des [MNIST](http://yann.lecun.com/exdb/mnist/)-Datasets und [scikit-learn](http://scikit-learn.org) mit dem Azure Machine Learning-Dienst trainiert.  MNIST ist ein populäres Dataset, das aus 70.000 Graustufenbildern besteht. Jedes Bild ist eine handgeschriebene Ziffer von 0 bis 9 im Format von 28 × 28 Pixeln. Das Ziel besteht darin, einen Multiklassen-Klassifizierer zu erstellen, um die in einem bestimmten Bild dargestellte Ziffer zu erkennen. 

In diesem Artikel werden folgende Themen erläutert:

> [!div class="checklist"]
> * Einrichten der Entwicklungsumgebung
> * Zugreifen auf und Untersuchen der Daten
> * Lokales Trainieren einer einfachen logistischen Regression mithilfe der populären scikit-learn-Bibliothek für maschinelles Lernen 
> * Trainieren mehrerer Modelle in einem Remotecluster
> * Überprüfen der Trainingsergebnisse und Registrieren des besten Modells

In [Teil 2 dieses Tutorials](tutorial-deploy-models-with-aml.md) erfahren Sie, wie Sie ein Modell auswählen und bereitstellen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="get-the-notebook"></a>Abrufen des Notebooks

Dieses Tutorial wird auch als [Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/01.train-models.ipynb) bereitgestellt. Führen Sie das Notebook `01.train-models.ipynb` in Azure Notebooks oder auf Ihrem eigenen Jupyter Notebook-Server aus.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

>[!NOTE]
> Dieses Tutorial wurde mit Version 0.168 des Azure Machine Learning SDK getestet. 

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Das gesamte Setup für Ihre Entwicklungsarbeit kann in einem Python Notebook erfolgen.  Zum Setup gehört Folgendes:

* Importieren von Python-Paketen
* Herstellen einer Verbindung mit einem Arbeitsbereich, um die Kommunikation zwischen Ihrem lokalen Computer und Remoteressourcen zu ermöglichen
* Erstellen eines Experiments zum Nachverfolgen aller Ausführungen
* Erstellen eines Remotecomputeziels für das Training

### <a name="import-packages"></a>Importieren von Paketen

Importieren Sie die Python-Pakete, die Sie in dieser Sitzung benötigen. Zeigen Sie außerdem die Azure Machine Learning SDK-Version an.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-workspace"></a>Herstellen einer Verbindung mit dem Arbeitsbereich

Erstellen Sie ein Arbeitsbereichsobjekt aus dem vorhandenen Arbeitsbereich. `Workspace.from_config()` liest die Datei **config.JSON** und lädt die Details in ein Objekt namens `ws`.

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-experiment"></a>Erstellen eines Experiments

Erstellen Sie ein Experiment, um die Ausführungen in Ihrem Arbeitsbereich nachzuverfolgen. Ein Arbeitsbereich kann über mehrere Experimente verfügen. 

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-remote-compute-target"></a>Erstellen eines Remotecomputeziels

Azure Batch AI ist ein verwalteter Dienst, mit dem Data Scientists Machine Learning-Modelle in Clustern mit virtuellen Azure-Computern (z.B. VMs mit GPU-Unterstützung) trainieren können.  In diesem Tutorial erstellen Sie einen Azure Batch AI-Cluster als Trainingsumgebung. Dieser Code erstellt einen Cluster für Sie, sofern in Ihrem Arbeitsbereich noch keiner vorhanden ist. 

 **Die Erstellung des Clusters dauert etwa fünf Minuten.** Wenn der Cluster bereits im Arbeitsbereich enthalten ist, wird er von diesem Code verwendet, und der Erstellungsvorgang wird übersprungen.


```python
from azureml.core.compute import ComputeTarget, BatchAiCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
batchai_cluster_name = "traincluster"

try:
    # look for the existing cluster by name
    compute_target = ComputeTarget(workspace=ws, name=batchai_cluster_name)
    if type(compute_target) is BatchAiCompute:
        print('found compute target {}, just use it.'.format(batchai_cluster_name))
    else:
        print('{} exists but it is not a Batch AI cluster. Please choose a different name.'.format(batchai_cluster_name))
except ComputeTargetException:
    print('creating a new compute target...')
    compute_config = BatchAiCompute.provisioning_configuration(vm_size="STANDARD_D2_V2", # small CPU-based VM
                                                                #vm_priority='lowpriority', # optional
                                                                autoscale_enabled=True,
                                                                cluster_min_nodes=0, 
                                                                cluster_max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, compute_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it uses the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
    # Use the 'status' property to get a detailed status for the current cluster. 
    print(compute_target.status.serialize())
```

Sie verfügen jetzt über die erforderlichen Pakete und Computeressourcen, die Sie zum Trainieren eines Modells in der Cloud benötigen. 

## <a name="explore-data"></a>Erkunden von Daten

Bevor Sie ein Modell trainieren, müssen Sie die Daten verstehen, die zum Trainieren verwendet werden.  Außerdem müssen Sie die Daten in die Cloud kopieren, damit sie für Ihre Cloudtrainingsumgebung zugänglich sind.  In diesem Abschnitt lernen Sie Folgendes:

* Laden des MNIST-Datasets
* Anzeigen einiger Beispielbilder
* Hochladen von Daten in die Cloud

### <a name="download-the-mnist-dataset"></a>Laden des MNIST-Datasets

Laden Sie das MNIST-Dataset, und speichern Sie die Dateien lokal in einem `data`-Verzeichnis.  Bilder und Bezeichnungen für Trainings und Tests werden heruntergeladen.  


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>Anzeigen einiger Beispielbilder

Laden Sie die komprimierten Dateien in `numpy`-Arrays. Verwenden Sie dann `matplotlib`, um 30 zufällige Bilder aus dem Dataset mit den zugehörigen Bezeichnungen darüber zu zeichnen. Beachten Sie, dass dieser Schritt eine `load_data`-Funktion erfordert, die in der `util.py`-Datei enthalten ist. Diese Datei befindet sich im Beispielordner. Achten Sie darauf, dass sie sich im gleichen Ordner wie dieses Notebook befindet. Die `load_data`-Funktion analysiert die komprimierten Dateien in NumPy-Arrays.



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

Eine zufällige Stichprobe von Bildern wird angezeigt:

![Zufällige Stichprobe von Bildern](./media/tutorial-train-models-with-aml/digits.png)

Sie haben jetzt einen Eindruck vom Aussehen dieser Bilder und vom erwarteten Vorhersageergebnis.

### <a name="upload-data-to-the-cloud"></a>Hochladen von Daten in die Cloud

Machen Sie die Daten jetzt für den Remotezugriff verfügbar, indem Sie die Daten von Ihrem lokalen Computer in Azure hochladen, sodass sie für das Remotetraining zugänglich sind. Der Datenspeicher ist ein praktisches Konstrukt, das Ihrem Arbeitsbereich zugeordnet wird, damit Sie Daten hoch- und herunterladen sowie von Ihren Remotecomputezielen aus damit interagieren können. Der Speicher wird durch das Azure Blob Storage-Konto unterstützt.

Die MNIST-Dateien werden in ein Verzeichnis namens `mnist` hochgeladen, das sich im Stammverzeichnis des Datenspeichers befindet.

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
Jetzt haben Sie alles, was Sie zum Trainieren eines Modells benötigen. 

## <a name="train-a-model-locally"></a>Lokales Trainieren eines Modells

Trainieren Sie ein einfaches logistisches Regressionsmodell lokal mithilfe von scikit-learn.

Je nach Konfiguration des Computers kann **das lokale Trainieren eine oder zwei Minuten dauern**.

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

Treffen Sie anschließend Vorhersagen anhand des Testsatzes, und berechnen Sie die Genauigkeit. 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

Als Genauigkeit des lokalen Modells wird Folgendes angezeigt:

`0.9202`

Mit nur wenigen Codezeilen haben Sie Genauigkeit von 92 % erzielt.

## <a name="train-on-a-remote-cluster"></a>Trainieren in einem Remotecluster

Jetzt können Sie dieses einfache Modell erweitern, indem Sie ein Modell mit einer anderen Regularisierungsrate erstellen. Dieses Mal trainieren Sie das Modell in einer Remoteressource.  

Für diese Aufgabe übermitteln Sie den Auftrag an den Remotetrainingscluster, den Sie zuvor eingerichtet haben.  So senden Sie einen Auftrag
* Erstellen eines Verzeichnisses
* Erstellen eines Trainingsskripts
* Erstellen eines Estimators
* Übermitteln des Auftrags 

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

Erstellen Sie ein Verzeichnis, um den erforderlichen Code von Ihrem Computer an die Remoteressource zu übermitteln.

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Erstellen eines Trainingsskripts

Um den Auftrag an den Cluster zu übermitteln, erstellen Sie zuerst ein Trainingsskript. Führen Sie folgenden Code aus, um in dem soeben erstellten Verzeichnis ein Trainingsskript namens `train.py` zu erstellen. In diesem Training wird dem Trainingsalgorithmus eine Regularisierungsrate hinzugefügt. Daher entsteht ein etwas anderes Modell als bei der lokalen Version.

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_context()

print('Train a logistic regression model with regularizaion rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

Beachten Sie, wie das Skript Daten abruft und Modelle speichert:

+ Das Trainingsskript liest ein Argument, um das Verzeichnis mit den Daten zu finden.  Wenn Sie den Auftrag später senden, verweisen Sie auf den Datenspeicher für dieses Argument: `parser.add_argument('--data-folder', type = str, dest = 'data_folder', help = 'data directory mounting point')`

    
+ Das Trainingsskript speichert das Modell in einem Verzeichnis namens „outputs“. <br/>
`joblib.dump(value = clf, filename = 'outputs/sklearn_mnist_model.pkl')`<br/>
Alles, was in dieses Verzeichnis geschrieben wurde, wird automatisch in Ihren Arbeitsbereich hochgeladen. Von diesem Verzeichnis aus greifen Sie im weiteren Verlauf dieses Tutorials auf Ihr Modell zu.

Die Datei `utils.py` wird vom Trainingsskript referenziert, damit das Dataset ordnungsgemäß geladen wird.  Kopieren Sie dieses Skript in den Skriptordner, damit es zusammen mit dem Trainingsskript in der Remoteressource zugänglich ist.


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>Erstellen eines Estimators

Ein Estimator-Objekt wird verwendet, um die Ausführung zu übermitteln.  Erstellen Sie Ihren Estimator durch Ausführen des folgenden Codes, um Folgendes zu definieren:

* Den Namen des Estimator-Objekts, `est`.
* Das Verzeichnis, das Ihre Skripts enthält. Alle Dateien in dieses Verzeichnis werden zur Ausführung in die Clusterknoten hochgeladen. 
* Das Computeziel.  In diesem Fall verwenden Sie den Batch AI-Cluster, den Sie erstellt haben.
* Den Namen des Trainingsskripts, „Train.py“.
* Die für das Trainingsskript erforderlichen Parameter. 
* Die für das Training erforderlichen Python-Pakete.

In diesem Tutorial wird der Batch AI-Cluster als Ziel verwendet. Alle Dateien im Projektverzeichnis werden zur Ausführung in die Clusterknoten hochgeladen. „data_folder“ wird auf die Verwendung des Datenspeichers (`ds.as_mount()`) festgelegt.

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>Übermitteln des Auftrags an den Cluster

Führen Sie das Experiment aus, indem Sie das Estimator-Objekt übermitteln.

```python
run = exp.submit(config=est)
run
```

Da der Aufruf asynchron erfolgt, wird direkt nach dem Start des Auftrags ein **Vorbereitung-** oder **Ausführungsstatus** zurückgegeben.

## <a name="monitor-a-remote-run"></a>Überwachen einer Remoteausführung

Insgesamt dauert die erste Ausführung **ungefähr zehn Minuten**. Solange sich die Skriptabhängigkeiten nicht ändern, wird bei nachfolgenden Ausführungen jedoch dasselbe Image wiederverwendet, sodass der Container wesentlich schneller gestartet wird.

Während der Wartezeit passiert Folgendes:

- **Image-Erstellung**: Ein Docker-Image wird erstellt, das der vom Estimator angegebenen Python-Umgebung entspricht. Das Image wird in den Arbeitsbereich hochgeladen. Das Erstellen und Hochladen des Images nimmt **etwa fünf Minuten** in Anspruch. 

  Diese Phase erfolgt für jede Python-Umgebung einmal, weil der Container für nachfolgende Ausführungen zwischengespeichert wird.  Während der Imageerstellung werden Protokolle in den Ausführungsverlauf gestreamt. Anhand dieser Protokolle können Sie den Fortschritt der Imageerstellung überwachen.

- **Skalierung**: Wenn der Remotecluster zum Ausführen mehr Knoten benötigt werden als derzeit verfügbar sind, werden weitere Knoten automatisch hinzugefügt. Die Skalierung dauert normalerweise **etwa fünf Minuten**.

- **Ausführung**: In dieser Phase werden die erforderlichen Skripts und Dateien an das Computeziel gesendet. Anschließend werden die Datenspeicher eingebunden/kopiert, und das entry_script wird ausgeführt. Während der Auftrag ausgeführt wird, werden „stdout“ und das Verzeichnis „./logs“ an den Ausführungsverlauf gestreamt. Anhand dieser Protokolle können Sie den Fortschritt der Ausführung überwachen.

- **Nachverarbeitung**: Das Verzeichnis „./outputs“ der Ausführung wird über den Ausführungsverlauf in Ihrem Arbeitsbereich kopiert, sodass Sie auf diese Ergebnisse zugreifen können.


Sie können den Verlauf eines Auftrags während seiner Ausführung auf verschiedene Weise überprüfen. In diesem Tutorial werden ein Jupyter-Widget sowie eine `wait_for_completion`-Methode verwendet. 

### <a name="jupyter-widget"></a>Jupyter-Widget

Sehen Sie sich den Verlauf der Ausführung mit einem Jupyter-Widget an.  Ebenso wie die Übermittlung der Ausführung ist das Widget asynchron und stellt alle 10 bis 15 Sekunden Liveupdates bereit, bis der Auftrag abgeschlossen ist.


```python
from azureml.train.widgets import RunDetails
RunDetails(run).show()
```

Hier sehen Sie eine Momentaufnahme des Widgets, die am Ende des Trainings angezeigt wird:

![Notebook-Widget](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>Abrufen von Protokollergebnissen nach Abschluss

Das Modelltraining und die Überwachung werden im Hintergrund ausgeführt. Warten Sie, bis das Training des Modells abgeschlossen ist, bevor Sie weiteren Code ausführen. Verwenden Sie `wait_for_completion`, um anzuzeigen, wenn das Modelltraining abgeschlossen ist. 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>Anzeigen von Ausführungsergebnissen

Sie verfügen jetzt über ein Modell, das in einem Remotecluster trainiert wurde.  Rufen Sie die Genauigkeit des Modells ab:

```python
print(run.get_metrics())
```
Die Ausgabe zeigt, dass das Remotemodell aufgrund der hinzugefügten Regularisierungsrate während des Trainings eine geringfügig höhere Genauigkeit aufweist als das lokale Modell.  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

Im Bereitstellungstutorial werden Sie dieses Modell noch ausführlicher untersuchen.

## <a name="register-model"></a>Registrieren des Modells

Im letzten Schritt des Trainingsskripts wurde die Datei `outputs/sklearn_mnist_model.pkl` in ein Verzeichnis namens `outputs` auf dem virtuellen Computer des Clusters geschrieben, in dem der Auftrag ausgeführt wird. `outputs` ist ein spezielles Verzeichnis, weil alle Inhalte dieses Verzeichnisses automatisch in Ihren Arbeitsbereich hochgeladen werden.  Diese Inhalte werden in der Ausführungsaufzeichnung im Experiment unter Ihrem Arbeitsbereich angezeigt. Daher steht die Modelldatei jetzt auch in Ihrem Arbeitsbereich zur Verfügung.

Sie können Dateien anzeigen, die dieser Ausführung zugeordnet sind.

```python
print(run.get_file_names())
```

Registrieren Sie das Modell im Arbeitsbereich, damit Sie (oder andere Mitarbeiter) dieses Modell später abfragen, untersuchen und bereitstellen können.

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Sie können auch nur den verwalteten Azure-Computecluster löschen. Da jedoch die automatische Skalierung aktiviert ist und die Clustermindestanzahl 0 beträgt, entstehen durch diese Ressource bei Nichtverwendung keine zusätzlichen Computegebühren.


```python
# optionally, delete the Azure Managed Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial für den Azure Machine Learning-Dienst haben Sie Python für folgende Zwecke verwendet:

> [!div class="checklist"]
> * Einrichten der Entwicklungsumgebung
> * Zugreifen auf und Untersuchen der Daten
> * Lokales Trainieren einer einfachen logistischen Regression mithilfe der populären scikit-learn-Bibliothek für maschinelles Lernen
> * Trainieren mehrerer Modelle in einem Remotecluster
> * Überprüfen der Trainingsdetails und Registrieren des besten Modells

Jetzt können Sie dieses registrierte Modell anhand der Anweisungen im nächsten Teil der Tutorialreihe bereitstellen:

> [!div class="nextstepaction"]
> [Tutorial 2: Bereitstellen von Modellen](tutorial-deploy-models-with-aml.md)
