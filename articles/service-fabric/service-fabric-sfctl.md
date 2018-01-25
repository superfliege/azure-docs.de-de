---
title: "Azure Service Fabric CLI – sfctl | Microsoft-Dokumentation"
description: Beschreibt die sfctl-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 23adc7fa6ee53d1c5edd10b2772f8825dbcef2a8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl"></a>sfctl 
Befehle zum Verwalten von Service Fabric-Clustern und -Entitäten. Diese Version ist mit der Service Fabric 6.0 Runtime kompatibel. Befehle folgen dem Substantiv-Verb-Muster. Weitere Informationen finden Sie in den folgenden Untergruppen.

## <a name="subgroups"></a>Untergruppen
|Untergruppe|BESCHREIBUNG|
| --- | --- |
| [application](service-fabric-sfctl-application.md)| Ermöglicht es, Anwendungen und Anwendungstypen zu erstellen, zu löschen und zu verwalten.|
| [chaos](service-fabric-sfctl-chaos.md)   | Ermöglicht es, den Chaos-Testdienst zu starten und zu beenden sowie seine Berichte anzuzeigen.|
| [cluster](service-fabric-sfctl-cluster.md) | Ermöglicht es, Service Fabric-Cluster auszuwählen, zu verwalten und zu betreiben.|
| [compose](service-fabric-sfctl-compose.md) | Ermöglicht es, Docker Compose Anwendungen zu erstellen, zu löschen und zu verwalten.|
| [is](service-fabric-sfctl-is.md)      | Ermöglicht es, Befehle an den Infrastrukturdienst abzufragen und zu senden.|
| [Node](service-fabric-sfctl-node.md)    | Ermöglicht es, Knoten zu verwalten, die einen Cluster bilden.|
| [partition](service-fabric-sfctl-partition.md)  | Ermöglicht es, Partitionen für jeden Dienst abzufragen und zu verwalten.|
| [rpm](service-fabric-sfctl-rpm.md)        | Ermöglicht es, Befehle an den Reparatur-Manager-Dienst abzufragen und zu senden.|
| [replica](service-fabric-sfctl-replica.md) | Ermöglicht es, Replikate zu verwalten, die zu Servicepartitionen gehören.|
| [service](service-fabric-sfctl-service.md) | Ermöglicht es, Dienste, Diensttypen und Dienstpakete zu erstellen, zu löschen und zu verwalten.|
| [store](service-fabric-sfctl-store.md)   | Ermöglicht es, grundlegende Vorgänge auf Dateiebene für den Clusterimagespeicher auszuführen.|

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)