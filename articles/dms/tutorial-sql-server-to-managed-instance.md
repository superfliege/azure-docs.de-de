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
ms.date: 07/12/2018
ms.openlocfilehash: c911b096af6662e11afb4c4262b92c239d252c36
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990226"
---
# <a name="migrate-sql-server-to-azure-sql-database-managed-instance-using-dms"></a>Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz mithilfe von DMS
Mit Azure Database Migration Service können Sie die Datenbanken aus einer lokalen SQL Server-Instanz zu einer [verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance.md) migrieren. Informationen zu weiteren Methoden, für die etwas manueller Aufwand erforderlich ist, finden Sie im Artikel [Migration einer SQL Server-Instanz zu einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-migrate.md).

> [!IMPORTANT]
> Migrationsprojekte von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz sind in der Vorschauversion enthalten und unterliegen den [ergänzenden Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Tutorial migrieren Sie die Datenbank **Adventureworks2012** von einer lokalen SQL Server-Instanz mithilfe von Azure Database Migration Service zu einer verwalteten Azure SQL-Datenbank-Instanz.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen Sie ein Migrationsprojekt mithilfe von Azure Database Migration Service.
> * Ausführen der Migration
> * Überwachen der Migration
> * Laden Sie einen Migrationsbericht herunter.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

- Erstellen Sie ein VNET für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bietet. [Lernen Sie die Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service kennen](https://aka.ms/dmsnetworkformi).
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln Ihres Azure Virtual Network (VNET) nicht die Kommunikationsports 443, 53, 9354, 445 und 12000 blockieren. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Konfigurieren Sie Ihre [Windows-Firewall für Quelldatenbank-Engine-Zugriff](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die SQL Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 1433).
- Bei der Ausführung mehrerer benannter SQL Server-Instanzen mit dynamischen Ports empfiehlt es sich, den SQL-Browser-Dienst zu aktivieren und den Zugriff auf den UDP-Port 1434 durch Ihre Firewalls zuzulassen, sodass Azure Database Migration Service eine Verbindung mit einer benannten Instanz auf Ihrem Quellserver herstellen kann.
- Wenn Sie eine Firewallappliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration sowie auf Dateien über SMB-Port 445 zugreifen kann.
- Erstellen Sie eine verwaltete Azure SQL-Datenbank-Instanz, indem Sie die Anweisungen im Artikel [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz im Azure-Portal](https://aka.ms/sqldbmi) befolgen.
- Stellen Sie sicher, dass die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der verwalteten Zielinstanz verwendeten Anmeldungen Mitglieder der sysadmin-Serverrolle sind.
- Erstellen Sie eine Netzwerkfreigabe, mit der Azure Database Migration Service die Quelldatenbank sichern kann.
- Stellen Sie sicher, dass das Dienstkonto, unter dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für die Netzwerkfreigabe verfügt, die Sie erstellt haben, und dass das Computerkonto für den Quellserver über Lese-/Schreibzugriff auf dieselbe Freigabe verfügt.
- Notieren Sie sich einen Windows-Benutzer (mit Kennwort), der Vollzugriff auf die von Ihnen zuvor erstellte Netzwerkfreigabe hat. Azure Database Migration Service verwendet die Benutzeranmeldeinformationen, um die Sicherungsdateien für Wiederherstellungsvorgänge in den Azure Storage-Container hochzuladen.
- Erstellen Sie entsprechend der Schritte im Artikel [Verwalten von Azure Blob Storage-Ressourcen mit dem Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) einen Blobcontainer, und rufen Sie den zugehörigen SAS-URI ab. Wählen Sie beim Erstellen des SAS-URI im Richtlinienfenster alle Berechtigungen aus (Lesen, Schreiben, Löschen, Auflisten). Durch dieses Detail wird Azure Database Migration Service Zugriff auf Ihren Speicherkontocontainer gewährt, um die Sicherungsdateien hochladen zu können, die für die Migration von Datenbanken zur verwalteten Azure SQL-Datenbank-Instanz verwendet werden

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“

1. Melden Sie sich beim Azure-Portal an, und wählen Sie **Alle Dienste** und anschließend **Abonnements** aus.

    ![Abonnements im Portal anzeigen](media\tutorial-sql-server-to-managed-instance\portal-select-subscriptions.png)        

2. Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie auf **Ressourcenanbieter**.

    ![Ressourcenanbieter anzeigen](media\tutorial-sql-server-to-managed-instance\portal-select-resource-provider.png)

3. Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.

    ![Registrieren des Ressourcenanbieters](media\tutorial-sql-server-to-managed-instance\portal-register-resource-provider.png)   

## <a name="create-an-azure-database-migration-service-instance"></a>Erstellen einer Instanz von Azure Database Migration Service

1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen**, suchen Sie nach **Azure Database Migration Service**, und wählen Sie anschließend **Azure Database Migration Service** aus der Dropdownliste aus.

     ![Azure Marketplace](media\tutorial-sql-server-to-managed-instance\portal-marketplace.png)

2. Wählen Sie auf dem Bildschirm **Azure Database Migration Service** die Schaltfläche **Erstellen** aus.

    ![Erstellen einer Instanz von Azure Database Migration Service](media\tutorial-sql-server-to-managed-instance\dms-create1.png)

3. Geben Sie auf dem Bildschirm **Migrationsdienst erstellen** einen Namen für den Dienst, das Abonnement und eine neue oder vorhandene Ressourcengruppe an.

4. Wählen Sie ein vorhandenes virtuelles Netzwerk (VNET) aus oder erstellen Sie eins.
 
    Das VNET erteilt Azure Database Migration Service Zugriff auf die SQL Server-Quellinstanz und die verwaltete Azure SQL-Datenbank-Zielinstanz.

    Weitere Informationen zum Erstellen des VNET im Azure-Portal finden Sie im Artikel [Erstellen eines virtuellen Netzwerks im Azure Portal](https://aka.ms/DMSVnet).

    Weitere Einzelheiten finden Sie im Artikel [Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service](https://aka.ms/dmsnetworkformi).

5. Wählen Sie einen Tarif.

    Weitere Informationen zu Kosten und Tarifen finden Sie in der [Preisübersicht](https://aka.ms/dms-pricing).
   
    ![Erstellen eines DMS-Diensts](media\tutorial-sql-server-to-managed-instance\dms-create-service1.png)

6.  Wählen Sie **Erstellen**, um den Dienst zu erstellen.

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts

Nachdem der Dienst erstellt wurde, suchen Sie diesen im Azure-Portal, öffnen Sie ihn, und erstellen Sie anschließend ein neues Migrationsprojekt.

1. Wählen Sie im Azure-Portal **Alle Dienste**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus.

    ![Suchen aller Instanzen von Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Suchen Sie auf dem Bildschirm **Azure Database Migration Service** nach dem Namen der von Ihnen erstellten Instanz, und wählen Sie anschließend die Instanz aus.
 
3. Wählen Sie **+ Neues Migrationsprojekt** aus.

4. Geben Sie im Bildschirm **Neues Migrationsprojekt** einen Projektnamen im Textfeld **Quellservertyp** an, und wählen Sie **SQL Server** und dann im Textfeld **Zielservertyp** die Option **Verwaltete Azure SQL-Datenbank-Instanz** aus.

   ![Erstellen eines DMS-Projekts](media\tutorial-sql-server-to-managed-instance\dms-create-project1.png)

5. Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.

## <a name="specify-source-details"></a>Angeben von Quelldetails

1. Geben Sie im Bildschirm **Source details** (Quelldetails) die Verbindungsdetails für die SQL Server-Quellinstanz an.

2. Wenn Sie auf Ihrem Server kein vertrauenswürdiges Zertifikat installiert haben, aktivieren Sie das Kontrollkästchen **Serverzertifikat vertrauen**.

    Wenn ein vertrauenswürdiges Zertifikat nicht installiert ist, erzeugt SQL Server beim Starten der Instanz ein selbstsigniertes Zertifikat. Dieses Zertifikat wird zum Verschlüsseln der Anmeldeinformationen für Clientverbindungen verwendet.

    > [!CAUTION]
    > SSL-Verbindungen, die mit einem selbstsignierten Zertifikat verschlüsselt sind, bieten keine hohe Sicherheit. Sie sind anfällig für Man-in-the-Middle-Angriffe. Verlassen Sie sich in einer Produktionsumgebung oder auf Servern, die mit dem Internet verbunden sind, nicht auf SSL-Verbindungen, die selbstsignierte Zertifikate verwenden.

   ![Quellendetails](media\tutorial-sql-server-to-managed-instance\dms-source-details1.png)

3. Wählen Sie **Speichern**aus.

4. Wählen Sie auf dem Bildschirm **Quelldatenbanken auswählen** die Datenbank **Adventureworks2012** für die Migration aus.

   ![Auswählen von Quelldatenbanken](media\tutorial-sql-server-to-managed-instance\dms-source-database1.png)

5. Wählen Sie **Speichern**aus.

## <a name="specify-target-details"></a>Angeben von Zieldetails

1.  Geben Sie auf dem Bildschirm **Zieldetails** die Verbindungsdetails für das Ziel an, d.h. für die zuvor bereitgestellte verwaltete Azure SQL-Datenbank-Instanz, zu der die Datenbank **AdventureWorks2012** migriert wird.

    Wenn Sie die verwaltete Azure SQL-Datenbank-Instanz noch nicht bereitgestellt haben, wählen Sie **Nein** aus, um einen Link zur Unterstützung für die Bereitstellung der Instanz zu öffnen. Sie können trotzdem mit der Projekterstellung fortfahren und anschließend, wenn die verwaltete Azure SQL-Datenbank-Instanz bereit ist, für die Ausführung der Migration zu diesem bestimmten Projekt zurückkehren.   
 
       ![Auswählen des Ziels](media\tutorial-sql-server-to-managed-instance\dms-target-details1.png)

2.  Wählen Sie **Speichern**aus.

3.  Überprüfen Sie auf dem Bildschirm **Projektzusammenfassung** die dem Migrationsprojekt zugeordneten Details.
 
    ![Zusammenfassung des Migrationsprojekts](media\tutorial-sql-server-to-managed-instance\dms-project-summary1.png)

4.  Wählen Sie **Speichern**aus.   

## <a name="run-the-migration"></a>Ausführen der Migration

1.  Wählen Sie das kürzlich gespeicherte Projekt, **+ Neue Aktivität** und dann **Migration ausführen** aus.

    ![Erstellen einer neuen Aktivität](media\tutorial-sql-server-to-managed-instance\dms-create-new-activity1.png)

2.  Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen der Quell- und Zielserver ein, und wählen Sie dann **Speichern** aus.

3.  Wählen Sie auf dem Bildschirm **Quelldatenbanken auswählen** die Quelldatenbank(en) aus, die Sie migrieren möchten.

    ![Auswählen von Quelldatenbanken](media\tutorial-sql-server-to-managed-instance\dms-select-source-databases2.png)

4.  Wählen Sie **Speichern** aus, und wählen Sie dann auf dem Bildschirm **Benutzernamen auswählen** die Benutzernamen, die Sie migrieren möchten.

    Nur die Migration von SQL-Anmeldeinformationen wird von der aktuellen Release unterstützt.

    ![Auswählen von Benutzernamen](media\tutorial-sql-server-to-managed-instance\dms-select-logins.png)

5. Wählen Sie **Speichern** aus, und geben Sie anschließend auf dem Bildschirm **Migrationseinstellungen konfigurieren** folgende Details an:

    | | |
    |--------|---------|
    |**Freigabe der Netzwerkadresse** | Die lokale Netzwerkfreigabe, auf die Azure Database Migration Service die Quelldatenbanksicherungen übertragen kann. Das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, muss über Schreibberechtigungen für diese Netzwerkfreigabe verfügen. Geben Sie in der Netzwerkfreigabe einen FQDN oder IP-Adressen des Servers an, wie z.B. „\\\servername.domainname.com\backupfolder\\“ oder „\IP address\backupfolder“.|
    |**Benutzername** | Der Windows-Benutzername, den Azure Database Migration Service annehmen und mit dem Azure Database Migration Service die Sicherungsdateien für Wiederherstellungsvorgänge in den Azure Storage-Container hochladen kann. |
    |**Kennwort** | Kennwort für den Benutzer |
    |**Speicherkontoeinstellungen** | Der SAS-URI, der Azure Database Migration Service Zugriff auf Ihren Speicherkontocontainer gewährt, in den der Dienst die Sicherungsdateien hochlädt und der für die Migration von Datenbanken zur verwalteten Azure SQL-Datenbank-Instanz verwendet wird. [Erfahren Sie, wie der SAS-URI für Blobcontainer abgerufen wird](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container).|
    
    ![Konfigurieren von Migrationseinstellungen](media\tutorial-sql-server-to-managed-instance\dms-configure-migration-settings2.png)

5.  Wählen Sie **Speichern** aus, und geben Sie anschließend auf dem Bildschirm **Migrationszusammenfassung** im Textfeld **Aktivitätsname** einen Namen für die Migrationsaktivität an.

    ![Migrationszusammenfassung](media\tutorial-sql-server-to-managed-instance\dms-migration-summary2.png)

6. Erweitern Sie den Abschnitt **Überprüfungsoption**, um den Bildschirm **Überprüfungsoption auswählen** anzuzeigen. Geben Sie an, ob die migrierte Datenbank auf Richtigkeit der Abfrage überprüft werden soll, und wählen Sie anschließend **Speichern** aus.  

7. Wählen Sie **Migration ausführen** aus.

    Das Fenster „Migrationsaktivität“ wird angezeigt, und der Status der Aktivität lautet **Ausstehend**.

## <a name="monitor-the-migration"></a>Überwachen der Migration

1. Wählen Sie auf dem Bildschirm zur Migrationsaktivität die Option **Aktualisieren** aus, um die Anzeige zu aktualisieren.
 
   ![Migrationsaktivität in Bearbeitung](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-in-progress.png)

2. Sie können die Datenbanken und Anmeldekategorien erweitern, um den Migrationsstatus des jeweiligen Serverobjekts zu überwachen.

   ![Migrationsaktivität in Bearbeitung](media\tutorial-sql-server-to-managed-instance\dms-migration-activity-monitor.png)

3. Wählen Sie nach Abschluss der Migration die Option **Bericht herunterladen** aus, um einen Bericht abzurufen, in dem die Details zum Migrationsprozess aufgeführt werden.
 
4. Überprüfen Sie die Zieldatenbank in der Umgebung der verwalteten Azure SQL-Datenbank-Zielinstanz.

## <a name="next-steps"></a>Nächste Schritte

- Ein Tutorial, in dem gezeigt wird, wie Sie eine Datenbank mit dem T-SQL-Befehl RESTORE zu einer verwalteten Instanz migrieren können, finden Sie unter [Restore a database backup to an Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-restore-from-backup-tutorial.md).
- Informationen zur verwalteten Instanz finden Sie unter [Was ist eine verwaltete Instanz?](../sql-database/sql-database-managed-instance.md)
- Informationen zum Verbinden von Apps mit einer verwalteten Instanz finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-connect-app.md).
