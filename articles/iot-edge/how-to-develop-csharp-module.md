---
title: Debuggen von C#-Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Entwickeln, Erstellen und Debuggen eines C#-Moduls für Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/27/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: c237b1eb9874357afa922f809109cf8f2181561e
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048185"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Debuggen von C#-Modulen für Azure IoT Edge

Sie können Ihre Geschäftslogik für die Ausführung im Edge-Bereich senden, indem Sie sie in Module für Azure IoT Edge umwandeln. Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von Visual Studio Code als Hauptentwicklungstool zum Entwickeln von C#-Modulen.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

> [!NOTE]
> In diesem Debugtutorial werden das Anfügen eines Prozesses in einem Modulcontainer und das Debuggen mit Visual Studio Code beschrieben. Sie können nur C#-Funktionen in Containern vom Typ „linux-amd64“ debuggen. Wenn Sie mit den Debugfunktionen von Visual Studio Code nicht vertraut sind, informieren Sie sich über [Debuggen](https://code.visualstudio.com/Docs/editor/debugging). 

Da in diesem Artikel Visual Studio Code als Hauptentwicklungstool verwendet wird, installieren Sie Visual Studio Code, und fügen Sie die erforderlichen Erweiterungen hinzu:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Um ein Modul zu erstellen, benötigen Sie .NET zum Erstellen des Projektordners, Docker zum Erstellen des Modulimages und eine Containerregistrierung zum Speichern des Modulimages:
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/) auf dem Entwicklungscomputer. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden. 

Um das Modul auf einem Gerät zu testen, benötigen Sie einen aktiven IoT Hub mit mindestens einem IoT Edge-Gerät. Wenn Ihr Computer als IoT Edge-Gerät verwendet werden soll, können Sie dazu die Schritte im Tutorial für [Windows](quickstart.md) bzw. [Linux](quickstart-linux.md) ausführen. 

## <a name="create-a-new-solution-template"></a>Erstellen einer neuen Lösungsvorlage

Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis von .NET Core 2.0 erstellen. Als Erstes erstellen Sie eine Projektmappe, dann generieren Sie das erste Modul in dieser Projektmappe. Jede Projektmappe kann mehrere Module enthalten. 

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Integriertes Terminal**.
3. Wählen Sie **Ansicht** > **Befehlspalette** aus. 
4. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus.

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

5. Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll, und klicken Sie auf **Ordner auswählen**. 
6. Geben Sie einen Namen für die Projektmappe an. 
7. Wählen Sie **C# Module** als Vorlage für das erste Modul in der Projektmappe aus.
8. Geben Sie einen Namen für das Modul an. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist. 
9. Geben Sie das Imagerepository für das Modul an. Visual Studio Code trägt den Modulnamen automatisch ein, daher müssen Sie nur **localhost:5000** durch Ihre eigenen Registrierungsinformationen ersetzen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie „localhost“ verwenden. Wenn Sie Azure Container Registry verwenden, verwenden Sie den Anmeldeserver aus Ihren Registrierungseinstellungen. Der Anmeldeserver hat die Form **\<Registrierungsname\>.azurecr.io**.

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe, und lädt diese in einem neuen Fenster.

   ![Anzeigen der IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/view-solution.png)

Die Projektmappe enthält drei Elemente: 
* Einen Ordner **.vscode**, der Debugkonfigurationen enthält.
* Einen Ordner **modules**, der Unterordner für jedes Modul enthält. Im Moment gibt es nur einen Ordner, Sie können jedoch weitere über die Befehlspalette mit dem Befehl **Azure IoT Edge: Add IoT Edge Module** hinzufügen. 
* Eine Datei **.env**, in der Ihre Umgebungsvariablen aufgeführt werden. Wenn Sie ACR als Registrierung verwenden, befinden sich derzeit der ACR-Benutzername und das Kennwort darin. 
* Eine Datei **deployment.template.json**, in der Ihr neues Modul zusammen mit einem Beispielmodul **tempSensor**, das Daten zum Testen simuliert, aufgeführt wird. Weitere Informationen zur Funktionsweise von Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können](module-composition.md).

## <a name="build-and-deploy-your-module-for-debugging"></a>Erstellen und Bereitstellen Ihres Moduls zum Debuggen

In jedem Modulordner gibt es mehrere Docker-Dateien für unterschiedliche Containertypen. Sie können jede Datei mit der Erweiterung **.debug** verwenden, um Ihr Modul zum Testen zu erstellen. C#-Module unterstützen derzeit nur das Debuggen in Containern vom Typ „linux-amd64“.

1. Wechseln Sie in Visual Studio Code zu der Datei `deployment.template.json`. Aktualisieren Sie Ihre Funktionsimage-URL, indem Sie am Ende **.debug** hinzufügen.

   ![Hinzufügen von „.debug“ zum Imagenamen](./media/how-to-develop-csharp-module/image-debug.png)

2. Geben Sie in der VS Code-Befehlspalette den Befehl **Edge: Build IoT Edge solution** ein, und führen Sie ihn aus.
3. Wählen Sie in der Befehlspalette die Datei `deployment.template.json` für die Projektmappe aus. 
4. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Create deployment for IoT Edge device** (Bereitstellung für IoT Edge-Gerät erstellen) aus. 
5. Öffnen Sie den Ordner **config** der Projektmappe, und wählen Sie die Datei `deployment.json` aus. Klicken Sie auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen). 

Sie können nun sehen, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Sie können Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker ps` im Terminal überprüfen.

## <a name="start-debugging-c-module-in-vs-code"></a>Debuggen des C#-Moduls in VS Code
VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird. 

1. Navigieren Sie zur Visual Studio Code-Debugansicht, und wählen Sie die Debugkonfigurationsdatei für Ihr Modul aus. Der Name der Debugoption sollte **ModuleName Remote Debug (.NET Core)** ähneln – ![Auswählen der Debugkonfiguration](./media/how-to-develop-csharp-module/debug-config.png)

2. Navigieren Sie zu `program.cs`. Fügen Sie einen Breakpoint in dieser Datei hinzu.

3. Klicken Sie auf Schaltfläche **Debuggen starten**, oder drücken Sie **F5**, und wählen Sie den gewünschten Prozess für die Anfügung aus.

4. In der Debugansicht von VS Code können Sie die Variablen im linken Bereich sehen. 

> [!NOTE]
> Das vorhergehende Beispiel zeigt, wie Sie .NET Core IoT Edge-Module in Containern debuggen. Es basiert auf der Debugversion von `Dockerfile.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Nachdem Sie das Debuggen Ihrer C#-Module abgeschlossen haben, empfehlen wir Ihnen, direkt `Dockerfile` ohne VSDBG für produktionsbereite IoT Edge-Module zu verwenden oder anzupassen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).

