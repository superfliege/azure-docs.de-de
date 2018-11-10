---
title: 'Tutorial: Bereitstellen von ASA-Aufträgen auf Azure IoT Edge-Geräten | Microsoft-Dokumentation'
description: In diesem Tutorial stellen Sie Azure Stream Analytics als Modul auf einem IoT Edge-Gerät bereit.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: b85fe520bf29d10bb3dac1246349abbdadaf6df3
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158736"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module-preview"></a>Tutorial: Bereitstellen von Azure Stream Analytics als IoT Edge-Modul (Vorschauversion)

Viele IoT-Lösungen verwenden Analysedienste, um Erkenntnisse zu Daten zu gewinnen, die von den IoT-Geräten an die Cloud übermittelt werden. Mit Azure IoT Edge können Sie die Logik von [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) direkt auf dem Gerät bereitstellen. Die Verarbeitung von Telemetriedatenströmen im Edgebereich verringert die Menge an Uploaddaten sowie die Zeit, die benötigt wird, um auf verwertbare Erkenntnisse zu reagieren.

Dank der Integration von Azure IoT Edge und Azure Stream Analytics können Sie über das Azure-Portal einen Azure Stream Analytics-Auftrag erstellen und ihn anschließend ohne zusätzlichen Code als IoT-Edge-Modul bereitstellen.  

Azure Stream Analytics bietet eine aufwändig strukturierte Abfragesyntax für die Datenanalyse in der Cloud und auf IoT Edge-Geräten. Weitere Informationen zu Azure Stream Analytics auf IoT Edge finden Sie in der [Azure Stream Analytics-Dokumentation](../stream-analytics/stream-analytics-edge.md).

Das Stream Analytics-Modul in diesem Tutorial berechnet die Durchschnittstemperatur der letzten 30 Sekunden. Bei Erreichen einer Durchschnittstemperatur von 70 Grad sendet das Modul eine Warnung, damit das Gerät eine entsprechende Aktion ausführen kann. In diesem Fall wird der simulierte Temperatursensor zurückgesetzt. In einer Produktionsumgebung kann diese Funktion dazu verwendet werden, einen Computer herunterzufahren oder vorbeugende Maßnahmen zu ergreifen, wenn die Temperatur eine gefährliche Höhe erreicht. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Stream Analytics-Auftrags zum Verarbeiten von Daten auf dem Edge-Gerät.
> * Verbinden des neuen Azure Stream Analytics-Auftrags mit anderen IoT Edge-Modulen.
> * Bereitstellen des Azure Stream Analytics-Auftrags auf einem IoT Edge-Gerät über das Azure-Portal.

<center>
![Tutorial: Architekturdiagramm](./media/tutorial-deploy-stream-analytics/ASATutorialDiagram.png)
</center>

>[!NOTE]
>Azure Stream Analytics-Module für IoT Edge sind als [Public Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) verfügbar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

* Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als Edge-Gerät verwenden, indem Sie die Schritte ausführen, die in der Schnellstartanleitung für [Linux](quickstart-linux.md)- oder [Windows](quickstart.md)-Geräte beschrieben sind.

Cloudressourcen:

