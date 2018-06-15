---
title: Python-Modul in Azure IoT Edge | Microsoft-Dokumentation
description: Erstellen eines IoT Edge-Moduls mit Python-Code und Bereitstellen dieses Moduls auf einem Edge-Gerät
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 88d772306cb9e67216b380aa885284ebedc77b5f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632107"
---
# <a name="develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device---preview"></a>Entwickeln und Bereitstellen eines Python-IoT Edge-Moduls für Ihr simuliertes Gerät – Vorschau

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. In diesem Tutorial wird beschrieben, wie Sie ein IoT Edge-Modul, mit dem Sensordaten gefiltert werden, erstellen und bereitstellen. Sie verwenden das simulierte IoT Edge-Gerät, das Sie in den Tutorials zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] erstellt haben. In diesem Tutorial lernen Sie Folgendes:    

> [!div class="checklist"]
> * Erstellen eines IoT Edge-Python-Moduls mit Visual Studio Code
> * Erstellen und Veröffentlichen eines Docker-Image in Ihrer Registrierung mithilfe von Visual Studio Code und Docker 
> * Bereitstellen des Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten


Das IoT Edge-Modul, das Sie in diesem Tutorial erstellen, filtert die von Ihrem Gerät generierten Temperaturdaten. Es leitet Nachrichten nur dann an nachgeschaltete Geräte weiter, wenn die Temperatur einen angegebenen Schwellenwert überschreitet. Diese Art der Analyse auf Edge-Ebene eignet sich zur Verringerung der Datenmenge, die in die Cloud weitergeleitet und dort gespeichert wird. 

> [!IMPORTANT]
> Das Python-Modul kann derzeit nur in amd64-Linux-Containern ausgeführt werden. Die Ausführung in Windows-Containern oder ARM-basierten Containern ist nicht möglich. 

## <a name="prerequisites"></a>Voraussetzungen

