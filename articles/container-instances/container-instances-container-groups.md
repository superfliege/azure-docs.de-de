---
title: Containergruppen von Azure Container Instances
description: Funktionsweise von Containergruppen in Azure Container Instances
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 03/19/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 6f7f0d9aea86594140c302e6d12e6528e802b9e7
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Containergruppen in Azure Container Instances

Die oberste Ressource in Azure Container Instances ist die *Containergruppe*. In diesem Artikel wird beschrieben, was Containergruppen sind und welche Arten von Szenarien damit möglich sind.

## <a name="how-a-container-group-works"></a>Funktionsweise einer Containergruppe

Eine Containergruppe ist eine Sammlung mit Containern, die auf demselben Hostcomputer geplant werden. Für die Container einer Containergruppe werden der Lebenszyklus, das lokale Netzwerk und die Speichervolumes gemeinsam genutzt. Dies ähnelt dem *Pod*-Konzept in [Kubernetes][kubernetes-pod] und [DC/OS][dcos-pod].

Das folgende Diagramm zeigt ein Beispiel für eine Containergruppe, die mehrere Container enthält:

![Diagramm zu Containergruppen][container-groups-example]

Für diese Beispielcontainergruppe gilt Folgendes:

* Sie wird auf einem einzelnen Hostcomputer geplant.
* Ihr ist eine DNS-Namensbezeichnung zugewiesen.
* Sie macht eine einzelne öffentliche IP-Adresse mit einem Port verfügbar.
* Sie besteht aus zwei Containern. Ein Container lauscht über Port 80 und der andere über Port 5000.
* Sie enthält zwei Azure-Dateifreigaben als Volumebereitstellungen, und jeder Container stellt eine der Freigaben lokal bereit.

> [!NOTE]
> Gruppen mit mehreren Containern sind aktuell auf Linux-Container beschränkt. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

### <a name="deployment"></a>Bereitstellung

**Containergruppen** verfügen über eine minimale Ressourcenzuordnung von 1 vCPU und 1 GB Arbeitsspeicher. Einzelne **Container** können mit weniger als 1 vCPU und 1 GB Arbeitsspeicher bereitgestellt werden. Innerhalb einer Containergruppe kann die Verteilung von Ressourcen an mehrere Container angepasst werden, solange diese innerhalb der Grenzwerte liegen, die auf Containergruppenebene festgelegt wurden. Beispielsweise sind zwei Container mit je einer halben vCPU innerhalb einer Containergruppe 1 vCPU zugeordnet.

### <a name="networking"></a>Netzwerk

Containergruppen nutzen eine IP-Adresse und einen Portnamespace dieser IP-Adresse gemeinsam. Sie müssen den Port unter der IP-Adresse und für den Container verfügbar machen, um es externen Clients zu ermöglichen, einen Container in der Gruppe zu erreichen. Da Container in der Gruppe einen Portnamespace gemeinsam nutzen, wird die Portzuordnung nicht unterstützt. Container in einer Gruppe können einander per localhost auf den Ports erreichen, die sie verfügbar gemacht haben. Dies gilt auch, wenn diese Port für die IP-Adresse der Gruppe nicht extern verfügbar gemacht werden.

### <a name="storage"></a>Speicher

Sie können externe Volumes für die Bereitstellung in einer Containergruppe angeben. Sie können diese Volumes bestimmten Pfaden in den einzelnen Containern einer Gruppe zuordnen.

## <a name="common-scenarios"></a>Häufige Szenarios

Gruppen mit mehreren Containern sind nützlich, wenn Sie eine einzelne funktionale Aufgabe auf eine kleine Anzahl von Containerimages aufteilen wollen. Diese Images können dann von verschiedenen Teams bereitgestellt werden und separate Ressourcenanforderungen aufweisen.

Beispiel für Verwendung:

* Ein Anwendungscontainer und ein Protokollierungscontainer. Mit dem Protokollierungscontainer werden die Protokolle und Metriken gesammelt, die von der Hauptanwendung ausgegeben werden, und in den Langzeitspeicher geschrieben.
* Eine Anwendung und ein Überwachungscontainer. Der Überwachungscontainer sendet regelmäßig eine Anforderung an die Anwendung, um sicherzustellen, dass sie ausgeführt wird und richtig reagiert. Andernfalls wird eine Warnung ausgelöst.
* Ein Container, über den eine Webanwendung bereitgestellt wird, und ein Container, der den aktuellen Inhalt aus der Quellcodeverwaltung abruft.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Bereitstellen einer Gruppe mit mehreren Containern](container-instances-multi-container-group.md) per Azure Resource Manager-Vorlage.

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
