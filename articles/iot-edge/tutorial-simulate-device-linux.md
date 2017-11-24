---
title: Simulieren von Azure IoT Edge unter Linux | Microsoft-Dokumentation
description: "Installieren der Azure IoT Edge-Runtime auf einem simulierten Gerät unter Linux und Bereitstellen des ersten Moduls"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 041919fd729880d429e08d8942f8d1ee087ccf61
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux---preview"></a>Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter Linux – Vorschau

Mit Azure IoT Edge können Sie Analyse und Datenbearbeitung auf Ihren Geräten durchführen, statt alle Daten per Push in die Cloud zu übertragen. Die IoT Edge-Tutorials veranschaulichen, wie Sie verschiedene Modultypen bereitstellen, die aus Azure-Diensten oder benutzerdefiniertem Code entwickelt wurden. Zunächst jedoch benötigen Sie ein Testgerät. 

Dieses Tutorial leitet Sie durch die Erstellung eines simulierten IoT Edge-Geräts und die nachfolgende Bereitstellung eines Moduls, das Sensordaten erzeugt. Folgendes wird vermittelt:

![Aufbau des Tutorials][2]

Das simulierte Gerät, das Sie in diesem Tutorial erstellen, ist ein Überwachungssensor, der Temperatur-, Luftfeuchtigkeits- und Druckdaten generiert. Die anderen Tutorials zu Azure IoT Edge bauen darauf auf und erläutern die Bereitstellung von Modulen, mit denen die Daten analysiert werden, um geschäftsrelevante Erkenntnisse zu gewinnen. 

## <a name="prerequisites"></a>Voraussetzungen

Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder einen virtuellen Computer mit Linux zum Simulieren eines Geräts im Internet der Dinge verwenden. Zur erfolgreichen Bereitstellung eines IoT Edge-Geräts sind folgende Dienste erforderlich:

- [Installieren Sie Docker für Linux][lnk-docker-ubuntu], und stellen Sie sicher, dass das Programm ausgeführt wird. 
- In den meisten Linux-Distributionen, einschließlich Ubuntu, ist Python 2.7 bereits installiert. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob pip installiert ist: `sudo apt-get install python-pip`.

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

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie umfasst zwei Module. Erstens: Der IoT Edge-Agent erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät. Zweitens: Der IoT Edge-Hub verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. 

Führen Sie zum Installieren und Starten der IoT Edge-Runtime die folgenden Schritte aus:

1. Laden Sie auf dem Computer, auf dem das IoT Edge-Gerät ausgeführt wird, das IoT Edge-Steuerskript herunter.

   ```
   sudo pip install -U azure-iot-edge-runtime-ctl
   ```

1. Konfigurieren Sie die Runtime mit der Verbindungszeichenfolge für das IoT Edge-Gerät aus dem vorherigen Abschnitt.

   ```
   sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Starten Sie die Runtime.

   ```
   sudo iotedgectl start
   ```

1. Überprüfen Sie Docker, um festzustellen, ob der IoT Edge-Agent als Modul ausgeführt wird.

   ```
   sudo docker ps
   ```

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

Verwalten Sie Ihr Azure IoT Edge-Gerät über die Cloud, um ein Modul bereitzustellen, das Telemetriedaten an IoT Hub sendet.
![Registrieren eines Geräts][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Anzeigen generierter Daten

Mit dieser Schnellstartanleitung haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Runtime darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um mit einem Pushvorgang ein IoT Edge-Modul zur Ausführung auf dem Gerät zu übertragen, ohne Änderungen an dem Gerät selbst vornehmen zu müssen. In diesem Fall erstellt das mit einem Pushvorgang übertragene Modul Umgebungsdaten, die Sie für die Tutorials verwenden können. 

Zeigen Sie die vom tempSensor-Modul gesendeten Nachrichten an:

```cmd/sh
docker logs -f tempSensor
```

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
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
