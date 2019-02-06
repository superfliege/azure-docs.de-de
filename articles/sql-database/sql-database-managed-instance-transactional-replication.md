---
title: Transaktionsreplikation mit Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Verwendung der SQL Server-Transaktionsreplikation mit eigenständigen, in einem Pool zusammengefassten und Instanzdatenbanken in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 548bc9afb37f8c4a1c6c208a8741d1e3da0a784c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469395"
---
# <a name="transactional-replication-with-standalone-pooled-and-instance-databases-in-azure-sql-database"></a>Transaktionsreplikation mit eigenständigen, in einem Pool zusammengefassten und Instanzdatenbanken in Azure SQL-Datenbank

Die Transaktionsreplikation ist ein Feature von Azure SQL-Datenbank, verwalteten Instanzen und SQL Server, die Ihnen die Replikation von Daten aus einer Tabelle in Azure SQL-Datenbank oder SQL Server zu den in Remotedatenbanken platzierten Tabellen ermöglicht. Mit diesem Feature können Sie mehrere Tabellen in unterschiedlichen Datenbanken synchronisieren.

## <a name="when-to-use-transactional-replication"></a>Wann ist die Transaktionsreplikation zu verwenden?

Die Transaktionsreplikation ist in den folgenden Szenarien nützlich:

- Veröffentlichen von Änderungen an Tabellen in einer Datenbank, die an SQL Server-Instanzen oder Azure SQL-Datenbanken verteilt werden, die die Änderungen abonniert haben
- Synchronisierthalten mehrerer verteilter Datenbanken
- Migrieren von Datenbanken aus einer SQL Server- oder einer verwalteten Instanz zu einer anderen Datenbank durch fortlaufendes Veröffentlichen der Änderungen

## <a name="overview"></a>Übersicht

Die wichtigsten Komponenten der Transaktionsreplikation sind in der folgenden Abbildung dargestellt:  

![Replikation zu SQL-Datenbank](media/replication-to-sql-database/replication-to-sql-database.png)


Der **Herausgeber** ist eine Instanz oder ein Server, die bzw. der Änderungen an Tabellen (Artikeln) veröffentlicht, indem die Aktualisierungen an den Verteiler gesendet werden. Das Veröffentlichen in einer Azure SQL-Datenbank von einem lokalen SQL Server wird unter den folgenden Versionen von SQL Server unterstützt:

    - SQL Server 2019 (Vorschauversion)
    - SQL Server 2016 bis SQL 2017
    - SQL Server 2014 SP1 CU3 oder höher (12.00.4427)
    - SQL Server 2014 RTM CU10 (12.00.2556)
    - SQL Server 2012 SP3 oder höher (11.0.6020)
    - SQL Server 2012 SP2 CU8 (11.0.5634.0)
    - Für andere Versionen von SQL Server, die keine Veröffentlichung in Objekten in Azure unterstützen, kann die Methode der [erneuten Veröffentlichung von Daten](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) verwendet werden, um Daten in neuere Versionen von SQL Server verschieben. 

Der **Verteiler** ist eine Instanz oder ein Server, die bzw. der Änderungen an den Artikeln von einem Herausgeber erfasst und an die Abonnenten verteilt. Der Verteiler kann entweder eine verwaltete Azure SQL-Datenbank-Instanz oder eine SQL Server-Instanz (beliebige Version, sofern nicht älter als die Herausgeber-Version) sein. 

Der **Abonnent** ist eine Instanz oder ein Server, die bzw. der die auf dem Herausgeber vorgenommenen Änderungen erhält. Abonnenten können eigenständige, in einem Pool zusammengefasste und Instanzdatenbanken in Azure SQL-Datenbank oder SQL-Serverdatenbanken sein. Ein Abonnent einer eigenständigen oder in einem Pool zusammengefassten Datenbank muss als Push-Abonnent konfiguriert sein. 

| Rolle | Eigenständige und in einem Pool zusammengefasste Datenbanken | Instanzdatenbanken |
| :----| :------------- | :--------------- |
| **Herausgeber** | Nein  | JA | 
| **Verteiler** | Nein  | JA|
| **Pull-Abonnent** | Nein  | JA|
| **Push-Abonnent**| JA | JA|
| &nbsp; | &nbsp; | &nbsp; |

Es gibt verschiedene [Replikationstypen](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication?view=sql-server-2017):


