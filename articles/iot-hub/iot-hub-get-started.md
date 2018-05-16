---
title: 'Azure IoT Hub: Erste Schritte beim Herstellen von Verbindungen zwischen IoT-Geräten und der Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Ihre IoT-Boards und Starter Kits mit Azure IoT Hub verbinden. Ihre Geräte können Telemetriedaten an IoT Hub senden, und IoT Hub kann Ihre Geräte überwachen und verwalten.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
keywords: Tutorial zu Azure IoT Hub
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: dafb8aca34a5a41f45f76d526aa3b8f3b1b792c4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="azure-iot-hub-get-started-with-real-devices"></a>Erste Schritte für Azure IoT Hub mit realen Geräten

Mit Azure IoT Hub und den Azure IoT-Geräte-SDKs können Sie IoT-Lösungen (Internet of Things, Internet der Dinge) entwickeln:

* Azure IoT Hub ist ein vollständig verwalteter Dienst in der Cloud, der Ihre IoT-Geräte sicher verbindet, überwacht und verwaltet. Verwenden Sie die Azure IoT-Geräte-SDKs, um Ihre IoT-Geräte zu implementieren.
* Verwenden Sie ein IoT-Gateway in komplexeren IoT-Szenarien, z.B., wenn Sie Faktoren wie veraltete Geräte, Kosten für Bandbreite, Sicherheit, Datenschutzrichtlinien oder Edgedatenverarbeitung berücksichtigen müssen. Verwenden Sie in diesen Szenarien [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) zum Implementieren eines Gateways, das Geräte mit Ihrem IoT Hub verbindet.

## <a name="what-the-how-to-articles-cover"></a>Was decken die „Gewusst-wie“-Artikel ab?

Diese Artikel dienen als Einführung in Azure IoT Hub und die Geräte-SDKs. In den Artikeln werden allgemeine IoT-Szenarien behandelt, um die Fähigkeiten von IoT Hub zu veranschaulichen. Die Artikel erläutern auch das Kombinieren von IoT Hub mit anderen Azure-Diensten und -Tools zum Erstellen leistungsfähiger IoT-Lösungen. In den Artikeln verwenden Sie reale IoT-Geräte.

## <a name="set-up-your-device"></a>Einrichten des Geräts

Stellen Sie die Verbindung eines IoT-Geräts oder -Gateways mit Azure IoT Hub her:

| IoT-Gerät                       | Programmiersprache |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                       | [Arduino in Visual Studio Code][DevKit]     |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Adafruit Feather HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 Thing Dev       | [Arduino][Th_Ard]              |
| Adafruit Feather M0              | [Arduino][M0_Ard]              |
| Onlinegerätesimulator         | [Raspberry Pi (Node.js)][Ol_Sim] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
