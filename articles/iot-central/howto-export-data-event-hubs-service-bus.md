---
title: Exportieren Ihrer Daten in Azure Event Hubs und Azure Service Bus | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Daten aus Ihrer Azure IoT Central-Anwendung in Azure Event Hubs und Azure Service Bus exportiert werden.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 14b51f109ca76661ac10c99d42002dda45bc0500
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318403"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportieren von Daten in Azure IoT Central

*Dieses Thema gilt für Administratoren.*

In diesem Artikel wird ausführlich beschrieben, wie Sie in Azure IoT Central das Feature für den fortlaufenden Datenexport verwenden, um Daten regelmäßig in Ihre **Azure Event Hubs**- und **Azure Service Bus**-Instanzen zu exportieren. Sie können **Messungen**, **Geräte** und **Gerätevorlagen** an Ihr eigenes Ziel exportieren, um Einblicke in Pfade und Analysen zu erhalten. Dies umfasst das Auslösen benutzerdefinierter Regeln in Azure Stream Analytics, das Auslösen benutzerdefinierter Workflows in Azure Logic Apps oder das Transformieren der Daten und deren Weiterleitung durch Azure Functions. 

> [!Note]
> Zur Erinnerung: Wenn Sie den fortlaufenden Datenexport aktivieren, erhalten Sie nur die Daten ab dem jeweiligen Aktivierungszeitpunkt. Derzeit können Daten nicht für Zeiten abgerufen werden, in denen der Datenexport deaktiviert war. Aktivieren Sie den fortlaufenden Datenexport frühzeitig, um umfassendere Verlaufsdaten zu erhalten.


## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen Administrator in Ihrer IoT Central-Anwendung sein.

## <a name="export-to-azure-event-hubs-and-azure-service-bus"></a>Exportieren in Azure Event Hubs und Azure Service Bus

Messungen, Geräte und Gerätevorlagen werden nahezu in Echtzeit in Ihren Event Hub, Ihre Service Bus-Warteschlange oder ein Thema exportiert. Exportierte Messdaten enthalten die Gesamtheit der Nachricht, die Ihre Geräte an IoT Central gesendet haben, nicht nur die Werte der Messungen selbst. Exportierte Gerätedaten enthalten Änderungen an Eigenschaften und Einstellungen aller Geräte, und exportierte Gerätevorlagen enthalten Änderungen an allen Gerätevorlagen. Die exportierten Daten befinden sich in der Eigenschaft „body“ und haben das JSON-Format.

> [!NOTE]
> Wenn Sie eine Service Bus-Instanz als Exportziel auswählen, dürfen für Warteschlangen und Themen **weder „Sitzungen“ noch „Duplikaterkennung“ aktiviert sein**. Wenn eine dieser Optionen aktiviert ist, werden einige Nachrichten nicht in der Warteschlange oder im Thema eingehen.

### <a name="measurements"></a>Messungen

Eine neue Nachricht wird schnell exportiert, nachdem IoT Central die Nachricht von einem Gerät empfangen hat. Jede exportierte Nachricht in Event Hubs und Service Bus enthält die vollständige Nachricht, die das Gerät gesendet hat, in der Eigenschaft „body“ im JSON-Format. 

> [!NOTE]
> Die Geräte, die die Messdaten senden, werden durch Geräte-IDs (siehe folgende Abschnitte) dargestellt. Um die Namen der Geräte zu erhalten, exportieren Sie Gerätedaten. Korrelieren Sie anschließend die einzelnen Nachrichten, indem Sie jeweils die **connectionDeviceId** verwenden, die mit der **deviceId** der Gerätenachricht übereinstimmt.

Das folgende Beispiel zeigt eine Nachricht zu Messdaten, die im Event Hub, in der Service-Bus-Warteschlange oder im Thema empfangen wurden.

```json
{
  "body": {
    "humidity": 29.06963648666288,
    "temp": 8.4503795661685,
    "pressure": 1075.8334910110093,
    "magnetometerX": 408.6966458887116,
    "magnetometerY": -532.8809796603962,
    "magnetometerZ": 174.70653875528205,
    "accelerometerX": 1481.546749013788,
    "accelerometerY": -1139.4316656437406,
    "accelerometerZ": 811.6928695575307,
    "gyroscopeX": 442.19879163299856,
    "gyroscopeY": 123.23710975717177,
    "gyroscopeZ": 708.5397575786151,
    "deviceState": "DANGER"
  },
  "annotations": {
    "iothub-connection-device-id": "<connectionDeviceId>",
    "iothub-connection-auth-method": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id": "<generationId>",
    "iothub-enqueuedtime": 1539381029965,
    "iothub-message-source": "Telemetry",
    "x-opt-sequence-number": 25325,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539381030200
  },
  "sequenceNumber": 25325,
  "enqueuedTimeUtc": "2018-10-12T21:50:30.200Z",
  "offset": "<offset>",
  "properties": {
    "content_type": "application/json",
    "content_encoding": "utf-8"
  }
}
```

