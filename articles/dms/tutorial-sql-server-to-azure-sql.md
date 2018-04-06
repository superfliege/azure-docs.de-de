---
title: Verwenden von Azure Database Migration Service zur Migration von SQL Server zu Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure Database Migration Service eine Migration von einer lokalen SQL Server-Instanz nach Azure SQL durchführen.
services: dms
author: HJToland3
ms.author: jtoland
manager: jhubbard
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 03/29/2018
ms.openlocfilehash: b16c3666b932beb771c51bb8dec3ebd5fa36e8a0
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="migrate-sql-server-to-azure-sql-database"></a>Migrieren von SQL Server zu Azure SQL-Datenbank
Mit Azure Database Migration Service können Sie die Datenbanken einer lokalen SQL Server-Instanz zu Azure SQL-Datenbank migrieren. In diesem Tutorial migrieren Sie die Datenbank **Adventureworks2012**, die in einer lokalen Instanz von SQL Server 2016 (oder höher) wiederhergestellt wurde, mithilfe von Azure Database Migration Service zu einer Azure SQL-Datenbank.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Bewerten Ihrer lokalen Datenbank mithilfe des Data Migration Assistant
> * Migrieren des Beispielschemas über den Data Migration Assistant
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen Sie ein Migrationsprojekt mithilfe von Azure Database Migration Service.
> * Ausführen der Migration
> * Überwachen der Migration

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

- Laden Sie [SQL Server 2016 oder höher](https://www.microsoft.com/sql-server/sql-server-downloads) (beliebige Edition) herunter, und installieren Sie es.
- Aktivieren Sie das TCP/IP-Protokoll (dieses wird während der SQL Server Express-Installation standardmäßig deaktiviert), indem Sie die Anweisungen im Artikel [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) befolgen.
- Erstellen Sie eine Azure SQL-Datenbankinstanz, indem Sie die Anweisungen im Artikel [Erstellen einer Azure SQL-Datenbank im Azure-Portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) befolgen.
- Laden Sie den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595), Version 3.3 oder höher, herunter, und installieren Sie ihn.
- Erstellen Sie ein VNET für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bietet.
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln Ihres Azure Virtual Network (VNET) nicht die Kommunikationsports 443, 53, 9354, 445 und 12000 blockieren. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Konfigurieren Sie Ihre [Windows-Firewall für Datenbank-Engine-Zugriff](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die SQL Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 1433).
- Bei der Ausführung mehrerer benannter SQL Server-Instanzen mit dynamischen Ports wird empfohlen, den SQL-Browser-Dienst zu aktivieren und den Zugriff auf UDP-Port 1434 durch die Firewalls zuzulassen, sodass Azure Database Migration Service eine Verbindung mit einer benannten Instanz auf Ihrem Quellserver herstellen kann.
- Wenn Sie eine Firewall-Appliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration zugreifen kann.
- Erstellen Sie für Azure SQL-Datenbankserver eine [Firewallregel](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) auf Serverebene, um den Zugriff auf die Zieldatenbanken durch Azure Database Migration Service zu ermöglichen. Geben Sie den Subnetzbereich des für Azure Database Migration Service verwendeten VNET an.
- Stellen Sie sicher, dass die für die Verbindung mit der SQL Server-Quellinstanz verwendeten Anmeldeinformationen über [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql)-Berechtigungen verfügen.
- Stellen Sie sicher, dass die für die Verbindung mit der Azure SQL-Zieldatenbankinstanz verwendeten Anmeldeinformationen die Berechtigung CONTROL DATABASE für die Azure SQL-Zieldatenbanken besitzen.

## <a name="assess-your-on-premises-database"></a>Bewerten Ihrer lokalen Datenbank
Damit Sie Daten von einer lokalen SQL Server-Instanz zu Azure SQL-Datenbank migrieren können, müssen Sie die SQL Server-Datenbank auf Probleme untersuchen, die eine Migration möglicherweise verhindern könnten. Führen Sie im Data Migration Assistant, Version 3.3 oder höher, die im Artikel [Durchführen einer SQL Server-Migrationsbewertung](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) beschriebenen Schritte aus, um die Bewertung der lokalen Datenbank abzuschließen. Hier sehen Sie eine Zusammenfassung der erforderlichen Schritte:
1.  Wählen Sie im Data Migration Assistant das Symbol „New (+)“ (Neu), und wählen Sie dann den Projekttyp **Assessment** (Bewertung) aus.
2.  Geben Sie einen Projektnamen im Textfeld **Source server type** (Quellservertyp) an, wählen Sie **SQL Server**, und wählen Sie dann im Textfeld **Target server type** (Zielservertyp) die Option **Azure SQL Database** (Azure SQL-Datenbank).
3.  Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.

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

