---
title: Übersicht über Azure IoT-Solution Accelerators | Microsoft-Dokumentation
description: Hier finden Sie eine Beschreibung der Azure IoT Solution Accelerators und ihrer Architektur mit Links zu zusätzlichen Ressourcen.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 17/01/2018
ms.author: dobett
ms.openlocfilehash: 80192b115f4c6c8f6961fdd4d9fb7f3876fda898
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34362123"
---
# <a name="what-are-the-iot-solution-accelerators"></a>Was sind IoT Solution Accelerators?

Bei den Azure IoT _Solution Accelerators_ handelt es sich um eine Reihe von Lösungen, die Folgendes ermöglichen:

* Bereitstellung in wenigen Minuten
* Schneller Einstieg
* Mögliche Anpassung an Ihre jeweiligen Anforderungen

Alle Solution Accelerators wurden mit den gleichen Prinzipien und Zielsetzungen entworfen.

Das folgende Video bietet einen Überblick über den Solution Accelerator für die Remoteüberwachung:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Meet-the-new-Remote-Monitoring-accelerator-for-Azure-IoT/Player]

## <a name="solution-accelerators-overview"></a>Übersicht über Solution Accelerators

Bei einem Solution Accelerator handelt es sich um eine Open Source-Implementierung allgemeiner IoT-Lösungsmuster, die Sie über Ihr Abonnement in Azure bereitstellen können. Jeder Solution Accelerator kombiniert benutzerdefinierten Code und Azure-Dienste, um bestimmte IoT-Szenarien zu implementieren. Sie können alle Szenarien an Ihre jeweiligen Anforderungen anpassen. Zu diesen Szenarien gehören:

* Visualisieren Sie Daten in einem umfassenden Dashboard, um fundierte Erkenntnisse und Informationen zum Status der Lösung zu erhalten.
* Konfigurieren Sie Regeln und Alarme für die IoT-Gerätetelemetrie im Livezustand.
* Planen Sie Geräteverwaltungsaufträge, z.B. Updates von Software und Konfiguration.
* Stellen Sie Ihre eigenen benutzerdefinierten physischen oder simulierten Geräte bereit.
* Führen Sie die Problembehandlung und -behebung in Ihren Gruppen mit IoT-Geräten durch.

Bei jedem Solution Accelerator handelt es sich um eine vollständige End-to-End-Implementierung, die simulierte oder physische Geräte zum Generieren von Telemetriedaten verwenden kann. Sie können die Solution Accelerators für folgende Zwecke verwenden:

* Als Ausgangspunkt für eigene IoT-Lösungen.
* Zum Erlernen der allgemeinen Muster beim Entwerfen und Entwickeln von IoT-Lösungen.

Derzeit stehen drei Solution Accelerators zur Verfügung:

* [Remoteüberwachung](iot-accelerators-remote-monitoring-explore.md)
* [Predictive Maintenance](../iot-suite/iot-suite-predictive-overview.md)
* [Verbundene Factory](iot-accelerators-connected-factory-overview.md)

In der folgenden Tabelle wird gezeigt, welchen IoT-Features die Lösungen zugeordnet sind:

| Lösung | Datenerfassung | Geräteidentität | Geräteverwaltung | Verarbeitung im Edge-Bereich | Befehl und Steuerung | Regeln und Aktionen | Predictive Analytics |
| ------------------------------------------------------------ | -- | -- | -- | -- | -- | -- | -- |
| [Remoteüberwachung](iot-accelerators-remote-monitoring-explore.md)  |Ja |Ja |Ja |-   |Ja |Ja |-   |
| [Predictive Maintenance](../iot-suite/iot-suite-predictive-overview.md)   |Ja |Ja |-   |-   |Ja |Ja |Ja |
| [Verbundene Factory](iot-accelerators-connected-factory-overview.md) |Ja |- |- |Ja |Ja |Ja |-   |

