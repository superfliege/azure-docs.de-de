---
title: Trainieren von Machine Learning-Modellen mit Azure Machine Learning
description: Erfahren Sie, wie Sie Trainings auf einem einzelnen Knoten und verteilte Trainings konventioneller Machine Learning- und Deep Learning-Modelle mit Azure Machine Learning Services ausführen.
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 50b808b5769f2160d765ecdb431d71856e651b33
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983581"
---
# <a name="how-to-train-models-with-azure-machine-learning"></a>Trainieren von Modellen mit Azure Machine Learning

Das Training von Machine Learning-Modellen, insbesondere tiefer neuronaler Netzwerke, ist häufig eine zeit- und rechenintensive Aufgabe. Sobald Sie Ihr Trainingsskript geschrieben haben und eine kleine Teilmenge der Daten auf dem lokalen Computer ausgeführt wird, möchten Sie wahrscheinlich Ihre Workload zentral hochskalieren.

Um Schulungen zu erleichtern, bietet das Azure Machine Learning-Python-SDK eine allgemeine Abstraktion, die Estimator-Klasse, die Benutzern ermöglicht, problemlos ihre Modelle im Azure-Ökosystem zu trainieren. Sie können ein Estimator-Objekt erstellen und dazu verwenden, einen Trainingscode zu übermitteln, den Sie auf einem Remotecomputeziel ausführen möchten. Die Ausführung ist als Einzelknotentraining oder verteiltes Training auf einem GPU-Cluster möglich. Für PyTorch- und TensorFlow-Aufträge bietet Azure Machine Learning außerdem die entsprechenden benutzerdefinierten PyTorch- und TensorFlow-Estimators, die die Nutzung dieses Frameworks erleichtern.

## <a name="train-with-an-estimator"></a>Trainieren mit einem Estimator

Nach der Erstellung Ihres [Arbeitsbereichs](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace) und Einrichten Ihrer [Entwicklungsumgebung](how-to-configure-environment.md) umfasst das Trainieren eines Modells in Azure Machine Learning die folgenden Schritte:  
1. [Erstellen eines Remotecomputeziels](how-to-set-up-training-targets.md)
2. [Hochladen Ihrer Trainingsdaten](how-to-access-data.md) (Optional)
3. Erstellen Ihres Trainingsskripts
4. Erstellen eines Estimator-Objekts
5. Übermitteln Ihres Trainingsauftrags

Der Schwerpunkt dieses Artikels liegt auf den Schritten 4-5. Für die Schritte 1 bis 3 finden Sie in diesem [Tutorial](tutorial-train-models-with-aml.md) ein Beispiel.

### <a name="single-node-training"></a>Training auf einem einzelnen Knoten

