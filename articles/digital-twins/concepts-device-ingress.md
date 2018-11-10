---
title: Gerätekonnektivität und eingehende Telemetriedaten mit Azure Digital Twins | Microsoft-Dokumentation
description: Übersicht über das Einbinden (Onboarding) eines Geräts mit Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 7eddea7e0d57b89318232da6f086bbe2f649ee77
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50211926"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Gerätekonnektivität und eingehende Telemetriedaten

Die von Geräten und Sensoren gesendeten Telemetriedaten bilden das Rückgrat jeder IoT-Lösung. Daher ist es bei der Entwicklung von IoT-Apps ein Hauptanliegen, diese unterschiedlichen Ressourcen darzustellen und im Kontext eines Standorts zu verwalten. Azure Digital Twins vereinfacht die Entwicklung von IoT-Lösungen, indem Geräte und Sensoren in einem Raumintelligenzgraphen kombiniert werden.

Zunächst sollte eine IoT Hub-Ressource im Stammknoten des Bereichsgraphen erstellt werden, sodass alle Geräte unter dem Stammraum Nachrichten senden können. Sobald der IoT Hub erstellt wurde und Geräte mit Sensoren in der Digital Twins-Instanz registriert wurden, können die Geräte beginnen, Daten über das [Azure IoT-Geräte-SDK](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks#azure-iot-device-sdks) an einen Digital Twins-Dienst zu senden.

Eine ausführliche Anleitung zum Einbinden von Geräten finden Sie im [Tutorial zum Bereitstellen und Konfigurieren von Digital Twins](tutorial-facilities-setup.md). In Kurzform sind dies folgende Schritte:

- Bereitstellen einer Azure Digital Twins-Instanz aus dem [Azure-Portal](https://portal.azure.com)
- Erstellen von Räumen in Ihrem Graphen
- Erstellen einer IoT Hub-Ressource und Zuweisen dieser Ressource zu einem Bereich in Ihrem Graphen
- Erstellen von Geräten und Sensoren in Ihrem Graphen und Zuweisen dieser Geräte und Sensoren zu den in den vorherigen Schritten erstellten Räumen
- Erstellen eines Matchers (Abgleicher), um Telemetrienachrichten anhand von Bedingungen zu filtern
- Erstellen einer [benutzerdefinierten Funktion](concepts-user-defined-functions.md) und Zuweisen dieser Funktion zu einem Raum im Graphen, sodass eine angepasste Verarbeitung der Telemetrienachrichten erfolgt
- Zuweisen einer Rolle, damit die benutzerdefinierte Funktion auf die Graphdaten zugreifen kann
- Abrufen der IoT Hub-Verbindungszeichenfolge des Geräts aus den Digital Twins-Verwaltungs-APIs
- Konfigurieren der Verbindungszeichenfolge des Geräts im Gerät mit dem Azure IoT-Geräte-SDK

Im Folgenden erfahren Sie, wie Sie die IoT Hub-Verbindungszeichenfolge eines Geräts aus der Digital Twins-Verwaltungs-API abrufen und wie Sie das IoT Hub-Telemetrienachrichtenformat verwenden, um sensorbasierte Telemetriedaten zu senden. Digital Twins erfordert, dass jegliche von ihm empfangenen Telemetriedaten mit einem Sensor im Raumgraphen verknüpft sind, damit sichergestellt ist, dass die Daten im richtigen räumlichen Kontext verarbeitet und weitergeleitet werden.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>Abrufen der IoT Hub-Verbindungszeichenfolge eines Geräts aus der Verwaltungs-API

Führen Sie einen GET-Aufruf für die Geräte-API mit einem `includes=ConnectionString`-Parameter aus, um die IoT Hub-Verbindungszeichenfolge des Geräts abzurufen. Sie können nach Geräte-GUID oder Hardware-ID filtern, um das angegebene Gerät zu finden:

```plaintext
https://yourManagementApiUrl/api/v1.0/devices/yourDeviceGuid?includes=ConnectionString
```

```plaintext
https://yourManagementApiUrl/api/v1.0/devices?hardwareIds=yourDeviceHardwareId&includes=ConnectionString
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| *yourManagementApiUrl* | Der vollständige URL-Pfad für Ihre Verwaltungs-API |
| *yourDeviceGuid* | Der Geräte-ID |
| *yourDeviceHardwareId* | Der Gerätehardware-ID |

Kopieren Sie aus der Antwortnutzlast die *connectionString*-Eigenschaft des Geräts. Diese Eigenschaft verwenden Sie, wenn Sie das Azure IoT-Geräte-SDK aufrufen, um Daten an Azure Digital Twins zu senden.

## <a name="device-to-cloud-message"></a>Gerät-zu-Cloud-Nachricht

Sie können das Nachrichtenformat und die Nutzlast Ihres Geräts entsprechend den Anforderungen Ihrer Lösung anpassen. Sie können jeden Datenvertrag verwenden, der in ein Bytearray oder einen Datenstrom serialisiert werden kann, das oder der von [der Klasse für Clientnachrichten für Azure IoT-Geräte, Message(Byte[] byteArray)](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___) unterstützt wird. Die Nachricht kann in einem beliebigen benutzerdefinierten Binärformat vorliegen, solange Sie den Datenvertrag in einer entsprechenden benutzerdefinierten Funktion decodieren. Die einzige Anforderung für eine Gerät-zu-Cloud-Nachricht besteht darin, dass ein Satz von Eigenschaften (siehe unten) verwaltet wird, um sicherzustellen, dass die Nachricht ordnungsgemäß an die Verarbeitungs-Engine weitergeleitet wird.

### <a name="telemetry-properties"></a>Telemetrieeigenschaften

Während der Nutzlastinhalt einer **Message** aus beliebigen Daten bis zu einer Größe von 256 KB bestehen kann, gibt es einige Anforderungen für erwartete [`Message.Properties`](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet). Die nachstehend skizzierten Schritte entsprechen den erforderlichen und optionalen Eigenschaften, die vom System unterstützt werden:

| Eigenschaftenname | Wert | Erforderlich | BESCHREIBUNG |
|---|---|---|---|
| *DigitalTwins-Telemetry* | 1.0 | JA | Ein konstanter Wert, der eine Nachricht an das System kennzeichnet |
| *DigitalTwins-SensorHardwareId* | `string(72)` | JA | Ein eindeutiger Bezeichner des Sensors, der die **Message** sendet. Dieser Wert muss mit der **HardwareId**-Eigenschaft eines Objekts übereinstimmen, damit es vom System verarbeitet wird. Zum Beispiel, `00FF0643BE88-CO2` |
| *CreationTimeUtc* | `string` | Nein  | Eine gemäß [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formatierte Datumszeichenfolge, die die Abtastzeit der Nutzlast kennzeichnet. Zum Beispiel, `2018-09-20T07:35:00.8587882-07:00` |
| *CorrelationId* | `string` | Nein  | Eine UUID, mit der Ereignisse im gesamten System verfolgt werden können. Zum Beispiel, `cec16751-ab27-405d-8fe6-c68e1412ce1f`

### <a name="sending-your-message-to-digital-twins"></a>Senden einer Nachricht an Digital Twins

Verwenden Sie den „DeviceClient“-Aufruf [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) oder [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet), um eine Nachricht an Ihren Digital Twins-Dienst zu senden.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Datenverarbeitung von Azure Digital Twins und zu den Möglichkeiten von benutzerdefinierten Funktionen in Azure Digital Twins finden Sie unter [Datenverarbeitung und benutzerdefinierte Funktionen](concepts-user-defined-functions.md).

