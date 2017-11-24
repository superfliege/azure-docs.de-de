---
title: "Azure SQL-Datenbank: Beispiel für mehrinstanzenfähige App – Wingtip-SaaS | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie eine mehrinstanzenfähige Beispielanwendung nutzen, die Azure SQL-Datenbank verwendet – das Wingtip-SaaS-Beispiel."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: sstein
ms.openlocfilehash: ddd51c23c7e7d01e38b02c79c27d1951eea61e70
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-a-sql-database-multi-tenant-saas-app-example"></a>Einführung zum Beispiel einer mehrinstanzenfähigen SQL-Datenbank-SaaS-App

Bei der SaaS-Anwendung *Wingtip* handelt es sich um eine mehrinstanzenfähige Beispiel-App, mit der die einzigartigen Vorteile der SQL-Datenbank veranschaulicht werden. Die App verwendet eine SaaS-Anwendungsmuster mit Datenbank pro Mandant, um mit mehreren Mandanten zu kommunizieren. Die App ist auf die Vorstellung der Funktionen von Azure SQL-Datenbank ausgelegt, die SaaS-Szenarien ermöglichen, einschließlich verschiedener SaaS-Entwurfs- und -Verwaltungsmuster. Um einen schnellen Einstieg sicherzustellen, wird die SaaS-App Wingtip in weniger als fünf Minuten bereitgestellt.

Der Quellcode und die Verwaltungsskripts der Anwendung stehen im GitHub-Repository [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) zur Verfügung. Um die Skripts auszuführen, [laden Sie den Ordner „Learning Modules“](#download-and-unblock-the-wingtip-saas-scripts) auf den lokalen Computer herunter.

## <a name="application-architecture"></a>Anwendungsarchitektur

Die SaaS-App Wingtip verwendet das Modell mit einer Datenbank pro Mandant sowie elastische SQL-Pools, um die Effizienz zu maximieren. Für die Bereitstellung von Mandanten und deren Zuordnung mit den jeweiligen Daten wird eine Katalogdatenbank verwendet. Die SaaS-Kernanwendung Wingtip verwendet einen Pool mit drei Beispielmandanten sowie eine Katalogdatenbank. Um die zahlreichen Tutorials zur SaaS-Anwendung Wingtip zu absolvieren, sind bei der ersten Bereitstellung Add-ons erforderlich, wenn analytische Datenbanken, datenbankübergreifende Schemaverwaltung usw. eingeführt werden.


![Architektur der SaaS-Anwendung Wingtip](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Wenn Sie die Tutorials durchgehen und mit der App arbeiten, ist es wichtig, sich auf die SaaS-Muster zu konzentrieren, da sie im Zusammenhang mit der Datenebene stehen. Mit anderen Worten: Konzentrieren Sie sich auf die Datenebene, und nehmen Sie keine übermäßige Analyse der App selbst vor. Es ist von entscheidender Bedeutung, einen klaren Überblick über die Implementierung dieser SaaS-Muster zu besitzen, wenn Sie die Muster in Ihren Anwendungen implementieren und gleichzeitig nötige Modifizierungen für Ihre konkreten Geschäftsanforderungen berücksichtigen möchten.

## <a name="sql-database-wingtip-saas-tutorials"></a>Tutorials zur SaaS-App Wingtip für die SQL-Datenbank

Untersuchen Sie nach der Bereitstellung der App die folgenden Tutorials, die auf der erstmaligen Bereitstellung aufbauen. In diesen Tutorials werden allgemeine SaaS-Muster behandelt, die die integrierten Funktionen der SQL-Datenbank sowie der von SQL Data Warehouse und anderen Azure-Dienste nutzen. Die Tutorials enthalten PowerShell-Skripts sowie ausführliche Erläuterungen, die das Verständnis und das Implementieren derselben SaaS-Verwaltungsmustern in Ihren Anwendungen erheblich erleichtern.


| Tutorial | Beschreibung |
|:--|:--|
| [Anweisungen und Tipps zu einer mehrinstanzenfähigen SaaS-Beispiel-App in Azure SQL-Datenbank](saas-dbpertenant-wingtip-app-guidance-tips.md) | **BEGINNEN SIE HIER.** Laden Sie PowerShell-Skripts herunter, und führen Sie sie aus, um Teile der Anwendung vorzubereiten. |
|[Bereitstellen und Kennenlernen der SaaS-Anwendung Wingtip](saas-dbpertenant-get-started-deploy.md)|  Stellen Sie die SaaS-Anwendung Wingtip in Ihrem Azure-Abonnement bereit, und machen Sie sich mit ihr vertraut. |
|[Bereitstellen und Katalogisieren von Mandanten](saas-dbpertenant-provision-and-catalog.md)| Hier erfahren Sie, wie die Anwendung eine Verbindung mit einer Katalogdatenbank herstellt und wie den Mandanten im Katalog die entsprechenden Daten zugeordnet werden. |
|[Überwachen und Verwalten der Leistung](saas-dbpertenant-performance-monitoring.md)| Hier erfahren Sie, wie Überwachungsfunktionen der SQL-Datenbank verwendet und Benachrichtigungen festgelegt werden, wenn Leistungsschwellenwerte überschritten werden. |
|[Überwachen mit Log Analytics (OMS)](saas-dbpertenant-log-analytics.md) | Hier erfahren Sie, wie mithilfe von [Log Analytics](../log-analytics/log-analytics-overview.md) große Mengen von Ressourcen über mehrere Pools hinweg überwacht werden. |
|[Wiederherstellen einzelner Mandanten](saas-dbpertenant-restore-single-tenant.md)| Hier erfahren Sie, wie Sie eine Mandantendatenbank zu einem früheren Zeitpunkt wiederherstellen. Die Schritte zum Wiederherstellen einer parallelen Datenbank und die Onlineschaltung der vorhandenen Mandantendatenbank werden ebenfalls erläutert. |
|[Verwalten von Mandantenschemas](saas-tenancy-schema-management.md)| Hier erfahren Sie, wie Sie Schemas und Verweisdaten über alle Wingtip-SaaS-Mandanten hinweg aktualisieren. |
|[Ausführen von Ad-hoc-Analysen](saas-tenancy-adhoc-analytics.md) | Erstellen Sie eine Datenbank für Ad-hoc-Analysen, und führen Sie verteilte Echtzeitabfragen für alle Mandanten aus.  |
|[Ausführen von Mandantenanalysen](saas-tenancy-tenant-analytics.md) | Extrahieren Sie für die Offlineausführung von Analyseabfragen Mandantendaten in eine Analysedatenbank oder ein Data Warehouse. |


## <a name="next-steps"></a>Nächste Schritte

- [Anweisungen und Tipps zu einer mehrinstanzenfähigen SaaS-Beispiel-App in Azure SQL-Datenbank](saas-dbpertenant-wingtip-app-guidance-tips.md)

- [Bereitstellen der SaaS-Anwendung Wingtip](saas-dbpertenant-get-started-deploy.md)
