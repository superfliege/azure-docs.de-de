---
title: Verwenden von Visual Studio Code mit Azure Machine Learning
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie Azure Machine Learning für Visual Studio Code installieren und ein einfaches Experiment mit Azure Machine Learning erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: bbeb0dbbd5e9c919eda4b298dc5bee31965e9bac
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995250"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Erste Schritte mit Azure Machine Learning für Visual Studio Code

In diesem Artikel erfahren Sie, wie Sie die Erweiterung **Azure Machine Learning für Visual Studio Code** verwenden können, um Machine Learning und Deep Learning-Modelle mit Azure Machine Learning Service in Visual Studio Code (VS Code) zu trainieren und bereitzustellen.

Azure Machine Learning Service bietet Unterstützung für die Ausführung von Experimenten, lokal und auf Remotecomputezielen. Für jedes Experiment können Sie mehrere Ausführungen nachverfolgen, so oft wie Sie verschiedene Techniken, Hyperparameter und mehr iterativ ausprobieren müssen. Sie können Azure Machine Learning verwenden, um benutzerdefinierte Metriken und experimentelle Läufe nachzuverfolgen, wodurch Sie Reproduzierbarkeit und Überwachung im Sinne von Data Science gewinnen.

Und Sie können diese Modelle für Ihre Test- und Produktionsanforderungen bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie heute die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

