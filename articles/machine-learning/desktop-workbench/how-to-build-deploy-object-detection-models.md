---
title: Erstellen Sie ein Modell zur Erkennung von Objekten mithilfe des Azure Machine Learning-Pakets für maschinelles Sehen, und stellen Sie es bereit.
description: Informationen zum Erstellen, Trainieren, Testen und Bereitstellen eines Objekterkennungsmodells für maschinelles Sehen mithilfe des Azure Machine Learning-Pakets für Computer Vision.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 06/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 166adc2b464ede5ba77d049075479e3b37a02a88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46954857"
---
# <a name="build-and-deploy-object-detection-models-with-azure-machine-learning"></a>Erstellen und Bereitstellen von Objekterkennungsmodellen mit Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


In diesem Artikel erfahren Sie, wie Sie das **Azure Machine Learning-Paket für maschinelles Sehen** zum Trainieren, Testen und Bereitstellen eines [Faster R-CNN](https://arxiv.org/abs/1506.01497)-Objekterkennungsmodells verwenden. 

Viele Probleme im Bereich maschinellen Sehens können mit Objekterkennung gelöst werden. Zu diesen Problemen gehört das Erstellen von Modellen, die eine variable Anzahl von Objekten in einem Bild finden. 

Beim Erstellen und Bereitstellen dieses Modells mit diesem Paket durchlaufen Sie die folgenden Schritte:
1.  Dataseterstellung
2.  DNN-Modelldefinition (Deep Neural Network)
3.  Modelltraining
4.  Auswertung und Visualisierung
5.  Webdienstbereitstellung
6.  Webdienst-Auslastungstests

In diesem Beispiel wird TensorFlow als Deep Learning-Framework verwendet, das Training erfolgt lokal auf einem GPU-gestützten Computer wie der [Deep Learning Data Science-VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview), und die Bereitstellung verwendet die Azure ML Operationalization CLI.

Ausführliche Informationen zu jedem Modul und jeder Klasse finden Sie in der [Paketreferenzdokumentation](https://aka.ms/aml-packages/vision).

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
> [Jupyter Notebook abrufen](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>Laden der Beispieldaten

Für diese Demo wird ein aus 30 Bildern und 8 Klassen bestehendes Dataset mit Lebensmitteln in Kühlschränken bereitgestellt (Eierschachtel, Joghurt, Ketchup, Champignons, Senf, Orange, Fruchtsaft und Wasser). Zu jedem JPG-Bild ist eine XML-Anmerkungsdatei desselben Namens vorhanden. 

Die folgende Abbildung zeigt die empfohlene Ordnerstruktur. 

![Ordnerstruktur](media/how-to-build-deploy-object-detection-models/data_directory.JPG)

## <a name="image-annotation"></a>Bildanmerkung

Mit Anmerkungen versehene Objektstandorte sind zum Trainieren und Auswerten eines Objektdetektors erforderlich. [LabelImg](https://tzutalin.github.io/labelImg) ist ein Open-Source-Anmerkungstool, das Bilder mit einer Anmerkung versehen kann. LabelImg schreibt eine XML-Datei pro Bild im Pascal-VOC-Format, das von diesem Paket gelesen werden kann. 


```python
import warnings
warnings.filterwarnings("ignore")
import os, time
from cvtk.core import Context, ObjectDetectionDataset, TFFasterRCNN
from cvtk.evaluation import DetectionEvaluation
from cvtk.evaluation.evaluation_utils import graph_error_counts
from cvtk.utils import detection_utils

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)

from matplotlib import pyplot as plt
# Display the images
%matplotlib inline
```

## <a name="create-a-dataset"></a>Erstellen eines Datasets

Erstellen Sie ein CVTK-Dataset, das aus einem Satz von Bildern mit ihren jeweiligen Begrenzungsrahmenanmerkungen besteht. In diesem Beispiel werden die Kühlschrankbilder aus dem Ordner „../sample_data/foods/training“ verwendet. Es werden nur JPEG-Bilder unterstützt.


```python
image_folder = "detection/sample_data/foods/train"
data_train = ObjectDetectionDataset.create_from_dir(dataset_name='training_dataset', data_dir=image_folder,
                                                    annotations_dir="Annotations", image_subdirectory='JPEGImages')

# Show some statistics of the training image, and also give one example of the ground truth rectangle annotations
data_train.print_info()
_ = data_train.images[2].visualize_bounding_boxes(image_size = (10,10))
```

    F1 2018-05-25 23:12:21,727 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-05-25 23:12:21,733 INFO azureml.vision:dataset creating dataset for scenario=detection 
    Dataset name: training_dataset
    Total classes: 8, total images: 25
    Label-wise object counts:
        Label eggBox: 20 objects
        Label joghurt: 20 objects
        Label ketchup: 20 objects
        Label mushroom: 20 objects
        Label mustard: 20 objects
        Label orange: 20 objects
        Label squash: 40 objects
        Label water: 20 objects
    Bounding box width and height distribution:
        Bounding box widths  0/5/25/50/75/95/100-percentile: 54/61/79/117/133/165/311 pixels
        Bounding box heights 0/5/25/50/75/95/100-percentile: 48/58/75/124/142/170/212 pixels
    


![png](media/how-to-build-deploy-object-detection-models/output_6_1.JPG)


## <a name="define-a-model"></a>Definieren eines Modells

In diesem Beispiel wird das Faster R-CNN-Modell verwendet. Verschiedene Parameter können bei der Definition dieses Modells angegeben werden. Sowohl die Bedeutung dieser Parameter als auch der für das Training (siehe nächster Abschnitt) verwendeten Parameter finden Sie entweder in der CVTK-API-Dokumentation oder auf der [Tensorflow-Website zur Objekterkennung](https://github.com/tensorflow/models/tree/master/research/object_detection). Weitere Informationen zum Fast R-CNN-Modell finden Sie unter [diesem Link](https://docs.microsoft.com/cognitive-toolkit/Object-Detection-using-Faster-R-CNN#technical-details). Dieses Modell basiert auf Fast R-CNN, und weitere Informationen dazu finden Sie [hier](https://docs.microsoft.com/cognitive-toolkit/Object-Detection-using-Fast-R-CNN#algorithm-details).


```python
score_threshold = 0.0       # Threshold on the detection score, use to discard lower-confidence detections.
max_total_detections = 300  # Maximum number of detections. A high value slows down training but might increase accuracy.
my_detector = TFFasterRCNN(labels=data_train.labels, 
                           score_threshold=score_threshold, 
                           max_total_detections=max_total_detections)
```

## <a name="train-the-model"></a>Modelltraining

Das COCO-trainierte Fast R-CNN-Modell mit ResNet50 dient als Ausgangspunkt für das Training. 

Um den Detektor zu trainieren, ist die Anzahl der Trainingsschritte im Code auf 350 festgelegt, damit das Training schneller (~5 Minuten mit GPU) ausgeführt wird. Legen Sie sie in der Praxis mindestens auf das 10-fache der Anzahl der Bilder im Trainingssatz fest.

In diesem Beispiel wird die Anzahl der Trainingsschritte für den Detektor für das schnelle Training auf 350 festgelegt. In der Praxis hat sich jedoch die Faustregel bewährt, die Zahl der Schritte mindestens auf das 10-fache der Anzahl der Bilder im Trainingssatz festzulegen.

Zwei wichtige Parameter für das Training sind:
- Anzahl der Schritte zum Trainieren des Modells, die durch das num_seps-Argument dargestellt wird. Jeder Schritt trainiert das Modell mit einem Minibatch der Batchgröße eins.
- Lerngeschwindigkeiten, die durch „initial_learning_rate“ festgelegt werden können.

```python
print("tensorboard --logdir={}".format(my_detector.train_dir))

# to get good results, use a larger value for num_steps, e.g., 5000.
num_steps = 350
learning_rate = 0.001 # learning rate

start_train = time.time()
my_detector.train(dataset=data_train, num_steps=num_steps, 
                  initial_learning_rate=learning_rate)
end_train = time.time()
print(end_train-start_train)
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\train
    F1 2018-05-25 23:12:22,764 INFO azureml.vision:Fit starting in experiment  1125722225 
    F1 2018-05-25 23:12:22,767 INFO azureml.vision:model starting trainging for scenario=detection 
    Using existing checkpoint file that's saved at 'C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\models\detection\faster_rcnn_resnet50_coco_2018_01_28\model.ckpt.index'.
    TFRecords creation started.
    F1 2018-05-25 23:12:22,773 INFO On image 0 of 25
    TFRecords creation completed.
    Training started.
    Training progressing: step 0 ...
    Training progressing: step 100 ...
    Training progressing: step 200 ...
    Training progressing: step 300 ...
    F1 2018-05-25 23:18:02,730 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:18:10,722 INFO 2953 ops in the final graph.
    F1 2018-05-25 23:18:24,244 INFO azureml.vision:Fit finished in experiment  1125722225 
    Training completed.
    361.604615688324
    

TensorBoard kann verwendet werden, um den Trainingsfortschritt zu visualisieren. TensorBoard-Ereignisse befinden sich in dem Ordner, der durch das train_dir-Attribut des Modellobjekts angegeben wird. Um TensorBoard anzuzeigen, gehen Sie folgendermaßen vor:
1. Kopieren Sie den Ausdruck, der mit „tensorboard --logdir“ beginnt, in eine Befehlszeile, und führen Sie sie aus. 
2. Kopieren Sie die zurückgegebene URL aus der Befehlszeile in einen Webbrowser, um TensorBoard anzuzeigen. 

TensorBoard sollte wie im folgenden Screenshot aussehen. Es dauert einige Zeit, bis der Trainingsordner aufgefüllt ist. Wenn TensorBoard also beim ersten Mal nicht ordnungsgemäß angezeigt wird, wiederholen Sie die Schritte 1-2.  

![tensorboard](media/how-to-build-deploy-object-detection-models/tensorboard.JPG)

## <a name="evaluate-the-model"></a>Auswerten des Modells

Die Methode "evaluate" wird zum Auswerten des Modells verwendet. Diese Funktion erfordert ein ObjectDetectionDataset-Objekt als Eingabe. Das Auswertungsdataset kann mit derselben Funktion erstellt werden, die für das Trainingsdataset verwendet wurde. Die unterstützte Metrik ist „Average Precision“, wie für die [PASCAL VOC Challenge](http://host.robots.ox.ac.uk/pascal/VOC/pubs/everingham10.pdf) definiert.  


```python
image_folder = "detection/sample_data/foods/test"
data_val = ObjectDetectionDataset.create_from_dir(dataset_name='val_dataset', data_dir=image_folder)
eval_result = my_detector.evaluate(dataset=data_val)
```

    F1 2018-05-25 23:18:24,253 INFO azureml.vision:dataset creating dataset for scenario=detection 
    F1 2018-05-25 23:18:24,286 INFO On image 0 of 5
    F1 2018-05-25 23:18:29,300 INFO Starting evaluation at 2018-05-26-03:18:29
    F1 2018-05-25 23:18:32,403 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:33,158 INFO Detection visualizations written to summary with tag image-0.
    F1 2018-05-25 23:18:33,518 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:34,342 INFO Detection visualizations written to summary with tag image-1.
    F1 2018-05-25 23:18:34,714 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:35,470 INFO Detection visualizations written to summary with tag image-2.
    F1 2018-05-25 23:18:35,835 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:36,654 INFO Detection visualizations written to summary with tag image-3.
    F1 2018-05-25 23:18:37,010 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:37,798 INFO Detection visualizations written to summary with tag image-4.
    F1 2018-05-25 23:18:37,804 INFO Running eval batches done.
    F1 2018-05-25 23:18:37,805 INFO # success: 5
    F1 2018-05-25 23:18:37,806 INFO # skipped: 0
    F1 2018-05-25 23:18:38,119 INFO Writing metrics to tf summary.
    F1 2018-05-25 23:18:38,121 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/eggBox: 1.000000
    F1 2018-05-25 23:18:38,205 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/joghurt: 0.942857
    F1 2018-05-25 23:18:38,206 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/ketchup: 1.000000
    F1 2018-05-25 23:18:38,207 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mushroom: 1.000000
    F1 2018-05-25 23:18:38,208 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mustard: 1.000000
    F1 2018-05-25 23:18:38,209 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/orange: 1.000000
    F1 2018-05-25 23:18:38,210 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/squash: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/water: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/Precision/mAP@0.5IOU: 0.992857
    F1 2018-05-25 23:18:38,253 INFO Metrics written to tf summary.
    F1 2018-05-25 23:18:38,254 INFO Finished evaluation!
    

Die Ergebnisse der Auswertung können in einem Klartextformat ausgegeben werden.


```python
# print out the performance metric values
for label_obj in data_train.labels:
    label = label_obj.name
    key = 'PASCAL/PerformanceByCategory/AP@0.5IOU/' + label
    print('{0: <15}: {1: <3}'.format(label, round(eval_result[key], 2)))
print('{0: <15}: {1: <3}'.format("overall:", round(eval_result['PASCAL/Precision/mAP@0.5IOU'], 2))) 
```

    joghurt        : 0.94
    squash         : 1.0
    mushroom       : 1.0
    eggBox         : 1.0
    ketchup        : 1.0
    mustard        : 1.0
    water          : 1.0
    orange         : 1.0
    overall:       : 0.99
    

Auf ähnliche Weise können Sie die Genauigkeit des Modells im Trainingssatz berechnen. Dadurch lässt sich sicherstellen, dass das Training sich zu einer guten Lösung hin bewegt hat. Die Genauigkeit des Trainingssatzes nach erfolgreichem Training erreicht häufig fast 100%.

Die Ergebnisse der Auswertung können auch in TensorBoard angezeigt werden, einschließlich mAP-Werten und Bildern mit vorhergesagten Begrenzungsrahmen. Kopieren Sie den Ausdruck aus dem folgenden Code in ein Befehlszeilenfenster, um den TensorBoard-Client zu starten. Hier wird ein Portwert von 8008 verwendet, um einen Konflikt mit dem Standardwert 6006 zu vermeiden, der zur Anzeige des Trainingsstatus verwendet wurde.


```python
print("tensorboard --logdir={} --port=8008".format(my_detector.eval_dir))
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\eval --port=8008
    

## <a name="score-an-image"></a>Bewerten eines Bildes

Sobald Sie mit der Leistung des trainierten Modells zufrieden sind, kann die „score“-Funktion des Modellobjekts zum Bewerten neuer Bilder verwendet werden. Die zurückgegebenen Ergebnisse können mit der „visualize“-Funktion visuell dargestellt werden. 


```python
image_path = data_val.images[1].storage_path
detections_dict = my_detector.score(image_path)
path_save = "./scored_images/scored_image_preloaded.jpg"
ax = detection_utils.visualize(image_path, detections_dict, image_size=(8, 12))
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

![png](media/how-to-build-deploy-object-detection-models/output_20_0.JPG)

##  <a name="save-the-model"></a>Speichern des Modells

Das trainierte Modell kann auf einem Datenträger gespeichert und wieder in den Arbeitsspeicher geladen werden, wie in den folgenden Codebeispielen dargestellt.


```python
save_model_path = "./frozen_model/faster_rcnn.model"
my_detector.save(save_model_path)
```

    F1 2018-05-25 23:18:55,166 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:19:03,867 INFO 2953 ops in the final graph.
    

## <a name="load-the-saved-model-for-scoring"></a>Laden des gespeicherten Modells für die Bewertung

Um das gespeicherte Modell zu verwenden, laden Sie es mit der „load“-Funktion in den Arbeitsspeicher. Sie müssen nur einmal laden. 

```python
my_detector_loaded = TFFasterRCNN.load(save_model_path)
```

Nach dem Laden kann das Modell zum Bewerten eines Bildes oder einer Liste von Bildern verwendet werden. Für ein einzelnes Bild wird ein Wörterbuch mit Schlüsseln wie „detection_boxes“, „detection_scores“ und „num_detections“ zurückgegeben. Wenn die Eingabe eine Liste der Bilder ist, wird eine Wörterbuchliste zurückgegeben, wobei ein Wörterbuch einem Bild entspricht. 


```python
detections_dict = my_detector_loaded.score(image_path)
```

Die erkannten Objekte mit Bewertungen über 0,5 können einschließlich Bezeichnungen, Bewertungen und Koordinaten gedruckt werden.


```python
look_up = dict((v,k) for k,v in my_detector.class_map.items())
n_obj = 0
for i in range(detections_dict['num_detections']):
    if detections_dict['detection_scores'][i] > 0.5:
        n_obj += 1
        print("Object {}: label={:11}, score={:.2f}, location=(top: {:.2f}, left: {:.2f}, bottom: {:.2f}, right: {:.2f})".format(
            i, look_up[detections_dict['detection_classes'][i]], 
            detections_dict['detection_scores'][i], 
            detections_dict['detection_boxes'][i][0],
            detections_dict['detection_boxes'][i][1], 
            detections_dict['detection_boxes'][i][2],
            detections_dict['detection_boxes'][i][3]))    
        
print("\nFound {} objects in image {}.".format(n_obj, image_path))           
```

    Object 0: label=squash     , score=0.99, location=(top: 0.74, left: 0.30, bottom: 0.84, right: 0.42)
    Object 1: label=squash     , score=0.98, location=(top: 0.27, left: 0.21, bottom: 0.37, right: 0.33)
    Object 2: label=orange     , score=0.98, location=(top: 0.31, left: 0.39, bottom: 0.37, right: 0.48)
    Object 3: label=joghurt    , score=0.98, location=(top: 0.57, left: 0.29, bottom: 0.67, right: 0.39)
    Object 4: label=eggBox     , score=0.97, location=(top: 0.41, left: 0.53, bottom: 0.49, right: 0.69)
    Object 5: label=water      , score=0.95, location=(top: 0.23, left: 0.51, bottom: 0.37, right: 0.57)
    Object 6: label=mustard    , score=0.88, location=(top: 0.61, left: 0.47, bottom: 0.66, right: 0.57)
    Object 7: label=ketchup    , score=0.80, location=(top: 0.62, left: 0.62, bottom: 0.68, right: 0.72)
    
    Found 8 objects in image ../sample_data/foods/test\JPEGImages\10.jpg.
    

Visualisieren Sie die Ergebnisse wie zuvor.


```python
path_save = "./scored_images/scored_image_frozen_graph.jpg"
ax = detection_utils.visualize(image_path, detections_dict, path_save=path_save, image_size=(8, 12))
# ax.get_figure() # use this code extract the returned image
```

![png](media/how-to-build-deploy-object-detection-models/output_30_0.JPG)

## <a name="operationalization-deploy-and-consume"></a>Operationalisierung: Bereitstellung und Nutzung

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
# ##### OPTIONAL - Interactive CLI setup helper ###### 
# # Interactive CLI setup helper, including model management account and deployment environment.
# # If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
# # UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT

# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Create deployment object
# It will use the current deployment environment (you can check it with CLI command "az ml env show").
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=my_detector, replicas=1)

# Alternatively, you can provide azure machine learning deployment cluster name (environment name) and resource group name
# to deploy your model. It will use the provided cluster to deploy. To do that, please uncomment the following lines to create 
# the deployment object.

# azureml_rscgroup = "<resource group>"
# cluster_name = "<cluster name>"
# deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=my_detector,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# create the webservice
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>Nutzen des Webdiensts

Nach der Erstellung des Webdiensts können Sie Bilder mit dem bereitgestellten Webdienst bewerten. Sie haben mehrere Möglichkeiten:

   - Sie können den Webdienst direkt mit dem Bereitstellungsobjekt bewerten mit: deploy_obj.score_image(image_path_or_url) 
   - Oder Sie können Dienstendpunkt-URL und Dienstschlüssel („None“ für die lokale Bereitstellung) verwenden mit: AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)
   - Generieren Sie die HTTP-Anforderungen direkt, um den Webdienstendpunkt zu bewerten (für fortgeschrittene Benutzer).

### <a name="score-with-existing-deployment-object"></a>Bewerten mit vorhandenem Bereitstellungsobjekt
```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = data_train.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])

```


```python
# Time image scoring
import timeit

num_images = 3
for img_index, img_obj in enumerate(data_train.images[:num_images]):
    print("Calling API for image {} of {}: {}...".format(img_index, num_images, img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Bewerten mit Dienstendpunkt-URL und Dienstschlüssel
```
    AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)
```


```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "http://xxx" # please replace with your own service url
service_key = "xxx" # please replace with your own service key

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])
```

### <a name="score-endpoint-with-http-request-directly"></a>Direktes Bewerten des Endpunkts mit HTTP-Anforderung
Der folgende Beispielcode generiert die HTTP-Anforderung direkt in Python. Sie können auch andere Programmiersprachen nutzen.


```python
def score_image_with_http(image, service_endpoint_url, service_key=None, parameters={}):
    """Score local image with http request

    Args:
        image (str): Image file path
        service_endpoint_url(str): web service endpoint url
        service_key(str): Service key. None for local deployment.
        parameters (dict): Additional request paramters in dictionary. Default is {}.


    Returns:
        str: serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    import json

    if service_key is None:
        headers = {'Content-Type': 'application/json'}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key)}
    payload = []
    encoded = None
    
    # Read image
    with open(image,'rb') as f:
        image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        
    # Convert your image to base64 string
    # image_in_base64 : "b'{base64}'"
    encoded = base64.b64encode(image_buffer.getvalue())
    image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
    payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
        json.loads(result[0])
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result[0]

```

### <a name="parse-serialized-result-from-webservice"></a>Analysieren des serialisierten Ergebnisses aus dem Webdienst
Das Ergebnis aus dem Webdienst liegt in einer JSON-Zeichenfolge vor, die analysiert werden kann.


```python
image_path_or_url = image_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])
```


```python
# Parse result from json string
import numpy as np
parsed_result = TFFasterRCNN.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
```


```python
ax = detection_utils.visualize(image_path, parsed_result)
path_save = "./scored_images/scored_image_web.jpg"
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie in den folgenden Artikeln mehr über das Azure Machine Learning-Paket für maschinelles Sehen:

+ Lesen Sie die [Paketübersicht](https://aka.ms/aml-packages/vision).

+ Lesen Sie die [Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) zu diesem Paket.

+ Erfahren Sie mehr über [andere Python-Pakete für Azure Machine Learning](reference-python-package-overview.md).
