---
title: Debuggen von C#-Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Entwickeln, Erstellen und Debuggen eines C#-Moduls für Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/27/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 88659d31b64b4a98043606a71602f7c29316a31e
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423288"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Debuggen von C#-Modulen für Azure IoT Edge

Sie können Ihre Geschäftslogik in Azure IoT Edge-Module umwandeln. In diesem Artikel wird veranschaulicht, wie Sie Visual Studio Code (VS Code) als wichtigstes Tool zum Entwickeln und Debuggen von C#-Modulen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Sie können einen Computer oder virtuellen Computer unter Windows, macOS oder Linux als Entwicklungscomputer verwenden. Ein IoT Edge-Gerät kann ein anderes physisches Gerät sein.

Es gibt zwei Möglichkeiten zum Debuggen Ihres C#-Moduls in VS Code. Eine Möglichkeit ist das Anfügen eines Prozesses in einem Modulcontainer, die andere das Starten des Modulcodes im Debugmodus. Wenn Sie mit den Debugfunktionen von Visual Studio Code nicht vertraut sind, informieren Sie sich über [Debuggen](https://code.visualstudio.com/Docs/editor/debugging).

Installieren Sie zuerst Visual Studio Code. Fügen Sie dann die folgenden erforderlichen Erweiterungen hinzu:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Um ein Modul zu erstellen, benötigen Sie .NET zum Erstellen des Projektordners, Docker zum Erstellen des Modulimages und eine Containerregistrierung zum Speichern des Modulimages:
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker Community Edition](https://docs.docker.com/install/) auf Ihrem Entwicklungscomputer. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden. 

Zum Einrichten der lokalen Entwicklungsumgebung, in der Sie Ihre IoT Edge-Projektmappe debuggen, ausführen und testen, benötigen Sie das [Entwicklertool für Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Installieren Sie [Python (2.7/3.6) und Pip](https://www.python.org/). Installieren Sie anschließend **iotedgehubdev**, indem Sie den folgenden Befehl in Ihrem Terminal ausführen.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Um das Modul auf einem Gerät zu testen, benötigen Sie einen aktiven IoT Hub mit mindestens einer erstellten IoT Edge-Geräte-ID. Wenn Sie IoT Edge-Daemon auf dem Entwicklungscomputer ausführen, müssen Sie möglicherweise EdgeHub und EdgeAgent beenden, bevor Sie mit nächsten Schritt fortfahren. 

## <a name="create-a-new-solution-with-c-module"></a>Erstellen einer neuen Projektmappe mit dem C#-Modul

Erstellen Sie in diesen Schritten mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis von .NET Core 2.1. Als Erstes erstellen Sie eine Projektmappe, und dann generieren Sie das erste Modul in dieser Projektmappe. Jede Projektmappe kann mehrere Module enthalten. 

1. Klicken Sie in Visual Studio Code auf **Ansicht** > **Integriertes Terminal**.
2. Wählen Sie **Ansicht** > **Befehlspalette** aus. 
3. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge Solution** ein, und führen Sie ihn aus.

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

4. Navigieren Sie zu dem Ordner, in dem die neue Projektmappe erstellt werden soll. Wählen Sie **Ordner auswählen** aus. 
5. Geben Sie einen Namen für die Lösung ein. 
6. Wählen Sie **C# Module** als Vorlage für das erste Modul in der Projektmappe aus.
7. Geben Sie einen Namen für Ihr Modul ein. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist. 
8. Geben Sie den Namen des Imagerepositorys des Moduls an. VS Code trägt als Modulnamen automatisch **localhost:5000** ein. Ersetzen Sie ihn durch Ihre eigenen Registrierungsinformationen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie **localhost** nutzen. Nutzen Sie den Anmeldeserver aus Ihren Registrierungseinstellungen, wenn Sie Azure Container Registry verwenden. Der Anmeldeserver hat die Form **\<Registrierungsname\>.azurecr.io**. Ersetzen Sie nur den localhost-Teil der Zeichenfolge, löschen Sie nicht Ihren Modulnamen.

   ![Bereitstellen eines Docker-Imagerepositorys](./media/how-to-develop-csharp-module/repository.png)

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe, und lädt diese in einem neuen Fenster.

   ![Anzeigen der IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/view-solution.png)

Es gibt vier Elemente in der Projektmappe: 

* Einen Ordner **.vscode**, der Debugkonfigurationen enthält.
* Einen Ordner **modules**, der Unterordner für jedes Modul enthält. An diesem Punkt haben Sie nur einen. Sie können aber über die Befehlspalette mit dem Befehl **Azure IoT Edge: Add IoT Edge Module** weitere hinzufügen. 
* Eine **.env**-Datei, in der Ihre Umgebungsvariablen aufgeführt werden. Wenn Azure Container Registry Ihre Registrierung ist, enthält sie einen Azure Container Registry-Benutzernamen und ein Kennwort. 

   >[!NOTE]
   >Die Umgebungsdatei wird nur erstellt, wenn Sie ein Imagerepository für das Modul angeben. Wenn Sie die „localhost“-Standardeinstellungen zum lokalen Testen und Debuggen akzeptiert haben, müssen Sie keine Umgebungsvariablen deklarieren. 

* Eine Datei **deployment.template.json**, in der Ihr neues Modul zusammen mit dem Beispielmodul **tempSensor**, mit dem Daten zum Testen simuliert werden, aufgeführt ist. Weitere Informationen zur Funktionsweise von Bereitstellungsmanifesten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen mithilfe von Bereitstellungsmanifesten](module-composition.md). 

## <a name="develop-your-module"></a>Entwickeln Ihres Moduls

Der in der Projektmappe enthaltene C#-Standardmodulcode befindet sich unter **modules** > ** [Name Ihres Moduls] ** > **Program.cs**. Das Modul und die Datei „deployment.template.json“ werden so eingerichtet, dass Sie die Projektmappe erstellen, in Ihre Containerregistrierung verschieben und zum Starten des Tests für ein Gerät bereitstellen können, ohne Code ändern zu müssen. Das Modul ist so konzipiert, dass es einfach eine Eingabe aus einer Quelle akzeptiert (in diesem Fall das Daten simulierende tempSensor-Modul) und IoT Hub übergibt. 

Wenn Sie bereit sind, die C#-Vorlage mit Ihrem eigenen Code anzupassen, erstellen Sie mit den [Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md) Module, die die wesentlichen Anforderungen für IoT-Lösungen wie Sicherheit, Geräteverwaltung und Zuverlässigkeit berücksichtigen. 

Die C#-Unterstützung in Visual Studio Code ist für die plattformübergreifende .NET Core-Entwicklung optimiert. Unter [Gewusst wie: Arbeiten mit C# in Visual Studio Code](https://code.visualstudio.com/docs/languages/csharp) finden Sie weitere Informationen.

## <a name="launch-and-debug-module-code-without-container"></a>Starten und Debuggen von Modulcode ohne Container

Das IoT Edge-C#-Modul ist eine .NET Core-Anwendung. und abhängig vom Azure IoT-C#-Geräte-SDK. Weil das IoT C#-Modul die Umgebungseinstellungen zum Starten und Ausführen benötigt, initialisieren Sie im Standardmodulcode ein **ModuleClient**-Objekt mit Umgebungseinstellungen und Eingabename. Außerdem benötigen Sie zum Senden oder Weiterleiten von Nachrichten die Eingabekanäle. Ihr C#-Standardmodul enthält nur einen Eingabekanal, dessen Name **input1** lautet.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>Einrichten des IoT Edge-Simulators für eine Einzelmodul-App

1. Geben Sie zum Einrichten und Starten des Simulators in der VS Code-Befehlspalette den Befehl **Azure IoT Edge: Start IoT Edge Hub Simulator for Single Module** ein, und führen Sie ihn aus. Sie müssen auch den Eingabenamen für die Einzelmodulanwendung angeben. Geben Sie **input1** ein, und drücken Sie die EINGABETASTE. Der Befehl löst die Befehlszeilenschnittstelle **Iotedgehubdev** aus und startet den IoT Edge-Simulator sowie ein Testprogramm für den Modulcontainer. Die Ausgaben werden unten im integrierten Terminal angezeigt, wenn der Simulator im Einzelmodulmodus erfolgreich gestartet wurde. Angezeigt wird auch ein `curl`-Befehl, über den das Senden von Nachrichten erfolgen kann. Er wird später noch benötigt.

   ![Einrichten des IoT Edge-Simulators für eine Einzelmodul-App](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Sie können zu Docker-Explorer wechseln und den Ausführungsstatus des Moduls anzeigen.

   ![Modulstatus im Simulator](media/how-to-develop-csharp-module/simulator-status.png)

   Der **EdgeHubDev**-Container ist das Kernstück des lokalen IoT Edge-Simulators. Er kann auf dem Entwicklungscomputer ohne IoT Edge-Sicherheits-Daemon ausgeführt werden und stellt Umgebungseinstellungen für Ihre native Modul-App oder Modulcontainer bereit. Der **Eingabe**-Container macht RestAPIs verfügbar, um Nachrichten an den Zieleingabekanal Ihres Moduls zu übermitteln.

2. Geben Sie in der VS Code-Befehlspalette den Befehl **Azure IoT Edge: Set Module Credentials to User Settings** ein, und führen Sie ihn aus, um die Umgebungseinstellungen des Moduls auf `azure-iot-edge.EdgeHubConnectionString` und `azure-iot-edge.EdgeModuleCACertificateFile` in den Benutzereinstellungen festzulegen. Sie werden feststellen, dass diese Umgebungseinstellungen auch in **.vscode** > **launch.json** und den [VS Code-Benutzereinstellungen](https://code.visualstudio.com/docs/getstarted/settings) referenziert werden.

### <a name="build-module-app-and-debug-in-launch-mode"></a>Erstellen der Modul-App und Debuggen im Startmodus

1. Wählen Sie im integrierten Terminal den Ordner **CSharpModule** als neues Verzeichnis aus, und führen Sie den folgenden Befehl aus, um die .NET Core-Anwendung zu erstellen.

    ```cmd
    dotnet build
    ```

2. Navigieren Sie zu `program.cs`. Fügen Sie einen Breakpoint in dieser Datei hinzu.

3. Navigieren Sie zur Visual Studio Code-Debugansicht: „Ansicht“ > „Debuggen“. Wählen Sie aus der Dropdownliste die Debugkonfiguration **ModuleName Local Debug (.NET Core)** aus. 

  ![Wechseln in den Debugmodus in VS Code](media/how-to-develop-csharp-module/debug-view.png)

4. Klicken Sie auf **Debuggen starten**, oder drücken Sie **F5**. Dadurch wird die Debugsitzung gestartet.

   > [!NOTE]
   > Wenn Ihr .Net Core-`TargetFramework` mit Ihrem Programmpfad in `launch.json` nicht übereinstimmt, müssen Sie den Programmpfad in `launch.json` manuell aktualisieren, damit er das `TargetFramework` in Ihrer CSPROJ-Datei berücksichtigt. Dann kann der VS Code dieses Programm erfolgreich starten.

5. Führen Sie im integrierten VS Code-Terminal den folgenden Befehl aus, um die Nachricht **Hello World** an Ihr Modul zu senden. Dies ist der Befehl, der in vorherigen Schritten gezeigt wurde, wenn der IoT Edge-Simulator erfolgreich installiert wurde.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Stellen Sie unter Windows sicher, dass **Git Bash** oder **WSL Bash** als Shell für Ihr integriertes VS Code-Terminal verwendet wird. Den `curl`-Befehl können Sie nicht in PowerShell oder an der Eingabeaufforderung ausführen. 
   
   > [!TIP]
   > Sie können auch [PostMan](https://www.getpostman.com/) oder andere API-Tools (anstelle von `curl`) zum Senden von Nachrichten verwenden.

6. In der Debugansicht von VS Code sehen Sie die Variablen im linken Bereich. 

    ![Beachten der Variablen](media/how-to-develop-csharp-module/single-module-variables.png)

7. Um die Debugsitzung zu beenden, klicken Sie auf die Schaltfläche „Beenden“, oder drücken Sie **UMSCHALT+F5**. Geben Sie in der VS Code-Befehlspalette den Befehl **Azure IoT Edge: Stop IoT Edge Simulator** ein, und führen Sie ihn aus, um den Simulator zu beenden und zu bereinigen.

## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Erstellen eines Modulcontainers zum Debuggen und Debuggen im Modus „Anfügen“

Ihre Standardprojektmappe enthält zwei Module: ein Modul mit simuliertem Temperatursensor und ein C#-Pipemodul. Der simulierte Temperatursensor sendet beständig Nachrichten an das C#-Pipemodul; anschließend werden die Nachrichten an den IoT Hub geleitet. In dem von Ihnen erstellten Modulordner gibt es mehrere Docker-Dateien für unterschiedliche Containertypen. Verwenden Sie jede Datei mit der Erweiterung **.debug**, um Ihr Modul zum Testen zu erstellen. C#-Module unterstützen derzeit das Debuggen nur Linux-amd64-Containern im Modus „Anfügen“.

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>Einrichten des IoT Edge-Simulators für IoT Edge-Projektmappe

Auf dem Entwicklungscomputer brauchen Sie zum Ausführen der IoT Edge-Projektmappe nicht den IoT Edge-Sicherheits-Daemon zu installieren, sondern können den IoT Edge-Simulator starten. 

1. Klicken Sie im Geräte-Explorer auf der linken Seite mit der rechten Maustaste auf die IoT Edge-Geräte-ID, und wählen Sie **IoT Edge-Simulator einrichten** aus, um den Simulator mit der Verbindungszeichenfolge des Geräts zu starten.

2. Im integrierten Terminal können Sie sehen, dass der IoT Edge-Simulator erfolgreich eingerichtet wurde.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Erstellen und Ausführen eines Containers zum Debuggen und Debuggen im Modus „Anfügen“

1. Wechseln Sie in Visual Studio Code zu der Datei `deployment.template.json`. Aktualisieren Sie die URL Ihres C#-Modulimages, indem Sie am Ende **.debug** hinzufügen.

   ![Hinzufügen von **.debug** zum Imagenamen](./media/how-to-develop-csharp-module/image-debug.png)

2. Navigieren Sie zu `program.cs`. Fügen Sie einen Breakpoint in dieser Datei hinzu.

3. Wählen Sie im VS Code-Datei-Explorer die Datei `deployment.template.json` für Ihre Projektmappe aus, und klicken Sie im Kontextmenü auf **IoT Edge-Projektmappe im Simulator erstellen und ausführen**. Im gleichen Fenster werden die Protokolle aller Modulcontainer angezeigt. Sie können auch zu Docker-Explorer navigieren, um den Containerstatus zu beobachten.

   ![Beachten der Variablen](media/how-to-develop-csharp-module/view-log.png)

4. Navigieren Sie zur VS Code-Debugansicht. Wählen Sie die Debugkonfigurationsdatei für Ihr Modul aus. Der Name der Debugoption sollte **ModuleName Remote Debug (.NET Core)** ähneln.

   ![Auswählen von „Konfiguration“](media/how-to-develop-csharp-module/debug-config.png)

5. Wählen Sie **Debuggen starten** oder **F5** aus. Wählen Sie den Prozess zum Anfügen aus.

6. In der Debugansicht von VS Code sehen Sie die Variablen im linken Bereich.

7. Um die Debugsitzung zu beenden, klicken Sie auf die Schaltfläche „Beenden“, oder drücken Sie **UMSCHALT+F5**. Geben Sie in der VS Code-Befehlspalette den Befehl **Azure IoT Edge: Stop IoT Edge Simulator** ein, und führen Sie ihn aus.

    > [!NOTE]
    > Das Beispiel zeigt, wie Sie .NET Core IoT Edge-Module in Containern debuggen. Es basiert auf der Debugversion von `Dockerfile.debug`, die den Visual Studio .NET Core-Befehlszeilendebugger (VSDBG) beim Erstellen in Ihr Containerimage einbindet. Nach dem Debuggen Ihrer C#-Module sollten Sie direkt `Dockerfile` ohne VSDBG für produktionsbereite IoT Edge-Module verwenden oder anpassen.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).