### <a name="devices"></a>Geräte

Nachrichten, die Gerätedaten enthalten, werden alle paar Minuten an Ihren Event Hub, Ihre Service Bus-Warteschlange oder Ihr Thema gesendet. Das bedeutet, dass alle paar Minuten ein Batch von Nachrichten mit Daten zu Folgendem eingeht
- Neuen Geräten, die hinzugefügt wurden
- Geräten mit geänderten Eigenschafts- und Einstellungswerten

Jede Nachricht stellt eine oder mehrere Änderungen an einem Gerät seit der letzten exportierten Nachricht dar. Zu den Informationen, die in jeder Nachricht gesendet werden, gehören die folgenden:
- `id` des Geräts in IoT Central
- `name` des Geräts
- `deviceId` von [Device Provisioning Service](https://aka.ms/iotcentraldocsdps)
- Gerätevorlageninformationen
- Eigenschaftswerte
- Einstellungswerte

> [!NOTE]
> Geräte, die seit dem letzten Batch gelöscht wurden, werden nicht exportiert. Derzeit gibt es in exportierten Nachrichten keine Kennzeichnung gelöschter Geräte.
>
> Die Gerätevorlage, der die einzelnen Geräte zugewiesen sind, wird durch eine Gerätevorlagen-ID dargestellt. Um den Namen der Gerätevorlage abzurufen, müssen Sie auch Gerätevorlagendaten exportieren.

Das folgende Beispiel zeigt eine Nachricht zu Gerätedaten im Event Hub, in der Service-Bus-Warteschlange oder im Thema:


```json
{
  "body": {
    "id": "<id>",
    "name": "<deviceName>",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
      "id": "<templateId>",
      "version": "1.0.0"
    },
    "properties": {
      "cloud": {
        "location": "Seattle"
      },
      "device": {
        "dieNumber": 5445.5862873026645
      }
    },
    "settings": {
      "device": {
        "fanSpeed": 0
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "devices",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 39740,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274959654
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 39740,
  "enqueuedTimeUtc": "2018-10-11T16:22:39.654Z",
  "offset": "<offset>",
}
```

### <a name="device-templates"></a>Gerätevorlagen

Nachrichten, die Gerätevorlagendaten enthalten, werden alle paar Minuten an Ihren Event Hub, Ihre Service Bus-Warteschlange oder Ihr Thema gesendet. Das bedeutet, dass alle paar Minuten ein Batch von Nachrichten mit Daten zu Folgendem eingeht
- Neuen Gerätevorlagen, die hinzugefügt wurden
- Gerätevorlagen, deren Definition für Messungen, Eigenschaften und Einstellungen geändert wurden

Jede Nachricht stellt eine oder mehrere Änderungen an einer Gerätevorlage seit der letzten exportierten Nachricht dar. Zu den Informationen, die in jeder Nachricht gesendet werden, gehören die folgenden:
- `id` der Gerätevorlage
- `name` der Gerätevorlage
- `version` der Gerätevorlage
- Typen der Messdaten und Mindest- bzw. Maximalwerte
- Typen von Eigenschaftsdaten und Standardwerte
- Typen von Einstellungsdaten und Standardwerte

> [!NOTE]
> Gerätevorlagen, die seit dem letzten Batch gelöscht wurden, werden nicht exportiert. Derzeit gibt es in exportierten Nachrichten keine Kennzeichnung gelöschter Gerätevorlagen.

Das folgende Beispiel zeigt eine Nachricht zu Gerätevorlagendaten im Event Hub, in der Service-Bus-Warteschlange oder im Thema:

```json
{
  "body": {
    "id": "<id>",
    "version": "1.0.0",
    "name": "<templateName>",
    "measurements": {
      "telemetry": {
        "humidity": {
          "dataType": "double",
          "name": "humidity"
        },
        "pressure": {
          "dataType": "double",
          "name": "pressure"
        },
        "temp": {
          "dataType": "double",
          "name": "temperature"
        }
      }
    },
    "properties": {
      "cloud": {
        "location": {
          "dataType": "string",
          "name": "Location"
        }
      },
      "device": {
        "dieNumber": {
          "dataType": "double",
          "name": "Die Number"
        }
      }
    },
    "settings": {
      "device": {
        "fanSpeed": {
          "dataType": "double",
          "name": "Fan Speed",
          "initialValue": 0
        }
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "deviceTemplates",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 25315,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274985085
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 25315,
  "enqueuedTimeUtc": "2018-10-11T16:23:05.085Z",
  "offset": "<offset>",
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt mit dem Exportieren Ihrer Daten in Azure Event Hubs und Azure Service Bus vertraut sind, können Sie mit dem nächsten Schritt fortfahren:

> [!div class="nextstepaction"]
> [Auslösen von Azure Functions](howto-trigger-azure-functions.md)
