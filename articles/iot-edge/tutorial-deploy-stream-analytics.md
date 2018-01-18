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
ms.openlocfilehash: f40fb81fc03e796b906db12bf3bf6904b27b46eb
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Bereitstellen von Azure Stream Analytics als IoT Edge-Modul – Vorschau

IoT-Geräte können sehr große Datenmengen erzeugen. Um den Umfang der übertragenen Daten zu reduzieren oder die Roundtriplatenz aussagekräftiger Erkenntnisse zu beseitigen, müssen diese Daten in bestimmten Fällen analysiert oder verarbeitet werden, bevor sie die Cloud erreichen.

Azure IoT Edge profitiert von vorgefertigten Azure Service IoT Edge-Modulen für eine schnelle Bereitstellung. [Azure Stream Analytics][azure-stream] (ASA) ist ein solches Modul. Sie können über das Portal einen Azure Stream Analytics-Auftrag erstellen und anschließend zum Azure IoT Hub-Portal wechseln, um es als IoT Edge-Modul bereitzustellen. 

Azure Stream Analytics bietet eine aufwändig strukturierte Abfragesyntax für die Datenanalyse in der Cloud und auf IoT Edge-Geräten. Weitere Informationen zu Azure Stream Analytics auf IoT Edge finden Sie in der [Azure Stream Analytics-Dokumentation](../stream-analytics/stream-analytics-edge.md).

Dieses Tutorial führt Sie durch die Erstellung eines Azure Stream Analytics-Auftrags und dessen Bereitstellung auf einem IoT Edge-Gerät. Auf diese Weise können Sie einen lokalen Telemetriedatenstrom direkt auf dem Gerät verarbeiten und Warnungen generieren, die eine sofortige Aktion auf dem Gerät auslösen. 

Das Tutorial präsentiert zwei Module: 
* Ein simuliertes Temperatursensormodul (tempSensor), das Temperaturdaten zwischen 20 und 120 Grad erzeugt, die alle 5 Sekunden um den Wert 1 erhöht werden. 
* Ein Stream Analytics-Modul, das „tempSensor“ zurücksetzt, sobald über 30 Sekunden ein Durchschnittswert von 70 erreicht wird. In einer Produktionsumgebung kann diese Funktion dazu verwendet werden, einen Computer herunterzufahren oder vorbeugende Maßnahmen zu ergreifen, wenn die Temperatur eine gefährliche Höhe erreicht. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Stream Analytics-Auftrags zum Verarbeiten von Daten auf dem Edge-Gerät.
> * Verbinden des neuen Azure Stream Analytics-Auftrags mit anderen IoT Edge-Modulen.
> * Bereitstellen des Azure Stream Analytics-Auftrags auf einem IoT Edge-Gerät.

## <a name="prerequisites"></a>Voraussetzungen

* Einen IoT Hub. 
* Das Gerät, das Sie im Schnellstart oder in den Artikeln zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] erstellt und konfiguriert haben. Sie müssen den Geräteverbindungsschlüssel und die Geräte-ID kennen. 
* Docker auf dem IoT Edge-Gerät.
    * [Installieren von Docker unter Windows][lnk-docker-windows]
    * [Installieren von Docker unter Linux][lnk-docker-linux]
* Python 2.7.x auf Ihrem IoT Edge-Gerät
    * [Installieren Sie Python 2.7 unter Windows][lnk-python].
    * In den meisten Linux-Distributionen, einschließlich Ubuntu, ist Python 2.7 bereits installiert. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob pip installiert ist: `sudo apt-get install python-pip`.

## <a name="create-an-azure-stream-analytics-job"></a>Erstellen eines Azure Stream Analytics-Auftrags

