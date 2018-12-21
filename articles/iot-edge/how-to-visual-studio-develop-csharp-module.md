---
title: Entwickeln und Debuggen von C#-Modulen in Visual Studio – Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden von Visual Studio 2017 zum Entwickeln und Debuggen von C#-Modulen für Azure IoT Edge
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/24/2018
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 547989152320678ec195c4e8a93965cfbbd0f341
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097844"
---
# <a name="use-visual-studio-2017-to-develop-and-debug-c-modules-for-azure-iot-edge-preview"></a>Verwenden von Visual Studio 2017 zum Entwickeln und Debuggen von C#-Modulen für Azure IoT Edge (Vorschauversion)

Sie können Ihre Geschäftslogik in Azure IoT Edge-Module umwandeln. In diesem Artikel wird gezeigt, wie Sie Visual Studio 2017 als wichtigstes Tool zum Entwickeln und Debuggen von C#-Modulen verwenden.

Die Azure IoT Edge-Tools für Visual Studio bietet die folgenden Vorteile:

- Sie können Azure IoT Edge-Lösungen und -Module auf Ihrem lokalen Entwicklungscomputer erstellen, bearbeiten, kompilieren, ausführen und debuggen.
- Sie können Ihre Azure IoT Edge-Lösung über den Azure IoT Hub auf einem Azure IoT Edge-Gerät bereitstellen.
- Sie können Ihre Azure IoT-Module in C# kodieren, während Sie von allen Vorteilen der Entwicklung mit Visual Studio profitieren.
- Sie können Ihre Azure IoT Edge-Geräte und -Module in der Benutzeroberfläche verwalten. 

In diesem Artikel wird gezeigt, wie Sie mithilfe von Azure IoT Edge-Tools für Visual Studio 2017 Ihre IoT Edge-Module in C# entwickeln. Außerdem erfahren Sie, wie Sie Ihr Projekt auf Ihrem Azure IoT Edge-Gerät bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows als Entwicklungscomputer verwenden. Ihr IoT Edge-Gerät kann ein anderes physisches Gerät sein.

Weil in diesem Artikel Visual Studio 2017 als Hauptentwicklungstool verwendet wird, installieren Sie Visual Studio. Stellen Sie außerdem sicher, dass Sie den **Azure-Entwicklungsworkload** in der Installation von Visual Studio 2017 einschließen. Sie können [Visual Studio 2017 ändern](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) und den Azure-Entwicklungsworkload hinzufügen.

Wenn Ihr Visual Studio 2017 bereit ist, ist noch Folgendes erforderlich:

