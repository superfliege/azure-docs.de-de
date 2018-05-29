---
title: Debuggen von C#-Modulen mit Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden Sie Visual Studio Code, um ein C#-Modul mit Azure IoT Edge in Visual Studio Code zu debuggen.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 035cb129bc3933c10f430b593226108f5d160972
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34165722"
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Debuggen eines C#-Moduls mit Azure IoT Edge
Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio (VS) Code](https://code.visualstudio.com/) als Hauptentwicklungstool zum Debuggen Ihrer Azure IoT Edge-Module.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

> [!NOTE]
> Sie können nur C#-Module in Containern vom Typ „linux-amd64“ debuggen.

Führen Sie die Schritte unter [Entwickeln einer IoT Edge-Projektmappe mit mehreren Modulen in Visual Studio Code](tutorial-multiple-modules-in-vscode.md) aus, bevor Sie die Anleitung in diesem Artikel befolgen. Danach sollten folgende Elemente bereitstehen:
- Eine lokale Docker-Registrierung, die auf dem Entwicklungscomputer ausgeführt wird. Es wird empfohlen, eine lokale Docker-Registrierung als Prototyp bzw. für Testzwecke zu verwenden. Sie können die Containerregistrierung in der Datei `module.json` in jedem Modulordner aktualisieren.
- Einen Projektarbeitsbereich einer IoT Edge-Lösung mit einem C#-Modul-Unterordner darin.
- Die Datei `Program.cs` mit dem neuesten Modulcode.
- Eine Edge-Runtime, die auf dem Entwicklungscomputer ausgeführt wird.

## <a name="build-your-iot-edge-c-module-for-debugging"></a>Erstellen des IoT Edge-C#-Moduls zum Debuggen
1. Um mit dem Debuggen zu beginnen, müssen Sie mithilfe von **Dockerfile.amd64.debug** Ihr Dockerimage neu erstellen und Ihre Edge-Lösung erneut bereitstellen. Navigieren Sie im VS Code-Explorer zur Datei `deployment.template.json`. Aktualisieren Sie Ihre Funktionsimage-URL, indem Sie am Ende `.debug` hinzufügen.

2. Erstellen Sie Ihre Lösung neu. Geben Sie in der VS Code-Befehlspalette den Befehl **Edge: IoT Edge-Projektmappe erstellen** ein, und führen Sie ihn aus.

3. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Create deployment for Edge device** (Bereitstellung für Edge-Gerät erstellen) aus. Wählen Sie die Datei `deployment.json` im Ordner `config` aus. Sie können nun sehen, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Sie können Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker images` im Terminal überprüfen.

## <a name="start-debugging-c-module-in-vs-code"></a>Debuggen des C#-Moduls in VS Code
1. VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul mit Debuggingunterstützung hinzufügen. Navigieren Sie zur Debugansicht, und wählen Sie die entsprechende Debugkonfigurationsdatei aus.
    ![Auswählen der Debugkonfiguration](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navigieren Sie zu `program.cs`. Fügen Sie einen Breakpoint in dieser Datei hinzu.

3. Klicken Sie auf Schaltfläche **Debuggen starten**, oder drücken Sie **F5**, und wählen Sie den gewünschten Prozess für die Anfügung aus.

4. In der Debugansicht von VS Code können Sie die Variablen im linken Bereich sehen. 

> [!NOTE]
> Das vorhergehende Beispiel zeigt, wie Sie .NET Core IoT Edge-Module in Containern debuggen. Es basiert auf der Debugversion von `Dockerfile.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Nachdem Sie das Debuggen Ihrer C#-Module abgeschlossen haben, empfehlen wir Ihnen, direkt `Dockerfile` ohne VSDBG für produktionsbereite IoT Edge-Module zu verwenden oder anzupassen.

## <a name="next-steps"></a>Nächste Schritte

[Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge](how-to-vscode-debug-azure-function.md)

