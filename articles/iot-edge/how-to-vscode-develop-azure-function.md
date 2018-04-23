---
title: Verwenden von Visual Studio Code zum Entwickeln und Bereitstellen von Azure Functions in Azure IoT Edge | Microsoft-Dokumentation
description: Entwickeln und Bereitstellen einer C#-Azure Functions-Funktion mit Azure IoT Edge in VSCode ohne Kontextwechsel
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 47d420b4b283b390f67719233c4bea59495a589a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2018
---
# <a name="use-visual-studio-code-to-develop-and-deploy-azure-functions-to-azure-iot-edge"></a>Verwenden von Visual Studio Code zum Entwickeln und Bereitstellen von Azure Functions-Funktionen in Azure IoT Edge

Dieser Artikel enthält detaillierte Anweisungen für die Verwendung von [Visual Studio Code](https://code.visualstudio.com/) als Hauptentwicklungstool zum Entwickeln und Bereitstellen von Azure Functions-Funktionen in IoT Edge. 

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer unter Windows oder Linux als Entwicklungscomputer verwenden. Bei Ihrem IoT Edge-Gerät kann es sich um ein anderes physisches Gerät handeln, oder Sie können Ihr IoT Edge-Gerät auf dem Entwicklungscomputer simulieren.

Stellen Sie sicher, dass Sie folgende Tutorials abgeschlossen haben, bevor Sie mit diesem Leitfaden beginnen.
- Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) oder [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Deploy Azure Functions (Bereitstellen von Azure Functions)](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-function)

