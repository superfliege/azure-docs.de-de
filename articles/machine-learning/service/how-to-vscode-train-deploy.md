---
title: Verwenden der Visual Studio Code Tools for AI-Erweiterung mit Azure Machine Learning
description: Erfahren Sie etwas über Visual Studio Code Tools for AI und die ersten Schritte beim Trainieren und Bereitstellen von Machine Learning- und Deep Learning-Modellen mit dem Azure Machine Learning-Dienst in Visual Studio Code.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: 6372aada8c3d380f8d3cefb0479e45bbd7c63898
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945245"
---
# <a name="vs-code-tools-for-ai-train-and-deploy-ml-models-from-vs-code"></a>Visual Studio Code Tools for AI: Trainieren und Bereitstellen von ML-Modellen aus VS Code
In diesem Artikel erfahren Sie etwas über die Verwendung der **VS Code Tools for AI**-Erweiterung zum Trainieren und Bereitstellen von Modellen für maschinelles Lernen und Deep Learning mit dem Azure Machine Learning-Dienst in VS Code.

Azure Machine Learning bietet Unterstützung für die Ausführung von Experimenten, lokal und auf Remotecomputezielen. Für jedes Experiment können Sie mehrere Ausführungen nachverfolgen, so oft wie Sie verschiedene Techniken, Hyperparameter und mehr iterativ ausprobieren müssen. Sie können Azure Machine Learning verwenden, um benutzerdefinierte Metriken und experimentelle Läufe nachzuverfolgen, wodurch Sie Reproduzierbarkeit und Überwachung im Sinne von Data Science gewinnen.

Und Sie können diese Modelle für Ihre Test- und Produktionsanforderungen bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

+ [VS Code Tools for AI, eingerichtet](how-to-vscode-tools.md) für Azure Machine Learning.

+ [Azure Machine Learning SDK für Python, installiert](how-to-vscode-tools.md) mit VS Code.

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-and-manage-compute-targets"></a>Erstellen und Verwalten von Computezielen

Mit Visual Studio Code Tools for AI können Sie Ihre Daten vorbereiten, Modelle trainieren und sie sowohl lokal als auch auf Remotecomputezielen bereitstellen.

Diese Erweiterung unterstützt mehrere verschiedene Remotecomputeziele für Azure Machine Learning. Betrachten Sie dazu die [vollständige Liste der unterstützten Computeziele](how-to-set-up-training-targets.md) für Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Erstellen von Computezielen für Azure Machine Learning in VS Code

**So erstellen Sie ein Computeziel:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

2. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts. In der animierten Grafik ist der Name des Abonnements „OpenMind Studio“, und der des Arbeitsbereichs lautet „MyWorkspace“. 

3. Klicken Sie unter dem Arbeitsbereichsknoten mit der rechten Maustaste auf den Knoten **Compute**, und wählen Sie **Create Compute** (Compute erstellen) aus.

4. Wählen Sie den Computezieltyp in der Liste aus. 

5. Geben Sie in dem Feld einen eindeutigen Namen für dieses Computeziel ein, und geben Sie die Größe des virtuellen Computers an.

6. Geben Sie alle erweiterten Eigenschaften in der JSON-Konfigurationsdatei an, die in einer neuen Registerkarte geöffnet wird. 

7. Wenn Sie das Konfigurieren Ihres Computeziels abgeschlossen haben, klicken Sie unten rechts auf **Fertig stellen**.

