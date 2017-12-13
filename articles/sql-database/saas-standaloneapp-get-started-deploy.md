---
title: "Tutorial für mehrinstanzenfähige SaaS-Anwendungen – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Stellen Sie eine eigenständige SaaS-Anwendung für einzelne Mandanten, die Azure SQL-Datenbank verwendet, bereit, und untersuchen Sie sie."
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
ms.date: 11/30/2017
ms.author: genemi
ms.openlocfilehash: d38cd108821bce05824732bbdbdd322ae8563bde
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Bereitstellen und Untersuchen einer eigenständigen SaaS-Anwendung für einzelne Mandanten, die Azure SQL-Datenbank verwendet

In diesem Tutorial stellen Sie die eigenständige SaaS-Anwendung Wingtip Tickets bereit und machen sich mit dieser vertraut. Die Anwendung ist dafür ausgelegt, Features von Azure SQL-Datenbank zu veranschaulichen, mit denen die Aktivierung von SaaS-Szenarien vereinfacht wird.

Beim eigenständigen Anwendungsmuster wird für jeden Mandanten eine Azure-Ressourcengruppe mit einer Anwendung für einen einzelnen Mandanten und einer Datenbank mit nur einem Mandanten bereitgestellt.  Für eine Lösung für mehrere Mandanten können mehrere Instanzen der Anwendung bereitgestellt werden.

In diesem Tutorial stellen Sie für mehrere Mandanten in Ihrem Azure-Abonnement Ressourcengruppen bereit.  Mit diesem Muster wird es ermöglicht, dass Ressourcengruppen im Azure-Abonnement eines Mandanten bereitgestellt werden. Azure verfügt über Partnerprogramme, mit denen diese Ressourcengruppen im Auftrag des Mandanten durch einen Dienstanbieter verwaltet werden können. Der Dienstanbieter ist ein Administrator im Abonnement des Mandanten.

Der Abschnitt zur Bereitstellung weiter unten enthält drei blaue Schaltflächen **Deploy to Azure** (In Azure bereitstellen). Mit jeder Schaltfläche wird eine andere Instanz der Anwendung bereitgestellt. Jede Instanz wurde für einen spezifischen Mandanten angepasst. Wenn Sie auf die einzelnen Schaltflächen klicken, wird die entsprechende Anwendung innerhalb von fünf Minuten vollständig bereitgestellt.  Die Apps werden im Azure-Abonnement bereitgestellt.  Sie haben Vollzugriff auf die einzelnen Anwendungskomponenten, um diese untersuchen und verwenden zu können.

Der Quellcode der Anwendung und die Verwaltungsskripts sind im GitHub-Repository [WingtipTicketsSaaS-StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) verfügbar.


In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
> * Bereitstellen der eigenständigen SaaS-Anwendung Wingtip Tickets
> * Abrufen des Quellcodes der Anwendung und der Verwaltungsskripts
> * Informationen zu Servern und Datenbanken, aus denen sich die App zusammensetzt

Es werden noch weitere Tutorials veröffentlicht. In diesen Tutorials können Sie verschiedene Verwaltungsszenarien basierend auf diesem Anwendungsmuster erkunden.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Bereitstellen der eigenständigen SaaS-Anwendung Wingtip Tickets

Stellen Sie die App für die drei bereitgestellten Mandanten bereit:

1. Klicken Sie jeweils auf die blaue Schaltfläche **Deploy to Azure** (In Azure bereitstellen), um die Bereitstellungsvorlage im [Azure-Portal](https://portal.azure.com) zu öffnen. Für jede Vorlage sind zwei Parameterwerte erforderlich: ein Name für eine neue Ressourcengruppe und ein Benutzername, mit dem diese Bereitstellung von anderen Bereitstellungen der App unterschieden wird. Im nächsten Schritt sind Details zum Festlegen dieser Werte angegeben.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Contoso Concert Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Dogwood Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Fabrikam Jazz Club**

2. Geben Sie die erforderlichen Parameterwerte für jede Bereitstellung ein.

    > [!IMPORTANT]
    > Der Schutz einiger Authentifizierungs- und Serverfirewalls wurde zu Vorführungszwecken absichtlich aufgehoben. **Erstellen Sie eine neue Ressourcengruppe** für jede Anwendungsbereitstellung.  Verwenden Sie keine vorhandene Ressourcengruppe. Verwenden Sie diese Anwendung und alle damit erstellten Ressourcen nicht für die Produktion. Wenn Sie sich umfassend mit den Anwendungen vertraut gemacht haben, löschen Sie alle Ressourcengruppen, um die zugehörige Abrechnung einzustellen.

    Es wird empfohlen, bei Ressourcennamen nur Kleinbuchstaben, Zahlen und Bindestriche zu verwenden.
    * **Ressourcengruppe:** Wählen Sie **Neu erstellen** aus, und geben Sie für die Ressourcengruppe einen **Namen** in Kleinbuchstaben an.
        * Es wird empfohlen, Sie einen Bindestrich gefolgt von Ihren Initialen und einer Ziffer anzufügen, z.B. *wingtip-sa-af1*.
        * Wählen Sie einen **Speicherort** in der Dropdownliste aus.

    * **Benutzer:** Es wird empfohlen, hierfür einen kurzen Wert auszuwählen, z.B. Ihre Initialen und eine Ziffer (Beispiel: *af1*).


3. **Bereitstellen der Anwendung**.

    * Klicken Sie auf die entsprechende Option, um den Geschäftsbedingungen zuzustimmen.
    * Klicken Sie auf **Kaufen**.

4. Überwachen Sie den Bereitstellungsstatus aller drei Bereitstellungen, indem Sie auf **Benachrichtigungen** klicken (das Glockensymbol rechts neben dem Suchfeld). Das Bereitstellen der App dauert fünf Minuten.


## <a name="run-the-application"></a>Ausführen der Anwendung

In der App werden Veranstaltungsorte vorgestellt. Beispiele hierfür sind Konzerthallen, Jazz-Clubs und Sportclubs. Die Veranstaltungsorte sind die Kunden der Wingtip Tickets-App. In Wingtip Tickets werden Veranstaltungsorte als *Mandanten* registriert. Als Mandant kann ein Veranstaltungsort auf einfache Weise eine Liste mit Veranstaltungen bereitstellen und Tickets an die Kunden verkaufen. Jeder Veranstaltungsort erhält eine personalisierte Website, über die Veranstaltungen aufgeführt und Tickets verkauft werden können. Jeder Mandant ist von anderen Mandanten isoliert und unabhängig. Im Hintergrund erhält jeder Mandant eine separate Anwendungsinstanz mit dazugehöriger eigenständiger SQL-Datenbank.

1. Öffnen Sie die Ereignisseite für jeden der drei Mandanten in eigenen Browserregisterkarten:

    - http://events.contosoconcerthall.&lt;BENUTZER&gt;.trafficmanager.net
    - http://events.dogwooddojo.&lt;BENUTZER&gt;.trafficmanager.net
    - http://events.fabrikamjazzclub.&lt;BENUTZER&gt;.trafficmanager.net

    (Ersetzen Sie &lt;BENUTZER&gt; für jede URL durch den Benutzerwert Ihrer Bereitstellung.)

   ![Ereignisse](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Zum Steuern der Verteilung eingehender Anforderungen nutzt die App [*Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Jede mandantenspezifische App-Instanz enthält den Namen des Mandanten als Teil des Domänennamens in der URL. Alle Mandanten-URLs enthalten Ihren spezifischen Wert für **Benutzer**. Die URLs haben das folgende Format:
- http://events.&lt;Name_des_Veranstaltungsorts&gt;.&lt;BENUTZER&gt;.trafficmanager.net

Der **Speicherort** der Datenbank jedes Mandanten ist in den App-Einstellungen der zugehörigen bereitgestellten App enthalten.

In einer Produktionsumgebung erstellen Sie normalerweise einen CNAME-DNS-Eintrag zum [*Zuordnen einer Unternehmensinternetdomäne*](../traffic-manager/traffic-manager-point-internet-domain.md) zur URL des Traffic Manager-Profils.


## <a name="explore-the-servers-and-tenant-databases"></a>Untersuchen der Server und Mandantendatenbanken

Betrachten wir einige der Ressourcen, die bereitgestellt wurden:

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zur Liste der Ressourcengruppen.
2. Sehen Sie sich die Ressourcengruppe **wingtip-sa-catalog-&lt;BENUTZER&gt;** an.
    - In dieser Ressourcengruppe wird der Server **catalog-sa-&lt;BENUTZER&gt;** bereitgestellt. Der Server enthält die Datenbank **tenantcatalog**.
    - Außerdem sollten die drei Mandantenressourcengruppen angezeigt werden.
3. Öffnen Sie die Ressourcengruppe **wingtip-sa-fabrikam-&lt;BENUTZER&gt;** mit den Ressourcen für die Bereitstellung des Fabrikam Jazz Club.  Der Server **fabrikamjazzclub-&lt;BENUTZER&gt;** enthält die Datenbank **fabrikamjazzclub**.

Jede Mandantendatenbank ist eine *eigenständige* Datenbank mit 50 DTUs.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

<!--
* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Weitere Informationen zu mehrinstanzenfähigen SaaS-Anwendungen finden Sie unter [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen](saas-tenancy-app-design-patterns.md).


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen der eigenständigen SaaS-Anwendung Wingtip Tickets
> * Informationen zu Servern und Datenbanken, aus denen sich die App zusammensetzt
> * Löschen von Beispielressourcen, um die zugehörige Abrechnung einzustellen

