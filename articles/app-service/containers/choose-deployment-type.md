---
title: 'Bereitstellung von Azure App Service unter Linux: benutzerdefiniertes Image, mehrere Container oder integriertes Plattformimage?  | Microsoft-Dokumentation'
description: Erwägungen bei der Entscheidung zwischen benutzerdefinierter Docker-Containerbereitstellung, mehreren Containern und einem integrierten Anwendungsframework für App Service unter Linux
keywords: Azure App Service, Web-App, Linux, OSS
services: app-service
documentationCenter: ''
authors: msangapu
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: msangapu
ms.openlocfilehash: c619ae164f8f8b6e94d9061c4346de58bd6cb795
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319437"
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>Benutzerdefiniertes Image, mehrere Container oder integriertes Plattformimage?

[App Service unter Linux](app-service-linux-intro.md) bietet drei verschiedene Möglichkeiten für die Veröffentlichung Ihrer Anwendung im Web:

- **Bereitstellung mit benutzerdefinierten Images:** Wandeln Sie Ihre App in ein Docker-Image um, das alle Dateien und Abhängigkeiten in einem ausführbereiten Paket enthält.
- **Bereitstellung mit mehreren Containern:** „Verpacken“ Sie Ihre App in Docker über mehrere Container mit Docker Compose oder einer Kubernetes-Konfigurationsdatei. Weitere Informationen finden Sie unter [App mit mehreren Containern](#multi-container-apps-supportability).
- **App-Bereitstellung mit einem integrierten Plattformimage:** Unsere integrierten Plattformimages enthalten allgemeine Web-App-Runtimes und Abhängigkeiten wie Node und PHP. Verwenden Sie eine der [Azure App Service-Bereitstellungsmethoden](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) zum Bereitstellen Ihrer App im Speicher Ihrer Web-App, und führen Sie sie dann mithilfe eines integrierten Plattformimages aus.

## <a name="which-method-is-right-for-your-app"></a>Welche Methode ist für Ihre App geeignet? 

Hauptsächlich sind folgende Faktoren zu berücksichtigen:

- **Verfügbarkeit von Docker in Ihrem Entwicklungsworkflow:** Für die Entwicklung benutzerdefinierter Images sind grundlegende Kenntnisse des Docker-Entwicklungsworkflows erforderlich. Zur Bereitstellung eines benutzerdefinierten Images für eine Web-App muss das benutzerdefinierte Image auf einem Repositoryhost wie Docker Hub veröffentlicht werden. Wenn Sie mit Docker vertraut sind und Ihrem Buildworkflow Docker-Aufgaben hinzufügen können oder wenn Sie Ihre App bereits als Docker-Image veröffentlichen, ist ein benutzerdefiniertes Image nahezu immer die beste Wahl.
- **Mehrschichtige Architektur:** Stellen Sie mehrere Container, z.B. eine Webanwendungsebene und eine API-Schicht, bereit, um so die Funktionen zu trennen. 
- **Anwendungsleistung:** Erhöhen Sie die Leistung Ihrer App mit mehreren Containern über eine Cacheebene, z.B. Redis. Wählen Sie die Option für mehrere Container aus, um dies zu erreichen.
- **Besondere Laufzeitanforderungen:** Die integrierten Plattformimages sind dafür vorgesehen, die Anforderungen der meisten Web-Apps zu erfüllen, können aber nur begrenzt angepasst werden. Möglicherweise weist Ihre App besondere Abhängigkeiten oder andere Laufzeitanforderungen auf, die die Möglichkeiten integrierter Images überschreiten.
- **Buildanforderungen:** Mit [Continuous Deployment](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) können Sie Ihre App in Azure direkt aus dem Quellcode ausführen. Es ist kein externer Build- oder Veröffentlichungsprozess erforderlich. Die Anpassbarkeit und die Verfügbarkeit von Buildtools innerhalb der [Kudu](https://github.com/projectkudu/kudu/wiki)-Bereitstellungs-Engine sind jedoch begrenzt. Ihre App kann die Möglichkeiten von Kudu überschreiten, wenn ihre Abhängigkeiten oder Anforderungen an benutzerdefinierte Buildlogik steigen.
- **Anforderungen für Lese-/Schreibzugriff auf den Datenträger:** Allen Web-Apps wird ein Speichervolume für Webinhalte zugeordnet. Dieses durch Azure Storage abgesicherte Volume wird im App-Dateisystem unter `/home` eingebunden. Im Gegensatz zu den Dateien im Containerdateisystem kann auf Dateien im Inhaltsvolume über alle Skalierungsinstanzen einer App zugegriffen werden, und Änderungen bleiben nach einem Neustart der App erhalten. Die Datenträgerlatenz des Inhaltsvolumes ist jedoch größer und variabler als die Latenz des lokalen Containerdateisystems, und der Zugriff kann durch Plattformupgrades, ungeplante Ausfallzeiten und Probleme mit der Netzwerkkonnektivität beeinträchtigt werden. Für Apps, die umfassenden Lesezugriff auf Inhaltsdateien erfordern, ist eine Bereitstellung über ein benutzerdefiniertes Images möglicherweise von Vorteil, da hierbei die Dateien im Imagedateisystem platziert werden und nicht auf dem Inhaltsvolume.
- **Nutzung von Buildressourcen:** Wenn eine App aus dem Quellcode bereitgestellt wird, verwenden die von Kudu ausgeführten Bereitstellungsskripts dieselben Compute- und Speicherressourcen des App Service-Plans wie die laufende App. Große App-Bereitstellungen verbrauchen möglicherweise mehr Ressourcen oder Zeit als gewünscht. Insbesondere erzeugen viele Bereitstellungsworkflows starke Datenträgeraktivität auf dem App-Inhaltsvolume, das für Aktivitäten dieser Art nicht optimiert ist. Ein benutzerdefiniertes Image stellt alle Dateien und Abhängigkeiten Ihrer App in einem einzelnen Paket für Azure bereit, ohne dass weitere Dateiübertragungen oder Bereitstellungsaktionen erforderlich sind.
- **Anforderung für schnelle Iteration:** Das Umwandeln einer App in ein Docker-Image erfordert zusätzliche Schritte. Damit die Änderungen wirksam werden, müssen Sie das neue Image bei jedem Update mithilfe von Push in ein Repository übertragen. Diese Updates werden dann per Pull in die Azure-Umgebung übernommen. Wenn einer der integrierten Container die Anforderungen Ihrer App erfüllt, kann die Bereitstellung aus dem Quellcode einen schnelleren Entwicklungsworkflow bieten.

## <a name="multi-container-apps-supportability"></a>Unterstützungsfähigkeit für Apps mit mehreren Containern

### <a name="supported-docker-compose-configuration-options"></a>Unterstützte Docker Compose-Konfigurationsoptionen
- command
- entrypoint
- Environment
- image
- ports
- restart
- services
- volumes

### <a name="unsupported-docker-compose-configuration-options"></a>Nicht unterstützte Docker Compose-Konfigurationsoptionen
- build (unzulässig)
- depends_on (ignoriert)
- networks (ignoriert)
- secrets (ignoriert)
- andere Ports als 80 und 8080 (ignoriert)

> [!NOTE]
> Alle weiteren Optionen, die nicht ausdrücklich aufgeführt sind, werden in der Public Preview ebenfalls ignoriert.

### <a name="supported-kubernetes-configuration-options"></a>Unterstützte Kubernetes-Konfigurationsoptionen
- args
- command
- containers
- image
- name
- ports
- spec

> [!NOTE]
>Alle weiteren Optionen, die nicht ausdrücklich aufgeführt sind, werden in der Public Preview nicht unterstützt.
>
