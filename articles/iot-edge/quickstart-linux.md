---
title: Schnellstart für Azure IoT Edge + Linux | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie vordefinierten Code per Remotezugriff auf einem IoT Edge-Gerät bereitstellen.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: dfcb764d75b7328d1234d47d82afdae8d6a0deef
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413013"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Schnellstart: Bereitstellen des ersten IoT Edge-Moduls auf einem Linux-basierten x64-Gerät

Mit Azure IoT Edge werden die Vorteile der Cloud auf Ihre IoT-Geräte übertragen. In dieser Schnellstartanleitung erfahren Sie, wie Sie die Cloudschnittstelle für die Remotebereitstellung von vordefiniertem Code auf einem IoT Edge-Gerät verwenden.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

1. Erstellen Sie einen IoT Hub.
2. Registrieren eines IoT Edge-Geräts für Ihren IoT Hub
3. Installieren und Starten der IoT Edge-Runtime auf Ihrem Gerät
4. Durchführen der Remotebereitstellung eines Moduls auf einem IoT Edge-Gerät

![Architektur der Schnellstartanleitung][2]

In dieser Schnellstartanleitung wird Ihr Linux-Computer bzw. virtueller Computer in ein IoT Edge-Gerät verwandelt. Anschließend können Sie ein Modul aus dem Azure-Portal auf Ihrem Gerät bereitstellen. Das Modul, das Sie in dieser Schnellstartanleitung bereitstellen, ist ein simulierter Sensor, mit dem Daten zu Temperatur, Luftfeuchtigkeit und Luftdruck generiert werden. Die anderen Tutorials zu Azure IoT Edge bauen darauf auf und erläutern die Bereitstellung von Modulen, mit denen die simulierten Daten analysiert werden, um geschäftsrelevante Erkenntnisse zu gewinnen. 

Wenn Sie über kein aktives Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][lnk-account] erstellen, bevor Sie beginnen.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Sie verwenden die Azure CLI für viele Schritte in dieser Schnellstartanleitung, und Azure IoT verfügt über eine Erweiterung zur Aktivierung der zusätzlichen Funktionalität. 

Fügen Sie die Azure IoT-Erweiterung der Cloud Shell-Instanz hinzu.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```
   
## <a name="prerequisites"></a>Voraussetzungen

Cloudressourcen: 

* Eine Ressourcengruppe zum Verwalten aller Ressourcen, die Sie in dieser Schnellstartanleitung verwenden. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

* Ein virtueller Linux-Computer als IoT Edge-Gerät. 

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username azureuser --generate-ssh-keys --size Standard_B1ms
   ```

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Beginnen Sie mit der Schnellstartanleitung, indem Sie Ihre IoT Hub-Instanz mit Azure CLI erstellen. 

![IoT Hub erstellen][3]

Der kostenlose IoT Hub kann für diesen Schnellstart verwendet werden. Wenn Sie den IoT Hub schon einmal genutzt und bereits einen kostenlosen Hub erstellt haben, können Sie diesen IoT Hub verwenden. Jedes Abonnement kann nur über einen kostenlosen IoT Hub verfügen. 

Mit dem folgenden Code wird ein kostenloser **F1**-Hub in der Ressourcengruppe **IoTEdgeResources** erstellt. Ersetzen Sie *{hub_name}* durch einen eindeutigen Namen für Ihren IoT Hub.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Wenn Sie eine Fehlermeldung erhalten, da bereits ein kostenloser Hub in Ihrem Abonnement vorhanden ist, ändern Sie die SKU auf **S1**. 

## <a name="register-an-iot-edge-device"></a>Registrieren eines IoT Edge-Geräts

Registrieren Sie ein IoT Edge-Gerät bei Ihrem neu erstellten IoT Hub.
![Registrieren eines Geräts][4]

Erstellen Sie eine Geräteidentität für das simulierte Gerät, sodass es mit dem IoT Hub kommunizieren kann. Da IoT Edge-Geräte sich von typischen IoT-Geräten unterscheiden und auf unterschiedliche Weise verwaltet werden können, deklarieren Sie dieses Gerät von Anfang an als IoT Edge-Gerät. 

1. Geben Sie in Azure Cloud Shell den folgenden Befehl ein, um in Ihrem Hub ein Gerät mit dem Namen **myEdgeDevice** zu erstellen.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

1. Rufen Sie die Verbindungszeichenfolge für Ihr Gerät ab, über die Ihr physisches Gerät mit seiner Identität auf dem IoT Hub verknüpft wird. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Kopieren Sie die Verbindungszeichenfolge, und speichern Sie sie. Sie verwenden diesen Wert, um im nächsten Abschnitt die IoT Edge-Runtime zu konfigurieren. 


