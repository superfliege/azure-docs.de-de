---
title: Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Quelle und Ziel für den Database Migration Service konfigurieren.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/06/2018
ms.openlocfilehash: 892cff02b5b70f09236bb37ae786f180ddca9316
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service
In diesem Artikel lernen Sie verschiedene Netzwerktopologien kennen, die der Azure Database Migration Service verwenden kann, um eine nahtlose Migration von lokalen SQL-Servern zu einer verwalteten Azure SQL-Datenbank-Instanz zu ermöglichen.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Verwaltete Azure SQL-Datenbank-Instanz konfiguriert für Hybridworkloads 
Verwenden Sie diese Topologie, wenn die verwaltete Azure SQL-Datenbank-Instanz mit Ihrem lokalen Netzwerk verbunden ist. Dieser Ansatz nutzt das einfachste Netzwerkrouting bei maximalem Datendurchsatz während der Migration.

![Netzwerktopologie für Hybridworkloads](media\resource-network-topologies\hybrid-workloads.png)

**Anforderungen**
- In diesem Szenario werden die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz im selben Azure-VNET erstellt, sie nutzen jedoch unterschiedliche Subnetze.  
- Das in diesem Szenario verwendete VNET ist über ExpressRoute oder VPN auch mit dem lokalen Netzwerk verbunden.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Verwaltete Azure SQL-Datenbank-Instanz isoliert vom lokalen Netzwerk
Verwenden Sie diese Netzwerktopologie, wenn Ihre Umgebung mindestens eines der folgenden Szenarien erfordert:
- Die verwaltete Azure SQL-Datenbank-Instanz ist von der lokalen Konnektivität isoliert, aber die Azure Database Migration Service-Instanz ist mit dem lokalen Netzwerk verbunden.
- Es gelten RBAC-Richtlinien (rollenbasierte Zugriffssteuerung), und Sie schränken den Benutzerzugriff auf das Abonnement ein, das die verwaltete Azure SQL-Datenbank-Instanz hostet.
- Die für die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz verwendeten VNETs befinden sich in unterschiedlichen Abonnements.

![Netzwerktopologie für eine vom lokalen Netzwerk isolierte verwaltete Instanz](media\resource-network-topologies\mi-isolated-workload.png)

**Anforderungen**
- Das VNET, das Azure Database Migration Service für dieses Szenario verwendet, muss über ExpressRoute oder VPN auch mit dem lokalen Netzwerk verbunden sein.
- Erstellen Sie ein VNET-Netzwerkpeering zwischen dem VNET für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service.


## <a name="cloud-to-cloud-migrations"></a>Cloud-zu-Cloud-Migrationen
Verwenden Sie diese Topologie, wenn der SQL-Quellserver auf einem virtuellen Azure-Computer gehostet wird.

![Netzwerktopologie für Cloud-zu-Cloud-Migrationen](media\resource-network-topologies\cloud-to-cloud.png)

**Anforderungen**
- Erstellen Sie ein VNET-Netzwerkpeering zwischen dem VNET für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service.

## <a name="see-also"></a>Siehe auch
- [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Erstellen eines virtuellen Netzwerks im Azure-Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Nächste Schritte
Eine Übersicht über Azure Database Migration Service sowie Informationen zur regionalen Verfügbarkeit als Public Preview finden Sie im Artikel [Was ist die Vorschauversion von Azure Database Migration Service?](dms-overview.md). 