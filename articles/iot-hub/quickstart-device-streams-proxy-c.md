---
title: 'Azure IoT Hub-Gerätestreams: C-Schnellstartanleitung für SSH/RDP (Vorschauversion) | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung führen Sie eine C-Beispielanwendung aus, die als Proxy fungiert, um SSH-/RDP-Szenarien über IoT Hub-Gerätestreams zu ermöglichen.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 300b42c9452fc58c857d075a7fd8c42fd6a1c409
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55731732"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-c-proxy-application-preview"></a>Schnellstart: SSH/RDP über IoT Hub-Gerätestreams unter Verwendung einer C-Proxyanwendung (Vorschauversion)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Über [IoT Hub-Gerätestreams](./iot-hub-device-streams-overview.md) können Dienst- und Geräteanwendungen sicher und firewallfreundlich kommunizieren. Eine entsprechende Übersicht finden Sie auf [dieser Seite](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp).

In diesem Dokument wird das Setup zum Tunneln von SSH-Datenverkehr (unter Verwendung von Port 22) über Gerätestreams beschrieben. Das Setup für RDP-Datenverkehr ist ähnlich und bedarf lediglich einer einfachen Konfigurationsänderung. Gerätestreams sind anwendungs- und protokollunabhängig. Die vorliegende Schnellstartanleitung kann daher durch Ändern der Kommunikationsports für andere Arten von Anwendungsdatenverkehr angepasst werden.

