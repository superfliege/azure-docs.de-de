---
title: Verwenden von Visual Studio Code zur Entwicklung eines C#-Moduls mit Azure IoT Edge | Microsoft-Dokumentation
description: Entwickeln und Bereitstellen eines C#-Moduls mit Azure IoT Edge in VS Code ohne Kontextwechsel
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 269f77e5015175e45e0078926ef06699811889a4
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2017
---
# <a name="use-visual-studio-code-to-develop-c-module-with-azure-iot-edge"></a>Verwenden von Visual Studio Code zur Entwicklung eines C#-Moduls mit Azure IoT Edge
Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio Code](https://code.visualstudio.com/) als Hauptentwicklungstool zum Entwickeln und Bereitstellen Ihrer IoT Edge-Module. 

## <a name="prerequisites"></a>Voraussetzungen
Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer mit Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihre IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

Stellen Sie sicher, dass Sie folgende Tutorials abgeschlossen haben, bevor Sie mit diesem Leitfaden beginnen.
- Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) oder [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Entwickeln und Bereitstellen eines C#-IoT Edge-Moduls für Ihr simuliertes Gerät](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Im Folgenden sehen Sie eine Checkliste mit den Komponenten, über die Sie nach Abschluss der vorherigen Tutorials verfügen sollten.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [Erweiterung C# for Visual Studio Code mit Unterstützung von OmniSharp](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 
- [Python 2.7](https://www.python.org/downloads/)
- [IoT Edge-Steuerungsskript](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- AzureIoTEdgeModule-Vorlage (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Ein aktives IoT Hub mit mindestens einem IoT Edge-Gerät

Zudem wird empfohlen, die [Docker-Unterstützung für VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) zu installieren, um die Verwaltung Ihrer Modulimages und -container zu optimieren.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>Bereitstellen eines Azure IoT Edge-Moduls in VS Code

### <a name="list-your-iot-hub-devices"></a>Auflisten Ihrer IoT Hub-Geräte
Es gibt zwei Möglichkeiten, um Ihre IoT Hub-Geräte in VS Code aufzulisten. Sie können mit einer der beiden Optionen fortfahren.

#### <a name="sign-in-your-azure-account-in-vscode-and-choose-your-iot-hub"></a>Anmelden beim Azure-Konto in VSCode und Auswählen Ihres IoT Hub
1. Geben Sie in der Befehlspalette (F1 oder STRG+UMSCHALT+P) **Azure: Anmelden** ein, und wählen Sie es aus. Klicken Sie im Popupfenster dann auf **Kopieren* und öffnen**. Fügen Sie den Code in Ihren Browser ein (STRG+V), und klicken Sie auf die Schaltfläche „Weiter“. Melden Sie sich dann bei Ihrem Azure-Konto an. Die Kontoinformationen werden in der VS Code-Statusleiste angezeigt.
2. Geben Sie in der Befehlspalette (F1 oder STRG+UMSCHALT+P) **IoT: IoT Hub auswählen** ein, und wählen Sie es aus. Sie wählen zuerst das Abonnement aus, in dem Sie Ihren IoT Hub im vorherigen Tutorial erstellt haben. Wählen Sie dann den IoT Hub aus, der das IoT Edge-Gerät enthält.


#### <a name="set-iot-hub-connection-string"></a>Festlegen der IoT Hub-Verbindungszeichenfolge
1. Geben Sie in der Befehlspalette (F1 oder STRG+UMSCHALT+P) **IoT: IoT Hub-Verbindungszeichenfolge festlegen** ein, und wählen Sie es aus. Fügen Sie unter der Richtlinie **iothubowner** die Verbindungszeichenfolge ein (Sie finden sie in den SAS-Richtlinien Ihres IoT Hub im Azure-Portal).
 

Sie können diese im IoT Hub-Geräte-Explorer auf der linken Seitenleiste sehen.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>Starten Ihrer IoT Edge-Runtime und Bereitstellen eines Moduls
Installieren und starten Sie die Azure IoT Edge-Runtime auf Ihrem Gerät. Stellen Sie ein simuliertes Sensormodul bereit, das Telemetriedaten an IoT Hub sendet.
1. Wählen Sie in der Befehlspalette **Edge: Edge einrichten** und Ihre IoT Edge-Geräte-ID aus. Klicken Sie alternativ mit der rechten Maustaste auf die Edge-Geräte-ID in der Geräteliste, und wählen Sie **Edge einrichten** aus.
2. Wählen Sie in der Befehlspalette **Edge: Edge starten** aus, um die Edge-Runtime zu starten. Sie können die entsprechende Ausgaben im integrierten Terminal sehen.
3. Überprüfen Sie den Edge-Runtimestatus im Docker-Explorer. Grün bedeutet, dass sie momentan ausgeführt wird. Die IoT Edge-Runtime wurde erfolgreich gestartet.
4. Nun wird Ihre Edge-Runtime ausgeführt, d.h., Ihr PC simuliert jetzt ein Edge-Gerät. Als Nächstes wird ein Sensormodul simuliert, das Nachrichten an Ihr Edge-Gerät sendet. Geben Sie in die Befehlspalette **Edge: Edge-Konfigurationsdatei generieren** ein, und wählen Sie diese Option aus. Wählen Sie dann einen Ordner zum Erstellen dieser Datei aus. Ersetzen Sie in der generierten Datei „deployment.json“ die Zeile „<registry>/<image>:<tag>“ durch `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
5. Wählen Sie **Edge: Bereitstellung für Edge-Gerät erstellen** und die Edge-Geräte-ID zum Erstellen einer neuen Bereitstellung aus. Alternativ können Sie mit der rechten Maustaste auf die Edge-Geräte-ID in der Geräteliste klicken und **Bereitstellung für Edge-Gerät erstellen** auswählen. 
6. Nun sollten Sie sehen können, dass Ihr IoT Edge im Docker-Explorer mit dem simulierten Sensor gestartet wird. Klicken Sie mit der rechten Maustaste auf den Container im Docker-Explorer. Sie können für jedes Modul Docker-Protokolle anzeigen.
7. Durch Klicken mit der rechten Maustaste auf die Edge-Geräte-ID können Sie D2C-Nachrichten in VS Code überwachen.
8. Um Ihre IoT Edge-Runtime und das Sensor-Modul zu beenden, können Sie in der Befehlspalette **Edge: Edge beenden** eingeben und auswählen.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Entwickeln und Bereitstellen eines C#-Moduls in VS Code
Im Tutorial [Entwickeln eines C#-Moduls](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module) aktualisieren, erstellen und veröffentlichen Sie Ihr Modulimage in VS Code und stellen dann im Azure-Portal Ihr C#-Modul bereit. In diesem Abschnitt wird erläutert, wie Sie Ihr C#-Modul mithilfe von VS Code bereitstellen und überwachen.

### <a name="start-a-local-docker-registry"></a>Starten einer lokalen Docker-Registrierung
Sie können für dieses Tutorial jede beliebige Docker-kompatible Registrierung verwenden. Zwei beliebte Docker-Registrierungsdienste, die in der Cloud verfügbar sind, sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Abschnitt wird eine [lokale Docker-Registrierung](https://docs.docker.com/registry/deploying/) eingesetzt, die für Testzwecke in der frühen Entwicklungsphase einfacher zu verwenden ist.
Führen Sie im **integrierten Terminal** von VS Code (STRG+`) folgende Befehle aus, um eine lokale Registrierung zu starten.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Im obigen Beispiel werden Registrierungskonfigurationen angezeigt, die nur für Tests geeignet sind. Eine produktionsbereite Registrierung muss durch TLS geschützt werden und sollte idealerweise einen Zugriffssteuerungsmechanismus verwenden. Es wird empfohlen, [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker-Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) für die Bereitstellung von produktionsbereiten IoT Edge-Modulen zu verwenden.

### <a name="create-an-iot-edge-module-project"></a>Erstellen eines IoT Edge-Modulprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis von .NET Core 2.0 erstellen. Wenn Sie diesen Abschnitt im vorherigen Tutorial absolviert haben, können Sie ihn einfach überspringen.
1. Wählen Sie in Visual Studio Code die Optionen **Ansicht** > **Integriertes Terminal** aus, um das in Visual Studio Code integrierte Terminal zu öffnen.
3. Geben Sie im integrierten Terminal den folgenden Befehl ein, um die Vorlage **AzureIoTEdgeModule** in Dotnet zu installieren (oder zu aktualisieren):

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Erstellen Sie ein Projekt für das neue Modul. Mit dem folgenden Befehl wird der Projektordner **FilterModule** im aktuellen Arbeitsverzeichnis erstellt:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```
 
3. Wählen Sie **Datei** > **Ordner öffnen** aus.
4. Navigieren Sie zum Ordner **FilterModule**, und klicken Sie auf **Ordner auswählen**, um das Projekt in Visual Studio Code zu öffnen.
5. Klicken Sie im Visual Studio Code-Explorer auf **Program.cs**, um es zu öffnen.
6. Fügen Sie der **Program**-Klasse die Variable `temperatureThreshold` hinzu. Mit dieser Variable wird der Wert festgelegt, den die gemessene Temperatur übersteigen muss, damit die Daten an IoT Hub gesendet werden. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

7. Fügen Sie der **Program**-Klasse die Klassen `MessageBody`, `Machine` und `Ambient` hinzu. Diese Klassen definieren das erwartete Schema für den Textkörper eingehender Nachrichten.

    ```csharp
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
    ```

8. In der **Init**-Methode erstellt und konfiguriert der Code ein **DeviceClient**-Objekt. Mit diesem Objekt kann das Modul die Verbindung mit der lokalen Azure IoT Edge-Runtime herstellen, um Nachrichten zu senden und zu empfangen. Die in der **Init**-Methode verwendete Verbindungszeichenfolge wird über die IoT Edge-Runtime für das Modul bereitgestellt. Nach dem Erstellen von **DeviceClient** registriert der Code einen Rückruf für den Empfang von Nachrichten vom IoT Edge-Hub über Endpunkt **input1**. Ersetzen Sie die `SetInputMessageHandlerAsync`-Methode durch eine neue Methode, und fügen Sie eine `SetDesiredPropertyUpdateCallbackAsync`-Methode für die Aktualisierungen der gewünschten Eigenschaften hinzu. Zu diesem Zweck ersetzen Sie die letzte Zeile der **Init**-Methode durch folgenden Code:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Fügen Sie der **Program**-Klasse die `onDesiredPropertiesUpdate`-Methode hinzu. Diese Methode empfängt Aktualisierungen der gewünschten Eigenschaften aus dem Modulzwilling und aktualisiert die Variable **temperatureThreshold** entsprechend. Alle Module haben ihren eigenen Modulzwilling, über den Sie den in einem Modul ausgeführten Code direkt in der Cloud konfigurieren können.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

10. Ersetzen Sie die `PipeMessage`-Methode durch die `FilterMessages`-Methode. Diese Methode wird aufgerufen, wenn das Modul eine Nachricht vom IoT Edge-Hub empfängt. Sie filtert Nachrichten heraus, die Temperaturen unter dem über den Modulzwilling festgelegten Temperaturschwellenwert melden. Sie fügt der Nachricht außerdem die **MessageType**-Eigenschaft hinzu, deren Wert auf **Alert** festgelegt ist. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        int counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            byte[] messageBytes = message.GetBytes();
            string messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get message body
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await deviceClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Klicken Sie zum Erstellen des Projekts im Explorer mit der rechten Maustaste auf die Datei **FilterModule.csproj**, und klicken Sie dann auf **Build IoT Edge module** (IoT Edge-Modul erstellen). Bei diesem Vorgang werden das Modul kompiliert und die Binärdatei und die zugehörigen Abhängigkeiten in einen Ordner exportiert, der zum Erstellen eines Docker-Images verwendet wird.


### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Erstellen und Veröffentlichen eines Docker-Image in der Registrierung

1. Erweitern Sie im Visual Studio Code-Explorer den Ordner **Docker**. Erweitern Sie dann den Ordner für Ihre Containerplattform (entweder **linux-x64** oder **windows-nano**).
2. Klicken Sie mit der rechten Maustaste auf die Datei **Dockerfile**, und klicken Sie auf **Build IoT Edge Module Docker Image** (Docker-Image für IoT Edge-Modul erstellen). 
3. Navigieren Sie im Fenster **Ordner auswählen** zum Ordner `./bin/Debug/netcoreapp2.0/publish`, oder geben Sie den Ordnernamen direkt ein. Klicken Sie auf **Select Folder as EXE_DIR** (Ordner als EXE_DIR auswählen).
4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filtermodule:latest`. Wenn Sie eine Bereitstellung für die lokale Registrierung durchführen, sollte diese `localhost:5000/filtermodule:latest` lauten.
5. Pushen Sie das Image in Ihr Docker-Repository. Klicken Sie auf **Edge: Docker-Image für IoT Edge-Modul mithilfe von Push übertragen**, und geben Sie oben im VS Code-Fenster in das Popuptextfeld die Image-URL ein. Verwenden Sie dabei die Image-URL aus dem obigen Schritt.

### <a name="deploy-your-iot-edge-modules"></a>Bereitstellen Ihrer IoT Edge-Module

1. Öffnen Sie die Datei `deployment.json`, und ersetzen Sie den Abschnitt **modules** durch folgenden Inhalt:
    ```json
    "tempSensor": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
            "createOptions": ""
        }
    },
    "filtermodule": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "localhost:5000/filtermodule:latest",
            "createOptions": ""
        }
    }
    ```

2. Ersetzen Sie den Abschnitt **routes** durch folgenden Inhalt:
    ```json
    {
        "routes": {
            "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
            "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
        }
    }
    ```
   > [!NOTE]
   > Deklarative Regeln in der Runtime definieren, wohin diese Nachrichten fließen. In diesem Tutorial benötigen Sie zwei Routen. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „input1“ zum Filtermodul. Dies ist der Endpunkt, für den Sie den FilterMessages-Handler konfiguriert haben. Die zweite Route transportiert Nachrichten vom Filtermodul an IoT Hub. In dieser Route ist „upstream“ ein spezielles Ziel, das Edge-Hub anweist, Nachrichten an IoT Hub zu senden.

3. Speichern Sie diese Datei.
4. Wählen Sie in der Befehlspalette **Edge: Bereitstellung für Edge-Gerät erstellen** aus. Wählen Sie dann die IoT Edge-Geräte-ID aus, um eine Bereitstellung zu erstellen. Klicken Sie alternativ mit der rechten Maustaste auf die Geräte-ID in der Geräteliste, und wählen Sie **Bereitstellung für Edge-Gerät erstellen** aus.
5. Wählen Sie die Datei `deployment.json` aus, die Sie aktualisiert haben. Im Ausgabefenster können Sie die entsprechenden Ausgaben für Ihre Bereitstellung sehen.
6. Starten Sie Ihre Edge-Runtime in der Befehlspalette. **Edge: Edge starten**
7. Nun sollten Sie sehen können, dass Ihre IoT Edge-Runtime im Docker-Explorer mit dem simulierten Sensor und Filtermodul gestartet wird.
8. Durch Klicken mit der rechten Maustaste auf die Edge-Geräte-ID können Sie D2C-Nachrichten in VS Code überwachen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul erstellt und für das IoT Edge-Gerät in VS Code bereitgestellt. Sie können mit einem der beiden folgenden Tutorials fortfahren, um andere Szenarien bei der Entwicklung von Azure IoT Edge in VS Code kennenzulernen.

> [!div class="nextstepaction"]
> [C#-Modul in VS Code debuggen](how-to-vscode-debug-csharp-module.md)
