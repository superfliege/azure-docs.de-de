---
title: Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service zum Durchführen von Datenbankmigrationen.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: b3c974eaf4e44a80046efc4b1545f29797fcaf0c
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887665"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service
Zur Gewährleistung reibungsloser Datenbankmigrationen mit Azure Database Migration Service müssen mehrere Voraussetzungen erfüllt werden. Einige der Voraussetzungen gelten für alle unterstützten Szenarien (Quelle-Ziel-Paare) des Diensts, andere nur für ein bestimmtes Szenario.

In den folgenden Abschnitten sind die Voraussetzungen für die Verwendung von Azure Database Migration Service aufgeführt.

## <a name="prerequisites-common-across-migration-scenarios"></a>Voraussetzungen, die für alle Migrationsszenarien gelten
Folgende Voraussetzungen von Azure Database Migration Service gelten für alle unterstützten Migrationsszenarien:
- Erstellen Sie ein VNET für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bietet.
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln Ihres Azure Virtual Network (VNET) nicht die Kommunikationsports 443, 53, 9354, 445 und 12000 blockieren. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Wenn Sie eine Firewall-Appliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration zugreifen kann.
- Konfigurieren Sie Ihre [Windows-Firewall für Datenbank-Engine-Zugriff](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Aktivieren Sie das TCP/IP-Protokoll (dieses wird während der SQL Server Express-Installation standardmäßig deaktiviert), indem Sie die Anweisungen im Artikel [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) befolgen.

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Voraussetzungen für die Migration von SQL Server zu Azure SQL-Datenbank 
Neben den Voraussetzungen für Azure Database Migration Service, die für alle Migrationsszenarien gelten, gibt es auch Voraussetzungen, die nur für ein bestimmtes Szenario gelten.

Wenn Sie Azure Database Migration Service für Migrationen von SQL Server zu Azure SQL-Datenbank verwenden, gelten nicht nur die Voraussetzungen für alle Migrationsszenarien, sondern auch folgende zusätzliche Voraussetzungen:

- Erstellen Sie eine Azure SQL-Datenbankinstanz, indem Sie die Anweisungen im Artikel [Erstellen einer Azure SQL-Datenbank im Azure-Portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) befolgen.
- Laden Sie den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595), Version 3.3 oder höher, herunter, und installieren Sie ihn.
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die SQL Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 1433).
- Bei der Ausführung mehrerer benannter SQL Server-Instanzen mit dynamischen Ports empfiehlt es sich, den SQL-Browser-Dienst zu aktivieren und den Zugriff auf den UDP-Port 1434 durch Ihre Firewalls zuzulassen, sodass Azure Database Migration Service eine Verbindung mit einer benannten Instanz auf Ihrem Quellserver herstellen kann.
- Erstellen Sie für Azure SQL-Datenbankserver eine [Firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) auf Serverebene, um den Zugriff auf die Zieldatenbanken durch Azure Database Migration Service zu ermöglichen. Geben Sie den Subnetzbereich des für Azure Database Migration Service verwendeten VNET an.
- Stellen Sie sicher, dass die für die Verbindung mit der SQL Server-Quellinstanz verwendeten Anmeldeinformationen über [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql)-Berechtigungen verfügen.
- Stellen Sie sicher, dass die für die Verbindung mit der Azure SQL-Zieldatenbankinstanz verwendeten Anmeldeinformationen die Berechtigung CONTROL DATABASE für die Azure SQL-Zieldatenbanken besitzen.

   > [!NOTE]
   > Eine vollständige Liste der Voraussetzungen für die Verwendung von Azure Database Migration Service für Migrationen von SQL Server zu Azure SQL-Datenbank finden Sie im Tutorial [Migrieren von SQL Server zu Azure SQL-Datenbank](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database-managed-instance"></a>Voraussetzungen für die Migration von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz
- Erstellen Sie eine verwaltete Azure SQL-Datenbank-Instanz, indem Sie die Anweisungen im Artikel [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz im Azure-Portal](https://aka.ms/sqldbmi) befolgen.
- Öffnen Sie die Firewalls, um SMB-Datenverkehr über Port 445 für die IP-Adresse oder den Subnetzbereich von Azure Database Migration Service zuzulassen.
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die SQL Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 1433).
- Bei der Ausführung mehrerer benannter SQL Server-Instanzen mit dynamischen Ports empfiehlt es sich, den SQL-Browser-Dienst zu aktivieren und den Zugriff auf den UDP-Port 1434 durch Ihre Firewalls zuzulassen, sodass Azure Database Migration Service eine Verbindung mit einer benannten Instanz auf Ihrem Quellserver herstellen kann.
- Stellen Sie sicher, dass die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der verwalteten Zielinstanz verwendeten Anmeldungen Mitglieder der sysadmin-Serverrolle sind.
- Erstellen Sie eine Netzwerkfreigabe, mit der Azure Database Migration Service die Quelldatenbank sichern kann.
- Stellen Sie sicher, dass das Dienstkonto, unter dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für die Netzwerkfreigabe verfügt, die Sie erstellt haben, und dass das Computerkonto für den Quellserver über Lese-/Schreibzugriff auf dieselbe Freigabe verfügt.
- Notieren Sie sich einen Windows-Benutzer (mit Kennwort), der Vollzugriff auf die von Ihnen zuvor erstellte Netzwerkfreigabe hat. Azure Database Migration Service verwendet die Benutzeranmeldeinformationen, um die Sicherungsdateien für Wiederherstellungsvorgänge in den Azure Storage-Container hochzuladen.
- Erstellen Sie einen Blobcontainer, und rufen Sie dessen SAS-URI mithilfe der Schritte im Artikel [Verwalten von Azure Blob Storage-Ressourcen mit dem Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) ab. Achten Sie beim Erstellen des SAS-URI darauf, im Richtlinienfenster alle Berechtigungen (Lesen, Schreiben, Löschen, Auflisten) auszuwählen.

   > [!NOTE]
   > Eine vollständige Liste der Voraussetzungen für die Verwendung von Azure Database Migration Service für Migrationen von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz finden Sie im Tutorial [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Nächste Schritte
Eine Übersicht über Azure Database Migration Service und Informationen zur regionalen Verfügbarkeit finden Sie im Artikel [Was ist Azure Database Migration Service?](dms-overview.md). 