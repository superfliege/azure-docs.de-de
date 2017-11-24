---
title: C#-Modul in Azure IoT Edge | Microsoft-Dokumentation
description: "Erstellen eines IoT Edge-Moduls mit C#-Code und Bereitstellen dieses Moduls auf einem Edge-Gerät"
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: fb7674d8c292e7d571a94ac4625b0858a90704b3
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Entwickeln und Bereitstellen eines C#-IoT Edge-Moduls für Ihr simuliertes Gerät – Vorschau

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. Dieses Tutorial führt Sie durch die Erstellung und Bereitstellung eines IoT Edge-Moduls, das Sensordaten des simulierten IoT Edge-Geräts filtert, das Sie im Tutorial zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] erstellt haben. In diesem Tutorial lernen Sie Folgendes:    

> [!div class="checklist"]
> * Erstellen eines IoT Edge-Moduls auf Grundlage von .NET Core 2.0 mithilfe von Visual Studio Code
> * Erstellen und Veröffentlichen eines Docker-Image in Ihrer Registrierung mithilfe von Visual Studio Code und Docker 
> * Bereitstellen des Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten


Das von Ihnen in diesem Tutorial erstellte IoT Edge-Modul filtert die von Ihrem Gerät generierten Temperaturdaten und leitet Nachrichten nur dann an nachgeschaltete Geräte weiter, wenn die Temperatur einen angegebenen Schwellenwert überschreitet. 

## <a name="prerequisites"></a>Voraussetzungen

