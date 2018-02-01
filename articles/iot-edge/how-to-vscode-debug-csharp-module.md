---
title: Verwenden von Visual Studio Code zum Debuggen eines C#-Moduls mit Azure IoT Edge | Microsoft-Dokumentation
description: Debuggen eines C#-Modul mit Azure IoT Edge in VS Code
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1ab67cd8aaf59cde3157fcb877ce13f10cb432bb
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-debug-c-module-with-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Debuggen eines C#-Moduls mit Azure IoT Edge
Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio Code](https://code.visualstudio.com/) als Hauptentwicklungstool zum Debuggen Ihrer IoT Edge-Module.

## <a name="prerequisites"></a>Voraussetzungen
Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer mit Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

Stellen Sie sicher, dass Sie folgende Tutorials abgeschlossen haben, bevor Sie mit diesem Leitfaden beginnen.
- [Verwenden von Visual Studio Code zur Entwicklung eines C#-Moduls mit Azure IoT Edge](how-to-vscode-develop-csharp-module.md)

Nach Abschluss des vorherigen Tutorials sollten Sie die folgenden Elemente vorbereitet haben:
- Eine lokale Docker-Registrierung, die auf dem Entwicklungscomputer ausgeführt wird. Es wird empfohlen, eine lokale Docker-Registrierung als Prototyp bzw. für Testzwecke zu verwenden.
- Die `Program.cs`-Datei mit dem neuesten Filtermodulcode.
- Eine aktualisierte `deployment.json`-Datei für Ihr Sensormodul und Ihr Filtermodul.
- Eine Edge-Runtime, die auf dem Entwicklungscomputer ausgeführt wird.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Erstellen Ihres IoT Edge-Moduls zum Debuggen
1. Um mit dem Debuggen zu beginnen, müssen Sie mithilfe von **dockerfile.debug** Ihr Docker-Image neu erstellen und Ihre Edge-Lösung erneut bereitstellen. Klicken Sie im Visual Studio Code-Explorer auf den Ordner „Docker“, um ihn zu öffnen. Anschließend klicken Sie auf den Ordner `linux-x64`, klicken mit der rechten Maustaste auf **Dockerfile.debug** und klicken auf **Docker-Image für IoT Edge-Modul erstellen**.

    ![Erstellen eines Debugimages](./media/how-to-debug-csharp-module/build-debug-image.png)

3. Navigieren Sie im Fenster **Ordner auswählen** zum Ordner `./bin/Debug/netcoreapp2.0/publish`, oder geben Sie den Ordnernamen direkt ein. Klicken Sie auf **Select Folder as EXE_DIR** (Ordner als EXE_DIR auswählen).
4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filtermodule:latest`. Wenn Sie eine Bereitstellung für die lokale Registrierung durchführen, sollte diese `localhost:5000/filtermodule:latest` lauten.
5. Pushen Sie das Image in Ihr Docker-Repository. Klicken Sie auf **Edge: Docker-Image für IoT Edge-Modul mithilfe von Push übertragen**, und geben Sie oben im VS Code-Fenster in das Popuptextfeld die Image-URL ein. Verwenden Sie dabei die Image-URL aus dem obigen Schritt.
6. Sie können die Datei `deployment.json` zum erneuten Bereitstellen wiederverwenden. Geben Sie in der Befehlspalette den Text **Edge: Edge neu starten** ein, und wählen Sie diese Option aus, um das mit der Debugversion ausgeführte Filtermodul abzurufen.

## <a name="start-debugging-in-vs-code"></a>Starten des Debugvorgangs in Visual Studio Code
1. Wechseln Sie zum Visual Studio Code-Debugfenster. Drücken Sie **F5**, und wählen Sie **IoT Edge(.Net Core)** aus.

    ![Drücken Sie F5.](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. Navigieren Sie in `launch.json` zum Abschnitt **Benutzerdefiniertes IoT Edge-Modul debuggen (.NET Core)**, und geben Sie `<container_name>` unter `pipeArgs` ein. In diesem Tutorial sollte dies `filtermodule` sein.

    ![Ändern von pipeArgs](./media/how-to-debug-csharp-module/f5-debug-option.png)

3. Navigieren Sie zu „Program.cs“. Fügen Sie einen Haltepunkt in `method static async Task<MessageResponse> FilterModule(Message message, object userContext)` hinzu.
4. Drücken Sie erneut **F5**. Und wählen Sie den Prozess zum Anfügen aus. In diesem Tutorial sollte der Name des Prozesses `FilterModule.dll` sein.

    ![Anfügen des Prozesses](./media/how-to-debug-csharp-module/attach-process.png)

5. Im Debugfenster von Visual Studio Code können Sie die Variablen im linken Bereich sehen. 

> [!NOTE]
> Das obige Beispiel zeigt, wie .NET Core IoT Edge-Module in Containern gedebuggt werden. Es basiert auf der Debugversion von `Dockerfile.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Wir empfehlen Ihnen, `Dockerfile` ohne VSDBG für produktionsbereite IoT Edge-Module direkt zu verwenden oder anzupassen, nachdem Sie das Debugging Ihrer C#-Module abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul erstellt, es für das Debugging bereitgestellt und anschließend mit dem Debuggen in VS Code begonnen. In den nächsten Tutorials können Sie mehr über andere Szenarien bei der Entwicklung von Azure IoT Edge in VS Code erfahren. 

> [!div class="nextstepaction"]
> [Entwickeln und Bereitstellen eines C#-Moduls in VS Code](how-to-vscode-develop-csharp-module.md)