7.  Prüfen Sie die Bewertungsergebnisse auf die oben genannten Probleme, indem Sie die jeweiligen Optionen auswählen.
    - Die Kategorie **SQL Server-Featureparität** bietet eine umfassende Reihe von Empfehlungen, in Azure verfügbaren alternativen Herangehensweisen sowie Schritte zur Risikominimierung, sodass Sie den Aufwand bei Ihren Migrationsprojekten einplanen können.
    - Die Kategorie **Kompatibilitätsprobleme** zeigt teilweise oder nicht unterstützte Features, die zu Kompatibilitätsproblemen führen können, durch die eine Migration der lokalen SQL Server-Datenbank(en) zu Azure SQL-Datenbank verhindert werden könnte. Zudem werden Empfehlungen bereitgestellt, mit deren Hilfe Sie diese Probleme behandeln können.


## <a name="migrate-the-sample-schema"></a>Migrieren des Beispielschemas
Nachdem Sie sich mit der Bewertung vertraut gemacht und sich davon überzeugt haben, dass die ausgewählte Datenbank ein guter Kandidat für die Migration zu Azure SQL-Datenbank ist, verwenden Sie den Data Migration Assistant zum Migrieren des Schemas zu Azure SQL-Datenbank.

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
 
   ![Abonnements im Portal anzeigen](media\tutorial-sql-server-to-azure-sql\portal-select-subscription.png)
       
2. Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie auf **Ressourcenanbieter**.
 
    ![Ressourcenanbieter anzeigen](media\tutorial-sql-server-to-azure-sql\portal-select-resource-provider.png)    
3.  Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.
 
    ![Registrieren des Ressourcenanbieters](media\tutorial-sql-server-to-azure-sql\portal-register-resource-provider.png)    


## <a name="create-an-instance"></a>Erstellen einer Instanz
1.  Wählen Sie im Azure-Portal die Option **+ Ressource erstellen**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus der Dropdownliste aus.

    ![Azure Marketplace](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)
2.  Wählen Sie im Bildschirm **Azure Database Migration Service (Vorschau)** die Schaltfläche **Erstellen**.
 
    ![Erstellen einer Instanz von Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-create.png)
  
