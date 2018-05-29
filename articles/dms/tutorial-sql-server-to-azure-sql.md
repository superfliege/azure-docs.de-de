---
title: Verwenden von Azure Database Migration Service zur Migration von SQL Server zu Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure Database Migration Service eine Migration von einer lokalen SQL Server-Instanz zu Azure SQL-Datenbank durchführen.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/07/2018
ms.openlocfilehash: c110011f3b4c3c677354bc8423c8cd86cca6ac90
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32776175"
---
# <a name="migrate-sql-server-to-azure-sql-database-using-dms"></a>Migrieren von SQL Server zu Azure SQL-Datenbank mit DMS
Mit Azure Database Migration Service können Sie die Datenbanken von einer lokalen SQL Server-Instanz zu [Azure SQL-Datenbank](https://docs.microsoft.com/en-us/azure/sql-database/) migrieren. In diesem Tutorial migrieren Sie die Datenbank **Adventureworks2012**, die in einer lokalen Instanz von SQL Server 2016 (oder höher) wiederhergestellt wurde, mithilfe von Azure Database Migration Service zu einer Azure SQL-Datenbank.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Bewerten Ihrer lokalen Datenbank mithilfe des Data Migration Assistant
> * Migrieren des Beispielschemas über den Data Migration Assistant
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen Sie ein Migrationsprojekt mithilfe von Azure Database Migration Service.
> * Ausführen der Migration
> * Überwachen der Migration
> * Laden Sie einen Migrationsbericht herunter.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

- Laden Sie [SQL Server 2016 oder höher](https://www.microsoft.com/sql-server/sql-server-downloads) (beliebige Edition) herunter, und installieren Sie es.
- Aktivieren Sie das TCP/IP-Protokoll (dieses wird während der SQL Server Express-Installation standardmäßig deaktiviert), indem Sie die Anweisungen im Artikel [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) befolgen.
- Erstellen Sie eine Azure SQL-Datenbankinstanz, indem Sie die Anweisungen im Artikel [Erstellen einer Azure SQL-Datenbank im Azure-Portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) befolgen.
- Laden Sie den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595), Version 3.3 oder höher, herunter, und installieren Sie ihn.
- Erstellen Sie ein VNET für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bietet.
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln Ihres Azure Virtual Network (VNET) nicht die Kommunikationsports 443, 53, 9354, 445 und 12000 blockieren. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Konfigurieren Sie Ihre [Windows-Firewall für Datenbank-Engine-Zugriff](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die SQL Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 1433).
- Bei der Ausführung mehrerer benannter SQL Server-Instanzen mit dynamischen Ports empfiehlt es sich, den SQL-Browser-Dienst zu aktivieren und den Zugriff auf den UDP-Port 1434 durch Ihre Firewalls zuzulassen, sodass Azure Database Migration Service eine Verbindung mit einer benannten Instanz auf Ihrem Quellserver herstellen kann.
- Wenn Sie eine Firewall-Appliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration zugreifen kann.
- Erstellen Sie für Azure SQL-Datenbankserver eine [Firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) auf Serverebene, um den Zugriff auf die Zieldatenbanken durch Azure Database Migration Service zu ermöglichen. Geben Sie den Subnetzbereich des für Azure Database Migration Service verwendeten VNET an.
- Stellen Sie sicher, dass die für die Verbindung mit der SQL Server-Quellinstanz verwendeten Anmeldeinformationen über [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql)-Berechtigungen verfügen.
- Stellen Sie sicher, dass die für die Verbindung mit der Azure SQL-Zieldatenbankinstanz verwendeten Anmeldeinformationen die Berechtigung CONTROL DATABASE für die Azure SQL-Zieldatenbanken besitzen.

## <a name="assess-your-on-premises-database"></a>Bewerten Ihrer lokalen Datenbank
Damit Sie Daten von einer lokalen SQL Server-Instanz zu Azure SQL-Datenbank migrieren können, müssen Sie die SQL Server-Datenbank auf Probleme untersuchen, die eine Migration möglicherweise verhindern könnten. Führen Sie im Datenmigrations-Assistenten, Version 3.3 oder höher, die im Artikel [Durchführen einer SQL Server-Migrationsbewertung](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) beschriebenen Schritte aus, um die Bewertung der lokalen Datenbank abzuschließen. Hier sehen Sie eine Zusammenfassung der erforderlichen Schritte:
1.  Wählen Sie im Data Migration Assistant das Symbol „New (+)“ (Neu), und wählen Sie dann den Projekttyp **Assessment** (Bewertung) aus.
2.  Geben Sie einen Projektnamen im Textfeld **Source server type** (Quellservertyp) an, wählen Sie **SQL Server**, wählen Sie dann im Textfeld **Target server type** (Zielservertyp) die Option **Azure SQL Database** (Azure SQL-Datenbank) und anschließend **Create** (Erstellen), um das Projekt zu erstellen.

    Bei der Bewertung der SQL Server-Quelldatenbank für die Migration zu Azure SQL-Datenbank können Sie einen oder beide der folgenden Bewertungsberichtsarten auswählen:
    - Check database compatibility (Datenbankkompatibilität prüfen)
    - Check feature parity (Featureparität prüfen)

    Beide Berichtsarten sind standardmäßig ausgewählt.

4.  Wählen Sie im Data Migration Assistant im Bildschirm **Options** (Optionen) die Schaltfläche **Weiter**.
5.  Geben Sie im Bildschirm **Select sources** (Quellen auswählen) im Dialogfeld **Connect to a server** (Verbindung mit einem Server herstellen) die Verbindungsdetails für SQL Server an, und wählen Sie dann **Connect** (Verbinden).
6.  Wählen Sie im Dialogfeld **Quellen hinzufügen** den Eintrag **AdventureWorks2012** und dann nacheinander **Hinzufügen** und **Bewertung starten** aus.

    Nach Abschluss der Bewertung werden die Ergebnisse wie in der folgenden Abbildung angezeigt:

    ![Bewerten der Datenmigration](media\tutorial-sql-server-to-azure-sql\dma-assessments.png)

    Für Azure SQL-Datenbank werden durch die Bewertungen Probleme mit der Featureparität sowie Probleme ermittelt, die eine Migration verhindern können.

    - Die Kategorie **SQL Server-Featureparität** bietet eine umfassende Reihe von Empfehlungen, in Azure verfügbaren alternativen Herangehensweisen sowie Schritte zur Risikominimierung, sodass Sie den Aufwand bei Ihren Migrationsprojekten einplanen können.
    - Die Kategorie **Kompatibilitätsprobleme** zeigt teilweise oder nicht unterstützte Features, die zu Kompatibilitätsproblemen führen können, durch die eine Migration der lokalen SQL Server-Datenbank(en) zu Azure SQL-Datenbank verhindert werden könnte. Zudem werden Empfehlungen bereitgestellt, mit deren Hilfe Sie diese Probleme behandeln können.

7.  Prüfen Sie die Bewertungsergebnisse auf die oben genannten Probleme, indem Sie die jeweiligen Optionen auswählen.

## <a name="migrate-the-sample-schema"></a>Migrieren des Beispielschemas
Nachdem Sie sich mit der Bewertung vertraut gemacht und sich davon überzeugt haben, dass die ausgewählte Datenbank ein guter Kandidat für die Migration zu Azure SQL-Datenbank ist, verwenden Sie den Datenmigrations-Assistenten zum Migrieren des Schemas zu Azure SQL-Datenbank.

> [!NOTE]
> Bevor Sie ein Migrationsprojekt im Data Migration Assistant erstellen, vergewissern Sie sich, dass Sie bereits eine Azure SQL-Datenbank bereitgestellt haben, wie in den Voraussetzungen aufgeführt. Zum Zweck dieses Tutorials lautet der Name der Azure SQL-Datenbank **AdventureWorksAzure**. Sie können sie jedoch nach Bedarf umbenennen.

Führen Sie zur Migration des **AdventureWorks2012**-Schemas zu Azure SQL-Datenbank die folgenden Schritte durch:

1.  Wählen Sie im Data Migration Assistant das Symbol „Neu (+)“ aus, und wählen Sie dann unter **Projekttyp** die Option **Migration** aus.
3.  Geben Sie einen Projektnamen im Textfeld **Source server type** (Quellservertyp) an, wählen Sie **SQL Server**, und wählen Sie dann im Textfeld **Target server type** (Zielservertyp) die Option **Azure SQL Database** (Azure SQL-Datenbank).
4.  Wählen Sie unter **Migration Scope** (Migrationsumfang) die Option **Schema only** (Nur Schema).

    Nachdem Sie die vorherigen Schritte durchgeführt haben, wird die Data Migration Assistant-Oberfläche wie in der folgenden Abbildung angezeigt:
    
    ![Erstellen eines Data Migration Assistant-Projekts](media\tutorial-sql-server-to-azure-sql\dma-create-project.png)

5.  Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.
6.  Geben Sie im Data Migration Assistant die Quellverbindungsdetails für Ihre SQL Server-Instanz an, wählen Sie **Connect** (Verbinden), und wählen Sie dann die Datenbank **AdventureWorks2012** aus.

    ![Data Migration Assistant-Quellverbindungsdetails](media\tutorial-sql-server-to-azure-sql\dma-source-connect.png)

7.  Wählen Sie **Weiter** aus. Geben Sie unter **Verbindung mit Zielserver herstellen** die Zielverbindungsdetails für die Azure SQL-Datenbank an, wählen Sie **Verbinden**, und wählen Sie dann die Datenbank **AdventureWorksAzure** aus, die Sie zuvor in Azure SQL-Datenbank bereitgestellt hatten.

    ![Data Migration Assistant-Zielverbindungsdetails](media\tutorial-sql-server-to-azure-sql\dma-target-connect.png)

8.  Wählen Sie **Weiter**, um zum Bildschirm **Select objects** (Objekte auswählen) zu gelangen. Hier können Sie die Schemaobjekte in der Datenbank **AdventureWorks2012** angeben, die in Azure SQL-Datenbank bereitgestellt werden sollen.

    Standardmäßig sind alle Objekte ausgewählt.

    ![Generieren von SQL-Skripts](media\tutorial-sql-server-to-azure-sql\dma-assessment-source.png)

9.  Wählen Sie **Generate SQL script** (SQL-Skript generieren) aus, um die SQL-Skripts zu erstellen, und prüfen Sie die Skripts dann auf Fehler.

    ![Schemaskript](media\tutorial-sql-server-to-azure-sql\dma-schema-script.png)

10. Wählen Sie **Deploy schema** (Schema bereitstellen) aus, um das Schema für Azure SQL-Datenbank bereitzustellen, und prüfen Sie den Zielserver anschließend auf Anomalien.

    ![Bereitstellen des Schemas](media\tutorial-sql-server-to-azure-sql\dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“
1. Melden Sie sich beim Azure-Portal an, und wählen Sie **Alle Dienste** und anschließend **Abonnements** aus.
 
   ![Abonnements im Portal anzeigen](media\tutorial-sql-server-to-azure-sql\portal-select-subscription1.png)
       
2. Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie auf **Ressourcenanbieter**.
 
    ![Ressourcenanbieter anzeigen](media\tutorial-sql-server-to-azure-sql\portal-select-resource-provider.png)
    
3.  Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.
 
    ![Registrieren des Ressourcenanbieters](media\tutorial-sql-server-to-azure-sql\portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Erstellen einer Instanz
1.  Wählen Sie im Azure-Portal die Option **+ Ressource erstellen**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus der Dropdownliste aus.

    ![Azure Marketplace](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)

2.  Wählen Sie auf dem Bildschirm **Azure Database Migration Service** die Schaltfläche **Erstellen** aus.
 
    ![Erstellen einer Instanz von Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-create1.png)
  
3.  Geben Sie auf dem Bildschirm **Migrationsdienst erstellen** einen Namen für den Dienst, das Abonnement und eine neue oder vorhandene Ressourcengruppe an.

4. Wählen Sie ein vorhandenes virtuelles Netzwerk (VNET) aus, oder erstellen Sie ein neues.

    Das VNET erteilt Azure Database Migration Service Zugriff auf die SQL Server-Quellinstanz und die Azure SQL-Datenbank-Zielinstanz.

    Weitere Informationen zum Erstellen des VNET im Azure-Portal finden Sie im Artikel [Erstellen eines virtuellen Netzwerks im Azure Portal](https://aka.ms/DMSVnet).

5. Wählen Sie einen Tarif.

    Weitere Informationen zu Kosten und Tarifen finden Sie in der [Preisübersicht](https://aka.ms/dms-pricing).

    Wenn Sie Hilfe bei der Wahl des richtigen Azure Database Migration Service-Tarifs benötigen, beachten Sie die Empfehlungen in [diesem Blogbeitrag](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Konfigurieren der Einstellungen einer Azure Database Migration Service-Instanz](media\tutorial-sql-server-to-azure-sql\dms-settings1.png)

6.  Wählen Sie **Erstellen**, um den Dienst zu erstellen.

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts
Nachdem der Dienst erstellt wurde, suchen Sie diesen im Azure-Portal, öffnen Sie ihn, und erstellen Sie anschließend ein neues Migrationsprojekt.

1. Wählen Sie im Azure-Portal **Alle Dienste**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus.
 
      ![Suchen aller Instanzen von Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Suchen Sie im Bildschirm **Azure Database Migration Services** nach dem Namen der von Ihnen erstellten Azure Database Migration Service-Instanz, und wählen Sie die Instanz aus.
 
     ![Suchen Ihrer Instanz von Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Wählen Sie **+ Neues Migrationsprojekt** aus.
4. Geben Sie im Bildschirm **New migration project** (Neues Migrationsprojekt) einen Projektnamen im Textfeld **Source server type** (Quellservertyp) an, wählen Sie **SQL Server**, und wählen Sie dann im Textfeld **Target server type** (Zielservertyp) die Option **Azure SQL Database** (Azure SQL-Datenbank).

    ![Erstellen eines Database Migration Service-Projekts](media\tutorial-sql-server-to-azure-sql\dms-create-project1.png)

5.  Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.

## <a name="specify-source-details"></a>Angeben von Quelldetails
1. Geben Sie im Bildschirm **Quelldetails** die Verbindungsdetails für den SQL Server-Quellinstanznamen an.
 
    Stellen Sie sicher, dass Sie einen vollständig qualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) für den SQL Server-Quellinstanznamen verwenden. Sie können auch die IP-Adresse für Situationen verwenden, in denen DNS-Namensauflösung nicht möglich ist.

2. Wenn Sie auf Ihrem Quellserver kein vertrauenswürdiges Zertifikat installiert haben, aktivieren Sie das Kontrollkästchen **Serverzertifikat vertrauen**.

    Wenn ein vertrauenswürdiges Zertifikat nicht installiert ist, erzeugt SQL Server beim Starten der Instanz ein selbstsigniertes Zertifikat. Dieses Zertifikat wird zum Verschlüsseln der Anmeldeinformationen für Clientverbindungen verwendet.

    > [!CAUTION]
    > SSL-Verbindungen, die mit einem selbstsignierten Zertifikat verschlüsselt sind, bieten keine hohe Sicherheit. Sie sind anfällig für Man-in-the-Middle-Angriffe. Sie sollten sich in einer Produktionsumgebung oder auf Servern, die mit dem Internet verbunden sind, nicht auf SSL-Verbindungen verlassen, die selbstsignierte Zertifikate verwenden.

   ![Quellendetails](media\tutorial-sql-server-to-azure-sql\dms-source-details1.png)
  
2. Wählen Sie **Save** (Speichern), und wählen Sie dann die Datenbank **AdventureWorks2012** für die Migration aus.

    ![Auswählen der Quell-DB](media\tutorial-sql-server-to-azure-sql\dms-select-source-db1.png)

## <a name="specify-target-details"></a>Angeben von Zieldetails
1. Wählen Sie **Save** (Speichern), und geben Sie dann im Bildschirm **Target details** (Zieldetails) die Verbindungsdetails für den gewünschten Azure SQL-Datenbankserver an, das heißt, für die zuvor bereitgestellte Azure SQL-Datenbank, für die das **AdventureWorks2012**-Schema über den Datenmigrations-Assistenten bereitgestellt wurde.

    ![Auswählen des Ziels](media\tutorial-sql-server-to-azure-sql\dms-select-target1.png)

2. Klicken Sie zum Speichern des Projekts auf **Save** (Speichern).

3. Überprüfen Sie auf dem Bildschirm **Projektzusammenfassung** die dem Migrationsprojekt zugeordneten Details.

    ![DMS-Zusammenfassung](media\tutorial-sql-server-to-azure-sql\dms-summary1.png)

4. Wählen Sie **Speichern**aus.

## <a name="run-the-migration"></a>Ausführen der Migration
1.  Wählen Sie das kürzlich gespeicherte Projekt, **+ Neue Aktivität** und dann **Migration ausführen** aus.

    ![Neue Aktivität](media\tutorial-sql-server-to-azure-sql\dms-new-activity1.png)

2.  Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für die Quell- und Zielserver ein, und wählen Sie dann **Save** (Speichern).

3.  Ordnen Sie im Bildschirm **Map to target databases** (Zu Zieldatenbanken zuordnen) die Quell- und die Zieldatenbank für die Migration einander zu.

    Wenn die Zieldatenbank denselben Datenbanknamen wie die Quelldatenbank enthält, wählt Azure DMS die Zieldatenbank standardmäßig aus.

    ![Zuordnen zu Zieldatenbanken](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity1.png)

4. Wählen Sie **Save** (Speichern) im Bildschirm **Select tables** (Tabellen auswählen) aus, erweitern Sie die Tabellenauflistung, und prüfen Sie die Liste betroffener Felder.

    Beachten Sie, dass Azure Database Migration Service automatisch alle leeren Quelltabellen auswählt, die auf der gewünschten Azure SQL-Datenbankinstanz vorhanden sind. Wenn Sie Tabellen, die bereits Daten enthalten, erneut migrieren möchten, müssen Sie die Tabellen auf diesem Blatt explizit auswählen.

    ![Auswählen von Tabellen](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity1.png)

5.  Wählen Sie **Save** (Speichern) im Bildschirm **Migration summary** (Migrationszusammenfassung) aus, und geben Sie im Textfeld **Activity name** (Aktivitätsname) einen Namen für die Migrationsaktivität an.

6. Erweitern Sie den Abschnitt **Überprüfungsoption**, um den Bildschirm **Überprüfungsoption auswählen** anzuzeigen. Geben Sie an, ob für die migrierte Datenbank ein Schemavergleich sowie eine Überprüfung auf Datenkonsistenz und Abfragerichtigkeit durchgeführt werden soll.
    
    ![Auswählen der Validierungsoption](media\tutorial-sql-server-to-azure-sql\dms-configuration1.png)

6.  Wählen Sie **Save** (Speichern), und prüfen Sie die Zusammenfassung, um sicherzustellen, dass die Quell- und die Zieldetails Ihren vorherigen Angaben entsprechen.

    ![Migrationszusammenfassung](media\tutorial-sql-server-to-azure-sql\dms-run-migration1.png)

7.  Wählen Sie **Migration ausführen** aus.

    Das Fenster „Migrationsaktivität“ wird angezeigt, und der **Status** der Aktivität lautet **Ausstehend**.

    ![Aktivitätsstatus](media\tutorial-sql-server-to-azure-sql\dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Überwachen der Migration
1. Wählen Sie auf dem Bildschirm „Migrationsaktivität“ die Option **Aktualisieren** aus, um die Anzeige so lange zu aktualisieren, bis Sie sehen, dass der **Status** der Migration **Abgeschlossen** lautet.

    ![Aktivitätsstatus „Abgeschlossen“](media\tutorial-sql-server-to-azure-sql\dms-completed-activity1.png)

2. Wählen Sie nach Abschluss der Migration die Option **Bericht herunterladen** aus, um einen Bericht abzurufen, in dem die Details zum Migrationsprozess aufgeführt werden.

3. Überprüfen Sie die Zieldatenbank(en) in der Azure SQL-Zieldatenbank.

### <a name="additional-resources"></a>Zusätzliche Ressourcen

 * [SQL-Migration mit Azure Data Migration Service (DMS)](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587) – praktische Übungseinheit.