---
title: "Vorkonfigurierte Azure IoT Suite-Lösungen – Übersicht | Microsoft-Dokumentation"
description: "Enthält eine Beschreibung der vorkonfigurierten Azure IoT Suite-Lösungen und ihrer Architektur mit Links zu zusätzlichen Ressourcen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 502b7678e0c47f594291409a9ede976dea3895e5
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="what-is-azure-iot-suite"></a>Was ist Azure IoT Suite?

Azure IoT Suite besteht aus einem Satz mit *vorkonfigurierten Lösungen*, für die Folgendes gilt:

* Bereitstellung in wenigen Minuten
* Schneller Einstieg
* Mögliche Anpassung an Ihre jeweiligen Anforderungen

Die vorkonfigurierten *IoT Suite*-Lösungen wurden alle gemäß denselben Prinzipien und Zielsetzungen entworfen.

## <a name="preconfigured-solutions-overview"></a>Übersicht über vorkonfigurierte Lösungen

Bei einer vorkonfigurierten Lösung handelt es sich um eine Open Source-Implementierung von allgemeinen IoT-Lösungsmustern, die Sie in Azure mit Ihrem Abonnement bereitstellen können. Für jede vorkonfigurierte Lösung werden benutzerdefinierter Code und Azure-Dienste kombiniert, um ein oder mehrere bestimmte IoT-Szenarien zu implementieren. Sie können alle Szenarien an Ihre jeweiligen Anforderungen anpassen. Zu diesen Szenarien gehören:

* Visualisieren Sie Daten in einem umfassenden Dashboard, um fundierte Erkenntnisse und Informationen zum Status der Lösung zu erhalten.
* Konfigurieren Sie Regeln und Alarme für die IoT-Gerätetelemetrie im Livezustand.
* Planen Sie Geräteverwaltungsaufträge, z.B. Updates von Software und Konfiguration.
* Stellen Sie Ihre eigenen benutzerdefinierten physischen oder simulierten Geräte bereit.
* Führen Sie die Problembehandlung und -behebung in Ihren Gruppen mit IoT-Geräten durch.

Bei jeder vorkonfigurierten Lösung handelt es sich um eine vollständige End-to-End-Implementierung, bei der simulierte oder physische Geräte zum Generieren von Telemetriedaten verwendet werden können. Sie können die vorkonfigurierten Lösungen als „Solution Accelerators“ für folgende Zwecke verwenden:

* Als Ausgangspunkt für eigene IoT-Lösungen.
* Zum Erlernen der allgemeinen Muster beim Entwerfen und Entwickeln von IoT-Lösungen.

Derzeit sind drei vorkonfigurierte Lösungen verfügbar:

* [Remoteüberwachung](iot-suite-remote-monitoring-explore.md)
* [Vorhersagbarer Wartungsbedarf](iot-suite-predictive-overview.md)
* [Verbundene Factory](iot-suite-connected-factory-overview.md)

In der folgenden Tabelle wird gezeigt, welchen IoT-Features die Lösungen zugeordnet sind:

| Lösung | Datenerfassung | Geräteidentität | Geräteverwaltung | Verarbeitung im Edge-Bereich | Befehl und Steuerung | Regeln und Aktionen | Predictive Analytics |
| ------------------------------------------------------------ | -- | -- | -- | -- | -- | -- | -- |
| [Remoteüberwachung](iot-suite-remote-monitoring-explore.md)  |Ja |Ja |Ja |-   |Ja |Ja |-   |
| [Vorhersagbarer Wartungsbedarf](iot-suite-predictive-overview.md)   |Ja |Ja |-   |-   |Ja |Ja |Ja |
| [Verbundene Factory](iot-suite-connected-factory-overview.md) |Ja |Ja |Ja |Ja |Ja |Ja |-   |

* *Datenerfassung:*Skalierbarer Dateneingang in der Cloud
* *Geräteidentität:* Verwalten eindeutiger Geräteidentitäten und Steuern des Gerätezugriffs auf die Lösung.
* *Geräteverwaltung:* Verwalten von Gerätemetadaten und Ausführen von Vorgängen wie Geräteneustarts und Firmwareupgrades.
* *Befehl und Steuerung:* Senden von Nachrichten aus der Cloud an ein Gerät, um eine Aktion des Geräts zu veranlassen.
* *Regeln und Aktionen:* Das Lösung-Back-End verwendet Regeln, um auf bestimmte D2C-Daten (Device-to-Cloud) zu reagieren.
* *Predictive Analytics:* Das Lösungs-Back-End analysiert D2C-Daten, um vorherzusagen, wann bestimmte Aktionen ausgeführt werden sollen. Beispiel: Analysieren von Telemetriedaten eines Flugzeugtriebwerks, um zu bestimmen, wann die Triebwerkwartung erforderlich ist.

