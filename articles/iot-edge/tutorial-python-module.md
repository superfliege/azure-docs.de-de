---
title: Tutorial für Python und Azure IoT Edge | Microsoft-Dokumentation
description: In diesem Tutorial wird gezeigt, wie Sie ein IoT Edge-Modul mit Python-Code erstellen und dieses Modul auf einem Edge-Gerät bereitstellen.
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5766f9708d2439f42f9ad77169fd1fe7f7dc451e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439111"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device"></a>Tutorial: Entwickeln und Bereitstellen eines Python-IoT Edge-Moduls für Ihr simuliertes Gerät

Mithilfe von Azure IoT Edge-Modulen können Sie Code bereitstellen, der Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert. In diesem Tutorial wird beschrieben, wie Sie ein IoT Edge-Modul, mit dem Sensordaten gefiltert werden, erstellen und bereitstellen. Sie verwenden das simulierte IoT Edge-Gerät, das Sie in den Schnellstartanleitungen zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-quickstart-win] oder [Linux][lnk-quickstart-lin] erstellt haben. In diesem Tutorial lernen Sie Folgendes:    

> [!div class="checklist"]
> * Erstellen eines IoT Edge-Python-Moduls mit Visual Studio Code
> * Erstellen und Veröffentlichen eines Docker-Images in Ihrer Registrierung unter Verwendung von Visual Studio Code und Docker
> * Bereitstellen des Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten


Das IoT Edge-Modul, das Sie in diesem Tutorial erstellen, filtert die von Ihrem Gerät generierten Temperaturdaten. Es leitet Nachrichten nur dann an nachgeschaltete Geräte weiter, wenn die Temperatur einen angegebenen Schwellenwert überschreitet. Diese Art der Analyse auf Edge-Ebene trägt zur Verringerung der Datenmenge bei, die an die Cloud übermittelt und dort gespeichert wird. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

* Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als Edge-Gerät verwenden, indem Sie die Schritte in der Schnellstartanleitung für [Linux](quickstart-linux.md) ausführen.
* Python-Module für IoT Edge unterstützen keine ARM-Prozessoren oder Windows-Geräte.

Cloudressourcen:

* Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) mit Standardtarif in Azure. 

