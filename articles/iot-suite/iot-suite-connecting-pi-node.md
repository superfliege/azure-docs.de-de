---
title: "Bereitstellen von Raspberry Pi für die Remoteüberwachung in Node.js – Azure | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe einer in Node.js geschriebenen Anwendung eine Verbindung zwischen einem Raspberry Pi-Gerät und der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite herstellen."
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 5242d6d7abba1dc06f8e01b26a2d3bfdecb1d630
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Herstellen einer Verbindung zwischen Ihrem Raspberry Pi-Gerät und der vorkonfigurierten Remoteüberwachungslösung (Node.js)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Dieses Tutorial zeigt, wie Sie eine Verbindung zwischen einem physischen Gerät und der vorkonfigurierten Remoteüberwachungslösung herstellen. In diesem Tutorial verwenden Sie Node.js. Dies ist eine gute Wahl für Umgebungen mit minimalen Ressourceneinschränkungen.

### <a name="required-hardware"></a>Erforderliche Hardware

Ein Desktopcomputer, über den Sie eine Remoteverbindung mit der Befehlszeile auf dem Raspberry Pi herstellen können.

[Microsoft IoT Starter Kit für Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) oder entsprechende Komponenten. In diesem Tutorial wird Folgendes aus dem Kit verwendet:

- Raspberry Pi 3
- MicroSD-Karte (mit NOOBS)
- Ein Mini-USB-Kabel
- Ein Ethernetkabel

### <a name="required-desktop-software"></a>Erforderliche Desktopsoftware

Sie benötigen einen SSH-Client auf Ihrem Desktopcomputer, um per Remotezugriff auf die Befehlszeile des Raspberry Pi zugreifen zu können.

