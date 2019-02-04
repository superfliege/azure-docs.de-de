---
title: 'Schnellstart: Einzeldatenbanken in Azure SQL-Datenbank | Microsoft-Dokumentation'
description: Hier finden Sie eine schnelle Einführung in die Verwendung von Einzeldatenbanken in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 72ffdb357b5469b71da7655104add1135bf114fb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55464808"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Erste Schritte mit Einzeldatenbanken in Azure SQL-Datenbank

Eine [Einzeldatenbank](sql-database-single-index.yml) in Azure SQL-Datenbank ist eine vollständig verwaltete PaaS-Datenbank als Dienst (Database-as-a-Service, DbaaS) und eine ideale Speicher-Engine für moderne cloudbasierte Anwendungen. In diesem Abschnitt erfahren Sie, wie Sie eine SQL-Datenbank schnell konfigurieren und erstellen.

## <a name="quickstart-overview"></a>Schnellstartübersicht

Dieser Abschnitt enthält eine Übersicht über die verfügbaren Artikel für den schnellen Einstieg in die Verwendung von Einzeldatenbanken. Ihre erste SQL-Datenbank erstellen Sie am einfachsten über das [Azure-Portal](sql-database-get-started-portal.md). Dort können Sie die erforderlichen Parameter konfigurieren.
Nach der Erstellung müssen Sie [Ihre Datenbank durch Konfigurieren von Firewallregeln schützen](sql-database-get-started-portal-firewall.md). 

Falls Sie über eine vorhandene Datenbank in SQL Server verfügen, die Sie zu Azure migrieren möchten, sollten Sie den [Datenmigrations-Assistenten (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) installieren. Dieser analysiert Ihre Datenbanken in SQL Server und ermittelt sämtliche Probleme, die unter Umständen die Migration zur Einzeldatenbank verhindern. Wurden keine Probleme gefunden, können Sie Ihre Datenbank als Datei vom Typ `.bacpac` exportieren und [über das Azure-Portal oder mithilfe von „SqlPackage“ importieren](sql-database-import.md).

In diesen Schnellstartanleitungen erfahren Sie, wie Sie Ihre Datenbanken schnell konfigurieren, erstellen und in die Azure-Cloud importieren.

## <a name="automating-management-operations"></a>Automatisieren von Verwaltungsvorgängen

Über das Azure-Portal können Sie ganz einfach eine Einzeldatenbank erstellen und ändern. Datenbanken können aber auch mithilfe von [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) oder über die [Azure-Befehlszeilenschnittstelle](scripts/sql-database-create-and-configure-database-cli.md) erstellt werden.
Darüber hinaus können Sie mithilfe von [PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md) oder über die [Azure-Befehlszeilenschnittstelle](scripts/sql-database-monitor-and-scale-database-cli.md) Ihre Einzeldatenbank aktualisieren und Ressourcen skalieren.

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>Migrieren zu einer Einzeldatenbank mit minimaler Downtime

In den Artikeln dieser Schnellstartanleitungen erfahren Sie, wie Sie Ihre Datenbank schnell erstellen oder unter Verwendung einer Datei vom Typ `.bacpac` in Azure importieren. Dateien vom Typ `.bacpac` und `.dacpack` dienen dazu, Datenbanken schnell zwischen verschiedenen Versionen von SQL Server und Azure SQL-Datenbank (Einzeldatenbank, Pool für elastische Datenbanken oder verwaltete Instanz) zu migrieren oder Continuous Integration in Ihrer DevOps-Pipeline zu implementieren. Diese Methode ist jedoch nicht für die schnelle Migration Ihrer Produktionsdatenbanken mit minimaler Downtime geeignet, da Sie warten müssen, bis die Quelldatenbank als Datei vom Typ `.bacpac` exportiert und in Azure SQL-Datenbank importiert wurde, wodurch eine gewisse Downtime für Ihre Anwendung entsteht (insbesondere bei größeren Datenbanken). Für die Verschiebung Ihrer Produktionsdatenbank benötigen Sie wahrscheinlich eine bessere Migrationsmethode mit möglichst geringer Downtime bei der Migration. Der [Datenmigrationsdienst](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) kann Ihre Datenbank mit minimaler Downtime migrieren. Dadurch können Sie Ihre Anwendung schnell und mit minimaler Downtime von der Quell- auf die Zieldatenbank umstellen.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit einer [allgemeinen Liste unterstützter Features in Azure SQL-Datenbank](sql-database-features.md) vertraut. 
* Informieren Sie sich darüber, wie Sie [Ihre Datenbank sicherer machen](sql-database-security-tutorial.md). 
* Befassen Sie sich mit [komplexeren Tutorials](sql-database-howto-single-database.md). 
* Sehen Sie sich weitere Beispielskripts für [PowerShell](sql-database-powershell-samples.md) und die [Azure-Befehlszeilenschnittstelle](sql-database-cli-samples.md) an. 
* Informieren Sie sich ausführlicher über die [Verwaltungs-API](sql-database-single-databases-manage.md), die Sie zum Konfigurieren Ihrer Datenbanken verwenden können. 