In diesem Abschnitt erstellen Sie einen Azure Stream Analytics-Auftrag, um Daten aus Ihrem IoT Hub abzurufen, die gesendeten Telemetriedaten bei Ihrem Gerät abzufragen und die Ergebnisse an einen Azure Blob Storage-Container weiterzuleiten. Weitere Informationen finden Sie im Abschnitt „Übersicht“ der [Stream Analytics-Dokumentation][azure-stream]. 

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Ein Azure Storage-Konto ist erforderlich, um einen Endpunkt anzugeben, der als Ausgabe in Ihrem Azure Stream Analytics-Auftrag verwendet wird. Das Beispiel in diesem Abschnitt verwendet den Speichertyp „Blob“. Weitere Informationen finden Sie im Abschnitt „Blobs“ in der [Azure Storage-Dokumentation][azure-storage].

1. Wechseln Sie im Azure-Portal zu **Ressource erstellen**, geben Sie **Speicherkonto** in das Suchfeld ein und wählen Sie **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.

2. Geben Sie im Bereich **Speicherkonto erstellen** einen Namen für Ihr Speicherkonto ein, wählen Sie den gleichen Speicherort aus, in dem Ihr IoT Hub gespeichert ist, und wählen Sie dann **Erstellen** aus. Notieren Sie sich den Namen für die spätere Verwendung.

    ![Speicherkonto erstellen][1]

3. Wechseln Sie zu dem Speicherkonto, das Sie soeben erstellt haben, und wählen Sie **Blobs durchsuchen** aus. 

4. Erstellen Sie einen neuen Container für das Azure Stream Analytics-Modul, um Daten zu speichern, setzen Sie die Zugriffsebene auf **Container** und wählen Sie dann **OK** aus.

    ![Speichereinstellungen][10]

### <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

1. Navigieren Sie im Azure-Portal zu **Ressource erstellen** > **Internet der Dinge**, und wählen Sie dann **Stream Analytics-Auftrag** aus.

2. Gehen Sie im Bereich **Neuer Stream Analytics-Auftrag** wie folgt vor:

    a. Geben Sie im Feld **Auftragsname** einen Auftragsnamen ein.
    
    b. Wählen Sie unter **Hosting-Umgebung** die Option **Edge** aus.
    
    c. Verwenden Sie in den verbleibenden Feldern die Standardwerte.

    > [!NOTE]
    > Aktuell werden Azure Stream Analytics-Aufträge für IoT Edge in der Region „USA, Westen 2“ nicht unterstützt. 

3. Klicken Sie auf **Erstellen**.

4. Wählen Sie im erstellten Auftrag unter **Auftragstopologie** die Option **Eingaben** und dann **Hinzufügen** aus.

5. Gehen Sie im Bereich **Neue Eingabe** folgendermaßen vor:

    a. Geben Sie im Feld **Eingabealias** den Wert **Temperatur** ein.
    
    b. Wählen Sie im Feld **Quelltyp** die Option **Datenstrom** aus.
    
    c. Verwenden Sie in den verbleibenden Feldern die Standardwerte.

   ![Azure Stream Analytics-Eingabe](./media/tutorial-deploy-stream-analytics/asa_input.png)

6. Klicken Sie auf **Erstellen**.

7. Wählen Sie unter **Auftragstopologie** den Eintrag **Ausgaben** und dann **Hinzufügen** aus.

8. Gehen Sie im Bereich **Neue Ausgabe** folgendermaßen vor:

    a. Geben Sie im Feld **Ausgabealias** den Wert **Warnung** ein.
    
    b. Verwenden Sie in den verbleibenden Feldern die Standardwerte. 
    
    c. Klicken Sie auf **Erstellen**.

   ![Azure Stream Analytics-Ausgabe](./media/tutorial-deploy-stream-analytics/asa_output.png)


9. Wählen Sie unter **Auftragstopologie** die Option **Abfrage** aus und ersetzen Sie dann den Standardtext durch die folgende Abfrage:

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

10. Wählen Sie **Speichern** aus.

## <a name="deploy-the-job"></a>Bereitstellen des Auftrags

