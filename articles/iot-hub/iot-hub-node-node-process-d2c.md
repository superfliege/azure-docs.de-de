---
title: Weiterleiten von Nachrichten mit Azure IoT Hub (Node) | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure IoT Hub-D2C-Nachrichten mithilfe von Routingregeln und benutzerdefinierten Endpunkten verarbeitet werden, um Nachrichten an andere Back-End-Dienste zu senden.
services: iot-hub
documentationcenter: node
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2017
ms.author: v-masebo;dobett
ms.openlocfilehash: baed8a4c5086a9e3c757a3ecb8a6811c7be6b09e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="routing-messages-with-iot-hub-node"></a>Weiterleiten von Nachrichten mit IoT Hub (Node)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Dieses Tutorial baut auf dem Tutorial [Erste Schritte mit IoT Hub] auf.  Das Tutorial bietet Folgendes:

* Zeigt, wie Sie Routingregeln zum Senden von Nachrichten vom Gerät an die Cloud auf einfache Weise und auf Konfigurationsbasis einsetzen.
* Veranschaulicht das Isolieren von interaktiven Nachrichten, die vom Lösungs-Back-End sofortiges Eingreifen zur weiteren Verarbeitung erfordern.  Beispielsweise könnte ein Gerät eine Alarmnachricht senden, die das Einfügen eines Tickets in ein CRM-System auslöst.  Im Gegensatz dazu werden Datenpunktnachrichten wie Temperaturtelemetrie in eine Analyse-Engine eingegeben.

Am Ende dieses Tutorials führen Sie drei Node.js-Konsolen-Apps aus:

* **SimulatedDevice.js**, eine geänderte Version der im Tutorial [Erste Schritte mit IoT Hub] erstellten App, die jede Sekunde Datenpunkt-D2C-Nachrichten und in zufälligen Intervallen interaktive D2C-Nachrichten sendet. In dieser App wird das MQTT-Protokoll für die Kommunikation mit IoT Hub verwendet.
* **ReadDeviceToCloudMessages.js** zeigt die Telemetrie an, die Ihre Geräte-App sendet.
* **ReadCriticalQueue.js** nimmt die kritischen Nachrichten aus der Service Bus-Warteschlange, die mit dem IoT Hub verbunden ist.

> [!NOTE]
> IoT Hub verfügt über SDK-Unterstützung für zahlreiche Geräteplattformen und Sprachen, z.B. C, Java und JavaScript. Anleitungen zum Ersetzen des Geräts in diesem Tutorial durch ein physisches Gerät und Informationen zum Verbinden von Geräten mit einem IoT Hub finden Sie im [Azure IoT Developer Center].

Für dieses Tutorial benötigen Sie Folgendes:

* Eine vollständig funktionierende Version des Tutorials [Erste Schritte mit IoT Hub] .
* Node.js Version 4.0.x oder höher
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Außerdem sollten Sie sich mit [Azure Storage] und [Azure Service Bus] vertraut machen.

## <a name="send-interactive-messages-from-a-device-app"></a>Senden interaktiver Nachrichten von einer Geräte-App aus
In diesem Abschnitt ändern Sie die Geräte-App, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, sodass sie gelegentlich Nachrichten sendet, die sofort verarbeitet werden müssen.

1. Öffnen Sie mithilfe eines Text-Editors die Datei **simulateddevice\SimulatedDevice.js**. Diese Datei enthält den Code für die App **SimulatedDevice**, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben.

