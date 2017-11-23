---
title: "Tutorial für mehrinstanzenfähige SaaS-Anwendungen – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Stellen Sie eine SaaS-Anwendung für einzelne Mandanten, die Azure SQL-Datenbank verwendet, bereit, und untersuchen Sie sie."
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
ms.date: 11/14/2017
ms.author: sstein
ms.openlocfilehash: ce0268f800dcf1900730d6ad9c476fb06320a79e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Bereitstellen und Untersuchen einer SaaS-Anwendung für einzelne Mandanten, die Azure SQL-Datenbank verwendet

In diesem Tutorial stellen Sie die Anwendung Wingtip Tickets SaaS Standlone bereit und machen sich mit dieser vertraut. Die Anwendung ist dafür ausgelegt, Features von Azure SQL-Datenbank zu veranschaulichen, mit denen die Aktivierung von SaaS-Szenarien vereinfacht wird.

Beim eigenständigen Anwendungsmuster wird für jeden Mandanten eine Azure-Ressourcengruppe mit einer Anwendung für einen einzelnen Mandanten und einer Datenbank mit nur einem Mandanten bereitgestellt.  Für eine Lösung für mehrere Mandanten können mehrere Instanzen der Anwendung bereitgestellt werden.

Auch wenn Sie in diesem Tutorial Ressourcengruppen für mehrere Mandanten in Ihrem Azure-Abonnement bereitstellen, können die Ressourcengruppen mit diesem Muster auch im Azure-Abonnement eines Mandanten bereitgestellt werden.  Azure bietet Partnerprogramme, mit denen diese Ressourcengruppen durch einen Dienstanbieter im Auftrag des Mandanten als Administrator im Abonnement des Mandanten verwaltet werden können.

Im Abschnitt zur Bereitstellung unten finden Sie drei Schaltflächen für die Bereitstellung in Azure, die jeweils eine andere Instanz der Anwendung angepasst an einen bestimmten Mandanten bereitstellen. Jede Schaltfläche stellt die zugehörige Anwendung innerhalb von fünf Minuten vollständig bereit.  Die Apps werden im Azure-Abonnement bereitgestellt.  Sie haben Vollzugriff auf die einzelnen Anwendungskomponenten, um diese untersuchen und verwenden zu können.

Der Quellcode der Anwendung und die Verwaltungsskripts sind im GitHub-Repository [WingtipTicketsSaaS-StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) verfügbar.


In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]

> * Bereitstellen der Anwendung Wingtip Tickets SaaS Standalone
> * Abrufen des Quellcodes der Anwendung und der Verwaltungsskripts
> * Informationen zu Servern und Datenbanken, aus denen sich die App zusammensetzt

Weitere Tutorials, mit denen Sie verschiedene Verwaltungsszenarien basierend auf diesem Anwendungsmuster untersuchen können, werden demnächst veröffentlicht.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Bereitstellen der Anwendung Wingtip Tickets SaaS Standalone

Stellen Sie die App für die drei bereitgestellten Mandanten bereit:

1. Klicken Sie auf jede Schaltfläche **Bereitstellung in Azure**, um die Bereitstellungsvorlage im Azure-Portal zu öffnen. Für jede Vorlage sind zwei Parameterwerte erforderlich: ein Name für eine neue Ressourcengruppe und ein Benutzername, mit dem diese Bereitstellung von anderen Bereitstellungen der App unterschieden wird. Im nächsten Schritt sind Details zum Festlegen dieser Werte angegeben.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>     &nbsp;&nbsp;**Contoso Concert Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp;&nbsp;**Dogwood Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>    &nbsp;&nbsp;**Fabrikam Jazz Club**

2. Geben Sie die erforderlichen Parameterwerte für jede Bereitstellung ein.

    > [!IMPORTANT]
    > Die Sicherheit einiger Authentifizierungs- und Serverfirewalls wurde zu Vorführungszwecken absichtlich aufgehoben. **Erstellen Sie eine neue Ressourcengruppe** für jede Anwendungsbereitstellung.  Verwenden Sie keine vorhandene Ressourcengruppe. Verwenden Sie diese Anwendung und alle damit erstellten Ressourcen nicht für die Produktion. Wenn Sie sich umfassend mit den Anwendungen vertraut gemacht haben, löschen Sie alle Ressourcengruppen, um die zugehörige Abrechnung einzustellen.

    Es wird empfohlen, bei Ressourcennamen nur Kleinbuchstaben, Zahlen und Bindestriche zu verwenden.
    * **Ressourcengruppe:** Wählen Sie „Neu erstellen“ aus, und geben Sie einen Namen für die Ressourcengruppe an (Groß-/Kleinschreibung wird berücksichtigt).
        * Es wird empfohlen, alle Buchstaben in Ihrem Ressourcengruppennamen klein zu schreiben.
        * Es wird empfohlen, Sie einen Bindestrich gefolgt von Ihren Initialen und einer Ziffer anzufügen, z.B. _wingtip-sa-af1_.
        * Wählen Sie in der Dropdownliste einen Speicherort aus.

    * **Benutzer:** Es wird empfohlen, einen kurzen Wert wie Ihre Initialen und eine Ziffer auszuwählen, z.B. _af1_.