* Das Azure IoT Edge-Gerät, das Sie in der Schnellstartanleitung oder im vorherigen Tutorial erstellt haben
* Die IoT Hub-Verbindungszeichenfolge für den IoT Hub, mit dem Ihr IoT Edge-Gerät eine Verbindung herstellt  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) (Sie können die Erweiterung über den Erweiterungsbereich in Visual Studio Code installieren)
* [Erweiterung C# for Visual Studio Code (mit Unterstützung von OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) (Sie können die Erweiterung über den Erweiterungsbereich in Visual Studio Code installieren)
* Azure IoT Edge-Erweiterung für Visual Studio Code
* [Docker](https://docs.docker.com/engine/installation/). Die Community Edition (CE) für Ihre Plattform ist für dieses Tutorial ausreichend. Stellen Sie sicher, dass die Installation auf dem Computer erfolgt, auf dem Sie Visual Studio Code ausführen.
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 

## <a name="choose-or-sign-up-for-a-docker-registry"></a>Wählen Sie eine Docker-Registrierung aus, oder registrieren Sie sich für eine solche.
In diesem Tutorial verwenden Sie zum Entwickeln eines Moduls und zum Erstellen eines [Docker-Image](https://docs.docker.com/glossary/?term=image) die Azure IoT Edge-Erweiterung für Visual Studio Code. Danach pushen Sie dieses Docker-Image in ein [Docker-Repository](https://docs.docker.com/glossary/?term=repository), das in einer [Docker-Registrierung](https://docs.docker.com/glossary/?term=registry) gehostet wird. Abschließend stellen Sie Ihr Docker-Image gepackt als [Docker-Container](https://docs.docker.com/glossary/?term=container) aus der Registrierung für Ihr IoT Edge-Gerät bereit.  

Sie können für dieses Tutorial jede beliebige Docker-kompatible Registrierung verwenden. Zwei beliebte Docker-Registrierungsdienste, die in der Cloud verfügbar sind, sind **Azure Container Registry** und **Docker Hub**:

- [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/) ist mit einem [kostenpflichtigen Abonnement](https://azure.microsoft.com/en-us/pricing/details/container-registry/) erhältlich. Für die Zwecke dieses Tutorials ist ein **Basic**-Abonnement ausreichend. 

- Mit [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) erhalten Sie ein kostenloses privates Repository, wenn Sie eine (kostenlose) Docker-ID registrieren. 
    1. Zur Registrierung einer Docker-ID folgen Sie den Anweisungen unter [Register for a Docker ID](https://docs.docker.com/docker-id/#register-for-a-docker-id) (Docker-ID registrieren) auf der Docker-Website. 

    2. Um ein privates Docker-Repository zu erstellen, folgen Sie danach den Anweisungen im Abschnitt [Creating a New Repository on Docker Hub](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) (Erstellen eines neuen Repositorys auf Docker Hub) auf der Docker-Website.

In diesem Tutorial werden wir an den jeweiligen Stellen Befehle sowohl für Azure Container Registry als auch für Docker Hub angeben.

## <a name="create-an-iot-edge-module-project"></a>Erstellen eines IoT Edge-Modulprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis von .NET Core 2.0 erstellen.
1. Öffnen Sie Visual Studio Code.
2. Öffnen Sie über den Menübefehl **Ansicht > Integriertes Terminal** das in Visual Studio Code integrierte Terminal.
3. Geben Sie im integrierten Terminal den folgenden Befehl ein, um die Vorlage **AzureIoTEdgeModule** in Dotnet zu installieren (oder zu aktualisieren):

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Geben Sie im integrierten Terminal den folgenden Befehl ein, um ein Projekt für das neue Modul zu erstellen:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```

    >[!NOTE]
    > Mit diesem Befehl wird der Projektordner **FilterModule** im aktuellen Arbeitsverzeichnis erstellt. Wenn Sie ihn an einem anderen Speicherort erstellen möchten, wechseln Sie vor dem Ausführen des Befehls dorthin.
 
3. Klicken Sie auf **Datei > Ordner öffnen**, navigieren Sie zum Ordner **FilterModule**, und klicken Sie auf **Ordner auswählen**, um das Projekt in Visual Studio Code zu öffnen.
4. Klicken Sie im Visual Studio Code-Explorer auf **Program.cs**, um es zu öffnen.
5. Fügen Sie der **Program**-Klasse das folgende Feld hinzu.

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

1. Fügen Sie der **Program**-Klasse die folgenden Klassen hinzu. Diese Klassen definieren das erwartete Schema für den Textkörper eingehender Nachrichten.

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

1. In der **Init**-Methode erstellt und konfiguriert der Code ein **DeviceClient**-Objekt. Mit diesem Objekt kann das Modul die Verbindung mit der lokalen Azure IoT Edge-Runtime herstellen, um Nachrichten zu senden und zu empfangen. Der in der **Init**-Methode verwendete Verbindungszeichenfolgen-Parameter wird von der IoT Edge-Runtime in der Umgebungsvariablen **EdgeHubConnectionString** an das Modul übergeben. Nach dem Erstellen von **DeviceClient** registriert der Code einen Rückruf für den Empfang von Nachrichten vom IoT Edge-Hub über Endpunkt **input1**. Ersetzen Sie die Methode, die als Rückruf für die Verarbeitung von Nachrichten verwendet wird, durch eine neue Methode, und fügen Sie einen Rückruf für Aktualisierungen der gewünschten Eigenschaften im Modulzwilling hinzu. Zu diesem Zweck ersetzen Sie die letzte Zeile der **Init**-Methode durch folgenden Code:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

1. Fügen Sie die folgende Methode zur **Program**-Klasse hinzu, um das Feld **TemperatureThreshold** auf Basis der gewünschten Eigenschaften zu aktualisieren, die vom Back-End-Dienst über den Modulzwilling gesendet werden. Alle Module haben ihren eigenen Modulzwilling. Ein Modulzwilling ermöglicht einem Back-End-Dienst das Konfigurieren von Code, der in einem Modul ausgeführt wird.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"].exists())
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

1. Ersetzen Sie die **PipeMessage**-Methode durch die folgende Methode: Diese Methode wird aufgerufen, wenn dem Modul eine Nachricht vom Edge-Hub gesendet wird. Sie filtert Nachrichten anhand des im Nachrichtentext angegebenen Temperaturwerts und des im Modulzwilling festgelegten Temperaturschwellenwerts.

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

11. Erstellen Sie das Projekt. Öffnen Sie über den Menübefehl **Ansicht > Explorer** den Visual Studio Code-Explorer. Klicken Sie im Explorer mit der rechten Maustaste auf die Datei **FilterModule.csproj**, und klicken Sie auf **Build IoT Edge Module** (IoT Edge-Modul erstellen), um das Modul zu kompilieren und die Binärdatei und ihre Abhängigkeiten in einen Ordner zu exportieren, aus dem im nächsten Schritt das Docker-Image erstellt wird.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Erstellen und Veröffentlichen eines Docker-Image in der Registrierung

1. Erstellen Sie das Docker-Image.
    1. Klicken Sie im Visual Studio Code-Explorer auf den Ordner **Docker**, um ihn zu öffnen. Wählen Sie nun den Ordner für Ihre Containerplattform aus (entweder **linux-x64** oder **windows-nano**). 
    2. Klicken Sie mit der rechten Maustaste auf die Datei **Dockerfile**, und klicken Sie auf **Build IoT Edge Module Docker Image** (Docker-Image für IoT Edge-Modul erstellen). 
    3. Navigieren Sie im Feld **Ordner auswählen** zum Ordner `./bin/Debug/netcoreapp2.0/publish`, oder geben Sie den Ordnernamen direkt ein. Klicken Sie auf **Select Folder as EXE_DIR** (Ordner als EXE_DIR auswählen).
    4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel:`<docker registry address>/filtermodule:latest`. Hierbei ist *docker registry address* Ihre Docker-ID, sofern Sie Docker Hub verwenden; setzen Sie hingegen Azure Container Registry ein, dann ist dies ein Wert ähnlich wie `<your registry name>.azurecr.io`.
 
4. Melden Sie sich bei Docker an. Geben Sie den folgenden Befehl in das integrierte Terminal ein: 

    - Docker Hub (geben Sie auf Aufforderung Ihre Anmeldeinformationen ein):
        
        ```csh/sh
        docker login
        ```

    - Bei Azure Container Registry:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Zur Ermittlung von Benutzername, Kennwort und die Anmeldeserver für diesen Befehl wechseln Sie zum [Azure-Portal](https://portal.azure.com). Klicken Sie unter **Alle Ressourcen** auf die Kachel Ihrer Azure-Containerregistrierung, um deren Eigenschaften zu öffnen. Klicken Sie dann auf **Zugriffsschlüssel**. Kopieren Sie die Werte der Felder **Benutzername**, **Kennwort** und **Anmeldeserver**. Der Anmeldeserver sollte folgendes Format aufweisen: `<your registry name>.azurecr.io`.

3. Pushen Sie das Image in Ihr Docker-Repository. Klicken Sie auf **Ansicht >Befehlspalette > Edge: Push IoT Edge Module Docker Image** (Edge: Docker-Image für IoT Edge-Modul pushen), und geben Sie den Imagenamen in das Popuptextfeld oben im Visual Studio Code-Fenster ein. Verwenden Sie dabei den Imagenamen aus Schritt 1.c.

## <a name="add-registry-credentials-to-edge-runtime-on-your-edge-device"></a>Hinzufügen von Registrierungsanmeldeinformationen zur Edge-Runtime auf Ihrem Edge-Gerät
Fügen Sie die Anmeldeinformationen für Ihre Registrierung zur Edge-Runtime auf dem Computer hinzu, auf dem Sie Ihr Edge-Gerät ausführen. Hierdurch erhält die Runtime Zugriff zum Pullen des Containers. 

- Führen Sie für Windows den folgenden Befehl aus:
    
    ```cmd/sh
    iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

- Führen Sie für Linux den folgenden Befehl aus:
    
    ```cmd/sh
    sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

## <a name="run-the-solution"></a>Ausführen der Lösung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
2. Wechseln Sie zu **IoT Edge (Vorschau)**, und wählen Sie Ihr IoT Edge-Gerät aus.
3. Wählen Sie **Module festlegen** aus. 
2. Fügen Sie das Modul **tempSensor** hinzu. Dieser Schritt ist nur erforderlich, wenn Sie das Modul **tempSensor** noch nicht auf Ihrem IoT Edge-Gerät bereitgestellt haben.
    1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `tempSensor` ein.
    3. Geben Sie im Feld **Image-URI** die Zeichenfolge `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` ein.
    4. Behalten Sie die restlichen Einstellungen unverändert bei, und wählen Sie **Speichern** aus.
9. Hinzufügen von **filtermodule**
    1. Wählen Sie erneut **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `filtermodule` ein.
    3. Geben Sie im Feld **Image** die Imageadresse ein, z.B. `<docker registry address>/filtermodule:latest`.
    4. Aktivieren Sie das Kontrollkästchen **Edit Module Twin** (Modulzwilling bearbeiten).
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

13. Kopieren Sie im Schritt **Routen angeben** den folgenden JSON-Code in das Textfeld. Module veröffentlichen alle Nachrichten in der Edge-Laufzeit. Deklarative Regeln in der Laufzeit definieren, wohin diese Nachrichten fließen. In diesem Tutorial benötigen Sie zwei Routen. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „input1“ zum Filtermodul. Dies ist der Endpunkt, für den Sie den **FilterMessages**-Handler konfiguriert haben. Die zweite Route transportiert Nachrichten vom Filtermodul an IoT Hub. In dieser Route ist `upstream` ein spezieller Empfänger, der Edge Hub anweist, Nachrichten an IoT Hub zu senden. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Klicken Sie auf **Weiter**.

5. Klicken Sie im Schritt **Vorlage überprüfen** auf **Senden**. 
6. Kehren Sie zur Seite mit den IoT Edge-Gerätedetails zurück, und klicken Sie auf **Aktualisieren**. Es sollte nun angezeigt werden, dass das neue **filtermodule**-Modul zusammen mit dem **tempSensor**-Modul und der **IoT Edge-Laufzeit** ausgeführt wird. 

## <a name="view-generated-data"></a>Anzeigen generierter Daten

So überwachen Sie Gerät-zu-Cloud-Nachrichten, die von Ihrem IoT Edge-Gerät an IoT Hub gesendet wurden:
1. Konfigurieren Sie die Verbindungszeichenfolge für Ihren IoT Hub in der Azure IoT Toolkit-Erweiterung: 
    1. Öffnen Sie über den Menübefehl **Ansicht > Explorer** den Visual Studio Code-Explorer. 
    3. Klicken Sie im Explorer auf **IOT HUB DEVICES** (IOT HUB-GERÄTE), und klicken Sie dann auf **...**. Klicken Sie auf **Set IoT Hub Connection String** (IoT Hub-Verbindungszeichenfolge festlegen), und geben Sie die Verbindungszeichenfolge für den IoT Hub, mit dem Ihr IoT Edge-Gerät verbunden ist, in das Popupfenster ein. 

        Um die Verbindungszeichenfolge zu ermitteln, klicken Sie im Azure-Portal auf die Kachel für Ihren IoT Hub, und klicken Sie dann auf **Richtlinien für gemeinsamen Zugriff**. Klicken Sie unter **Richtlinien für gemeinsamen Zugriff** auf die Richtlinie **iothubowner**, und kopieren Sie die im Fenster **iothubowner** angegebene IoT Hub-Verbindungszeichenfolge.   

1. Zum Überwachen von Daten, die beim IoT Hub eintreffen, verwenden Sie den Menübefehl **Ansicht > Befehlspalette > IoT: Start Monitoring D2C Message** (Überwachung von Gerät-zu-Cloud-Nachricht starten). 
2. Zum Beenden der Datenüberwachung verwenden Sie den Menübefehl **Ansicht > Befehlspalette > IoT: Stop Monitoring D2C Message** (Überwachung von Gerät-zu-Cloud-Nachricht beenden). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul erstellt, das Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Sie können mit einem der nachfolgenden Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Bereitstellen einer Azure-Funktion als Modul](tutorial-deploy-function.md)
> [Bereitstellen von Azure Stream Analytics als Modul](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
