---
title: Bereitstellen von Azure-Funktionen mit Azure IoT Edge | Microsoft-Dokumentation
description: Bereitstellen einer Azure-Funktion als Modul auf einem Edge-Gerät
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0445817f9ff403156025e38a1e14a3892a9a292b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031191"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules---preview"></a>Tutorial: Bereitstellen von Azure Functions als IoT Edge-Module (Vorschau)

Mithilfe von Azure Functions können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. Dieses Tutorial führt Sie durch die Erstellung und Bereitstellung einer Azure-Funktion, die Sensordaten des simulierten IoT Edge-Geräts filtert, das Sie im Tutorial zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] erstellt haben. In diesem Tutorial lernen Sie Folgendes:     

> [!div class="checklist"]
> * Erstellen einer Azure-Funktion mit Visual Studio Code
> * Erstellen und Veröffentlichen eines Docker-Images in einer Containerregistrierung mithilfe von VS Code und Docker 
> * Bereitstellen des Moduls über die Containerregistrierung auf Ihrem IoT Edge-Gerät
> * Anzeigen gefilterter Daten

>[!NOTE]
>Die Azure Functions-Module in Azure IoT Edge sind als öffentliche [Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) verfügbar. 

Das von Ihnen in diesem Tutorial erstellte Azure-Funktion filtert die von Ihrem Gerät generierten Temperaturdaten und leitet Nachrichten nur dann an Azure IoT Hub weiter, wenn die Temperatur einen angegebenen Schwellenwert überschreitet. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein IoT Edge-Gerät, um die in diesem Tutorial erstellten Functions-Module zu testen. Sie können das Gerät verwenden, das Sie in der Schnellstartanleitung für [Linux](quickstart-linux.md) oder [Windows](quickstart.md) konfiguriert haben.

Auf dem Entwicklungscomputer müssen die folgenden Komponenten vorhanden sein: 
* [Visual Studio Code](https://code.visualstudio.com/). 
* [C#-Erweiterung für Visual Studio Code (unterstützt von OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) für Visual Studio Code 
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)
* [Docker CE](https://docs.docker.com/install/) auf dem Entwicklungscomputer 

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung
In diesem Tutorial verwenden Sie die Azure IoT Edge-Erweiterung für VSCode zum Entwickeln eines Moduls und zum Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.  

Sie können für dieses Tutorial jede beliebige Docker-kompatible Registrierung verwenden. Zwei beliebte Docker-Registrierungsdienste, die in der Cloud verfügbar sind, sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet. 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Container Registry** aus.

    ![Erstellen einer Containerregistrierung](./media/tutorial-deploy-function/create-container-registry.png)

2. Benennen Sie die Registrierung, und wählen Sie ein Abonnement aus.
3. Es wird empfohlen, für die Ressourcengruppe den gleichen Namen wie für die Ressourcengruppe zu verwenden, die Ihre IoT Hub-Instanz enthält. Indem Sie alle Ressourcen in der gleichen Gruppe zusammenfassen, können Sie sie zusammen verwalten. Wenn Sie beispielsweise die zum Testen verwendete Ressourcengruppe löschen, werden alle Testressourcen in dieser Gruppe gelöscht. 
4. Legen Sie die SKU auf **Basic** fest, und schalten Sie **Administratorbenutzer** auf **Aktivieren** um. 
5. Klicken Sie auf **Create**.
6. Navigieren Sie nach der Erstellung der Containerregistrierung zu dieser Registrierung, und wählen Sie **Zugriffsschlüssel** aus. 
7. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. Sie verwenden diese Werte später im Tutorial. 

## <a name="create-a-function-project"></a>Erstellen eines Funktionsprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung eine IoT Edge-Funktion erstellen.

1. Öffnen Sie Visual Studio Code.
2. Öffnen Sie das in VS Code integrierte Terminal, indem Sie **Ansicht** > **Integriertes Terminal** auswählen. 
2. Öffnen Sie die VS Code-Befehlspalette, indem Sie auf **Ansicht** > **Befehlspalette** klicken.
3. Geben Sie in der Befehlspalette den Befehl **Azure: Sign in** (Azure: Anmelden) ein, und führen Sie ihn aus. Befolgen Sie die Anweisungen zum Anmelden beim Azure-Konto. Wenn Sie bereits angemeldet sind, können Sie diesen Schritt überspringen.
3. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: Neue IoT Edge-Projektmappe) ein, und führen Sie ihn aus. Geben Sie in der Befehlspalette die folgenden Informationen an, um die Projektmappe zu erstellen: 

   1. Wählen Sie den Ordner aus, in dem die Projektmappe erstellt werden soll. 
   2. Geben Sie einen Namen für Ihre Projektmappe ein, oder übernehmen Sie den Standardnamen **EdgeSolution**.
   3. Wählen Sie als Modulvorlage die Option **Azure Functions - C#** aus. 
   4. Nennen Sie das Modul **CSharpFunction**. 
   5. Geben Sie die Azure Container Registry-Instanz an, die Sie im vorherigen Abschnitt als Imagerepository für das erste Modul erstellt haben. Ersetzen Sie **localhost:5000** durch den kopierten Wert für den Anmeldeserver. Die endgültige Zeichenfolge sieht wie folgt aus: **\<Registrierungsname\>.azurecr.io/csharpfunction**.

4. Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Er enthält den Ordner **.vscode**, einen Ordner für **Module**, eine Bereitstellungsmanifest-Vorlagendatei und eine Datei vom Typ **.env**. Öffnen Sie **Module** > **CSharpFunction** > **EdgeHubTrigger-Csharp** > **run.csx**.

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
                    filteredMessage.Properties.Add(prop.Key, prop.Value);                }
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
        public Machine machine {get; set;}
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

