---
title: Weiterleiten von Nachrichten mit Azure IoT Hub (Python) | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure IoT Hub-D2C-Nachrichten mithilfe von Routingregeln und benutzerdefinierten Endpunkten verarbeitet werden, um Nachrichten an andere Back-End-Dienste zu senden.
services: iot-hub
documentationcenter: python
author: kgremban
manager: timlt
editor: ''
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: v-masebo;kgremban
ms.openlocfilehash: 98216328e970af5f8acf04c7e7c3ca855f7065f1
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="routing-messages-with-iot-hub-python"></a>Weiterleiten von Nachrichten mit IoT Hub (Python)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Dieses Tutorial baut auf dem Tutorial [Erste Schritte mit IoT Hub] auf.  Das Tutorial bietet Folgendes:

* Zeigt, wie Sie Routingregeln zum Senden von Nachrichten vom Gerät an die Cloud auf einfache Weise und auf Konfigurationsbasis einsetzen.
* Veranschaulicht das Isolieren von interaktiven Nachrichten, die vom Lösungs-Back-End sofortiges Eingreifen zur weiteren Verarbeitung erfordern.  Beispielsweise könnte ein Gerät eine Alarmnachricht senden, die das Einfügen eines Tickets in ein CRM-System auslöst.  Im Gegensatz dazu werden Datenpunktnachrichten wie Temperaturtelemetrie in eine Analyse-Engine eingegeben.

Am Ende dieses Tutorials führen Sie drei Python-Konsolen-Apps aus:

* **SimulatedDevice.py**, eine geänderte Version der im Tutorial [Erste Schritte mit IoT Hub] erstellten App, die jede Sekunde Datenpunkt-D2C-Nachrichten und in zufälligen Intervallen interaktive D2C-Nachrichten sendet. In dieser App wird das MQTT-Protokoll für die Kommunikation mit IoT Hub verwendet.
* **ReadCriticalQueue.py** entfernt die kritischen Nachrichten aus der Service Bus-Warteschlange, die mit dem IoT Hub verbunden ist.

> [!NOTE]
> IoT Hub verfügt über SDK-Unterstützung für zahlreiche Geräteplattformen und Sprachen, z.B. C, Java und JavaScript. Anleitungen zum Ersetzen des Geräts in diesem Tutorial durch ein physisches Gerät und Informationen zum Verbinden von Geräten mit einem IoT Hub finden Sie im [Azure IoT Developer Center].

Für dieses Tutorial benötigen Sie Folgendes:

* Eine vollständig funktionierende Version des Tutorials [Erste Schritte mit IoT Hub] .
* [Python 2.x oder 3.x][lnk-python-download]. Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Fügen Sie Python Ihrer plattformspezifischen Umgebungsvariablen hinzu, wenn Sie während der Installation dazu aufgefordert werden. Bei Verwendung von Python 2.x müssen Sie ggf. *pip*, das Python-Paketverwaltungssystem, [installieren oder aktualisieren][lnk-install-pip].
* Wenn Sie das Windows-Betriebssystem nutzen, wird das [Visual C++ Redistributable Package][lnk-visual-c-redist] verwendet, um die Nutzung von nativen DLLs aus Python zu ermöglichen.
* [Node.js 4.0 oder höher][lnk-node-download]. Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Sie ist erforderlich, um das [IoT Hub-Explorer-Tool][lnk-iot-hub-explorer] zu installieren.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Außerdem sollten Sie sich mit [Azure Storage] und [Azure Service Bus] vertraut machen.


## <a name="send-interactive-messages-from-a-device-app"></a>Senden interaktiver Nachrichten von einer Geräte-App aus
In diesem Abschnitt ändern Sie die Geräte-App, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, sodass sie gelegentlich Nachrichten sendet, die sofort verarbeitet werden müssen.

1. Öffnen Sie mithilfe eines Text-Editors die Datei **SimulatedDevice.py**. Diese Datei enthält den Code für die App **SimulatedDevice**, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben.

