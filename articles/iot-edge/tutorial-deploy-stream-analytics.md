---
title: 'Tutorial: Bereitstellen von ASA-Aufträgen auf Azure IoT Edge-Geräten | Microsoft-Dokumentation'
description: Bereitstellen von Azure Stream Analytics als Modul auf einem IoT Edge-Gerät
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/25/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: ad2895a457a20632823260f2429ac95fad82089c
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060194"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Tutorial: Bereitstellen von Azure Stream Analytics als IoT Edge-Modul (Vorschauversion)

Viele IoT-Lösungen verwenden Analysedienste, um Erkenntnisse zu Daten zu gewinnen, die von den IoT-Geräten an die Cloud übermittelt werden. Mit Azure IoT Edge können Sie die Logik von [Azure Stream Analytics][azure-stream] direkt auf dem Gerät bereitstellen. Die Verarbeitung von Telemetriedatenströmen im Edgebereich verringert die Menge an Uploaddaten sowie die Zeit, die benötigt wird, um auf verwertbare Erkenntnisse zu reagieren.

Dank der Integration von Azure IoT Edge und Azure Stream Analytics können Sie über das Azure-Portal einen Azure Stream Analytics-Auftrag erstellen und ihn anschließend ohne zusätzlichen Code als IoT-Edge-Modul bereitstellen.  

Azure Stream Analytics bietet eine aufwändig strukturierte Abfragesyntax für die Datenanalyse in der Cloud und auf IoT Edge-Geräten. Weitere Informationen zu Azure Stream Analytics auf IoT Edge finden Sie in der [Azure Stream Analytics-Dokumentation](../stream-analytics/stream-analytics-edge.md).

Das Stream Analytics-Modul in diesem Tutorial berechnet die Durchschnittstemperatur der letzten 30 Sekunden. Bei Erreichen einer Durchschnittstemperatur von 70 Grad sendet das Modul eine Warnung, damit das Gerät eine entsprechende Aktion ausführen kann. In diesem Fall wird der simulierte Temperatursensor zurückgesetzt. In einer Produktionsumgebung kann diese Funktion dazu verwendet werden, einen Computer herunterzufahren oder vorbeugende Maßnahmen zu ergreifen, wenn die Temperatur eine gefährliche Höhe erreicht. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Stream Analytics-Auftrags zum Verarbeiten von Daten auf dem Edge-Gerät.
> * Verbinden des neuen Azure Stream Analytics-Auftrags mit anderen IoT Edge-Modulen.
> * Bereitstellen des Azure Stream Analytics-Auftrags auf einem IoT Edge-Gerät über das Azure-Portal.

