---
title: Überwachung und Diagnose von Service Fabric Mesh-Anwendungen in Azure | Microsoft-Dokumentation
description: Weitere Informationen zur Überwachung und Diagnose von Service Fabric Mesh-Anwendungen in Azure
services: service-fabric-mesh
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2018
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 9f857c23b5500bc7790a0ff7fcf81eaec51f37c9
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358305"
---
# <a name="monitoring-and-diagnostics"></a>Überwachung und Diagnose
Azure Service Fabric Mesh ist ein vollständig verwalteter Dienst, der es Entwicklern ermöglicht, Microservicesanwendungen zu implementieren, ohne virtuelle Computer, Speicher oder Netzwerke verwalten zu müssen. Die Überwachung und Diagnose für Service Fabric Mesh besteht aus drei Hauptkategorien von Diagnosedaten:

- Anwendungsprotokolle: Diese sind definiert als die Protokolle aus Ihren Containeranwendungen, basierend darauf, wie Sie Ihre Anwendung instrumentiert haben (z. B. Docker-Protokolle).
- Plattformereignisse: Die für Containervorgänge relevanten Ereignisse der Mesh-Plattform umfassen derzeit Containeraktivierung, -deaktivierung und -beendigung.
- Containermetriken: Diese umfassen die Ressourcenverwendung und Leistungsmetriken für Ihre Container (Docker-Statistiken).

Dieser Artikel beschreibt die Überwachungs- und Diagnoseoptionen der aktuellen Vorschauversion.

## <a name="application-logs"></a>Anwendungsprotokolle

Sie können Ihre Docker-Protokolle von Ihren bereitgestellten Containern aus pro Container einsehen. Im Service Fabric Mesh-Anwendungsmodell ist jeder Container ein Codepaket in Ihrer Anwendung. Verwenden Sie den folgenden Befehl, um die zugehörigen Protokolle mit einem Codepaket anzuzeigen:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> Mit dem Befehl „az mesh service-replica“ können Sie den Namen des Replikats abrufen. Replikatnamen sind inkrementelle Nummern ab 0.*

So sehen die Protokolle aus dem VotingWeb.Code-Container aus der Abstimmungsanwendung aus:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="next-steps"></a>Nächste Schritte
In der Übersicht erfahren Sie mehr über Service Fabric Mesh:
- [Übersicht über Service Fabric Mesh](service-fabric-mesh-overview.md)