## <a name="how-it-works"></a>So funktioniert's
Die folgende Abbildung zeigt, wie die lokalen Geräte- und Dienstproxyprogramme eine End-to-End-Konnektivität zwischen SSH-Client- und SSH-Daemonprozessen ermöglichen. Während der Public Preview-Phase unterstützt das C SDK nur geräteseitige Gerätestreams. Aus diesem Grund wird in dieser Schnellstartanleitung nur das Ausführen der lokalen Geräteproxyanwendung erläutert. Es empfiehlt sich, auch eine entsprechende lokale Dienstproxyanwendung auszuführen (verfügbar in der Schnellstartanleitung für [C#](./quickstart-device-streams-proxy-csharp.md) oder [Node.js](./quickstart-device-streams-proxy-nodejs.md)).

![Alternativer Text](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg "Setup des lokalen Proxys")


1. Der lokale Dienstproxy stellt eine Verbindung mit IoT Hub her und initiiert einen Gerätestream an das Zielgerät.

2. Der lokale Geräteproxy schließt den Handshake für die Streaminitiierung ab und richtet über den IoT Hub-Streamingendpunkt einen End-to-End-Streamingtunnel mit der Dienstseite ein.

3. Der lokale Geräteproxy stellt eine Verbindung mit dem SSH-Daemon (SSHD) her, der auf dem Gerät am Port 22 lauscht. Dies ist konfigurierbar, wie [weiter unten](#run-the device-local-proxy-application) beschrieben.

4. Der lokale Dienstproxy wartet auf neue SSH-Verbindungen des Benutzers, indem er am angegebenen Port (in diesem Fall: Port 2222) lauscht. Dies ist ebenfalls konfigurierbar, wie [weiter unten](#run-the-device-local-proxy-application) beschrieben. Wenn der Benutzer eine Verbindung über den SSH-Client herstellt, ermöglicht der Tunnel die Übertragung von SSH-Anwendungsdatenverkehr zwischen SSH-Client und Serverprogrammen.

> [!NOTE]
> Über einen Gerätestream gesendeter SSH-Datenverkehr wird nicht direkt zwischen Dienst und Gerät gesendet, sondern über den IoT Hub-Streamingendpunkt getunnelt. Die dadurch entstehenden Vorteile werden [hier](./iot-hub-device-streams-overview.md#benefits) erläutert. Die Abbildung zeigt außerdem, dass der SSH-Daemon auf dem gleichen Gerät (oder Computer) ausgeführt wird wie der lokale Geräteproxy. Wenn Sie in dieser Schnellstartanleitung die IP-Adresse des SSH-Daemons angeben, können der lokale Geräteproxy und der Daemon auch auf unterschiedlichen Geräten ausgeführt werden.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/vs/) mit der aktivierten Workload „[Desktopentwicklung mit C++](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/)“.
* Installieren Sie die aktuelle Version von [Git](https://git-scm.com/download/).

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

Für diese Schnellstartanleitung verwenden Sie das [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md). Sie bereiten eine Entwicklungsumgebung vor, die zum Klonen und Erstellen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) aus GitHub verwendet wird. Das SDK auf GitHub enthält den in dieser Schnellstartanleitung verwendeten Beispielcode. 


1. Laden Sie Version 3.11.4 des [CMake-Buildsystems](https://cmake.org/download/) von [GitHub](https://github.com/Kitware/CMake/releases/tag/v3.11.4) herunter. Überprüfen Sie die heruntergeladene Binärdatei mit dem entsprechenden kryptografischen Hashwert. Im folgenden Beispiel wird Windows PowerShell verwendet, um den kryptografischen Hash für Version 3.11.4 der x64 MSI-Verteilung zu überprüfen:

    ```PowerShell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.11.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869"
    True
    ```
    
    Zum Zeitpunkt der Abfassung dieses Artikels waren auf der Website von CMake die folgenden Hashwerte für Version 3.11.4 aufgeführt:

    ```
    6dab016a6b82082b8bcd0f4d1e53418d6372015dd983d29367b9153f1a376435  cmake-3.11.4-Linux-x86_64.tar.gz
    72b3b82b6d2c2f3a375c0d2799c01819df8669dc55694c8b8daaf6232e873725  cmake-3.11.4-win32-x86.msi
    56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869  cmake-3.11.4-win64-x64.msi
    ```

    Wichtig: Die Voraussetzungen für Visual Studio (Visual Studio und die Workload „Desktopentwicklung mit C++“) müssen **vor** Beginn der Installation von `CMake` auf dem Computer installiert sein. Sobald die Voraussetzungen erfüllt sind und der Download überprüft wurde, installieren Sie das CMake-Buildsystem.

2. Öffnen Sie eine Eingabeaufforderung oder die Git Bash-Shell. Führen Sie den folgenden Befehl zum Klonen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)-GitHub-Repositorys aus:
    
    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```
    Die Größe dieses Repositorys beträgt derzeit ca. 220 MB. Sie sollten damit rechnen, dass die Ausführung dieses Vorgangs mehrere Minuten in Anspruch nimmt.


3. Erstellen Sie ein `cmake`-Unterverzeichnis im Stammverzeichnis des Git-Repositorys, und navigieren Sie zu diesem Ordner. 

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Erstellen Sie mithilfe des folgenden Befehls eine spezifische SDK-Version für Ihre Entwicklungsclientplattform. Unter Windows wird eine Visual Studio-Projektmappe für das simulierte Gerät im Verzeichnis `cmake` generiert. 

```
    # In Linux
    cmake ..
    make -j
```

Führen Sie unter Windows an der Developer-Eingabeaufforderung für Visual Studio 2015 oder 2017 die folgenden Befehle aus:

```
    rem In Windows
    rem For VS2015
    cmake .. -G "Visual Studio 15 2015"

    rem Or for VS2017
    cmake .. -G "Visual Studio 15 2017"

    rem Then build the project
    cmake --build . -- /m /p:Configuration=Release
```
    

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrieren eines Geräts

Ein Gerät muss bei Ihrer IoT Hub-Instanz registriert sein, um eine Verbindung herstellen zu können. In diesem Abschnitt verwenden Sie Azure Cloud Shell mit der [IoT-Erweiterung](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest), um ein simuliertes Gerät zu registrieren.

1. Führen Sie die folgenden Befehle in Azure Cloud Shell aus, um die IoT Hub-CLI-Erweiterung hinzuzufügen und die Geräteidentität zu erstellen. 

   **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub wählen.

   **MyDevice**: Der für das registrierte Gerät angegebene Name. Verwenden Sie „MyDevice“ wie gezeigt. Wenn Sie für Ihr Gerät einen anderen Namen wählen, müssen Sie diesen innerhalb des gesamten Artikels verwenden und den Gerätenamen in den Beispielanwendungen aktualisieren, bevor Sie sie ausführen.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
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


## <a name="ssh-to-a-device-via-device-streams"></a>Herstellen einer SSH-Verbindung mit einem Gerät über Gerätestreams

### <a name="run-the-device-local-proxy-application"></a>Ausführen der lokalen Geräteproxyanwendung

- Bearbeiten Sie die Quelldatei `iothub_client/samples/iothub_client_c2d_streaming_proxy_sample/iothub_client_c2d_streaming_proxy_sample.c`, und geben Sie die Geräteverbindungszeichenfolge, die IP-Adresse/den Hostnamen des Zielgeräts sowie den RDP-Port 22 an:
```C
  /* Paste in the your iothub connection string  */
  static const char* connectionString = "[Connection string of IoT Hub]";
  static const char* localHost = "[IP/Host of your target machine]"; // Address of the local server to connect to.
  static const size_t localPort = 22; // Port of the local server to connect to.
```

- Kompilieren Sie das Beispiel wie folgt:

```
    # In Linux
    # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
    make -j
```

```
    rem In Windows
    rem Go to cmake at root of repository
    cmake --build . -- /m /p:Configuration=Release
```

- Führen Sie das kompilierte Programm auf dem Gerät aus:
```
    # In Linux
    # Go to sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
    ./iothub_client_c2d_streaming_proxy_sample
```

```
    rem In Windows
    rem Go to sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_proxy_sample\Release
    iothub_client_c2d_streaming_proxy_sample.exe
```

### <a name="run-the-service-local-proxy-application"></a>Ausführen der lokalen Dienstproxyanwendung

Wie [weiter oben](#how-it-works) bereits erwähnt, wird zur Einrichtung eines End-to-End-Streams für das Tunneln von SSH-Datenverkehr ein lokaler Proxy an jedem Ende (Dienst und Gerät) benötigt. Während der Public Preview-Phase unterstützt das IoT Hub C SDK allerdings nur geräteseitige Gerätestreams. Informationen zum lokalen Dienstproxy finden Sie in der Schnellstartanleitung für [C#](./quickstart-device-streams-proxy-csharp.md) oder [Node.js](./quickstart-device-streams-proxy-nodejs.md).


### <a name="establish-an-ssh-session"></a>Einrichten einer SSH-Sitzung

Wenn sowohl der lokale Geräteproxy als auch der lokale Dienstproxy ausgeführt wird, können Sie über Ihr SSH-Clientprogramm eine Verbindung mit dem lokalen Dienstproxy am Port 2222 herstellen (anstatt eine Direktverbindung mit dem SSH-Daemon). 

```
ssh <username>@localhost -p 2222
```

Daraufhin wird die SSH-Anmeldeaufforderung für die Eingabe Ihrer Anmeldeinformationen angezeigt.


Im Anschluss sehen Sie die Konsolenausgabe des lokalen Geräteproxys, der unter `IP_address:22` eine Verbindung mit dem SSH-Daemon herstellt: ![Alternativer Text](./media/quickstart-device-streams-proxy-c/device-console-output.PNG "Ausgabe des lokalen Geräteproxys")

Im Anschluss sehen Sie die Konsolenausgabe des SSH-Clientprogramms. (Der SSH-Client kommuniziert mit dem SSH-Daemon, indem er eine Verbindung mit dem Port 22 herstellt, an dem der lokale Dienstproxy lauscht.) ![Alternativer Text](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png "Ausgabe des SSH-Clients")

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen IoT-Hub eingerichtet, ein Gerät registriert, ein lokales Geräteproxyprogramm und ein lokales Dienstproxyprogramm bereitgestellt, um einen Gerätestream über IoT Hub einzurichten, sowie die Proxys zum Tunneln von SSH-Datenverkehr verwendet.

Weitere Informationen zu Gerätestreams finden Sie hier:

> [!div class="nextstepaction"]
> [IoT Hub-Gerätestreams (Vorschau)](./iot-hub-device-streams-overview.md)
