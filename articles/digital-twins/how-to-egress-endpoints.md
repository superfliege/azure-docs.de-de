---
title: Ausgang und Endpunkte in Azure Digital Twins | Microsoft-Dokumentation
description: Anleitung zum Erstellen von Endpunkten mit Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: c09ee84cda5f0a9747d3ee1f8f1b37d1323f2cc2
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212249"
---
# <a name="egress-and-endpoints"></a>Ausgang und Endpunkte

Azure Digital Twins unterstützt das Konzept von _Endpunkten_, wobei jeder Endpunkt einem Nachrichten-/Ereignisbroker im Azure-Abonnement des Benutzers entspricht. Ereignisse und Meldungen können an **Event Hub**, **Event Grid**, und **Service Bus-Themen** an gesendet werden.

Ereignisse werden entsprechend den vordefinierten Routingeinstellungen an die Endpunkte gesendet: Der Benutzer kann angeben, welcher Endpunkt jedes der folgenden Ereignisse empfangen soll: **TopologyOperation**, **UdfCustom**, **SensorChange**, **SpaceChange** oder **DeviceMessage**.

Grundlegende Informationen zur Weiterleitung von Ereignissen und zu Ereignistypen finden Sie unter [Weiterleiten von Ereignissen und Nachrichten](concepts-events-routing.md).

## <a name="event-types-description"></a>Beschreibung der Ereignistypen

Die Ereignisformate für jeden der Ereignistypen sehen wie folgt aus:

