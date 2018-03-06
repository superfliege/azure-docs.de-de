---
title: Azure IoT Hub und Event Grid | Microsoft-Dokumentation
description: "Lösen Sie mit Azure Event Grid Prozesse auf der Basis von Aktionen aus, die in IoT Hub auftreten."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2018
ms.author: kgremban
ms.openlocfilehash: 7c75a65714898f27ab0008ad5a30a5714d7174f4
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions---preview"></a>Reagieren auf Ereignisse in IoT Hub mithilfe von Event Grid zum Auslösen von Aktionen – Vorschau

Azure IoT Hub ist in Azure Event Grid integriert, sodass Sie Ereignisbenachrichtigungen an andere Dienste senden und nachfolgende Prozesse auslösen können. Konfigurieren Sie Ihre Geschäftsanwendungen zum Lauschen auf IoT Hub-Ereignisse, sodass Sie zuverlässig, skalierbar und sicher auf kritische Ereignisse reagieren können. Erstellen Sie beispielsweise eine Anwendung zum Durchführen mehrerer Aktionen wie Aktualisieren einer Datenbank, Erstellen eines Tickets und Übermitteln einer E-Mail-Benachrichtigung bei jeder Registrierung eines neuen IoT Hub-Geräts bei Ihrem IoT Hub. 

[Azure Event Grid][lnk-eg-overview] ist ein vollständig verwalteter Ereignis-Routingdienst, der ein Veröffentlichen-Abonnieren-Modell verwendet. Event Grid verfügt über integrierte Unterstützung für Azure-Dienste wie [Azure Functions](../azure-functions/functions-overview.md) und [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) und kann mithilfe von Webhooks Ereigniswarnungen an Nicht-Azure-Dienste übermitteln. Eine vollständige Liste der Ereignishandler, die Event Grid unterstützt, finden Sie unter [Einführung in Azure Event Grid][lnk-eg-overview]. 

![Azure Event Grid-Architektur](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Die Event Grid-Integration ist für IoT-Hubs verfügbar, die sich in den Regionen befinden, in denen Event Grid unterstützt wird. Die neueste Liste der Regionen finden Sie unter [Einführung in das Azure Event Grid][lnk-eg-overview]. 

## <a name="event-types"></a>Ereignistypen

IoT Hub veröffentlicht die folgenden Ereignistypen: 

| Ereignistypen | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Wird ausgelöst, wenn ein Gerät bei einem IoT Hub registriert wird. |
| Microsoft.Devices.DeviceDeleted | Veröffentlicht das Löschen eines Geräts von einem IoT Hub. | 

Konfigurieren Sie entweder mit dem Azure-Portal oder der Azure CLI, welche Ereignisse von jedem IoT Hub veröffentlicht werden sollen. Ein Beispiel finden Sie in dem Tutorial [Senden von E-Mail-Benachrichtigungen zu Azure IoT Hub-Ereignissen mit Logik-Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md). 

## <a name="event-schema"></a>Ereignisschema

IoT Hub-Ereignisse enthalten alle Informationen, die Sie für die Reaktion auf Änderungen in Ihrem Gerätelebenszyklus benötigen. Um ein IoT Hub-Ereignis zu identifizieren, überprüfen Sie, ob die Eigenschaft eventType mit **Microsoft.Devices** beginnt. Weitere Informationen zur Verwendung von Event Grid-Ereigniseigenschaften finden Sie unter [Azure Event Grid-Ereignisschema](../event-grid/event-schema.md).

### <a name="device-created-schema"></a>Vom Gerät erstelltes Schema

Das folgende Beispiel zeigt das Schema eines von einem Gerät erstellten Ereignisses: 

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
      "status": "enabled",
      "statusUpdateTime": "0001-01-01T00:00:00",
      "connectionState": "Disconnected",
      "lastActivityTime": "0001-01-01T00:00:00",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "sas",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "version": 2,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "operationTimestamp": "2018-01-02T19:17:44.4383997Z",
    "opType": "DeviceCreated"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Eine ausführliche Beschreibung der einzelnen Eigenschaften finden Sie unter [Azure Event Grid-Ereignisschema für IoT Hub](../event-grid/event-schema-iot-hub.md).

## <a name="filter-events"></a>Filtern von Ereignissen

IoT Hub-Ereignisabonnements können Ereignisse basierend auf Ereignistyp und Gerätename filtern. Betrefffilter in Event Grid funktionieren anhand von Übereinstimmungen bei **Präfix** und **Suffix**. Der Filter verwendet einen `AND`-Operator, sodass Ereignisse mit einem Betreff, der sowohl mit dem Präfix als auch mit dem Suffix übereinstimmt, an den Abonnenten übermittelt werden. 

Der Betreff von IoT-Ereignissen verwendet das Format:

```json
devices/{deviceId}
```

### <a name="tips-for-consuming-events"></a>Tipps zum Nutzen von Ereignissen

Anwendungen, die IoT Hub-Ereignisse behandeln, sollten diesen empfohlenen Methoden entsprechen:

* Mehrere Abonnements können zum Weiterleiten von Ereignissen an denselben Ereignishandler konfiguriert werden; darum ist es wichtig, nicht davon auszugehen, dass Ereignisse aus einer bestimmten Quelle stammen. Überprüfen Sie immer das Nachrichtenthema, um sicherzustellen, dass es von einem IoT Hub stammt, den Sie erwarten. 
* Gehen Sie nicht davon aus, dass alle Ereignisse, die Sie erhalten, den Typen entsprechen, die Sie erwarten. Überprüfen Sie vor der Verarbeitung der Nachricht immer den eventType.
* Nachrichten können in falscher Reihenfolge oder nach einer Verzögerung eingehen. Verwenden Sie das etag-Feld, um zu erfahren, ob Ihre Informationen zu Objekten auf dem neuesten Stand sind.



## <a name="next-steps"></a>Nächste Schritte

* [Ausprobieren des Tutorials zu IoT Hub-Ereignissen](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* [Weitere Informationen zu Event Grid][lnk-eg-overview]
* [Vergleichen der Unterschiede zwischen dem Routing von IoT Hub-Ereignissen und Nachrichten][lnk-eg-compare]

<!-- Links -->
[lnk-eg-overview]: ../event-grid/overview.md
[lnk-eg-compare]: iot-hub-event-grid-routing-comparison.md