2. Ersetzen Sie die Funktion **connectCallback** durch folgenden Code:

    ```nodejs
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
    
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
        var data, message;
        if (Math.random() > 0.7) {
            if (Math.random() > 0.5) {
                data = "This is a critical message.";
                message = new Message(data);
                message.properties.add("level", "critical");
            } else {
                data = "This is a storage message.";
                message = new Message(data);
                message.properties.add("level", "storage");
            }
            console.log("Sending message: " + message.getData());
        }
        else {
                var temperature = 20 + (Math.random() * 15);
                var humidity = 60 + (Math.random() * 20);            
                data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
                message = new Message(data);
                message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
                console.log("Sending message: " + message.getData());
            }
        client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
    Mit dieser Methode werden vom Gerät gesendeten Nachrichten nach dem Zufallsprinzip die Eigenschaften `"level": "critical"` und `"level": "storage"` hinzugefügt. Dadurch wird eine Nachricht simuliert, die eine sofortige Aktion durch das Anwendungs-Back-End erfordert oder dauerhaft gespeichert werden muss. Die Anwendung unterstützt das Weiterleiten von Nachrichten auf der Grundlage des Nachrichtentexts.
   
   > [!NOTE]
   > Sie können Nachrichteneigenschaften zum Weiterleiten von Nachrichten für verschiedene Szenarien zusätzlich zu dem hier gezeigten Beispiel des langsamsten Pfads verwenden – einschließlich der Cold-Path-Verarbeitung.

2. Speichern und schließen Sie die Datei **simulateddevice\SimulatedDevice.js**.

    > [!NOTE]
    > Es wird dringend empfohlen, eine Wiederholungsrichtlinie zu implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Beheben vorübergehender Fehler] beschrieben.

## <a name="add-service-bus-queue-to-your-iot-hub-and-route-messages-to-it"></a>Hinzufügen einer Service Bus-Warteschlange zu Ihrem IoT Hub und Weiterleiten von Nachrichten an diese

In diesem Abschnitt erstellen Sie eine Service Bus-Warteschlange, verbinden sie mit Ihrem IoT Hub und konfigurieren Ihren IoT Hub zum Senden von Nachrichten an die Warteschlange, sofern eine Eigenschaft in der Nachricht vorhanden ist. Weitere Informationen zum Verarbeiten von Nachrichten von der Service Bus-Warteschlange finden Sie in [Erste Schritte mit Event Hubs][lnk-sb-queues-node].

1. Erstellen Sie eine Service Bus-Warteschlange wie in [Erste Schritte mit Warteschlangen][lnk-sb-queues-node] beschrieben. Notieren Sie den Namespace und den Warteschlangennamen.

    > [!NOTE]
    > Für Service Bus-Warteschlangen und -Themen, die als IoT Hub-Endpunkte verwendet werden, dürfen **Sitzungen** oder **Duplikaterkennung** nicht aktiviert werden. Wenn eine dieser Optionen aktiviert ist, wird der Endpunkt im Azure-Portal als **Nicht erreichbar** angezeigt.

2. Öffnen Sie Ihren IoT-Hub im Azure-Portal, und klicken Sie auf **Endpunkte**.

    ![Endpunkte in IoT Hub][30]

3. Klicken Sie oben auf dem Blatt **Endpunkte** auf **Hinzufügen**, um die Warteschlange Ihrem IoT-Hub hinzuzufügen. Benennen Sie den Endpunkt **CriticalQueue**, und wählen Sie mithilfe der Dropdownlisten **Service Bus-Warteschlange**, den Service Bus-Namespace, in dem sich die Warteschlange befindet, und den Namen der Warteschlange. Klicken Sie abschließend am unteren Rand auf **OK**.  

    ![Hinzufügen eines Endpunkts][31]

4. Klicken Sie jetzt in Ihrem IoT-Hub auf **Routen**. Klicken Sie am oberen Rand des Blatts auf **Hinzufügen**, um eine Routingregel zu erstellen, die Nachrichten an die gerade von Ihnen hinzugefügte Warteschlange leitet. Wählen Sie **Gerätemeldungen** als Datenquelle aus. Geben Sie `level="critical"` als Bedingung ein, und wählen Sie **CriticalQueue** als benutzerdefinierten Endpunkt des Routingregel-Endpunkts aus. Klicken Sie am unteren Rand auf **Speichern**.  

    ![Hinzufügen einer Route][32]

    Stellen Sie sicher, dass die Fallbackroute auf **EIN** festgelegt ist. Diese Einstellung ist die Standardkonfiguration eines IoT-Hubs.

    ![Fallbackroute][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(Optional) Lesen aus dem Warteschlangen-Endpunkt

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die kritische Nachrichten aus dem IoT-Service Bus liest. Weitere Informationen finden Sie unter [Erste Schritte mit Warteschlangen][lnk-sb-queues-node]. 

1. Erstellen Sie einen leeren Ordner mit dem Namen `readcriticalqueue`. Erstellen Sie im Ordner `readcriticalqueue` die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Übernehmen Sie alle Standardeinstellungen:

    ```cmd/sh
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner `readcriticalqueue` den folgenden Befehl aus, um das **azure**-Paket zu installieren:

    ```cmd/sh
    npm install azure --save
    ```

3. Erstellen Sie mit einem Text-Editor die Datei **ReadCriticalQueue.js** im Ordner `readcriticalqueue`.

4. Fügen Sie am Anfang der Datei **ReadCriticalQueue.js** die folgenden `require`-Anweisungen hinzu:

    ```nodejs
    'use strict';

    var azure = require('azure');
    ```

5. Fügen Sie die folgende Variablendeklaration hinzu, und ersetzen Sie den Platzhalterwert durch die Verbindungszeichenfolge und den Namen der Warteschlange für den IoT-Service Bus:

    ```nodejs
    var connectionString = '{iotservicebus connection string}';
    var queueName = '{iotservice bus queue name}';
    ```

6. Fügen Sie die folgenden beiden Funktionen hinzu, mit denen die Ausgabe an die Konsole erfolgt:

    ```nodejs
    var serviceBusService = azure.createServiceBusService(connectionString);

    setInterval(function() {serviceBusService.receiveQueueMessage(queueName, function(error, receivedMessage) {
        if(!error){
            // Message received and deleted
            console.log(receivedMessage);
        } else { console.log(error); }
        });
    }, 3000);
    ```

7. Speichern und schließen Sie die Datei **ReadCriticalQueue.js**.

## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können jetzt die drei Anwendungen ausführen.

