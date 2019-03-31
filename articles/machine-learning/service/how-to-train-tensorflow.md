---
title: Trainieren von Modellen mit TensorFlow und Keras
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie mit den TensorFlow- und Keras-Estimatoren TensorFlow- und Keras-Modelle auf einem Knoten oder verteilt trainieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 02/21/2019
ms.custom: seodec18
ms.openlocfilehash: b41098907f801f7dae839a470249834b02c8d519
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2019
ms.locfileid: "57338551"
---
# <a name="train-tensorflow-and-keras-models-with-azure-machine-learning-service"></a>Trainieren von TensorFlow- und Keras-Modellen mit Azure Machine Learning Service

Azure Machine Learning stellt für das DNN-Training (Deep Neural Networks) mit TensorFlow eine spezielle `TensorFlow`-Klasse des `Estimator`s bereit. Mit dem [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)-Estimator aus dem Azure SDK (nicht zu verwechseln mit der Klasse [`tf.estimator.Estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator)) können Sie TensorFlow-Trainingsjobs sowohl für die Ausführung auf einem Knoten als auch für die verteilte Ausführung auf Azure-Computezielen auf einfache Weise übermitteln.

## <a name="single-node-training"></a>Training auf einem einzelnen Knoten
Das Trainieren mithilfe des `TensorFlow`-Estimators funktioniert in etwa so wie mit dem [Basis-`Estimator`](how-to-train-ml-models.md). Lesen Sie daher zuerst diesen Artikel, und machen Sie sich mit den dort vorgestellten Konzepten vertraut.
  
Instanziieren Sie ein `TensorFlow`-Objekt, um einen TensorFlow-Job auszuführen. Das [Computeziel](how-to-set-up-training-targets.md#amlcompute)-Objekt `compute_target` sollten Sie bereits erstellt haben.

```Python
from azureml.train.dnn import TensorFlow

script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py',
                    conda_packages=['scikit-learn'], # in case you need scikit-learn in train.py
                    use_gpu=True)
```

Hier geben Sie für den TensorFlow-Konstruktor die folgenden Parameter an:

Parameter | BESCHREIBUNG
--|--
`source_directory` | Lokales Verzeichnis, das den gesamten für den Trainingsauftrag erforderlichen Code enthält. Dieser Ordner wird von Ihrem lokalen Computer auf das Remotecomputeziel kopiert.
`script_params` | Wörterbuch, in dem die Befehlszeilenargumente für Ihr Trainingsskript `entry_script` in Wertpaaren der Form <Befehlszeilenargument, Wert> festgelegt sind
`compute_target` | Remotecomputeziel, auf dem Ihr Trainingsskript ausgeführt wird, in diesem Fall ein Azure Machine Learning Compute-Cluster ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)).
`entry_script` | Dateipfad des Trainingsskripts (relativ zu `source_directory`), das auf dem Remotecomputeziel ausgeführt werden soll. Diese Datei und alle von ihr abhängigen Dateien sollten sich in diesem Ordner befinden.
`conda_packages` | Liste der Python-Pakete, die über conda installiert werden und für Ihr Trainingsskript erforderlich sind. In diesem Fall greift das Trainingsskript zum Laden der Daten auf `sklearn` zurück. Legen Sie daher fest, dass dieses Paket installiert wird.  Im Konstruktor kann zusätzlich der Parameter `pip_packages` für alle erforderlichen pip-Pakete angegeben werden.
`use_gpu` | : Legen Sie dieses Flag auf `True` fest, um die GPU für das Training zu nutzen. Der Standardwert lautet `False`.

Da Sie den TensorFlow-Estimator verwenden, enthält der Container, der für das Training verwendet wird, standardmäßig das TensorFlow-Paket und alle zugehörigen Abhängigkeiten, die für das Training mit CPUs und GPUs erforderlich sind.

Übermitteln Sie anschließend den TensorFlow-Job:
```Python
run = exp.submit(tf_est)
```

## <a name="keras-support"></a>Unterstützung für Keras
[Keras](https://keras.io/) ist eine beliebte High-Level-DNN Python-API, die TensorFlow, CNTK oder Theano als Back-Ends unterstützt. Wenn Sie TensorFlow als Back-End verwenden, können Sie den TensFlow-Estimator ganz einfach verwenden, um ein Keras-Modell zu trainieren. Hier ist ein Beispiel für einen TensorFlow-Estimator mit hinzugefügten Keras:

```Python
from azureml.train.dnn import TensorFlow

keras_est = TensorFlow(source_directory='./my-keras-proj',
                       script_params=script_params,
                       compute_target=compute_target,
                       entry_script='keras_train.py',
                       pip_packages=['keras'], # just add keras through pip
                       use_gpu=True)
```
Der oben stehende Konstruktor des TensorFlow-Estimators weist den Azure Machine Learning Service an, Keras über pip in die Ausführungsumgebung zu installieren. Und `keras_train.py` kann dann die Keras-API importieren, um ein Keras-Modell zu trainieren. Ein vollständiges Beispiel finden Sie in [diesem Jupyter-Notizbuch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb).

## <a name="distributed-training"></a>Verteiltes Training
Mit dem TensorFlow-Estimator können Sie Modelle in unterschiedlichen Größenordnungen trainieren und dabei das Training auf mehrere CPU- und GPU-Cluster Ihrer Azure-VMs aufteilen. Das verteilte TensorFlow-Training lässt sich leicht mit wenigen API-Aufrufen ausführen. Azure Machine Learning verwaltet dabei intern die Infrastruktur und die Orchestrierung, die zur Ausführung dieser Workloads erforderlich sind.

Azure Machine Learning unterstützt zwei Methoden des verteilten Trainings in TensorFlow:
* verteiltes Training auf Grundlage von MPI mithilfe des [Horovod](https://github.com/uber/horovod)-Frameworks
* Natives [verteiltes TensorFlow-Training](https://www.tensorflow.org/deploy/distributed) über die Parameterservermethode

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) ist ein Open Source-Ring-Allreduce-Framework, das von Uber für verteiltes Training entwickelt wurde.

Erstellen Sie das TensorFlow-Objekt wie folgt, um das verteilte TensorFlow-Training mithilfe des Horovod-Frameworks auszuführen:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi',
                    use_gpu=True)
```

Im obigen Code werden im TensorFlow-Konstruktor die folgenden neuen Parameter angegeben:

Parameter | BESCHREIBUNG | Standard
--|--|--
`node_count` | Die Anzahl der Knoten, die für Ihren Trainingsauftrag verwendet werden sollen. | `1`
`process_count_per_node` | Die Anzahl der Prozesse (oder „Worker“), die auf jedem Knoten ausgeführt werden sollen.|`1`
`distributed_backend` | Das über MPI vom Estimator bereitgestellte Back-End für den Start des verteilten Trainings. Wenn Sie mit MPI (und Horovod) parallele oder verteilte Trainings ausführen möchten (wenn also z.B. `node_count` > 1, `process_count_per_node` > 1 oder beides zutrifft), legen Sie `distributed_backend='mpi'` fest. Von Azure Machine Learning wird die MPI-Implementierung [Open MPI](https://www.open-mpi.org/) verwendet. | `None`

Im obigen Beispiel wird verteiltes Training mit zwei Workern, also einem Worker pro Knoten, ausgeführt.

Horovod und alle zugehörigen Abhängigkeiten werden automatisch installiert. Sie können das entsprechende Modul daher wie folgt in das Trainingsskript `train.py` importieren:

```Python
import tensorflow as tf
import horovod
```

Übermitteln Sie abschließend den TensorFlow-Job:
```Python
run = exp.submit(tf_est)
```

### <a name="parameter-server"></a>Parameterserver
Sie können alternativ auch das [native verteilte TensorFlow-Training](https://www.tensorflow.org/deploy/distributed) ausführen, das auf das Parameterservermodell zurückgreift. Bei dieser Methode führen Sie das Training auf einem Cluster mit Parameterservern und Workern aus. Während des Trainings berechnen die Worker die Gradienten, und die Parameterserver aggregieren diese.

Erstellen Sie das TensorFlow-Objekt wie folgt:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps',
                    use_gpu=True)
```

Achten Sie besonders auf die folgenden Parameter, die im obigen Code im TensorFlow-Konstruktor angegeben wurden:

Parameter | BESCHREIBUNG | Standard
--|--|--
`worker_count` | Anz. von Workern. | `1`
`parameter_server_count` | Anzahl der Parameterserver. | `1`
`distributed_backend` | Back-End, das für das verteilte Training verwendet werden soll. Wenn Sie verteiltes Training über den Parameterserver ausführen möchten, legen Sie `distributed_backend='ps'` fest. | `None`

#### <a name="note-on-tfconfig"></a>Hinweis zu `TF_CONFIG`
Sie benötigen für [`tf.train.ClusterSpec`](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec) zusätzlich die Netzwerkadressen und Ports des Clusters, damit Azure Machine Learning die `TF_CONFIG`-Umgebungsvariable automatisch festlegt.

Die `TF_CONFIG`-Umgebungsvariable ist eine JSON-Zeichenfolge. Im folgenden Codeausschnitt finden Sie ein Beispiel für eine Parameterservervariable:
```
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

Wenn Sie die High-Level-[`tf.estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator)-API von TensorFlow verwenden, wird automatisch die `TF_CONFIG`-Variable analysiert und die Clusterspezifikation erstellt. 

Wenn Sie stattdessen grundlegende Low-Level-APIs von TensorFlow für das Training verwenden, müssen Sie selbst die `TF_CONFIG`-Variable analysieren und `tf.train.ClusterSpec` in Ihrem Trainingscode erstellen. In [diesem Beispiel](https://aka.ms/aml-notebook-tf-ps) würden Sie dazu **Ihr Trainingsskript** wie folgt anpassen:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

Nachdem Sie das Trainingsskript und das TensorFlow-Objekt erstellt haben, können Sie Ihren Trainingsjob übermitteln:
```Python
run = exp.submit(tf_est)
```

## <a name="examples"></a>Beispiele

[Hier finden Sie verschiedene Notizbücher für verteiltes Deep Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
