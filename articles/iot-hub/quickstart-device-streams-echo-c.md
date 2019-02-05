---
title: 'Azure IoT Hub-Gerätestreams: C-Schnellstartanleitung (Vorschauversion) | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung führen Sie eine dienstseitige C-Anwendung aus, die über einen Gerätestream mit einem IoT-Gerät kommuniziert.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 24b00a589454bfa8413cd98407c2022671cb92ce
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54887953"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Schnellstart: Kommunizieren mit einer Geräteanwendung in C über IoT Hub-Gerätestreams (Vorschauversion)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Über [IoT Hub-Gerätestreams](./iot-hub-device-streams-overview.md) können Dienst- und Geräteanwendungen sicher und firewallfreundlich kommunizieren. Während der Public Preview-Phase unterstützt das C SDK nur geräteseitige Gerätestreams. Aus diesem Grund wird in dieser Schnellstartanleitung nur das Ausführen der geräteseitigen Anwendung erläutert. Es empfiehlt sich, auch eine entsprechende dienstseitige Anwendung auszuführen (verfügbar in der Schnellstartanleitung für [C#](./quickstart-device-streams-echo-csharp.md) oder [Node.js](./quickstart-device-streams-echo-nodejs.md)).

Die geräteseitige C-Anwendung in dieser Schnellstartanleitung hat folgende Funktionen:

* Einrichten eines Gerätestreams mit einem IoT-Gerät

* Empfangen von dienstseitig gesendeten Daten und Zurückgeben dieser Daten

Der Code veranschaulicht den Initiierungsprozess eines Gerätestreams sowie das Senden und Empfangen von Daten.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/vs/) mit der aktivierten Workload „[Desktopentwicklung mit C++](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/)“.
* Installieren Sie die aktuelle Version von [Git](https://git-scm.com/download/).

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

Für diese Schnellstartanleitung verwenden Sie das [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md). Sie bereiten eine Entwicklungsumgebung vor, die zum Klonen und Erstellen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) aus GitHub verwendet wird. Das SDK auf GitHub enthält den in dieser Schnellstartanleitung verwendeten Beispielcode. 


1. Laden Sie Version 3.11.4 des [CMake-Buildsystems](https://cmake.org/download/) herunter. Überprüfen Sie die heruntergeladene Binärdatei mit dem entsprechenden kryptografischen Hashwert. Im folgenden Beispiel wird Windows PowerShell verwendet, um den kryptografischen Hash für Version 3.11.4 der x64 MSI-Verteilung zu überprüfen:

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

2. Öffnen Sie eine Eingabeaufforderung oder die Git Bash-Shell. Führen Sie den folgenden Befehl zum Klonen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)-GitHub-Repositorys aus:
    
    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

    Die Größe dieses Repositorys beträgt derzeit ca. 220 MB.

3. Erstellen Sie ein `cmake`-Unterverzeichnis im Stammverzeichnis des Git-Repositorys, und navigieren Sie zu diesem Ordner. 

    ```
    cd azure-iot-sdk-c
    git checkout public-preview
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
    # In Windows
    # For VS2015
    cmake .. -G "Visual Studio 15 2015"
    
    # Or for VS2017
    cmake .. -G "Visual Studio 15 2017

    # Then build the project
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


## <a name="communicate-between-device-and-service-via-device-streams"></a>Kommunizieren zwischen Gerät und Dienst über Gerätestreams

### <a name="run-the-device-side-application"></a>Ausführen der geräteseitigen Anwendung

Gehen Sie wie folgt vor, um die geräteseitige Anwendung auszuführen:
- Richten Sie Ihre Entwicklungsumgebung gemäß der Anleitung in [diesem Artikel über Gerätestreams](https://github.com/Azure/azure-iot-sdk-c-tcpstreaming/blob/master/iothub_client/readme.md#compiling-the-device-sdk-for-c) ein.

- Geben Sie Ihre Geräteanmeldeinformationen an, indem Sie die Quelldatei `iothub_client/samples/iothub_client_c2d_streaming_sample/iothub_client_c2d_streaming_sample.c` bearbeiten, und geben Sie Ihre Geräteverbindungszeichenfolge an.
```C
  /* Paste in the your iothub connection string  */
  static const char* connectionString = "[device connection string]";
```

- Kompilieren Sie den Code:

```
  # In Linux
  # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
  make -j


  # In Windows
  # Go to the cmake folder at the root of repo
  cmake --build . -- /m /p:Configuration=Release
```

- Führen Sie das kompilierte Programm aus:

```
  # In Linux
  # Go to sample's folder
  cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
  ./iothub_client_c2d_streaming_sample


  # In Windows
  # Go to sample's release folder
  cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
  iothub_client_c2d_streaming_sample.exe
```

### <a name="run-the-service-side-application"></a>Ausführen der dienstseitigen Anwendung

Wie bereits erwähnt, unterstützt das IoT Hub C SDK während der Public Preview-Phase nur geräteseitige Gerätestreams. Verwenden Sie als dienstseitige Anwendung die entsprechenden Dienstprogramme aus der Anleitung für [C#](./quickstart-device-streams-echo-csharp.md) oder [Node.js](./quickstart-device-streams-echo-nodejs.md).


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen IoT-Hub eingerichtet, ein Gerät registriert, mit einer C-Anwendung simulierte Telemetriedaten an den Hub gesendet und mithilfe von Azure Cloud Shell die Telemetriedaten aus dem Hub gelesen.

Weitere Informationen zu Gerätestreams finden Sie hier:

> [!div class="nextstepaction"]
> [IoT Hub-Gerätestreams (Vorschau)](./iot-hub-device-streams-overview.md)