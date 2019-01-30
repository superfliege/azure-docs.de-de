---
title: Übersicht zu OpenShift in Azure | Microsoft-Dokumentation
description: Eine Übersicht zu OpenShift in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: bf34d7e85632999056388eb74816140ce667ce10
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382360"
---
# <a name="openshift-in-azure"></a>OpenShift in Azure

OpenShift ist eine offene und erweiterbare Containeranwendungsplattform, die Docker und Kubernetes in das Unternehmen einführt.  

OpenShift enthält Kubernetes für die Containerorchestrierung und -verwaltung. Es fügt entwickler- und vorgangsorientierte Tools hinzu, die Folgendes ermöglichen:

- Schnelle Anwendungsentwicklung
- Einfache Bereitstellung und Skalierung
- Langfristige Lebenszykluswartung für Teams und Anwendungen

Von OpenShift sind mehrere Versionen verfügbar:

- OpenShift Container Platform
- OpenShift in Azure (vollständig verwaltete OpenShift-Lösung voraussichtlich gegen Ende des ersten Quartals 2019)
- OKD (vormals OpenShift Origin)
- OpenShift Dedicated
- OpenShift Online

Von den fünf in diesem Artikel behandelten Versionen können derzeit nur zwei von Kunden in Azure bereitgestellt werden: OpenShift Container Platform und OKD.

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Container Platform ist eine [kommerzielle Version](https://www.openshift.com) für unternehmen von und unterstützt von Red Hat. Mit dieser Version erwirbt der Kunde die erforderlichen Berechtigungen für OpenShift Container Platform und ist für die Installation und Verwaltung der gesamten Infrastruktur zuständig.

Da der Kunde der Besitzer der gesamten Plattform ist, kann die Installation in seinem lokalen Rechenzentrum oder in einer öffentlichen Cloud (z. B. Azure, AWS oder Google) erfolgen.

## <a name="openshift-on-azure"></a>OpenShift in Azure

OpenShift in Azure ist ein vollständig verwaltetes Angebot von OpenShift, das in Azure ausgeführt wird. Dieser Dienst wird gemeinsam von Microsoft und Red Hat verwaltet und unterstützt. Der Cluster wird unter dem Azure-Abonnement des Kunden bereitgestellt. Der Dienst befindet sich derzeit in der privaten Vorschau und wird voraussichtlich gegen Ende des ersten Quartals 2019 allgemein verfügbar. Kunden, die an der privaten Vorschau teilnehmen möchten, füllen bitte das [Antragsformular](http://aka.ms/openshiftazureinterest) aus.  Weitere Informationen werden bereitgestellt, wenn der Termin für die öffentliche Verfügbarkeit näher rückt.

## <a name="okd-formerly-openshift-origin"></a>OKD (vormals OpenShift Origin)

OKD ist ein [Open Source](https://www.okd.io/) Upstream-Projekt von OpenShift, das von der Community unterstützt wird. OKD kann auf CentOS oder Red Hat Enterprise Linux (RHEL) installiert werden.

## <a name="openshift-dedicated"></a>OpenShift Dedicated

Dedicated ist eine von Red Hat verwaltete OpenShift-Lösung für einen *einzelnen Mandanten*, die OpenShift Container Platform verwendet. Die gesamte zugrunde liegende Infrastruktur (virtuelle Computer, OpenShift-Cluster, Netzwerk, Speicher usw.) wird von Red Hat verwaltet. Der Cluster ist für einen einzelnen Kunden bestimmt und wird in einer öffentlichen Cloud (beispielsweise AWS oder Google) ausgeführt. Ein Einstiegscluster umfasst vier Anwendungsknoten, und alle Kosten werden jährlich im Voraus bezahlt.

## <a name="openshift-online"></a>OpenShift Online

Online ist ein von Red Hat verwaltetes OpenShift für *mehrere Mandanten*, das Container Platform verwendet. Die gesamte zugrunde liegende Infrastruktur (virtuelle Computer, OpenShift-Cluster, Netzwerke und Speicher) wird von Red Hat verwaltet. 

Mit dieser Version stellt der Kunde Container bereit, hat jedoch keinen Einfluss darauf, auf welchen Hosts die Container ausgeführt werden. Da Online für mehrere Mandanten vorgesehen ist, können sich Container auf den gleichen VM-Hosts wie Container von anderen Kunden befinden. Die Kosten werden pro Container berechnet.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren allgemeiner Voraussetzungen für OpenShift in Azure](./openshift-prerequisites.md)
- [Bereitstellen von OpenShift Container Platform in Azure](./openshift-container-platform.md)
- [Bereitstellen von OKD in Azure](./openshift-okd.md)
- [Bereitstellen von OpenShift in Azure Stack](./openshift-azure-stack.md)
- [Aufgaben nach der Bereitstellung](./openshift-post-deployment.md)
- [Beheben von Problemen bei der Bereitstellung von OpenShift](./openshift-troubleshooting.md)
