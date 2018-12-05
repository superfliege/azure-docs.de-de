---
title: Debuggen von Azure-Funktionsmodulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Debuggen von Azure-Funktionen für C# mit Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 40b8dfef3100ff8440165de74fb41f6b36afe37a
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315102"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Debuggen von Azure-Funktionen für Azure IoT Edge

In diesem Artikel erfahren Sie, wie Sie mit [Visual Studio Code (VS Code)](https://code.visualstudio.com/) Ihre Azure-Funktionen unter Azure IoT Edge debuggen.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Ihr IoT Edge-Gerät kann ein anderes physisches Gerät sein. Alternativ können Sie ein IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

> [!NOTE]
> In diesem Debugartikel werden das Anfügen eines Prozesses in einem Modulcontainer und das Debuggen mit Visual Studio Code beschrieben. Sie können nur C#-Funktionen in Containern vom Typ „Linux amd64“ debuggen. Wenn Sie mit den Debugfunktionen von Visual Studio Code nicht vertraut sind, informieren Sie sich über [Debuggen](https://code.visualstudio.com/Docs/editor/debugging). 

In diesem Artikel wird Visual Studio Code als das Hauptentwicklungstool verwendet. Installieren Sie VS Code. Fügen Sie dann die erforderlichen Erweiterungen hinzu: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Um ein Modul zu erstellen, benötigen Sie .NET zum Erstellen des Projektordners, Docker zum Erstellen des Modulimages und eine Containerregistrierung zum Speichern des Modulimages:

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)
* [Docker Community Edition](https://docs.docker.com/install/) auf Ihrem Entwicklungscomputer 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden. 

Um das Modul auf einem Gerät zu testen, benötigen Sie einen aktiven IoT Hub mit mindestens einem IoT Edge-Gerät. Um Ihren Computer als IoT Edge-Gerät zu verwenden, führen Sie die Schritte in der Schnellstartanleitung für [Windows](quickstart.md) oder [Linux](quickstart-linux.md) aus. 

## <a name="create-a-new-solution-template"></a>Erstellen einer neuen Lösungsvorlage

Erstellen Sie mit diesen Schritten eine IoT Edge-Projektmappe mit einem C#-Funktionsmodul. Jede Projektmappe kann mehrere Module haben.

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Integriertes Terminal**.
3. Wählen Sie **Ansicht** > **Befehlspalette** aus.
4. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus. 

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

5. Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll. Wählen Sie **Ordner auswählen** aus. 
6. Geben Sie einen Namen für die Lösung ein. 
7. Wählen Sie **Azure Functions – C#** als Vorlage für das erste Modul in der Projektmappe aus.
8. Geben Sie einen Namen für Ihr Modul ein. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist. 
9. Geben Sie das Imagerepository für das Modul an. VS Code trägt als Modulnamen automatisch **localhost:5000** ein. Ersetzen Sie ihn durch Ihre eigenen Registrierungsinformationen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie **localhost** nutzen. Nutzen Sie den Anmeldeserver aus Ihren Registrierungseinstellungen, wenn Sie Azure Container Registry verwenden. Der Anmeldeserver hat die Form **\<Registrierungsname\>.azurecr.io**. Ersetzen Sie nur den localhost-Teil der Zeichenfolge, löschen Sie nicht Ihren Modulnamen.

   ![Bereitstellen eines Docker-Imagerepositorys](./media/how-to-develop-csharp-function/repository.png)

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe mit einem Azure Functions-Projekt und lädt diese in einem neuen Fenster.

Es gibt vier Elemente in der Projektmappe: 

* Einen Ordner **.vscode**, der Debugkonfigurationen enthält.
* Einen Ordner **modules**, der Unterordner für jedes Modul enthält. An diesem Punkt haben Sie nur einen. Sie können aber über die Befehlspalette mit dem Befehl **Azure IoT Edge: Add IoT Edge Module** weitere hinzufügen.
* Eine **.env**-Datei, in der Ihre Umgebungsvariablen aufgeführt werden. Wenn Azure Container Registry Ihre Registrierung ist, enthält sie einen Azure Container Registry-Benutzernamen und ein Kennwort. 

   >[!NOTE]
   >Die Umgebungsdatei wird nur erstellt, wenn Sie ein Imagerepository für das Modul angeben. Wenn Sie die „localhost“-Standardeinstellungen zum lokalen Testen und Debuggen akzeptiert haben, müssen Sie keine Umgebungsvariablen deklarieren. 

* Eine Datei **deployment.template.json**, in der Ihr neues Modul zusammen mit dem Beispielmodul **tempSensor**, mit dem Daten zum Testen simuliert werden, aufgeführt ist. Weitere Informationen zur Funktionsweise von Bereitstellungsmanifesten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen mithilfe von Bereitstellungsmanifesten](module-composition.md).
* Eine **deployment.debug.template.json**-Datei enthält die Debugversion Ihrer Modulimages mit den entsprechenden Containeroptionen.

