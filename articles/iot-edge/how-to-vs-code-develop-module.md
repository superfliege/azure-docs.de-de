---
title: Entwickeln und Debuggen von Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio Code zum Entwickeln, Erstellen und Debuggen eines Moduls für Azure IoT Edge mithilfe von C#, Python, Node.js, Java oder C
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 12/14/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: fb3d19d35a15d5476594948b035a39ae703f1c3a
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550964"
---
# <a name="use-visual-studio-code-to-develop-and-debug-modules-for-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Debuggen von Modulen für Azure IoT Edge

Sie können Ihre Geschäftslogik in Azure IoT Edge-Module umwandeln. In diesem Artikel wird veranschaulicht, wie Sie Visual Studio Code als wichtigstes Tool zum Entwickeln und Debuggen von Modulen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Sie können einen Computer oder virtuellen Computer unter Windows, macOS oder Linux als Entwicklungscomputer verwenden. Ein IoT Edge-Gerät kann ein anderes physisches Gerät sein.

Für Module, die in C#, Node.js oder Java geschrieben wurden, bestehen zwei Möglichkeiten zum Debuggen in Visual Studio Code: Sie können entweder einen Prozess in einem Modulcontainer anfügen oder den Modulcode im Debugmodus starten. Module, die Python oder C geschrieben wurden, können nur durch Anfügen an einen Prozess in Linux-amd64-Containern debuggt werden.

