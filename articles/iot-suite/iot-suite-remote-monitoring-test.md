---
title: "Gerätesimulation in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "In diesem Tutorial wird erläutert, wie Sie den Gerätesimulator mit der vorkonfigurierten Remoteüberwachungslösung verwenden."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 09/16/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 732ec45003481b0e2f2eca03b6ae13772d325ef1
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="test-your-solution-with-simulated-devices"></a>Testen der Lösung mit simulierten Geräten

In diesem Tutorial wird erläutert, wie Sie den Microservice zur Gerätesimulation mit der vorkonfigurierten Remoteüberwachungslösung verwenden. Zur Veranschaulichung der Funktionen des Gerätesimulators werden in diesem Tutorial zwei Szenarien in der Contoso IoT-Anwendung verwendet.

Im ersten Szenario möchte Contoso ein neues intelligentes Leuchtmittelgerät (Lightbulb) testen. Zum Durchführen der Tests erstellen Sie ein neues simuliertes Gerät mit den folgenden Merkmalen:

*Eigenschaften*

| Name                     | Werte                      |
| ------------------------ | --------------------------- |
| Farbe                    | White, Red, Blue            |
| Brightness               | 0 bis 100                    |
| Estimated remaining life | Countdown ab 10.000 Stunden |

*Telemetrie*

In der folgenden Tabelle sind die Daten aufgeführt, die das Leuchtmittel als Datenstrom an die Cloud sendet:

| Name   | Werte      |
| ------ | ----------- |
| Status | „on“, „off“ |

*Methoden*

In der folgenden Tabelle sind die Aktionen aufgeführt, die das neue Gerät unterstützt:

| Name        |
| ----------- |
| Switch on   |
| Switch off  |

*Anfangszustand*

In der folgenden Tabelle ist der Anfangszustand des Geräts aufgeführt:

| Name                     | Werte |
| ------------------------ | -------|
| Initial color            | Weiß  |
| Initial brightness       | 75     |
| Initial remaining life   | 10.000 |
| Initial telemetry status | „on“   |

Im zweiten Szenario fügen Sie dem vorhandenen **Chiller**-Gerät von Contoso einen neuen Telemetrietyp hinzu.

In diesem Tutorial wird erläutert, wie Sie den Gerätesimulator mit der vorkonfigurierten Remoteüberwachungslösung verwenden.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Erstellen eines neuen Gerätetyps
> * Simulieren des Verhaltens des benutzerdefinierten Geräts
> * Hinzufügen eines neuen Gerätetyps im Dashboard
> * Senden benutzerdefinierter Telemetriedaten von einem vorhandenen Gerätetyp

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Falls Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, ist es ratsam, das Tutorial [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-deploy.md) durchzuarbeiten.

<!-- Dominic please this use as your reference https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models -->

## <a name="the-device-simulation-service"></a>Der Gerätesimulationsdienst

Mit dem Gerätesimulationsdienst in der vorkonfigurierten Lösung können Sie Änderungen an den integrierten simulierten Gerätetypen vornehmen und neue simulierte Gerätetypen erstellen. Mithilfe der benutzerdefinierten Gerätetypen können Sie das Verhalten der Remoteüberwachungslösung testen, bevor Sie eine Verbindung Ihrer physischen Geräte mit der Lösung herstellen.

## <a name="create-a-simulated-device-type"></a>Erstellen eines simulierten Gerätetyps

Am einfachsten lässt sich ein neuer Gerätetyp im Microservice zur Gerätesimulation durch Kopieren und Ändern eines vorhandenen Gerätetyps erstellen. Mit den folgenden Schritten wird veranschaulicht, wie Sie das integrierte **Chiller**-Gerät kopieren, um ein neues **Lightbulb**-Gerät zu erstellen:

1. Führen Sie den folgenden Befehl aus, um das GitHub-Repository **device-simulation** auf Ihrem lokalen Computer zu klonen:

    ```cmd/sh
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

1. Jeder Gerätetyp verfügt über eine JSON-Modelldatei und zugeordnete Skripts im Ordner `Services/data/devicemodels`. Kopieren Sie die **Chiller**-Dateien, um die **Lightbulb**-Dateien wie in der folgenden Tabelle dargestellt zu erstellen:

    | Quelle                      | Ziel                   |
    | --------------------------- | ----------------------------- |
    | chiller-01.json             | lightbulb-01.json             |
    | scripts/chiller-01-state.js | scripts/lightbulb-01-state.js |
    | scripts/reboot-method.js    | scripts/SwitchOn-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Definieren der Merkmale des neuen Gerätetyps

Die Datei `lightbulb-01.json` definiert die Merkmale des Typs, z.B. die generierte Telemetrie oder die unterstützten Methoden. Mit den folgenden Schritten wird die Datei `lightbulb-01.json` aktualisiert und das **Lightbulb**-Gerät definiert:

1. Aktualisieren Sie in der Datei `lightbulb-01.json` die Gerätemetadaten wie im folgenden Codeausschnitt gezeigt:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. Aktualisieren Sie in der Datei `lightbulb-01.json` die Simulationsdefinition wie im folgenden Codeausschnitt gezeigt:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "status": "on"
      },
      "Script": {
        "Type": "javascript",
        "Path": "lightbulb-01-state.js",
        "Interval": "00:00:20"
      }
    },
    ```

