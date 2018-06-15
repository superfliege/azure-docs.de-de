---
title: Verwenden von Log Analytics mit einer mehrinstanzenfähigen App für SQL-Datenbank | Microsoft-Dokumentation
description: Einrichten und Verwenden von Log Analytics mit einer mehrinstanzenfähigen SaaS-App für Azure SQL-Datenbank
keywords: Tutorial zur SQL-Datenbank
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 38adf3dd2be0770dd815644ece452a82bc98baf9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34645316"
---
# <a name="set-up-and-use-log-analytics-with-a-multitenant-sql-database-saas-app"></a>Einrichten und Verwenden von Log Analytics mit einer mehrinstanzenfähigen SaaS-App für SQL-Datenbank

In diesem Tutorial richten Sie [Log Analytics](/azure/log-analytics/log-analytics-overview) zum Überwachen von Pools für elastische Datenbanken sowie Datenbanken ein. Dieses Tutorial baut auf dem [Tutorial zum Überwachen und Verwalten der Leistung](saas-dbpertenant-performance-monitoring.md) auf. Es zeigt, wie Sie mit Log Analytics die im Azure-Portal bereitgestellte Überwachungs- und Warnungsfunktionalität erweitern können. Log Analytics unterstützt die Überwachung von zigtausend elastischen Pools und mehreren hunderttausend Datenbanken. Mit Log Analytics verfügen Sie über eine zentrale Überwachungslösung, in der die Überwachung von unterschiedlichen Anwendungen und Azure-Diensten für mehrere Azure-Abonnements integriert werden kann.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Installieren und Konfigurieren von Log Analytics
> * Überwachen von Pools und Datenbanken mithilfe von Log Analytics

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Wingtip Tickets-SaaS-App mit einer Datenbank pro Mandant ist bereitgestellt. Unter [Bereitstellen und Kennenlernen der Wingtip Tickets-SaaS-App mit einer Datenbank pro Mandant](saas-dbpertenant-get-started-deploy.md) finden Sie Informationen zur Bereitstellung in weniger als fünf Minuten.
* Azure PowerShell wurde installiert. Weitere Informationen finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Im [Tutorial zum Überwachen und Verwalten der Leistung](saas-dbpertenant-performance-monitoring.md) finden Sie eine Beschreibung der SaaS-Szenarien und -Muster, und es wird erläutert, wie sie die Anforderungen an eine Überwachungslösung beeinflussen.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-log-analytics"></a>Überwachen und Verwalten der Leistung von Datenbanken und Pools für elastische Datenbanken mit Log Analytics

Bei Azure SQL-Datenbank stehen Überwachungs- und Warnungsfunktionen für Datenbanken und Pools im Azure-Portal zur Verfügung. Diese integrierten Überwachungs- und Warnungsfunktionen sind praktisch, aber auch ressourcenspezifisch. Sie eigenen sich daher weniger, um große Installationen zu überwachen oder eine zentrale Übersicht über Ressourcen und Abonnements zu erhalten.

Bei Szenarien mit hohem Volumen kann Log Analytics zur Überwachung und für Warnungen eingesetzt werden. Log Analytics ist ein separater Azure-Dienst, der die Analyse von Diagnoseprotokollen und Telemetriedaten ermöglicht, die in einem Arbeitsbereich von möglicherweise vielen Diensten gesammelt werden. Mit der integrierten Abfragesprache und den Tools zur Datenvisualisierung ermöglicht Log Analytics die Analyse operativer Daten. SQL-Analyse bietet mehrere vordefinierte Überwachungs- und Warnungsansichten und -abfragen für Datenbanken und für Pools für elastische Datenbanken. Log Analytics bietet außerdem einen Designer für benutzerdefinierte Ansichten.

Sowohl Log Analytics-Arbeitsbereiche als auch Analyselösungen können im Azure-Portal und in Operations Management Suite geöffnet werden. Das Azure-Portal stellt den neueren Zugangspunkt dar, bleibt jedoch möglicherweise in einigen Bereichen hinter dem Portal von Operations Management Suite zurück.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Erstellen von Leistungsdiagnosedaten durch Simulieren einer Arbeitsauslastung für Ihre Mandanten 

1. Öffnen Sie in der PowerShell ISE „*..\\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\Performance Monitoring and Management\\Demo-PerformanceMonitoringAndManagement.ps1*“. Lassen Sie dieses Skript geöffnet, da Sie während dieses Szenarios u.U. mehrere Lastgenerierungsszenarien ausführen.
2. Falls noch nicht geschehen, stellen Sie einen Batch von Mandanten bereit, um einen interessanteren Überwachungskontext herzustellen. Dieser Vorgang dauert einige Minuten.

   a. Legen Sie **$DemoScenario = 1** fest – _Bereitstellen eines Batchs von Mandanten_

   b. Um das Skript ausführen und weitere 17 Mandanten bereitzustellen, drücken Sie F5.

3. Starten Sie jetzt den Lastgenerator, um eine simulierte Auslastung für alle Mandanten auszuführen.

    a. Legen Sie **$DemoScenario = 2** fest, _Generieren einer Last mit normaler Intensität (ca. 30 DTUs)_.

    b. Drücken Sie F5, um das Skript auszuführen.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Abrufen der Skripts zur SaaS-Anwendung Wingtip Tickets mit einer Datenbank pro Mandant

