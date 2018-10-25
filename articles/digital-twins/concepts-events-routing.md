---
title: Weiterleiten von Ereignissen und Nachrichten mit Azure Digital Twins | Microsoft-Dokumentation
description: Übersicht über das Weiterleiten von Ereignissen und Nachrichten an Dienstendpunkte mit Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 9b861f0991b11637c7b27b645d4506e658ea53b3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323899"
---
# <a name="routing-events-and-messages"></a>Weiterleiten von Ereignissen und Nachrichten

IoT-Lösungen umfassen oft mehrere leistungsstarke Dienste wie Speicher, Analysen etc. Dieser Artikel erläutert, wie Sie Azure Digital Twins-Apps mit Azure-Analysen, KI und Speicherdiensten verbinden, um ihnen weitere Informationen und Funktionen bereitzustellen.

## <a name="route-types"></a>Routentypen

Digital Twins bietet zwei Möglichkeiten zum Integrieren von IoT-Ereignissen in andere Azure-Dienste oder Geschäftsanwendungen:

* **Weiterleiten von Digital Twins-Ereignissen**: Azure Digital Twins-Ereignisse werden bei Objektänderungen im Raumgraphen ausgelöst, wenn Telemetriedaten empfangen werden oder eine benutzerdefinierte Funktion eine Benachrichtigung basierend auf vordefinierten Bedingungen erstellt. Benutzer können diese Ereignisse zur weiteren Verarbeitung an [Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Service Bus-Themen](https://azure.microsoft.com/services/service-bus/) oder [Event Grid](https://azure.microsoft.com/services/event-grid/) senden.

* **Weiterleiten von Gerätetelemetrie**: Azure Digital Twins kann nicht nur Ereignisse, sondern auch nicht formatierte Gerätetelemetrienachrichten an Event Hubs weiterleiten, um weitere Informationen und Analysen bereitzustellen. Diese Arten von Nachrichten werden nicht von Digital Twins verarbeitet und nur an **Event Hub** weitergeleitet.

Benutzer können mindestens einen ausgehenden Endpunkt angeben, um Ereignisse zu senden oder Nachrichten weiterzuleiten. Ereignisse und Nachrichten werden gemäß diesen vordefinierten Routingeinstellungen an die Endpunkte gesendet. Das heißt, Benutzer können beispielsweise einen Endpunkt zum Empfangen von Graphvorgängen und einen anderen zum Empfangen von Gerätetelemetrieereignissen angeben.

![Weiterleiten von Digital Twins-Ereignissen][1]

Beim Weiterleiten an **Event Hubs** werden Telemetrienachrichten in der ursprünglichen Reihenfolge gesendet. Das bedeutet, sie kommen beim Endpunkt in der gleichen Reihenfolge an wie sie ursprünglich empfangen wurden. **Event Grid** und **Service Bus** garantieren nicht, dass Endpunkte die Ereignisse in der gleichen Reihenfolge empfangen, in der sie aufgetreten sind. Das Ereignisschema schließt jedoch einen Zeitstempel ein, mit dem die Reihenfolge ermittelt werden kann, nachdem die Ereignisse am Endpunkt angekommen sind.

## <a name="route-implementation"></a>Implementieren von Routen

Der Azure Digital Twins-Dienst unterstützt derzeit die folgenden Endpunkttypen (**EndpointTypes**):

* **EventHub**: der Verbindungszeichenfolge-Endpunkt von Event Hub
* **ServiceBus**: der Verbindungszeichenfolge-Endpunkt von Service Bus
* **EventGrid**: der Verbindungszeichenfolge-Endpunkt von Event Grid

Azure Digital Twins unterstützt derzeit die folgenden Ereignistypen (**EventTypes**), die an den ausgewählten Endpunkt gesendet werden:

* **DeviceMessages**: Telemetrienachrichten, die von Benutzergeräten gesendet und vom System weitergeleitet werden.
* **TopologyOperation**: Vorgänge, die den Graph oder seine Metadaten ändern. Dazu gehört beispielsweise das Hinzufügen oder Löschen einer Entität, z.B. eines Leerzeichens.
* **SpaceChange**: Änderungen des berechneten Werts eines Bereichs als Ergebnis einer Gerätetelemetrienachricht.
* **SensorChange**: Änderungen des berechneten Werts eines Sensors als Ergebnis einer Gerätetelemetrienachricht.
* **UdfCustom**: Benutzerdefinierte Benachrichtigungen von einer benutzerdefinierten Funktion.

> [!IMPORTANT]
> Nicht jedes **EndpointTypes**-Element unterstützt alle **EventTypes**-Elemente.
> In der folgenden Tabelle finden Sie die für das jeweilige **EndpointType**-Element zulässigen **EventTypes**-Elemente.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| **EventHub**|     X          |         X         |     X       |      X       |   X       |
| **ServiceBus**|              |         X         |     X       |      X       |   X       |
| **EventGrid**|               |         X         |     X       |      X       |   X       |

>[!NOTE]
>Weitere Informationen zum Erstellen von Endpunkten und Beispiele für Ereignisschemata finden Sie unter [Endpunkte und ausgehende Daten](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Einschränkungen der Public Preview finden Sie unter [Azure Digital Twins (Vorschau): Einschränkungen](concepts-service-limits.md).

Azure Digital Twins-Beispiele zum Testen finden Sie unter [Schnellstart: Suchen nach verfügbaren Räumen](quickstart-view-occupancy-dotnet.md).

<!-- Images -->
[1]: media/concepts/digital-twins-events-routing.png