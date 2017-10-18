---
title: Private Docker-Containerregistrierungen in Azure
description: "Enthält eine Einführung in den Dienst „Azure-Containerregistrierung“ und die Bereitstellung von cloudbasierten, verwalteten, privaten Docker-Registrierungen."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 664696d2f355609c76477765c2238c6d62253482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Einführung in private Docker-Containerregistrierungen in Azure

Die Azure-Containerregistrierung ist ein verwalteter Dienst vom Typ [Docker-Registrierung](https://docs.docker.com/registry/), der auf Version 2.0 der Open Source-Docker-Registrierung basiert. Erstellen und verwalten Sie Azure-Containerregistrierungen, um Ihre privaten [Docker-Container](https://www.docker.com/what-docker)images zu speichern und zu verwalten. Verwenden Sie Containerregistrierungen in Azure mit Ihren vorhandenen Containerentwicklungs- und Bereitstellungspipelines, und nutzen Sie das umfangreiche Wissen der Docker-Community.

Hintergrundinformationen zu Docker und Containern finden Sie im [Docker-Benutzerhandbuch](https://docs.docker.com/engine/userguide/).

## <a name="use-cases"></a>Anwendungsfälle
Rufen Sie Images aus einer Azure-Containerregistrierung für verschiedene Bereitstellungsziele ab:

* **Skalierbare Orchestrierungssysteme** zum Verwalten von Anwendungen in Containern über Cluster mit Hosts hinweg, z.B. [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) und [Kubernetes](http://kubernetes.io/docs/).
* **Azure-Dienste**, die die bedarfsorientierte Erstellung und Ausführung von Anwendungen unterstützen, z.B. [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/) und andere.

Entwickler können im Rahmen eines Workflows der Containerentwicklung auch eine Pushübertragung in eine Containerregistrierung durchführen. Sie können Daten beispielsweise mit einem Tool für Continuous Integration und Entwicklung an eine Containerregistrierung wie z.B. [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) oder [Jenkins](https://jenkins.io/) übertragen.

## <a name="key-concepts"></a>Wichtige Begriffe
* **Registrierung**: Erstellen Sie in Ihrem Azure-Abonnement eine oder mehrere Containerregistrierungen. Jede Registrierung verfügt über ein standardmäßiges Azure-[Speicherkonto](../storage/common/storage-introduction.md) an demselben Standort. Nutzen Sie den lokalen Speicher in räumlicher Nähe zu Ihren Containerimages, indem Sie eine Registrierung an demselben Azure-Standort wie Ihre Bereitstellungen erstellen. Ein vollständig qualifizierter Registrierungsname hat folgendes Format: `myregistry.azurecr.io`.

  Sie [steuern den Zugriff](container-registry-authentication.md) auf eine Containerregistrierung mit einem auf Azure Active Directory basierenden [Dienstprinzipal](../active-directory/active-directory-application-objects.md) oder einem bereitgestellten Administratorkonto. Führen Sie den Standardbefehl `docker login` aus, um die Authentifizierung für eine Registrierung durchzuführen.

* **Verwaltete Registrierung**: Bei der Registrierungserstellung haben Sie die Wahl zwischen einer verwalteten Registrierung und einer Registrierung, die auf Ihrem eigenen Speicherkonto basiert. Verwaltete Registrierungen bieten zusätzliche Funktionen in drei SKUs: Basic, Standard und Premium. Die Images in diesen SKUs werden in Azure-Speicherkonten gespeichert, die vom Azure Container Registry-Dienst verwaltet werden, was die Zuverlässigkeit erhöht und die Verwendung neuer Funktionen ermöglicht. Neue Funktionen umfassen Webhookintegration, Repositoryauthentifizierung mit Azure Active Directory und Unterstützung von Löschfunktionen.

* **Repository**: Eine Registrierung enthält mindestens ein Repository, also eine Gruppe von Containerimages. Die Azure-Containerregistrierung unterstützt Repositorynamespaces mit mehreren Ebenen. Mit Namespaces mit mehreren Ebenen können Sie Sammlungen mit Images für eine bestimmte App oder eine Sammlung von Apps für bestimmte Entwicklungs- oder Betriebsteams gruppieren. Beispiel:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` stellt ein unternehmensweites Image dar.
  * `myregistry.azurecr.io/warrantydept/dotnet-build` stellt ein Image dar, das zum Erstellen von .NET-Apps verwendet wird, die für die Garantieabteilung freigegeben werden.
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` stellt ein Webimage dar, das in der App „customersubmissions“ gruppiert und im Besitz der Garantieabteilung ist.

* **Image**: Jedes Image wird in einem Repository gespeichert und ist eine schreibgeschützte Momentaufnahme eines Docker-Containers. Azure-Containerregistrierungen können sowohl Windows- als auch Linux-Images enthalten. Sie steuern Imagenamen für alle Containerbereitstellungen. Verwenden Sie [Docker-Standardbefehle](https://docs.docker.com/engine/reference/commandline/), um Images in ein Repository zu übertragen (Push) oder ein Image aus einem Repository abzurufen (Pull).

* **Container**: Ein Container definiert eine Softwareanwendung und ihre Abhängigkeiten innerhalb eines vollständigen Dateisystems, einschließlich Code, Laufzeit, Systemtools und Bibliotheken. Führen Sie Docker-Container basierend auf Windows- oder Linux-Images aus, die Sie aus einer Containerregistrierung abrufen. Für Container, die auf demselben Computer ausgeführt werden, wird derselbe Betriebssystemkernel genutzt. Docker-Container sind ohne Einschränkungen für alle gängigen Linux-Distributionen sowie für macOS und Windows portierbar.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen einer Containerregistrierung mit dem Azure-Portal](container-registry-get-started-portal.md)
* [Erstellen einer Containerregistrierung mit der Azure-Befehlszeilenschnittstelle](container-registry-get-started-azure-cli.md)
* [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)
* Informationen zum Erstellen eines Workflows mit Continuous Integration und Continuous Deployment unter Verwendung von Visual Studio Team Services, Azure Container Service und Azure Container Registry finden Sie unter [Vollständige CI/CD-Pipeline zum Bereitstellen einer Anwendung mit mehreren Containern in Azure Container Service mit Docker Swarm mithilfe von Visual Studio Team Services](../container-service/dcos-swarm/container-service-docker-swarm-setup-ci-cd.md).