Der folgende Code führt Sie durch die Ausführung eines Trainings auf einem einzelnen Knoten auf einem Remotecomputeziel in Azure für ein Scikit-Modell. Das [Computeziel](how-to-set-up-training-targets.md#batch)-Objekt `compute_target` und Ihr [Datenspeicherobjekt](how-to-access-data.md) `ds` sollten Sie bereits erstellt haben.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])
```

Der obige Codeausschnitt gibt die folgenden Parameter für den Estimator-Konstruktor an:
* `source_directory`: das lokale Verzeichnis mit dem gesamten Code, der für den Trainingsauftrag erforderlich ist. Dieser Ordner wird von Ihrem lokalen Computer auf das Remotecomputeziel kopiert. 
* `script_params`: ein Wörterbuch, in dem die Befehlszeilenargumente Ihres Trainingsskripts `entry_script` mit Wertpaaren der Form <Befehlszeilenargument, Wert> festgelegt werden.
* `compute_target`: das Remotecomputeziel, auf dem Ihr Trainingsskript ausgeführt wird. In diesem Fall handelt es sich um einen [Batch AI](how-to-set-up-training-targets.md#batch)-Cluster.
* `entry_script`: der Dateipfad des Trainingsskripts (relativ zu `source_directory`), das auf dem Remotecomputeziel ausgeführt werden soll. Diese Datei und alle von ihr abhängigen Dateien sollten sich in diesem Ordner befinden.
* `conda_packages`: die Liste der Python-Pakete, die über conda installiert werden und für Ihr Trainingsskript erforderlich sind.  
Im Konstruktor kann zusätzlich der Parameter `pip_packages` für alle erforderlichen pip-Pakete angegeben werden.

Nun haben Sie Ihr Estimator-Objekt erstellt und können den auszuführenden Trainingsauftrag auf dem Remotecomputeziel über einen Aufruf der `submit`-Funktion auf Ihrem [Experiment](concept-azure-machine-learning-architecture.md#experiment)-Objekt `experiment` ausführen. 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Spezielle Ordner** Zwei Ordner, *outputs* und *logs*, erhalten eine besondere Behandlung durch Azure Machine Learning. Wenn Sie während des Trainings Dateien in die Ordner *outputs* und *logs* schreiben, die zum Stammverzeichnis relativ sind (`./outputs` und `./logs`), werden diese Dateien automatisch in Ihren Ausführungsverlauf hochgeladen, damit Sie Zugriff darauf haben, wenn die Ausführung abgeschlossen ist. 
>
> Um auf Elemente zuzugreifen, die während des Trainings erstellt wurden (z.B. Modelldateien, Prüfpunkte, Datendateien oder gezeichnete Bilder), schreiben Sie diese in den `./outputs`-Ordner.
>
> Auf ähnliche Weise können Sie alle Protokolle aus Ihrer Trainingsausführung in den `./logs`-Ordner schreiben. Um die [TensorBoard-Integration](https://aka.ms/aml-notebook-tb) in Azure Machine Learning zu nutzen, stellen Sie sicher, dass Sie Ihre TensorBoard-Protokolle in diesen Ordner schreiben. Während Ihre Ausführung läuft, können Sie TensorBoard starten und diese Protokolle streamen.  Später können Sie auch die Protokolle aus Ihrer vorherigen Ausführungen wiederherstellen.
>
> So laden Sie beispielsweise eine Datei herunter, die nach Ihrer Remotetrainingausführung in den *outputs*-Ordner auf Ihrem lokalen Computer geschrieben wurde: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Verteiltes Training und benutzerdefinierte Docker-Images

Es gibt zwei zusätzliche Trainingsszenarien, die Sie mit dem Estimator durchführen können:
1. Verwenden eines benutzerdefinierten Docker-Images
2. Verteiltes Training in einem Cluster mit mehreren Knoten

Der folgende Code zeigt, wie Sie ein verteiltes Training für ein CNTK-Modell ausführen können. Außerdem geht er, statt die standardmäßigen Azure Machine Learning-Images zu verwenden, davon aus, dass Sie über Ihr eigenes benutzerdefiniertes Docker-Image verfügen, das Sie für das Training verwenden möchten.

Das [Computeziel](how-to-set-up-training-targets.md#batch)-Objekt `compute_target` sollten Sie bereits erstellt haben. Sie können den Estimator wie folgt erstellen:

```Python
from azureml.train.estimator import Estimator

estimator = Estimator(source_directory='./my-cntk-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',     
                      pip_packages=['cntk==2.5.1'],
                      custom_docker_base_image='microsoft/mmlspark:0.12')
```

Im obigen Code werden im Estimator-Konstruktor die folgenden neuen Parameter angegeben:
* `custom_docker_base_image`: der Name des zu verwendenden Images. Sie können nur Images bereitstellen, die in öffentlichen Docker-Repositorys (in diesem Fall Docker Hub) verfügbar sind. Um ein Image aus einem privaten Docker-Repository zu verwenden, verwenden Sie stattdessen den `environment_definition`-Parameter des Konstruktors.
* `node_count`: die Anzahl der Knoten, die für Ihren Trainingsauftrag verwendet werden sollen. Der Standardwert für dieses Argument ist `1`.
* `process_count_per_node`: die Anzahl der Prozesse (oder „Worker“), die auf jedem Knoten ausgeführt werden sollen. In diesem Fall verwenden Sie die auf jedem Knoten verfügbaren `2`-GPUs. Der Standardwert für dieses Argument ist `1`.
* `distributed_backend`: das über MPI vom Estimator bereitgestellte Back-End für den Start des verteilten Trainings. Der Standardwert für dieses Argument ist `None`. Wenn Sie parallele oder verteilte Trainings ausführen möchten (wenn also z.B. `node_count` > 1, `process_count_per_node` > 1 oder beides zutrifft), legen Sie `distributed_backend='mpi'` fest. Die von AML verwendete MPI-Implementierung ist [Open MPI](https://www.open-mpi.org/).

Übermitteln Sie abschließend den Trainingsauftrag:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Beispiele
Ein Tutorial für das Trainieren eines sklearn-Modells finden Sie unter:
* `tutorials/01.train-models.ipynb`

Ein Tutorial für verteiltes CNTK mithilfe von benutzerdefiniertem Docker finden Sie unter:
* `training/06.distributed-cntk-with-custom-docker/06.distributed-cntk-with-custom-docker.ipynb`

Rufen Sie diese Notebooks ab:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)
* [Trainieren von PyTorch-Modellen](how-to-train-pytorch.md)
* [Trainieren von TensorFlow-Modellen](how-to-train-tensorflow.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
