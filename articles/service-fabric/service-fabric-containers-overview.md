---
title: "Übersicht über Service Fabric und Container | Microsoft-Dokumentation"
description: "Der Artikel enthält eine Übersicht über Service Fabric und die Nutzung von Containern zur Bereitstellung von Microserviceanwendungen. Dieser Artikel enthält eine Übersicht über die Verwendungsweise von Containern sowie über die verfügbaren Funktionen in Service Fabric."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/20/2017
ms.author: msfussell
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: f47a855b94a29a2e9bbf4ca509e68612423aa65d
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="service-fabric-and-containers"></a>Service Fabric und Container
> [!NOTE]
> Das Bereitstellen von Containern für einen Service Fabric-Cluster unter Windows 10 wird noch nicht unterstützt. 
>   

## <a name="introduction"></a>Einführung
Azure Service Fabric ist ein [Orchestrator](service-fabric-cluster-resource-manager-introduction.md) von Diensten in einem Cluster von Computern, der seit Jahren für zahlreiche Dienste bei Microsoft verwendet und optimiert wird. Dienste können auf vielerlei Arten entwickelt werden – von der Nutzung von [Service Fabric](service-fabric-choose-framework.md)-Programmiermodellen bis zur Bereitstellung von [ausführbaren Gastanwendungsdateien](service-fabric-deploy-existing-app.md). Standardmäßig werden diese Dienste von Service Fabric als Prozesse bereitgestellt und aktiviert. Prozesse ermöglichen die schnellste Aktivierung und Nutzung von Ressourcen in einem Cluster mit der höchsten Dichte. Service Fabric kann Dienste auch in Containerimages bereitstellen. Wichtig ist, dass Sie in derselben Anwendung Dienste in Prozessen und Dienste in Containern mischen können.   

## <a name="what-are-containers"></a>Was sind Container?
Container sind gekapselte, individuell bereitstellbare Komponenten, die als isolierte Instanzen in demselben Kernel ausgeführt werden. Hierbei wird die von einem Betriebssystem bereitgestellte Virtualisierung genutzt. Daher kann jede Anwendung sowie ihre Laufzeit und ihre Abhängigkeiten und Systembibliotheken in einem Container mit privatem Vollzugriff auf die eigene isolierte Containersicht von Betriebssystemkonstrukten ausgeführt werden. Zusammen mit der Portabilität ist dieser hohe Grad an Sicherheit und Ressourcenisolierung der Hauptvorteil bei der Verwendung von Containern mit Service Fabric. Unter Service Fabric werden Dienste ansonsten in Prozessen ausgeführt.

Container sind eine Virtualisierungstechnologie, bei der das zugrunde liegende Betriebssystem von Anwendungen virtualisiert wird. Sie stellen eine unveränderliche Umgebung für Anwendungen zur Ausführung mit verschiedenen Isolationsgraden dar. Container werden direkt oberhalb des Kernels ausgeführt und verfügen über eine isolierte Ansicht des Dateisystems und anderer Ressourcen. Im Vergleich zu virtuellen Computern haben Container die folgenden Vorteile:

* **Geringe Größe**: Für Container werden nur ein Speicherbereich und Ebenen und Updates verwendet, um die Effizienz zu steigern.
* **Schnell**: Container müssen nicht ein gesamtes Betriebssystem starten. Daher ist ihr Start deutlich schneller und erfolgt üblicherweise innerhalb weniger Sekunden.
* **Portabilität:** Ein in einem Container enthaltenes Anwendungsimage kann für die Ausführung in der Cloud oder lokal, auf virtuellen Computern oder direkt auf physischen Computern portiert werden.
* **Ressourcenkontrolle:** Ein Container kann die physischen Ressourcen beschränken, die er auf seinem Host verbrauchen kann.

## <a name="container-types-and-supported-environments"></a>Containertypen und unterstützte Umgebungen
Service Fabric unterstützt Container unter Linux und Windows. Unter Windows wird auch der Hyper-V-Isolationsmodus unterstützt. 

> [!NOTE]
> Das Bereitstellen von Containern für einen Service Fabric-Cluster unter Windows 10 wird noch nicht unterstützt. 
> 

### <a name="docker-containers-on-linux"></a>Docker-Container unter Linux
Docker bietet High-Level-APIs zum Erstellen und Verwalten von Containern oberhalb von Linux-Kernelcontainern. Docker Hub ist ein zentrales Repository zum Speichern und Abrufen von Containerimages.
Ein Tutorial finden Sie unter [Bereitstellen eines Docker-Containers in Service Fabric](service-fabric-get-started-containers-linux.md).

### <a name="windows-server-containers"></a>Windows Server-Container
Unter Windows Server 2016 werden zwei verschiedene Arten von Containern bereitgestellt, die sich anhand des Isolationsgrads unterscheiden. Windows Server-Container und Docker-Containern ähneln sich, da beide über eine Namespace- und Dateisystemisolation verfügen, den Kernel aber gemeinsam mit dem Host nutzen, auf dem sie ausgeführt werden. Unter Linux wurde diese Isolation üblicherweise mit `cgroups` und `namespaces` erzielt. Windows Server-Container verhalten sich ähnlich.

