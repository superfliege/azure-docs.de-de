---
title: Herstellen einer Verbindung zwischen einem Windows IoT Core-Gerät und Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Geräteentwickler ein MXChip IoT DevKit-Gerät mit Ihrer Azure IoT Central-Anwendung verbinden.
author: miriambrus
ms.author: miriamb
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 7d593a992d0ff189d23185b3422dee86a55308d1
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309449"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Herstellen einer Verbindung zwischen einem Windows IoT Core-Gerät und Ihrer Azure IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Geräteentwickler eine Verbindung zwischen einem Windows IoT Core-Gerät und Ihrer Microsoft Azure IoT Central-Anwendung herstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

1. Eine Azure IoT Central-Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central.md).
2. Ein Gerät, auf dem das Betriebssystem Windows 10 IoT Core ausgeführt wird. In dieser exemplarischen Vorgehensweise wird Raspberry Pi verwendet.


## <a name="sample-devkits-application"></a>Anwendungsvorlage **Beispiel-DevKits**

Eine Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde, enthält eine **Windows IoT Core**-Gerätevorlage mit folgenden Merkmalen: 

- Telemetriedaten, die die Messwerte für das Gerät enthält: **Luftfeuchtigkeit**, **Temperatur** und **Druck** 
- Einstellungen, die **Lüftergeschwindigkeit** zeigen
- Eigenschaften mit der Geräteeigenschaft **Nummer** und der Cloudeigenschaft **Standort**


