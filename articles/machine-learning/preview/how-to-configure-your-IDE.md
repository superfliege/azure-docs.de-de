---
title: Konfigurieren von Azure Machine Learning Workbench zum Arbeiten mit einer IDE  | Microsoft Docs
description: Ein Handbuch zum Konfigurieren von Azure Machine Learning Workbench zum Arbeiten mit einer IDE.
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/05/2017
ms.openlocfilehash: 4e18a413a0559b1ddebecf1b29722d21ef35c337
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-azure-machine-learning-workbench-to-work-with-an-ide"></a>Konfigurieren von Azure Machine Learning Workbench zum Arbeiten mit einer IDE 

Azure Machine Learning Workbench kann zum Arbeiten mit beliebten Python-IDEs (Integrated Development Environment, integrierte Entwicklungsumgebung) konfiguriert werden. Sie ermöglicht bei der Data Science-Entwicklung einen reibungslosen Wechsel zwischen Datenvorbereitung, Erstellen von Code, Nachverfolgung und Operationalisierung. Derzeit werden die folgenden IDEs unterstützt:
- Microsoft Visual Studio Code 
- JetBrain PyCharm 

## <a name="configure-workbench"></a>Konfigurieren der Workbench
1. Klicken Sie in der oberen linken Ecke der App auf das Menü **File**. 
2. Wählen Sie aus dem Flyout die Option **Configure Project IDE**. 
3. Geben Sie `VS Code` oder `PyCharm` in das Feld **Name** ein (der Name ist willkürlich gewählt).
4. Geben Sie den Speicherort der ausführbaren IDE (vollständig mit den Namen der ausführbaren Datei und der Erweiterung) in **Execution Path** ein.

### <a name="default-install-path-for-visual-studio-code"></a>Standardinstallationspfad für Visual Studio Code  

* Windows 32-Bit – `C:\Program Files (x86)\Microsoft VS Code\Code.exe`
* Windows 64-Bit – `C:\Program Files\Microsoft VS Code\Code.exe`
* macOS – wählen Sie den .app-Pfad, z.B. `/Applications/Visual Studio Code.app`, und die App fügt den Rest des Pfads für Sie an. Der vollständige Pfad zur ausführbaren Datei ist standardmäßig `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code`. Wenn Sie den `Shell Command: Install 'code' command in PATH`-Befehl in Visual Studio Code ausgeführt haben, können Sie auch unter `/usr/local/bin/code` auf das Visual Studio Code-Skript verweisen.

### <a name="default-install-path-for-pycharm"></a>Standardinstallationspfad für PyCharm 

* Windows 32-Bit – `C:\Program Files (x86)\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm.exe`. 
* Windows 64-Bit – `C:\Program Files\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm64.exe`.
* macOS – wählen Sie den .app-Pfad, z.B. „/Applications/PyCharm CE.app“, und die App fügt den Rest des Pfads für Sie an. Der vollständige Pfad zur ausführbaren Datei ist standardmäßig `/Applications/PyCharm CE.app/Contents/MacOS/pycharm`. Möglicherweise finden Sie PyCharm auch im bin-Ordner, `/usr/local/bin/charm`.

## <a name="open-project-in-ide"></a>Öffnen des Projekts in der IDE 
Sobald die Konfiguration abgeschlossen ist, können Sie ein Azure Machine Learning-Projekt öffnen. Öffnen Sie dazu in Azure Machine Learning Workbench das Menü **File**, und klicken Sie dann auf **Open Project (<IDE-Name>)**. Diese Aktion öffnet das aktuell aktive Projekt in der konfigurierten IDE.  _Hinweis: Wenn Sie sich nicht in einem Projekt befinden, ist **Open Project (<IDE-Name>)** deaktiviert._

## <a name="configuring-the-integrated-terminal-in-visual-studio-code"></a>Konfigurieren des integrierten Terminals in Visual Studio Code

### <a name="windows"></a>Windows 
Wir haben die Standardshell überschrieben, sodass sie Cmd anstelle von PowerShell ist. Bei Klicken auf **Open Project (<IDE_Name>)** wird eine Eingabeaufforderung angezeigt: 

_Do you allow shell: `C:\windows\System32\cmd.exe` (defined as a workspace setting) to be launched in the terminal?_

Antworten Sie mit `yes`, um die Konfiguration der Shell zur nahtlosen Zusammenarbeit mit der Befehlszeilenschnittstelle der Azure ML Workbench zu ermöglichen.

### <a name="mac"></a>Mac
Zum Ausführen eines `az`-Befehls mithilfe des integrierten Terminals von Visual Studio Code auf dem Mac müssen Sie für `PATH` manuell den gleichen Wert festlegen wie für `PATH` in der Datei `.vscode/settings.json` des Projekts unter dem Schlüssel `terminal.integrated.env.osx`. Führen Sie dazu im Terminal den folgenden Befehl aus: `PATH=<PATH in .vscode/settings>`
