---
title: Private Docker-Containerregistrierungen in Azure – Übersicht
description: Enthält eine Einführung in den Dienst „Azure-Containerregistrierung“ und die Bereitstellung von cloudbasierten, verwalteten, privaten Docker-Registrierungen.
services: container-registry
author: stevelas
ms.service: container-registry
ms.topic: overview
ms.date: 03/29/2019
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: 39f643bd66e2a96b0b9b93989d2941a9c30ea7fc
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894012"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Einführung in private Docker-Containerregistrierungen in Azure

Die Azure-Containerregistrierung ist ein verwalteter Dienst vom Typ [Docker-Registrierung](https://docs.docker.com/registry/), der auf Version 2.0 der Open Source-Docker-Registrierung basiert. Erstellen und verwalten Sie Azure-Containerregistrierungen, um Ihre privaten [Docker-Container](https://www.docker.com/what-docker)images zu speichern und zu verwalten.

Verwenden Sie Containerregistrierungen in Azure mit Ihren vorhandenen Containerentwicklungs- und Bereitstellungspipelines. Verwenden Sie Azure Container Registry Build (ACR Build) zum Erstellen von Containerimages in Azure. Erstellen Sie bedarfsgesteuerte oder voll automatisierte Builds mit Triggern zum Ausführen von Commits für Quellcode und zum Aktualisieren des Basisimagebuilds.

Hintergrundinformationen zu Docker und Containern finden Sie im [Docker – Übersicht](https://docs.docker.com/engine/docker-overview/).

## <a name="use-cases"></a>Anwendungsfälle

Rufen Sie Images aus einer Azure-Containerregistrierung für verschiedene Bereitstellungsziele ab:

* **Skalierbare Orchestrierungssysteme** zum Verwalten von Anwendungen in Containern über Cluster mit Hosts hinweg, z.B. [DC/OS](https://kubernetes.io/docs/), [Docker Swarm](https://docs.mesosphere.com/) und [Kubernetes](https://docs.docker.com/swarm/).
* **Azure-Dienste**, die die bedarfsorientierte Erstellung und Ausführung von Anwendungen unterstützen, z. B. [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) und andere.

Entwickler können im Rahmen eines Workflows der Containerentwicklung auch eine Pushübertragung in eine Containerregistrierung durchführen. Sie können Daten beispielsweise mit einem Tool für Continuous Integration und Bereitstellung an eine Containerregistrierung wie z.B. [Azure DevOps Services](https://docs.microsoft.com/azure/devops/) oder [Jenkins](https://jenkins.io/) übertragen.

Konfigurieren Sie ACR Tasks, um Anwendungsimages automatisch neu zu erstellen, wenn ihre Basisimages aktualisiert werden. Verwenden Sie ACR Tasks, um Imagebuilds zu automatisieren, wenn das Team ein Commit für Code an ein Git-Repository ausführt.

## <a name="key-concepts"></a>Wichtige Begriffe

* **Registrierung**: Erstellen Sie in Ihrem Azure-Abonnement eine oder mehrere Containerregistrierungen. Registrierungen sind in drei SKUs verfügbar: [Basic, Standard und Premium](container-registry-skus.md). Diese unterstützen jeweils die Webhook-Integration, die Registrierungsauthentifizierung mit Azure Active Directory sowie Löschfunktionen. Nutzen Sie den lokalen Speicher in räumlicher Nähe zu Ihren Containerimages, indem Sie eine Registrierung an demselben Azure-Standort wie Ihre Bereitstellungen erstellen. Verwenden der [Georeplikation](container-registry-geo-replication.md) von Premium-Registrierungen für erweiterte Replikations- und Containerimageverteilung-Szenarien. Ein vollständig qualifizierter Registrierungsname hat folgendes Format: `myregistry.azurecr.io`.

  Sie [steuern den Zugriff](container-registry-authentication.md) auf eine Containerregistrierung mit einer Azure-Identität, einem auf Azure Active Directory basierenden [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md) oder einem bereitgestellten Administratorkonto. Melden Sie sich mithilfe der Azure-Befehlszeilenschnittstelle oder mit dem Standardbefehl `docker login` bei der Registrierung an.

* **Repository**: Eine Registrierung enthält mindestens ein Repository, das Gruppen von Containerimages speichert. Die Azure-Containerregistrierung unterstützt Repositorynamespaces mit mehreren Ebenen. Mit Namespaces mit mehreren Ebenen können Sie Sammlungen mit Images für eine bestimmte App oder eine Sammlung von Apps für bestimmte Entwicklungs- oder Betriebsteams gruppieren. Beispiel: 

  * `myregistry.azurecr.io/aspnetcore:1.0.1` stellt ein unternehmensweites Image dar.
  * `myregistry.azurecr.io/warrantydept/dotnet-build` stellt ein Image dar, das zum Erstellen von .NET-Apps verwendet wird, die für die Garantieabteilung freigegeben werden.
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` stellt ein Webimage dar, das in der App „customersubmissions“ gruppiert und im Besitz der Garantieabteilung ist.

* **Image**: Jedes Image wird in einem Repository gespeichert und ist eine schreibgeschützte Momentaufnahme eines Docker-kompatiblen Containers. Azure-Containerregistrierungen können sowohl Windows- als auch Linux-Images enthalten. Sie steuern Imagenamen für alle Containerbereitstellungen. Verwenden Sie [Docker-Standardbefehle](https://docs.docker.com/engine/reference/commandline/), um Images in ein Repository zu übertragen (Push) oder ein Image aus einem Repository abzurufen (Pull). Zusätzlich zu Containerimages speichert Azure Container Registry [zugehörige Inhaltsformate](container-registry-image-formats.md) wie [Helm-Diagramme](container-registry-helm-repos.md), die zum Bereitstellen von Anwendungen in Kubernetes verwendet werden.

* **Container**: Ein Container definiert eine Softwareanwendung und ihre Abhängigkeiten innerhalb eines vollständigen Dateisystems, einschließlich Code, Laufzeit, Systemtools und Bibliotheken. Führen Sie Docker-Container basierend auf Windows- oder Linux-Images aus, die Sie aus einer Containerregistrierung abrufen. Für Container, die auf demselben Computer ausgeführt werden, wird derselbe Betriebssystemkernel genutzt. Docker-Container sind ohne Einschränkungen für alle gängigen Linux-Distributionen sowie für macOS und Windows portierbar.

## <a name="azure-container-registry-tasks"></a>Azure Container Registry Tasks

[Azure Container Registry Tasks](container-registry-tasks-overview.md) (ACR Tasks) ist eine Suite mit Features in Azure Container Registry, die optimierte und effiziente Docker-Containerimage-Builds in Azure bietet. Verwenden Sie ACR Tasks, um die Inner Loop-Entwicklungsumgebung in die Cloud zu erweitern, indem Sie `docker build`-Vorgänge in Azure auslagern. Konfigurieren Sie Buildaufgaben, um die Pipeline für das Containerbetriebssystem- und Frameworkpatching zu automatisieren und automatisch Images zu erstellen, wenn das Team ein Commit für Code an die Quellcodeverwaltung ausführt.

[Mehrstufige Aufgaben](container-registry-tasks-overview.md#multi-step-tasks) bieten schrittbasierte Aufgabendefinition und -ausführung für Erstellen, Testen und Patchen von Containerimages in der Cloud. Aufgabenschritte definieren einzelne Containerimagebuild- und Pushvorgänge. Sie können auch die Ausführung eines oder mehrerer Container mit jedem Schritt definieren, wobei der Container als Ausführungsumgebung verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Containerregistrierung mit dem Azure-Portal](container-registry-get-started-portal.md)
* [Erstellen einer Containerregistrierung mit der Azure-Befehlszeilenschnittstelle](container-registry-get-started-azure-cli.md)
* [Automatisieren von Betriebssystem- und Frameworkpatches mit ACR Tasks](container-registry-tasks-overview.md)
