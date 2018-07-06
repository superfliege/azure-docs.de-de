---
title: Hochladen von Dateien von Geräten nach Azure IoT Hub mit Python | Microsoft-Dokumentation
description: Informationen zum Hochladen von Dateien von einem Gerät in die Cloud mithilfe des Azure IoT-Geräte-SDK für Python. Hochgeladene Dateien werden in einem Azure Storage-Blobcontainer gespeichert.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: kgremban
ms.openlocfilehash: 532ae26dfd7add5c5ecc61db259903239e449f40
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "34635014"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Hochladen von Dateien von Ihrem Gerät in die Cloud mit IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Dieses Tutorial folgt auf die Verwendung der [Dateiuploadfunktionen von IoT Hub](iot-hub-devguide-file-upload.md) zum Hochladen einer Datei in [Azure Blob Storage](../storage/index.yml). Das Tutorial veranschaulicht folgende Vorgehensweisen:

- Sicheres Angeben eines Speichercontainers zum Hochladen einer Datei
- Hochladen einer Datei über Ihren IoT Hub mit dem Python-Client

Das Tutorial [Herstellen einer Verbindung zwischen dem simulierten Gerät und Ihrem IoT-Hub mit Node](iot-hub-node-node-getstarted.md) veranschaulicht die grundlegenden Gerät-zu-Cloud-Messagingfunktionen von IoT Hub. In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen D2C-Nachrichten zuordnen, die IoT Hub akzeptiert. Wenn Sie Dateien von einem Gerät hochladen müssen, können Sie weiterhin die Sicherheit und Zuverlässigkeit des IoT Hub nutzen.

> [!NOTE]
> Das IoT Hub-Python-SDK unterstützt derzeit nur das Hochladen von zeichenbasierten Dateien wie z.B. **TXT**-Dateien.

Am Ende dieses Tutorials führen Sie die folgende Python-Konsolen-App aus:

* **FileUpload.py**, die eine Datei mit dem Python-Geräte-SDK in den Speicher hochlädt.

> [!NOTE]
> IoT Hub bietet über Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, .NET, Javascript, Python und Java). Im [Azure IoT Developer Center] finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit Azure IoT Hub.

Für dieses Tutorial benötigen Sie Folgendes:

* [Python 2.x oder 3.x][lnk-python-download]. Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Fügen Sie Python Ihrer plattformspezifischen Umgebungsvariablen hinzu, wenn Sie während der Installation dazu aufgefordert werden. Bei Verwendung von Python 2.x müssen Sie ggf. *pip*, das Python-Paketverwaltungssystem, [installieren oder aktualisieren][lnk-install-pip].
* Wenn Sie das Windows-Betriebssystem nutzen, wird das [Visual C++ Redistributable Package][lnk-visual-c-redist] verwendet, um die Nutzung von nativen DLLs aus Python zu ermöglichen.
* Ein aktives Azure-Konto. (Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](http://azure.microsoft.com/pricing/free-trial/) erstellen.)


[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]


## <a name="upload-a-file-from-a-device-app"></a>Hochladen einer Datei von einer Geräte-App

In diesem Abschnitt erstellen Sie die Geräte-App zum Hochladen einer Datei in IoT Hub.

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um das Paket **azure-iothub-device-client** zu installieren:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Erstellen Sie mit einem Text-Editor in Ihrem Arbeitsordner die Datei **FileUpload.py**.

1. Fügen Sie am Anfang der Datei **FileUpload.py** die folgenden `import`-Anweisungen und Variablen hinzu. Ersetzen Sie `deviceConnectionString` durch die Verbindungszeichenfolge Ihres IoT Hub-Geräts:

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "[Device Connection String]"
    PROTOCOL = IoTHubTransportProvider.HTTP

    PATHTOFILE = "[Full path to file]"
    FILENAME = "[File name on storage after upload]"
    ```

1. Erstellen Sie einen Rückruf für die **upload_blob**-Funktion:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

1. Fügen Sie den folgenden Code hinzu, um eine Verbindung mit dem Client herzustellen und die Datei hochzuladen. Schließen Sie auch die Routine `main` ein:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )

            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            f = open(PATHTOFILE, "r")
            content = f.read()

            client.upload_blob_async(FILENAME, content, len(content), blob_upload_conf_callback, 0)

            print ( "" )
            print ( "File upload initiated..." )

            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )

    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

1. Speichern und schließen Sie die Datei **UploadFile.py**.

1. Kopieren Sie eine Beispiel-Textdatei in den Arbeitsordner, und benennen Sie sie in `sample.txt` um.

    > [!NOTE]
    > Das IoT Hub-Python-SDK unterstützt derzeit nur das Hochladen von zeichenbasierten Dateien wie z.B. **TXT**-Dateien.


## <a name="run-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt ausführen.

1. Führen Sie an der Eingabeaufforderung in Ihrem Arbeitsordner den folgenden Befehl aus:

    ```cmd/sh
    python FileUpload.py
    ```

1. Der folgende Screenshot zeigt die Ausgabe der App **FileUpload**:

    ![Ausgabe der simulated-device-App](./media/iot-hub-python-python-file-upload/1.png)

1. Sie können das Verwaltungsportal verwenden, um die hochgeladene Datei im Speichercontainer anzuzeigen, den Sie konfiguriert haben:

    ![Hochgeladene Datei](./media/iot-hub-python-python-file-upload/2.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. In den folgenden Artikeln werden weitere IoT Hub-Features und -Szenarien vorgestellt:

* [Programmgesteuertes Erstellen eines IoT Hubs][lnk-create-hub]
* [Einführung in das C SDK][lnk-c-sdk]
* [Azure IoT SDKs][lnk-sdks]

<!-- Links -->
[Azure IoT Developer Center]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