| Replikation | Eigenständige und in einem Pool zusammengefasste Datenbanken | Instanzdatenbanken|
| :----| :------------- | :--------------- |
| [**Transaktion**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Ja (nur als Abonnent) | JA | 
| [**Momentaufnahme**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Ja (nur als Abonnent) | JA|
| [**Mergereplikation**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Nein  | Nein |
| [**Peer-to-Peer**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Nein  | Nein |
| **Unidirektional** | JA | JA|
| [**Bidirektional**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Nein  | JA|
| [**Aktualisierbare Abonnements**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Nein  | Nein |
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - Der Versuch, Replikationen mit einer älteren Version zu konfigurieren, kann zu dem Fehler mit der Nummer MSSQL_REPL20084 (Der Prozess konnte keine Verbindung mit dem Abonnenten herstellen.) oder MSSQ_REPL40532 (Der von der Anmeldung angeforderte Server \<Name> kann nicht geöffnet werden) führen. Die Anmeldung ist fehlgeschlagen.)
  > - Um alle Features von Azure SQL-Datenbank verwenden zu können, müssen Sie die neuesten Versionen von [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) und [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017) verwenden.

## <a name="requirements"></a>Requirements (Anforderungen)

- Für die Verbindung zwischen den Teilnehmern der Replikation wird SQL-Authentifizierung verwendet. 
- Ein Azure-Speicherkonto für das von der Replikation verwendete Arbeitsverzeichnis. 
- Port 445 (TCP ausgehend) muss in den Sicherheitsregeln des Subnetzes der verwalteten Instanz geöffnet sein, um auf die Azure-Dateifreigabe zugreifen zu können. 
- Port 1433 (TCP ausgehend) muss geöffnet werden, wenn sich der Herausgeber/Verteiler auf einer verwalteten Instanz und der Abonnent in der lokalen Umgebung befindet. 

## <a name="common-configurations"></a>Häufig verwendete Konfigurationen

Im Allgemeinen müssen sich Herausgeber und Verteiler gemeinsam entweder in der Cloud oder an einem lokalen Standort befinden. Die folgenden Konfigurationen werden unterstützt: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Herausgeber mit lokalem Verteiler in verwalteter Instanz

![Einzelinstanz als Herausgeber und Verteiler ](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

Herausgeber und Verteiler sind in einer einzelnen verwalteten Instanz konfiguriert und verteilen Änderungen an andere verwaltete Instanzen, Einzeldatenbanken, in einem Pool zusammengefasste Datenbanken oder lokale SQL Server-Instanzen. In dieser Konfiguration kann die verwaltete Herausgeber-/Verteilerinstanz nicht mit [Georeplikation und Auto-Failovergruppen](sql-database-auto-failover-group.md) konfiguriert werden.

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Herausgeber mit Remoteverteiler in verwalteter Instanz

In dieser Konfiguration veröffentlicht eine verwaltete Instanz Änderungen auf dem Verteiler in einer anderen verwalteten Instanz, die zahlreiche verwaltete Quellinstanzen bedienen und Änderungen an ein oder mehrere Ziele in verwalteten Instanzen, Einzeldatenbanken, in einem Pool zusammengefassten Datenbanken oder SQL Server-Instanzen verteilen kann.

![Separate Instanzen als Herausgeber und Verteiler](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

Herausgeber und Verteiler werden in zwei verwalteten Instanzen konfiguriert. Bei dieser Konfiguration gilt Folgendes:

- Beide verwaltete Instanzen befinden sich im gleichen VNET.
- Beide verwaltete Instanzen befinden sich am gleichen Standort.
- Verwaltete Instanzen, die veröffentlichte Datenbanken und Verteilerdatenbanken hosten, können nicht [mithilfe von Auto-Failovergruppen georepliziert werden](sql-database-auto-failover-group.md).

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-standalone-pooled-and-instance-database"></a>Herausgeber und Verteiler am lokalen Standort mit einem Abonnenten einer eigenständigen, in einem Pool zusammengefassten und Instanzdatenbank 

![Azure SQL-DB als Abonnent](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
In dieser Konfiguration ist eine Azure SQL-Datenbank (eigenständige, in einem Pool zusammengefasste und Instanzdatenbank) ein Abonnent. Diese Konfiguration unterstützt die Migration vom lokalen Standort zu Azure. Ein Abonnent einer eigenständigen oder in einem Pool zusammengefassten Datenbank muss im Pushmodus sein.  

## <a name="next-steps"></a>Nächste Schritte

1. [Konfigurieren Sie Transaktionsreplikation für eine verwaltete Instanz](replication-with-sql-database-managed-instance.md#configure-publishing-and-distribution-example). 
1. [Erstellen Sie eine Veröffentlichung](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Erstellen Sie ein Push-Abonnements](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) mit dem Servernamen von Azure SQL-Datenbank als Abonnent (z.B. `N'azuresqldbdns.database.windows.net`) und dem Namen der Azure SQL-Datenbank als Zieldatenbank (z. B. **AdventureWorks**). )


## <a name="see-also"></a>Siehe auch  

- [Replikation zu SQL-Datenbank-Einzeldatenbanken und in einem Pool zusammengefassten Datenbanken](replication-to-sql-database.md)
- [Replikation zu einer verwalteten Instanz](replication-with-sql-database-managed-instance.md)
- [Erstellen einer Veröffentlichung](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Erstellen eines Pushabonnements](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Replikationstypen](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Überwachen (Replikation)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Initialisieren eines Abonnements](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
