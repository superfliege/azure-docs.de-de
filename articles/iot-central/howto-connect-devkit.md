---
title: Herstellen einer Verbindung zwischen einem DevKit-Gerät und Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Geräteentwickler ein MXChip IoT-Entwickler-Kit-Gerät mit Ihrer Azure IoT Central-Anwendung verbinden.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 4c7074e5e7d3858919f3fc17005fea4f8dce1560
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34200740"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Azure IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Geräteentwickler eine Verbindung zwischen einem MXChip IoT DevKit-Gerät (DevKit) und Ihrer Microsoft Azure IoT Central-Anwendung herstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

1. Eine Azure IoT Central-Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde. Weitere Informationen finden Sie unter [Erstellen Ihrer Azure IoT Central-Anwendung](howto-create-application.md).
1. Ein DevKit-Gerät. Um ein DevKit-Gerät zu erwerben, besuchen Sie [MXChip IoT DevKit](http://mxchip.com/az3166).

Eine Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde, enthält eine Gerätevorlage **MXChip** mit den folgenden Eigenschaften:

### <a name="telemetry-measurements"></a>Telemetriemessungen

| Feldname     | Units  | Minimum | Maximum | Dezimalstellen |
| -------------- | ------ | ------- | ------- | -------------- |
| Luftfeuchtigkeit       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | mg     | -2000   | 2000    | 0              |
| accelerometerY | mg     | -2000   | 2000    | 0              |
| accelerometerZ | mg     | -2000   | 2000    | 0              |
| gyroscopeX     | mdps   | -2000   | 2000    | 0              |
| gyroscopeY     | mdps   | -2000   | 2000    | 0              |
| gyroscopeZ     | mdps   | -2000   | 2000    | 0              |

### <a name="settings"></a>Einstellungen

Numerische Einstellungen

| Anzeigename | Feldname | Units | Dezimalstellen | Minimum | Maximum | Initial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Spannung      | setVoltage | Volt | 0              | 0       | 240     | 0       |
| Aktuell      | setCurrent | Ampere  | 0              | 0       | 100     | 0       |
| Lüfterdrehzahl    | fanSpeed   | U/Min   | 0              | 0       | 1000    | 0       |

Einstellungen zum Ein-/Ausschalten

| Anzeigename | Feldname | Text, wenn „eingeschaltet“ | Text, wenn „ausgeschaltet“ | Initial |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | EIN      | OFF      | Aus     |

### <a name="properties"></a>Eigenschaften

| Typ            | Anzeigename | Feldname | Datentyp |
| --------------- | ------------ | ---------- | --------- |
| Geräteeigenschaft | Nummer   | dieNumber  | number    |
| Text            | Speicherort     | location   | N/V       |

### <a name="states"></a>Zustände 

| NAME          | Anzeigename   | NORMAL | VORSICHT | GEFAHR | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Gerätestatus   | Grün  | Orange  | Rot    | 

### <a name="events"></a>Ereignisse 

| NAME             | Anzeigename      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Schaltfläche „B“ gedrückt  | 

### <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Fügen Sie in Ihrer Azure IoT Central-Anwendung ein echtes Gerät aus der Gerätevorlage **MXChip** hinzu, und notieren Sie sich die Verbindungszeichenfolge des Geräts. Weitere Informationen finden Sie unter [Hinzufügen eines echten Geräts zu Ihrer Azure IoT Central-Anwendung](tutorial-add-device.md).

## <a name="prepare-the-devkit-device"></a>Vorbereiten des DevKit-Geräts

> [!TIP]
> Anleitungen zur Problembehandlung bei DevKit-Geräten finden Sie unter [Einstieg in IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/).

So bereiten Sie das DevKit-Gerät vor

1. Laden Sie die neueste vorgefertigte Azure IoT Central-Firmware für den MXChip von der Seite [Releases](https://github.com/Azure/iot-central-firmware/releases) in GitHub herunter. Der Downloaddateiname auf der Seite mit Releases sieht folgendermaßen aus: `AZ3166-IoT-Central-X.X.X.bin`.

1. Stellen Sie über ein USB-Kabel eine Verbindung zwischen dem DevKit-Gerät und dem Entwicklungscomputer her. Unter Windows wird ein Datei-Explorer-Fenster für ein Laufwerk geöffnet, das dem Speicher auf dem DevKit-Gerät zugeordnet ist. Das Laufwerk heißt beispielsweise **AZ3166 (D:)**.

1. Ziehen Sie die Datei **iotCentral.bin** auf das Laufwerksfenster. Wenn der Kopiervorgang abgeschlossen ist, wird das Gerät mit der neuen Firmware neu gestartet.

1. Beim Neustart des DevKit-Geräts wird der folgende Bildschirm angezeigt:

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1 
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Wenn im Bildschirm etwas anderes angezeigt wird, drücken Sie am Gerät die **Rücksetztaste**. 

1. Das Gerät befindet sich jetzt im Zugriffspunktmodus (Access Point, AP). Sie können von Ihrem Computer oder Mobilgerät aus eine Verbindung mit diesem WLAN-Zugriffspunkt herstellen.

1. Stellen Sie auf Ihrem Computer, Smartphone oder Tablet eine Verbindung mit dem WLAN-Namen her, der auf dem Bildschirm des Geräts angezeigt wird. Wenn Sie eine Verbindung mit diesem Netzwerk herstellen, können Sie nicht auf das Internet zugreifen. Dies ist so beabsichtigt. Sie werden mit diesem Netzwerk nur für kurze Zeit verbunden, solange Sie das Gerät konfigurieren.

1. Öffnen Sie Ihren Webbrowser, und navigieren Sie zu [http://192.168.0.1/start](http://192.168.0.1/start). Die folgende Webseite wird angezeigt:

    ![Seite „Gerätekonfiguration“](media/howto-connect-devkit/configpage.png)

    Fügen Sie auf der Webseite Folgendes hinzu: 
    - Den Namen Ihres WLAN. 
    - Das Kennwort für das WLAN. 
    - Den auf dem Geräte-LCD angezeigten PIN-Code. 
    - Die Verbindungszeichenfolge Ihres Geräts. 
      Sie finden die Verbindungszeichenfolge unter `https://apps.iotcentral.com` -> `Device Explorer` -> `Device` -> `Select or Create a new Real Device` -> `Connect this device` (oben rechts). 
    - Wählen Sie alle verfügbaren Telemetriemessungen aus. 

1. Nachdem Sie **Gerät konfiguriere** ausgewählt haben, wird diese Seite angezeigt:

    ![Gerät konfiguriert](media/howto-connect-devkit/deviceconfigured.png)

1. Drücken Sie an Ihrem Gerät die **Rücksetztaste**.

> [!NOTE]
> Um das Gerät für ein anderes WLAN, eine andere Verbindungszeichenfolge oder eine andere Telemetriemessung zu konfigurieren, drücken Sie am Board gleichzeitig die Tasten **A** und **B**. Wenn dies nicht funktioniert, drücken Sie **Rücksetztaste**, und wiederholen Sie den Vorgang. 

## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Beim Neustart des DevKit-Geräts wird auf dem Gerätebildschirm Folgendes angezeigt:

* Die Anzahl gesendeter Telemetrienachrichten
* Die Fehleranzahl
* Die Anzahl der erhaltenen gewünschten Eigenschaften und die Anzahl der gesendeten gemeldeten Eigenschaften.

Durch Schütteln des Geräts wird die Anzahl der gesendeten gemeldeten Eigenschaften inkrementell erhöht. Das Gerät sendet eine zufällige Zahl als Geräteeigenschaft **Nummer**.

Sie können die Telemetriemessungen und die gemeldeten Eigenschaftswerte anzeigen und Einstellungen in Azure IoT Central konfigurieren:

1. Verwenden Sie den **Device Explorer**, um die Seite **Messungen** für das reale MXChip-Gerät aufzurufen, das Sie hinzugefügt haben:

    ![Navigieren zum realen Gerät](media/howto-connect-devkit/realdevice.png)

1. Auf der Seite **Messungen** sehen Sie die vom MXChip-Gerät gesendeten Telemetriedaten:

    ![Anzeigen der Telemetrie vom realen Gerät](media/howto-connect-devkit/realtelemetry.png)

1. Auf der Seite **Eigenschaften** sehen Sie die letzte vom Gerät gemeldete Nummer:

    ![Anzeigen von Geräteeigenschaften](media/howto-connect-devkit/deviceproperties.png)

1. Auf der Seite **Einstellungen** können Sie die Einstellungen für das MXChip-Gerät aktualisieren:

    ![Anzeigen der Geräteeinstellungen](media/howto-connect-devkit/settings.png)

## <a name="download-the-source-code"></a>Herunterladen des Quellcodes

Wenn Sie den Gerätecode untersuchen und ändern möchten, können Sie ihn von GitHub herunterladen. Wenn Sie den Code ändern möchten, befolgen Sie die Anweisungen zum [Vorbereiten der Entwicklungsumgebung](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) für Ihr Desktopbetriebssystem.

Um den Quellcode herunterzuladen, führen Sie den folgenden Befehl auf dem Desktopcomputer aus:

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

Mit dem vorherige Befehl wird der Quellcode in einen Ordner namens `iot-central-firmware` heruntergeladen. 

> [!NOTE]
> Wenn **Git** in Ihrer Entwicklungsumgebung nicht installiert ist, können Sie es von [https://git-scm.com/download](https://git-scm.com/download) herunterladen.

## <a name="review-the-code"></a>Überprüfen des Codes

Verwenden Sie Visual Studio Code (bei der Vorbereitung Ihrer Entwicklungsumgebung installiert), um den Ordner `AZ3166` im Ordner `iot-central-firmware` zu öffnen: 

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Um zu sehen, wie die Telemetrie an die Azure IoT Central-Anwendung gesendet wird, öffnen Sie die Datei **main_telemetry.cpp** im Quellordner.

Die Funktion `buildTelemetryPayload` erstellt die JSON-Telemetrienutzlast anhand von Daten von den Sensoren des Geräts.

Die Funktion `sendTelemetryPayload` ruft `sendTelemetry` in der Datei **iotHubClient.cpp** auf, um die JSON-Nutzlast an den IoT Hub zu senden, der von Ihrer Azure IoT Central-Anwendung verwendet wird.

Um zu sehen, wie Eigenschaftswerte an die Azure IoT Central-Anwendung gemeldet werden, öffnen Sie die Datei **main_telemetry.cpp** im Quellordner.

Die Funktion `telemetryLoop` sendet die gemeldete Eigenschaft **doubleTap**, wenn der Beschleunigungsmesser einen Doppeltipp erkennt. Dabei wird die Funktion `sendReportedProperty` in der Quelldatei **iotHubClient.cpp** verwendet.

Der Code in der Quelldatei **iotHubClient.cpp** verwendet Funktionen aus den [Microsoft Azure IoT SDKs und Bibliotheken für C](https://github.com/Azure/azure-iot-sdk-c) für die Interaktion mit IoT Hub.

Informationen zum Ändern, Erstellen und Hochladen des Beispielcodes auf Ihr Gerät finden Sie in der Datei **readme.md** im Ordner `AZ3166`.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie ein DevKit-Gerät mit Ihrer Azure IoT Central-Anwendung verbunden wird, werden als Nächstes die folgenden Schritte empfohlen:

* [Vorbereiten und Verbinden eines Raspberry Pi](howto-connect-raspberry-pi-python.md)