---
title: Trainieren und Bereitstellen von Modellen aus VS Code
titleSuffix: Azure Machine Learning service
description: Erfahren Sie mehr über Azure Machine Learning für Visual Studio Code und wie Sie mit dem Training und der Bereitstellung von Machine Learning und Deep Learning-Modellen in Azure Machine Learning Service mit Visual Studio Code beginnen können.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 0910cce92c410a68dce6e2c44d29e72e594cd153
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271707"
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

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Azure Machine Learning-Seitenleiste wird angezeigt.

2. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts. Im animierten Bild ist der Name des Abonnements „Free Trial“, und der Arbeitsbereich lautet „TeamWorkspace“. 

3. Klicken Sie unter dem Arbeitsbereichsknoten mit der rechten Maustaste auf den Knoten **Compute**, und wählen Sie **Create Compute** (Compute erstellen) aus.

4. Wählen Sie den Computezieltyp in der Liste aus. 

5. Geben Sie alle erweiterten Eigenschaften in der JSON-Konfigurationsdatei an, die in einer neuen Registerkarte geöffnet wird. Sie können einen eindeutigen Namen für das Computeziel in dieser Datei angeben.

6. Wenn Sie das Konfigurieren Ihres Computeziels abgeschlossen haben, klicken Sie unten rechts auf **Absenden**.

Hier ist ein Beispiel für Azure Machine Learning Compute (AMLCompute): [![Erstellen von AML-Compute in VS Code](./media/vscode-tools-for-ai/CreateAMLCompute.gif)](./media/vscode-tools-for-ai/CreateAMLCompute.gif#lightbox)

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

Verwenden Sie Azure Machine Learning für VS Code (Vorschau), um Ihren Code schnell zu durchlaufen, ihn schrittweise auszuführen und zu debuggen, und Ihre bevorzugte Quellcodeverwaltung zu verwenden. 

**So führen Sie Ihre Experiment mit Azure Machine Learning aus:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Azure Machine Learning-Seitenleiste wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts. 

1. Erweitern Sie unter dem Knoten „Arbeitsbereich“ den Knoten **Compute**, und klicken Sie mit der rechten Maustaste auf die **Ausführungskonfiguration** der Computeressource, die Sie verwenden möchten. 

1. Wählen Sie **Run Experiment** (Experiment ausführen) aus.

1. Klicken Sie auf **View Experiment Run** (Experimentausführung anzeigen), um das integrierte Azure Machine Learning-Portal anzuzeigen, in dem Sie Ihre Ausführungen überwachen und Ihre trainierten Modelle anzeigen können.

## <a name="deploy-and-manage-models"></a>Erstellen und Verwalten von Modellen
Azure Machine Learning ermöglicht das Bereitstellen und Verwalten Ihrer Machine Learning-Modelle in der Cloud und auf dem Edge. 

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Registrieren Ihres Modells bei Azure Machine Learning aus VS Code

Nachdem Sie Ihr Modell nun trainiert haben, können Sie es in Ihrem Arbeitsbereich registrieren.
Registrierte Modelle können nachverfolgt und bereitgestellt werden.

**So registrieren Sie Ihr Modell:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Azure Machine Learning-Seitenleiste wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Arbeitsbereich des Azure Machine Learning-Diensts.

1. Klicken Sie unter dem Knoten „Arbeitsbereich“ mit der rechten Maustaste auf **Modelle**, und wählen Sie **Register Model** (Modell registrieren) aus.

1. Wählen Sie in der Liste aus, ob Sie eine **Modelldatei** (für einzelne Modelle) oder einen **Modellordner** (für Modelle mit mehreren Dateien, wie etwa Tensorflow) hochladen möchten. 

1. Wählen Sie Ihren Ordner oder die Datei aus.

1. Wenn Sie die Konfiguration Ihrer Modelleigenschaften abgeschlossen haben, klicken Sie unten rechts auf **Absenden**. 



### <a name="deploy-your-service-from-vs-code"></a>Bereitstellen Ihres Diensts aus VS Code

Mithilfe von VS Code können Sie Ihren Webdienst an diesen Orten bereitstellen:
+ Azure Container Instance (ACI): zum Testen
+ Azure Kubernetes Service (AKS): zur Produktion 

Sie brauchen für das Testen nicht vorab einen ACI-Container zu erstellen, da sie dynamisch erstellt werden. Allerdings müssen AKS-Cluster im Voraus konfiguriert werden. 

Erfahren Sie mehr über [Bereitstellung mit Azure Machine Learning](how-to-deploy-and-where.md) im Allgemeinen.

**So stellen Sie einen Webdienst bereit:**

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Azure Machine Learning-Seitenleiste wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und Ihren Arbeitsbereich des Azure Machine Learning-Diensts.

1. Erweitern Sie unter dem Arbeitsbereich-Knoten den Knoten **Modelle**.

1. Klicken Sie mit der rechten Maustaste auf das Modell, das Sie bereitstellen möchten, und wählen Sie im Kontextmenü den Befehl **Deploy Service from Registered Model** (Dienst aus registriertem Modell bereitstellen) aus.

1. Wählen Sie in der Befehlspalette in der Liste das Computeziel aus, auf dem die Bereitstellung erfolgen soll. 

1. Geben Sie im Feld einen Namen für den Dienst ein. 

1. Drücken Sie in der Befehlspalette die EINGABETASTE auf Ihrer Tastatur, um die Optionen zu durchsuchen und die Skriptdatei auszuwählen.

1. Drücken Sie in der Befehlspalette die EINGABETASTE auf Ihrer Tastatur, um die Optionen zu durchsuchen und die Conda-Abhängigkeitsdatei auszuwählen.

1. Wenn Sie die Konfiguration Ihrer Diensteigenschaften abgeschlossen haben, klicken Sie unten rechts auf **Absenden**. In dieser Diensteigenschaftendatei können Sie eine lokale Docker-Datei oder eine Datei „schema.json“ angeben, die Sie verwenden möchten.

Der Webdienst ist nun bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Eine exemplarische Vorgehensweise zum Trainieren mit Machine Learning außerhalb von VS Code finden Sie in [Tutorial: Trainieren von Modellen mit Azure Machine Learning](tutorial-train-models-with-aml.md).

Eine exemplarische Vorgehensweise zum lokalen Bearbeiten, Ausführen und Debuggen finden Sie im [Python-Hello World-Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)
