---
title: Hinzufügen eines echten Geräts zu einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Fügen Sie Ihrer Azure IoT Central-Anwendung als Bediener ein echtes Gerät hinzu.
author: sandeeppujar
ms.author: sandeepu
ms.date: 10/12/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: c3b8f4f8188a6a28cca8b77f169a00657b11dfc5
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154810"
---
# <a name="tutorial-add-a-real-device-to-your-azure-iot-central-application"></a>Tutorial: Hinzufügen eines echten Geräts zu Ihrer Azure IoT Central-Anwendung

In diesem Tutorial erfahren Sie, wie Sie Ihrer Microsoft Azure IoT Central-Anwendung ein echtes Gerät hinzufügen und das Gerät konfigurieren.

Das Tutorial besteht aus zwei Teilen:

1. Der erste Teil richtet sich an Bediener und erläutert, wie Sie Ihrer Microsoft Azure IoT Central-Anwendung ein echtes Gerät hinzufügen und das Gerät konfigurieren. Am Ende dieses Teils rufen Sie eine Verbindungszeichenfolge für den zweiten Teil ab.
2. Der zweite Teil richtet sich an Geräteentwickler und enthält Informationen zum Code auf Ihrem echten Gerät. Dem Beispielcode wird die Verbindungszeichenfolge aus dem ersten Teil hinzugefügt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines neuen echten Geräts
> * Konfigurieren des echten Geräts
> * Abrufen der Verbindungszeichenfolge für das echte Gerät aus der Anwendung
> * Nachvollziehen der Zuordnung zwischen Clientcode und Anwendung
> * Konfigurieren des Clientcodes für das echte Gerät

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, muss der Ersteller mindestens das erste Tutorial für Ersteller absolvieren, um die Azure IoT Central-Anwendung zu erstellen:

* [Definieren eines neuen Gerätetyps](tutorial-define-device-type.md) (obligatorisch)
* [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules.md) (optional)
* [Anpassen der Azure IoT Central-Ansicht für Bediener](tutorial-customize-operator.md) (optional)

## <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Um Ihrer Anwendung ein echtes Gerät hinzuzufügen, verwenden Sie die Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage), die Sie im Tutorial [Definieren eines neuen Gerätetyps](tutorial-define-device-type.md) erstellt haben.

1. Klicken Sie als Bediener im linken Navigationsmenü auf **Device Explorer**, um ein neues Gerät hinzuzufügen:

   ![Seite „Device Explorer“ mit verbundener Klimaanlage](media/tutorial-add-device/explorer.png)

   Der **Device Explorer** zeigt die Gerätevorlage **Connected Air Conditioner** und das simulierte Gerät an, das automatisch erstellt wurde, als der Ersteller die Gerätevorlage erstellt hat.

2. Klicken Sie auf **Neu** und anschließend auf **Real** (Echt), um damit zu beginnen, eine Verbindung mit einer echten verbundenen Klimaanlage herzustellen:

   ![Beginnen mit dem Herstellen einer Verbindung mit einer echten verbundenen Klimaanlage](media/tutorial-add-device/newreal.png)

3. Geben Sie die Geräte-ID (**muss in Kleinbuchstaben angegeben werden**) ein, oder verwenden Sie die vorgeschlagene Geräte-ID. Sie können auch einen Namen für Ihr neues Gerät eingeben und auf **Erstellen** klicken.  

   ![Umbenennen des Geräts](media/tutorial-add-device/rename.png)



## <a name="configure-a-real-device"></a>Konfigurieren eines echten Geräts

Das echte Gerät wird auf der Grundlage der Gerätevorlage **Connected Air Conditioner** erstellt. Sie können Ihr Gerät mithilfe von **Einstellungen** konfigurieren und Eigenschaftswerte zur Erfassung von Geräteinformationen festlegen.

