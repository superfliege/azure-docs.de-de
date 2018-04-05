---
title: Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge | Microsoft-Dokumentation
description: Debuggen von Azure Functions (C#) mit Azure IoT Edge in VS Code
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 3/20/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 8da16ffe72ad265f0201c2fe7e00e585dfa255e8
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge

Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio Code](https://code.visualstudio.com/) als Hauptentwicklungstool zum Debuggen von Azure Functions in IoT Edge.

## <a name="prerequisites"></a>Voraussetzungen
Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer mit Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

Stellen Sie sicher, dass Sie folgende Tutorials abgeschlossen haben, bevor Sie mit diesem Leitfaden beginnen.
- [Develop an IoT Edge solution with multiple modules in Visual Studio Code](tutorial-multiple-modules-in-vscode.md) (Entwickeln einer IoT Edge-Lösung mit mehreren Modulen in Visual Studio Code)

Nach Abschluss des vorherigen Tutorials sollten Sie die folgenden Elemente vorbereitet haben.
- Eine lokale Docker-Registrierung, die auf dem Entwicklungscomputer ausgeführt wird. Es wird empfohlen, eine lokale Docker-Registrierung als Prototyp bzw. für Testzwecke zu verwenden. Sie können die Containerregistrierung in der `module.json`-Datei in jedem Modulordner aktualisieren.
- Ein Projektarbeitsbereich einer IoT Edge-Lösung mit einem Azure-Funktionsmodul-Unterordner darin.
- Die `run.csx`-Datei mit Ihrem Funktionscode.
- Eine Edge-Runtime, die auf dem Entwicklungscomputer ausgeführt wird.

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Erstellen Ihres IoT Edge-Funktionsmoduls zum Debuggen
1. Um mit dem Debuggen zu beginnen, müssen Sie mithilfe von **Dockerfile.amd64.debug** Ihr Dockerimage neu erstellen und Ihre Edge-Lösung erneut bereitstellen. Navigieren Sie im VS Code-Explorer zur `deployment.template.json`-Datei. Aktualisieren Sie Ihre Funktionsimage-URL, indem Sie am Ende `.debug` hinzufügen.

    ![Erstellen eines Debugimage](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Erstellen Sie Ihre Lösung neu. Geben Sie in der VS Code-Befehlspalette den Befehl **Edge: IoT Edge-Lösung neu erstellen** ein, und führen Sie diesen aus.

3. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Bereitstellung für Edge-Gerät erstellen**. Wählen Sie `deployment.json` unterhalb des `config`-Ordners aus. Dann können Sie sehen, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im Terminal erstellt wurde, der in VS Code integriert ist.

> [!NOTE]
> Sie können Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker images` im Terminal überprüfen.

## <a name="start-debugging-c-function-in-vs-code"></a>Starten des Debugging von C#-Funktion in VS Code
1. VS Code speichert die Konfigurationsinformationen für das Debugging in einer `launch.json`-Datei, die sich in einem `.vscode`-Ordner in Ihrem Arbeitsbereich befindet. Diese `launch.json`-Datei wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Und sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, das Debugging unterstützt. Navigieren Sie zur Debugansicht, und wählen Sie die entsprechende Debugkonfigurationsdatei aus.
    ![Auswählen der Debugkonfiguration](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navigieren Sie zu `run.csx`. Fügen Sie in der Funktion einen Haltepunkt hinzu.

3. Klicken Sie auf Schaltfläche „Debuggen starten“, oder drücken Sie die Taste **F5**, und wählen Sie den Prozess aus, an den die Anfügung erfolgen soll.

4. In der Debugansicht von Visual Studio Code können Sie die Variablen im linken Bereich sehen. 


> [!NOTE]
> Das obige Beispiel veranschaulicht das Debuggen von .NET Core IoT Edge-Funktionen in Containern. Es basiert auf der Debugversion von `Dockerfile.amd64.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Es wird empfohlen, für produktionsbereite IoT Edge-Module `Dockerfile` ohne VSDBG direkt zu verwenden oder anzupassen, nachdem Sie das Debugging Ihrer C#-Module abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Funktion erstellt und zu Debugzwecken in IoT Edge bereitgestellt. Anschließend haben Sie mit dem Debuggen in VS Code begonnen. Sie können mit einem der beiden folgenden Tutorials fortfahren, um andere Szenarien bei der Entwicklung von Azure IoT Edge in VSCode kennenzulernen. 

> [!div class="nextstepaction"]
> [Develop an IoT Edge solution with multiple modules in Visual Studio Code](tutorial-multiple-modules-in-vscode.md) (Entwickeln einer IoT Edge-Lösung mit mehreren Modulen in Visual Studio Code)