- Laden Sie die [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) vom Visual Studio Marketplace herunter und installieren Sie sie, um ein IoT Edge-Projekt in Visual Studio 2017 zu erstellen.
- Sie benötigen die [Docker Community Edition](https://docs.docker.com/install/) auf dem Entwicklungscomputer, um Ihre Modulimages erstellen und ausführen zu können. Sie müssen die Docker CE, die im Linux- oder im Windows-Containermodus ausgeführt wird, richtig einstellen.
- Zum Einrichten der lokalen Entwicklungsumgebung, in der Sie Ihre IoT Edge-Lösung debuggen, ausführen und testen, benötigen Sie das [Entwicklertool für Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Installieren Sie [Python (2.7/3.6) und Pip](https://www.python.org/). Installieren Sie anschließend **iotedgehubdev**, indem Sie den folgenden Befehl in Ihrem Terminal ausführen. Stellen Sie sicher, dass Sie eine Version des Azure IoT EdgeHub-Entwicklertools von höher als 0.3.0 verwenden.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   - Sie können anstelle einer Cloudregistrierung auch eine lokale Docker-Registrierung für Prototyp- bzw. Testzwecke verwenden. 

- Um das Modul zu testen, benötigen Sie einen aktiven IoT Hub mit mindestens einer erstellten IoT Edge-Geräte-ID. Wenn Sie den IoT Edge-Sicherheitsdaemon auf dem Entwicklungscomputer ausführen, müssen Sie möglicherweise EdgeHub und EdgeAgent beenden, bevor Sie mit der Entwicklung in Visual Studio beginnen. 

### <a name="check-your-tools-version"></a>Überprüfen Ihrer Tools-Version

1. Wählen Sie im Menü **Extras** auf **Erweiterungen und Updates**. Erweitern Sie **Installiert > Tools**. Dort finden Sie **Azure IoT Edge** und **Cloud-Explorer**.

2. Notieren Sie sich die installierte Version. Sie können diese Version mit der neuesten Version in Visual Studio Marketplace vergleichen ([Cloud-Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)).

3. Wenn Ihre Version älter ist, aktualisieren Sie Ihre Tools in Visual Studio, wie im folgenden Abschnitt gezeigt.

### <a name="update-your-tools"></a>Aktualisieren Ihrer Tools

1. Erweitern Sie im Dialogfeld **Erweiterungen und Updates** die Option **Updates > Visual Studio Marketplace**, wählen Sie **Azure IoT Edge** oder **Cloud-Explorer** und anschließend **Aktualisieren**.

2. Nachdem das Toolupdate heruntergeladen wurde, schließen Sie Visual Studio, damit die Tools mit dem VSIX-Installationsprogramm aktualisiert werden.

3. Wählen Sie im Installationsprogramm **OK**, um den Vorgang zu starten, und dann „Ändern“, um die Tools zu aktualisieren.

4. Nachdem das Update abgeschlossen ist, wählen Sie „Schließen“ und starten Visual Studio neu.

### <a name="create-an-azure-iot-edge-project"></a>Erstellen eines Azure IoT Edge-Projekts

Die Azure IoT Edge-Projektvorlage in Visual Studio erstellt ein Projekt, das auf Azure IoT Edge-Geräten im Azure IoT Hub bereitgestellt werden kann. Als Erstes erstellen Sie eine Azure IoT Edge-Lösung, und dann generieren Sie das erste C#-Modul in dieser Lösung. Jede IoT Edge-Lösung kann mehrere Module enthalten. 

1. Wählen Sie in Visual Studio im Menü **Datei** die Optionen **Neu** > **Projekt**.

2. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Installiert**, erweitern Sie **Visual C# > Cloud**, und wählen Sie **Azure IoT Edge** aus. Geben Sie unter **Name** einen Namen für Ihr Projekt ein, geben Sie einen Speicherort an, und klicken Sie auf **OK**. Der Standardname für das Projekt ist **AzureIoTEdgeApp1**. 

   ![Neues Projekt](./media/how-to-visual-studio-develop-csharp-module/create-new.jpg)

3. Wählen Sie im Fenster **IoT Edge-Modulkonfiguration** die Option **C#-Modul**, und geben Sie den Modulnamen und das Repository für das Modulimage an.  VS trägt als Modulnamen automatisch **localhost:5000** ein. Ersetzen Sie ihn durch Ihre eigenen Registrierungsinformationen. Wenn Sie eine lokale Docker-Registrierung zum Testen verwenden, können Sie „localhost“ nutzen. Nutzen Sie den Anmeldeserver aus Ihren Registrierungseinstellungen, wenn Sie Azure Container Registry verwenden. Der Anmeldeserver hat die Form **<registry name>.azurecr.io**. Ersetzen Sie nur den localhost-Teil der Zeichenfolge, löschen Sie nicht Ihren Modulnamen. Der Standardname für das Modul ist **IoTEdgeModule1**.

4. Klicken Sie auf **OK**, um das Azure IoT Edge-Projekt mit einem C#-Modul zu erstellen.

Ihr Lösung enthält jetzt ein **AzureIoTEdgeApp1**- und ein **IoTEdgeModule1**-Projekt. Die **AzureIoTEdgeApp1**-Projekt verfügt über eine **deployment.template.json**-Datei. Diese Datei definiert die Module, die Sie für Ihre IoT Edge-Lösung erstellen und bereitstellen möchten, und definiert die Routen zwischen Modulen. Die Standardlösung besteht aus einem **TempSensor**-und einem **IoTEdgeModule1**-Modul. Das **TempSensor**-Modul generiert simulierte Daten für das **IoTEdgeModule1**-Modul, und der Standardcode im **IoTEdgeModule1** -Modul ist ein Modul des Pipenachrichtenmodul, das empfangene Nachrichten direkt an den IoT Hub sendet.

Das **IoTEdgeModule1**-Projekt ist eine .NET Core 2.1-Konsolenanwendung. Es enthält die erforderlichen **Dockerfiles**, die Sie für Ihr IoT Edge-Gerät benötigen, das mit Windows- oder Linux-Container läuft. Die **module.json**-Datei beschreibt die Metadaten eines Moduls. Die **program.cs**-Datei ist der tatsächlichen Modulcode, der das Azure IoT-Geräte-SDK als Abhängigkeit verwendet.

## <a name="develop-your-module"></a>Entwickeln Ihres Moduls

Der in der Lösung enthaltene C#-Standardmodulcode befindet sich unter **IoTEdgeModule1** > **Program.cs**. Das Modul und die Datei „deployment.template.json“ werden so eingerichtet, dass Sie die Projektmappe erstellen, in Ihre Containerregistrierung verschieben und zum Starten des Tests für ein Gerät bereitstellen können, ohne Code ändern zu müssen. Das Modul ist so konzipiert, dass es einfach eine Eingabe aus einer Quelle akzeptiert (in diesem Fall das Daten simulierende tempSensor-Modul) und IoT Hub übergibt. 

Wenn Sie bereit sind, die C#-Vorlage mit Ihrem eigenen Code anzupassen, erstellen Sie mit den [Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md) Module, die die wesentlichen Anforderungen für IoT-Lösungen wie Sicherheit, Geräteverwaltung und Zuverlässigkeit berücksichtigen. 

## <a name="initialize-iotegehubdev-with-iot-edge-device-connection-string"></a>Initialisieren von **iotegehubdev** mit der IoT Edge-Geräteverbindungszeichenfolge

1. Um die Verbindungszeichenfolge eines IoT Edge-Geräts abzurufen, können Sie den Wert „Primäre Verbindungszeichenfolge“ im Cloud-Explorer in Visual Studio 2017 wie folgt kopieren. Kopieren Sie nicht die Verbindungszeichenfolge eines Nicht-Edge-Geräts, da sich das Symbol von IoT Edge-Geräten von dem von Nicht-Edge-Geräten unterscheidet.

   ![Kopieren der Edge-Geräteverbindungszeichenfolge](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

2. Klicken Sie mit der rechten Maustaste auf das Projekt **AzureIoTEdgeApp1**, um das Kontextmenü zu öffnen, und klicken Sie dann auf **Set Edge Device Connection String** (Edge-Geräteverbindungszeichenfolge festlegen). Das Setupfenster für Azure IoT Edge wird angezeigt.

   ![Öffnen des Fensters „Set Edge Device Connection String“ (Edge-Geräteverbindungszeichenfolge festlegen)](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

3. Geben Sie im Setupfenster die Verbindungszeichenfolge ein, die Sie im ersten Schritt abgerufen haben, und klicken Sie auf die Schaltfläche **OK**.

>[!NOTE]
>Dieser Vorgang muss auf einem Computer nur einmal ausgeführt werden. Alle späteren Azure IoT Edge-Lösungen erhalten die Zeichenfolge automatisch. Selbstverständlich können Sie diesen Schritt erneut ausführen, wenn die Verbindungszeichenfolge ungültig ist oder Sie eine andere Verbindungszeichenfolge verwenden müssen.

## <a name="build-and-debug-single-c-module"></a>Erstellen und Debuggen eines einzelnen C#-Moduls

In der Regel wollen wir jedes Modul testen/debuggen, bevor wir es in einer Gesamtlösung mit mehreren Modulen laufen lassen.

1. Wählen Sie im Kontextmenü **IoTEdgeModule1** als Startprojekt.

   ![Startprojekt festlegen](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

2. Drücken Sie **F5** oder klicken Sie auf die Schaltfläche unten, um das Modul ausgeführt. Beim ersten Mal dauert dies 10 bis 20 Sekunden.

   ![Modul ausführen](./media/how-to-visual-studio-develop-csharp-module/run-module.png)


3. Es wird eine .Net Core-Konsolenanwendung gestartet, wenn das Modul erfolgreich initialisiert wurde.

   ![Modul wird ausgeführt](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

4. Jetzt können Sie einen Breakpoint in **PipeMessage** in **Program.cs** setzen und dann eine Nachricht senden, indem Sie den folgenden Befehl in Ihrem **Git Bash** oder **WSL Bash** ausführen (nicht in CMD oder Powershell ausführen) (Sie finden diesen Befehl auch im VS-Ausgabefenster):

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Debuggen eines Einzelmoduls](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Es sollte ein Breakpoint ausgelöst werden. Sie können Variablen im Fenster „Lokal“ in Visual Studio ansehen.

   > [!TIP]
   > Sie können auch [PostMan](https://www.getpostman.com/) oder andere API-Tools (anstelle von `curl`) zum Senden von Nachrichten verwenden.

5. Drücken Sie zum Beenden des Debuggen auf **STRG + F5** oder klicken Sie auf die Schaltfläche „Beenden“.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Erstellen und Debuggen einer IoT Edge-Lösung mit mehreren Modulen

Nachdem wir ein Einzelmodul entwickelt haben, möchten wird jetzt die Gesamtlösung mit mehreren Modulen ausführen und debuggen.

1. Fügen Sie Ihr zweites C#-Modul in der Lösung hinzu. Klicken Sie mit der rechten Maustaste auf **AzureIoTEdgeApp1**, und wählen Sie **Hinzufügen** -> **Neues IoT Edge-Modul**. Der Standardname des zweiten Moduls lautet **IoTEdgeModule2** und dies immer noch ein Pipemodul.

2. Navigieren Sie zu **deployment.template.json**. Sie sehen, dass **IoTEdgeModule2** im Abschnitt **Module** hinzugefügt wurde. Ersetzen Sie den Abschnitt **routes** durch Folgendes: Wenn Sie Ihre Modulnamen angepasst haben, stellen Sie sicher, dass Sie die Namen unten nach dem Austausch aktualisieren.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

3. Legen Sie im Kontextmenü das **AzureIoTEdgeApp1**-Projekt als Startprojekt fest.

4. Legen Sie Breakpoints fest, und drücken Sie F5. Dann können Sie mehrere Module gleichzeitig ausführen und debuggen. Sie sollten mehrere .Net Core-Konsolenanwendungsfenster mit jeweils einem C#-Modul sehen. 

   ![Debuggen mehrerer Module](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

5. Drücken Sie zum Beenden des Debuggen auf **STRG + F5** oder klicken Sie auf die Schaltfläche „Beenden“.

## <a name="build-and-push-images"></a>Erstellen und Übermitteln des Images mithilfe von Push

1. Stellen Sie sicher, dass **AzureIoTEdgeApp1** Ihr Startprojekt ist. Wählen Sie die Konfiguration **Debuggen** oder **Version**, um Ihre Modulimages zu kompilieren.

    > [!NOTE]
    > Bei der Auswahl von **Debuggen** verwendet VS `Dockerfile.debug` zum Erstellen der Docker-Images. Dies bindet den .NET Core-Befehlszeilendebugger VSDBG beim Erstellen in Ihr Containerimage ein. Wir empfehlen Ihnen, die Konfiguration **Version** zu verwenden, die `Dockerfile` ohne VSDBG für produktionsreife IoT Edge-Module verwendet.

2. Wenn Sie eine private Registrierung wie Azure Container Registry verwenden, führen Sie das Dockerprotokoll in Ihrem Terminal mit dem folgenden Befehl aus. Wenn Sie eine lokale Registrierung verwenden, können Sie [Eine lokale Registrierung ausführen](https://docs.docker.com/registry/deploying/#run-a-local-registry).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server> 
    ```

3. Wenn Sie eine private Registrierung wie die Azure Container Registry verwenden, müssen Sie den Benutzernamen und das Kennwort der Registrierung in `deployment.template.json` unter „Laufzeiteinstellungen“ mit folgendem Inhalt eingeben. Denken Sie daran, den Platzhalter durch Ihren tatsächlichen-Administratorbenutzernamen und das Kennwort zu ersetzen.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

4. Klicken Sie mit der rechten Maustaste auf **AzureIoTEdgeApp1**, und wählen Sie im Kontextmenü die Option **Edge-Lösung kompilieren und pushen**. Anschließend wird das Docker-Image für jedes Modul kompiliert und gepusht.

   ![Erstellen und des Images mithilfe von Push](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)


## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Im Schnellstartartikel, anhand dessen Sie Ihr IoT Edge-Gerät eingerichtet haben, haben Sie ein Modul über das Azure-Portal bereitgestellt. Sie können Module auch mit dem Cloud-Explorer für Visual Studio bereitstellen. Sie haben bereits ein Bereitstellungsmanifest für Ihr Szenario vorbereitet: die `deployment.json`-Datei. Nun müssen Sie nur noch ein Gerät auswählen, um die Bereitstellung zu empfangen.

1. Öffnen Sie den **Cloud-Explorer** durch Klicken auf **Ansicht** > **Cloud-Explorer**. Stellen Sie sicher, dass Sie mit Ihrem Konto bei Visual Studio 2017 angemeldet haben.

2. Erweitern Sie in **Cloud-Explorer** Ihr Abonnement, suchen Sie Ihren Azure IoT Hub und das Azure IoT Edge-Gerät, das Sie bereitstellen möchten.

3. Klicken Sie mit der rechten Maustaste auf das IoT Edge-Gerät, um die Bereitstellung dafür zu erstellen. Sie müssen unter `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json` die Bereitstellungsmanifestdatei auswählen.

>>[!NOTE]
>>Wählen Sie auf keinen Fall `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json` aus.

4. Klicken Sie auf die Schaltfläche „Aktualisieren“. Nun sollte die neuen Module zusammen mit dem **TempSensor**-Modul sowie **$edgeAgent** und **$edgeHub** ausgeführt werden.

## <a name="view-generated-data"></a>Anzeigen generierter Daten

1. Wenn Sie die D2C-Nachrichten für ein bestimmtes Gerät überwachen möchten, klicken Sie auf das Gerät in der Liste, und klicken Sie dann auf **Überwachung von D2C-Nachrichten starten** im Fenster **Aktion**.

2. Um die Überwachung von Daten zu beenden, klicken Sie auf das Gerät in der Liste, und wählen Sie **Überwachen von D2C-Nachrichten beenden**  im Fenster **Aktion**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwickeln Ihrer Module für Ihre IoT Edge-Geräte finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](../iot-hub/iot-hub-devguide-sdks.md).
