---
title: Erstellen und Bereitstellen eines Bildklassifizierungsmodells mithilfe des Azure Machine Learning-Pakets für maschinelles Sehen.
description: Erfahren Sie, wie ein Bildklassifizierungsmodell für maschinelles Sehen mithilfe des Azure Machine Learning-Pakets für maschinelles Sehen erstellt, trainiert, getestet und bereitgestellt wird.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ROBOTS: NOINDEX
ms.openlocfilehash: f5917cd7a5e4fcc2733765f642ad0958092372c1
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51616213"
---
# <a name="build-and-deploy-image-classification-models-with-azure-machine-learning"></a>Erstellen und Bereitstellen von Bildklassifizierungsmodellen mit Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

In diesem Artikel erfahren Sie, wie Sie das Azure Machine Learning-Paket für maschinelles Sehen (Azure Machine Learning Package for Computer Vision, AMLPCV) zum Trainieren, Testen und Bereitstellen eines Bildklassifizierungsmodells verwenden. Eine Übersicht über dieses Paket und die ausführliche Referenzdokumentation [finden Sie hier](https://aka.ms/aml-packages/vision).

Eine große Anzahl von Problemen im Bereich maschinelles Sehen kann mit Bildklassifizierung gelöst werden. Diese Probleme beinhalten das Erstellen von Modellen, die z.B. die folgenden Fragen beantworten:
+ _Ist ein Objekt im Bild vorhanden? Beispielsweise ein „Hund“, „Auto“ oder „Schiff“ usw._
+ _Welche Augenkrankheit-Schweregradklasse zeigt der Netzhautscan des Patienten?_

Beim Erstellen und Bereitstellen dieses Modell mit AMLPCV durchlaufen Sie die folgenden Schritte:
1. Dataseterstellung
2. Bildvisualisierung und -anmerkung
3. Bilderweiterung
4. DNN-Modelldefinition (Deep Neural Network)
5. Klassifizierertraining
6. Auswertung und Visualisierung
7. Webdienstbereitstellung
8. Webdienst-Auslastungstests

Das [CNTK](https://www.microsoft.com/cognitive-toolkit/) wird als Deep Learning-Framework verwendet, das Training erfolgt lokal auf einem GPU-gestützten Computer wie der [Deep Learning Data Science-VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview), und die Bereitstellung verwendet die Azure ML Operationalization CLI.

## <a name="prerequisites"></a>Voraussetzungen

1. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

1. Die folgenden Konten und Anwendungen müssen eingerichtet und installiert werden:
   - Ein Azure Machine Learning-Experimentieren-Konto 
   - Ein Azure Machine Learning-Modellverwaltungskonto
   - Installation von Azure Machine Learning Workbench

   Wenn diese drei Aspekte noch nicht erstellt oder installiert wurden, lesen und befolgen Sie den Artikel [Azure Machine Learning: Schnellstart und Workbench-Installation](../desktop-workbench/quickstart-installation.md). 

1. Das Azure Machine Learning-Paket für maschinelles Sehen muss installiert sein. Erfahren Sie [hier, wie dieses Paket installiert wird](https://aka.ms/aml-packages/vision).

## <a name="sample-data-and-notebook"></a>Beispieldaten und Notebook

### <a name="get-the-jupyter-notebook"></a>Abrufen des Jupyter Notebooks

Laden Sie das Notebook herunter, um das hier beschriebene Beispiel selbst auszuführen.

> [!div class="nextstepaction"]
> [Jupyter Notebook abrufen](https://aka.ms/aml-packages/vision/notebooks/image_classification)

### <a name="load-the-sample-data"></a>Laden der Beispieldaten

Im folgenden Beispiel wird ein Dataset verwendet, das aus 63 Bildern mit Tafelausstattung besteht. Jedes Bild ist als zugehörig zu einer von vier verschiedenen Klassen gekennzeichnet: bowl (Schüssel), cup (Tasse), cutlery (Besteck) oder plate (Platte). Die Anzahl der Bilder in diesem Beispiel ist gering, sodass dieses Beispiel schnell ausgeführt werden kann. In der Praxis sollten mindestens 100 Bilder pro Klasse bereitgestellt werden. Alle Bilder befinden sich unter *„../sample_data/imgs_recycling/“* in Unterverzeichnissen namens „bowl“, „cup“, „cutlery“ und „plate“.

![Azure Machine Learning-Dataset](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)


```python
import warnings
warnings.filterwarnings("ignore")
import json, numpy as np, os, timeit 
from azureml.logging import get_azureml_logger
from imgaug import augmenters
from IPython.display import display
from sklearn import svm
from cvtk import ClassificationDataset, CNTKTLModel, Context, Splitter, StorageContext
from cvtk.augmentation import augment_dataset
from cvtk.core.classifier import ScikitClassifier
from cvtk.evaluation import ClassificationEvaluation, graph_roc_curve, graph_pr_curve, graph_confusion_matrix
import matplotlib.pyplot as plt

from classification.notebook.ui_utils.ui_annotation import AnnotationUI
from classification.notebook.ui_utils.ui_results_viewer import ResultsUI
from classification.notebook.ui_utils.ui_precision_recall import PrecisionRecallUI

%matplotlib inline

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)
```



## <a name="create-a-dataset"></a>Erstellen eines Datasets

Sobald Sie die Abhängigkeiten importiert und den Speicherkontext festgelegt haben, können Sie das Datasetobjekt erstellen.

Um dieses Objekt mit dem Azure Machine Learning-Paket für maschinelles Sehen zu erstellen, geben Sie das Stammverzeichnis der Bilder auf dem lokalen Datenträger an. Dieses Verzeichnis muss die gleiche allgemeine Struktur wie das Tableware-Dataset aufweisen, also Unterverzeichnisse mit den eigentlichen Bildern enthalten:
- root
    - Bezeichnung 1
    - Bezeichnung 2
    - ...
    - Bezeichnung n
  
Das Trainieren eines Bildklassifizierungsmodells für ein anderes Dataset ist ganz einfach: Ändern Sie nur den Stammpfad „`dataset_location`“ im folgenden Code, damit er auf andere Bilder verweist.


```python
# Root image directory
dataset_location = os.path.abspath("classification/sample_data/imgs_recycling")

dataset_name = 'recycling'
dataset = ClassificationDataset.create_from_dir(dataset_name, dataset_location)
print("Dataset consists of {} images with {} labels.".format(len(dataset.images), len(dataset.labels)))
print("Select information for image 2: name={}, label={}, unique id={}.".format(
    dataset.images[2].name, dataset.images[2]._labels[0].name, dataset.images[2]._storage_id))
```

    F1 2018-04-23 17:12:57,593 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-04-23 17:12:57,599 INFO azureml.vision:dataset creating dataset for scenario=classification 
    Dataset consists of 63 images with 4 labels.
    Select information for image 2: name=msft-plastic-bowl20170725152154282.jpg, label=bowl, unique id=3.

Das Datasetobjekt stellt Funktionen zum Herunterladen von Bildern mithilfe der [Bing-Bildersuche-API](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) bereit. 

Zwei Typen von Suchabfragen werden unterstützt: 
+ Reguläre Textabfragen
+ Bild-URL-Abfragen

Diese Abfragen sowie die Klassenbezeichnung müssen in einer JSON-codierten Textdatei angegeben werden. Beispiel: 

```json
{
    "bowl": [
                    "plastic bowl",
                    "../imgs_recycling/bowl"
    ],
    "cup": [
                    "plastic cup",
                    "../imgs_recycling/cup",
                    "http://cdnimg2.webstaurantstore.com/images/products/main/123662/268841/dart-solo-ultra-clear-conex-tp12-12-oz-pet-plastic-cold-cup-1000-case.jpg"
    ],
    "cutlery": [
                    "plastic cutlery",
                    "../imgs_recycling/cutlery",
                    "http://img4.foodservicewarehouse.com/Prd/1900SQ/Fineline_2514-BO.jpg"
    ],
    "plate": [
                    "plastic plate",
                    "../imgs_recycling/plate"
    ]
}
```

Darüber hinaus müssen Sie explizit ein Kontextobjekt erstellen, das den Schlüssel der Bing-Bildersuche-API enthält. Dies erfordert ein Abonnement für die Bing-Bildersuche-API.

## <a name="visualize-and-annotate-images"></a>Visuelles Darstellen und Versehen von Bildern mit Anmerkungen

Sie können die Bilder und die richtigen Bezeichnungen im Datasetobjekt mithilfe des folgenden Widgets visuell darstellen. 

Wenn der Fehler „Widget-Javascript nicht erkannt“ auftritt, führen Sie diesen Befehl aus, um das Problem zu beheben:
<br>`jupyter nbextension enable --py --sys-prefix widgetsnbextension`


```python
annotation_ui = AnnotationUI(dataset, Context.get_global_context())
display(annotation_ui.ui)
```

![Azure Machine Learning-Dataset](media/how-to-build-deploy-image-classification-models/image_annotation.png)

## <a name="augment-images"></a>Erweitern von Bildern

Das [`augmentation`-Modul](https://docs.microsoft.com/python/api/cvtk.augmentation) stellt Funktionen bereit, um ein Datasetobjekt mithilfe aller Transformationen zu erweitern, die in der [imgaug](https://github.com/aleju/imgaug)-Bibliothek beschriebenen werden. Bildtransformationen können in einer einzelnen Pipeline gruppiert werden. In diesem Fall werden alle Transformationen in der Pipeline gleichzeitig auf alle Bilder angewendet. 

Wenn Sie verschiedene Erweiterungsschritte separat oder auf eine andere Weise anwenden möchten, können Sie mehrere Pipelines definieren und an die *augment_dataset*-Funktion übergeben. Weitere Informationen und Beispiele für die Bilderweiterung finden Sie in der [imgaug-Dokumentation](https://github.com/aleju/imgaug).

Das Hinzufügen von erweiterten Bildern zum Trainingssatz ist insbesondere bei kleinen Datasets vorteilhaft. Da der DNN-Trainingsvorgang aufgrund der erhöhten Anzahl von Trainingsbildern langsamer ist, wird empfohlen, dass Sie Ihre Experimente ohne Erweiterung starten.


```python
# Split the dataset into train and test  
train_set_orig, test_set = dataset.split(train_size = 0.66, stratify = "label")
print("Number of training images = {}, test images = {}.".format(train_set_orig.size(), test_set.size()))
```

    F1 2018-04-23 17:13:01,780 INFO azureml.vision:splitter splitting a dataset 
    F1 2018-04-23 17:13:01,805 INFO azureml.vision:dataset creating dataset for scenario=classification 
    F1 2018-04-23 17:13:01,809 INFO azureml.vision:dataset creating dataset for scenario=classification 
    Number of training images = 41, test images = 22.
    


```python
augment_train_set = False

if augment_train_set:
    aug_sequence = augmenters.Sequential([
            augmenters.Fliplr(0.5),             # horizontally flip 50% of all images
            augmenters.Crop(percent=(0, 0.1)),  # crop images by 0-10% of their height/width
        ])
    train_set = augment_dataset(train_set_orig, [aug_sequence])
    print("Number of original training images = {}, with augmented images included = {}.".format(train_set_orig.size(), train_set.size()))
else:
    train_set = train_set_orig  
```

## <a name="define-dnn-models"></a>Definieren von DNN-Modellen

Mit diesem Paket werden die folgenden vortrainierten Deep Neural Network-Modelle unterstützt: 
+ Resnet-18
+ Resnet-34
+ Resnet-50
+ Resnet-101
+ Resnet-152

Diese DNNs können entweder als Klassifizierer oder als Featurizer verwendet werden. 

Weitere Informationen zu den Netzwerken finden Sie [hier](https://github.com/Microsoft/CNTK/blob/master/PretrainedModels/Image.md). Eine grundlegende Einführung in Übertragungslernen finden Sie [hier](https://blog.slavv.com/a-gentle-intro-to-transfer-learning-2c0b674375a0).

Die Standardparameter für die Bildklassifizierung für dieses Paket sind eine Auflösung von 224 x 224 Pixeln und ein Resnet-18-DNN. Diese Parameter wurden ausgewählt, weil sie gut für eine Vielzahl von Aufgaben funktionieren. Die Genauigkeit kann häufig z.B. durch Erhöhen der Bildauflösung auf 500 x 500 Pixel und/oder die Auswahl eines tieferen Modells (Resnet-50) verbessert werden. Das Ändern der Parameter kann jedoch zu einem beträchtlichen Anstieg der Trainingszeit führen. Weitere Informationen finden Sie im Artikel zur [Verbesserung der Genauigkeit](https://docs.microsoft.com/azure/machine-learning/service/how-to-improve-accuracy-for-computer-vision-models).


```python
# Default parameters (224 x 224 pixels resolution, Resnet-18)
lr_per_mb = [0.05]*7 + [0.005]*7 +  [0.0005]
mb_size = 32
input_resoluton = 224
base_model_name = "ResNet18_ImageNet_CNTK"

# Suggested parameters for 500 x 500 pixels resolution, Resnet-50
# (see in the Appendix "How to improve accuracy", last row in table)
# lr_per_mb   = [0.01] * 7 + [0.001] * 7 + [0.0001]
# mb_size    = 8
# input_resoluton = 500
# base_model_name = "ResNet50_ImageNet_CNTK"

# Initialize model
dnn_model = CNTKTLModel(train_set.labels,
                       base_model_name=base_model_name,
                       image_dims = (3, input_resoluton, input_resoluton))
```

    Successfully downloaded ResNet18_ImageNet_CNTK
    

## <a name="train-the-classifier"></a>Trainieren des Klassifizierers

Sie können eine der folgenden Methoden für das vortrainierte DNN auswählen.

  - **DNN Refinement (DNN-Optimierung)**: Diese Methode trainiert das DNN so, dass die Klassifizierung direkt ausgeführt wird. Während das DNN-Training langsam ist, führt es in der Regel zu den besten Ergebnisse, weil alle Netzwerkgewichtungen während des Trainings verbessert werden können, um die größte Genauigkeit zu erzielen.

  - **DNN Featurization (DNN-Featurebereitstellung)**: Bei dieser Methode wird das DNN unverändert ausgeführt, um eine niedrigdimensionale Darstellung eines Bildes zu erhalten (512, 2048 oder 4096 Floats). Diese Darstellung wird dann als Eingabe verwendet, um einen separaten Klassifizierer zu trainieren. Da das DNN unverändert beibehalten wird, ist dieser Ansatz im Vergleich zu DNN-Optimierung viel schneller, aber die Genauigkeit ist schlechter. Davon abgesehen kann das Trainieren eines externen Klassifizierers (z.B. einer linearen SVM wie im folgenden Code gezeigt) eine sichere Baseline ermöglichen und Sie beim Verstehen der Durchführbarkeit eines Problems unterstützen.
  
TensorBoard kann verwendet werden, um den Trainingsfortschritt zu visualisieren. So aktivieren Sie TensorBoard:
1. Fügen Sie den Parameter `tensorboard_logdir=PATH` wie im folgenden Code gezeigt hinzu.
1. Starten Sie den TensorBoard-Client mithilfe des Befehls `tensorboard --logdir=PATH` in einer neuen Konsole.
1. Öffnen Sie gemäß den Anweisungen durch TensorBoard einen Webbrowser. Standardmäßig ist dies localhost:6006. 


```python
# Train either the DNN or a SVM as classifier 
classifier_name = "dnn"

if classifier_name.lower() == "dnn":  
    dnn_model.train(train_set, lr_per_mb = lr_per_mb, mb_size = mb_size, eval_dataset=test_set) #, tensorboard_logdir=r"tensorboard"
    classifier = dnn_model
elif classifier_name.lower() == "svm":
    learner = svm.LinearSVC(C=1.0, class_weight='balanced', verbose=0)
    classifier = ScikitClassifier(dnn_model, learner = learner)
    classifier.train(train_set)
else:
    raise Exception("Classifier unknown: " + classifier)   
```

    F1 2018-04-23 17:13:28,238 INFO azureml.vision:Fit starting in experiment  1541466320 
    F1 2018-04-23 17:13:28,239 INFO azureml.vision:model starting training for scenario=classification 
    <class 'int'>
    1 worker
    Training transfer learning model for 15 epochs (epoch_size = 41).
    non-distributed mode
    Training 15741700 parameters in 53 parameter tensors.
    Training 15741700 parameters in 53 parameter tensors.
    Learning rate per minibatch: 0.05
    Momentum per minibatch: 0.9
    PROGRESS: 0.00%
    Finished Epoch[1 of 15]: [Training] loss = 2.820586 * 41, metric = 68.29% * 41 5.738s (  7.1 samples/s);
    Evaluation Set Error :: 29.27%
    Finished Epoch[2 of 15]: [Training] loss = 0.286728 * 41, metric = 9.76% * 41 0.752s ( 54.5 samples/s);
    Evaluation Set Error :: 34.15%
    Finished Epoch[3 of 15]: [Training] loss = 0.206938 * 41, metric = 4.88% * 41 0.688s ( 59.6 samples/s);
    Evaluation Set Error :: 41.46%
    Finished Epoch[4 of 15]: [Training] loss = 0.098931 * 41, metric = 2.44% * 41 0.785s ( 52.2 samples/s);
    Evaluation Set Error :: 48.78%
    Finished Epoch[5 of 15]: [Training] loss = 0.046547 * 41, metric = 0.00% * 41 0.724s ( 56.6 samples/s);
    Evaluation Set Error :: 43.90%
    Finished Epoch[6 of 15]: [Training] loss = 0.059709 * 41, metric = 4.88% * 41 0.636s ( 64.5 samples/s);
    Evaluation Set Error :: 34.15%
    Finished Epoch[7 of 15]: [Training] loss = 0.005817 * 41, metric = 0.00% * 41 0.710s ( 57.7 samples/s);
    Evaluation Set Error :: 14.63%
    Learning rate per minibatch: 0.005
    Finished Epoch[8 of 15]: [Training] loss = 0.014917 * 41, metric = 0.00% * 41 0.649s ( 63.2 samples/s);
    Evaluation Set Error :: 9.76%
    Finished Epoch[9 of 15]: [Training] loss = 0.040539 * 41, metric = 2.44% * 41 0.777s ( 52.8 samples/s);
    Evaluation Set Error :: 9.76%
    Finished Epoch[10 of 15]: [Training] loss = 0.024606 * 41, metric = 0.00% * 41 0.626s ( 65.5 samples/s);
    Evaluation Set Error :: 7.32%
    PROGRESS: 0.00%
    Finished Epoch[11 of 15]: [Training] loss = 0.004225 * 41, metric = 0.00% * 41 0.656s ( 62.5 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[12 of 15]: [Training] loss = 0.004364 * 41, metric = 0.00% * 41 0.702s ( 58.4 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[13 of 15]: [Training] loss = 0.007974 * 41, metric = 0.00% * 41 0.721s ( 56.9 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[14 of 15]: [Training] loss = 0.000655 * 41, metric = 0.00% * 41 0.711s ( 57.7 samples/s);
    Evaluation Set Error :: 4.88%
    Learning rate per minibatch: 0.0005
    Finished Epoch[15 of 15]: [Training] loss = 0.024865 * 41, metric = 0.00% * 41 0.688s ( 59.6 samples/s);
    Evaluation Set Error :: 4.88%
    Stored trained model at ../../../cvtk_output\model_trained\ImageClassification.model
    F1 2018-04-23 17:13:45,097 INFO azureml.vision:Fit finished in experiment  1541466320 
    


```python
# Plot how the training and test accuracy increases during gradient descent. 
if classifier_name == "dnn":
    [train_accs, test_accs, epoch_numbers] = classifier.train_eval_accs
    plt.xlabel("Number of training epochs") 
    plt.ylabel("Classification accuracy") 
    train_plot = plt.plot(epoch_numbers, train_accs, 'r-', label = "Training accuracy")
    test_plot = plt.plot(epoch_numbers, test_accs, 'b-.', label = "Test accuracy")
    plt.legend()
```

![png](media/how-to-build-deploy-image-classification-models/output_17_0.png)


## <a name="evaluate-and-visualize-model-performance"></a>Bewerten und Visualisieren der Modellleistung

Sie können die Leistung des trainierten Modells für ein unabhängiges Testdataset mithilfe des Auswertungsmoduls auswerten. Die folgenden Auswertungsmetriken werden beispielsweise berechnet:
 
+ Genauigkeit (standardmäßig für die Klasse gemittelt)
+ PR-Kurve
+ ROC-Kurve
+ Fläche unter der Kurve
+ Konfusionsmatrix


```python
# Run the classifier on all test set images
ce = ClassificationEvaluation(classifier, test_set, minibatch_size = mb_size)

# Compute Accuracy and the confusion matrix
acc = ce.compute_accuracy()
print("Accuracy = {:2.2f}%".format(100*acc))
cm  = ce.compute_confusion_matrix()
print("Confusion matrix = \n{}".format(cm))

# Show PR curve, ROC curve, and confusion matrix
fig, ((ax1, ax2, ax3)) = plt.subplots(1,3)
fig.set_size_inches(18, 4)
graph_roc_curve(ce, ax=ax1)
graph_pr_curve(ce, ax=ax2)
graph_confusion_matrix(ce, ax=ax3)
plt.show()
```

    F1 2018-04-23 17:14:37,449 INFO azureml.vision:evaluation doing evaluation for scenario=classification 
    F1 2018-04-23 17:14:37,450 INFO azureml.vision:model scoring dataset for scenario=classification 
    Accuracy = 95.45%
    Confusion matrix = 
    [[ 0  1  0  1]
     [ 0  7  0  0]
     [ 0  0  2  0]
     [ 0  0  0 11]]
    


![png](media/how-to-build-deploy-image-classification-models/output_20_1.png)



```python
# Results viewer UI
labels = [l.name for l in dataset.labels] 
pred_scores = ce.scores #classification scores for all images and all classes
pred_labels = [labels[i] for i in np.argmax(pred_scores, axis=1)]

results_ui = ResultsUI(test_set, Context.get_global_context(), pred_scores, pred_labels)
display(results_ui.ui)
```

![Azure Machine Learning-Dataset](media/how-to-build-deploy-image-classification-models/Image_Classification_Results.png)


```python
# Precision / recall curve UI
precisions, recalls, thresholds = ce.compute_precision_recall_curve() 
thresholds = list(thresholds)
thresholds.append(thresholds[-1])
pr_ui = PrecisionRecallUI(100*precisions[::-1], 100*recalls[::-1], thresholds[::-1])
display(pr_ui.ui) 
```

![Azure Machine Learning-Dataset](media/how-to-build-deploy-image-classification-models/image_precision_curve.png)

## <a name="operationalization-deploy-and-consume"></a>Operationalisierung: Bereitstellen und Nutzen

Als „Operationalisierung“ wird das Veröffentlichen von Modellen und Code als Webdienste und die Nutzung dieser Dienste zum Generieren von Geschäftsergebnissen bezeichnet. 

Sobald Ihr Modell trainiert ist, können Sie dieses Modell als Webdienst für die Nutzung mithilfe von [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning) bereitstellen. Ihre Modelle können auf dem lokalen Computer oder in einem ACS-Cluster (Azure Container Service) bereitgestellt werden. Mithilfe von ACS können Sie Ihren Webdienst manuell skalieren, oder Sie verwenden die Funktionalität für automatische Skalierung.

**Anmelden bei Azure CLI**

Melden Sie sich mit einem [Azure](https://azure.microsoft.com/)-Konto mit einem gültigen Abonnement mit dem folgenden CLI-Befehl an:
<br>`az login`

+ Um zu einem anderen Azure-Abonnement zu wechseln, verwenden Sie den folgenden Befehl:
<br>`az account set --subscription [your subscription name]`

+ Um das aktuelle Modellverwaltungskonto anzuzeigen, verwenden Sie den folgenden Befehl:
  <br>`az ml account modelmanagement show`

**Erstellen und Einrichten Ihrer Clusterbereitstellungsumgebung**

Sie müssen Ihre Bereitstellungsumgebung nur ein Mal einrichten. Wenn Sie noch nicht über eine Bereitstellungsumgebung verfügen, richten Sie sie jetzt unter Verwendung [dieser Anweisungen](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup) ein. 

Um Ihre aktive Bereitstellungsumgebung anzuzeigen, verwenden Sie den folgenden CLI-Befehl:
<br>`az ml env show`
   
Azure CLI-Beispielbefehl zum Erstellen und Festlegen der Bereitstellungsumgebung

```CLI
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. westcentralus] [-g [resource group]]
az ml env set -n [environment name] -g [resource group]
az ml env cluster
```
    
### <a name="manage-web-services-and-deployments"></a>Verwalten von Webdiensten und Bereitstellungen

Die folgenden APIs können zum Bereitstellen von Modellen als Webdienste, Verwalten dieser Webdienste und Verwalten von Bereitstellungen verwendet werden.

|Aufgabe|API|
|----|----|
|Erstellen des Bereitstellungsobjekts|`deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model, aml_env="cluster")`
|Bereitstellen des Webdiensts|`deploy_obj.deploy()`|
|Bewerten des Bilds|`deploy_obj.score_image(local_image_path_or_image_url)`|
|Löschen des Webdiensts|`deploy_obj.delete()`|
|Erstellen eines Docker-Images ohne Webdienst|`deploy_obj.build_docker_image()`|
|Auflisten der vorhandenen Bereitstellung|`AMLDeployment.list_deployment()`|
|Löschen (wenn der Dienst vorhanden ist) mit dem Bereitstellungsnamen|`AMLDeployment.delete_if_service_exist(deployment_name)`|

[API-Dokumentation:](https://aka.ms/aml-packages/vision) Ausführliche Informationen zu jedem Modul und jeder Klasse finden Sie in der **Paketreferenzdokumentation**.

**CLI-Referenz:** Informationen zu erweiterten Vorgängen im Zusammenhang mit der Bereitstellung finden Sie in der [Modellverwaltungs-CLI-Referenz](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-cli-reference).

**Bereitstellungsverwaltung im Azure-Portal**: Sie können Ihre Bereitstellungen im [Azure-Portal](https://ms.portal.azure.com/) nachverfolgen und verwalten. Ermitteln Sie Ihre Kontoseite für die Machine Learning-Modellverwaltung im Azure-Portal mithilfe ihres Namens. Navigieren Sie dann zur Modellverwaltungs-Kontoseite und anschließend zu „Modellverwaltung“ > „Dienste“.


```python
# ##### OPTIONAL###### 
# Interactive CLI setup helper, including model management account and deployment environment.
# If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
#
# UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT
#
# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
# # Optional. Persist your model on disk and reuse it later for deployment. 
# from cvtk import TFFasterRCNN, Context
# import os
# save_model_path = os.path.join(Context.get_global_context().storage.persistent_path, "saved_classifier.model")
# # Save model to disk
# dnn_model.serialize(save_model_path)
# # Load model from disk
# dnn_model = CNTKTLModel.deserialize(save_model_path)
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Optional Azure Machine Learning deployment cluster name (environment name) and resource group name
# If you don't provide here. It will use the current deployment environment (you can check with CLI command "az ml env show").
azureml_rscgroup = "<resource group>"
cluster_name = "<cluster name>"

# If you provide the cluster information, it will use the provided cluster to deploy. 
# Example: deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Create deployment object
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=dnn_model, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# Create the web service
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>Nutzen des Webdiensts 

Nach der Bereitstellung des Modells als Webdienst können Sie Bilder mit dem Webdienst mit einer dieser Methoden bewerten:

- Bewerten Sie den Webdienst direkt mit dem Bereitstellungsobjekt mit `deploy_obj.score_image(image_path_or_url)`.

- Verwenden Sie die Dienstendpunkt-URL und den Dienstschlüssel (keiner für die lokale Bereitstellung) mit: `AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`.

- Generieren Sie die HTTP-Anforderungen direkt, um den Webdienstendpunkt zu bewerten. Diese Option eignet sich für fortgeschrittene Benutzer.

### <a name="score-with-existing-deployment-object"></a>Bewerten mit vorhandenem Bereitstellungsobjekt

```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json)

# Score image url with added parameters. Add softmax to score.
print("Score image url with added parameters. Add softmax to score")
from cvtk.utils.constants import ClassificationRESTApiParameters
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224], parameters={ClassificationRESTApiParameters.ADD_SOFTMAX:True})
print("serialized_result_in_json:", serialized_result_in_json)
```


```python
# Time image scoring
import timeit

for img_index, img_obj in enumerate(test_set.images[:10]):
    print("Calling API for image {} of {}: {}...".format(img_index, len(test_set.images), img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
    print(return_json)
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Bewerten mit Dienstendpunkt-URL und Dienstschlüssel

`AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`

```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "" # please replace with your own service url
service_key = "" # please replace with your own service key
# score local image with file path
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key)
print("serialized_result_in_json:", serialized_result_in_json)

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)
```

### <a name="score-endpoint-with-http-request-directly"></a>Direktes Bewerten des Endpunkts mit HTTP-Anforderung

Der folgende Beispielcode generiert die HTTP-Anforderung direkt in Python. Allerdings können Sie auch andere Programmiersprachen nutzen.


```python
def score_image_list_with_http(images, service_endpoint_url, service_key=None, parameters={}):
    """Score image list with http request

    Args:
        images(list): list of (input image file path, base64 image string, url or buffer)
        service_endpoint_url(str): endpoint url
        service_key(str): service key, None for local deployment.
        parameters(dict): service additional parameters in dictionary


    Returns:
        result (list): list of serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    routing_id = ""

    if service_key is None:
        headers = {'Content-Type': 'application/json',
                   'X-Marathon-App-Id': routing_id}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key), 'X-Marathon-App-Id': routing_id}
    payload = []
    for image in images:
        encoded = None
        # read image
        with open(image,'rb') as f:
            image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        # convert your image to base64 string
        encoded = base64.b64encode(image_buffer.getvalue())
        image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
        payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result

# Test with images
images = [test_set.images[0].storage_path, test_set.images[1].storage_path] # A list of local image files
score_image_list_with_http(images, service_endpoint_url, service_key)
```

### <a name="parse-serialized-result-from-web-service"></a>Analysieren des serialisierten Ergebnisses aus dem Webdienst

Die Ausgabe aus dem Webdienst ist eine JSON-Zeichenfolge. Sie können diese JSON-Zeichenfolge mit anderen DNN-Modellklassen analysieren.


```python
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)
```


```python
# Parse result from json string
import numpy as np
parsed_result = CNTKTLModel.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
# Map result to image class
class_index = np.argmax(np.array(parsed_result))
print("Class index:", class_index)
dnn_model.class_map
print("Class label:", dnn_model.class_map[class_index])
```


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie in den folgenden Artikeln mehr über das Azure Machine Learning-Paket für maschinelles Sehen:

+ Erfahren Sie, wie Sie die [Genauigkeit des Modells verbessern](how-to-improve-accuracy-for-computer-vision-models.md) können.

+ Lesen Sie die [Paketübersicht](https://aka.ms/aml-packages/vision).

+ Lesen Sie die [Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) zu diesem Paket.

+ Erfahren Sie mehr über [andere Python-Pakete für Azure Machine Learning](reference-python-package-overview.md).
