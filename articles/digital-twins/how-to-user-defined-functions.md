---
title: Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins | Microsoft-Dokumentation
description: Anleitung zum Erstellen von benutzerdefinierten Funktionen, Matchern (Abgleicher) und Rollenzuweisungen mit Azure Digital Twins.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 7fbaff5ed1b60a4434ba2eb0c78c6aa1f3fd6645
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323857"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins

[Benutzerdefinierte Funktionen](./concepts-user-defined-functions.md) ermöglichen es dem Benutzer, benutzerdefinierte Logik für eingehende Telemetrienachrichten und Raumgraphmetadaten auszuführen, sodass der Benutzer Ereignisse an vordefinierte Endpunkte senden kann. In diesem Leitfaden durchlaufen Sie ein Beispiel zur Verarbeitung von Temperaturereignissen, um jede Messung zu erkennen, bei der eine bestimmte Temperatur überschritten wurde, und eine Warnung für eine solche Messung zu senden.

In den folgenden Beispielen bezieht sich `https://yourManagementApiUrl` auf den URI der Digital Twins-APIs:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourInstanceName` | Den Namen Ihrer Azure Digital Twins-Instanz |
| `yourLocation` | Die Serverregion, in der Ihre Instanz gehostet wird |

## <a name="client-library-reference"></a>Referenz zur Clientbibliothek

Die Funktionen, die als Hilfsmethoden in der Runtime für benutzerdefinierte Funktionen verfügbar sind, sind in der folgenden [Clientreferenz](#Client-Reference) aufgeführt.

## <a name="create-a-matcher"></a>Erstellen eine Matchers

Matchers (Abgleicher) sind Graphobjekte, die bestimmen, welche benutzerdefinierten Funktionen für eine bestimmte Telemetrienachricht ausgeführt werden.

Gültige Matcherbedingungsvergleiche:

- `Equals`
- `NotEquals`
- `Contains`

Gültige Matcherbedingungsziele:

- `Sensor`
- `SensorDevice`
- `SensorSpace`

Der folgende Beispielmatcher hat den Ergebniswert „true“ für jedes Sensortelemetrieereignis, das den Datentyp `Temperature` hat. Sie können mehrere Matcher für eine benutzerdefinierte Funktion erstellen.

```text
POST https://yourManagementApiUrl/api/v1.0/matchers
{
  "Name": "Temperature Matcher",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ],
  "SpaceId": "yourSpaceIdentifier"
}
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourManagementApiUrl` | Den vollständigen URL-Pfad für Ihre Verwaltungs-API  |
| `yourSpaceIdentifier` | Die Serverregion, in der Ihre Instanz gehostet wird |

## <a name="create-a-user-defined-function-udf"></a>Erstellen einer benutzerdefinierten Funktion (UDF)

Nachdem der Matcher erstellt wurde, laden Sie den Funktionsausschnitt mit dem folgenden POST-Aufruf hoch:

> [!IMPORTANT]
> - Legen Sie in den Headern Folgendes fest: `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - Der Hauptteil ist mehrteilig:
>   - Im ersten Teil geht es um die Metadaten, die für die benutzerdefinierte Funktion erforderlich sind.
>   - Der zweite Teil enthält die Javascript-Compute-Logik.
> - Ersetzen Sie im `userDefinedBoundary`-Abschnitt die `SpaceId`- und die `Machers`-GUID.

```plaintext
POST https://yourManagementApiUrl/api/v1.0/userdefinedfunctions with Content-Type: multipart/form-data; boundary="userDefinedBoundary"
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourManagementApiUrl` | Den vollständigen URL-Pfad für Ihre Verwaltungs-API  |

Hauptteil:

```plaintext
--userDefinedBoundary
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "yourSpaceIdentifier",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["yourMatcherIdentifier"]
}
--userDefinedBoundary
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--userDefinedBoundary--
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourSpaceIdentifier` | Den Raumbezeichner (Raum-ID)  |
| `yourMatcherIdentifier` | Die ID des Matchers, den Sie verwenden möchten |

### <a name="example-functions"></a>Beispielfunktionen

Legen Sie die Sensortelemetriemesswert direkt für den Sensor mit dem Datentyp `Temperature` fest, der „sensor.DataType“ entspricht:

```javascript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

Protokollieren Sie eine Nachricht, wenn der Sensortelemetriemesswert einen vordefinierten Schwellenwert überschreitet. Wenn Ihre Diagnoseeinstellungen in der Digital Twins-Instanz aktiviert sind, werden Protokolle von benutzerdefinierten Funktionen weitergeleitet:

```javascript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

Im folgenden Code wird eine Benachrichtigung ausgelöst, wenn die Temperatur den festgelegten Wert der Konstanten überschreitet.

```javascript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Ein komplexeres Beispiel einer benutzerdefinierten Funktion finden Sie in der [benutzerdefinierten Funktion zum Prüfen auf verfügbare Räume mit frischer Luft](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Erstellen einer Rollenzuweisung

Es muss eine Rollenzuweisung erstellt werden, unter der die benutzerdefinierte Funktion ausgeführt werden kann. Wird keine solche Zuweisung erstellt, hat die Funktion nicht die richtigen Berechtigungen zur Verwendung der Verwaltungs-API, um Aktionen für Graphobjekte auszuführen. Die Aktionen, die in der benutzerdefinierten Funktion ausgeführt werden, sind nicht von der rollenbasierten Zugriffskontrolle in den Digital Twins-Verwaltungs- APIs ausgenommen. Sie können hinsichtlich des Geltungsbereichs eingeschränkt werden, indem bestimmte Rollen oder bestimmte Zugriffssteuerungspfade angegeben werden. Weitere Informationen finden Sie in dem Artikel [Rollenbasierte Zugriffssteuerung](./security-role-based-access-control.md).

- Fragen Sie die Rollen ab, und rufen Sie die ID der Rolle ab, die Sie der benutzerdefinierten Funktion zuweisen möchten. Übergeben Sie die ID an „RoleId“ (siehe weiter unten).

```plaintext
GET https://yourManagementApiUrl/api/v1.0/system/roles
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourManagementApiUrl` | Den vollständigen URL-Pfad für Ihre Verwaltungs-API  |

- „ObjectId“ ist die früher erstellte ID der benutzerdefinierten Funktion.
- Suchen Sie nach `Path`, indem Sie die Bereiche (Spaces) mit ihrem vollständigen Pfad abfragen, und kopieren Sie den Wert von `spacePaths`. Fügen Sie diesen Wert in „Path“ ein, wenn Sie die Rollenzuweisung für die benutzerdefinierte Funktion erstellen.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/spaces?name=yourSpaceName&includes=fullpath
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourManagementApiUrl` | Den vollständigen URL-Pfad für Ihre Verwaltungs-API  |
| `yourSpaceName` | Der Name des Raums, den Sie verwenden möchten |

```plaintext
POST https://yourManagementApiUrl/api/v1.0/roleassignments
{
  "RoleId": "yourDesiredRoleIdentifier",
  "ObjectId": "yourUserDefinedFunctionId",
  "ObjectIdType": "UserDefinedFunctionId",
  "Path": "yourAccessControlPath"
}
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourManagementApiUrl` | Den vollständigen URL-Pfad für Ihre Verwaltungs-API  |
| `yourDesiredRoleIdentifier` | Der Bezeichner für die gewünschte Rolle |
| `yourUserDefinedFunctionId` | Die ID für die benutzerdefinierte Funktion, die Sie verwenden möchten |
| `yourAccessControlPath` | Der Zugriffssteuerungspfad |

## <a name="send-telemetry-to-be-processed"></a>Senden der Telemetriedaten, die verarbeitet werden sollen

Telemetriedaten, die von dem Sensor erfasst werden, der im Graphen beschrieben ist, müssen die Ausführung der benutzerdefinierten Funktion auslösen, die hochgeladen wurde. Sobald die Telemetriedaten vom Datenprozessor übernommen wurden, wird ein Ausführungsplan für den Aufruf der benutzerdefinierten Funktion erstellt.

1. Rufen Sie die Matcher für den Sensor ab, von dem der Messwert generiert wurde.
1. Je nachdem, welche Matcher erfolgreich ausgewertet wurden, rufen Sie die zugehörigen benutzerdefinierten Funktionen ab.
1. Führen Sie jede benutzerdefinierte Funktion aus.

## <a name="client-reference"></a>Clientreferenz

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Bei angegebenem Raumbezeichner wird der Raum aus dem Graphen abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| id  | `guid` | Raumbezeichner |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Bei angegebenem Sensorbezeichner wird der Sensor aus dem Graphen abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| id  | `guid` | Sensorbezeichner |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Bei angegebenem Gerätebezeichner wird das Gerät aus dem Graphen abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| id  | `guid` | Gerätebezeichner |