1. Auf der Seite **Einstellungen** sehen Sie, dass die Einstellung **Set Temperature** (Sollwerttemperatur) den Status **no update** (Keine Aktualisierung) besitzt. Dieser Zustand ändert sich erst, wenn das echte Gerät eine Verbindung mit der Anwendung herstellt und bestätigt, dass auf die Einstellung reagiert wurde. 

    ![Einstellungen mit Synchronisierungsangabe](media/tutorial-add-device/settingssyncing.png)

2. Auf der Seite **Eigenschaften** Ihrer neuen, echten verbundenen Klimaanlage können Sie den Standort und das Datum der letzten Wartung des Geräts bearbeiten. Die Felder für Seriennummer und Firmwareversion sind leer, bis das Gerät mit der Anwendung verbunden wird. Die Werte sind schreibgeschützt. Sie werden vom Gerät gesendet und können nicht bearbeitet werden.

    ![Geräteeigenschaften für das echte Gerät](media/tutorial-add-device/setproperties1.png)

3. Sie können die Seiten **Measurements** (Messungen), **Regeln** und **Dashboard** für Ihr echtes Gerät anzeigen.

## <a name="generate-connection-string-for-real-device-from-application"></a>Generieren der Verbindungszeichenfolge für das echte Gerät über die Anwendung

Ein Geräteentwickler muss die *Verbindungszeichenfolge* für Ihr echtes Gerät in den Code einbetten, der auf dem Gerät ausgeführt wird. Die Verbindungszeichenfolge ermöglicht es dem Gerät, eine sichere Verbindung mit Ihrer Azure IoT Central-Anwendung herzustellen. Die Verbindungszeichenfolge wird in den nächsten Schritten im Rahmen der Vorbereitung des in Node.js geschriebenen Clientcodes generiert. Die Node.js-Anwendung stellt die echte verbundene Klimaanlage dar. 

## <a name="prepare-the-client-code"></a>Vorbereiten des Clientcodes

