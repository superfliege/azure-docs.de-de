---
title: Übersicht über Azure Service Fabric Mesh | Microsoft Docs
description: Informationen zu Azure Service Fabric Mesh. Mit Service Fabric Mesh können Sie Ihre Anwendung bereitstellen und skalieren, ohne sich um die Infrastrukturanforderungen Ihrer Anwendung kümmern zu müssen.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 10/1/2018
ms.topic: overview
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 44c58e09c478c9dba9143b10d150babc553f4695
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337679"
---
# <a name="what-is-service-fabric-mesh"></a>Was ist Service Fabric Mesh?

Dieses Video enthält eine kurze Übersicht über Service Fabric Mesh.
> [!VIDEO https://www.youtube.com/embed/7qWeVGzAid0]

Azure Service Fabric Mesh ist ein vollständig verwalteter Dienst, der es Entwicklern ermöglicht, Microservicesanwendungen zu implementieren, ohne virtuelle Computer, Speicher oder Netzwerke verwalten zu müssen. Anwendungen, die in Service Fabric Mesh gehostet werden, werden ausgeführt und skaliert, ohne dass Sie sich Gedanken über die Infrastruktur machen müssen.  Service Fabric Mesh besteht aus Clustern mit Tausenden von Computern.  Alle Clustervorgänge werden vor dem Entwickler verborgen. Laden Sie einfach Ihren Code hoch, und geben Sie die benötigten Ressourcen, Verfügbarkeitsanforderungen und Ressourceneinschränkungen an.  Service Fabric Mesh ordnet automatisch die Infrastruktur zu und behandelt auch Infrastrukturausfälle, um sicherzustellen, dass Ihre Anwendungen hoch verfügbar sind. Sie müssen sich nur um die Integrität und die Reaktionsfähigkeit der Anwendung, aber nicht um die Infrastruktur kümmern.  

[!INCLUDE [preview note](./includes/include-preview-note.md)]

Dieser Artikel bietet einen Überblick über die wichtigsten Vorteile von Service Fabric Mesh.

## <a name="great-developer-experience"></a>Großartige Entwicklerumgebung

Service Fabric Mesh unterstützt alle Programmiersprachen und Frameworks, die in einem Container ausgeführt werden können. Die Unterstützung von Visual Studio 2017 und Visual Studio Code-Tools bietet eine leistungsstarke Bearbeitungs- und Debugumgebung für .NET- und .NET-Core-Anwendungen. 

Mit Service Fabric Mesh können Sie die folgenden Aufgaben ausführen:

- „Optimieren und Verschieben“ vorhandener Anwendungen in Container, um Ihre aktuellen Anwendungen mit entsprechender Skalierung zu modernisieren und auszuführen.
- Erstellen und Bereitstellen von neuen Microserviceanwendungen mit entsprechender Skalierung in Azure.  Integration in andere Azure-Dienste oder vorhandene Anwendungen, die in Containern ausgeführt werden. Jeder Microservice ist Teil einer sicheren, netzwerkisolierten Anwendung mit Richtlinien zur Ressourcenverwaltung, die für CPU-Kerne, Arbeitsspeicher, Datenträgerspeicher und mehr definiert werden.
- Integration in vorhandene und Erweitern vorhandener Anwendungen ohne Änderungen an diesen Anwendungen vornehmen zu müssen. Verwenden Ihres eigenen virtuellen Netzwerks, um vorhandene Anwendungen mit der neuen Anwendung zu verbinden.  
- Modernisieren Ihrer vorhandenen Clouddienstanwendungen durch die Migration zu Service Fabric Mesh.  

## <a name="simple-operational-lifecycle"></a>Einfacher operativer Lebenszyklus

Einfache Verwaltung aktuell ausgeführter Anwendungen, einschließlich Anwendungsupgrades und Versionsverwaltung, Überwachen von Anwendungen und Debuggen in Produktionsumgebungen. Diese Anwendungen können aus einem einzelnen Microservice oder mehreren Microservices bestehen, die in ihrem eigenen Netzwerk isoliert sind. Anwendungen werden effizient mit schnellen Bereitstellungs-, Platzierungs- und Failoverzeiten ausgeführt.

Mit Service Fabric Mesh können Sie die folgenden Aufgaben ausführen:

- Bereitstellen und Verwalten von Anwendungen ohne explizit Infrastruktur bereitstellen und verwalten zu müssen.  Service Fabric Mesh stellt die zugrunde liegende Infrastruktur für Sie bereit, wendet Upgrades und Patches an und übernimmt die Verwaltung.
- Einrichten von Continuous Integration mit den integrierten Tools zur einfachen Paketerstellung und Bereitstellung von Anwendungen.
- Nutzen aller Features von Azure Resource Manager-Ressourcen (z.B. Überwachungsprotokolle und [rollenbasierte Zugriffssteuerung](/azure/role-based-access-control/overview)), da alle Ressourcen, z.B. Anwendungen, Dienste, Geheimnisse usw., die Sie für den Service Fabric Mesh-Dienst in Azure bereitstellen, Azure Resource Manager-Ressourcen sind.
- Bereitstellen und Verwalten von Ressourcen mithilfe des [Azure-Portals](https://portal.azure.com), Resource Manager-Vorlagen oder der Azure CLI/PowerShell-Bibliotheken.
- Einrichten von operativer Überwachung und Warnungen mithilfe von [Application Insights](/azure/application-insights/) (oder Ihrem bevorzugten Tool) zum Erfassen von Betriebs- und Diagnoseablaufverfolgungen von der Plattform.
- Zugreifen auf aus dem Anwendungsmodell ausgegebene Anwendungsdiagnoseinformationen mithilfe von [Application Insights](/azure/application-insights/) oder dem Tool Ihrer Wahl.
- Optimieren der Nutzung von Ressourcen durch Angeben von Regeln für automatische Skalierung für die Dienste in der Anwendungsdefinition.

## <a name="mission-critical-platform-capabilities"></a>Unternehmenswichtige Plattformfunktionen

Service Fabric Mesh erstellt eine Sammlung von Clustern, die [Azure-Verfügbarkeitszonen](/azure/availability-zones/az-overview) und/oder geopolitische regionale Grenzen umfassen. Anwendungen werden mit einer Sammlung von Absichten wie Skalierung, Hardwareanforderungen und Anforderungen an Dauerhaftigkeit sowie Sicherheitsrichtlinien beschrieben.  Wenn die Anwendung bereitgestellt wird, findet Service Fabric Mesh den optimalen Ort für die Ausführung.

Mit Service Fabric Mesh können Sie die folgenden Aufgaben ausführen:

- Profitieren von Hochverfügbarkeit, Skalierung, Ermittelbarkeit, Orchestrierung, Nachrichtenrouting, zuverlässigem Messaging, Upgrades ohne Ausfallzeit, Verwaltung von Sicherheit/geheimen Schlüsseln, Notfallwiederherstellung, Zustandsverwaltung, Konfigurationsverwaltung und verteilten Transaktionen.
- Auswählen zwischen mehreren Anwendungsmodellen beim Erstellen von Anwendungen.
- Verwenden von Plattformfunktionen, die über REST-Endpunkte bereitgestellt werden, indem sprachspezifische Bindungen genutzt werden, die mit Swagger generiert werden.
- Bereitstellen von Anwendungen über [Verfügbarkeitszonen](/azure/availability-zones/az-overview) und mehrere Regionen hinweg für Geozuverlässigkeit.
- Verwenden aller Sicherheits- und Konformitätsfeatures, die Azure bietet.

## <a name="next-steps"></a>Nächste Schritte

Es sind nur einige Schritte erforderlich, um ein Beispielprojekt mit Visual Studio bereitzustellen. Weitere Informationen finden Sie unter [Erstellen einer ASP.NET Core-Website](service-fabric-mesh-quickstart-dotnet-core.md). 

Antworten auf [häufig gestellte Fragen](service-fabric-mesh-faq.md)


<!-- Links -->

[service-fabric-overview]: ../service-fabric/service-fabric-overview.md
