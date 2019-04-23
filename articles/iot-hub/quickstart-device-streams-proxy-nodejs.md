---
title: 'Azure IoT Hub-Gerätestreams: Node.js-Schnellstartanleitung für SSH/RDP (Vorschauversion) | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung führen Sie eine Node.js-Beispielanwendung aus, die als Proxy fungiert, um SSH-/RDP-Szenarien über IoT Hub-Gerätestreams zu ermöglichen.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: a459473e04f9cbf3b11b75f3b9dbea2732455084
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59005429"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-nodejs-proxy-application-preview"></a>Schnellstart: SSH/RDP über IoT Hub-Gerätestreams unter Verwendung einer Node.js-Proxyanwendung (Vorschauversion)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub unterstützt derzeit Gerätestreams als [Previewfunktion](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Über [IoT Hub-Gerätestreams](./iot-hub-device-streams-overview.md) können Dienst- und Geräteanwendungen sicher und firewallfreundlich kommunizieren. In dieser Schnellstartanleitung erfahren Sie, wie Sie eine serverseitig ausgeführte Node.js-Proxyanwendung ausführen, sodass SSH- und RDP-Datenverkehr über einen Gerätestream an das Gerät gesendet werden kann. Eine entsprechende Übersicht finden Sie [hier](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp). Während der Public Preview-Phase unterstützt das Node.js SDK nur dienstseitige Gerätestreams. Aus diesem Grund wird in dieser Schnellstartanleitung nur das Ausführen des lokalen Dienstproxys erläutert. Es empfiehlt sich, auch einen entsprechenden lokalen Geräteproxy auszuführen (verfügbar in der Schnellstartanleitung für [C](./quickstart-device-streams-proxy-c.md) oder [C#](./quickstart-device-streams-proxy-csharp.md)).

Als Erstes wird die Einrichtung für SSH (unter Verwendung des Ports 22) beschrieben. Danach erfahren Sie, wie Sie das Setup für RDP (Port 3389) ändern. Gerätestreams sind anwendungs- und protokollunabhängig. Das gleiche Beispiel kann daher für andere Arten von Anwendungsdatenverkehr von Clients/Servern angepasst werden (in der Regel durch Ändern des Kommunikationsports).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Die Vorschau der Gerätestreams wird derzeit nur für IoT Hubs unterstützt, die in folgenden Regionen erstellt werden:

  - **USA (Mitte)**
  - **USA, Mitte (EUAP)**

Auf Ihrem Entwicklungscomputer muss sich mindestens Node.js v4.x.x befinden, um die lokale Dienstanwendung in dieser Schnellstartanleitung ausführen zu können.

Sie können Node.js für mehrere Plattformen von [nodejs.org](https://nodejs.org) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle Node.js-Version auf Ihrem Entwicklungscomputer überprüfen:

```
node --version
```

Führen Sie den folgenden Befehl aus, um Ihrer Cloud Shell-Instanz die Microsoft Azure IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle hinzuzufügen. Die IoT-Erweiterung fügt der Azure-Befehlszeilenschnittstelle spezifische Befehle für IoT Hub, IoT Edge und IoT Device Provisioning Service (DPS) hinzu.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Laden Sie das Node.js-Beispielprojekt von https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip herunter, und extrahieren Sie das ZIP-Archiv (falls Sie dies nicht bereits getan haben).

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Wenn Sie das vorherige Tutorial [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Node.js)](quickstart-send-telemetry-node.md) absolviert haben, können Sie diesen Schritt überspringen.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrieren eines Geräts

Wenn Sie das vorherige Tutorial [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Node.js)](quickstart-send-telemetry-node.md) absolviert haben, können Sie diesen Schritt überspringen.

Ein Gerät muss bei Ihrer IoT Hub-Instanz registriert sein, um eine Verbindung herstellen zu können. In dieser Schnellstartanleitung verwenden Sie Azure Cloud Shell, um ein simuliertes Gerät zu registrieren.

1. Führen Sie in Azure Cloud Shell den folgenden Befehl aus, um die Geräteidentität zu erstellen.

   **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben.

   **MyDevice**: Der für das registrierte Gerät angegebene Name. Verwenden Sie „MyDevice“ wie gezeigt. Wenn Sie für Ihr Gerät einen anderen Namen wählen, müssen Sie diesen innerhalb des gesamten Artikels verwenden und den Gerätenamen in den Beispielanwendungen aktualisieren, bevor Sie sie ausführen.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Darüber hinaus benötigen Sie eine _Dienstverbindungszeichenfolge_, damit die Back-End-Anwendung eine Verbindung mit Ihrer IoT Hub-Instanz herstellen und die Nachrichten abrufen kann. Der folgende Befehl ruft die Dienstverbindungszeichenfolge für Ihre IoT Hub-Instanz ab:

    **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Notieren Sie sich den zurückgegebenen Wert, der in etwa wie folgt aussieht:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="ssh-to-a-device-via-device-streams"></a>Herstellen einer SSH-Verbindung mit einem Gerät über Gerätestreams

### <a name="run-the-device-local-proxy"></a>Ausführen des lokalen Geräteproxys

Wie bereits erwähnt, unterstützt das IoT Hub Node.js SDK während der Public Preview-Phase nur dienstseitige Gerätestreams. Verwenden Sie als lokale Geräteanwendung die entsprechenden Geräteproxyprogramme aus der Anleitung für [C](./quickstart-device-streams-proxy-c.md) oder [C#](./quickstart-device-streams-proxy-csharp.md). Vergewissern Sie sich, dass der lokale Geräteproxy ausgeführt wird, bevor Sie mit dem nächsten Schritt fortfahren.

### <a name="run-the-service-local-proxy"></a>Ausführen des lokalen Dienstproxys

Nachdem Sie sich vergewissert haben, dass der [lokale Geräteproxy](#run-the-device-local-proxy) ausgeführt wird, gehen Sie wie folgt vor, um den in Node.js geschriebenen lokalen Dienstproxy auszuführen.

- Geben Sie Ihre Dienstanmeldeinformationen, die ID des Zielgeräts, auf dem der SSH-Daemon ausgeführt wird, und die Portnummer für den auf dem Gerät ausgeführten Proxy als Umgebungsvariablen ein.
  ```
  # In Linux
  export IOTHUB_CONNECTION_STRING="<provide_your_service_connection_string>"
  export STREAMING_TARGET_DEVICE="MyDevice"
  export PROXY_PORT=2222

  # In Windows
  SET IOTHUB_CONNECTION_STRING=<provide_your_service_connection_string>
  SET STREAMING_TARGET_DEVICE=MyDevice
  SET PROXY_PORT=2222
  ```
  Ändern Sie die Werte oben basierend auf Ihrer Geräte-ID und Verbindungszeichenfolge.

- Navigieren Sie in Ihrem entzippten Projektordner zu `Quickstarts/device-streams-service`, und führen Sie den lokalen Dienstproxy aus.
  ```
  cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service

  # Install the preview service SDK, and other dependencies
  npm install azure-iothub@streams-preview
  npm install

  # Run the service-local proxy application
  node proxy.js
  ```

### <a name="ssh-to-your-device-via-device-streams"></a>Herstellen einer SSH-Verbindung mit Ihrem Gerät über Gerätestreams

Verwenden Sie unter Linux `ssh $USER@localhost -p 2222` in einem Terminal, um SSH auszuführen. Verwenden Sie unter Windows Ihren bevorzugten SSH-Client (beispielsweise PuTTY).

Im Anschluss sehen Sie die Konsolenausgabe des lokalen Diensts nach Einrichtung der SSH-Sitzung. (Der lokale Dienstproxy lauscht am Port 2222.) ![Alternativer Text](./media/quickstart-device-streams-proxy-nodejs/service-console-output.PNG "SSH-Terminalausgabe")

Im Anschluss sehen Sie die Konsolenausgabe des SSH-Clientprogramms. (Der SSH-Client kommuniziert mit dem SSH-Daemon, indem er eine Verbindung mit dem Port 22 herstellt, an dem der lokale Dienstproxy lauscht.) ![Alternativer Text](./media/quickstart-device-streams-proxy-nodejs/ssh-console-output.PNG "Ausgabe des SSH-Clients")

### <a name="rdp-to-your-device-via-device-streams"></a>Herstellen einer RDP-Verbindung mit Ihrem Gerät über Gerätestreams

Stellen Sie nun mithilfe Ihres RDP-Clientprogramms eine Verbindung mit dem Dienstproxy am Port 2222 her. (Hierbei handelt es sich um einen verfügbaren Port, den Sie zuvor willkürlich ausgewählt haben.)

> [!NOTE]
> Vergewissern Sie sich, dass Ihr Geräteproxy ordnungsgemäß für RDP sowie mit dem RDP-Port 3389 konfiguriert ist.

![Alternativer Text](./media/quickstart-device-streams-proxy-nodejs/rdp-screen-capture.PNG "RDP-Clientverbindung mit lokalem Dienstproxy")

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen IoT-Hub eingerichtet, ein Gerät registriert und ein Dienstproxyprogramm bereitgestellt, um RDP- und SSH-Verbindungen mit einem IoT-Gerät zu ermöglichen. RDP- und SSH-Datenverkehr wird über einen Gerätestream über IoT Hub getunnelt. Dadurch ist keine direkte Verbindung mit dem Gerät erforderlich.

Weitere Informationen zu Gerätestreams finden Sie hier:

> [!div class="nextstepaction"]
> [IoT Hub-Gerätestreams (Vorschau)](./iot-hub-device-streams-overview.md)
