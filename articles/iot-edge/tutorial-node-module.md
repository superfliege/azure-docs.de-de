---
title: Tutorial für Node.js und Azure IoT Edge | Microsoft-Dokumentation
description: In diesem Tutorial wird gezeigt, wie Sie ein IoT Edge-Modul mit Node.js-Code erstellen und dieses Modul auf einem Edge-Gerät bereitstellen.
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 40fa0357245ad77fbdb08c5dbb4839d69322954f
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566791"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-to-your-simulated-device"></a>Tutorial: Entwickeln und Bereitstellen eines Node.js-IoT Edge-Moduls für Ihr simuliertes Gerät

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. In diesem Tutorial wird beschrieben, wie Sie ein IoT Edge-Modul, mit dem Sensordaten gefiltert werden, erstellen und bereitstellen. Sie verwenden das simulierte IoT Edge-Gerät, das Sie in den Schnellstartanleitungen erstellt haben. In diesem Tutorial lernen Sie Folgendes:    

> [!div class="checklist"]
> * Erstellen eines IoT Edge-Node.js-Moduls mit Visual Studio Code
> * Erstellen und Veröffentlichen eines Docker-Image in Ihrer Registrierung mithilfe von Visual Studio Code und Docker 
> * Bereitstellen des Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten


Das IoT Edge-Modul, das Sie in diesem Tutorial erstellen, filtert die von Ihrem Gerät generierten Temperaturdaten. Es leitet Nachrichten nur dann an nachgeschaltete Geräte weiter, wenn die Temperatur einen angegebenen Schwellenwert überschreitet. Diese Art der Analyse auf Edge-Ebene eignet sich zur Verringerung der Datenmenge, die in die Cloud weitergeleitet und dort gespeichert wird. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

* Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als Edge-Gerät verwenden, indem Sie die Schritte ausführen, die in der Schnellstartanleitung für [Linux](quickstart-linux.md)- oder [Windows](quickstart.md)-Geräte beschrieben sind.

Cloudressourcen:

