---
title: Häufig gestellte Fragen zu Azure Database Migration Service | Microsoft-Dokumentation
description: Hier finden Sie häufig gestellte Fragen zur Durchführung von Datenbankmigrationen unter Verwendung von Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/08/2018
ms.openlocfilehash: 0e10907ac3db2d792b73e0a37aa2d0f6a916d746
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850071"
---
# <a name="faq-about-using-the-azure-database-migration-service"></a>Häufig gestellte Fragen zu Azure Database Migration Service
Dieser Artikel enthält häufig gestellte Fragen zur Verwendung von Azure Database Migration Service sowie entsprechende Antworten.

### <a name="q-what-is-azure-database-migration-service"></a>F: Was ist Azure Database Migration Service?
Azure Database Migration Service ist ein vollständig verwalteter Dienst, der die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen mit minimaler Downtime ermöglicht. Der Dienst ist derzeit in Public Preview, und bei der Entwicklung wurden folgende Schwerpunkte berücksichtigt:
- Zuverlässigkeit und Leistung
- Iteratives Hinzufügen von Quelle-Ziel-Paaren
- Kontinuierliche Investition in reibungslose Migrationen

### <a name="q-what-source-target-pairs-does-the-azure-database-migration-service-currently-support"></a>F: Welche Quelle-Ziel-Paare werden derzeit von Azure Database Migration Service unterstützt?
Der als Public Preview erhältliche Dienst unterstützt derzeit die Migration von SQL Server zu Azure SQL-Datenbank, und Sie können nun zum Azure-Portal wechseln, um Azure Database Migration Service für dieses Szenario zu verwenden. Weitere Quelle-Ziel-Paare (beispielsweise SQL Server zu verwalteter Azure SQL-Datenbank-Instanz oder Oracle zu Azure SQL-Datenbank) sind im Rahmen einer eingeschränkten privaten Vorschauversion verfügbar. Interessenten für die eingeschränkte private Vorschauversion können sich [hier](https://sqldatabase-migrationpreview.azurewebsites.net/) registrieren.

### <a name="q-how-does-the-azure-database-migration-service-compare-to-other-microsoft-database-migration-tools-such-as-the-database-migration-assistant-dma-or-sql-server-migration-assistant-ssma"></a>F: Inwiefern unterscheidet sich Azure Database Migration Service von anderen Microsoft-Datenbankmigrationstools wie DBA (Database Migration Assistant) oder SSMA (SQL Server Migration Assistant)?
Azure Database Migration Service ist die bevorzugte Methode für bedarfsorientierte Datenbankmigrationen zu Microsoft Azure. Ausführlichere Informationen zu den Unterschieden zwischen Azure Database Migration Service und anderen Microsoft-Datenbankmigrationstools sowie Empfehlungen für die Verwendung des Diensts in verschiedenen Szenarien finden Sie im Blogbeitrag [Differentiating Microsoft’s Database Migration Tools and Services](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/) (Abgrenzung der Datenbankmigrationstools und -dienste von Microsoft).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-the-azure-migrate-offering"></a>F: Inwiefern unterscheidet sich Azure Database Migration Service vom Azure Migrate-Angebot?
Der Azure Migrate-Dienst unterstützt Benutzer bei der Migration lokaler virtueller Computer zu Azure IaaS. Der Dienst bewertet die Eignung für die Migration und die leistungsbasierte Größenauslegung und stellt Kostenschätzungen für die Ausführung Ihrer lokalen virtuellen Computer in Azure bereit. Azure Migrate ist hilfreich bei Lift & Shift-Migrationen lokaler, VM-basierter Workloads zu virtuellen Azure IaaS-Computern. Im Gegensatz zu Azure Database Migration Service handelt es sich bei Azure Migrate allerdings nicht um einen speziellen Datenbankmigrationsdienst für relationale Azure PaaS-Datenbankplattformen wie Azure SQL-Datenbank, SQL Azure oder eine verwaltete Azure SQL-Datenbank-Instanz.

### <a name="q-what-versions-of-sql-server-does-the-azure-database-migration-service-support-as-a-source"></a>F: Welche Versionen von SQL Server werden von Azure Database Migration Service als Datenquelle unterstützt?
Für die Migration von SQL Server unterstützt Azure Database Migration Service die SQL Server-Versionen 2005 bis 2017.

### <a name="q-what-is-a-summary-of-the-steps-required-to-use-the-azure-database-migration-service-to-perform-a-database-migration"></a>F: Welche Schritte sind allgemein für eine Datenbankmigration mit Azure Database Migration Service erforderlich?
Eine typische einfache Datenbankmigration umfasst Folgendes:
1.  Erstellen von Zieldatenbanken
2.  Migrieren des Schemas der Datenbanken per [Database Migration Assistant](https://www.microsoft.com/en-us/download/details.aspx?id=53595)
3.  Erstellen einer Instanz von Azure Database Migration Service
4.  Erstellen eines Migrationsprojekts unter Angabe der Quelldatenbanken, der Zieldatenbanken und der zu migrierenden Tabellen
5.  Initiieren des vollständigen Ladens
6.  Auswählen der anschließenden Überprüfung
7.  Durchführen eines manuellen Wechsels zur neuen cloudbasierten Datenbank für Ihre Produktionsumgebung 

### <a name="q-what-are-the-prerequisites-for-using-the-azure-database-migration-service"></a>F: Welche Voraussetzungen müssen für die Verwendung von Azure Database Migration Service erfüllt sein?
Zur Gewährleistung reibungsloser Datenbankmigrationen mit Azure Database Migration Service müssen mehrere Voraussetzungen erfüllt werden. Einige der Voraussetzungen gelten für alle unterstützten Szenarien (Quelle-Ziel-Paare) des Diensts, andere nur für ein bestimmtes Szenario.
Folgende Voraussetzungen von Azure Database Migration Service gelten für alle unterstützten Migrationsszenarien:
- Erstellen Sie ein VNET für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bietet.
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln Ihres Azure Virtual Network (VNET) nicht die Kommunikationsports 443, 53, 9354, 445 und 12000 blockieren. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Wenn Sie eine Firewall-Appliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration zugreifen kann.
 
Eine Liste mit allen Voraussetzungen für spezifische Migrationsszenarien mit Azure Database Migration Service finden Sie auf docs.microsoft.com in den entsprechenden Tutorials der [Azure Database Migration Service-Dokumentation](https://docs.microsoft.com/azure/dms/dms-overview).

### <a name="q-how-do-i-find-the-ip-address-for-the-azure-database-migration-service-so-that-i-can-create-an-allow-list-for-the-firewall-rules-used-to-access-my-source-database-for-migration"></a>F: Wie finde ich die IP-Adresse für Azure Database Migration Service, damit ich eine Liste zugelassener IP-Adressen für die Firewallregeln erstellen kann, die für den Zugriff auf meine Quelldatenbank für die Migration verwendet werden?
Möglicherweise müssen Firewallregeln hinzugefügt werden, damit Azure Database Migration Service auf Ihre Quelldatenbank für die Migration zugreifen kann. Die IP-Adresse für den Dienst ist zwar dynamisch, bei Verwendung von ExpressRoute wird sie allerdings privat durch Ihr Unternehmensnetzwerk zugewiesen. Die einfachste Möglichkeit zur Ermittlung der entsprechenden IP-Adresse besteht darin, in der Ressourcengruppe, in der sich auch Ihre bereitgestellte Azure Database Migration Service-Ressource befindet, nach der zugeordneten Netzwerkschnittstelle zu suchen. Der Name der Netzwerkschnittstellenressource beginnt üblicherweise mit dem NIC-Präfix, gefolgt von einer eindeutigen Zeichen- und Nummernfolge (Beispiel: NIC-jj6tnztnmarpsskr82rbndyp). Wenn Sie auf diese Netzwerkschnittstellenressource klicken, wird die IP-Adresse angezeigt, die auf der Ressourcenübersichtsseite im Azure-Portal in die Liste zugelassener IP-Adressen aufgenommen werden muss.

Gegebenenfalls müssen Sie der Zulassungsliste auch die Portquelle hinzufügen, an der SQL Server lauscht. Dies ist standardmäßig der Port 1433. Es kann jedoch sein, dass die SQL Server-Quellinstanz für das Lauschen an anderen Ports konfiguriert wurde. In diesem Fall müssen Sie diese Ports ebenfalls der Zulassungsliste hinzufügen. Den Port, an dem SQL Server lauscht, können Sie mithilfe einer Abfrage vom Typ „Dynamische Verwaltungssicht“ ermittelt:

```sql
    SELECT DISTINCT 
        local_tcp_port 
    FROM sys.dm_exec_connections 
    WHERE local_tcp_port IS NOT NULL
```
Zum Ermitteln des Ports, an dem SQL Server lauscht, können Sie auch das SQL Server-Fehlerprotokoll abfragen:

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on' 
    GO
```

### <a name="q-are-there-any-recommendations-for-optimizing-the-performance-of-the-azure-database-migration-service"></a>F: Gibt es Leistungsoptimierungsempfehlungen für Azure Database Migration Service?
Die Datenmigration mit diesem Dienst lässt sich durch folgende Maßnahmen beschleunigen:
- Verwenden Sie beim Erstellen Ihrer Dienstinstanz den universellen Tarif mit mehreren CPUs, damit dem Dienst mehrere vCPUs für Parallelisierung und schnellere Datenübertragungen zur Verfügung stehen.
- Skalieren Sie Ihre Azure SQL-Datenbank-Zielinstanz während der Datenmigration vorübergehend zentral auf die Premium-Tarifebene hoch, um die Drosselung durch Azure SQL-Datenbank zu minimieren, die bei SKUs einer niedrigeren Ebene unter Umständen zu einer Beeinträchtigung der Datenübertragungsaktivitäten führt.

### <a name="q-how-do-i-set-up-an-azure-virtual-network"></a>F: Wie richte ich ein virtuelles Azure-Netzwerk ein?
Die Einrichtung eines Azure-VNets wird in mehreren Microsoft-Tutorials ausführlich beschrieben. Die offizielle Dokumentation befindet sich jedoch im Artikel [Virtuelles Azure-Netzwerk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

### <a name="q-why-is-my-azure-database-migration-service-unavailable-or-stopped"></a>F: Warum ist mein Azure Database Migration Service nicht verfügbar oder wurde beendet?
Wenn der Benutzer den Azure Database Migration Service (DMS) explizit beendet oder der Dienst für einen Zeitraum von 24 Stunden inaktiv ist, wird der Dienst beendet oder automatisch angehalten. In jedem Fall ist der Dienst nicht verfügbar und beendet.  Um aktive Migrationen wieder aufzunehmen, starten Sie den Dienst neu.

### <a name="q-where-can-i-leave-feedback-about-the-azure-database-migration-service"></a>F: Wo kann ich Feedback zu Azure Database Migration Service abgeben?
Teilen Sie uns Ihre Meinung mit! Feedback und Vorschläge für Azure Database Migration Service können Sie uns über [UserVoice](https://feedback.azure.com/forums/906100-azure-database-migration-service) zukommen lassen.

## <a name="next-steps"></a>Nächste Schritte
Eine Übersicht über Azure Database Migration Service sowie Informationen zur regionalen Verfügbarkeit als Public Preview finden Sie im Artikel [Was ist die Vorschauversion von Azure Database Migration Service?](dms-overview.md). 
