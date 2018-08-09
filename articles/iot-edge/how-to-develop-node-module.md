---
title: Debuggen von Node.js-Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Entwickeln und Debuggen von Node.js-Modulen für Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a5ab49beed79a8ea3a7ded0848c09acad27a5fb1
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39390536"
---
# <a name="develop-and-debug-nodejs-modules-with-azure-iot-edge-for-visual-studio-code"></a>Entwickeln und Debuggen von Node.js-Modulen mit Azure IoT Edge für Visual Studio Code

Sie können Ihre Geschäftslogik für die Ausführung im Edge-Bereich senden, indem Sie sie in Module für Azure IoT Edge umwandeln. Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von Visual Studio Code als Hauptentwicklungstool zum Entwickeln von C#-Modulen.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

> [!NOTE]
> In diesem Debugtutorial werden das Anfügen eines Prozesses in einem Modulcontainer und das Debuggen mit Visual Studio Code beschrieben. Sie können Node.js-Module in linux-amd64-, windows- und arm32-Containern debuggen. Wenn Sie mit den Debugfunktionen von Visual Studio Code nicht vertraut sind, helfen Ihnen die Informationen unter [Debuggen](https://code.visualstudio.com/Docs/editor/debugging) weiter. 

Da in diesem Artikel Visual Studio Code als Hauptentwicklungstool verwendet wird, müssen Sie Visual Studio Code installieren und die erforderlichen Erweiterungen hinzufügen:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Um ein Modul zu erstellen, benötigen Sie Node.js (enthält npm) zum Erstellen des Projektordners, Docker zum Erstellen des Modulimages und eine Containerregistrierung zum Speichern des Modulimages:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   >[!TIP]
   >Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden. 

Um das Modul auf einem Gerät zu testen, benötigen Sie einen aktiven IoT Hub mit mindestens einem IoT Edge-Gerät. Wenn Ihr Computer als IoT Edge-Gerät verwendet werden soll, können Sie hierzu die Schritte im Tutorial für [Windows](quickstart.md) bzw. [Linux](quickstart-linux.md) ausführen. 

## <a name="create-a-new-solution-template"></a>Erstellen einer neuen Lösungsvorlage

Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis von .NET Core 2.0 erstellen. Als Erstes erstellen Sie eine Projektmappe, dann generieren Sie das erste Modul in dieser Projektmappe. Jede Projektmappe kann mehrere Module enthalten. 

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Integriertes Terminal**.
2. Geben Sie im integrierten Terminal den folgenden Befehl ein, um die aktuelle Version der Azure IoT Edge-Modulvorlage für Node.js zu installieren (bzw. zu aktualisieren):

   ```cmd/sh
   npm install -g yo generator-azure-iot-edge-module
   ```
3. Klicken Sie in Visual Studio Code auf **Ansicht** > **Befehlspalette**. 
4. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus.

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

5. Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll, und klicken Sie auf **Ordner auswählen**. 
6. Geben Sie einen Namen für die Projektmappe an. 
7. Wählen Sie **Node.js Module** (Node.js-Modul) als Vorlage für das erste Modul in der Projektmappe aus.
8. Geben Sie einen Namen für das Modul an. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist. 
9. Geben Sie das Imagerepository für das Modul an. Visual Studio Code trägt den Modulnamen automatisch ein, sodass Sie nur **localhost:5000** durch Ihre eigenen Registrierungsinformationen ersetzen müssen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie „localhost“ nutzen. Nutzen Sie den Anmeldeserver aus Ihren Registrierungseinstellungen, wenn Sie Azure Container Registry verwenden. Der Anmeldeserver hat die Form **\<Registrierungsname\>.azurecr.io**.

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe und lädt diese in einem neuen Fenster.

In der Projektmappe sind drei Elemente enthalten: 
* Einen Ordner **.vscode**, der Debugkonfigurationen enthält.
* Einen Ordner **modules**, der Unterordner für jedes Modul enthält. Im Moment gibt es nur einen Ordner, aber Sie können über die Befehlspalette mit dem Befehl **Azure IoT Edge: Add IoT Edge Module** weitere hinzufügen. 
* Eine **.env**-Datei, in der Ihre Umgebungsvariablen aufgeführt werden. Wenn Sie ACR als Registrierung verwenden, befinden sich darin derzeit der ACR-Benutzername und das Kennwort. 

   >[!NOTE]
   >Die Umgebungsdatei wird nur erstellt, wenn Sie ein Imagerepository für das Modul angeben. Wenn Sie die „localhost“-Standardeinstellungen zum lokalen Testen und Debuggen akzeptiert haben, müssen Sie keine Umgebungsvariablen deklarieren. 

* Eine Datei **deployment.template.json**, in der Ihr neues Modul zusammen mit dem Beispielmodul **tempSensor**, mit dem Daten zum Testen simuliert werden, aufgeführt ist. Weitere Informationen zur Funktionsweise von Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können](module-composition.md).

