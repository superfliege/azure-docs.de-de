---
title: Debuggen von C-Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Entwickeln, Erstellen und Debuggen eines C-Moduls für Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 07/20/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 9fc067c46828079f7369683b5edec682747cd5c7
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391451"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Debuggen von C-Modulen für Azure IoT Edge

Sie können Ihre Geschäftslogik in Azure IoT Edge-Module umwandeln. In diesem Artikel wird veranschaulicht, wie Sie Visual Studio Code (VS Code) als wichtigstes Tool zum Entwickeln und Debuggen von C-Modulen verwenden.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Sie simulieren außerdem ein IoT Edge-Gerät auf dem Entwicklungscomputer.

> [!NOTE]
> In diesem Debugartikel werden das Anfügen eines Prozesses in einem Modulcontainer und das Debuggen mit Visual Studio Code beschrieben. Sie können nur C-Module in Containern vom Typ „Linux amd64“ debuggen. Wenn Sie mit den Debugfunktionen von Visual Studio Code nicht vertraut sind, informieren Sie sich über [Debuggen](https://code.visualstudio.com/Docs/editor/debugging). 

Weil in diesem Artikel Visual Studio Code als Hauptentwicklungstool verwendet wird, installieren Sie VS Code. Fügen Sie dann die erforderlichen Erweiterungen hinzu:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C/C++-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) für Visual Studio Code
* [Docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Um ein Modul zu erstellen, benötigen Sie Docker zum Erstellen des Modulimages und eine Containerregistrierung zum Speichern des Modulimages:
* [Docker Community Edition](https://docs.docker.com/install/) auf Ihrem Entwicklungscomputer. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden. 

Um das Modul auf einem Gerät zu testen, benötigen Sie einen aktiven IoT Hub mit mindestens einem IoT Edge-Gerät. Um Ihren Computer als IoT Edge-Gerät zu verwenden, führen Sie die Schritte in der Schnellstartanleitung für [Windows](quickstart.md) oder [Linux](quickstart-linux.md) aus. 

## <a name="create-a-new-solution-template"></a>Erstellen einer neuen Lösungsvorlage

Erstellen Sie in diesen Schritten mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis des Azure IoT C SDK. Als Erstes erstellen Sie eine Projektmappe, und dann generieren Sie das erste Modul in dieser Projektmappe. Jede Projektmappe kann mehrere Module enthalten. 

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Integriertes Terminal**.
3. Wählen Sie **Ansicht** > **Befehlspalette** aus. 
4. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus.

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

5. Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll. Wählen Sie **Ordner auswählen** aus. 
6. Geben Sie einen Namen für die Lösung ein. 
7. Wählen Sie **C Module** als Vorlage für das erste Modul in der Projektmappe aus.
8. Geben Sie einen Namen für Ihr Modul ein. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist. 
9. Geben Sie den Namen des Imagerepositorys des Moduls an. VS Code trägt als Modulnamen automatisch **localhost:5000** ein. Ersetzen Sie ihn durch Ihre eigenen Registrierungsinformationen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie **localhost** nutzen. Nutzen Sie den Anmeldeserver aus Ihren Registrierungseinstellungen, wenn Sie Azure Container Registry verwenden. Der Anmeldeserver hat die Form **\<Registrierungsname\>.azurecr.io**.

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe, und lädt diese in einem neuen Fenster.

   ![Anzeigen der IoT Edge-Projektmappe](./media/how-to-develop-c-module/view-solution.png)

Es gibt vier Elemente in der Projektmappe: 
* Einen Ordner **.vscode**, der Debugkonfigurationen enthält.
* Einen Ordner **modules**, der Unterordner für jedes Modul enthält. An diesem Punkt haben Sie nur einen. Sie können aber über die Befehlspalette mit dem Befehl **Azure IoT Edge: Add IoT Edge Module** weitere hinzufügen. 
* Eine **.env**-Datei, in der Ihre Umgebungsvariablen aufgeführt werden. Wenn Azure Container Registry Ihre Registrierung ist, enthält sie einen Azure Container Registry-Benutzernamen und ein Kennwort. 

   > [!NOTE]
   > Die Umgebungsdatei wird nur erstellt, wenn Sie ein Imagerepository für das Modul angeben. Wenn Sie die „localhost“-Standardeinstellungen zum lokalen Testen und Debuggen akzeptiert haben, müssen Sie keine Umgebungsvariablen deklarieren. 

* Eine Datei **deployment.template.json**, in der Ihr neues Modul zusammen mit dem Beispielmodul **tempSensor**, mit dem Daten zum Testen simuliert werden, aufgeführt ist. Weitere Informationen zur Funktionsweise von Bereitstellungsmanifesten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen mithilfe von Bereitstellungsmanifesten](module-composition.md). 

## <a name="develop-your-module"></a>Entwickeln Ihres Moduls

Der im Lieferumfang der Projektmappe enthaltene standardmäßige C-Modulcode befindet sich unter **modules** > **\<Name Ihres Moduls\>** > **main.c**. Das Modul und die Datei „deployment.template.json“ werden so eingerichtet, dass Sie die Projektmappe erstellen, in Ihre Containerregistrierung verschieben und zum Starten des Tests für ein Gerät bereitstellen können, ohne Code ändern zu müssen. Das Modul ist so konzipiert, dass es einfach eine Eingabe aus einer Quelle akzeptiert (in diesem Fall das Daten simulierende tempSensor-Modul) und IoT Hub übergibt. 

Wenn Sie bereit sind, die C-Vorlage mit Ihrem eigenen Code anzupassen, erstellen Sie mit den [Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md) Module, die die wesentlichen Anforderungen für IoT-Lösungen wie Sicherheit, Geräteverwaltung und Zuverlässigkeit berücksichtigen. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Erstellen und Bereitstellen Ihres Moduls zum Debuggen

In jedem Modulordner gibt es mehrere Docker-Dateien für unterschiedliche Containertypen. Verwenden Sie jede Datei mit der Erweiterung **.debug**, um Ihr Modul zum Testen zu erstellen. C-Module unterstützen derzeit nur das Debuggen in Containern vom Typ „Linux-amd64“.

1. Wechseln Sie in Visual Studio Code zu der Datei `deployment.template.json`. Aktualisieren Sie Ihre Modulimage-URL, indem Sie am Ende **.debug** hinzufügen.

    ![Hinzufügen von **.debug** zum Imagenamen](./media/how-to-develop-c-module/image-debug.png)

2. Ersetzen Sie „createOptions“ für das Node.js-Modul in **deployment.template.json** durch den unten angegebenen Code, und speichern Sie die Datei: 
    
    ```json
    "createOptions": "{\"HostConfig\": {\"Privileged\": true}}"
    ```

2. Geben Sie in der Befehlspalette von VS Code den Befehl **Edge: Build IoT Edge solution** ein, und führen Sie ihn aus.
3. Wählen Sie in der Befehlspalette die Datei `deployment.template.json` für die Projektmappe aus. 
4. Klicken Sie im Azure IoT Hub Device Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID. Wählen Sie dann **Bereitstellung für IoT Edge-Gerät erstellen** aus. 
5. Öffnen Sie den **config**-Ordner Ihrer Projektmappe. Wählen Sie dann die `deployment.json`-Datei aus. Wählen Sie **Edge-Bereitstellungsmanifest auswählen** aus. 

Sie sehen nun, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Überprüfen Sie Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker ps` im Terminal.

## <a name="start-debugging-c-module-in-vs-code"></a>Debuggen des C-Moduls in VS Code
VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird. 

1. Navigieren Sie zur VS Code-Debugansicht. Wählen Sie die Debugkonfigurationsdatei für Ihr Modul aus. Der Name der Debugoption sollte **ModuleName Remote Debug (C)** ähneln.

   ![Auswählen der Debugkonfiguration](./media/how-to-develop-c-module/debug-config.png)zu erstellen und zu verwalten.

2. Navigieren Sie zu `main.c`. Fügen Sie einen Breakpoint in dieser Datei hinzu.

3. Wählen Sie **Debuggen starten** oder **F5** aus. Wählen Sie den Prozess zum Anfügen aus.

4. In der Debugansicht von VS Code sehen Sie die Variablen im linken Bereich. 

Im obigen Beispiel wird veranschaulicht, wie Sie C-IoT Edge-Module in Containern debuggen. Dem createOptions-Modulcontainer wurden verfügbar gemachte Ports hinzugefügt. Nachdem Sie das Debuggen Ihrer Node.js-Module abgeschlossen haben, empfehlen wir Ihnen, diese verfügbar gemachten Ports für IoT Edge-Module, die für die Produktion bereit sind, zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).