> [!TIP]
> Wenn Sie mit den Debugfunktionen von Visual Studio Code nicht vertraut sind, informieren Sie sich über [Debuggen](https://code.visualstudio.com/Docs/editor/debugging).

Installieren Sie zuerst [Visual Studio Code](https://code.visualstudio.com/), und fügen Sie dann die folgenden Erweiterungen hinzu:

- [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- [Docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- Spezifische Visual Studio-Erweiterungen für die Sprache, die in der Sie entwickeln:
  - C#, einschließlich Azure Functions: [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  - Python: [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - Java: [Java-Erweiterungspaket für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
  - C: [C/C++-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

Ferner müssen Sie noch einige zusätzliche, sprachspezifische Tools installieren, um Ihr Modul zu entwickeln:

- C#, einschließlich Azure Functions: [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)

- Python: [Python](https://www.python.org/downloads/) und [Pip](https://pip.pypa.io/en/stable/installing/#installation) zum Installieren von Python-Paketen (normalerweise in Ihrer Python-Installation enthalten). Nachdem Pip installiert ist, installieren Sie das **Cookiecutter**-Paket mit dem folgenden Befehl:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

- Node.js: [Node.js](https://nodejs.org). Sie sollten außerdem [Yeoman](https://www.npmjs.com/package/yo) und den [Azure IoT Edge Node.js-Modulgenerator](https://www.npmjs.com/package/generator-azure-iot-edge-module) installieren.

- Java: [Java SE Development Kit 10](https://aka.ms/azure-jdks) und [Maven](https://maven.apache.org/). Sie müssen [die `JAVA_HOME`-Umgebungsvariable so festlegen](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/), dass sie auf Ihre JDK-Installation verweist.

Um Ihr Modulimage zu erstellen und bereitzustellen, müssen Sie Docker das Modulimage erstellen lassen und benötigen eine Containerregistrierung, die das Modulimage aufnimmt:

- [Docker Community Edition](https://docs.docker.com/install/) auf Ihrem Entwicklungscomputer.

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

    > [!TIP]
    > Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden.

Sofern Sie Ihr Modul nicht in C entwickeln, benötigen Sie außerdem das Python-basierte [Azure IoT EdgeHub Dev Tool](https://pypi.org/project/iotedgehubdev/), um Ihre lokale Entwicklungsumgebung für das Debuggen, Ausführen und Testen Ihrer IoT Edge-Lösung einzurichten. Wenn das noch nicht erfolgt ist, installieren Sie [Python (2.7/3.6) und Pip](https://www.python.org/), und installieren Sie anschließend **iotedgehubdev**, indem Sie diesen Befehl in Ihrem Terminal ausführen.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

> [!NOTE]
> Zum Testen Ihres Moduls auf einem Gerät benötigen Sie einen aktiven IoT Hub mit mindestens einem IoT Edge-Gerät. Um Ihren Computer als IoT Edge-Gerät zu verwenden, führen Sie die Schritte in der Schnellstartanleitung für [Linux](quickstart-linux.md) oder [Windows](quickstart.md) aus. Wenn Sie IoT Edge-Daemon auf Ihrem Entwicklungscomputer ausführen, müssen Sie möglicherweise EdgeHub und EdgeAgent beenden, bevor Sie mit nächsten Schritt fortfahren.

## <a name="create-a-new-solution-template"></a>Erstellen einer neuen Lösungsvorlage

Die folgenden Schritte veranschaulichen, wie Sie ein IoT Edge-Modul in Ihrer bevorzugten Entwicklungssprache (einschließlich des in C# geschriebenen Azure Functions) mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung erstellen. Als Erstes erstellen Sie eine Projektmappe, dann generieren Sie das erste Modul in dieser Projektmappe. Jede Projektmappe kann mehrere Module enthalten.

1. Wählen Sie **Ansicht** > **Befehlspalette** aus.

1. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: Neue IoT Edge-Projektmappe** ein.

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

1. Navigieren Sie zu dem Ordner, in dem Sie die neue Projektmappe erstellen möchten, und wählen Sie dann **Ordner auswählen** aus.

1. Geben Sie einen Namen für die Lösung ein.

1. Wählen Sie als erstes Modul in der Projektmappe eine Modulvorlage für Ihre bevorzugte Entwicklungssprache aus.

1. Geben Sie einen Namen für Ihr Modul ein. Wählen Sie einen Namen, der in der Containerregistrierung eindeutig ist.

1. Geben Sie den Namen des Imagerepositorys des Moduls an. Visual Studio Code füllt den Modulnamen automatisch mit **localhost:5000/<Name Ihres Moduls\>** auf. Ersetzen Sie ihn durch Ihre eigenen Registrierungsinformationen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie **localhost** nutzen. Nutzen Sie den Anmeldeserver aus Ihren Registrierungseinstellungen, wenn Sie Azure Container Registry verwenden. Der Anmeldeserver hat die Form ***\<Registrierungsname\>*.azurecr.io**. Ersetzen Sie nur den Teil **localhost:5000** der Zeichenfolge, damit das endgültige Ergebnis die folgende Form hat: **\<* Registrierungsname*\>.azurecr.io/*\<Name Ihres Moduls\>***.

   ![Bereitstellen eines Docker-Imagerepositorys](./media/how-to-develop-csharp-module/repository.png)

Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe, und lädt diese in einem neuen Fenster.

Es gibt vier Elemente in der Projektmappe:

- Einen Ordner **.vscode**, der Debugkonfigurationen enthält.

- Einen Ordner **modules**, der Unterordner für jedes Modul enthält. An diesem Punkt haben Sie nur einen. Sie können aber über die Befehlspalette weitere hinzufügen. Verwenden Sie dazu den Befehl **Azure IoT Edge: Add IoT Edge Module** (Azure IoT Edge: IoT Edge-Modul hinzufügen) aus.

- Eine **.env**-Datei, in der Ihre Umgebungsvariablen aufgeführt werden. Wenn Azure Container Registry Ihre Registrierung ist, enthält sie einen Azure Container Registry-Benutzernamen und ein Kennwort.

  > [!NOTE]
  > Die Umgebungsdatei wird nur erstellt, wenn Sie ein Imagerepository für das Modul angeben. Wenn Sie die „localhost“-Standardeinstellungen zum lokalen Testen und Debuggen akzeptiert haben, müssen Sie keine Umgebungsvariablen deklarieren.

- Eine Datei **deployment.template.json**, in der Ihr neues Modul zusammen mit dem Beispielmodul **tempSensor**, mit dem Daten zum Testen simuliert werden, aufgeführt ist. Weitere Informationen zur Funktionsweise von Bereitstellungsmanifesten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen mithilfe von Bereitstellungsmanifesten](module-composition.md).

## <a name="add-additional-modules"></a>Hinzufügen zusätzlicher Module

Um Ihrer Projektmappe zusätzliche Module hinzuzufügen, führen Sie den Befehl **Azure IoT Edge: IoT Edge-Modul hinzufügen** aus der Befehlspalette aus. Sie können aber auch mit der rechten Maustaste auf den Ordner **Module** oder auf die `deployment.template.json`-Datei in der Exploreransicht von Visual Studio Code klicken und dann **Zu IoT Edge-Module hinzufügen** auswählen.

## <a name="develop-your-module"></a>Entwickeln Ihres Moduls

Der Modulstandardcode, der zum Lieferumfang der Projektmappe gehört, befindet sich an diesem Speicherort:

- Azure Function (C#): **Module > *&lt;Name Ihres Moduls&gt;* > *&lt;Name Ihres Moduls&gt;*.cs**
- C#: **Module > *&lt;Name Ihres Moduls&gt;* > Program.cs**
- Python: **Module > *&lt;Name Ihres Moduls&gt;* > main.py**
- Node.js: **Module > *&lt;Name Ihres Moduls&gt;* > app.js**
- Java: **Module > *&lt;Name Ihres Moduls&gt;* > src > main > java > com > edgemodulemodules > App.java**
- C: **Module > *&lt;Name Ihres Moduls&gt;* > main.c**

Das Modul und die Datei „deployment.template.json“ werden so eingerichtet, dass Sie die Projektmappe erstellen, in Ihre Containerregistrierung verschieben und zum Starten des Tests für ein Gerät bereitstellen können, ohne Code ändern zu müssen. Das Modul ist so konzipiert, dass es einfach eine Eingabe aus einer Quelle akzeptiert (in diesem Fall das Daten simulierende tempSensor-Modul) und IoT Hub übergibt.

Wenn Sie bereit sind, die Vorlage mit Ihrem eigenen Code anzupassen, erstellen Sie mit den [Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md) Module, die die wesentlichen Anforderungen für IoT-Lösungen wie Sicherheit, Geräteverwaltung und Zuverlässigkeit berücksichtigen.

## <a name="debug-a-module-without-a-container-c-nodejs-java"></a>Debuggen eines Moduls ohne einen Container (C#, Node.js, Java)

Wenn Sie in C#, Node.js oder Java entwickeln, ist für Ihr Modul die Verwendung eines **ModuleClient**-Objekts im Standardcode des Moduls erforderlich, damit es gestartet und ausgeführt werden und Nachrichten weiterleiten kann. Sie verwenden außerdem den Standardeingabekanal **input1** für Aktionen, wenn das Modul Nachrichten empfängt.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>Einrichten des IoT Edge-Simulators für eine IoT Edge-Projektmappe

Auf dem Entwicklungscomputer brauchen Sie zum Ausführen der IoT Edge-Projektmappe nicht den IoT Edge-Sicherheits-Daemon zu installieren, sondern können den IoT Edge-Simulator starten.

1. Klicken Sie im Geräte-Explorer auf der linken Seite mit der rechten Maustaste auf Ihre IoT Edge-Geräte-ID, und wählen Sie dann **IoT Edge Simulator einrichten** aus, um den Simulator ohne die Geräteverbindungszeichenfolge zu starten.
1. Sie können sehen, dass der IoT Edge-Simulator erfolgreich eingerichtet wurde, indem Sie die Verlaufsdetails im integrierten Terminal lesen.

### <a name="set-up-iot-edge-simulator-for-single-module-app"></a>Einrichten des IoT Edge-Simulators für die Einzelmodul-App

Um den Simulator einzurichten und zu starten, führen Sie den Befehl **Azure IoT Edge: Edge Hub-Simulator für Einzelmodul starten** in der Visual Studio Code-Befehlspalette aus. Wenn Sie aufgefordert werden, verwenden Sie den Wert **input1** aus dem Modulstandardcode (oder den entsprechenden Wert aus Ihrem Code) als Eingabenamen für Ihre Anwendung. Der Befehl öffnet das **iotedgehubdev**-CLI und startet anschließend den IoT Edge-Simulator und einen Modulcontainer eines Testhilfsprogramms. Die Ausgaben werden unten im integrierten Terminal angezeigt, wenn der Simulator im Einzelmodulmodus erfolgreich gestartet wurde. Angezeigt wird auch ein `curl`-Befehl, über den das Senden von Nachrichten erfolgen kann. Er wird später noch benötigt.

   ![Einrichten des IoT Edge-Simulators für die Einzelmodul-App](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Sie können die Docker Explorer-Ansicht in Visual Studio Code verwenden, um den Ausführungsstatus des Moduls zu sehen.

   ![Modulstatus im Simulator](media/how-to-develop-csharp-module/simulator-status.png)

   Der **EdgeHubDev**-Container ist das Kernstück des lokalen IoT Edge-Simulators. Er kann auf dem Entwicklungscomputer ohne IoT Edge-Sicherheits-Daemon ausgeführt werden und stellt Umgebungseinstellungen für Ihre native Modul-App oder Modulcontainer bereit. Der **input**-Container macht REST APIs verfügbar, um die Übermittlung von Nachrichten an den Zieleingabekanal Ihres Moduls zu unterstützen.

### <a name="debug-module-in-launch-mode"></a>Modul im Startmodus debuggen

1. Bereiten Sie Ihre Umgebung entsprechend den Anforderungen Ihrer Entwicklungssprache für das Debuggen vor, legen Sie einen Haltepunkt in Ihrem Modul fest, und wählen Sie die zu verwendende Debugkonfiguration aus:
   - **C#**
     - Führen Sie im integrierten Terminal von Visual Studio Code einen Verzeichniswechsel zum Ordner ***&lt;Name Ihres Moduls&gt;*** durch, und führen Sie dann den folgenden Befehl aus, um die .Net Core-Anwendung zu erstellen.

       ```cmd
       dotnet build
       ```

     - Öffnen Sie die Datei `program.cs`, und fügen Sie einen Haltepunkt hinzu.

     - Navigieren Sie zur Visual Studio Code-Debugansicht, indem Sie **Ansicht > Debuggen** auswählen. Wählen Sie in der Dropdownliste die Debugkonfiguration ***&lt;Name Ihres Moduls&gt;* Local Debug (.NET Core)** aus.

        > [!NOTE]
        > Wenn Ihr .Net Core-`TargetFramework` nicht mit Ihrem Programmpfad in `launch.json` übereinstimmt, müssen Sie den Programpfad in `launch.json` manuell aktualisieren, damit er zum `TargetFramework` in Ihrer CSPROJ-Datei passt, so dass Visual Studio Code dieses Programm erfolgreich starten kann.

   - **Node.js**
     - Führen Sie im integrierten Terminal von Visual Studio Code einen Verzeichniswechsel zum Ordner ***&lt;Name Ihres Moduls&gt;*** durch, und führen Sie dann den folgenden Befehl aus, um Node-Pakete zu installieren.

       ```cmd
       npm install
       ```

     - Öffnen Sie die Datei `app.js`, und fügen Sie einen Haltepunkt hinzu.

     - Navigieren Sie zur Visual Studio Code-Debugansicht, indem Sie **Ansicht > Debuggen** auswählen. Wählen Sie aus der Dropdownliste die Debugkonfiguration ***&lt;Name Ihres Moduls&gt;* Local Debug (Node.js)** aus.
   - **Java**
     - Öffnen Sie die Datei `App.java`, und fügen Sie einen Haltepunkt hinzu.

     - Navigieren Sie zur Visual Studio Code-Debugansicht, indem Sie **Ansicht > Debuggen** auswählen. Wählen Sie aus der Dropdownliste die Debugkonfiguration ***&lt;Name Ihres Moduls&gt;* Local Debug (Java)** aus.

1. Klicken Sie auf **Debuggen starten**, oder drücken Sie **F5**, um die Debugsitzung zu starten.

1. Führen Sie im integrierten Visual Studio Code-Terminal den folgenden Befehl aus, um die Nachricht **Hallo Welt** an Ihr Modul zu senden. Dies ist der Befehl, der in vorherigen Schritten gezeigt wurde, wenn der IoT Edge-Simulator installiert wurde.

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Wenn Sie Windows verwenden, achten Sie darauf, dass die Shell Ihres integrierten Terminals von Visual Studio Code **Git Bash** oder **WSL Bash** ist. Den `curl`-Befehl können Sie nicht aus einer PowerShell oder Eingabeaufforderung ausführen.
   > [!TIP]
   > Sie können auch [PostMan](https://www.getpostman.com/) oder andere API-Tools (anstelle von `curl`) zum Senden von Nachrichten verwenden.

1. In der Visual Studio Code-Debugansicht werden die Variablen im linken Bereich angezeigt.

1. Um Ihre Debugsitzung zu beenden, wählen Sie die Schaltfläche „Stopp“ aus, oder drücken Sie **UMSCHALT + F5**, und führen Sie dann **Azure IoT Edge: IoT Edge-Simulator beenden** in der Befehlspalette aus, um den Simulator zu beenden und zu bereinigen.

## <a name="debug-in-attach-mode-with-iot-edge-simulator-c-nodejs-java-azure-functions"></a>Debuggen im Anfügemodus mit IoT Edge Simulator (C#, Node.js, Java, Azure Functions)

Ihre Standardprojektmappe enthält zwei Module: ein Modul mit simuliertem Temperatursensor und ein Pipemodul. Der simulierte Temperatursensor sendet Nachrichten an das Pipemodul; anschließend werden die Nachrichten an den IoT Hub geleitet. In dem von Ihnen erstellten Modulordner gibt es mehrere Docker-Dateien für unterschiedliche Containertypen. Verwenden Sie eine der Dateien mit der Erweiterung **.debug**, um Ihr Modul zum Testen zu erstellen.

Aktuell wird Debuggen im Anfügemodus nur in folgender Weise unterstützt:

- C#-Module, einschließlich der für Azure Functions, unterstützen das Debuggen in Linux-amd64-Containern
- Node.js-Module unterstützen das Debuggen in Linux-amd64- und -arm32v7-Containern und in Windows-amd64-Containern
- Java-Module unterstützen das Debuggen in Linux-amd64- und -arm32v7-Containern

> [!TIP]
> Sie können zwischen den Optionen für die Standardplattform für ihre IoT Edge-Projektmappe umschalten, indem Sie in der Visual Studio Code-Statusleiste auf das Element klicken.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>Einrichten des IoT Edge-Simulators für eine IoT Edge-Projektmappe

Auf dem Entwicklungscomputer brauchen Sie zum Ausführen der IoT Edge-Projektmappe nicht den IoT Edge-Sicherheits-Daemon zu installieren, sondern können den IoT Edge-Simulator starten.

1. Klicken Sie im Geräte-Explorer auf der linken Seite mit der rechten Maustaste auf Ihre IoT Edge-Geräte-ID, und wählen Sie dann **IoT Edge Simulator einrichten** aus, um den Simulator ohne die Geräteverbindungszeichenfolge zu starten.

1. Sie können sehen, dass der IoT Edge-Simulator erfolgreich eingerichtet wurde, indem Sie die Verlaufsdetails im integrierten Terminal lesen.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Erstellen und Ausführen eines Containers zum Debuggen und Debuggen im Modus „Anfügen“

1. Öffnen Sie Ihre Moduldatei (`program.cs`, `app.js`, `App.java` oder `<your module name>.cs`), und fügen Sie einen Haltepunkt hinzu.

1. Klicken Sie in der Visual Studio Code-Exploreransicht mit der rechten Maustaste auf die `deployment.debug.template.json`-Datei für Ihre Projektmappe, und wählen Sie dann **IoT Edge-Projektmappe im Simulator erstellen und ausführen** aus. Im gleichen Fenster werden die Protokolle aller Modulcontainer angezeigt. Alternativ können Sie zur Docker-Ansicht navigieren, um den Containerstatus anzuzeigen.

   ![Beachten der Variablen](media/how-to-develop-csharp-module/view-log.png)

1. Navigieren Sie zur Visual Studio Code-Debugansicht, und wählen Sie die Debugkonfigurationsdatei für Ihr Modul aus. Der Name der Debugoption sollte ähnlich lauten wie ***&lt;Name Ihres Moduls&gt;*-Remotedebuggen**

1. Wählen Sie **Debuggen starten** aus, oder drücken Sie **F5**. Wählen Sie den Prozess zum Anfügen aus.

1. In der Visual Studio Code-Debugansicht werden die Variablen im linken Bereich angezeigt.

1. Um die Debugsitzung zu beenden, wählen Sie zunächst die Schaltfläche „Stopp“ aus, oder drücken Sie **UMSCHALT + F5**, und wählen Sie dann **Azure IoT Edge: IoT Edge-Simulator beenden** in der Befehlspalette aus.

> [!NOTE]
> Im obigen Beispiel wird veranschaulicht, wie Sie IoT Edge-Module in Containern debuggen. Dabei wurden den Containereinstellungen Ihres Moduls verfügbar gemachte Ports für `createOptions` hinzugefügt. Nachdem Sie das Debuggen Ihrer Module abgeschlossen haben, empfehlen wir Ihnen, diese verfügbar gemachten Ports für IoT Edge-Module, die für die Produktion bereit sind, zu entfernen.
>
> Für Module, die in C# geschrieben wurden, einschließlich Azure Functions, basiert dieses Beispiel auf der Debugversion von `Dockerfile.amd64.debug`, die den .NET Core-Befehlszeilendebugger (VSDBG) beim Erstellen in Ihr Containerimage einbindet. Nach dem Debuggen Ihrer C#-Module sollten Sie direkt das Dockerfile ohne VSDBG für produktionsbereite IoT Edge-Module verwenden.

## <a name="debug-a-module-with-iot-edge-runtime-python-c"></a>Debuggen eines Moduls mit IoT Edge-Runtime (Python, C)

In jedem Modulordner gibt es mehrere Docker-Dateien für unterschiedliche Containertypen. Verwenden Sie eine der Dateien mit der Erweiterung **.debug**, um Ihr Modul zum Testen zu erstellen.

Derzeit ist Debugunterstützung für Python- und C-Module nur in Linux-amd64-Containern verfügbar.

### <a name="build-and-deploy-your-module"></a>Erstellung und Bereitstellung Ihres Moduls

1. Öffnen Sie in Visual Studio Code die `deployment.debug.template.json`-Datei, die die Debugversion Ihres Modulimages mit den korrekt festgelegten Werten für `createOptions` enthält.

1. Wenn Sie Ihr Modul in Python bereitstellen, führen Sie diese Schritte aus, bevor Sie fortfahren:
   - Öffnen Sie die Datei `main.py`, und fügen Sie hinter dem Importabschnitt diesen Code ein:

      ```python
      import ptvsd
      ptvsd.enable_attach(('0.0.0.0',  5678))
      ```
   - Fügen Sie in den Rückruf, den Sie debuggen möchten, die folgende einzelne Codezeile ein:

      ```python
      ptvsd.break_into_debugger()
      ```

     Wenn Sie beispielsweise die `receive_message_callback`-Methode debuggen möchten, würden Sie diese Codezeile wie unten dargestellt einfügen:

      ```python
      def receive_message_callback(message, hubManager):
          ptvsd.break_into_debugger()
          global RECEIVE_CALLBACKS
          message_buffer = message.get_bytearray()
          size = len(message_buffer)
          print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode ('utf-8'), size) )
          map_properties = message.properties()
          key_value_pair = map_properties.get_internals()
          print ( "    Properties: %s" % key_value_pair )
          RECEIVE_CALLBACKS += 1
          print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
          hubManager.forward_event_to_output("output1", message, 0)
          return IoTHubMessageDispositionResult.ACCEPTED
      ```

1. In der Visual Studio Code-Befehlspalette:
   1. Führen Sie den Befehl **Azure IoT Edge: IoT Edge-Projektmappe erstellen und pushen** aus.

   1. Wählen Sie die `deployment.debug.template.json`-Datei für Ihre Projektmappe aus.

1. Führen Sie im Abschnitt **Azure IoT Hub-Geräte** der Visual Studio Code-Exploreransicht Folgendes aus:
   1. Klicken Sie mit der rechten Maustaste auf eine IoT Edge-Geräte-ID, und wählen Sie dann **Bereitstellung für einzelnes Gerät erstellen** aus.

   1. Navigieren Sie zum Ordner **config** Ihrer Projektmappe, wählen Sie die Datei `deployment.debug.amd64.json` und dann **Edge-Bereitstellungsmanifest auswählen** aus.

Sie sehen nun, dass die Bereitstellung erfolgreich mit einer Bereitstellungs-ID im integrierten Terminal erstellt wurde.

Sie können Ihren Containerstatus in der Visual Studio Code-Docker-Ansicht oder durch Ausführen des Befehls `docker ps` im Terminal überprüfen.

### <a name="debug-your-module"></a>Debuggen Ihres Moduls

Visual Studio Code speichert die Konfigurationsinformationen für das Debuggen in einer Datei `launch.json`, die sich in einem Ordner `.vscode` in Ihrem Arbeitsbereich befindet. Die Datei `launch.json` wurde beim Erstellen einer neuen IoT Edge-Lösung generiert. Sie wird jedes Mal aktualisiert, wenn Sie ein neues Modul hinzufügen, für das Debuggen unterstützt wird.

1. Wählen Sie in der Visual Studio Code-Debugansicht die Debugkonfigurationsdatei für Ihr Modul aus. Der Name der Debugoption sollte ähnlich lauten wie ***&lt;Name Ihres Moduls&gt;*-Remotedebuggen**

1. Öffnen Sie die Moduldatei für Ihre Entwicklungssprache, und fügen Sie einen Haltepunkt hinzu:
   - **Python**: Öffnen Sie `main.py`, und fügen Sie einen Haltepunkt in der Rückrufmethode hinzu, der Sie die Zeile `ptvsd.break_into_debugger()` hinzugefügt hatten.
   - **C**: Öffnen Sie die Datei `main.c`, und fügen Sie einen Haltepunkt hinzu.

1. Wählen Sie **Debuggen starten** oder **F5** aus. Wählen Sie den Prozess zum Anfügen aus.

1. In der Visual Studio Code-Debugansicht werden die Variablen im linken Bereich angezeigt.

> [!NOTE]
> Im obigen Beispiel wird veranschaulicht, wie Sie IoT Edge-Module in Containern debuggen. Dabei wurden den Containereinstellungen Ihres Moduls verfügbar gemachte Ports für `createOptions` hinzugefügt. Nachdem Sie das Debuggen Ihrer Module abgeschlossen haben, empfehlen wir Ihnen, diese verfügbar gemachten Ports für IoT Edge-Module, die für die Produktion bereit sind, zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Modul erstellt haben, erfahren Sie, wie Sie [Azure IoT Edge-Module aus Visual Studio Code bereitstellen](how-to-deploy-modules-vscode.md).

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).
