---
title: Vergleich zwischen Azure IoT Hub und Azure Event Hubs | Microsoft Docs
description: Hier finden Sie einen Vergleich der Azure-Dienste IoT Hub und Event Hubs, wobei besonders auf Unterschiede bei den Funktionen und Anwendungsbeispiele eingegangen wird. Der Vergleich umfasst die unterstützten Protokolle, Geräteverwaltung, Überwachung, und Dateiuploads.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 303a2bde0a1e0b25ca6eb145e7b0cd6c91fff351
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Vergleich zwischen Azure IoT Hub und Azure Event Hubs

Azure IoT Hub und Azure Event Hubs sind Clouddienste, die große Datenmengen aufnehmen und diese Daten für Einblicke in Geschäftsabläufe verarbeiten oder speichern können. Die beiden Dienste sind insofern ähnlich, dass sie beide Prozessereignis- und Telemetriedaten mit niedriger Latenz und hoher Zuverlässigkeit unterstützen. Allerdings wurde nur IoT Hub mit den spezifischen Funktionen entwickelt, die zur Unterstützung von „Internet der Dinge“-Szenarien im angemessenen Umfang erforderlich sind. 

Azure IoT Hub ist das verbindende Cloudgateway zwischen Geräten und erfasst Daten für Einblicke in Geschäftsabläufe und Automatisierung. Es vereinfacht das Streamen von Daten in die Cloud und das Verwalten Ihrer Geräte im angemessenen Umfang. Ein wichtiges Unterscheidungsmerkmal zwischen IoT Hub und anderen Datenerfassungsdiensten ist, dass IoT Hub Features enthält, die die Beziehung zwischen Ihren Geräten und Ihren Back-End-Systemen ergänzen. Bidirektionale Kommunikationsfunktionen bedeuten: Während Sie Daten von Geräten empfangen, können Sie auch Nachrichten an Geräte zurücksenden, um Eigenschaften zu aktualisieren oder eine Aktion aufzurufen. Mit Identität auf Geräteebene können Sie Ihr System schützen. Mit verteilter Verarbeitung wird Clouddienstlogik auf Edgegeräte verschoben.

[Azure Event Hubs][Azure Event Hubs] ist ein Ereigniserfassungsdienst, der große Mengen an Daten und Telemetrie verarbeiten und speichern kann. Event Hubs ist zur Ereigniserfassung im großen Stil konzipiert, sowohl im Kontext rechenzentrenübergreifender als auch rechenzentreninterner Szenarien, bietet aber nicht die umfangreichen IoT-spezifischen Funktionen, die mit IoT Hub verfügbar sind. Aus diesem Grund empfehlen wir Event Hubs nicht für Ihre IoT-Lösungen. 

Die folgende Tabelle enthält Details zum Vergleich der beiden Tarife von IoT Hub mit Event Hubs, wenn Sie IoT Hub und Event Hubs gemäß ihrer IoT-Funktionen bewerten möchten. Weitere Informationen zu den IoT Hub-Tarifen „Basic“ und „Standard“ finden Sie unter [Skalieren einer IoT Hub-Lösung][lnk-scaling].

| IoT-Funktion | IoT Hub-Standard-Tarif | IoT Hub-Basic-Tarif | Event Hubs |
| --- | --- | --- | --- |
| Nachrichten, die von Geräten an die Cloud gesendet werden | ![Prüfen][1] | ![Prüfen][1] | ![Prüfen][1] |
| Protokolle: HTTPS, AMQP, AMQP über Websockets | ![Prüfen][1] | ![Prüfen][1] | ![Prüfen][1] |
| Protokolle: MQTT, MQTT über Websockets | ![Prüfen][1] | ![Prüfen][1] |  |
| Gerätebasierte Identität | ![Prüfen][1] | ![Prüfen][1] |  |
| Dateiupload von Geräten | ![Prüfen][1] | ![Prüfen][1] |  |
| Device Provisioning-Dienst | ![Prüfen][1] | ![Prüfen][1] |  |
| Senden von Nachrichten aus der Cloud an Geräte | ![Prüfen][1] |  |  |
| Gerätezwillinge und Geräteverwaltung | ![Prüfen][1] |  |  |
| IoT Edge | ![Prüfen][1] |  |  |

Auch wenn die D2C-Clouddatenerfassung der einzige Anwendungsfall ist, sollten Sie unbedingt IoT Hub verwenden, da IoT Hub einen Dienst bietet, der für die Verbindung von IoT-Geräten ausgelegt ist. 

### <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter [Entwicklungsleitfaden für Azure IoT Hub][lnk-devguide].


[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md

<!--Image references-->
[1]: ./media/iot-hub-compare-event-hubs/ic195031.png