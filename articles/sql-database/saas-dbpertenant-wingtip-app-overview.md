---
title: 'Azure SQL-Datenbank: Beispiel für mehrinstanzenfähige App – Wingtip-SaaS | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie eine mehrinstanzenfähige Beispielanwendung nutzen, die Azure SQL-Datenbank verwendet – das Wingtip-SaaS-Beispiel.
keywords: Tutorial zur SQL-Datenbank
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 11/12/2017
ms.author: sstein
ms.openlocfilehash: 451e1fc87fc5f626e78760d8cd5c4115ea02bb0d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="introduction-to-a-multi-tenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Einführung in eine mehrinstanzenfähige SaaS-App, die das Muster mit einer Datenbank pro Mandant mit SQL-Datenbank verwendet

Die Anwendung *Wingtip SaaS* ist eine Beispiel-App mit mehreren Mandanten. Die App verwendet das SaaS-Anwendungsmuster mit einer Datenbank pro Mandant, um mit mehreren Mandanten zu kommunizieren. Die App stellt Features von Azure SQL-Datenbank vor, die mithilfe verschiedener SaaS-Entwurfs- und -Verwaltungsmuster SaaS-Szenarien ermöglichen. Um einen schnellen Einstieg sicherzustellen, wird die SaaS-App Wingtip in weniger als fünf Minuten bereitgestellt.

Der Quellcode der Anwendung und die Verwaltungsskripts sind im GitHub-Repository [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) verfügbar. Bevor Sie beginnen, lesen Sie die Schritte zum Herunterladen und Entsperren der Wingtip Tickets-Verwaltungsskripts unter [Allgemeine Hinweise](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="application-architecture"></a>Anwendungsarchitektur

Die SaaS-App Wingtip verwendet das Modell mit einer Datenbank pro Mandant sowie Pools für elastische SQL-Datenbanken, um die Effizienz zu maximieren. Für die Bereitstellung von Mandanten und deren Zuordnung mit den jeweiligen Daten wird eine Katalogdatenbank verwendet. Die SaaS-Kernanwendung Wingtip verwendet einen Pool mit drei Beispielmandanten sowie eine Katalogdatenbank. Um die zahlreichen Tutorials zur SaaS-Anwendung Wingtip zu absolvieren, sind bei der ersten Bereitstellung Add-Ons erforderlich, wenn analytische Datenbanken, datenbankübergreifende Schemaverwaltung usw. eingeführt werden.


![Architektur der SaaS-Anwendung Wingtip](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Wenn Sie die Tutorials durchgehen und mit der App arbeiten, ist es wichtig, sich auf die SaaS-Muster zu konzentrieren, da sie im Zusammenhang mit der Datenebene stehen. Mit anderen Worten: Konzentrieren Sie sich auf die Datenebene, und nehmen Sie keine übermäßige Analyse der App selbst vor. Es ist von entscheidender Bedeutung, einen klaren Überblick über die Implementierung dieser SaaS-Muster zu besitzen, wenn Sie die Muster in Ihren Anwendungen implementieren und gleichzeitig nötige Modifizierungen für Ihre konkreten Geschäftsanforderungen berücksichtigen möchten.

## <a name="sql-database-wingtip-saas-tutorials"></a>Tutorials zur SaaS-App Wingtip für die SQL-Datenbank

Untersuchen Sie nach der Bereitstellung der App die folgenden Tutorials, die auf der erstmaligen Bereitstellung aufbauen. In diesen Tutorials werden allgemeine SaaS-Muster behandelt, die die integrierten Funktionen der SQL-Datenbank sowie der von SQL Data Warehouse und anderen Azure-Dienste nutzen. Die Tutorials enthalten PowerShell-Skripts sowie ausführliche Erläuterungen, die das Verständnis und das Implementieren derselben SaaS-Verwaltungsmustern in Ihren Anwendungen erheblich erleichtern.


| Tutorial | BESCHREIBUNG |
|:--|:--|
| [Anweisungen und Tipps zu einer mehrinstanzenfähigen SaaS-Beispiel-App in Azure SQL-Datenbank](saas-tenancy-wingtip-app-guidance-tips.md) | **BEGINNEN SIE HIER.** Laden Sie PowerShell-Skripts herunter, und führen Sie sie aus, um Teile der Anwendung vorzubereiten. |
|[Bereitstellen und Kennenlernen der SaaS-Anwendung Wingtip](saas-dbpertenant-get-started-deploy.md)|  Stellen Sie die SaaS-Anwendung Wingtip in Ihrem Azure-Abonnement bereit, und machen Sie sich mit ihr vertraut. |
|[Bereitstellen und Katalogisieren von Mandanten](saas-dbpertenant-provision-and-catalog.md)| Hier erfahren Sie, wie die Anwendung eine Verbindung mit einer Katalogdatenbank herstellt und wie den Mandanten im Katalog die entsprechenden Daten zugeordnet werden. |
|[Überwachen und Verwalten der Leistung](saas-dbpertenant-performance-monitoring.md)| Hier erfahren Sie, wie Überwachungsfunktionen der SQL-Datenbank verwendet und Benachrichtigungen festgelegt werden, wenn Leistungsschwellenwerte überschritten werden. |
|[Überwachen mit Log Analytics (OMS)](saas-dbpertenant-log-analytics.md) | Hier erfahren Sie, wie mithilfe von [Log Analytics](../log-analytics/log-analytics-overview.md) große Mengen von Ressourcen über mehrere Pools hinweg überwacht werden. |
|[Wiederherstellen einzelner Mandanten](saas-dbpertenant-restore-single-tenant.md)| Hier erfahren Sie, wie Sie eine Mandantendatenbank zu einem früheren Zeitpunkt wiederherstellen. Die Schritte zum Wiederherstellen einer parallelen Datenbank und die Onlineschaltung der vorhandenen Mandantendatenbank werden ebenfalls erläutert. |
|[Manage schema for multiple tenants in the WTP SaaS application](saas-tenancy-schema-management.md) (Verwalten des Schemas für mehrere Mandanten in der SaaS-Anwendung WTP)| Erfahren Sie, wie Sie Schemas und Verweisdaten in allen Mandantendatenbanken aktualisieren. |
|[Ausführen von mandantenübergreifenden verteilten Abfragen](saas-tenancy-cross-tenant-reporting.md) | Erstellen Sie eine Datenbank für Ad-hoc-Analysen, und führen Sie verteilte Echtzeitabfragen für alle Mandanten aus.  |
|[Ausführen von Analysen für extrahierte Mandantendaten](saas-tenancy-tenant-analytics.md) | Extrahieren Sie Mandantendaten in eine Analysedatenbank oder ein Data Warehouse für Offlineanalyseabfragen. |


## <a name="next-steps"></a>Nächste Schritte

- [Allgemeine Richtlinien und Tipps für das Bereitstellen und Verwenden des Wingtip Tickets-SaaS-App-Beispiels](saas-tenancy-wingtip-app-guidance-tips.md)

- [Bereitstellen der SaaS-Anwendung Wingtip](saas-dbpertenant-get-started-deploy.md)