* Eine [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-Instanz in Azure im Tarif „Free“ oder „Standard“. 


## <a name="create-an-azure-stream-analytics-job"></a>Erstellen eines Azure Stream Analytics-Auftrags

In diesem Abschnitt erstellen Sie einen Azure Stream Analytics-Auftrag, um Daten aus Ihrem IoT Hub abzurufen, die gesendeten Telemetriedaten bei Ihrem Gerät abzufragen und die Ergebnisse an einen Azure Blob Storage-Container weiterzuleiten. 

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Wenn Sie einen Azure Stream Analytics-Auftrag zur Ausführung auf einem IoT Edge-Gerät erstellen, muss er so gespeichert werden, dass er vom Gerät aufgerufen werden kann. Sie können ein vorhandenes Azure-Speicherkonto auswählen oder jetzt ein neues erstellen. 

1. Rufen Sie im Azure-Portal **Ressource erstellen** > **Speicher** > **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** auf. 

1. Geben Sie die folgenden Werte an, um Ihr Speicherkonto zu erstellen:

   | Feld | Wert |
   | ----- | ----- |
   | NAME | Geben Sie einen eindeutigen Namen für Ihr Speicherkonto an. | 
   | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. |
   | Abonnement | Wählen Sie das gleiche Abonnement wie für Ihren IoT Hub. |
   | Ressourcengruppe | Es wird empfohlen, die gleiche Ressourcengruppe für alle Testressourcen zu verwenden, die Sie während der IoT Edge-Schnellstarts und -Tutorials erstellen. Beispielsweise **IoTEdgeResources**. |

1. Behalten Sie in den restlichen Feldern die Standardwerte bei, und wählen Sie **Erstellen**. 

### <a name="create-a-new-job"></a>Erstellen eines neuen Auftrags

1. Wechseln Sie im Azure-Portal zu **Ressource erstellen** > **Internet der Dinge** > **Stream Analytics-Auftrag**.

1. Geben Sie die folgenden Werte an, um Ihren Auftrag zu erstellen:

   | Feld | Wert |
   | ----- | ----- |
   | Auftragsname | Geben Sie einen Namen für Ihren Auftrag an. Beispielsweise **IoTEdgeJob** | 
   | Abonnement | Wählen Sie das gleiche Abonnement wie für Ihren IoT Hub. |
   | Ressourcengruppe | Es wird empfohlen, die gleiche Ressourcengruppe für alle Testressourcen zu verwenden, die Sie während der IoT Edge-Schnellstarts und -Tutorials erstellen. Beispielsweise **IoTEdgeResources**. |
   | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. | 
   | Hosting-Umgebung | Wählen Sie **Edge** aus. |
 
1. Klicken Sie auf **Erstellen**.

### <a name="configure-your-job"></a>Konfigurieren des Auftrags

Nachdem Ihr Stream Analytics-Auftrag im Azure-Portal erstellt wurde, können Sie ihn mit einer Eingabe, einer Ausgabe und einer Abfrage konfigurieren, die für die Daten ausgeführt wird, die er durchläuft. 

Mithilfe der drei Elemente – Eingabe, Ausgabe und Abfrage – wird in diesem Abschnitt ein Auftrag erstellt, der Temperaturdaten vom IoT Edge-Gerät empfängt. Die Daten werden in einem rollierenden Fenster von 30 Sekunden analysiert. Wenn die Durchschnittstemperatur in diesem Fenster auf mehr als 70 Grad ansteigt, wird eine Warnung an das IoT Edge-Gerät gesendet. Im nächsten Abschnitt legen Sie beim Bereitstellen des Auftrags genau fest, woher die Daten stammen und wohin sie gesendet werden.  

1. Navigieren Sie im Azure-Portal zu Ihrem Stream Analytics-Auftrag. 

1. Wählen Sie unter **Auftragstopologie** die Option **Eingaben** und dann **Datenstromeingabe hinzufügen**.

   ![Azure Stream Analytics-Eingabe](./media/tutorial-deploy-stream-analytics/asa_input.png)

1. Wählen Sie in der Dropdownliste die Option **Edge-Hub** aus.

1. Geben Sie im Bereich **Neue Eingabe** die **Temperatur** als Eingabealias ein. 

1. Behalten Sie in den restlichen Feldern die Standardwerte bei, und wählen Sie **Speichern**.

1. Öffnen Sie unter **Auftragstopologie** die Option **Ausgaben**, und wählen Sie dann **Hinzufügen**.

   ![Azure Stream Analytics-Ausgabe](./media/tutorial-deploy-stream-analytics/asa_output.png)

1. Wählen Sie in der Dropdownliste die Option **Edge-Hub** aus.

1. Geben Sie im Bereich **Neue Ausgabe** die **Warnung** als Ausgabealias ein. 

1. Behalten Sie in den restlichen Feldern die Standardwerte bei, und wählen Sie **Speichern**.

1. Wählen Sie unter **Auftragstopologie** die Option **Abfrage** aus.

1. Ersetzen Sie den Standardtext mit der folgenden Abfrage. Der SQL-Code sendet einen Zurücksetzungsbefehl an die Warnungsausgabe, wenn die durchschnittliche Maschinentemperatur in einem 30-sekündigen Fenster 70 Grad erreicht. Der Zurücksetzungsbefehl wurde im Sensor als ausführbare Aktion vorprogrammiert. 

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

1. Wählen Sie **Speichern**aus.

### <a name="configure-iot-edge-settings"></a>Konfigurieren von IoT Edge-Einstellungen

Um Ihren Stream Analytics-Auftrag auf die Bereitstellung als IoT Edge-Gerät vorzubereiten, müssen Sie den Auftrag einem Container in einem Speicherkonto zuordnen. Wenn Sie Ihren Auftrag bereitstellen, wird die Auftragsdefinition in den Speichercontainer extrahiert. 

1. Wählen Sie unter **Konfigurieren** die Option **Speicherkontoeinstellungen** aus.

1. Wählen Sie **Speicherkonto hinzufügen** aus. 

1. Wählen Sie im Dropdownmenü Ihr **Speicherkonto** aus.

1. Klicken Sie für das Feld **Container** auf **Neu erstellen**, und geben Sie einen Namen für den Speichercontainer an. 

1. Wählen Sie **Speichern**aus. 

## <a name="deploy-the-job"></a>Bereitstellen des Auftrags

Sie können den Azure Stream Analytics-Auftrag jetzt auf Ihrem IoT Edge-Gerät bereitstellen. 

In diesem Abschnitt verwenden Sie den Assistenten zum **Festlegen von Modulen** im Azure-Portal zum Erstellen eines *Bereitstellungsmanifests*. Ein Bereitstellungsmanifest ist eine JSON-Datei mit Beschreibungen für: alle Module, die auf einem Gerät bereitgestellt werden, die Containerregistrierungen, welche die Modulimages speichern, die Verwaltung der Module und die Kommunikationsmöglichkeiten für die Module untereinander. Ihr IoT Edge-Gerät ruft das Bereitstellungsmanifest aus IoT Hub ab und verwendet die Informationen darin zur Bereitstellung und Konfiguration aller seiner zugewiesenen Module. 

In diesem Tutorial stellen Sie zwei Module bereit. Das erste ist **tempSensor**, ein Modul, das einen Sensor für Temperatur und Luftfeuchtigkeit simuliert. Das zweite ist Ihr Stream Analytics-Auftrag. Das Sensormodul stellt den Datenstrom bereit, der von Ihrer Auftragsabfrage analysiert wird. 

1. Wechseln Sie im Azure-Portal in Ihrem IoT-Hub zu **IoT Edge**, und öffnen Sie dann die Detailseite für Ihr IoT Edge-Gerät.

1. Wählen Sie **Module festlegen** aus.  

1. Wenn Sie zuvor das tempSensor-Modul auf diesem Gerät bereitgestellt haben, wird es unter Umständen automatisch aufgefüllt. Führen Sie andernfalls die folgenden Schritte aus, um das Modul hinzuzufügen:

   1. Klicken Sie auf **Hinzufügen** und anschließend auf **IoT Edge-Modul**.
   1. Geben Sie für den Namen **tempSensor** ein.
   1. Geben Sie als Bild-URI **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** ein. 
   1. Behalten Sie die restlichen Einstellungen unverändert bei, und wählen Sie **Speichern** aus.

1. Fügen Sie Ihren Azure Stream Analytics Edge-Auftrag wie folgt hinzu:

   1. Klicken Sie auf **Hinzufügen** und anschließend auf **Azure Stream Analytics-Modul**.
   1. Wählen Sie das Abonnement und den erstellten Azure Stream Analytics Edge-Auftrag aus. 
   1. Wählen Sie **Speichern**aus.

1. Nachdem IhreStream Analytics-Auftrag im von Ihnen erstellten Speichercontainer veröffentlicht ist, klicken Sie auf den Modulnamen, um zu sehen, wie ein Stream Analytics-Modul strukturiert ist. 

   Der Image-URI verweist auf ein Azure Stream Analytics-Standardimage. Dieses Image wird für jeden Auftrag verwendet, der auf einem IoT Edge-Gerät bereitgestellt wird. 

   Der Modulzwilling ist mit einer gewünschten Eigenschaft namens **ASAJobInfo** konfiguriert. Der Wert dieser Eigenschaft verweist auf die Auftragsdefinition in Ihrem Speichercontainer. Diese Eigenschaft gibt an, wie das Stream Analytics-Image mit Ihren individuellen Auftragsinformationen konfiguriert wird. 

1. Schließen Sie die Modulseite.

1. Notieren Sie sich den Namen Ihres Stream Analytics-Moduls, da Sie ihn im nächsten Schritt benötigen, und wählen Sie dann **Weiter**, um den Vorgang fortzusetzen.

1. Ersetzen Sie den Standardwert in **Routen** durch den folgenden Code. Aktualisieren Sie alle drei Instanzen von _{moduleName}_ mit dem Namen Ihres Azure Stream Analytics-Moduls. 

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

   Die Routen, die Sie hier deklarieren, definieren den Fluss der Daten durch das IoT Edge-Gerät. Die Telemetriedaten von „tempSensor“ werden an IoT Hub und die **Temperatureingabe** gesendet, die im Stream Analytics-Auftrag konfiguriert wurde. Die Meldungen der **Warnungsausgabe** werden an IoT Hub und das tempSensor-Modul gesendet, um den Zurücksetzungsbefehl auszulösen. 

1. Klicken Sie auf **Weiter**.

1. Klicken Sie im Schritt für die **Bereitstellungsüberprüfung** auf **Übermitteln**.

1. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie dann **Aktualisieren** aus.  

    Es sollte nun angezeigt werden, dass das neue Stream Analytics-Modul zusammen mit dem IoT Edge-Agent-Modul und dem IoT Edge-Hub ausgeführt wird.

    ![Modulausgabe](./media/tutorial-deploy-stream-analytics/module_output2.png)

## <a name="view-data"></a>Anzeigen von Daten

Wechseln Sie nun zu Ihrem IoT Edge-Gerät, um sich über die Interaktion zwischen dem Azure Stream Analytics-Modul und dem tempSensor-Modul zu informieren.

1. Überprüfen Sie, ob alle Module in Docker ausgeführt werden:

   ```cmd/sh
   iotedge list  
   ```
<!--
   ![Docker output](./media/tutorial-deploy-stream-analytics/docker_output.png)
-->
1. Zeigen Sie alle Systemprotokolle und Metrikdaten an. Verwenden Sie den Namen des Stream Analytics-Moduls:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

Sie sollten beobachten können, wie die Temperatur des Computers allmählich steigt, bis sie 30 Sekunden lang 70 Grad beträgt. Dann löst das Stream Analytics-Modul eine Zurücksetzung aus, und die Computertemperatur fällt zurück auf 21. 

   ![Docker-Protokoll](./media/tutorial-deploy-stream-analytics/docker_log.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden. Sie können auch dasselbe IoT Edge-Gerät als Testgerät weiter nutzen. 

Andernfalls können Sie die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden. 
 
[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Streaming Analytics-Auftrag zum Analysieren von Daten konfiguriert, die von ihrem IoT Edge-Gerät stammen. Anschließend haben Sie das Azure Stream Analytics-Modul auf Ihrem IoT Edge-Gerät geladen, um die Daten lokal zu verarbeiten, lokal auf einen Temperaturanstieg zu reagieren und den aggregierten Datenstrom an die Cloud zu senden. Sie können nun die anderen Tutorials bearbeiten, um noch mehr darüber zu erfahren, wie sich mit Azure IoT Edge weitere Lösungen für Ihr Unternehmen erstellen lassen.

> [!div class="nextstepaction"] 
> [Bereitstellen eines Azure Machine Learning-Modells als Modul](tutorial-deploy-machine-learning.md)
