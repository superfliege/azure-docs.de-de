---
title: Geräteschema in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt das JSON-Schema, das ein simuliertes Gerät in der Remoteüberwachungslösung definiert.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 186eaee952435573a861d144195c3165e4940cc1
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="understand-the-device-model-schema"></a>Grundlegendes zum Gerätemodellschema

Sie können simulierte Geräte in der Remoteüberwachungslösung verwenden, um deren Verhalten zu testen. Beim Bereitstellen der Remoteüberwachungslösung wird automatisch eine Sammlung simulierter Geräte bereitgestellt. Sie können die vorhandenen simulierten Geräte anpassen oder eigene erstellen.

Dieser Artikel beschreibt das Gerätemodellschema, das die Funktionen und das Verhalten eines simulierten Geräts bestimmt. Das Gerätemodell ist in einer JSON-Datei gespeichert.

Die folgenden Artikel beziehen sich auf den aktuellen Artikel:

* [Implementieren des Gerätemodellverhaltens](../iot-suite/iot-suite-remote-monitoring-device-behavior.md) beschreibt die JavaScript-Dateien, mit denen Sie das Verhalten eines simulierten Geräts implementieren.
* [Erstellen eines neuen simuliertes Geräts](iot-accelerators-remote-monitoring-test.md) zeigt, wie Sie einen neuen simulierten Gerätetyp in Ihrer Lösung bereitstellen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

>[!div class="checklist"]
> * Verwenden einer JSON-Datei, um ein simuliertes Gerätemodell zu definieren
> * Festlegen der Eigenschaften des simulierten Geräts
> * Festlegen der Telemetrie, die das simulierte Gerät sendet
> * Festlegen der Cloud-zu-Gerät-Methoden (C2D), auf die das Gerät reagiert

## <a name="the-parts-of-the-device-model-schema"></a>Die Komponenten des Gerätemodellschemas

Jedes Gerätemodell, z.B. eine Kühlanlage oder ein LKW, definiert einen simulierten Gerätetyp zum Verbinden mit der Remoteüberwachungslösung. Jedes Gerätemodell wird in einer JSON-Datei mit folgendem Schema gespeichert:

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "elevator-01",
  "Version": "0.0.1",
  "Name": "Elevator",
  "Description": "Elevator with floor, vibration and temperature sensors.",
  "Protocol": "AMQP",
  "Simulation": {
    // Specify the simulation behavior
  },
  "Properties": {
    // Define properties
  },
  "Telemetry": [
    // Specify telemetry
  ],
  "CloudToDeviceMethods": {
    // Specify methods
  }
}
```

Die Schemadateien für die simulierten Standardgeräte können Sie im [devicemodels](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels)-Ordner auf GitHub anzeigen.

Die folgende Tabelle beschreibt die Schemaeinträge der obersten Ebene:

| Schemaeintrag | BESCHREIBUNG |
| -- | --- |
| `SchemaVersion` | Die Schemaversion ist immer `1.0.0` und bezieht sich auf das Format dieser Datei. |
| `Id` | Die eindeutige ID für dieses Gerätemodell. |
| `Version` | Identifiziert die Version des Gerätemodells. |
| `Name` | Der Anzeigename für das Gerätemodell. |
| `Description` | Die Beschreibung des Gerätelebenszyklus. |
| `Protocol` | Das vom Gerät verwendete Verbindungsprotokoll. Mögliche Optionen: `AMQP`, `MQTT` und `HTTP`. |

In den folgenden Abschnitten werden die anderen Abschnitte im JSON-Schema beschrieben:

## <a name="simulation"></a>Simulation

Im Abschnitt `Simulation` können Sie den internen Status des simulierten Geräts festlegen. Sämtliche vom Gerät gesendeten Telemetriewerte müssen Teil dieses Gerätestatus sein.

Die Definition des Gerätestatus besteht aus zwei Elementen:

* `InitialState` definiert Anfangswerte für alle Eigenschaften des Gerätestatusobjekts.
* `Script` identifiziert eine JavaScript-Datei, die nach einem Zeitplan zum Aktualisieren des Gerätestatus ausgeführt wird. Mit dieser Skriptdatei können Sie die vom Gerät gesendeten Telemetriewerte zufällig festlegen.

Weitere Informationen zu der JavaScript-Datei, die das Gerätestatusobjekt aktualisiert, finden Sie unter [Grundlegendes zum Gerätemodellverhalten](iot-accelerators-remote-monitoring-device-behavior.md).

Das folgende Beispiel zeigt die Definition des Gerätestatusobjekts für eine simulierte Kühlanlage:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Interval": "00:00:10",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

Der Simulationsdienst führt die Datei **chiller-01-state.js** alle fünf Sekunden aus, um den Gerätestatus zu aktualisieren. Die JavaScript-Dateien für die simulierten Standardgeräte können Sie im [devicemodels](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts)-Ordner auf GitHub anzeigen. Gemäß der Konvention weisen diese JavaScript-Dateien das Suffix **-state** auf, um sie von den Dateien zu unterscheiden, die das Methodenverhalten implementieren.

## <a name="properties"></a>Eigenschaften

Der Abschnitt `Properties` des Schemas definiert die Eigenschaftswerte, die das Gerät an die Lösung meldet. Beispiel: 

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

Wenn die Lösung startet, fragt sie alle simulierten Geräte ab, um Werte vom Typ `Type` für die Benutzeroberfläche zu erstellen. Die Lösung verwendet die Eigenschaften `Latitiude` und `Longitude`, um der Karte auf dem Dashboard den Speicherort des Geräts hinzuzufügen.

## <a name="telemetry"></a>Telemetrie

Das Array `Telemetry` führt alle Telemetrietypen auf, die das simulierte Gerät an die Lösung sendet.

Im folgenden Beispiel wird alle 10 Sekunden eine JSON-Telemetrienachricht mit den Daten vom Typ `floor`, `vibration` und `temperature` von den Aufzugsensoren gesendet:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate": "{\"floor\":${floor},\"vibration\":${vibration},\"vibration_unit\":\"${vibration_unit}\",\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
    "MessageSchema": {
      "Name": "elevator-sensors;v1",
      "Format": "JSON",
      "Fields": {
        "floor": "integer",
        "vibration": "double",
        "vibration_unit": "text",
        "temperature": "double",
        "temperature_unit": "text"
      }
    }
  }
]
```

