---
title: "Azure Event Grid-Schema für IoT Hub | Microsoft-Dokumentation"
description: "Referenzseite für das Schemaformat und die Eigenschaften von Ereignissen in IoT Hub"
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 29ad1233a344c3085286c27cb925b2dc9fb41f7e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="azure-event-grid-event-schema-for-iot-hub"></a>Azure Event Grid-Ereignisschema für IoT Hub

In diesem Artikel werden die Eigenschaften und das Schema für Azure IoT Hub-Ereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md). 

## <a name="available-event-types"></a>Verfügbare Ereignistypen

Azure IoT Hub gibt die folgenden Ereignistypen aus:

| Ereignistypen | Beschreibung |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Wird ausgelöst, wenn ein Gerät bei einem IoT Hub registriert wird. |
| Microsoft.Devices.DeviceDeleted | Wird ausgelöst, wenn ein Gerät aus einem IoT Hub gelöscht wird. | 

## <a name="example-event"></a>Beispielereignis

Das Schema für DeviceCreated- und DeviceDeleted-Ereignisse verfügen über die gleiche Struktur. Das Beispielereignis zeigt das Schema eines Ereignisses, das ausgelöst wird, wenn ein Gerät bei einem IoT Hub registriert wird:

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

### <a name="event-properties"></a>Ereigniseigenschaften

Alle Ereignisse enthalten die gleichen Daten der obersten Ebene: 

| Eigenschaft | Typ | Beschreibung |
| -------- | ---- | ----------- |
| id | string | Eindeutiger Bezeichner für das Ereignis. |
| topic | string | Vollständiger Ressourcenpfad zu der Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt. |
| subject | string | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | string | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | string | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| data | object | IoT Hub-Ereignisdaten.  |
| dataVersion | string | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | string | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Die Inhalte des Datenobjekts unterscheiden sich für jeden Ereignisherausgeber. Für IoT Hub-Ereignisse enthält das Datenobjekt die folgenden Eigenschaften:

| Eigenschaft | Typ | Beschreibung |
| -------- | ---- | ----------- |
| hubName | string | Name des IoT Hubs, in dem das Gerät erstellt bzw. aus dem das Gerät gelöscht wurde. |
| deviceId | string | Der eindeutige Bezeichner des Geräts. Eine Zeichenfolge mit Beachtung von Groß-/Kleinschreibung, die bis zu 128 Zeichen lang sein kann und alphanumerische 7-Bit-ASCII-Zeichen sowie die folgenden Sonderzeichen unterstützt: `- : . + % _ # * ? ! ( ) , = @ ; $ '`. |
| operationTimestamp | string | Der ISO8601-Zeitstempel des Vorgangs. |
| opType | string | Der Ereignistyp, der von IoT Hub für diesen Vorgang angegeben wurde: entweder `DeviceCreated` oder `DeviceDeleted`.
| twin | object | Informationen zum Gerätezwilling, der Cloudrepräsentation der Anwendungsgeräte-Metadaten. | 
| deviceID | string | Der eindeutige Bezeichner des Gerätezwillings. | 
| etag | string | Informationen, die den Inhalt des Gerätezwillings beschreiben. Jedes ETag ist pro Gerätezwilling garantiert eindeutig. | 
| status | string | Gibt an, ob der Gerätezwilling aktiviert oder deaktiviert ist. | 
| statusUpdateTime | string | Der ISO8601-Zeitstempel der letzten Statusaktualisierung für den Gerätezwilling. |
| connectionState | string | Gibt an, ob das Gerät verbunden oder getrennt ist. | 
| lastActivityTime | string | Der ISO8601-Zeitstempel der letzten Aktivität. | 
| cloudToDeviceMessageCount | integer | Die Anzahl von Nachrichten, die von der Cloud an das Gerät gesendet wurden. | 
| authenticationType | string | Der für dieses Gerät verwendete Authentifizierungstyp: entweder `SAS`, `SelfSigned` oder `CertificateAuthority`. |
| x509Thumbprint | string | Der Fingerabdruck ist ein eindeutiger Wert für das x509-Zertifikat. Dieser wird üblicherweise zur Suche nach einem bestimmten Zertifikat in einem Zertifikatspeicher verwendet. Der Fingerabdruck wird mithilfe des SHA1-Algorithmus dynamisch generiert und ist nicht physisch im Zertifikat vorhanden. | 
| primaryThumbprint | string | Der primäre Fingerabdruck für das x509-Zertifikat. |
| secondaryThumbprint | string | Der sekundäre Fingerabdruck für das x509-Zertifikat. | 
| version | integer | Ein ganzzahliger Wert, der bei jeder Aktualisierung des Gerätezwillings um 1 erhöht wird. |
| desired | object | Ein Teil der Eigenschaften, der nur vom Anwendungs-Back-End geschrieben und vom Gerät gelesen werden kann. | 
| reported | object | Ein Teil der Eigenschaften, der nur vom Gerät geschrieben und vom Anwendungs-Back-End gelesen werden kann. |
| lastUpdated | string | Der ISO8601-Zeitstempel der letzten Eigenschaftenaktualisierung für den Gerätezwilling. | 

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zur Zusammenarbeit von IoT Hub und Event Grid finden Sie unter [Reagieren auf IoT Hub-Ereignisse mithilfe von Event Grid zum Auslösen von Aktionen](../iot-hub/iot-hub-event-grid.md).