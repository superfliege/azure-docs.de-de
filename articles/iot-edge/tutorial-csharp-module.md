---
title: C#-Modul in Azure IoT Edge | Microsoft-Dokumentation
description: "Erstellen eines IoT Edge-Moduls mit C#-Code und Bereitstellen dieses Moduls auf einem Edge-Gerät"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: bd186341329721ee097a5b3ad3e7ad11b8e189f9
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2018
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Entwickeln und Bereitstellen eines C#-IoT Edge-Moduls für Ihr simuliertes Gerät – Vorschau

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. In diesem Tutorial wird beschrieben, wie Sie ein IoT Edge-Modul, mit dem Sensordaten gefiltert werden, erstellen und bereitstellen. Sie verwenden das simulierte IoT Edge-Gerät, das Sie in den Tutorials zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] erstellt haben. In diesem Tutorial lernen Sie Folgendes:    

> [!div class="checklist"]
> * Erstellen eines IoT Edge-Moduls auf Grundlage von .NET Core 2.0 mithilfe von Visual Studio Code
> * Erstellen und Veröffentlichen eines Docker-Image in Ihrer Registrierung mithilfe von Visual Studio Code und Docker 
> * Bereitstellen des Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten


Das IoT Edge-Modul, das Sie in diesem Tutorial erstellen, filtert die von Ihrem Gerät generierten Temperaturdaten. Es leitet Nachrichten nur dann an nachgeschaltete Geräte weiter, wenn die Temperatur einen angegebenen Schwellenwert überschreitet. Diese Art der Analyse auf Edge-Ebene eignet sich zur Verringerung der Datenmenge, die in die Cloud weitergeleitet und dort gespeichert wird. 

## <a name="prerequisites"></a>Voraussetzungen

* Das Azure IoT Edge-Gerät, das Sie mit der Schnellstartanleitung oder im ersten Tutorial erstellt haben.
* Die Verbindungszeichenfolge des primären Schlüssels für das IoT Edge-Gerät  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Erweiterung C# for Visual Studio Code mit Unterstützung von OmniSharp](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker](https://docs.docker.com/engine/installation/) auf dem gleichen Computer, auf dem Visual Studio Code installiert ist. Die Community Edition (CE) ist für dieses Tutorial ausreichend. 
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung
In diesem Tutorial verwenden Sie die Azure IoT Edge-Erweiterung für VSCode zum Entwickeln eines Moduls und zum Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.  

Sie können für dieses Tutorial jede beliebige Docker-kompatible Registrierung verwenden. Zwei beliebte Docker-Registrierungsdienste, die in der Cloud verfügbar sind, sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet. 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Azure Container Registry** aus.
2. Benennen Sie die Registrierung, wählen Sie ein Abonnement und eine Ressourcengruppe aus, und legen Sie die SKU auf **Basic** fest. 
3. Klicken Sie auf **Erstellen**.
4. Navigieren Sie nach der Erstellung der Containerregistrierung zu dieser Registrierung, und wählen Sie **Zugriffsschlüssel** aus. 
5. Legen Sie **Administratorbenutzer** auf **Aktivieren** fest.
6. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. Sie verwenden diese Werte später im Tutorial. 

## <a name="create-an-iot-edge-module-project"></a>Erstellen eines IoT Edge-Modulprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis von .NET Core 2.0 erstellen.
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

   ![Öffnen von „Program.cs“][1]

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

