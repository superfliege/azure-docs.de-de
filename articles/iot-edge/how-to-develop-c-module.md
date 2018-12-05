---
title: Debuggen von C-Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Entwickeln, Erstellen und Debuggen eines C-Moduls für Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/13/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: b8d8223647d42213eff53c2ff8310bed0cfe6cdb
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446737"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Debuggen von C-Modulen für Azure IoT Edge

Sie können Ihre Geschäftslogik in Azure IoT Edge-Module umwandeln. In diesem Artikel wird veranschaulicht, wie Sie Visual Studio Code (VS Code) als wichtigstes Tool zum Entwickeln und Debuggen von C-Modulen verwenden.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Darüber hinaus simulieren Sie Ihr IoT Edge-Gerät auf Ihrem Entwicklungscomputer mit IoT Edge-Sicherheits-Daemon.

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
   * Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden. 

Um das Modul auf einem Gerät zu testen, benötigen Sie einen aktiven IoT Hub mit mindestens einem IoT Edge-Gerät. Um Ihren Computer als IoT Edge-Gerät zu verwenden, führen Sie die Schritte in der Schnellstartanleitung für [Linux](quickstart-linux.md) aus. 

## <a name="create-a-new-solution-template"></a>Erstellen einer neuen Lösungsvorlage

Erstellen Sie in diesen Schritten mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis des Azure IoT C SDK. Als Erstes erstellen Sie eine Projektmappe, und dann generieren Sie das erste Modul in dieser Projektmappe. Jede Projektmappe kann mehrere Module enthalten. 

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Integriertes Terminal**.

2. Wählen Sie **Ansicht** > **Befehlspalette** aus. 

3. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus.

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

4. Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll. Wählen Sie **Ordner auswählen** aus. 

5. Geben Sie einen Namen für die Lösung ein. 

6. Wählen Sie **C Module** als Vorlage für das erste Modul in der Projektmappe aus.

7. Geben Sie einen Namen für Ihr Modul ein. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist. 

8. Geben Sie den Namen des Imagerepositorys des Moduls an. VS Code trägt als Modulnamen automatisch **localhost:5000** ein. Ersetzen Sie ihn durch Ihre eigenen Registrierungsinformationen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie **localhost** nutzen. Nutzen Sie den Anmeldeserver aus Ihren Registrierungseinstellungen, wenn Sie Azure Container Registry verwenden. Der Anmeldeserver hat die Form **\<Registrierungsname\>.azurecr.io**. Ersetzen Sie nur den localhost-Teil der Zeichenfolge, löschen Sie nicht Ihren Modulnamen. Die endgültige Zeichenfolge sieht wie folgt aus: \<registrierungsname\>.azurecr.io/\<modulname\>.

   ![Bereitstellen eines Docker-Imagerepositorys](./media/how-to-develop-c-module/repository.png)

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

Der in der Projektmappe enthaltene C#-Standardmodulcode befindet sich unter **modules** > [Name Ihres Moduls] > **main.c**. Das Modul und die Datei „deployment.template.json“ werden so eingerichtet, dass Sie die Projektmappe erstellen, in Ihre Containerregistrierung verschieben und zum Starten des Tests für ein Gerät bereitstellen können, ohne Code ändern zu müssen. Das Modul ist so konzipiert, dass es einfach eine Eingabe aus einer Quelle akzeptiert (in diesem Fall das Daten simulierende tempSensor-Modul) und IoT Hub übergibt. 

Wenn Sie bereit sind, die C-Vorlage mit Ihrem eigenen Code anzupassen, erstellen Sie mit den [Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md) Module, die die wesentlichen Anforderungen für IoT-Lösungen wie Sicherheit, Geräteverwaltung und Zuverlässigkeit berücksichtigen. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Erstellen und Bereitstellen Ihres Moduls zum Debuggen

In jedem Modulordner gibt es mehrere Docker-Dateien für unterschiedliche Containertypen. Verwenden Sie jede Datei mit der Erweiterung **.debug**, um Ihr Modul zum Testen zu erstellen. C-Module unterstützen derzeit nur das Debuggen in Containern vom Typ „Linux-amd64“.

1. Wechseln Sie in Visual Studio Code zu der Datei `deployment.debug.template.json`. Diese Datei enthält die Debugversion Ihres Modulimages mit den richtigen Erstellungsoptionen. 
2. Geben Sie in der Befehlspalette von Visual Studio Code den Befehl **Azure IoT Edge: Build and Push IoT Edge solution** ein, und führen Sie ihn aus.
3. Wählen Sie in der Befehlspalette die Datei `deployment.debug.template.json` für die Projektmappe aus. 
4. Klicken Sie im Azure IoT Hub Device Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID. Klicken Sie dann auf **Create Deployment for Single Device** (Bereitstellung für einzelnes Gerät erstellen). 
5. Öffnen Sie den **config**-Ordner Ihrer Projektmappe. Wählen Sie dann die `deployment.debug.amd64.json`-Datei aus. Wählen Sie **Edge-Bereitstellungsmanifest auswählen** aus. 

Sie sehen nun, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Überprüfen Sie Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker ps` im Terminal.

## <a name="start-debugging-c-module-in-vs-code"></a>Debuggen des C-Moduls in VS Code
VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird. 

1. Navigieren Sie zur VS Code-Debugansicht. Wählen Sie die Debugkonfigurationsdatei für Ihr Modul aus. Der Name der Debugoption sollte **ModuleName Remote Debug (C)** ähneln.

   ![Auswählen der Debugkonfiguration](./media/how-to-develop-c-module/debug-config.png)

2. Navigieren Sie zu `main.c`. Fügen Sie einen Breakpoint in dieser Datei hinzu.

3. Wählen Sie **Debuggen starten** oder **F5** aus. Wählen Sie den Prozess zum Anfügen aus.

4. In der Debugansicht von VS Code sehen Sie die Variablen im linken Bereich. 

Im obigen Beispiel wird veranschaulicht, wie Sie C-IoT Edge-Module in Containern debuggen. Dem createOptions-Modulcontainer wurden verfügbar gemachte Ports hinzugefügt. Nachdem Sie das Debuggen Ihrer C-Module abgeschlossen haben, empfehlen wir Ihnen, diese verfügbar gemachten Ports für IoT Edge-Module, die für die Produktion bereit sind, zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).
