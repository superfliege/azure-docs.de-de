---
title: "Bereitstellen einer mehrinstanzenfähigen SaaS-Datenbankanwendung mit Sharding, die Azure SQL-Datenbank verwendet | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die mehrinstanzenfähige Wingtip Tickets-SaaS-Datenbankanwendung mit Sharding bereitstellen und kennenlernen, mit der SaaS-Muster per Azure SQL-Datenbank dargestellt werden."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: billgib;MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: sstein
ms.openlocfilehash: bc96221abf62677b53df43daa44a925ac5792043
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Bereitstellen und Kennenlernen einer mehrinstanzenfähigen Anwendung mit Sharding, die Azure SQL-Datenbank verwendet

In diesem Tutorial wird beschrieben, wie Sie eine mehrinstanzenfähige SaaS-Beispiel-Datenbankanwendung mit dem Namen Wingtip Tickets bereitstellen und untersuchen. Die Wingtip-App ist dafür ausgelegt, Features von Azure SQL-Datenbank zu veranschaulichen, mit denen die Implementierung von SaaS-Szenarien vereinfacht wird.

Bei dieser Implementierung von Wingtips wird ein mehrinstanzenfähiges Datenbankmuster mit Sharding verwendet. Das Sharding erfolgt nach Mandantenbezeichner. Mandantendaten werden abhängig von Werten der Mandanten-ID an bestimmte Datenbanken verteilt. Unabhängig davon, wie viele Mandanten eine bestimmte Datenbank enthält, sind alle Datenbanken in dem Sinne mehrinstanzenfähig, dass die Tabellenschemas eine Mandanten-ID enthalten. 

Diese Datenbankmuster ermöglichen Ihnen, in jedem Shard oder jeder Datenbank einen oder mehrere Mandanten zu speichern. Sie können die Optimierung nach den niedrigsten Kosten vornehmen, indem jede Datenbank von mehreren Mandanten gemeinsam genutzt wird. Oder Sie können die Optimierung im Hinblick auf Isolation vornehmen, indem Sie in jeder Datenbank nur einen einzigen Mandanten speichern. Ihre Wahl für die Optimierung kann für jeden einzelnen Mandanten unabhängig vorgenommen werden. Sie können Ihre Wahl treffen, wenn der Mandant zum ersten Mal gespeichert wird, oder Sie können Ihre Meinung später ändern. Die Anwendung ist so konzipiert, dass sie in beiden Fällen gut funktioniert.

#### <a name="app-deploys-quickly"></a>App wird schnell bereitgestellt

Der folgende Abschnitt zur Bereitstellung enthält die Schaltfläche **Bereitstellung in Azure**. Wenn Sie auf die Schaltfläche klicken, ist die Wingtip-App bereits fünf Minuten später vollständig bereitgestellt. Die Wingtip-App wird in der Azure-Cloud ausgeführt und verwendet die Azure SQL-Datenbank. Wingtip wird in Ihrem Azure-Abonnement bereitgestellt. Sie haben vollen Zugriff auf die einzelnen Anwendungskomponenten.

Die Anwendung wird mit Daten für drei Beispielmandanten bereitgestellt. Die Mandanten werden zusammen in einer mehrinstanzenfähigen Datenbank gespeichert.

Jeder kann den C#- und PowerShell-Quellcode für Wingtip Tickets aus [unserem GitHub-Repository][link-github-wingtip-multitenantdb-55g] herunterladen.

#### <a name="learn-in-this-tutorial"></a>In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]

> - Bereitstellen der Wingtip-SaaS-Anwendung
> - Abrufen des Quellcodes der Anwendung und der Verwaltungsskripts
> - Informationen zu Servern und Datenbanken, aus denen sich die App zusammensetzt
> - Zuordnen der Mandanten zu den zugehörigen Daten mithilfe des *Katalogs*
> - Bereitstellen eines neuen Mandanten
> - Überwachen der Mandantenaktivität in der App

