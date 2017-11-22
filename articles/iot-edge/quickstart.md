---
title: "Schnellstartanleitung für Azure IoT Edge + Windows | Microsoft-Dokumentation"
description: "Ausprobieren von Azure IoT Edge durch Ausführen von Analysen für ein simuliertes Edgegerät"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: e10b5dba6f91c97a5c6b71aee76eef062a8be82c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Schnellstartanleitung: Bereitstellen des ersten IoT Edge-Moduls aus dem Azure-Portal auf einem Windows-Gerät – Vorschau

Bei dieser Schnellstartanleitung verwenden Sie die Azure IoT Edge-Cloudschnittstelle zur Remotebereitstellung von vorgefertigtem Code auf einem IoT Edge-Gerät. Für diese Aufgabe verwenden Sie zunächst Ihr Windows-Gerät, um ein IoT Edge-Gerät zu simulieren. Anschließend können Sie darauf ein Modul bereitstellen.

Wenn Sie kein aktives Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][lnk-account] erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Bei diesem Tutorial wird davon ausgegangen, dass Sie einen Computer oder virtuellen Computer mit Windows zum Simulieren eines IoT-Geräts (Internet of Things, Internet der Dinge) verwenden. Wenn Sie Windows auf einem virtuellen Computer ausführen, aktivieren Sie die [geschachtelte Virtualisierung][lnk-nested], und weisen Sie mindestens 2 GB Speicher zu. 

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
> Im Fall von Windows IoT Core folgen Sie den Anweisungen unter [Installieren der IoT Edge-Laufzeit unter Windows IoT Core] [lnk-install-iotcore]. Andernfalls [konfigurieren Sie einfach Docker für die Verwendung von Windows-Containern][lnk-docker-containers], und überprüfen Sie optional die Voraussetzungen mit dem folgenden PowerShell-Befehl:
>    ```
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```

## <a name="create-an-iot-hub-with-azure-cli"></a>Erstellen eines IoT Hubs mit der Azure-Befehlszeilenschnittstelle

Erstellen Sie einen IoT Hub in Ihrem Azure-Abonnement. Der kostenlose IoT Hub kann für diese Schnellstartanleitung verwendet werden. Wenn Sie IoT Hub schon früher verwendet und bereits einen kostenlosen Hub erstellt haben, können Sie diesen Abschnitt überspringen und mit [Registrieren eines IoT Edge-Geräts][anchor-register] fortfahren. Jedes Abonnement kann nur über einen kostenlosen IoT Hub verfügen. 

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an. 
1. Wählen Sie die Schaltfläche **Cloud Shell** aus. 

   ![Schaltfläche „Cloud Shell“][1]

1. Erstellen Sie eine Ressourcengruppe. Mit dem folgenden Code wird eine Ressourcengruppe namens **IoTEdge** in der Region **West US** erstellt:

   ```azurecli
   az group create --name IoTEdge --location westus
   ```

1. Erstellen Sie einen IoT Hub in Ihrer neuen Ressourcengruppe. Mit dem folgenden Code wird ein kostenloser **F1**-Hub namens **MyIotHub** in der Ressourcengruppe **IoTEdge** erstellt:

   ```azurecli
   az iot hub create --resource-group IoTEdge --name MyIotHub --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Registrieren eines IoT Edge-Geräts

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>Konfigurieren der IoT Edge-Laufzeit

Die IoT Edge-Laufzeit wird auf allen IoT Edge-Geräten bereitgestellt. Sie umfasst zwei Module. Erstens: Der IoT Edge-Agent erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät. Zweitens: Der IoT Edge-Hub verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. 

Konfigurieren Sie die Laufzeit mit der Verbindungszeichenfolge für das IoT Edge-Gerät aus dem vorherigen Abschnitt.

```
iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Starten Sie die Laufzeit.

```
iotedgectl start
```

Überprüfen Sie Docker, um festzustellen, ob der IoT Edge-Agent als Modul ausgeführt wird.

```
docker ps
```

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Anzeigen generierter Daten

Mit dieser Schnellstartanleitung haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Laufzeit darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um mit einem Pushvorgang ein IoT Edge-Modul zur Ausführung auf dem Gerät zu übertragen, ohne Änderungen an dem Gerät selbst vornehmen zu müssen. In diesem Fall erstellt das mit einem Pushvorgang übertragene Modul Umgebungsdaten, die Sie für die Tutorials verwenden können. 

Zeigen Sie die vom tempSensor-Modul gesendeten Nachrichten an:

```cmd/sh
docker logs -f tempSensor
```

Mithilfe des [IoT Hub-Explorer-Tools][lnk-iothub-explorer] können Sie auch die vom Gerät gesendete Telemetrie anzeigen. 
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den erstellten IoT Hub nicht mehr benötigen, können Sie mit dem Befehl [az iot hub delete][lnk-delete] die Ressource und alle damit verbundenen Geräte entfernen:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie Sie ein IoT Edge-Modul auf einem IoT Edge-Gerät bereitstellen. Versuchen Sie nun, verschiedene Typen von Azure-Diensten als Module bereitzustellen, sodass Sie Daten auf Edge-Ebene analysieren können. 

* [Bereitstellen einer Azure-Funktion als Modul](tutorial-deploy-function.md)
* [Bereitstellen von Azure Stream Analytics als Modul](tutorial-deploy-stream-analytics.md)
* [Bereitstellen eines eigenen Codes als Modul](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-python]: https://www.python.org/downloads/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