- **TopologyOperation**

  Gilt für Graphänderungen. Die *subject*-Eigenschaft gibt den Typ des betroffenen Objekts an. Typen von Objekten, die dieses Ereignis auslösen können: **Device**, **DeviceBlobMetadata**, **DeviceExtendedProperty**, **ExtendedPropertyKey**, **ExtendedType**, **KeyStore**, **Report**, **RoleDefinition**, **Sensor**, **SensorBlobMetadata**, **SensorExtendedProperty**, **Space**,  **SpaceBlobMetadata**, **SpaceExtendedProperty**, **SpaceResource**, **SpaceRoleAssignment**, **System**, **User**, **UserBlobMetadata**, **UserExtendedProperty**.

  Beispiel:

  ```JSON
  {
    "id": "00000000-0000-0000-0000-000000000000",
    "subject": "ExtendedPropertyKey",
    "data": {
      "SpacesToNotify": [
        "3a16d146-ca39-49ee-b803-17a18a12ba36"
      ],
      "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
      "AccessType": "Create"
    },
    "eventType": "TopologyOperation",
    "eventTime": "2018-04-17T17:41:54.9400177Z",
    "dataVersion": "1",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Name des benutzerdefinierten Attributs | Ersetzen durch |
    | --- | --- |
    | *yourTopicName* | Den Namen Ihres angepassten Themas |

- **UdfCustom**

  Ein Ereignis, das von einer benutzerdefinierten Funktion gesendet wurde. 
  
  > [!IMPORTANT]
  > Dieses Ereignis muss explizit von der benutzerdefinierten Funktion selbst gesendet werden.

  Beispiel:

  ```JSON
  {
    "id": "568fd394-380b-46fa-925a-ebb96f658cce",
    "subject": "UdfCustom",
    "data": {
      "TopologyObjectId": "7c799bfc-1bff-4b9e-b15a-669933969d20",
      "ResourceType": "Space",
      "Payload": "\"Room is not available or air quality is poor\"",
      "CorrelationId": "568fd394-380b-46fa-925a-ebb96f658cce"
    },
    "eventType": "UdfCustom",
    "eventTime": "2018-10-02T06:50:15.198Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Name des benutzerdefinierten Attributs | Ersetzen durch |
    | --- | --- |
    | *yourTopicName* | Den Namen Ihres angepassten Themas |

- **SensorChange**

  Eine Aktualisierung des Zustands eines Sensors anhand von Telemetriedatenänderungen.

  Beispiel:

  ```JSON
  {
    "id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
    "subject": "SensorChange",
    "data": {
      "Type": "Classic",
      "DataType": "Motion",
      "Id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
      "Value": "False",
      "PreviousValue": "True",
      "EventTimestamp": "2018-04-17T17:46:15.4964262Z",
      "MessageType": "sensor",
      "Properties": {
        "ms-client-request-id": "c9e576b7-5eea-4f61-8617-92a57add5179",
        "ms-activity-id": "ct22YwXEGJ5u.605.0"
      }
    },
    "eventType": "SensorChange",
    "eventTime": "2018-04-17T17:46:18.5452993Z",
    "dataVersion": "1",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Name des benutzerdefinierten Attributs | Ersetzen durch |
    | --- | --- |
    | *yourTopicName* | Den Namen Ihres angepassten Themas |

- **SpaceChange**

  Eine Aktualisierung des Zustands eines Raums anhand von Telemetriedatenänderungen.

  Beispiel:

  ```JSON
  {
    "id": "42522e10-b1aa-42ff-a5e7-7181788ffc4b",
    "subject": "SpaceChange",
    "data": {
      "Type": null,
      "DataType": "AvailableAndFresh",
      "Id": "7c799bfc-1bff-4b9e-b15a-669933969d20",
      "Value": "Room is not available or air quality is poor",
      "PreviousValue": null,
      "RawData": null,
      "transactionId": null,
      "EventTimestamp": null,
      "MessageType": null,
      "Properties": null,
      "CorrelationId": "42522e10-b1aa-42ff-a5e7-7181788ffc4b"
    },
    "eventType": "SpaceChange",
    "eventTime": "2018-10-02T06:50:20.128Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Name des benutzerdefinierten Attributs | Ersetzen durch |
    | --- | --- |
    | *yourTopicName* | Den Namen Ihres angepassten Themas |

- **DeviceMessage**

  Ermöglicht Ihnen die Angabe einer **EventHub**-Verbindung, an die Telemetrierohdatenereignisse direkt oder aus Azure Digital Twins weitergeleitet werden können.

> [!NOTE]
> - **DeviceMessage** kann nur mit **EventHub** kombiniert werden. Es ist nicht möglich, **DeviceMessage** mit irgendeinem der anderen Ereignistypen zu kombinieren.
> - Sie können nur einen einzigen Endpunkt der Kombination der Typen **EventHub** oder **DeviceMessage** angeben.

## <a name="configuring-endpoints"></a>Konfigurieren von Endpunkten

Endpunktverwaltung erfolgt über die Endpunkte-API. Es folgen einige Beispiele, in denen gezeigt wird, wie die verschiedenen unterstützten Endpunkte zu konfigurieren sind. Achten Sie besonders auf das Array für Ereignistypen (eventTypes), weil es das Routing (Weiterleiten) für den Endpunkt definiert:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
```

- Ereignistypen für das Weiterleiten an **Service Bus**: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourTopicName"
  }
  ```

    | Name des benutzerdefinierten Attributs | Ersetzen durch |
    | --- | --- |
    | *yourNamespace* | Der Namespace Ihres Endpunkts |
    | *yourPrimaryKey* | Die primäre Verbindungszeichenfolge, die zur Authentifizierung verwendet wird |
    | *yourSecondaryKey* | Die sekundäre Verbindungszeichenfolge, die zur Authentifizierung verwendet wird |
    | *yourTopicName* | Den Namen Ihres angepassten Themas |

- Ereignistypen für das Weiterleiten an **Event Grid**: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "yourPrimaryKey",
    "secondaryConnectionString": "yourSecondaryKey",
    "path": "yourTopicName.westus-1.eventgrid.azure.net"
  }
  ```

    | Name des benutzerdefinierten Attributs | Ersetzen durch |
    | --- | --- |
    | *yourPrimaryKey* | Die primäre Verbindungszeichenfolge, die zur Authentifizierung verwendet wird|
    | *yourSecondaryKey* | Die sekundäre Verbindungszeichenfolge, die zur Authentifizierung verwendet wird |
    | *yourTopicName* | Den Namen Ihres angepassten Themas |

- Ereignistypen für das Weiterleiten an **Event Hub**: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourEventHubName"
  }
  ```

    | Name des benutzerdefinierten Attributs | Ersetzen durch |
    | --- | --- |
    | *yourNamespace* | Der Namespace Ihres Endpunkts |
    | *yourPrimaryKey* | Die primäre Verbindungszeichenfolge, die zur Authentifizierung verwendet wird |
    | *yourSecondaryKey* | Die sekundäre Verbindungszeichenfolge, die zur Authentifizierung verwendet wird |
    | *yourEventHubName* | Der Name Ihres **Event Hubs** |

- Ereignistyp für das Weiterleiten an **Event Hub**: **DeviceMessage**. `EntityPath` muss in **connectionString** enthalten sein.

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey;EntityPath=yourEventHubName",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey;EntityPath=yourEventHubName",
    "path": "yourEventHubName"
  }
  ```

    | Name des benutzerdefinierten Attributs | Ersetzen durch |
    | --- | --- |
    | *yourNamespace* | Der Namespace Ihres Endpunkts |
    | *yourPrimaryKey* | Die primäre Verbindungszeichenfolge, die zur Authentifizierung verwendet wird |
    | *yourSecondaryKey* | Die sekundäre Verbindungszeichenfolge, die zur Authentifizierung verwendet wird |
    | *yourEventHubName* | Der Name Ihres **Event Hubs** |

> [!NOTE]
> Nach dem Erstellen eines neuen Endpunkts kann es 5 bis 10 Minuten dauern, bis erstmals Ereignisse am Endpunkt empfangen werden.

## <a name="primary-and-secondary-connection-keys"></a>Primärer und sekundärer Verbindungsschlüssel

Wenn ein primärer Verbindungsschlüssel nicht länger autorisiert ist, versucht das System automatisch, den sekundären Verbindungsschlüssel zu verwenden. Dies bietet eine Sicherung und eröffnet die Möglichkeit, den primären Schlüssel ungestört über die API-Endpunkte zu authentifizieren und zu aktualisieren.

Wenn sowohl der primäre als auch der sekundäre Verbindungsschlüssel nicht autorisiert sind, wechselt das System in eine exponentielle Backoff-Wartezeit von bis zu 30 Minuten. Ereignisse werden bei jeder ausgelösten Backoff-Wartezeit gelöscht.

Wenn sich das System in einem Backoff-Wartezustand befindet, kann es bis zu 30 Minuten dauern, bis ein Aktualisieren von Verbindungsschlüsseln über die Endpunkte-API wirksam wird.

## <a name="unreachable-endpoints"></a>Nicht erreichbare Endpunkte

Wenn ein Endpunkt nicht mehr erreichbar ist, wechselt das System eine exponentielle Backoff-Wartezeit von bis zu 30 Minuten. Ereignisse werden bei jeder ausgelösten Backoff-Wartezeit gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Azure Digital Twins Swagger verwendet werden kann, finden Sie unter [Verwenden von Azure Digital Twins Swagger](how-to-use-swagger.md).

Weitere Informationen zum Weiterleiten von Ereignissen und Meldungen in Azure Digital Twins finden Sie unter [Weiterleiten von Ereignissen und Nachrichten](concepts-events-routing.md).