## <a name="develop-your-module"></a>Entwickeln Ihres Moduls

Der im Lieferumfang der Projektmappe enthaltene standardmäßige Azure-Funktionscode befindet sich unter**modules** > [Name Ihres Moduls] > **modulname.cs**. Das Modul und die Datei „deployment.template.json“ werden so eingerichtet, dass Sie die Projektmappe erstellen, in Ihre Containerregistrierung verschieben und zum Starten des Tests für ein Gerät bereitstellen können, ohne Code ändern zu müssen. Das Modul ist so konzipiert, dass es einfach eine Eingabe aus einer Quelle akzeptiert (in diesem Fall das Daten simulierende tempSensor-Modul) und IoT Hub übergibt. 

Wenn Sie bereit sind, die Azure Function-Vorlage mit Ihrem eigenen Code anzupassen, erstellen Sie mit den [Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md) Module, die die wesentlichen Anforderungen für IoT-Lösungen wie Sicherheit, Geräteverwaltung und Zuverlässigkeit berücksichtigen. 

## <a name="build-your-module-for-debugging"></a>Erstellen Ihres Moduls zum Debuggen
1. Um mit dem Debuggen zu beginnen, müssen Sie mithilfe von **Dockerfile.amd64.debug** Ihr Docker-Image neu erstellen und Ihre Edge-Lösung erneut bereitstellen. Navigieren Sie im VS Code-Explorer zur Datei `deployment.debug.template.json`.
2. Erstellen Sie Ihre Lösung neu. Geben Sie in der Befehlspalette von Visual Studio Code den Befehl **Azure IoT Edge: Build IoT Edge solution** ein, und führen Sie ihn aus.
3. Wählen Sie in der Befehlspalette die Datei `deployment.debug.template.json` für die Projektmappe aus. 
4. Klicken Sie im Azure IoT Hub-Geräte-Explorer mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Bereitstellung für Edge-Gerät erstellen** aus. Wählen Sie die Datei `deployment.debug.amd64.json` im Ordner `config` aus. Sie sehen nun, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal von VS Code erstellt wurde.

Überprüfen Sie Ihren Containerstatus im VS Code-Docker-Explorer oder durch Ausführen des Befehls `docker ps` im Terminal.

## <a name="start-debugging-c-functions-in-vs-code"></a>Starten des Debuggens von C#-Funktionen in VS Code
1. VS Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird. Navigieren Sie zur Debugansicht. Wählen Sie die entsprechende Debugkonfigurationsdatei aus. Der Name der Debugoption sollte **ModuleName Remote Debug (.NET Core)** ähneln.

   ![Auswählen der Debugkonfiguration](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navigieren Sie zu `modulename.cs`. Fügen Sie in der Funktion einen Haltepunkt hinzu.
3. Wählen Sie **Debuggen starten** oder **F5** aus. Wählen Sie den Prozess zum Anfügen aus.
4. In der Debugansicht von VS Code sehen Sie die Variablen im linken Bereich. 


> [!NOTE]
> Das Beispiel zeigt, wie Sie .Net Core IoT Edge-Funktionen in Containern debuggen. Es basiert auf der Debugversion von `Dockerfile.amd64.debug`, die den .NET Core-Befehlszeilendebugger VSDBG beim Erstellen in Ihr Containerimage einbindet. Es wird empfohlen, für produktionsbereite IoT Edge-Module `Dockerfile` ohne VSDBG direkt zu verwenden oder anzupassen, nachdem Sie das Debuggen Ihrer C#-Module abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).
