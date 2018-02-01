---
title: Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge | Microsoft-Dokumentation
description: Debuggen von Azure Functions (C#) mit Azure IoT Edge in VS Code
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: db86a08a19e97f8f415849aa060fe87d77cccf68
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge

Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio Code](https://code.visualstudio.com/) als Hauptentwicklungstool zum Debuggen von Azure Functions in IoT Edge.

## <a name="prerequisites"></a>Voraussetzungen
Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer mit Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

Stellen Sie sicher, dass Sie folgende Tutorials abgeschlossen haben, bevor Sie mit diesem Leitfaden beginnen.
- [Verwenden von Visual Studio Code zum Entwickeln und Bereitstellen von Azure Functions in Azure IoT Edge](how-to-vscode-develop-azure-function.md)

Nach Abschluss des vorherigen Tutorials sollten Sie die folgenden Elemente vorbereitet haben.
- Eine lokale Docker-Registrierung, die auf dem Entwicklungscomputer ausgeführt wird. Es wird empfohlen, eine lokale Docker-Registrierung als Prototyp und für Testzwecke zu verwenden.
- Die `run.csx`-Datei mit dem neuesten Filterfunktionscode.
- Eine aktualisierte `deployment.json`-Datei für Ihr Sensormodul und Ihr Filterfunktionsmodul.
- Eine Edge-Runtime, die auf dem Entwicklungscomputer ausgeführt wird.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Erstellen Ihres IoT Edge-Moduls zum Debuggen
1. Um mit dem Debuggen zu beginnen, müssen Sie mithilfe von **dockerfile.debug** Ihr Docker-Image neu erstellen und Ihre Edge-Lösung erneut bereitstellen. Klicken Sie im Visual Studio Code-Explorer auf den Ordner „Docker“, um ihn zu öffnen. Anschließend klicken Sie auf den Ordner `linux-x64`, klicken mit der rechten Maustaste auf **Dockerfile.debug** und klicken auf **Docker-Image für IoT Edge-Modul erstellen**.

    ![Erstellen eines Debugimage](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Navigieren Sie im Fenster **Ordner auswählen** zum Projektordner **FilterFunction**, und klicken Sie dann auf **Ordner als EXE_DIR auswählen**.
3. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filterfunction:latest`. Wenn Sie eine Bereitstellung für die lokale Registrierung durchführen, sollte diese `localhost:5000/filterfunction:latest` lauten.

    ![Übertragen eines Image mithilfe von Push](./media/how-to-debug-csharp-function/push-image.png)

4. Pushen Sie das Image in Ihr Docker-Repository. Klicken Sie auf **Edge: Docker-Image für IoT Edge-Modul mithilfe von Push übertragen**, und geben Sie oben im VS Code-Fenster in das Popuptextfeld die Image-URL ein. Verwenden Sie dabei die Image-URL aus dem obigen Schritt.
5. Sie können die Datei `deployment.json` zum erneuten Bereitstellen wiederverwenden. Geben Sie in der Befehlspalette den Text **Edge: Edge neu starten** ein, und wählen Sie diese Option aus, um die mit der Debugversion ausgeführte Filterfunktion abzurufen.

## <a name="start-debugging-in-vs-code"></a>Starten des Debugvorgangs in Visual Studio Code
1. Wechseln Sie zum Visual Studio Code-Debugfenster. Drücken Sie **F5**, und wählen Sie **IoT Edge(.Net Core)** aus.

    ![Ansicht beim Drücken der Taste F5](./media/how-to-debug-csharp-function/f5-debug-option.png)

2. Navigieren Sie in `launch.json` zum Abschnitt **Benutzerdefinierte IoT Edge-Funktion (.NET Core) debuggen**, und geben Sie unter `pipeArgs` den `<container_name>` ein. In diesem Tutorial sollte dies `filterfunction` sein.

    ![Aktualisieren der Datei „launch.json“](./media/how-to-debug-csharp-function/update-launch-json.png)

3. Navigieren Sie zu „run.csx“. Fügen Sie in der Funktion einen Haltepunkt hinzu.
4. Navigieren Sie zum Debugfenster (STRG+UMSCHALT+D), und wählen Sie aus der Dropdownliste **IoT Edge-Funktion debuggen (.NET Core)** aus. 

    ![Auswählen des Debugmodus](./media/how-to-debug-csharp-function/choose-debug-mode.png)

5. Klicken Sie auf Schaltfläche „Debuggen starten“, oder drücken Sie die Taste **F5**, und wählen Sie den Prozess aus, an den die Anfügung erfolgen soll.

    ![Prozess zum Anfügen einer Funktion](./media/how-to-debug-csharp-function/attach-function-process.png)

6. Im Debugfenster von Visual Studio Code können Sie die Variablen im linken Bereich sehen. 

> [!NOTE]
> Das obige Beispiel veranschaulicht das Debuggen von .NET Core IoT Edge-Funktionen in Containern. Es basiert auf der Debugversion von `Dockerfile.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Es wird empfohlen, für produktionsbereite IoT Edge-Module `Dockerfile` ohne VSDBG direkt zu verwenden oder anzupassen, nachdem Sie das Debugging Ihrer C#-Module abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Funktion erstellt und zu Debugzwecken in IoT Edge bereitgestellt. Anschließend haben Sie mit dem Debuggen in VS Code begonnen. Sie können mit einem der folgenden Tutorials fortfahren, um andere Szenarien bei der Entwicklung von Azure IoT Edge in VS Code kennenzulernen. 

> [!div class="nextstepaction"]
> [Entwickeln und Bereitstellen eines C#-Moduls in VS Code](how-to-vscode-develop-csharp-module.md)

