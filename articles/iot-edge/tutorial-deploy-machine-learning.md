---
title: Bereitstellen von Azure Machine Learning mit Azure IoT Edge | Microsoft-Dokumentation
description: Bereitstellen von Azure Machine Learning als Modul auf einem Edgegerät
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/25/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 62ca816f7bdc183727eb22806ba9e733c8b97c44
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173504"
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Bereitstellen von Azure Machine Learning als IoT Edge-Modul – Vorschau

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie ein Azure Machine Learning-Modul bereitstellen, das anhand der Temperaturdaten eines simulierten Computers den Ausfall eines Geräts vorhersagt. Weitere Informationen zu Azure ML in IoT Edge finden Sie in der [Azure Machine Learning-Dokumentation](../machine-learning/desktop-workbench/use-azure-iot-edge-ai-toolkit.md).

Das Azure Machine Learning-Modul, das Sie in diesem Tutorial erstellen, liest die von Ihrem Gerät generierten Umgebungsdaten und kennzeichnet die Nachrichten ggf. als anomal.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Machine Learning-Moduls
> * Pushen eines Modulcontainers in eine Azure Container Registry
> * Bereitstellen eines Azure Machine Learning-Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten

>[!NOTE]
>Azure Machine Learning-Module in Azure IoT Edge sind als Public Preview verfügbar. 

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein IoT Edge-Gerät, um die in diesem Tutorial erstellten Machine Learning-Module zu testen. Sie können das Gerät verwenden, das Sie im Rahmen der Schnellstartanleitung für [Linux-Geräte](quickstart-linux.md) oder [Windows-Geräte](quickstart.md) konfiguriert haben. 

Das Azure Machine Learning-Modul unterstützt keine ARM-Prozessoren.

