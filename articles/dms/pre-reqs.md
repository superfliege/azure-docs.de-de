---
title: "Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service | Microsoft-Dokumentation"
description: "Hier finden Sie eine Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service zum Durchführen von Datenbankmigrationen."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/25/2018
ms.openlocfilehash: a103bb4802bc4a20b6d82f0c6bb427133d9e5643
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service
Zur Gewährleistung reibungsloser Datenbankmigrationen mit Azure Database Migration Service müssen mehrere Voraussetzungen erfüllt werden. Einige der Voraussetzungen gelten für alle unterstützten Szenarien (Quelle-Ziel-Paare) des Diensts, andere nur für ein bestimmtes Szenario.

In den folgenden Abschnitten sind die Voraussetzungen für die Verwendung von Azure Database Migration Service aufgeführt.

## <a name="prerequisites-common-across-migration-scenarios"></a>Voraussetzungen, die für alle Migrationsszenarien gelten
Folgende Voraussetzungen von Azure Database Migration Service gelten für alle unterstützten Migrationsszenarien:
- Erstellen Sie ein VNET für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bietet.
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln Ihres Azure Virtual Network (VNET) nicht die Kommunikationsports 443, 53, 9354, 445 und 12000 blockieren. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Wenn Sie eine Firewall-Appliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration zugreifen kann.

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Voraussetzungen für die Migration von SQL Server zu Azure SQL-Datenbank 
Neben den Voraussetzungen für Azure Database Migration Service, die für alle Migrationsszenarien gelten, gibt es auch Voraussetzungen, die nur für ein bestimmtes Szenario gelten.

Wenn Sie Azure Database Migration Service für Migrationen von SQL Server zu Azure SQL-Datenbank verwenden, gelten nicht nur die Voraussetzungen für alle Migrationsszenarien, sondern auch folgende zusätzliche Voraussetzungen:
- Konfigurieren Sie Ihre [Windows-Firewall für Datenbank-Engine-Zugriff](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Aktivieren Sie das TCP/IP-Protokoll (dieses wird während der SQL Server Express-Installation standardmäßig deaktiviert), indem Sie die Anweisungen im Artikel [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) befolgen.
- Erstellen Sie eine Azure SQL-Datenbankinstanz, indem Sie die Anweisungen im Artikel [Erstellen einer Azure SQL-Datenbank im Azure-Portal](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal) befolgen.
- Laden Sie den [Data Migration Assistant](https://www.microsoft.com/en-us/download/details.aspx?id=53595), Version 3.3 oder höher, herunter, und installieren Sie ihn.
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die Quelldatenbank(en) zugreifen kann.
- Erstellen Sie für Azure SQL-Datenbankserver eine [Firewallregel](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) auf Serverebene, um den Zugriff auf die Zieldatenbanken durch Azure Database Migration Service zu ermöglichen. Geben Sie den Subnetzbereich des für Azure Database Migration Service verwendeten VNET an.
- Stellen Sie sicher, dass die für die Verbindung mit der SQL Server-Quellinstanz verwendeten Anmeldeinformationen über [CONTROL SERVER](https://docs.microsoft.com/en-us/sql/t-sql/statements/grant-server-permissions-transact-sql)-Berechtigungen verfügen.
- Stellen Sie sicher, dass die für die Verbindung mit der Azure SQL-Zieldatenbankinstanz verwendeten Anmeldeinformationen die Berechtigung CONTROL DATABASE für die Azure SQL-Zieldatenbanken besitzen.

   > [!NOTE]
   > Eine vollständige Liste der Voraussetzungen für die Verwendung von Azure Database Migration Service für Migrationen von SQL Server zu Azure SQL-Datenbank finden Sie im Tutorial [Migrieren von SQL Server zu Azure SQL-Datenbank](https://docs.microsoft.com/en-us/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="next-steps"></a>Nächste Schritte
Eine Übersicht über Azure Database Migration Service sowie Informationen zur regionalen Verfügbarkeit als Public Preview finden Sie im Artikel [Was ist die Vorschauversion von Azure Database Migration Service?](dms-overview.md). 