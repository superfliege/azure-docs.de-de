---
title: Bereitstellen von Azure Stream Analytics mit Azure IoT Edge | Microsoft-Dokumentation
description: "Bereitstellen von Azure Stream Analytics als Modul für ein Edge-Gerät"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/28/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5a143bbf7abb5304ac51782d517c02ec184a05a2
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Bereitstellen von Azure Stream Analytics als IoT Edge-Modul – Vorschau

IoT-Geräte können sehr große Datenmengen erzeugen. Um die Größe der übertragenen Daten zu reduzieren oder die Roundtriplatenz aussagekräftiger Erkenntnisse zu beseitigen, müssen diese Daten in bestimmten Fällen analysiert oder verarbeitet werden, bevor sie die Cloud erreichen.

IoT Edge profitiert von vorgefertigten Azure Service IoT Edge-Modulen für eine schnelle Bereitstellung, und [Azure Stream Analytics][azure-stream] (ASA) ist ein solches Modul. Sie können über das Portal einen ASA-Auftrag erstellen und anschließend zum IoT Hub-Portal wechseln, um es als IoT Edge-Modul bereitzustellen.  

Azure Stream Analytics bietet eine aufwändig strukturierte Abfragesyntax für die Datenanalyse in der Cloud und auf IoT Edge-Geräten. Weitere Informationen zu ASA auf IoT Edge finden Sie in der [ASA-Dokumentation](../stream-analytics/stream-analytics-edge.md).

Dieses Tutorial leitet Sie durch die Erstellung eines Azure Stream Analytics-Auftrags und seine Bereitstellung auf einem IoT Edge-Gerät, damit ein lokaler Telemetriestream direkt auf dem Gerät verarbeitet werden kann und Warnungen erzeugt werden, mit denen Sofortmaßnahmen auf dem Gerät ausgelöst werden können.  In diesem Tutorial geht es um zwei Module: Ein simuliertes Temperatursensormodul (tempSensor), das Temperaturdaten zwischen 20 und 120 Grad erzeugt, die alle 5 Sekunden um den Wert 1 erhöht werden Ein Stream Analytics-Modul, das tempSensor zurücksetzt, sobald über 30 Sekunden ein Durchschnittswert von 70 erreicht wird. In einer Produktionsumgebung kann diese Funktion dazu verwendet werden, einen Computer herunterzufahren oder vorbeugende Maßnahmen zu ergreifen, wenn die Temperatur eine gefährliche Höhe erreicht. 

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines ASA-Auftrags zum Verarbeiten von Daten auf dem Edge-Gerät
> * Verbinden des neuen ASA-Auftrags mit anderen IoT Edge-Modulen
> * Bereitstellen des ASA-Auftrags auf einem IoT Edge-Gerät

## <a name="prerequisites"></a>Voraussetzungen

* IoT Hub 
* Das Gerät, das Sie im Schnellstart oder im Abschnitt „Bereitstellen von Azure IoT Edge“ erstellt und konfiguriert haben, oder ein simuliertes Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] Sie müssen den Geräteverbindungsschlüssel und die Geräte-ID kennen. 
* Docker auf dem IoT Edge-Gerät
    * [Installieren von Docker unter Windows][lnk-docker-windows]
    * [Installieren von Docker unter Linux][lnk-docker-linux]
* Python 2.7.x auf Ihrem IoT Edge-Gerät
    * [Installieren Sie Python 2.7 unter Windows][lnk-python].
    * In den meisten Linux-Distributionen, einschließlich Ubuntu, ist Python 2.7 bereits installiert.  Führen Sie den folgenden Befehl aus, um zu überprüfen, ob pip installiert ist: `sudo apt-get install python-pip`.


## <a name="create-an-asa-job"></a>Erstellen eines ASA-Auftrags

In diesem Abschnitt erstellen Sie einen Azure Stream Analytics-Auftrag, um Daten aus Ihrem IoT Hub abzurufen, die gesendeten Telemetriedaten bei Ihrem Gerät abzufragen und die Ergebnisse an einen Azure Storage-Container (Blob) weiterzuleiten. Weitere Informationen finden Sie im Abschnitt **Übersicht** der [Stream Analytics-Dokumentation][azure-stream]. 

### <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.

Ein Azure Storage-Konto ist erforderlich, um einen Endpunkt anzugeben, der als Ausgabe in Ihrem ASA-Auftrag verwendet wird. Im folgenden Beispiel wird der Speichertyp Blob verwendet.  Weitere Informationen finden Sie im Abschnitt **Blobs** in der [Azure Storage-Dokumentation][azure-storage].

1. Navigieren Sie im Azure-Portal zu **Ressource erstellen**, und geben Sie `Storage account` in die Suchleiste ein. Wählen Sie **Speicherkonto – Blob, Datei, Tabelle, Warteschlange**.

2. Geben Sie einen Namen für Ihr Speicherkonto ein, und wählen Sie den Speicherort aus, in dem sich auch Ihr IoT Hub befindet. Klicken Sie auf **Erstellen**. Notieren Sie sich den Namen für die spätere Verwendung.

    ![Neues Speicherkonto][1]

