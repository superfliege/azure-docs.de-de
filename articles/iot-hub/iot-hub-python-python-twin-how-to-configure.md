---
title: Verwenden von Azure IoT Hub-Gerätezwillingseigenschaften (Python) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Geräte konfigurieren. Sie verwenden die Azure IoT SDKs für Python, um eine simulierte Geräte-App und eine Dienst-App zu implementieren, die eine Gerätekonfiguration mithilfe eines Gerätezwillings ändert.
services: iot-hub
documentationcenter: .net
author: kgremban
manager: timlt
editor: ''
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2018
ms.author: kgremban
ms.openlocfilehash: 0ba614ab26caad31a045849d85a1abcbb5a9f818
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="use-desired-properties-to-configure-devices-python"></a>Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Python)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Python-Konsolen-Apps:

* **SimulateDeviceConfiguration.py**, eine App für ein simuliertes Gerät, die auf eine gewünschte Konfigurationsaktualisierung wartet und den Status eines simulierten Konfigurationsaktualisierungsvorgangs meldet.
* **SetDesiredConfigurationAndQuery.py**, eine Python-Back-End-App, die die gewünschte Konfiguration für ein Gerät festlegt und den Konfigurationsaktualisierungsprozess abfragt.

> [!NOTE]
> Im Artikel [Azure IoT SDKs][lnk-hub-sdks] finden Sie Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Apps erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* [Python 2.x oder 3.x][lnk-python-download]. Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Fügen Sie Python Ihrer plattformspezifischen Umgebungsvariablen hinzu, wenn Sie während der Installation dazu aufgefordert werden. Bei Verwendung von Python 2.x müssen Sie ggf. *pip*, das Python-Paketverwaltungssystem, [installieren oder aktualisieren][lnk-install-pip].
* Wenn Sie das Windows-Betriebssystem nutzen, wird das [Visual C++ Redistributable Package][lnk-visual-c-redist] verwendet, um die Nutzung von nativen DLLs aus Python zu ermöglichen.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Wenn Sie das Tutorial [Erste Schritte mit Gerätezwillingen][lnk-twin-tutorial] ausgeführt haben, haben Sie bereits einen IoT Hub und die Geräteidentität **myDeviceId** und können daher mit dem Abschnitt [Erstellen der simulierten Geräte-App][lnk-how-to-configure-createapp] fortfahren.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-simulated-device-app"></a>Erstellen der simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, die als **myDeviceId** eine Verbindung mit dem Hub herstellt, auf eine gewünschte Konfigurationsaktualisierung wartet und dann Aktualisierungen für den simulierten Konfigurationsaktualisierungsvorgang meldet.

1. Installieren Sie das **Azure IoT-Python-Geräte-SDK** mit dem folgenden Befehl an der Eingabeaufforderung:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Erstellen Sie in einem Text-Editor eine neue Datei namens **SimulateDeviceConfiguration.py**.

1. Fügen Sie in der Datei **SimulateDeviceConfiguration.py** den folgenden Code ein, und ersetzen Sie den Platzhalter **{device connection string}** durch die Geräteverbindungszeichenfolge, die Sie beim Erstellen der Geräteidentität **myDeviceId** kopiert haben:

    ```python   
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0

    TWIN_CALLBACKS = 0
    SEND_REPORTED_STATE_CALLBACKS = 0

    CONFIG_ID = "1"
    TWIN_PAYLOAD = "{\"configId\":" + CONFIG_ID + ",\"sendFrequency\":\"24h\"}"
    ```

    Das **CLIENT**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Geräts erforderlich sind. In weiterem Code fügen Sie einen Handler für die Aktualisierung von gewünschten Eigenschaften an und dann einen zusätzlichen Handler, um sicherzustellen, dass eine tatsächliche Anforderung zur Konfigurationsänderung vorliegt. Dazu werden die configIds verglichen und dann eine Methode aufgerufen, die die Konfigurationsänderung startet. Beim vorherigen Code wird der Einfachheit halber ein hartcodierter Standardwert für die anfängliche Konfiguration verwendet. Mit einer realen App wird diese Konfiguration wahrscheinlich aus einem lokalen Speicher geladen.
   
   > [!IMPORTANT]
   > Änderungsereignisse der gewünschten Eigenschaften werden bei der Geräteverbindung immer einmal ausgegeben. Stellen Sie vor dem Ausführen einer Aktion sicher, dass bei den gewünschten Eigenschaften eine tatsächliche Änderung vorliegt.
   > 