1. Um die Anwendung **ReadDeviceToCloudMessages.js** auszuführen, navigieren Sie in einer Befehlszeile oder Shell zum Ordner „readdevicetocloudmessages“, und führen Sie den folgenden Befehl aus:

   ```cmd/sh
   node ReadDeviceToCloudMessages.js
   ```

   ![Ausführen von „read-d2c-messages“][readd2c]

2. Um die Anwendung **ReadCriticalQueue.js** auszuführen, navigieren Sie in einer Befehlszeile oder Shell zum Ordner „readcriticalqueue“, und führen Sie den folgenden Befehl aus:

   ```cmd/sh
   node ReadCriticalQueue.js
   ```
   
   ![Ausführen von „read-critical-messages“][readqueue]

3. Um die App **SimulatedDevice.js** auszuführen, navigieren Sie in einer Befehlszeile oder Shell zum Ordner „simulateddevice“, und geben Sie folgenden Befehl ein:

   ```cmd/sh
   node SimulatedDevice.js
   ```
   
   ![Ausführen von „simulated-device“][simulateddevice]

## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(Optional:) Hinzufügen eines Speichercontainers zu Ihrer IoT Hub-Instanz und Weiterleiten von Nachrichten an diese

In diesem Abschnitt erstellen Sie ein Speicherkonto, verbinden es mit Ihrer IoT Hub-Instanz und konfigurieren Ihre IoT Hub-Instanz für das Senden von Nachrichten an das Konto, wenn eine Eigenschaft in der Nachricht vorhanden ist. Weitere Informationen zum Verwalten von Speicher finden Sie unter [Erste Schritte mit Azure Storage][Azure Storage].

 > [!NOTE]
   > Sofern Sie sich nicht auf einen einzelnen **Endpunkt** beschränken müssen, können Sie **StorageContainer** zusätzlich zu **CriticalQueue** einrichten und beides parallel betreiben.

1. Erstellen Sie ein Speicherkonto, wie unter [Dokumentation zu Azure Storage][lnk-storage] beschrieben. Notieren Sie sich den Kontonamen.

2. Öffnen Sie Ihren IoT-Hub im Azure-Portal, und klicken Sie auf **Endpunkte**.

3. Wählen Sie auf dem Blatt **Endpunkte** den Endpunkt **CriticalQueue** aus, und klicken Sie auf **Löschen**. Klicken Sie auf **Ja** und dann auf **Hinzufügen**. Nennen Sie den Endpunkt **StorageContainer**, wählen Sie über die Dropdownfelder **Azure-Speichercontainer** aus, und erstellen Sie ein **Speicherkonto** und einen **Speichercontainer**.  Notieren Sie sich die Namen.  Klicken Sie abschließend am unteren Rand auf **OK**. 

 > [!NOTE]
   > Wenn Sie sich nicht auf einen einzelnen **Endpunkt** beschränken müssen, ist es nicht notwendig, **CriticalQueue** zu löschen.

4. Klicken Sie in Ihrer IoT Hub-Instanz auf **Routen**. Klicken Sie am oberen Rand des Blatts auf **Hinzufügen**, um eine Routingregel zu erstellen, die Nachrichten an die gerade von Ihnen hinzugefügte Warteschlange leitet. Wählen Sie **Gerätemeldungen** als Datenquelle aus. Geben Sie `level="storage"` als Bedingung ein, und wählen Sie **StorageContainer** als benutzerdefinierten Endpunkt des Routingregelendpunkts aus. Klicken Sie am unteren Rand auf **Speichern**.  

    Stellen Sie sicher, dass die Fallbackroute auf **EIN** festgelegt ist. Diese Einstellung ist die Standardkonfiguration eines IoT-Hubs.

1. Vergewissern Sie sich, dass Ihre vorherige Anwendung **SimulatedDevice.js** noch ausgeführt wird. 

1. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto, und klicken Sie unter **Blob-Dienst** auf **Blobs durchsuchen**.  Wählen Sie Ihren Container aus, navigieren Sie zu der JSON-Datei, klicken Sie auf die Datei, und klicken Sie anschließend auf **Herunterladen**, um die Daten anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, D2C-Nachrichten zuverlässig mit der Nachrichtenroutingfunktion von IoT Hub zu versenden.

Im Tutorial [Gewusst wie: Senden von C2D-Nachrichten mithilfe von IoT Hub][lnk-c2d] erfahren Sie, wie Sie Nachrichten aus Ihrem Lösungs-Back-End an Ihre Geräte senden.

Beispiele vollständiger End-to-End-Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT-Solution Accelerator für Remoteüberwachung][lnk-suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [Entwicklungsleitfaden für IoT Hub].

Weitere Informationen zum Nachrichtenrouting in IoT Hub finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-node-node-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-node-node-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-node-node-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-node-node-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-node-node-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-node-node-process-d2c/route-creation.png
[33]: ./media/iot-hub-node-node-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-node]: ../service-bus-messaging/service-bus-nodejs-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Entwicklungsleitfaden für IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Erste Schritte mit IoT Hub]: iot-hub-node-node-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[Beheben vorübergehender Fehler]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-node-node-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
[lnk-storage]: https://docs.microsoft.com/azure/storage/