Im Folgenden finden Sie eine Prüfliste mit den Elementen, über die Sie nach Abschluss der vorherigen Tutorials verfügen sollten.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [Erweiterung C# for Visual Studio Code mit Unterstützung von OmniSharp](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 
- [Python 2.7](https://www.python.org/downloads/)
- [IoT Edge-Steuerskript](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- AzureIoTEdgeFunction-Vorlage (`dotnet new -i Microsoft.Azure.IoT.Edge.Function`)
- Ein aktiver IoT Hub mit mindestens einem IoT Edge-Gerät

Es wird außerdem empfohlen, die [Docker-Unterstützung für VSCode](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) zu installieren, um die Verwaltung Ihrer Modulimages und -container zu verbessern.

> [!NOTE]
> Derzeit unterstützt Azure Functions unter IoT Edge nur C#.

## <a name="deploy-azure-iot-functions-in-vs-code"></a>Bereitstellen von Azure IoT-Funktionen in VSCode
Im Tutorial [Bereitstellen von Azure Functions](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-function) aktualisieren, erstellen und veröffentlichen Sie Ihre Funktionsmodulimages in Visual Studio Code und besuchen dann das Azure-Portal, um Azure Functions bereitzustellen. In diesem Abschnitt wird erläutert, wie Sie Ihre Azure Functions-Funktion mithilfe von VSCode bereitstellen und überwachen.

### <a name="start-a-local-docker-registry"></a>Starten einer lokalen Docker-Registrierung
Sie können für diesen Artikel eine beliebige, mit Docker kompatible Registrierung verwenden. Zwei beliebte Docker-Registrierungsdienste, die in der Cloud verfügbar sind, sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Abschnitt wird eine [lokale Docker-Registrierung](https://docs.docker.com/registry/deploying/) eingesetzt, die für Testzwecke in der frühen Entwicklungsphase einfacher zu verwenden ist.
Führen Sie im **integrierten Terminal** von VSCode (STRG+`) folgende Befehle aus, um eine lokale Registrierung zu starten.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Im Beispiel oben werden Registrierungskonfigurationen verwendet, die nur für Tests geeignet sind. Eine produktionsbereite Registrierung muss durch TLS geschützt werden und sollte idealerweise einen Zugriffssteuerungsmechanismus verwenden. Es wird empfohlen, [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) oder [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) für die Bereitstellung von produktionsbereiten IoT Edge-Modulen zu verwenden.

### <a name="create-a-function-project"></a>Erstellen eines Funktionsprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Modul auf Basis von .NET Core 2.0 erstellen. Wenn Sie diesen Abschnitt in einem vorherigen Tutorial abgeschlossen haben, können Sie ihn problemlos überspringen.

1. Wählen Sie in Visual Studio Code die Optionen **Ansicht** > **Integriertes Terminal** aus, um das in Visual Studio Code integrierte Terminal zu öffnen.
2. Geben Sie im integrierten Terminal den folgenden Befehl ein, um die Vorlage **AzureIoTEdgeFunction** in Dotnet zu installieren (oder zu aktualisieren):

   ```cmd/sh
   dotnet new -i Microsoft.Azure.IoT.Edge.Function
   ```
3. Erstellen Sie ein Projekt für das neue Modul. Mit dem folgenden Befehl wird der Projektordner **FilterFunction** im aktuellen Arbeitsverzeichnis erstellt:

   ```cmd/sh
   dotnet new aziotedgefunction -n FilterFunction
   ```
 
4. Wählen Sie **Datei > Ordner öffnen** aus, navigieren Sie zum Ordner **FilterFunction**, und öffnen Sie das Projekt in Visual Studio Code.
5. Navigieren Sie zum Ordner **FilterFunction**, und klicken Sie auf **Ordner auswählen**, um das Projekt in VSCode zu öffnen.
6. Erweitern Sie im Visual Studio Code-Explorer den Ordner **EdgeHubTrigger-Csharp**, und öffnen Sie dann die Datei **run.csx**.
7. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

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

8. Speichern Sie die Datei .

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Erstellen und Veröffentlichen eines Docker-Image in der Registrierung

1. Erweitern Sie im Visual Studio Code-Explorer den Ordner **Docker**. Erweitern Sie dann den Ordner für Ihre Containerplattform (entweder **linux-x64** oder **windows-nano**).
2. Klicken Sie mit der rechten Maustaste auf die Datei **Dockerfile**, und klicken Sie auf **Build IoT Edge Module Docker Image** (Docker-Image für IoT Edge-Modul erstellen). 

    ![Erstellen eines Docker-Images](./media/how-to-vscode-develop-csharp-function/build-docker-image.png)

3. Navigieren Sie zum Projektordner **FilterFunction**, und klicken Sie dann auf **Ordner als EXE_DIR auswählen**. 
4. Geben Sie den Imagenamen in das Popupfeld oben im Visual Studio Code-Fenster ein. Beispiel: `<your container registry address>/filterfunction:latest`. Wenn Sie eine Bereitstellung für die lokale Registrierung durchführen, sollte diese `localhost:5000/filterfunction:latest` lauten.
5. Pushen Sie das Image in Ihr Docker-Repository. Klicken Sie auf **Edge: Docker-Image für IoT Edge-Modul mithilfe von Push übertragen**, und geben Sie oben im VS Code-Fenster in das Popuptextfeld die Image-URL ein. Verwenden Sie dabei die Image-URL aus dem oben angegebenen Schritt.
    ![Pushen des Docker-Images](./media/how-to-vscode-develop-csharp-function/push-image.png)

### <a name="deploy-your-function-to-iot-edge"></a>Bereitstellen Ihrer Funktion in IoT Edge

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
   "filterfunction": {
      "version": "1.0",
      "type": "docker",
      "status": "running",
      "restartPolicy": "always",
      "settings": {
         "image": "localhost:5000/filterfunction:latest",
         "createOptions": ""
      }
   }
   ```

2. Ersetzen Sie den Abschnitt **routes** durch folgenden Inhalt:
   ```json
       "routes":{
           "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
           "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
   ```
   > [!NOTE]
   > Deklarative Regeln in der Runtime definieren, wohin diese Nachrichten fließen. In diesem Artikel benötigen Sie zwei Routen. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „input1“ zur Filterfunktion, den Endpunkt, für den Sie den FilterMessages-Handler konfiguriert haben. Die zweite Route transportiert Nachrichten von der Filterfunktion zu IoT Hub. In dieser Route ist „upstream“ ein spezielles Ziel, das Edge Hub anweist, Nachrichten an IoT Hub zu senden.

3. Speichern Sie diese Datei.
4. Wählen Sie in der Befehlspalette **Edge: Bereitstellung für Edge-Gerät erstellen** aus. Wählen Sie dann die ID Ihres IoT Edge-Geräts aus, um eine Bereitstellung zu erstellen. Klicken Sie alternativ mit der rechten Maustaste auf die Geräte-ID in der Geräteliste, und wählen Sie **Bereitstellung für Edge-Gerät erstellen** aus.

    ![Bereitstellung erstellen](./media/how-to-vscode-develop-csharp-function/create-deployment.png)

5. Wählen Sie die Datei `deployment.json` aus, die Sie aktualisiert haben. Im Ausgabefenster können Sie die entsprechenden Ausgaben für Ihre Bereitstellung sehen.
6. Starten Sie Ihre Edge-Runtime in der Befehlspalette. **Edge: Start Edge (Edge: Edge starten)**
7. Nun sehen Sie, dass Ihre IoT Edge-Runtime im Docker-Explorer mit dem simulierten Sensor und der Filterfunktion gestartet wird.

    ![Ausgeführte Lösung](./media/how-to-vscode-develop-csharp-function/solution-running.png)

8. Durch Klicken mit der rechten Maustaste auf die Edge-Geräte-ID können Sie D2C-Nachrichten in VS Code überwachen.

    ![Überwachen von Meldungen](./media/how-to-vscode-develop-csharp-function/monitor-d2c-messages.png)


## <a name="next-steps"></a>Nächste Schritte

[Debuggen von Azure Functions in VSCode](how-to-vscode-debug-azure-function.md)
