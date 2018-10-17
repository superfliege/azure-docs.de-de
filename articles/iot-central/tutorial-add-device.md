---
title: Hinzufügen eines echten Geräts zu einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Fügen Sie Ihrer Azure IoT Central-Anwendung als Bediener ein echtes Gerät hinzu.
author: sandeeppujar
ms.author: sandeepu
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 2e01f61ff915a8fe4327aa78c8867d666dc36fda
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983225"
---
# <a name="tutorial-add-a-real-device-to-your-azure-iot-central-application"></a>Tutorial: Hinzufügen eines echten Geräts zu Ihrer Azure IoT Central-Anwendung

In diesem Tutorial erfahren Sie, wie Sie Ihrer Microsoft Azure IoT Central-Anwendung ein echtes Gerät hinzufügen und das Gerät konfigurieren.

Das Tutorial besteht aus zwei Teilen:

1. Der erste Teil richtet sich an Bediener und erläutert, wie Sie Ihrer Microsoft Azure IoT Central-Anwendung ein echtes Gerät hinzufügen und das Gerät konfigurieren. Am Ende dieses Teils rufen Sie eine Verbindungszeichenfolge für den zweiten Teil ab.
2. Der zweite Teil richtet sich an Geräteentwickler und enthält Informationen zum Code auf Ihrem echten Gerät. Dem Beispielcode wird die Verbindungszeichenfolge aus dem ersten Teil hinzugefügt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines neuen echten Geräts
> * Konfigurieren des neuen Geräts
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

3. Geben Sie die Geräte-ID (**muss in Kleinbuchstaben angegeben werden**) ein, oder verwenden Sie die vorgeschlagene Geräte-ID. Sie können auch einen Namen für Ihr neues Gerät eingeben.  

   ![Umbenennen des Geräts](media/tutorial-add-device/rename.png)

## <a name="configure-a-real-device"></a>Konfigurieren eines echten Geräts

Das echte Gerät wird auf der Grundlage der Gerätevorlage **Connected Air Conditioner** erstellt. Als Ersteller können Sie Ihr Gerät mithilfe von **Einstellungen** konfigurieren und Eigenschaftswerte zur Erfassung von Geräteinformationen festlegen.

1. Auf der Seite **Einstellungen** sehen Sie, dass die Einstellung **Set Temperature** (Sollwerttemperatur) den Status **no update** (Keine Aktualisierung) besitzt. Dieser Zustand ändert sich erst, wenn das echte Gerät eine Verbindung herstellt und bestätigt, dass auf die Einstellung reagiert wurde:

    ![Einstellungen mit Synchronisierungsangabe](media/tutorial-add-device/settingssyncing.png)

2. Legen Sie auf der Seite **Eigenschaften** für Ihre neue, echte vernetzte Klimaanlage die **Seriennummer** auf **10001** und die **Firmwareversion** auf „9.75“ fest. Klicken Sie anschließend auf **Speichern**:

    ![Festlegen der Eigenschaften für ein echtes Gerät](media/tutorial-add-device/setproperties.png)

3. Als Ersteller können Sie die Seiten **Measurements** (Messungen), **Regeln** und **Dashboard** für Ihr echtes Gerät anzeigen.

## <a name="get-connection-details-for-real-device-from-application"></a>Abrufen der Verbindungsdetails für ein echtes Gerät aus der Anwendung

Ein Geräteentwickler muss die *Verbindungsdetails für das Gerät* für Ihr echtes Gerät in den Code einbetten, der auf dem Gerät ausgeführt wird. Die Verbindungszeichenfolge ermöglicht es dem Gerät, eine sichere Verbindung mit Ihrer Azure IoT Central-Anwendung herzustellen. Die folgenden Schritte zeigen, wie Sie die Verbindungszeichenfolge für eine Geräteinstanz in Ihrer Anwendung finden:

1. Klicken Sie im Bildschirm **Gerät** für Ihre echte verbundene Klimaanlage auf **Connect this device** (Dieses Gerät verbinden):

    ![Geräteseite mit Link zum Anzeigen der Verbindungsinformationen](media/tutorial-add-device/connectionlink.png)