3.  Geben Sie im Bildschirm **Database Migration Service** einen Namen für den Dienst, das Abonnement, ein virtuelles Netzwerk und den Tarif an.

    Weitere Informationen zu Kosten und Tarifen finden Sie in der [Preisübersicht](https://aka.ms/dms-pricing).

     ![Konfigurieren der Einstellungen einer Azure Database Migration Service-Instanz](media\tutorial-sql-server-to-azure-sql\dms-settings.png)

4.  Wählen Sie **Erstellen**, um den Dienst zu erstellen.

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts
Nachdem der Dienst erstellt wurde, suchen Sie danach im Azure-Portal, und erstellen Sie dann ein Migrationsprojekt.
1. Wählen Sie im Azure-Portal **Alle Dienste**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus.
 
      ![Suchen aller Instanzen von Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-search.png)
2. Suchen Sie im Bildschirm **Azure Database Migration Services** nach dem Namen der von Ihnen erstellten Azure DMS-Instanz, und wählen Sie die Instanz aus.
 
     ![Suchen Ihrer Instanz von Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Wählen Sie **+ New Migration Project** (Neues Migrationsprojekt) aus.
4. Geben Sie im Bildschirm **New migration project** (Neues Migrationsprojekt) einen Projektnamen im Textfeld **Source server type** (Quellservertyp) an, wählen Sie **SQL Server**, und wählen Sie dann im Textfeld **Target server type** (Zielservertyp) die Option **Azure SQL Database** (Azure SQL-Datenbank).

    ![Erstellen eines Database Migration Service-Projekts](media\tutorial-sql-server-to-azure-sql\dms-create-project.png)

5.  Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.


## <a name="specify-source-details"></a>Angeben von Quelldetails
1. Geben Sie im Bildschirm **Source details** (Quelldetails) die Verbindungsdetails für die SQL Server-Quellinstanz an.

    ![Auswählen der Quelle](media\tutorial-sql-server-to-azure-sql\dms-select-source.png)

2. Wählen Sie **Save** (Speichern), und wählen Sie dann die Datenbank **AdventureWorks2012** für die Migration aus.

    ![Auswählen der Quell-DB](media\tutorial-sql-server-to-azure-sql\dms-select-source-db.png)

## <a name="specify-target-details"></a>Angeben von Zieldetails
1. Wählen Sie **Save** (Speichern), und geben Sie dann im Bildschirm **Target details** (Zieldetails) die Verbindungsdetails für das Ziel an, das heißt, für die zuvor bereitgestellte Azure SQL-Datenbank, für die das **AdventureWorks2012**-Schema über den Data Migration Assistant bereitgestellt wurde.

    ![Auswählen des Ziels](media\tutorial-sql-server-to-azure-sql\dms-select-target.png)

2. Klicken Sie zum Speichern des Projekts auf **Save** (Speichern).
3. Überprüfen Sie den Bildschirm **Migration summary** (Migrationszusammenfassung), und überprüfen Sie die dem Migrationsprojekt zugeordneten Details.

    ![DMS-Zusammenfassung](media\tutorial-sql-server-to-azure-sql\dms-summary.png)

4. Wählen Sie **Speichern**aus.

## <a name="run-the-migration"></a>Ausführen der Migration
1.  Wählen Sie das kürzlich gespeicherte Projekt, wählen Sie **+ New Activity** (Neue Aktivität), und wählen Sie dann **Run Data Migration** (Datenmigration ausführen).

    ![Neue Aktivität](media\tutorial-sql-server-to-azure-sql\dms-new-activity.png)

2.  Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für die Quell- und Zielserver ein, und wählen Sie dann **Save** (Speichern).
3.  Ordnen Sie im Bildschirm **Map to target databases** (Zu Zieldatenbanken zuordnen) die Quell- und die Zieldatenbank für die Migration einander zu.

    Wenn die Zieldatenbank denselben Datenbanknamen wie die Quelldatenbank enthält, wählt Azure DMS die Zieldatenbank standardmäßig aus.

    ![Zuordnen zu Zieldatenbanken](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity.png)

4. Wählen Sie **Save** (Speichern) im Bildschirm **Select tables** (Tabellen auswählen) aus, erweitern Sie die Tabellenauflistung, und prüfen Sie die Liste betroffener Felder.

    ![Auswählen von Tabellen](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity.png)

5.  Wählen Sie **Save** (Speichern) im Bildschirm **Migration summary** (Migrationszusammenfassung) aus, und geben Sie im Textfeld **Activity name** (Aktivitätsname) einen Namen für die Migrationsaktivität an.

    In diesem Bildschirm können Sie auch den Bildschirm **Choose validation option** (Validierungsoption auswählen) erweitern. In diesem können Sie die Kriterien angeben, nach denen die migrierte Datenbank validiert werden soll:
    - Schema
    - Datenkonsistenz
    - Richtigkeit und Leistung der Abfrage

    ![Auswählen der Validierungsoption](media\tutorial-sql-server-to-azure-sql\dms-configuration.png)

6.  Wählen Sie **Save** (Speichern), und prüfen Sie die Zusammenfassung, um sicherzustellen, dass die Quell- und die Zieldetails Ihren vorherigen Angaben entsprechen.

    ![Migrationszusammenfassung](media\tutorial-sql-server-to-azure-sql\dms-run-migration.png)

7.  Wählen Sie **Run migration** (Migration ausführen), um die Migrationsaktivität zu starten, und wählen Sie dann **Refresh** (Aktualisieren), um den aktuellen Status zu prüfen.

    ![Aktivitätsstatus](media\tutorial-sql-server-to-azure-sql\dms-activity-status.png)

## <a name="monitor-the-migration"></a>Überwachen der Migration
1. Wählen Sie die Migrationsaktivität aus, um den Status der Aktivität zu prüfen.
2. Stellen Sie sicher, dass die Azure SQL-Zieldatenbank nach der Migration vollständig ist.

    ![Abgeschlossen](media\tutorial-sql-server-to-azure-sql\dms-completed-activity.png)