Hier sehen Sie ein Beispiel für Azure Batch AI: [![Erstellen von Azure Batch AI-Computeressourcen in VS Code](./media/vscode-tools-for-ai/createcompute.gif)](./media/vscode-tools-for-ai/createcompute.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>Verwenden von Remotecomputeressourcen für Experimente in VS Code

Um beim Trainieren ein Remotecomputeziel zu verwenden, müssen Sie eine Laufzeitkonfigurationsdatei erstellen. Diese Datei teilt Azure Machine Learning nicht nur mit, wo Ihr Experiment ausgeführt sondern auch, wie die Umgebung vorbereitet werden soll.

#### <a name="the-run-configuration-file"></a>Die Laufzeitkonfigurationsdatei

Die VS Code-Erweiterung erstellt automatisch eine Laufzeitkonfiguration für Ihre **lokalen** und **Docker**-Umgebungen auf Ihrem lokalen Computer.

Dies ist ein Codeausschnitt aus der Standard-Laufkonfigurationsdatei.

Wenn Sie alle Ihre Bibliotheken/Abhängigkeiten selbst installieren möchten, legen Sie `userManagedDependencies: True` fest, dann verwenden Ihre lokalen Experimentausführungen Ihre Python-Standardumgebung, wie von der VS Code Python-Erweiterung angegeben.

```yaml
# user_managed_dependencies=True indicates that the environment will be user managed. False indicates that AzureML will manage the user environment.
    userManagedDependencies: False
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

#### <a name="the-conda-dependencies-file"></a>Die Conda-Abhängigkeitsdatei

Standardmäßig wird für Sie eine neue Conda-Umgebung erstellt, und Ihre Installationsabhängigkeiten werden verwaltet. Allerdings müssen Sie Ihre Abhängigkeiten in der `aml_config/conda_dependencies.yml`-Datei angeben.

Dies ist ein Codeausschnitt aus der Standarddatei „aml_config/conda_dependencies.yml“.
Sie können der Konfigurationsdatei zusätzliche Abhängigkeiten hinzufügen.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

## <a name="train-and-tune-models"></a>Trainieren und Optimieren von Modellen

Verwenden Sie Azure Machine Learning aus VS Code, um Ihren Code schnell zu durchlaufen, ihn schrittweise auszuführen und zu debuggen, und setzen Sie Ihre bevorzugte Quellcodeverwaltung ein. 

**So führen Sie Ihre Experiment mit Azure Machine Learning aus:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts. In der animierten Grafik ist der Name des Abonnements „OpenMind Studio“, und der des Arbeitsbereichs lautet „MyWorkspace“. 

1. Erweitern Sie unter dem Knoten „Arbeitsbereich“ den Knoten **Compute**, und klicken Sie mit der rechten Maustaste auf die **Ausführungskonfiguration** der Computeressource, die Sie verwenden möchten. 

1. Wählen Sie **Run Experiment** (Experiment ausführen) aus.

1. Klicken Sie auf **View Experiment Run** (Experimentausführung anzeigen), um das integrierte Azure Machine Learning-Portal anzuzeigen, in dem Sie Ihre Ausführungen überwachen und Ihre trainierten Modelle anzeigen können.

   [![Ausführen des Machine Learning-Experiments aus VS Code](./media/vscode-tools-for-ai/runexperiment.gif)](./media/vscode-tools-for-ai/runexperiment.gif#lightbox)

## <a name="deploy-and-manage-models"></a>Erstellen und Verwalten von Modellen
Azure Machine Learning ermöglicht das Bereitstellen und Verwalten Ihrer Machine Learning-Modelle in der Cloud und auf dem Edge. 

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Registrieren Ihres Modells bei Azure Machine Learning aus VS Code

Nachdem Sie Ihr Modell nun trainiert haben, können Sie es in Ihrem Arbeitsbereich registrieren.
Registrierte Modelle können nachverfolgt und bereitgestellt werden.

**So registrieren Sie Ihr Modell:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts.

1. Klicken Sie unter dem Knoten „Arbeitsbereich“ mit der rechten Maustaste auf **Modelle**, und wählen Sie **Register Model** (Modell registrieren) aus.

1. Wählen Sie in der Liste aus, ob Sie eine **Modelldatei** (für einzelne Modelle) oder einen **Modellordner** (für Modelle mit mehreren Dateien, wie etwa Tensorflow) hochladen möchten. 

1. Verwenden Sie das Dateiauswahl-Dialogfeld, um die Datei oder den Ordner auszuwählen.

   [![Compute](./media/vscode-tools-for-ai/registermodel.gif)](./media/vscode-tools-for-ai/registermodel.gif#lightbox)

> [!Warning]
> Entfernen Sie zunächst bitte die Tags aus der generierten JSON-Datei.

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

1. Wählen Sie in der VS Code-Befehlspalette in der Liste das Computeziel aus, auf dem die Bereitstellung erfolgen soll. 

1. Geben Sie im Feld einen Namen für den Dienst ein. 

1. Klicken Sie im Dialogfeld in der unteren rechten Ecke auf **Durchsuchen**, und wählen Sie Ihr Bewertungsskript aus. Das Dialogfeld wird geschlossen.

1. Wenn Sie über eine lokale Docker-Datei verfügen, klicken Sie im zweiten Dialogfeld, das angezeigt wird, auf **Durchsuchen**. 

   Wenn Sie das Dialogfeld abbrechen, ohne eine lokale Docker-Datei anzugeben, wird standardmäßig eine Docker-Datei „Azure Machine Learning“ verwendet.

1. Klicken Sie im dritten Dialogfeld, das angezeigt wird, auf **Durchsuchen**, und wählen Sie den lokalen conda-Dateipfad aus, oder geben Sie den Dateipfad später im Json-Editor ein.

1. Wenn Sie eine schema.json-Datei verwenden möchten, klicken Sie im vierten Dialogfeld, das angezeigt wird, auf **Durchsuchen**, und wählen Sie die Datei aus.

Der Webdienst ist nun bereitgestellt.

Hier sehen Sie ein Beispiel für Azure Container Instance: [ ![Azure Container Instance aus VS Code](./media/vscode-tools-for-ai/deploy.gif)](./media/vscode-tools-for-ai/deploy.gif#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Eine exemplarische Vorgehensweise zum Trainieren mit Machine Learning außerhalb von VS Code finden Sie in [Tutorial: Train models with Azure Machine Learning](tutorial-train-models-with-aml.md) (Tutorial: Trainieren von Modellen mit Azure Machine Learning).

Eine exemplarische Vorgehensweise zum lokalen Bearbeiten, Ausführen und Debuggen finden Sie im [Python-Hello World-Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)
