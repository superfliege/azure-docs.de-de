---
title: 'Azure IoT Hub-Gerätestreams: C-Schnellstartanleitung für SSH/RDP (Vorschauversion) | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung führen Sie zwei C#-Beispielanwendungen aus, die SSH-/RDP-Szenarien über einen IoT Hub-Gerätestream ermöglichen.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 514c2e0ea1ef33406c6633064434239d8bdd0e3f
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2019
ms.locfileid: "65832971"
---
# <a name="quickstart-sshrdp-over-an-iot-hub-device-stream-using-a-c-proxy-application-preview"></a>Schnellstart: SSH/RDP über einen IoT Hub-Gerätestream unter Verwendung einer C#-Proxyanwendung (Vorschauversion)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub unterstützt derzeit Gerätestreams als [Previewfunktion](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Über [IoT Hub-Gerätestreams](iot-hub-device-streams-overview.md) können Dienst- und Geräteanwendungen sicher und firewallfreundlich kommunizieren. In dieser Schnellstartanleitung werden zwei C#-Programme verwendet, die es ermöglichen, Anwendungsdatenverkehr von Clients/Servern (etwa SSH und RDP) über einen per IoT Hub eingerichteten Gerätestream zu senden. Eine Übersicht über das Setup finden Sie in den [Beispielen für lokale Proxys für SSH und RDP](iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp).

Als Erstes wird die Einrichtung für SSH (unter Verwendung des Ports 22) beschrieben. Danach erfahren Sie, wie Sie den eingerichteten Port für RDP ändern. Gerätestreams sind anwendungs- und protokollunabhängig. Das gleiche Beispiel kann daher für andere Arten von Anwendungsdatenverkehr angepasst werden. In der Regel wird dabei der Kommunikationsport in den von der gewünschten Anwendung verwendeten Port geändert.

## <a name="how-it-works"></a>So funktioniert's

Die folgende Abbildung zeigt, wie die lokalen Geräte- und Dienstproxyprogramme in diesem Beispiel eine End-to-End-Konnektivität zwischen SSH-Client und SSH-Daemon ermöglichen. Hierbei wird davon ausgegangen, dass der Daemon auf dem gleichen Gerät ausgeführt wird wie der lokale Geräteproxy.