Entwicklungsressourcen:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) für Visual Studio Code
* [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python) für Visual Studio Code 
* [Docker CE](https://docs.docker.com/engine/installation/). 
* [Python](https://www.python.org/downloads/).
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) zum Installieren von Python-Paketen (normalerweise in der Python-Installation enthalten).

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung
In diesem Tutorial verwenden Sie die Azure IoT Edge-Erweiterung für VSCode zum Entwickeln eines Moduls und zum Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.  

Sie können für dieses Tutorial jede beliebige Docker-kompatible Registrierung verwenden. Zwei beliebte, in der Cloud verfügbare Docker-Registrierungsdienste sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet. 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Azure Container Registry** aus.
2. Benennen Sie die Registrierung, wählen Sie ein Abonnement und eine Ressourcengruppe aus, und legen Sie die SKU auf **Basic** fest. 
3. Klicken Sie auf **Erstellen**.
4. Navigieren Sie nach der Erstellung der Containerregistrierung zu dieser Registrierung, und klicken Sie auf **Zugriffsschlüssel**. 
5. Legen Sie **Administratorbenutzer** auf **Aktivieren** fest.
6. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. Diese Werte werden im weiteren Verlauf des Tutorials benötigt. 

## <a name="create-an-iot-edge-module-project"></a>Erstellen eines IoT Edge-Modulprojekts
Die folgenden Schritte dienen zum Erstellen eines IoT Edge-Python-Moduls unter Verwendung von Visual Studio Code und der Azure IoT Edge-Erweiterung.

### <a name="create-a-new-solution"></a>Erstellen einer neuen Lösung

Verwenden Sie das Python-Paket **cookiecutter**, um eine Python-Lösungsvorlage zu erstellen, auf der Sie aufbauen können. 

1. Wählen Sie in Visual Studio Code die Optionen **Ansicht** > **Integriertes Terminal** aus, um das in Visual Studio Code integrierte Terminal zu öffnen.

2. Geben Sie im integrierten Terminal den folgenden Befehl ein, um **cookiecutter** zu installieren (oder zu aktualisieren). „cookiecutter“ wird zum Erstellen der IoT Edge-Lösungsvorlage in VS Code verwendet.

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Klicken Sie auf **Ansicht** > **Befehlspalette**, um die VS Code-Befehlspalette zu öffnen. 

4. Geben Sie in der Befehlspalette den Befehl **Azure: Sign in** ein, und führen Sie ihn aus. Befolgen Sie die Anweisungen für die Anmeldung bei Ihrem Azure-Konto. Falls Sie bereits angemeldet sind, können Sie diesen Schritt überspringen.

5. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge solution** ein, und führen Sie ihn aus. Geben Sie in der Befehlspalette die folgenden Informationen an, um die Projektmappe zu erstellen: 

   1. Wählen Sie den Ordner aus, in dem die Projektmappe erstellt werden soll. 
   2. Geben Sie einen Namen für Ihre Projektmappe ein, oder übernehmen Sie den Standardnamen **EdgeSolution**.
   3. Wählen Sie **Python-Modul** als Modulvorlage aus. 
   4. Nennen Sie das Modul **PythonModule**. 
   5. Geben Sie die Azure-Containerregistrierung an, die Sie im vorherigen Abschnitt als Imagerepository für das erste Modul erstellt haben. Ersetzen Sie **localhost:5000** durch den kopierten Wert für den Anmeldeserver. Die endgültige Zeichenfolge sieht wie folgt aus: \<Registrierungsname\>.azurecr.io/pythonmodule.
 
Im VS Code-Fenster wird der Arbeitsbereich Ihrer IoT Edge-Projektmappe geladen: der Modulordner, eine Vorlagendatei für das Bereitstellungsmanifest und eine Datei vom Typ „\.env“. 

### <a name="add-your-registry-credentials"></a>Hinzufügen von Registrierungsanmeldeinformationen

Die Umgebungsdatei speichert die Anmeldeinformationen für Ihr Containerrepository und gibt sie an die IoT Edge-Runtime weiter. Die Runtime benötigt diese Anmeldeinformationen, um Ihre privaten Images per Pull auf das IoT Edge-Gerät zu übertragen. 

1. Öffnen Sie im VS Code-Explorer die ENV-Datei. 
2. Aktualisieren Sie die Felder mit den Werten für **Benutzername** und **Kennwort**, die Sie aus der Azure-Containerregistrierung kopiert haben. 
3. Speichern Sie diese Datei. 

### <a name="update-the-module-with-custom-code"></a>Aktualisieren des Moduls mit benutzerdefiniertem Code

Jede Vorlage enthält Beispielcode, der simulierte Sensordaten aus dem Modul **tempSensor** an die IoT Hub-Instanz weiterleitet. Fügen Sie in diesem Abschnitt den Code hinzu, der **pythonModule** erweitert, um die Nachrichten vor dem Senden zu analysieren. 

1. Öffnen Sie im VS Code-Explorer **Module** > **PythonModule** > **main.py**.

2. Importieren Sie oben in der Datei **main.py** die Bibliothek **json**:

    ```python
    import json
    ```

3. Fügen Sie die Variablen **TEMPERATURE_THRESHOLD** und **TWIN_CALLBACKS** unter den globalen Leistungsindikatoren hinzu. Der Temperaturschwellenwert legt den Wert fest, den die gemessene Computertemperatur übersteigen muss, damit die Daten an die IoT Hub-Instanz gesendet werden.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

4. Ersetzen Sie die Funktion **receive_message_callback** durch folgenden Code:

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we forward this message to the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print ( "    Data: <<<%s>>> & Size=%d" % (message_text, size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature %s exceeds threshold %s" % (data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

5. Fügen Sie eine neue Funktion namens **module_twin_callback** hinzu. Diese Funktion wird aufgerufen, wenn die gewünschten Eigenschaften aktualisiert werden.

    ```python
    # module_twin_callback is invoked when the module twin's desired properties are updated.
    def module_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print ( "\nTwin callback called with:\nupdateStatus = %s\npayload = %s\ncontext = %s" % (update_state, payload, user_context) )
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print ( "Total calls confirmed: %d\n" % TWIN_CALLBACKS )
    ```

6. Fügen Sie der Methode **__init__** in der Klasse **HubManager** eine neue Methode hinzu, um die soeben hinzugefügte Funktion **module_twin_callback** zu initialisieren:

    ```python
    # Sets the callback when a module twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Speichern Sie diese Datei.

## <a name="build-your-iot-edge-solution"></a>Erstellen Ihrer IoT Edge-Projektmappe

Im vorherigen Abschnitt haben Sie eine IoT Edge-Projektmappe erstellt und **PythonModule** Code hinzugefügt, der Nachrichten herausfiltert, deren gemeldete Computertemperatur unter dem zulässigen Schwellenwert liegt. Nun müssen Sie die Projektmappe als Containerimage erstellen und per Push an die Containerregistrierung übertragen. 

1. Melden Sie sich bei Docker an. Geben Sie dazu im integrierten Terminal von Visual Studio Code den folgenden Befehl ein. Anschließend können Sie Ihr Modulimage an Ihre Azure-Containerregistrierung pushen: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Verwenden Sie den Benutzernamen, das Kennwort und den Anmeldeserver, die Sie im ersten Abschnitt aus Ihrer Azure-Containerregistrierung kopiert haben. Diese Werte finden Sie alternativ auch im Azure-Portal im Abschnitt **Zugriffsschlüssel** Ihrer Registrierung.

2. Öffnen Sie im VS Code-Explorer die Datei „deployment.template.json“ im Arbeitsbereich Ihrer IoT Edge-Projektmappe. 

   Diese Datei weist **$edgeAgent** an, zwei Module bereitzustellen: **tempSensor** (zum Simulieren von Gerätedaten) und **PythonModule**. Der Wert **PythonModule.image** wird auf eine Linux-amd64-Version des Images festgelegt. Weitere Informationen zu Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können – Vorschau](module-composition.md).

   Diese Datei enthält außerdem Ihre Anmeldeinformationen für die Registrierung. In der Vorlagendatei sind Ihr Benutzername und Ihr Kennwort als Platzhalter angegeben. Beim Erstellen des Bereitstellungsmanifests werden die Felder mit den Werten aktualisiert, die Sie der ENV-Datei hinzugefügt haben. 

3. Fügen Sie dem Bereitstellungsmanifest den Modulzwilling **PythonModule** hinzu. Fügen Sie am Ende des Abschnitts **moduleContent** nach dem Modulzwilling **$edgeHub** den folgenden JSON-Inhalt ein: 
    ```json
        "PythonModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Speichern Sie diese Datei.

5. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei „deployment.template.json“, und klicken Sie anschließend auf **Build IoT Edge solution** (IoT Edge-Projektmappe erstellen). 

Wenn Sie Visual Studio Code anweisen, die Projektmappe zu erstellen, wird zunächst basierend auf den Informationen in der Bereitstellungsvorlage eine Datei vom Typ „deployment.json“ in einem neuen Ordner namens **config** erstellt. Anschließend werden zwei Befehle im integrierten Terminal ausgeführt: `docker build` und `docker push`. Diese beiden Befehle erstellen Ihren Code, packen den Python-Code in Container und pushen den Code anschließend an die Containerregistrierung, die Sie beim Initialisieren der Projektmappe angegeben haben. 

Sie können die vollständige Adresse des Containerimages mit Tag im Befehl `docker build` anzeigen, der im integrierten VS Code-Terminal ausgeführt wird. Die Imageadresse wird auf der Grundlage der Informationen in der Datei „module.json“ erstellt und hat folgendes Format: \<Repository\>:\<Version\>-\<Platform\>. In diesem Tutorial sollte sie wie folgt aussehen: registryname.azurecr.io/pythonmodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>Bereitstellen und Ausführen der Projektmappe

Sie können das Azure-Portal verwenden, um Ihr Python-Modul wie in den Schnellstartanleitungen auf einem IoT Edge-Gerät bereitzustellen. Sie können Module aber auch über Visual Studio Code bereitstellen und überwachen. In den folgenden Abschnitten wird die Azure IoT Edge-Erweiterung für VS Code verwendet, die als erforderliche Komponente aufgeführt ist. Sofern Sie die Erweiterung noch nicht installiert haben, holen Sie dies jetzt nach. 

1. Öffnen Sie die VS Code-Befehlspalette, indem Sie auf **Ansicht** > **Befehlspalette** klicken.

2. Suchen Sie den Befehl **Azure: Sign in** (Azure: Anmelden), und führen Sie ihn aus. Befolgen Sie die Anweisungen zum Anmelden bei Ihrem Azure-Konto. 

3. Suchen Sie in der Befehlspalette nach dem Befehl **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: IoT Hub auswählen), und führen Sie den Befehl aus. 

4. Wählen Sie das Abonnement mit Ihrer IoT Hub-Instanz und anschließend die gewünschte IoT Hub-Instanz aus.

5. Erweitern Sie im VS Code-Explorer den Abschnitt **Azure IoT Hub-Geräte**. 

6. Klicken Sie mit der rechten Maustaste auf den Namen Ihres IoT Edge-Geräts, und klicken Sie anschließend auf **Create Deployment for IoT Edge device** (Bereitstellung für IoT Edge-Gerät erstellen). 

7. Navigieren Sie zu dem Projektmappenordner, der **PythonModule** enthält. Öffnen Sie den Konfigurationsordner, wählen Sie die Datei „deployment.json“ aus, und klicken Sie anschließend auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen).

8. Aktualisieren Sie den Abschnitt **Azure IoT Hub-Geräte**. Nun sollte das neue **PythonModule**-Modul zusammen mit dem **TempSensor**-Modul sowie **$edgeAgent** und **$edgeHub** ausgeführt werden. 

## <a name="view-generated-data"></a>Anzeigen generierter Daten

1. Klicken Sie zum Überwachen von Daten, die bei der IoT Hub-Instanz eingehen, auf die Auslassungspunkte (**...**) und anschließend auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).
2. Wenn Sie die D2C-Nachrichten für ein bestimmtes Gerät überwachen möchten, klicken Sie mit der rechten Maustaste auf das Gerät in der Liste, und klicken Sie dann auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).
3. Wenn Sie die Datenüberwachung beenden möchten, führen Sie in der Befehlspalette den Befehl **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: Überwachung von D2C-Nachrichten beenden) aus. 
4. Klicken Sie zum Anzeigen oder Aktualisieren des Modulzwillings mit der rechten Maustaste auf das Modul in der Liste, und klicken Sie auf **Edit module twin** (Modulzwilling bearbeiten). Speichern Sie zum Aktualisieren des Modulzwillings die JSON-Datei des Zwillings, klicken Sie mit der rechten Maustaste auf den Editorbereich, und klicken Sie anschließend auf **Update Module Twin** (Modulzwilling aktualisieren).
5. Installieren Sie zum Anzeigen von Docker-Protokollen [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) für VS Code. Ihre ausgeführten Module finden Sie lokal im Docker-Explorer. Klicken Sie zum Anzeigen im integrierten Terminal im Kontextmenü auf **Protokolle anzeigen**. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden.

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 

