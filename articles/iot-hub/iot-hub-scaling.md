---
title: Skalieren von Azure IoT Hub | Microsoft Docs
description: "Skalieren der IoT Hub-Instanz zur Unterstützung des erwarteten Nachrichtendurchsatzes. Enthält eine Zusammenfassung der unterstützten Durchsätze für die einzelnen Ebenen und Optionen für das Sharding."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0050b620d1cdbe95f6cf53a8e6f65271865c9e0b
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="scale-your-iot-hub-solution"></a>Skalieren einer IoT Hub-Lösung
Azure IoT Hub kann bis zu einer Million gleichzeitig verbundener Geräte unterstützen. Weitere Informationen finden Sie unter [IoT Hub – Preise][lnk-pricing]. Jede IoT Hub-Einheit lässt eine bestimmte Anzahl von täglichen Nachrichten zu.

Um Ihre Lösung richtig skalieren zu können, sollten Sie sich nach dem jeweiligen IoT Hub-Anwendungsfall richten. Achten Sie besonders auf den erforderlichen Spitzendurchsatz für die folgenden Kategorien von Vorgängen:

* D2C-Nachrichten
* C2D-Nachrichten
* Identitätsregistrierungsvorgänge

Sehen Sie sich zusätzlich zu diesen Durchsatzinformationen auch [IoT Hub-Kontingente und -Drosselungen][IoT Hub quotas and throttles] an, und entwerfen Sie Ihre Lösung entsprechend.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>D2C- und C2D-Nachrichtendurchsatz
Die beste Methode zum Skalieren einer IoT Hub-Lösung ist die Auswertung des Datenverkehrs pro Einheit.

D2C-Nachrichten basieren auf diesen Richtlinien für den anhaltenden Durchsatz:

| Tarif | Anhaltender Durchsatz | Anhaltende Senderate |
| --- | --- | --- |
| S1 |Bis zu 1111 KB/Minute pro Einheit<br/>(1,5 GB/Tag/Einheit) |Durchschnittlich 278 Nachrichten/Minute pro Einheit<br/>(400.000 Nachrichten/Tag pro Einheit) |
| S2 |Bis zu 16 MB/Minute pro Einheit<br/>(22,8 GB/Tag/Einheit) |Durchschnittlich 4.167 Nachrichten/Minute pro Einheit<br/>(6 Millionen Nachrichten/Tag pro Einheit) |
| S3 |Bis zu 814 MB/Minute pro Einheit<br/>(1.144,4 GB/Tag/Einheit) |Durchschnittlich 208.333 Nachrichten/Minute pro Einheit<br/>(300 Millionen Nachrichten/Tag pro Einheit) |

## <a name="identity-registry-operation-throughput"></a>Durchsatz von Identitätsregistrierungsvorgängen
IoT Hub-Identitätsregistrierungsvorgänge sollten keine Laufzeitvorgänge sein, da sie sich größtenteils auf die Gerätebereitstellung beziehen.

Genaue Zahlen zur Burstleistung finden Sie unter [IoT Hub-Kontingente und -Drosselungen][IoT Hub quotas and throttles].

## <a name="sharding"></a>Sharding (Horizontales Partitionieren)
Eine einzelne IoT Hub-Einheit kann zwar auf Millionen von Geräten skaliert werden, aber es kann sein, dass Ihre Lösung bestimmte Leistungsmerkmale benötigt, die von einer einzelnen IoT Hub-Einheit nicht gewährleistet werden können. In diesem Fall wird empfohlen, Ihre Geräte auf mehrere IoT Hubs zu partitionieren. Mehrere IoT Hubs glätten Datenverkehrsbursts und erzielen den erforderlichen Durchsatz oder die erforderlichen Vorgangsraten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit Azure IoT Edge][lnk-iotedge]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
