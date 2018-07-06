---
title: Simulieren von Azure IoT Edge unter Linux | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie vordefinierten Code per Remotezugriff auf einem IoT Edge-Gerät bereitstellen.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0e0d22b3363b00c81be5091fd12773f9e486c09e
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099184"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Schnellstart: Bereitstellen des ersten IoT Edge-Moduls auf einem Linux-basierten x64-Gerät

Mit Azure IoT Edge können Sie Analysen und die Datenverarbeitung auf Ihren Geräten durchführen, statt alle Daten per Push in die Cloud zu übertragen. Die IoT Edge-Tutorials veranschaulichen, wie Sie verschiedene Modultypen bereitstellen. Zunächst benötigen Sie aber ein Testgerät. 

In dieser Schnellstartanleitung wird Folgendes vermittelt:

1. Erstellen Sie einen IoT Hub.
2. Registrieren eines IoT Edge-Geräts für Ihren IoT Hub
3. Starten der IoT Edge-Runtime
4. Durchführen der Remotebereitstellung eines Moduls auf einem IoT Edge-Gerät

![Aufbau des Tutorials][2]

In dieser Schnellstartanleitung wird Ihr Linux-Computer bzw. virtueller Computer in ein IoT Edge-Gerät verwandelt. Anschließend können Sie ein Modul aus dem Azure-Portal auf Ihrem Gerät bereitstellen. Das Modul, das Sie in dieser Schnellstartanleitung bereitstellen, ist ein simulierter Sensor, mit dem Daten zu Temperatur, Luftfeuchtigkeit und Luftdruck generiert werden. Die anderen Tutorials zu Azure IoT Edge bauen darauf auf und erläutern die Bereitstellung von Modulen, mit denen die simulierten Daten analysiert werden, um geschäftsrelevante Erkenntnisse zu gewinnen. 

Wenn Sie nicht über ein aktives Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][lnk-account] erstellen, bevor Sie beginnen.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Beginnen Sie mit der Schnellstartanleitung, indem Sie Ihre IoT Hub-Instanz im Azure-Portal erstellen.
![Erstellen des IoT Hubs][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registrieren eines IoT Edge-Geräts

Registrieren Sie ein IoT Edge-Gerät bei Ihrem neu erstellten IoT Hub.
![Registrieren eines Geräts][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]


## <a name="install-and-start-the-iot-edge-runtime"></a>Installieren und Starten der IoT Edge-Runtime

Installieren und starten Sie die Azure IoT Edge-Runtime auf Ihrem Gerät. 
![Registrieren eines Geräts][5]

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie besteht aus drei Komponenten. Der **Daemon für die IoT Edge-Sicherheit** wird jedes Mal gestartet, wenn ein Edge-Gerät gestartet wird, und startet den IoT Edge-Agent, um einen Bootstrapvorgang durchzuführen. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät, einschließlich des IoT Edge-Hubs. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. 

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

Installieren Sie Moby, eine Containerruntime und die zugehörigen CLI-Befehle. 

   ```bash
   sudo apt-get install moby-engine
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

3. Fügen Sie die Verbindungszeichenfolge des IoT Edge-Geräts hinzu, die Sie bei der Registrierung Ihres Geräts kopiert haben. Ersetzen Sie den Wert für die Variable **device_connection_string** durch den Wert, den Sie weiter oben in dieser Schnellstartanleitung kopiert haben.

4. Starten Sie den Daemon für die Edge-Sicherheit neu:

   ```bash
   sudo systemctl restart iotedge
   ```

5. Überprüfen Sie, ob der Daemon für die Edge-Sicherheit als Systemdienst ausgeführt wird:

   ```bash
   sudo systemctl status iotedge
   ```

   ![Anzeigen der Ausführung des Edge-Daemons als Systemdienst](./media/quickstart-linux/iotedged-running.png)

   Sie können die Protokolle des Daemons für die Edge-Sicherheit anzeigen, indem Sie den folgenden Befehl ausführen:

   ```bash
   journalctl -u iotedge
   ```

6. Zeigen Sie die Module an, die auf Ihrem Gerät ausgeführt werden: 

   ```bash
   sudo iotedge list
   ```
Nach einer Ab- und Anmeldung ist *sudo* für den obigen Befehl nicht erforderlich.

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

Wenn Sie mit den IoT Edge-Tutorials fortfahren möchten, können Sie das Gerät verwenden, das Sie in dieser Schnellstartanleitung registriert und eingerichtet haben. Verwenden Sie die folgenden Befehle, wenn Sie die Installationen von Ihrem Gerät entfernen möchten.  

Entfernen Sie die IoT Edge-Runtime.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Löschen Sie die Container, die auf Ihrem Gerät erstellt wurden. 

   ```bash
   sudo docker rm -f $(sudo docker ps -aq)
   ```

Entfernen Sie die Containerruntime.

   ```bash
   sudo apt-get remove --purge moby
   ```

Wenn Sie den Azure IoT-Hub oder das IoT Edge-Gerät, das Sie in dieser Schnellstartanleitung erstellt haben, nicht mehr benötigen, können Sie diese Komponenten im Azure-Portal löschen. Navigieren Sie zur Übersichtsseite Ihrer IoT Hub-Instanz, und klicken Sie auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein neues IoT Edge-Gerät erstellt und die Azure IoT Edge-Cloudschnittstelle zum Bereitstellen von Code auf dem Gerät verwendet. Sie verfügen nun über ein simuliertes Gerät, das Rohdaten zu seiner Umgebung generiert. 

Diese Schnellstartanleitung ist die Voraussetzung für alle IoT Edge-Tutorials. Sie können mit den weiteren Tutorials fortfahren, um zu erfahren, wie Azure IoT Edge Ihnen beim Umwandeln dieser Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Filtern von Sensordaten mit einer Azure-Funktion](tutorial-deploy-function.md)


<!-- Images -->
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
