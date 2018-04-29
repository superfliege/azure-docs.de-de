---
title: Verwenden von DMS, um zu einer verwalteten Azure SQL-Datenbank-Instanz zu migrieren | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure Database Migration Service eine Migration von einer lokalen SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz durchführen.
services: dms
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 04/10/2018
ms.openlocfilehash: 6628ea218c4c7a9aacc0c2899c1ea4e5b6169b51
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-using-dms"></a>Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz mithilfe von DMS
Mit Azure Database Migration Service können Sie bei einer Ausfallzeit von fast null die Datenbanken aus einer lokalen SQL Server-Instanz zu einer [verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance.md) migrieren. Informationen zu weiteren Methoden, für die etwas Ausfallzeit benötigt wird, finden Sie unter [Migration einer SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-migrate.md).

In diesem Tutorial migrieren Sie die Datenbank **Adventureworks2012** von einer lokalen SQL Server-Instanz mithilfe von Azure Database Migration Service zu Azure SQL-Datenbank.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen Sie ein Migrationsprojekt mithilfe von Azure Database Migration Service.
> * Ausführen der Migration
> * Überwachen der Migration

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

- Erstellen Sie ein VNET für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bietet. [Lernen Sie die Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service kennen](https://aka.ms/dmsnetworkformi).
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln Ihres Azure Virtual Network (VNET) nicht die Kommunikationsports 443, 53, 9354, 445 und 12000 blockieren. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Konfigurieren Sie Ihre [Windows-Firewall für Quelldatenbank-Engine-Zugriff](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die SQL Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 1433).
- Bei der Ausführung mehrerer benannter SQL Server-Instanzen mit dynamischen Ports empfiehlt es sich, den SQL-Browser-Dienst zu aktivieren und den Zugriff auf den UDP-Port 1434 durch Ihre Firewalls zuzulassen, sodass Azure Database Migration Service eine Verbindung mit einer benannten Instanz auf Ihrem Quellserver herstellen kann.
- Wenn Sie eine Firewallappliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration sowie auf Dateien über SMB-Port 445 zugreifen kann.
- Erstellen Sie eine verwaltete Azure SQL-Datenbank-Instanz, indem Sie die Anweisungen im Artikel [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz im Azure-Portal](https://aka.ms/sqldbmi) befolgen.
- Stellen Sie sicher, dass die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der verwalteten Zielinstanz verwendeten Anmeldungen Mitglieder der sysadmin-Serverrolle sind.
- Erstellen Sie eine Netzwerkfreigabe, mit der Azure Database Migration Service die Quelldatenbank sichern kann.
- Stellen Sie sicher, dass das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für die von Ihnen erstellte Netzwerkfreigabe verfügt.
- Notieren Sie sich einen Windows-Benutzer (mit dem Kennwort), der eine Berechtigung für Vollzugriff auf die von Ihnen soeben erstellte Netzwerkfreigabe besitzt. Azure Database Migration Service verwendet die Benutzeranmeldeinformationen, um die Sicherungsdateien für Wiederherstellungsvorgänge in den Azure Storage-Container hochzuladen.
- Erstellen Sie entsprechend der Schritte im Artikel [Verwalten von Azure Blob Storage-Ressourcen mit dem Storage-Explorer](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) einen Blobcontainer, und rufen Sie den zugehörigen SAS-URI ab. Wählen Sie beim Erstellen des SAS-URI im Richtlinienfenster alle Berechtigungen aus (Lesen, Schreiben, Löschen, Auflisten). Hierdurch wird Azure Database Migration Service Zugriff auf Ihren Speicherkontocontainer gewährt, um die Sicherungsdateien hochladen zu können, die für die Migration von Datenbanken zur verwalteten Azure SQL-Datenbank-Instanz verwendet werden.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“

1.  Melden Sie sich beim Azure-Portal an, und wählen Sie **Alle Dienste** und anschließend **Abonnements** aus.
![Anzeigen von Abonnements im Portal](media\tutorial-sql-server-to-managed-instance\portal-select-subscription.png)

1.  Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie auf **Ressourcenanbieter**.
![Anzeigen von Ressourcenanbietern](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

1.  Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.
![Registrieren von Ressourcenanbietern](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)    

## <a name="create-an-azure-database-migration-service-instance"></a>Erstellen einer Instanz von Azure Database Migration Service

1.  Wählen Sie im Azure-Portal die Option **+ Ressource erstellen**, suchen Sie nach **Azure Database Migration Service**, und wählen Sie dann **Azure Database Migration Service** aus der Dropdownliste aus.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

1.  Wählen Sie im Bildschirm **Azure Database Migration Service (Vorschau)** die Schaltfläche **Erstellen**.

    ![Erstellen einer Instanz von Azure Database Migration Service](media\tutorial-sql-server-to-managed-instance\dms-create.png)

1.  Geben Sie im Bildschirm **Database Migration Service** einen Namen für den Dienst, das Abonnement, die Ressourcengruppe, ein virtuelles Netzwerk und den Tarif an.

    Weitere Informationen zu Kosten und Tarifen finden Sie in der [Preisübersicht](https://aka.ms/dms-pricing). *Azure Database Migration Service ist derzeit in der Vorschauversion verfügbar, und für die Nutzung fallen keine Kosten an.*

    **Netzwerk**: Wählen Sie ein vorhandenes Netzwerk aus, oder erstellen Sie ein neues VNET, das Azure Database Migration Service Zugriff auf die SQL Server-Quellinstanz und die verwaltete Azure SQL-Datenbank-Zielinstanz erteilt. [Lernen Sie die Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service kennen](https://aka.ms/dmsnetworkformi).

    Weitere Informationen zum Erstellen des VNET im Azure-Portal finden Sie unter [Erstellen eines virtuellen Netzwerks im Azure Portal](https://aka.ms/DMSVnet).

    ![Erstellen eines DMS-Diensts](media\tutorial-sql-server-to-managed-instance\dms-create-service.png)

1.  Wählen Sie **Erstellen**, um den Dienst zu erstellen.


## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts

Nachdem der Dienst erstellt wurde, suchen Sie danach im Azure-Portal, und öffnen Sie ihn.

1.  Wählen Sie **+ New Migration Project** (Neues Migrationsprojekt) aus.

1.  Geben Sie im Bildschirm **Neues Migrationsprojekt** einen Projektnamen im Textfeld **Quellservertyp** an, und wählen Sie **SQL Server** und dann im Textfeld **Zielservertyp** die Option **Verwaltete Azure SQL-Datenbank-Instanz** aus.

    ![Erstellen eines DMS-Projekts](media\tutorial-sql-server-to-managed-instance\dms-create-project.png)

1.  Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.

## <a name="specify-source-details"></a>Angeben von Quelldetails

1.  Geben Sie im Bildschirm **Source details** (Quelldetails) die Verbindungsdetails für die SQL Server-Quellinstanz an.

    ![Quellendetails](media\tutorial-sql-server-to-managed-instance\dms-source-details.png)

1.  Wählen Sie **Speichern** und dann die Datenbank **Adventureworks2012** für die Migration aus.

    ![Auswählen von Quelldatenbanken](media\tutorial-sql-server-to-managed-instance\dms-source-database.png)

## <a name="specify-target-details"></a>Angeben von Zieldetails

1.  Wählen Sie **Speichern** aus, und geben Sie dann im Bildschirm **Zieldetails** die Verbindungsdetails für das Ziel an, d.h. für die zuvor bereitgestellte verwaltete Azure SQL-Datenbank-Instanz, zu der die **AdventureWorks2012**-Datenbank migriert wird.

    ![Auswählen des Ziels](media\tutorial-sql-server-to-managed-instance\dms-target-details.png)

1.  Wählen Sie **Speichern**aus.

1.  Überprüfen Sie auf dem Bildschirm **Projektzusammenfassung** die dem Migrationsprojekt zugeordneten Details.

## <a name="run-the-migration"></a>Ausführen der Migration

1.  Wählen Sie das kürzlich gespeicherte Projekt, **+ Neue Aktivität** und dann **Migration ausführen** aus.

    ![Erstellen einer neuen Aktivität](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity.png)

1.  Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen der Quell- und Zielserver ein, und wählen Sie dann **Speichern** aus.

1.  Wählen Sie auf dem Bildschirm **Zu Zieldatenbanken zuordnen** die Quelldatenbanken aus, die Sie migrieren möchten.

    ![Auswählen von Quelldatenbanken](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases.png)

1.  Wählen Sie auf dem Bildschirm **Migrationseinstellungen konfigurieren** die Option **Speichern** aus, und geben Sie die folgenden Details an:

    | | |
    |--------|---------|
    |**Speicherort für die Serversicherung** | Die lokale Netzwerkfreigabe, auf die Azure Database Migration Service die Quelldatenbanksicherungen übertragen kann. Das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, muss über Schreibberechtigungen für diese Netzwerkfreigabe verfügen. |
    |**Benutzername** | Der Windows-Benutzername, den Azure Database Migration Service annehmen und mit dem Azure Database Migration Service die Sicherungsdateien für Wiederherstellungsvorgänge in den Azure Storage-Container hochladen kann. |
    |**Kennwort** | Das Kennwort für den oben genannten Benutzer. |
    |**Speicher-SAS-URI** | Der SAS-URI, der Azure Database Migration Service Zugriff auf Ihren Speicherkontocontainer gewährt, in den der Dienst die Sicherungsdateien hochlädt und der für die Migration von Datenbanken zur verwalteten Azure SQL-Datenbank-Instanz verwendet wird. [Erfahren Sie, wie der SAS-URI für Blobcontainer abgerufen wird](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Konfigurieren von Migrationseinstellungen](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings.png)

1.  Wählen Sie **Speichern** im Bildschirm „Migrationszusammenfassung“ aus, und geben Sie im Textfeld **Aktivitätsname** einen Namen für die Migrationsaktivität an.

    ![Migrationszusammenfassung](media\tutorial-sql-server-to-managed-instance\dms-migration-summary.png)


## <a name="monitor-the-migration"></a>Überwachen der Migration

1.  Wählen Sie die Migrationsaktivität aus, um den Status der Aktivität zu prüfen.

1.  Überprüfen Sie nach Abschluss der Migration die Zieldatenbanken für die verwaltete Azure SQL-Datenbank-Zielinstanz.

    ![Überwachen der Migration](media\tutorial-sql-server-to-managed-instance\dms-monitor-migration.png)

## <a name="next-steps"></a>Nächste Schritte

- Ein Tutorial, in dem gezeigt wird, wie Sie eine Datenbank mit dem T-SQL-Befehl RESTORE zu einer verwalteten Instanz migrieren können, finden Sie unter [Restore a database backup to an Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Informationen zum Importieren einer Datenbank aus einer BACPAC-Datei finden Sie unter [Importieren einer BACPAC-Datei in eine neue Azure SQL-Datenbank](../sql-database/sql-database-import.md).
- Informationen zur verwalteten Instanz finden Sie unter [Was ist eine verwaltete Instanz?](../sql-database/sql-database-managed-instance.md)
- Informationen zum Verbinden von Apps mit einer verwalteten Instanz finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-connect-app.md).