Auf dem Entwicklungscomputer müssen die folgenden Komponenten vorhanden sein: 
* Ein Azure Machine Learning-Konto. Halten Sie sich an die Anleitung in [Erstellen von Azure Machine Learning-Konten und Installieren von Azure Machine Learning Workbench](../machine-learning/service/quickstart-installation.md#create-azure-machine-learning-services-accounts). Sie müssen die Workbenchanwendung für dieses Tutorial nicht installieren. 
* Modellverwaltung für Azure ML auf Ihrem Computer. Befolgen Sie die Anweisungen in [Setup für die Modellverwaltung](../machine-learning/desktop-workbench/deployment-setup-configuration.md), um Ihre Umgebung einzurichten und ein Konto zu erstellen. Verwenden Sie beim Einrichten der Bereitstellung nach Möglichkeit die lokalen Schritte anstelle der Clusterschritte.

### <a name="disable-process-identification"></a>Deaktivieren der Prozessidentifizierung

>[!NOTE]
>
> Während der Vorschauphase unterstützt Azure Machine Learning das Sicherheitsfeature der Prozessidentifizierung nicht, das standardmäßig für IoT Edge aktiviert ist. 
> Das Feature kann mithilfe folgender Schritte deaktiviert werden. Dies ist jedoch für Produktionsumgebungen nicht geeignet. Diese Schritte sind nur unter Linux erforderlich, da sie bei der Einrichtung der Windows Edge-Runtime bereits ausgeführt wurden.

Um die Prozessidentifizierung auf Ihrem IoT Edge-Gerät zu deaktivieren, müssen Sie die IP-Adresse und den Port für **workload_uri** und **management_uri** im Abschnitt **connect** der IoT Edge-Daemonkonfiguration angeben.

Ermitteln Sie zunächst die IP-Adresse. Geben Sie `ifconfig` in Ihre Befehlszeile ein, und kopieren Sie die IP-Adresse der Schnittstelle **docker0**.

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

Laden Sie auf dem Computer, auf dem die Modulverwaltung für Azure ML ausgeführt wird, [iot_score.py](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/iot_score.py) und [model.pkl](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/model.pkl) aus dem Azure ML IoT-Toolkit auf GitHub herunter und speichern Sie sie. Diese Dateien definieren das trainierte Machine Learning-Modell, das Sie auf Ihrem Iot Edge-Gerät bereitstellen werden.

Verwenden Sie das trainierte Modell, um einen Container zu erstellen, der für IoT Edge-Geräte bereitgestellt werden kann. Verwenden Sie den folgenden Befehl, um folgende Aktionen auszuführen:

   * Registrieren des Modells
   * Erstellen Sie ein Manifest.
   * Erstellen eines Docker-Containerimages namens *machinelearningmodule*
   * Bereitstellen des Images in Ihrem AKS-Cluster (Azure Kubernetes Service)

```cmd
az ml service create realtime --model-file model.pkl -f iot_score.py -n machinelearningmodule -r python
```

### <a name="view-the-container-repository"></a>Anzeigen des Containerrepositorys

Überprüfen Sie, ob Ihr Containerimage erfolgreich erstellt und in der Azure Container Registry-Instanz gespeichert wurde, die Ihrer Machine Learning-Umgebung zugeordnet ist.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu **Alle Dienste**, und wählen Sie **Containerregistrierungen** aus.
2. Wählen Sie Ihre Registrierung aus. Der Name sollte mit **mlcr** beginnen und zu der Ressourcengruppe, dem Speicherort und dem Abonnement gehören, die Sie zum Einrichten der Modulverwaltung verwendet haben.
3. Wählen Sie **Zugriffsschlüssel** aus.
4. Kopieren Sie **Anmeldeserver**, **Benutzername** und **Kennwort**.  Diese sind für den Zugriff auf die Registrierung über Ihre Edge-Geräte erforderlich.
5. Wählen Sie **Repositorys** aus.
6. Wählen Sie **machinelearningmodule** aus.
7. Sie verfügen jetzt über den vollständigen Imagepfad des Containers. Notieren Sie sich diesen Imagepfad für den nächsten Abschnitt. Er sollte wie folgt aussehen: **<Registrierungsname>.azureacr.io/machinelearningmodule:1**

## <a name="deploy-to-your-device"></a>Bereitstellen auf Ihrem Gerät

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.

1. Wechseln Sie zu **IoT Edge**, und wählen Sie Ihr IoT Edge-Gerät aus.

1. Wählen Sie **Module festlegen** aus.

1. Fügen Sie im Abschnitt **Registrierungseinstellungen** die Anmeldeinformationen hinzu, die Sie aus der Azure Container Registry-Instanz kopiert haben. 

   ![Hinzufügen der Registrierungsanmeldeinformationen](./media/tutorial-deploy-machine-learning/registry-settings.png)

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

Sie können die Gerät-zu-Cloud-Nachrichten, die Ihre IoT Hub-Instanz empfängt, mithilfe des [IoT Hub-Explorers](https://github.com/azure/iothub-explorer) oder mithilfe der [Azure IoT Toolkit-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) anzeigen.

Die folgenden Schritte zeigen, wie Sie Visual Studio Code einrichten, um Gerät-zu-Cloud-Nachrichten zu überwachen, die bei Ihrer IoT Hub-Instanz eingehen. 

1. Wählen Sie in Visual Studio Code **IoT Hub-Geräte** aus.

2. Wählen Sie **...** und dann **IoT Hub-Verbindungszeichenfolge festlegen** aus dem Menü aus.

   ![Menü „...“ für IoT Hub-Geräte](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Geben Sie in das Textfeld, das sich oben auf der Seite öffnet, die Verbindungszeichenfolge „iothubowner“ für Ihren IoT Hub ein. Ihr IoT Edge-Gerät sollte in der Liste der IoT Hub-Geräte angezeigt werden.

4. Wähle Sie **...** erneut aus, und wählen Sie anschließend **Start monitoring D2C message** (Überwachung von D2C-Nachricht starten).

5. Beobachten Sie die ankommenden Nachrichten von tempSensor alle fünf Sekunden. Der Nachrichtentext enthält die **anomaly**-Eigenschaft, die vom Machine Learning-Modul den Wert TRUE oder FALSE erhält. Die **AzureMLResponse**-Eigenschaft enthält den Wert „OK“, wenn das Modell erfolgreich ausgeführt wurde.

   ![Azure ML-Antwort im Nachrichtentext](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die bereits erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden.

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 

> [!IMPORTANT]
> Das Löschen der Azure-Ressourcen und der Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. Wenn Sie die IoT Hub-Ressource in einer bereits vorhandenen Ressourcengruppe erstellt haben, die Ressourcen enthält, die Sie behalten möchten, löschen Sie nicht die Ressourcengruppe, sondern nur die IoT Hub-Ressource.
>

Wenn Sie nur die IoT Hub-Instanz löschen möchten, führen Sie den folgenden Befehl mit dem Namen des Hubs und dem Namen der Ressourcengruppe aus:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Löschen Sie die gesamte Ressourcengruppe wie folgt anhand des Namens:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.

2. Geben Sie im Textfeld **Nach Name filtern...** den Namen der Ressourcengruppe ein, die Ihre IoT Hub-Ressource enthält. 

3. Klicken Sie in der Ergebnisliste rechts neben Ihrer Ressourcengruppe auf **...** und dann auf **Ressourcengruppe löschen**.

<!--
   ![Delete](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)
-->
4. Sie werden aufgefordert, das Löschen der Ressourcengruppe zu bestätigen. Geben Sie zur Bestätigung erneut den Namen Ihrer Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**. Daraufhin werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein von Azure Machine Learning unterstütztes IoT Edge-Modul bereitgestellt. Sie können mit einem der anderen Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Filtern von Sensordaten mithilfe von C#-Code](tutorial-csharp-module.md)

<!--Links-->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
