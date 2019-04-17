---
title: 'Tutorial: Erstellen eines benutzerdefinierten Node.js-Moduls – Azure IoT Edge | Microsoft-Dokumentation'
description: In diesem Tutorial wird gezeigt, wie Sie ein IoT Edge-Modul mit Node.js-Code erstellen und dieses Modul auf einem Edge-Gerät bereitstellen.
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 3b79c75b9846a4f8966a113c6e06fabc25bcf011
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2019
ms.locfileid: "59470949"
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

* Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als Edge-Gerät verwenden, indem Sie die Schritte in der Schnellstartanleitung für [Linux](quickstart-linux.md) ausführen.
* Node.js-Module für IoT Edge unterstützen keine Windows-Container. 

Cloudressourcen:

* Eine [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-Instanz in Azure im Tarif „Free“ oder „Standard“. 

Entwicklungsressourcen:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) für Visual Studio Code. 
* [Docker CE](https://docs.docker.com/engine/installation/). 
   * Stellen Sie bei der Entwicklung auf einem Windows-Gerät sicher, dass Docker [zur Verwendung von Linux-Containern konfiguriert ist](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). 
* [Node.js und npm](https://nodejs.org). Das npm-Paket wird mit Node.js verteilt. Das bedeutet, dass npm beim Download von Node.js automatisch auf Ihrem Computer installiert wird.

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

In diesem Tutorial verwenden Sie die Azure IoT-Tools für Visual Studio Code zum Entwickeln eines Moduls und Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.  

Sie können eine beliebige Docker-kompatible Registrierung zum Speichern Ihrer Containerimages verwenden. Zwei beliebte Docker-Registrierungsdienste sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet. 

Sollten Sie noch keine Containerregistrierung besitzen, führen Sie die folgenden Schritte aus, um eine neue Containerregistrierung in Azure zu erstellen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Container Registry** aus.

2. Geben Sie die folgenden Werte an, um Ihre Containerregistrierung zu erstellen:

   | Feld | Wert |
   | ----- | ----- |
   | Registrierungsname | Geben Sie einen eindeutigen Namen an. |
   | Abonnement | Wählen Sie ein Abonnement aus der Dropdownliste aus. |
   | Ressourcengruppe | Es wird empfohlen, die gleiche Ressourcengruppe für alle Testressourcen zu verwenden, die Sie während der IoT Edge-Schnellstarts und -Tutorials erstellen. Beispielsweise **IoTEdgeResources**. |
   | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. |
   | Administratorbenutzer | Legen Sie **Aktivieren** fest. |
   | SKU | Wählen Sie **Basic** aus. |

5. Klicken Sie auf **Erstellen**.

6. Navigieren Sie nach der Erstellung der Containerregistrierung zu dieser Registrierung, und klicken Sie anschließend auf **Zugriffsschlüssel**. 

7. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. Sie verwenden diese Werte an späterer Stelle des Tutorials, um Zugriff auf die Containerregistrierung zu gewähren. 

## <a name="create-an-iot-edge-module-project"></a>Erstellen eines IoT Edge-Modulprojekts
Die folgenden Schritte zeigen, wie Sie mithilfe von Visual Studio Code und der Azure IoT-Tools ein IoT Edge-Node.js-Modul erstellen.

### <a name="create-a-new-solution"></a>Erstellen einer neuen Lösung

Verwenden Sie **npm** zum Erstellen einer Node.js-Lösungsvorlage, auf die Sie aufbauen können. 

1. Wählen Sie in Visual Studio Code die Optionen **Ansicht** > **Integriertes Terminal** aus, um das in Visual Studio Code integrierte Terminal zu öffnen.

2. Geben Sie im integrierten Terminal den folgenden Befehl ein, um **yeoman** und den Generator für das Node.js-Azure IoT Edge-Modul zu installieren: 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. Klicken Sie auf **Ansicht** > **Befehlspalette**, um die VS Code-Befehlspalette zu öffnen. 

3. Geben Sie in der Befehlspalette den Befehl **Azure: Sign in** ein, und führen Sie ihn aus, und befolgen Sie die Anweisungen zum Anmelden bei Ihrem Azure-Konto. Wenn Sie bereits angemeldet sind, können Sie diesen Schritt überspringen.

4. Geben Sie in der Befehlspalette den Befehl **Azure IoT Edge ein: New IoT Edge Solution** ein, und führen Sie ihn aus. Folgen Sie den Anweisungen in der Befehlspalette, um Ihre Projektmappe zu erstellen.

   | Feld | Wert |
   | ----- | ----- |
   | Ordner auswählen | Wählen Sie den Speicherort auf Ihrem Entwicklungscomputer aus, an dem Visual Studio Code die Projektmappendateien erstellen soll. |
   | Provide a solution name (Projektmappennamen angeben) | Geben Sie für Ihre Projektmappe einen aussagekräftigen Namen ein, oder übernehmen Sie den Standardnamen **EdgeSolution**. |
   | Select module template (Modulvorlage auswählen) | Wählen Sie **Node.js-Modul** aus. |
   | Provide a module name (Modulname angeben) | Nennen Sie das Modul **NodeModule**. |
   | Provide Docker image repository for the module (Docker-Imagerepository für das Modul angeben) | Ein Imagerepository enthält den Namen Ihrer Containerregistrierung und den Namen Ihres Containerimages. Für das Containerimage wird der von Ihnen im vorherigen Schritt angegebene Name übernommen. Ersetzen Sie **localhost:5000** durch den Anmeldeserverwert aus Ihrer Azure-Containerregistrierung. Den Anmeldeserver können Sie im Azure-Portal auf der Übersichtsseite Ihrer Containerregistrierung ermitteln. <br><br>Das endgültige Imagerepository sieht wie folgt aus: \<Registrierungsname\>.azurecr.io/nodemodule. |
 
   ![Bereitstellen eines Docker-Imagerepositorys](./media/tutorial-node-module/repository.png)

Das VS Code-Fenster lädt den Arbeitsbereich für Ihre IoT Edge-Projektmappe. Der Arbeitsbereich der Projektmappe enthält fünf Komponenten auf oberster Ebene. Der Ordner **Module** enthält den Node.js-Code für Ihre Module sowie Dockerfiles für die Erstellung Ihres Moduls als Containerimage. In der Datei **\.env** werden Ihre Anmeldeinformationen für die Containerregistrierung gespeichert. Die Datei **deployment.template.json** enthält die Informationen, die die IoT Edge-Runtime zum Bereitstellen von Modulen auf einem Gerät verwendet, und die Datei **deployment.debug.template.json** enthält die Debugversion von Modulen. In diesem Tutorial wird nicht der Ordner **\.vscode** oder die Datei **\.gitignore** bearbeitet. 

Wenn Sie beim Erstellen Ihrer Lösung keine Containerregistrierung angegeben, aber den Standardwert „localhost:5000“ übernommen haben, besitzen Sie keine Datei vom Typ „\.env“. 

<!--
   ![Node.js solution workspace](./media/tutorial-node-module/workspace.png)
-->

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

9. Speichern Sie die Datei „app.js“.

10. Öffnen Sie im VS Code-Explorer die Datei **deployment.template.json** im Arbeitsbereich für Ihre IoT Edge-Projektmappe. Über diese Datei erhält der IoT Edge-Agent die Informationen dazu, welche Module bereitgestellt werden sollen (hier: **tempSensor** und **NodeModule**). Außerdem wird der IoT Edge-Hub angewiesen, wie die Nachrichten weitergeleitet werden sollen. Die Visual Studio Code-Erweiterung fügt zwar automatisch die meisten Informationen ein, die Sie für die Bereitstellungsvorlage benötigen, aber Sie sollten überprüfen, ob alle Angaben für Ihre Lösung stimmen: 

   1. Die Standardplattform Ihrer IoT Edge-Instanz ist auf Ihrer VS Code-Statusleiste auf **amd64** festgelegt. Das bedeutet, dass **NodeModule** auf die Linux-amd64-Version des Images festgelegt ist. Ändern Sie die Standardplattform auf der Statusleiste von **amd64** in **arm32v7**, sofern es sich dabei um die Architektur Ihres IoT Edge-Geräts handelt. 

      ![Aktualisieren der Modulimageplattform](./media/tutorial-node-module/image-platform.png)

   2. Stellen Sie sicher, dass die Vorlage den richtigen Modulnamen und nicht den Standardnamen **SampleModule** besitzt, den Sie beim Erstellen der IoT Edge-Lösung geändert haben.

   3. Im Abschnitt **registryCredentials** werden Ihre Anmeldeinformationen für die Docker-Registrierung gespeichert, damit der IoT Edge-Agent Ihr Modulimage per Pullvorgang abrufen kann. Die tatsächlichen Benutzername-/Kennwortpaare werden in der ENV-Datei gespeichert. Diese Datei wird von Git ignoriert. Fügen Sie Ihre Anmeldeinformationen der ENV-Datei hinzu, falls Sie dies noch nicht durchgeführt haben.  

   4. Weitere Informationen zu Bereitstellungsmanifesten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen in IoT Edge](module-composition.md).

11. Fügen Sie den Modulzwilling „NodeModule“ zum Bereitstellungsmanifest hinzu. Fügen Sie am Ende des Abschnitts `moduleContent` nach dem Modulzwilling `$edgeHub` den folgenden JSON-Inhalt ein: 

   ```json
     "NodeModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Hinzufügen des Modulzwillings zur Bereitstellungsvorlage](./media/tutorial-node-module/module-twin.png)

12. Speichern Sie die Datei „deployment.template.json“.


## <a name="build-your-iot-edge-solution"></a>Erstellen Ihrer IoT Edge-Projektmappe

Im vorherigen Abschnitt haben Sie eine IoT Edge-Projektmappe erstellt und NodeModule Code hinzugefügt, der Nachrichten herausfiltert, deren gemeldete Computertemperatur unter dem zulässigen Schwellenwert liegt. Nun müssen Sie die Projektmappe als Containerimage erstellen und per Push an die Containerregistrierung übertragen. 

1. Melden Sie sich durch Eingabe des folgenden Befehls im integrierten Visual Studio Code-Terminal bei Docker an, um das Modulimage per Push an ACR zu übertragen: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Verwenden Sie den Benutzernamen, das Kennwort und den Anmeldeserver, die Sie im ersten Abschnitt aus Azure Container Registry kopiert haben. Oder rufen Sie sie erneut über den Abschnitt **Zugriffsschlüssel** der Registrierung im Azure-Portal ab.

2. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **deployment.template.json**, und klicken Sie anschließend auf **Build and Push IoT Edge solution** (IoT Edge-Projektmappe erstellen und übertragen). 

Wenn Sie Visual Studio Code anweisen, die Projektmappe zu erstellen, wird zuerst basierend auf den Informationen in der Bereitstellungsvorlage eine `deployment.json`-Datei in einem neuen Konfigurationsordner (**config**) generiert. Anschließend werden zwei Befehle im integrierten Terminal ausgeführt: `docker build` und `docker push`. Diese beiden Befehle erstellen den Code, packen Ihren Node.js-Code in Container und führen dann eine Pushübertragung an die Containerregistrierung durch, die Sie beim Initialisieren der Projektmappe angegeben haben. 

Sie können die vollständige Adresse des Containerimages mit Tag im Befehl `docker build` anzeigen, der im integrierten VS Code-Terminal ausgeführt wird. Die Imageadresse wird auf der Grundlage der Informationen in der Datei `module.json` erstellt und hat folgendes Format: **\<Repository\>:\<Version\>-\<Plattform\>**. In diesem Tutorial sollte sie wie folgt aussehen: **registryname.azurecr.io/nodemodule:0.0.1-amd64**.

>[!TIP]
>Wenn beim Versuch, Ihr Modul zu erstellen und zu pushen ein Fehler auftritt, führen Sie die folgenden Überprüfungen durch:
>* Haben Sie sich mit den Anmeldeinformationen aus Ihrer Containerregistrierung bei Docker in Visual Studio Code angemeldet? Dabei handelt es sich um andere Anmeldeinformationen, als Sie zur Anmeldung beim Azure-Portal verwenden.
>* Ist Ihr Containerrepository richtig? Öffnen Sie **modules** > **nodemodule** > **module.json**, und suchen Sie nach dem Feld **Repository**. Das Imagerepository sollte wie folgt aussehen: **\<Registrierungsname\>.azurecr.io/nodemodule**. 
>* Erstellen Sie die gleiche Art von Containern, die von Ihrem Entwicklungscomputer ausgeführt wird? Visual Studio Code ist standardmäßig auf Container vom Typ „Linux amd64“ festgelegt. Wenn auf Ihrem Entwicklungscomputer Container vom Typ „Linux arm32v7“ ausgeführt werden, aktualisieren Sie die Plattform auf der blauen Statusleiste am unteren Rand des VS Code-Fensters so, dass sie Ihrer Containerplattform entspricht.
>* Node.js-Module für IoT Edge unterstützen keine Windows-Container.

## <a name="deploy-and-run-the-solution"></a>Bereitstellen und Ausführen der Projektmappe

Im Schnellstartartikel, anhand dessen Sie Ihr IoT Edge-Gerät eingerichtet haben, haben Sie ein Modul über das Azure-Portal bereitgestellt. Sie können Module auch mithilfe der Azure IoT Hub-Toolkit-Erweiterung (bisher als Azure IoT-Toolkit-Erweiterung bekannt) für Visual Studio Code bereitstellen. Sie haben bereits ein Bereitstellungsmanifest für Ihr Szenario vorbereitet: die Datei **deployment.json**. Nun müssen Sie nur noch ein Gerät auswählen, um die Bereitstellung zu empfangen.

1. Führen Sie in der Befehlspalette von VS Code den Befehl **Azure IoT Hub: Select IoT Hub** aus. 

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
3. Wenn Sie die Datenüberwachung beenden möchten, führen Sie in der Befehlspalette den Befehl **Azure IoT Hub: Stop monitoring D2C message** aus. 
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