1. Aktualisieren Sie in der Datei `lightbulb-01.json` die Eigenschaften des Gerätetyps wie im folgenden Codeausschnitt gezeigt:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. Aktualisieren Sie in der Datei `lightbulb-01.json` die Telemetriedefinitionen des Gerätetyps wie im folgenden Codeausschnitt gezeigt:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "status": "text"
          }
        }
      }
    ],
    ```

1. Aktualisieren Sie in der Datei `lightbulb-01.json` die Methoden des Gerätetyps wie im folgenden Codeausschnitt gezeigt:

    ```json
    "CloudToDeviceMethods": {
      "SwitchOn": {
        "Type": "javascript",
        "Path": "SwitchOn-method.js"
      },
      "SwitchOff": {
        "Type": "javascript",
        "Path": "SwitchOff-method.js"
      },
    }
    ```

1. Speichern Sie die Datei `lightbulb-01.json`.

### <a name="simulate-custom-device-behavior"></a>Simulieren des Verhaltens des benutzerdefinierten Geräts

Die Datei `scripts/lightbulb-01-state.js` definiert das Simulationsverhalten des **Lightbulb**-Gerätetyps. Mit den folgenden Schritten wird die Datei `scripts/lightbulb-01-state.js` aktualisiert und das Verhalten des **Lightbulb**-Geräts definiert:

1. Bearbeiten Sie die Statusdefinition in der Datei `scripts/lightbulb-01-state.js` wie im folgenden Codeausschnitt gezeigt:

    ```js
    // Default state
    var state = {
      online: true,
      status: "on"
    };
    ```

1. Ersetzen Sie die **vary**-Funktion durch die folgende **flip**-Funktion:

    ```js
    /**
    * Simple formula that sometimes flips the status of the lightbulb
    */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }
    ```

1. Bearbeiten Sie die **main**-Funktion, um das Verhalten wie im folgenden Codeausschnitt gezeigt zu implementieren:

    ```js
    function main(context, previousState) {

      // Restore the global state before generating the new telemetry, so that
      // the telemetry can apply changes using the previous function state.
      restoreState(previousState);

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Speichern Sie die Datei `scripts/lightbulb-01-state.js`.

Die Datei `scripts/SwitchOn-method.js` implementiert die **Switch On**-Methode in einem **Lightbulb**-Gerät. Mit den folgenden Schritten wird die Datei `scripts/SwitchOn-method.js` aktualisiert:

1. Bearbeiten Sie die Statusdefinition in der Datei `scripts/SwitchOn-method.js` wie im folgenden Codeausschnitt gezeigt:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Bearbeiten Sie zum Einschalten des Leuchtmittels die **main**-Funktion wie folgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Speichern Sie die Datei `scripts/SwitchOn-method.js`.

1. Erstellen Sie eine Kopie der Datei `scripts/SwitchOn-method.js` mit dem Namen `scripts/SwitchOff-method.js`.

1. Bearbeiten Sie zum Ausschalten des Leuchtmittels die **main**-Funktion in der Datei `scripts/SwitchOff-method.js` wie folgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Speichern Sie die Datei `scripts/SwitchOff-method.js`.

### <a name="test-the-lightbulb-device-type"></a>Testen des Leuchtmittel-Gerätetyps

Zum Testen des **Lightbulb**-Gerätetyps können Sie zunächst prüfen, ob der Gerätetyp sich wie erwartet verhält, indem Sie eine lokale Kopie des Diensts **device-simulation** ausführen. Nachdem Sie den neuen Gerätetyp lokal getestet und gedebuggt haben, können Sie den Container neu erstellen und den Dienst **device-simulation** in Azure erneut bereitstellen.

Informationen zum lokalen Testen und Debuggen Ihrer Änderungen finden Sie unter [Device Simulation Overview](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md) (Übersicht über die Gerätesimulation).

Konfigurieren Sie das Projekt so, dass die Dateien des neuen **Lightbulb**-Geräts in das Ausgabeverzeichnis kopiert werden.

Informationen zum Testen des neuen Geräts in einer bereitgestellten Lösung finden Sie in folgenden Quellen:

* [Deploying containers from custom docker-hub account](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account) (Bereitstellen von Containern über ein benutzerdefiniertes Docker-Hub-Konto)
* [Update a deployed container via manual copy](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy) (Aktualisieren eines bereitgestellten Containers über eine manuelle Kopie)

Auf der Seite **Devices** (Geräte) können Sie Instanzen des neuen Gerätetyps bereitstellen:

![Anzeigen der Liste der verfügbaren Simulationen](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

Sie können die Telemetriedaten von dem simulierten Gerät anzeigen:

![Anzeigen der Telemetrie des Leuchtmittels](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

Sie können die **SwitchOn**- und die **SwitchOff**-Methode für das Gerät aufrufen:

![Aufrufen von Methoden für das Leuchtmittel](media/iot-suite-remote-monitoring-test/devicesmethods.png)

Informationen zum Erstellen eines Docker-Images mit dem neuen Gerätetyp zur Bereitstellung in Azure finden Sie unter [Building a customized Docker image](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image) (Erstellen eines benutzerdefinierten Docker-Images).

## <a name="add-a-new-telemetry-type"></a>Hinzufügen eines neuen Telemetrietyps

In diesem Abschnitt wird beschrieben, wie ein vorhandener simulierter Gerätetyp so geändert wird, dass ein neuer Telemetrietyp unterstützt wird.

### <a name="locate-the-chiller-device-type-files"></a>Suchen der Dateien des Chiller-Gerätetyps

Mit den folgenden Schritten wird veranschaulicht, wie Sie die Dateien suchen, die das integrierte **Chiller**-Gerät definieren:

1. Wenn dies noch nicht erfolgt ist, führen Sie den folgenden Befehl aus, um das GitHub-Repository **device-simulation** auf Ihrem lokalen Computer zu klonen:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Jeder Gerätetyp verfügt über eine JSON-Modelldatei und zugeordnete Skripts im Ordner `data/devicemodels`. Der simulierte **Chiller**-Gerätetyp wird in den folgenden Dateien definiert:
    * `data/devicemodels/chiller-01.json`
    * `data/devicemodels/scripts/chiller-01-state.js`

### <a name="specify-the-new-telemetry-type"></a>Angeben des neuen Telemetrietyps

Mit den folgenden Schritten wird veranschaulicht, wie Sie dem **Chiller**-Gerätetyp den neuen Telemetrietyp **Internal Temperature** hinzufügen:

1. Öffnen Sie die Datei `chiller-01.json` .

1. Aktualisieren Sie den **SchemaVersion**-Wert wie folgt:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. Fügen Sie im **InitialState**-Abschnitt die folgenden zwei Definitionen hinzu:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. Fügen Sie im **Telemetry**-Array die folgende Definition hinzu:

    ```json
    {
      "Interval": "00:00:05",
      "MessageTemplate": "{\"internal_temperature\":${internal_temperature},\"internal_temperature_unit\":\"${internal_temperature_unit}\"}",
      "MessageSchema": {
        "Name": "chiller-internal-temperature;v1",
        "Format": "JSON",
        "Fields": {
          "temperature": "double",
          "temperature_unit": "text"
        }
      }
    },
    ```

1. Speichern Sie die Datei `chiller-01.json`.

1. Öffnen Sie die Datei `scripts/chiller-01-state.js` .

1. Fügen Sie der **state**-Variable die folgenden Felder hinzu:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Fügen Sie der **main**-Funktion die folgende Zeile hinzu:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Speichern Sie die Datei `scripts/chiller-01-state.js`.

### <a name="test-the-chiller-device-type"></a>Testen des Chiller-Gerätetyps

Zum Testen des aktualisierten **Chiller**-Gerätetyps können Sie zunächst prüfen, ob der Gerätetyp sich wie erwartet verhält, indem Sie eine lokale Kopie des Diensts **device-simulation** ausführen. Nachdem Sie den aktualisierten Gerätetyp lokal getestet und gedebuggt haben, können Sie den Container neu erstellen und den Dienst **device-simulation** in Azure erneut bereitstellen.

Wenn Sie den Dienst **device-simulation** lokal ausführen, sendet er Telemetriedaten an die Remoteüberwachungslösung. Auf der Seite **Devices** (Geräte) können Sie Instanzen des aktualisierten Gerätetyps bereitstellen.

Informationen zum lokalen Testen und Debuggen Ihrer Änderungen finden Sie unter [Running the service with Visual Studio](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#running-the-service-with-visual-studio) (Ausführen des Diensts mit Visual Studio) oder [Build and Run from the command line](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#build-and-run-from-the-command-line) (Erstellen und Ausführen über die Befehlszeile).

Informationen zum Testen des neuen Geräts in einer bereitgestellten Lösung finden Sie in folgenden Quellen:

* [Deploying containers from custom docker-hub account](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account) (Bereitstellen von Containern über ein benutzerdefiniertes Docker-Hub-Konto)
* [Update a deployed container via manual copy](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy) (Aktualisieren eines bereitgestellten Containers über eine manuelle Kopie)

Auf der Seite **Devices** (Geräte) können Sie Instanzen des aktualisierten Gerätetyps bereitstellen:

![Hinzufügen des aktualisierten Chiller-Gerätetyps](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

Sie können die neue Telemetrie **Internal Temperature** von dem simulierten Gerät anzeigen.

Informationen zum Erstellen eines Docker-Images mit dem neuen Gerätetyp zur Bereitstellung in Azure finden Sie unter [Building a customized Docker image](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image) (Erstellen eines benutzerdefinierten Docker-Images).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde Folgendes veranschaulicht:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Erstellen eines neuen Gerätetyps
> * Simulieren des Verhaltens des benutzerdefinierten Geräts
> * Hinzufügen eines neuen Gerätetyps im Dashboard
> * Senden benutzerdefinierter Telemetriedaten von einem vorhandenen Gerätetyp

Sie wissen nun, wie Sie den Gerätesimulationsdienst verwenden können. Im nächsten empfohlenen Schritt erfahren Sie, wie Sie [ein physisches Gerät mit der Remoteüberwachungslösung verbinden](iot-suite-connecting-devices-node.md).

Weitere Informationen für Entwickler zur Remoteüberwachungslösung finden Sie in folgenden Quellen:

* [Referenzhandbuch für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Handbuch zur Problembehandlung für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->