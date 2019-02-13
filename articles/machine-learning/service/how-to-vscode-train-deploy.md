---
title: Trainieren und Bereitstellen von Modellen aus VS Code
titleSuffix: Azure Machine Learning service
description: Erfahren Sie mehr über Azure Machine Learning für Visual Studio Code und wie Sie mit dem Training und der Bereitstellung von Machine Learning und Deep Learning-Modellen in Azure Machine Learning Service mit Visual Studio Code beginnen können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6d190e4c5d283b9f803cf7287445d7591b5465db
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55753111"
---
# <a name="use-visual-studio-code-to-train-and-deploy-machine-learning-models"></a>Verwenden von Visual Studio Code zum Trainieren und Bereitstellen von Machine Learning-Modellen

In diesem Artikel erfahren Sie, wie Sie die Erweiterung **Azure Machine Learning für Visual Studio Code** verwenden können, um Machine Learning und Deep Learning-Modelle mit Azure Machine Learning Service in Visual Studio Code (VS Code) zu trainieren und bereitzustellen.

Azure Machine Learning bietet Unterstützung für die Ausführung von Experimenten, lokal und auf Remotecomputezielen. Für jedes Experiment können Sie mehrere Ausführungen nachverfolgen, so oft wie Sie verschiedene Techniken, Hyperparameter und mehr iterativ ausprobieren müssen. Sie können Azure Machine Learning verwenden, um benutzerdefinierte Metriken und experimentelle Läufe nachzuverfolgen, wodurch Sie Reproduzierbarkeit und Überwachung im Sinne von Data Science gewinnen.

