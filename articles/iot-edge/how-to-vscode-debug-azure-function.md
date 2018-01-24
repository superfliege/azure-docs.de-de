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
ms.openlocfilehash: 4344a450d218a7424cd055cf086c1e4865c9af10
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2017
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Debuggen von Azure Functions mit Azure IoT Edge

Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio Code](https://code.visualstudio.com/) als Hauptentwicklungstool zum Debuggen von Azure Functions in IoT Edge.

## <a name="prerequisites"></a>Voraussetzungen
Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

Stellen Sie sicher, dass Sie folgende Tutorials abgeschlossen haben, bevor Sie mit diesem Leitfaden beginnen.
- [Verwenden von Visual Studio Code zum Entwickeln und Bereitstellen von Azure Functions in Azure IoT Edge](how-to-vscode-develop-azure-function.md)

Nach Abschluss des vorherigen Tutorials sollten Sie die folgenden Elemente vorbereitet haben.
- Eine lokale Docker-Registrierung, die auf dem Entwicklungscomputer ausgeführt wird. Es wird empfohlen, eine lokale Docker-Registrierung als Prototyp und für Testzwecke zu verwenden.
- Die `run.csx`-Datei mit dem neuesten Filterfunktionscode.
- Eine aktualisierte `deployment.json`-Datei für Ihr Sensormodul und Ihr Filterfunktionsmodul.
- Eine Edge-Runtime, die auf dem Entwicklungscomputer ausgeführt wird.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Erstellen Ihres IoT Edge-Moduls zum Debuggen
1. Um mit dem Debuggen zu beginnen, müssen Sie mithilfe von **dockerfile.debug** Ihr Docker-Image neu erstellen und Ihre Edge-Lösung erneut bereitstellen. Klicken Sie im Visual Studio Code-Explorer auf den Ordner „Docker“, um ihn zu öffnen. Anschließend klicken Sie auf den Ordner `linux-x64`, klicken mit der rechten Maustaste auf **Dockerfile.debug** und klicken auf **Docker-Image für IoT Edge-Modul erstellen**.
3. Navigieren Sie im Fenster **Ordner auswählen** zum Projektordner **FilterFunction**, und klicken Sie dann auf **Ordner als EXE_DIR auswählen**.
4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filterfunction:latest`. Wenn Sie eine Bereitstellung für die lokale Registrierung durchführen, sollte der Name `localhost:5000/filterfunction:latest` lauten.
5. Pushen Sie das Image in Ihr Docker-Repository. Klicken Sie auf **Edge: Docker-Image für IoT Edge-Modul mithilfe von Push übertragen**, und geben Sie oben im VS Code-Fenster in das Popuptextfeld die Image-URL ein. Verwenden Sie dabei die Image-URL aus dem obigen Schritt.
6. Sie können die Datei `deployment.json` zum erneuten Bereitstellen wiederverwenden. Geben Sie in der Befehlspalette den Text **Edge: Edge neu starten** ein, und wählen Sie diese Option aus, um die mit der Debugversion ausgeführte Filterfunktion abzurufen.

## <a name="start-debugging-in-vs-code"></a>Starten des Debugvorgangs in Visual Studio Code
1. Wechseln Sie zum Visual Studio Code-Debugfenster. Drücken Sie **F5**, und wählen Sie **IoT Edge(.Net Core)** aus.
2. Navigieren Sie in `launch.json` zum Abschnitt **Benutzerdefinierte IoT Edge-Funktion (.NET Core) debuggen**, und geben Sie unter `pipeArgs` den `<container_name>` ein. In diesem Tutorial sollte dies `filterfunction` sein.
3. Navigieren Sie zu „run.csx“. Fügen Sie in der Funktion einen Haltepunkt hinzu.
4. Drücken Sie erneut **F5**. Wählen Sie den Prozess zum Anfügen aus.
5. Im Debugfenster von Visual Studio Code können Sie die Variablen im linken Bereich sehen. 

> [!NOTE]
> Das obige Beispiel veranschaulicht das Debuggen von .NET Core IoT Edge-Funktionen in Containern. Es basiert auf der Debugversion von `Dockerfile.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Es wird empfohlen, für produktionsbereite IoT Edge-Module `Dockerfile` ohne VSDBG direkt zu verwenden oder anzupassen, nachdem Sie das Debugging Ihrer C#-Module abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Funktion erstellt und zu Debugzwecken in IoT Edge bereitgestellt. Anschließend haben Sie mit dem Debuggen in VS Code begonnen. Sie können mit einem der folgenden Tutorials fortfahren, um andere Szenarien bei der Entwicklung von Azure IoT Edge in VS Code kennenzulernen. 

> [!div class="nextstepaction"]
> [Entwickeln und Bereitstellen eines C#-Moduls in VS Code](how-to-vscode-develop-csharp-module.md)

