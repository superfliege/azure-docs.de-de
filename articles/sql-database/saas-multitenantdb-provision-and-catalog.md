---
title: "Bereitstellen und Katalogisieren neuer Mandanten in einer SaaS-Anwendung unter Verwendung einer mehrinstanzfähigen SQL-Datenbank in Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie neue Mandanten in einer mehrinstanzenfähigen SaaS-App in Azure SQL-Datenbank bereitstellen und katalogisieren können."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: saas apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2017
ms.author: billgib
ms.openlocfilehash: ec753027c8ce8040cbc574279a44eb24590fcb05
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2017
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-sharded-multi-tenant-sql-database"></a>Bereitstellen und Katalogisieren neuer Mandanten in einer SaaS-Anwendung unter Verwendung einer mehrinstanzfähigen SQL-Datenbank mit Sharding

In diesem Tutorial erfahren Sie mehr über Muster für die Bereitstellung und Katalogisierung von Mandanten, wenn Sie mit einem mehrinstanzfähigen Datenbankmodell mit Sharding arbeiten. 

Ein mehrinstanzfähiges Schema, das eine Mandanten-ID im Primärschlüssel von Tabellen mit Mandantendaten enthält, ermöglicht es, mehrere Mandanten in einer einzigen Datenbank zu speichern. Um eine große Anzahl von Mandanten zu unterstützen, werden Mandantendaten auf mehrere Shards bzw. Datenbanken verteilt. Die Daten eines Mandanten sind immer vollständig in einer einzelnen Datenbank enthalten.  Ein Katalog enthält die Zuordnung von Mandanten zu Datenbanken.   

Sie haben auch die Möglichkeit, einige Datenbanken nur mit Daten eines einzigen Mandanten aufzufüllen. Datenbanken mit Daten mehrerer Mandanten kommen in den Genuss niedrigere Gebühren pro Mandant auf Kosten der Mandantenisolierung.  Bei Datenbanken, die nur Daten eines einzigen Mandanten enthalten, liegt der Schwerpunkt auf der Isolierung und nicht auf niedrigen Kosten.  Datenbanken mit mehreren Mandanten und einzelnen Mandanten können in ein und derselben SaaS-Anwendung kombiniert werden, um Kosten oder Isolierung für jeden Mandanten zu optimieren. Mandanten können bei der Bereitstellung eine eigene Datenbank erhalten oder später in eine eigene Datenbank verschoben werden.

   ![Mehrinstanzenfähige Datenbanken-App mit Sharding mit Mandantenkatalog](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)


## <a name="tenant-catalog-pattern"></a>Mandantenkatalogmuster

Wenn Mandantendaten auf mehrere Datenbanken verteilt sind, ist es wichtig zu wissen, wo die Daten jedes Mandanten gespeichert sind. Eine Katalogdatenbank enthält die Zuordnung zwischen einem Mandanten und der Datenbank, in der seine Daten gespeichert sind.

Jeder Mandant wird anhand eines Schlüssels im Katalog identifiziert. In der SaaS-App Wingtip Tickets wird der Schlüssel des Mandanten aus einem Hash des Mandantennamens gebildet. Dies ermöglicht es der Anwendung, den Namen des Mandanten aus der URL einer Webseite zu extrahieren und damit eine Verbindung mit der entsprechenden Datenbank herzustellen. Andere Mandantenschlüsselschemas können auch verwendet werden.

Die Verwendung eines Katalogs ermöglicht es, den Namen oder Speicherort einer Mandantendatenbank nach der Bereitstellung zu ändern, ohne die Anwendung zu stören.  In einem mehrinstanzfähigen Datenbankmodell kann dadurch ein Mandant zwischen Datenbanken verschoben werden.  Darüber hinaus kann der Katalog einer Anwendung angeben, ob ein Mandant oder eine Datenbank offline ist (beispielsweise zur Wartung).

Der Katalog kann zusätzliche Mandanten- oder Datenbankmetadaten (etwa die Ebene oder Edition einer Datenbank, Schemaversion der Datenbank, den Namen, Serviceplan oder SLAs für Mandanten) und andere Informationen für die Anwendungsverwaltung, den Kundensupport oder DevOps-Prozesse speichern.  

Der Katalog kann auch verwendet werden, um eine mandantenübergreifende Berichterstellung, Schemaverwaltung und Datenextraktion für Analysezwecke zu ermöglichen. 