## <a name="devlop-your-module"></a>Entwickeln Ihres Moduls

Der im Lieferumfang der Projektmappe enthaltene standardmäßige Azure-Funktionscode befindet sich unter **modules** > **\<Name Ihres Moduls\>** > **app.js**. Das Modul und die Datei „deployment.template.json“ werden so eingerichtet, dass Sie die Projektmappe erstellen, in Ihre Containerregistrierung verschieben und zum Starten des Tests für ein Gerät bereitstellen können, ohne Code ändern zu müssen. Das Modul ist so konzipiert, dass es einfach eine Eingabe aus einer Quelle akzeptiert (in diesem Fall das Daten simulierende tempSensor-Modul) und IoT Hub übergibt. 

Wenn Sie bereit sind, die Node.js-Vorlage mit Ihrem eigenen Code anzupassen, erstellen Sie mit den [Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md) Module, die die wesentlichen Anforderungen für IoT-Lösungen wie Sicherheit, Geräteverwaltung und Zuverlässigkeit berücksichtigen. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Erstellen und Bereitstellen Ihres Moduls zum Debuggen

In jedem Modulordner gibt es mehrere Docker-Dateien für unterschiedliche Containertypen. Sie können jede Datei mit der Erweiterung **.debug** verwenden, um Ihr Modul zum Testen zu erstellen. C#-Module unterstützen derzeit nur das Debuggen in Containern vom Typ „linux-amd64“.

1. Wechseln Sie in Visual Studio Code zu der Datei `deployment.template.json`. Aktualisieren Sie Ihre Modulimage-URL, indem Sie am Ende **.debug** hinzufügen.
2. Ersetzen Sie „createOptions“ für das Node.js-Modul in **deployment.template.json** durch den unten angegebenen Code, und speichern Sie die Datei: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

2. Geben Sie in der Visual Studio Code-Befehlspalette den Befehl **Azure IoT Edge: Build IoT Edge solution** (Azure IoT Edge: IoT Edge-Projektmappe erstellen) ein, und führen Sie ihn aus.
3. Wählen Sie in der Befehlspalette die Datei `deployment.template.json` für die Projektmappe aus. 
4. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Create deployment for IoT Edge device** (Bereitstellung für IoT Edge-Gerät erstellen) aus. 
5. Öffnen Sie den Ordner **config** der Projektmappe, und wählen Sie die Datei `deployment.json` aus. Klicken Sie auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen). 

Sie können nun sehen, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Sie können Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker ps` im Terminal überprüfen.

## <a name="start-debugging-nodejs-module-in-vs-code"></a>Starten des Debuggens für das Node.js-Modul in VS Code

VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird. 

1. Navigieren Sie zur Visual Studio Code-Debugansicht, und wählen Sie die Debugkonfigurationsdatei für Ihr Modul aus.

2. Navigieren Sie zu `app.js`. Fügen Sie einen Breakpoint in dieser Datei hinzu.

3. Klicken Sie auf Schaltfläche **Debuggen starten**, oder drücken Sie **F5**, und wählen Sie den gewünschten Prozess für die Anfügung aus.

4. In der Debugansicht von VS Code können Sie die Variablen im linken Bereich sehen. 

Im obigen Beispiel wird veranschaulicht, wie Sie Node.js-IoT Edge-Module in Containern debuggen. Dem createOptions-Modulcontainer wurden verfügbar gemachte Ports hinzugefügt. Nachdem Sie das Debuggen Ihrer Node.js-Module abgeschlossen haben, empfehlen wir Ihnen, diese verfügbar gemachten Ports für IoT Edge-Module, die für die Produktion bereit sind, zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).
