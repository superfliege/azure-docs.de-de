---
title: Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Quelle und Ziel für den Database Migration Service konfigurieren.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: 39bcea36f3599530413aa9fc4dbb308ee2fb1681
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49066852"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service
In diesem Artikel werden verschiedene Netzwerktopologien erläutert, die Azure Database Migration Service verwenden kann, um eine umfassende Migration von lokalen SQL Server-Instanzen zu einer verwalteten Azure SQL-Datenbank-Instanz zu ermöglichen.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Verwaltete Azure SQL-Datenbank-Instanz konfiguriert für Hybridworkloads 
Verwenden Sie diese Topologie, wenn die verwaltete Azure SQL-Datenbank-Instanz mit Ihrem lokalen Netzwerk verbunden ist. Dieser Ansatz nutzt das einfachste Netzwerkrouting bei maximalem Datendurchsatz während der Migration.

![Netzwerktopologie für Hybridworkloads](media\resource-network-topologies\hybrid-workloads.png)

**Anforderungen**
- In diesem Szenario werden die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz im selben Azure-VNET erstellt, sie nutzen jedoch unterschiedliche Subnetze.  
- Das in diesem Szenario verwendete VNET ist über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) auch mit dem lokalen Netzwerk verbunden.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Verwaltete Azure SQL-Datenbank-Instanz isoliert vom lokalen Netzwerk
Verwenden Sie diese Netzwerktopologie, wenn Ihre Umgebung mindestens eines der folgenden Szenarien erfordert:
- Die verwaltete Azure SQL-Datenbank-Instanz ist von der lokalen Konnektivität isoliert, aber die Azure Database Migration Service-Instanz ist mit dem lokalen Netzwerk verbunden.
- Es gelten RBAC-Richtlinien (rollenbasierte Zugriffssteuerung), und Sie müssen den Benutzerzugriff auf das Abonnement beschränken, unter dem die verwaltete Azure SQL-Datenbank-Instanz gehostet wird.
- Die für die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz verwendeten VNETs befinden sich in unterschiedlichen Abonnements.

![Netzwerktopologie für eine vom lokalen Netzwerk isolierte verwaltete Instanz](media\resource-network-topologies\mi-isolated-workload.png)

**Anforderungen**
- Das VNET, das Azure Database Migration Service für dieses Szenario verwendet, muss über ExpressRoute (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) auch mit dem lokalen Netzwerk verbunden sein.
- Richten Sie ein [VNET-Netzwerkpeering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zwischen dem VNET für die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz ein.


## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Cloud-zu-Cloud-Migrationen: Gemeinsam genutztes VNET

Verwenden Sie diese Topologie, wenn die SQL Server-Quelle auf einer Azure-VM gehostet wird und dasselbe VNET wie die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz nutzt.

![Netzwerktopologie für Cloud-zu-Cloud-Migrationen mit einem gemeinsam genutzten VNET](media\resource-network-topologies\cloud-to-cloud.png)

**Anforderungen**
- Es bestehen keine weiteren Anforderungen.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Cloud-zu-Cloud-Migrationen: Isoliertes VNET

Verwenden Sie diese Netzwerktopologie, wenn Ihre Umgebung mindestens eines der folgenden Szenarien erfordert:
- Die verwaltete Azure SQL-Datenbank-Instanz wird in einem isolierten VNET bereitgestellt.
- Es gelten RBAC-Richtlinien (rollenbasierte Zugriffssteuerung), und Sie müssen den Benutzerzugriff auf das Abonnement beschränken, unter dem die verwaltete Azure SQL-Datenbank-Instanz gehostet wird.
- Die für die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz verwendeten VNETs befinden sich in unterschiedlichen Abonnements.

![Netzwerktopologie für Cloud-zu-Cloud-Migrationen mit einem isolierten VNET](media\resource-network-topologies\cloud-to-cloud-isolated.png)

**Anforderungen**
- Richten Sie ein [VNET-Netzwerkpeering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zwischen dem VNET für die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz ein.


## <a name="see-also"></a>Siehe auch
- [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Erstellen eines virtuellen Netzwerks im Azure-Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Nächste Schritte
Eine Übersicht über Azure Database Migration Service sowie Informationen zur regionalen Verfügbarkeit als Public Preview finden Sie im Artikel [Was ist die Vorschauversion von Azure Database Migration Service?](dms-overview.md). 