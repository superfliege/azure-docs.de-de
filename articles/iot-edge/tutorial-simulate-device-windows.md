---
title: Simulieren von Azure IoT Edge unter Windows | Microsoft-Dokumentation
description: Installieren der Azure IoT Edge-Laufzeit auf einem simulierten Gerät unter Windows und Bereitstellen des ersten Moduls
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/16/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 213a0e7cebda6a8b89ef460799cbec477b487a64
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34166113"
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter Windows – Vorschau

Mit Azure IoT Edge können Sie Analysen und die Datenverarbeitung auf Ihren Geräten durchführen, statt alle Daten per Push in die Cloud zu übertragen. Die IoT Edge-Tutorials veranschaulichen, wie Sie verschiedene Modultypen bereitstellen, die aus Azure-Diensten oder benutzerdefiniertem Code entwickelt wurden. Zunächst jedoch benötigen Sie ein Testgerät. 

In diesem Tutorial lernen Sie Folgendes:

1. Erstellen eines IoT Hubs
2. Registrieren eines IoT Edge-Geräts
3. Starten der IoT Edge-Runtime
4. Bereitstellen eines Moduls

![Aufbau des Tutorials][2]

Das simulierte Gerät, das Sie in diesem Tutorial erstellen, dient zur Überwachung einer Windturbine und generiert Daten zu Temperatur, Luftfeuchtigkeit und Druck. Sie sind an diesen Daten interessiert, da die Turbinen je nach Wetterbedingungen mit unterschiedlicher Effizienz arbeiten. Die anderen Tutorials zu Azure IoT Edge bauen darauf auf und erläutern die Bereitstellung von Modulen, mit denen die Daten analysiert werden, um geschäftsrelevante Erkenntnisse zu gewinnen. 

## <a name="prerequisites"></a>Voraussetzungen

Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer mit Windows zum Simulieren eines IoT-Geräts (Internet of Things, Internet der Dinge) verwenden. 

>[!TIP]
>Wenn Sie Windows auf einem virtuellen Computer ausführen, aktivieren Sie die [geschachtelte Virtualisierung][lnk-nested], und weisen Sie mindestens 2 GB Speicher zu. 

1. Stellen Sie sicher, dass Sie eine unterstützte Windows-Version verwenden:
   * Windows 10 
   * Windows Server
2. Installieren Sie [Docker für Windows][lnk-docker], und stellen Sie sicher, dass dieses Programm ausgeführt wird.
3. Installieren Sie [Python unter Windows][lnk-python], und vergewissern Sie sich, dass Sie den Befehl „pip“ verwenden können. Dieses Tutorial wurde mit Python-Versionen ab 2.7.9 und ab 3.5.4 getestet.  
4. Führen Sie den folgenden Befehl aus, um das IoT Edge-Steuerskript herunterzuladen.

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> In Azure IoT Edge können entweder Windows-Container oder Linux-Container ausgeführt werden. Wenn Sie eine der folgenden Windows-Versionen ausführen, können Sie Windows-Container verwenden:
>    * Windows 10 Fall Creators Update
>    * Windows Server 1709 (Build 16299)
>    * Windows IoT Core (Build 16299) auf einem X64-basierten Gerät
>
> Im Fall von Windows IoT Core folgen Sie den Anweisungen unter [Installieren der IoT Edge-Laufzeit unter Windows IoT Core][lnk-install-iotcore]. Andernfalls [konfigurieren Sie Docker zur Verwendung von Windows-Containern][lnk-docker-containers]. Verwenden Sie den folgenden Befehl zum Überprüfen Ihrer Voraussetzungen:
>    ```powershell
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```


## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Starten Sie das Tutorial, indem Sie Ihren IoT Hub erstellen.
![Erstellen des IoT Hubs][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registrieren eines IoT Edge-Geräts

Registrieren Sie ein IoT Edge-Gerät bei Ihrem neu erstellten IoT Hub.
![Registrieren eines Geräts][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>Konfigurieren der IoT Edge-Laufzeit

Installieren und starten Sie die Azure IoT Edge-Laufzeit auf Ihrem Gerät. 
![Registrieren eines Geräts][5]

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie umfasst zwei Module. Der **IoT Edge-Agent** erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät. Der **IoT Edge-Hub** verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. Wenn Sie die Runtime auf dem neuen Gerät konfigurieren, wird zunächst nur der IoT Edge-Agent gestartet. Der IoT Edge-Hub wird später beim Bereitstellen eines Moduls verwendet. 


Konfigurieren Sie die Runtime mit der Verbindungszeichenfolge für das IoT Edge-Gerät aus dem vorherigen Abschnitt.

```cmd
iotedgectl setup --connection-string "{device connection string}" --nopass
```

Starten Sie die Runtime.

```cmd
iotedgectl start
```

Überprüfen Sie Docker, um festzustellen, ob der IoT Edge-Agent als Modul ausgeführt wird.

```cmd
docker ps
```

![Edge-Agent in Docker](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

Verwalten Sie Ihr Azure IoT Edge-Gerät über die Cloud, um ein Modul bereitzustellen, das Telemetriedaten an IoT Hub sendet.
![Registrieren eines Geräts][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Anzeigen generierter Daten

In diesem Tutorial haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Runtime darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um mit einem Pushvorgang ein IoT Edge-Modul zur Ausführung auf dem Gerät zu übertragen, ohne Änderungen an dem Gerät selbst vornehmen zu müssen. In diesem Fall erstellt das mit einem Pushvorgang übertragene Modul Umgebungsdaten, die Sie für die Tutorials verwenden können. 

Öffnen Sie erneut eine Eingabeaufforderung auf dem Computer, auf dem das simulierte Gerät ausgeführt wird. Vergewissern Sie sich, dass das über die Cloud bereitgestellte Modul auf dem IoT Edge-Gerät ausgeführt wird. 

```cmd
docker ps
```

![Anzeigen von drei Modulen auf dem Gerät](./media/tutorial-simulate-device-windows/docker-ps2.png)

Zeigen Sie die vom tempSensor-Modul an die Cloud gesendeten Nachrichten an. 

```cmd
docker logs -f tempSensor
```

![Anzeigen der Daten von Ihrem Modul](./media/tutorial-simulate-device-windows/docker-logs.png)

Mithilfe des [IoT Hub-Explorer-Tools][lnk-iothub-explorer] können Sie auch die vom Gerät gesendete Telemetrie anzeigen. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein neues IoT Edge-Gerät erstellt und die Azure IoT Edge-Cloudschnittstelle zum Bereitstellen von Code auf dem Gerät verwendet. Sie verfügen nun über ein simuliertes Gerät, das Rohdaten zu seiner Umgebung generiert. 

Dieses Tutorial bildet die Voraussetzung für alle anderen IoT Edge-Tutorials. Sie können mit einem der anderen Tutorials fortfahren, um zu erfahren, wie Azure IoT Edge Ihnen beim Umwandeln dieser Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Entwickeln und Bereitstellen von C#-Code als Modul](tutorial-csharp-module.md)

<!-- Images -->
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md