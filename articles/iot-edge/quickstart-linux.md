---
title: "Schnellstart für Azure IoT Edge + Linux | Microsoft-Dokumentation"
description: "Testen von Azure IoT Edge durch Ausführen von Analysen für ein simuliertes Edgegerät"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: fb93efcf00cb7b165c497d7ef38685f80bce84c0
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-linux-device---preview"></a>Schnellstart: Bereitstellen des ersten IoT Edge-Moduls über das Azure-Portal auf einem Linux-Gerät – Vorschau

Mit Azure IoT Edge werden die Vorteile der Cloud auf Ihre IoT-Geräte übertragen. In diesem Thema erfahren Sie, wie Sie die Cloudschnittstelle zur Remotebereitstellung von vordefiniertem Code auf einem IoT Edge-Gerät verwenden.

Wenn Sie über kein aktives Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][lnk-account] erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Simulieren Sie für diese Aufgabe ein IoT-Gerät mit Ihrem Computer oder einem virtuellen Computer. Zur erfolgreichen Bereitstellung eines IoT Edge-Geräts sind folgende Dienste erforderlich:

- [Installieren Sie Docker unter Linux][lnk-docker-ubuntu], und stellen Sie sicher, dass dieses Programm ausgeführt wird. 
- In den meisten Linux-Distributionen, einschließlich Ubuntu, ist Python 2.7 bereits installiert. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob pip installiert ist: `sudo apt-get install python-pip`.

## <a name="create-an-iot-hub-with-azure-cli"></a>Erstellen eines IoT Hubs mit der Azure-Befehlszeilenschnittstelle

Erstellen Sie einen IoT Hub in Ihrem Azure-Abonnement. Der kostenlose IoT Hub kann für diesen Schnellstart verwendet werden. Wenn Sie IoT Hub schon früher verwendet und bereits einen kostenlosen Hub erstellt haben, können Sie diesen Abschnitt überspringen und mit [Registrieren eines IoT Edge-Geräts][anchor-register] fortfahren. Jedes Abonnement kann nur über einen kostenlosen IoT Hub verfügen. 

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

Erstellen Sie eine Geräteidentität für das simulierte Gerät, sodass es mit dem IoT Hub kommunizieren kann. Da IoT Edge-Geräte sich von typischen IoT-Geräten unterscheiden und auf unterschiedliche Weise verwaltet werden können, deklarieren Sie dieses Gerät von Anfang an als IoT Edge-Gerät. 

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.
1. Wählen Sie **IoT Edge (Vorschau)** aus.
1. Wählen Sie **Add IoT Edge device** (IoT Edge-Gerät hinzufügen) aus.
1. Weisen Sie dem simulierten Gerät eine eindeutige Geräte-ID zu.
1. Wählen Sie **Speichern** aus, um das Gerät hinzuzufügen.
1. Wählen Sie das neue Gerät in der Liste der Geräte aus. 
1. Kopieren Sie den Wert für **Verbindungszeichenfolge – Primärschlüssel**, und speichern Sie ihn. Sie verwenden diesen Wert, um im nächsten Abschnitt die IoT Edge-Runtime zu konfigurieren. 

## <a name="install-and-start-the-iot-edge-runtime"></a>Installieren und Starten der IoT Edge-Runtime

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Sie umfasst zwei Module. Erstens: Der IoT Edge-Agent erleichtert die Bereitstellung und Überwachung von Modulen auf dem IoT Edge-Gerät. Zweitens: Der IoT Edge-Hub verwaltet die Kommunikation zwischen Modulen auf dem IoT Edge-Gerät sowie zwischen dem Gerät und IoT Hub. 

Laden Sie auf dem Computer, auf dem das IoT Edge-Gerät ausgeführt wird, das IoT Edge-Steuerskript herunter:
```python
sudo pip install -U azure-iot-edge-runtime-ctl
```

Konfigurieren Sie die Runtime mit der Verbindungszeichenfolge für das IoT Edge-Gerät aus dem vorherigen Abschnitt:
```python
sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Starten Sie die Runtime:
```python
sudo iotedgectl start
```

Überprüfen Sie Docker, um festzustellen, ob der IoT Edge-Agent als Modul ausgeführt wird:
```python
sudo docker ps
```

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Anzeigen generierter Daten

In diesem Schnellstart haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Runtime darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um mit einem Pushvorgang ein IoT Edge-Modul zur Ausführung auf dem Gerät zu übertragen, ohne Änderungen an dem Gerät selbst vornehmen zu müssen. In diesem Fall erstellt das mit einem Pushvorgang übertragene Modul Umgebungsdaten, die Sie für die Tutorials verwenden können. 

Zeigen Sie die vom tempSensor-Modul gesendeten Nachrichten an:

```cmd/sh
sudo docker logs -f tempSensor
```

Mithilfe des [IoT Hub-Explorer-Tools][lnk-iothub-explorer] können Sie auch die vom Gerät gesendete Telemetrie anzeigen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den erstellten IoT Hub nicht mehr benötigen, können Sie mit dem Befehl [az iot hub delete][lnk-delete] die Ressource und alle damit verbundenen Geräte entfernen:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie Sie ein IoT Edge-Modul auf einem IoT Edge-Gerät bereitstellen. Versuchen Sie nun, verschiedene Typen von Azure-Diensten als Module bereitzustellen, sodass Sie Daten auf Edge-Ebene analysieren können. 

* [Bereitstellen eines eigenen Codes als Modul](tutorial-csharp-module.md)
* [Bereitstellen einer Azure-Funktion als Modul](tutorial-deploy-function.md)
* [Bereitstellen von Azure Stream Analytics als Modul](tutorial-deploy-stream-analytics.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
