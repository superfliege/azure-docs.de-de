---
title: Trainieren von PyTorch-Modellen mit Azure Machine Learning
description: Erfahren Sie, wie Sie mit dem PyTorch-Estimator PyTorch-Modelle auf einem Knoten oder verteilt trainieren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: e569b63f676fb750bcbab88dda6cda39156d41f5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977031"
---
# <a name="how-to-train-pytorch-models"></a>Trainieren von PyTorch-Modellen

Azure Machine Learning stellt für das DNN-Training (Deep Neural Networks) mit PyTorch eine spezielle PyTorch-Klasse des Estimator bereit. Mit dem PyTorch-Estimator aus dem Azure SDK können Sie PyTorch-Trainingsaufträge sowohl für die Ausführung auf einem Knoten als auch für die verteilte Ausführung auf Azure-Computezielen auf einfache Weise übermitteln.

## <a name="single-node-training"></a>Training auf einem einzelnen Knoten
Das Trainieren mithilfe des PyTorch-Estimator funktioniert in etwa so wie mit dem [Basis-Estimator](how-to-train-ml-models.md). Lesen Sie daher zuerst diesen Artikel, und machen Sie sich mit den dort vorgestellten Konzepten vertraut.
  
Instanziieren Sie ein `PyTorch`-Objekt, um einen PyTorch-Auftrag auszuführen. Das [Computeziel](how-to-set-up-training-targets.md#batch)-Objekt `compute_target` und Ihr [Datenspeicherobjekt](how-to-access-data.md) `ds` sollten Sie bereits erstellt haben.

```Python
from azureml.train.dnn import PyTorch

script_params = {
    '--data_dir': ds
}

pt_est = PyTorch(source_directory='./my-pytorch-proj',
                 script_params=script_params,
                 compute_target=compute_target,
                 entry_script='train.py',
                 use_gpu=True)
```

Hier geben Sie für den PyTorch-Konstruktor die folgenden Parameter an:
* `source_directory`: das lokale Verzeichnis mit dem gesamten Code, der für den Trainingsauftrag erforderlich ist. Dieser Ordner wird von Ihrem lokalen Computer auf das Remotecomputeziel kopiert.
* `script_params`: ein Wörterbuch, in dem die Befehlszeilenargumente zum Trainieren Ihres Skripts `entry_script` mit Wertpaaren der Form <Befehlszeilenargument, Wert> festgelegt werden.
* `compute_target`: das Remotecomputeziel, auf dem Ihr Trainingsskript ausgeführt wird. In diesem Fall handelt es sich um einen [Batch AI](how-to-set-up-training-targets.md#batch)-Cluster.
* `entry_script`: der Dateipfad des Trainingsskripts (relativ zu `source_directory`), das auf dem Remotecomputeziel ausgeführt werden soll. Diese Datei und alle von ihr abhängigen Dateien sollten sich in diesem Ordner befinden.
* `conda_packages`: die Liste der Python-Pakete, die über conda installiert werden und für Ihr Trainingsskript erforderlich sind.
Im Konstruktor kann zusätzlich der Parameter `pip_packages` für alle erforderlichen pip-Pakete angegeben werden.
* `use_gpu`: Legen Sie dieses Flag auf `True` fest, um die GPU für das Training zu nutzen. Der Standardwert lautet `False`.

Da Sie den PyTorch-Estimator verwenden, enthält der Container, der für das Training verwendet wird, standardmäßig das PyTorch-Paket und alle zugehörigen Abhängigkeiten, die für das Training mit CPUs und GPUs erforderlich sind.

Übermitteln Sie anschließend den PyTorch-Auftrag:
```Python
run = exp.submit(pt_est)
```

## <a name="distributed-training"></a>Verteiltes Training
Mit dem PyTorch-Estimator können Sie Modelle in unterschiedlichen Größenordnungen trainieren und dabei das Training auf mehrere CPU- und GPU-Cluster Ihrer Azure-VMs aufteilen. Das verteilte PyTorch-Training lässt sich leicht mit wenigen API-Aufrufen ausführen. Azure Machine Learning verwaltet dabei intern die Infrastruktur und die Orchestrierung, die zur Ausführung dieser Workloads erforderlich sind.

Azure Machine Learning unterstützt derzeit MPI-basiertes verteiltes Training von PyTorch mit dem Horovod-Framework.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) ist ein Open Source-Ring-Allreduce-Framework, das von Uber für verteiltes Training entwickelt wurde.

Erstellen Sie das PyTorch-Objekt wie folgt, um das verteilte PyTorch-Training mithilfe des Horovod-Frameworks auszuführen:

```Python
from azureml.train.dnn import PyTorch

pt_est = PyTorch(source_directory='./my-pytorch-project',
                 script_params={},
                 compute_target=compute_target,
                 entry_script='train.py',
                 node_count=2,
                 process_count_per_node=1,
                 distributed_backend='mpi',
                 use_gpu=True)
```

Im obigen Code werden im PyTorch-Konstruktor die folgenden neuen Parameter angegeben:
* `node_count`: die Anzahl der Knoten, die für Ihren Trainingsauftrag verwendet werden sollen. Der Standardwert für dieses Argument ist `1`.
* `process_count_per_node`: die Anzahl der Prozesse (oder „Worker“), die auf jedem Knoten ausgeführt werden sollen. Der Standardwert für dieses Argument ist `1`.
* `distributed_backend`: das über MPI vom Estimator bereitgestellte Back-End für den Start des verteilten Trainings. Der Standardwert für dieses Argument ist `None`. Wenn Sie mit MPI (und Horovod) parallele oder verteilte Trainings ausführen möchten (wenn also z.B. `node_count` > 1, `process_count_per_node` > 1 oder beides zutrifft), legen Sie `distributed_backend='mpi'` fest. Von Azure Machine Learning wird die MPI-Implementierung [Open MPI](https://www.open-mpi.org/) verwendet.

Im obigen Beispiel wird verteiltes Training mit zwei Workern, also einem Worker pro Knoten, ausgeführt.

Horovod und alle zugehörigen Abhängigkeiten werden automatisch installiert. Sie können das entsprechende Modul daher einfach wie folgt in das Trainingsskript `train.py` importieren:
```Python
import torch
import horovod
```

Am Ende übermitteln Sie Ihre verteilten PyTorch-Auftrag:
```Python
run = exp.submit(pt_est)
```

## <a name="examples"></a>Beispiele
Tutorial zu PyTorch-Training auf einem Knoten:
* `training/01.train-tune-deploy-pytorch/01.train-tune-deploy-pytorch.ipynb`

Tutorial zu verteiltem PyTorch-Training mit Horovod:
* `training/02.distributed-pytorch-with-horovod/02.distributed-pytorch-with-horovod.ipynb`

Rufen Sie diese Notebooks ab:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