6. Speichern Sie die Datei .

## <a name="build-your-iot-edge-solution"></a>Erstellen Ihrer IoT Edge-Projektmappe

Im vorherigen Abschnitt haben Sie eine IoT Edge-Projektmappe erstellt und dem CSharpFunction-Modul Code hinzugefügt, der Nachrichten herausfiltert, deren gemeldete Computertemperatur unter dem zulässigen Schwellenwert liegt. Nun müssen Sie die Projektmappe als Containerimage erstellen und per Push an die Containerregistrierung übertragen.

1. Melden Sie sich durch Eingabe des folgenden Befehls im integrierten Visual Studio Code-Terminal bei Docker an, um das Modulimage per Push an ACR zu übertragen: 
     
    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Verwenden Sie den Benutzernamen und den Anmeldeserver, die Sie zuvor aus Azure Container Registry kopiert haben. Sie werden zur Eingabe des Kennworts aufgefordert. Fügen Sie Ihr Kennwort in die Eingabeaufforderung ein, und drücken Sie die **EINGABETASTE**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

2. Öffnen Sie im VS Code-Explorer die Datei **deployment.template.json** im Arbeitsbereich für Ihre IoT Edge-Projektmappe. Diese Datei teilt der IoT Edge-Runtime mit, welche Module auf einem Gerät bereitgestellt werden sollen. Weitere Informationen zu Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können – Vorschau](module-composition.md).

3. Suchen Sie im Bereitstellungsmanifest den Abschnitt **registryCredentials**. Aktualisieren Sie **Benutzername**, **Kennwort** und **Adresse** mit den Anmeldeinformationen der Containerregistrierung. In diesem Abschnitt wird der IoT Edge-Runtime auf Ihrem Gerät die Berechtigung erteilt, die in der privaten Registrierung gespeicherten Containerimages per Pull abzurufen. Die tatsächlichen Benutzername- und Kennwortpaare werden in der ENV-Datei gespeichert, die von Git ignoriert wird.

5. Speichern Sie diese Datei.

6. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **deployment.template.json**, und klicken Sie dann auf **Build IoT Edge solution** (IoT Edge-Projektmappe erstellen). 

Wenn Sie Visual Studio Code anweisen, die Projektmappe zu erstellen, wird zuerst basierend auf den Informationen in der Bereitstellungsvorlage eine `deployment.json`-Datei in einem neuen Konfigurationsordner (**config**) generiert. Anschließend werden zwei Befehle im integrierten Terminal ausgeführt: `docker build` und `docker push`. Diese beiden Befehle erstellen den Code, packen die Funktionen in Container und führen dann eine Pushübertragung an die Containerregistrierung durch, die Sie beim Initialisieren der Projektmappe angegeben haben. 

## <a name="view-your-container-image"></a>Anzeigen des Containerimages

Visual Studio Code gibt eine Erfolgsmeldung aus, wenn das Containerimage an die Containerregistrierung übertragen wurde. Wenn Sie selbst überprüfen möchten, ob die Ausführung erfolgreich war, können Sie das Image in der Registrierung anzeigen. 

1. Navigieren Sie im Azure-Portal wieder zu Ihrer Azure-Containerregistrierung. 
2. Wählen Sie **Repositorys** aus.
3. **csharpfunction** sollte in Ihren Repositorys aufgeführt sein. Wählen Sie das Repository aus, um weitere Details anzuzeigen.
4. Im Abschnitt **Tags** sollte **0.0.1-amd64** angezeigt werden. Dieses Tag spiegelt die Version und die Plattform des erstellten Images wider. Diese Werte werden in der Datei **module.json** im Ordner „CSharpFunction“ festgelegt. 