Windows-Container mit Hyper-V-Unterstützung bieten mehr Isolation und Sicherheit, da kein Container den Betriebssystemkernel mit anderen Containern oder mit dem Host gemeinsam verwendet. Aufgrund dieses höheren Grads an Sicherheitsisolation sind für Hyper-V aktivierte Container für kritischere Szenarios mit mehreren Mandanten geeignet.
Ein Tutorial finden Sie unter [Bereitstellen eines Windows-Containers in Service Fabric](service-fabric-get-started-containers.md).

In der folgenden Abbildung sind die unterschiedlichen Arten der Virtualisierung und der Isolationsgrade dargestellt, die unter dem Betriebssystem verfügbar sind.
![Service Fabric-Plattform][Image1]

## <a name="scenarios-for-using-containers"></a>Szenarien für die Verwendung von Containern
In den folgenden typischen Beispielen ist ein Container eine gute Wahl:

* **IIS-Lift und -Shift:** Wenn bereits [ASP.NET MVC](https://www.asp.net/mvc)-Apps vorhanden sind, die Sie weiterhin verwenden möchten, migrieren Sie sie nicht zu ASP.NET Core, sondern platzieren Sie sie in einem Container. Diese ASP.NET MVC-Apps sind von Internetinformationsdiensten (Internet Information Services, IIS) abhängig. Sie können diese Anwendungen in Containerimages aus dem vorab erstellten IIS-Image verpacken und mit Service Fabric bereitstellen. Informationen zu Windows-Containern finden Sie unter [Container Images on Windows Server (Containerimages unter Windows Server)](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-server).
* **Mischen von Containern und Service Fabric-Microservices:** Verwenden Sie ein vorhandenes Containerimage für einen Teil Ihrer Anwendung. Beispielsweise können Sie den [NGINX-Container](https://hub.docker.com/_/nginx/) für das Web-Front-End Ihrer Anwendung und zustandsbehaftete Dienste für die rechenintensiveren Back-End-Vorgänge verwenden.
* **Reduzieren der Auswirkungen der Beeinträchtigung durch andere Dienste („Noisy Neighbors):** Sie können die Funktion zur Ressourcenkontrolle für Container verwenden, um die Ressourcen einzuschränken, die ein Dienst auf einem Host verwendet. Wenn Dienste eine große Menge von Ressourcen nutzen und die Leistung anderer Dienste beeinträchtigen (z.B. ein Abfragevorgang mit langer Ausführungsdauer), können Sie diese Dienste in Containern mit Ressourcenkontrolle anordnen.

## <a name="service-fabric-support-for-containers"></a>Service Fabric-Unterstützung für Container
Service Fabric unterstützt die Bereitstellung von Docker-Containern in Linux- und Windows Server-Containern unter Windows Server 2016 zusammen mit der Unterstützung für den Hyper-V-Isolationsmodus. 

Im Service Fabric- [Anwendungsmodell](service-fabric-application-model.md)stellt ein Container einen Anwendungshost dar, in dem mehrere Dienstreplikate angeordnet werden. Service Fabric kann alle Container ausführen, und das Szenario ähnelt dem [Gastanwendungsszenario](service-fabric-deploy-existing-app.md), bei dem eine vorhandene Anwendung in einem Container verpackt wird. Dieses Szenario wird für Container häufig angewendet. Beispiele sind etwa das Ausführen einer Anwendung, die in einer beliebigen Sprache oder beliebigen Frameworks erstellt wurde, aber nicht die integrierten Service Fabric-Programmiermodelle verwendet.

Darüber hinaus können Sie [Service Fabric-Dienste innerhalb von Containern](service-fabric-services-inside-containers.md) ausführen. Die Unterstützung für das Ausführen von Service Fabric-Diensten innerhalb von Containern ist derzeit begrenzt, sie wird in zukünftigen Versionen jedoch verbessert.

Service Fabric verfügt über mehrere Containerfunktionen für die Erstellung von Anwendungen, die sich aus Microservices in Containern zusammensetzen. Service Fabric umfasst die folgenden Funktionen für Dienste in Containern:

* Bereitstellung und Aktivierung des Containerimage
* Ressourcenkontrolle einschließlich des standardmäßigen Festlegens von Ressourcenwerten auf Azure-Clustern.
* Repositoryauthentifizierung
* Containerport zum Hosten der Portzuordnung
* Container-zu-Container-Ermittlung und -Kommunikation
* Möglichkeit zum Konfigurieren und Festlegen von Umgebungsvariablen
* Möglichkeit zum Festlegen von Sicherheitsanmeldeinformationen für den Container.
* Auswahl verschiedener Netzwerkmodi für Container.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, was Container sind, dass Service Fabric ein Containerorchestrator ist und dass Service Fabric Features zur Unterstützung von Containern bereitstellt. Im nächsten Schritt arbeiten wir Beispiele für die einzelnen Funktionen durch, um deren Nutzung zu veranschaulichen.

[Erstellen Ihrer ersten Service Fabric-Containeranwendung unter Windows](service-fabric-get-started-containers.md)

[Erstellen Ihrer ersten Service Fabric-Containeranwendung unter Linux](service-fabric-get-started-containers-linux.md)

[Weitere Informationen zu Windows-Containern](https://docs.microsoft.com/en-us/virtualization/windowscontainers/about/)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png

