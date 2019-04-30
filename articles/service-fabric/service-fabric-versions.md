---
title: Unterstützte Clusterversionen in Azure Service Fabric | Microsoft-Dokumentation
description: Informationen zu Clusterversionen in Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/24/2018
ms.author: aljo
ms.openlocfilehash: 75e95737eecb9407a80103d1cad00d4987fe7091
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998820"
---
# <a name="supported-service-fabric-versions"></a>Unterstützte Service Fabric-Versionen

Achten Sie darauf, dass in Ihrem Cluster immer eine unterstützte Azure Service Fabric-Version ausgeführt wird. Nach der Ankündigung einer neuen Service Fabric-Version beträgt die verbleibende Supportdauer für die vorherige Version noch mindestens 60 Tage. Ankündigungen neuer Versionen finden Sie im [Blog des Service Fabric-Teams](https://blogs.msdn.microsoft.com/azureservicefabric/).

In den folgenden Dokumenten finden Sie Details dazu, wie Sie sicherstellen können, dass in Ihrem Cluster immer eine unterstützte Service Fabric-Version ausgeführt wird:

- [Upgrade von Azure Service Fabric-Clustern](service-fabric-cluster-upgrade.md)
- [Upgraden der in Ihrem Cluster ausgeführten Service Fabric-Version](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Unterstützte Versionen

Die folgende Tabelle enthält die Versionen von Service Fabric und das jeweilige Datum des Ablaufs des Supports.

| Service Fabric-Runtime im Cluster | Kann direkt von Clusterversion aktualisiert werden |Kompatible SDK- oder NuGet-Paket-Version | Ende des Supports |
| --- | --- |--- | --- |
| Alle Clusterversionen vor 5.3.121 | 5.1.158.* |Alle Versionen bis einschließlich Version 2.3 |20. Januar 2017 |
| 5.3.* | 5.1.158.* |Alle Versionen bis einschließlich Version 2.3 |24. Februar 2017 |
| 5.4.* | 5.1.158.* |Alle Versionen bis einschließlich Version 2.4 |10. Mai 2017       |
| 5.5.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.5 |10. August 2017    |
| 5.6.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.6 |13. Oktober 2017   |
| 5.7.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.7 |15. Dezember 2017  |
| 6.0.* | 5.6.205.* |Alle Versionen bis einschließlich Version 2.8 |30. März 2018     |
| 6.1.* | 5.7.221.* |Alle Versionen bis einschließlich Version 3.0 |15. Juli 2018      |
| 6.2.* | 6.0.232.* |Alle Versionen bis einschließlich Version 3.1 |26. Oktober 2018   |
| 6.3.* | 6.1.480.* |Alle Versionen bis einschließlich Version 3.2 |31. März 2019  |
| 6.4.* | 6.2.301.* |Alle Versionen bis einschließlich Version 3.3 |Aktuelle Version, daher kein Enddatum |

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die folgende Tabelle enthält die unterstützten Betriebssysteme für die unterstützten Service Fabric-Versionen.

| Betriebssystem | Erste unterstützte Service Fabric-Version |
| --- | --- |
| Windows Server 2012 R2 | Alle Versionen |
| Windows Server 2016 | Alle Versionen |
| Windows Server 1709 | 6,0 |
| Windows Server 1803 | 6.4. |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6,0 |

