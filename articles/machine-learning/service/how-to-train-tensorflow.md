---
title: Trainieren von TensorFlow-Modellen mit Azure Machine Learning
description: Erfahren Sie, wie Sie mit dem TensorFlow-Estimator TensorFlow-Modelle auf einem Knoten oder verteilt trainieren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: ba43593e90b78aaa0083faf4f8162a7663c0ad47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974220"
---
# <a name="how-to-train-tensorflow-models"></a>Trainieren von TensorFlow-Modellen

Azure Machine Learning stellt für das DNN-Training (Deep Neural Networks) mit TensorFlow eine spezielle TensorFlow-Klasse des Estimator bereit. Mit dem TensorFlow-Estimator aus dem Azure SDK (nicht zu verwechseln mit der Klasse [`tf.estimator.Estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator)) können Sie TensorFlow-Trainingsjobs sowohl für die Ausführung auf einem Knoten als auch für die verteilte Ausführung auf Azure-Computezielen auf einfache Weise übermitteln.

## <a name="single-node-training"></a>Training auf einem einzelnen Knoten
Das Trainieren mithilfe des TensorFlow-Estimator funktioniert in etwa so wie mit dem [Basis-Estimator](how-to-train-ml-models.md). Lesen Sie daher zuerst diesen Artikel, und machen Sie sich mit den dort vorgestellten Konzepten vertraut.
  
Instanziieren Sie ein `TensorFlow`-Objekt, um einen TensorFlow-Job auszuführen. Das [Computeziel](how-to-set-up-training-targets.md#batch)-Objekt `compute_target` sollten Sie bereits erstellt haben.

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
                    conda_packages=['scikit-learn'],
                    use_gpu=True)
```

Hier geben Sie für den TensorFlow-Konstruktor die folgenden Parameter an:
* `source_directory`: das lokale Verzeichnis mit dem gesamten Code, der für den Trainingsjob erforderlich ist. Dieser Ordner wird von Ihrem lokalen Computer auf das Remotecomputeziel kopiert.
* `script_params`: ein Wörterbuch, in dem die Befehlszeilenargumente zum Trainieren Ihres Skripts `entry_script` mit Wertpaaren der Form <Befehlszeilenargument, Wert> festgelegt werden.
* `compute_target`: das Remotecomputeziel, auf dem Ihr Trainingsskript ausgeführt wird. In diesem Fall handelt es sich um einen [Batch AI](how-to-set-up-training-targets.md#batch)-Cluster.
* `entry_script`: der Dateipfad des Trainingsskripts (relativ zu `source_directory`), das auf dem Remotecomputeziel ausgeführt werden soll. Diese Datei und alle von ihr abhängigen Dateien sollten sich in diesem Ordner befinden.
* `conda_packages`: die Liste der Python-Pakete, die über Conda installiert werden und für Ihr Trainingsskript erforderlich sind. In diesem Fall greift das Trainingsskript zum Laden der Daten auf `sklearn` zurück. Legen Sie daher fest, dass dieses Paket installiert wird.  
Im Konstruktor kann zusätzlich der Parameter `pip_packages` für alle erforderlichen pip-Pakete angegeben werden.
* `use_gpu`: Legen Sie dieses Flag auf `True` fest, um die GPU für das Training zu nutzen. Der Standardwert lautet `False`.

Da Sie den TensorFlow-Estimator verwenden, enthält der Container, der für das Training verwendet wird, standardmäßig das TensorFlow-Paket und alle zugehörigen Abhängigkeiten, die für das Training mit CPUs und GPUs erforderlich sind.

Übermitteln Sie anschließend den TensorFlow-Job:
```Python
run = exp.submit(tf_est)
```

## <a name="distributed-training"></a>Verteiltes Training
Mit dem TensorFlow-Estimator können Sie Modelle in unterschiedlichen Größenordnungen trainieren und dabei das Training auf mehrere CPU- und GPU-Cluster Ihrer Azure-VMs aufteilen. Das verteilte TensorFlow-Training lässt sich leicht mit wenigen API-Aufrufen ausführen. Azure Machine Learning verwaltet dabei intern die Infrastruktur und die Orchestrierung, die zur Ausführung dieser Workloads erforderlich sind.

Azure Machine Learning unterstützt zwei Methoden des verteilten Trainings in TensorFlow:
1. verteiltes Training auf Grundlage von MPI mithilfe des [Horovod](https://github.com/uber/horovod)-Frameworks
2. natives [verteiltes TensorFlow-Training](https://www.tensorflow.org/deploy/distributed) über die Parameterservermethode

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
* `node_count`: die Anzahl der Knoten, die für Ihren Trainingsjob verwendet werden sollen. Der Standardwert für dieses Argument ist `1`.
* `process_count_per_node`: die Anzahl der Prozesse (oder „Worker“), die auf jedem Knoten ausgeführt werden sollen. Der Standardwert für dieses Argument ist `1`.
* `distributed_backend`: das über MPI vom Estimator bereitgestellte Back-End für den Start des verteilten Trainings. Der Standardwert für dieses Argument ist `None`. Wenn Sie mit MPI (und Horovod) parallele oder verteilte Trainings ausführen möchten (wenn also z.B. `node_count` > 1, `process_count_per_node` > 1 oder beides zutrifft), legen Sie `distributed_backend='mpi'` fest. Von Azure Machine Learning wird die MPI-Implementierung [Open MPI](https://www.open-mpi.org/) verwendet.

Im obigen Beispiel wird verteiltes Training mit zwei Workern, also einem Worker pro Knoten, ausgeführt.

Horovod und alle zugehörigen Abhängigkeiten werden automatisch installiert. Sie können das entsprechende Modul daher einfach wie folgt in das Trainingsskript `train.py` importieren:
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
* `worker_count`: die Anzahl der Worker. Der Standardwert für dieses Argument ist `1`.
* `parameter_server_count`: die Anzahl der Parameterserver. Der Standardwert für dieses Argument ist `1`.
* `distributed_backend`: das Back-End, das für das verteilte Training verwendet werden soll. Der Standardwert für dieses Argument ist `None`. Wenn Sie verteiltes Training über den Parameterserver ausführen möchten, müssen Sie `distributed_backend='ps'` festlegen.

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
Tutorial zu TensorFlow-Training auf einem Knoten:
* `training/03.train-tune-deploy-tensorflow/03.train-tune-deploy-tensorflow.ipynb`

Tutorial zu verteiltem TensorFlow-Training mit Horovod:
* `training/04.distributed-tensorflow-with-horovod/04.distributed-tensorflow-with-horovod.ipynb`

Tutorial zu nativem verteilten TensorFlow-Training:
* `training/05.distributed-tensorflow-with-parameter-server/05.distributed-tensorflow-with-parameter-server.ipynb`

Gehen Sie zum Abrufen der Notebooks wie folgt vor:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
