---
title: Bereitstellen von Azure Machine Learning mit Azure IoT Edge | Microsoft-Dokumentation
description: "Bereitstellen von Azure Machine Learning als Modul auf einem Edgegerät"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 9b8475dcc51fb24fadd1faa4a2008b25a4464080
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Bereitstellen von Azure Machine Learning als IoT Edge-Modul – Vorschau

Mithilfe von IoT Edge-Modulen können Sie Code bereitstellen, mit dem Ihre Geschäftslogik direkt auf Ihren IoT Edge-Geräten implementiert wird. Dieses Tutorial führt Sie durch die Bereitstellung eines Azure Machine Learning-Moduls, das vorhersagt, wann bei einem Gerät ein Fehler auftritt. Dies basiert auf Sensordaten des simulierten IoT Edge-Geräts, das Sie im Tutorial zum Bereitstellen von Azure IoT Edge auf einem simulierten Gerät unter [Windows][lnk-tutorial1-win] oder [Linux][lnk-tutorial1-lin] erstellt haben. Folgendes wird vermittelt: 

> [!div class="checklist"]
> * Bereitstellen eines Azure Machine Learning-Moduls auf Ihrem IoT Edge-Gerät
> * Anzeigen generierter Daten

Wenn Sie ein eigenes [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/)-Modell in Ihrer Lösung verwenden möchten, [stellen Sie ein Modell für IoT Edge bereit](https://aka.ms/aml-iot-edge-doc), und hosten Sie es in einer Containerregistrierung wie [Azure Container Registry](../container-registry/index.yml) oder Docker.

## <a name="prerequisites"></a>Voraussetzungen

* Das Azure IoT Edge-Gerät, das Sie mit der Schnellstartanleitung oder im ersten Tutorial erstellt haben.
* Die IoT Hub-Verbindungszeichenfolge für den IoT Hub, mit dem Ihr IoT Edge-Gerät eine Verbindung herstellt.
* Der Azure ML-Container

## <a name="create-the-azure-ml-container"></a>Erstellen des Azure ML-Containers
Zum Erstellen des Azure ML-Containers folgen Sie den Anweisungen unter [AI Toolkit für Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection).

## <a name="run-the-solution"></a>Ausführen der Lösung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
1. Wechseln Sie zu **IoT Edge (Vorschau)**, und wählen Sie Ihr IoT Edge-Gerät aus.
1. Wählen Sie **Module festlegen** aus.
1. Wählen Sie **IoT Edge-Modul hinzufügen** aus.
1. Geben Sie im Feld **Name** die Zeichenfolge `tempSensor` ein.
1. Geben Sie im Feld **Image-URI** die Zeichenfolge `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` ein.
1. Behalten Sie die restlichen Einstellungen unverändert bei, und wählen Sie **Speichern** aus.
1. Bleiben Sie beim Schritt **Module hinzufügen**, und wählen Sie erneut **IoT Edge-Modul hinzufügen** aus.
1. Geben Sie im Feld **Name** den Namen des Containers ein, den Sie im vorherigen Abschnitt erstellt haben. Hilfe beim Suchen nach dem Namen finden Sie im [AI Toolkit für Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection).
1. Geben Sie im Feld **Image** den Image-URI des Containers ein, die Sie im vorherigen Abschnitt erstellt haben. Hilfe beim Suchen nach dem Image finden Sie im [AI Toolkit für Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection).
1. Klicken Sie auf **Speichern**.
1. Wenn Sie zum Schritt **Module hinzufügen** zurückgekehrt sind, klicken Sie auf **Weiter**.
1. Aktualisieren Sie Routen für das Modul:
1. Kopieren Sie im Schritt **Routen angeben** den folgenden JSON-Code in das Textfeld. Module veröffentlichen alle Nachrichten in der Edge-Laufzeit. Deklarative Regeln in der Laufzeit definieren, wohin diese Nachrichten fließen. In diesem Tutorial benötigen Sie zwei Routen. Die erste Route transportiert Nachrichten vom Temperatursensor über den Endpunkt „mlInput“ zum Machine Learning-Modul. Dies ist der Endpunkt, den alle Azure Machine Learning-Module verwenden. Die zweite Route transportiert Nachrichten aus dem Machine Learning-Modul an IoT Hub. Bei dieser Route ist „mlOutput“ der Endpunkt, den alle Azure Machine Learning-Module zur Ausgabe von Daten verwenden, und „upstream“ ist ein spezielles Ziel, das Edge Hub anweist, Nachrichten an IoT Hub zu senden. 

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ``` 

1. Klicken Sie auf **Weiter**.
 
1. Klicken Sie im Schritt „Vorlage überprüfen“ auf „Senden“. 
1. Kehren Sie zur Seite mit Gerätedetails zurück, und klicken Sie auf „Aktualisieren“.  Es sollte nun angezeigt werden, dass das neue Machine Learning-Modul zusammen mit dem tempSensor-Modul und der IoT Edge-Laufzeit ausgeführt wird.

## <a name="view-generated-data"></a>Anzeigen generierter Daten

 Verwenden Sie in Visual Studio Code den Menübefehl **Ansicht | Befehlspalette... | IoT: Überwachung von D2C-Nachrichten starten**, um Daten zu überwachen, die im IoT Hub eintreffen. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein von Azure Machine Learning unterstütztes IoT Edge-Modul bereitgestellt. Sie können mit einem der anderen Tutorials fortfahren, um andere Möglichkeiten kennenzulernen, wie Azure IoT Edge Ihnen beim Umwandeln von Daten in geschäftliche Erkenntnisse auf Edge-Ebene helfen kann.

> [!div class="nextstepaction"]
> [Bereitstellen einer Azure-Funktion als Modul](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md