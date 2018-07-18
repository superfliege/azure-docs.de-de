---
title: Grundlegendes zu benutzerdefinierten Azure IoT Hub-Endpunkten | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Verwenden von Routingregeln zum Weiterleiten von D2C-Nachrichten an benutzerdefinierte Endpunkte.'
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/09/2018
ms.author: dobett
ms.openlocfilehash: b035c7ef6dfe56c4b4534e081e70d95ea7c14847
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808025"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Verwenden von Nachrichtenrouten und benutzerdefinierten Endpunkten für D2C-Nachrichten

Mit IoT Hub können Sie [D2C-Nachrichten][lnk-device-to-cloud] basierend auf den Nachrichteneigenschaften an IoT Hub-Endpunkte weiterleiten. Mit Routingregeln können Sie Nachrichten flexibel an den Bestimmungsort senden, ohne zusätzliche Dienste oder benutzerdefinierten Code zu benötigen. Jede Routingregel, die Sie konfigurieren, hat die folgenden Eigenschaften:

| Eigenschaft      | BESCHREIBUNG |
| ------------- | ----------- |
| **Name**      | Der eindeutige Name, mit dem die Regel identifiziert wird. |
| **Quelle**    | Der Ursprung des zu verarbeitenden Datenstroms. Beispiel: Gerätetelemetrie. |
| **Condition** | Der Abfrageausdruck für die Routingregel, die für den Header und den Text der Nachricht ausgeführt wird und ermittelt, ob für den Endpunkt eine Übereinstimmung vorhanden ist. Weitere Informationen zur Erstellung einer Routenbedingung finden Sie unter [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-devguide-query-language]. |
| **Endpunkt**  | Der Name des Endpunkts, an den vom IoT Hub diejenigen Nachrichten gesendet werden, für die sich eine Übereinstimmung ergeben hat. Es ist ratsam, dass sich Endpunkte in derselben Region wie der IoT Hub befinden, da sonst ggf. Kosten für regionsübergreifende Schreibvorgänge anfallen. |

Es kann vorkommen, dass sich für eine einzelne Nachricht Übereinstimmungen mit den Bedingungen mehrerer Routingregeln ergeben. In diesem Fall sendet der IoT Hub die Nachricht jeweils an alle Endpunkte, die den entsprechenden Regeln zugeordnet sind. Der IoT Hub führt bei der Nachrichtenzustellung eine automatische Deduplizierung durch. Wenn also eine Nachricht mit mehreren Regeln mit demselben Ziel übereinstimmt, wird sie nur einmal in das Ziel geschrieben.

## <a name="endpoints-and-routing"></a>Endpunkte und Routing

Eine IoT Hub-Instanz verfügt standardmäßig über einen [integrierten Endpunkt][lnk-built-in]. Sie können benutzerdefinierte Endpunkte für das Routing von Nachrichten erstellen, indem Sie andere Dienste Ihres Abonnements mit dem Hub verknüpfen. IoT Hub unterstützt derzeit Azure Storage Container, Event Hubs, Service Bus-Warteschlangen und Service Bus-Themen als benutzerdefinierte Endpunkte.

Bei Verwendung von Routing und benutzerdefinierten Endpunkten werden Nachrichten nur an den integrierten Endpunkt übermittelt, wenn sie mit keinen Regeln übereinstimmen. Fügen Sie für die Übermittlung von Nachrichten an den integrierten Endpunkt und einen benutzerdefinierten Endpunkt eine Route hinzu, die Nachrichten an den Endpunkt **events** sendet.

> [!NOTE]
> IoT Hub unterstützt nur das Schreiben von Daten in Azure Storage-Container als BLOBs.

> [!WARNING]
> Service Bus-Warteschlangen und -Themen, bei denen **Sitzungen** oder **Duplikaterkennung** aktiviert wurden, werden nicht als benutzerdefinierte Endpunkte unterstützt.

Weitere Informationen zur Erstellung von benutzerdefinierten Endpunkten auf dem IoT Hub finden Sie unter [IoT Hub-Endpunkte][lnk-devguide-endpoints].

Weitere Informationen zum Lesen von benutzerdefinierten Endpunkten finden Sie unter:

* Lesen aus [Azure Storage-Containern][lnk-getstarted-storage].
* Lesen aus [Event Hubs][lnk-getstarted-eh]
* Lesen aus [Service Bus-Warteschlangen][lnk-getstarted-queue]
* Lesen aus [Service Bus-Themen][lnk-getstarted-topic]

## <a name="latency"></a>Latency

Wenn Sie Geräte-zu-Cloud-Telemetrienachrichten über integrierte Endpunkte weiterleiten, kommt es nach der Erstellung der ersten Route zu einer leichten Erhöhung der Gesamtlatenz.

In den meisten Fällen beträgt die durchschnittliche Latenz weniger als eine Sekunde. Sie können die Latenz mithilfe der [IoT Hub-Metrik](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) **d2c.endpoints.latency.builtIn.events** überwachen. Das Erstellen oder Löschen einer Route nach der ersten hat keinen Einfluss auf die Gesamtlatenz.

### <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu IoT Hub-Endpunkten finden Sie unter [IoT Hub-Endpunkte][lnk-devguide-endpoints].

Weitere Informationen zur Abfragesprache, mit der Sie Routingregeln definieren, finden Sie unter [IoT Hub-Abfragesprache für Gerätezwillinge, Aufträge und Nachrichtenrouting][lnk-devguide-query-language].

Im Tutorial [Verarbeiten von IoT Hub-D2C-Nachrichten mithilfe von Routen][lnk-d2c-tutorial] erfahren Sie, wie Sie Routingregeln und benutzerdefinierte Endpunkte verwenden.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: tutorial-routing.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[lnk-getstarted-storage]: ../storage/blobs/storage-blobs-introduction.md