* *Datenerfassung:* Skalierbarer Dateneingang in der Cloud
* *Geräteidentität:* Verwalten eindeutiger Geräteidentitäten und Steuern des Gerätezugriffs auf die Lösung.
* *Geräteverwaltung:* Verwalten von Gerätemetadaten und Ausführen von Vorgängen wie Geräteneustarts und Firmwareupgrades.
* *Befehl und Steuerung:* Senden von Nachrichten aus der Cloud an ein Gerät, um eine Aktion des Geräts zu veranlassen.
* *Regeln und Aktionen:* Das Lösung-Back-End verwendet Regeln, um auf bestimmte D2C-Daten (Device-to-Cloud) zu reagieren.
* *Predictive Analytics:* Das Lösungs-Back-End analysiert D2C-Daten, um vorherzusagen, wann bestimmte Aktionen ausgeführt werden sollen. Beispiel: Analysieren von Telemetriedaten eines Flugzeugtriebwerks, um zu bestimmen, wann die Triebwerkwartung erforderlich ist.

> [!NOTE]
> Informationen zum Bereitstellen von Solution Accelerators und zu ihrer Anpassung finden Sie unter [Microsoft Azure IoT Solution Accelerators](https://www.azureiotsuite.com/).

## <a name="azure-services"></a>Azure-Dienste

Wenn Sie einen Solution Accelerator bereitstellen, konfiguriert der Bereitstellungsprozess eine Reihe von Azure-Diensten. Die folgende Tabelle zeigt die Dienste, die in den Solution Accelerators verwendet werden:

|                      | Remoteüberwachung  | Predictive Maintenance | Verbundene Factory |
| -------------------- | ------------------ | ---------------------- | ----------------- |
| IoT Hub              | Ja                | Ja                    | Ja               |
| Event Hubs           |                    | Ja                    |                   |
| Time Series Insights |                    |                        | Ja               |
| Containerdienste   | Ja                |                        |                   |
| Stream Analytics     |                    | Ja                    |                   |
| Web-Apps             | Ja                | Ja                    | Ja               |
| Cosmos DB            | Ja                | Ja                    |                    |
| Azure Storage         |                    | Ja                    | Ja               |

> [!NOTE]
> Weitere Informationen zu den Ressourcen, die im Solution Accelerator für die Remoteüberwachung bereitgestellt werden, finden Sie in diesem [Artikel](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/blob/master/README.md#basic-vs-standard-deployments) auf GitHub.

* [Azure IoT Hub](../iot-hub/index.yml): Dieser Dienst stellt die Gerät-zu-Cloud- und Cloud-zu-Gerät-Messagingfunktionen bereit und fungiert als Gateway zur Cloud und andere wichtige Solution Accelerator-Dienste. Der Dienst ermöglicht das Empfangen von Nachrichten von Ihren verwendeten Geräten und das Senden von Befehlen an diese Geräte. Außerdem können Sie mit dem Dienst Ihre [Geräte verwalten](../iot-hub/iot-hub-device-management-overview.md). Beispielsweise können Sie für Geräte, die mit dem Hub verbunden sind, die Konfiguration, einen Neustart oder das Zurücksetzen auf die Werkseinstellungen durchführen.
* [Azure Event Hubs](../active-directory-domain-services/index.md): Dieser Dienst ermöglicht eine Ereigniserfassung in der Cloud in hohem Umfang. Siehe [Vergleich zwischen Azure IoT Hub und Azure Event Hubs](../iot-hub/iot-hub-compare-event-hubs.md).
* [Azure Time Series Insights](../time-series-insights/index.yml): Die Solution Accelerators verwenden diesen Dienst, um die Telemetriedaten von Ihren Geräten zu analysieren und anzuzeigen.
* [Azure Container Service](../container-service/index.yml): Dieser Dienst hostet und verwaltet die Microservices in den Solution Accelerators.
* [Azure Cosmos DB](../cosmos-db/index.yml) und [Azure Storage](../storage/index.yml) für die Datenspeicherung.
* [Azure Stream Analytics](../stream-analytics/index.yml): Die vorkonfigurierte Lösung für die vorbeugende Wartung nutzt diesen Dienst zum Verarbeiten von eingehenden Telemetriedaten, Durchführen der Aggregation und Erkennen von Ereignissen. Diese vorkonfigurierte Lösung verwendet Stream Analytics auch für die Verarbeitung informativer Meldungen, die Daten wie Metadaten oder Antworten auf Befehle von Geräten enthalten.
* [Azure-Web-Apps](../app-service/index.yml) zum Hosten des benutzerdefinierten Anwendungscodes in den vorkonfigurierten Lösungen.

Eine Übersicht über die Architektur einer typischen IoT-Lösung finden Sie unter [Microsoft Azure und das Internet der Dinge (IoT)](iot-accelerators-what-is-azure-iot.md).

## <a name="whats-new-in-solution-accelerators"></a>Was ist neu bei Solution Accelerators?

Microsoft aktualisiert die Solution Accelerators auf eine neue Architektur auf Basis von Microservices. Die folgende Tabelle zeigt den aktuellen Status der Solution Accelerators:

| Solution Accelerator | Architecture  | Sprachen     |
| ---------------------- | ------------- | ------------- |
| Remoteüberwachung      | Microservices | Java und .NET |
| Predictive Maintenance | MVC           | .NET          |
| Verbundene Factory      | MVC           | .NET          |

In den folgenden Abschnitten wird beschrieben, welche neuen Features und Funktionen es für die microservicebasierten Solution Accelerators gibt:

### <a name="microservices"></a>Microservices

Die neue Version des Solution Accelerators für die Remoteüberwachung verwendet eine Microservices-Architektur. Dieser Solution Accelerator besteht aus mehreren Microservices, z.B. einem *IoT Hub-Manager* und einem *Speicher-Manager*. Sowohl Java- als auch .NET-Versionen jedes Microservice mit der dazugehörigen Entwicklerdokumentation sind als Download verfügbar. Weitere Informationen zu den Microservices finden Sie unter [Architektur der vorkonfigurierten Remoteüberwachungslösung](iot-accelerators-remote-monitoring-sample-walkthrough.md).

Diese Microservices-Architektur ist ein bewährtes Muster für Cloudlösungen mit folgenden Eigenschaften:

* Skalierbar
* Erweiterbar
* Leicht verständlich
* Einzelne Dienste gegen Alternativen austauschbar

> [!TIP]
> Weitere Informationen zu Microservice-Architekturen finden Sie unter [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (.NET-Anwendungsarchitektur) und [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices: Eine Anwendungsrevolution auf Cloudbasis).

Wenn Sie die neue Version der Remoteüberwachung bereitstellen, müssen Sie eine der folgenden Bereitstellungsoptionen auswählen:

* **Basic:** Kostengünstigere Version für Demonstrationszwecke oder zum Testen einer Bereitstellung. Alle Microservices werden auf einem einzelnen virtuellen Azure-Computer bereitgestellt.
* **Standard:** Erweiterte Infrastrukturbereitstellung zum Entwickeln einer Produktionsbereitstellung. Der Azure Container Service stellt die Microservices auf mehreren virtuellen Azure-Computern bereit. Kubernetes orchestriert die Docker-Container, die die einzelnen Microservices hosten.

### <a name="language-choices-java-and-net"></a>Sprachauswahl: Java und .NET

Implementierungen der einzelnen Microservices sind sowohl für Java als auch für .NET verfügbar. Wie auch beim .NET-Code, handelt es sich beim Java-Quellcode um Open Source-Code, den Sie an Ihre jeweiligen Anforderungen anpassen können:

* [Remoteüberwachung – .NET-GitHub-Repository](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Remoteüberwachung – Java-GitHub-Repository](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

Falls Sie sich Implementierungen weiterer Sprachen wünschen, können Sie unter [Azure IoT User Voice](https://feedback.azure.com/forums/321918-azure-iot) eine Anfrage hinterlassen.

### <a name="react-user-interface-framework"></a>React-Benutzeroberflächen-Framework

Die Benutzeroberfläche wird basierend auf der [React](https://facebook.github.io/react/)-JavaScript-Bibliothek erstellt. Der Quellcode ist Open Source-Code und kann von Ihnen heruntergeladen und angepasst werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine Übersicht über die IoT Solution Accelerators erhalten haben, finden Sie hier die vorgeschlagenen nächsten Schritte für jeden Solution Accelerator:

* [Erkunden der Funktionen der vorkonfigurierten Remoteüberwachungslösung](iot-accelerators-remote-monitoring-explore.md)
* [Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf](../iot-suite/iot-suite-predictive-overview.md)
* [Erste Schritte mit der vorkonfigurierten Connected Factory-Lösung](iot-accelerators-connected-factory-overview.md)

Weitere Informationen zu den IoT-Lösungsarchitekturen finden Sie unter [Microsoft Azure IoT services: Reference Architecture](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf) (Microsoft Azure IoT-Dienste: Referenzarchitektur).
