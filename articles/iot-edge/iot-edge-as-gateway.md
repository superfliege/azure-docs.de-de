---
title: Verwenden von Azure IoT Edge-Geräten als Gateways | Microsoft-Dokumentation
description: Verwenden Sie Azure IoT Edge zum Erstellen eines transparenten oder nicht transparenten Gateway-Geräts oder eines Proxy-Gateway-Geräts, das Daten von mehreren nachgeschalteten Geräten in die Cloud sendet oder lokal verarbeitet.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/27/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ff5e50df1f7a9059bf37fe99d89050a48aa6e64e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631597"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway---preview"></a>Verwendung eines IoT Edge-Geräts als Gateway – Vorschauversion

Der Zweck von Gateways in IoT-Lösungen ist lösungsspezifisch und vereint Gerätekonnektivität mit Edgeanalysen. Mit Azure IoT Edge lassen sich alle Anforderungen an ein IoT-Gateway im Bezug auf Konnektivität, Identität und Edgeanalysen erfüllen. Gatewaymuster in diesem Artikel beziehen sich nur auf Merkmale der Konnektivität und Identität nachgeschalteter Geräte, nicht auf die Verarbeitung von Gerätedaten auf dem Gateway.

## <a name="patterns"></a>Muster
Es gibt drei Muster für die Verwendung eines IoT Edge-Geräts als Gateway: transparent, Protokollübersetzung und Identitätsübersetzung:
* **Transparent**: Geräte, die theoretisch eine Verbindung mit dem IoT Hub herstellen könnten, können stattdessen eine Verbindung mit einem Gatewaygerät herstellen. Dies bedeutet, dass die nachgeschalteten Geräte über ihre eigenen IoT Hub-Identitäten verfügen und MQTT-, AMQP- oder HTTP-Protokolle verwenden. Das Gateway übergibt einfach die Kommunikation zwischen den Geräten und IoT Hub. Die Geräte erkennen nicht, dass sie über ein Gateway mit der Cloud kommunizieren, und auch ein Benutzer, der mit den Geräten in IoT Hub interagiert, weiß nichts vom zwischengeschalteten Gatewaygerät. Daher ist das Gateway transparent. Einzelheiten zur Verwendung eines IoT Edge-Geräts als transparentes Gateway finden Sie in der Vorgehensweise [Erstellen eines transparenten Gateways][lnk-iot-edge-as-transparent-gateway].
* **Protokollübersetzung**: Geräte, die MQTT, AMQP oder HTTP nicht unterstützen, senden Daten über ein Gatewaygerät an IoT Hub. Das Gateway ist intelligent genug, um das von den nachgeschalteten Geräten verwendete Protokoll zu verstehen; es handelt sich jedoch um einzige Gerät mit Identität in IoT Hub. Alle Informationen scheinen von einem Gerät – dem Gateway – zu stammen. Dies bedeutet, dass nachgeschaltete Geräte zusätzliche Informationen in ihre Nachrichten einbetten müssen, wenn Cloudanwendungen die Daten pro Gerät begründen möchten. Außerdem sind IoT Hub-Methoden und -Grundtypen wie Zwillinge nur für das Gateway-Gerät und nicht für nachgeschaltete Geräte verfügbar.
* **Identitätsübersetzung**: Geräte, die keine Verbindung mit IoT Hub herstellen können, stellen eine Verbindung mit einem Gatewaygerät her, das im Namen der nachgeschalteten Geräte IoT Hub-Identität und Protokollübersetzung bereitstellt. Das Gateway ist intelligent genug, um das von den nachgeschalteten Geräten verwendete Protokoll zu verstehen, ihnen Identität bereitzustellen und IoT Hub-Grundtypen zu übersetzen. Nachgeschaltete Geräte werden in IoT Hub als erstrangige Geräte mit Zwillingen und Methoden angezeigt. Ein Benutzer kann mit den Geräten in IoT Hub interagieren, weiß jedoch nichts vom zwischengeschalteten Gatewaygerät.

![Diagramme von Gatewaymustern][1]