Vollständige Informationen zur Konfiguration der Gerätevorlage finden Sie unter [Details zur Windows IoT Core-Gerätevorlage](howto-connect-windowsiotcore.md#windows-iot-core-device-template-details).

## <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Fügen Sie in Ihrer Azure IoT Central-Anwendung ein echtes Gerät aus der Gerätevorlage **Windows IoT Core** hinzu, und notieren Sie sich die Verbindungszeichenfolge des Geräts. Weitere Informationen finden Sie unter [Hinzufügen eines echten Geräts zu Ihrer Azure IoT Central-Anwendung](tutorial-add-device.md).

### <a name="prepare-the-windows-iot-core-device"></a>Vorbereiten des Windows IoT Core-Geräts

Befolgen Sie zum Einrichten eines Windows IoT Core-Geräts die Anweisungen im ausführlichen Leitfaden unter [Einrichten eines Windows IoT Core-Geräts](https://github.com/Azure/iot-central-firmware/tree/master/WindowsIoT#setup-a-physical-device).

### <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Fügen Sie in Ihrer Azure IoT Central-Anwendung ein echtes Gerät aus der Gerätevorlage **Windows IoT Core** hinzu, und notieren Sie sich die Verbindungsdetails des Geräts (**Bereichs-ID, Geräte-ID und Primärschlüssel**). Weitere Informationen finden Sie unter [Hinzufügen eines echten Geräts zu Ihrer Azure IoT Central-Anwendung](tutorial-add-device.md).

 > [!NOTE]
   > Azure IoT Central verwendet nun Azure IoT Hub Device Provisioning Service für alle Geräteverbindungen. Führen Sie die Schritte zum [Abrufen der Verbindungszeichenfolge des Geräts](concepts-connectivity.md#getting-device-connection-string) aus, und fahren Sie dann mit dem Tutorial fort.

## <a name="prepare-the-windows-10-iot-core-device"></a>Vorbereiten des Windows 10 IoT Core-Geräts

### <a name="what-youll-need"></a>Voraussetzungen

Um ein physisches Windows 10 IoT Core-Gerät einzurichten, benötigen Sie zunächst ein Gerät, auf dem Windows 10 IoT Core ausgeführt wird. Informationen zum Einrichten eines Windows 10 IoT Core-Geräts finden Sie [hier](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup).

Sie benötigen außerdem eine Clientanwendung, die mit Azure IoT Central kommunizieren kann. Sie können entweder mit dem Azure SDK eine eigene Anwendung erstellen und mit Visual Studio für Ihr Gerät bereitstellen oder ein [vorgefertigtes Beispiel](https://developer.microsoft.com/windows/iot/samples) herunterladen und einfach für das Gerät bereitstellen und darauf ausführen. 

### <a name="deploying-the-sample-client-application"></a>Bereitstellen der Beispielclientanwendung

Im Folgenden erfahren Sie, wie Sie die Clientanwendung aus dem vorherigen Schritt zur Vorbereitung auf Ihrem Windows 10 IoT-Gerät bereitstellen:

**Sicherstellen, dass die Verbindungszeichenfolge für die Verwendung durch die Clientanwendung auf dem Gerät gespeichert ist**
* Speichern Sie auf dem Desktop die Verbindungszeichenfolge in einer Textdatei namens „connection.string.iothub“.
* Kopieren Sie die Textdatei in den Dokumentordner des Geräts: `[device-IP-address]\C$\Data\Users\DefaultAccount\Documents\connection.string.iothub`

Rufen Sie anschließend das [Windows-Geräteportal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal) auf, indem Sie in einen beliebigen Browser „http://[Geräte-IP-Adresse]:8080“ eingeben.

In dieser Ansicht sollten Sie wie unten gezeigt folgende Vorgänge ausführen:
1. Erweitern Sie auf der linken Seite den Knoten „Apps“.
2. Klicken Sie auf „Beispiele zur schnellen Ausführung“.
3. Klicken Sie auf „Azure IoT Hub-Client“.
4. Klicken Sie auf „Bereitstellen und ausführen“.

![GIF des Azure IoT Hub-Clients im Windows-Geräteportal](./media/howto-connect-windowsiotcore/iothubapp.gif)

Bei erfolgreicher Ausführung wird die Anwendung auf dem Gerät gestartet und sieht wie folgt aus:

![Screenshot der Azure IoT Hub-Client-App](./media/howto-connect-windowsiotcore/IoTHubForegroundClientScreenshot.png)

In Azure IoT Central können Sie sehen, wie der auf dem Raspberry Pi-Gerät ausgeführte Code mit der Anwendung interagiert:

* Auf der Seite **Messungen** für Ihr echtes Gerät können Sie die Telemetrie sehen.
* Auf der Seite **Eigenschaften** können Sie den Wert der gemeldeten „Nummer“-Eigenschaft sehen.
* Auf der Seite **Einstellungen** können Sie verschiedene Einstellungen am Raspberry Pi-Gerät ändern, wie z.B. Spannung und Lüfterdrehzahl.

## <a name="download-the-source-code"></a>Herunterladen des Quellcodes

Wenn Sie den Quellcode für die Clientanwendung weiter untersuchen und bearbeiten möchten, können Sie diesen [hier](https://github.com/Microsoft/Windows-iotcore-samples/tree/develop/Samples/Azure/IoTHubClients) von GitHub herunterladen. Wenn Sie den Code bearbeiten möchten, befolgen Sie die Anweisungen für Ihr Desktopbetriebssystem in [dieser Infodatei](https://github.com/Microsoft/Windows-iotcore-samples).

> [!NOTE]
> Wenn **Git** in Ihrer Entwicklungsumgebung nicht installiert ist, können Sie es von [https://git-scm.com/download](https://git-scm.com/download) herunterladen.

## <a name="windows-iot-core-device-template-details"></a>Details zur Windows IoT Core-Gerätevorlage

Eine Anwendung, die mit der Anwendungsvorlage **Beispiel-Entwickler-Kits** erstellt wurde, enthält eine **Windows IoT Core**-Gerätevorlage mit folgenden Merkmalen:

### <a name="telemetry-measurements"></a>Telemetriemessungen

| Feldname     | Units  | Minimum | Maximum | Dezimalstellen |
| -------------- | ------ | ------- | ------- | -------------- |
| Luftfeuchtigkeit       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Einstellungen

Numerische Einstellungen

| Anzeigename | Feldname | Units | Dezimalstellen | Minimum | Maximum | Initial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Lüfterdrehzahl    | fanSpeed   | U/Min   | 0              | 0       | 1000    | 0       |


### <a name="properties"></a>Eigenschaften

| Typ            | Anzeigename | Feldname | Datentyp |
| --------------- | ------------ | ---------- | --------- |
| Geräteeigenschaft | Nummer   | dieNumber  | number    |
| Text            | Standort     | location   | N/V       |