Die Skripts und der Anwendungsquellcode der mehrinstanzenfähigen Wingtip Tickets-SaaS-Datenbank stehen im GitHub-Repository [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) zur Verfügung. Schritte zum Herunterladen und Entsperren der Wingtip Tickets-PowerShell-Skripts finden Sie unter [Allgemeine Hinweise](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="install-and-configure-log-analytics-and-the-azure-sql-analytics-solution"></a>Installieren und Konfigurieren von Log Analytics und Azure SQL-Analyse

Log Analytics ist ein separater Dienst, der konfiguriert werden muss. Log Analytics erfasst Protokoll- und Telemetriedaten sowie Metriken in einem Log Analytics-Arbeitsbereich. Ein Log Analytics-Arbeitsbereich muss wie andere Ressourcen in Azure erstellt werden. Der Arbeitsbereich muss nicht in derselben Ressourcengruppe erstellt werden, in der sich auch die überwachten Anwendungen befinden. In den meisten Fällen ist das jedoch am sinnvollsten. Bei der Wingtip Tickets-App wird durch das Verwenden einer einzigen Ressourcengruppe sichergestellt, dass der Arbeitsbereich mit der Anwendung gelöscht wird.

1. Öffnen Sie in der PowerShell ISE *..\\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\Demo-LogAnalytics.ps1*.
2. Drücken Sie F5, um das Skript auszuführen.

Jetzt können Sie Log Analytics im Azure-Portal oder im Portal der Operations Management Suite öffnen. Es dauert einige Minuten, bis Telemetriedaten im Log Analytics-Arbeitsbereich erfasst und angezeigt werden. Je länger Sie dem System Zeit zum Sammeln von Diagnosedaten lassen, desto interessanter wird das Ergebnis. 

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Überwachen von Pools und Datenbanken mithilfe von Log Analytics und der SQL-Analyselösung


Öffnen Sie in dieser Übung Log Analytics und das Operations Management Suite-Portal, um die Telemetriedaten zu untersuchen, die für die Datenbanken und Pools gesammelt werden.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Wählen Sie **Alle Dienste** aus, um Log Analytics zu öffnen. Suchen Sie dann nach „Log Analytics“.

   ![Öffnen von Log Analytics](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

2. Wählen Sie den Arbeitsbereich namens _wtploganalytics-&lt;Benutzer&gt;_ aus.

3. Wählen Sie **Übersicht** aus, um die Log Analytics-Lösung im Azure-Portal zu öffnen.

   ![Übersicht](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Es kann einige Minuten dauern, bis die Lösung aktiv ist. 

4. Klicken Sie auf die Kachel **Azure SQL-Analyse**, um sie zu öffnen.

    ![Übersichtskachel](media/saas-dbpertenant-log-analytics/overview.png)

5. Die Ansichten in der Lösung werden mithilfe einer eigenen Bildlaufleiste am unteren Rand seitwärts gescrollt. Aktualisieren Sie die Seite bei Bedarf.

6. Zum Erkunden der Übersichtsseite wählen Sie die Kacheln oder die einzelnen Datenbanken aus, um einen Drilldown-Explorer zu öffnen.

    ![Log Analytics-Dashboard](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

7. Ändern Sie die Filtereinstellung, um den Zeitbereich zu ändern. In diesem Tutorial wählen Sie **Letzte Stunde** aus.

    ![Zeitfilter](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

8. Wählen Sie eine einzelne Datenbank aus, um die Verwendung von Abfragen und die Metriken für diese Datenbank zu erkunden.

    ![Datenbankanalyse](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

9. Scrollen Sie zur Anzeige der Nutzungsmetriken auf der Analyseseite nach rechts.
 
     ![Datenbankmetriken](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

10. Scrollen Sie auf der Analyseseite nach links, und wählen Sie in der Liste **Ressourceninfo** die Kachel „Server“ aus.  

    ![Liste „Ressourceninfo“](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    Dadurch wird eine Seite mit den Pools und Datenbanken auf dem Server geöffnet.

    ![Server mit Pools und Datenbanken](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

11. Wählen Sie einen Pool aus. Scrollen Sie auf der daraufhin geöffneten Poolseite nach rechts, um die Metriken des Pools anzuzeigen. 

    ![Metriken des Pools](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


12. Wechseln Sie zurück zum Log Analytics-Arbeitsbereich, und wählen Sie **OMS-Portal** aus, um den Arbeitsbereich dort zu öffnen.

    ![Kachel des Operations Management Suite-Portals](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

Im Operations Management Suite-Portal können Sie die Protokoll- und Metrikdaten im Arbeitsbereich weiter untersuchen. 

Überwachung und Warnung in Log Analytics beruhen – im Gegensatz zu den Warnungen, die im Azure-Portal für jede Ressource definiert werden – auf Abfragen der Daten im Arbeitsbereich. Da Warnungen auf Abfragen beruhen, können Sie statt einer Warnung pro Datenbank eine einzige Warnung definieren, die alle Datenbanken abdeckt. Abfragen sind nur durch die im Arbeitsbereich verfügbaren Daten beschränkt.

Weitere Informationen zum Abfragen und Festlegen von Warnungen mit Log Analytics finden Sie unter [Arbeiten mit Warnungsregeln in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Die Rechnungsstellung für Log Analytics für SQL-Datenbank basiert auf dem jeweiligen Datenvolumen im Arbeitsbereich. In diesem Tutorial haben Sie einen kostenlosen Arbeitsbereich erstellt, der auf 500 MB pro Tag beschränkt ist. Sobald dieser Grenzwert erreicht wird, werden dem Arbeitsbereich keine Daten mehr hinzugefügt.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Installieren und Konfigurieren von Log Analytics
> * Überwachen von Pools und Datenbanken mithilfe von Log Analytics

Absolvieren Sie das [Tutorial zu Mandantenanalysen](saas-dbpertenant-log-analytics.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Weitere Tutorials, die auf der anfänglichen Anwendungsbereitstellung der Wingtip Tickets-SaaS-App mit einer Datenbank pro Mandant aufbauen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
