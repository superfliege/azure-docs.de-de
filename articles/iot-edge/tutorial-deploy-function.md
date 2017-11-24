---
title: Bereitstellen von Azure-Funktionen mit Azure IoT Edge | Microsoft-Dokumentation
description: "Bereitstellen einer Azure-Funktion als Modul auf einem Edge-Gerät"
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 34ed5083b952c42d4ed119b6986db965eb9eb67a
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Bereitstellen einer Azure-Funktion als IoT Edge-Modul – Vorschau
Mithilfe von Azure Functions können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. Dieses Tutorial führt Sie durch die Erstellung und Bereitstellung einer Azure-Funktion, die Sensordaten des simulierten IoT Edge-Geräts filtert, das Sie im Tutorial zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] erstellt haben. In diesem Tutorial lernen Sie Folgendes:     

> [!div class="checklist"]
> * Erstellen einer Azure-Funktion mit Visual Studio Code
> * Erstellen und Veröffentlichen eines Docker-Image in Ihrer Registrierung mithilfe von Visual Studio Code und Docker 
> * Bereitstellen des Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten


Das von Ihnen in diesem Tutorial erstellte Azure-Funktion filtert die von Ihrem Gerät generierten Temperaturdaten und leitet Nachrichten nur dann an Azure IoT Hub weiter, wenn die Temperatur einen angegebenen Schwellenwert überschreitet. 

## <a name="prerequisites"></a>Voraussetzungen

