---
title: Bereitstellen von Azure Machine Learning mit Azure IoT Edge | Microsoft-Dokumentation
description: Bereitstellen von Azure Machine Learning als Modul auf einem Edgegerät
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/12/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1b8a6079a68c50f80b593a1f56bc5256047ef31c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Bereitstellen von Azure Machine Learning als IoT Edge-Modul – Vorschau

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. Dieses Tutorial führt Sie durch die Bereitstellung eines Azure Machine Learning-Moduls, das vorhersagt, wann bei einem Gerät ein Fehler auftritt. Dies basiert auf Sensordaten des simulierten IoT Edge-Geräts, das Sie im Tutorial zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] erstellt haben. 

In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Erstellen eines Azure Machine Learning-Moduls
> * Pushen eines Modulcontainers in eine Azure Container Registry
> * Bereitstellen eines Azure Machine Learning-Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten

Das Azure Machine Learning-Modul, das Sie in diesem Tutorial erstellen, liest die von Ihrem Gerät generierten Umgebungsdaten und kennzeichnet die Nachrichten ggf. als anomal. 

## <a name="prerequisites"></a>Voraussetzungen

* Das Azure IoT Edge-Gerät, das Sie mit der Schnellstartanleitung oder im ersten Tutorial erstellt haben.
* Die IoT Hub-Verbindungszeichenfolge für den IoT Hub, mit dem Ihr IoT Edge-Gerät eine Verbindung herstellt
* Ein Azure Machine Learning-Konto. Befolgen Sie zum Erstellen eines Kontos die Anweisungen in [Erstellen von Azure Machine Learning-Konten und Installieren von Azure Machine Learning Workbench](../machine-learning/preview/quickstart-installation.md#create-azure-machine-learning-services-accounts). Sie müssen die Workbenchanwendung für dieses Tutorial nicht installieren. 
* Modulverwaltung für Azure ML auf Ihrem Computer. Befolgen Sie die Anweisungen in [Setup für die Modellverwaltung](https://docs.microsoft.com/azure/machine-learning/preview/deployment-setup-configuration), um Ihre Umgebung einzurichten und ein Konto zu erstellen.

## <a name="create-the-azure-ml-container"></a>Erstellen des Azure ML-Containers
In diesem Abschnitt laden Sie die trainierten Modelldateien herunter und konvertieren sie in einen Azure ML Container.  

Laden Sie auf dem Computer, auf dem die Modulverwaltung für Azure ML ausgeführt wird, [iot_score.py](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/iot_score.py) und [model.pkl](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/model.pkl) aus dem Azure ML IoT-Toolkit auf GitHub herunter und speichern Sie sie. Diese Dateien definieren das trainierte Machine Learning-Modell, das Sie auf Ihrem Iot Edge-Gerät bereitstellen werden. 

Verwenden Sie das trainierte Modell, um einen Container zu erstellen, der für IoT Edge-Geräte bereitgestellt werden kann. Verwenden Sie den folgenden Befehl, um folgende Aktionen auszuführen:

   * Registrieren des Modells
   * Erstellen Sie ein Manifest.
   * Erstellen eines Docker-Containerimages namens *machinelearningmodule*
   * Bereitstellen des Images in Ihrem Azure Container Service-Cluster (AKS)

```cmd
az ml service create realtime --model-file model.pkl -f iot_score.py -n machinelearningmodule -r python
```

### <a name="view-the-container-repository"></a>Anzeigen des Containerrepositorys

Überprüfen Sie, ob Ihr Containerimage erfolgreich erstellt und im Azure Container Repository gespeichert wurde, das Ihrer Machine Learning-Umgebung zugeordnet ist.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu **Alle Dienste**, und wählen Sie **Containerregistrierungen** aus.
2. Wählen Sie Ihre Registrierung aus. Der Name sollte mit **mlcr** beginnen und zu der Ressourcengruppe, dem Speicherort und dem Abonnement gehören, die Sie zum Einrichten der Modulverwaltung verwendet haben.
3. Wählen Sie **Zugriffsschlüssel** aus.
4. Kopieren Sie **Anmeldeserver**, **Benutzername** und **Kennwort**.  Diese sind für den Zugriff auf die Registrierung über Ihre Edge-Geräte erforderlich.
5. Wählen Sie **Repositorys** aus.
6. Wählen Sie **machinelearningmodule** aus.
7. Sie verfügen jetzt über den vollständigen Imagepfad des Containers. Notieren Sie sich diesen Imagepfad für den nächsten Abschnitt. Er sollte wie folgt aussehen: **<Registrierungsname>.azureacr.io/machinelearningmodule:1**

## <a name="add-registry-credentials-to-your-edge-device"></a>Hinzufügen von Registrierungsanmeldeinformationen zur Edge-Runtime auf Ihrem Edge-Gerät

Fügen Sie die Anmeldeinformationen für Ihre Registrierung zur Edge-Runtime auf dem Computer hinzu, auf dem Sie Ihr Edge-Gerät ausgeführen. Durch diesen Befehl erhält die Runtime Zugriff zum Pullen des Containers.

Linux:
   ```cmd
   sudo iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password> 
   ```

Windows:
   ```cmd
   iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password> 
   ```

## <a name="run-the-solution"></a>Ausführen der Lösung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
1. Wechseln Sie zu **IoT Edge (Vorschau)**, und wählen Sie Ihr IoT Edge-Gerät aus.
1. Wählen Sie **Module festlegen** aus.
1. Wenn Sie zuvor das tempSensor-Modul auf Ihrem IoT Edge-Gerät bereitgestellt haben, wird es unter Umständen automatisch aufgefüllt. Wenn es nicht bereits in Ihrer Liste der Module enthalten ist, fügen Sie es hinzu.
    1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
    2. Geben Sie im Feld **Name** die Zeichenfolge `tempSensor` ein.
    3. Geben Sie im Feld **Image-URI** die Zeichenfolge `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` ein.
    4. Wählen Sie **Speichern**aus.
1. Fügen Sie das von Ihnen erstellte Machine Learning-Modul hinzu.
    1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
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
1. Wählen Sie im Schritt **Vorlage überprüfen** die Option **Senden** aus. 
1. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie **Aktualisieren** aus.  Es sollte nun angezeigt werden, dass das neue **Machine Learning-Modul** zusammen mit dem **tempSensor**-Modul und den IoT Edge-Laufzeitmodulen ausgeführt wird.

## <a name="view-generated-data"></a>Anzeigen generierter Daten

Sie können die Gerät-zu-Cloud-Nachrichten, die Ihr IoT Edge-Gerät sendet, mithilfe des [IoT Hub-Explorers](https://github.com/azure/iothub-explorer) oder der Azure IoT Toolkit-Erweiterung für Visual Studio Code anzeigen. 

1. Wählen Sie in Visual Studio Code **IoT Hub-Geräte** aus. 
2. Wählen Sie **...** und dann **IoT Hub-Verbindungszeichenfolge festlegen** aus dem Menü aus. 

   ![Menü „...“ für IoT Hub-Geräte](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Geben Sie in das Textfeld, das sich oben auf der Seite öffnet, die Verbindungszeichenfolge „iothubowner“ für Ihren IoT Hub ein. Ihr IoT Edge-Gerät sollte in der Liste der IoT Hub-Geräte angezeigt werden.
4. Wähle Sie **...** erneut aus, und wählen Sie anschließend **Start monitoring D2C message** (Überwachung von D2C-Nachricht starten).
5. Beobachten Sie die ankommenden Nachrichten von tempSensor alle fünf Sekunden. Der Nachrichtentext enthält die **anomaly**-Eigenschaft, die vom Machine Learning-Modul den Wert TRUE oder FALSE erhält. Die **AzureMLResponse**-Eigenschaft enthält den Wert „OK“, wenn das Modell erfolgreich ausgeführt wurde. 

   ![Azure ML-Antwort im Nachrichtentext](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein von Azure Machine Learning unterstütztes IoT Edge-Modul bereitgestellt. Sie können mit einem der anderen Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Bereitstellen einer Azure-Funktion als Modul](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
