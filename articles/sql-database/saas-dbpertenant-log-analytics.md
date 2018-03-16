---
title: "Verwenden von Log Analytics mit einer mehrinstanzenfähigen App für SQL-Datenbank | Microsoft-Dokumentation"
description: "Einrichten und Verwenden von Log Analytics (OMS) mit einer mehrinstanzenfähigen SaaS-App für Azure SQL-Datenbank"
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: billgib; sstein
ms.openlocfilehash: 7747092d5613a40fa0aff09cfbdfb9b786b37954
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="set-up-and-use-log-analytics-oms-with-a-multi-tenant-azure-sql-database-saas-app"></a>Einrichten und Verwenden von Log Analytics (OMS) mit einer mehrinstanzenfähigen SaaS-App für Azure SQL-Datenbank

In diesem Tutorial richten Sie *Log Analytics([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* zum Überwachen von Pools für elastische Datenbanken sowie Datenbanken ein. Dieses Tutorial baut auf dem Tutorial [Überwachen der Leistung der SaaS-Anwendung Wingtip](saas-dbpertenant-performance-monitoring.md) auf. Es zeigt, wie Sie mit *Log Analytics* die im Azure-Portal bereitgestellte Überwachungs- und Warnungsfunktionalität erweitern können. Log Analytics unterstützt die Überwachung von zigtausend elastischen Pools und mehreren hunderttausend Datenbanken. Mit Log Analytics verfügen Sie über eine einzige Überwachungslösung, in der die Überwachung von unterschiedlichen Anwendungen und Azure-Diensten für mehrere Azure-Abonnements integriert werden kann.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Installieren und Konfigurieren von Log Analytics (OMS)
> * Überwachen von Pools und Datenbanken mithilfe von Log Analytics

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die SaaS-App Wingtip Tickets mit einer Datenbank pro Mandant ist bereitgestellt. Unter [Bereitstellen und Untersuchen der SaaS-App Wingtip Tickets mit einer Datenbank pro Mandant](saas-dbpertenant-get-started-deploy.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Im [Tutorial zum Überwachen und Verwalten der Leistung](saas-dbpertenant-performance-monitoring.md) finden Sie eine Beschreibung der SaaS-Szenarien und -Muster, und es wird erläutert, wie sie die Anforderungen an eine Überwachungslösung beeinflussen.

## <a name="monitoring-and-managing-database-and-elastic-pool-performance-with-log-analytics-or-operations-management-suite-oms"></a>Überwachen und Verwalten der Leistung von Datenbanken und elastischen Pools mit Log Analytics oder Operations Management Suite (OMS)

Bei SQL-Datenbank sind Überwachung und Warnung für Datenbanken und Pools im Azure-Portal verfügbar. Diese integrierte Überwachung und Warnung ist bequem, aber ressourcenspezifisch und eignet sich daher weniger für die Überwachung großer Installationen oder für die Bereitstellung einer einheitlichen, ressourcen- und abonnementübergreifenden Ansicht.

Bei Szenarien mit hohem Volumen kann Log Analytics zur Überwachung und Warnung eingesetzt werden. Log Analytics ist ein separater Azure-Dienst, der die Analyse von Diagnoseprotokollen und Telemetriedaten ermöglicht, die in einem Arbeitsbereich von möglicherweise vielen Diensten gesammelt werden. Mit der integrierten Abfragesprache und den Tools zur Datenvisualisierung ermöglicht Log Analytics die Analyse operativer Daten. Die SQL-Analyselösung bietet mehrere vordefinierte Überwachungs- und Warnungsansichten und -abfragen für elastische Pools und Datenbanken. OMS bietet außerdem einen Designer für benutzerdefinierte Ansichten.

Sowohl Log Analytics-Arbeitsbereiche als auch Analyselösungen können im Azure-Portal und in OMS geöffnet werden. Das Azure-Portal stellt den neueren Zugangspunkt dar, bleibt jedoch möglicherweise in einigen Bereichen hinter dem OMS-Portal zurück.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Erstellen von Leistungsdiagnosedaten durch Simulieren einer Arbeitsauslastung für Ihre Mandanten 

1. Öffnen Sie in der **PowerShell ISE** *..\\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\Performance Monitoring and Management\\die Datei **Demo-PerformanceMonitoringAndManagement.ps1***. Lassen Sie dieses Skript geöffnet, da Sie während dieses Szenarios u.U. mehrere Lastgenerierungsszenarien ausführen.
1. Falls noch nicht geschehen, stellen Sie einen Batch von Mandanten bereit, um einen interessanteren Überwachungskontext herzustellen. Dieser Vorgang dauert einige Minuten:
   1. Legen Sie **$DemoScenario = 1** fest, _Bereitstellen eines Batchs von Mandanten_
   1. Um das Skript ausführen und weitere 17 Mandanten bereitzustellen, drücken Sie **F5**.  

1. Starten Sie jetzt den Lastgenerator, um eine simulierte Auslastung für alle Mandanten auszuführen.  
    1. Legen Sie **$DemoScenario = 2** fest, _Generieren einer Last mit normaler Intensität (ca. 30 DTUs)_.
    1. Betätigen Sie **F5**, um das Skript auszuführen.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Abrufen der Skripts zur SaaS-Anwendung Wingtip Tickets mit einer Datenbank pro Mandant

Die Skripts und der Anwendungsquellcode der mehrinstanzenfähigen Wingtip Tickets-SaaS-Datenbank stehen im GitHub-Repository [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) zur Verfügung. Schritte zum Herunterladen und Entsperren der Wingtip Tickets-PowerShell-Skripts finden Sie unter [Allgemeine Hinweise](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Installieren und Konfigurieren von Log Analytics und der Azure SQL-Analyselösung

Log Analytics ist ein separater Dienst, der konfiguriert werden muss. Log Analytics erfasst Protokolldaten sowie Telemetriedaten und Metriken in einem Log Analytics-Arbeitsbereich. Ein Log Analytics-Arbeitsbereich ist eine Ressource und muss wie andere Ressourcen in Azure erstellt werden. Es ist am sinnvollsten, den Arbeitsbereich in der Ressourcengruppe zu erstellen, in der sich auch die überwachte(n) Anwendung(en) befinden, auch wenn dies nicht unbedingt erforderlich ist. Bei der Wingtip Tickets-App wird durch das Verwenden einer einzigen Ressourcengruppe sichergestellt, dass der Arbeitsbereich mit der Anwendung gelöscht wird.

1. Öffnen Sie in der **PowerShell ISE** *..\\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\**Demo-LogAnalytics.ps1***.
1. Betätigen Sie **F5**, um das Skript auszuführen.

An dieser Stelle sollten Sie in der Lage sein, Log Analytics im Azure-Portal (oder im OMS-Portal) zu öffnen. Es dauert einige Minuten, bis Telemetriedaten im Log Analytics-Arbeitsbereich erfasst und angezeigt werden. Je länger Sie dem System Zeit zum Sammeln von Diagnosedaten lassen, desto interessanter wird das Ergebnis. Dies ist ein guter Zeitpunkt, eine Pause einzulegen – vergewissern Sie sich nur, dass der Last-Generator immer noch ausgeführt wird!

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Überwachen von Pools und Datenbanken mithilfe von Log Analytics und der SQL-Analyselösung


Öffnen Sie in dieser Übung Log Analytics und das OMS-Portal, um die Telemetriedaten zu untersuchen, die für die Datenbanken und -Pools gesammelt werden.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und öffnen Sie Log Analytics, indem Sie auf **Alle Dienste** klicken und dann nach Log Analytics suchen:

   ![Öffnen von Log Analytics](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Wählen Sie den Arbeitsbereich namens _wtploganalytics-&lt;Benutzer&gt;_ aus.

1. Wählen Sie **Übersicht** aus, um die Log Analytics-Lösung im Azure-Portal zu öffnen.

   ![Link „Übersicht“](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Es kann einige Minuten dauern, bis die Lösung aktiv ist. Haben Sie etwas Geduld!

1. Klicken Sie auf die Kachel „Azure SQL-Analyse“, um sie zu öffnen.

    ![Übersicht](media/saas-dbpertenant-log-analytics/overview.png)

    ![Analytics](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. Die Ansichten in der Lösung werden mithilfe einer eigenen Bildlaufleiste am unteren Rand seitwärts gescrollt (bei Bedarf muss die Seite aktualisiert werden).

1. Erkunden Sie die Übersichtsseite, indem Sie auf die Kacheln oder auf eine einzelne Datenbank klicken, um einen Drilldown-Explorer zu öffnen.

1. Ändern Sie die Filtereinstellung zum Modifizieren des Zeitbereichs – wählen Sie für dieses Lernprogramm _Letzte Stunde_ aus.

    ![Zeitfilter](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Wählen Sie eine einzelne Datenbank aus, um die Verwendung von Abfragen und Metriken für diese Datenbank zu erkunden.

    ![Datenbankanalyse](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Scrollen Sie zur Anzeige der Nutzungsmetriken die Analyseseite nach rechts.
 
     ![Datenbankmetriken](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Scrollen Sie die Analyseseite nach links, und klicken Sie in der Liste „Ressourceninfo“ auf die Kachel "Server". Dadurch wird eine Seite mit den Pools und Datenbanken auf dem Server geöffnet. 

     ![Ressourceninfo](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

 
     ![Server mit Pools und Datenbanken](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Klicken Sie auf der geöffneten Serverseite, auf der die Pools und Datenbanken auf dem Server angezeigt werden, auf den Pool.  Scrollen Sie auf der daraufhin geöffneten Poolseite nach rechts, um die Metriken des Pools anzuzeigen.  

     ![Metriken des Pools](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)



1. Wechseln Sie zurück zum Log Analytics-Arbeitsbereich, und wählen Sie **OMS-Portal** aus, um den dortigen Arbeitsbereich zu öffnen.

    ![OMS](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

Im OMS-Portal können Sie die Protokoll- und Metrikdaten im Arbeitsbereich weiter untersuchen.  

Überwachung und Warnung in Log Analytics und OMS beruhen – im Gegensatz zu den Warnungen, die im Azure-Portal für jede Ressource definiert werden – auf Abfragen der Daten im Arbeitsbereich. Da Warnungen auf Abfragen beruhen, können Sie statt einer Warnung pro Datenbank eine einzige Warnung definieren, die alle Datenbanken abdeckt. Abfragen sind nur durch die im Arbeitsbereich verfügbaren Daten beschränkt.

Weitere Informationen zum Abfragen und Festlegen von Warnungen mit OMS finden Sie unter [Arbeiten mit Warnungsregeln in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Die Rechnungsstellung für Log Analytics für SQL-Datenbank basiert auf dem jeweiligen Datenvolumen im Arbeitsbereich. In diesem Tutorial haben Sie einen kostenlosen Arbeitsbereich erstellt, der auf 500 MB pro Tag beschränkt ist. Sobald dieser Grenzwert erreicht wird, werden dem Arbeitsbereich keine Daten mehr hinzugefügt.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Installieren und Konfigurieren von Log Analytics (OMS)
> * Überwachen von Pools und Datenbanken mithilfe von Log Analytics

[Tutorial zu Mandantenanalysen](saas-dbpertenant-log-analytics.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Weitere Tutorials, die auf der anfänglichen Anwendungsbereitstellung von Wingtip Tickets SaaS Database Per Tenant aufbauen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
