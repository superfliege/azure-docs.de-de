---
title: 'Schnellstart: Senden von Telemetriedaten an Azure IoT Hub (Java) | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung führen Sie zwei Java-Beispielanwendung aus, um simulierte Telemetriedaten an eine IoT Hub-Instanz zu senden und zur Verarbeitung in der Cloud aus der IoT Hub-Instanz zu lesen.
services: iot-hub
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: 36005988611e7ec3f16146919e3ab3f04755e7e5
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-java"></a>Schnellstart: Senden von Telemetriedaten von einem Gerät an einen IoT-Hub und Lesen der Telemetriedaten mit einer Back-End-Anwendung (Java) aus dem Hub

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub ist ein Azure-Dienst, mit dem Sie umfangreiche Telemetriedaten von Ihren Geräten in der Cloud erfassen können, um sie zu speichern oder zu verarbeiten. In dieser Schnellstartanleitung senden Sie Telemetriedaten von einer Anwendung zur Simulation eines Geräts über IoT Hub zur Verarbeitung an eine Back-End-Anwendung.

In der Schnellstartanleitung werden zwei vorgefertigte Java-Anwendungen verwendet: eine zum Senden der Telemetriedaten und eine andere zum Lesen der Telemetriedaten aus dem Hub. Vor dem Ausführen dieser beiden Anwendungen erstellen Sie eine IoT Hub-Instanz und registrieren ein Gerät bei dieser Instanz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Die beiden in dieser Schnellstartanleitung ausgeführten Beispielanwendungen sind in Java geschrieben. Sie benötigen auf Ihrem Entwicklungscomputer Java SE 8 oder höher.

