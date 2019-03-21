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
ms.openlocfilehash: 5e7e405bdb251bbebc5da81253eea2414b434cb8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57838311"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Erste Schritte mit Azure Machine Learning für Visual Studio Code

In diesem Artikel erfahren Sie, wie Sie die Erweiterung **Azure Machine Learning für Visual Studio Code** installieren und Ihr erstes Experiment mit Azure Machine Learning Service in Visual Studio Code (VS Code) erstellen.

Mit der Erweiterung Azure Machine Learning in Visual Studio Code können Sie Azure Machine Learning Service für die Vorbereitung Ihrer Daten, das Trainieren und Testen von Machine Learning-Modellen auf lokalen und Remotecomputezielen, das Bereitstellen dieser Modelle und das Verfolgen benutzerdefinierter Metriken und Experimente verwenden.

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

1. In Visual Studio Code wird eine Begrüßungsregisterkarte für die Erweiterung geöffnet, und das Azure-Symbol wird der Aktivitätsleiste hinzugefügt.

   ![Azure-Symbol auf der Aktivitätsleiste von Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. Klicken Sie im Dialogfeld auf **Anmelden**, und befolgen Sie die Aufforderung auf dem Bildschirm für die Authentifizierung bei Azure. 
   
   Die Erweiterung Azure Account, die zusammen mit der Erweiterung Azure Machine Learning für VS Code installiert wurde, hilft Ihnen bei der Authentifizierung bei Ihrem Azure-Konto. Weitere Informationen finden Sie in der Liste der Befehle auf der Seite der [Erweiterung Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Sehen Sie sich die [IntelliCode-Erweiterung für Visual Studio Code (Vorschauversion)](https://go.microsoft.com/fwlink/?linkid=2006060) an. IntelliCode bietet eine Reihe von KI-gestützten Funktionen für IntelliSense in Python, z.B. das Ableiten der relevantesten automatischen Vervollständigung basierend auf dem aktuellen Codekontext.

## <a name="azure-ml-sdk-installation"></a>Installation des Azure ML SDK

1. Stellen Sie sicher, dass Python 3.5 oder höher installiert ist und von VS Code erkannt wird. Wenn Sie es jetzt installieren, starten Sie VS Code neu, und wählen Sie anhand der Anweisungen unter https://code.visualstudio.com/docs/python/python-tutorial einen Python-Interpreter aus.

1. Geben Sie im integrierten Terminalfenster den zu verwendenden Python-Interpreter an, oder drücken Sie die **EINGABETASTE**, um Ihren Python-Standardinterpreter zu verwenden.

   ![Auswählen des Interpreters](./media/vscode-tools-for-ai/python.png)

1. Rechts unten im Fenster wird die Benachrichtigung eingeblendet, dass das Azure ML SDK automatisch installiert wird.    Es wird eine lokale private Python-Umgebung erstellt, die die Visual Studio Code-Voraussetzungen für die Arbeit mit Azure Machine Learning erfüllt.

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

### <a name="use-keyboard-shortcuts"></a>Verwenden von Tastenkombinationen

So wie die meisten Elemente von Visual Studio Code sind die Azure Machine Learning-Features in Visual Studio Code über die Tastatur zugänglich. Die wichtigste Tastenkombination, die Sie kennen sollten, ist STRG+UMSCHALT+P, mit der die Befehlspalette geöffnet wird. Von hier aus haben Sie Zugriff auf alle Funktionen von Visual Studio Code, einschließlich der Tastenkombinationen für die am häufigsten verwendeten Vorgänge.

[![Tastenkombinationen für Azure Machine Learning für VS Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Sie können nun Visual Studio Code für die Arbeit mit Azure Machine Learning verwenden.

Erfahren Sie, wie Sie [Computeziele erstellen und Modelle in Visual Studio Code trainieren und bereitstellen](how-to-vscode-train-deploy.md).
