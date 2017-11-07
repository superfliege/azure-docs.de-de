---
title: "Übersicht zu OpenShift in Azure | Microsoft-Dokumentation"
description: "Übersicht zu OpenShift in Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: f9641b52db91a4356f6d5789a8cd78a6bb3da02b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="openshift-overview"></a>OpenShift – Übersicht

OpenShift ist eine offene und erweiterbare Containeranwendungsplattform, die Docker und Kubernetes in das Unternehmen einführt.  

OpenShift enthält Kubernetes für die Containerorchestrierung und -verwaltung. Es fügt Entwicklertools und vorgangssorientierte Tools hinzu, mit denen Folgendes ermöglicht wird:

- Schnelle Anwendungsentwicklung
- Einfache Bereitstellung und Skalierung
- Langfristige Lebenszykluswartung für Teams und Anwendungen

Es gibt mehrere Angebote von OpenShift, von denen zwei in Azure ausgeführt werden können.

- OpenShift Origin
- OpenShift Container Platform
- OpenShift Online
- OpenShift Dedicated

Von den vier behandelten Angeboten sind zwei für Kunden für die eigene Bereitstellung in Azure verfügbar – OpenShift Origin und OpenShift Container Platform.

## <a name="openshift-origin"></a>OpenShift Origin

[Open Source](https://www.openshift.org/) Upstream-Projekt von OpenShift, das von der Community unterstützt wird. Origin kann unter CentOS oder RHEL installiert werden.

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Unternehmensgerechte ([kommerzielles Angebot](https://www.openshift.com)) Version von Red Hat, die von Red Hat unterstützt wird. Der Kunde erwirbt die erforderlichen Berechtigungen für OpenShift Container Platform und ist für die Installation und Verwaltung der gesamten Infrastruktur zuständig.

Da der Kunde der Besitzer der gesamten Plattform ist, kann die Installation in seinem lokalen Rechenzentrum, in der öffentlichen Cloud (Azure, AWS, Google und weitere) usw. erfolgen.

## <a name="openshift-online"></a>OpenShift Online

Von Red Hat verwaltetes OpenShift für **mehrere Mandanten** (mithilfe von Container Platform). Die gesamte zugrunde liegende Infrastruktur (virtuelle Computer, OpenShift-Cluster, Netzwerk, Speicher usw.) wird von Red Hat verwaltet. 

Der Kunde stellt Container bereit, hat jedoch keinen Einfluss darauf, auf welchen Hosts die Container ausgeführt werden. Da es für mehrere Mandanten vorgesehen ist, können sich Container auf den gleichen VM-Hosts wie Container von anderen Kunden befinden. Die Kosten werden pro Container berechnet.

## <a name="openshift-dedicated"></a>OpenShift Dedicated

Von Red Hat verwaltetes OpenShift für einen **einzelnen Mandanten** (mithilfe von Container Platform). Die gesamte zugrunde liegende Infrastruktur (virtuelle Computer, OpenShift-Cluster, Netzwerk, Speicher usw.) wird von Red Hat verwaltet. Der Cluster ist für einen einzelnen Kunden bestimmt und wird in einer öffentlichen Cloud (AWS, Google, Azure – Anfang 2018 verfügbar) ausgeführt. Der Startcluster umfasst vier Anwendungsknoten für 48T USD/Jahr (Vorauszahlung für ein ganzes Jahr).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren allgemeiner Voraussetzungen für OpenShift in Azure](./openshift-prerequisites.md)
- [Bereitstellen von OpenShift Origin](./openshift-origin.md)
- [Bereitstellen von OpenShift Container Platform](./openshift-container-platform.md)
- [Aufgaben nach der Bereitstellung](./openshift-post-deployment.md)
- [Beheben von Problemen bei der Bereitstellung von OpenShift](./openshift-troubleshooting.md)