>[!NOTE]
>Azure Stream Analytics-Module für IoT Edge sind als [Public Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

* Ein IoT Hub
* Das IoT Edge-Gerät, das Sie im Rahmen der Schnellstartanleitung für [Windows][lnk-quickstart-win] oder [Linux][lnk-quickstart-lin] erstellt und konfiguriert haben 

## <a name="create-an-azure-stream-analytics-job"></a>Erstellen eines Azure Stream Analytics-Auftrags

In diesem Abschnitt erstellen Sie einen Azure Stream Analytics-Auftrag, um Daten aus Ihrem IoT Hub abzurufen, die gesendeten Telemetriedaten bei Ihrem Gerät abzufragen und die Ergebnisse an einen Azure Blob Storage-Container weiterzuleiten. Weitere Informationen finden Sie im Abschnitt „Übersicht“ der [Stream Analytics-Dokumentation][azure-stream]. 

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Für Azure Stream Analytics-Aufträge ist ein Azure Storage-Konto erforderlich, das als Endpunkt für die Auftragsausgabe fungiert. Das Beispiel in diesem Abschnitt verwendet den Speichertyp „Blob“. Weitere Informationen finden Sie im Abschnitt „Blobs“ in der [Azure Storage-Dokumentation][azure-storage].

1. Wechseln Sie im Azure-Portal zu **Ressource erstellen**, geben Sie **Speicherkonto** in das Suchfeld ein und wählen Sie **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.

2. Geben Sie im Bereich **Speicherkonto erstellen** einen Namen für Ihr Speicherkonto ein, und wählen Sie den gleichen Speicherort aus, in dem Ihr IoT Hub gespeichert ist. Wählen Sie die gleiche Ressourcengruppe wie für Ihren IoT Hub aus, und wählen Sie dann **Erstellen**. Notieren Sie sich den Namen für die spätere Verwendung.

    ![Speicherkonto erstellen][1]


### <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

1. Navigieren Sie im Azure-Portal zu **Ressource erstellen** > **Internet der Dinge**, und wählen Sie dann **Stream Analytics-Auftrag** aus.

2. Gehen Sie im Bereich **Neuer Stream Analytics-Auftrag** wie folgt vor:

   1. Geben Sie im Feld **Auftragsname** einen Auftragsnamen ein.
   
   2. Verwenden Sie die gleiche **Ressourcengruppe** und den gleichen **Standort** wie für Ihre IoT Hub-Instanz. 

      > [!NOTE]
      > Aktuell werden Azure Stream Analytics-Aufträge für IoT Edge in der Region „USA, Westen 2“ nicht unterstützt. 

   3. Wählen Sie unter **Hosting-Umgebung** die Option **Edge** aus.
    
3. Klicken Sie auf **Erstellen**.

4. Öffnen Sie im erstellten Auftrag unter **Auftragstopologie** die Option **Eingaben**.

   ![Azure Stream Analytics-Eingabe](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Wählen Sie **Datenstromeingabe hinzufügen** und dann **Edge Hub** aus.

6. Geben Sie im Bereich **Neue Eingabe** die **Temperatur** als Eingabealias ein. 

7. Wählen Sie **Speichern**aus.

8. Öffnen Sie unter **Auftragstopologie** die Option **Ausgaben**.

   ![Azure Stream Analytics-Ausgabe](./media/tutorial-deploy-stream-analytics/asa_output.png)

9. Wählen Sie **Hinzufügen** und dann **Edge Hub** aus.

10. Geben Sie im Bereich **Neue Ausgabe** die **Warnung** als Ausgabealias ein. 

11. Wählen Sie **Speichern**aus.

12. Klicken Sie unter **Auftragstopologie** auf **Abfrage**, und ersetzen Sie den Standardtext durch die folgende Abfrage, die eine Warnung erstellt, wenn die Durchschnittstemperatur des Computers in einem Zeitfenster von 30 Sekunden 70 Grad erreicht:

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

13. Wählen Sie **Speichern**aus.

14. Klicken Sie unter **Konfigurieren** auf **IoT Edge-Einstellungen**.

15. Wählen Sie im Dropdownmenü Ihr **Speicherkonto** aus.

16. Klicken Sie für das Feld **Container** auf **Neu erstellen**, und geben Sie einen Namen für den Speichercontainer an. 

17. Wählen Sie **Speichern**aus. 


## <a name="deploy-the-job"></a>Bereitstellen des Auftrags

Sie können den Azure Stream Analytics-Auftrag jetzt auf Ihrem IoT Edge-Gerät bereitstellen.

1. Wechseln Sie im Azure-Portal in Ihrem IoT-Hub zu **IoT Edge**, und öffnen Sie dann die Detailseite für Ihr IoT Edge-Gerät.

2. Wählen Sie **Module festlegen** aus.  

   Wenn Sie zuvor das tempSensor-Modul auf diesem Gerät bereitgestellt haben, wird es unter Umständen automatisch aufgefüllt. Gehen Sie andernfalls wie folgt vor, um das Modul hinzuzufügen:

   1. Klicken Sie auf **Hinzufügen** und anschließend auf **IoT Edge-Modul**.
   2. Geben Sie als Name **tempsensor** ein.
   3. Geben Sie als Bild-URI **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** ein. 
   4. Behalten Sie die restlichen Einstellungen unverändert bei.
   5. Wählen Sie **Speichern**aus.

3. Fügen Sie Ihren Azure Stream Analytics Edge-Auftrag wie folgt hinzu:

   1. Klicken Sie auf **Hinzufügen** und anschließend auf **Azure Stream Analytics-Modul**.
   2. Wählen Sie das Abonnement und den erstellten Azure Stream Analytics Edge-Auftrag aus. 
   3. Wählen Sie **Speichern**aus.

4. Klicken Sie auf **Weiter**.

5. Ersetzen Sie den Standardwert in **Routen** durch den folgenden Code. Aktualisieren Sie _{moduleName}_ mit dem Namen Ihres Azure Stream Analytics-Moduls. Das Modul muss den gleichen Namen haben wie der Auftrag, auf dessen Grundlage es erstellt wurde. 

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

6. Klicken Sie auf **Weiter**.

7. Klicken Sie im Schritt für die **Bereitstellungsüberprüfung** auf **Übermitteln**.

8. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie dann **Aktualisieren** aus.  

    Es sollte nun angezeigt werden, dass das neue Stream Analytics-Modul zusammen mit dem IoT Edge-Agent-Modul und dem IoT Edge-Hub ausgeführt wird.

    ![Modulausgabe][7]

## <a name="view-data"></a>Anzeigen von Daten

Wechseln Sie nun zu Ihrem IoT Edge-Gerät, um sich über die Interaktion zwischen dem Azure Stream Analytics-Modul und dem tempSensor-Modul zu informieren.

1. Überprüfen Sie, ob alle Module in Docker ausgeführt werden:

   ```cmd/sh
   iotedge list  
   ```
<!--
   ![Docker output][8]
-->
2. Zeigen Sie alle Systemprotokolle und Metrikdaten an. Verwenden Sie den Namen des Stream Analytics-Moduls:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

Sie sollten beobachten können, wie die Temperatur des Computers allmählich steigt, bis sie 30 Sekunden lang 70 Grad beträgt. Dann löst das Stream Analytics-Modul eine Zurücksetzung aus, und die Computertemperatur fällt zurück auf 21. 

   ![Docker-Protokoll][9]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die bereits erstellten Ressourcen und Konfigurationen behalten und wiederverwenden.

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 

> [!IMPORTANT]
> Das Löschen der Azure-Ressourcen und der Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. Wenn Sie die IoT Hub-Ressource in einer bereits vorhandenen Ressourcengruppe erstellt haben, die Ressourcen enthält, die Sie behalten möchten, löschen Sie nicht die Ressourcengruppe, sondern nur die IoT Hub-Ressource.
>

Wenn Sie nur die IoT Hub-Instanz löschen möchten, führen Sie den folgenden Befehl mit dem Namen des Hubs und dem Namen der Ressourcengruppe aus:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


So löschen Sie die gesamte Ressourcengruppe anhand des Namens:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.

2. Geben Sie im Textfeld **Nach Name filtern...** den Namen der Ressourcengruppe ein, die Ihre IoT Hub-Ressource enthält. 

3. Klicken Sie in der Ergebnisliste rechts neben Ihrer Ressourcengruppe auf **...** und dann auf **Ressourcengruppe löschen**.

<!--
   ![Delete](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)
-->
4. Sie werden aufgefordert, das Löschen der Ressourcengruppe zu bestätigen. Geben Sie zur Bestätigung erneut den Namen Ihrer Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**. Daraufhin werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Streaming Analytics-Auftrag zum Analysieren von Daten konfiguriert, die von ihrem IoT Edge-Gerät stammen. Anschließend haben Sie das Azure Stream Analytics-Modul auf Ihrem IoT Edge-Gerät geladen, um die Daten lokal zu verarbeiten, lokal auf einen Temperaturanstieg zu reagieren und den aggregierten Datenstrom an die Cloud zu senden. Sie können nun die anderen Tutorials bearbeiten, um noch mehr darüber zu erfahren, wie sich mit Azure IoT Edge weitere Lösungen für Ihr Unternehmen erstellen lassen.

> [!div class="nextstepaction"] 
> [Bereitstellen eines Azure Machine Learning-Modells als Modul][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output2.png
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
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

