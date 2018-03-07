---
title: Verwenden von Visual Studio Code zum Debuggen eines C#-Moduls mit Azure IoT Edge | Microsoft-Dokumentation
description: Debuggen eines C#-Moduls mit Azure IoT Edge in Visual Studio Code
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5ed517cf8d70cd279a55b79ad448709116cf511b
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Debuggen eines C#-Moduls mit Azure IoT Edge
Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio Code](https://code.visualstudio.com/) als Hauptentwicklungstool zum Debuggen Ihrer Azure IoT Edge-Module.

## <a name="prerequisites"></a>Voraussetzungen
Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer mit Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

Bevor Sie mit diesem Leitfaden beginnen, absolvieren Sie das folgende Tutorial:
- [Verwenden von Visual Studio Code zur Entwicklung eines C#-Moduls mit Azure IoT Edge](how-to-vscode-develop-csharp-module.md)

Nach Abschluss des vorherigen Tutorials sollten Sie die folgenden Elemente vorbereitet haben:
- Eine lokale Docker-Registrierung, die auf dem Entwicklungscomputer ausgeführt wird. Diese ist für die Prototyperstellung und für Testzwecke erforderlich.
- Die Datei `Program.cs` mit dem neuesten Filtermodulcode
- Eine aktualisierte Datei `deployment.json` für Ihre Sensor- und Filtermodule
- Eine IoT Edge-Runtime, die auf Ihrem Entwicklungscomputer ausgeführt wird

## <a name="build-your-iot-edge-module-for-debugging"></a>Erstellen Ihres IoT Edge-Moduls zum Debuggen
1. Um mit dem Debuggen zu beginnen, müssen Sie mithilfe von **dockerfile.debug** Ihr Docker-Image neu erstellen und Ihre IoT Edge-Lösung erneut bereitstellen. Wählen Sie im Visual Studio Code-Explorer den Docker-Ordner aus, um diesen zu öffnen. Anschließend wählen Sie den Ordner **linux-x64** aus, klicken mit der rechten Maustaste auf **Dockerfile.debug** und klicken auf **Docker-Image für IoT Edge-Modul erstellen**.

    ![Screenshot des VS Code-Explorers](./media/how-to-debug-csharp-module/build-debug-image.png)

3. Navigieren Sie im Fenster **Ordner auswählen** zum Ordner **./bin/Debug/netcoreapp2.0/publish**, oder geben Sie den Ordnernamen direkt ein. Wählen Sie dann **Ordner als EXE_DIR auswählen** aus.
4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filtermodule:latest`. Wenn Sie eine Bereitstellung für die lokale Registrierung durchführen, sollte diese `localhost:5000/filtermodule:latest` lauten.
5. Pushen Sie das Image in Ihr Docker-Repository. Klicken Sie auf **Edge: Docker-Image für IoT Edge-Modul mithilfe von Push übertragen**, und geben Sie oben im VS Code-Fenster in das Popuptextfeld die Image-URL ein. Verwenden Sie dabei die Image-URL aus dem vorherigen Schritt.
6. Sie können die Datei `deployment.json` zum erneuten Bereitstellen wiederverwenden. Geben Sie in der Befehlspalette den Text **Edge: Edge neu starten** ein, und wählen Sie diese Option aus, um das mit der Debugversion ausgeführte Filtermodul abzurufen.

## <a name="start-debugging-in-vs-code"></a>Starten des Debugvorgangs in Visual Studio Code
1. Wechseln Sie zum VS Code-Debugfenster. Drücken Sie **F5**, und wählen Sie **IoT Edge (.NET Core)** aus.

    ![Screenshot des VS Code-Debugfensters](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. Navigieren Sie in `launch.json` zum Abschnitt **Benutzerdefiniertes IoT Edge-Modul (.NET Core) debuggen**. Geben Sie unter **pipeArgs** einen Wert für `<container_name>` ein. Dieses sollte in diesem Tutorial das Format `filtermodule` aufweisen.

    ![Screenshot der VS Code-Datei „launch.json“](./media/how-to-debug-csharp-module/add-container-name.png)

3. Navigieren Sie zu **Program.cs**. Fügen Sie einen Haltepunkt in `method static async Task<MessageResponse> FilterModule(Message message, object userContext)` hinzu.
4. Drücken Sie erneut **F5**, und wählen Sie den Prozess aus, an den die Anfügung erfolgen soll. In diesem Tutorial sollte der Name des Prozesses `FilterModule.dll` sein.

    ![Screenshot des VS Code-Debugfensters](./media/how-to-debug-csharp-module/attach-process.png)

5. Im Debugfenster von VS Code können Sie die Variablen im linken Bereich sehen. 

> [!NOTE]
> Das vorhergehende Beispiel zeigt, wie Sie .NET Core IoT Edge-Module in Containern debuggen. Es basiert auf der Debugversion von `Dockerfile.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Nachdem Sie das Debuggen Ihrer C#-Module abgeschlossen haben, empfehlen wir Ihnen, direkt `Dockerfile` ohne VSDBG für produktionsbereite IoT Edge-Module zu verwenden oder anzupassen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul erstellt und zum Debuggen bereitgestellt. Sie haben mit dem Debuggen in VS Code begonnen. Informationen zu weiteren Szenarien bei der Entwicklung von Azure IoT Edge in VS Code finden Sie in folgenden Themen: 

> [!div class="nextstepaction"]
> [Entwickeln und Bereitstellen eines C#-Moduls in VS Code](how-to-vscode-develop-csharp-module.md)
