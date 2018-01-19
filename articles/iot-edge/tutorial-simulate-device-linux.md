---
title: Simulieren von Azure IoT Edge unter Linux | Microsoft-Dokumentation
description: "Installieren der Azure IoT Edge-Runtime auf einem simulierten Gerät unter Linux und Bereitstellen des ersten Moduls"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 55770c92f5d5959e83066b425bc6ccf2b9dcc62e
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux-or-macos---preview"></a>Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter Linux oder macOS – Vorschau

Mit Azure IoT Edge können Sie Analysen und die Datenverarbeitung auf Ihren Geräten durchführen, statt alle Daten per Push in die Cloud zu übertragen. Die IoT Edge-Tutorials veranschaulichen, wie Sie verschiedene Modultypen bereitstellen, die aus Azure-Diensten oder benutzerdefiniertem Code entwickelt wurden. Zunächst jedoch benötigen Sie ein Testgerät. 

In diesem Tutorial lernen Sie Folgendes:

1. Erstellen eines IoT Hubs
2. Registrieren eines IoT Edge-Geräts
3. Starten der IoT Edge-Runtime
4. Bereitstellen eines Moduls

![Aufbau des Tutorials][2]

Das simulierte Gerät, das Sie in diesem Tutorial erstellen, ist ein Überwachungssensor, der Temperatur-, Luftfeuchtigkeits- und Druckdaten generiert. Die anderen Tutorials zu Azure IoT Edge bauen darauf auf und erläutern die Bereitstellung von Modulen, mit denen die Daten analysiert werden, um geschäftsrelevante Erkenntnisse zu gewinnen. 

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial verwendet Ihren Computer oder Ihren virtuellen Computer wie ein IoT-Gerät (Internet der Dinge). Um Ihr Gerät in ein IoT Edge-Gerät umzuwandeln, sind folgende Dienste erforderlich:

* Python pip zum Installieren der IoT Edge-Runtime.
   * Linux: `sudo apt-get install python-pip`.
   * macOS: `sudo easy_install pip`.
* Docker zum Ausführen des IoT Edge-Moduls
   * [Installieren Sie Docker für Linux][lnk-docker-ubuntu], und stellen Sie sicher, dass das Programm ausgeführt wird. 
   * [Installieren Sie Docker für macOS][lnk-docker-mac], und stellen Sie sicher, dass das Programm ausgeführt wird. 

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Starten Sie das Tutorial, indem Sie Ihren IoT Hub erstellen.
![Erstellen des IoT Hubs][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registrieren eines IoT Edge-Geräts

Registrieren Sie ein IoT Edge-Gerät bei Ihrem neu erstellten IoT Hub.
![Registrieren eines Geräts][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>Installieren und Starten der IoT Edge-Runtime

Installieren und starten Sie die Azure IoT Edge-Runtime auf Ihrem Gerät. 
![Registrieren eines Geräts][5]

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie umfasst zwei Module. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. Wenn Sie die Runtime auf dem neuen Gerät konfigurieren, wird zunächst nur der IoT Edge-Agent gestartet. Der IoT Edge-Hub wird später beim Bereitstellen eines Moduls verwendet. 

Laden Sie auf dem Computer, auf dem das IoT Edge-Gerät ausgeführt wird, das IoT Edge-Steuerskript herunter:
```cmd
sudo pip install -U azure-iot-edge-runtime-ctl
```

Konfigurieren Sie die Runtime mit der Verbindungszeichenfolge für das IoT Edge-Gerät aus dem vorherigen Abschnitt:
```cmd
sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Starten Sie die Runtime:
```cmd
sudo iotedgectl start
```

Überprüfen Sie Docker, um festzustellen, ob der IoT Edge-Agent als Modul ausgeführt wird:
```cmd
sudo docker ps
```

![Edge-Agent in Docker](./media/tutorial-simulate-device-linux/docker-ps.png)

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

Verwalten Sie Ihr Azure IoT Edge-Gerät über die Cloud, um ein Modul bereitzustellen, das Telemetriedaten an IoT Hub sendet.
![Registrieren eines Geräts][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Anzeigen generierter Daten

In diesem Tutorial haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Runtime darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um mit einem Pushvorgang ein IoT Edge-Modul zur Ausführung auf dem Gerät zu übertragen, ohne Änderungen an dem Gerät selbst vornehmen zu müssen. In diesem Fall erstellt das mit einem Pushvorgang übertragene Modul Umgebungsdaten, die Sie für die Tutorials verwenden können. 

Öffnen Sie erneut eine Eingabeaufforderung auf dem Computer, auf dem das simulierte Gerät ausgeführt wird. Vergewissern Sie sich, dass das über die Cloud bereitgestellte Modul auf dem IoT Edge-Gerät ausgeführt wird:

```cmd
sudo docker ps
```

![Anzeigen von drei Modulen auf dem Gerät](./media/tutorial-simulate-device-linux/docker-ps2.png)

Zeigen Sie die vom tempSensor-Modul an die Cloud gesendeten Nachrichten an:

```cmd
sudo docker logs -f tempSensor
```

![Anzeigen der Daten von Ihrem Modul](./media/tutorial-simulate-device-linux/docker-logs.png)

Mithilfe des [IoT Hub-Explorer-Tools][lnk-iothub-explorer] können Sie auch die vom Gerät gesendete Telemetrie anzeigen. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein neues IoT Edge-Gerät erstellt und die Azure IoT Edge-Cloudschnittstelle zum Bereitstellen von Code auf dem Gerät verwendet. Sie verfügen nun über ein simuliertes Gerät, das Rohdaten zu seiner Umgebung generiert. 

Dieses Tutorial bildet die Voraussetzung für alle anderen IoT Edge-Tutorials. Sie können mit einem der anderen Tutorials fortfahren, um zu erfahren, wie Azure IoT Edge Ihnen beim Umwandeln dieser Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Entwickeln und Bereitstellen von C#-Code als Modul](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/tutorial-install-iot-edge/view-module.png
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-docker-mac]: https://docs.docker.com/docker-for-mac/install/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