### <a name="getsensorvaluesensorid-datatype--value"></a>getSensorValue(sensorId, dataType) ⇒ `value`

Nach Angabe eines Sensorbezeichners und dessen Datentyps wird der aktuelle Wert dieses Sensors abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Sensorbezeichner |
| dataType  | `string` | Sensordatentyp |

### <a name="getspacevaluespaceid-valuename--value"></a>getSpaceValue(spaceId, valueName) ⇒ `value`

Nach Angabe eines Raumbezeichners und des Wertnamens wird der aktuelle Wert dieser Raumeigenschaft abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Raumbezeichner |
| valueName  | `string` | Name der Raumeigenschaft |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>getSensorHistoryValues(sensorId, dataType) ⇒ `value[]`

Nach Angabe eines Sensorbezeichners und des Sensordatentyps wird der aktuelle Wert dieses Sensors abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Sensorbezeichner |
| dataType  | `string` | Sensordatentyp |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>getSpaceHistoryValues(spaceId, dataType) ⇒ `value[]`

Nach Angabe eines Raumbezeichners und des Wertnamens werden die früheren Werte dieser Raumeigenschaft abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Raumbezeichner |
| valueName  | `string` | Name der Raumeigenschaft |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Nach Angabe eines Raumbezeichners werden die untergeordneten Räume dieses übergeordneten Raums abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Raumbezeichner |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Nach Angabe eines Raumbezeichners werden die untergeordneten Sensoren dieses übergeordneten Raums abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Raumbezeichner |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Nach Angabe eines Raumbezeichners werden die untergeordneten Geräte dieses übergeordneten Raums abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Raumbezeichner |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Nach Angabe eines Raumbezeichners werden die untergeordneten Sensoren dieses übergeordneten Geräts abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| deviceId  | `guid` | Gerätebezeichner |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Nach Angabe eines Raumbezeichners wird der übergeordnete Raum dieses Raums abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| childSpaceId  | `guid` | Raumbezeichner |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Nach Angabe eines Sensorbezeichners wird der übergeordnete Raum dieses Sensors abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| childSensorId  | `guid` | Sensorbezeichner |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Nach Angabe eines Gerätebezeichners wird der übergeordnete Raum dieses Geräts abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| childDeviceId  | `guid` | Gerätebezeichner |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Nach Angabe eines Sensorbezeichners wird das übergeordnete Gerät dieses Sensors abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| childSensorId  | `guid` | Sensorbezeichner |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>getSpaceExtendedProperty(spaceId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Raumbezeichners und des Namens der Eigenschaft wird deren Wert für den Raum abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Raumbezeichner |
| propertyName  | `string` | Name der Raumeigenschaft |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>getSensorExtendedProperty(sensorId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Sensorbezeichners und des Namens der Eigenschaft wird deren Wert für den Sensor abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Sensorbezeichner |
| propertyName  | `string` | Name der Sensoreigenschaft |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>getDeviceExtendedProperty(deviceId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Gerätebezeichners und des Namens der Eigenschaft wird deren Wert für das Gerät abgerufen.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| deviceId  | `guid` | Gerätebezeichner |
| propertyName  | `string` | Name der Geräteeigenschaft |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue(sensorId, dataType, value)

Legt den Wert des Sensorobjekts mit dem angegebenen Datentyp fest.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Sensorbezeichner |
| dataType  | `string` | Sensordatentyp |
| value  | `string` | value |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue(spaceId, dataType, value)

Legt den Wert des Raumobjekts mit dem angegebenen Datentyp fest.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Raumbezeichner |
| dataType  | `string` | Datentyp |
| value  | `string` | value |

### <a name="logmessage"></a>log(message)

Protokolliert die folgende Meldung in der benutzerdefinierten Funktion.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| Message:  | `string` | Die zu protokollierende Meldung |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification(topologyObjectId, topologyObjectType, payload)

Sendet eine benutzerdefinierte Benachrichtigung, die erledigt werden soll.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| topologyObjectId  | `Guid` | Bezeichner des Graphobjekts (Beispiel: Raum-/Sensor-/Geräte-ID)|
| topologyObjectType  | `string` | (Beispiel: Raum/Sensor/Gerät)|
| payload  | `string` | Die JSON-Nutzlast, die mit die Benachrichtigung gesendet werden soll |

## <a name="return-types"></a>Rückgabetypen

Die folgenden Angaben sind Modelle, die die Rückgabeobjekte aus der obigen Clientreferenz beschreiben.

### <a name="space"></a>Leerzeichen

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000000",
  "Name": "Space",
  "FriendlyName": "Conference Room",
  "TypeId": 0,
  "ParentSpaceId": "00000000-0000-0000-0000-000000000001",
  "SubtypeId": 0
}
```

### <a name="space-methods"></a>Raummethoden

#### <a name="parent--space"></a>Parent() ⇒ `space`

Gibt den übergeordneten Raum des aktuellen Raums zurück.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Gibt die untergeordneten Sensoren des aktuellen Raums zurück.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Gibt die untergeordneten Geräte des aktuellen Raums zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Gibt die erweiterte Eigenschaft und deren Wert für den aktuellen Raum zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| propertyName | `string` | Der Name der erweiterten Eigenschaft |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Gibt den Wert des aktuellen Raums zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| valueName | `string` | Der Name des Werts |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Gibt die früheren Werte des aktuellen Raums zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| valueName | `string` | Der Name des Werts |

#### <a name="notifypayload"></a>Notify(payload)

Sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| payload | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

### <a name="device"></a>Gerät

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000002",
  "Name": "Device",
  "FriendlyName": "Temperature Sensing Device",
  "Description": "This device contains a sensor that captures temperature readings.",
  "Type": "None",
  "Subtype": "None",
  "TypeId": 0,
  "SubtypeId": 0,
  "HardwareId": "ABC123",
  "GatewayId": "ABC",
  "SpaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Gerätemethoden

#### <a name="parent--space"></a>Parent() ⇒ `space`

Gibt den übergeordneten Raum des aktuellen Geräts zurück.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Gibt die untergeordneten Sensoren des aktuellen Geräts zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Gibt die erweiterte Eigenschaft und deren Wert für das aktuelle Gerät zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| propertyName | `string` | Der Name der erweiterten Eigenschaft |

#### <a name="notifypayload"></a>Notify(payload)

Sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| payload | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

### <a name="sensor"></a>Sensor

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000003",
  "Port": "30",
  "PollRate": 3600,
  "DataType": "Temperature",
  "DataSubtype": "None",
  "Type": "Classic",
  "PortType": "None",
  "DataUnitType": "FahrenheitTemperature",
  "SpaceId": "00000000-0000-0000-0000-000000000000",
  "DeviceId": "00000000-0000-0000-0000-000000000001",
  "PortTypeId": 0,
  "DataUnitTypeId": 0,
  "DataTypeId": 0,
  "DataSubtypeId": 0,
  "TypeId": 0  
}
```

### <a name="sensor-methods"></a>Sensormethoden

#### <a name="space--space"></a>Space() ⇒ `space`

Gibt den übergeordneten Raum des aktuellen Sensors zurück.

#### <a name="device--device"></a>Device() ⇒ `device`

Gibt das übergeordnete Gerät des aktuellen Sensors zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Gibt die erweiterte Eigenschaft und deren Wert für den aktuellen Sensor zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| propertyName | `string` | Der Name der erweiterten Eigenschaft |

#### <a name="value--value"></a>Value() ⇒ `value`

Gibt den Wert des aktuellen Sensors zurück.

#### <a name="history--value"></a>History() ⇒ `value[]`

Gibt die früheren Werte des aktuellen Sensors zurück.

#### <a name="notifypayload"></a>Notify(payload)

Sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| payload | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

### <a name="value"></a>Wert

```JSON
{
  "DataType": "Temperature",
  "Value": "70",
  "CreatedTime": ""
}
```

### <a name="extended-property"></a>Extended-Eigenschaft

```JSON
{
  "Name": "OccupancyStatus",
  "Value": "Occupied"
}
```

## <a name="next-steps"></a>Nächste Schritte

Informationen, wie Digital Twins-Endpunkte erstellt werden, an die Ereignisse gesendet werden sollen, finden Sie unter [Erstellen von Digital Twins-Endpunkten](how-to-egress-endpoints.md).

Weitere ausführliche Informationen zu Digital Twins-Endpunkten finden Sie unter [Weiterleiten von Ereignissen und Nachrichten](concepts-events-routing.md).
