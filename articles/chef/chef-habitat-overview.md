---
title: Verwenden von Habitat zum Bereitstellen Ihrer Anwendung in Azure
description: Erfahren Sie, wie Sie Ihre Anwendung auf virtuellen Azure-Computern und in Containern einheitlich bereitstellen.
keywords: Azure, Chef, Devops, virtuelle Computer, Übersicht, automatisieren, Habitat
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 4847d9ce551c9acf1e4fb6325c770187b2cfd89f
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052290"
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Verwenden von Habitat zum Bereitstellen Ihrer Anwendung in Azure
[Habitat](https://www.habitat.sh/) ist ein erstes Open-Source-Projekt seiner Art, das einen völlig neuen Ansatz für die Anwendungsverwaltung bietet. Habitat macht die Anwendung und ihre Automatisierung zur Entwicklungseinheit. Wenn Anwendungen in ein einfaches „Habitat“ eingeschlossen werden, steht die Laufzeitumgebung, ganz gleich ob Container, Bare-Metal oder PaaS, nicht mehr im Mittelpunkt und schränkt die Anwendung nicht ein. 

Dieser Artikel beschreibt die Vorteile der Verwendung von Habitat.

## <a name="support-for-the-modern-application"></a>Unterstützung für die moderne Anwendung
Habitat-Pakete enthalten alles, was eine Anwendung zur Ausführung während ihres gesamten Lebenszyklus benötigt. Kernkomponenten von Habitat sind:
- Verpackungsformat: Anwendungen in einem Habitat-Paket sind atomar, unveränderlich und prüfbar.
- Der Habitat-Supervisor führt Anwendungspakete unter Berücksichtigung der Peer-Beziehungen, Aktualisierungsstrategien und Sicherheitsrichtlinien der Pakete aus. Der Habitat-Supervisor konfiguriert und verwaltet die Anwendung für alle vorhandenen Umgebungen.
- Das Habitat-Ökosystem bietet außerdem einen Builddienst, der den Habitat-Buildplan übernimmt, das Habitat-Paket erstellt und in einem Depot veröffentlicht.

## <a name="run-any-application-anywhere"></a>Ausführen einer beliebigen Anwendung – und zwar überall
Mit Habitat können Anwendungen unverändert in allen Laufzeitumgebungen ausgeführt werden. Es kann sich um eine beliebige Anwendung handeln: von Bare-Metal und virtuellen Computern bis hin zu Containern (z.B. Docker), Clusterverwaltungssystemen (z.B. Mesosphere oder Kubernetes) und PaaS-Systemen (z.B. Pivot Cloud Foundry).

## <a name="easily-port-legacy-applications"></a>Problemlose Portierung von Legacyanwendungen
Wenn Legacyanwendungen in einem Habitat-Paket eingeschlossen werden, sind die Anwendungen unabhängig von der Umgebung, für die sie ursprünglich entworfen wurden. Die Pakete können schnell auf modernere Umgebungen verschoben werden, z. B. in die Cloud oder in Container. Da Habitat-Pakete über eine standardisierte, nach außen gerichtete Schnittstelle verfügen, lassen sich Legacyanwendungen zudem viel einfacher verwalten.

## <a name="improve-the-container-experience"></a>Verbessern der Containerumgebung
Habitat verringert die Komplexität der Verwaltung von Containern in Produktionsumgebungen. Durch die Automatisierung der Anwendungskonfiguration innerhalb eines Containers können Entwickler mit Habitat die Herausforderungen meistern, vor denen sie beim Verschieben von containerbasierten Anwendungen aus der Entwicklungsumgebung in Produktionsumgebungen stehen.

## <a name="integrate-into-the-chef-devops-workflow"></a>Integration in den Chef DevOps-Workflow
Das Habitat-Projekt wird von Chef unterstützt. Habitat nutzt die weitreichenden Erfahrungen von Chef mit Infrastrukturautomatisierung, um beispiellose Automatisierungsmöglichkeiten für Anwendungen zu schaffen. Chef bietet kommerzielle Unterstützung für Habitat und stellt die nahtlose Integration zwischen Habitat und Chef Delivery sicher, um den Anwendungsfreigabezyklus von der Entwicklung bis zur Bereitstellung zu automatisieren.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines virtuellen Windows-Computers in Azure mithilfe von Chef](/azure/virtual-machines/windows/chef-automation)