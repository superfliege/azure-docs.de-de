---
title: Tarif „Universell“ – Azure SQL-Datenbankdienst | Microsoft-Dokumentation
description: Informationen zum Tarif „Universell“ für Azure SQL-Datenbank
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 8b708818584be6cdb84530ce008295ccee080d94
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52885068"
---
# <a name="general-purpose-tier---azure-sql-database"></a>Tarif „Universell“ – Azure SQL-Datenbank

> [!NOTE]
> Der Tarif „Universell“ heißt „Standard“ im DTU-Tarifmodell. Einen Vergleich zwischen V-Kern-basiertem Kaufmodell und DTU-basiertem Kaufmodell finden Sie unter [Kaufmodelle für Azure SQL-Datenbank und Ressourcen](sql-database-service-tiers.md).

Azure SQL-Datenbank basiert auf der an die Cloudumgebung angepasste Architektur der SQL Server-Datenbank-Engine, um selbst bei Infrastrukturausfällen eine Verfügbarkeit von 99,99 % sicherzustellen. In Azure SQL-Datenbank werden drei Architekturmodelle verwendet:
- Allgemeiner Zweck 
- Unternehmenskritisch
- Hyperscale

Das Modell des Typs „Universell“ basiert auf der Trennung von Compute und Speicher. Dieses Architekturmodell beruht auf der Hochverfügbarkeit und Zuverlässigkeit von Azure Storage Premium mit transparenter Replikation von Datenbankdateien und garantiert ohne Datenverlust bei Ausfall der zugrunde liegenden Infrastruktur.

In der folgenden Abbildung sind vier Knoten im Architekturmodell des Typs „Standard“ mit getrennter Compute- und Speicherebene dargestellt.

![Trennung der Compute- und Speicherebene](media/sql-database-managed-instance/general-purpose-service-tier.png)

Das universelle Modell umfasst zwei Ebenen:

- Eine zustandslose Computeebene, auf der der Prozess `sqlserver.exe` ausgeführt wird und die nur temporäre und zwischengespeicherte Daten (z.B. Plancache, Pufferpool, Spaltenspeicherpool) enthält. Dieser zustandslose SQL Server-Knoten wird von der Azure Service Fabric-Plattform gesteuert, die Prozesse initialisiert, die Integrität des Knotens steuert und bei Bedarf ein Failover zu einer anderen Stelle durchführt.
- Eine zustandsbehaftete Datenebene mit Datenbankdateien (MDF- und LDF-Dateien), die in Azure Storage Premium gespeichert werden. Azure Storage garantiert die Vermeidung von Datenverlusten jeglicher Datensätze, die in Datensatzdateien platziert werden. Azure Storage verfügt über integrierte Datenverfügbarkeit und -redundanz, die sicherstellen, dass jeder Datensatz in einer Protokolldatei und jede Seite in einer Datendatei beibehalten wird, auch wenn der SQL Server-Prozess abstürzt.

Bei jeder Aktualisierung der Datenbank-Engine oder des Betriebssystems, bei Fehlern in Teilen der zugrunde liegenden Infrastruktur oder wenn im SQL Server-Prozess ein schwerwiegendes Problem erkannt wird, wird der zustandslose SQL Server-Prozess in Azure Service Fabric auf einen anderen zustandslosen Serverknoten verschoben. Es sind mehrere Reserveknoten vorhanden, auf denen im Fall eines Failovers des primären Knotens ein neuer Computedienst ausgeführt werden kann, um die Failoverzeit zu minimieren. Daten in der Azure Storage-Ebene sind nicht betroffen, und Daten- und Protokolldateien werden an den neu initialisierten SQL Server-Prozess angefügt. Dieser Prozess garantiert eine Verfügbarkeit von 99,99 %, kann jedoch aufgrund der Übergangszeit und der Tatsache, dass der neue SQL Server-Knoten mit „kaltem“ Cache gestartet wird, Auswirkungen auf die Leistung großer Workloads haben.

## <a name="when-to-choose-this-service-tier"></a>Wann sollte dieser Diensttarif gewählt werden?

Der Diensttarif „Universell“ ist ein Standarddiensttarif in Azure SQL-Datenbank, der für die meisten generischen Workloads entwickelt wurde. Wenn Sie eine vollständig verwaltete Datenbank-Engine mit einer SLA von 99,99 % mit einer Speicherlatenz von 5 bis 10 ms benötigen, die in den meisten Fällen der Azure SQL-IaaS-Lösung entspricht, ist der Tarif „Universell“ für Sie die richtige Wahl.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Tarifen [Unternehmenskritisch](sql-database-service-tier-business-critical.md) und [Hyperscale](sql-database-service-tier-hyperscale.md)
- Informationen zu [Service Fabric](../service-fabric/service-fabric-overview.md)
- Weitere Optionen zu Hochverfügbarkeit und Notfallwiederherstellung finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md).
