---
title: Gerätesimulation in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird erläutert, wie Sie den Gerätesimulator mit dem Solution Accelerator für Remoteüberwachung verwenden.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/15/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 9b4f7f9a9c501204d48b738089dc3cbd015a744c
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
---
# <a name="create-a-new-simulated-device"></a>Erstellen eines neuen simulierten Geräts

In diesem Tutorial wird erläutert, wie Sie den Microservice zur Gerätesimulation mit dem Solution Accelerator für Remoteüberwachung anpassen. Zur Veranschaulichung der Funktionen des Gerätesimulators werden in diesem Tutorial zwei Szenarien in der Contoso IoT-Anwendung verwendet.

Das folgende Video gibt einen Überblick über die Optionen zum Anpassen des Microservices zur Gerätesimulation:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/How-to-customize-the-Remote-Monitoring-Preconfigured-Solution-for-Azure-IoT/Player]

Im ersten Szenario möchte Contoso ein neues intelligentes Leuchtmittelgerät (Lightbulb) testen. Zum Durchführen der Tests erstellen Sie ein neues simuliertes Gerät mit den folgenden Merkmalen:

*Eigenschaften*

| NAME                     | Werte                      |
| ------------------------ | --------------------------- |
| Farbe                    | White, Red, Blue            |
| Brightness               | 0 bis 100                    |
| Estimated remaining life | Countdown ab 10.000 Stunden |

*Telemetrie*

In der folgenden Tabelle sind die Daten aufgeführt, die das Leuchtmittel als Datenstrom an die Cloud sendet:

| NAME   | Werte      |
| ------ | ----------- |
| Status | „on“, „off“ |
| Temperatur | Grad F |
| online | true, false |

> [!NOTE]
> Der Telemetriewert **online** ist für alle simulierten Typen erforderlich.

*Methoden*

In der folgenden Tabelle sind die Aktionen aufgeführt, die das neue Gerät unterstützt:

| NAME        |
| ----------- |
| Switch on   |
| Switch off  |

*Anfangszustand*

In der folgenden Tabelle ist der Anfangszustand des Geräts aufgeführt:

| NAME                     | Werte |
| ------------------------ | -------|
| Initial color            | Weiß  |
| Initial brightness       | 75     |
| Initial remaining life   | 10.000 |
| Initial telemetry status | „on“   |
| Anfängliche Telemetrietemperatur | 200   |

Im zweiten Szenario fügen Sie dem vorhandenen **Chiller**-Gerät von Contoso einen neuen Telemetrietyp hinzu.

In diesem Tutorial wird erläutert, wie Sie den Gerätesimulator mit dem Solution Accelerator für Remoteüberwachung verwenden:

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Erstellen eines neuen Gerätetyps
> * Simulieren des Verhaltens des benutzerdefinierten Geräts
> * Hinzufügen eines neuen Gerätetyps im Dashboard
> * Senden benutzerdefinierter Telemetriedaten von einem vorhandenen Gerätetyp

Im folgenden Video wird eine exemplarische Vorgehensweise zum Verbinden von simulierten und physischen Geräten mit der Remoteüberwachungslösung gezeigt:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-38-Customizing-Azure-IoT-Suite-solution-and-connect-a-real-device/Player]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Eine bereitgestellte Instanz der Lösung für die Remoteüberwachung in Ihrem Azure-Abonnement. Sollten Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, absolvieren Sie zuerst das Tutorial [Bereitstellen des Solution Accelerators für die Remoteüberwachung](../iot-accelerators/iot-accelerators-remote-monitoring-deploy.md).