3. Navigieren Sie zum gerade erstellten Speicherkonto. Klicken Sie auf **Blobs durchsuchen**. 
4. Erstellen Sie einen neuen Container für das ASA-Modul, um Daten zu speichern. Legen Sie als Zugriffsebene **Container** fest. Klicken Sie auf **OK**.

    ![Speichereinstellungen][10]

### <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

1. Navigieren Sie im Azure-Portal zu **Ressourcen erstellen** > **Internet der Dinge**, und wählen Sie **Stream Analytics-Auftrag** aus.

2. Geben Sie einen Namen ein, wählen Sie **Edge** als Hostingumgebung aus, und behalten Sie die übrigen Standardwerte bei.  Klicken Sie auf **Erstellen**.

    >[!NOTE]
    >Aktuell werden ASA-Aufträge für IoT Edge in der Region „USA, Westen 2“ nicht unterstützt. 

3. Wechseln Sie zum erstellten Auftrag. Klicken Sie auf **Eingaben** und dann auf **Hinzufügen**.

4. Geben Sie als Eingabealias `temperature` ein, legen Sie für den Quelltyp **Datenstrom** fest, und behalten Sie die Standardeinstellungen für die anderen Parameter bei. Klicken Sie auf **Erstellen**.

   ![ASA-Eingabe](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Klicken Sie auf **Ausgaben** und dann auf **Hinzufügen**.

6. Geben Sie als Ausgabealias `alert` ein, und behalten Sie die Standardeinstellungen für die anderen Parameter bei. Klicken Sie auf **Erstellen**.

   ![ASA-Ausgabe](./media/tutorial-deploy-stream-analytics/asa_output.png)


7. Wählen Sie **Abfrage**.
8. Ersetzen Sie den Standardtext durch die folgende Abfrage:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```
9. Klicken Sie auf **Speichern**.

## <a name="deploy-the-job"></a>Bereitstellen des Auftrags

Sie können den ASA-Auftrag jetzt auf Ihrem IoT Edge-Gerät bereitstellen.

1. Wechseln Sie im Azure-Portal in Ihrem IoT-Hub zu **IoT Edge (Vorschau)**, und öffnen Sie die Detailseite für Ihr IoT Edge-Gerät.
1. Wählen Sie **Module festlegen** aus.
1. Wenn Sie zuvor das tempSensor-Modul auf diesem Gerät bereitgestellt haben, wird es unter Umständen automatisch aufgefüllt. Wenn dies nicht der Fall ist, fügen Sie dieses Modul mit den folgenden Schritten hinzu:
   1. Klicken Sie auf **IoT Edge-Modul hinzufügen**.
   1. Geben Sie als Name `tempSensor` und als Image-URI `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` ein. 
   1. Behalten Sie die restlichen Einstellungen unverändert bei, und klicken Sie auf **Speichern**.
1. Klicken Sie zum Hinzufügen des ASA-Edge-Auftrags auf **Import Azure Stream Analytics IoT Edge Module** (Azure Stream Analytics-IoT Edge-Modul importieren).
1. Wählen Sie das Abonnement und den erstellten ASA-Edge-Auftrag aus. 
1. Wählen Sie das Abonnement und das erstellte Speicherkonto aus. Klicken Sie auf **Speichern**.

    ![Modul festlegen][6]

1. Kopieren Sie den Namen, der automatisch für Ihr ASA-Modul generiert wurde. 

    ![Temperaturmodul][11]

1. Klicken Sie auf **Weiter**, um Routen zu konfigurieren.
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

1. Kehren Sie zur Seite mit den Gerätedetails zurück, und klicken Sie auf **Aktualisieren**.  Es sollte nun angezeigt werden, dass das neue Stream Analytics-Modul zusammen mit dem **IoT Edge-Agent**-Modul und dem **IoT Edge-Hub** ausgeführt wird.

    ![Modulausgabe][7]

## <a name="view-data"></a>Anzeigen von Daten

Wechseln Sie nun zu Ihrem IoT Edge-Gerät, um sich über die Interaktion zwischen dem ASA-Modul und dem tempSensor-Modul zu informieren.

Überprüfen Sie, ob alle Module in Docker ausgeführt werden:

   ```cmd/sh
   docker ps  
   ```

   ![Docker-Ausgabe][8]

Zeigen Sie alle Systemprotokolle und Metrikdaten an. Verwenden Sie den Namen des Stream Analytics-Moduls:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

Sie sollten beobachten können, wie die Temperatur des Computers allmählich steigt, bis sie 30 Sekunden lang 70 Grad beträgt. Dann löst das Stream Analytics-Modul eine Zurücksetzung aus, und die Computertemperatur fällt zurück auf 21. 

   ![Docker-Protokoll][9]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Storage-Container und einen Streaming Analytics-Auftrag zum Analysieren von Daten konfiguriert, die von ihrem IoT Edge-Gerät stammen.  Danach haben Sie ein benutzerdefiniertes ASA-Modul geladen, um Daten von Ihrem Gerät über den Datenstrom in ein Blob zu verschieben, das heruntergeladen werden kann.  Sie können nun weitere Tutorials bearbeiten, um noch mehr darüber zu erfahren, wie sich mit Azure IoT Edge Lösungen für Ihr Unternehmen erstellen lassen.

> [!div class="nextstepaction"] 
> [Bereitstellen eines Azure Machine Learning-Modells als Modul][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
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