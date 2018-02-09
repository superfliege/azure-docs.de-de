---
title: "Vergleich von Event Grid und Weiterleitung für IoT Hub | Microsoft-Dokumentation"
description: "IoT Hub bietet einen eigenen Nachrichtenweiterleitungsdienst, ist aber auch für die Ereignisveröffentlichung mit Event Grid integriert. Hier werden die beiden Features verglichen."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 5a0a97ccf033b2981ba13be455482146ba212228
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="compare-message-routing-and-event-grid-for-iot-hub"></a>Vergleichen von Nachrichtenweiterleitung und Event Grid für IoT Hub

Azure IoT Hub stellt die Möglichkeit bereit, Daten von Ihren verbundenen Geräten zu streamen und diese Daten in Ihre Geschäftsanwendungen zu integrieren. IoT Hub bietet zwei Methoden zum Integrieren von IoT-Ereignissen in andere Azure-Dienste oder Geschäftsanwendungen. Dieser Artikel beschreibt die beiden Features, die diese Funktion bereitstellen, damit Sie entscheiden können, welche Option für Ihr Szenario am besten geeignet ist.

* **IoT Hub-Nachrichtenweiterleitung:** Dieses IoT Hub-Feature ermöglicht es Benutzern, [Gerät-zu-Cloud-Nachrichten](iot-hub-devguide-messages-read-custom.md) an Dienstendpunkte wie Azure Storage-Container, Event Hubs, Service Bus-Warteschlangen oder Service Bus-Themen weiterzuleiten. Routingregeln bieten die erforderliche Flexibilität zum Ausführen von auf Abfragen basierenden Routen. Sie ermöglichen auch [kritische Warnungen](iot-hub-devguide-messages-d2c.md), die Aktionen über Abfragen auslösen, und können auf den Nachrichtenheadern und dem Nachrichtentext basieren. 
* **IoT Hub-Integration mit Event Grid:** Azure Event Grid ist ein vollständig verwalteter Ereignisroutingdienst, der ein Veröffentlichen-Abonnieren-Modell verwendet. IoT Hub und Event Grid [integrieren gemeinsam IoT Hub-Ereignisse in Azure- und Nicht-Azure-Dienste](iot-hub-event-grid.md) praktisch in Echtzeit. 

## <a name="similarities-and-differences"></a>Ähnlichkeiten und Unterschiede

Sowohl bei der Nachrichtenweiterleitung als auch bei Even Grid ist eine Warnungskonfiguration möglich, es gibt jedoch einige wichtige Unterschiede zwischen den beiden Methoden. Details finden Sie in der folgenden Tabelle:

| Feature | IoT Hub-Nachrichtenweiterleitung | IoT Hub-Integration mit Event Grid |
| ------- | --------------- | ---------- |
| **Gerätemeldungen** | Ja, die Nachrichtenweiterleitung kann für Telemetriedaten verwendet werden. | Nein, Event Grid kann nur für telemetriefremde IoT Hub-Ereignisse verwendet werden. |
| **Ereignistyp** | Ja, die Nachrichtenweiterleitung kann Änderungen am Gerätezwilling und Gerätelebenszyklus-Ereignisse melden. | Ja, Event Grid kann melden, wenn Geräte bei einem IoT Hub registriert sind und wenn Geräte gelöscht werden. |
| **Reihenfolge** | Ja, die Reihenfolge von Ereignissen wird beibehalten.  | Nein, die Reihenfolge von Ereignissen ist nicht garantiert. | 
| **Maximale Nachrichtengröße** | 256 KB, Gerät-zu-Cloud | 64 KB |
| **Filterung** | Umfassende Filtermöglichkeiten über eine SQL-ähnliche Sprache unterstützen das Filtern anhand von Nachrichtenheadern und -texten. Beispiele finden Sie unter [IoT Hub-Abfragesprache](iot-hub-devguide-query-language.md). | Filterung anhand des Suffix/Präfix von Geräte-IDs, die gut für hierarchische Dienste wie Storage funktioniert. |
| **Endpunkte** | <ul><li>Event Hub</li> <li>Speicherblob</li> <li>Service Bus-Warteschlange</li> <li>Service Bus-Themen</li></ul><br>Kostenpflichtige IoT Hub-SKUs (S1, S2 und S3) sind auf 10 benutzerdefinierte Endpunkte beschränkt. Pro IoT Hub können 100 Routen erstellt werden. | <ul><li>Azure-Funktionen</li> <li>Azure-Automatisierung</li> <li>Event Hub</li> <li>Logic Apps</li> <li>Microsoft Flow</li> <li>Drittanbieterdienste über Webhooks</li></ul><br>Die aktuelle Liste der Endpunkte finden Sie unter [Event Grid-Ereignishandler](../event-grid/overview.md#event-handlers). |
| **Kosten** | Für die Nachrichtenweiterleitung fallen keine separaten Gebühren an. Nur der Eingang von Telemetriedaten in IoT Hub wird in Rechnung gestellt. Wenn beispielsweise eine Nachricht an drei verschiedene Endpunkte weitergeleitet wird, wird Ihnen nur eine Nachricht berechnet. | Es fallen keine Gebühren von IoT Hub an. Event Grid bietet die ersten 100.000 Vorgänge pro Monat kostenlos, danach 0,60 US-Dollar pro Million Vorgänge. |

IoT Hub-Nachrichtenweiterleitung und Event Grid weisen auch Ähnlichkeiten auf, von denen einige in der folgenden Tabelle beschrieben werden:

| Feature | IoT Hub-Nachrichtenweiterleitung | IoT Hub-Integration mit Event Grid |
| ------- | --------------- | ---------- |
| **Zuverlässigkeit** | Hoch: Jede Nachricht wird für jede Route mindestens einmal an den Endpunkt übermittelt. Alle Nachrichten, die nicht innerhalb einer Stunde übermittelt werden, laufen ab. | Hoch: Jede Nachricht wird für jedes Abonnement mindestens einmal an den Webhook übermittelt. Alle Ereignisse, die nicht innerhalb von 24 Stunden übermittelt werden, laufen ab. | 
| **Skalierbarkeit** | Hoch: für die Unterstützung von Millionen gleichzeitig verbundener Geräte, die Milliarden Nachrichten senden, optimiert. | Hoch: kann 10.000.000 Ereignisse pro Sekunde pro Region weiterleiten. |
| **Latenz** | Niedrig: nahezu in Echtzeit. | Niedrig: nahezu in Echtzeit. |
| **Senden an mehrere Endpunkte** | Ja, Senden einer einzelnen Nachricht an mehrere Endpunkte. | Ja, Senden einer einzelnen Nachricht an mehrere Endpunkte.  | 
| **Sicherheit** | IoT Hub ermöglicht gerätespezifische Identitäten und widerrufbare Zugriffssteuerung. Weitere Informationen finden Sie unter [IoT Hub-Zugriffssteuerung](iot-hub-devguide-security.md). | Event Grid bietet eine Überprüfung an drei Punkten: Ereignisabonnements, Ereignisveröffentlichung und Webhook-Ereignisübermittlung. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](../event-grid/security-authentication.md). |