1. Fügen Sie den folgenden Code am Ende der Datei **SimulateDeviceConfiguration.py** hinzu:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global CONFIG_ID

        desired_configId = payload[payload.find("configId")+11:payload.find("configId")+12]
        if CONFIG_ID != desired_configId:
            print ( "" )
            print ( "Reported pending config change: %s" % payload)
        
            desired_sendFrequency = payload[payload.find("sendFrequency")+17:payload.find("sendFrequency")+19]
            print ( "...desired configId: " + desired_configId)
            print ( "...desired sendFrequency: " + desired_sendFrequency)
            new_payload = "{\"configId\":" + desired_configId + ",\"sendFrequency\":\"" + desired_sendFrequency + "\"}"
            CLIENT.send_reported_state(new_payload, len(new_payload), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        
        CONFIG_ID = desired_configId
    
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS
    
        print ( "" )
        print ( "Device twins updated." )
    ```
   
    Die _device_twin_callback_-Methode aktualisiert die gemeldeten Eigenschaften des lokalen Gerätezwillingsobjekts mit der Anforderung zur Konfigurationsaktualisierung und legt die **configId** fest. Nach der Aktualisierung des Gerätezwillings gibt die Methode eine Aktualisierungsmeldung aus. Zum Einsparen von Bandbreite wird beim Aktualisieren der gemeldeten Eigenschaften nicht das gesamte Dokument ersetzt, sondern es werden nur die zu ändernden Eigenschaften (mit dem Namen **TWIN_PAYLOAD** im Code oben) angegeben.
   
   > [!NOTE]
   > In diesem Tutorial wird nicht das Verhalten für gleichzeitige Konfigurationsaktualisierungen simuliert. Bei einigen Konfigurationsaktualisierungsvorgängen können Änderungen der Zielkonfiguration möglicherweise während der Ausführung der Aktualisierung erfolgen, und bei einigen müssen diese eventuell in die Warteschlange gestellt werden. Bei anderen wiederum können sie ohne eine Fehlerbedingung abgelehnt werden. Stellen Sie sicher, dass Sie das gewünschte Verhalten für den spezifischen Konfigurationsvorgang berücksichtigen, und fügen Sie vor dem Initiieren der Konfigurationsänderung die entsprechende Logik hinzu.
   > 

1. Fügen Sie den folgenden Code am Ende der Datei **SimulateDeviceConfiguration.py** hinzu: 

    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

    def iothub_client_sample_run():
        try:
            iothub_client_init()
        
            CLIENT.send_reported_state(TWIN_PAYLOAD, len(TWIN_PAYLOAD), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

1. Führen Sie die Geräte-App aus:
   
    ```cmd/sh
    node SimulateDeviceConfiguration.py
    ```
   
    Die Meldung `Device twins updated.` sollte angezeigt werden. Führen Sie die App weiter aus.


## <a name="create-the-service-app"></a>Erstellen der Dienst-App
In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, die die *gewünschten Eigenschaften* für den **myDeviceId** zugeordneten Gerätezwilling mit einem neuen Telemetriekonfigurationsobjekt aktualisiert. Anschließend werden die im IoT Hub gespeicherten Gerätezwillinge abgefragt und die Unterschiede zwischen den gewünschten und den gemeldeten Konfigurationen des Geräts angezeigt.

1. Installieren Sie das **Azure IoT-Python-Dienst-SDK** mit dem folgenden Befehl an der Eingabeaufforderung:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Erstellen Sie in einem Text-Editor eine neue Datei namens **SetDesiredAndQuery.py**.

1. Fügen Sie in der Datei **SetDesiredAndQuery.py** den folgenden Code ein, und ersetzen Sie dabei den Platzhalter **{IoTHubConnectionString}** durch die IoT Hub-Verbindungszeichenfolge, die Sie beim Erstellen des Hubs kopiert haben, und den Platzhalter **{deviceId}** durch den Namen Ihres Geräts:

    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubError, IoTHubDeviceTwin

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"configId\":3, \"sendFrequency\":\"" + sys.argv[1:][0] + "\"}}}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

1. Fügen Sie den folgenden Code am Ende der Datei **SetDesiredAndQuery.py** hinzu:

    ```python
    def iothub_devicetwin_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
        
            twin_info = iothub_twin_method.get_twin(DEVICE_ID)
            print ( "" )
            print ( "Device Twin before update    :" )
            print ( "{0}".format(twin_info) )

            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)
            print ( "" )
            print ( "Device Twin after update     :" )
            print ( "{0}".format(twin_info) )
        
            while True:
                time.sleep(10)

                twin_info = iothub_twin_method.get_twin(DEVICE_ID)
                print ( "" )
                print ( "Device Twin after client change initiated    :" )
                print ( "{0}".format(twin_info) )
        
                print ( "" )
                print ( "IoT Hub service sample complete, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(30)
                
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceTwins Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicetwin_sample_run()
    ```

1. Während **SimulateDeviceConfiguration.py** ausgeführt wird, führen Sie die Anwendung folgendermaßen an einer neuen Eingabeaufforderung aus:

    ```cmd/sh
    python SetDesiredAndQuery.py 5m
    ```
   
    Die gemeldete Konfigurations-ID sollte von **1** in **2** geändert werden, wobei die neue aktive Sendehäufigkeit fünf Minuten anstelle von 24 Stunden beträgt.


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine gewünschte Konfiguration als *gewünschte Eigenschaften* in einer Back-End-App festgelegt und eine simulierte Geräte-App geschrieben, die diese Änderung erkennt und einen Aktualisierungsvorgang simuliert, bei dem der entsprechende Status als *gemeldete Eigenschaften* an den Gerätezwilling gemeldet wird.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Senden von Telemetriedaten von Geräten im Tutorial [Erste Schritte mit IoT Hub][lnk-iothub-getstarted],
* Informationen zum Planen oder Durchführen von Vorgängen für eine Vielzahl von Geräten finden Sie im Tutorial [Planen und Übertragen von Aufträgen][lnk-schedule-jobs].
* Informationen zur interaktiven Steuerung von Geräten (z.B. Einschalten eines Lüfters über eine benutzergesteuerte App) im Tutorial [Verwenden von direkten Methoden][lnk-methods-tutorial].

<!-- links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-python-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-python-getstarted.md
[lnk-methods-tutorial]: iot-hub-python-python-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