2. Ersetzen Sie die Funktion **iothub_client_telemetry_sample_run** durch folgenden Code:

    ```python
    def iothub_client_telemetry_sample_run():

    try:
        client = iothub_client_init()
        print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
        message_counter = 0

        while True:
            random_seed = random.random()
            msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random_seed * 4 + 2))
            # messages can be encoded as string or bytearray
            if (message_counter & 1) == 1:
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
            else:
                message = IoTHubMessage(msg_txt_formatted)
            # optional: assign ids
            message.message_id = "message_%d" % message_counter
            message.correlation_id = "correlation_%d" % message_counter
            # optional: assign properties
            prop_map = message.properties()
            if random_seed > .5:
                if random_seed > .8:
                    prop_map.add("level", 'critical')
                else:
                    prop_map.add("level", 'storage')

            client.send_event_async(message, send_confirmation_callback, message_counter)
            print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

            status = client.get_send_status()
            print ( "Send status: %s" % status )
            time.sleep(30)

            status = client.get_send_status()
            print ( "Send status: %s" % status )

            message_counter += 1

    except IoTHubError as iothub_error:
        print ( "Unexpected error %s from IoTHub" % iothub_error )
        return
    except KeyboardInterrupt:
        print ( "IoTHubClient sample stopped" )
    ```
   
    Mit dieser Methode werden vom Gerät gesendeten Nachrichten nach dem Zufallsprinzip die Eigenschaften `"level": "critical"` und `"level": "storage"` hinzugefügt. Dadurch wird eine Nachricht simuliert, die eine sofortige Aktion durch das Anwendungs-Back-End erfordert oder dauerhaft gespeichert werden muss. Die Anwendung unterstützt das Weiterleiten von Nachrichten auf der Grundlage des Nachrichtentexts.
   
   > [!NOTE]
   > Sie können Nachrichteneigenschaften zum Weiterleiten von Nachrichten für verschiedene Szenarien zusätzlich zu dem hier gezeigten Beispiel des langsamsten Pfads verwenden – einschließlich der Cold-Path-Verarbeitung.

1. Speichern und schließen Sie die Datei **SimulatedDevice.py**.

    > [!NOTE]
    > Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie eine Wiederholungsrichtlinie implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler]beschrieben.


## <a name="add-queues-to-your-iot-hub-and-route-messages-to-them"></a>Hinzufügen von Warteschlangen zu Ihrer IoT Hub-Instanz und Weiterleiten von Nachrichten an diese Warteschlangen

In diesem Abschnitt erstellen Sie eine Service Bus-Warteschlange und ein Storage-Konto, verbinden sie mit Ihrem IoT Hub und konfigurieren Ihre IoT Hub-Instanz so, dass Nachrichten an die Warteschlange gesendet werden, wenn eine Eigenschaft in der Nachricht vorhanden ist, und alle Nachrichten an das Storage-Konto gesendet werden. Weitere Informationen zur Verarbeitung von Nachrichten aus Service Bus-Warteschlangen finden Sie unter [Verwenden von Service Bus-Warteschlangen mit Python][lnk-sb-queues-node]. Weitere Informationen zum Verwalten des Speichers finden Sie in der [Dokumentation für Azure Storage][Azure Storage].

1. Erstellen Sie eine Service Bus-Warteschlange wie in [Erste Schritte mit Warteschlangen][lnk-sb-queues-node] beschrieben. Notieren Sie den Namespace und den Warteschlangennamen.

    > [!NOTE]
    > Für Service Bus-Warteschlangen und -Themen, die als IoT Hub-Endpunkte verwendet werden, dürfen **Sitzungen** oder **Duplikaterkennung** nicht aktiviert werden. Wenn eine dieser Optionen aktiviert ist, wird der Endpunkt im Azure-Portal als **Nicht erreichbar** angezeigt.

1. Öffnen Sie Ihren IoT-Hub im Azure-Portal, und klicken Sie auf **Endpunkte**.

    ![Endpunkte in IoT Hub][30]

1. Klicken Sie oben auf dem Blatt **Endpunkte** auf **Hinzufügen**, um die Warteschlange Ihrem IoT-Hub hinzuzufügen. Benennen Sie den Endpunkt **CriticalQueue**, und wählen Sie mithilfe der Dropdownlisten **Service Bus-Warteschlange**, den Service Bus-Namespace, in dem sich die Warteschlange befindet, und den Namen der Warteschlange. Klicken Sie abschließend am unteren Rand auf **OK**.  

    ![Hinzufügen eines Endpunkts][31]