8. In der **Init**-Methode erstellt und konfiguriert der Code ein **DeviceClient**-Objekt. Mit diesem Objekt kann das Modul die Verbindung mit der lokalen Azure IoT Edge-Runtime herstellen, um Nachrichten zu senden und zu empfangen. Die in der **Init**-Methode verwendete Verbindungszeichenfolge wird über die IoT Edge-Runtime für das Modul bereitgestellt. Nach dem Erstellen von **DeviceClient** liest der Code den TemperatureThreshold aus den erwünschten Eigenschaften des Modulzwillings und registriert einen Rückruf für den Empfang von Nachrichten vom IoT Edge-Hub über Endpunkt **input1**. Ersetzen Sie die `SetInputMessageHandlerAsync`-Methode durch eine neue Methode, und fügen Sie eine `SetDesiredPropertyUpdateCallbackAsync`-Methode für die Aktualisierungen der gewünschten Eigenschaften hinzu. Zu diesem Zweck ersetzen Sie die letzte Zeile der **Init**-Methode durch folgenden Code:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    if (moduleTwinCollection["TemperatureThreshold"] != null)
    {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    }

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

   ![Build IoT Edge module (IoT Edge-Modul erstellen)][2]

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Erstellen und Veröffentlichen eines Docker-Image in der Registrierung

1. Erweitern Sie im Visual Studio Code-Explorer den Ordner **Docker**. Erweitern Sie dann den Ordner für Ihre Containerplattform (entweder **linux-x64** oder **windows-nano**).

   ![Auswählen der Docker-Containerplattform][3]

