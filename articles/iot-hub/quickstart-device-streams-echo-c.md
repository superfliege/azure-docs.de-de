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
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: b5d93219009554efe6931a02097dcb9085e8da9d
ms.sourcegitcommit: 41015688dc94593fd9662a7f0ba0e72f044915d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/11/2019
ms.locfileid: "59500242"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Schnellstart: Kommunizieren mit einer Geräteanwendung in C über IoT Hub-Gerätestreams (Vorschauversion)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IoT Hub unterstützt derzeit Gerätestreams als [Previewfunktion](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Über [IoT Hub-Gerätestreams](./iot-hub-device-streams-overview.md) können Dienst- und Geräteanwendungen sicher und firewallfreundlich kommunizieren. Während der Public Preview-Phase unterstützt das C SDK nur geräteseitige Gerätestreams. Aus diesem Grund wird in dieser Schnellstartanleitung nur das Ausführen der geräteseitigen Anwendung erläutert. Es empfiehlt sich, auch eine entsprechende dienstseitige Anwendung auszuführen (verfügbar in der Schnellstartanleitung für [C#](./quickstart-device-streams-echo-csharp.md) oder [Node.js](./quickstart-device-streams-echo-nodejs.md)).

Die geräteseitige C-Anwendung in dieser Schnellstartanleitung hat folgende Funktionen:

* Einrichten eines Gerätestreams mit einem IoT-Gerät

* Empfangen von dienstseitig gesendeten Daten und Zurückgeben dieser Daten

Der Code veranschaulicht den Initiierungsprozess eines Gerätestreams sowie das Senden und Empfangen von Daten.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Die Vorschau der Gerätestreams wird derzeit nur für IoT Hubs unterstützt, die in folgenden Regionen erstellt werden:

  * **USA (Mitte)**
  * **USA, Mitte (EUAP)**

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/vs/) mit der aktivierten Workload „[Desktopentwicklung mit C++](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/)“.
* Installieren Sie die aktuelle Version von [Git](https://git-scm.com/download/).
* Führen Sie den folgenden Befehl aus, um Ihrer Cloud Shell-Instanz die Microsoft Azure IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle hinzuzufügen. Die IoT-Erweiterung fügt der Azure-Befehlszeilenschnittstelle spezifische Befehle für IoT Hub, IoT Edge und IoT Device Provisioning Service (DPS) hinzu.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

Für diese Schnellstartanleitung verwenden Sie das [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md). Sie bereiten eine Entwicklungsumgebung vor, die zum Klonen und Erstellen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) aus GitHub verwendet wird. Das SDK auf GitHub enthält den in dieser Schnellstartanleitung verwendeten Beispielcode.

1. Laden Sie das [CMake-Buildsystem](https://cmake.org/download/) herunter.

    Wichtig: Die Voraussetzungen für Visual Studio (Visual Studio und die Workload „Desktopentwicklung mit C++“) müssen **vor** Beginn der Installation von `CMake` auf dem Computer installiert sein. Sobald die Voraussetzungen erfüllt sind und der Download überprüft wurde, installieren Sie das CMake-Buildsystem.

2. Öffnen Sie eine Eingabeaufforderung oder die Git Bash-Shell. Führen Sie den folgenden Befehl zum Klonen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)-GitHub-Repositorys aus:
    
    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Dieser Vorgang dauert einige Minuten.

3. Erstellen Sie ein `cmake`-Unterverzeichnis im Stammverzeichnis des Git-Repositorys, und navigieren Sie zu diesem Ordner. 

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Erstellen Sie mithilfe der folgenden Befehle aus dem Verzeichnis `cmake` eine spezifische SDK-Version für Ihre Entwicklungsclientplattform.

   * Linux:

      ```bash
      cmake ..
      make -j
      ```

   * Führen Sie unter Windows an der Developer-Eingabeaufforderung für Visual Studio 2015 oder 2017 die folgenden Befehle aus. Im `cmake`-Verzeichnis wird eine Visual Studio-Projektmappe für das simulierte Gerät generiert.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrieren eines Geräts

Ein Gerät muss bei Ihrer IoT Hub-Instanz registriert sein, um eine Verbindung herstellen zu können. In diesem Abschnitt verwenden Sie Azure Cloud Shell mit der [IoT-Erweiterung](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest), um ein simuliertes Gerät zu registrieren.

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

## <a name="communicate-between-device-and-service-via-device-streams"></a>Kommunizieren zwischen Gerät und Dienst über Gerätestreams

### <a name="run-the-device-side-application"></a>Ausführen der geräteseitigen Anwendung

Gehen Sie wie folgt vor, um die geräteseitige Anwendung auszuführen:

1. Geben Sie Ihre Geräteanmeldeinformationen an, indem Sie die Quelldatei `iothub_client/samples/iothub_client_c2d_streaming_sample/iothub_client_c2d_streaming_sample.c` bearbeiten, und geben Sie Ihre Geräteverbindungszeichenfolge an.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[device connection string]";
   ```

2. Kompilieren Sie den Code:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

3. Führen Sie das kompilierte Programm aus:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>Ausführen der dienstseitigen Anwendung

Wie bereits erwähnt, unterstützt das IoT Hub C SDK nur geräteseitige Gerätestreams. Wenn Sie die dienstseitige Anwendung erstellen und ausführen möchten, befolgen Sie die Schritte in der Schnellstartanleitung für [C#](./quickstart-device-streams-echo-csharp.md) oder [Node.js](./quickstart-device-streams-echo-nodejs.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen IoT-Hub eingerichtet, ein Gerät registriert, einen Gerätestream zwischen einer C-Anwendung und einer anderen dienstseitigen Anwendung eingerichtet und mithilfe des Streams Daten zwischen den Anwendungen hin und her gesendet.

Weitere Informationen zu Gerätestreams finden Sie hier:

> [!div class="nextstepaction"]
> [Übersicht über Gerätestreams](./iot-hub-device-streams-overview.md)