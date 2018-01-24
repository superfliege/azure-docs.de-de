---
title: "Bereitstellen von Raspberry Pi für die Remoteüberwachung mit C – Azure | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe einer in C geschriebenen Anwendung eine Verbindung zwischen einem Raspberry Pi-Gerät und der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite herstellen."
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.author: dobett
ms.openlocfilehash: 7cfa6dd93c6db7477e03ff966b2ac8af15de3614
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-c"></a>Herstellen einer Verbindung zwischen Ihrem Raspberry Pi-Gerät und der vorkonfigurierten Remoteüberwachungslösung (C)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

In diesem Tutorial erfahren Sie, wie Sie eine Verbindung zwischen einem physischen Gerät und der vorkonfigurierten Remoteüberwachungslösung herstellen. Wie bei den meisten eingebetteten Anwendungen, die auf eingeschränkten Geräten ausgeführt werden, wird der Clientcode für die Raspberry Pi-Geräteanwendung in C geschrieben. In diesem Tutorial erstellen Sie die Anwendung auf einem Raspberry Pi mit dem Raspbian-Betriebssystem.

### <a name="required-hardware"></a>Erforderliche Hardware

Ein Desktopcomputer, über den Sie eine Remoteverbindung mit der Befehlszeile auf dem Raspberry Pi herstellen können.

[Microsoft IoT Starter Kit für Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) oder entsprechende Komponenten. In diesem Tutorial wird Folgendes aus dem Kit verwendet:

- Raspberry Pi 3
- MicroSD-Karte (mit NOOBS)
- Ein Mini-USB-Kabel
- Ein Ethernetkabel

### <a name="required-desktop-software"></a>Erforderliche Desktopsoftware

Sie benötigen einen SSH-Client auf Ihrem Desktopcomputer, um per Remotezugriff auf die Befehlszeile des Raspberry Pi zugreifen zu können.

- In Windows ist kein SSH-Client enthalten. Wir empfehlen die Verwendung von [PuTTY](http://www.putty.org/).
- In den meisten Linux-Distributionen sowie in Mac OS ist ein SSH-Befehlszeilenprogramm enthalten. Weitere Informationen finden Sie unter [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md) (SSH unter Linux oder Mac OS).

### <a name="required-raspberry-pi-software"></a>Erforderliche Raspberry Pi-Software

In diesem Artikel wird vorausgesetzt, dass Sie die neueste Version des [Raspbian-Betriebssystems auf Ihrem Raspberry Pi](https://www.raspberrypi.org/learning/software-guide/quickstart/) installiert haben.

Die folgenden Schritte zeigen, wie Sie Ihren Raspberry Pi für die Erstellung einer C-Anwendung vorbereiten, die eine Verbindung mit der vorkonfigurierten Lösung herstellt:

1. Stellen Sie mithilfe von **SSH** eine Verbindung mit Ihrem Raspberry Pi her. Weitere Informationen finden Sie auf der [Raspberry Pi-Website](https://www.raspberrypi.org/) unter [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md).

1. Verwenden Sie den folgenden Befehl aus, um Ihr Raspberry Pi zu aktualisieren:

    ```sh
    sudo apt-get update
    ```

1. Verwenden Sie den folgenden Befehl, um Ihrem Raspberry Pi die erforderlichen Entwicklungstools und Bibliotheken hinzuzufügen:

    ```sh
    sudo apt-get purge libssl-dev
    sudo apt-get install g++ make cmake gcc git libssl1.0-dev build-essential curl libcurl4-openssl-dev uuid-dev
    ```

1. Verwenden Sie die folgenden Befehle, um die IoT Hub-Clientbibliotheken auf Ihren Raspberry Pi herunterzuladen, sie zu erstellen und zu installieren:

    ```sh
    cd ~
    git clone --recursive https://github.com/azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c/build_all/linux
    ./build.sh --no-make
    cd ../../cmake/iotsdk_linux
    make
    sudo make install
    ```

## <a name="create-a-project"></a>Erstellen eines Projekts

Verwenden Sie für die folgenden Schritte die **SSH**-Verbindung mit Ihrem Raspberry Pi:

1. Erstellen Sie im Stammordner des Raspberry Pi einen Ordner namens `remote_monitoring`. Navigieren Sie in Ihrer Shell zu diesem Ordner:

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. Erstellen Sie die vier Dateien **main.c**, **remote_monitoring.c**, **remote_monitoring.h** und **CMakeLists.txt** im Ordner `remote_monitoring`.

1. Öffnen Sie in einem Text-Editor die Datei **remote_monitoring.c**. Auf dem Raspberry Pi können Sie den Text-Editor **nano** oder **vi** verwenden. Fügen Sie die folgenden `#include`-Anweisungen ein:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

Speichern Sie die Datei **remote_monitoring.c**, und beenden Sie den Editor.

## <a name="add-code-to-run-the-app"></a>Hinzufügen von Code zum Ausführen der App

Öffnen Sie in einem Text-Editor die Datei **remote_monitoring.h**. Fügen Sie den folgenden Code hinzu:

```c
void remote_monitoring_run(void);
```

Speichern Sie die Datei **remote_monitoring.h**, und beenden Sie den Editor.

Öffnen Sie in einem Text-Editor die Datei **main.c** . Fügen Sie den folgenden Code hinzu:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

Speichern Sie die Datei **main.c**, und beenden Sie den Editor.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

Die folgenden Schritte beschreiben, wie Sie *CMake* verwenden, um Ihre Clientanwendung zu erstellen.

1. Öffnen Sie in einem Text-Editor die Datei **CMakeLists.txt** im Ordner `remote_monitoring`.

1. Fügen Sie die folgenden Anweisungen hinzu, um zu definieren, wie Sie Ihre Clientanwendung erstellen:

    ```cmake
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "/usr/local/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```

1. Speichern Sie die Datei **CMakeLists.txt**, und beenden Sie den Editor.

1. Erstellen Sie im Ordner `remote_monitoring` einen Ordner zum Speichern der *make*-Dateien, die CMake generiert. Führen Sie dann die Befehle **cmake** und **make** wie folgt aus:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Führen Sie die Clientanwendung aus, und senden Sie die Telemetriedaten an IoT Hub:

    ```sh
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
