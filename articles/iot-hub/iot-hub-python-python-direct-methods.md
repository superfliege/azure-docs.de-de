---
title: Direkte Azure IoT Hub-Methoden (Python) | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie direkte Azure IoT Hub-Methoden verwenden. Sie verwenden die Azure IoT SDKs für Python, um eine simulierte Geräte-App mit einer direkten Methode und eine Dienst-App zu implementieren, die die direkte Methode aufruft."
services: iot-hub
documentationcenter: 
author: msebolt
manager: timlt
editor: 
ms.assetid: ea9c73ca-7778-4e38-a8f1-0bee9d142f04
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: v-masebo
ms.openlocfilehash: 9dac7b45894c2da0dcd32e456c8806faadf814e9
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
# <a name="use-direct-methods-on-your-iot-device-with-python"></a>Verwenden direkter Methoden auf Ihrem IoT-Gerät mit Python
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Python-Konsolen-Apps:

* **CallMethodOnDevice.py**, die eine Methode in der simulierten Geräte-App aufruft und die Antwort anzeigt.
* **SimulatedDevice.py**, die mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub herstellt und auf die von der Cloud aufgerufene Methode antwortet.

> [!NOTE]
> Im Artikel [IoT Hub SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen Azure IoT SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.
> 

Für dieses Tutorial benötigen Sie Folgendes:

* [Python 2.x oder 3.x][lnk-python-download]. Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Fügen Sie Python Ihrer plattformspezifischen Umgebungsvariablen hinzu, wenn Sie während der Installation dazu aufgefordert werden. Bei Verwendung von Python 2.x müssen Sie ggf. *pip*, das Python-Paketverwaltungssystem, [installieren oder aktualisieren][lnk-install-pip].
* Wenn Sie das Windows-Betriebssystem nutzen, wird das [Visual C++ Redistributable Package][lnk-visual-c-redist] verwendet, um die Nutzung von nativen DLLs aus Python zu ermöglichen.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, die auf eine von der Cloud aufgerufene Methode antwortet.

1. Erstellen Sie in einem Text-Editor eine neue Datei namens **SimulatedDevice.py**.

1. Fügen Sie am Anfang der Datei **SimulatedDevice.py** die folgenden `import`-Anweisungen und -Variablen hinzu:
   
    ```python
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    
    WAIT_COUNT = 5
    METHOD_CONTEXT = 0
    METHOD_CALLBACKS = 0

    # chose MQTT or MQTT_WS as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "<deviceConnectionString>"
    ```

1. Fügen Sie die folgenden Funktionen hinzu, um den Gerätemethodenrückruf und die direkte Methode auf dem Gerät zu implementieren:
   
    ```python
    def device_method_callback(method_name, payload, user_context):
        global METHOD_CALLBACKS
    
        if method_name == "DeviceMethod":
            onDeviceMethod()
        
        print ( "\nMethod callback called with:\nmethodName = %s\npayload = %s\ncontext = %s" % (method_name, payload, user_context) )
        METHOD_CALLBACKS += 1
    
        print ( "Total calls confirmed: %d\n" % METHOD_CALLBACKS )
        device_method_return_value = DeviceMethodReturnValue()
        device_method_return_value.response = "{ \"Response\": \"This is the response from the device\" }"
        device_method_return_value.status = 200
    
        return device_method_return_value

    def onDeviceMethod():
        print ( "Direct method called." )
    ```

1. Fügen Sie de folgende Funktion hinzu, um die Verbindung mit Ihrem IoT Hub zu öffnen und den Methodenlistener zu initialisieren:
   
    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        if client.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            client.set_device_method_callback(device_method_callback, METHOD_CONTEXT)

        return client
    ```

1. Fügen Sie die main-Funktionen hinzu:

    ```python
    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for direct method call, press Ctrl-C to exit" )

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
        print ( "Starting the IoT Hub Python direct methods sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

1. Speichern und schließen Sie die Datei **SimulatedDevice.py**.

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen erneuten Verbindungsversuch), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.
> 


## <a name="call-a-method-on-a-device"></a>Aufrufen einer Methode auf einem Gerät
In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, die eine Methode in der simulierten Geräte-App aufruft und dann die Antwort anzeigt.


1. Erstellen Sie in einem Text-Editor eine neue Datei namens **CallMethodOnDevice.py**.

1. Fügen Sie am Anfang der Datei **CallMethodOnDevice.py** die folgenden `import`-Anweisungen und -Variablen hinzu:
   
    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "<IoTHubConnectionString>"
    DEVICE_ID = "<deviceId>"

    METHOD_NAME = "DeviceMethod"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    ```

1. Fügen Sie die folgenden Funktionen hinzu, um den Gerätemethodenrückruf und die direkte Methode auf dem Gerät zu implementieren:
   
    ```python
    def iothub_devicemethod_sample_run():
        try:
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)

            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Device Method called" )
            print ( "Device Method name       : {0}".format(METHOD_NAME) )
            print ( "Device Method payload    : {0}".format(METHOD_PAYLOAD) )
            print ( "" )
            print ( "Response status          : {0}".format(response.status) )
            print ( "Response payload         : {0}".format(response.payload) )

            try:
                # Try Python 2.xx first
                raw_input("Press Enter to continue...\n")
            except:
                pass
                # Use Python 3.xx in the case of exception
                input("Press Enter to continue...\n")

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )
    ```

1. Fügen Sie die main-Funktion hinzu:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceMethod Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicemethod_sample_run()
    ```

1. Speichern und schließen Sie die Datei **CallMethodOnDevice.py**.

## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.

1. Führen Sie an einer Eingabeaufforderung den folgenden Befehl aus, um mit dem Lauschen auf Methodenaufrufe von Ihrer IoT Hub-Instanz zu beginnen:
   
    ```cmd/sh
    python SimulatedDevice.py
    ```
   
    ![][7]

1. Führen Sie an einer Eingabeaufforderung den folgenden Befehl aus, um mit der Überwachung Ihrer IoT Hub-Instanz zu beginnen:
   
    ```cmd/sh
    python CallMethodOnDevice.py
    ```
   
    ![][8]

1. Durch das Ausgeben der Meldung sehen Sie das Gerät, das auf die Methode reagiert. Außerdem können Sie die Anwendung, die die Methode aufgerufen hat, mit der angezeigten Antwort vom Gerät anzeigen:
   
    ![][9]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die simulierte Geräte-App für das Reagieren auf von der Cloud aufgerufene Methoden zu aktivieren. Sie haben außerdem eine App erstellt, die Methoden auf dem Gerät aufruft und die Antwort vom Gerät angezeigt. 

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Erste Schritte mit IoT Hub]
* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

<!-- Images. -->
[7]: ./media/iot-hub-python-python-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-python-python-direct-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-python-python-direct-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Erste Schritte mit IoT Hub]: iot-hub-node-node-getstarted.md