Und Sie können diese Modelle für Ihre Test- und Produktionsanforderungen bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](http://aka.ms/AMLFree) aus.

+ Richten Sie die Erweiterung [Azure Machine Learning für Visual Studio Code](how-to-vscode-tools.md) ein.

+ [Azure Machine Learning SDK für Python, installiert](how-to-vscode-tools.md) mit VS Code.

## <a name="create-and-manage-compute-targets"></a>Erstellen und Verwalten von Computezielen

Mit Azure Machine Learning für VS Code können Sie Ihre Daten vorbereiten, Modelle trainieren und sie sowohl lokal als auch auf Remotecomputezielen bereitstellen.

Diese Erweiterung unterstützt mehrere verschiedene Remotecomputeziele für Azure Machine Learning. Betrachten Sie dazu die [vollständige Liste der unterstützten Computeziele](how-to-set-up-training-targets.md) für Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Erstellen von Computezielen für Azure Machine Learning in VS Code

**So erstellen Sie ein Computeziel:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

2. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts. Im animierten Bild ist der Name des Abonnements „Free Trial“, und der Arbeitsbereich lautet „TeamWorkspace“.

3. Klicken Sie unter dem Arbeitsbereichsknoten mit der rechten Maustaste auf den Knoten **Compute**, und wählen Sie **Create Compute** (Compute erstellen) aus.

4. Wählen Sie den Computezieltyp in der Liste aus.

5. Wählen Sie in der Befehlspalette eine VM-Größe aus.

6. Geben Sie in der Befehlspalette einen Namen für das Computeziel in das Feld ein.

7. Geben Sie alle erweiterten Eigenschaften in der JSON-Konfigurationsdatei an, die in einer neuen Registerkarte geöffnet wird. Sie können Eigenschaften wie etwa die maximale Knotenanzahl angeben.

8. Wenn Sie das Computeziel konfiguriert haben, klicken Sie in der rechten unteren Bildschirmecke auf **Übermitteln**.

Hier sehen Sie ein Beispiel für die Erstellung von Azure Machine Learning Compute (AMLCompute): [![Erstellen von AML-Compute in VS Code](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Die Laufzeitkonfigurationsdatei

Die VS Code-Erweiterung erstellt automatisch ein lokales Computeziel und Laufzeitkonfigurationen für Ihre **lokalen** und **Docker-basierten** Umgebungen auf Ihrem lokalen Computer. Die Laufzeitkonfigurationsdateien befinden sich unter dem zugeordneten Computeziel.

Hier sehen Sie einen Codeausschnitt aus der lokalen Standard-Laufzeitkonfigurationsdatei. Standardmäßig gilt: `userManagedDependencies: True`. Daher müssen Sie alle Ihre Bibliotheken/Abhängigkeiten selbst installieren. Anschließend verwenden Ihre lokalen Experimentausführungen Ihre Python-Standardumgebung, wie von der VS Code Python-Erweiterung angegeben.

```yaml
# user_managed_dependencies = True indicates that the environment will be user managed. False indicates that Azure Machine Learning service will manage the user environment.
    userManagedDependencies: True
# The python interpreter path
    interpreterPath: python
# Path to the conda dependencies file to use for this run. If a project
# contains multiple programs with different sets of dependencies, it may be
# convenient to manage those environments with separate files.
    condaDependenciesFile: aml_config/conda_dependencies.yml
# Docker details
    docker:
# Set True to perform this run inside a Docker container.
    enabled: false
```

## <a name="train-and-tune-models"></a>Trainieren und Optimieren von Modellen

Verwenden Sie Azure Machine Learning für VS Code (Vorschau), um Ihren Code schnell zu durchlaufen, ihn schrittweise auszuführen und zu debuggen, und Ihre bevorzugte Quellcodeverwaltung zu verwenden.

**So führen Sie Ihr Experiment lokal mit Azure Machine Learning aus:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts.

1. Erweitern Sie unter dem Arbeitsbereichknoten den Knoten **Compute**, und klicken Sie mit der rechten Maustaste auf die **Ausführungskonfiguration** der Computeressource, die Sie verwenden möchten.

1. Wählen Sie **Run Experiment** (Experiment ausführen) aus.

1. Wählen Sie im Datei-Explorer das auszuführende Skript aus.

1. Klicken Sie auf **View Experiment Run** (Experimentausführung anzeigen), um das integrierte Azure Machine Learning-Portal anzuzeigen, in dem Sie Ihre Ausführungen überwachen und Ihre trainierten Modelle anzeigen können.

Hier sehen Sie ein Beispiel für das lokale Ausführen eines Experiments: [![Lokales Ausführen eines Experiments](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>Verwenden von Remotecomputeressourcen für Experimente in VS Code

Um beim Trainieren ein Remotecomputeziel zu verwenden, müssen Sie eine Laufzeitkonfigurationsdatei erstellen. Diese Datei teilt Azure Machine Learning nicht nur mit, wo Ihr Experiment ausgeführt sondern auch, wie die Umgebung vorbereitet werden soll.

#### <a name="the-conda-dependencies-file"></a>Die Conda-Abhängigkeitsdatei

Standardmäßig wird für Sie eine neue Conda-Umgebung erstellt, und Ihre Installationsabhängigkeiten werden verwaltet. Allerdings müssen Sie Ihre Abhängigkeiten und deren Versionen in der Datei `aml_config/conda_dependencies.yml` angeben. 

Dies ist ein Codeausschnitt aus der Standarddatei „aml_config/conda_dependencies.yml“. Sie können beispielsweise wie unten angegeben „tensorflow=1.12.0“ angeben. Wenn Sie nicht die Version der Abhängigkeit angeben, wird die neueste Version verwendet.  
Sie können der Konfigurationsdatei zusätzliche Abhängigkeiten hinzufügen.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure Machine Learning service only supports 3.5.2 and later.

- python=3.6.2
- tensorflow=1.12.0

- pip:
    # Required packages for Azure Machine Learning service execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

**So führen Sie Ihr Experiment mit Azure Machine Learning auf einem Remotecomputeziel aus:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts.

1. Klicken Sie mit der rechten Maustaste im Editor-Fenster auf das Python-Skript, und wählen Sie **AML: Run as Experiment in Azure** (AML: als Experiment in Azure ausführen) aus.

1. Wählen Sie in der Befehlspalette das Computeziel aus.

1. Geben Sie in der Befehlspalette den Namen der Laufzeitkonfiguration in das Feld ein.

1. Bearbeiten Sie die Datei „conda_dependencies.yml“, um die Laufzeitabhängigkeiten des Experiments anzugeben, und klicken Sie anschließend in der rechten unteren Bildschirmecke auf **Übermitteln**.

1. Klicken Sie auf **View Experiment Run** (Experimentausführung anzeigen), um das integrierte Azure Machine Learning-Portal anzuzeigen, in dem Sie Ihre Ausführungen überwachen und Ihre trainierten Modelle anzeigen können.

Hier sehen Sie ein Beispiel für das Ausführen eines Experiments auf einem Remotecomputeziel: [![Ausführen eines Experiments auf einem Remoteziel](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Erstellen und Verwalten von Modellen
Azure Machine Learning ermöglicht das Bereitstellen und Verwalten Ihrer Machine Learning-Modelle in der Cloud und auf dem Edge.

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Registrieren Ihres Modells bei Azure Machine Learning aus VS Code

Nachdem Sie Ihr Modell nun trainiert haben, können Sie es in Ihrem Arbeitsbereich registrieren.
Registrierte Modelle können nachverfolgt und bereitgestellt werden.

**So registrieren Sie Ihr Modell:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts.

1. Klicken Sie unter dem Knoten „Arbeitsbereich“ mit der rechten Maustaste auf **Modelle**, und wählen Sie **Register Model** (Modell registrieren) aus.

1. Geben Sie in der Befehlspalette einen Modellnamen in das Feld ein.

1. Wählen Sie in der Liste aus, ob Sie eine **Modelldatei** (für einzelne Modelle) oder einen **Modellordner** (für Modelle mit mehreren Dateien, wie etwa Tensorflow) hochladen möchten.

1. Wählen Sie Ihren Ordner oder die Datei aus.

1. Wenn Sie die Modelleigenschaften konfiguriert haben, klicken Sie in der rechten unteren Bildschirmecke auf **Übermitteln**.

Hier sehen Sie ein Beispiel für die Registrierung Ihres Modells bei AML: [![Registrieren eines Modells bei AML](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


### <a name="deploy-your-service-from-vs-code"></a>Bereitstellen Ihres Diensts aus VS Code

Mithilfe von VS Code können Sie Ihren Webdienst an diesen Orten bereitstellen:
+ Azure Container Instance (ACI): zum Testen
+ Azure Kubernetes Service (AKS): zur Produktion

Sie brauchen für das Testen nicht vorab einen ACI-Container zu erstellen, da sie dynamisch erstellt werden. Allerdings müssen AKS-Cluster im Voraus konfiguriert werden.

Erfahren Sie mehr über [Bereitstellung mit Azure Machine Learning](how-to-deploy-and-where.md) im Allgemeinen.

**So stellen Sie einen Webdienst bereit:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und Ihren Arbeitsbereich des Azure Machine Learning-Diensts.

1. Erweitern Sie unter dem Arbeitsbereich-Knoten den Knoten **Modelle**.

1. Klicken Sie mit der rechten Maustaste auf das Modell, das Sie bereitstellen möchten, und wählen Sie im Kontextmenü den Befehl **Deploy Service from Registered Model** (Dienst aus registriertem Modell bereitstellen) aus.

1. Wählen Sie in der Befehlspalette in der Liste das Computeziel aus, auf dem die Bereitstellung erfolgen soll.

1. Geben Sie in der Befehlspalette einen Namen für diesen Dienst in das Feld ein.

1. Drücken Sie in der Befehlspalette die EINGABETASTE auf Ihrer Tastatur, um die Optionen zu durchsuchen und die Skriptdatei auszuwählen.

1. Drücken Sie in der Befehlspalette die EINGABETASTE auf Ihrer Tastatur, um die Optionen zu durchsuchen und die Conda-Abhängigkeitsdatei auszuwählen.

1. Wenn Sie die Diensteigenschaften konfiguriert haben, klicken Sie zum Bereitstellen in der rechten unteren Bildschirmecke auf **Übermitteln**. In dieser Diensteigenschaftendatei können Sie eine lokale Docker-Datei oder eine Datei vom Typ „schema.json“ angeben, die Sie verwenden möchten.

Der Webdienst ist nun bereitgestellt.

Hier sehen Sie ein Beispiel für die Bereitstellung eines Webdiensts: [![Bereitstellen eines Webdiensts](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Eine exemplarische Vorgehensweise zum Trainieren mit Machine Learning außerhalb von VS Code finden Sie in [Tutorial: Trainieren von Modellen mit Azure Machine Learning](tutorial-train-models-with-aml.md).

Eine exemplarische Vorgehensweise zum lokalen Bearbeiten, Ausführen und Debuggen finden Sie im [Python-Hello World-Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)
