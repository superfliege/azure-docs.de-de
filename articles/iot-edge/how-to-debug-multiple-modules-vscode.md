---
title: Debuggen mehrerer Module für Azure IoT Edge in Visual Studio Code | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Debuggen mehrerer Module mit Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 38c66129ac8244d18b0f94c1485c785015e29ab4
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049590"
---
# <a name="use-visual-studio-code-to-debug-multiple-modules-with-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Debuggen mehrerer Module mit Azure IoT Edge
Dieser Artikel enthält detaillierte Anweisungen zum Verwenden von [Visual Studio Code](https://code.visualstudio.com/) zum Debuggen mehrerer Module bei IoT Edge.

## <a name="prerequisites"></a>Voraussetzungen
Schließen Sie das Tutorial [Entwickeln einer IoT Edge-Projektmappe mit mehreren Modulen in Visual Studio Code](tutorial-multiple-modules-in-vscode.md) ab, und stellen Sie sicher, dass auf Ihrem IoT Edge-Gerät mindestens zwei Module ausgeführt werden.

## <a name="multi-target-and-remote-debugging-in-vs-code"></a>Mehrere Ziele und Remotedebuggen in Visual Studio Code
Mit Visual Studio Code und der Azure IoT Edge-Erweiterung können Sie den Modulprozess an einen Container anfügen, unabhängig davon, ob der Container auf Ihrem Entwicklungscomputer oder einem physischen IoT Edge-Remotegerät ausgeführt wird. Beim Debuggen mehrerer in Containern ausgeführter Module werden tatsächlich mehrere Prozesse an separate Containern angefügt. Das [Debuggen mehrerer Ziele](https://code.visualstudio.com/docs/editor/debugging#_multitarget-debugging) in Visual Studio Code kann zum Debuggen mehrerer Module verwendet werden.

   > [!TIP]
   > Wenn Ihr Modulcontainer auf einem physischen IoT Edge-Remotegerät ausgeführt wird, müssen Sie eventuell [Docker-Computer](https://docs.docker.com/machine/overview/) einrichten, damit das Docker-Modul auf dem Entwicklungscomputer mit den Docker-Remotehosts kommunizieren kann.

### <a name="build-your-iot-edge-modules-for-debugging-purpose"></a>Erstellen Ihrer IoT Edge-Module zum Debuggen
1. Um mit dem Debuggen mehrerer Module zu beginnen, müssen Sie mithilfe von **Dockerfile.amd64.debug** Ihr Docker-Image neu erstellen und Ihre Edge-Projektmappe erneut bereitstellen. Navigieren Sie im VS Code-Explorer zur Datei `deployment.template.json`. Aktualisieren Sie Ihre Image-URLs, indem Sie am Ende `.debug` hinzufügen. Sie benötigen mindestens zwei Modulimages mit `.debug`. Wenn Sie mit der Projektmappe aus dem vorherigen Tutorial arbeiten, sollten Sie ein C#-Functions-Modul und ein C#-Modul haben. Aktualisieren Sie diese beiden Image-URLs, indem Sie `.debug` am Ende hinzufügen, und speichern Sie diese Datei. 
2. Erstellen Sie Ihre Lösung neu. Geben Sie in der Visual Studio Code-Befehlspalette den Befehl **Azure IoT Edge: Build IoT Edge solution** ein, und führen Sie ihn aus.
3. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Create deployment for Edge device** (Bereitstellung für Edge-Gerät erstellen) aus. Wählen Sie die Datei `deployment.json` unter dem Ordner `config` aus. Sie können nun sehen, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Sie können Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker ps` im Terminal überprüfen.

### <a name="start-debugging-c-function-in-vs-code"></a>Starten des Debugging von C#-Funktion in VS Code
1. VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird. Navigieren Sie zur Debugansicht, und wählen Sie die entsprechende Debugkonfigurationsdatei für das Remotedebuggen des C#-Functions-Moduls aus.
2. Navigieren Sie zu `run.csx`. Fügen Sie in der Funktion einen Haltepunkt hinzu.
3. Klicken Sie auf Schaltfläche **Debuggen starten**, oder drücken Sie **F5**, und wählen Sie den gewünschten Prozess für die Anfügung aus.
4. In der Debugansicht von VS Code können Sie die Variablen im linken Bereich sehen. 

### <a name="start-debugging-c-module-at-the-same-time-in-vs-code"></a>Gleichzeitiges Beginnen des Debuggens des C#-Moduls in VS Code
1. Geben Sie in der Befehlspalette von Visual Studio Code den Befehl „Workspace: Duplicate Workspace in New Window“ ein, und führen Sie ihn aus. Ein neues Visual Studio Code-Fenster wird im selben Arbeitsbereich geöffnet.
2. Navigieren Sie zur Debugansicht, und wählen Sie die Debugkonfigurationsdatei für das Remotedebugging des C#-Moduls aus.
3. Navigieren Sie zu `program.cs`. Fügen Sie in im C#-Modul einen Breakpoint hinzu.
4. Klicken Sie auf Schaltfläche **Debuggen starten**, oder drücken Sie **F5**, und wählen Sie den gewünschten Prozess für die Anfügung aus.
5. In der Debugansicht von VS Code können Sie die Variablen im linken Bereich sehen. 

### <a name="see-variables-in-multiple-debugging-windows"></a>Anzeigen von Variablen in mehreren Debugfenstern
1. Nun verfügen Sie über mindestens zwei Debugsitzungen in zwei Visual Studio Code-Fenstern. Einer der Breakpoints sollte erreicht werden.
2. Drücken Sie `F10`, oder klicken Sie auf die Schaltfläche zum Überspringen in der **Debugsymbolleiste**.
3. Der Breakpoint in einem anderen Visual Studio Code-Fenster sollte erreicht werden. 
4. Wenn Sie mit den beiden obigen Schritte fortfahren, werden Variablen aus mehreren Modulen in mehreren Debugfenstern von Visual Studio Code angezeigt.

> [!NOTE]
> Das vorherige Beispiel zeigt das Debuggen mehrerer Module mit Azure IoT Edge. Es basiert auf der Debugversion von `Dockerfile.amd64.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Es wird empfohlen, für produktionsbereite IoT Edge-Module `Dockerfile` ohne VSDBG direkt zu verwenden oder anzupassen, nachdem Sie das Debugging Ihrer C#-Module abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).
