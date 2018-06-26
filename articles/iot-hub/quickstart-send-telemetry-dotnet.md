---
title: 'Schnellstart: Senden von Telemetriedaten an Azure IoT Hub (C#) | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung führen Sie zwei C#-Beispielanwendungen aus, um simulierte Telemetriedaten an eine IoT Hub-Instanz zu senden und Telemetriedaten zur Verarbeitung in der Cloud aus der IoT Hub-Instanz zu lesen.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/20/2018
ms.author: dobett
ms.openlocfilehash: dbb4ce971e6504f33de82e31cf289a42a1640952
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293168"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-c"></a>Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (C#)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub ist ein Azure-Dienst, mit dem Sie umfangreiche Telemetriedaten von Ihren Geräten in der Cloud erfassen können, um sie zu speichern oder zu verarbeiten. In dieser Schnellstartanleitung senden Sie Telemetriedaten von einer Anwendung zur Simulation eines Geräts über IoT Hub zur Verarbeitung an eine Back-End-Anwendung.

In der Schnellstartanleitung werden zwei vorab geschriebene C#-Anwendungen verwendet: eine zum Senden der Telemetriedaten und eine andere zum Lesen der Telemetriedaten aus dem Hub. Vor dem Ausführen dieser beiden Anwendungen erstellen Sie eine IoT Hub-Instanz und registrieren ein Gerät bei dieser Instanz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Die beiden in dieser Schnellstartanleitung ausgeführten Beispielanwendungen sind in C# geschrieben. Sie benötigen auf Ihrem Entwicklungscomputer das .NET Core SDK 2.1.0 oder höher.

Sie können das .NET Core SDK für mehrere Plattformen von [.NET](https://www.microsoft.com/net/download/all) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle C#-Version auf Ihrem Entwicklungscomputer überprüfen:

```cmd/sh
dotnet --version
```

Laden Sie das C#-Beispielprojekt von https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip herunter, und extrahieren Sie das ZIP-Archiv.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Registrieren eines Geräts

Ein Gerät muss bei Ihrer IoT Hub-Instanz registriert sein, um eine Verbindung herstellen zu können. In dieser Schnellstartanleitung verwenden Sie die Azure-Befehlszeilenschnittstelle, um ein simuliertes Gerät zu registrieren.

1. Fügen Sie die IoT Hub-CLI-Erweiterung hinzu, und erstellen Sie die Geräteidentität. Ersetzen Sie `{YourIoTHubName}` durch den Namen, den Sie für Ihre IoT Hub-Instanz ausgewählt haben:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDotnetDevice
    ```

    Wenn Sie einen anderen Namen für Ihr Gerät auswählen, aktualisieren Sie den Gerätenamen in den Beispielanwendungen, bevor sie ausgeführt werden.

2. Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDotnetDevice --output table
    ```

    Notieren Sie sich die Geräteverbindungszeichenfolge (`Hostname=...=`). Dieser Wert wird später in der Schnellstartanleitung benötigt.

3. Darüber hinaus benötigen Sie den mit _Event Hubs kompatiblen Endpunkt_, den mit _Event Hubs kompatiblen Pfad_ und den _iothubowner-Primärschlüssel_ Ihre IoT Hub-Instanz, um der Back-End-Anwendung das Herstellen einer Verbindung mit Ihrer IoT Hub-Instanz und das Abrufen der Nachrichten zu ermöglichen. Die folgenden Befehle rufen diese Werte für Ihre IoT Hub-Instanz ab:

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name {YourIoTHubName}

    az iot hub show --query properties.eventHubEndpoints.events.path --name {YourIoTHubName}

    az iot hub policy show --name iothubowner --query primaryKey --hub-name {your IoT Hub name}
    ```

    Notieren Sie sich diese drei Werte. Sie benötigen sie später in der Schnellstartanleitung.

## <a name="send-simulated-telemetry"></a>Senden simulierter Telemetriedaten

Die Anwendung zur Simulation eines Geräts stellt eine Verbindung mit einem gerätespezifischen Endpunkt in Ihrer IoT Hub-Instanz her und sendet simulierte Telemetriedaten für Temperatur und Luftfeuchtigkeit.

1. Navigieren Sie in einem Terminalfenster zum Stammordner des C#-Beispielprojekts. Navigieren Sie anschließend zum Ordner **iot-hub\Quickstarts\simulated-device**.

2. Öffnen Sie die Datei **SimulatedDevice.cs** in einem Text-Editor Ihrer Wahl.

    Ersetzen Sie den Wert der Variablen `connectionString` durch die Geräteverbindungszeichenfolge, die Sie sich zuvor notiert haben. Speichern Sie dann die Änderungen an der Datei **SimulatedDevice.cs**.

3. Führen Sie im Terminalfenster den folgenden Befehl aus, um die erforderlichen Pakete für die Anwendung zur Simulation eines Geräts zu installieren:

    ```cmd/sh
    dotnet restore
    ```

4. Führen Sie im Terminalfenster den folgenden Befehl aus, um die Anwendung zur Simulation eines Geräts zu erstellen und auszuführen:

    ```cmd/sh
    dotnet run
    ```

    Der folgende Screenshot zeigt die Ausgabe, während die Anwendung zur Simulation eines Geräts Telemetriedaten an Ihre IoT Hub-Instanz sendet:

    ![Ausführen des simulierten Geräts](media/quickstart-send-telemetry-dotnet/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Lesen der Telemetriedaten aus Ihrem Hub

Die Back-End-Anwendung stellt eine Verbindung mit dem dienstseitigen Endpunkt **Events** in Ihrer IoT Hub-Instanz her. Die Anwendung empfängt die vom simulierten Gerät gesendeten Gerät-zu-Cloud-Nachrichten. Eine IoT Hub-Back-End-Anwendung wird in der Regel in der Cloud ausgeführt, um Gerät-zu-Cloud-Nachrichten zu empfangen und zu verarbeiten.

1. Navigieren Sie in einem anderen Terminalfenster zum Stammordner des C#-Beispielprojekts. Navigieren Sie anschließend zum Ordner **iot-hub\Quickstarts\read-d2c-messages**.

2. Öffnen Sie die Datei **ReadDeviceToCloudMessages.cs** in einem Text-Editor Ihrer Wahl. Aktualisieren Sie die folgenden Variablen, und speichern Sie Ihre Änderungen an der Datei.

    | Variable | Wert |
    | -------- | ----------- |
    | `eventHubsCompatibleEndpoint` | Ersetzen Sie den Wert der Variablen durch den mit Event Hubs kompatiblen Endpunkt, den Sie sich zuvor notiert haben. |
    | `eventHubsCompatiblePath`     | Ersetzen Sie den Wert der Variablen durch den mit Event Hubs kompatiblen Pfad, den Sie sich zuvor notiert haben. |
    | `iotHubSasKey`                | Ersetzen Sie den Wert der Variablen durch den iothubowner-Primärschlüssel, den Sie sich zuvor notiert haben. |

3. Führen Sie im Terminalfenster die folgenden Befehle aus, um die erforderlichen Bibliotheken für die Back-End-Anwendung zu installieren:

    ```cmd/sh
    dotnet restore
    ```

4. Führen Sie im Terminalfenster die folgenden Befehle aus, um die Back-End-Anwendung zu erstellen und auszuführen:

    ```cmd/sh
    dotnet run
    ```

    Der folgende Screenshot zeigt die Ausgabe, während die Back-End-Anwendung vom simulierten Gerät an den Hub gesendete Telemetriedaten empfängt:

    ![Ausführen der Back-End-Anwendung](media/quickstart-send-telemetry-dotnet/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine IoT Hub-Instanz eingerichtet, ein Gerät registriert, mit einer C#-Anwendung simulierte Telemetriedaten an die IoT Hub-Instanz gesendet und mit einer einfachen Back-End-Anwendung die Telemetriedaten aus der Instanz gelesen.

Um zu erfahren, wie Sie das simulierte Gerät über eine Back-End-Anwendung steuern, fahren Sie mit der nächsten Schnellstartanleitung fort.

> [!div class="nextstepaction"]
> [Schnellstart: Steuern eines mit einer IoT Hub-Instanz verbundenen Geräts](quickstart-control-device-dotnet.md)