### <a name="elastic-database-client-library"></a>Clientbibliothek für elastische Datenbanken 
In den Beispielen zu Wingtip Tickets-SaaS-Apps wird der Katalog mithilfe der Shardverwaltungsfunktionen der [Clientbibliothek für elastische Datenbanken (Elastic Database Client Library, EDCL)](sql-database-elastic-database-client-library.md) in der Datenbank *tenantcatalog* implementiert. Die Bibliothek ermöglicht einer Anwendung die Erstellung, Verwaltung und Verwendung einer datenbankgestützten Shardzuordnung. Eine Shardzuordnung enthält eine Liste mit Shards (Datenbanken) und die Zuordnung zwischen Schlüsseln (Mandanten-IDs) und Shards.  EDCL-Funktionen können in Anwendungen oder PowerShell-Skripts im Rahmen der Mandantenbereitstellung verwendet werden, um Einträge in der Shardzuordnung zu erstellen, und später, um eine Verbindung mit der richtigen Datenbank herzustellen. Die Bibliothek speichert Verbindungsinformationen zwischen, um den Datenverkehr für die Katalogdatenbank zu minimieren und die Verbindung zu beschleunigen. 

> [!IMPORTANT]
> Sie können auf die Zuordnungsdaten in der Katalogdatenbank zugreifen, *allerdings sollten Sie sie nicht bearbeiten!* Bearbeiten Sie Zuordnungsdaten nur über EDCL-APIs. Ein direktes Bearbeiten der Zuordnungsdaten birgt das Risiko, den Katalog zu beschädigen, und wird daher nicht unterstützt.


## <a name="tenant-provisioning-pattern"></a>Muster der Bereitstellung von Mandanten

Bei der Bereitstellung eines neuen Mandanten im mehrinstanzfähigen Datenbankmodell mit Sharding muss zunächst festgelegt werden, ob der Mandant in einer gemeinsam genutzten Datenbank oder eigenen Datenbank bereitgestellt werden soll. Bei einer gemeinsam genutzten Datenbank muss ermittelt werden, ob in einer bestehenden Datenbank Speicherplatz vorhanden ist oder eine neue Datenbank benötigt wird. Wenn eine neue Datenbank nötig ist, muss sie am ordnungsgemäßen Speicherort und auf der richtigen Dienstebene erstellt, mit dem ordnungsgemäßen Schema und den richtigen Referenzdaten initialisiert und anschließend im Katalog registriert werden. Schließlich kann die Mandantenzuordnung hinzugefügt werden, um auf den entsprechenden Shard zu verweisen.

Stellen Sie die Datenbank bereit, indem Sie SQL-Skripts ausführen, eine BACPAC-Datei einsetzen oder eine Vorlagendatenbank kopieren. Die Wingtip Tickets-SaaS-Anwendungen kopieren eine Vorlagendatenbank, um neue Mandantendatenbanken zu erstellen.

Der gewählte Ansatz für die Datenbankbereitstellung muss Teil Ihrer allgemeinen Schemaverwaltungsstrategie sein. Dabei muss gewährleistet sein, dass neue Datenbanken mit dem neuesten Schema bereitgestellt werden.  Dies wird im Tutorial [Verwalten des Schemas für mehrere Mandanten in der Wingtip-SaaS-Anwendung](saas-tenancy-schema-management.md) eingehender behandelt.  

Die Mandantenbereitstellungsskripts in diesem Tutorial decken sowohl die Bereitstellung eines Mandanten in einer bestehenden Datenbank, die mit anderen Mandanten gemeinsam genutzt wird, als auch die Bereitstellung eines Mandanten in einer eigenen Datenbank ab. Mandantendaten werden anschließend initialisiert und in der Shardzuordnung im Katalog registriert. In der Beispielanwendung werden Datenbanken mit mehreren Mandanten mit einem allgemeinen Namen versehen, wie *tenants1*, *tenants2* usw., während Datenbanken mit einem einzigen Mandanten den Namen des Mandanten erhalten. Die spezifischen Benennungskonventionen, die im Beispiel verwendet werden, sind kein kritischer Teil des Musters, da die Verwendung eines Katalogs es ermöglicht, der Datenbank einen beliebigen Namen zuzuweisen.  

## <a name="provision-and-catalog-tutorial"></a>Tutorial zum Bereitstellen und Katalogisieren von Mandanten

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]

