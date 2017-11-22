---
title: "Übersicht zu OpenShift in Azure | Microsoft-Dokumentation"
description: "Eine Übersicht zu OpenShift in Azure."
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
ms.openlocfilehash: 5d154a1c860a5ebd7af2efd55b470bb14efe6c67
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="openshift-in-azure"></a>OpenShift in Azure

OpenShift ist eine offene und erweiterbare Containeranwendungsplattform, die Docker und Kubernetes in das Unternehmen einführt.  

OpenShift enthält Kubernetes für die Containerorchestrierung und -verwaltung. Es fügt Entwicklertools und vorgangsorientierte Tools hinzu, mit denen Folgendes ermöglicht wird:

- Schnelle Anwendungsentwicklung
- Einfache Bereitstellung und Skalierung
- Langfristige Lebenszykluswartung für Teams und Anwendungen

Es gibt mehrere Versionen von OpenShift, von denen zwei in Azure ausgeführt werden können:

- OpenShift Origin
- OpenShift Container Platform
- OpenShift Online
- OpenShift Dedicated

Von den vier in diesem Artikel behandelten Versionen sind zwei für Kunden für die eigene Bereitstellung in Azure verfügbar: OpenShift Origin und OpenShift Container Platform.

## <a name="openshift-origin"></a>OpenShift Origin

Origin ist ein [Open Source](https://www.openshift.org/) Upstream-Projekt von OpenShift, das von der Community unterstützt wird. Origin kann auf CentOS oder Red Hat Enterprise Linux (RHEL) installiert werden.

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Container Platform ist eine [kommerzielle Version](https://www.openshift.com) für unternehmen von und unterstützt von Red Hat. Mit dieser Version erwirbt der Kunde die erforderlichen Berechtigungen für OpenShift Container Platform und ist für die Installation und Verwaltung der gesamten Infrastruktur zuständig.

Da der Kunde der Besitzer der gesamten Plattform ist, kann die Installation in seinem lokalen Rechenzentrum oder in einer öffentlichen Cloud (z. B. Azure, AWS oder Google) erfolgen.

## <a name="openshift-online"></a>OpenShift Online

Online ist ein von Red Hat verwaltetes OpenShift für *mehrere Mandanten*, das Container Platform verwendet. Die gesamte zugrunde liegende Infrastruktur (virtuelle Computer, OpenShift-Cluster, Netzwerke und Speicher) wird von Red Hat verwaltet. 

Mit dieser Version stellt der Kunde Container bereit, hat jedoch keinen Einfluss darauf, auf welchen Hosts die Container ausgeführt werden. Da Online für mehrere Mandanten vorgesehen ist, können sich Container auf den gleichen VM-Hosts wie Container von anderen Kunden befinden. Die Kosten werden pro Container berechnet.

## <a name="openshift-dedicated"></a>OpenShift Dedicated

Dedicated ist ein von Red Hat verwaltetes OpenShift für einen *einzelnen Mandanten*, das Container Platform verwendet. Die gesamte zugrunde liegende Infrastruktur (virtuelle Computer, OpenShift-Cluster, Netzwerk, Speicher usw.) wird von Red Hat verwaltet. Der Cluster ist für einen einzelnen Kunden bestimmt und wird in einer öffentlichen Cloud (z. B. AWS oder Google) ausgeführt (Azure wird Anfang 2018 verfügbar sein). Ein erster Cluster enthält vier Anwendungsknoten für 48.000 $ pro Jahr (im voraus bezahlt).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren allgemeiner Voraussetzungen für OpenShift in Azure](./openshift-prerequisites.md)
- [Bereitstellen von OpenShift Origin in Azure](./openshift-origin.md)
- [Bereitstellen von OpenShift Container Platform in Azure](./openshift-container-platform.md)
- [Aufgaben nach der Bereitstellung](./openshift-post-deployment.md)
- [Beheben von Problemen bei der Bereitstellung von OpenShift](./openshift-troubleshooting.md)
