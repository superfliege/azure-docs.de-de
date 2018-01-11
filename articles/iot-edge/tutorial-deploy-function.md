---
title: Bereitstellen von Azure-Funktionen mit Azure IoT Edge | Microsoft-Dokumentation
description: "Bereitstellen einer Azure-Funktion als Modul auf einem Edge-Gerät"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1dfe46d307a076ae02362c4bba292602001ed915
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/09/2017
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
* [Erweiterung C# for Visual Studio Code mit Unterstützung von OmniSharp](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker](https://docs.docker.com/engine/installation/) Die Community Edition (CE) für Ihre Plattform ist für dieses Tutorial ausreichend. 
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

## <a name="create-a-function-project"></a>Erstellen eines Funktionsprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung eine IoT Edge-Funktion erstellen.
1. Öffnen Sie Visual Studio Code.
2. Um das in Visual Studio Code integrierte Terminal zu öffnen, wählen Sie **Ansicht** > **Integriertes Terminal** aus.
3. Geben Sie im integrierten Terminal den folgenden Befehl ein, um die Vorlage **AzureIoTEdgeFunction** in Dotnet zu installieren (oder zu aktualisieren):

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Erstellen Sie ein Projekt für das neue Modul. Mit dem folgenden Befehl wird der Projektordner **FilterFunction** im aktuellen Arbeitsverzeichnis erstellt:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

3. Wählen Sie **Datei** > **Ordne öffnen** aus, und navigieren Sie dann zum Ordner **FilterFunction**, um das Projekt in Visual Studio Code zu öffnen.
4. Erweitern Sie im Visual Studio Code-Explorer den Ordner **EdgeHubTrigger-Csharp**, und öffnen Sie dann die Datei **run.csx**.
5. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }
                // Add a new property to the message to indicate it is an alert
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message        
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages
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

11. Speichern Sie die Datei.

## <a name="publish-a-docker-image"></a>Veröffentlichen eines Docker-Image

