---
title: Einführung in Azure IoT Solution Accelerators | Microsoft-Dokumentation
description: Enthält eine Beschreibung der Azure IoT Solution Accelerators. Bei IoT Solution Accelerators handelt es sich um vollständige End-to-End-IoT-Lösungen, die fertig für die Bereitstellung sind.
author: dominicbetts
ms.author: dobett
ms.date: 07/24/2018
ms.topic: overview
ms.custom: mvc
ms.service: iot-accelerators
services: iot-accelerators
manager: timlt
ms.openlocfilehash: 7020d8a1756702d8c2b1998eef5a3fc64809ca5e
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308111"
---
# <a name="what-are-azure-iot-solution-accelerators"></a>Was sind Azure IoT Solution Accelerators?

Für eine cloudbasierte IoT-Lösung werden normalerweise benutzerdefinierter Code und mehrere Clouddienste verwendet, um die Gerätekonnektivität, die Datenverarbeitung und -analyse und die Darstellung zu verwalten.

Bei IoT Solution Accelerators handelt es sich um eine Sammlung mit vollständigen IoT-Lösungen, die fertig für die Bereitstellung sind und mit denen häufige IoT-Szenarien implementiert werden, z.B. Remoteüberwachung, verbundene Factory, Predictive Maintenance und Gerätesimulation. Wenn Sie einen Solution Accelerator bereitstellen, enthält die Bereitstellung alle erforderlichen cloudbasierten Dienste sowie den gesamten erforderlichen Anwendungscode.

Die Solution Accelerators sind Ausgangspunkte für Ihre eigenen IoT-Lösungen. Der Quellcode für alle Solution Accelerators ist Open-Source-Code und auf GitHub verfügbar. Wir empfehlen Ihnen, die Solution Accelerators herunterzuladen und anzupassen, damit sie Ihre Anforderungen erfüllen.

Sie können die Solution Accelerators auch als Lerntools verwenden, bevor Sie eine benutzerdefinierte IoT-Lösung selbst neu erstellen. Mit den Solution Accelerators werden bewährte Methoden für cloudbasierte IoT-Lösungen implementiert, an die Sie sich halten können.

Der Anwendungscode in den einzelnen Solution Accelerators enthält eine Web-App, mit der Sie den Solution Accelerator verwalten können.

## <a name="supported-iot-scenarios"></a>Unterstützte IoT-Szenarien

Derzeit sind vier Solution Accelerators verfügbar, die Sie bereitstellen können:

### <a name="remote-monitoring"></a>Remoteüberwachung

Verwenden Sie diesen Solution Accelerator, um Telemetriedaten von mehreren Remotegeräten zu erfassen und zu steuern. Beispiele für Geräte sind Kühlsysteme, die beim Kunden installiert sind, oder Ventile in abgelegenen Pumpstationen.

Sie können das Remoteüberwachungs-Dashboard verwenden, um die Telemetriedaten Ihrer verbundenen Geräte anzuzeigen, neue Geräte bereitzustellen oder die Firmware auf Ihren verbundenen Geräten zu aktualisieren:

[![Remoteüberwachungslösung-Dashboard](./media/about-iot-accelerators/rm-dashboard-inline.png)](./media/about-iot-accelerators/rm-dashboard-expanded.png#lightbox)

### <a name="connected-factory"></a>Verbundene Factory

Verwenden Sie diesen Solution Accelerator, um Telemetriedaten von Industrieanlagen mit einer Schnittstelle vom Typ [OPC Unified Architecture](https://opcfoundation.org/about/opc-technologies/opc-ua/) zu erfassen und zu steuern. Beispiele für Industrieanlagen sind Montage- und Teststationen einer Fertigungsstrecke.

Sie können das Dashboard für verbundene Factorys zum Überwachen und Verwalten Ihrer Industriegeräte verwenden:

[![Dashboard der Connected Factory-Lösung](./media/about-iot-accelerators/cf-dashboard-inline.png)](./media/about-iot-accelerators/cf-dashboard-expanded.png#lightbox)

### <a name="predictive-maintenance"></a>Predictive Maintenance

Verwenden Sie diesen Solution Accelerator, um vorherzusagen, wann ein Remotegerät voraussichtlich ausfällt. Sie können dann eine Wartung durchführen, bevor der vorhergesagte Fehler auftritt. Für diesen Solution Accelerator werden Machine Learning-Algorithmen genutzt, um über die Gerätetelemetrie Fehler vorherzusagen. Beispiele für Geräte sind Flugzeugtriebwerke oder Aufzüge.

Sie können das Dashboard für vorbeugende Wartung verwenden, um Predictive Maintenance-Analysen anzuzeigen:

[![Dashboard der Connected Factory-Lösung](./media/about-iot-accelerators/pm-dashboard-inline.png)](./media/about-iot-accelerators/pm-dashboard-expanded.png#lightbox)

### <a name="device-simulation"></a>Gerätesimulation

Verwenden Sie diesen Solution Accelerator, um mehrere simulierte Geräte auszuführen, die realistische Telemetriedaten generieren. Sie können diesen Solution Accelerator nutzen, um das Verhalten der anderen Solution Accelerators oder Ihre eigenen benutzerdefinierten IoT-Lösungen zu testen.

Sie können die Gerätesimulations-Web-App zum Konfigurieren und Ausführen von Simulationen verwenden:

[![Dashboard der Connected Factory-Lösung](./media/about-iot-accelerators/ds-dashboard-inline.png)](./media/about-iot-accelerators/ds-dashboard-expanded.png#lightbox)

## <a name="design-principles"></a>Entwurfsprinzipien

Alle Solution Accelerators basieren auf den gleichen Entwurfsprinzipien und -zielen. Sie sind wie folgt konzipiert:

* **Skalierbar**: Sie können Millionen von verbundenen Geräten verknüpfen und verwalten.
* **Erweiterbar**: Sie können Anpassungen vornehmen, um Ihre Anforderungen zu erfüllen.
* **Verständlich**: Sie können leicht verstehen, wie sie funktionieren und implementiert werden.
* **Modular**: Sie können Dienste gegen Alternativen austauschen.
* **Sicher**: Sie können die Azure-Sicherheit mit integrierten Features für Konnektivität und Gerätesicherheit kombinieren.

## <a name="architectures-and-languages"></a>Architekturen und Sprachen

Die ursprünglichen Solution Accelerators wurden mit .NET geschrieben, indem eine MVC-Architektur (Model-View-Controller) verwendet wurde. Microsoft führt für die Solution Accelerators die Aktualisierung auf eine neue Microservice-Architektur durch. Die folgende Tabelle zeigt den aktuellen Status der Solution Accelerators mit Links zu den GitHub-Repositorys:

| Solution Accelerator   | Architecture  | Sprachen     |
| ---------------------- | ------------- | ------------- |
| Remoteüberwachung      | Microservices | [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) und [.NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) |
| Predictive Maintenance | MVC           | [.NET](https://github.com/Azure/azure-iot-predictive-maintenance)          |
| Verbundene Factory      | MVC           | [.NET](https://github.com/Azure/azure-iot-connected-factory)          |
| Gerätesimulation      | Microservices | [.NET](https://github.com/Azure/device-simulation-dotnet)          |

Weitere Informationen zu Microservice-Architekturen finden Sie unter [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (.NET-Anwendungsarchitektur) und [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices: Eine Anwendungsrevolution auf Cloudbasis).

## <a name="deployment-options"></a>Bereitstellungsoptionen

Sie können die Solution Accelerators über die Website [Microsoft Azure IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators#) oder mithilfe der Befehlszeile bereitstellen.

Für die Bereitstellung der Solution Accelerators für Remoteüberwachung stehen die folgenden Konfigurationen zur Verfügung:

* **Standard:** Erweiterte Infrastrukturbereitstellung zum Entwickeln einer Produktionsbereitstellung. Der Azure Container Service stellt die Microservices auf mehreren virtuellen Azure-Computern bereit. Kubernetes orchestriert die Docker-Container, die die einzelnen Microservices hosten.
* **Basic:** Kostengünstigere Version für Demonstrationszwecke oder zum Testen einer Bereitstellung. Alle Microservices werden auf einem einzelnen virtuellen Azure-Computer bereitgestellt.
* **Lokal:** Lokale Computerbereitstellung für das Testen und die Entwicklung. Bei diesem Ansatz werden die Microservices in einem lokalen Docker-Container bereitgestellt, und die Verbindung mit IoT Hub-, Azure Cosmos DB- und Azure-Speicherdiensten in der Cloud wird hergestellt.

Die Kosten für die Ausführung eines Solution Accelerators ergeben sich aus den zusammengefassten [Kosten der zugrunde liegenden Azure-Dienste](https://azure.microsoft.com/pricing). Die Details der verwendeten Azure-Dienste werden angezeigt, wenn Sie Ihre Bereitstellungsoptionen auswählen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Schnellstarts an, wenn Sie einen der IoT-Solution Accelerators testen möchten:

* [Testen einer Lösung für die Remoteüberwachung](quickstart-remote-monitoring-deploy.md)
* [Testen einer Lösung für verbundene Factorys](quickstart-connected-factory-deploy.md)
* [Testen einer Predictive Maintenance-Lösung](quickstart-predictive-maintenance-deploy.md)
* [Testen einer Gerätesimulationslösung](quickstart-device-simulation-deploy.md)
