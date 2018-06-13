---
title: Übersicht über Azure Container Instances
description: Grundlegende Informationen zu Azure Container Instances
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: overview
ms.date: 03/29/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2da61d52e43857237f6e231c03ea62ba5eefdadd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33767970"
---
# <a name="azure-container-instances"></a>Azure Container Instances

Container entwickeln sich mehr und mehr zum bevorzugten Instrument für das Packen, Bereitstellen und Verwalten von Cloudanwendungen. Mit Azure Container Instances lassen sich Container in Azure besonders schnell und einfach ausführen, ohne dass Sie dazu virtuelle Computer verwalten oder einen übergeordneten Dienst einführen müssen.

Azure Container Instances ist eine großartige Lösung für jedes Szenario, das für die Verwendung isolierter Container geeignet ist. Hierzu zählen unter anderem einfache Anwendungen, Aufgabenautomatisierung und Erstellungsaufträge. Für Szenarien, die eine umfassende Containerorchestrierung erfordern (etwa für die containerübergreifende Dienstermittlung, automatische Skalierung und koordinierte Anwendungsupgrades), empfehlen wir [Azure Kubernetes Service](../aks/index.yml) (AKS).

## <a name="fast-startup-times"></a>Schneller Start

Container bieten im Vergleich zu virtuellen Computern bedeutende Vorteile beim Start. Azure Container Instances kann in Sekundenschnelle Container in Azure starten, ohne virtuelle Computer bereitstellen und verwalten zu müssen.

## <a name="public-ip-connectivity-and-dns-name"></a>Öffentliche IP-Konnektivität und DNS-Name

Mit Azure Container Instances können Sie Ihre Container direkt über eine öffentliche IP-Adresse und einen vollqualifizierten Domainnamen (FQDN) im Internet verfügbar machen. Beim Erstellen einer Containerinstanz können Sie eine benutzerdefinierte DNS-Namensbezeichnung angeben, sodass Ihre Anwendung unter „*customlabel*.*azureregion*.azurecontainer.io“ erreichbar ist.

## <a name="hypervisor-level-security"></a>Sicherheit auf Hypervisor-Ebene

In der Vergangenheit verfügten Container zwar über Anwendungsabhängigkeitsisolierung und Ressourcenkontrolle, galten aber nicht als ausreichend gehärtet, um in einer gefährlichen Umgebung mit mehreren Mandanten verwendet werden zu können. Azure Container Instances gewährleistet, dass Ihre Anwendung in einem Container isoliert ist – genau wie bei einem virtuellen Computer.

## <a name="custom-sizes"></a>Benutzerdefinierte Größen

Container sind in der Regel für die Ausführung einer einzelnen Anwendung optimiert. Die genauen Anforderungen dieser Anwendungen können sich jedoch erheblich voneinander unterscheiden. Azure Container Instances ermöglicht exakte Angaben für CPU-Kerne und Arbeitsspeicher und bietet dadurch eine optimale Auslastung. Dank sekundengenauer Abrechnung können Sie Ihre Ausgaben auf der Grundlage Ihres tatsächlichen Bedarfs präzise optimieren.

## <a name="persistent-storage"></a>Permanenter Speicher

Dank der Möglichkeit zur direkten [Einbindung von Azure Files-Freigaben](container-instances-mounting-azure-files-volume.md) können Sie mit Azure Container Instances den Zustand abrufen und speichern.

## <a name="linux-and-windows-containers"></a>Linux- und Windows-Container

Azure Container Instances kann sowohl Windows- als auch Linux-Container mit der gleichen API planen. Geben Sie einfach den BS-Typ an, wenn Sie Ihre [Containergruppen](container-instances-container-groups.md) erstellen.

Einige Features sind momentan auf Linux-Container beschränkt. Bis zur Erreichung der Featureparität für Windows-Container finden Sie die aktuellen Plattformunterschiede unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="co-scheduled-groups"></a>Gemeinsam geplante Gruppen

Azure Container Instances unterstützt die Planung von [Gruppen mit mehreren Containern](container-instances-container-groups.md) mit gemeinsamem Hostcomputer, lokalem Netzwerk, Speicher und Lebenszyklus. Dadurch können Sie Ihren Hauptanwendungscontainer mit anderen unterstützenden Rollencontainern (beispielsweise Protokollierungs-Sidecars) kombinieren.

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie mit einem einzelnen Befehl einen Container in Azure bereit. Eine entsprechende Anleitung finden Sie in unserem Schnellstarthandbuch:

> [!div class="nextstepaction"]
> [Schnellstartanleitung: Erstellen Ihres ersten Containers in Azure Container Instances](container-instances-quickstart.md)