## <a name="how-to-choose"></a>Auswahl

Die IoT Hub-Nachrichtenweiterleitung und die IoT Hub-Integration mit Event Grid erzielen über unterschiedliche Aktionen ähnliche Ergebnisse. Beide übernehmen Informationen aus Ihrer IoT Hub-Lösung und geben sie weiter, damit andere Dienste reagieren können. Wie entscheiden Sie also, welche Methode Sie verwenden? Beantworten Sie zusätzlich zu den Daten aus dem vorherigen Abschnitt die folgenden Fragen, um Ihre Entscheidung zu unterstützen: 

* **Welche Art von Daten senden Sie an die Endpunkte?**

   Verwenden Sie die IoT Hub-Nachrichtenweiterleitung, wenn Sie Telemetriedaten an andere Dienste senden müssen. Die Nachrichtenweiterleitung ermöglicht auch das Abfragen von Nachrichtenheadern und Nachrichtentexten. 

   Die IoT Hub-Integration mit Event Grid arbeitet mit Ereignissen, die im IoT Hub-Dienst auftreten. Zu diesen IoT Hub-Ereignissen gehören das Erstellen und Löschen von Geräten. 

* **Welche Endpunkte müssen diese Informationen erhalten?**

   Die IoT Hub-Nachrichtenweiterleitung unterstützt eingeschränkte Endpunkte, Sie können jedoch Connectors erstellen, um die Daten und Ereignisse an zusätzliche Endpunkte umzuleiten. Eine vollständige Liste der unterstützten Endpunkte finden Sie in der Tabelle im vorherigen Abschnitt. 

   Die IoT Hub-Integration mit Event Grid unterstützt mehrere Endpunkte. Sie funktioniert auch mit Webhooks zur Erweiterung der Weiterleitung auf Bereiche außerhalb der Azure-Dienstumgebung und in Geschäftsanwendungen von Drittanbietern. 

* **Ist es wichtig, ob Ihre Daten in der richtigen Reihenfolge empfangen werden?**

   Die IoT Hub-Nachrichtenweiterleitung behält die Reihenfolge, in der Nachrichten gesendet werden, bei, sodass sie bei der Ankunft unverändert bleibt.

   Event Grid garantiert nicht, dass Endpunkte die Ereignisse in der gleichen Reihenfolge empfangen, in der sie aufgetreten sind. Das Ereignisschema schließt jedoch einen Zeitstempel ein, mit dem die Reihenfolge ermittelt werden kann, nachdem die Ereignisse am Endpunkt angekommen sind. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [IoT Hub-Nachrichtenweiterleitung](iot-hub-devguide-messages-d2c.md) und [IoT Hub-Endpunkte](iot-hub-devguide-endpoints.md).

* Erhalten Sie weitere Informationen zu [Azure Event Grid](../event-grid/overview.md).

* Probieren Sie die Event Grid-Integration aus, indem Sie [E-Mail-Benachrichtigungen zu Azure IoT Hub-Ereignissen mit Logic Apps senden](../event-grid/publish-iot-hub-events-to-logic-apps.md).