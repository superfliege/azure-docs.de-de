---
title: Bereitstellen von Azure Stream Analytics mit Azure IoT Edge | Microsoft-Dokumentation
description: "Bereitstellen von Azure Stream Analytics als Modul für ein Edge-Gerät"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: ebda79442b8feb9f052c3ae455fa43aafb7b5a6a
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Bereitstellen von Azure Stream Analytics als IoT Edge-Modul – Vorschau

IoT-Geräte können sehr große Datenmengen erzeugen. Um die Größe der übertragenen Daten zu reduzieren oder die Roundtriplatenz aussagekräftiger Erkenntnisse zu beseitigen, müssen diese Daten in bestimmten Fällen analysiert oder verarbeitet werden, bevor sie die Cloud erreichen.

[Azure Stream Analytics][azure-stream] (ASA) bietet eine aufwändig strukturierte Abfragesyntax für die Datenanalyse in der Cloud und auf IoT Edge-Geräten. Weitere Informationen zu ASA auf IoT Edge finden Sie in der [ASA-Dokumentation](../stream-analytics/stream-analytics-edge.md).

Dieses Tutorial leitet Sie durch die Erstellung eines Azure Stream Analytics-Auftrags und seine Bereitstellung auf einem IoT Edge-Gerät, damit ein lokaler Telemetriestream direkt auf dem Gerät verarbeitet werden kann und Warnungen erzeugt werden, mit denen Sofortmaßnahmen auf dem Gerät ausgelöst werden können.  In diesem Tutorial geht es um zwei Module: ein simuliertes Temperatursensormodul (tempSensor), das Temperaturdaten zwischen 20 und 120 Grad erzeugt, die alle 5 Sekunden um den Wert 1 erhöht werden, und ein ASA-Modul, das Temperaturen über 100 Grad herausfiltert. Das ASA-Modul setzt zudem tempSensor zurück, sobald über 30 Sekunden ein Durchschnittswert von 100 erreicht wird.

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines ASA-Auftrags zum Verarbeiten von Daten auf dem Edge-Gerät
> * Verbinden des neuen ASA-Auftrags mit anderen IoT Edge-Modulen
> * Bereitstellen des ASA-Auftrags auf einem IoT Edge-Gerät

## <a name="prerequisites"></a>Voraussetzungen

* IoT Hub 
* Das Gerät, das Sie im Schnellstart oder im Abschnitt „Bereitstellen von Azure IoT Edge“ erstellt und konfiguriert haben, oder ein simuliertes Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin]
* Docker auf Ihrem IoT Edge-Gerät
    * [Installieren Sie Docker unter Windows][lnk-docker-windows], und stellen Sie sicher, dass dieses Programm ausgeführt wird.
    * [Installieren Sie Docker unter Linux][lnk-docker-linux], und stellen Sie sicher, dass dieses Programm ausgeführt wird.
* Python 2.7.x auf Ihrem IoT Edge-Gerät
    * [Installieren Sie Python 2.7 unter Windows][lnk-python].
    * In den meisten Linux-Distributionen, einschließlich Ubuntu, ist Python 2.7 bereits installiert.  Führen Sie den folgenden Befehl aus, um zu überprüfen, ob pip installiert ist: `sudo apt-get install python-pip`.

> [!NOTE]
> Beachten Sie, dass Ihre Geräte-Verbindungszeichenfolge und Ihre IoT Edge-Geräte-ID für dieses Tutorial benötigt werden.

IoT Edge profitiert von vorgefertigten Azure Service IoT Edge-Modulen für eine schnelle Bereitstellung, und Azure Stream Analytics (ASA) ist ein solches Modul. Sie können über das Portal einen ASA-Auftrag erstellen und anschließend zum IoT Hub-Portal wechseln, um es als IoT Edge-Modul bereitzustellen.  

Weitere Informationen zu Azure Stream Analytics finden Sie im Abschnitt **Übersicht** der [Azure Stream Analytics-Dokumentation][azure-stream].