2. Klicken Sie mit der rechten Maustaste auf die Datei **Dockerfile**, und klicken Sie auf **Build IoT Edge Module Docker Image** (Docker-Image für IoT Edge-Modul erstellen). 
3. Navigieren Sie im Fenster **Ordner auswählen** zum Ordner `./bin/Debug/netcoreapp2.0/publish`, oder geben Sie den Ordnernamen direkt ein. Klicken Sie auf **Select Folder as EXE_DIR** (Ordner als EXE_DIR auswählen).
4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filtermodule:latest`. Die Adresse der Containerregistrierung ist identisch mit dem Anmeldeserver, den Sie aus der Registrierung kopiert haben. Sie sollte das Format `<your container registry name>.azurecr.io` aufweisen.
5. Melden Sie sich bei Docker an. Geben Sie dazu den folgenden Code im Terminal von Visual Studio Code ein: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Verwenden Sie den Benutzernamen, das Kennwort und den Anmeldeserver, die Sie bei der Erstellung aus Ihrer Azure-Containerregistrierung kopiert haben.

3. Pushen Sie das Image in Ihr Docker-Repository. Wählen Sie **Ansicht** > **Befehlspalette** aus, und suchen Sie den Menübefehl **Edge: Push IoT Edge module Docker image** (Edge: Docker-Image für IoT Edge-Modul pushen). Geben Sie den Imagenamen im Popup-Textfeld oben im Visual Studio Code-Fenster ein. Verwenden Sie dabei den Imagenamen aus Schritt 4.

## <a name="add-registry-credentials-to-edge-runtime"></a>Hinzufügen von Registrierungsanmeldeinformationen zur Edge-Runtime
Fügen Sie die Anmeldeinformationen für Ihre Registrierung zur Edge-Runtime auf dem Computer hinzu, auf dem Sie Ihr Edge-Gerät ausgeführen. Über diese Anmeldeinformationen erhält die Runtime Zugriff zum Pullen des Containers. 

- Führen Sie für Windows den folgenden Befehl aus:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Führen Sie für Linux den folgenden Befehl aus:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Ausführen der Lösung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
2. Wechseln Sie zu **IoT Edge (Vorschau)**, und wählen Sie Ihr IoT Edge-Gerät aus.
3. Wählen Sie **Module festlegen** aus. 
2. Überprüfen Sie, ob das Modul **tempSensor** automatisch ausgefüllt wird. Wenn dies nicht der Fall ist, fügen Sie es mit den folgenden Schritten hinzu:
    1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `tempSensor` ein.
    3. Geben Sie im Feld **Image-URI** die Zeichenfolge `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` ein.
    4. Behalten Sie die restlichen Einstellungen unverändert bei, und wählen Sie **Speichern** aus.
9. Fügen Sie das Modul **filterModule** hinzu, das Sie in den vorherigen Abschnitten erstellt haben. 
    1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `filterModule` ein.
    3. Geben Sie im Feld **Image-URI** die Imageadresse ein, z.B. `<your container registry address>/filtermodule:latest`.
    4. Aktivieren Sie das Kontrollkästchen **Aktivieren**, damit Sie den Modulzwilling bearbeiten können. 
    5. Ersetzen Sie den JSON-Code im Textfeld für den Modulzwilling durch den folgenden JSON-Code: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Klicken Sie auf **Speichern**.
12. Klicken Sie auf **Weiter**.

13. Kopieren Sie im Schritt **Routen angeben** den folgenden JSON-Code in das Textfeld. Module veröffentlichen alle Nachrichten in der Edge-Runtime. Deklarative Regeln in der Runtime definieren, wohin die Nachrichten fließen. In diesem Tutorial benötigen Sie zwei Routen. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „input1“ zum Filtermodul. Dies ist der Endpunkt, für den Sie den **FilterMessages**-Handler konfiguriert haben. Die zweite Route transportiert Nachrichten vom Filtermodul an IoT Hub. In dieser Route ist `upstream` ein spezieller Empfänger, der Edge Hub anweist, Nachrichten an IoT Hub zu senden. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Klicken Sie auf **Weiter**.

5. Klicken Sie im Schritt **Vorlage überprüfen** auf **Senden**. 
6. Kehren Sie zur Seite mit den IoT Edge-Gerätedetails zurück, und klicken Sie auf **Aktualisieren**. Es sollte nun angezeigt werden, dass das neue **filtermodule**-Modul zusammen mit dem **tempSensor**-Modul und der **IoT Edge-Laufzeit** ausgeführt wird. 

## <a name="view-generated-data"></a>Anzeigen generierter Daten

So überwachen Sie Gerät-zu-Cloud-Nachrichten, die von Ihrem IoT Edge-Gerät an IoT Hub gesendet wurden:
1. Konfigurieren Sie die Verbindungszeichenfolge für Ihren IoT Hub in der Azure IoT-Toolkit-Erweiterung: 
    1. Öffnen Sie den Visual Studio Code-Explorer durch Auswählen von **Ansicht** > **Explorer**. 
    3. Klicken Sie im Explorer auf **IOT HUB DEVICES** (IOT HUB-GERÄTE), und klicken Sie dann auf **...**. Klicken Sie auf **Set IoT Hub Connection String** (IoT Hub-Verbindungszeichenfolge festlegen), und geben Sie die Verbindungszeichenfolge für den IoT Hub, mit dem Ihr IoT Edge-Gerät verbunden ist, in das Popupfenster ein. 

        Um die Verbindungszeichenfolge zu ermitteln, klicken Sie im Azure-Portal auf die Kachel für Ihren IoT Hub, und klicken Sie dann auf **Richtlinien für gemeinsamen Zugriff**. Klicken Sie unter **Richtlinien für gemeinsamen Zugriff** auf die Richtlinie **iothubowner**, und kopieren Sie die im Fenster **iothubowner** angegebene IoT Hub-Verbindungszeichenfolge.   

1. Zum Überwachen von Daten, die beim IoT Hub eingehen, wählen Sie **Ansicht** > **Befehlspalette** aus, und suchen Sie den Menübefehl **IoT: Start monitoring D2C message** (Überwachung von Gerät-zu-Cloud-Nachricht starten). 
2. Zum Beenden der Datenüberwachung verwenden Sie den Menübefehl **IoT: Stop Monitoring D2C Message** (Überwachung von Gerät-zu-Cloud-Nachricht beenden). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul erstellt, das Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Sie können mit einem der nachfolgenden Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Bereitstellen einer Azure-Funktion als Modul](tutorial-deploy-function.md)
> [Bereitstellen von Azure Stream Analytics als Modul](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