2. Kopieren Sie auf der Seite **Verbinden** die **Bereichs-ID, die Geräte-ID und den primären Schlüssel**, und speichern Sie diese Werte.

   ![Verbindungsdetails](media/tutorial-add-device/device-connect.PNG)

   Nutzen Sie das unten stehende Befehlszeilentool, um die Verbindungszeichenfolge abzurufen.  

    ```cmd/sh
    npm i -g dps-keygen
    ```
    **Verwendung**
    
    Um eine Verbindungszeichenfolge zu erstellen, suchen Sie die binäre Datei im Ordner „/bin“.
    ```cmd/sh
    dps_cstr <scope_id> <device_id> <Primary Key(for device)>
    ```
    Erfahren Sie mehr über das [Befehlszeilentool](https://www.npmjs.com/package/dps-keygen).

## <a name="prepare-the-client-code"></a>Vorbereiten des Clientcodes

Der Beispielcode in diesem Artikel ist in [Node.js](https://nodejs.org/) geschrieben und zeigt gerade genügend Code für Folgendes:

* Herstellen einer Verbindung als Gerät mit Ihrer Azure IoT Central-Anwendung
* Senden von Telemetriedaten zur Temperatur als verbundene Klimaanlage
* Reagieren auf einen Bediener, der die Einstellung **Set Temperature** (Sollwerttemperatur) verwendet

Die Anleitungsartikel unter [Nächste Schritte](#next-steps) enthalten weitere vollständige Beispiele und veranschaulichen die Verwendung anderer Programmiersprachen. Weitere Informationen zur Verbindungsherstellung zwischen Geräten und Azure IoT Central finden Sie im Artikel [Device connectivity in Azure IoT Central](concepts-connectivity.md) (Gerätekonnektivität in Azure IoT Central).

Die folgenden Schritte zeigen, wie Sie das Beispiel für [Node.js](https://nodejs.org/) vorbereiten:

1. Installieren Sie auf Ihrem Computer mindestens die Version 4.0.x von [Node.js](https://nodejs.org/). Node.js ist für eine Vielzahl von Betriebssystemen verfügbar.

2. Erstellen Sie auf Ihrem Computer den Ordner `connectedairconditioner`.

3. Navigieren Sie in der Befehlszeilenumgebung zum erstellten Ordner `connectedairconditioner`.

4. Führen Sie zum Initialisieren Ihres Node.js-Projekts den folgenden Befehl aus, und behalten Sie dabei alle Standardwerte bei:

   ```cmd/sh
   npm init
   ```

5. Führen Sie zum Installieren der erforderlichen Pakete den folgenden Befehl aus:

   ```cmd/sh
   npm install azure-iot-device azure-iot-device-mqtt --save
   ```

6. Erstellen Sie im Ordner `connectedairconditioner` mit einem Text-Editor eine Datei namens **ConnectedAirConditioner.js**.

7. Fügen Sie am Anfang der Datei **ConnectedAirConditioner.js** die folgenden `require`-Anweisungen hinzu:

   ```javascript
   'use strict';

   var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
   var Message = require('azure-iot-device').Message;
   var ConnectionString = require('azure-iot-device').ConnectionString;
   ```

8. Fügen Sie der Datei folgende Variablendeklarationen hinzu:

 

   ```javascript
   var connectionString = '{your device connection string}';
   var targetTemperature = 0;
   var client = clientFromConnectionString(connectionString);
   ```
   

   > [!NOTE]
   > Der Platzhalter `{your device connection string}` wird in einem späteren Schritt aktualisiert. 

9. Speichern Sie die bisherigen Änderungen, lassen Sie die Datei aber noch geöffnet.

## <a name="understand-how-client-code-maps-to-the-application"></a>Nachvollziehen der Zuordnung zwischen Clientcode und Anwendung

Im vorherigen Abschnitt haben Sie ein provisorisches Node.js-Projekt für eine Anwendung erstellt, die eine Verbindung mit Ihrer Azure IoT Central-Anwendung herstellt. In diesem Abschnitt fügen Sie den Code für Folgendes hinzu:

* Herstellen einer Verbindung mit Ihrer Azure IoT Central-Anwendung
* Senden von Telemetriedaten an Ihre Azure IoT Central-Anwendung
* Empfangen von Einstellungen von Ihrer Azure IoT Central-Anwendung

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

2. Fügen Sie die folgende Definition hinzu, um die Einstellungen zu definieren, die Ihr Gerät unterstützt (beispielsweise **setTemperature**):

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

3. Fügen Sie für die Behandlung der von Azure IoT Central gesendeten Einstellungen die folgende Funktion hinzu, die den geeigneten Gerätecode sucht und ausführt:

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

4. Fügen Sie den folgenden Code hinzu, um die Verbindungsherstellung mit Azure IoT Central abzuschließen und die Funktionen im Clientcode einzubinden:

   ```javascript
   // Handle device connection to Azure IoT Central.
   var connectCallback = (err) => {
     if (err) {
       console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
     } else {
       console.log('Device successfully connected to Azure IoT Central');
        // Send telemetry measurements to Azure IoT Central every 1 second.
       setInterval(sendTelemetry, 1000);
        // Get device twin from Azure IoT Central.
       client.getTwin((err, twin) => {
         if (err) {
           console.log(`Error getting device twin: ${err.toString()}`);
         } else {
           // Apply device settings and handle changes to device settings.
           handleSettings(twin);
         }
       });
     }
   };

   client.open(connectCallback);
   ```

5. Speichern Sie die bisherigen Änderungen, lassen Sie die Datei aber noch geöffnet.

## <a name="configure-client-code-for-the-real-device"></a>Konfigurieren des Clientcodes für das echte Gerät

<!-- Add the connection string to the sample code, build, and run --> Um den Clientcode für die Verbindungsherstellung mit Ihrer Azure IoT Central-Anwendung zu konfigurieren, müssen Sie die Verbindungszeichenfolge für Ihr echtes Gerät hinzufügen, die Sie sich zuvor in diesem Tutorial notiert haben.

1. Suchen Sie in der Datei **ConnectedAirConditioner.js** nach folgender Codezeile:

   ```javascript
   var connectionString = '{your device connection string}';
   ```

2. Ersetzen Sie `{your device connection string}` durch die Verbindungszeichenfolge Ihres echten Geräts. Die Verbindungszeichenfolge haben Sie sich am Ende des Abschnitts „Abrufen der Verbindungszeichenfolge für das echte Gerät aus der Anwendung“ notiert.

3. Speichern Sie die geänderte Datei **ConnectedAirConditioner.js**.

4. Geben Sie zum Ausführen des Beispiels den folgenden Befehl in der Befehlszeilenumgebung ein:

   ```cmd/sh
   node ConnectedAirConditioner.js
   ```

   > [!NOTE]
   > Achten Sie beim Ausführen dieses Befehls darauf, dass Sie sich im Ordner `connectedairconditioner` befinden.

5. Die Anwendung zeigt eine Ausgabe in der Konsole an:

   ![Ausgabe der Clientanwendung](media/tutorial-add-device/output.png)

6. Nach etwa 30 Sekunden werden die Telemetriedaten auf der Seite **Measurements** (Messungen) des Geräts angezeigt:

   ![Echte Telemetriedaten](media/tutorial-add-device/realtelemetry.png)

7. Auf der Seite **Einstellungen** sehen Sie, dass die Einstellung jetzt synchronisiert ist. Bei der erstmaligen Verbindungsherstellung hat das Gerät den Einstellungswert empfangen und die Änderung bestätigt:

   ![Einstellung synchronisiert](media/tutorial-add-device/settingsynced.png)

8. Legen Sie auf der Seite **Einstellungen** die Gerätetemperatur auf **95** fest, und klicken Sie auf **Update device** (Gerät aktualisieren). Ihre Beispielanwendung empfängt und verarbeitet diese Änderung:

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