* Das Azure IoT Edge-Gerät, das Sie mit der Schnellstartanleitung oder im ersten Tutorial erstellt haben.
* Die Verbindungszeichenfolge des primären Schlüssels für das IoT Edge-Gerät  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Python-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Docker](https://docs.docker.com/engine/installation/) auf dem gleichen Computer, auf dem Visual Studio Code installiert ist. Die Community Edition (CE) ist für dieses Tutorial ausreichend. 
* [Python](https://www.python.org/downloads/).
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) zum Installieren von Python-Paketen (normalerweise in der Python-Installation enthalten).

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
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Python-Modul erstellen.
1. Wählen Sie in Visual Studio Code die Optionen **Ansicht** > **Integriertes Terminal** aus, um das in Visual Studio Code integrierte Terminal zu öffnen.
2. Geben Sie im integrierten Terminalfenster den folgenden Befehl ein, um **cookiecutter** zu installieren (oder zu aktualisieren) – es wird empfohlen, dies in einer virtuellen Umgebung oder als Benutzerinstallation durchzuführen, wie unten dargestellt:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Erstellen Sie ein Projekt für das neue Modul. Mit dem folgenden Befehl wird der Projektordner **FilterModule** in Ihrem Containerrepository erstellt. Der Parameter von `image_repository` muss bei Verwendung der Azure-Containerregistrierung in Form von `<your container registry name>.azurecr.io/filtermodule` vorliegen. Geben Sie den folgenden Befehl im aktuellen Arbeitsordner ein:

    ```cmd/sh
    cookiecutter --no-input https://github.com/Azure/cookiecutter-azure-iot-edge-module module_name=FilterModule image_repository=<your container registry address>/filtermodule
    ```
 
4. Wählen Sie **Datei** > **Ordner öffnen** aus.
5. Navigieren Sie zum Ordner **FilterModule**, und klicken Sie auf **Ordner auswählen**, um das Projekt in VS Code zu öffnen.
6. Klicken Sie im VS Code-Explorer auf **main.py**, um es zu öffnen.
7. Importieren Sie oben im **FilterModule**-Namespace die `json`-Bibliothek:

    ```python
    import json
    ```

8. Fügen Sie `TEMPERATURE_THRESHOLD`, `RECEIVE_CALLBACKS` und `TWIN_CALLBACKS` unter den globalen Leistungsindikatoren hinzu. Der Temperaturschwellenwert legt den Wert fest, den die gemessene Temperatur übersteigen muss, damit die Daten an IoT Hub gesendet werden.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = RECEIVE_CALLBACKS = 0
    ```

9. Aktualisieren Sie die Funktion `receive_message_callback` mit dem folgenden Inhalt.

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we will forward this message onto the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print("    Data: <<<{}>>> & Size={:d}".format(message_text, size))
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print("    Properties: {}".format(key_value_pair))
        RECEIVE_CALLBACKS += 1
        print("    Total calls received: {:d}".format(RECEIVE_CALLBACKS))
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature {} exceeds threshold {}".format(data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

10. Fügen Sie eine neue Funktion `device_twin_callback` hinzu. Diese Funktion wird aufgerufen, wenn die gewünschten Eigenschaften aktualisiert werden.

    ```python
    # device_twin_callback is invoked when twin's desired properties are updated.
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print("\nTwin callback called with:\nupdateStatus = {}\npayload = {}\ncontext = {}".format(update_state, payload, user_context))
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print("Total calls confirmed: {:d}\n".format(TWIN_CALLBACKS))
    ```

11. Fügen Sie in Klasse `HubManager` der `__init__`-Methode eine neue Zeile zum Initialisieren der `device_twin_callback`-Funktion hinzu, die Sie gerade hinzugefügt haben.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_device_twin_callback(device_twin_callback, self)
    ```


12. Speichern Sie diese Datei.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Erstellen und Veröffentlichen eines Docker-Image in der Registrierung

1. Melden Sie sich bei Docker an. Geben Sie dazu den folgenden Code im Terminal von Visual Studio Code ein: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Verwenden Sie den Benutzernamen, das Kennwort und den Anmeldeserver, die Sie bei der Erstellung aus Ihrer Azure-Containerregistrierung kopiert haben.

2. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **module.json**, und klicken Sie auf **IoT Edge-Modul-Docker-Image erstellen und pushen**. Wählen Sie im Popup-Dropdownlistenfeld oben im VS Code-Fenster Ihre Containerplattform aus, z.B. **amd64** für Linux-Container. Bringen Sie die Datei `main.py` und erforderliche Abhängigkeiten mit VS Code in einem Container unter, und pushen Sie sie in die angegebene Containerregistrierung. Das erstmalige Erstellen des Images kann einige Minuten dauern.

3. Sie können die vollständige Adresse des Containerimages mit Tag in dem Terminal abrufen, das in VS Code integriert ist. Weitere Informationen zur Definition für Erstellen und Pushen finden Sie in der Datei `module.json`.

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
    3. Geben Sie im Feld **Image-URI** die Imageadresse ein, z.B. `<your container registry address>/filtermodule:0.0.1-amd64`. Die vollständige Imageadresse finden Sie im vorherigen Abschnitt.
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
10. Klicken Sie auf **Weiter**.
11. Kopieren Sie im Schritt **Routen angeben** den folgenden JSON-Code in das Textfeld. Module veröffentlichen alle Nachrichten in der Edge-Runtime. Deklarative Regeln in der Runtime definieren, wohin die Nachrichten fließen. In diesem Tutorial benötigen Sie zwei Routen. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „input1“ zum Filtermodul. Dies ist der Endpunkt, für den Sie den **FilterMessages**-Handler konfiguriert haben. Die zweite Route transportiert Nachrichten vom Filtermodul an IoT Hub. In dieser Route ist `upstream` ein spezieller Empfänger, der Edge Hub anweist, Nachrichten an IoT Hub zu senden. 

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
