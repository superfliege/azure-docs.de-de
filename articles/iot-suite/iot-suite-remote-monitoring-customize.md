---
title: "Anpassen der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Informationen dazu, wie Sie auf den Quellcode für die vorkonfigurierte Remoteüberwachungslösung zugreifen können."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 886a4412ac348869563a03d697f4363cb3dea8f9
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="customize-the-remote-monitoring-preconfigured-solution"></a>Anpassen der vorkonfigurierten Remoteüberwachungslösung

Dieser Artikel enthält Informationen dazu, wie Sie auf den Quellcode zugreifen und die vorkonfigurierte Remoteüberwachungslösung anpassen können. Der Artikel beschreibt Folgendes:

* Die GitHub-Repositorys, die den Quellcode und Ressourcen für die Microservices enthalten, die die vorkonfigurierte Lösung bilden.
* Allgemeine Anpassungsszenarien, z.B. Hinzufügen eines neuen Gerätetyps.

## <a name="project-overview"></a>Projektübersicht

### <a name="implementations"></a>Implementierungen

Die Remoteüberwachungslösung umfasst .NET- und Java-Implementierungen. Beide Implementierungen bieten eine ähnliche Funktionalität und basieren auf den gleichen zugrunde liegenden Azure-Diensten. Die GitHub-Repositorys der obersten Ebene finden Sie hier:

* [.NET-Lösung](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Java-Lösung](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

### <a name="microservices"></a>Microservices

Wenn Sie sich für ein bestimmtes Feature der Lösung interessieren, können Sie auf die GitHub-Repositorys für die einzelnen Microservices zugreifen. Jeder Microservice implementiert einen anderen Teil der Funktionalität der Lösung. Weitere Informationen zu der Gesamtarchitektur finden Sie unter [Architektur der vorkonfigurierten Lösung für Remoteüberwachung](iot-suite-remote-monitoring-sample-walkthrough.md).

Diese Tabelle enthält eine Übersicht über die aktuelle Verfügbarkeit der einzelnen Microservices für jede Sprache:

<!-- please add links for each of the repos in the table, you can find them here https://github.com/Azure/azure-iot-pcs-team/wiki/Repositories-->

| Microservice      | Beschreibung | Java | .NET |
| ----------------- | ----------- | ---- | ---- |
| Web-Benutzeroberfläche            | Web-App für die Remoteüberwachungslösung Implementiert die Benutzeroberfläche mithilfe des React.js-Frameworks. | [– (React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) | [– (React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) |
| IoT Hub-Manager   | Verarbeitet die Kommunikation mit IoT Hub.        | [Verfügbar](https://github.com/Azure/iothub-manager-java) | [Verfügbar](https://github.com/Azure/iothub-manager-dotnet)   |
| Authentifizierung    |  Verwaltet die Azure Active Directory-Integration.  | Noch nicht verfügbar | [Verfügbar](https://github.com/Azure/pcs-auth-dotnet)   |
| Gerätesimulation | Verwaltet einen Pool von simulierten Geräten. | Noch nicht verfügbar | [Verfügbar](https://github.com/Azure/device-simulation-dotnet)   |
| Telemetrie         | Stellt die Gerätetelemetrie in der Benutzeroberfläche bereit. | [Verfügbar](https://github.com/Azure/device-telemetry-java) | [Verfügbar](https://github.com/Azure/device-telemetry-dotnet)   |
| Telemetrie-Agent   | Analysiert den Telemetriedatenstrom, speichert Nachrichten von Azure IoT Hub und generiert Warnungen nach definierten Regeln.  | [Verfügbar](https://github.com/Azure/telemetry-agent-java) | [Verfügbar](https://github.com/Azure/telemetry-agent-dotnet)   |
| Benutzeroberflächen-Konfiguration         | Verwaltet die Konfigurationsdaten von der Benutzeroberfläche. | [Verfügbar](https://github.com/azure/pcs-ui-config-java) | [Verfügbar](https://github.com/azure/pcs-ui-config-dotnet)   |
| Speicheradapter   |  Verwaltet die Interaktionen mit dem Speicherdienst.   | [Verfügbar](https://github.com/azure/pcs-storage-adapter-java) | [Verfügbar](https://github.com/azure/pcs-storage-adapter-dotnet)   |
| Reverseproxy     | Macht private Ressourcen in einer verwalteten Methode über einen eindeutigen Endpunkt verfügbar. | Noch nicht verfügbar | [Verfügbar](https://github.com/Azure/reverse-proxy-dotnet)   |

Die Java-Lösung verwendet derzeit die Microservices für die .NET-Authentifizierung und -Simulation und für den .NET-Reverseproxy. Diese Microservices werden durch Java-Versionen ersetzt, sobald sie verfügbar sind.

## <a name="presentation-and-visualization"></a>Darstellung und Visualisierung

In den folgenden Abschnitten werden die Optionen zum Anpassen der Darstellungs- und Visualisierungsschicht in der Remoteüberwachungslösung beschrieben:

### <a name="change-the-logo-in-the-ui"></a>Ändern des Logos in der Benutzeroberfläche

In der Standardbereitstellung werden der Name und das Logo des Unternehmens Contoso in der Benutzeroberfläche verwendet. So ändern Sie diese Elemente der Benutzeroberfläche, um den Namen und das Logo Ihres Unternehmens anzuzeigen

1. Führen Sie den folgenden Befehl aus, um das Repository für die Web-Benutzeroberfläche zu klonen:

    ```cmd/sh
    git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
    ```

1. Öffnen Sie zum Ändern des Firmennamens die Datei `src/common/lang.js` in einem Text-Editor.

1. Suchen Sie in der Datei die folgende Zeile:

    ```js
    CONTOSO: 'Contoso',
    ```

1. Ersetzen Sie `Contoso` durch den Namen Ihres Unternehmens. Beispiel:

    ```js
    CONTOSO: 'YourCo',
    ```

1. Speichern Sie die Datei.

1. Um das Logo zu aktualisieren, fügen Sie im Ordner `assets/icons` eine neue SVG-Datei ein. Das vorhandene Logo ist die Datei `assets/icons/Contoso.svg`.

1. Öffnen Sie die Datei `src/components/layout/leftNav/leftNav.js` in einem Text-Editor.

1. Suchen Sie in der Datei die folgende Zeile:

    ```js
    import ContosoIcon from '../../../assets/icons/Contoso.svg';
    ```

1. Ersetzen Sie `Contoso.svg` durch den Namen Ihrer Logodatei. Beispiel:

    ```js
    import ContosoIcon from '../../../assets/icons/YourCo.svg';
    ```

1. Suchen Sie in der Datei die folgende Zeile:

    ```js
    alt="ContosoIcon"
    ```

1. Ersetzen Sie `ContosoIcon` durch Ihren `alt`-Text. Beispiel:

    ```js
    alt="YourCoIcon"
    ```

1. Speichern Sie die Datei.

1. Zum Testen der Änderungen können Sie die aktualisierte `webui` auf Ihrem lokalen Computer ausführen. Informationen zum Erstellen und lokalen Ausführen der `webui`-Lösung finden Sie unter [Build, run and test locally](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/README.md#build-run-and-test-locally) (Erstellen, Testen und lokales Ausführen) in der Infodatei des `webui`-GitHub-Repositorys.

1. Informationen zum Bereitstellen Ihrer Änderungen finden Sie im [Developer Reference Guide](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide) (Referenzhandbuch für Entwickler).

<!--

### Add a new KPI to the Dashboard page

The following steps describe how to add a new KPI to display on the **Dashboard** page. The new KPI shows information about the number of alarms with specific status values as a pie chart:

1. Step 1

1. Step 2
-->

### <a name="customize-the-map"></a>Anpassen der Karte

Ausführliche Informationen zu den Kartenkomponenten in der Lösung finden Sie auf der Seite [Customize map](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) (Anpassungsübersicht) in GitHub.

<!--
### Customize the telemetry chart

See the [Customize telemetry chart](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of the telemetry chart components in the solution.

### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

### Duplicate an existing control

To duplicate an existing UI element such as a chart or alert, see the [Duplicate a control](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub.

-->

### <a name="other-customization-options"></a>Andere Optionen zur Anpassung

Um die Darstellungs- und Visualisierungsschicht in der Remoteüberwachungslösung weiter zu ändern, können Sie den Code bearbeiten. Relevante GitHub-Repositorys:

* [UIConfig (.NET)](https://github.com/Azure/pcs-ui-config-dotnet/)
* [UIConfig (Java)](https://github.com/Azure/pcs-ui-config-java/)
* [Azure PCS Remote Monitoring WebUI](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="device-connectivity-and-streaming"></a>Geräteverbindungen und Streaming

In den folgenden Abschnitten werden die Optionen zum Anpassen der Geräteverbindungs- und Streamingschicht in der Remoteüberwachungslösung beschrieben. [Gerätemodelle](https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models) beschreiben die Gerätetypen und Telemetrie in der Lösung. Sie verwenden Gerätemodelle für simulierte und physische Geräte.

Ein Beispiel für die Implementierung eines physischen Geräts finden Sie unter [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung](iot-suite-connecting-devices-node.md).

Bei der Verwendung eines _physischen Geräts_ müssen Sie die Clientanwendung mit einem Gerätemodell bereitstellen, das die Spezifikationen für die Gerätemetadaten und die Gerätetelemetrie enthält.

In den folgenden Abschnitten wird die Verwendung von Gerätemodellen mit simulierten Geräten beschrieben.

### <a name="add-a-telemetry-type"></a>Hinzufügen eines Telemetriedatentyps

Die Gerätetypen in der Contoso-Demolösung geben die Telemetriedaten an, die die einzelnen Gerätetypen senden. Um die zusätzlichen Telemetrietypen anzugeben, kann ein Gerät Telemetriedefinitionen als Metadaten an die Lösung senden. Wenn Sie dieses Format verwenden, nutzt das Dashboard die Gerätetelemetrie und die verfügbaren Methoden dynamisch, und Sie müssen die Benutzeroberfläche nicht ändern. Alternativ können Sie die Gerätetypdefinition in der Lösung ändern.

Informationen zum Hinzufügen benutzerdefinierter Telemetrie im Microservice _Gerätesimulator_ finden Sie unter [Testen der Lösung mit simulierten Geräten](iot-suite-remote-monitoring-test.md).

### <a name="add-a-device-type"></a>Hinzufügen eines Gerätetyps

In der Contoso-Demolösung sind einige Beispielgerätetypen definiert. In der Lösung können Sie benutzerdefinierte Gerätetypen entsprechend Ihren spezifischen Geräteanforderungen definieren. Ihr Unternehmen verwendet z.B. ein Industriegateway als primäres mit der Lösung verbundenes Gerät.

Um eine genaue Darstellung Ihres Geräts zu erstellen, müssen Sie die Anwendung, die auf Ihrem Gerät ausgeführt wird, entsprechend den Geräteanforderungen ändern.

Informationen zum Hinzufügen eines neuen Gerätetyps im Microservice _Gerätesimulator_ finden Sie unter [Testen der Lösung mit simulierten Geräten](iot-suite-remote-monitoring-test.md).

### <a name="define-custom-methods-for-simulated-devices"></a>Definieren benutzerdefinierter Methoden für simulierte Geräte

Informationen zum Definieren von benutzerdefinierten Methoden für simulierte Geräte in der Remoteüberwachungslösung finden Sie im GitHub-Repository unter [Device Models](https://github.com/Azure/device-simulation-dotnet/wiki/%5BAPI-Specifications%5D-Device-Models) (Gerätemodelle).

<!--
#### Using the simulator service

TODO: add steps for the simulator microservice here
-->

#### <a name="using-a-physical-device"></a>Verwenden eines physischen Geräts

Informationen zum Implementieren von Methoden und Aufträgen auf Ihren physischen Geräten finden Sie in den folgenden Artikeln zu IoT Hub:

* [Verstehen und Aufrufen direkter Methoden von IoT Hub](../iot-hub/iot-hub-devguide-direct-methods.md)
* [Planen von Aufträgen auf mehreren Geräten](../iot-hub/iot-hub-devguide-jobs.md)

### <a name="other-customization-options"></a>Andere Optionen zur Anpassung

Um die Geräteverbindungs- und Streamingschicht in der Remoteüberwachungslösung weiter zu ändern, können Sie den Code bearbeiten. Relevante GitHub-Repositorys:

* [Device Telemetry (.NET)](https://github.com/Azure/device-telemetry-dotnet)
* [Device Telemetry (Java)](https://github.com/Azure/device-telemetry-java)
* [Telemetry Agent (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Telemetry Agent (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="data-processing-and-analytics"></a>Datenverarbeitung und Analysen

<!--
The following sections describe options to customize the data processing and analytics layer in the remote monitoring solution:

### Rules and actions

See the [Customize rules and actions](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to customize the rules and actions in solution.


### Other customization options
-->

Um die Datenverarbeitungs- und Analysenschicht in der Remoteüberwachungslösung zu ändern, können Sie den Code bearbeiten. Relevante GitHub-Repositorys:

* [Telemetry Agent (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Telemetry Agent (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="infrastructure"></a>Infrastruktur

<!--
The following sections describe options for customizing the infrastructure services in the remote monitoring solution:

### Change storage

The default storage service for the remote monitoring solution is Cosmos DB. See the [Customize storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses.

### Change log storage

The default storage service for logs is Cosmos DB. See the [Customize log storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses for logging.

### Other customization options
-->

Um die Infrastruktur in der Remoteüberwachungslösung zu ändern, können Sie den Code bearbeiten. Relevante GitHub-Repositorys:

* [IoTHub Manager (.NET)](https://github.com/Azure/iothub-manager-dotnet)
* [IoTHub Manager (Java)](https://github.com/Azure/iothub-manager-java)
* [Storage Adapter (.NET)](https://github.com/Azure/pcs-storage-adapter-dotnet)
* [Storage Adapter (Java)](https://github.com/Azure/pcs-storage-adapter-java)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den verfügbaren Ressourcen erhalten, mit denen Sie die vorkonfigurierte Lösung anpassen können.

Weitere konzeptuelle Informationen zur vorkonfigurierten Remoteüberwachungslösung finden Sie unter [Remoteüberwachungsarchitektur](iot-suite-remote-monitoring-sample-walkthrough.md).

Weitere Informationen zum Anpassen der Remoteüberwachungslösung finden Sie in folgenden Quellen:

* [Referenzhandbuch für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Handbuch zur Problembehandlung für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->