- In Windows ist kein SSH-Client enthalten. Wir empfehlen die Verwendung von [PuTTY](http://www.putty.org/).
- In den meisten Linux-Distributionen sowie in Mac OS ist ein SSH-Befehlszeilenprogramm enthalten. Weitere Informationen finden Sie unter [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md) (SSH unter Linux oder Mac OS).

### <a name="required-raspberry-pi-software"></a>Erforderliche Raspberry Pi-Software

Auf dem Raspberry Pi muss mindestens Node.js 4.0.0 installiert sein. Die folgenden Schritte zeigen, wie Sie Node.js 6.11.4 auf Ihrem Raspberry Pi installieren:

1. Stellen Sie mithilfe von `ssh` eine Verbindung mit Ihrem Raspberry Pi her. Weitere Informationen finden Sie auf der [Raspberry Pi-Website](https://www.raspberrypi.org/) unter [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md).

1. Verwenden Sie den folgenden Befehl aus, um Ihr Raspberry Pi zu aktualisieren:

    ```sh
    sudo apt-get update
    ```

1. Verwenden Sie den folgenden Befehl zum Herunterladen der Node.js-Binärdateien auf Ihr Raspberry Pi:

    ```sh
    wget https://nodejs.org/dist/v6.11.4/node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Verwenden Sie den folgenden Befehl, um die Binärdateien zu installieren:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob Sie Node.js 6.11.4 erfolgreich installiert haben:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Erstellen einer Node.js-Lösung

Verwenden Sie für die folgenden Schritte die `ssh`-Verbindung mit Ihrem Raspberry Pi:

1. Erstellen Sie im Stammordner des Raspberry Pi einen Ordner namens `RemoteMonitoring`. Navigieren Sie über die Befehlszeile zu diesem Ordner:

    ```sh
    cd ~
    mkdir RemoteMonitoring
    cd RemoteMonitoring
    ```

1. Führen Sie die folgenden Befehle aus, um die Pakete herunterzuladen und zu installieren, die Sie zum Fertigstellen der Beispiel-App benötigen:

    ```sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Erstellen Sie im Ordner `RemoteMonitoring` eine Datei mit dem Namen **remote_monitoring.js**. Öffnen Sie diese Datei in einem Texteditor. Auf dem Raspberry Pi können Sie den Text-Editor `nano` oder `vi` verwenden.

1. Fügen Sie der Datei **remote_monitoring.js** die folgenden `require`-Anweisungen hinzu:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Fügen Sie die folgenden Variablendeklarationen nach den `require` -Anweisungen hinzu. Ersetzen Sie die Platzhalterwerte `{Device Id}` und `{Device Key}` durch die Werte, die Sie für das Gerät, das Sie in der Remoteüberwachungslösung bereitgestellt haben, notiert haben. Ersetzen Sie `{IoTHub Name}` durch den IoT Hub-Hostnamen aus der Lösung. Wenn der IoT Hub-Hostname z.B. `contoso.azure-devices.net` lautet, ersetzen Sie `{IoTHub Name}` durch `contoso`:

    ```nodejs
    var connectionString = 'HostName={IoTHub Name}.azure-devices.net;DeviceId={Device Id};SharedAccessKey={Device Key}';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Fügen Sie die folgenden Variablen hinzu, um einige grundlegende Telemetriedaten zu definieren:

    ```nodejs
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Fügen Sie die folgenden Variablen hinzu, um einige Eigenschaftswerte zu definieren:

    ```nodejs
    var temperatureSchema = 'chiller-temperature;v1';
    var humiditySchema = 'chiller-humidity;v1';
    var pressureSchema = 'chiller-pressure;v1';
    var interval = "00:00:05";
    var deviceType = "Chiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    ```

1. Fügen Sie die folgende Variable hinzu, um die gemeldeten Eigenschaften zu definieren, die an die Lösung gesendet werden. Zu diesen Eigenschaften zählen Metadaten, die die Methoden und die Telemetrie beschreiben, die das Gerät verwendet:

    ```nodejs
    var reportedProperties = {
      "Protocol": "MQTT",
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        "TemperatureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
          "MessageSchema": {
            "Name": temperatureSchema,
            "Format": "JSON",
            "Fields": {
              "temperature": "Double",
              "temperature_unit": "Text"
            }
          }
        },
        "HumiditySchema": {
          "Interval": interval,
          "MessageTemplate": "{\"humidity\":${humidity},\"humidity_unit\":\"${humidity_unit}\"}",
          "MessageSchema": {
            "Name": humiditySchema,
            "Format": "JSON",
            "Fields": {
              "humidity": "Double",
              "humidity_unit": "Text"
            }
          }
        },
        "PressureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"pressure\":${pressure},\"pressure_unit\":\"${pressure_unit}\"}",
          "MessageSchema": {
            "Name": pressureSchema,
            "Format": "JSON",
            "Fields": {
              "pressure": "Double",
              "pressure_unit": "Text"
            }
          }
        }
      },
      "Type": deviceType,
      "Firmware": deviceFirmware,
      "FirmwareUpdateStatus": deviceFirmwareUpdateStatus,
      "Location": deviceLocation,
      "Latitude": deviceLatitude,
      "Longitude": deviceLongitude
    }
    ```

1. Fügen Sie die folgende Hilfsfunktion hinzu, um Ergebnisse des Vorgangs auszugeben:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Fügen Sie die folgende Hilfsfunktion für die zufällige Festlegung der Telemetriewerte hinzu:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Fügen Sie die folgende Funktion zur Verarbeitung der Aufrufe von direkten Methoden aus der Lösung hinzu. Die Lösung verwendet direkte Methoden, die auf Geräte einwirken:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (!!err) {
          console.error('An error ocurred when sending a method response:\n' +
            err.toString());
        } else {
          console.log('Response to method \'' + request.methodName +
            '\' sent successfully.');
        }
      });
    }
    ```

1. Fügen Sie den folgenden Code zum Senden von Telemetriedaten an die Lösung hinzu. Die Clientanwendung fügt der Nachricht Eigenschaften zur Identifizierung des Nachrichtenschemas hinzu:

    ```node.js
    function sendTelemetry(data, schema) {
      var d = new Date();
      var payload = JSON.stringify(data);
      var message = new Message(payload);
      message.properties.add('$$CreationTimeUtc', d.toISOString());
      message.properties.add('$$MessageSchema', schema);
      message.properties.add('$$ContentType', 'JSON');

      console.log('Sending device message data:\n' + payload);
      client.sendEvent(message, printErrorFor('send event'));
    }
    ```

1. Fügen Sie den folgenden Code zum Erstellen einer Clientinstanz hinzu:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Fügen Sie den folgenden Code zu folgenden Zwecken hinzu:

    - Öffnen der Verbindung.
    - Einrichten eines Handlers für die gewünschten Eigenschaften.
    - Senden gemeldeter Eigenschaften.
    - Registrieren des Handlers für die direkten Methoden.
    - Starten des Sendens von Telemetriedaten.

    ```nodejs
    client.open(function (err) {
      if (err) {
        printErrorFor('open')(err);
      } else {
        // Create device Twin
        client.getTwin(function (err, twin) {
          if (err) {
            console.error('Could not get device twin');
          } else {
            console.log('Device twin created');

            twin.on('properties.desired', function (delta) {
              // Handle desired properties set by solution
              console.log('Received new desired properties:');
              console.log(JSON.stringify(delta));
            });

            // Send reported properties
            twin.properties.reported.update(reportedProperties, function (err) {
              if (err) throw err;
              console.log('twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onDirectMethod);
            client.onDeviceMethod('EmergencyValveRelease', onDirectMethod);
            client.onDeviceMethod('IncreasePressure', onDirectMethod);
          }
        });

        // Start sending telemetry
        var sendTemperatureInterval = setInterval(function () {
          temperature += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit
          };
          sendTelemetry(data, temperatureSchema)
        }, 5000);

        var sendHumidityInterval = setInterval(function () {
          humidity += generateRandomIncrement();
          var data = {
            'humidity': humidity,
            'humidity_unit': humidityUnit
          };
          sendTelemetry(data, humiditySchema)
        }, 5000);

        var sendPressureInterval = setInterval(function () {
          pressure += generateRandomIncrement();
          var data = {
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, pressureSchema)
        }, 5000);

        client.on('error', function (err) {
          printErrorFor('client')(err);
          if (sendTemperatureInterval) clearInterval(sendTemperatureInterval);
          if (sendHumidityInterval) clearInterval(sendHumidityInterval);
          if (sendPressureInterval) clearInterval(sendPressureInterval);
          client.close(printErrorFor('client.close'));
        });
      }
    });
    ```

1. Speichern Sie die Änderungen in der Datei **remote_monitoring.js**.

1. Führen Sie an der Eingabeaufforderung des Raspberry Pi den folgenden Befehl aus, um die Beispielanwendung zu starten:

    ```sh
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