## <a name="create-an-asa-job"></a>Erstellen eines ASA-Auftrags

In diesem Abschnitt erstellen Sie einen Azure Stream Analytics-Auftrag, um Daten aus Ihrem IoT Hub abzurufen, die gesendeten Telemetriedaten bei Ihrem Gerät abzufragen und die Ergebnisse an einen Azure Storage-Container (Blob) weiterzuleiten. Weitere Informationen finden Sie im Abschnitt **Übersicht** der [Stream Analytics-Dokumentation][azure-stream]. 

> [!NOTE]
> Ein Azure Storage-Konto ist erforderlich, um einen Endpunkt anzugeben, der als Ausgabe in Ihrem ASA-Auftrag verwendet wird. Im folgenden Beispiel wird der Speichertyp Blob verwendet.  Weitere Informationen finden Sie im Abschnitt **Blobs** in der [Azure Storage-Dokumentation][azure-storage].

1. Navigieren Sie im Azure-Portal zu **Ressource erstellen > Speicher**, klicken Sie auf **Alle anzeigen**, und klicken Sie dann auf **Storage Account – Blob, File, Table, Queue** (Speicherkonto – Blob, Datei, Tabelle, Warteschlange).

2. Geben Sie einen Namen für Ihr Speicherkonto ein, und wählen Sie den gleichen Speicherort aus, in dem Ihr IoT Hub gespeichert ist. Klicken Sie auf **Erstellen**. Notieren Sie sich den Namen für die spätere Verwendung.

    ![Neues Speicherkonto][1]

3. Navigieren Sie im Azure-Portal zum gerade erstellten Speicherkonto. Klicken Sie unter **Blob-Dienst** auf **Blobs durchsuchen**. 
1. Erstellen Sie einen neuen Container für das ASA-Modul, um Daten zu speichern. Legen Sie als Zugriffsebene _Container_ fest. Klicken Sie auf **OK**.

    ![Speichereinstellungen][10]

1. Navigieren Sie im Azure-Portal zu **Ressourcen erstellen** > **Internet der Dinge**, und wählen Sie **Stream Analytics-Auftrag** aus.

1. Geben Sie einen Namen ein, **wählen Sie „Edge“ als Hostingumgebung**, und behalten Sie die übrigen Standardwerte bei.  Klicken Sie auf **Erstellen**.

    ![ASA erstellen][5]

2. Wechseln Sie in den erstellten Auftrag, wählen Sie unter **Auftragstopologie** die Option **Eingaben** aus, und klicken Sie auf **Hinzufügen**.

3. Geben Sie den Namen `temperature` ein, wählen Sie **Datenstrom** als Typ der Datenquelle aus, und behalten Sie die Standardeinstellungen für die anderen Parameter bei. Klicken Sie auf **Erstellen**.

    ![ASA-Eingabe][2]

    > [!NOTE]
    > Zusätzliche Eingaben können IoT Edge-spezifische Endpunkte enthalten.

4. Wählen Sie unter **Auftragstopologie** den Eintrag **Ausgaben** aus, und klicken Sie auf **Hinzufügen**.

5. Geben Sie den Namen `alert` ein und behalten Sie die Standardwerte bei. Klicken Sie auf **Erstellen**.

    ![ASA-Ausgabe][3]