> * Bereitstellen eines Mandanten in einer mehrinstanzfähigen Datenbank
> * Bereitstellen eines Mandanten in einer Datenbank mit nur einem Mandanten
> * Bereitstellen einer Gruppe von Mandanten in einer mehrinstanzfähigen Datenbank und in einer Datenbank mit nur einem Mandanten
> * Registrieren einer Datenbank und Mandantenzuordnung in einem Katalog

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die App „Wingtip Tickets SaaS Multi-tenant Database“ wurde bereitgestellt. Unter [Bereitstellen und Kennenlernen der App „Wingtip Tickets SaaS Multi-tenant Database“](saas-multitenantdb-get-started-deploy.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="get-the-wingtip-tickets-management-scripts"></a>Abrufen der Verwaltungsskripts für Wingtip Tickets

Die Verwaltungsskripts und der Quellcode der Anwendung sind im GitHub-Repository [WingtipTicketsSaaS-MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) verfügbar. <!--See [Steps to download the Wingtip SaaS scripts](saas-tenancy-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts).-->


## <a name="provision-a-tenant-in-a-shared-database-with-other-tenants"></a>Bereitstellen eines Mandanten in einer mit anderen Mandanten gemeinsam genutzten Datenbank

Um nachzuvollziehen, wie die Wingtip Tickets-Anwendung die Bereitstellung neuer Mandanten in einer gemeinsam genutzten Datenbank implementiert, fügen Sie einen Haltepunkt hinzu und durchlaufen den Workflow:

1. Öffnen Sie in _PowerShell ISE_ \\...Learning Modules\\ProvisionAndCatalog\\_Demo-ProvisionAndCatalog.ps1_, und legen Sie die folgenden Parameter fest:
   * **$TenantName** = **Bushwillow Blues**, der Name des neuen Veranstaltungsorts.
   * **$VenueType** = **blues**, einer der vordefinierten Veranstaltungsorttypen: *blues*, classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer (Kleinschreibung, keine Leerzeichen).
   * **$Scenario** = **1**, *Bereitstellen eines Mandanten in einer mit anderen Mandanten gemeinsam genutzten Datenbank*.

1. Fügen Sie einen Haltepunkt hinzu, indem Sie den Cursor an eine beliebige Stelle in Zeile 38 bewegen (der Zeile mit *New-Tenant `*) und **F9** drücken.

   ![Haltepunkt](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

1. Betätigen Sie **F5**, um das Skript auszuführen.

1. Nachdem die Skriptausführung am Haltepunkt beendet wurde, drücken Sie **F11**, um den Code schrittweise auszuführen.

   ![debug](media/saas-multitenantdb-provision-and-catalog/debug.png)

Verfolgen Sie die Ausführung des Skripts mit den Optionen (**F10** und **F11**) im Menü **Debug** nach, um aufgerufene Funktionen zu überspringen oder einzeln auszuführen. Weitere Informationen zum Debuggen von PowerShell-Skripts finden Sie unter [Tipps zum Arbeiten mit und Debuggen von PowerShell-Skripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Im Folgenden finden Sie die wichtigsten Elemente des Bereitstellungsworkflows, den Sie durchlaufen:

* **Berechnen Sie den neuen Mandantenschlüssel**. Eine Hashfunktion wird verwendet, um den Mandantenschlüssel aus dem Mandantennamen zu erstellen.
* **Überprüfen Sie, ob der Mandantenschlüssel bereits vorhanden ist**. Der Katalog wird überprüft, um sicherzustellen, dass der Schlüssel nicht bereits registriert wurde.
* **Initialisieren Sie den Mandanten in der Standarddatenbank des Mandanten**. Die Mandantendatenbank wird mit den hinzugefügten neuen Informationen des Mandanten aktualisiert.  
* **Registrieren Sie den Mandanten im Katalog**. Die Zuordnung zwischen dem neuen Mandantenschlüssel und der bestehenden Datenbank1 „tenants1“ wird dem Katalog hinzugefügt. 
* **Fügen Sie den Namen des Mandanten einer Erweiterungstabelle des Katalogs hinzu**. Der Name des Veranstaltungsorts wird der Tabelle „Tenants“ (Mandanten) im Katalog hinzugefügt.  Dies veranschaulicht, wie die Katalogdatenbank erweitert werden kann, um zusätzliche anwendungsspezifische Daten zu unterstützen.
* **Öffnen Sie die Seite „Events“ (Veranstaltungen) für den neuen Mandanten**. Die zu *Bushwillow Blues* gehörige Veranstaltungsseite wird im Browser geöffnet:

   ![Veranstaltungen](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)


## <a name="provision-a-tenant-in-its-own-database"></a>Bereitstellen eines Mandanten in seiner eigenen Datenbank

Durchlaufen Sie nun den Prozess zum Anlegen eines Mandanten in seiner eigenen Datenbank:

1. Legen Sie weiterhin in „...\\Learning Modules\\ProvisionAndCatalog\\_Demo-ProvisionAndCatalog.ps1_“ die folgenden Parameter fest:
   * **$TenantName** = **Sequoia Soccer**, der Name des neuen Veranstaltungsorts.
   * **$VenueType** = **soccer**, einer der vordefinierten Veranstaltungsorttypen: *blues*, classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer (Kleinschreibung, keine Leerzeichen).
   * **$Scenario** = **2**, *Bereitstellen eines Mandanten in einer mit anderen Mandanten gemeinsam genutzten Datenbank*.

1. Fügen Sie einen neuen Haltepunkt hinzu, indem Sie den Cursor an eine beliebige Stelle in Zeile 57 bewegen (der Zeile mit *&&nbsp;$PSScriptRoot\New-TenantAndDatabase `*) und **F9** drücken.

   ![Haltepunkt](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

1. Drücken Sie **F5** , um das Skript auszuführen.

1. Nachdem die Skriptausführung am Haltepunkt beendet wurde, drücken Sie **F11**, um den Code schrittweise auszuführen.  Drücken Sie **F10** und **F11**, um zum Verfolgen der Ausführung Prozedurschritte zu durchlaufen und Funktionen schrittweise aufzurufen.

Im Folgenden finden Sie die wichtigsten Elemente des Workflows, den Sie beim Nachverfolgen des Skripts durchlaufen:

* **Berechnen Sie den neuen Mandantenschlüssel**. Eine Hashfunktion wird verwendet, um den Mandantenschlüssel aus dem Mandantennamen zu erstellen.
* **Überprüfen Sie, ob der Mandantenschlüssel bereits vorhanden ist**. Der Katalog wird überprüft, um sicherzustellen, dass der Schlüssel nicht bereits registriert wurde.
* **Erstellen Sie eine neue Mandantendatenbank**. Die Datenbank wird durch Kopieren der Datenbank *basetenantdb* unter Verwendung einer Ressourcen-Manager-Vorlage erstellt.  Der Name der neuen Datenbank basiert auf dem Mandantennamen.
* **Fügen Sie die Datenbank dem Katalog hinzu**. Die neue Mandantendatenbank wird als Shard im Katalog registriert.
* **Initialisieren Sie den Mandanten in der Standarddatenbank des Mandanten**. Die Mandantendatenbank wird mit den hinzugefügten neuen Informationen des Mandanten aktualisiert.  
* **Registrieren Sie den Mandanten im Katalog**. Die Zuordnung zwischen dem neuen Mandantenschlüssel und der bestehenden Datenbank *sequoiasoccer* wird dem Katalog hinzugefügt.
* **Der Mandantenname wird dem Katalog hinzugefügt**. Der Name des Veranstaltungsorts wird der Erweiterungstabelle „Tenants“ (Mandanten) im Katalog hinzugefügt.
* **Öffnen Sie die Seite „Events“ (Veranstaltungen) für den neuen Mandanten**. Die zu *Sequoia Soccer* gehörige Veranstaltungsseite wird im Browser geöffnet:

   ![Veranstaltungen](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)


## <a name="provision-a-batch-of-tenants"></a>Bereitstellen eines Batches von Mandanten

In dieser Übung wird ein Batch mit 17 Mandanten bereitgestellt. Es empfiehlt sich, diese Mandantengruppe bereitzustellen, bevor Sie mit anderen Wingtip-SaaS-Tutorials beginnen, damit Sie mit mehreren Datenbanken arbeiten können.

1. Öffnen Sie in *PowerShell ISE* \\...Learning Modules\\ProvisionAndCatalog\\*Demo-ProvisionAndCatalog.ps1*, und ändern Sie den *$Scenario*-Parameter in „3“:
   * **$DemoScenario** = **3**, *Bereitstellen einer Gruppe von Mandanten in einer gemeinsam genutzten Datenbank*.
1. Drücken Sie **F5**, um das Skript auszuführen.


### <a name="verify-the-deployed-set-of-tenants"></a>Überprüfen der bereitgestellten Gruppe von Mandanten 
In dieser Phase haben Sie eine Kombination von Mandanten, die in einer gemeinsam genutzten Datenbank bereitgestellt wurden, und Mandanten, die in ihren eigenen Datenbanken bereitgestellt wurden. Im Azure-Portal können die erstellten Datenbanken überprüft werden:  

* Öffnen Sie im [Azure-Portal](https://portal.azure.com) den Server **tenants1-mt-\<USER\>**, indem Sie zur Liste der SQL-Server navigieren.  Die Liste **SQL-Datenbanken** sollte die gemeinsame genutzte Datenbank **tenants1** und die Datenbanken für die Mandanten mit eigener Datenbank enthalten:

   ![Datenbankliste](media/saas-multitenantdb-provision-and-catalog/Databases.png)

Sie können zwar im Azure-Portal die Mandantendatenbanken sehen, aber nicht die Mandanten *innerhalb* der gemeinsam genutzten Datenbank einsehen. Die vollständige Liste der Mandanten sehen auf der Wingtip Tickets-Hubseite „Events“ und bei Durchsuchen des Katalogs:   

1. Öffnen Sie die Hubseite „Events“ im Browser (http:events.wingtip-mt.\<USER\>.trafficmanager.net).  

   Die vollständige Liste der Mandanten und ihre entsprechende Datenbank sind im Katalog verfügbar. In der Datenbank „tenantcatalog“ steht eine SQL-Sicht zur Verfügung, die den in der Tabelle „Tenants“ gespeicherten Mandantennamen mit dem Datenbanknamen in den Tabellen zur Shardverwaltung verbindet. Diese Sicht veranschaulicht eindrucksvoll, wie nützlich das Erweitern der im Katalog gespeicherten Metadaten ist.

2. Verbinden Sie sich in *SQL Server Management Studio (SSMS)* mit dem Server „tenants“ unter **tenants1-mt.\<USER\>.database.windows.net** mit den Anmeldedaten: **developer**, Kennwort: **P@ssword1**

    ![SSMS-Verbindungsdialogfeld](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

2. Navigieren Sie im *Objekt-Explorer* zu den Sichten in der Datenbank *tenantcatalog*.
2. Klicken Sie mit der rechten Maustaste auf die Sicht *TenantsExtended*, und wählen Sie **Select Top 1000 Rows** aus. Beachten Sie die Zuordnung zwischen dem Namen des Mandanten und der Datenbank für die verschiedenen Mandanten.

    ![Sicht „ExtendedTenants“ in SSMS](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Weitere Bereitstellungsmuster

Es folgen andere interessante Bereitstellungsmuster:

**Vorabbereitstellung von Datenbanken in Pools für elastische Datenbanken.** Das Vorabbereitstellungsmuster profitiert von der Tatsache, dass bei Verwenden von Datenbanken in einem Pool für elastische Datenbanken Kosten für den Pool und nicht für die Datenbanken anfallen. Daher können Datenbanken ohne zusätzliche Kosten einem Pool elastischer Datenbanken hinzugefügt werden, bevor sie benötigt werden. Indem Sie Datenbanken vorab in einem Pool bereitstellen und sie bei Bedarf zuordnen, kann das Bereitstellen eines Mandanten in einer Datenbank erheblich beschleunigt werden. Die Anzahl der vorab bereitgestellten Datenbanken kann bedarfsorientiert angepasst werden, um einen für die erwartete Bereitstellungsrate geeigneten Puffer bereitzuhalten.

**Automatische Bereitstellung.** Bei der Vorabbereitstellung wird ein dedizierter Bereitstellungsdienst verwendet, um Server, Pools und Datenbanken bei Bedarf automatisch bereitzustellen – einschließlich vorab bereitgestellter Datenbanken in Pools für elastische Datenbanken. Wenn Datenbanken außer Betrieb genommen und gelöscht werden, können entstehende Lücken in den Pools für elastische Datenbanken durch den Bereitstellungsdienst bedarfsorientiert gefüllt werden. Ein solcher Dienst kann einfach oder komplex sein. Die Bereitstellung kann also beispielsweise über mehrere geografische Regionen erfolgen, und die Georeplikation kann zur Notfallwiederherstellung eingerichtet werden. Beim Muster der automatischen Bereitstellung sendet eine Clientanwendung oder ein Skript eine Bereitstellungsanforderung an eine Warteschlange, damit diese vom Bereitstellungsdienst verarbeitet wird. Anschließend wird der Abschluss des Vorgangs abgefragt. Wenn Datenbanken vorab bereitgestellt wurden, werden Anforderungen schnell verarbeitet, während der Dienst die Bereitstellung einer Ersatzdatenbank im Hintergrund abwickelt.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Bereitstellen eines neuen Mandanten in einer gemeinsam genutzten mehrinstanzfähigen Datenbank und in seiner eigenen Datenbank
> * Bereitstellen einer Gruppe zusätzlicher Mandanten
> * Kennenlernen der Details zur Bereitstellung von Mandanten und zu deren Registrierung im Katalog

Sehen Sie sich das [Tutorial zur Leistungsüberwachung](saas-multitenantdb-performance-monitoring.md) an.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

<!--* Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md)
* [So wird's gemacht: Debuggen von Skripts in Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
