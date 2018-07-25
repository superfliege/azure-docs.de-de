---
title: Terminologie für Azure Service Fabric Mesh | Microsoft-Dokumentation
description: Erfahren Sie mehr über gängige Begriffe im Zusammenhang mit Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 7c3ce5571c54d6c613114ea49999e450934c8ff4
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136197"
---
# <a name="service-fabric-mesh-terminology"></a>Service Fabric Mesh-Terminologie

Azure Service Fabric Mesh ist ein vollständig verwalteter Dienst, der es Entwicklern ermöglicht, Microservicesanwendungen zu implementieren, ohne virtuelle Computer, Speicher oder Netzwerke verwalten zu müssen. In diesem Artikel wird die von Azure Service Fabric Mesh verwendete Terminologie erläutert, damit Sie die in der Dokumentation verwendeten Begriffe verstehen.

## <a name="service-fabric"></a>Service Fabric

Service Fabric ist eine Open-Source-Plattform für verteilte Systeme, die das Packen, Bereitstellen und Verwalten skalierbarer und zuverlässiger Microservices vereinfacht. Service Fabric ist der Orchestrator für Service Fabric Mesh. Service Fabric bietet Optionen für das Erstellen und Ausführen Ihrer Microservicesanwendungen. Sie können ein beliebiges Framework verwenden, um Ihre Dienste zu programmieren, und aus verschiedenen Umgebungen auszuwählen, in denen die Anwendung ausgeführt werden soll.

## <a name="deployment-and-application-models"></a>Bereitstellungs- und Anwendungsmodelle 

Für die Bereitstellung Ihrer Dienste müssen Sie beschreiben, wie sie ausgeführt werden sollen. Service Fabric unterstützt drei Bereitstellungsmodelle:

### <a name="resource-model"></a>Ressourcenmodell
Ressourcen sind alles, was in Service Fabric einzeln bereitgestellt werden kann, einschließlich Anwendungen, Dienste, Netzwerke und Volumes. Ressourcen werden mithilfe einer YAML- oder JSON-Datei mit dem Azure Resource Manager-Bereitstellungsmodell definiert.

Das Ressourcenmodell ist die einfachste Möglichkeit zum Beschreiben Ihrer Service Fabric-Anwendungen. Sein Hauptaugenmerk liegt auf der einfachen Bereitstellung und Verwaltung von Diensten in Containern.

Ressourcen können überall bereitgestellt werden, wo Service Fabric ausgeführt wird.

### <a name="native-model"></a>Natives Modell
Das native Anwendungsmodell bietet Ihren Anwendungen eine Low-level-Vollzugriff auf Service Fabric. Anwendungen und Dienste werden als registrierte Typen in XML-Manifestdateien definiert.

Das native Datenmodell unterstützt das Reliable Services-Framework, das Zugriff auf die Service Fabric-Laufzeit-APIs und Clusterverwaltungs-APIs in C# und Java bietet. Das native Modell unterstützt auch beliebige Container und ausführbare Dateien.

Es wird nicht in der Service Fabric Mesh-Umgebung unterstützt.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) ist Teil des Docker-Projekts. Service Fabric bietet eingeschränkte Unterstützung für die Bereitstellung von Anwendungen mit dem Docker Compose-Modell.

## <a name="environments"></a>Umgebungen

Service Fabric ist eine Open-Source-Plattformtechnologie, auf der mehrere Dienste und Produkte basieren. Microsoft bietet die folgenden Optionen:

 - **Service Fabric Mesh**: Ein vollständig verwalteter Dienst für die Ausführung von Service Fabric-Anwendungen in Microsoft Azure.
 - **Azure Service Fabric**: Das von Azure gehostete Angebot für Service Fabric-Cluster. Es bietet eine Integration zwischen Service Fabric und der Azure-Infrastruktur sowie Upgrades und eine Konfigurationsverwaltung für Service Fabric-Cluster.
 - **Eigenständiges Service Fabric**: Eine Zusammenstellung von Installations- und Konfigurationstools, um [Service Fabric-Cluster überall bereitzustellen](/azure/service-fabric/service-fabric-deploy-anywhere) (lokal oder über einen Cloudanbieter). Nicht von Azure verwaltet.
 - **Service Fabric-Entwicklungscluster**: Eine lokale Entwicklungsumgebung unter Windows, Linux oder Mac für die Entwicklung von Service Fabric-Anwendungen.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Matrix für die Unterstützung von Umgebungen, Frameworks und Bereitstellungsmodellen
Je nach Umgebung werden unterschiedliche Frameworks und Bereitstellungsmodelle unterstützt. In der folgenden Tabelle werden die unterstützten Kombinationen aus Framework und Bereitstellungsmodell beschrieben.

| Typ der Anwendung | Beschrieben von | Azure Service Fabric Mesh | Azure-Dienstcluster (beliebiges Betriebssystem)| Lokaler Cluster: Windows | Lokaler Cluster: Linux | Lokaler Cluster: Mac | Eigenständiger Cluster (Windows)
|---|---|---|---|---|---|---|---|---|---|
| Service Fabric Mesh-Anwendungen | Ressourcenmodell (YAML und JSON) | Unterstützt |Nicht unterstützt | Unterstützt |Nicht unterstützt | Nicht unterstützt | Nicht unterstützt |
|Native Service Fabric-Anwendungen | Natives Anwendungsmodell (XML) | Nicht unterstützt| Unterstützt|Unterstützt|Unterstützt|Unterstützt|Unterstützt|

In der folgenden Tabelle werden die verschiedenen Anwendungsmodelle und Tools beschrieben, die sie für Service Fabric bieten.

| Typ der Anwendung | Beschrieben von | Visual Studio 2017 | Visual Studio 2015 | Eclipse | VS-Code | SFCTL | Azure CLI | PowerShell
|---|---|---|---|---|---|---|---|---|---|
| Service Fabric Mesh-Anwendungen | Ressourcenmodell (YAML und JSON) | Unterstützt |Nicht unterstützt |Nicht unterstützt |Nicht unterstützt |Nicht unterstützt | Unterstützt: Nur Mesh-Umgebung | Nicht unterstützt
|Native Service Fabric-Anwendungen | Natives Anwendungsmodell (XML) | Unterstützt| Unterstützt|Unterstützt|Unterstützt|Unterstützt|Unterstützt|Unterstützt|

## <a name="next-steps"></a>Nächste Schritte

In der Übersicht erfahren Sie mehr über Service Fabric Mesh:
- [Übersicht über Service Fabric Mesh](service-fabric-mesh-overview.md)