1. **Bereitstellen der Anwendung**.

    * Klicken Sie auf die entsprechende Option, um den Geschäftsbedingungen zuzustimmen.
    * Klicken Sie auf **Kaufen**.

1. Überwachen Sie den Bereitstellungsstatus aller drei Bereitstellungen, indem Sie auf **Benachrichtigungen** klicken (das Glockensymbol rechts neben dem Suchfeld). Das Bereitstellen der App dauert ungefähr fünf Minuten.


## <a name="run-the-application"></a>Ausführen der Anwendung

In der App werden Veranstaltungsorte wie Konzerthallen, Jazzclubs, Sportstadien usw. vorgestellt, in denen Veranstaltungen stattfinden. Die Veranstalter registrieren sich als Kunden (oder Mandanten) von Wingtip Tickets und verfügen damit über eine einfache Möglichkeit, Veranstaltungen anzukündigen und Tickets zu verkaufen. Jeder Veranstalter erhält eine personalisierte Website zum Verwalten und Auflisten seiner Veranstaltungen und zum Verkaufen von Tickets. Diese ist unabhängig und abgegrenzt von denen anderer Mandanten. Im Hintergrund erhält jeder Mandant eine separate Anwendungsinstanz und eine eigenständige SQL-Datenbank.

1. Öffnen Sie die Ereignisseite für jeden der drei Mandanten in eigenen Browserregisterkarten:

    http://events.contosoconcerthall.&lt;BENUTZER&gt;.trafficmanager.net <br>
    http://events.dogwooddojo.&lt;BENUTZER&gt;.trafficmanager.net<br>
    http://events.fabrikamjazzclub.&lt;BENUTZER&gt;.trafficmanager.net

    (Ersetzen Sie &lt;BENUTZER&gt; durch den Benutzerwert Ihrer Bereitstellung.)

   ![Ereignisse](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)


Zum Steuern der Verteilung eingehender Anforderungen nutzt die App [*Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). Jede mandantenspezifische App enthält den Namen des Mandanten als Teil des Domänennamens in der URL. Alle Mandanten-URLs enthalten Ihren speziellen Wert für *Benutzer* und weisen das folgende Format auf: http://events.&lt;Veranstaltungsort&gt;.&lt;BENUTZER&gt;.trafficmanager.net. Der Datenbankspeicherort jedes Mandanten ist in den App-Einstellungen der zugehörigen bereitgestellten App enthalten.

In einer Produktionsumgebung würde typischerweise ein CNAME-DNS-Eintrag zum [*Zuordnen einer Unternehmensinternetdomäne*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) zur URL des Traffic Manager-Profils erstellt werden.


## <a name="explore-the-servers-and-tenant-databases"></a>Untersuchen der Server und Mandantendatenbanken

Betrachten wir einige der Ressourcen, die bereitgestellt wurden:

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zur Liste der Ressourcengruppen.  Die Ressourcengruppe **wingtip-sa-catalog-&lt;BENUTZER&gt;** sollte angezeigt werden. In dieser wird der Server **catalog-sa-&lt;BENUTZER&gt;** mit der Datenbank **tenantcatalog** bereitgestellt. Außerdem sollten die drei Mandantenressourcengruppen angezeigt werden.

1. Öffnen Sie die Ressourcengruppe **wingtip-sa-fabrikam-&lt;BENUTZER&gt;** mit den Ressourcen für die Bereitstellung des Fabrikam Jazz Club.  Der Server **fabrikamjazzclub-&lt;BENUTZER&gt;** enthält die Datenbank **fabrikamjazzclub**.


Jede Mandantendatenbank ist eine _eigenständige_ Datenbank mit 50 DTUs.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Bereitstellen der Anwendung Wingtip Tickets SaaS Standalone
> * Informationen zu Servern und Datenbanken, aus denen sich die App zusammensetzt
> * Löschen von Beispielressourcen, um die zugehörige Abrechnung einzustellen


## <a name="additional-resources"></a>Zusätzliche Ressourcen

<!--* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview) -->
* Weitere Informationen zu mehrinstanzenfähigen SaaS-Anwendungen finden Sie unter [*Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen*](saas-tenancy-app-design-patterns.md)