![Setup des lokalen Proxys](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. Der lokale Dienstproxy stellt eine Verbindung mit dem IoT-Hub her und initiiert mithilfe der Geräte-ID einen Gerätestream an das Zielgerät.

2. Der lokale Geräteproxy schließt den Handshake für die Streaminitiierung ab und richtet über den IoT Hub-Streamingendpunkt einen End-to-End-Streamingtunnel mit der Dienstseite ein.

3. Der lokale Geräteproxy stellt eine Verbindung mit dem SSH-Daemon (SSHD) her, der auf dem Gerät am Port 22 lauscht. Dies ist konfigurierbar, wie im Abschnitt [Ausführen des lokalen Geräteproxys](#run-the-device-local-proxy) beschrieben.

4. Der lokale Dienstproxy wartet auf neue SSH-Verbindungen des Benutzers, indem er am angegebenen Port (in diesem Fall: Port 2222) lauscht. Dies ist ebenfalls konfigurierbar, wie im Abschnitt [Ausführen des lokalen Geräteproxys](#run-the-service-local-proxy) beschrieben. Wenn der Benutzer eine Verbindung über den SSH-Client herstellt, ermöglicht der Tunnel den Austausch von Anwendungsdatenverkehr zwischen dem SSH-Client und Dienstprogrammen.

> [!NOTE]
> Über den Stream gesendeter SSH-Datenverkehr wird nicht direkt zwischen Dienst und Gerät gesendet, sondern über den IoT Hub-Streamingendpunkt getunnelt. Weitere Informationen finden Sie im Abschnitt zu [Vorteilen von Gerätestreams](./iot-hub-device-streams-overview.md#benefits).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Die Vorschau der Gerätestreams wird derzeit nur für IoT Hubs unterstützt, die in folgenden Regionen erstellt werden:

*  **USA (Mitte)**

*  **USA, Mitte (EUAP)**

Die beiden in dieser Schnellstartanleitung ausgeführten Beispielanwendungen sind in C# geschrieben. Sie benötigen auf Ihrem Entwicklungscomputer das .NET Core SDK 2.1.0 oder höher.

Sie können das [.NET Core SDK für mehrere Plattformen](https://www.microsoft.com/net/download/all) von .NET herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle C#-Version auf Ihrem Entwicklungscomputer überprüfen:

```
dotnet --version
```

Führen Sie den folgenden Befehl aus, um Ihrer Cloud Shell-Instanz die Microsoft Azure IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle hinzuzufügen. Die IoT-Erweiterung fügt der Azure-Befehlszeilenschnittstelle spezifische Befehle für IoT Hub, IoT Edge und IoT Device Provisioning Service (DPS) hinzu.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Laden Sie das C#-Beispielprojekt von https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip herunter, und extrahieren Sie das ZIP-Archiv.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrieren eines Geräts

Ein Gerät muss bei Ihrer IoT Hub-Instanz registriert sein, um eine Verbindung herstellen zu können. In dieser Schnellstartanleitung verwenden Sie Azure Cloud Shell, um ein simuliertes Gerät zu registrieren.

1. Führen Sie in Azure Cloud Shell den folgenden Befehl aus, um die Geräteidentität zu erstellen.

   **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub wählen.

   **MyDevice**: Der für das registrierte Gerät angegebene Name. Verwenden Sie „MyDevice“ wie gezeigt. Wenn Sie für Ihr Gerät einen anderen Namen wählen, müssen Sie diesen innerhalb des gesamten Artikels verwenden und den Gerätenamen in den Beispielanwendungen aktualisieren, bevor Sie sie ausführen.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Führen Sie die folgenden Befehle in Azure Cloud Shell aus, um die _Geräteverbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen:

   **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub wählen.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Notieren Sie sich die Geräteverbindungszeichenfolge. Diese sieht wie im folgenden Beispiel aus:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Dieser Wert wird später in der Schnellstartanleitung benötigt.

3. Darüber hinaus benötigen Sie die *Dienstverbindungszeichenfolge* aus Ihrem IoT-Hub, damit die dienstseitige Anwendung eine Verbindung mit Ihrem IoT-Hub herstellen und einen Gerätestream einrichten kann. Der folgende Befehl ruft diesen Wert für Ihren IoT-Hub ab:

   **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub wählen.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Notieren Sie sich den zurückgegebenen Wert, der in etwa wie folgt aussieht:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="ssh-to-a-device-via-device-streams"></a>Herstellen einer SSH-Verbindung mit einem Gerät über Gerätestreams

In diesem Abschnitt richten Sie einen End-to-End-Stream ein, um SSH-Datenverkehr zu tunneln.

### <a name="run-the-device-local-proxy"></a>Ausführen des lokalen Geräteproxys

Navigieren Sie in Ihrem entzippten Projektordner zu `device-streams-proxy/device`. Halten Sie folgende Informationen bereit:

| Argumentname | Argumentwert |
|----------------|-----------------|
| `deviceConnectionString` | Die zuvor erstellte Geräteverbindungszeichenfolge. |
| `targetServiceHostName` | Die IP-Adresse, an der der SSH-Server lauscht. (`localhost`, wenn die gleiche IP-Adresse verwendet wird, unter der auch der lokale Geräteproxy ausgeführt wird.) |
| `targetServicePort` | Der von Ihrem Anwendungsprotokoll verwendete Port. (Für SSH ist das standardmäßig der Port 22.)  |

Kompilieren Sie den Code, und führen Sie ihn aus:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $deviceConnectionString localhost 22

# In Windows
dotnet run %deviceConnectionString% localhost 22
```

### <a name="run-the-service-local-proxy"></a>Ausführen des lokalen Dienstproxys

Navigieren Sie in Ihrem entzippten Projektordner zu `device-streams-proxy/service`. Halten Sie folgende Informationen bereit:

| Parametername | Parameterwert |
|----------------|-----------------|
| `iotHubConnectionString` | Die Dienstverbindungszeichenfolge Ihres IoT-Hubs. |
| `deviceId` | Die zuvor erstellte Geräte-ID. |
| `localPortNumber` | Ein lokaler Port für die Verbindung mit Ihrem SSH-Client. In diesem Beispiel wird der Port 2222 verwendet, er kann jedoch auf einen beliebigen anderen Wert festgelegt werden. |

Kompilieren Sie den Code, und führen Sie ihn aus:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-ssh-client"></a>Ausführen des SSH-Clients

Stellen Sie nun über Ihr SSH-Clientprogramm eine Verbindung mit dem lokalen Dienstproxy am Port 2222 her (anstatt eine Direktverbindung mit dem SSH-Daemon).

```
ssh <username>@localhost -p 2222
```

Daraufhin wird die SSH-Anmeldeaufforderung für die Eingabe Ihrer Anmeldeinformationen angezeigt.

Im Anschluss sehen Sie die dienstseitige Konsolenausgabe. (Der lokale Dienstproxy lauscht am Port 2222.)

![Ausgabe des lokalen Dienstproxys](./media/quickstart-device-streams-proxy-csharp/service-console-output.png)

Im Anschluss sehen Sie die Konsolenausgabe des lokalen Geräteproxys, der unter `IP_address:22` eine Verbindung mit dem SSH-Daemon herstellt:

![Ausgabe des lokalen Geräteproxys](./media/quickstart-device-streams-proxy-csharp/device-console-output.png)

Im Anschluss sehen Sie die Konsolenausgabe des SSH-Clientprogramms. (Der SSH-Client kommuniziert mit dem SSH-Daemon, indem er eine Verbindung mit dem Port 22 herstellt, an dem der lokale Dienstproxy lauscht.)

![Ausgabe des SSH-Clientprogramms](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="rdp-to-a-device-via-device-streams"></a>Herstellen einer RDP-Verbindung mit einem Gerät über Gerätestreams

Das Setup für RDP unterscheidet sich nur minimal vom weiter oben beschriebenen Setup für SSH. Wir müssen im Grunde nur die RDP-Ziel-IP-Adresse und den Port 3389 sowie einen RDP-Client (anstelle eines SSH-Clients) verwenden.

### <a name="run-the-device-local-proxy-rdp"></a>Ausführen des lokalen Geräteproxys (RDP)

Navigieren Sie in Ihrem entzippten Projektordner zu `device-streams-proxy/device`. Halten Sie folgende Informationen bereit:

| Argumentname | Argumentwert |
|----------------|-----------------|
| `DeviceConnectionString` | Die zuvor erstellte Geräteverbindungszeichenfolge. |
| `targetServiceHostName` | Der Hostname oder die IP-Adresse für die RDP-Serverausführung. (`localhost`, wenn die gleiche IP-Adresse verwendet wird, unter der auch der lokale Geräteproxy ausgeführt wird.) |
| `targetServicePort` | Der von Ihrem Anwendungsprotokoll verwendete Port. (Für RDP ist das standardmäßig der Port 3389.)  |

Kompilieren Sie den Code, und führen Sie ihn aus:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device

# Run the application
# In Linux/MacOS
dotnet run $DeviceConnectionString localhost 3389

# In Windows
dotnet run %DeviceConnectionString% localhost 3389
```

### <a name="run-the-service-local-proxy-rdp"></a>Ausführen des lokalen Dienstproxys (RDP)

Navigieren Sie in Ihrem entzippten Projektordner zu `device-streams-proxy/service`. Halten Sie folgende Informationen bereit:

| Parametername | Parameterwert |
|----------------|-----------------|
| `iotHubConnectionString` | Die Dienstverbindungszeichenfolge Ihres IoT-Hubs. |
| `deviceId` | Die zuvor erstellte Geräte-ID. |
| `localPortNumber` | Ein lokaler Port für die Verbindung mit Ihrem SSH-Client. In diesem Beispiel wird der Port 2222 verwendet, er kann jedoch auf einen beliebigen anderen Wert festgelegt werden. |

Kompilieren Sie den Code, und führen Sie ihn aus:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-rdp-client"></a>Ausführen des RDP-Clients

Stellen Sie nun mithilfe Ihres RDP-Clientprogramms eine Verbindung mit dem lokalen Dienstproxy am Port 2222 her. (Hierbei handelt es sich um einen verfügbaren Port, den Sie zuvor willkürlich ausgewählt haben.)

![RDP-Verbindung mit lokalem Dienstproxy](./media/quickstart-device-streams-proxy-csharp/rdp-screen-capture.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen IoT-Hub eingerichtet, ein Gerät registriert, ein lokales Geräteproxyprogramm und ein lokales Dienstproxyprogramm bereitgestellt, um einen Gerätestream über IoT Hub einzurichten, sowie die Proxys zum Tunneln von SSH oder RDP-Datenverkehr verwendet. Das gleiche Muster kann auch für andere Client-/Serverprotokolle verwendet werden, bei denen der Server auf dem Gerät ausgeführt wird (beispielsweise ein SSH-Daemon).

Weitere Informationen zu Gerätestreams finden Sie hier:

> [!div class="nextstepaction"]
> [IoT Hub-Gerätestreams (Vorschau)](./iot-hub-device-streams-overview.md)