* Eine [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-Instanz in Azure im Tarif „Free“ oder „Standard“. 

Entwicklungsressourcen:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) für Visual Studio Code 
* [Docker CE](https://docs.docker.com/engine/installation/). 
* [Node.js und npm](https://nodejs.org). Das npm-Paket wird mit Node.js verteilt. Das bedeutet, dass npm beim Download von Node.js automatisch auf Ihrem Computer installiert wird.

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
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT Edge-Erweiterung ein IoT Edge-Node.js-Modul erstellen.

### <a name="create-a-new-solution"></a>Erstellen einer neuen Lösung

Verwenden Sie **npm** zum Erstellen einer Node.js-Lösungsvorlage, auf die Sie aufbauen können. 

1. Wählen Sie in Visual Studio Code die Optionen **Ansicht** > **Integriertes Terminal** aus, um das in Visual Studio Code integrierte Terminal zu öffnen.

2. Geben Sie im integrierten Terminal den folgenden Befehl ein, um **yeoman** und den Generator für das Node.js-Azure IoT Edge-Modul zu installieren: 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. Klicken Sie auf **Ansicht** > **Befehlspalette**, um die VS Code-Befehlspalette zu öffnen. 

3. Geben Sie in der Befehlspalette den Befehl **Azure: Sign in** (Azure: Anmelden) ein, und führen Sie ihn aus. Befolgen Sie die Anweisungen zum Anmelden beim Azure-Konto. Wenn Sie bereits angemeldet sind, können Sie diesen Schritt überspringen.

4. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: Neue IoT Edge-Projektmappe) ein, und führen Sie ihn aus. Geben Sie in der Befehlspalette die folgenden Informationen an, um die Projektmappe zu erstellen: 

   1. Wählen Sie den Ordner aus, in dem die Projektmappe erstellt werden soll. 
   2. Geben Sie einen Namen für Ihre Projektmappe ein, oder übernehmen Sie den Standardnamen **EdgeSolution**.
   3. Wählen Sie **Node.js-Modul** als Modulvorlage aus. 
   4. Nennen Sie das Modul **NodeModule**. 
   5. Geben Sie die Azure Container Registry-Instanz an, die Sie im vorherigen Abschnitt als Imagerepository für das erste Modul erstellt haben. Ersetzen Sie **localhost:5000** durch den kopierten Wert für den Anmeldeserver. Die endgültige Zeichenfolge sieht wie folgt aus: **\<Registrierungsname\>.azurecr.io/nodemodule**.

   ![Bereitstellen eines Docker-Imagerepositorys](./media/tutorial-node-module/repository.png)

Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Der Arbeitsbereich der Projektmappe enthält fünf Komponenten auf oberster Ebene. In diesem Tutorial wird nicht der Ordner **\.vscode** oder die Datei **\.gitignore** bearbeitet. Der Ordner **Module** enthält den Node.js-Code für Ihre Module sowie Dockerfiles für die Erstellung Ihres Moduls als Containerimage. In der Datei **\.env** werden Ihre Anmeldeinformationen für die Containerregistrierung gespeichert. Die Datei **deployment.template.json** enthält die Informationen, die die IoT Edge-Runtime zum Bereitstellen von Modulen auf einem Gerät verwendet. 

Wenn Sie beim Erstellen Ihrer Lösung keine Containerregistrierung angegeben, aber den Standardwert „localhost:5000“ übernommen haben, besitzen Sie keine Datei vom Typ „\.env“. 

   ![Node.js-Lösungsarbeitsbereich](./media/tutorial-node-module/workspace.png)

### <a name="add-your-registry-credentials"></a>Hinzufügen von Registrierungsanmeldeinformationen

Die Umgebungsdatei speichert die Anmeldeinformationen für Ihr Containerrepository und gibt sie an die IoT Edge-Runtime weiter. Die Runtime benötigt diese Anmeldeinformationen, um Ihre privaten Images per Pull auf das IoT Edge-Gerät zu übertragen. 

1. Öffnen Sie im VS Code-Explorer die Datei vom Typ **.env**. 
2. Aktualisieren Sie die Felder mit den Werten für **Benutzername** und **Kennwort**, die Sie aus der Azure-Containerregistrierung kopiert haben. 
3. Speichern Sie diese Datei. 

### <a name="update-the-module-with-custom-code"></a>Aktualisieren des Moduls mit benutzerdefiniertem Code

Jede Vorlage enthält Beispielcode, der simulierte Sensordaten aus dem Modul **tempSensor** abruft und an IoT Hub weiterleitet. In diesem Abschnitt fügen Sie Code hinzu, damit NodeModule die Nachrichten vor dem Senden analysiert. 

1. Öffnen Sie im VS Code-Explorer **Module** > **NodeModule** > **app.js**.

5. Fügen Sie unterhalb der erforderlichen Knotenmodule eine Variable für einen Temperaturschwellenwert hinzu. Der Temperaturschwellenwert legt den Wert fest, den die gemessene Temperatur übersteigen muss, damit die Daten an IoT Hub gesendet werden.

    ```javascript
    var temperatureThreshold = 25;
    ```

6. Ersetzen Sie die gesamte Funktion `PipeMessage` durch die Funktion `FilterMessage`.
    
    ```javascript
    // This function filters out messages that report temperatures below the temperature threshold.
    // It also adds the MessageType property to the message with the value set to Alert.
    function filterMessage(client, inputName, msg) {
        client.complete(msg, printResultFor('Receiving message'));
        if (inputName === 'input1') {
            var message = msg.getBytes().toString('utf8');
            var messageBody = JSON.parse(message);
            if (messageBody && messageBody.machine && messageBody.machine.temperature && messageBody.machine.temperature > temperatureThreshold) {
                console.log(`Machine temperature ${messageBody.machine.temperature} exceeds threshold ${temperatureThreshold}`);
                var outputMsg = new Message(message);
                outputMsg.properties.add('MessageType', 'Alert');
                client.sendOutputEvent('output1', outputMsg, printResultFor('Sending received message'));
            }
        }
    }

    ```

7. Ersetzen Sie in der Funktion `client.on()` den Funktionsnamen `pipeMessage` durch `filterMessage`.

    ```javascript
    client.on('inputMessage', function (inputName, msg) {
        filterMessage(client, inputName, msg);
        });
    ```

8. Kopieren Sie den folgenden Codeausschnitt in den Funktionsrückruf `client.open()` (nach `client.on()` in der `else`-Anweisung). Diese Funktion wird aufgerufen, wenn die gewünschten Eigenschaften aktualisiert werden.

    ```javascript
    client.getTwin(function (err, twin) {
        if (err) {
            console.error('Error getting twin: ' + err.message);
        } else {
            twin.on('properties.desired', function(delta) {
                if (delta.TemperatureThreshold) {
                    temperatureThreshold = delta.TemperatureThreshold;
                }
            });
        }
    });
    ```

9. Speichern Sie diese Datei.

## <a name="build-your-iot-edge-solution"></a>Erstellen Ihrer IoT Edge-Projektmappe

Im vorherigen Abschnitt haben Sie eine IoT Edge-Projektmappe erstellt und NodeModule Code hinzugefügt, der Nachrichten herausfiltert, deren gemeldete Computertemperatur unter dem zulässigen Schwellenwert liegt. Nun müssen Sie die Projektmappe als Containerimage erstellen und per Push an die Containerregistrierung übertragen. 

1. Melden Sie sich durch Eingabe des folgenden Befehls im integrierten Visual Studio Code-Terminal bei Docker an, um das Modulimage per Push an ACR zu übertragen: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Verwenden Sie den Benutzernamen, das Kennwort und den Anmeldeserver, die Sie im ersten Abschnitt aus Azure Container Registry kopiert haben. Oder rufen Sie sie erneut über den Abschnitt **Zugriffsschlüssel** der Registrierung im Azure-Portal ab.

2. Öffnen Sie im VS Code-Explorer die Datei **deployment.template.json** im Arbeitsbereich für Ihre IoT Edge-Projektmappe. 

   Diese Datei weist `$edgeAgent` an, zwei Module bereitzustellen: das Modul **tempSensor**, das Gerätedaten simuliert, und das Modul **NodeModule**. Der Wert `NodeModule.image` wird auf eine Linux-amd64-Version des Images festgelegt. Weitere Informationen zu Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können – Vorschau](module-composition.md).

   Diese Datei enthält außerdem Ihre Anmeldeinformationen für die Registrierung. In der Vorlagendatei sind Ihr Benutzername und Ihr Kennwort als Platzhalter angegeben. Beim Erstellen des Bereitstellungsmanifests werden die Felder mit den Werten aktualisiert, die Sie zu **.env** hinzugefügt haben. 