* Visual Studio 2017 Falls Sie Visual Studio 2017 nicht installiert haben, können Sie die kostenlose [Visual Studio Community](https://www.visualstudio.com/free-developer-offers/) Edition herunterladen.

* Visual Studio-Erweiterung [Cloud-Explorer für Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVS15Preview).

* Ein Konto für den [Docker-Hub](https://hub.docker.com/). Sie können sich für den Einstieg kostenlos registrieren.

* [Git](https://git-scm.com/downloads) auf dem Desktopcomputer

## <a name="prepare-your-development-environment"></a>Vorbereiten Ihrer Entwicklungsumgebung

Führen Sie die folgenden Aufgaben durch, um Ihre Entwicklungsumgebung für das Hinzufügen eines neuen simulierten Geräts zu Ihrer Lösung für die Remoteüberwachung vorzubereiten:

### <a name="configure-ssh-access-to-the-solution-virtual-machine-in-azure"></a>Konfigurieren von SSH-Zugriff auf den virtuellen Computer der Lösung in Azure

Wählen Sie beim Erstellen Ihrer Lösung für die Remoteüberwachung unter [www.azureiotsuite.com](https://www.azureiotsuite.com) einen Lösungsnamen. Der Lösungsname dient als Name für die Azure-Ressourcengruppe, die die verschiedenen von der Lösung verwendeten bereitgestellten Ressourcen enthält. Die folgenden Befehle verwenden eine Ressourcengruppe mit dem Namen **Contoso-01**. Ersetzen Sie **Contoso-01** durch den Namen Ihrer Ressourcengruppe.

Die folgenden Befehle verwenden den Befehl `az` von der [Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest). Sie können die Azure CLI 2.0 auf Ihrem Entwicklungscomputer installieren oder [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) im [Azure-Portal](http://portal.azure.com) verwenden. Die Azure CLI 2.0 ist in Cloud Shell vorinstalliert.

1. Um den Namen der Ressourcengruppe zu überprüfen, die Remoteüberwachungsressourcen enthält, führen Sie den folgenden Befehl aus:

    ```sh
    az group list | grep "name"
    ```

    Dieser Befehl listet alle Ressourcengruppen in Ihrem Abonnement auf. Diese Liste sollte eine Ressourcengruppe mit dem gleichen Namen wie Ihre Lösung für die Remoteüberwachung enthalten.

1. Um die Ressource als Standardgruppe für nachfolgende Befehle festzulegen, führen Sie den folgenden Befehl anstelle von **Contoso-01** mit dem Ressourcengruppennamen aus:

    ```sh
    az configure --defaults group=Contoso-01
    ```

1. Um die Ressourcen in Ihrer Ressourcengruppe aufzulisten, führen Sie den folgenden Befehl aus:

    ```sh
    az resource list -o table
    ```

    Notieren Sie sich die Namen Ihres virtuellen Computers und Ihrer Netzwerksicherheitsgruppe. Sie benötigen diese Werte für die nachfolgenden Schritte.

1. Um SSH-Zugriff auf Ihrem virtuellen Computer zu ermöglichen, führen Sie den folgenden Befehl mit dem Namen Ihrer Netzwerksicherheitsgruppe aus dem vorherigen Schritt aus:

    ```sh
    az network nsg rule create --name SSH --nsg-name YOUR-NETWORK-SECURITY-GROUP --priority 101 --destination-port-ranges 22 --access Allow --protocol TCP
    ```

    Um die Liste der Eingangsregeln für Ihr Netzwerk anzuzeigen, führen Sie den folgenden Befehl aus:

    ```sh
    az network nsg rule list --nsg-name YOUR-NETWORK-SECURITY-GROUP -o table
    ```

1. Führen Sie den folgenden Befehl aus, um das Kennwort für den virtuellen Computer zu ändern. Verwenden Sie den zuvor notierten Namen des virtuellen Computers und das von Ihnen gewählte Kennwort:

    ```sh
    az vm user update --name YOUR-VM-NAME --username azureuser --password YOUR-PASSWORD
    ```
1. Um die IP-Adresse des virtuellen Computers zu ermitteln, verwenden Sie den folgenden Befehl und notieren sich die öffentliche IP-Adresse:

    ```sh
    az vm list-ip-addresses --name YOUR-VM-NAME
    ```

1. Sie können nun für das Herstellen einer Verbindung mit Ihrem virtuellen Computer SSH auswählen. Der Befehl `ssh` ist in Cloud Shell vorinstalliert. Verwenden Sie die öffentliche IP-Adresse aus dem vorherigen Schritt und bei entsprechender Aufforderung das für den virtuellen Computer konfigurierte Kennwort:

    ```sh
    ssh azureuser@public-ip-address
    ```

    Sie haben jetzt auf dem virtuellen Computer, der die Docker-Container in der Lösung für die Remoteüberwachung ausführt, Zugriff auf die Shell. Um die ausgeführten Container anzuzeigen, verwenden Sie den folgenden Befehl:

    ```sh
    docker ps
    ```

### <a name="find-the-service-connection-strings"></a>Ermitteln der Dienstverbindungszeichenfolgen

In diesem Tutorial arbeiten Sie mit einer Visual Studio-Projektmappe, die eine Verbindung mit dem Cosmos DB- und IoT Hub-Dienst der Lösung herstellt. In den folgenden Schritten wird gezeigt, wie Sie die benötigten Werte der Verbindungszeichenfolge ermitteln:

1. Um die Cosmos DB-Verbindungszeichenfolge zu ermitteln, führen Sie den folgenden Befehl in der mit dem virtuellen Computer verbundenen SSH-Sitzung aus:

    ```sh
    sudo grep STORAGEADAPTER_DOCUMENTDB /app/env-vars
    ```

    Notieren Sie sich die Verbindungszeichenfolge. Sie verwenden diesen Wert später im Tutorial.

1. Um die IoT Hub-Verbindungszeichenfolge zu ermitteln, führen Sie den folgenden Befehl in der mit dem virtuellen Computer verbundenen SSH-Sitzung aus:

    ```sh
    sudo grep IOTHUB_CONNSTRING /app/env-vars
    ```

    Notieren Sie sich die Verbindungszeichenfolge. Sie verwenden diesen Wert später im Tutorial.

> [!NOTE]
> Die Verbindungszeichenfolgen können Sie auch im Azure-Portal oder mit dem Befehl `az` ermitteln.

### <a name="stop-the-device-simulation-service-in-the-virtual-machine"></a>Beenden des Gerätesimulationsdiensts auf dem virtuellen Computer

Wenn Sie den Gerätesimulationsdienst ändern, können Sie ihn zum Testen Ihrer Änderungen lokal ausführen. Bevor Sie den Gerätesimulationsdienst lokal ausführen, müssen Sie die auf dem virtuellen Computer ausgeführte Instanz wie folgt beenden:

1. Um die **CONTAINER-ID** des Diensts **device-simulation** zu ermitteln, führen Sie den folgenden Befehl in der mit dem virtuellen Computer verbundenen SSH-Sitzung aus:

    ```sh
    docker ps
    ```

    Notieren Sie sich die Container-ID des Diensts **device-simulation**.

1. Um den Container **device-simulation** zu beenden, führen Sie den folgenden Befehl aus:

    ```sh
    docker stop container-id-from-previous-step
    ```

### <a name="clone-the-github-repositories"></a>Klonen der GitHub-Repositorys

In diesem Tutorial arbeiten Sie mit den Visual Studio-Projekten **device-simulation** und **storage-adapter**. Sie können die Quellcoderepositorys von GitHub klonen. Führen Sie diesen Schritt auf dem lokalen Entwicklungscomputer durch, auf dem Visual Studio installiert ist:

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu dem Ordner, in dem Ihre Kopie der GitHub-Repositorys **device-simulation** und **storage-adapter** gespeichert werden soll.

1. Um die .NET-Version des Repositorys **device-simulation** zu klonen, führen Sie den folgenden Befehl aus:

    ```cmd
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

    Mit dem Gerätesimulationsdienst in der Lösung für die Remoteüberwachung können Sie Änderungen an den integrierten simulierten Gerätetypen vornehmen und neue simulierte Gerätetypen erstellen. Mithilfe der benutzerdefinierten Gerätetypen können Sie das Verhalten der Remoteüberwachungslösung testen, bevor Sie Ihre physischen Geräte verbinden.

1. Um die .NET-Version des Repositorys **storage-adapter** zu klonen, führen Sie den folgenden Befehl aus:

    ```cmd
    git clone https://github.com/Azure/pcs-storage-adapter-dotnet.git
    ```

    Der Gerätesimulationsdienst stellt mithilfe des Speicheradapterdiensts eine Verbindung mit dem Cosmos DB-Dienst in Azure her. Die Remoteüberwachungslösung speichert die Konfigurationsdaten simulierter Geräte in einer Cosmos DB-Datenbank.

### <a name="run-the-storage-adapter-service-locally"></a>Lokales Ausführen des Speicheradapterdiensts

Der Gerätesimulationsdienst stellt mithilfe des Speicheradapterdiensts eine Verbindung mit der Cosmos DB-Datenbank der Lösung her. Wenn Sie den Gerätesimulationsdienst lokal ausführen, müssen Sie den Speicheradapterdienst lokal ausführen. In den folgenden Schritten wird gezeigt, wie der Speicheradapterdienst in Visual Studio ausgeführt wird:

1. Öffnen Sie in Visual Studio die Projektmappendatei **pcs-storage-adapter.sln** in Ihrem lokalen Klon des Repositorys **storage-adapter**.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **WebService**, und wählen Sie **Eigenschaften** sowie **Debuggen**.

1. Legen Sie im Abschnitt **Umgebungsvariablen** den Wert der Variable **PCS\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** als Cosmos DB-Verbindungszeichenfolge fest, die Sie zuvor notiert haben. Speichern Sie anschließend die Änderungen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **WebService**, wählen Sie **Debuggen** und dann **Neue Instanz starten**.

1. Der Dienst wird lokal ausgeführt und öffnet `http://localhost:9022/v1/status` in Ihrem Standardbrowser. Überprüfen Sie, ob der Wert von **Status** „OK: Aktiv und gut“ lautet.

1. Belassen Sie den Speicheradapterdienst in der lokalen Ausführung, bis Sie das Tutorial abgeschlossen haben.

Jetzt ist alles eingerichtet und Sie können mit dem Hinzufügen eines neuen Typs eines simulierten Geräts zu Ihrer Remoteüberwachungslösung beginnen.

## <a name="create-a-simulated-device-type"></a>Erstellen eines simulierten Gerätetyps

Am einfachsten lässt sich ein neuer Gerätetyp im Gerätesimulationsdienst durch Kopieren und Ändern eines vorhandenen Gerätetyps erstellen. Mit den folgenden Schritten wird veranschaulicht, wie Sie das integrierte **Chiller**-Gerät kopieren, um ein neues **Lightbulb**-Gerät zu erstellen:

1. Öffnen Sie in Visual Studio die Projektmappendatei **device-simulation.sln** in Ihrem lokalen Klon des Repositorys **device-simulation**.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **SimulationAgent**, und wählen Sie **Eigenschaften** sowie **Debuggen**.

1. Legen Sie im Abschnitt **Umgebungsvariablen** den Wert der Variable **PCS\_IOTHUB\_CONNSTRING** als IoT Hub-Verbindungszeichenfolge fest, die Sie zuvor notiert haben. Speichern Sie anschließend die Änderungen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **WebService**, und wählen Sie **Eigenschaften** sowie **Debuggen**.

1. Legen Sie im Abschnitt **Umgebungsvariablen** den Wert der Variable **PCS\_IOTHUB\_CONNSTRING** als IoT Hub-Verbindungszeichenfolge fest, die Sie zuvor notiert haben. Speichern Sie anschließend die Änderungen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe **device-simulation**, und klicken Sie auf **Startprojekte festlegen**. Wählen Sie **Einzelnes Startprojekt** und dann **WebService**. Klicken Sie dann auf **OK**.

1. Jeder Gerätetyp verfügt über eine JSON-Modelldatei und zugeordnete Skripts im Ordner **Services/data/devicemodels**. Kopieren Sie im Projektmappen-Explorer die **Chiller**-Dateien, um die **Lightbulb**-Dateien wie in der folgenden Tabelle dargestellt zu erstellen:

    | Quelle                      | Ziel                   |
    | --------------------------- | ----------------------------- |
    | chiller-01.json             | lightbulb-01.json             |
    | scripts/chiller-01-state.js | scripts/lightbulb-01-state.js |
    | scripts/reboot-method.js    | scripts/SwitchOn-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Definieren der Merkmale des neuen Gerätetyps

Die Datei **lightbulb-01.json** definiert die Merkmale des Typs, z.B. die generierte Telemetrie oder die unterstützten Methoden. Mit den folgenden Schritten wird die Datei **lightbulb-01.json** aktualisiert und der **Lightbulb**-Dienst definiert:

1. Aktualisieren Sie in der Datei **lightbulb-01.json** die Gerätemetadaten wie im folgenden Ausschnitt gezeigt:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. Aktualisieren Sie in der Datei **lightbulb-01.json** die Simulationsdefinition wie im folgenden Ausschnitt gezeigt:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "temperature": 200.0,
        "temperature_unit": "F",
        "status": "on"
      },
      "Interval": "00:00:20",
      "Scripts": [
        {
          "Type": "javascript",
          "Path": "lightbulb-01-state.js"
        }
      ]
    },
    ```

1. Aktualisieren Sie in der Datei **lightbulb-01.json** die Eigenschaften des Gerätetyps wie im folgenden Ausschnitt gezeigt:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. Aktualisieren Sie in der Datei **lightbulb-01.json** die Telemetriedefinitionen des Gerätetyps wie im folgenden Ausschnitt gezeigt:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "temperature": "double",
            "temperature_unit": "text",
            "status": "text"
          }
        }
      }
    ],
    ```

1. Aktualisieren Sie in der Datei **lightbulb-01.json** die Methoden des Gerätetyps wie im folgenden Ausschnitt gezeigt:

    ```json
    "CloudToDeviceMethods": {
      "SwitchOn": {
        "Type": "javascript",
        "Path": "SwitchOn-method.js"
      },
      "SwitchOff": {
        "Type": "javascript",
        "Path": "SwitchOff-method.js"
      }
    }
    ```

1. Speichern Sie die Datei **lightbulb 01.json**.

### <a name="simulate-custom-device-behavior"></a>Simulieren des Verhaltens des benutzerdefinierten Geräts

Die Datei **scripts/lightbulb-01-state.js** definiert das Simulationsverhalten des **Lightbulb**-Gerätetyps. Mit den folgenden Schritten wird die Datei **scripts/lightbulb-01-state.js** aktualisiert und das Verhalten des **Lightbulb**-Geräts definiert:

1. Bearbeiten Sie die Statusdefinition in der Datei **scripts/lightbulb-01-state.js** wie im folgenden Ausschnitt gezeigt:

    ```js
    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
    };
    ```

1. Fügen Sie zur folgenden Definition eine **flip**-Funktion nach der **vary**-Funktion hinzu:

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

      state.temperature = vary(200, 5, 150, 250);

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Speichern Sie die Datei **scripts/lightbulb-01-state.js**.

Die Datei **scripts/SwitchOn-method.js** implementiert die **Switch On**-Methode in einem **Lightbulb**-Gerät. In den folgenden Schritten wird die Datei **scripts/SwitchOn-method.js** aktualisiert:

1. Bearbeiten Sie die Statusdefinition in der Datei **scripts/SwitchOn-method.js** wie im folgenden Ausschnitt gezeigt:

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

1. Speichern Sie die Datei **scripts/SwitchOn-method.js**.

1. Erstellen Sie eine Kopie der Datei **scripts/SwitchOn-method.js** mit dem Namen **scripts/SwitchOff-method.js**.

1. Um das Leuchtmittel auszuschalten, bearbeiten Sie die **main**-Funktion in der Datei **scripts/SwitchOff-method.js** wie folgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Speichern Sie die Datei **scripts/SwitchOff-method.js**.

1. Wählen Sie im Projektmappen-Explorer nacheinander die vier neuen Dateien aus. Stellen Sie im Fenster **Eigenschaften** sicher, dass **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** festgelegt ist.

### <a name="configure-the-device-simulation-service"></a>Konfigurieren des Gerätesimulationsdiensts

Um die Anzahl der simulierten Geräte zu beschränken, die während der Tests eine Verbindung mit der Projektmappe herstellen, konfigurieren Sie den Dienst für die Ausführung eines einzelnen Kühlers und Leuchtmittelgeräts. Die Konfigurationsdaten werden in der Cosmos DB-Instanz in der Ressourcengruppe der Projektmappe gespeichert. Um die Konfigurationsdaten zu bearbeiten, verwenden Sie die Ansicht **Cloud-Explorer** in Visual Studio:

1. Um die Ansicht **Cloud-Explorer** in Visual Studio zu öffnen, wählen Sie **Ansicht** und dann **Cloud-Explorer**.

1. Um das Simulationskonfigurationsdokument zu ermitteln, geben Sie unter **Nach Ressourcen suchen** **simulations.1** ein.

1. Doppelklicken Sie auf das Dokument **simulations.1**, um es für die Bearbeitung zu öffnen.

1. Suchen Sie im Wert für **Data** das Array **DeviceModels** wie im folgenden Ausschnitt:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"chiller-02\",\"Count\":1},{\"Id\":\"elevator-01\",\"Count\":1},{\"Id\":\"elevator-02\",\"Count\":1},{\"Id\":\"engine-01\",\"Count\":1},{\"Id\":\"engine-02\",\"Count\":1},{\"Id\":\"prototype-01\",\"Count\":1},{\"Id\":\"prototype-02\",\"Count\":1},{\"Id\":\"truck-01\",\"Count\":1},{\"Id\":\"truck-02\",\"Count\":1}]
    ```

1. Um einen einzelnen Kühler und ein einzelnes simuliertes Leuchtmittelgerät zu definieren, ersetzen Sie das Array **DeviceModels** durch folgenden Code:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"lightbulb-01\",\"Count\":1}]
    ```

    Speichern Sie die Änderung im Dokument **simulations.1**.

> [!NOTE]
> Sie können auch den Cosmos DB-Daten-Explorer im Azure-Portal verwenden, um das Dokument **simulations.1** zu bearbeiten.

### <a name="test-the-lightbulb-device-type-locally"></a>Lokales Testen des Leuchtmittel-Gerätetyps

Sie können nun Ihren neuen simulierten Leuchtmitteltyp testen, indem Sie das Projekt für die Gerätesimulation lokal ausführen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **WebService**, und wählen Sie **Debuggen** und dann **Neue Instanz starten**.

1. Um zu überprüfen, ob die beiden simulierten Geräte mit IoT Hub verbunden sind, rufen Sie das Azure-Portal in Ihrem Browser auf.

1. Navigieren Sie zu IoT Hub in der Ressourcengruppe, die Ihre Lösung für die Remoteüberwachung verwendet.

1. Wählen Sie im Abschnitt **Überwachung** die Option **Metriken** aus. Überprüfen Sie dann, ob die Anzahl unter **Verbundene Geräte** zwei lautet:

    ![Anzahl verbundener Geräte](media/iot-suite-remote-monitoring-test/connecteddevices.png)

1. Navigieren Sie in Ihrem Browser zu dem **Dashboard** Ihrer Lösung für die Remoteüberwachung. Wählen Sie im Telemetriebereich auf dem **Dashboard** die Option **Temperatur** aus. Die Temperatur aller Ihrer simulierten Geräte wird im Diagramm angezeigt:

    ![Temperaturtelemetrie](media/iot-suite-remote-monitoring-test/telemetry.png)

Die Simulation des Leuchtmittelgeräts wird jetzt lokal ausgeführt. Der nächste Schritt besteht darin, Ihren aktualisierten Simulatorcode für den virtuellen Computer bereitzustellen, auf dem die Microservices für die Remoteüberwachung in Azure ausgeführt werden.

Bevor Sie fortfahren, können Sie das Debuggen des Gerätesimulations- und Speicheradapterprojekts in Visual Studio beenden.

### <a name="deploy-the-updated-simulator-to-the-cloud"></a>Bereitstellen des aktualisierten Simulators in der Cloud

Die Microservices in der Lösung für die Remoteüberwachung werden in Docker-Containern ausgeführt. Die Container werden auf dem virtuellen Computer der Projektmappe in Azure gehostet. In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen Sie ein neues Docker-Image für die Gerätesimulation.
* Laden Sie das Image in Ihr Docker-Hubrepository hoch.
* Importieren Sie das Image auf den virtuellen Computer Ihrer Projektmappe.

Bei den folgenden Schritten wird davon ausgegangen, dass Sie über ein Repository namens **lightbulb** in Ihrem Docker-Hubkonto verfügen.

1. Öffnen Sie in Visual Studio im Projekt **device-simulation** die Datei **solution\scripts\docker\build.cmd**.

1. Bearbeiten Sie die Zeile, die die Umgebungsvariable **DOCKER_IMAGE** als Namen für Ihr Docker-Hubrepository festlegt:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Speichern Sie die Änderungen.

1. Öffnen Sie in Visual Studio im Projekt **device-simulation** die Datei **solution\scripts\docker\publish.cmd**.

1. Bearbeiten Sie die Zeile, die die Umgebungsvariable **DOCKER_IMAGE** als Namen für Ihr Docker-Hubrepository festlegt:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Speichern Sie die Änderungen.

1. Öffnen Sie eine Eingabeaufforderung als Administrator. Navigieren Sie dann zum Ordner **scripts\docker** in Ihrem Klon des GitHub-Repositorys **device-simulation**.

1. Um das Docker-Image zu erstellen, führen Sie den folgenden Befehl aus:

    ```cmd
    build.cmd
    ```

1. Um sich bei Ihrem Docker-Hub anzumelden, führen Sie den folgenden Befehl aus:

    ```cmd
    docker login
    ```

1. Um ein neues Image in Ihr Docker-Hubkonto hochzuladen, führen Sie den folgenden Befehl aus:

    ```cmd
    publish.cmd
    ```

1. Um den Upload zu überprüfen, rufen Sie [https://hub.docker.com/](https://hub.docker.com/) auf. Suchen Sie Ihr Repository namens **lightbulb**, und wählen Sie **Details**. Wählen Sie dann **Tags**:

    ![Docker-Hub](media/iot-suite-remote-monitoring-test/dockerhub.png)

    Mit den Skripts wurde der Tag **testing** zum Image hinzugefügt.

1. Verwenden Sie SSH, um eine Verbindung mit dem virtuellen Computer Ihrer Projektmappe in Azure herzustellen. Navigieren Sie zum **App**-Ordner, und bearbeiten Sie die Datei **docker-compose.yaml**:

    ```sh
    cd /app
    sudo nano docker-compose.yaml
    ```

1. Bearbeiten Sie den Eintrag für den Gerätesimulationsdienst für die Verwendung des Docker-Image:

    ```yaml
    devicesimulation:
      image: {your docker ID}/lightbulb:testing
    ```

    Speichern Sie die Änderungen.

1. Um alle Dienste mit den neuen Einstellungen neu zu starten, führen Sie den folgenden Befehl aus:

    ```sh
    sudo ./start.sh
    ```

1. Um die Protokolldatei von Ihrem neuen Gerätesimulationscontainer zu überprüfen, führen Sie den folgenden Befehl zum Ermitteln der Container-ID aus:

    ```sh
    docker ps
    ```

    Führen Sie mit der Container-ID anschließend den folgenden Befehl aus:

    ```sh
    docker logs {container ID}
    ```

Sie haben jetzt die Schritte zum Bereitstellen einer aktualisierten Version des Gerätesimulationsdiensts für die Lösung zur Remoteüberwachung durchgeführt.

Navigieren Sie in Ihrem Browser zu dem **Dashboard** Ihrer Lösung für die Remoteüberwachung. Wählen Sie im Telemetriebereich auf dem **Dashboard** die Option **Temperatur** aus. Die Temperatur Ihrer zwei simulierten Geräte wird im Diagramm angezeigt:

![Temperaturtelemetrie](media/iot-suite-remote-monitoring-test/telemetry.png)

Auf der Seite **Devices** (Geräte) können Sie Instanzen des neuen Gerätetyps bereitstellen:

![Anzeigen der Liste der verfügbaren Simulationen](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

Sie können die Telemetriedaten von dem simulierten Gerät anzeigen:

![Anzeigen der Telemetrie des Leuchtmittels](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

Sie können die **SwitchOn**- und die **SwitchOff**-Methode für das Gerät aufrufen:

![Aufrufen von Methoden für das Leuchtmittel](media/iot-suite-remote-monitoring-test/devicesmethods.png)

## <a name="add-a-new-telemetry-type"></a>Hinzufügen eines neuen Telemetrietyps

In diesem Abschnitt wird beschrieben, wie ein vorhandener simulierter Gerätetyp so geändert wird, dass ein neuer Telemetrietyp unterstützt wird.

### <a name="locate-the-chiller-device-type-files"></a>Suchen der Dateien des Chiller-Gerätetyps

Mit den folgenden Schritten wird veranschaulicht, wie Sie die Dateien suchen, die das integrierte **Chiller**-Gerät definieren:

1. Wenn dies noch nicht erfolgt ist, führen Sie den folgenden Befehl aus, um das GitHub-Repository **device-simulation** auf Ihrem lokalen Computer zu klonen:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Jeder Gerätetyp verfügt über eine JSON-Modelldatei und zugeordnete Skripts im Ordner `data/devicemodels`. Der simulierte **Chiller**-Gerätetyp wird in den folgenden Dateien definiert:

    * **data/devicemodels/chiller-01.json**
    * **data/devicemodels/scripts/chiller-01-state.js**

### <a name="specify-the-new-telemetry-type"></a>Angeben des neuen Telemetrietyps

Mit den folgenden Schritten wird veranschaulicht, wie Sie dem **Chiller**-Gerätetyp den neuen Telemetrietyp **Internal Temperature** hinzufügen:

1. Öffnen Sie die Datei **chiller-01.json**.

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

1. Speichern Sie die Datei **chiller-01.json**.

1. Öffnen Sie die Datei **scripts/chiller-01-state.js**.

1. Fügen Sie der **state**-Variable die folgenden Felder hinzu:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Fügen Sie der **main**-Funktion die folgende Zeile hinzu:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Speichern Sie die Datei **scripts/chiller-01-state.js**.

### <a name="test-the-chiller-device-type"></a>Testen des Chiller-Gerätetyps

Zum Testen des aktualisierten **Chiller**-Gerätetyps führen Sie zunächst eine lokale Kopie des Diensts **device-simulation** aus, um zu prüfen, ob sich Ihr Gerätetyp wie erwartet verhält. Nachdem Sie den aktualisierten Gerätetyp lokal getestet und gedebuggt haben, können Sie den Container neu erstellen und den Dienst **device-simulation** in Azure erneut bereitstellen.

Wenn Sie den Dienst **device-simulation** lokal ausführen, sendet er Telemetriedaten an die Remoteüberwachungslösung. Auf der Seite **Devices** (Geräte) können Sie Instanzen des aktualisierten Gerätetyps bereitstellen.

Um Ihre Änderungen lokal zu testen und zu debuggen, lesen Sie den vorherigen Abschnitt [Lokales Testen des Leuchtmittel-Gerätetyps](#test-the-lightbulb-device-type-locally).

Um Ihren aktualisierten Gerätesimulationsdienst für den virtuellen Computer der Projektmappe in Azure bereitzustellen, lesen Sie den vorherigen Abschnitt [Bereitstellen des aktualisierten Simulators in der Cloud](#deploy-the-updated-simulator-to-the-cloud).

Auf der Seite **Devices** (Geräte) können Sie Instanzen des aktualisierten Gerätetyps bereitstellen:

![Hinzufügen des aktualisierten Chiller-Gerätetyps](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

Sie können die neue Telemetrie **Internal Temperature** von dem simulierten Gerät anzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde Folgendes veranschaulicht:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Erstellen eines neuen Gerätetyps
> * Simulieren des Verhaltens des benutzerdefinierten Geräts
> * Hinzufügen eines neuen Gerätetyps im Dashboard
> * Senden benutzerdefinierter Telemetriedaten von einem vorhandenen Gerätetyp

Nun haben Sie erfahren, wie der Gerätesimulationsdienst angepasst wird. Als Nächstes wird empfohlen, sich mit dem [Verbinden eines physischen Geräts mit der Remoteüberwachungslösung](iot-suite-connecting-devices-node.md) auseinanderzusetzen.

Weitere Informationen für Entwickler zur Remoteüberwachungslösung finden Sie in folgenden Quellen:

* [Referenzhandbuch für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Handbuch zur Problembehandlung für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
