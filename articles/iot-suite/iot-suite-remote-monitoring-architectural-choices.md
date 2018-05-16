---
title: Optionen für die Architektur der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die architekturbezogenen und technischen Auswahlmöglichkeiten für die Remoteüberwachung.
services: iot-suite
suite: iot-suite
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-suite
ms.date: 04/30/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 192a763c01e60d816ae2046587176627fc9d8736
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="remote-monitoring-architectural-choices"></a>Optionen für die Remoteüberwachungsarchitektur

Die Azure-IoT-Remoteüberwachung ist ein Open Source-Solution Accelerator mit MIT-Lizenz, der häufig verwendete IoT-Szenarien ermöglicht, wie z.B. Gerätekonnektivität, Geräteverwaltung oder Streamverarbeitung, sodass Kunden ihren Entwicklungsprozess beschleunigen können.  Die Remoteüberwachung folgt der [hier](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/) veröffentlichten Azure IoT-Referenzarchitektur.  

Dieser Artikel beschreibt die architekturbezogenen und technischen Auswahlmöglichkeiten für jedes Subsystem der Remoteüberwachung und erläutert mögliche Alternativen.  Beachten Sie unbedingt, dass die technischen Optionen, die Sie für die Remoteüberwachung auswählen, nicht die einzige Möglichkeit sind, eine Remoteüberwachung-IoT-Lösung zu implementieren.  Die technische Implementierung stellt die Grundwerte für den Aufbau einer erfolgreichen Anwendung dar und sollte an die Kenntnisse, die Erfahrung und die vertikalen Anwendungsanforderungen einer Kundenlösung angepasst werden.

## <a name="architectural-choices"></a>Architekturoptionen

### <a name="microservices-serverless-and-cloud-native"></a>Microservices, serverlos und cloudbasiert

Die empfohlene Architektur für IoT-Anwendungen ist serverlos, cloudbasiert und besteht aus Microservices.  Die verschiedenen Subsysteme einer IoT-Anwendung sollten als separate Dienste erstellt werden, die unabhängig voneinander bereitgestellt und skaliert werden können.  Diese Attribute ermöglichen mehr Skalierbarkeit und Flexibilität bei der Aktualisierung einzelner Subsysteme und ermöglichen zudem die flexible Auswahl der geeigneten Technologie für jedes Subsystem.  Microservices können mit mehreren Technologien implementiert werden. Es ist z.B. möglich, Containertechnologie wie Docker mit serverloser Technologie wie Azure Functions zu kombinieren oder Microservices in PaaS-Diensten wie Azure App Services zu hosten.

## <a name="core-subsystem-technology-choices"></a>Technologieoptionen für das grundlegende Subsystem

In diesem Abschnitt werden die Technologieoptionen erläutert, die in der Remoteüberwachung für jedes der grundlegenden Subsysteme ausgewählt wurden.

![Diagramm der grundlegenden Subsysteme](media/iot-suite-remote-monitoring-architectural-choices/subsystem.png) 

### <a name="cloud-gateway"></a>Cloudgateway
Azure IoT Hub wird als Cloudgateway für die Remoteüberwachung verwendet.  Die IoT Hub-Instanz bietet eine sichere bidirektionale Kommunikation mit Geräten. Weitere Informationen zu IoT Hub finden Sie [hier](https://azure.microsoft.com/services/iot-hub/). Für die IoT-Gerätekonnektivität werden die .NET Core und Java IoT Hub SDKs verwendet.  Die SDKs bieten Wrapper für die IoT Hub-REST-API und verarbeiten Szenarien wie beispielsweise Wiederholungen, 

### <a name="stream-processing"></a>Datenstromverarbeitung
Für die Datenstromverarbeitung verwendet die Remoteüberwachung Azure Stream Analytics, um eine komplexe Regelverarbeitung durchzuführen.  Für Kunden, die einfachere Regeln wünschen, steht auch ein benutzerdefinierter Microservice mit Unterstützung für die Verarbeitung einfacherer Regeln zur Verfügung. Dieses Setup gehört allerdings nicht zur schlüsselfertigen Bereitstellung. Die Referenzarchitektur empfiehlt die Verwendung von Azure Functions für die einfache Regelverarbeitung und Azure Stream Analytics (ASA) für die komplexe Regelverarbeitung.  

### <a name="storage"></a>Speicher
Cosmos DB wird für alle Speicheranforderungen verwendet: kalter Speicher, warmer Speicher, Regelspeicher und Warnungen. Wir sind derzeit dabei, zu Azure Blob Storage zu wechseln, wie von der Referenzarchitektur empfohlen.  Cosmos DB ist die empfohlene allgemeine warme Speicherlösung für IoT-Anwendungen. Lösungen wie Azure Time Series Insights oder Azure Data Lake sind jedoch in vielen Anwendungsfällen auch geeignet.

### <a name="business-integration"></a>Geschäftliche Integration
Die geschäftliche Integration der Remoteüberwachung ist auf die Erzeugung von Warnungen beschränkt, die in warmem Speicher abgelegt werden. Weitere geschäftliche Integrationen können durch Integration der Lösung in Azure Logic Apps ausgeführt werden.

### <a name="user-interface"></a>Benutzeroberfläche
Die Webbenutzeroberfläche basiert auf JavaScript React.  React bietet ein häufig verwendetes, branchenspezifisches Framework für Benutzeroberflächen und ähnelt anderen beliebten Frameworks wie Angular.  

### <a name="runtime-and-orchestration"></a>Runtime und Orchestrierung
Die Anwendungsruntime für die Subsystemimplementierung in der Remoteüberwachung befindet sich in Docker-Containern, Kubernetes (K8s) fungiert als Orchestrator für die horizontale Hochskalierung.  Diese Architektur ermöglicht eine individuelle Skalierungsdefinition für jedes Subsystem. Allerdings fallen DevOps-Kosten an, um die virtuellen Computer und Container hinsichtlich der Sicherheit auf dem neuesten Stand zu halten.  Als Alternativen zu Docker und K8s können Microservices in PaaS-Diensten (z.B. Azure App Service) gehostet oder Service Fabric, DCOS, Swarm usw. als Orchestrator verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
* Stellen Sie Ihre Remoteüberwachungslösung [hier](https://www.azureiotsuite.com/) bereit.
* Erkunden Sie [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/)- und [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/)-Code in GitHub.  
* Erfahren Sie [hier](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/) mehr über die IoT-Referenzarchitektur.
