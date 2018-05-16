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
ms.openlocfilehash: b86132b42aef981e6218b27e271e6db645d14071
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>Verbinden von IoT-Geräten mit Azure: IoT Hub und Event Hubs

Azure bietet Dienste, die speziell für verschiedene Arten von Konnektivität und Kommunikation entwickelt wurden, um Ihnen zu helfen, Ihre Daten mit der Leistungsstärke der Cloud zu verbinden. Azure IoT Hub und Azure Event Hubs sind Clouddienste, die große Datenmengen aufnehmen und diese Daten für Einblicke in Geschäftsabläufe verarbeiten oder speichern können. Die beiden Dienste sind insofern ähnlich, als sie beide die Erfassung von Daten mit geringer Wartezeit und hoher Zuverlässigkeit unterstützen, jedoch für unterschiedliche Zwecke konzipiert sind. IoT Hub wurde speziell entwickelt, um die besonderen Anforderungen bei der Verbindung von IoT-Geräten mit der Azure Cloud nach Maß zu erfüllen, während Event Hubs für das Streaming von Big Data entwickelt wurde. Aus diesem Grund empfiehlt Microsoft den Einsatz von Azure IoT Hub, um IoT-Geräte mit Azure zu verbinden.

Azure IoT Hub ist das Cloudgateway, das IoT-Geräte verbindet, um Daten zu sammeln und so geschäftliche Erkenntnisse und Automatisierung zu fördern. Darüber hinaus bietet IoT Hub Funktionen, die die Beziehung zwischen Ihren Geräten und Ihren Back-End-Systemen optimieren. Bidirektionale Kommunikationsfunktionen bedeuten, dass Sie während des Datenempfangs von Geräten auch Befehle und Richtlinien an Geräte zurücksenden können, um beispielsweise Eigenschaften zu aktualisieren oder Geräteverwaltungsaktionen aufzurufen.  Diese Cloud-zu-Gerät-Verbindung ermöglicht auch die Bereitstellung von Cloudintelligenz für Ihre Edge-Geräte mit Azure IoT Edge. Die besondere von IoT Hub bereitgestellte Identität auf Geräteebene hilft Ihnen, Ihre IoT-Lösung besser vor möglichen Angriffen zu schützen. 

[Azure Event Hubs][Azure Event Hubs] ist der Big Data-Streamingdienst von Azure. Die Lösung ist auf Szenarien für das Streaming von Daten mit hohem Durchsatz ausgelegt, bei denen Kunden täglich Milliarden von Anforderungen senden können. Event Hubs nutzt ein partitioniertes Consumermodell zur horizontalen Skalierung Ihres Streams und ist in die Big Data- und Analysedienste von Azure integriert, darunter Databricks, Stream Analytics, ADLS und HDInsight. Mit Funktionen wie Event Hubs Capture und „Automatische Vergrößerung“ unterstützt dieser Dienst Ihre Big Data-Apps und -Lösungen. Zusätzlich wird Event Hubs von IoT Hub für seinen Telemetrie-Datenflusspfad genutzt, sodass auch Ihre IoT-Lösung von der enormen Leistungsfähigkeit von Event Hubs profitiert.

Zusammenfassend lässt sich sagen, dass beide Lösungen zwar für die Datenerfassung in großem Maßstab ausgelegt sind, aber nur IoT Hub die umfangreichen IoT-spezifischen Funktionen bietet, die für Sie entwickelt wurden, um den betriebswirtschaftlichen Nutzen der Verbindung Ihrer IoT-Geräte mit der Azure-Cloud zu maximieren.  Wenn Sie gerade erst am Anfang Ihrer IoT-Projekte stehen, können Sie mit IoT Hub zur Unterstützung Ihrer Datenerfassungsszenarien beginnen, um sicherzustellen, dass Sie sofortigen Zugriff auf die vollständigen IoT-Funktionen haben, sobald Ihre geschäftlichen und technischen Anforderungen dies erfordern.

Die folgende Tabelle enthält Details zum Vergleich der beiden Tarife von IoT Hub mit Event Hubs, wenn Sie IoT Hub und Event Hubs gemäß ihrer IoT-Funktionen bewerten möchten. Weitere Informationen zu den IoT Hub-Tarifen „Basic“ und „Standard“ finden Sie unter [Skalieren einer IoT Hub-Lösung][lnk-scaling].

| IoT-Funktion | IoT Hub-Standard-Tarif | IoT Hub-Basic-Tarif | Event Hubs |
| --- | --- | --- | --- |
| Nachrichten, die von Geräten an die Cloud gesendet werden | ![Prüfen][1] | ![Prüfen][1] | ![Prüfen][1] |
| Protokolle: HTTPS, AMQP, AMQP über webSockets | ![Prüfen][1] | ![Prüfen][1] | ![Prüfen][1] |
| Protokolle: MQTT, MQTT über webSockets | ![Prüfen][1] | ![Prüfen][1] |  |
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