+ Visual Studio Code muss installiert sein. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor, der auf Ihrem Desktop ausgeführt wird. Er bietet integrierte Unterstützung für Python und vieles mehr.  [Erfahren Sie, wie Sie VS Code installieren](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Installieren Sie Python 3.5 oder höher](https://www.anaconda.com/download/).


## <a name="install-the-azure-machine-learning-for-vs-code-extension"></a>Installieren der Erweiterung Azure Machine Learning für Visual Studio Code

Beim Installieren der **Azure Machine Learning**-Erweiterung werden automatisch zwei weitere Erweiterungen installiert (sofern Sie Zugriff auf das Internet haben). Es handelt sich um die [Azure-Kontoerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) und die [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)-Erweiterung.

Um mit Azure Machine Learning zu arbeiten, muss VS Code in eine Python-IDE umgestaltet werden. Für die Arbeit mit [Python in Visual Studio Code](https://code.visualstudio.com/docs/languages/python) ist die Microsoft Python-Erweiterung erforderlich, die mit der Erweiterung Azure Machine Learning automatisch installiert wird. Die Erweiterung macht Visual Studio Code zu einer ausgezeichneten IDE, und sie funktioniert auf jedem Betriebssystem und mit einer Vielzahl von Python-Interpretern. Sie nutzt das gesamte Leistungsspektrum von Visual Studio Code und bietet automatische Vervollständigung und IntelliSense, Linting, Debugging und Komponententests sowie die Möglichkeit, einfach zwischen Python-Umgebungen zu wechseln, einschließlich virtueller und Conda-Umgebungen. Eine exemplarische Vorgehensweise des Bearbeitens, Ausführens und Debuggens von Python-Code finden Sie im [Python-Hallo Welt-Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)

**So installieren Sie die Erweiterung Azure Machine Learning:**

1. Starten Sie VS Code.

1. Navigieren Sie in einem Browser zu: Erweiterung [Azure Machine Learning für Visual Studio Code (Vorschau)](https://aka.ms/vscodetoolsforai)

1. Klicken Sie auf dieser Webseite auf **Install** (Installieren). 

1. Klicken Sie auf der Registerkarte der Erweiterung auf **Install** (Installieren).

1. In Visual Studio Code wird eine Begrüßungsregisterkarte für die Erweiterung geöffnet, und das Azure-Symbol (im Bild unten im roten Feld beschrieben) wird der Aktivitätsleiste hinzugefügt.

   ![Azure-Symbol auf der Aktivitätsleiste von Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. Klicken Sie im Dialogfeld auf **Anmelden**, und befolgen Sie die Aufforderung auf dem Bildschirm für die Authentifizierung bei Azure. 
   
   Die Erweiterung Azure Account, die zusammen mit der Erweiterung Azure Machine Learning für VS Code installiert wurde, hilft Ihnen bei der Authentifizierung bei Ihrem Azure-Konto. Weitere Informationen finden Sie in der Liste der Befehle auf der Seite der [Erweiterung Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Sehen Sie sich die [IntelliCode-Erweiterung für Visual Studio Code (Vorschauversion)](https://go.microsoft.com/fwlink/?linkid=2006060) an. IntelliCode bietet eine Reihe von KI-gestützten Funktionen für IntelliSense in Python, z.B. das Ableiten der relevantesten automatischen Vervollständigung basierend auf dem aktuellen Codekontext.

## <a name="azure-ml-sdk-installation"></a>Installation des Azure ML SDK

1. Stellen Sie sicher, dass Python 3.5 oder höher installiert ist und von VS Code erkannt wird. Wenn Sie es jetzt installieren, starten Sie VS Code neu, und wählen Sie anhand der Anweisungen unter https://code.visualstudio.com/docs/python/python-tutorial einen Python-Interpreter aus.

1. Geben Sie im integrierten Terminalfenster den zu verwendenden Python-Interpreter an, oder drücken Sie die **EINGABETASTE**, um Ihren Python-Standardinterpreter zu verwenden.

   ![Auswählen des Interpreters](./media/vscode-tools-for-ai/python.png)

1. Rechts unten im Fenster wird die Benachrichtigung eingeblendet, dass das Azure ML SDK automatisch installiert wird.    Es wird eine lokale private Python-Umgebung erstellt, die die Visual Studio Code-Voraussetzungen für die Arbeit mit dem Azure Machine Learning Service erfüllt.

   ![Installieren des Azure Machine Learning SDK für Python](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Erste Schritte mit Azure Machine Learning

Bevor Sie mit dem Trainieren und Bereitstellen von Machine Learning-Modellen mithilfe von Visual Studio Code beginnen, müssen Sie einen [Arbeitsbereich für Azure Machine Learning Service](concept-azure-machine-learning-architecture.md#workspace) in der Cloud erstellen, der Ihre Modelle und Ressourcen enthalten soll. Erfahren Sie, wie Sie Ihr erstes Experiment in diesem Arbeitsbereich erstellen.

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

   [![Installieren](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Klicken Sie mit der rechten Maustaste auf Ihr Azure-Abonnement, und wählen Sie **Arbeitsbereich erstellen** aus. Eine Liste wird angezeigt. Im animierten Bild ist der Name des Abonnements „Free Trial“, und der Arbeitsbereich lautet „TeamWorkspace“. 

1. Wählen Sie in der Liste eine vorhandene Ressourcengruppe aus, oder erstellen Sie mithilfe des Assistenten in der Befehlspalette eine neue Ressourcengruppe.

1. Geben Sie im Feld einen eindeutigen und klaren Namen für Ihren neuen Arbeitsbereich ein. In den Screenshots trägt der Arbeitsbereich den Namen „TeamWorkspace“.

1. Drücken Sie die EINGABETASTE, um den neuen Arbeitsbereich zu erstellen. Er wird dann in der Struktur unter dem Namen des Abonnements angezeigt.

1. Klicken Sie mit der rechten Maustaste auf den Knoten „Experiment“, und wählen Sie dann im Kontextmenü **Experiment erstellen** aus.  Für Experimente werden Ihre Ausführungen mit Azure Machine Learning gespeichert.

1. Geben Sie in dem Feld einen Namen für das Experiment ein. In den Screenshots hat das Experiment den Namen „MNIST“.
 
1. Drücken Sie die EINGABETASTE, um das neue Experiment zu erstellen. Es wird dann in der Struktur unter dem Namen des Arbeitsbereichs angezeigt.

1. Sie können mit der rechten Maustaste auf ein Experiment in einem Arbeitsbereich klicken und „Set as Active Experiment“ (Als aktives Experiment festlegen) auswählen. Das **aktive** Experiment ist das Experiment, das Sie aktuell verwenden, und der geöffnete Ordner in VS Code wird mit diesem Experiment in der Cloud verknüpft. Dieser Ordner sollte Ihre lokalen Python-Skripts enthalten.

   Nun wird jedes Ihrer Experimente vollständig ausgeführt, sodass alle wichtigen Metriken im Experimentverlauf gespeichert werden. Die trainierten Modelle werden zudem automatisch in Azure Machine Learning hochgeladen und mit den Metriken und Protokollen Ihres Experiments gespeichert.

   [![Hinzufügen eines Ordners in VS Code](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)


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

Die VS Code-Erweiterung erstellt automatisch ein lokales Computeziel und Laufzeitkonfigurationen für Ihre **lokalen** und **Docker-basierten** Umgebungen auf Ihrem lokalen Computer. Die Laufzeitkonfigurationsdateien befinden sich unter dem zugeordneten Computezielknoten. 

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

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2
- tensorflow=1.12.0

- pip:
    # Required packages for AzureML execution, history, and data preparation.

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

### <a name="use-keyboard-shortcuts"></a>Verwenden von Tastenkombinationen

So wie die meisten Elemente von Visual Studio Code sind die Azure Machine Learning-Features in Visual Studio Code über die Tastatur zugänglich. Die wichtigste Tastenkombination, die Sie kennen sollten, ist STRG+UMSCHALT+P, mit der die Befehlspalette geöffnet wird. Von hier aus haben Sie Zugriff auf alle Funktionen von Visual Studio Code, einschließlich der Tastenkombinationen für die am häufigsten verwendeten Vorgänge.

[![Tastenkombinationen für Azure Machine Learning für VS Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Eine exemplarische Vorgehensweise zum Trainieren mit Machine Learning außerhalb von VS Code finden Sie in [Tutorial: Trainieren von Modellen mit Azure Machine Learning](tutorial-train-models-with-aml.md).

Eine exemplarische Vorgehensweise zum lokalen Bearbeiten, Ausführen und Debuggen finden Sie im [Python-Hello World-Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)
