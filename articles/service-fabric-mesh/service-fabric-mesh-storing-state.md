---
title: Optionen für die Zustandsspeicherung in Azure Service Fabric Mesh | Microsoft-Dokumentation
description: Weitere Informationen zur zuverlässigen Zustandsspeicherung in Service Fabric Mesh-Anwendungen, die in Azure Service Fabric Mesh ausgeführt werden
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 6aa268cf56bfb8be9c27a9e0d9e5c9f4464b0c9d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39075509"
---
# <a name="state-management-with-service-fabric"></a>Zustandsverwaltung mit Service Fabric
Service Fabric unterstützt viele verschiedene Optionen für die Zustandsspeicherung. Eine grundlegende Übersicht über die Zustandsverwaltungsmuster und Service Fabric finden Sie unter [Service Fabric-Konzepte: Zustand](/azure/service-fabric/service-fabric-concepts-state). Alle diese Konzepte gelten unabhängig davon, ob Ihre Dienste innerhalb oder außerhalb von Service Fabric Mesh ausgeführt werden. 

## <a name="state-storage-options-in-azure-service-fabric-mesh"></a>Optionen für die Zustandsspeicherung in Azure Service Fabric Mesh
Mit Service Fabric Mesh können Sie ganz einfach eine neue Anwendung bereitstellen und mit einem bestehenden Datenspeicher verbinden, der in Azure gehostet wird. Neben der Verwendung einer beliebigen Remotedatenbank gibt es weitere Möglichkeiten, Daten zu speichern, je nachdem, ob der Dienst lokal oder remote gespeichert werden soll. 

* Lokal gespeicherte replizierte Daten
  * Zuverlässige Sammlungen (nicht in der Vorschauversion verfügbar)
    * Eine Bibliothek, die Datenstrukturen wie Warteschlangen und Schlüssel-Wert-Paare zur Verwendung im Dienst bereitstellt
    * Die einfachste und schnellste Möglichkeit, mit Daten zu interagieren und gleichzeitig ein einfaches Partitionsrouting in Kombination mit intelligentem Routing in Service Fabric Mesh zu ermöglichen
  * Service Fabric-Volumetreiber (nicht in der Vorschauversion verfügbar)
    * Ein Docker-Volumetreiber, der ein lokales Volume in einem Container bereitstellt
    * Bietet Ihnen die ultimative Flexibilität bei der lokalen Datenspeicherung über eine beliebige API, die Dateispeicherung unterstützt

* Remotespeicher
  * Azure Files-Volumetreiber
    * Ein Docker-Volumetreiber, der eine Azure Files-Freigabe in einen Container einbindet
    * Bietet Ihnen eine weniger leistungsstarke, dafür aber kostengünstigere, komplett flexible und zuverlässige Datenoption über jede beliebige API, die Dateispeicherung unterstützt
    * [Anleitung: Bereitstellen einer App mit Azure Files-Volumes](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)
    
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Anwendungsmodell finden Sie unter [Service Fabric-Ressourcen](service-fabric-mesh-service-fabric-resources.md).
