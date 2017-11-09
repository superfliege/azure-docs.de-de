---
title: "Bereitstellen von Windows-Geräten für die Remoteüberwachung in C – Azure | Microsoft-Dokumentation"
description: "Beschreibt, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in C geschriebene Anwendung unter Windows ausführen."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2017
ms.author: dobett
ms.openlocfilehash: ef38517b55b352acf036e62d407f1ff840d6f804
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

In diesem Tutorial wird gezeigt, wie Sie die Verbindung eines physischen Geräts mit der vorkonfigurierten Lösung für die Remoteüberwachung herstellen.

## <a name="create-a-c-client-solution-on-windows"></a>Erstellen einer C-Clientlösung unter Windows

Wie bei den meisten eingebetteten Anwendungen, die auf eingeschränkten Geräten ausgeführt werden, wird der Clientcode für die Geräteanwendung in C geschrieben. In diesem Tutorial erstellen Sie die Anwendung auf einem Computer, auf dem Windows ausgeführt wird.

### <a name="create-the-starter-project"></a>Erstellen des Startprojekts

Erstellen Sie ein Startprojekt in Visual Studio 2017, und fügen Sie die NuGet-Pakete für den IoT Hub-Geräteclient hinzu:

1. Erstellen Sie in Visual Studio eine C-Konsolenanwendung mit Visual C++ in der **Windows-Konsolenanwendungsvorlage**. Geben Sie dem Projekt den Namen **RMDevice**.

    ![Erstellen einer Windows-Konsolenanwendung in Visual C++](media/iot-suite-connecting-devices/visualstudio01.png)

1. Löschen Sie im **Projektmappen-Explorer** die Dateien `stdafx.h`, `targetver.h` und `stdafx.cpp`.

1. Benennen Sie im **Projektmappen-Explorer** die Datei `RMDevice.cpp` in `RMDevice.c` um.

    ![Umbenannte Datei „RMDevice.c“ wird im Projektmappen-Explorer angezeigt.](media/iot-suite-connecting-devices/visualstudio02.png)

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, und klicken Sie dann auf **NuGet-Pakete verwalten**. Wählen Sie **Durchsuchen**, suchen Sie dann die folgenden NuGet-Pakete, und installieren Sie sie:

    * Microsoft.Azure.IoTHub.Serializer
    * Microsoft.Azure.IoTHub.IoTHubClient
    * Microsoft.Azure.IoTHub.MqttTransport

    ![Installierte Microsoft.Azure.IoTHub-Pakete werden im NuGet-Paket-Manager angezeigt.](media/iot-suite-connecting-devices/visualstudio03.png)

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, und wählen Sie dann **Eigenschaften**, um das Dialogfeld **Eigenschaftenseiten** des Projekts zu öffnen. Weitere Informationen hierzu finden Sie unter [Festlegen von Visual C++-Projekteigenschaften](https://docs.microsoft.com/cpp/ide/working-with-project-properties).

1. Wählen Sie den **C/C++**-Ordner und dann die Eigenschaftenseite **Vorkompilierte Header**.

1. Legen Sie **Vorkompilierten Header** auf **Vorkompilierte Header werden nicht verwendet** fest. Wählen Sie anschließend die Option **Übernehmen**.

    ![In den Projekteigenschaften wird das Projekt ohne Verwendung vorkompilierter Header angezeigt.](media/iot-suite-connecting-devices/visualstudio04.png)

1. Wählen Sie den Ordner **Linker** und dann die Eigenschaftenseite **Eingabe**.

1. Fügen Sie `crypt32.lib` der Eigenschaft **Zusätzliche Abhängigkeiten** hinzu. Um die Eigenschaftenwerte für das Projekt zu speichern, wählen Sie **OK** und dann erneut **OK**.

    ![In den Projekteigenschaften wird „Linker“ einschließlich „crypt32.lib“ angezeigt.](media/iot-suite-connecting-devices/visualstudio05.png)

### <a name="add-the-parson-json-library"></a>Hinzufügen der Parson JSON-Bibliothek

Fügen Sie die Parson JSON-Bibliothek dem **RMDevice**-Projekt hinzu, und fügen Sie die erforderlichen `#include`-Anweisungen hinzu:

1. Klonen Sie in einem geeigneten Ordner auf Ihrem Computer das Parson GitHub-Repository mit dem folgenden Befehl:

    ```cmd
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopieren Sie die Dateien `parson.h` und `parson.c` aus der lokalen Kopie des Parson-Repositorys in Ihren **RMDevice**-Projektordner.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **RMDevice**. Wählen Sie **Hinzufügen** und dann **Vorhandenes Element**.

1. Wählen Sie im Dialogfeld **Vorhandenes Element hinzufügen** die Dateien `parson.h` und `parson.c` im **RMDevice**-Projektordner aus. Um diese zwei Dateien Ihrem Projekt hinzuzufügen, wählen Sie **Hinzufügen**.

    ![Dateien „parson.h“ und „parson.c“ werden im Projektmappen-Explorer angezeigt.](media/iot-suite-connecting-devices/visualstudio06.png)

1. Öffnen Sie in Visual Studio die Datei `RMDevice.c`. Ersetzen Sie die vorhandenen `#include`-Anweisungen durch folgenden Code:

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

    > [!NOTE]
    > Jetzt können Sie die Lösung erstellen und dabei überprüfen, ob für Ihr Projekt die richtigen Abhängigkeiten eingerichtet sind.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Fügen Sie Code zum Aufrufen der Funktion **remote\_monitoring\_run** hinzu, erstellen Sie anschließend die Geräteanwendung, und führen Sie sie aus:

1. Um die **remote\_monitoring\_run**-Funktion aufzurufen, ersetzen Sie die **main**-Funktion durch folgenden Code:

    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Wählen Sie **Erstellen** und dann **Projektmappe erstellen**, um die Geräteanwendung zu erstellen. Ignorieren Sie die Warnung bezüglich der **gmtime**-Funktion.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **RMDevice**, wählen Sie **Debuggen** und dann **Neue Instanz starten**, um das Beispiel auszuführen. Die Konsole zeigt Meldungen an wie:

    * Die Anwendung sendet Beispieltelemetriedaten an die vorkonfigurierte Lösung.
    * Empfängt die im Lösungsdashboard festgelegten gewünschten Eigenschaftswerte.
    * Reagiert auf Methoden, die über das Lösungsdashboard aufgerufen werden.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
