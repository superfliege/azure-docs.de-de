---
title: Informationen zu Azure Service Fabric-Clusterversionen | Microsoft-Dokumentation
description: Unterstützte Azure Service Fabric-Clusterversionen
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
ms.openlocfilehash: de5522e68d1329ce2b80a4d3c7045d38c13169e5
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57191646"
---
# <a name="supported-service-fabric-versions"></a>Unterstützte Service Fabric-Versionen

Achten Sie darauf, dass in Ihrem Cluster immer eine unterstützte Service Fabric-Version ausgeführt wird. Nach der Ankündigung einer neuen Service Fabric-Version beträgt die verbleibende Supportdauer der vorherigen Version noch mindestens 60 Tage. Neue Versionen werden im [Blog des Service Fabric-Teams](https://blogs.msdn.microsoft.com/azureservicefabric/) angekündigt.

In den folgenden Dokumenten finden Sie Details dazu, wie Sie sicherstellen können, dass in Ihrem Cluster immer eine unterstützte Service Fabric-Version ausgeführt wird.

- [Upgraden der Service Fabric-Version in einem Azure-Cluster](service-fabric-cluster-upgrade.md)
- [Upgraden der Service Fabric-Version in einem eigenständigen Windows Server-Cluster](service-fabric-cluster-upgrade-windows-server.md)

In der folgenden Liste sind die unterstützten Service Fabric-Versionen und das Datum des Ablaufs des Supports aufgeführt.

| **Service Fabric-Runtime im Cluster** | **Kann direkt von Clusterversion aktualisiert werden** |**Kompatible SDK-/NuGet-Paket-Versionen** | **Ablauf des Supports** |
| --- | --- |--- | --- |
| Alle Clusterversionen vor 5.3.121 | 5.1.158* |Alle Versionen bis einschließlich Version 2.3 |20. Januar 2017 |
| 5.3.* | 5.1.158.* |Alle Versionen bis einschließlich Version 2.3 |24. Februar 2017 |
| 5.4.* | 5.1.158.* |Alle Versionen bis einschließlich Version 2.4 |10. Mai 2017       |
| 5.5.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.5 |10. August 2017    |
| 5.6.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.6 |13. Oktober 2017   |
| 5.7.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.7 |15. Dezember 2017  |
| 6.0.* | 5.6.205.* |Alle Versionen bis einschließlich Version 2.8 |30. März 2018     |
| 6.1.* | 5.7.221.* |Alle Versionen bis einschließlich Version 3.0 |15. Juli 2018      |
| 6.2.* | 6.0.232.* |Alle Versionen bis einschließlich Version 3.1 |26. Oktober 2018   |
| 6.3.* | 6.1.480.* |Alle Versionen bis einschließlich Version 3.2 |31. März 2019  |
| 6.4.* | 6.2.301.* |Alle Versionen bis einschließlich Version 3.3 |Aktuelle Version, daher kein Datum des Ablaufs des Supports |