4. Fügen Sie den Modulzwilling „NodeModule“ zum Bereitstellungsmanifest hinzu. Fügen Sie am Ende des Abschnitts `moduleContent` nach dem Modulzwilling `$edgeHub` den folgenden JSON-Inhalt ein: 
    ```json
        "NodeModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```
5. Speichern Sie diese Datei.
6. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **deployment.template.json**, und klicken Sie anschließend auf **Build and Push IoT Edge solution** (IoT Edge-Projektmappe erstellen und übertragen). 

Wenn Sie Visual Studio Code anweisen, die Projektmappe zu erstellen, wird zuerst basierend auf den Informationen in der Bereitstellungsvorlage eine `deployment.json`-Datei in einem neuen Konfigurationsordner (**config**) generiert. Anschließend werden zwei Befehle im integrierten Terminal ausgeführt: `docker build` und `docker push`. Diese beiden Befehle erstellen den Code, packen den Node.js-Code in Container und führen dann eine Pushübertragung an die Containerregistrierung durch, die Sie beim Initialisieren der Projektmappe angegeben haben. 

Sie können die vollständige Adresse des Containerimages mit Tag im Befehl `docker build` anzeigen, der im integrierten VS Code-Terminal ausgeführt wird. Die Imageadresse wird auf der Grundlage der Informationen in der Datei `module.json` erstellt und hat folgendes Format: **\<Repository\>:\<Version\>-\<Plattform\>**. In diesem Tutorial sollte sie wie folgt aussehen: **registryname.azurecr.io/nodemodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Bereitstellen und Ausführen der Projektmappe

Im Schnellstartartikel, anhand dessen Sie Ihr IoT Edge-Gerät eingerichtet haben, haben Sie ein Modul über das Azure-Portal bereitgestellt. Sie können Module auch mithilfe der Azure IoT-Toolkit-Erweiterung für Visual Studio Code bereitstellen. Sie haben bereits ein Bereitstellungsmanifest für Ihr Szenario vorbereitet: die Datei **deployment.json**. Nun müssen Sie nur noch ein Gerät auswählen, um die Bereitstellung zu empfangen.

1. Führen Sie in der Befehlspalette von VS Code den Befehl **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: IoT Hub auswählen) aus. 

2. Wählen Sie das Abonnement und den IoT-Hub aus, die das zu konfigurierende IoT Edge-Gerät enthalten. 

3. Erweitern Sie im VS Code-Explorer den Abschnitt **Azure IoT Hub-Geräte**. 

