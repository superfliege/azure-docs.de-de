---
title: Herstellen einer Verbindung zwischen einem DevKit-Gerät und Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Geräteentwickler ein MXChip IoT-Entwickler-Kit-Gerät mit Ihrer Azure IoT Central-Anwendung verbinden.
author: dominicbetts
ms.author: dobett
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 44af0ccab45f1335d9dfec06287303a34391eded
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58113196"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Azure IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Geräteentwickler eine Verbindung zwischen einem MXChip IoT DevKit-Gerät (DevKit) und Ihrer Microsoft Azure IoT Central-Anwendung herstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

1. Eine Azure IoT Central-Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central.md).
1. Ein DevKit-Gerät. Um ein DevKit-Gerät zu erwerben, besuchen Sie [MXChip IoT DevKit](http://mxchip.com/az3166).

## <a name="sample-devkits-application"></a>Anwendung Beispiel-DevKits

Eine Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde, enthält eine Gerätevorlage **MXChip** mit den folgenden Eigenschaften:

- Telemetriedaten, die die Messwerte für das Gerät enthalten: **Luftfeuchtigkeit**, **Temperatur**, **Druck**, **Magnetometer** (entlang der X-, Y- und Z-Achse gemessen), **Beschleunigungssensor** (entlang der X-, Y- und Z-Achse gemessen) und **Gyroskop** (entlang der X-, Y- und Z-Achse gemessen).
- Zustand, der einen Beispielmesswert für **Gerätestatus** enthält
- Ereignismessung mit einem Ereignis namens **Taste B gedrückt** 
- Einstellungen für **Spannung**, **Stromstärke** und **Lüftergeschwindigkeit** und eine Umschaltschaltfläche namens **IR**
- Eigenschaften, die die Geräteeigenschaft **Nummer**, den **Gerätestandort**, d.h. eine Eigenschaft für den Standort, sowie eine Cloudeigenschaft namens **Hergestellt in** enthält 

Vollständige Informationen zur Konfiguration finden Sie unter [Details zur MXChip-Gerätevorlage](#mxchip-device-template-details).


## <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Fügen Sie in Ihrer Azure IoT Central-Anwendung ein echtes Gerät aus der Gerätevorlage **MXChip** hinzu, und notieren Sie sich die Verbindungsdetails des Geräts (**Bereichs-ID, Geräte-ID und Primärschlüssel**).

1. Fügen Sie über den Device Explorer ein **echtes Gerät** hinzu. Wählen Sie **+ Neu > Echt** aus, um ein echtes Gerät hinzuzufügen.

   * Geben Sie die Geräte-ID **<span style="color:Red">(muss in Kleinbuchstaben angegeben werden)</span>** ein, oder verwenden Sie die vorgeschlagene Geräte-ID.
   * Geben Sie den Gerätenamen ein, oder verwenden Sie den vorgeschlagenen Namen.

     ![Gerät hinzufügen](media/howto-connect-devkit/add-device.png)

1. Rufen Sie Verbindungsdetails wie **Bereichs-ID, Geräte-ID und Primärschlüssel** für das hinzugefügte Gerät ab, indem Sie auf der Geräteseite **Verbinden** auswählen.

    ![Verbindungsdetails](media/howto-connect-devkit/device-connect.png)

1. Es ist wichtig, diese Details zu speichern, da bei der Vorbereitung des DevKit-Geräts Ihre Internetverbindung vorübergehend getrennt wird.

### <a name="prepare-the-devkit-device"></a>Vorbereiten des DevKit-Geräts

> [!NOTE]
> Wenn Sie das Gerät bereits vorher verwendet, WLAN-Anmeldeinformationen gespeichert haben und das Gerät für ein anderes WLAN, eine andere Verbindungszeichenfolge oder eine andere Telemetriemessung neu konfigurieren möchten, drücken Sie am Board gleichzeitig die Tasten **A** und **B**. Wenn dies nicht funktioniert, drücken Sie **Rücksetztaste**, und wiederholen Sie den Vorgang.

#### <a name="to-prepare-the-devkit-device"></a>So bereiten Sie das DevKit-Gerät vor

1. Laden Sie die neueste vorgefertigte Azure IoT Central-Firmware für den MXChip von der Seite [Releases](https://aka.ms/iotcentral-docs-MXChip-releases) in GitHub herunter.
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
    > Wenn auf dem Bildschirm andere Daten angezeigt werden, setzen Sie das Gerät zurück, und drücken Sie auf dem Gerät gleichzeitig die Tasten **A** und **B**, um das Gerät neu zu starten.

1. Das Gerät befindet sich jetzt im Zugriffspunktmodus (Access Point, AP). Sie können von Ihrem Computer oder Mobilgerät aus eine Verbindung mit diesem WLAN-Zugriffspunkt herstellen.

1. Stellen Sie auf Ihrem Computer, Smartphone oder Tablet eine Verbindung mit dem WLAN-Namen her, der auf dem Bildschirm des Geräts angezeigt wird. Wenn Sie eine Verbindung mit diesem Netzwerk herstellen, können Sie nicht auf das Internet zugreifen. Dies ist so beabsichtigt. Sie werden mit diesem Netzwerk nur für kurze Zeit verbunden, solange Sie das Gerät konfigurieren.

1. Öffnen Sie Ihren Webbrowser, und navigieren Sie zu [http://192.168.0.1/start](http://192.168.0.1/start). Die folgende Webseite wird angezeigt:

    ![Seite „Gerätekonfiguration“](media/howto-connect-devkit/configpage.png)

    Fügen Sie auf der Webseite Folgendes hinzu: 
    - Den Namen Ihres WLAN. 
    - Das Kennwort für das WLAN.
    - Den auf dem Geräte-LCD angezeigten PIN-Code. 
    - Die Verbindungsdetails **Bereichs-ID, Geräte-ID und Primärschlüssel** Ihres Geräts. (Diese Details sollten Sie bereits gemäß der Anleitung gespeichert haben.)      
    - Wählen Sie alle verfügbaren Telemetriemessungen aus. 

1. Nachdem Sie **Gerät konfiguriere** ausgewählt haben, wird diese Seite angezeigt:

    ![Gerät konfiguriert](media/howto-connect-devkit/deviceconfigured.png)

1. Drücken Sie an Ihrem Gerät die **Rücksetztaste**.


## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Beim Neustart des DevKit-Geräts wird auf dem Gerätebildschirm Folgendes angezeigt:

* Die Anzahl gesendeter Telemetrienachrichten
* Die Fehleranzahl
* Die Anzahl der erhaltenen gewünschten Eigenschaften und die Anzahl der gesendeten gemeldeten Eigenschaften.

> [!NOTE]
> Sollte das Gerät in einer Anmeldeschleife hängen bleiben, überprüfen Sie, ob das Gerät in IoT Central *blockiert* ist, und *heben Sie die Blockierung des Geräts auf*, um eine Verbindung mit der App zu ermöglichen.

Durch Schütteln des Geräts wird die Anzahl der gesendeten gemeldeten Eigenschaften inkrementell erhöht. Das Gerät sendet eine zufällige Zahl als Geräteeigenschaft **Nummer**.

Sie können die Telemetriemessungen und die gemeldeten Eigenschaftswerte anzeigen und Einstellungen in Azure IoT Central konfigurieren:

1. Verwenden Sie den **Device Explorer**, um die Seite **Messungen** für das reale MXChip-Gerät aufzurufen, das Sie hinzugefügt haben:

    ![Navigieren zum realen Gerät](media/howto-connect-devkit/realdevicenew.png)

1. Auf der Seite **Messungen** sehen Sie die vom MXChip-Gerät gesendeten Telemetriedaten:

    ![Anzeigen der Telemetrie vom realen Gerät](media/howto-connect-devkit/devicetelemetrynew.png)

1. Auf der Seite **Eigenschaften** sehen Sie die Nummer und den Gerätestandort, die zuletzt vom Gerät gemeldet wurden:

    ![Anzeigen von Geräteeigenschaften](media/howto-connect-devkit/devicepropertynew.png)

1. Auf der Seite **Einstellungen** können Sie die Einstellungen für das MXChip-Gerät aktualisieren:

    ![Anzeigen der Geräteeinstellungen](media/howto-connect-devkit/devicesettingsnew.png)

1. Auf der Seite **Dashboard** sehen Sie die Standortzuordnung.

    ![Anzeigen des Gerätedashboards](media/howto-connect-devkit/devicedashboardnew.png)


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

## <a name="mxchip-device-template-details"></a>Details zur MXChip-Gerätevorlage

Eine Anwendung, die mit der Anwendungsvorlage „Beispiel-DevKits“ erstellt wurde, enthält eine MXChip-Gerätevorlage mit den folgenden Eigenschaften:

### <a name="measurements"></a>Messungen

#### <a name="telemetry"></a>Telemetrie 

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


#### <a name="states"></a>Zustände 
| NAME          | Anzeigename   | NORMAL | VORSICHT | GEFAHR | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Gerätestatus   | Grün  | Orange  | Rot    | 

#### <a name="events"></a>Ereignisse 
| NAME             | Anzeigename      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Schaltfläche „B“ gedrückt  | 

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

| Type            | Anzeigename | Feldname | Datentyp |
| --------------- | ------------ | ---------- | --------- |
| Geräteeigenschaft | Nummer   | dieNumber  | number    |
| Geräteeigenschaft | Gerätestandort   | location  | location    |
| Text            | Hergestellt in     | manufacturedIn   | –       |



## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie ein DevKit-Gerät mit Ihrer Azure IoT Central-Anwendung verbunden wird, werden als Nächstes die folgenden Schritte empfohlen:

* [Vorbereiten und Verbinden eines Raspberry Pi](howto-connect-raspberry-pi-python.md)
