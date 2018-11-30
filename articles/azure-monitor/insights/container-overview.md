---
title: Überwachung von Azure-Containern – Übersicht | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die verschiedenen verfügbaren Methoden zum Überwachen von Containern in Azure, um sich schnell einen Eindruck von der Clusterintegrität und -verfügbarkeit zu verschaffen.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: ed5e3bdc1025b4b827d3b895bce95f6949c46e83
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51714313"
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Übersicht über die Überwachung von Containern in Azure
Mit Azure können Sie effizient Workloads überwachen und verwalten, die in Azure-Containern mit Kubernetes oder Docker bereitgestellt werden. Es ist wichtig zu verstehen, wie Container mit mehreren Microserviceanwendungen funktionieren, um einen zuverlässigen Dienst im großen Maßstab bereitstellen und Ihren Überwachungsplan unterstützen zu können. Dieser Artikel enthält eine kurze Übersicht über die Verwaltungs- und Überwachungsfunktionen in Azure, in denen ihre Eignung gemäß Ihren Anforderungen beschrieben werden.

Mit [Azure Monitor für Container](container-insights-overview.md) können Sie auf einen Blick die Leistung und Integrität Ihrer Linux-Containerinfrastruktur einsehen und rasch Problemen auf den Grund gehen. Die Telemetriedaten werden in einem Log Analytics-Arbeitsbereich gespeichert und sind im Azure-Portal integriert, in dem Sie aggregierte Daten über Dashboards untersuchen , filtern und segmentieren können. So behalten Sie die Last, Leistung und Integrität stets im Auge.  

Bei Containern, die außerhalb des gehosteten Azure Kubernetes-Diensts ausgeführt werden, können Sie mit der [Log Analytics-Lösung für Windows und Docker-Container](../../log-analytics/log-analytics-containers.md) Windows- und Docker-Containerhosts anzeigen und verwalten. In Ihrem Log Analytics-Arbeitsbereich können Sie die Bestandsdetails, Leistung und Ereignisse von Knoten und Containern in der Umgebung einsehen. Zudem können Sie ausführliche Überwachungsinformationen einsehen, in denen Befehle mit Containern verwendet werden, und Probleme durch Anzeigen und Durchsuchen zentralisierter Protokolle behandeln, ohne remote auf Docker- oder Windows-Hosts zugreifen zu müssen.

Um eine ganzheitliche oder End-to-End-Überwachung der Anwendung zu erzielen, sollten Abhängigkeiten – ganz gleich, ob es sich um eine Azure- oder lokale Ressource handelt – mit Azure Monitor oder Log Analytics überwacht werden.  Um den Sensibilisierungsgrad für Integrität zu erhöhen, sollte die Anwendungsschicht mit Application Insights sowohl auf Plattform- als auch auf Anwendungsebene eingeschlossen werden. Auf der Plattformebene sind Application Insights-SDKs für [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/) und [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) verfügbar. Für Microserviceanwendungen besteht Unterstützung für [Java](../../application-insights/app-insights-java-get-started.md), [Node.js](../../application-insights/app-insights-nodejs-quick-start.md), [.NET](../../application-insights/app-insights-asp-net.md), [.NET Core](../../application-insights/app-insights-asp-net-core.md) sowie eine Reihe von anderen [Sprachen bzw. Frameworks](../../application-insights/app-insights-platforms.md). 

Ohne diese Überwachungsmaßnahmen würden Probleme, die sich auf die Verfügbarkeit der Anwendung auswirken könnten, nicht ausfindig gemacht und keine SLA-Ziele (Service Level Agreement) erfüllt werden.  