4. Klicken Sie mit der rechten Maustaste auf den Namen Ihres IoT Edge-Geräts, und klicken Sie dann auf **Create Deployment for Single Device** (Bereitstellung für einzelnes Gerät erstellen). 

   ![Erstellen einer Bereitstellung für ein einzelnes Gerät](./media/tutorial-node-module/create-deployment.png)

5. Wählen Sie im Konfigurationsordner (**config**) die Datei **deployment.json** aus, und klicken Sie auf **Select Edge Deployment Manifest** (Edge-Bereitstellungsmanifest auswählen). Verwenden Sie nicht die Datei „deployment.template.json“. 

6. Klicken Sie auf die Schaltfläche „Aktualisieren“. Nun sollte das neue **NodeModule**-Modul zusammen mit dem **TempSensor**-Modul sowie **$edgeAgent** und **$edgeHub** ausgeführt werden. 


## <a name="view-generated-data"></a>Anzeigen generierter Daten

Sobald Sie das Bereitstellungsmanifest auf Ihr IoT Edge-Gerät angewendet haben, erfasst die IoT Edge-Runtime auf dem Gerät die neuen Bereitstellungsinformationen und verwendet sie bei der Ausführung. Alle auf dem Gerät ausgeführten Module, die nicht im Bereitstellungsmanifest enthalten sind, werden beendet. Module, die auf dem Gerät fehlen, werden gestartet. 

Der Status Ihres IoT Edge-Geräts wird im Abschnitt **Azure IoT Hub Devices** (Azure IoT Hub-Geräte) des Visual Studio Code-Explorers angezeigt. Erweitern Sie die Details Ihres Geräts, um eine Liste mit den bereitgestellten und ausgeführten Modulen anzuzeigen. 

Auf dem IoT Edge-Gerät selbst können Sie den Status Ihrer Bereitstellungsmodule mithilfe des Befehls `iotedge list` anzeigen. Es sollten vier Module angezeigt werden: die beiden IoT Edge-Runtime-Module, „tempSensor“ und das benutzerdefinierte Modul, das Sie in diesem Tutorial erstellt haben. Es kann einige Minuten dauern, bis alle Module gestartet wurden. Führen Sie den Befehl daher erneut aus, falls nicht gleich alle Module angezeigt werden. 

Mit dem Befehl `iotedge logs <module name>` können Sie die Nachrichten anzeigen, die von einem Modul generiert werden. 

Mithilfe von Visual Studio Code können Sie die bei Ihrem IoT-Hub eingehenden Nachrichten anzeigen. 

1. Klicken Sie zum Überwachen der bei IoT Hub eingehenden Daten auf **...** und dann auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).
2. Wenn Sie die D2C-Nachrichten für ein bestimmtes Gerät überwachen möchten, klicken Sie mit der rechten Maustaste auf das Gerät in der Liste, und klicken Sie dann auf **Start Monitoring D2C Messages** (Überwachung von D2C-Nachrichten starten).
3. Wenn Sie die Datenüberwachung beenden möchten, führen Sie in der Befehlspalette den Befehl **Azure IoT Hub: Stop monitoring D2C message** (Azure IoT Hub: Überwachung von D2C-Nachrichten beenden) aus. 
4. Klicken Sie zum Anzeigen oder Aktualisieren des Modulzwillings mit der rechten Maustaste auf das Modul in der Liste, und klicken Sie auf **Edit module twin** (Modulzwilling bearbeiten). Speichern Sie zum Aktualisieren des Modulzwillings die JSON-Datei des Zwillings, klicken Sie mit der rechten Maustaste auf den Editorbereich, und wählen Sie **Update Module Twin** (Modulzwilling aktualisieren).
5. Zum Anzeigen von Docker-Protokollen können Sie [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) für VS Code installieren und Ihre ausgeführten Module lokal im Docker-Explorer suchen. Klicken Sie zum Anzeigen im integrierten Terminal im Kontextmenü auf **Protokolle anzeigen**. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden. Sie können auch dasselbe IoT Edge-Gerät als Testgerät weiter nutzen. 

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein IoT Edge-Modul erstellt, das Code zum Filtern von Rohdaten enthält, die von Ihrem IoT Edge-Gerät generiert wurden. Sie können mit einem der nachfolgenden Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Bereitstellen einer Azure-Funktion als Modul](tutorial-deploy-function.md)
> [Bereitstellen von Azure Stream Analytics als Modul](tutorial-deploy-stream-analytics.md)

