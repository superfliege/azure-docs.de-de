---
title: Exportieren von Daten in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Daten aus Ihrer Azure IoT Central-Anwendung exportiert werden.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/3/2018
ms.topic: article
ms.prod: azure-iot-central
manager: peterpr
ms.openlocfilehash: 5b9564dfe40f292d289ee9ed680e816771d0b0ed
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282873"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportieren von Daten in Azure IoT Central

Mit dem fortlaufenden Datenexport können Sie in regelmäßigen Abständen Daten in Ihr Azure Blob Storage-Konto exportieren. Exportieren Sie **Messungen**, **Geräte** und **Gerätevorlagen** in Dateien mit dem Format [Apache AVRO](https://avro.apache.org/docs/current/index.html). Verwenden Sie die exportierten Daten für die Analyse von kalten Datenpfaden wie Trainingsmodelle in Azure Machine Learning oder die langfristige Trendanalyse in Power BI.

> [!Note]
> Wenn Sie den fortlaufenden Datenexport aktivieren, erhalten Sie nur die Daten, die von diesem Moment an eingehen. Derzeit besteht keine Möglichkeit, Daten von dem Zeitpunkt an abzurufen, an dem der fortlaufende Datenexport deaktiviert wurde. Aktivieren Sie frühzeitig den fortlaufenden Datenexport, um umfassendere Verlaufsdaten zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen

- Eine App mit einer erweiterten 30-Tage-Testlizenz oder eine kostenpflichtige App
- Azure-Konto mit Azure-Abonnement
- Das gleiche Azure-Konto ist ein Administratorkonto in Ihrer IoT Central-App
- Dasselbe Azure-Konto weist Berechtigungen zum Erstellen eines Speicherkontos oder Zugreifen auf ein vorhandenes Speicherkonto im selben Azure-Abonnement auf.

## <a name="types-of-data-to-export"></a>Typen der zu exportierenden Daten

### <a name="measurements"></a>Messungen

Die Messdaten, die Geräte senden, werden in Ihr Speicherkonto exportiert. Messdaten werden etwa einmal pro Minute exportiert und enthalten alle neuen Meldungen, die von IoT Central von allen Geräten innerhalb dieses Zeitfensters empfangen wurden. Die exportierten AVRO-Dateien weisen dasselbe Format auf wie die Meldungsdateien, die bei der [IoT Hub-Nachrichtenweiterleitung](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) in Blob Storage exportiert wurden.

> [!NOTE]
> Die Geräte, die die Messdaten gesendet haben, werden durch Geräte-IDs (siehe unten) dargestellt. Um die Namen der Geräte zu erhalten, müssen Sie auch Momentaufnahmen der Geräte exportieren. Sie können jeden Meldungsdatensatz mit der connectionDeviceId korrelieren, die der ID des Geräts entspricht.

Dies ist ein Beispiel für einen Datensatz in der decodierten AVRO-Datei.

```json
{
    "EnqueuedTimeUtc": "2018-06-11T00:00:08.2250000Z",
    "Properties": {},
    "SystemProperties": {
        "connectionDeviceId": "2383d8ba-c98c-403a-b4d5-8963859643bb",
        "connectionAuthMethod": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
        "connectionDeviceGenerationId": "636614021491644195",
        "enqueuedTime": "2018-06-11T00:00:08.2250000Z"
    },
    "Body": "{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Geräte

Wenn Sie zum ersten Mal den fortlaufenden Datenexport aktivieren, wird eine einzelne Momentaufnahme exportiert, die alle Geräte enthält. Dies umfasst:
- Geräte-IDs
- Gerätenamen
- Gerätevorlagen-IDs
- Eigenschaftenwerte
- Einstellungswerte

Ungefähr einmal pro Minute wird eine neue Momentaufnahme mit folgenden Informationen geschrieben:

- Die neuen Geräte, die seit der letzten Momentaufnahme hinzugefügt wurden
- Geräte, bei denen sich die Eigenschaften- und Einstellungswerte seit der letzten Momentaufnahme geändert haben

> [!NOTE]
> Geräte, die gelöscht wurden, seitdem die letzte Momentaufnahme nicht exportiert wurde Es gibt keinen Indikator in den Momentaufnahmen für Geräte, die zu diesem Zeitpunkt gelöscht wurden.

> [!NOTE]
> Die Gerätevorlage, der die einzelnen Geräte zugewiesen sind, wird durch eine Gerätevorlagen-ID dargestellt. Um den Namen der Gerätevorlage abzurufen, müssen Sie auch Momentaufnahmen von Gerätevorlagen exportieren.

Jeder Datensatz in der decodierten AVRO-Datei sieht folgendermaßen aus:

```json
{
    "id": "2383d8ba-c98c-403a-b4d5-8963859643bb",
    "name": "Refrigerator 2",
    "simulated": true,
    "deviceTemplate": {
        "id": "c318d580-39fc-4aca-b995-843719821049",
        "version": "1.0.0"
    },
    "properties": {
        "cloud": {
            "location": "New York",
            "maintCon": true,
            "tempThresh": 20
        },
        "device": {
            "lastReboot": "2018-02-09T22:22:47.156Z"
        }
    },
    "settings": {
        "device": {
            "fanSpeed": 0
        }
    }
}
```

### <a name="device-templates"></a>Gerätevorlagen

Wenn Sie zum ersten Mal den fortlaufenden Datenexport aktivieren, wird eine einzelne Momentaufnahme exportiert, die alle Gerätevorlagen enthält. Dies umfasst: 
- Gerätevorlagen-IDs
- Typen der Messdaten und Mindest- bzw. Maximalwerte
- Typen der Eigenschaftsdaten und Standardwerte
- Typen der Einstellungsdaten und Standardwerte

Ungefähr einmal pro Minute wird eine neue Momentaufnahme mit folgenden Informationen geschrieben:

- Die neuen Gerätevorlagen, die seit der letzten Momentaufnahme hinzugefügt wurden
- Gerätevorlagen, die Messwert-, Eigenschafts- und Einstellungsdefinitionen enthielten, die sich seit der letzten Momentaufnahme geändert haben

> [!NOTE]
> Gerätevorlagen, die gelöscht wurden, seitdem die letzte Momentaufnahme nicht exportiert wurde Es gibt keinen Indikator in den Momentaufnahmen für Gerätevorlagen, die zu diesem Zeitpunkt gelöscht wurden.

Jeder Datensatz in der decodierten AVRO-Datei sieht folgendermaßen aus:

```json
{
    "id": "c318d580-39fc-4aca-b995-843719821049",
    "name": "Refrigerated Vending Machine",
    "version": "1.0.0",
    "measurements": {
        "telemetry": {
            "humidity": {
                "dataType": "double",
                "name": "Humidity"
            },
            "magnetometerX": {
                "dataType": "double",
                "name": "Magnetometer X"
            },
            "magnetometerY": {
                "dataType": "double",
                "name": "Magnetometer Y"
            },
            "magnetometerZ": {
                "dataType": "double",
                "name": "Magnetometer Z"
            }
        },
        "states": {
            "connectivity": {
                "dataType": "enum",
                "name": "Connectivity"
            }
        },
        "events": {
            "opened": {
                "name": "Door Opened",
                "category": "informational"
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
    },
    "properties": {
        "cloud": {
            "location": {
                "dataType": "string",
                "name": "Location",
                "initialValue": "Seattle"
            },
            "maintCon": {
                "dataType": "boolean",
                "name": "Maintenance Contract",
                "initialValue": true
            },
            "tempThresh": {
                "dataType": "double",
                "name": "Temperature Alert Threshold",
                "initialValue": 30
            }
        },
        "device": {
            "lastReboot": {
                "dataType": "dateTime",
                "name": "Last Reboot"
            }
        }
    }
}
```

## <a name="how-to-set-up-data-export"></a>Gewusst wie: Einrichten des Datenexports

1. Wenn Sie noch kein Azure Storage-Konto besitzen, erstellen Sie eins **im Azure-Abonnement, unter dem Ihre App geführt wird**. [Klicken Sie hier](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM), um für die Erstellung eines neuen Azure Storage-Kontos zum Azure-Portal zu wechseln.

    - Wählen Sie den Kontotyp *Universell* oder *Blob Storage* aus.
    - Wählen Sie das Abonnement aus, in dem Ihre IoT Central-App enthalten ist. Wenn das Abonnement nicht angezeigt wird, müssen Sie sich möglicherweise bei einem anderen Azure-Konto anmelden oder Zugriff auf das Abonnement anfordern.
    - Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. Weitere Informationen finden Sie im Artikel zum [Erstellen eines neuen Speicherkontos](https://aka.ms/blobdocscreatestorageaccount).

2. Erstellen Sie einen Container in Ihrem Speicherkonto, um Ihre IoT Central-Daten zu exportieren. Navigieren Sie zu Ihrem Speicherkonto, klicken Sie auf „Blobs durchsuchen“, und erstellen Sie einen neuen Container. ![Erstellen eines Containerimage](media/howto-export-data/createcontainer.png)

3. Melden Sie sich mit dem gleichen Azure-Konto bei Ihrer IoT Central-Anwendung an.
1. Navigieren Sie zu „Verwaltung“ > „Fortlaufender Datenexport“.
![IoT Central CDE](media/howto-export-data/continuousdataexport.PNG)
1. Wählen Sie aus den Dropdownlisten Ihr Speicherkonto und Ihren Container aus. Dann können Sie mit den Optionen zum Umschalten die verschiedenen Arten der zu exportierenden Daten aktivieren oder deaktivieren.
1. Aktivieren Sie schließlich den fortlaufenden Datenexport mit der Option zum Umschalten, und klicken Sie auf „Speichern“.
1. Ihre Daten sollten nach einigen Minuten in Ihrem Speicherkonto angezeigt werden. Wenn Sie in Ihrem Speicherkonto auf „Blobs durchsuchen“ klicken und Ihren Container auswählen, werden drei Ordner angezeigt. Zu den Standardpfaden für die AVRO-Dateien mit den verschiedenen Datentypen zählen Folgende:
- Messungen: **{container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Geräte: **{container}/devices/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Gerätevorlagen: **{container}/deviceTemplates/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**

## <a name="how-to-read-exported-avro-files"></a>Gewusst wie: Lesen von exportierten AVRO-Dateien

AVRO ist ein Binärformat, mit dem die Dateien im unformatierten Zustand gelesen werden können. Diese können im JSON-Format decodiert werden. Die folgenden Beispiele zeigen, wie die AVRO-Dateien von Messungen, Geräten und Gerätevorlagen, die die obigen Beispiele enthalten, analysiert werden.

## <a name="python"></a>Python

### <a name="install-pandas-and-the-pandaavro-package"></a>Installieren des Pandas- und PandaAvro-Pakets:

```python
pip install pandas
pip install pandavro
```
### <a name="parse-measurements-avro-file"></a>Analysieren der AVRO-Datei von Messungen

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    measurements = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary with the device id
    # located under the "connectionDeviceId" key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of utf-8 bytes that is stringified and parsed
    # as json. In this example, we pull the "humidity" property off of each one
    # to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, we print the new DataFrame with our device ids and humidities
    print(transformed)

```
### <a name="parse-devices-avro-file"></a>Analysieren der AVRO-Datei von Geräten

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    devices = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device id is available directly in the "id" column
    transformed["device_id"] = devices["id"]

    # The template id and version are present in a dictionary under the
    # deviceTemplate column
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)

```

### <a name="parse-device-templates-avro-file"></a>Analysieren der AVRO-Datei von Gerätevorlagen

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    templates = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available directly in the "id" and "version"
    # columns, respectively
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)
```

## <a name="c"></a>C#

### <a name="install-microsofthadoopavro"></a>Installieren von Microsoft.Hadoop.Avro

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

### <a name="parse-measurements-avro-file"></a>Analysieren der AVRO-Datei von Messungen

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;
using Newtonsoft.Json;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contain multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the fields
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    var systemProperties = record.GetField<IDictionary<string, object>>("SystemProperties");
                    var deviceId = systemProperties["connectionDeviceId"] as string;
                    Console.WriteLine("Device ID: {0}", deviceId);

                    using (var stream = new MemoryStream(record.GetField<byte[]>("Body")))
                    {
                        using (var streamReader = new StreamReader(stream, Encoding.UTF8))
                        {
                            var body = JsonSerializer.Create().Deserialize(streamReader, typeof(IDictionary<string, dynamic>)) as IDictionary<string, dynamic>;
                            var humidity = body["humidity"];
                            Console.WriteLine("Humidity: {0}", humidity);
                        }
                    }
                }
            }
        }
    }
}
```

### <a name="parse-devices-avro-file"></a>Analysieren der AVRO-Datei von Geräten

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var deviceId = record.GetField<string>("id");

                    // The device template information is stored in a sub-record
                    // under the "deviceTemplate" field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {0}, Template ID: {1}, Template Version: {2}, Fan Speed: {3}",
                        deviceId,
                        templateId,
                        templateVersion,
                        fanSpeed
                    );
                }
            }
        }
    }
}

```
### <a name="parse-device-templates-avro-file"></a>Analysieren der AVRO-Datei von Gerätevorlagen

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {1}, Version: {2}, Fan Speed: {3}",
                        id,
                        version,
                        fanSpeed
                    );
                }
            }
        }
    }
}
```

## <a name="javascript"></a>JavaScript

### <a name="install-avsc"></a>Installieren von AVSC

```javascript
npm install avsc
```

### <a name="parse-measurements-avro-file"></a>Analysieren der AVRO-Datei von Messungen

```javascript
const avro = require('avsc');

// Read the avro file and parse the device id and humidity from each record
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the system properties
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the Body from a Buffer to a string and parse it
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property off of the body
        const humidity = body.humidity;

        // Log the device id and humidity we found
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-devices-avro-file"></a>Analysieren der AVRO-Datei von Geräten

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the id property
        const deviceId = record.id;

        // Fetch the tempalte id and version from the deviceTemplate property
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`ID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-device-templates-avro-file"></a>Analysieren der AVRO-Datei von Gerätevorlagen

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template id and version from the id and verison properties
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit dem Exportieren Ihrer Daten vertraut gemacht haben, schlagen wir Ihnen Folgendes als nächsten Schritt vor:

> [!div class="nextstepaction"]
> [Visualisieren Ihrer Daten in Power BI](howto-connect-powerbi.md)