1. Klicken Sie jetzt in Ihrem IoT-Hub auf **Routen**. Klicken Sie am oberen Rand des Blatts auf **Hinzufügen**, um eine Routingregel zu erstellen, die Nachrichten an die gerade von Ihnen hinzugefügte Warteschlange leitet. 

    ![Hinzufügen einer Route][34]

    Geben Sie einen Namen ein, und wählen Sie **Gerätemeldungen** als Datenquelle aus. Wählen Sie **CriticalQueue** als benutzerdefinierten Endpunkt des Routingregel-Endpunkts aus, und geben Sie `level="critical"` als Abfragezeichenfolge ein. Klicken Sie am unteren Rand auf **Speichern**.

    ![Routendetails][32]

    Stellen Sie sicher, dass die Fallbackroute auf **EIN** festgelegt ist. Diese Einstellung ist die Standardkonfiguration eines IoT-Hubs.

    ![Fallbackroute][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(Optional) Lesen aus dem Warteschlangen-Endpunkt

In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, die kritische Nachrichten aus IoT Service Bus liest. Weitere Informationen finden Sie unter [Erste Schritte mit Warteschlangen][lnk-sb-queues-node]. 

1. Öffnen Sie eine neue Eingabeaufforderung, und installieren Sie wie folgt das Azure IoT Hub Device SDK für Python. Schließen Sie die Eingabeaufforderung nach der Installation.

    ```cmd/sh
    pip install azure-servicebus
    ```

    > [!NOTE]
    > Informationen zu Problemen beim Installieren des Pakets **azure-servicebus** sowie weitere Installationsoptionen finden Sie unter [hier][lnk-python-service-bus].

1. Erstellen Sie eine Datei namens **ReadCriticalQueue.py**. Öffnen Sie diese Datei in einem Python-Editor bzw. einer IDE Ihrer Wahl (z.B. IDLE).

1. Fügen Sie den folgenden Code hinzu, um die erforderlichen Module aus dem Geräte-SDK zu importieren:

    ```python
    from azure.servicebus import ServiceBusService, Message, Queue
    ```

1. Fügen Sie den folgenden Code hinzu, und ersetzen Sie die Platzhalter durch Verbindungsdaten für Ihre Service Bus-Instanz:

    ```python
    SERVICE_BUS_NAME = {serviceBusName}
    SHARED_ACCESS_POLICY_NAME = {sharedAccessPolicyName}
    SHARED_ACCESS_POLICY_KEY_VALUE = {sharedAccessPolicyKeyValue}
    QUEUE_NAME = {queueName}    
    ```

5. Fügen Sie den folgenden Code hinzu, um eine Verbindung mit der Service Bus-Instanz herzustellen und daraus zu lesen: 

    ```python
    def setup_client():
        bus_service = ServiceBusService(
        service_namespace=SERVICE_BUS_NAME,
        shared_access_key_name=SHARED_ACCESS_POLICY_NAME,
        shared_access_key_value=SHARED_ACCESS_POLICY_KEY_VALUE)

        while True:
            msg = bus_service.receive_queue_message(QUEUE_NAME, peek_lock=False)
            print(msg.body)
    ```

1. Fügen Sie abschließend die main-Funktion hinzu. 

    ```python
    if __name__ == '__main__':
        setup_client()
    ```

1. Speichern und schließen Sie die Datei **ReadCriticalQueue.py**. Sie können nun die Anwendungen ausführen.


## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1. Öffnen Sie eine Eingabeaufforderung, und installieren Sie den IoT Hub-Explorer. 

    ```cmd/sh
    npm install -g iothub-explorer
    ```

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um damit zu beginnen, die D2C-Nachrichten von Ihrem Gerät zu überwachen. Verwenden Sie die Verbindungszeichenfolge Ihres IoT Hub im Platzhalter nach `--login`.

    ```cmd/sh
    iothub-explorer monitor-events [deviceId] --login "[IoTHub connection string]"
    ```

1. Öffnen Sie eine neue Eingabeaufforderung, und navigieren Sie zum Verzeichnis mit der Datei **SimulatedDevice.py**.

1. Führen Sie die Datei **SimulatedDevice.py** aus, mit der regelmäßig Telemetriedaten an Ihren IoT Hub gesendet werden. 
   
    ```cmd/sh
    python SimulatedDevice.py
    ```

1. Navigieren Sie zum Ausführen der Anwendung **ReadCriticalQueue** über eine Befehlszeile oder Shell zur Datei **ReadCriticalQueue.py**, und führen Sie den folgenden Befehl aus:

   ```cmd/sh
   python ReadCriticalQueue.py
   ```

1. Beachten Sie die Gerätenachrichten an der Eingabeaufforderung, indem Sie den IoT Hub-Explorer aus dem vorherigen Abschnitt ausführen. Beachten Sie die Nachrichten vom Typ `critical` in der Anwendung **ReadCriticalQueue**.

    ![Python-D2C-Nachrichten][2]


## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(Optional:) Hinzufügen eines Speichercontainers zu Ihrer IoT Hub-Instanz und Weiterleiten von Nachrichten an diese

In diesem Abschnitt erstellen Sie ein Speicherkonto, verbinden es mit Ihrer IoT Hub-Instanz und konfigurieren Ihre IoT Hub-Instanz für das Senden von Nachrichten an das Konto, wenn eine Eigenschaft in der Nachricht vorhanden ist. Weitere Informationen zum Verwalten von Speicher finden Sie unter [Erste Schritte mit Azure Storage][Azure Storage].

 > [!NOTE]
   > Mit dem Tarif _F1 Free_ erstellte IoT Hub-Konten sind auf einen einzelnen **Endpunkt** begrenzt. Sofern Sie sich nicht auf einen einzelnen **Endpunkt** beschränken müssen, können Sie **StorageContainer** zusätzlich zu **CriticalQueue** einrichten und beides parallel betreiben.

1. Erstellen Sie ein Speicherkonto, wie unter [Dokumentation zu Azure Storage][lnk-storage] beschrieben. Notieren Sie sich den Kontonamen.

2. Öffnen Sie Ihren IoT-Hub im Azure-Portal, und klicken Sie auf **Endpunkte**.

3. Wählen Sie auf dem Blatt **Endpunkte** den Endpunkt **CriticalQueue** aus, und klicken Sie auf **Löschen**. Klicken Sie auf **Ja** und dann auf **Hinzufügen**. Nennen Sie den Endpunkt **StorageContainer**, wählen Sie über die Dropdownfelder **Azure-Speichercontainer** aus, und erstellen Sie ein **Speicherkonto** und einen **Speichercontainer**.  Notieren Sie sich die Namen.  Klicken Sie abschließend am unteren Rand auf **OK**. 

 > [!NOTE]
   > Mit dem Tarif _F1 Free_ erstellte IoT Hub-Konten sind auf einen einzelnen **Endpunkt** begrenzt. Wenn Sie sich nicht auf einen einzelnen **Endpunkt** beschränken müssen, ist es nicht notwendig, **CriticalQueue** zu löschen.

4. Klicken Sie in Ihrer IoT Hub-Instanz auf **Routen**. Klicken Sie am oberen Rand des Blatts auf **Hinzufügen**, um eine Routingregel zu erstellen, die Nachrichten an die gerade von Ihnen hinzugefügte Warteschlange leitet. Wählen Sie **Gerätemeldungen** als Datenquelle aus. Geben Sie `level="storage"` als Bedingung ein, und wählen Sie **StorageContainer** als benutzerdefinierten Endpunkt des Routingregelendpunkts aus. Klicken Sie am unteren Rand auf **Speichern**.  

    Stellen Sie sicher, dass die Fallbackroute auf **EIN** festgelegt ist. Diese Einstellung ist die Standardkonfiguration eines IoT-Hubs.

1. Vergewissern Sie sich, dass Ihre vorherige Anwendung **SimulatedDevice.py** noch ausgeführt wird. 

1. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto, und klicken Sie unter **Blob-Dienst** auf **Blobs durchsuchen**.  Wählen Sie Ihren Container aus, navigieren Sie zu der JSON-Datei, klicken Sie auf die Datei, und klicken Sie anschließend auf **Herunterladen**, um die Daten anzuzeigen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, D2C-Nachrichten zuverlässig mit der Nachrichtenroutingfunktion von IoT Hub zu versenden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT-Solution Accelerator für Remoteüberwachung][lnk-suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [Entwicklungsleitfaden für IoT Hub].

Weitere Informationen zum Nachrichtenrouting in IoT Hub finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
[2]: ./media/iot-hub-python-python-process-d2c/output.png

[30]: ./media/iot-hub-python-python-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-python-python-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-python-python-process-d2c/route-creation.png
[33]: ./media/iot-hub-python-python-process-d2c/fallback-route.png
[34]: ./media/iot-hub-python-python-process-d2c/click-routes.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-sb-queues-node]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Entwicklungsleitfaden für IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Erste Schritte mit IoT Hub]: iot-hub-python-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot

[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
[lnk-storage]: https://docs.microsoft.com/azure/storage/
[lnk-python-service-bus]: https://pypi.python.org/pypi/azure-servicebus