* Das Azure IoT Edge-Gerät, das Sie mit der Schnellstartanleitung oder im ersten Tutorial erstellt haben.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Erweiterung C# for Visual Studio Code mit Unterstützung von OmniSharp](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) (Sie können die Erweiterung über den Erweiterungsbereich in Visual Studio Code installieren)
* [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) (Sie können die Erweiterung über den Erweiterungsbereich in Visual Studio Code installieren)
* [Docker](https://docs.docker.com/engine/installation/) Die Community Edition (CE) für Ihre Plattform ist für dieses Tutorial ausreichend. 
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 

## <a name="set-up-a-docker-registry"></a>Einrichten einer Docker-Registrierung
In diesem Tutorial verwenden Sie die Azure IoT Edge-Erweiterung für Visual Studio Code zum Erstellen einer Azure-Funktion und eines [Docker-Image](https://docs.docker.com/glossary/?term=image) aus dieser Azure-Funktion. Danach pushen Sie dieses Docker-Image in ein [Docker-Repository](https://docs.docker.com/glossary/?term=repository), das in einer [Docker-Registrierung](https://docs.docker.com/glossary/?term=registry) gehostet wird. Abschließend stellen Sie Ihr Docker-Image gepackt als [Docker-Container](https://docs.docker.com/glossary/?term=container) aus der Registrierung für Ihr IoT Edge-Gerät bereit.  

Sie können für dieses Tutorial jede beliebige Docker-kompatible Registrierung verwenden. Zwei beliebte Docker-Registrierungsdienste, die in der Cloud verfügbar sind, sind **Azure Container Registry** und **Docker Hub**:

- [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/) ist mit einem [kostenpflichtigen Abonnement](https://azure.microsoft.com/en-us/pricing/details/container-registry/) erhältlich. Für die Zwecke dieses Tutorials ist ein **Basic**-Abonnement ausreichend. 

- Mit [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) erhalten Sie ein kostenloses privates Repository, wenn Sie eine (kostenlose) Docker-ID registrieren. 
    1. Zur Registrierung einer Docker-ID folgen Sie den Anweisungen unter [Register for a Docker ID](https://docs.docker.com/docker-id/#register-for-a-docker-id) (Docker-ID registrieren) auf der Docker-Website. 

    2. Um ein privates Docker-Repository zu erstellen, folgen Sie danach den Anweisungen im Abschnitt [Creating a New Repository on Docker Hub](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) (Erstellen eines neuen Repositorys auf Docker Hub) auf der Docker-Website.

In diesem Tutorial werden wir an den jeweiligen Stellen Befehle sowohl für Azure Container Registry als auch für Docker Hub angeben.

## <a name="create-a-function-project"></a>Erstellen eines Funktionsprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung eine IoT Edge-Funktion erstellen.
1. Öffnen Sie Visual Studio Code.
2. Öffnen Sie über den Menübefehl **Ansicht > Integriertes Terminal** das in Visual Studio Code integrierte Terminal.
3. Geben Sie im integrierten Terminal den folgenden Befehl ein, um die Vorlage **AzureIoTEdgeModule** in Dotnet zu installieren (oder zu aktualisieren):

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Geben Sie im integrierten Terminal den folgenden Befehl ein, um ein Projekt für das neue Modul zu erstellen:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

    >[!NOTE]
    > Mit diesem Befehl wird der Projektordner **FilterModule** im aktuellen Arbeitsverzeichnis erstellt. Wenn Sie ihn an einen anderen Speicherort erstellen möchten, wechseln Sie vor dem Ausführen des Befehls dorthin.

3. Klicken Sie auf **Datei > Ordner öffnen**, navigieren Sie zum Ordner **FilterFunction**, und klicken Sie auf **Ordner auswählen**, um das Projekt in Visual Studio Code zu öffnen.
4. Klicken Sie im Visual Studio Code-Explorer auf den Ordner **EdgeHubTrigger-Csharp**, und klicken Sie dann auf die Datei **run.csx**, um sie zu öffnen.
5. Fügen Sie die folgende Anweisung nach der `#r "Microsoft.Azure.Devices.Client"`-Anweisung hinzu:

    ```csharp
    #r "Newtonsoft.Json"
    ```

5. Fügen Sie die folgenden using-Anweisungen hinter den vorhandenen `using`-Anweisungen hinzu:

    ```csharp
    using Newtonsoft.Json;
    ```

1. Fügen Sie die folgenden Klassen hinzu. Diese Klassen definieren das erwartete Schema für den Textkörper eingehender Nachrichten.

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

1. Ersetzen Sie den Methodenkörper der Methode **Run** durch den folgenden Code. Sie filtert Nachrichten anhand des im Nachrichtentext angegebenen Temperaturwerts und des Temperaturschwellenwerts.

    ```csharp
    const int temperatureThreshold = 25;

    byte[] messageBytes = messageReceived.GetBytes();
    var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

    if (!string.IsNullOrEmpty(messageString))
    {
        // Get the body of the message and deserialize it
        var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);
        
        if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
        {
            // We will send the message to the output as the temperature value is greater than the threashold
            var filteredMessage = new Message(messageBytes);
            // We need to copy the properties of the original message into the new Message object
            foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
            {
                filteredMessage.Properties.Add(prop.Key, prop.Value);
            }
            // We are adding a new property to the message to indicate it is an alert
            filteredMessage.Properties.Add("MessageType", "Alert");
            // Send the message        
            await output.AddAsync(filteredMessage);
            log.Info("Received and transfered a message with temperature above the threshold");
        }
    }
    ```

11. Speichern Sie die Datei.

## <a name="publish-a-docker-image"></a>Veröffentlichen eines Docker-Image

1. Erstellen Sie das Docker-Image.
    1. Klicken Sie im Visual Studio Code-Explorer auf den Ordner **Docker**, um ihn zu öffnen. Wählen Sie nun den Ordner für Ihre Containerplattform aus (entweder **linux-x64** oder **windows-nano**). 
    2. Klicken Sie mit der rechten Maustaste auf die Datei **Dockerfile**, und klicken Sie auf **Build IoT Edge Module Docker Image** (Docker-Image für IoT Edge-Modul erstellen). 
    3. Navigieren Sie im Feld **Ordner auswählen** zum Ordner **Docker/linux-x64**, und klicken Sie auf **Select Folder as EXE_DIR** (Ordner als EXE_DIR auswählen). 
    4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel:`<docker registry address>/filterfunction:latest`. Hierbei ist *docker registry address* Ihre Docker-ID, sofern Sie Docker Hub verwenden; setzen Sie hingegen Azure Container Registry ein, dann ist dies ein Wert ähnlich wie `<your registry name>.azurecr.io`.
 
4. Melden Sie sich bei Docker an. Geben Sie den folgenden Befehl in das integrierte Terminal ein: 

    - Docker Hub (geben Sie auf Aufforderung Ihre Anmeldeinformationen ein):
        
        ```csh/sh
        docker login
        ```

    - Azure Container Registry:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Zur Ermittlung von Benutzername, Kennwort und die Anmeldeserver für diesen Befehl wechseln Sie zum [Azure-Portal](https://portal.azure.com). Klicken Sie unter **Alle Ressourcen** auf die Kachel Ihrer Azure-Containerregistrierung, um deren Eigenschaften zu öffnen. Klicken Sie dann auf **Zugriffsschlüssel**. Kopieren Sie die Werte der Felder **Benutzername**, **Kennwort** und **Anmeldeserver**. Der Anmeldeserver sollte folgendes Format aufweisen: `<your registry name>.azurecr.io`.

3. Pushen Sie das Image in Ihr Docker-Repository. Klicken Sie auf **Ansicht >Befehlspalette > Edge: Push IoT Edge Module Docker Image** (Edge: Docker-Image für IoT Edge-Modul pushen), und geben Sie den Imagenamen in das Popuptextfeld oben im Visual Studio Code-Fenster ein. Verwenden Sie dabei den Imagenamen aus Schritt 1.c.

## <a name="add-registry-credentials-to-your-edge-device"></a>Hinzufügen von Registrierungsanmeldeinformationen zur Edge-Runtime auf Ihrem Edge-Gerät
Fügen Sie die Anmeldeinformationen für Ihre Registrierung zur Edge-Runtime auf dem Computer hinzu, auf dem Sie Ihr Edge-Gerät ausgeführen. Hierdurch erhält die Runtime Zugriff zum Pullen des Containers. 

- Führen Sie für Windows den folgenden Befehl aus:
    
    ```cmd/sh
    iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

- Führen Sie für Linux den folgenden Befehl aus:
    
    ```cmd/sh
    sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

## <a name="run-the-solution"></a>Ausführen der Lösung

1. Navigieren Sie im **Azure-Portal** zu Ihrem IoT Hub.
2. Wechseln Sie zu **IoT Edge (Vorschau)**, und wählen Sie Ihr IoT Edge-Gerät aus.
1. Wählen Sie **Module festlegen** aus. 
2. Fügen Sie das Modul **tempSensor** hinzu. Dieser Schritt ist nur erforderlich, wenn Sie das Modul **tempSensor** nicht bereits auf Ihrem IoT Edge-Gerät bereitgestellt haben.
    1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `tempSensor` ein.
    3. Geben Sie im Feld **Image-URI** die Zeichenfolge `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` ein.
    4. Behalten Sie die restlichen Einstellungen unverändert bei, und wählen Sie **Speichern** aus.
1. Fügen Sie das Modul **filterfunction** hinzu.
    1. Wählen Sie erneut **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `filterfunction` ein.
    3. Geben Sie im Feld **Image** die Imageadresse ein, z.B. `<docker registry address>/filterfunction:latest`.
    74. Klicken Sie auf **Speichern**.
2. Klicken Sie auf **Weiter**.

3. Kopieren Sie im Schritt **Routen angeben** den folgenden JSON-Code in das Textfeld. Module veröffentlichen alle Nachrichten in der Edge-Runtime. Deklarative Regeln in der Runtime definieren, wohin diese Nachrichten fließen. In diesem Tutorial benötigen Sie zwei Routen. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „input1“ zum Filtermodul. Dies ist der Endpunkt, für den Sie den **FilterMessages**-Handler konfiguriert haben. Die zweite Route transportiert Nachrichten vom Filtermodul an IoT Hub. In dieser Route ist `upstream` ein spezieller Empfänger, der Edge Hub anweist, Nachrichten an IoT Hub zu senden. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Klicken Sie auf **Weiter**.

5. Klicken Sie im Schritt **Vorlage überprüfen** auf **Senden**. 
6. Kehren Sie zur Seite mit den IoT Edge-Gerätedetails zurück, und klicken Sie auf **Aktualisieren**. Es sollte nun angezeigt werden, dass das neue **filterfunction**-Modul zusammen mit dem **tempSensor**-Modul und der **IoT Edge-Runtime** ausgeführt wird. 

## <a name="view-generated-data"></a>Anzeigen generierter Daten

So überwachen Sie Gerät-zu-Cloud-Nachrichten, die von Ihrem IoT Edge-Gerät an Ihren IoT Hub gesendet wurden:
1. Konfigurieren Sie die Verbindungszeichenfolge für Ihren IoT Hub in der Azure IoT-Toolkit-Erweiterung: 
    1. Öffnen Sie über den Menübefehl **Ansicht > Explorer** den Visual Studio Code-Explorer. 
    3. Klicken Sie im Explorer auf **IOT HUB DEVICES** (IOT HUB-GERÄTE), und klicken Sie dann auf **...**. Klicken Sie auf **Set IoT Hub Connection String** (IoT Hub-Verbindungszeichenfolge festlegen), und geben Sie die Verbindungszeichenfolge für den IoT Hub, mit dem Ihr IoT Edge-Gerät verbunden ist, in das Popupfenster ein. 

        Um die Verbindungszeichenfolge zu ermitteln, klicken Sie im Azure-Portal auf die Kachel für Ihren IoT Hub, und klicken Sie dann auf **Richtlinien für gemeinsamen Zugriff**. Klicken Sie unter **Richtlinien für gemeinsamen Zugriff** auf die Richtlinie **iothubowner**, und kopieren Sie die im Fenster **iothubowner** angegebene IoT Hub-Verbindungszeichenfolge.   

1. Zum Überwachen von Daten, die beim IoT Hub eintreffen, verwenden Sie den Menübefehl **Ansicht > Befehlspalette > IoT: Start Monitoring D2C Message** (Überwachung von Gerät-zu-Cloud-Nachricht starten). 
2. Zum Beenden der Datenüberwachung verwenden Sie den Menübefehl **Ansicht > Befehlspalette > IoT: Stop Monitoring D2C Message** (Überwachung von Gerät-zu-Cloud-Nachricht beenden). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Funktion erstellt, die Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Wenn Sie Azure IoT Edge weiter erkunden möchten, können Sie nun erfahren, wie Sie ein IoT Edge-Gerät als Gateway verwenden. 

> [!div class="nextstepaction"]
> [Erstellen eines IoT Edge-Gatewaygeräts](how-to-create-gateway-device.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
