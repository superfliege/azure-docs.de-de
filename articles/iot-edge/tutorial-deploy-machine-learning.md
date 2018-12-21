---
title: 'Tutorial: Bereitstellen von Azure Machine Learning für ein Gerät – Azure IoT Edge | Microsoft-Dokumentation'
description: In diesem Tutorial stellen Sie Azure Machine Learning als Modul auf einem Edgegerät bereit.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/15/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: db8318e94b646d57c00bc2e6958ba9e7f46ec7af
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344013"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Tutorial: Bereitstellen von Azure Machine Learning als IoT Edge-Modul (Vorschauversion)

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie ein Azure Machine Learning-Modul bereitstellen, das anhand der Temperaturdaten eines simulierten Computers den Ausfall eines Geräts vorhersagt. Weitere Informationen zu Azure ML in IoT Edge finden Sie in der [Azure Machine Learning-Dokumentation](../machine-learning/service/how-to-deploy-to-iot.md).

Das Azure Machine Learning-Modul, das Sie in diesem Tutorial erstellen, liest die von Ihrem Gerät generierten Umgebungsdaten und kennzeichnet die Nachrichten ggf. als anomal.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Machine Learning-Moduls
> * Pushen eines Modulcontainers in eine Azure Container Registry
> * Bereitstellen eines Azure Machine Learning-Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten

>[!NOTE]
>Azure Machine Learning-Module in Azure IoT Edge sind als Public Preview verfügbar. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

* Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als Edge-Gerät verwenden, indem Sie die Schritte ausführen, die in der Schnellstartanleitung für [Linux](quickstart-linux.md)- oder [Windows](quickstart.md)-Geräte beschrieben sind.
* Das Azure Machine Learning-Modul unterstützt keine ARM-Prozessoren.

Cloudressourcen:

* Eine [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-Instanz in Azure im Tarif „Free“ oder „Standard“. 
* Ein Azure Machine Learning-Arbeitsbereich. Gehen Sie gemäß der Anleitung unter [Vorbereiten der Bereitstellung von Modellen in IoT Edge](../machine-learning/service/how-to-deploy-to-iot.md) vor, um einen zu erstellen.


### <a name="disable-process-identification"></a>Deaktivieren der Prozessidentifizierung

>[!NOTE]
>
> Während der Vorschauphase unterstützt Azure Machine Learning das Sicherheitsfeature der Prozessidentifizierung nicht, das standardmäßig für IoT Edge aktiviert ist. 
> Das Feature kann mithilfe folgender Schritte deaktiviert werden. Dies ist jedoch für Produktionsumgebungen nicht geeignet. Diese Schritte sind nur unter Linux erforderlich, da sie bei der Installation der Windows Edge-Runtime bereits ausgeführt wurden.

Um die Prozessidentifizierung auf Ihrem IoT Edge-Gerät zu deaktivieren, müssen Sie die IP-Adresse und den Port für **workload_uri** und **management_uri** im Abschnitt **connect** der IoT Edge-Daemonkonfiguration angeben.

Ermitteln Sie zunächst die IP-Adresse. Geben Sie `ipconfig` in Ihre Befehlszeile ein, und kopieren Sie die IP-Adresse der Schnittstelle **docker0**.

Bearbeiten Sie die Konfigurationsdatei für den IoT Edge-Daemon:

```cmd/sh
sudo nano /etc/iotedge/config.yaml
```

Aktualisieren Sie den Abschnitt **connect** der Konfiguration mit Ihrer IP-Adresse. Beispiel:
```yaml
connect:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Geben Sie die gleichen Adressen in den Konfigurationsabschnitt **listen** ein. Beispiel:

```yaml
listen:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Erstellen Sie eine Umgebungsvariable vom Typ „IOTEDGE_HOST“ mit der Adresse für „management_uri“. (Wenn Sie sie dauerhaft festlegen möchten, fügen Sie sie zu `/etc/environment` hinzu.) Beispiel:

```cmd/sh
export IOTEDGE_HOST="http://172.17.0.1:15580"
```


## <a name="create-the-azure-ml-container"></a>Erstellen des Azure ML-Containers
In diesem Abschnitt laden Sie die trainierten Modelldateien herunter und konvertieren sie in einen Azure ML Container.

Erstellen Sie gemäß der Anleitung unter [Vorbereiten der Bereitstellung von Modellen in IoT Edge](../machine-learning/service/how-to-deploy-to-iot.md) einen Docker-Container mit Ihrem Machine Learning-Modell.  Alle erforderlichen Komponenten für das Docker-Image finden Sie im [Git-Repository mit dem KI-Toolkit für Azure IoT Edge](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

### <a name="view-the-container-repository"></a>Anzeigen des Containerrepositorys

Überprüfen Sie, ob Ihr Containerimage erfolgreich erstellt und in der Azure Container Registry-Instanz gespeichert wurde, die Ihrer Machine Learning-Umgebung zugeordnet ist.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu **Alle Dienste**, und wählen Sie **Containerregistrierungen** aus.
2. Wählen Sie Ihre Registrierung aus. Der Name sollte mit **mlcr** beginnen und zu der Ressourcengruppe, dem Speicherort und dem Abonnement gehören, die Sie zum Einrichten der Modulverwaltung verwendet haben.
3. Wählen Sie **Zugriffsschlüssel** aus.
4. Kopieren Sie **Anmeldeserver**, **Benutzername** und **Kennwort**.  Diese sind für den Zugriff auf die Registrierung über Ihre Edge-Geräte erforderlich.
5. Wählen Sie **Repositorys** aus.
6. Wählen Sie **machinelearningmodule** aus.
7. Sie verfügen jetzt über den vollständigen Imagepfad des Containers. Notieren Sie sich diesen Imagepfad für den nächsten Abschnitt. Er sollte wie folgt aussehen: **<Registrierungsname>.azurecr.io/machinelearningmodule:1**

## <a name="deploy-to-your-device"></a>Bereitstellen auf Ihrem Gerät

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.

1. Wechseln Sie zu **IoT Edge**, und wählen Sie Ihr IoT Edge-Gerät aus.

1. Wählen Sie **Module festlegen** aus.

1. Fügen Sie im Abschnitt **Registrierungseinstellungen** die Anmeldeinformationen hinzu, die Sie aus der Azure Container Registry-Instanz kopiert haben. 

   ![Hinzufügen von Registrierungsanmeldeinformationen zum Manifest](./media/tutorial-deploy-machine-learning/registry-settings.png)

1. Wenn Sie zuvor das tempSensor-Modul auf Ihrem IoT Edge-Gerät bereitgestellt haben, wird es unter Umständen automatisch aufgefüllt. Wenn es nicht bereits in Ihrer Liste der Module enthalten ist, fügen Sie es hinzu.

    1. Klicken Sie auf **Hinzufügen** und anschließend auf **IoT Edge-Modul**.
    2. Geben Sie im Feld **Name** die Zeichenfolge `tempSensor` ein.
    3. Geben Sie im Feld **Image-URI** die Zeichenfolge `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0` ein.
    4. Wählen Sie **Speichern**aus.

1. Fügen Sie das von Ihnen erstellte Machine Learning-Modul hinzu.

    1. Klicken Sie auf **Hinzufügen** und anschließend auf **IoT Edge-Modul**.
    1. Geben Sie im Feld **Name** die Zeichenfolge `machinelearningmodule` ein.
    1. Geben Sie im Feld **Image** die Imageadresse ein, z.B. `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. Wählen Sie **Speichern**aus.

1. Wenn Sie zum Schritt **Module hinzufügen** zurückgekehrt sind, wählen Sie **Weiter** aus.

1. Kopieren Sie im Schritt **Routen angeben** den folgenden JSON-Code in das Textfeld. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „amlInput“ zum Machine Learning-Modul. Dies ist der Endpunkt, den alle Azure Machine Learning-Module verwenden. Die zweite Route transportiert Nachrichten aus dem Machine Learning-Modul an IoT Hub. Bei dieser Route ist „amlOutput“ der Endpunkt, den alle Azure Machine Learning-Module zur Ausgabe von Daten verwenden, und „$upstream“ kennzeichnet IoT Hub.

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ```

1. Klicken Sie auf **Weiter**.

1. Klicken Sie im Schritt für die **Bereitstellungsüberprüfung** auf **Übermitteln**.

1. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie **Aktualisieren** aus.  Es sollte nun angezeigt werden, dass das neue **Machine Learning-Modul** zusammen mit dem **tempSensor**-Modul und den IoT Edge-Laufzeitmodulen ausgeführt wird.

## <a name="view-generated-data"></a>Anzeigen generierter Daten

Sie können Nachrichten anzeigen, die von den einzelnen IoT Edge-Modulen generiert werden, und Sie können Nachrichten anzeigen, die an Ihre IoT Hub-Instanz übermittelt werden.

### <a name="view-data-on-your-iot-edge-device"></a>Anzeigen von Daten auf Ihrem IoT Edge-Gerät

Auf Ihrem IoT Edge-Gerät können Sie die Nachrichten anzeigen, die von den einzelnen Modulen gesendet werden. 

Wenn Sie diese Befehle auf einem Linux-Gerät ausführen, müssen Sie ggf. `sudo` verwenden, um erhöhte Berechtigungen zu erhalten.

1. Zeigen Sie alle Module auf Ihrem IoT Edge-Gerät an.

   ```cmd/sh
   iotedge list
   ```

2. Zeigen Sie die Nachrichten an, die von einem bestimmten Gerät gesendet werden. Verwenden Sie den Modulnamen aus der Ausgabe des vorherigen Befehls.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>Anzeigen von Daten, die bei Ihrer IoT Hub-Instanz eingehen

Sie können die Gerät-zu-Cloud-Nachrichten, die Ihre IoT Hub-Instanz empfängt, mithilfe der [Azure IoT Hub-Toolkit-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (bisher als Azure IoT-Toolkit-Erweiterung bekannt) anzeigen.

Die folgenden Schritte zeigen, wie Sie Visual Studio Code einrichten, um Gerät-zu-Cloud-Nachrichten zu überwachen, die bei Ihrer IoT Hub-Instanz eingehen. 

1. Wählen Sie in Visual Studio Code **IoT Hub-Geräte** aus.

2. Wählen Sie **...** und dann **IoT Hub-Verbindungszeichenfolge festlegen** aus dem Menü aus.

   ![Festlegen der IoT Hub-Verbindungszeichenfolge](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Geben Sie in das Textfeld, das sich oben auf der Seite öffnet, die Verbindungszeichenfolge „iothubowner“ für Ihren IoT Hub ein. Ihr IoT Edge-Gerät sollte in der Liste der IoT Hub-Geräte angezeigt werden.

4. Wähle Sie **...** erneut aus, und wählen Sie anschließend **Start monitoring D2C message** (Überwachung von D2C-Nachricht starten).

5. Beobachten Sie die ankommenden Nachrichten von tempSensor alle fünf Sekunden. Der Nachrichtentext enthält die **anomaly**-Eigenschaft, die vom Machine Learning-Modul den Wert TRUE oder FALSE erhält. Die **AzureMLResponse**-Eigenschaft enthält den Wert „OK“, wenn das Modell erfolgreich ausgeführt wurde.

   ![Azure ML-Antwort im Nachrichtentext](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden. Sie können auch dasselbe IoT Edge-Gerät als Testgerät weiter nutzen. 

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein von Azure Machine Learning unterstütztes IoT Edge-Modul bereitgestellt. Sie können mit einem der anderen Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Tutorial: Perform image classification at the edge with Custom Vision Service](tutorial-deploy-custom-vision.md) (Tutorial: Ausführen der Bildklassifizierung im Edge-Bereich mit Custom Vision Service)