> [!IMPORTANT]
> Das Löschen von Azure-Ressourcen und -Ressourcengruppen kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. Wenn Sie die IoT Hub-Ressource in einer bereits vorhandenen Ressourcengruppe erstellt haben, die Ressourcen enthält, die Sie behalten möchten, löschen Sie nicht die Ressourcengruppe, sondern nur die IoT Hub-Ressource.
>

Wenn Sie nur die IoT Hub-Instanz löschen möchten, führen Sie den folgenden Befehl mit dem Namen des Hubs und dem Namen der Ressourcengruppe aus:

```azurecli-interactive
az iot hub delete --name {hub_name} --resource-group IoTEdgeResources
```


Löschen Sie die gesamte Ressourcengruppe wie folgt anhand des Namens:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.

2. Geben Sie im Textfeld **Nach Name filtern** den Namen der Ressourcengruppe ein, die Ihre IoT Hub-Instanz enthält. 

3. Klicken Sie in der Ergebnisliste rechts neben Ihrer Ressourcengruppe auf die Auslassungspunkte (**...**) und anschließend auf **Ressourcengruppe löschen**.

4. Sie werden aufgefordert, das Löschen der Ressourcengruppe zu bestätigen. Geben Sie zur Bestätigung erneut den Namen Ihrer Ressourcengruppe ein, und klicken Sie auf **Löschen**. Daraufhin werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul mit Code zum Filtern von Rohdaten erstellt, die von Ihrem IoT Edge-Gerät generiert werden. In den nächsten Tutorials erfahren Sie, wie Ihnen Azure IoT Edge noch dabei helfen kann, Daten in geschäftliche Erkenntnisse auf Edge-Ebene zu verwandeln.

> [!div class="nextstepaction"]
> [Bereitstellen von Azure Functions als IoT Edge-Module (Vorschau)](tutorial-deploy-function.md)
> [Bereitstellen von Azure Stream Analytics als IoT Edge-Modul (Vorschauversion)](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
