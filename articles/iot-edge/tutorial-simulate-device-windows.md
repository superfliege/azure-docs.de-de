---
title: Simulieren von Azure IoT Edge unter Windows | Microsoft-Dokumentation
description: "Installieren der Azure IoT Edge-Laufzeit auf einem simulierten Gerät unter Windows und Bereitstellen des ersten Moduls"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 08c501b9132bb21f47f099725d1fad5556befb4c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter Windows – Vorschau

Mit Azure IoT Edge werden die Vorteile der Cloud auf Ihre IoT-Geräte (Internet of Things, Internet der Dinge) übertragen. Dieses Tutorial führt Sie durch die Erstellung eines simulierten IoT Edge-Geräts, das Sensordaten generiert. Folgendes wird vermittelt:

![Aufbau des Tutorials][2]

Das simulierte Gerät, das Sie in diesem Tutorial erstellen, dient zur Überwachung einer Windturbine und generiert Daten zu Temperatur, Luftfeuchtigkeit und Druck. Sie sind an diesen Daten interessiert, da die Turbinen je nach Wetterbedingungen mit unterschiedlicher Effizienz arbeiten. Die anderen Tutorials zu Azure IoT Edge bauen darauf auf und erläutern die Bereitstellung von Modulen, mit denen die Daten hinsichtlich geschäftlicher Erkenntnisse analysiert werden. 

## <a name="prerequisites"></a>Voraussetzungen

Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer mit Windows zum Simulieren eines IoT-Geräts (Internet of Things, Internet der Dinge) verwenden. 

>[!TIP]
>Wenn Sie Windows auf einem virtuellen Computer ausführen, aktivieren Sie die [geschachtelte Virtualisierung][lnk-nested], und weisen Sie mindestens 2 GB Speicher zu. 

1. Stellen Sie sicher, dass Sie eine unterstützte Windows-Version verwenden:
   * Windows 10 
   * Windows Server
2. Installieren Sie [Docker für Windows][lnk-docker], und stellen Sie sicher, dass dieses Programm ausgeführt wird.
3. Installieren Sie [Python 2.7 unter Windows][lnk-python], und vergewissern Sie sich, dass Sie den Pip-Befehl verwenden können.
4. Führen Sie den folgenden Befehl aus, um das IoT Edge-Steuerskript herunterzuladen.

   ```
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> In Azure IoT Edge können entweder Windows-Container oder Linux-Container ausgeführt werden. Zur Verwendung von Windows-Containern muss Folgendes ausgeführt werden:
>    * Windows 10 Fall Creators Update oder
>    * Windows Server 1709 (Build 16299) oder
>    * Windows IoT Core (Build 16299) auf einem X64-basierten Gerät
>
> Im Fall von Windows IoT Core folgen Sie den Anweisungen unter [Installieren der IoT Edge-Laufzeit unter Windows IoT Core][lnk-install-iotcore]. Andernfalls [konfigurieren Sie einfach Docker für die Verwendung von Windows-Containern][lnk-docker-containers], und überprüfen Sie optional die Voraussetzungen mit dem folgenden PowerShell-Befehl:
>    ```
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

Die IoT Edge-Laufzeit wird auf allen IoT Edge-Geräten bereitgestellt. Sie umfasst zwei Module. Erstens: Der IoT Edge-Agent erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät. Zweitens: Der IoT Edge-Hub verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. 


Führen Sie zum Installieren und Starten der IoT Edge-Laufzeit die folgenden Schritte aus:

1. Konfigurieren Sie die Laufzeit mit der Verbindungszeichenfolge für das IoT Edge-Gerät aus dem vorherigen Abschnitt.

   ```
   iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Starten Sie die Laufzeit.

   ```
   iotedgectl start
   ```

1. Überprüfen Sie Docker, um festzustellen, ob der IoT Edge-Agent als Modul ausgeführt wird.

   ```
   docker ps
   ```

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

Verwalten Sie Ihr Azure IoT Edge-Gerät über die Cloud, um ein Modul bereitzustellen, das Telemetriedaten an IoT Hub sendet.
![Registrieren eines Geräts][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Anzeigen generierter Daten

Mit dieser Schnellstartanleitung haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Laufzeit darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um mit einem Pushvorgang ein IoT Edge-Modul zur Ausführung auf dem Gerät zu übertragen, ohne Änderungen an dem Gerät selbst vornehmen zu müssen. In diesem Fall erstellt das mit einem Pushvorgang übertragene Modul Umgebungsdaten, die Sie für die Tutorials verwenden können. 

Zeigen Sie die vom tempSensor-Modul gesendeten Nachrichten an:

```cmd/sh
sudo docker logs -f tempSensor
```

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