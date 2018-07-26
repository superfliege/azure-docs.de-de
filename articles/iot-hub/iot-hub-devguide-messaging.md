---
title: Grundlegendes zu Azure IoT Hub-Messaging | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Messaging zwischen Geräten und Cloud mit IoT Hub. Enthält Informationen zu Nachrichtenformaten und unterstützten Kommunikationsprotokollen.'
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: b0667f820145f16c75a07ebe1849e20d2de36cc7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185508"
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Messaging zwischen Geräten und Cloud mit IoT Hub

Verwenden Sie IoT Hub-Messaging für die Kommunikation mit Ihren Geräten durch:

* Senden von [D2C][lnk-d2c]-Nachrichten von Ihren Geräten an Ihr Lösungs-Back-End
* Senden von [C2D][lnk-c2d]-Nachrichten vom Lösungs-Back-End an Ihre Geräte

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Die wichtigsten Eigenschaften beim IoT Hub-Messaging sind eine zuverlässige und stabile Übermittlung von Nachrichten. Diese Eigenschaften ermöglichen Ausfallsicherheit bei zeitweiligen Verbindungsproblemen auf Geräteseite und Lastspitzen bei der Ereignisverarbeitung auf Cloudseite. IoT Hub implementiert *mindestens einmal* Übermittlungsgarantien für das D2C- und C2D-Messaging.

Eine Einführung in die Funktionen von IoT Hub finden Sie unter [Übersicht über den Azure IoT Hub-Dienst][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Szenarien für die Verwendung von IoT Hub-Messaging

Sie verwenden D2C-Nachrichten zum Senden von Zeitreihen-Telemetriedaten und Warnungen von Ihrer Geräte-App und von C2D-Nachrichten für unidirektionale Nachrichten an Ihre Geräte-App.

* Wenn Sie Entscheidungshilfen für die Verwendung von D2C-Nachrichten, gemeldeten Eigenschaften oder Dateiuploads benötigen, lesen Sie den [Leitfaden zur D2C-Kommunikation][lnk-d2c-guidance].
* Wenn Sie Entscheidungshilfen für die Verwendung von C2D-Nachrichten gemeldeten Eigenschaften oder direkten Methoden benötigen, lesen Sie den [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance].

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das IoT Hub-[D2C-Messaging][lnk-d2c].
* Erfahren Sie mehr über das IoT Hub-[C2D-Messaging][lnk-c2d].

[lnk-azure-iot]: ../iot-fundamentals/index.yml
[lnk-iot-hub-overview]: about-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md