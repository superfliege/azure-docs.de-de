---
title: Verwenden von Azure Database Migration Service zur Migration von SQL Server zur verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
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
ms.date: 02/28/2018
ms.openlocfilehash: d74a273061912ea2bdcc39301ce9a727b07ade41
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance"></a>Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz
Mit Azure Database Migration Service können Sie die Datenbanken einer lokalen SQL Server-Instanz zu Azure SQL-Datenbank migrieren. In diesem Tutorial migrieren Sie die Datenbank **Adventureworks2012** von einer lokalen SQL Server-Instanz mithilfe von Azure Database Migration Service zu Azure SQL-Datenbank.

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
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die SQL Server-Quellinstanz zugreifen kann.
- Wenn Sie eine Firewallappliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration sowie auf Dateien über SMB-Port 445 zugreifen kann.
- Erstellen Sie eine verwaltete Azure SQL-Datenbank-Instanz, indem Sie die Anweisungen im Artikel [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz im Azure-Portal](https://aka.ms/sqldbmi) befolgen.
- Stellen Sie sicher, dass die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der verwalteten Zielinstanz verwendeten Anmeldungen Mitglieder der sysadmin-Serverrolle sind.
- Erstellen Sie eine Netzwerkfreigabe, mit der Azure Database Migration Service die Quelldatenbank sichern kann.
- Stellen Sie sicher, dass das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für die von Ihnen erstellte Netzwerkfreigabe verfügt.
- Notieren Sie sich einen Windows-Benutzer (mit dem Kennwort), der eine Berechtigung für Vollzugriff auf die von Ihnen soeben erstellte Netzwerkfreigabe besitzt. Azure Database Migration Service nimmt die Anmeldeinformationen des Benutzers an, um die Sicherungsdateien für Wiederherstellungsvorgänge in den Azure Storage-Container hochzuladen.
- Erstellen Sie entsprechend der Schritte im Artikel [Verwalten von Azure Blob Storage-Ressourcen mit dem Storage-Explorer (Vorschauversion)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) einen Blobcontainer, und rufen Sie den zugehörigen SAS-URI ab. Wählen Sie beim Erstellen des SAS-URI im Richtlinienfenster alle Berechtigungen aus (Lesen, Schreiben, Löschen, Auflisten). Hierdurch erteilen Sie Azure Database Migration Service Zugriff auf Ihren Speicherkontocontainer zum Hochladen der Sicherungsdateien, die für die Migration von Datenbanken zur verwalteten Azure SQL-Datenbank-Instanz verwendet werden.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“

1.  Melden Sie sich beim Azure-Portal an, und wählen Sie **Alle Dienste** und anschließend **Abonnements** aus.
![Anzeigen von Abonnements im Portal](media\tutorial-sql-server-to-managed-instance\portal-select-subscription.png)

1.  Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie auf **Ressourcenanbieter**.
![Anzeigen von Ressourcenanbietern](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

1.  Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.
![Registrieren von Ressourcenanbietern](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Erstellen einer Instanz

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
    |**Serversicherungsspeicherort** | Die lokale Netzwerkfreigabe, auf die Azure Database Migration Service die Quelldatenbanksicherungen übertragen kann. Das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, muss über Schreibberechtigungen für diese Netzwerkfreigabe verfügen. |
    |**Benutzername** | Der Windows-Benutzername, den Azure Database Migration Service annehmen und mit dem Azure Database Migration Service die Sicherungsdateien für Wiederherstellungsvorgänge in den Azure Storage-Container hochladen kann. |
    |**Kennwort** | Das Kennwort für den oben genannten Benutzer. |
    |**Speicher-SAS-URI** | Der SAS-URI, der Azure Database Migration Service Zugriff auf Ihren Speicherkontocontainer erteilt. In diesen werden die Sicherungsdateien hochgeladen, und er wird für die Migration von Datenbanken zur verwalteten Azure SQL-Datenbank-Instanz verwendet.  [Erfahren Sie, wie der SAS-URI für Blobcontainer abgerufen wird](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Konfigurieren von Migrationseinstellungen](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings.png)

1.  Wählen Sie **Speichern** im Bildschirm „Migrationszusammenfassung“ aus, und geben Sie im Textfeld **Aktivitätsname** einen Namen für die Migrationsaktivität an.

    ![Migrationszusammenfassung](media\tutorial-sql-server-to-managed-instance\dms-migration-summary.png)


## <a name="monitor-the-migration"></a>Überwachen der Migration

1.  Wählen Sie die Migrationsaktivität aus, um den Status der Aktivität zu prüfen.

1.  Überprüfen Sie nach Abschluss der Migration die Zieldatenbanken für die verwaltete Azure SQL-Datenbank-Zielinstanz.

    ![Überwachen der Migration](media\tutorial-sql-server-to-managed-instance\dms-monitor-migration.png)