## <a name="install-and-start-the-iot-edge-runtime"></a>Installieren und Starten der IoT Edge-Runtime

Installieren und starten Sie die Azure IoT Edge-Runtime auf Ihrem Gerät. 
![Registrieren eines Geräts][5]

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie besteht aus drei Komponenten. Der **Daemon für die IoT Edge-Sicherheit** wird jedes Mal gestartet, wenn ein Edge-Gerät gestartet wird, und startet den IoT Edge-Agent, um einen Bootstrapvorgang durchzuführen. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät, einschließlich des IoT Edge-Hubs. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. 

Führen Sie die folgenden Schritte auf dem Linux-Computer oder virtuellen Computer durch, den Sie für diese Schnellstartanleitung vorbereitet haben. 

### <a name="register-your-device-to-use-the-software-repository"></a>Registrieren Ihres Geräts zur Verwendung des Softwarerepositorys

Die Pakete, die Sie zum Ausführen der IoT Edge-Runtime benötigen, werden in einem Softwarerepository verwaltet. Konfigurieren Sie Ihr IoT Edge-Gerät, um auf dieses Repository zuzugreifen. 

Die Schritte in diesem Abschnitt gelten für Geräte, auf denen **Ubuntu 16.04** ausgeführt wird. Informationen zum Zugreifen auf das Softwarerepository unter anderen Versionen von Linux finden Sie unter [Install the Azure IoT Edge runtime on Linux (x64)](how-to-install-iot-edge-linux.md) (Installieren der Azure IoT Edge-Runtime unter Linux (x64)) bzw. [Install Azure IoT Edge runtime on Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) (Installieren der Azure IoT Edge-Runtime unter Linux (ARM32v7/armhf)).

1. Installieren Sie auf dem Computer, den Sie als IoT Edge-Gerät verwenden, die Repositorykonfiguration.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Installieren Sie einen öffentlichen Schlüssel für den Zugriff auf das Repository.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Installieren einer Containerruntime

Die IoT Edge-Runtime umfasst einen Satz mit Containern, und die Logik, die Sie auf Ihrem IoT Edge-Gerät bereitstellen, ist in Form von Containern verpackt. Bereiten Sie Ihr Gerät für diese Komponenten vor, indem Sie eine Containerruntime installieren.

Aktualisieren Sie **apt-get**.

   ```bash
   sudo apt-get update
   ```

Installieren Sie **Moby**, eine Containerruntime.

   ```bash
   sudo apt-get install moby-engine
   ```

