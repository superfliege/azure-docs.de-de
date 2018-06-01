---
title: Gerätekonnektivität in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel werden die wichtigsten Konzepte in Bezug auf die Gerätekonnektivität in Azure IoT Central vorgestellt.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 00b621a4635ef1ceda26772ac5876fa2599b56f8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34202644"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Gerätekonnektivität in Azure IoT Central

In diesem Artikel werden die wichtigsten Konzepte in Bezug auf die Gerätekonnektivität in Microsoft Azure IoT Central vorgestellt.

Alle Geräte, die sich mit Ihrer Azure IoT Central-Anwendung verbinden, stellen diese Verbindung mit den [Endpunkten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints) her, die von dem von Azure IoT Central verwendeten IoT Hub verfügbar gemacht werden. IoT Hub ermöglicht skalierbare, sichere und zuverlässige Verbindungen über die verbundenen Geräte.

## <a name="sdk-support"></a>SDK-Unterstützung

Die Azure-Geräte-SDKs bieten Ihnen die einfachste Möglichkeit zum Implementieren des Codes auf Ihren Geräten, über den eine Verbindung mit Ihrer Azure IoT Central-Anwendung hergestellt wird. Derzeit sind folgende Geräte-SDKs verfügbar:

- [Azure IoT-SDK für C](https://github.com/azure/azure-iot-sdk-c)
- [Azure IoT-SDK für Python](https://github.com/azure/azure-iot-sdk-python)
- [Azure IoT-SDK für Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Azure IoT-SDK für Java](https://github.com/azure/azure-iot-sdk-java)
- [Azure IoT-SDK für .NET](https://github.com/azure/azure-iot-sdk-csharp)

Jedes Gerät stellt eine Verbindung über eine eindeutige Verbindungszeichenfolge her, die das Gerät identifiziert. Ein Gerät kann sich nur mit dem IoT Hub verbinden, bei dem es registriert ist. Wenn Sie ein reales Gerät in Ihrer Azure IoT Central-Anwendung erstellen, generiert die Anwendung eine Verbindungszeichenfolge.

## <a name="sdk-features-and-iot-hub-connectivity"></a>SDK-Features und IoT Hub-Konnektivität

Bei der gesamten Gerätekommunikation mit IoT Hub werden die folgenden IoT Hub-Konnektivitätsoptionen verwendet:

- [Nachrichten, die von Geräten an die Cloud gesendet werden](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Gerätezwillinge](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

In der folgenden Tabelle wird die Zuordnung von Azure IoT Central-Gerätefeatures zu IoT Hub-Features zusammengefasst:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Messung: Telemetrie | Nachrichten, die von Geräten an die Cloud gesendet werden |
| Geräteeigenschaften | Gemeldete Eigenschaften von Gerätezwillingen |
| Einstellungen | Gewünschte und gemeldete Eigenschaften von Gerätezwillingen |

Weitere Informationen zur Verwendung der Geräte-SDKs sowie Beispielcode finden Sie in einem der folgenden Artikel:

- [Verbinden eines generischen Node.js-Clients mit Ihrer Azure IoT Central-Anwendung](howto-connect-nodejs.md)
- [Verbinden eines Raspberry Pi-Geräts mit Ihrer Azure IoT Central-Anwendung](howto-connect-raspberry-pi-python.md)
- [Verbinden eines DevDiv-Kit-Geräts mit Ihrer Azure IoT Central-Anwendung](howto-connect-devkit.md)

Das folgende Video bietet einen Überblick über die Herstellung einer Verbindung zwischen einem realen Gerät und Azure IoT Central:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Connect-real-devices-to-Microsoft-IoT-Central/Player]

## <a name="protocols"></a>Protokolle

Die Geräte-SDKs unterstützen die folgenden Netzwerkprotokolle zum Herstellen einer Verbindung mit einem IoT Hub:

- MQTT
- AMQP
- HTTPS

Informationen zu diesen verschiedenen Protokollen sowie eine Anleitung zu deren Auswahl finden Sie unter [Auswählen eines Kommunikationsprotokolls](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Wenn Ihr Gerät keines der unterstützten Protokolle verwenden kann, können Sie mithilfe von Azure IoT Edge eine Protokollkonvertierung durchführen. IoT Edge unterstützt weitere Intelligence-on-the-Edge-Szenarien, um die Verarbeitung von der Azure IoT Central-Anwendung auf den Edge auszulagern.

## <a name="security"></a>Sicherheit

Alle Daten, die zwischen Geräten und Azure IoT Central ausgetauscht werden, werden verschlüsselt. IoT Hub authentifiziert jede Anforderung von einem Gerät, das eine Verbindung mit einem der geräteseitigen IoT Hub-Endpunkte herstellt. Um den unverschlüsselten Austausch von Anmeldeinformationen zu vermeiden, verwendet ein Gerät signierte Token zur Authentifizierung. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Derzeit müssen Geräte, die eine Verbindung mit Azure IoT Central herstellen, SAS-Token verwenden. X.509-Zertifikate werden für Geräte, die eine Verbindung mit Azure IoT Central herstellen, nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit der Gerätekonnektivität in Azure IoT Central vertraut gemacht haben, werden Ihnen als Nächstes die folgenden Schritte empfohlen:

- [Vorbereiten und Verbinden eines DevKit-Geräts](howto-connect-devkit.md)
- [Vorbereiten und Verbinden eines Raspberry Pi](howto-connect-raspberry-pi-python.md)
- [Verbinden eines generischen Node.js-Clients mit Ihrer Azure IoT Central-Anwendung](howto-connect-nodejs.md)