1. Erstellen Sie das Docker-Image.
    1. Erweitern Sie im Visual Studio Code-Explorer den Ordner **Docker**. Erweitern Sie dann den Ordner für Ihre Containerplattform (entweder **linux-x64** oder **windows-nano**). 
    2. Klicken Sie mit der rechten Maustaste auf die Datei **Dockerfile**, und klicken Sie auf **Build IoT Edge Module Docker Image** (Docker-Image für IoT Edge-Modul erstellen). 
    3. Navigieren Sie zum Projektordner **FilterFunction**, und klicken Sie dann auf **Ordner als EXE_DIR auswählen**. 
    4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filterfunction:latest`. Die Adresse der Containerregistrierung ist identisch mit dem Anmeldeserver, den Sie aus der Registrierung kopiert haben. Sie sollte das Format `<your container registry name>.azurecr.io` aufweisen.
 
4. Melden Sie sich bei Docker an. Geben Sie den folgenden Befehl in das integrierte Terminal ein: 

   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Zur Ermittlung von Benutzername, Kennwort und die Anmeldeserver für diesen Befehl wechseln Sie zum [Azure-Portal](https://portal.azure.com). Klicken Sie unter **Alle Ressourcen** auf die Kachel Ihrer Azure-Containerregistrierung, um deren Eigenschaften zu öffnen. Klicken Sie dann auf **Zugriffsschlüssel**. Kopieren Sie die Werte der Felder **Benutzername**, **Kennwort** und **Anmeldeserver**. 

3. Pushen Sie das Image in Ihr Docker-Repository. Wählen Sie **Ansicht** > **Befehlspalette...** aus, und suchen Sie **Edge: Push IoT Edge module Docker image** (Edge: Docker-Image für IoT Edge-Modul pushen).
4. Geben Sie im Popuptextfeld denselben Imagenamen ein, den Sie in Schritt 1.d verwendet haben.

## <a name="add-registry-credentials-to-your-edge-device"></a>Hinzufügen von Registrierungsanmeldeinformationen zur Edge-Runtime auf Ihrem Edge-Gerät
Fügen Sie die Anmeldeinformationen für Ihre Registrierung zur Edge-Runtime auf dem Computer hinzu, auf dem Sie Ihr Edge-Gerät ausgeführen. Hierdurch erhält die Runtime Zugriff zum Pullen des Containers. 

- Führen Sie für Windows den folgenden Befehl aus:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Führen Sie für Linux den folgenden Befehl aus:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Ausführen der Lösung

1. Navigieren Sie im **Azure-Portal** zu Ihrem IoT Hub.
2. Wechseln Sie zu **IoT Edge (Vorschau)**, und wählen Sie Ihr IoT Edge-Gerät aus.
1. Wählen Sie **Module festlegen** aus. 
2. Wenn Sie das Modul **tempSensor** bereits auf diesem Gerät bereitgestellt haben, wird es möglicherweise automatisch ausgefüllt. Andernfalls führen Sie die folgenden Schritte aus, um es hinzuzufügen:
    1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `tempSensor` ein.
    3. Geben Sie im Feld **Image-URI** die Zeichenfolge `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` ein.
    4. Behalten Sie die restlichen Einstellungen unverändert bei, und wählen Sie **Speichern** aus.
1. Fügen Sie das Modul **filterFunction** hinzu.
    1. Wählen Sie erneut **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `filterFunction` ein.
    3. Geben Sie im Feld **Image** die Imageadresse ein, z.B. `<docker registry address>/filterfunction:latest`.
    74. Klicken Sie auf **Speichern**.
2. Klicken Sie auf **Weiter**.

3. Kopieren Sie im Schritt **Routen angeben** den folgenden JSON-Code in das Textfeld. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „input1“ zum Filtermodul. Die zweite Route transportiert Nachrichten vom Filtermodul an IoT Hub. In dieser Route ist `$upstream` ein spezieller Empfänger, der Edge Hub anweist, Nachrichten an IoT Hub zu senden. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Klicken Sie auf **Weiter**.

5. Klicken Sie im Schritt **Vorlage überprüfen** auf **Senden**. 
6. Kehren Sie zur Seite mit den IoT Edge-Gerätedetails zurück, und klicken Sie auf **Aktualisieren**. Es sollte nun angezeigt werden, dass das neue **filterfunction**-Modul zusammen mit dem **tempSensor**-Modul und der **IoT Edge-Runtime** ausgeführt wird. 

## <a name="view-generated-data"></a>Anzeigen generierter Daten

So überwachen Sie Gerät-zu-Cloud-Nachrichten, die von Ihrem IoT Edge-Gerät an IoT Hub gesendet wurden:
1. Konfigurieren Sie die Verbindungszeichenfolge für Ihren IoT Hub in der Azure IoT-Toolkit-Erweiterung: 
    1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub, und wählen **Freigegebene Zugriffsrichtlinien** aus. 
    2. Wählen Sie **iothubowner** aus, und kopieren Sie dann den Wert von **Verbindungszeichenfolge – Primärschlüssel**.
    1. Klicken Sie im Visual Studio Code-Explorer auf **IOT HUB DEVICES** (IOT HUB-GERÄTE), und klicken Sie dann auf **...**. 
    1. Wählen Sie **Set IoT Hub Connection String** (IoT Hub-Verbindungszeichenfolge festlegen) aus, und geben Sie die IoT Hub-Verbindungszeichenfolge in das Popupfenster ein. 

1. Zum Überwachen von Daten, die beim IoT Hub eingehen, wählen Sie **Ansicht** > **Befehlspalette...** aus, und suchen Sie nach **IoT: Start monitoring D2C message** (Überwachung von Gerät-zu-Cloud-Nachricht starten). 
2. Zum Beenden der Datenüberwachung verwenden Sie den Befehl **IoT: Stop monitoring D2C message** (IoT: Überwachung von Gerät-zu-Cloud-Nachricht beenden). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Funktion erstellt, die Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Wenn Sie Azure IoT Edge weiter erkunden möchten, können Sie nun erfahren, wie Sie ein IoT Edge-Gerät als Gateway verwenden. 

> [!div class="nextstepaction"]
> [Erstellen eines IoT Edge-Gatewaygeräts](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
