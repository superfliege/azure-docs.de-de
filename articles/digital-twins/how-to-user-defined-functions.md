---
title: Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins | Microsoft-Dokumentation
description: Anleitung zum Erstellen benutzerdefinierter Funktionen, Matcher (Abgleicher) und Rollenzuweisungen mit Azure Digital Twins.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: alinast
ms.openlocfilehash: 6a757dca48dc3ff41adfe6f8802fad40e7a4ca81
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636831"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins

[Benutzerdefinierte Funktionen](./concepts-user-defined-functions.md) ermöglichen es dem Benutzer, benutzerdefinierte Logik für eingehende Telemetrienachrichten und Raumgraphmetadaten auszuführen, sodass der Benutzer Ereignisse an vordefinierte Endpunkte senden kann. In diesem Leitfaden durchlaufen Sie ein Beispiel zur Verarbeitung von Temperaturereignissen, um jede Messung zu erkennen, bei der eine bestimmte Temperatur überschritten wurde, und eine Warnung für eine solche Messung zu senden.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>Referenz zur Clientbibliothek

Die Funktionen, die als Hilfsmethoden in der Runtime für benutzerdefinierte Funktionen verfügbar sind, werden im Abschnitt [Clientreferenz](#Client-Reference) aufgeführt.

## <a name="create-a-matcher"></a>Erstellen eine Matchers

Matcher (Abgleicher) sind Graphobjekte, die bestimmen, welche benutzerdefinierten Funktionen für eine bestimmte Telemetrienachricht ausgeführt werden.

- Gültige Matcherbedingungsvergleiche:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Gültige Matcherbedingungsziele:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

Der folgende Beispielmatcher besitzt den Ergebniswert TRUE für jedes Sensortelemetrieereignis, das den Datentyp `"Temperature"` aufweist. Sie können mehrere Matcher für eine benutzerdefinierte Funktion erstellen:

```plaintext
POST YOUR_MANAGEMENT_API_URL/matchers
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
  "SpaceId": "YOUR_SPACE_IDENTIFIER"
}
```

| Wert | Ersetzen durch |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Die Serverregion, in der Ihre Instanz gehostet wird |

## <a name="create-a-user-defined-function-udf"></a>Erstellen einer benutzerdefinierten Funktion (UDF)

Nachdem die Matcher erstellt wurden, laden Sie den Funktionsausschnitt mit dem folgenden **POST**-Aufruf hoch:

> [!IMPORTANT]
> - Legen Sie in den Headern Folgendes fest: `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - Der Hauptteil ist mehrteilig:
>   - Im ersten Teil geht es um die Metadaten, die für die benutzerdefinierte Funktion erforderlich sind.
>   - Der zweite Teil enthält die JavaScript-Computelogik.
> - Ersetzen Sie im Abschnitt **USER_DEFINED_BOUNDARY** die Werte **SpaceId** und **Matchers**.

```plaintext
POST YOUR_MANAGEMENT_API_URL/userdefinedfunctions with Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"
```

| Parameterwert | Ersetzen durch |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | Name für die Begrenzung mehrteiliger Inhalte |

### <a name="body"></a>Body

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "YOUR_SPACE_IDENTIFIER",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Wert | Ersetzen durch |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Den Raumbezeichner (Raum-ID)  |
| YOUR_MATCHER_IDENTIFIER | Die ID des Matchers, den Sie verwenden möchten |

### <a name="example-functions"></a>Beispielfunktionen

Legen Sie den Sensortelemetriemesswert direkt für den Sensor mit dem Datentyp **Temperature** fest, der `sensor.DataType` entspricht:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

Der **telemetry**-Parameter macht die Attribute **SensorId** und **Message** entsprechend einer von einem Sensor gesendeten Nachricht verfügbar. Der **executionContext**-Parameter macht die folgenden Attribute verfügbar:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

Im nächsten Beispiel protokollieren wir eine Nachricht, wenn der Sensortelemetriemesswert einen vordefinierten Schwellenwert überschreitet. Wenn Ihre Diagnoseeinstellungen in der Azure Digital Twins-Instanz aktiviert sind, werden Protokolle von benutzerdefinierten Funktionen ebenfalls weitergeleitet:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

Im folgenden Code wird eine Benachrichtigung ausgelöst, wenn die Temperatur den festgelegten Wert der Konstanten überschreitet:

```JavaScript
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

Ein komplexeres Beispiel für eine benutzerdefinierte Funktion finden Sie in der [benutzerdefinierten Funktion zum Prüfen auf verfügbare Räume mit frischer Luft](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Erstellen einer Rollenzuweisung

Es muss eine Rollenzuweisung erstellt werden, unter der die benutzerdefinierte Funktion ausgeführt werden kann. Wird keine solche Zuweisung erstellt, hat die Funktion nicht die richtigen Berechtigungen zur Verwendung der Verwaltungs-API, um Aktionen für Graphobjekte auszuführen. Die Aktionen, die in der benutzerdefinierten Funktion ausgeführt werden, sind nicht von der rollenbasierten Zugriffskontrolle in den Azure Digital Twins-Verwaltungs-APIs ausgenommen. Sie können hinsichtlich des Geltungsbereichs eingeschränkt werden, indem bestimmte Rollen oder bestimmte Zugriffssteuerungspfade angegeben werden. Weitere Informationen finden Sie in der Dokumentation zur [rollenbasierten Zugriffssteuerung](./security-role-based-access-control.md).

1. Fragen Sie die Rollen ab, und rufen Sie die ID der Rolle ab, die Sie der benutzerdefinierten Funktion zuweisen möchten. Übergeben Sie die ID an **RoleId**:

    ```plaintext
    GET YOUR_MANAGEMENT_API_URL/system/roles
    ```

1. **ObjectId** ist die zuvor erstellte ID der benutzerdefinierten Funktion.
1. Suchen Sie den Wert von **Path** durch Abfragen Ihrer Räume mit `fullpath`.
1. Kopieren Sie den zurückgegebenen `spacePaths`-Wert. Sie verwenden diesen Wert im folgenden Code:

    ```plaintext
    GET YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Parameterwert | Ersetzen durch |
    | --- | --- |
    | *YOUR_SPACE_NAME* | Der Name des Raums, den Sie verwenden möchten |

1. Fügen Sie den zurückgegebenen `spacePaths`-Wert in **Path** ein, um eine UDF-Rollenzuweisung zu erstellen:

    ```plaintext
    POST YOUR_MANAGEMENT_API_URL/roleassignments
    {
      "RoleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "ObjectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "ObjectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "Path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Ihr Wert | Ersetzen durch |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | Der Bezeichner für die gewünschte Rolle |
    | YOUR_USER_DEFINED_FUNCTION_ID | Die ID für die benutzerdefinierte Funktion, die Sie verwenden möchten |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | Die ID zur Angabe des UDF-Typs |
    | YOUR_ACCESS_CONTROL_PATH | Der Zugriffssteuerungspfad |

## <a name="send-telemetry-to-be-processed"></a>Senden der Telemetriedaten, die verarbeitet werden sollen

Die Telemetriedaten, die von dem im Graphen beschriebenen Sensor generiert werden, lösen die Ausführung der hochgeladenen benutzerdefinierten Funktion aus. Die Datenverarbeitung übernimmt die Telemetriedaten. Anschließend wird ein Ausführungsplan für den Aufruf der benutzerdefinierten Funktion erstellt.

1. Rufen Sie die Matcher für den Sensor ab, von dem der Messwert generiert wurde.
1. Je nachdem, welche Matcher erfolgreich ausgewertet wurden, rufen Sie die zugehörigen benutzerdefinierten Funktionen ab.
1. Führen Sie die einzelnen benutzerdefinierten Funktionen aus.

## <a name="client-reference"></a>Clientreferenz

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Bei angegebenem Raumbezeichner ruft diese Funktion den Raum aus dem Graphen ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Raumbezeichner |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Bei angegebenem Sensorbezeichner ruft diese Funktion den Sensor aus dem Graphen ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Sensorbezeichner |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Bei angegebenem Gerätebezeichner ruft diese Funktion das Gerät aus dem Graphen ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Gerätebezeichner |

### <a name="getsensorvaluesensorid-datatype--value"></a>getSensorValue(sensorId, dataType) ⇒ `value`

Nach Angabe eines Sensorbezeichners und dessen Datentyps ruft diese Funktion den aktuellen Wert dieses Sensors ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | Sensorbezeichner |
| *dataType*  | `string` | Sensordatentyp |

### <a name="getspacevaluespaceid-valuename--value"></a>getSpaceValue(spaceId, valueName) ⇒ `value`

Nach Angabe eines Raumbezeichners und des Wertnamens ruft diese Funktion den aktuellen Wert für diese Raumeigenschaft ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | Raumbezeichner |
| *valueName* | `string` | Name der Raumeigenschaft |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>getSensorHistoryValues(sensorId, dataType) ⇒ `value[]`

Nach Angabe eines Sensorbezeichners und dessen Datentyps ruft diese Funktion die früheren Werte dieses Sensors ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensorbezeichner |
| *dataType* | `string` | Sensordatentyp |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>getSpaceHistoryValues(spaceId, dataType) ⇒ `value[]`

Nach Angabe eines Raumbezeichners und des Wertnamens ruft diese Funktion die früheren Werte dieser Raumeigenschaft ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |
| *valueName* | `string` | Name der Raumeigenschaft |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Nach Angabe eines Raumbezeichners ruft diese Funktion die untergeordneten Räume dieses übergeordneten Raums ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Nach Angabe eines Raumbezeichners ruft diese Funktion die untergeordneten Sensoren dieses übergeordneten Raums ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Nach Angabe eines Raumbezeichners ruft diese Funktion die untergeordneten Geräte dieses übergeordneten Raums ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Nach Angabe eines Gerätebezeichners ruft diese Funktion die untergeordneten Sensoren dieses übergeordneten Geräts ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Gerätebezeichner |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Nach Angabe eines Raumbezeichners ruft diese Funktion den übergeordneten Raum ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | Raumbezeichner |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Nach Angabe eines Sensorbezeichners ruft diese Funktion den übergeordneten Raum ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Sensorbezeichner |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Nach Angabe eines Gerätebezeichners ruft diese Funktion den übergeordneten Raum ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Gerätebezeichner |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Nach Angabe eines Sensorbezeichners ruft diese Funktion das übergeordnete Gerät ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Sensorbezeichner |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>getSpaceExtendedProperty(spaceId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Raumbezeichners ruft diese Funktion die Eigenschaft und deren Wert aus dem Raum ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |
| *propertyName* | `string` | Name der Raumeigenschaft |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>getSensorExtendedProperty(sensorId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Sensorbezeichners ruft diese Funktion die Eigenschaft und deren Wert vom Sensor ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensorbezeichner |
| *propertyName* | `string` | Name der Sensoreigenschaft |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>getDeviceExtendedProperty(deviceId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Gerätebezeichners ruft diese Funktion die Eigenschaft und deren Wert vom Gerät ab.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Gerätebezeichner |
| *propertyName* | `string` | Name der Geräteeigenschaft |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue(sensorId, dataType, value)

Diese Funktion legt den Wert des Sensorobjekts mit dem angegebenen Datentyp fest.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensorbezeichner |
| *dataType*  | `string` | Sensordatentyp |
| *value*  | `string` | Wert |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue(spaceId, dataType, value)

Diese Funktion legt den Wert des Raumobjekts mit dem angegebenen Datentyp fest.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |
| *dataType* | `string` | Datentyp |
| *value* | `string` | Wert |

### <a name="logmessage"></a>log(message)

Diese Funktion protokolliert die folgende Meldung in der benutzerdefinierten Funktion.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *message* | `string` | Die zu protokollierende Meldung |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification(topologyObjectId, topologyObjectType, payload)

Diese Funktion sendet eine benutzerdefinierte Benachrichtigung, die erledigt werden soll.

**Art**: globale Funktion

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Bezeichner des Graphobjekts. Beispiele: Raum-, Sensor- und Geräte-ID.|
| *topologyObjectType*  | `string` | Beispiele: Sensor und Gerät.|
| *payload*  | `string` | Die JSON-Nutzlast, die mit der Benachrichtigung gesendet werden soll. |

## <a name="return-types"></a>Rückgabetypen

Die folgenden Modelle beschreiben die Rückgabeobjekte aus der obigen Clientreferenz.

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

Diese Funktion gibt den übergeordneten Raum des aktuellen Raums zurück.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Diese Funktion gibt die untergeordneten Sensoren des aktuellen Raums zurück.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Diese Funktion gibt die untergeordneten Geräte des aktuellen Raums zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Diese Funktion gibt die erweiterte Eigenschaft und deren Wert für den aktuellen Raum zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Name der erweiterten Eigenschaft |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Diese Funktion gibt den Wert des aktuellen Raums zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Name des Werts |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Diese Funktion gibt die früheren Werte des aktuellen Raums zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Name des Werts |

#### <a name="notifypayload"></a>Notify(payload)

Diese Funktion sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

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

Diese Funktion gibt den übergeordneten Raum des aktuellen Geräts zurück.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Diese Funktion gibt die untergeordneten Sensoren des aktuellen Geräts zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Diese Funktion gibt die erweiterte Eigenschaft und deren Wert für das aktuelle Gerät zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Name der erweiterten Eigenschaft |

#### <a name="notifypayload"></a>Notify(payload)

Diese Funktion sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

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

Diese Funktion gibt den übergeordneten Raum des aktuellen Sensors zurück.

#### <a name="device--device"></a>Device() ⇒ `device`

Diese Funktion gibt das übergeordnete Gerät des aktuellen Sensors zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Diese Funktion gibt die erweiterte Eigenschaft und deren Wert für den aktuellen Sensor zurück.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Name der erweiterten Eigenschaft |

#### <a name="value--value"></a>Value() ⇒ `value`

Diese Funktion gibt den Wert des aktuellen Sensors zurück.

#### <a name="history--value"></a>History() ⇒ `value[]`

Diese Funktion gibt die früheren Werte des aktuellen Sensors zurück.

#### <a name="notifypayload"></a>Notify(payload)

Diese Funktion sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | Typ                | BESCHREIBUNG  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

### <a name="value"></a>Wert

```JSON
{
  "DataType": "Temperature",
  "Value": "70",
  "CreatedTime": ""
}
```

### <a name="extended-property"></a>Erweiterte Eigenschaft

```JSON
{
  "Name": "OccupancyStatus",
  "Value": "Occupied"
}
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Erstellen von Azure Digital Twins-Endpunkten](how-to-egress-endpoints.md), an die Ereignisse gesendet werden sollen.

- Weitere ausführliche Informationen zu Digital Twins-Endpunkten finden Sie in den [weiteren Informationen zu Endpunkten](concepts-events-routing.md).