Der Beispielcode in diesem Artikel ist in [Node.js](https://nodejs.org/) geschrieben und zeigt gerade genügend Code für Folgendes:

* Herstellen einer Verbindung als Gerät mit Ihrer Azure IoT Central-Anwendung
* Senden von Telemetriedaten zur Temperatur als verbundene Klimaanlage
* Senden von Geräteeigenschaften an Ihre Azure IoT Central-Anwendung
* Reagieren auf einen Bediener, der die Einstellung **Set Temperature** (Sollwerttemperatur) verwendet
* Behandeln des Echo-Befehls aus Ihrer Azure IoT Central-Anwendung


Die Anleitungsartikel unter [Nächste Schritte](#next-steps) enthalten weitere vollständige Beispiele und veranschaulichen die Verwendung anderer Programmiersprachen. Weitere Informationen zur Verbindungsherstellung zwischen Geräten und Azure IoT Central finden Sie im Artikel [Device connectivity in Azure IoT Central](concepts-connectivity.md) (Gerätekonnektivität in Azure IoT Central).

Die folgenden Schritte zeigen, wie Sie das Beispiel für [Node.js](https://nodejs.org/) vorbereiten:

1. Installieren Sie auf Ihrem Computer mindestens die Version 4.0.x von [Node.js](https://nodejs.org/). Node.js ist für eine Vielzahl von Betriebssystemen verfügbar.

1. Erstellen Sie auf Ihrem Computer den Ordner `connectedairconditioner`.

1. Navigieren Sie in der Befehlszeilenumgebung zum erstellten Ordner `connectedairconditioner`.

1. Installieren Sie den DPS-Schlüsselgenerator mithilfe des folgenden Befehls:
    
    ```cmd/sh
    npm i -g dps-keygen
    ```

   Erfahren Sie mehr über das [Befehlszeilentool](https://www.npmjs.com/package/dps-keygen).

1. Laden Sie das Tool „dps_cstr“ von [GitHub](https://github.com/Azure/dps-keygen/tree/master/bin) herunter, und entzippen Sie es (Windows).

    Achten Sie darauf, das passende Tool für Ihre Plattform auszuwählen. Unter Windows sollten nun beispielsweise die Datei „dps_cstr.exe“ und die DLL-Dateien in Ihrem Ordner verfügbar sein. 

1. Die Verbindungszeichenfolge für eine Geräteinstanz in Ihrer Anwendung wird auf der Grundlage von Geräteinformationen generiert, die von IoT Central bereitgestellt werden.

   Kehren Sie zum IoT Central-Portal zurück. Klicken Sie auf dem Gerätebildschirm für Ihre echte verbundene Klimaanlage auf **Verbinden**.

   ![Geräteseite mit Link zum Anzeigen der Verbindungsinformationen](media/tutorial-add-device/connectionlink.png)


1. Kopieren Sie auf der Geräteverbindungsseite die Bereichs-ID, die Geräte-ID und den Primärschlüssel, fügen Sie die Werte in einen Text-Editor ein, und speichern Sie sie. Die Werte werden im nächsten Schritt benötigt.

   ![Verbindungsdetails](media/tutorial-add-device/device-connect.PNG)

1. Kehren Sie zur Befehlszeilenumgebung zurück, und führen Sie Folgendes aus, um Ihre Verbindungszeichenfolge zu generieren:

   ```cmd/sh
   dps_cstr <scope_id> <device_id> <Primary Key>
   ```
   
   Kopieren Sie die Ausgabe, und speichern Sie sie in einer neuen Datei (Beispiel: connection.txt).

1. Führen Sie zum Initialisieren Ihres Node.js-Projekts den folgenden Befehl aus, und behalten Sie dabei alle Standardwerte bei:

   ```cmd/sh
    npm init
   ```

1. Führen Sie zum Installieren der erforderlichen Pakete den folgenden Befehl aus:

   ```cmd/sh
   npm install azure-iot-device azure-iot-device-mqtt --save
   ```

1. Erstellen Sie im Ordner `connectedairconditioner` mit einem Text-Editor eine Datei namens **ConnectedAirConditioner.js**.

1. Fügen Sie am Anfang der Datei **ConnectedAirConditioner.js** die folgenden `require`-Anweisungen hinzu:

   ```javascript
   'use strict';

   var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
   var Message = require('azure-iot-device').Message;
   var ConnectionString = require('azure-iot-device').ConnectionString;
   ```

1. Fügen Sie der Datei folgende Variablendeklarationen hinzu:

   ```javascript
   var connectionString = '{your device connection string}';
   var targetTemperature = 0;
   var client = clientFromConnectionString(connectionString);
   ```
   > [!NOTE]
   > Der Platzhalter `{your device connection string}` wird in einem späteren Schritt aktualisiert. 

1. Speichern Sie die bisherigen Änderungen, lassen Sie die Datei aber noch geöffnet.

## <a name="understand-how-client-code-maps-to-the-application"></a>Nachvollziehen der Zuordnung zwischen Clientcode und Anwendung

Im vorherigen Abschnitt haben Sie ein provisorisches Node.js-Projekt für eine Anwendung erstellt, die eine Verbindung mit Ihrer Azure IoT Central-Anwendung herstellt. In diesem Abschnitt fügen Sie den Code für Folgendes hinzu:

* Herstellen einer Verbindung mit Ihrer Azure IoT Central-Anwendung
* Senden von Telemetriedaten an Ihre Azure IoT Central-Anwendung
* Senden von Geräteeigenschaften an Ihre Azure IoT Central-Anwendung
* Empfangen von Einstellungen von Ihrer Azure IoT Central-Anwendung
* Behandeln des Echo-Befehls aus Ihrer Azure IoT Central-Anwendung


1. Fügen Sie der Datei **ConnectedAirConditioner.js** den folgenden Code hinzu, um temperaturbezogene Telemetriedaten an Ihre Azure IoT Central-Anwendung zu senden:

   ```javascript
   // Send device telemetry.
   function sendTelemetry() {
     var temperature = targetTemperature + (Math.random() * 15);
     var data = JSON.stringify({ temperature: temperature });
     var message = new Message(data);
     client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
       (err ? `; error: ${err.toString()}` : '') +
       (res ? `; status: ${res.constructor.name}` : '')));
   }
   ```

   Der im JSON-Format gesendete Name des Felds muss dem Namen des Felds entsprechen, den Sie in Ihrer Gerätevorlage für die temperaturbezogenen Telemetriedaten angegeben haben. In diesem Beispiel lautet der Name des Felds **temperature**.


1. Fügen Sie zum Senden von Geräteeigenschaften wie **firmwareVersion** und **serialNumber** die folgende Definition hinzu:

   ```javascript
   // Send device properties
   function sendDeviceProperties(twin) {
     var properties = {
       firmwareVersion: "9.75",
       serialNumber: "10001"
     };
     twin.properties.reported.update(properties, (errorMessage) => 
       console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
   }
   ```

1. Fügen Sie die folgende Definition hinzu, um die Einstellungen zu definieren, die Ihr Gerät unterstützt (beispielsweise **setTemperature**):

   ```javascript
   // Add any settings your device supports
   // mapped to a function that is called when the setting is changed.
   var settings = {
     'setTemperature': (newValue, callback) => {
       // Simulate the temperature setting taking two steps.
       setTimeout(() => {
         targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
         callback(targetTemperature, 'pending');
         setTimeout(() => {
           targetTemperature = newValue;
           callback(targetTemperature, 'completed');
         }, 5000);
       }, 5000);
     }
   };
   ```

1. Fügen Sie für die Behandlung der von Azure IoT Central gesendeten Einstellungen die folgende Funktion hinzu, die den geeigneten Gerätecode sucht und ausführt:

   ```javascript
   // Handle settings changes that come from Azure IoT Central via the device twin.
   function handleSettings(twin) {
     twin.on('properties.desired', function (desiredChange) {
       for (let setting in desiredChange) {
         if (settings[setting]) {
           console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
           settings[setting](desiredChange[setting].value, (newValue, status, message) => {
             var patch = {
               [setting]: {
                 value: newValue,
                 status: status,
                 desiredVersion: desiredChange.$version,
                 message: message
               }
             }
             twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
               (err ? `error: ${err.toString()}` : `status: success`)));
           });
         }
       }
     });
   }
   ```

    Diese Funktion bewirkt Folgendes:

    * Sie überwacht, ob Azure IoT Central eine gewünschte Eigenschaft sendet.
    * Sie sucht die geeignete aufzurufende Funktion, um die Einstellungsänderung zu behandeln.
    * Sie gibt eine Bestätigung an Ihre Azure IoT Central-Anwendung zurück.

1. Fügen Sie die folgende Definition hinzu, um auf einen Befehl aus Ihrer Azure IoT Central-Anwendung (beispielsweise **Echo**) zu reagieren:

   ```javascript
   // Respond to the echo command
   function onCommandEcho(request, response) {
     // Display console info
     console.log(' * Echo command received');
     // Respond
     response.send(10, 'Success', function (errorMessage) {});
   }
   ```

1. Fügen Sie den folgenden Code hinzu, um die Verbindungsherstellung mit Azure IoT Central abzuschließen und die Funktionen im Clientcode einzubinden:

   ```javascript
   // Handle device connection to Azure IoT Central.
   var connectCallback = (err) => {
     if (err) {
       console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
     } else {
       console.log('Device successfully connected to Azure IoT Central');
       // Send telemetry measurements to Azure IoT Central every 1 second.
       setInterval(sendTelemetry, 1000);
       // Setup device command callbacks
       client.onDeviceMethod('echo', onCommandEcho);
       // Get device twin from Azure IoT Central.
       client.getTwin((err, twin) => {
         if (err) {
           console.log(`Error getting device twin: ${err.toString()}`);
         } else {
           // Send device properties once on device start up
           sendDeviceProperties(twin);
           // Apply device settings and handle changes to device settings.
           handleSettings(twin);
         }
       });
     }
   };

   client.open(connectCallback);
   ```

1. Speichern Sie die bisherigen Änderungen, lassen Sie die Datei aber noch geöffnet.

## <a name="configure-client-code-for-the-real-device"></a>Konfigurieren des Clientcodes für das echte Gerät

<!-- Add the connection string to the sample code, build, and run --> Um den Clientcode für die Verbindungsherstellung mit Ihrer Azure IoT Central-Anwendung zu konfigurieren, müssen Sie die Verbindungszeichenfolge für Ihr echtes Gerät hinzufügen, die Sie sich zuvor in diesem Tutorial notiert haben.

1. Suchen Sie in der Datei **ConnectedAirConditioner.js** nach folgender Codezeile:

   ```javascript
   var connectionString = '{your device connection string}';
   ```

1. Ersetzen Sie `{your device connection string}` durch die Verbindungszeichenfolge Ihres echten Geräts. Die Verbindungszeichenfolge haben Sie zuvor in einem Text-Editor gespeichert.

1. Speichern Sie die geänderte Datei **ConnectedAirConditioner.js**.

1. Geben Sie zum Ausführen des Beispiels den folgenden Befehl in der Befehlszeilenumgebung ein:

   ```cmd/sh
   node ConnectedAirConditioner.js
   ```

   > [!NOTE]
   > Achten Sie beim Ausführen dieses Befehls darauf, dass Sie sich im Ordner `connectedairconditioner` befinden.

1. Die Anwendung zeigt eine Ausgabe in der Konsole an:

   ![Ausgabe der Clientanwendung](media/tutorial-add-device/output.png)

1. Nach etwa 30 Sekunden werden die Telemetriedaten auf der Seite **Measurements** (Messungen) des Geräts angezeigt:

   ![Echte Telemetriedaten](media/tutorial-add-device/realtelemetry.png)

1. Auf der Seite **Einstellungen** sehen Sie, dass die Einstellung jetzt synchronisiert ist. Bei der erstmaligen Verbindungsherstellung hat das Gerät den Einstellungswert empfangen und die Änderung bestätigt:

   ![Einstellung synchronisiert](media/tutorial-add-device/settingsynced.png)

1. Legen Sie auf der Seite **Einstellungen** die Gerätetemperatur auf **95** fest, und klicken Sie auf **Update device** (Gerät aktualisieren). Ihre Beispielanwendung empfängt und verarbeitet diese Änderung:

   ![Empfangen und Verarbeiten der Einstellung](media/tutorial-add-device/receivesetting.png)

   > [!NOTE]
   > Dabei werden zwei entsprechende Nachrichten angezeigt: eine, wenn der Status `pending` gesendet wird, und eine, wenn der Status `completed` gesendet wird.

1. Auf der Seite **Measurements** (Messungen) sehen Sie, dass das Gerät höhere Temperaturwerte sendet:

    ![Temperaturbezogene Telemetriedaten sind nun höher.](media/tutorial-add-device/highertemperature.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="nextstepaction"]
> * Hinzufügen eines neuen echten Geräts
> * Konfigurieren des neuen Geräts
> * Abrufen der Verbindungszeichenfolge für das echte Gerät aus der Anwendung
> * Nachvollziehen der Zuordnung zwischen Clientcode und Anwendung
> * Konfigurieren des Clientcodes für das echte Gerät

Nachdem Sie nun eine Verbindung zwischen einem echten Gerät und Ihrer Azure IoT Central-Anwendung hergestellt haben, können Sie mit folgenden Schritten fortfahren:

Bediener:

* [Verwalten von Geräten](howto-manage-devices.md)
* [Verwenden von Gerätegruppen](howto-use-device-sets.md)
* [Erstellen benutzerdefinierter Analysen](howto-create-analytics.md)

Geräteentwickler:

* [Vorbereiten und Verbinden eines DevKit-Geräts](howto-connect-devkit.md)
* [Vorbereiten und Verbinden eines Raspberry Pi](howto-connect-raspberry-pi-python.md)
* [Verbinden eines generischen Node.js-Clients mit Ihrer Azure IoT Central-Anwendung](howto-connect-nodejs.md)
* [Anpassen Ihres Codes][lnk-nodejs-device-ref]


[lnk-nodejs-device-ref]: /javascript/api/azure-iot-device/?view=azure-iot-typescript-latest