## <a name="deploy-and-run-the-solution"></a>Bereitstellen und Ausführen der Projektmappe

Wie in den Schnellstartanleitungen können Sie das Functions-Modul über das Azure-Portal auf einem IoT Edge-Gerät bereitstellen. Sie können Module jedoch auch über Visual Studio Code bereitstellen und überwachen. In den folgenden Abschnitten wird die Azure IoT Edge-Erweiterung für VS Code verwendet, die als erforderliche Komponente aufgeführt ist. Installieren Sie sie jetzt, sofern noch nicht geschehen. 

1. Öffnen Sie die VS Code-Befehlspalette, indem Sie auf **Ansicht** > **Befehlspalette** klicken.

2. Suchen Sie den Befehl **Azure: Sign in** (Azure: Anmelden), und führen Sie ihn aus. Befolgen Sie die Anweisungen zum Anmelden bei Ihrem Azure-Konto. 

3. Suchen Sie in der Befehlspalette nach dem Befehl **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: IoT Hub auswählen), und führen Sie den Befehl aus. 

4. Wählen Sie das Abonnement mit Ihrem IoT-Hub und dann den IoT-Hub aus, auf den Sie zugreifen möchten.

5. Erweitern Sie im VS Code-Explorer den Abschnitt **Azure IoT Hub-Geräte**. 

6. Klicken Sie mit der rechten Maustaste auf den Namen Ihres IoT Edge-Geräts, und klicken Sie dann auf **Create Deployment for IoT Edge device** (Bereitstellung für IoT Edge-Gerät erstellen). 

7. Navigieren Sie zum Projektmappenordner, der CSharpFunction enthält. Öffnen Sie den Konfigurationsordner (**config**), und wählen Sie die Datei **deployment.json** aus. Klicken Sie auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen).

8. Aktualisieren Sie den Abschnitt **Azure IoT Hub-Geräte**. Nun sollte das neue **CSharpFunction**-Modul zusammen mit dem **TempSensor**-Modul sowie **$edgeAgent** und **$edgeHub** ausgeführt werden. 

   ![Anzeigen der bereitgestellten Module in VS Code](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-generated-data"></a>Anzeigen generierter Daten

Sie können alle beim IoT-Hub eingehenden Nachrichten anzeigen, indem Sie in der Befehlspalette **Azure IoT Hub: Start Monitoring D2C Message** (Azure IoT Hub: Überwachung von D2C-Nachrichten starten) ausführen.

Sie können auch filtern, um alle Nachrichten anzuzeigen, die von einem bestimmten Gerät an Ihren IoT-Hub gesendet werden. Klicken Sie im Abschnitt **Azure IoT Hub-Geräte** mit der rechten Maustaste auf das Gerät, und klicken Sie auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).

Wenn Sie die Nachrichtenüberwachung beenden möchten, führen Sie in der Befehlspalette den Befehl **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: Überwachung von D2C-Nachrichten beenden) aus. 


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)]

Entfernen Sie die IoT Edge-Runtime basierend auf der IoT-Geräteplattform (Linux oder Windows).

#### <a name="windows"></a>Windows

Entfernen Sie die IoT Edge-Runtime.

```Powershell
stop-service iotedge -NoWait
sleep 5
sc.exe delete iotedge
```

Löschen Sie die Container, die auf Ihrem Gerät erstellt wurden. 

```Powershell
docker rm -f $(docker ps -a --no-trunc --filter "name=edge" --filter "name=tempSensor" --filter "name=CSharpFunction")
```

#### <a name="linux"></a>Linux

Entfernen Sie die IoT Edge-Runtime.

```bash
sudo apt-get remove --purge iotedge
```

Löschen Sie die Container, die auf Ihrem Gerät erstellt wurden. 

```bash
sudo docker rm -f $(sudo docker ps -a --no-trunc --filter "name=edge" --filter "name=tempSensor" --filter "name=CSharpFunction")
```

Entfernen Sie die Containerruntime.

```bash
sudo apt-get remove --purge moby
```



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Azure Functions-Modul erstellt, das Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Wenn Sie eigene Module erstellen möchten, finden Sie entsprechende Informationen unter [Use Visual Studio Code to develop and debug Azure Functions for Azure IoT Edge](how-to-develop-csharp-function.md) (Entwickeln und Debuggen von Azure Functions für Azure IoT Edge mithilfe von Visual Studio Code). 

Sie können mit den nächsten Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Bereitstellen von Azure Stream Analytics als IoT Edge-Modul – Vorschau](tutorial-deploy-stream-analytics.md)

<!--Links-->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