`MessageTemplate` definiert die Struktur der JSON-Nachricht, die vom simulierten Gerät gesendet wird. Die Platzhalter in `MessageTemplate` verwenden die Syntax `${NAME}`, in der `NAME` ein Schlüssel aus dem [Gerätestatusobjekt](#simulation) ist. Zeichenfolgen müssen in Anführungszeichen gesetzt werden, Zahlen nicht.

`MessageSchema` definiert das Schema der Nachricht, die vom simulierten Gerät gesendet wird. Das Nachrichtenschema wird auch im IoT Hub veröffentlicht, damit Back-End-Anwendungen die Informationen zum Interpretieren der eingehenden Telemetriedaten wiederverwenden können.

Derzeit können Sie nur JSON-Nachrichtenschemas verwenden. Die im Schema aufgeführten Felder können den folgenden Typen entsprechen:

* Objekt – mit JSON serialisiert
* Binär – mit base64 serialisiert
* Text
* Boolescher Wert
* Ganze Zahl 
* Double
* Datetime

Fügen Sie dem Array `Telemetry` mehrere Telemetrietypen hinzu, um Telemetrienachrichten in verschiedenen Intervallen zu senden. Im folgenden Beispiel werden die Daten zur Temperatur und Luftfeuchtigkeit alle 10 Sekunden und der Status des Lichts einmal pro Minute gesendet:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate":
      "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"humidity\":\"${humidity}\"}",
    "MessageSchema": {
      "Name": "RoomComfort;v1",
      "Format": "JSON",
      "Fields": {
        "temperature": "double",
        "temperature_unit": "text",
        "humidity": "integer"
      }
    }
  },
  {
    "Interval": "00:01:00",
    "MessageTemplate": "{\"lights\":${lights_on}}",
    "MessageSchema": {
      "Name": "RoomLights;v1",
      "Format": "JSON",
      "Fields": {
        "lights": "boolean"
      }
    }
  }
],
```

## <a name="cloudtodevicemethods"></a>CloudToDeviceMethods

Ein simuliertes Gerät kann auf von der Remoteüberwachungslösung abgerufene Cloud-zu-Gerät-Methoden reagieren. Der Abschnitt `CloudToDeviceMethods` in der Schemadatei zum Gerätemodell:

* definiert die Methoden, auf die das simulierte Gerät reagieren kann.
* identifiziert die JavaScript-Datei, die die auszuführende Logik enthält.

Das simulierte Gerät sendet die Liste der unterstützten Methoden an die Remoteüberwachungslösung.

Weitere Informationen zu der JavaScript-Datei, die das Geräteverhalten implementiert, finden Sie unter [Grundlegendes zum Gerätemodellverhalten](iot-accelerators-remote-monitoring-device-behavior.md).

Im folgenden Beispiel werden drei unterstützte Methoden und die JavaScript-Dateien, die diese Methoden implementieren, festgelegt:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Die JavaScript-Dateien für die simulierten Standardgeräte können Sie im [devicemodels](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts)-Ordner auf GitHub anzeigen. Gemäß der Konvention weisen diese JavaScript-Dateien das Suffix **-method** auf, um sie von den Dateien zu unterscheiden, die das Statusverhalten implementieren.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt, wie Sie Ihr eigenes benutzerdefiniertes simuliertes Gerätemodell erstellen. In diesem Artikel haben Sie erfahren, wie Sie Folgendes durchführen:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Verwenden einer JSON-Datei, um ein simuliertes Gerätemodell zu definieren
> * Festlegen der Eigenschaften des simulierten Geräts
> * Festlegen der Telemetrie, die das simulierte Gerät sendet
> * Festlegen der Cloud-zu-Gerät-Methoden (C2D), auf die das Gerät reagiert

Nachdem Sie nun mit dem JSON-Schema vertraut sind, sollten Sie mehr über das [Implementieren des Verhaltens Ihres simulierten Geräts](iot-accelerators-remote-monitoring-device-behavior.md) erfahren.

Weitere Informationen für Entwickler zur Remoteüberwachungslösung finden Sie in folgenden Quellen:

* [Referenzhandbuch für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Handbuch zur Problembehandlung für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->