Eine Reihe von verwandten Tutorials steht zur Verfügung, die auf dieser anfänglichen Bereitstellung aufbauen. In den Tutorials werden eine Reihe von SaaS-Entwurfs- und -Verwaltungsmustern untersucht. Beim Durcharbeiten der Tutorials sollten Sie sich die bereitgestellten Skripts genauer anschauen, um zu sehen, wie die verschiedenen SaaS-Muster implementiert werden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

- Die neueste Azure PowerShell ist installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Bereitstellen der Wingtip Tickets-App

1. Klicken Sie auf die folgende **Bereitstellung in Azure**-Schaltfläche.
    - Das Azure-Portal mit der Vorlage für die Wingtip Tickets-SaaS-Bereitstellung wird geöffnet. Für die Vorlage sind zwei Parameterwerte erforderlich: ein Name für eine neue Ressourcengruppe und ein Wert „Benutzer“, mit dem diese Bereitstellung von anderen Bereitstellungen der App unterschieden wird. Im nächsten Schritt sind Details zum Festlegen dieser Parameterwerte angegeben.
        - Notieren Sie sich auf jeden Fall die genauen Werte, die Sie verwenden, da Sie sie später für eine Konfigurationsdatei benötigen.

    [![Schaltfläche für „Bereitstellen in Azure“.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

2. Geben Sie die erforderlichen Parameterwerte für die Bereitstellung ein.

    > [!IMPORTANT]
    > Einige Einstellungen für die Authentifizierung und die Serverfirewall sind absichtlich unsicher, um die Demo zu erleichtern. Wählen Sie **Erstellen Sie eine neue Ressourcengruppe** aus, und verwenden Sie keine vorhandenen Ressourcengruppen, Server oder Pools. Verwenden Sie diese Anwendung und alle damit erstellten Ressourcen nicht für die Produktion. Wenn Sie sich umfassend mit der Anwendung vertraut gemacht haben, löschen Sie diese Ressourcengruppe, um die zugehörige Abrechnung einzustellen.

    Es wird empfohlen, in den Ressourcennamen nur Kleinbuchstaben, Zahlen und Bindestriche zu verwenden.

    - **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie einen **Namen** für die Ressourcengruppe an (Groß-/Kleinschreibung wird berücksichtigt).
        - Alle Buchstaben in Ihrem Ressourcengruppennamen sollten Kleinbuchstaben sein.
        - Es wird empfohlen, dass Sie einen Gedankenstrich, gefolgt von Ihren Initialen und einer Ziffer anfügen: z. B. *wingtip-af1*.
        - Wählen Sie einen **Speicherort** in der Dropdownliste aus.
    - **Benutzer**: Es wird empfohlen, einen kurzen **Benutzer**-Wert zu wählen, z. B. Ihre Initialen plus eine Ziffer: z. B. *af1*.

3. **Bereitstellen der Anwendung**.

    - Klicken Sie auf die entsprechende Option, um den Geschäftsbedingungen zuzustimmen.
    - Klicken Sie auf **Kaufen**.

4. Überwachen Sie den Bereitstellungsstatus, indem Sie auf **Benachrichtigungen** klicken (das Glockensymbol rechts neben dem Suchfeld). Das Bereitstellen der Wingtip-App dauert ungefähr fünf Minuten.

   ![Bereitstellung erfolgreich](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Herunterladen und Aufheben der Blockierung der Verwaltungsskripts

Laden Sie den Quellcode der Anwendung und die Verwaltungsskripts herunter, während die Anwendung bereitgestellt wird.

> [!IMPORTANT]
> Ausführbare Inhalte (Skripts, DLLs) können durch Windows blockiert werden, wenn ZIP-Dateien aus einer externen Quelle heruntergeladen und extrahiert werden. Führen Sie bei der Extraktion der Skripts aus einer ZIP-Datei die nachfolgenden Schritte aus, um die Blockierung der ZIP-Datei vor der Extraktion aufzuheben. Durch die Aufhebung der Blockierung der ZIP-Datei stellen Sie sicher, dass die Skripts ausgeführt werden dürfen.

1. Navigieren Sie zum [GitHub-Repository „WingtipTicketsSaaS-MultiTenantDb“](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Klicken Sie auf **Klonen oder herunterladen**.
3. Klicken Sie auf **ZIP herunterladen**, und speichern Sie die Datei.
4. Klicken Sie mit der rechten Maustaste auf die Datei **WingtipTicketsSaaS-MultiTenantDb-master.zip**, und wählen Sie **Eigenschaften** aus.
5. Wählen Sie auf der Registerkarte **Allgemein** die Option **Unblock** (Zulassen), und klicken Sie auf **Übernehmen**.
6. Klicken Sie auf **OK**.
7. Extrahieren Sie die Dateien.

Skripts befinden sich im Ordner „*..\\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\*“.

## <a name="update-the-configuration-file-for-this-deployment"></a>Aktualisieren der Konfigurationsdatei für diese Bereitstellung

Legen Sie vor dem Ausführen von Skripts die Werte *resource group* und *user* in der Datei **UserConfig.psm1** fest. Legen Sie diese Variablen auf die gleichen Werte fest, die Sie während der Bereitstellung angegeben haben.

1. Öffnen Sie ...\\Learning Modules\\*UserConfig.psm1* in der *PowerShell ISE*
2. Aktualisieren Sie *ResourceGroupName* und *Name* mit den jeweiligen Werten für Ihre Bereitstellung (nur in den Zeilen 10 und 11).
3. Speichern Sie die Änderungen.

Die in dieser Datei festgelegten Werte werden von allen Skripts verwendet, deshalb ist es wichtig, dass sie korrekt sind. Wenn Sie die App erneut bereitstellen, stellen Sie sicher, dass Sie einen anderen Ressourcengruppen- und Benutzerwert auswählen. Aktualisieren Sie die UserConfig-Datei anschließend mit den neuen Werten.

## <a name="run-the-application"></a>Ausführen der Anwendung

In der App werden Einrichtungen wie Konzerthallen, Jazzclubs, Sportstadien usw. vorgestellt, in denen Veranstaltungen stattfinden. Die Veranstalter registrieren sich als Kunden (oder Mandanten) der Wingtip-Plattform und verfügen damit über eine einfache Möglichkeit, Veranstaltungen anzukündigen und Tickets zu verkaufen. Jeder Veranstalter erhält eine personalisierte Web-App zum Verwalten und Auflisten seiner Veranstaltungen und zum Verkaufen von Tickets. Diese ist unabhängig und abgegrenzt von denen anderer Mandanten. Im Hintergrund werden die Daten jedes Mandanten standardmäßig in einer mehrinstanzenfähigen Sharddatenbank gespeichert.

In einem zentralen **Ereignis-Hub** wird eine Liste der Links zu den Mandanten in Ihrer spezifischen Bereitstellung aufgeführt.

1. Öffnen Sie den *Ereignis-Hub* in Ihrem Webbrowser:
    - http://events.wingtip.&lt;BENUTZER&gt;.trafficmanager.net &nbsp; *(durch den Benutzerwert Ihrer Bereitstellung ersetzen.)*

    ![Events Hub](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. Klicken Sie im **Ereignis-Hub** auf *Fabrikam Jazz Club*.

   ![Ereignisse](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

Zum Steuern der Verteilung eingehender Anforderungen nutzt die App [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Für die Ereignisseiten, die mandantenspezifisch sind, schließen Sie den Mandantennamen in die URLs ein. Die URLs enthalten zudem Ihren spezifischen Benutzerwert und weisen dieses Format auf:

- http://events.wingtip.&lt;BENUTZER&gt;.trafficmanager.net/*fabrikamjazzclub*
 
Die Ereignis-App analysiert den Mandantennamen in der URL und hashcodiert ihn, um einen Schlüssel für den Zugriff auf einen Katalog mithilfe der [Shard-Zuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md) zu erstellen. Im Katalog wird der Schlüssel dem Datenbankspeicherort für den Mandanten zugeordnet. Der **Ereignis-Hub** listet alle Mandanten auf, die im Katalog registriert sind. Der **Ereignis-Hub** ruft mit erweiterten Metadaten im Katalog den Namen des Mandanten für die jeweilige Zuordnung ab, um die URL zu erstellen.

In einer Produktionsumgebung würde typischerweise ein CNAME-DNS-Eintrag zum [Zuordnen einer Unternehmensinternetdomäne zum Traffic Manager-Profil](../traffic-manager/traffic-manager-point-internet-domain.md) erstellt werden.

## <a name="start-generating-load-on-the-tenant-databases"></a>Generieren von Last in den Mandantendatenbanken

Nachdem die App bereitgestellt wurde, können wir sie nutzen. Mit dem PowerShell-Skript *Demo-LoadGenerator* wird eine Workload gestartet, die für jeden Mandanten ausgeführt wird. Die Auslastung vieler echter SaaS-Apps ist häufig sporadischer Art und nicht vorhersagbar. Um diese Art der Auslastung zu simulieren, erzeugt der Generator eine Last, die auf alle Mandanten verteilt wird. Die Last enthält randomisierte Datenverkehrsspitzen auf jedem Mandanten, die in zufälligen Intervallen auftreten. Es dauert mehrere Minuten, bis das Auslastungsmuster erreicht ist. Es ist also am besten, den Generator mindestens drei oder vier Minuten laufen zu lassen, bevor die Auslastung überwacht wird.

1. Öffnen Sie in der *PowerShell ISE* das Skript „...\\Learning Modules\\Utilities\\*Demo-LoadGenerator.ps1*“.
2. Drücken Sie **F5**, um das Skript auszuführen und den Last-Generator zu starten (übernehmen Sie zunächst die Standard-Parameterwerte).

> [!IMPORTANT]
> Das Skript *Demo-LoadGenerator.ps1* öffnet eine andere PowerShell-Sitzung, in der der Lastengenerator ausgeführt wird. Der Lastengenerator wird in dieser Sitzung als ein Task im Vordergrund ausgeführt, der für jeden Mandanten je einen Lastengenerierungsauftrag im Hintergrund aufruft.

Nachdem der Task im Vordergrund gestartet wurde, bleibt er im Zustand für das Aufrufen von Aufträgen. Der Task startet zusätzliche Hintergrundaufträge für alle neuen Mandanten, die anschließend bereitgestellt werden.

Durch Schließen der PowerShell-Sitzung werden alle Aufträge beendet.

Möglicherweise möchten Sie die Lastengeneratorsitzung neu starten, um andere Parameterwerte zu verwenden. Wenn dies der Fall ist, schließen Sie die PowerShell-Generierungssitzung, und führen Sie *Demo-LoadGenerator.ps1* erneut aus.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Bereitstellen eines neuen Mandanten in der Sharddatenbank

Die erste Bereitstellung umfasst drei Beispielmandanten in der Datenbank *Tenants1*. Erstellen Sie einen anderen Mandanten, um zu sehen, wie sich dies auf die bereitgestellte Anwendung auswirkt. In diesem Schritt erstellen Sie schnell einen neuen Mandanten.

1. Öffnen Sie ...\\Learning Modules\Provision and Catalog\\*Demo-ProvisionTenants.ps1* in der *PowerShell ISE*.
2. Drücken Sie **F5**, um das Skript auszuführen (lassen Sie die Standardwerte zunächst unverändert).

   > [!NOTE]
   > Viele Wingtip Tickets-SaaS-Skripts verwenden *$PSScriptRoot*, um die Navigation in den Ordnern zu ermöglichen, andere Skripts aufzurufen oder Module zu importieren. Diese Variable wird nur ausgewertet, wenn das Skript durch das Drücken von **F5** als ganzes ausgeführt wird.  Das Markieren und Ausführen einer Auswahl (**F8**) kann zu Fehlern führen. Drücken Sie daher zum Ausführen von Skripts **F5**.

Der neue Mandant Red Maple Racing wird zur Datenbank *Tenants1* hinzugefügt und im Katalog registriert. Die Website des neuen Mandanten für den Ticketverkauf für *Ereignisse* wird in Ihrem Browser geöffnet:

![Neuer Mandant](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Aktualisieren Sie den *Ereignis-Hub*. Der neue Mandant wird jetzt in der Liste aufgeführt.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Bereitstellen eines neuen Mandanten in der eigenen Datenbank

Das mehrinstanzenfähige Modell mit Shards erlaubt zwei Optionen: Sie können einen neuen Mandanten in einer Datenbank, die andere Mandanten enthält, oder in einer eigenen Datenbank bereitstellen. Ein einzelner Mandant in einer Datenbank bietet den Vorteil, dass die Daten von den Daten anderer Mandanten isoliert sind. Die Isolation ermöglicht Ihnen, die Leistung dieses Mandanten unabhängig von anderen Mandanten zu verwalten. Darüber hinaus ist es einfacher, die Daten für den isolierten Mandanten zu einem früheren Zeitpunkt wiederherzustellen. Sie können beispielsweise Nutzer kostenloser Testversionen und reguläre Kunden in eine mehrinstanzenfähige Datenbank und Premium-Kunden in eine einzelne Datenbank platzieren. Wenn Sie viele Datenbanken erstellen, die jeweils nur einen einzigen Mandanten enthalten, können Sie diese zusammen in einem elastischen Pool verwalten, um die Ressourcenkosten zu optimieren.  

Nun stellen wir einen anderen Mandanten bereit, dieses Mal in seiner eigenen Datenbank.

1. Nehmen Sie in ...\\Learning Modules\\Provision and Catalog\*Demo-ProvisionTenants.ps1* folgende Änderungen vor: Ändern Sie *$TenantName* zu **Salix Salsa**, *$VenueType* zu **Tanz** und *$Scenario* zu **2**.

2. Drücken Sie **F5**, um das Skript erneut auszuführen.
    - Indem Sie F5 drücken, wird der neue Mandant in einer separaten Datenbank bereitgestellt. Die Datenbank und der Mandant werden im Katalog registriert. Dann wird im Browser die Seite „Ereignisse“ des Mandanten geöffnet.

   ![Seite „Salix Salsa-Ereignisse“](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Scrollen Sie zum Ende der Seite. Im Banner sehen Sie den Namen der Datenbank, in der die Mandantendaten gespeichert werden.

3. Aktualisieren Sie den Ereignis-Hub. Zwei neue Mandanten werden jetzt in der Liste aufgeführt.



## <a name="explore-the-servers-and-tenant-databases"></a>Untersuchen der Server und Mandantendatenbanken

Betrachten wir nun einige der Ressourcen, die bereitgestellt wurden:

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com), zur Liste der Ressourcengruppen. Öffnen Sie die Ressourcengruppe, die Sie bei der Bereitstellung der Anwendung erstellt haben.

   ![Ressourcengruppe](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. Klicken Sie auf den Server **catalog-mt&lt;BENUTZER&gt;**. Der Katalogserver enthält zwei Datenbanken: *tenantcatalog* und *basetenantdb*. Die Datenbank *basetenantdb* ist eine leere Vorlagendatenbank. Kopieren Sie diese Vorlagendatenbank, um eine neue Mandantendatenbank für mehrere oder für nur einen Mandanten zu erstellen.

   ![Katalogserver](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Wechseln Sie zurück zur Ressourcengruppe, und wählen den Server *tenants1-mt* aus, auf dem die Mandantendatenbanken gespeichert sind.
    - Die tenants1-Datenbank ist eine mehrinstanzenfähige Datenbank, in der die ursprünglichen drei Mandanten sowie der erste hinzugefügte Mandant gespeichert sind. Sie ist als Standarddatenbank mit 50 Datenübertragungseinheiten (DTU) konfiguriert.
    - Die Datenbank **salixsalsa** enthält den Salix Salsa-Tanztreffpunkt als einzigen Mandanten. Sie ist standardmäßig als eine Standard Edition-Datenbank mit 50 Datenübertragungseinheiten konfiguriert.

   ![Mandantenserver](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)


## <a name="monitor-the-performance-of-the-database"></a>Überwachen der Leistung der Datenbank

Wenn der Lastengenerator mehrere Minuten ausgeführt wurde, sollten genügend Telemetriedaten verfügbar sein, um einige der in das Portal integrierten Datenbanküberwachungsfunktionen zu betrachten.

1. Navigieren Sie zum Server **tenants1-mt&lt;BENUTZER&gt;**, und klicken Sie auf **tenants1**, um die Ressourcenverwendung für die Datenbank anzuzeigen, die vier Mandanten enthält. Jeder Mandant unterliegt einer sporadischen hohen Auslastung durch den Lastengenerator:

   ![Überwachen von tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   Das DTU-Workloaddiagramm veranschaulicht auf verständliche Weise, wie eine mehrinstanzenfähige Datenbank eine nicht vorhersagbare Arbeitslast über viele Mandanten hinweg unterstützen kann. In diesem Fall wendet der Lastengenerator eine sporadische Last von ungefähr 30 Datenübertragungseinheiten auf jeden Mandant an. Diese Last entspricht einer 60-prozentigen Auslastung einer Datenbank mit 50 Datenübertragungseinheiten. Spitzen, die die 60 % überschreiten, sind das Ergebnis der auf mehrere Mandanten gleichzeitig angewendeten Last.

2. Navigieren Sie zum Server **tenants1-mt&lt;USER&gt;**, und klicken Sie auf die Datenbank **salixsalsa**, um die Ressourcenverwendung für diese Datenbank anzuzeigen, die nur einen einzigen Mandanten enthält.

   ![Datenbank „salixsalsa“](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

Der Lastengenerator wendet eine ähnliche Last auf jeder Mandanten an, unabhängig davon, in welcher Datenbank sich der jeweilige Mandant befindet. Sie erkennen, dass die Datenbank **salixsalsa** mit nur einem Mandanten eine viel höhere Auslastung toleriert, als die Datenbank mit mehreren Mandanten. 

> [!NOTE]
> Die durch den Lastengenerator generierten Lasten dienen nur zu Illustrationszwecken.  Die der Datenbank mit mehreren Mandanten und der Datenbank mit nur einem Mandanten zugeordneten Ressourcen und die Anzahl der Mandanten, die Sie in einer mehrinstanzenfähigen Datenbank hosten können, hängen von den tatsächlichen Workloadmustern in der Anwendung ab.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> - Bereitstellen der mehrinstanzenfähigen Wingtip Tickets-SaaS-Datenbankanwendung
> - Informationen zu Servern und Datenbanken, aus denen sich die App zusammensetzt
> - Mandanten werden den zugehörigen Daten mithilfe des *Katalogs* zugeordnet
> - Bereitstellen neuer Mandanten in einer Datenbank mit mehreren Mandanten und in einer Datenbank mit nur einem Mandanten
> - Anzeigen der Poolnutzung zum Überwachen der Mandantenaktivität
> - Löschen von Beispielressourcen, um die zugehörige Abrechnung einzustellen

Arbeiten Sie nun das [Tutorial zum Bereitstellen und Katalogisieren von Mandanten](sql-database-saas-tutorial-provision-and-catalog.md) durch.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Weitere Informationen zu mehrinstanzenfähigen SaaS-Anwendungen finden Sie unter [*Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)








<!--  Link references.

A [series of related tutorials] is available that build upon this initial deployment.
[link-wtp-overivew-jumpto-saas-tutorials-97j]: saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials

-->

[link-aka-ms-deploywtp-mtapp-52k]: http://aka.ms/deploywtp-mtapp


[link-azure-get-started-powershell-41q]: https://docs.microsoft.com/powershell/azure/get-started-azureps

[link-github-wingtip-multitenantdb-55g]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/





<!--  Image references.

[image-deploy-to-azure-blue-48d]: http://aka.ms/deploywtp-mtapp "Button for Deploy to Azure."

-->

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png