6. Wählen Sie unter **Auftragstopologie** den Eintrag **Abfrage** aus, und geben Sie Folgendes ein:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 100
    ```

## <a name="deploy-the-job"></a>Bereitstellen des Auftrags

Sie können den ASA-Auftrag jetzt auf Ihrem IoT Edge-Gerät bereitstellen.

1. Wechseln Sie im Azure-Portal in Ihrem IoT Hub zu **IoT Edge (Vorschau)**, und öffnen Sie Ihr Blatt *{deviceId}*.

1. Wählen Sie **Set Modules** (Module festlegen) aus, und wählen Sie dann **Import Azure Service IoT Edge Module** (IoT Edge-Modul von Azure-Dienst importieren) aus.

1. Wählen Sie das Abonnement und den gerade erstellten ASA-Edge-Auftrag aus. Wählen Sie dann Ihr Speicherkonto aus. Klicken Sie auf **Speichern**.

    ![Modul festlegen][6]

1. Klicken Sie auf **IoT Edge-Modul hinzufügen**, um das Temperatursensormodul hinzuzufügen. Geben Sie _tempSensor_ als Name und `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` als Image-URL ein. Behalten Sie die restlichen Einstellungen unverändert bei, und klicken Sie auf **Speichern**.

    ![Temperaturmodul][11]

1. Kopieren Sie den Namen des ASA-Moduls. Klicken Sie auf **Weiter**, um Routen zu konfigurieren.

1. Kopieren Sie Folgendes nach **Routen**.  Ersetzen Sie _{moduleName}_ durch den zuvor kopierten Modulnamen:

    ```json
    {
        "routes": {                                                               
          "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream", 
          "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream", 
          "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")", 
          "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")" 
        }
    }
    ```

1. Klicken Sie auf **Weiter**.


1. Klicken Sie im Schritt **Vorlage überprüfen** auf **Senden**.

1. Kehren Sie zur Seite mit den Gerätedetails zurück, und klicken Sie auf **Aktualisieren**.  Es sollte nun angezeigt werden, dass das neue _{moduleName}_-Modul zusammen mit dem **IoT Edge-Agent**-Modul und dem **IoT Edge-Hub** ausgeführt wird.

    ![Modulausgabe][7]

## <a name="view-data"></a>Anzeigen von Daten

Wechseln Sie nun zu Ihrem IoT Edge-Gerät, um sich über die Interaktion zwischen dem ASA-Modul und dem tempSensor-Modul zu informieren.

1. Konfigurieren Sie die Runtime an der Eingabeaufforderung mit der Verbindungszeichenfolge für Ihr IoT Edge-Gerät:

    ```cmd/sh
    iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords  
    ```

1. Führen Sie diesen Befehl aus, um die Runtime zu starten:

    ```cmd/sh
    iotedgectl start  
    ```

1. Führen Sie diesen Befehl aus, um die Ausführung der Module zu beobachten:

    ```cmd/sh
    docker ps  
    ```

    ![Docker-Ausgabe][8]

1. Führen Sie diesen Befehl aus, um alle Systemprotokolle und Metrikdaten anzuzeigen. Verwenden Sie dabei den Modulnamen von oben:

    ```cmd/sh
    docker logs -f {moduleName}  
    ```

    ![Docker-Protokoll][9]

1. Klicken Sie im Azure-Portal in Ihrem Speicherkonto unter **Blob-Dienst** auf **Blobs durchsuchen**, wählen Sie den Container aus, und wählen Sie die neu erstellte JSON-Datei.

1. Klicken Sie auf **Herunterladen**, und zeigen Sie die Ergebnisse an.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Storage-Container und einen Streaming Analytics-Auftrag zum Analysieren von Daten konfiguriert, die von ihrem IoT Edge-Gerät stammen.  Danach haben Sie ein benutzerdefiniertes ASA-Modul geladen, um Daten von Ihrem Gerät über den Datenstrom in ein Blob zu verschieben, das heruntergeladen werden kann.  Sie können nun weitere Tutorials bearbeiten, um noch mehr darüber zu erfahren, wie sich mit Azure IoT Edge Lösungen für Ihr Unternehmen erstellen lassen.

> [!div class="nextstepaction"] 
> [Bereitstellen eines Azure Machine Learning-Modells als Modul][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[2]: ./media/tutorial-deploy-stream-analytics/asa_input.png
[3]: ./media/tutorial-deploy-stream-analytics/asa_output.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/en-us/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/en-us/azure/storage/
[azure-stream]: https://docs.microsoft.com/en-us/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/