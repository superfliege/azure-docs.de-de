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
ms.openlocfilehash: a2f37cffb37ce7008c3e372763784240e0d4250b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945551"
---
# <a name="vs-code-tools-for-ai-get-started-with-azure-machine-learning-from-visual-studio-code"></a>Visual Studio Code Tools for AI: erste Schritte mit Azure Machine Learning aus Visual Studio Code

In diesem Artikel erfahren Sie etwas über die Visual Studio Code (VS Code) **Tools for AI**-Erweiterung und die ersten Schritte beim Trainieren und Bereitstellen von Machine Learning- und Deep Learning-Modellen mit dem Azure Machine Learning-Dienst in Visual Studio Code.

Mit der Tools for AI-Erweiterung in Visual Studio Code können Sie den Azure Machine Learning-Dienst für die Vorbereitung Ihrer Daten, das Trainieren und Testen von ML-Modellen auf lokalen und Remotecomputezielen, das Bereitstellen dieser Modelle und das Verfolgen benutzerdefinierter Metriken und Experimente verwenden.

## <a name="prerequisite"></a>Voraussetzung

+ Visual Studio Code muss installiert sein. Visual Studio Code ist ein einfacher, aber leistungsstarker Quellcode-Editor, der auf Ihrem Desktop ausgeführt wird. Er bietet integrierte Unterstützung für Python und vieles mehr.  [Erfahren Sie, wie Sie VS Code installieren](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Installieren Sie Python 3.5 oder höher](https://www.anaconda.com/download/).

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="install-vs-code-tools-for-ai-extension"></a>Installieren der VS Code Tools for AI-Erweiterung

Beim Installieren der **Tools for AI**-Erweiterung werden automatisch zwei weitere Erweiterungen installiert (sofern Sie Zugriff auf das Internet haben). Es handelt sich um die [Azure-Kontoerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) und die [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)-Erweiterung.

Um mit Azure Machine Learning zu arbeiten, muss VS Code in eine Python-IDE umgestaltet werden. Für die Arbeit mit [Python in Visual Studio Code](https://code.visualstudio.com/docs/languages/python) ist die Microsoft Python-Erweiterung erforderlich, die mit Tools for AI automatisch installiert wird. Die Erweiterung macht Visual Studio Code zu einer ausgezeichneten IDE, und sie funktioniert auf jedem Betriebssystem und mit einer Vielzahl von Python-Interpretern. Sie nutzt das gesamte Leistungsspektrum von Visual Studio Code und bietet automatische Vervollständigung und IntelliSense, Linting, Debugging und Komponententests sowie die Möglichkeit, einfach zwischen Python-Umgebungen zu wechseln, einschließlich virtueller und Conda-Umgebungen. Eine exemplarische Vorgehensweise des Bearbeitens, Ausführens und Debuggens von Python-Code finden Sie im [Python-Hallo Welt-Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)

**So installieren Sie die VS Code Tools for AI-Erweiterung**

1. Starten Sie VS Code.

1. Navigieren Sie in einem Browser zu http://aka.ms/vscodetoolsforai. 

1. Klicken Sie auf dieser Webseite auf **Install** (Installieren). 

1. Klicken Sie auf der Registerkarte der Erweiterung auf **Install** (Installieren).

1. In Visual Studio Code wird eine Begrüßungsregisterkarte für die Erweiterung geöffnet, und das Azure-Symbol wird der Aktivitätsleiste hinzugefügt.

   ![Azure-Symbol auf der Aktivitätsleiste von Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. Klicken Sie im Dialogfeld auf **Anmelden**, und befolgen Sie die Aufforderung auf dem Bildschirm für die Authentifizierung bei Azure. 
   
   Die Erweiterung Azure Account, die zusammen mit Visual Studio Code Tools for AI installiert wurde, hilft Ihnen bei der Authentifizierung bei Ihrem Azure-Konto. Weitere Informationen finden Sie in der Liste der Befehle auf der Seite der [Erweiterung Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Sehen Sie sich die [IntelliCode-Erweiterung für Visual Studio Code (Vorschauversion)](https://go.microsoft.com/fwlink/?linkid=2006060) an. IntelliCode bietet eine Reihe von KI-gestützten Funktionen für IntelliSense in Python, z.B. das Ableiten der relevantesten automatischen Vervollständigung basierend auf dem aktuellen Codekontext.

## <a name="install-the-sdk"></a>Installieren des SDKs

1. Stellen Sie sicher, dass Python 3.5 oder höher installiert ist und von VS Code erkannt wird. Wenn Sie es jetzt installieren, starten Sie VS Code neu, und wählen Sie anhand der Anweisungen unter https://code.visualstudio.com/docs/python/python-tutorial einen Python-Interpreter aus.

1. Öffnen Sie in VS Code mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie „Install Azure ML SDK“ ein, um den pip-Installationsbefehl für das SDK zu suchen. Es wird eine lokale private Python-Umgebung erstellt, die die Visual Studio Code-Voraussetzungen für die Arbeit mit Azure Machine Learning erfüllt.
   ![Installieren](./media/vscode-tools-for-ai/install-sdk.png)

1. Geben Sie in dem integrierten Terminalfenster den zu verwendenden Python-Interpreter an, oder drücken Sie die **EINGABETASTE**, um Ihren Python-Standardinterpreter zu verwenden.

   ![Installieren](./media/vscode-tools-for-ai/python.png)

## <a name="get-started-with-azure-machine-learning"></a>Erste Schritte mit Azure Machine Learning

Bevor Sie mit dem Trainieren und Bereitstellen von ML-Modellen mithilfe von Visual Studio Code beginnen, müssen Sie einen [Arbeitsbereich für den Azure Machine Learning-Dienst](concept-azure-machine-learning-architecture.md#workspace) in der Cloud erstellen, der Ihre Modelle und Ressourcen enthalten soll. Erfahren Sie, wie Sie Ihr erstes Experiment in diesem Arbeitsbereich erstellen.

1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das Azure-Symbol. Die Seitenleiste für Azure Machine Learning wird angezeigt.

   ![Installieren](./media/vscode-tools-for-ai/createworkspace.gif)

1. Klicken Sie mit der rechten Maustaste auf Ihr Azure-Abonnement, und wählen Sie **Arbeitsbereich erstellen** aus. Eine Liste wird angezeigt. In dem animierten Bild ist der Name des Abonnements „OpenMind Studio“, und der Arbeitsbereich lautet „MyWorkspace“. 

1. Wählen Sie in der Liste eine vorhandene Ressourcengruppe aus, oder erstellen Sie mithilfe des Assistenten in der Befehlspalette eine neue.

1. Geben Sie im Feld einen eindeutigen und klaren Namen für Ihren neuen Arbeitsbereich ein. In den Screenshots hat der Arbeitsbereich den Namen „MyWorkspace“.

1. Drücken Sie die EINGABETASTE, um den neuen Arbeitsbereich zu erstellen. Er wird dann in der Struktur unter dem Namen des Abonnements angezeigt.

1. Klicken Sie mit der rechten Maustaste auf den Namen des Arbeitsbereichs, und wählen Sie im Kontextmenü **Experiment erstellen** aus.  Für Experimente werden Ihre Ausführungen mit Azure Machine Learning gespeichert.

1. Geben Sie in dem Feld einen Namen für das Experiment ein. In den Screenshots hat das Experiment den Namen „MNIST“.
 
1. Drücken Sie die EINGABETASTE, um das neue Experiment zu erstellen. Es wird dann in der Struktur unter dem Namen des Arbeitsbereichs angezeigt.

1. Klicken Sie mit der rechten Maustaste auf den Namen des Experiments, und wählen Sie **Attach a local folder** (Lokalen Ordner anfügen) aus. Dieser Ordner sollte Ihre lokalen Python-Skripts enthalten. Der Ordner wird dann mit dem Experiment in der Cloud verknüpft. 

   Nun wird jedes Ihrer Experiment vollständig ausgeführt, sodass alle wichtigen Metriken im Experimentverlauf gespeichert werden. Die trainierten Modelle werden zudem automatisch in Azure Machine Learning hochgeladen und mit den Metriken und Protokollen Ihres Experiments gespeichert.

   [![Hinzufügen eines Ordners in VS Code](./media/vscode-tools-for-ai/attachfolder.gif)](./media/vscode-tools-for-ai/attachfolder.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Verwenden von Tastenkombinationen

So wie die meisten Elemente von Visual Studio Code sind die Azure Machine Learning-Features in Visual Studio Code über die Tastatur zugänglich. Die wichtigste Tastenkombination, die Sie kennen sollten, ist STRG+UMSCHALT+P, mit der die Befehlspalette geöffnet wird. Von hier aus haben Sie Zugriff auf alle Funktionen von Visual Studio Code, einschließlich der Tastenkombinationen für die am häufigsten verwendeten Vorgänge.

[![Tastenkombinationen für Visual Studio Code Tools for AI](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Sie können nun Visual Studio Code für die Arbeit mit Azure Machine Learning verwenden.

Erfahren Sie, wie Sie [Computeziele erstellen und Modelle in Visual Studio Code trainieren und bereitstellen](how-to-vscode-train-deploy.md).