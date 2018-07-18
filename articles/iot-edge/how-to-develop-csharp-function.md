---
title: Debuggen von Functions-Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Debuggen von Azure Functions für C# mit Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5ee5d368bde0f176442a01ba266791f8a9c2cbc3
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052826"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Debuggen von Azure Functions-Funktionen für Azure IoT Edge

Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio (VS) Code](https://code.visualstudio.com/) zum Debuggen von Azure Functions in IoT Edge.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

> [!NOTE]
> In diesem Debugtutorial werden das Anfügen eines Prozesses in einem Modulcontainer und das Debuggen mit Visual Studio Code beschrieben. Sie können nur C#-Module in Containern vom Typ „linux-amd64“ debuggen. Wenn Sie mit den Debugfunktionen von Visual Studio Code nicht vertraut sind, informieren Sie sich über [Debuggen](https://code.visualstudio.com/Docs/editor/debugging). 

In diesem Artikel wird Visual Studio Code als das Hauptentwicklungstool verwendet. Installieren Sie Visual Studio Code, und fügen Sie die erforderlichen Erweiterungen hinzu: 

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

Die folgenden Schritte veranschaulichen, wie Sie eine IoT Edge-Projektmappe erstellen, die ein C#-Functions-Modul enthält. Jede Projektmappe kann mehrere Module enthalten.

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Integriertes Terminal**.
3. Wählen Sie **Ansicht** > **Befehlspalette** aus.
4. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus. 

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

5. Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll, und klicken Sie auf **Ordner auswählen**. 
6. Geben Sie einen Namen für die Projektmappe an. 
7. Wählen Sie **Azure Functions – C#** als Vorlage für das erste Modul in der Projektmappe aus.
8. Geben Sie einen Namen für das Modul an. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist. 
9. Geben Sie das Imagerepository für das Modul an. Visual Studio Code trägt den Modulnamen automatisch ein, daher müssen Sie nur **localhost:5000** durch Ihre eigenen Registrierungsinformationen ersetzen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie „localhost“ verwenden. Wenn Sie Azure Container Registry verwenden, verwenden Sie den Anmeldeserver aus Ihren Registrierungseinstellungen. Der Anmeldeserver hat die Form **\<Registrierungsname\>.azurecr.io**.

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe mit einem Functions-Projekt und lädt diese in einem neuen Fenster.

Die Projektmappe enthält drei Elemente: 

* Einen Ordner **.vscode**, der Debugkonfigurationen enthält
* Einen Ordner **modules**, der Unterordner für jedes Modul enthält. Im Moment gibt es nur einen Ordner, Sie können jedoch weitere über die Befehlspalette mit dem Befehl **Azure IoT Edge: Add IoT Edge Module** hinzufügen.
* Eine Datei **.env**, in der Ihre Umgebungsvariablen aufgeführt werden. Wenn Sie ACR als Registrierung verwenden, befinden sich derzeit der ACR-Benutzername und das Kennwort darin. 
* Eine Datei **deployment.template.json**, in der Ihr neues Modul zusammen mit einem Beispielmodul **tempSensor**, das Daten zum Testen simuliert, aufgeführt wird. Weitere Informationen zur Funktionsweise von Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können](module-composition.md).

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Erstellen Ihres IoT Edge-Funktionsmoduls zum Debuggen
1. Um mit dem Debuggen zu beginnen, müssen Sie mithilfe von **Dockerfile.amd64.debug** Ihr Dockerimage neu erstellen und Ihre Edge-Lösung erneut bereitstellen. Navigieren Sie im VS Code-Explorer zur Datei `deployment.template.json`. Aktualisieren Sie Ihre Funktionsimage-URL, indem Sie am Ende `.debug` hinzufügen.

    ![Erstellen eines Debugimage](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Erstellen Sie Ihre Lösung neu. Geben Sie in der Visual Studio Code-Befehlspalette den Befehl **Azure IoT Edge: Build IoT Edge solution** ein, und führen Sie ihn aus.
3. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Create deployment for Edge device** (Bereitstellung für Edge-Gerät erstellen) aus. Wählen Sie die Datei `deployment.json` unter dem Ordner `config` aus. Sie können nun sehen, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Sie können Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker images` im Terminal überprüfen.

## <a name="start-debugging-c-function-in-vs-code"></a>Starten des Debugging von C#-Funktion in VS Code
1. VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird. Navigieren Sie zur Debugansicht, und wählen Sie die entsprechende Debugkonfigurationsdatei aus. Der Name der Debugoption sollte **ModuleName Remote Debug (.NET Core)** ähneln – ![Auswählen der Debugkonfiguration](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navigieren Sie zu `run.csx`. Fügen Sie in der Funktion einen Haltepunkt hinzu.
3. Klicken Sie auf Schaltfläche **Debuggen starten**, oder drücken Sie **F5**, und wählen Sie den gewünschten Prozess für die Anfügung aus.
4. In der Debugansicht von VS Code können Sie die Variablen im linken Bereich sehen. 


> [!NOTE]
> Das obige Beispiel veranschaulicht das Debuggen von .NET Core IoT Edge-Funktionen in Containern. Es basiert auf der Debugversion von `Dockerfile.amd64.debug`, die VSDBG (den .NET Core-Befehlszeilendebugger) beim Erstellen in Ihr Containerimage einbindet. Es wird empfohlen, für produktionsbereite IoT Edge-Module `Dockerfile` ohne VSDBG direkt zu verwenden oder anzupassen, nachdem Sie das Debugging Ihrer C#-Module abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).