> [!NOTE]
> Informationen zum Bereitstellen einer vorkonfigurierten Lösung und zu ihrer Anpassung finden Sie unter [Microsoft Azure IoT Suite](https://www.azureiotsuite.com/).

## <a name="azure-services"></a>Azure-Dienste

Wenn Sie eine vorkonfigurierte Lösung bereitstellen, werden während des Bereitstellungsprozesses einige Azure-Dienste konfiguriert. In der folgenden Tabelle werden die Dienste angezeigt, die in den vorkonfigurierten Lösungen verwendet werden:

|                      | Remoteüberwachung  | Vorhersagbarer Wartungsbedarf | Verbundene Factory |
| -------------------- | ------------------ | ---------------------- | ----------------- |
| IoT Hub              | Ja                |                        | Ja               |
| Event Hubs           |                    | Ja                    |                   |
| Time Series Insights |                    |                        | Ja               |
| Containerdienste   | Ja                |                        | Ja               |
| Stream Analytics     |                    | Ja                    |                   |
| Web-Apps             | Ja                | Ja                    | Ja               |
| Cosmos DB            | Ja                | Ja                    | Ja               |
| Azure-Tabellen         |                    | Ja                    | Ja               |

* [Azure IoT Hub](../iot-hub/index.md): Dieser Dienst stellt die D2C- und C2D-Messagingfunktionen (Gerät zu Cloud und Cloud zu Gerät) und fungiert als Gateway für die Cloud und andere wichtige Dienste der IoT-Suite. Der Dienst ermöglicht das Empfangen von Nachrichten von Ihren verwendeten Geräten und das Senden von Befehlen an diese Geräte. Außerdem können Sie mit dem Dienst Ihre [Geräte verwalten](../iot-hub/iot-hub-device-management-overview.md). Beispielsweise können Sie für Geräte, die mit dem Hub verbunden sind, die Konfiguration, einen Neustart oder das Zurücksetzen auf die Werkseinstellungen durchführen.
* [Azure Event Hubs](../event-hubs/index.md): Dieser Dienst ermöglicht eine Ereigniserfassung in der Cloud in hohem Umfang. Siehe [Vergleich zwischen Azure IoT Hub und Azure Event Hubs](../iot-hub/iot-hub-compare-event-hubs.md).
* [Azure Time Series Insights](../time-series-insights/index.md): Dieser Dienst wird von den vorkonfigurierten Lösungen verwendet, um die Telemetriedaten Ihrer Geräte zu analysieren und anzuzeigen.
* [Azure Container Service](../container-service/index.yml): Dieser Dienst hostet und verwaltet die Microservices in den vorkonfigurierten Lösungen.
* [Azure Cosmos DB](../cosmos-db/index.yml) und [Azure Storage](../storage/index.yml) für die Datenspeicherung.
* [Azure Stream Analytics](../stream-analytics/index.md): Die vorkonfigurierte Lösung für die vorbeugende Wartung nutzt diesen Dienst zum Verarbeiten von eingehenden Telemetriedaten, Durchführen der Aggregation und Erkennen von Ereignissen. Diese vorkonfigurierte Lösung verwendet Stream Analytics auch für die Verarbeitung informativer Meldungen, die Daten wie Metadaten oder Antworten auf Befehle von Geräten enthalten.
* [Azure-Web-Apps](../app-service/index.yml) zum Hosten des benutzerdefinierten Anwendungscodes in den vorkonfigurierten Lösungen.

Eine Übersicht über die Architektur einer typischen IoT-Lösung finden Sie unter [Microsoft Azure und das Internet der Dinge (IoT)](iot-suite-what-is-azure-iot.md).

## <a name="whats-new-in-preconfigured-solutions"></a>Welche Neuigkeiten für vorkonfigurierte Lösungen gibt es?

Microsoft aktualisiert die vorkonfigurierten Lösungen in eine neue Architektur auf Basis von Microservices. In der folgenden Tabelle wird der aktuelle Status der vorkonfigurierten Lösungen angezeigt:

| Vorkonfigurierte Lösung | Architektur  | Sprachen     |
| ---------------------- | ------------- | ------------- |
| Remoteüberwachung      | Microservices | Java und .NET |
| Vorhersagbarer Wartungsbedarf | MVC           | .NET          |
| Verbundene Factory      | MVC           | .NET          |

In den folgenden Abschnitten wird beschrieben, welche Neuigkeiten es für die vorkonfigurierten Lösungen auf Basis von Microservices gibt:

### <a name="microservices"></a>Microservices

Für die neue Version der vorkonfigurierten Lösung für die Remoteüberwachung wird eine Microservices-Architektur verwendet. Diese vorkonfigurierte Lösung besteht aus mehreren Microservices, z.B. einem *IoT Hub-Manager* und einem *Speicher-Manager*. Sowohl Java- als auch .NET-Versionen jedes Microservice mit der dazugehörigen Entwicklerdokumentation sind als Download verfügbar. Weitere Informationen zu den Microservices finden Sie unter [Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung](iot-suite-remote-monitoring-sample-walkthrough.md).

Diese Microservices-Architektur ist ein bewährtes Muster für Cloudlösungen mit folgenden Eigenschaften:

* Skalierbar
* Erweiterbar
* Leicht verständlich
* Einzelne Dienste gegen Alternativen austauschbar

> [!TIP]
> Weitere Informationen zu Microservice-Architekturen finden Sie unter [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (.NET-Anwendungsarchitektur) und [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices: Eine Anwendungsrevolution auf Cloudbasis).

Wenn Sie die neue Version der Remoteüberwachung bereitstellen, müssen Sie eine der folgenden Bereitstellungsoptionen auswählen:

* **Basic:** Kostengünstigere Version für Demonstrationszwecke oder zum Testen einer Bereitstellung. Alle Microservices werden auf einem einzelnen virtuellen Azure-Computer bereitgestellt.
* **Enterprise:** Erweiterte Infrastrukturbereitstellung zum Entwickeln einer Produktionsbereitstellung. Der Azure Container Service stellt die Microservices auf mehreren virtuellen Azure-Computern bereit. Kubernetes orchestriert die Docker-Container, die die einzelnen Microservices hosten.

### <a name="language-choices-java-and-net"></a>Sprachauswahl: Java und .NET

Implementierungen der einzelnen Microservices sind sowohl für Java als auch für .NET verfügbar. Wie auch beim .NET-Code, handelt es sich beim Java-Quellcode um Open Source-Code, den Sie an Ihre jeweiligen Anforderungen anpassen können:

* [Remoteüberwachung – .NET-GitHub-Repository](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Remoteüberwachung – Java-GitHub-Repository](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

Falls Sie sich Implementierungen weiterer Sprachen wünschen, können Sie unter [Azure IoT User Voice](https://feedback.azure.com/forums/321918-azure-iot) eine Anfrage hinterlassen.

### <a name="react-user-interface-framework"></a>React-Benutzeroberflächen-Framework

Die Benutzeroberfläche wird basierend auf der [React](https://facebook.github.io/react/)-JavaScript-Bibliothek erstellt. Der Quellcode ist Open Source-Code und kann von Ihnen heruntergeladen und angepasst werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine Übersicht über die vorkonfigurierten IoT Suite-Lösungen erhalten haben, sind hier die vorgeschlagenen nächsten Schritte für die einzelnen vorkonfigurierten Lösungen aufgeführt:

* [Explore the capabilities of the remote monitoring preconfigured solution](iot-suite-remote-monitoring-explore.md) (Erkunden der Funktionen der vorkonfigurierten Lösung für die Remoteüberwachung)
* [Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf](iot-suite-predictive-overview.md)
* [Erste Schritte mit der vorkonfigurierten Connected Factory-Lösung](iot-suite-connected-factory-overview.md)

Weitere Informationen zu den IoT-Lösungsarchitekturen finden Sie unter [Microsoft Azure IoT services: Reference Architecture](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf) (Microsoft Azure IoT-Dienste: Referenzarchitektur).
