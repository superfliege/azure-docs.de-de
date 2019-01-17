---
title: Herstellen der Verbindung eines Raspberry Pi-Geräts mit Ihrer Azure IoT Central-Anwendung (Python) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Geräteentwickler ein Raspberry Pi-Gerät mit Ihrer Azure IoT Central-Anwendung über Python verbinden.
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 9f39832b50ed983e7d8a0bfc0a06366870717fa3
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051984"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Verbinden eines Raspberry Pi-Geräts mit Ihrer Azure IoT Central-Anwendung (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

In diesem Artikel wird beschrieben, wie Sie als Geräteentwickler ein Raspberry Pi-Gerät mithilfe der Programmiersprache Python mit Ihrer Microsoft Azure IoT Central-Anwendung verbinden.

## <a name="before-you-begin"></a>Voraussetzungen

Um die in diesem Artikel aufgeführten Schritte auszuführen, benötigen Sie die folgenden Komponenten:

* Eine Azure IoT Central-Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central.md).
* Sie verfügen über ein Raspberry Pi-Gerät, auf dem das Raspbian-Betriebssystem ausgeführt wird. Für den Zugriff auf die GUI-Umgebung müssen ein Monitor, eine Tastatur und eine Maus an Ihren Raspberry Pi angeschlossen sein. Der Raspberry Pi muss eine [Verbindung mit dem Internet herstellen](https://www.raspberrypi.org/learning/software-guide/wifi/) können.
* Optional brauchen Sie ein [Sense Hat](https://www.raspberrypi.org/products/sense-hat/)-Add-On-Board für den Raspberry Pi. Dieses Board erfasst Telemetriedaten von verschiedenen Sensoren, um sie an Ihre Azure IoT Central-Anwendung zu senden. Wenn Sie kein **Sense Hat**-Board besitzen, können Sie stattdessen einen Emulator verwenden (verfügbar im Rahmen des Raspberry Pi-Images).

## <a name="sample-devkits-application"></a>Anwendungsvorlage **Beispiel-DevKits**

Eine Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde, enthält eine Gerätevorlage **Raspberry Pi** mit den folgenden Eigenschaften: 

- Telemetriedaten, darunter die folgenden vom Gerät gesammelten Messungen:
    - Luftfeuchtigkeit
    - Temperatur
    - Pressure
    - Magnetometer (X, Y, Z)
    - Beschleunigungsmesser (X, Y, Z)
    - Gyroskop (X, Y, Z)
- Einstellungen
    - Spannung
    - Aktuell
    - Lüfterdrehzahl
    - IR-Umschalter
- Eigenschaften
    - Geräteeigenschaft „Nummer“
    - Standortcloudeigenschaft

Vollständige Informationen zur Konfiguration der Gerätevorlage finden Sie unter [Details zur Raspberry Pi-Gerätevorlage](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details).
    

## <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Fügen Sie in Ihrer Azure IoT Central-Anwendung ein echtes Gerät aus der **Raspberry Pi**-Gerätevorlage hinzu, und bewahren Sie die Verbindungsdetails des Geräts (**Bereichs-ID, Geräte-ID, Primärschlüssel**) auf. Weitere Informationen finden Sie unter [Hinzufügen eines echten Geräts zu Ihrer Azure IoT Central-Anwendung](tutorial-add-device.md).


### <a name="configure-the-raspberry-pi"></a>Konfigurieren des Raspberry Pi

Die folgenden Schritte beschreiben das Herunterladen und Konfigurieren der Python-Beispielanwendung über GitHub. Diese Beispielanwendung führt Folgendes durch:

* Senden von Telemetriedaten und Eigenschaftswerten an Azure IoT Central
* Reagieren auf Einstellungsänderungen, die in Azure IoT Central vorgenommen werden

[Befolgen Sie die ausführlichen Anweisungen auf GitHub](https://aka.ms/iotcentral-docs-Raspi-releases), um das Gerät zu konfigurieren.


> [!NOTE]
> Weitere Informationen zum Raspberry Pi-Python-Beispiel finden Sie in der Datei [README](https://aka.ms/iotcentral-docs-Raspi-releases) auf GitHub.


1. Wenn das Gerät konfiguriert wurde, beginnt es direkt mit dem Senden von Daten an Azure IoT Central.
1. In Ihrer Azure IoT Central-Anwendung sehen Sie, wie der auf dem Raspberry Pi-Gerät ausgeführte Code mit der Anwendung interagiert:

    * Auf der Seite **Messungen** für Ihr echtes Gerät werden Ihnen die vom Raspberry Pi gesendeten Telemetriedaten angezeigt. Bei Verwendung des **Sense HAT-Emulators** können Sie die Telemetriewerte in der GUI auf dem Raspberry Pi ändern.
    * Auf der Seite **Eigenschaften** sehen Sie den Wert der gemeldeten **Nummer**-Eigenschaft.
    * Auf der Seite **Einstellungen** können Sie verschiedene Einstellungen am Raspberry Pi-Gerät ändern, wie z.B. Spannung und Lüfterdrehzahl. Wenn der Raspberry Pi die Änderung bestätigt, wird die Einstellung in Azure IoT Central als **synchronisiert** angezeigt.


## <a name="raspberry-pi-device-template-details"></a>Details zur Raspberry Pi-Gerätevorlage

Eine Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde, enthält eine Gerätevorlage **Raspberry Pi** mit den folgenden Eigenschaften:

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
| Text            | Standort     | location   | N/V       |

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie ein Raspberry Pi-Gerät mit Ihrer Azure IoT Central-Anwendung verbunden wird, werden als Nächstes die folgenden Schritte empfohlen:

* [Verbinden einer generischen Node.js-Clientanwendung mit Azure IoT Central](howto-connect-nodejs.md)