## <a name="use-cases"></a>Anwendungsfälle
Alle Gatewaymuster bieten folgende Vorteile:
* **Edgeanalysen**: Verwenden Sie lokal KI-Dienste zur Verarbeitung von Daten, die von nachgeschalteten Geräten stammen, ohne vollständige Telemetriedanten in die Cloud zu senden. Suchen und reagieren Sie lokal auf Informationen, senden Sie nur eine Teilmenge von Daten an IoT Hub. 
* **Isolierung von nachgeschalteten Geräten**: Das Gatewaygerät kann alle nachgeschalteten Geräte vor dem Zugriff über das Internet schützen. Es kann sich zwischen einem OT-Netzwerk ohne Konnektivität und einem IT-Netzwerk mit Internetzugriff befinden. 
* **Multiplexing der Verbindung**: Alle Geräte, die über ein IoT Edge-Gerät mit IoT Hub verbunden sind, verwenden dieselbe zugrunde liegende Verbindung.
* **Glättung des Datenverkehrs**: Das IoT Edge-Gerät implementiert im Fall einer IoT Hub-Drosselung automatisch exponentiellen Backoff, wobei die Nachrichten lokal beibehalten werden. Dadurch wird Ihre Lösung unempfindlich gegenüber Spitzen im Datenverkehr.
* **Eingeschränkte Offlineunterstützung**: Das Gatewaygerät speichert Nachrichten und Zwillingsaktualisierungen lokal, die nicht an IoT Hub übermittelt werden können.

Ein Gateway, das Protokollübersetzungen vornimmt, kann auch Edgeanalysen, Geräteisolierung sowie eine Glättung des Datenverkehrs durchführen und bietet Offlineunterstützung für vorhandene Geräte und neue Geräte mit eingeschränkten Ressourcen. Zahlreiche vorhandene Geräte erzeugen Daten, aus denen geschäftliche Erkenntnissen gewonnen werden können; sie wurden jedoch nicht speziell für Cloudkonnektivität konzipiert. Nicht transparente Gateways ermöglichen das Entsperren und Verwenden dieser Daten in einer End-to-End-IoT-Lösung.

Ein Gateway, das Identitätsübersetzungen vornimmt, bietet die Vorteile der Protokollübersetzung und ermöglicht außerdem die vollständige Verwaltbarkeit von nachgeschalteten Geräten über die Cloud. Alle Geräte in Ihrer IoT-Lösung werden unabhängig vom Protokoll, mit dem sie kommunizieren, in IoT Hub angezeigt.

## <a name="cheat-sheet"></a>Cheat Sheet
Hier finden Sie ein schnelles Cheat Sheet, das IoT Hub-Grundtypen bei Verwendung von transparenten, nicht transparenten und Proxy-Gateways vergleicht.

| &nbsp; | Transparentes Gateway | Protokollübersetzung | Identitätsübersetzung |
|--------|-------------|--------|--------|
| In der IoT Hub-Identitätsregistrierung gespeicherte Identitäten | Identitäten aller verbundenen Geräte | Nur die Identität des Gatewaygeräts | Identitäten aller verbundenen Geräte |
| Gerätezwilling | Jedes verbundene Gerät hat einen eigenen Gerätezwilling | Nur das Gateway hat einen Geräte- und Modulzwilling | Jedes verbundene Gerät hat einen eigenen Gerätezwilling |
| Direkte Methoden und C2D-Nachrichten | Die Cloud kann jedes verbundene Gerät einzeln adressieren | Die Cloud kann nur das Gatewaygerät adressieren | Die Cloud kann jedes verbundene Gerät einzeln adressieren |
| [IoT Hub-Drosselungen und -Kontingente][lnk-iothub-throttles-quotas] | Gelten für jedes Gerät | Gelten für das Gatewaygerät | Gelten für jedes Gerät |

Bei Verwendung eines nicht transparenten Gatewaymusters (Protokollübersetzung) verwenden alle Geräte, die eine Verbindung über dieses Gateway herstellen, dieselbe C2D-Warteschlange, die höchstens 50 Nachrichten enthalten kann. Daraus folgt, dass das nicht transparente Gatewaymuster nur verwendet werden sollte, wenn sehr wenige Geräte eine Verbindung über die einzelnen Bereichsgateways herstellen und der C2D-Datenverkehr gering ist.

## <a name="next-steps"></a>Nächste Schritte
Verwenden eines IoT Edge-Geräts als [transparentes Gateway][lnk-iot-edge-as-transparent-gateway] 

[lnk-iot-edge-as-transparent-gateway]: ./how-to-create-transparent-gateway.md
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md

[1]: ./media/iot-edge-as-gateway/edge-as-gateway.png