Installieren Sie die CLI-Befehle für Moby. 

   ```bash
   sudo apt-get install moby-cli
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>Installieren und Konfigurieren des Daemons für die IoT Edge-Sicherheit

Der Daemon für die Sicherheit wird als Systemdienst installiert, sodass die IoT Edge-Runtime bei jedem Start Ihres Geräts gestartet wird. Die Installation enthält auch eine Version von **hsmlib**, mit der der Daemon für die Sicherheit mit der Hardwaresicherheit des Geräts interagieren kann. 

1. Laden Sie den Daemon für die IoT Edge-Sicherheit herunter, und installieren Sie ihn. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Öffnen Sie die IoT-Edge-Konfigurationsdatei. Da es eine geschützte Datei ist, müssen Sie ggf. erhöhte Rechte nutzen, um darauf zuzugreifen.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Fügen Sie die Verbindungszeichenfolge für das Azure IoT Edge-Gerät hinzu. Suchen Sie die Variable **device_connection_string**, und aktualisieren Sie den Wert mit der Zeichenfolge, die Sie nach der Registrierung Ihres Geräts kopiert haben.

4. Speichern und schließen Sie die Datei. 

   `CTRL + X`, `Y`, `Enter`

4. Starten Sie den Daemon für die Azure IoT Edge-Sicherheit neu.

   ```bash
   sudo systemctl restart iotedge
   ```

5. Überprüfen Sie, ob der Daemon für die Azure Edge-Sicherheit als Systemdienst ausgeführt wird.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Anzeigen der Ausführung des Edge-Daemons als Systemdienst](./media/quickstart-linux/iotedged-running.png)

   Sie können die Protokolle des Daemons für die Edge-Sicherheit anzeigen, indem Sie den folgenden Befehl ausführen:

   ```bash
   journalctl -u iotedge
   ```

6. Zeigen Sie die Module an, die auf Ihrem Gerät ausgeführt werden. 

   >[!TIP]
   >Sie benötigen *sudo*, um zunächst `iotedge`-Befehle auszuführen. Melden Sie sich bei Ihrem Computer ab und dann erneut an, um die Berechtigungen zu aktualisieren. Danach können Sie `iotedge`-Befehle ohne erweiterte Berechtigungen ausführen. 

   ```bash
   sudo iotedge list
   ```

   ![Anzeigen eines Moduls auf Ihrem Gerät](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

Verwalten Sie Ihr Azure IoT Edge-Gerät über die Cloud, um ein Modul bereitzustellen, das Telemetriedaten an die IoT Hub-Instanz sendet.
![Registrieren eines Geräts][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Anzeigen generierter Daten

In diesem Schnellstart haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Runtime darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um mit einem Pushvorgang ein IoT Edge-Modul zur Ausführung auf dem Gerät zu übertragen, ohne Änderungen an dem Gerät selbst vornehmen zu müssen. In diesem Fall erstellt das mit einem Pushvorgang übertragene Modul Umgebungsdaten, die Sie für die Tutorials verwenden können. 

Öffnen Sie erneut eine Eingabeaufforderung auf dem Computer, auf dem das simulierte Gerät ausgeführt wird. Vergewissern Sie sich, dass das über die Cloud bereitgestellte Modul auf dem IoT Edge-Gerät ausgeführt wird:

   ```bash
   sudo iotedge list
   ```
   Nach einer Ab- und Anmeldung ist *sudo* für den obigen Befehl nicht erforderlich.

   ![Anzeigen von drei Modulen auf dem Gerät](./media/quickstart-linux/iotedge-list-2.png)

Zeigen Sie die vom tempSensor-Modul gesendeten Nachrichten an:

   ```bash
   sudo iotedge logs tempSensor -f 
   ```
Nach einer Ab- und Anmeldung ist *sudo* für den obigen Befehl nicht erforderlich.

![Anzeigen der Daten von Ihrem Modul](./media/quickstart-linux/iotedge-logs.png)

Unter Umständen wartet das Temperatursensormodul darauf, die Verbindung mit dem Edge-Hub herzustellen, wenn die letzte Zeile im Protokoll `Using transport Mqtt_Tcp_Only` lautet. Versuchen Sie, das Modul zu beenden und vom Edge-Agent neu starten zu lassen. Sie können es mit dem Befehl `sudo docker stop tempSensor` beenden.

Die vom Gerät gesendeten Telemetriedaten können Sie auch anzeigen, indem Sie das [Tool „IoT Hub-Explorer“][lnk-iothub-explorer] oder die [Azure IoT-Toolkit-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) verwenden. 


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den IoT Edge-Tutorials fortfahren möchten, können Sie das Gerät verwenden, das Sie in dieser Schnellstartanleitung registriert und eingerichtet haben. Andernfalls können Sie die Azure-Ressourcen löschen, die Sie erstellt haben, und die IoT Edge-Runtime von Ihrem Gerät entfernen. 

### <a name="delete-azure-resources"></a>Löschen von Azure-Ressourcen

Wenn Sie Ihren virtuellen Computer und Azure IoT Hub in einer neuen Ressourcengruppe erstellt haben, können Sie diese Gruppe und alle zugehörigen Ressourcen löschen. Wenn Sie etwas in dieser Ressourcengruppe behalten möchten, klicken Sie einfach auf die einzelnen Ressourcen, die Sie bereinigen möchten. 

Entfernen Sie die Gruppe **IoTEdgeResources**. 

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

### <a name="remove-the-iot-edge-runtime"></a>Entfernen der IoT Edge-Runtime

Verwenden Sie die folgenden Befehle, wenn Sie die Installationen von Ihrem Gerät entfernen möchten.  

Entfernen Sie die IoT Edge-Runtime.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Sobald die IoT Edge-Runtime entfernt wird, werden die Container, die sie erstellt hat, angehalten. Sie sind jedoch weiterhin auf dem Gerät vorhanden. Zeigen Sie alle Container an.

   ```bash
   sudo docker ps -a
   ```

Löschen Sie die Container, die auf Ihrem Gerät von der IoT Edge-Runtime erstellt wurden. Ändern Sie den Namen des TempSensor-Containers, wenn Sie ihm einen anderen Namen gegeben haben. 

   ```bash
   sudo docker rm -f tempSensor
   sudo docker rm -f edgeHub
   sudo docker rm -f edgeAgent
   ```

Entfernen Sie die Containerruntime.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Nächste Schritte

Diese Schnellstartanleitung ist die Voraussetzung für alle IoT Edge-Tutorials. Sie können mit einem der anderen Tutorials fortfahren, um zu erfahren, wie Azure IoT Edge Ihnen beim Umwandeln dieser Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Filtern von Sensordaten mit einer Azure-Funktion](tutorial-deploy-function.md)



<!-- Images -->
[0]: ./media/quickstart-linux/cloud-shell.png
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png


<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
