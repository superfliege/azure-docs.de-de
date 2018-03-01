---
title: "Bereitstellen neuer Mandanten in einer mehrinstanzenfähigen App, die die Azure SQL-Datenbank verwendet | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie neue Mandanten in einer mehrinstanzenfähigen SaaS-App in Azure SQL-Datenbank bereitstellen und katalogisieren können."
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
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: 79b3743054f73914c6755a3c9b102b613b1944f2
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Hier erfahren Sie, wie Sie neue Mandanten bereitstellen und sie im Katalog registrieren.

In diesem Tutorial erfahren Sie, wie Sie SaaS-Muster bereitstellen und katalogisieren und wie diese Muster in der Wingtip Tickets SaaS-Anwendung mit einer Datenbank pro Mandant implementiert werden. Sie erstellen und initialisieren neue Mandantendatenbanken und registrieren sie im Mandantenkatalog der Anwendung. Der Katalog ist eine Datenbank, in der die Zuordnung zwischen den zahlreichen Mandanten von SaaS-Anwendungen und den dazugehörigen Daten verwaltet wird. Der Katalog spielt eine wichtige Rolle, da er die Anwendungs- und Verwaltungsanforderungen an die richtigen Datenbanken weiterleitet.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]

> * Bereitstellen eines einzelnen neuen Mandanten
> * Bereitstellen eines Batches von zusätzlichen Mandanten


Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Wingtip Tickets SaaS-App mit einer Datenbank pro Mandant ist bereitgestellt. Unter [Bereitstellen und Kennenlernen der Wingtip Tickets SaaS-App mit einer Datenbank pro Mandant](saas-dbpertenant-get-started-deploy.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>Einführung in die SaaS-Katalogmuster

Für eine mehrinstanzenfähige SaaS-Anwendung mit Datenbankunterstützung ist es wichtig zu wissen, wo die Informationen über jeden einzelnen Mandanten gespeichert sind. Im SaaS-Katalogmuster wird eine Katalogdatenbank zum Speichern der Zuordnungen zwischen den einzelnen Mandanten und der Datenbank verwendet, in der die Daten gespeichert werden. Dieses Muster trifft immer dann zu, wenn Mandantendaten auf mehrere Datenbanken verteilt sind.

Jeder Mandant wird anhand eines Schlüssels im Katalog identifiziert, der dem Speicherort der jeweiligen Datenbank zugeordnet ist. In der Wingtip Tickets-App wird der Schlüssel aus einem Hash des Mandantennamens gebildet. Dadurch kann die App den Schlüssel aus dem in der Anwendungs-URL enthaltenen Mandantennamen bilden. Es können auch andere Mandantenschlüsselschemas verwendet werden.  

Dank des Katalogs kann der Name oder Speicherort der Datenbank ohne größere Auswirkungen auf die Anwendung geändert werden.  In einem Datenbankmodell mit mehreren Mandanten kann dadurch auch ein Mandant zwischen Datenbanken „verschoben“ werden.  Darüber hinaus kann der Katalog angeben, ob ein Mandant oder eine Datenbank offline ist (beispielsweise zur Wartung). Weitere Informationen hierzu finden Sie im Tutorial zum [Wiederherstellen eines einzelnen Mandanten](saas-dbpertenant-restore-single-tenant.md).

Im Katalog können auch zusätzliche Mandanten- oder Datenbankmetadaten gespeichert werden, z. B. Schemaversion, Dienstplan oder Mandanten angebotene SLAs, sowie andere Informationen, die eine Anwendungsverwaltung, Kundensupport oder DevOps ermöglichen.  

Abgesehen von der SaaS-Anwendung kann der Katalog auch die Verwendung von Datenbanktools ermöglichen.  Im Beispiel zu Wingtip Tickets SaaS mit einer Datenbank pro Mandant wird der Katalog verwendet, um mandantenübergreifende Abfragen zu ermöglichen (siehe [Tutorial zur Ad-hoc-Berichterstellung](saas-tenancy-cross-tenant-reporting.md)). Die datenbankübergreifende Auftragsverwaltung wird in den Tutorials [Verwalten des Schemas für mehrere Mandanten in der Wingtip-SaaS-Anwendung](saas-tenancy-schema-management.md) und [Extrahieren von Daten aus Mandantendatenbanken in eine Analysedatenbank für die Offlineanalyse](saas-tenancy-tenant-analytics.md) behandelt. 

In den Beispielen zu Wingtip Tickets SaaS mit einer Datenbank pro Mandant wird der Katalog mithilfe der Shardverwaltungsfeatures der [Clientbibliothek für elastische Datenbanken (Elastic Database Client Library, EDCL)](sql-database-elastic-database-client-library.md) implementiert. Die EDCL ist in Java und .Net Framework verfügbar. Die EDCL ermöglicht einer Anwendung die Erstellung, Verwaltung und Verwendung einer datenbankgestützten Shardzuordnung. Eine Shardzuordnung enthält eine Liste mit Shards (Datenbanken) und die Zuordnung zwischen Schlüsseln (Mandanten) und Shards.  EDCL-Funktionen werden im Rahmen der Mandantenbereitstellung verwendet, um Einträge in der Shardzuordnung zu erstellen, und dann zur Laufzeit von Anwendungen genutzt, um eine Verbindung mit der richtigen Datenbank herzustellen. EDCL speichert Verbindungsinformationen zwischen, um den Datenverkehr für die Katalogdatenbank zu minimieren und die Anwendung zu beschleunigen.  

> [!IMPORTANT]
> Sie können auf die Zuordnungsdaten in der Katalogdatenbank zugreifen, *allerdings sollten Sie sie nicht bearbeiten*. Bearbeiten Sie Zuordnungsdaten nur über EDCL-APIs. Ein direktes Bearbeiten der Zuordnungsdaten birgt das Risiko, den Katalog zu beschädigen, und wird daher nicht unterstützt.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Einführung in das SaaS-Bereitstellungsmuster

Beim Onboarding eines neuen Mandanten in einer SaaS-Anwendung, die ein Datenbankmodell mit einem einzelnen Mandanten verwendet, muss eine neue Mandantendatenbank bereitgestellt werden.  Die Datenbank muss am korrekten Ort und auf der korrekten Dienstebene erstellt, mit dem korrekten Schema und den korrekten Referenzdaten initialisiert und im Katalog unter dem korrekten Mandantenschlüssel registriert werden.  

Für die Datenbankbereitstellung können verschiedene Ansätze verwendet werden. Dazu gehört das Ausführen von SQL-Skripts, Bereitstellen einer BACPAC-Datei oder Kopieren einer Vorlagendatenbank.  

Die Datenbankbereitstellung muss Teil Ihrer Schemaverwaltungsstrategie sein, durch die sichergestellt sein muss, dass neue Datenbanken mit dem neuesten Schema bereitgestellt werden. Diese Anforderung wird im [Tutorial zur Schemaverwaltung](saas-tenancy-schema-management.md) behandelt.  

Die Wingtip Tickets-App mit einer Datenbank pro Mandant stellt neue Mandanten bereit, indem sie eine auf dem Katalogserver bereitgestellte Vorlagendatenbank namens _basetenantdb_ kopiert.  Die Bereitstellung kann im Rahmen der Registrierung in die Anwendung integriert und/oder offline mithilfe von Skripts unterstützt werden. In diesem Tutorial wird die Bereitstellung mithilfe von PowerShell durchgeführt. Bereitstellungsskripts kopieren die Datenbank _basetenantdb_, um eine neue Mandantendatenbank in einem Pool für elastische Datenbanken zu erstellen. Anschließend wird die Datenbank mit mandantenspezifischen Informationen initialisiert und in der Katalogshardzuordnung registriert.  Mandantendatenbanken erhalten ihre Namen auf Grundlage des Mandantennamens, doch ist dieses Benennungsschema kein wichtiger Bestandteil des Musters. Da der Katalog den Mandantenschlüssel dem Datenbanknamen zuordnet, kann jede Benennungskonvention verwendet werden. 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Abrufen der Skripts zur SaaS-Anwendung Wingtip Tickets mit einer Datenbank pro Mandant

Die Wingtip Tickets SaaS-Skripts und der Quellcode der Anwendung stehen im GitHub-Repository [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) zur Verfügung. Schritte zum Herunterladen und Entsperren der Wingtip Tickets-SaaS-Skripts finden Sie unter [General guidance for working with Wingtip Tickets sample SaaS apps](saas-tenancy-wingtip-app-guidance-tips.md) (Allgemeine Hinweise zur Verwendung von Wingtip Tickets-Beispiel-SaaS-Apps).


## <a name="provision-and-catalog-detailed-walkthrough"></a>Bereitstellen und Katalogisieren: detaillierte exemplarische Vorgehensweise

Um nachzuvollziehen, wie die Wingtip Tickets-Anwendung die Bereitstellung neuer Mandanten implementiert, fügen Sie einen Haltepunkt hinzu, und durchlaufen Sie während der Bereitstellung eines Mandanten den Workflow:

1. Öffnen Sie in _PowerShell ISE_ \\...Learning Modules\\ProvisionAndCatalog\\_Demo-ProvisionAndCatalog.ps1_, und legen Sie die folgenden Parameter fest:
   * **$TenantName** = Name des neuen Veranstaltungsorts (z.B., *Bushwillow Blues*).
   * **$VenueType** = einer der vordefinierten Veranstaltungsorttypen: _blues, classicalmusic, dance, jazz, judo, motor racing, multipurpose, opera, rockmusic, soccer_.
   * **$DemoScenario** = **1**, für das *Bereitstellen eines einzelnen Mandanten*.

1. Fügen Sie einen Haltepunkt hinzu, indem Sie den Cursor an eine beliebige Stelle in der Zeile mit *New-Tenant `* bewegen und **F9** drücken.

   ![Haltepunkt](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

1. Betätigen Sie **F5**, um das Skript auszuführen.

1. Nachdem die Skriptausführung am Haltepunkt beendet wurde, drücken Sie **F11**, um den Code schrittweise auszuführen.

   ![Debuggen](media/saas-dbpertenant-provision-and-catalog/debug.png)



Verfolgen Sie die Ausführung des Skripts mit den Optionen (**F10** und **F11**) im Menü **Debug** nach, um die aufgerufenen Funktionen zu überspringen oder einzeln auszuführen. Weitere Informationen zum Debuggen von PowerShell-Skripts finden Sie unter [Tipps zum Arbeiten mit und Debuggen von PowerShell-Skripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Die folgenden Schritte müssen nicht genau befolgt werden, sondern stellen eine Erläuterung des Workflows dar, den Sie beim Debuggen des Skripts schrittweise durchlaufen:

1. **Importieren Sie das Modul „SubscriptionManagement.psm1“**, das Funktionen enthält, um sich bei Azure anzumelden und das Azure-Abonnement auszuwählen, mit dem Sie arbeiten.
1. **Importieren Sie das Modul „CatalogAndDatabaseManagement.psm1“**, das eine Abstraktion auf Katalog- und Mandantenebene über die [Shardverwaltungsfunktionen](sql-database-elastic-scale-shard-map-management.md) bereitstellt. Dieses Modul kapselt einen Großteil des Katalogmusters, und es lohnt sich, dieses Modul genauer zu betrachten.
1. **Rufen Sie Konfigurationsinformationen ab**. Wechseln Sie zu „Get-Configuration“ (mit F11), und sehen Sie sich an, wie die App-Konfiguration angegeben ist. Hier sind Ressourcennamen und andere App-spezifische Werte definiert. Ändern Sie diese Werte nicht, es sei denn, sie kennen sich gut mit Skripts aus.
1. **Rufen Sie das Katalogobjekt ab**. Wechseln Sie zu „Get-Catalog“. Hierdurch wird ein im übergeordneten Skript verwendetes Katalogobjekt erstellt und zurückgegeben.  Diese Funktion verwendet aus **AzureShardManagement.psm1** importierte Shardverwaltungsfunktionen. Das Katalogobjekt besteht aus den folgenden Elementen:
   * „$catalogServerFullyQualifiedName“ wird aus dem Standardstamm plus Ihrem Benutzernamen gebildet: _catalog-\<Benutzer\>.database.windows.net_.
   * „$catalogDatabaseName“ wird aus der Konfigurationsdatei abgerufen: *tenantcatalog*.
   * Das Objekt „$shardMapManager“ wird aus der Katalogdatenbank initialisiert.
   * Das Objekt „$shardMap“ wird aus der Shardzuordnung _tenantcatalog_ in der Katalogdatenbank initialisiert.
   Ein Katalogobjekt wird gebildet und aus zurückgegeben und im Skript der höheren Ebene verwendet.
1. **Berechnen Sie den neuen Mandantenschlüssel**. Eine Hashfunktion wird verwendet, um den Mandantenschlüssel aus dem Mandantennamen zu erstellen.
1. **Überprüfen Sie, ob der Mandantenschlüssel bereits vorhanden ist**. Der Katalog wird überprüft, um sicherzustellen, dass der Schlüssel verfügbar ist.
1. **Die Mandantendatenbank wird mit „New-TenantDatabase“ bereitgestellt.** Drücken Sie **F11** , und sehen Sie sich an, wie die Datenbank mithilfe einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-template-walkthrough.md) bereitgestellt wird.

    Der Datenbankname wird aus dem Mandantennamen gebildet, um zu verdeutlichen, welcher Shard zu welchem Mandanten gehört, doch können auch andere Benennungskonventionen für Datenbanken verwendet werden.
    Mit einer Resource Manager-Vorlage wird eine Mandantendatenbank erstellt, indem eine Vorlagendatenbank (_baseTenantDB_) auf dem Katalogserver kopiert wird. Alternativ können Sie eine Datenbank erstellen und sie durch Importieren einer BACPAC-Datei initialisieren oder ein Initialisierungsskript von einem bekannten Speicherort ausführen.

    Die Resource Manager-Vorlage befindet sich im Ordner „……\Learning Modules\Common\“: *tenantdatabasecopytemplate.json*

1. Nach Erstellung der Mandantendatenbank wird sie **mit dem Namen des Veranstaltungsorts (Mandantenname) und dem Veranstaltungsorttyp initialisiert**. Hier können Sie auch weitere Initialisierungen vornehmen.

1. Die **Mandantendatenbank wird im Katalog registriert**. Hierzu wird *Add-TenantDatabaseToCatalog* mit dem Mandantenschlüssel verwendet. Drücken Sie **F11**, um sich die Schritte im Detail anzuschauen:

    * Die Katalogdatenbank wird der Shardzuordnung (Liste der bekannten Datenbanken) hinzugefügt.
    * Die Zuordnung, die den Schlüsselwert mit dem Shard verknüpft, wird erstellt.
    * Zusätzliche Metadaten für den Mandanten (Name des Veranstaltungsorts) werden der *Mandantentabelle* im Katalog hinzugefügt.  Die Mandantentabelle ist nicht Teil des Schemas „ShardManagement“ und wird nicht durch die EDCL installiert.  Diese Tabelle veranschaulicht, wie die Katalogdatenbank erweitert werden kann, um zusätzliche anwendungsspezifische Daten zu unterstützen.   


Nach Abschluss der Bereitstellung kehrt die Ausführung zum ursprünglichen Skript *Demo-ProvisionAndCatalog* zurück, das die Seite **Ereignisse** für den neuen Mandanten im Browser öffnet:

   ![events](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Bereitstellen eines Batches von Mandanten

In dieser Übung wird ein Batch mit 17 Mandanten bereitgestellt. Es empfiehlt sich, diesen Mandantenbatch bereitzustellen, bevor Sie mit anderen Tutorials zu Wingtip Tickets SaaS mit einer Datenbank pro Mandant beginnen, damit Ihnen mehrere Datenbanken zur Verfügung stehen.

1. Öffnen Sie in *PowerShell ISE* \\...Learning Modules\\ProvisionAndCatalog\\*Demo-ProvisionAndCatalog.ps1*, und ändern Sie den *$DemoScenario*-Parameter in „3“:
   * **$DemoScenario** = **3**, für *Bereitstellen eines Batches von Mandanten*.
1. Drücken Sie **F5** , um das Skript auszuführen.

Das Skript stellt einen Batch von zusätzlichen Mandanten bereit. Es verwendet eine [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-manager-template-walkthrough.md), die den Batch steuert und die Bereitstellung der einzelnen Datenbanken an eine verknüpfte Vorlage delegiert. Indem Sie Vorlagen auf diese Weise verwenden, können Sie mit Azure Resource Manager den Bereitstellungsprozess für das Skript vermitteln. Die Vorlagen stellen Datenbanken parallel bereit und verarbeiten nötigenfalls Wiederholungen. Das Skript ist idempotent. Wenn also ein Fehler auftritt oder das Skript aus irgendeinem Grund angehalten wird, führen Sie es erneut aus.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Überprüfen, ob der Mandantenbatch erfolgreich bereitgestellt wurde

* Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Serverliste, und öffnen Sie den Server *tenants1*. Klicken Sie auf **SQL-Datenbanken**, und überprüfen Sie, ob der Batch von 17 zusätzlichen Datenbanken jetzt in der Liste aufgeführt wird:

   ![Datenbankliste](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Weitere Bereitstellungsmuster

Weitere Bereitstellungsmuster, die in diesem Tutorial nicht behandelt werden:

**Vorabbereitstellung von Datenbanken.** Die Vorabbereitstellung profitiert von der Tatsache, dass Datenbanken in einem Pool für elastische Datenbanken keine zusätzlichen Kosten hervorrufen. Die Abrechnung erfolgt für den Pool für elastische Datenbanken und nicht für die Datenbanken, und Datenbanken im Leerlauf verbrauchen keine Ressourcen. Indem Sie Datenbanken vorab in einem Pool bereitstellen und später bei Bedarf zuordnen, können Sie das Onboarding von Mandanten erheblich beschleunigen. Die Anzahl der vorab bereitgestellten Datenbanken kann bedarfsorientiert angepasst werden, um einen für die erwartete Bereitstellungsrate geeigneten Puffer bereitzuhalten.

**Automatische Bereitstellung.** Beim Muster der automatischen Bereitstellung stellt ein Bereitstellungsdienst Server, Pools und Datenbanken nach Bedarf automatisch bereit – einschließlich vorab bereitgestellter Datenbanken in Pools für elastische Datenbanken, falls gewünscht. Wenn Datenbanken außer Betrieb genommen und gelöscht werden, können Lücken in den Pools für elastische Datenbanken durch den Bereitstellungsdienst gefüllt werden. Ein solcher Dienst kann einfach oder komplex sein. Die Bereitstellung kann also beispielsweise über mehrere geografische Regionen erfolgen, und die Georeplikation kann zur Notfallwiederherstellung eingerichtet werden. Mit dem Muster der automatischen Bereitstellung sendet eine Clientanwendung oder ein Skript eine Bereitstellungsanforderung an eine Warteschlange, damit diese vom Bereitstellungsdienst verarbeitet wird, und fragt dann vom Dienst den Abschluss des Vorgangs ab. Wurden Datenbanken vorab bereitgestellt, werden die Bereitstellungsanforderungen schnell verarbeitet, indem der Dienst eine Ersatzdatenbank im Hintergrund bereitstellt.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Bereitstellen eines einzelnen neuen Mandanten
> * Bereitstellen eines Batches von zusätzlichen Mandanten
> * Kennenlernen der Details zur Bereitstellung von Mandanten und zu deren Registrierung im Katalog

Sehen Sie sich das [Tutorial zur Leistungsüberwachung](saas-dbpertenant-performance-monitoring.md) an.

## <a name="additional-resources"></a>Weitere Ressourcen

* [Weitere Tutorials, die auf der Wingtip Tickets SaaS-App mit einer Datenbank pro Mandant aufbauen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md)
* [So wird's gemacht: Debuggen von Skripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