Sie können Java für mehrere Plattformen von [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle Java-Version auf Ihrem Entwicklungscomputer überprüfen:

```cmd/sh
java --version
```

Für die Erstellung der Beispiele müssen Sie Maven 3 installieren. Sie können Maven für mehrere Plattformen von [Apache Maven](https://maven.apache.org/download.cgi) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle Maven-Version auf Ihrem Entwicklungscomputer überprüfen:

```cmd/sh
mvn --version
```

Laden Sie das Java-Beispielprojekt von https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip herunter, und extrahieren Sie das ZIP-Archiv.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Registrieren eines Geräts

Ein Gerät muss bei Ihrer IoT Hub-Instanz registriert sein, um eine Verbindung herstellen zu können. In dieser Schnellstartanleitung verwenden Sie die Azure-Befehlszeilenschnittstelle, um ein simuliertes Gerät zu registrieren.

1. Fügen Sie die IoT Hub-CLI-Erweiterung hinzu, und erstellen Sie die Geräteidentität. Ersetzen Sie `{YourIoTHubName}` durch den Namen, den Sie für Ihre IoT Hub-Instanz ausgewählt haben:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyJavaDevice
    ```

    Wenn Sie einen anderen Namen für Ihr Gerät auswählen, aktualisieren Sie den Gerätenamen in den Beispielanwendungen, bevor sie ausgeführt werden.

1. Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyJavaDevice --output table
    ```

    Notieren Sie sich die Geräteverbindungszeichenfolge (`Hostname=...=`). Dieser Wert wird später in der Schnellstartanleitung benötigt.

1. Darüber hinaus benötigen Sie den mit _Event Hubs kompatiblen Endpunkt_, den mit _Event Hubs kompatiblen Pfad_ und den _iothubowner-Primärschlüssel_ Ihre IoT Hub-Instanz, um der Back-End-Anwendung das Herstellen einer Verbindung mit Ihrer IoT Hub-Instanz und das Abrufen der Nachrichten zu ermöglichen. Die folgenden Befehle rufen diese Werte für Ihre IoT Hub-Instanz ab:

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name {YourIoTHubName}

    az iot hub show --query properties.eventHubEndpoints.events.path --name {YourIoTHubName}

    az iot hub policy show --name iothubowner --query primaryKey --hub-name {your IoT Hub name}
    ```

    Notieren Sie sich diese drei Werte. Sie benötigen sie später in der Schnellstartanleitung.

## <a name="send-simulated-telemetry"></a>Senden simulierter Telemetriedaten

Die Anwendung zur Simulation eines Geräts stellt eine Verbindung mit einem gerätespezifischen Endpunkt in Ihrer IoT Hub-Instanz her und sendet simulierte Telemetriedaten für Temperatur und Luftfeuchtigkeit.

1. Navigieren Sie in einem Terminalfenster zum Stammordner des Java-Beispielprojekts. Navigieren Sie anschließend zum Ordner **Quickstarts\simulated-device**.

1. Öffnen Sie die Datei **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** in einem Text-Editor Ihrer Wahl.

    Ersetzen Sie den Wert der Variablen `connString` durch die Geräteverbindungszeichenfolge, die sie sich zuvor notiert haben. Speichern Sie dann die Änderungen an der Datei **SimulatedDevice.java**.

1. Führen Sie im Terminalfenster die folgenden Befehle aus, um die erforderlichen Bibliotheken zu installieren. Erstellen Sie dann die simulierte Geräteanwendung:

    ```cmd/sh
    mvn clean package
    ```

1. Führen Sie im Terminalfenster die folgenden Befehle aus, um die simulierte Geräteanwendung auszuführen:

    ```cmd/sh
    java -jar target/simulated-device-1.0.0-with-deps.jar
    ```

    Der folgende Screenshot zeigt die Ausgabe, während die Anwendung zur Simulation eines Geräts Telemetriedaten an Ihre IoT Hub-Instanz sendet:

    ![Ausführen des simulierten Geräts](media/quickstart-send-telemetry-java/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Lesen der Telemetriedaten aus Ihrem Hub

Die Back-End-Anwendung stellt eine Verbindung mit dem dienstseitigen Endpunkt **Events** in Ihrer IoT Hub-Instanz her. Die Anwendung empfängt die vom simulierten Gerät gesendeten Gerät-zu-Cloud-Nachrichten. Eine IoT Hub-Back-End-Anwendung wird in der Regel in der Cloud ausgeführt, um Gerät-zu-Cloud-Nachrichten zu empfangen und zu verarbeiten.

1. Navigieren Sie in einem anderen Terminalfenster zum Stammordner des Java-Beispielprojekts. Navigieren Sie dann zum Ordner **Quickstarts\read-d2c-messages**.

1. Öffnen Sie die Datei **src/main/java/com/microsoft/docs/iothub/samples/ReadDeviceToCloudMessages.java** in einem Text-Editor Ihrer Wahl.

    Ersetzen Sie den Wert der Variablen `eventHubsCompatibleEndpoint` durch den mit Event Hubs kompatiblen Endpunkt, den sie sich zuvor notiert haben.

    Ersetzen Sie den Wert der Variablen `eventHubsCompatiblePath` durch den mit Event Hubs kompatiblen Pfad, den Sie sich zuvor notiert haben.

    Ersetzen Sie den Wert der Variablen `iotHubSasKey` durch den Primärschlüssel für „iothubowner“, den sie sich zuvor notiert haben. Speichern Sie dann die Änderungen an der Datei **ReadDeviceToCloudMessages.java**.

1. Führen Sie im Terminalfenster die folgenden Befehle aus, um die erforderlichen Bibliotheken zu installieren. Erstellen Sie dann die Back-End-Anwendung:

    ```cmd/sh
    mvn clean package
    ```

1. Führen Sie im Terminalfenster die folgenden Befehle aus, um die Back-End-Anwendung auszuführen:

    ```cmd/sh
    java -jar target/read-d2c-messages-1.0.0-with-deps.jar
    ```

    Der folgende Screenshot zeigt die Ausgabe, während die Back-End-Anwendung vom simulierten Gerät an den Hub gesendete Telemetriedaten empfängt:

    ![Ausführen der Back-End-Anwendung](media/quickstart-send-telemetry-java/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die nächste Schnellstartanleitung ausführen möchten, behalten Sie die Ressourcengruppe und die IoT Hub-Instanz bei, und verwenden Sie sie später erneut.

Falls Sie die IoT Hub-Instanz nicht mehr benötigen, löschen Sie die Ressourcengruppe über das Portal. Wählen Sie hierzu die Ressourcengruppe **qs-iot-hub-rg** aus, die Ihre IoT Hub-Instanz enthält, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine IoT Hub-Instanz eingerichtet, ein Gerät registriert, mit einer Java-Anwendung simulierte Telemetriedaten an die Hub-Instanz gesendet und mit einer einfachen Back-End-Anwendung die Telemetriedaten aus der Hub-Instanz gelesen.

Wenn Sie erfahren möchten, wie Sie das simulierte Gerät über eine Back-End-Anwendung steuern, fahren Sie mit der nächsten Schnellstartanleitung fort.

> [!div class="nextstepaction"]
> [Schnellstart: Steuern eines mit einer IoT Hub-Instanz verbundenen Geräts](quickstart-control-device-java.md)