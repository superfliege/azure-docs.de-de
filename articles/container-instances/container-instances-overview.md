---
title: "Übersicht über Azure Container Instances"
description: Grundlegende Informationen zu Azure Container Instances
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 01e539856adbdcf02dc4e49087a3ab71b328db5a
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2018
---
# <a name="azure-container-instances"></a>Azure Container Instances

Container entwickeln sich mehr und mehr zum bevorzugten Instrument für das Packen, Bereitstellen und Verwalten von Cloudanwendungen. Mit Azure Container Instances lassen sich Container in Azure besonders schnell und einfach ausführen, ohne dass Sie dazu virtuelle Computer bereitstellen oder einen übergeordneten Dienst einführen müssen.

Azure Container Instances ist eine großartige Lösung für jedes Szenario, das für die Verwendung isolierter Container geeignet ist. Hierzu zählen unter anderem einfache Anwendungen, Aufgabenautomatisierung und Erstellungsaufträge. Für Szenarien, die eine umfassende Containerorchestrierung erfordern (etwa für die containerübergreifende Dienstermittlung, automatische Skalierung und koordinierte Anwendungsupgrades), empfehlen wir [Azure Container Service](../aks/index.yml) (AKS).

## <a name="fast-startup-times"></a>Schneller Start

Container bieten im Vergleich zu virtuellen Computern bedeutende Vorteile beim Start. Mit Azure Container Instances können Sie in Azure in Sekundenschnelle einen Container starten, ohne virtuelle Computer bereitstellen und verwalten zu müssen.

## <a name="hypervisor-level-security"></a>Sicherheit auf Hypervisor-Ebene

In der Vergangenheit verfügten Container zwar über Anwendungsabhängigkeitsisolierung und Ressourcenkontrolle, galten aber nicht als ausreichend gehärtet, um in einer gefährlichen Umgebung mit mehreren Mandanten verwendet werden zu können. Mit Azure Container Instances ist Ihre Anwendung in einem Container isoliert – genau wie bei einem virtuellen Computer.

## <a name="custom-sizes"></a>Benutzerdefinierte Größen

Container sind in der Regel für die Ausführung einer einzelnen Anwendung optimiert. Die genauen Anforderungen dieser Anwendungen können sich jedoch erheblich voneinander unterscheiden. Mit Azure Container Instances können Sie exakt die Menge an CPU-Kernen und Arbeitsspeicher anfordern, die Sie benötigen. Die sekundengenaue Abrechnung erfolgt auf der Grundlage Ihrer Anforderung, sodass Sie Ihre Ausgaben sehr genau auf der Grundlage Ihres Bedarfs optimieren können.

## <a name="public-ip-connectivity"></a>Öffentliche IP-Konnektivität

Mit Azure Container Instances können Sie Ihre Container über eine öffentliche IP-Adresse direkt im Internet verfügbar machen. Die Netzwerkfunktionen werden in Zukunft noch erweitert, um die Integration in virtuelle Netzwerke, Load Balancer und andere zentrale Komponenten der Azure-Netzwerkinfrastruktur zu ermöglichen.

## <a name="persistent-storage"></a>Permanenter Speicher

Dank der Möglichkeit zur direkten [Einbindung von Azure Files-Freigaben](container-instances-mounting-azure-files-volume.md) können Sie mit Azure Container Instances den Zustand abrufen und speichern.

## <a name="linux-and-windows-containers"></a>Linux- und Windows-Container

Mit Azure Container Instances können Sie sowohl Windows- als auch Linux-Container mit der gleichen API planen. Geben Sie einfach den BS-Typ an, wenn Sie Ihre [Containergruppen](container-instances-container-groups.md) erstellen.

Einige Features sind momentan auf Linux-Container beschränkt. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="co-scheduled-groups"></a>Gemeinsam geplante Gruppen

Azure Container Instances unterstützt die Planung von [Gruppen mit mehreren Containern](container-instances-container-groups.md) mit gemeinsamem Hostcomputer, lokalem Netzwerk, Speicher und Lebenszyklus. Dadurch können Sie Ihre Hauptanwendung mit anderen (unterstützenden) Anwendungen kombinieren (etwa mit der Protokollierung).

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie mit einem einzelnen Befehl einen Container in Azure bereit. Eine entsprechende Anleitung finden Sie in unserem [Schnellstarthandbuch](container-instances-quickstart.md).