Sie können den Azure Stream Analytics-Auftrag jetzt auf Ihrem IoT Edge-Gerät bereitstellen.

1. Wechseln Sie im Azure-Portal in Ihrem IoT-Hub zu **IoT Edge (Vorschau)**, und öffnen Sie dann die Detailseite für Ihr IoT Edge-Gerät.

2. Wählen Sie **Module festlegen** aus.  
    Wenn Sie zuvor das tempSensor-Modul auf diesem Gerät bereitgestellt haben, wird es unter Umständen automatisch aufgefüllt. Wenn dies nicht der Fall ist, fügen Sie das Modul wie folgt hinzu:

   a. Wählen Sie **IoT Edge-Modul hinzufügen** aus.

   b. Geben Sie für den Namen **tempSensor** ein.
    
   c. Geben Sie für den Bild-URI **microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview** ein. 

   d. Behalten Sie die restlichen Einstellungen unverändert bei.
   
   e. Wählen Sie **Speichern** aus.

3. Klicken Sie zum Hinzufügen des Azure Stream Analytics Edge-Auftrags auf **Import Azure Stream Analytics IoT Edge Module** (Azure Stream Analytics-IoT Edge-Modul importieren).

4. Wählen Sie das Abonnement und den erstellten Azure Stream Analytics Edge-Auftrag aus. 

5. Wählen Sie das Abonnement, das erstellte Speicherkonto und dann **Speichern** aus.

    ![Modul festlegen][6]

6. Kopieren Sie den Namen Ihres Azure Stream Analytics-Moduls. 

    ![Temperaturmodul][11]

7. Wählen Sie **Weiter** aus, um Routen zu konfigurieren.

8. Kopieren Sie den folgenden Code zu **Routen**: Ersetzen Sie _{moduleName}_ durch den zuvor kopierten Modulnamen:

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

9. Klicken Sie auf **Weiter**.

10. Wählen Sie im Schritt **Vorlage überprüfen** die Option **Senden** aus.

11. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie dann **Aktualisieren** aus.  
    Es sollte nun angezeigt werden, dass das neue Stream Analytics-Modul zusammen mit dem IoT Edge-Agent-Modul und dem IoT Edge-Hub ausgeführt wird.

    ![Modulausgabe][7]

## <a name="view-data"></a>Anzeigen von Daten

Wechseln Sie nun zu Ihrem IoT Edge-Gerät, um sich über die Interaktion zwischen dem Azure Stream Analytics-Modul und dem tempSensor-Modul zu informieren.

1. Überprüfen Sie, ob alle Module in Docker ausgeführt werden:

   ```cmd/sh
   docker ps  
   ```

   ![Docker-Ausgabe][8]

2. Zeigen Sie alle Systemprotokolle und Metrikdaten an. Verwenden Sie den Namen des Stream Analytics-Moduls:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

Sie sollten beobachten können, wie die Temperatur des Computers allmählich steigt, bis sie 30 Sekunden lang 70 Grad beträgt. Dann löst das Stream Analytics-Modul eine Zurücksetzung aus, und die Computertemperatur fällt zurück auf 21. 

   ![Docker-Protokoll][9]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Storage-Container und einen Streaming Analytics-Auftrag zum Analysieren von Daten konfiguriert, die von ihrem IoT Edge-Gerät stammen. Danach haben Sie ein benutzerdefiniertes Azure Stream Analytics-Modul geladen, um Daten von Ihrem Gerät über den Datenstrom in ein Blob zu verschieben, das heruntergeladen werden kann. Sie können nun die anderen Tutorials bearbeiten, um noch mehr darüber zu erfahren, wie sich mit Azure IoT Edge weitere Lösungen für Ihr Unternehmen erstellen lassen.

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
[azure-iot]: https://docs.microsoft.com/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/azure/storage/
[azure-stream]: https://docs.microsoft.com/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/
