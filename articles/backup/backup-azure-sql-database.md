---
title: Sichern der SQL Server-Datenbank in Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird die SQL Server-Sicherung in Azure erläutert. In diesem Artikel wird auch die SQL Server-Wiederherstellung beschrieben.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/6/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 32f45b66c4b1d22da3ffc4310a8a47c17319301f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38302822"
---
# <a name="back-up-sql-server-database-in-azure"></a>Sichern einer SQL Server-Datenbank in Azure

SQL Server-Datenbanken sind kritische Workloads, die ein niedriges Recovery Point Objective (RPO) und eine langfristige Aufbewahrung erfordern. Azure Backup bietet eine SQL Server-Sicherungslösung, die keine Infrastruktur benötigt, sodass kein komplexer Sicherungsserver, kein Verwaltungs-Agent und auch kein Sicherungsspeicher verwaltet werden muss. Azure Backup bietet eine zentrale Verwaltung Ihrer Sicherungen über alle SQL Server-Instanzen oder sogar über verschiedene Workloads hinweg.

 In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Voraussetzungen für die Sicherung von SQL Server in Azure
> * Erstellen und Verwenden eines Recovery Services-Tresors
> * Konfigurieren von SQL Server-Datenbanksicherungen
> * Festlegen einer Richtlinie zur Sicherung (oder Aufbewahrung) für die Wiederherstellungspunkte
> * Wiederherstellen der Datenbank

Bevor Sie mit den Prozeduren in diesem Artikel beginnen, sollten Sie eine SQL Server-Instanz in Azure ausführen. [Sie können SQL-Marketplace-VMs verwenden, um schnell eine SQL Server-Instanz zu erstellen](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Einschränkungen der Public Preview

Die folgenden Elemente sind die bekannten Einschränkungen der Public Preview.

- Der virtuelle SQL-Computer benötigt eine Internetverbindung, um auf die öffentlichen IP-Adressen von Azure zuzugreifen. Weitere Informationen finden Sie im Abschnitt [Herstellen der Netzwerkverbindung](backup-azure-sql-database.md#establish-network-connectivity).
- Sie können bis zu 2000 SQL-Datenbank-Instanzen in einem Recovery Services-Tresor sichern. Zusätzliche SQL-Datenbank-Instanzen sollten in einem separaten Recovery Services-Tresor gespeichert werden.
- [Für die Sicherung verteilter Verfügbarkeitsgruppen gelten Einschränkungen.](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017)
- SQL-Failoverclusterinstanzen (FCI) werden nicht unterstützt.
- Verwenden Sie das Azure-Portal, um Azure Backup zum Schutz von SQL Server-Datenbanken zu konfigurieren. Unterstützung für Azure PowerShell, CLI und REST-APIs ist derzeit nicht verfügbar.

## <a name="supported-azure-geos"></a>Für Azure unterstützte geografische Räume

- Australien, Südosten (ASE) 
- Brasilien, Süden (BRS)
- Kanada, Mitte (CNC)
- Kanada, Osten (CE)
- USA, Mitte (CUS)
- Asien, Osten (EA)
- Australien, Osten (AE) 
- USA, Osten (EUS)
- USA, Osten 2 (EUS2)
- Japan, Osten (JPE)
- Japan, Westen (JPW)
- Indien, Mitte (INC) 
- Indien, Süden (INS)
- Südkorea, Mitte (KRC)
- Korea, Süden (KRS)
- USA, Norden-Mitte (NCUS) 
- Europa, Norden (NE) 
- USA, Süden-Mitte (SCUS) 
- Asien, Südosten (SEA)
- Vereinigtes Königreich, Süden (UKS) 
- Vereinigtes Königreich, Westen (UKW) 
- Europa, Westen (WE) 
- USA, Westen (WUS)
- USA, Westen-Mitte (WCUS)
- USA, Westen 2 (WUS 2) 

## <a name="supported-operating-systems-and-versions-of-sql-server"></a>Unterstützte Betriebssysteme und Versionen von SQL Server

Die folgenden Betriebssysteme werden unterstützt. SQL-Marketplace-VMs in Azure und Nicht-Marketplace-VMs (in denen SQL Server manuell installiert ist) werden unterstützt.

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux wird derzeit nicht unterstützt.

### <a name="supported-versionseditions-of-sql-server"></a>Unterstützte Versionen/Editionen von SQL Server

- SQL 2012 Enterprise, Standard, Web, Developer, Express
- SQL 2014 Enterprise, Standard, Web, Developer, Express
- SQL 2016 Enterprise, Standard, Web, Developer, Express
- SQL 2017 Enterprise, Standard, Web, Developer, Express


## <a name="prerequisites-for-using-azure-backup-to-protect-sql-server"></a>Voraussetzungen für die Verwendung von Azure Backup zum Schutz von SQL Server 

Bevor Sie SQL Server-Datenbanken sichern können, müssen folgende Bedingungen erfüllt sein :

- Identifizieren oder [erstellen Sie einen Recovery Services-Tresor](backup-azure-sql-database.md#create-a-recovery-services-vault) in der gleichen Region oder dem gleichen Gebietsschema wie der virtuelle Computer, der SQL Server hostet.
- [Überprüfen Sie die Berechtigungen auf dem virtuellen Computer](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms), die für die Sicherung von SQL-Datenbank-Instanzen erforderlich sind.
- [Der virtuelle SQL-Computer verfügt über eine Netzwerkverbindung.](backup-azure-sql-database.md#establish-network-connectivity)

> [!NOTE]
> Sie können jeweils nur eine Sicherungslösung nutzen, um SQL Server-Datenbanken zu sichern. Deaktivieren Sie alle anderen SQL-Sicherungen, bevor Sie diese Funktion verwenden, da die Sicherungen sonst beeinträchtigt werden oder Fehler auftreten. Sie können Azure Backup für IaaS-VMs zusammen mit der SQL-Sicherungslösung ohne Konflikte aktivieren. 
>

Wenn diese Bedingungen in Ihrer Umgebung erfüllt sind, fahren Sie mit dem Abschnitt [Konfigurieren Ihres Tresors zum Schutz einer SQL-Datenbank](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database) fort. Wenn eine der Voraussetzungen nicht gegeben ist, lesen Sie diesen Abschnitt weiter.


## <a name="establish-network-connectivity"></a>Herstellen der Netzwerkverbindung

Für alle Vorgänge benötigt der virtuelle SQL-Computer eine Verbindung, um auf die öffentlichen IP-Adressen von Azure zuzugreifen. SQL-VM-Vorgänge (z.B. Ermitteln von Datenbanken, Konfigurieren von Sicherungen, geplante Sicherungen, Zurücksetzen auf Wiederherstellungspunkte) schlagen ohne Verbindung zu öffentlichen IP-Adressen fehl. Verwenden Sie eine der folgenden Optionen, um einen eindeutigen Pfad für Sicherungsdatenverkehr bereitzustellen:

- Whitelist der IP-Adressbereiche der Azure-Rechenzentren: [Ausführliche Informationen und Anweisungen zur Aufnahme der IP-Adressbereiche der Azure-Rechenzentren in eine Whitelist finden Sie auf der Download Center-Website](https://www.microsoft.com/download/details.aspx?id=41653). 
- Bereitstellen eines HTTP-Proxyservers für das Routing des Datenverkehrs: Wenn Sie eine SQL-Datenbank-Instanz auf einer VM sichern, verwendet die Sicherungserweiterung auf der VM HTTPS-APIs, um Verwaltungsbefehle an Azure Backup und Daten an Azure Storage zu senden. Die Sicherungserweiterung verwendet auch Azure Active Directory (AAD) zur Authentifizierung. Leiten Sie den Datenverkehr der Sicherungserweiterung für diese drei Dienste über den HTTP-Proxy, da er die einzige Komponente ist, die für den Zugriff auf das öffentliche Internet konfiguriert ist.

Die Kompromisse zwischen den Optionen sind: Verwaltbarkeit, Feinsteuerung und Kosten.

> [!NOTE]
>Diensttags für Azure Backup sollten allgemein verfügbar sein.
>

| Option | Vorteile | Nachteile |
| ------ | ---------- | ------------- |
| Whitelist der IP-Bereiche | Keine zusätzlichen Kosten. <br/> Verwenden Sie zum Öffnen des Zugriffs in einer Netzwerksicherheitsgruppe das **Set-AzureNetworkSecurityRule**-Cmdlet. | Komplexe Verwaltung, da sich die betroffenen IP-Bereiche im Laufe der Zeit ändern. <br/>Ermöglicht den Zugriff auf Azure insgesamt, nicht nur auf den Speicher.|
| Verwenden eines HTTP-Proxys   | Feinsteuerung im Proxy über die Speicher-URLs ist zulässig. <br/>Zentraler Punkt für Internetzugriff auf virtuelle Computer. <br/> Unterliegt keinen Azure-IP-Adressänderungen. | Zusätzliche Kosten für die Ausführung eines virtuellen Computers mit Proxysoftware. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Festlegen von Berechtigungen für Nicht-Marketplace-SQL-VMs

Zum Sichern eines virtuellen Computers benötigt Azure Backup die **AzureBackupWindowsWorkload**-Erweiterung. Wenn Sie virtuelle Computer des Azure-Marketplace verwenden, fahren Sie mit [Ermitteln von SQL Server-Datenbanken](backup-azure-sql-database.md#discover-sql-server-databases) fort. Wurde der virtuelle Computer, der Ihre SQL-Datenbanken hostet, nicht vom Azure-Marketplace aus erstellt, lesen Sie den folgenden Abschnitt, um die Erweiterung zu installieren und die entsprechenden Berechtigungen festzulegen. Zusätzlich zur **AzureBackupWindowsWorkload**-Erweiterung benötigt Azure Backup Systemadministratorberechtigungen für SQL, um SQL-Datenbanken zu schützen. Beim Ermitteln von Datenbanken auf dem virtuellen Computer erstellt Azure Backup ein Konto „NT Service\AzureWLBackupPluginSvc“. Damit Azure Backup SQL-Datenbanken ermitteln kann, muss das Konto „NT Service\AzureWLBackupPluginSvc“ über SQL- und SQL-Systemadministratorberechtigungen verfügen. Im Folgenden wird erläutert, wie Sie diese Berechtigungen bereitstellen.

So konfigurieren Sie Berechtigungen

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) den Recovery Services-Tresor, den Sie zum Schutz von SQL-Datenbanken verwenden.
2. Klicken Sie im Menü des Tresordashboards auf **+ Sicherung**, um das Menü **Sicherungsziel** zu öffnen.

   ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Übernehmen Sie im Menü **Sicherungsziel** im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Standardeinstellung **Azure**.

4. Erweitern Sie im Menü **Was möchten Sie sichern?** das Dropdownmenü, und wählen Sie **SQL Server in Azure-VM** aus.

    ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Das Menü **Sicherungsziel** zeigt zwei neue Schritte an: **DBs in VMs ermitteln** und **Azure Backup konfigurieren**. Über **DBs in VMs ermitteln** wird eine Suche nach virtuellen Azure-Computern gestartet.

    ![Zeigt die neuen Schritte unter „Sicherungsziel“](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Klicken Sie auf **Ermittlung starten**, um nach nicht geschützten virtuellen Computern im Abonnement zu suchen. Abhängig von der Anzahl der nicht geschützten virtuellen Computer im Abonnement dauert es eine Weile, bis alle virtuellen Computer durchsucht wurden.

    ![Sicherung ausstehend](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Sobald ein ungeschützter virtueller Computer entdeckt wurde, erscheint er in der Liste. Ungeschützte virtuelle Computer werden nach ihrem Namen und ihrer Ressourcengruppe aufgelistet. Es ist möglich, dass mehrere virtuelle Computer den gleichen Namen haben. Virtuelle Computer mit dem gleichen Namen gehören jedoch verschiedenen Ressourcengruppen an. Wenn ein erwarteter virtueller Computer nicht in der Liste erscheint, prüfen Sie, ob er bereits in einem Tresor geschützt ist.

6. Wählen Sie aus der Liste der virtuellen Computer die VM aus, die die zu sichernde SQL-Datenbank-Instanz enthält, und klicken Sie auf **DBs ermitteln**. 

    Beim Ermittlungsprozess wird die **AzureBackupWindowsWorkload**-Erweiterung auf dem virtuellen Computer installiert. Die Erweiterung ermöglicht es dem Azure Backup-Dienst, mit dem virtuellen Computer zu kommunizieren, um die SQL-Datenbank-Instanzen zu sichern. Sobald die Erweiterung installiert ist, erstellt Azure Backup das virtuelle Windows-Dienstkonto **NT Service\AzureWLBackupPluginSvc** auf dem virtuellen Computer. Für das virtuelle Dienstkonto sind SQL-Systemadministratorberechtigungen erforderlich. Wenn während der Installation des virtuellen Dienstkontos der Fehler **UserErrorSQLNoSysadminMembership** angezeigt wird, lesen Sie den Abschnitt [Beheben von Problemen mit SQL-Systemadministratorberechtigungen](backup-azure-sql-database.md#fixing-sql-sysadmin-permissions).

    Im Benachrichtigungsbereich zeigt den Status der Datenbankermittlung. Je nachdem, wie viele Datenbanken sich auf dem virtuellen Computer befinden, kann es eine Weile dauern, bis der Auftrag abgeschlossen ist. Eine Erfolgsmeldung angezeigt, sobald ausgewählte Datenbanken ermittelt wurden.

    ![Meldung über erfolgreiche Bereitstellung](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nachdem Sie die Datenbank mit dem Recovery Services-Tresor verknüpft haben, ist der nächste Schritt das [Konfigurieren der Sicherung](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="fixing-sql-sysadmin-permissions"></a>Beheben von Problemen mit SQL-Systemadministratorberechtigungen

Wenn während der Installation der Fehler **UserErrorSQLNoSysadminMembership** angezeigt wird, verwenden Sie ein Konto mit SQL-Systemadministratorberechtigungen, um sich bei SQL Server Management Studio (SSMS) anzumelden. Wenn Sie keine speziellen Berechtigungen benötigen, sollte die Windows-Authentifizierung funktionieren.

1. Öffnen Sie auf der SQL Server-Instanz den Ordner **Sicherheit/Anmeldungen**.

    ![Öffnen Sie die SQL Server-Instanz und die Ordner für Sicherheit und Anmeldung, um die Konten anzuzeigen.](./media/backup-azure-sql-database/security-login-list.png)

2. Klicken Sie im Ordner „Anmeldungen“ mit der rechten Maustaste, wählen Sie **Neue Anmeldung** aus, und klicken Sie im Dialogfeld „Anmeldung – Neu“ auf **Suchen**.

    ![Öffnen der Suche im Dialogfeld „Anmeldung – Neu“](./media/backup-azure-sql-database/new-login-search.png)

3. Da das virtuelle Windows-Dienstkonto **NT Service\AzureWLBackupPluginSvc** bereits während der Registrierungs- und SQL-Ermittlungsphase erstellt wurde, geben Sie den Kontonamen so ein, wie er im Dialogfeld **Geben Sie die zu verwendenden Objektnamen ein** angezeigt wird. Klicken Sie auf **Namen überprüfen**, um den Namen aufzulösen. 

    ![Klicken auf die Schaltfläche „Namen überprüfen“, um die unbekannten Dienstnamen aufzulösen](./media/backup-azure-sql-database/check-name.png)

4. Klicken Sie auf **OK**, um das Dialogfeld „Benutzer oder Gruppe auswählen“ zu schließen.

5. Stellen Sie sicher, dass im Dialogfeld **Serverrollen** die Rolle **Sysadmin** ausgewählt ist. Klicken Sie dann auf **OK**, um **Anmeldung – Neu** zu schließen.

    ![Sicherstellen, dass die Serverrolle „Sysadmin“ ausgewählt ist](./media/backup-azure-sql-database/sysadmin-server-role.png)

    Die erforderlichen Berechtigungen sollten jetzt vorhanden sein.

6. Obwohl Sie den Berechtigungsfehler behoben haben, müssen Sie die Datenbank noch immer mit dem Recovery Services-Tresor verknüpfen. Klicken Sie im Azure-Portal in der Liste **Geschützte Server** mit der rechten Maustaste auf den fehlerhaften Server, und wählen Sie **Datenbanken neu ermitteln** aus.

    ![Überprüfen, ob der Server über die entsprechenden Berechtigungen verfügt](./media/backup-azure-sql-database/check-erroneous-server.png)

    Im Benachrichtigungsbereich zeigt den Status der Datenbankermittlung. Je nachdem, wie viele Datenbanken sich auf dem virtuellen Computer befinden, kann es eine Weile dauern, bis der Auftrag abgeschlossen ist. Sobald ausgewählte Datenbanken gefunden wurden, wird im Benachrichtigungsbereich eine Erfolgsmeldung angezeigt.

    ![Meldung über erfolgreiche Bereitstellung](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nachdem Sie die Datenbank mit dem Recovery Services-Tresor verknüpft haben, ist der nächste Schritt das [Konfigurieren der Sicherung](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

[!INCLUDE [Section explaining how to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Ermitteln von SQL Server-Datenbanken

Azure Backup kann alle Datenbanken auf einer SQL Server-Instanz ermitteln, sodass Sie sie entsprechend Ihren Sicherungsanforderungen schützen können. Gehen Sie wie folgt vor, um den virtuellen Computer zu identifizieren, der die SQL-Datenbanken hostet. Sobald Sie den virtuellen Computer identifiziert haben, installiert Azure Backup eine einfache Erweiterung, um die SQL Server-Datenbanken zu ermitteln.

1. Melden Sie sich bei Ihrem Abonnement im [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie im linken Menü **Alle Dienste** aus.

    ![Auswählen der Option „Alle Dienste“ im Hauptmenü](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Geben Sie im Dialogfeld „Alle Dienste“ *Recovery Services* ein. Sobald Sie mit der Eingabe beginnen, wird die Ressourcenliste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Recovery Services-Tresore** aus, wenn der Eintrag angezeigt wird.

    ![Eingeben von Recovery Services im Dialogfeld „Alle Dienste“](./media/backup-azure-sql-database/all-services.png) <br/>

    Die Liste mit den Recovery Services-Tresoren im Abonnement wird angezeigt. 

4. Wählen Sie in der Liste der Recovery Services-Tresore den Tresor aus, den Sie verwenden möchten, um Ihre SQL-Datenbanken zu schützen.

5. Klicken Sie im Menü des Tresordashboards auf **+ Sicherung**, um das Menü **Sicherungsziel** zu öffnen.

   ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

6. Übernehmen Sie im Menü **Sicherungsziel** im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Standardeinstellung **Azure**.

7. Erweitern Sie im Menü **Was möchten Sie sichern?** das Dropdownmenü, und wählen Sie **SQL Server in Azure-VM** aus.

    ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Nach der Auswahl zeigt das Menü **Sicherungsziel** zwei Schritte an: „DBs in VMs ermitteln“ und „Azure Backup konfigurieren“. 

    ![Zeigt die neuen Schritte unter „Sicherungsziel“](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Klicken Sie auf **Ermittlung starten**, um nach nicht geschützten virtuellen Computern im Abonnement zu suchen. Abhängig von der Anzahl der nicht geschützten virtuellen Computer im Abonnement dauert es eine Weile, bis alle virtuellen Computer durchsucht wurden.

    ![Sicherung ausstehend](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Sobald ein ungeschützter virtueller Computer entdeckt wurde, erscheint er in der Liste. Mehrere virtuelle Computer können den gleichen Namen haben. Wenn mehrere virtuelle Computer den gleichen Namen haben, gehören diese jedoch verschiedenen Ressourcengruppen an. Die ungeschützten virtuellen Computer werden nach ihrem Namen und ihrer Ressourcengruppe aufgelistet. Wenn ein erwarteter virtueller Computer nicht aufgeführt ist, prüfen Sie, ob er bereits in einem Tresor geschützt ist.

9. Aktivieren Sie in der Liste der virtuellen Computer das Kontrollkästchen der VM, die die zu schützenden SQL-Datenbanken enthält, und klicken Sie auf **DBs ermitteln**.

    Azure Backup ermittelt alle SQL-Datenbanken auf dem virtuellen Computer. Informationen über die Vorgänge während der Datenbankermittlung finden Sie im folgenden Abschnitt [Back-End-Vorgänge beim Ermitteln von SQL-Datenbanken](backup-azure-sql-database.md#backend-operations-when-discovering-sql-databases). Nachdem Sie die SQL-Datenbanken ermittelt haben, können Sie nun den [Sicherungsauftrag konfigurieren](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="backend-operations-when-discovering-sql-databases"></a>Back-End-Vorgänge beim Ermitteln von SQL-Datenbanken

Wenn Sie das Tool **DBs ermitteln** verwenden, führt Azure Backup die folgenden Vorgänge im Hintergrund aus:

- Der virtuelle Computer wird beim Recovery Services-Tresor für die Workloadsicherung registriert. Alle Datenbanken auf dem registrierten virtuellen Computer können nur in diesem Recovery Services-Tresor gesichert werden. 

- Die Erweiterung **AzureBackupWindowsWorkload** wird auf dem virtuellen Computer installiert. Die Sicherung einer SQL-Datenbank erfolgt ohne Agent, d.h. mit der im virtuellen Computer installierten Erweiterung wurde kein Agent in der SQL-Datenbank installiert.

- Das Dienstkonto **NT Service\AzureWLBackupPluginSvc** wird im virtuellen Computer erstellt. Für alle Sicherungs- und Wiederherstellungsvorgänge wird das Dienstkonto verwendet. **NT Service\AzureWLBackupPluginSvc** erfordert SQL-Systemadministratorberechtigungen. Für alle SQL-Marketplace-VMs ist die Erweiterung „SqlIaaSExtension“ installiert, und „AzureBackupWindowsWorkload“ verwendet die Erweiterung „SQLIaaSExtension“, um automatisch die erforderliche Berechtigungen zu erhalten. Wenn auf Ihrem virtuellen Computer keine „SqlIaaSExtension“-Erweiterung installiert ist, tritt bei der Ermittlung der Datenbanken ein Fehler mit der Meldung **UserErrorSQLNoSysAdminMembership** auf. Um die Systemadministratorberechtigung für Sicherungen hinzuzufügen, folgen Sie den Anweisungen unter [Festlegen von Azure Backup-Berechtigungen für Nicht-Marketplace-SQL-VMs](backup-azure-sql-database.md#set-permissions-for-non--marketplace-sql-vms).

    ![Auswählen der VM und Datenbank](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-database"></a>Konfigurieren der Sicherung für SQL Server-Datenbanken

Azure Backup bietet Verwaltungsdienste zum Schutz Ihrer SQL Server-Datenbanken und zur Verwaltung von Sicherungsaufträgen. Die Verwaltungs- und Überwachungsfunktionen richten sich nach Ihrem Recovery Services-Tresor. 

> [!NOTE]
> Sie können jeweils nur eine Sicherungslösung nutzen, um SQL Server-Datenbanken zu sichern. Deaktivieren Sie alle anderen SQL-Sicherungen, bevor Sie diese Funktion verwenden, da die Sicherungen sonst beeinträchtigt werden oder Fehler auftreten. Sie können Azure Backup für IaaS-VMs zusammen mit der SQL-Sicherungslösung ohne Konflikte aktivieren. 
>

So konfigurieren Sie den Schutz für Ihre SQL-Datenbank

1. Öffnen Sie den registrierten Recovery Services-Tresor mit dem virtuellen SQL-Computer.

2. Klicken Sie im Menü des Tresordashboards auf **+ Sicherung**, um das Menü **Sicherungsziel** zu öffnen.

    ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Übernehmen Sie im Menü **Sicherungsziel** im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Standardeinstellung **Azure**.

4. Erweitern Sie im Menü **Was möchten Sie sichern?** das Dropdownmenü, und wählen Sie **SQL Server in Azure-VM** aus.

    ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Nach der Auswahl zeigt das Menü **Sicherungsziel** zwei Schritte an: „DBs in VMs ermitteln“ und „Azure Backup konfigurieren“. Wenn Sie diesen Artikel in der richtigen Reihenfolge gelesen haben, haben Sie die ungeschützten virtuellen Computer bereits ermittelt, und dieser Tresor ist bei einem virtuellen Computer registriert. Sie können nun den Schutz für die SQL-Datenbanken konfigurieren.

5. Klicken Sie im Menü „Sicherungsziel“ auf **Sicherung konfigurieren**.

    ![Zeigt die neuen Schritte unter „Sicherungsziel“](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    Der Azure Backup-Dienst zeigt alle SQL-Instanzen mit eigenständigen Datenbanken sowie SQL AlwaysOn-Verfügbarkeitsgruppen an. Klicken Sie auf das Erweiterungschevron neben dem Instanznamen, um die eigenständigen Datenbanken in der SQL-Instanz anzuzeigen. Die folgenden Bilder zeigen Beispiele für eine eigenständige Instanz und eine AlwaysOn-Verfügbarkeitsgruppe.

    > [!NOTE]
    > Im Fall einer SQL Always On-Verfügbarkeitsgruppe wird die SQL-Sicherungsvoreinstellung verwendet. Wegen einer Einschränkung der SQL-Plattform müssen vollständige und differenzielle Sicherungen vom primären Knoten aus erfolgen. Die Protokollsicherung kann gemäß Ihrer Sicherungseinstellung erfolgen. Aufgrund dieser Einschränkung muss der primäre Knoten immer für Verfügbarkeitsgruppen registriert werden.
    >

    ![Liste der Datenbanken in der SQL Server-Instanz](./media/backup-azure-sql-database/discovered-databases.png)

    Klicken Sie auf das Erweiterungschevron neben den AlwaysOn-Verfügbarkeitsgruppen, um die Liste der Datenbanken anzuzeigen.

    ![Liste der Datenbanken in AlwaysOn-Verfügbarkeitsgruppe](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. Wählen Sie in der Liste alle Datenbanken aus, die Sie schützen möchten, und klicken Sie auf **OK**.

    ![Auswählen mehrerer Datenbanken, um sie zu schützen](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    Sie können bis zu 50 Datenbanken gleichzeitig auswählen. Wenn Sie mehr als 50 Datenbanken schützen möchten, wiederholen Sie den Vorgang mehrere Male. Nachdem Sie die ersten 50 Datenbanken geschützt haben, wiederholen Sie diesen Schritt, um die nächsten Gruppe von Datenbanken zu schützen.
    > [!Note] 
    > Um die Sicherungslasten zu optimieren, teilt Azure Backup große Sicherungsaufträge in mehrere Batches auf. Ein Sicherungsauftrag kann maximal 50 Datenbanken umfassen.
    >
    >

7. Zum Erstellen oder Auswählen einer Sicherungsrichtlinie wählen Sie im Menü **Sicherung** die Option**Sicherungsrichtlinie** aus, um das Menü zu öffnen.

    ![Auswählen der Sicherungsrichtlinie](./media/backup-azure-sql-database/select-backup-policy.png)

8. Wählen Sie im Dropdownmenü **Sicherungsrichtlinie auswählen** eine Sicherungsrichtlinie aus, und klicken Sie auf **OK**. Informationen zum Erstellen einer eigenen Sicherungsrichtlinie finden Sie im Abschnitt [Definieren einer Sicherungsrichtlinie](backup-azure-sql-database.md#define-a-backup-policy).

    ![Auswählen einer Sicherungsrichtlinie im Dropdownmenü](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    Im Menü „Sicherungsrichtlinie“ können Sie im Dropdownmenü **Sicherungsrichtlinie auswählen** Folgendes wählen: 
    - Die standardmäßige HourlyLogBackup-Richtlinie 
    - Eine bestehende, zuvor für SQL erstellte Sicherungsrichtlinie
    - Die [Definition einer neuen Richtlinie](backup-azure-sql-database.md#define-a-backup-policy) basierend auf Ihrem Recovery Point Objective (RPO) und Ihrer Beibehaltungsdauer 

    > [!Note]
    > Azure Backup unterstützt die Langzeitaufbewahrung basierend auf dem Großvater-Vater-Sohn-Sicherungsschema, um den Back-End-Speicherverbrauch zu optimieren und gleichzeitig die Konformitätsanforderungen zu erfüllen.
    >

9. Sobald Sie eine Sicherungsrichtlinie ausgewählt haben, klicken Sie im Menü **Sicherung** auf **Sicherung aktivieren**.

    ![Aktivieren der ausgewählten Sicherungsrichtlinie](./media/backup-azure-sql-database/enable-backup-button.png)

    Den Konfigurationsprozess können Sie über den Benachrichtigungsbereich des Portals nachverfolgen.

    ![Anzeigen des Benachrichtigungsbereichs](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Definieren einer Sicherungsrichtlinie

Mit einer Sicherungsrichtlinie wird eine Matrix dafür definiert, wann Sicherungen erstellt und wie lange diese aufbewahrt werden. Mit Azure Backup können Sie drei Arten von Sicherungen für SQL-Datenbanken planen:

* Vollständige Sicherung: Bei einer vollständigen Datenbanksicherung wird die gesamte Datenbank gesichert. Eine vollständige Sicherung enthält alle Daten in einer bestimmten Datenbank oder einem bestimmten Satz von Dateigruppen oder Dateien sowie ausreichende Protokolle, um diese Daten wiederherzustellen. Pro Tag kann höchstens eine vollständige Sicherung ausgelöst werden. Sie können wählen, ob Sie eine vollständige Sicherung in einem täglichen oder wöchentlichen Intervall durchführen möchten. 
* Differenzielle Sicherung: Eine differenzielle Sicherung basiert auf der letzten, vorherigen vollständigen Datensicherung. Bei einer differenziellen Sicherung werden nur die Daten erfasst, die sich seit der vollständigen Sicherung geändert haben. Pro Tag kann höchstens eine differenzielle Sicherung ausgelöst werden. Eine vollständige Sicherung und eine differenzielle Sicherung kann nicht für denselben Tag konfiguriert werden.
* Transaktionsprotokollsicherung: Eine Protokollsicherung ermöglicht eine Zeitpunktwiederherstellung bis zu einer bestimmten Sekunde. Transaktionsprotokollsicherungen können höchstens alle 15 Minuten durchgeführt werden.

Die Richtlinie wird auf Ebene des Recovery Services-Tresors erstellt. Wenn mehrere Tresore vorhanden sind, können diese die gleiche Sicherungsrichtlinie verwenden. Zuvor müssen Sie jedoch die Sicherungsrichtlinie auf jeden Tresor anwenden. Die Standardeinstellung beim Erstellen einer Sicherungsrichtlinie ist die tägliche vollständige Sicherung. Sie können eine differenzielle Sicherung nur hinzufügen, wenn Sie für die vollständige Sicherung festlegen, dass diese wöchentlich durchgeführt wird. Die folgende Vorgehensweise erklärt, wie Sie eine Sicherungsrichtlinie für eine SQL Server-Instanz in einem virtuellen Azure-Computer erstellen.

So erstellen Sie eine Sicherungsrichtlinie

1. Wählen Sie im Menü „Sicherungsrichtlinie“ im Dropdownmenü **Sicherungsrichtlinie auswählen** die Option **Neu erstellen** aus.

   ![Erstellen einer neuen Sicherungsrichtlinie](./media/backup-azure-sql-database/create-new-backup-policy.png)

    Im Menü „Sicherungsrichtlinie“ werden nun die erforderlichen Felder für jede neue SQL Server-Sicherungsrichtlinie angezeigt.

   ![Neue Felder für Sicherungsrichtlinien](./media/backup-azure-sql-database/blank-new-policy.png)

2. Geben Sie in **Richtlinienname** einen Namen ein. 

3. Eine vollständige Sicherung ist obligatorisch. Sie können die Standardwerte für die vollständige Sicherung übernehmen oder auf **Vollständige Sicherung** klicken, um die Richtlinie zu bearbeiten.

    ![Neue Felder für Sicherungsrichtlinien](./media/backup-azure-sql-database/full-backup-policy.png)

    Wählen Sie innerhalb der Richtlinie für die vollständige Sicherung die Häufigkeit „Täglich“ oder „Wöchentlich“. Wenn Sie „Täglich“ wählen, legen Sie den Beginn des Sicherungsauftrags über die Zeitzone und die Uhrzeit fest. Wenn Sie sich entscheiden, täglich eine vollständige Sicherung durchzuführen, können Sie keine differenziellen Sicherungen mehr erstellen.

   ![Einstellung für das tägliche Intervall](./media/backup-azure-sql-database/daily-interval.png)

    Wenn Sie „Wöchentlich“ wählen, legen Sie den Beginn des Sicherungsauftrags über die Zeitzone, den Wochentag und die Uhrzeit fest.

   ![Einstellung für das wöchentliche Intervall](./media/backup-azure-sql-database/weekly-interval.png)

4. Standardmäßig sind alle Optionen für die Beibehaltungsdauer (Täglich, Wöchentlich, Monatlich und Jährlich) aktiviert. Deaktivieren Sie alle einschränkenden Optionen für die Beibehaltungsdauer, die Sie nicht verwenden möchten, und legen Sie die zu gewünschten Intervalle fest. Klicken Sie im Menü „Vollständige Sicherung“ auf **OK**, um die Einstellungen zu übernehmen.

   ![Intervalleinstellung für Beibehaltungsdauer](./media/backup-azure-sql-database/retention-range-interval.png)

    Wiederherstellungspunkte werden unter Berücksichtigung ihrer Beibehaltungsdauer mit einer Markierung versehen. Wenn Sie beispielsweise eine tägliche vollständige Sicherung wählen, wird pro Tag nur eine vollständige Sicherung ausgelöst. Abhängig von Ihrer wöchentlichen Beibehaltung wird die Sicherung des jeweiligen Tages basierend auf der wöchentlichen Beibehaltungsdauer markiert und aufbewahrt. Bei der monatlichen und jährlichen Beibehaltungsdauer verhält es sich ähnlich.

5. Um eine Richtlinie für eine differenzielle Sicherung hinzuzufügen, klicken Sie auf **Differenzielle Sicherung**, um das Menü zu öffnen. 

   ![Öffnen des Menüs für differenzielle Richtlinie](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Wählen Sie im Menü zum Erstellen einer Richtlinie für die differenzielle Sicherung die Option **Aktivieren**, um die Einstellungen für Häufigkeit und Beibehaltung vorzunehmen. Pro Tag kann höchstens eine differenzielle Sicherung ausgelöst werden.
    > [!Important] 
    > Differenzielle Sicherungen können maximal 180 Tage aufbewahrt werden. Wenn eine längere Aufbewahrung erforderlich ist, müssen Sie vollständige Sicherungen anstelle von differenziellen Sicherungen verwenden.
    >

   ![Bearbeiten einer differenziellen Richtlinie](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Klicken Sie auf **OK**, um die Richtlinie zu speichern und zum Hauptmenü „Sicherungsrichtlinie“ zurückzukehren.

6. Um eine Richtlinie für eine Transaktionsprotokollsicherung hinzuzufügen, klicken Sie auf **Protokollsicherung**, um das Menü zu öffnen. Wählen Sie im Menü „Protokollsicherung“ die Option **Aktivieren**, und legen Sie die Einstellungen für Häufigkeit und Aufbewahrung fest. Transaktionsprotokollsicherungen können alle 15 Minuten erfolgen und bis zu 35 Tage aufbewahrt werden. Klicken Sie auf **OK**, um die Richtlinie zu speichern und zum Hauptmenü „Sicherungsrichtlinie“ zurückzukehren.

   ![Bearbeiten der Richtlinie für eine Transaktionsprotokollsicherung](./media/backup-azure-sql-database/log-backup-policy-editor.png)

7. Wählen Sie, ob die SQL-Sicherungskomprimierung aktiviert werden soll. Die Komprimierung ist standardmäßig deaktiviert.

    Auf dem Back-End verwendet Azure Backup die native SQL-Sicherungskomprimierung.

8. Wenn Sie alle Änderungen an der Sicherungsrichtlinie vorgenommen haben, klicken Sie auf **OK**. 

   ![Akzeptieren einer neuen Richtlinie](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Wiederherstellen einer SQL-Datenbank-Instanz

Azure Backup bietet Funktionen zur Wiederherstellung einzelner Datenbanken zu einem bestimmten Datum oder einer bestimmten Uhrzeit, bis zu einer bestimmten Sekunde, unter Verwendung von Transaktionsprotokollsicherungen. Basierend auf den von Ihnen angegebenen Wiederherstellungszeiten ermittelt Azure Backup automatisch die geeigneten vollständigen Sicherungen, differenziellen Sicherungen und die Kette von Protokollsicherungen, die für die Wiederherstellung Ihrer Daten benötigt werden.

Alternativ können Sie eine bestimmte vollständige oder differenzielle Sicherung auswählen, um sie auf einen bestimmten Wiederherstellungspunkt und nicht bezogen auf einen bestimmten Zeitpunkt wiederherzustellen.
 > [!Note]
 > Vor dem Auslösen der Wiederherstellung der „Master“datenbank starten Sie SQL Server im Einzelbenutzermodus mit der Startoption „-m AzureWorkloadBackup“. Das Argument für -m ist der Name des Clients. Nur diesem Client ist es möglich, die Verbindung zu öffnen. Beenden Sie für alle Systemdatenbanken (Modell, Master, MSDB) vor dem Auslösen der Wiederherstellung den SQL Agent-Dienst. Schließen Sie alle Anwendungen, die ggf. versuchen, eine Verbindung mit einer dieser Datenbanken zu stehlen.
>

So stellen Sie eine Datenbank wieder her

1. Öffnen Sie den registrierten Recovery Services-Tresor mit dem virtuellen SQL-Computer.

2. Wählen Sie im Tresordashboard die Option **Verwendung** für die Sicherungselemente, um das Menü „Sicherungselemente“ zu öffnen.

    ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png)zu erstellen und zu verwalten.

3. Wählen Sie im Menü **Sicherungselemente** den Verwaltungstyp für die Sicherung **SQL auf virtuellem Azure-Computer**. 

    ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    In der Liste „Sicherungselemente“ werden nun die SQL-Datenbank-Instanzen angezeigt. 

4. Wählen Sie in der Liste der SQL-Datenbank-Instanzen die Datenbank aus, die Sie wiederherstellen möchten.

    ![Auswählen von „SQL auf virtuellem Azure-Computer“ aus der Liste](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Bei Auswahl der Datenbank wird das entsprechende Menü geöffnet. Dieses Menü enthält die Sicherungsdetails für die Datenbank:

    * die ältesten und neuesten Wiederherstellungspunkte
    * Protokollsicherungsstatus der letzten 24 Stunden (für Datenbanken im Wiederherstellungsmodus für vollständige und massenprotokollierte Sicherungen, falls für Transaktionsprotokollsicherungen konfiguriert)

5. Klicken Sie im ausgewählten Datenbankmenü auf **Datenbank wiederherstellen**, um das Menü „Wiederherstellen“ zu öffnen.

    ![Auswählen von „Datenbank wiederherstellen“](./media/backup-azure-sql-database/restore-db-button.png)

    Das Menü **Wiederherstellen** wird geöffnet, ebenso wie das Menü **Konfiguration wiederherstellen**. Das Menü **Konfiguration wiederherstellen** ist der erste Schritt zur Konfiguration der Wiederherstellung. In diesem Menü wählen Sie aus, wo die Daten wiederhergestellt werden sollen. Die Optionen sind:
    * Alternativer Speicherort: Verwenden Sie diese Option, wenn Sie die Datenbank an einem anderen Speicherort wiederherstellen möchten, während die ursprüngliche Quelldatenbank erhalten bleibt.
    * Datenbank überschreiben: Diese Option stellt die Daten auf der gleichen SQL Server-Instanz her, auf der sich auch die ursprüngliche Quelle befunden hat. Dadurch wird die ursprüngliche Datenbank überschrieben.

    > [!Important]
    > Wenn die ausgewählte Datenbank zu einer AlwaysOn-Verfügbarkeitsgruppe gehört, lässt SQL das Überschreiben der Datenbank nicht zu. In diesem Fall ist nur die Option **Alternativer Standort** aktiviert.
    >

    ![Klicken auf „Konfigurieren“ zum Öffnen des Menüs „Konfiguration wiederherstellen“](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Wiederherstellen an einem alternativen Speicherort

Diese Vorgehensweise führt Sie durch die Wiederherstellung der Daten an einem alternativen Speicherort. Wenn Sie die Datenbank beim Wiederherstellen überschreiben möchten, springen Sie zum Abschnitt [Wiederherstellen und Überschreiben der Datenbank](backup-azure-sql-database.md#restore-and-overwrite-the-database). Für diese Vorgehensweise müssen Sie Ihren Recovery Services-Tresor geöffnet haben und sich im Menü „Konfiguration wiederherstellen“ befinden. Sollte dies nicht der Fall sein, beginnen Sie mit dem Abschnitt [Wiederherstellen einer SQL-Datenbank-Instanz](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> Sie können die Datenbank auf einem SQL Server in der gleichen Azure-Region wiederherstellen, und der Zielserver muss im Recovery Services-Tresor registriert sein. 
>

Im Dropdownmenü **Server** werden nur die SQL Server-Instanzen angezeigt, die im Recovery Services-Tresor registriert sind. Wenn sich der gewünschte Server nicht in der Liste **Server** befindet, lesen Sie den Abschnitt [Ermitteln von SQL Server-Datenbanken](backup-azure-sql-database.md#discover-sql-server-databases), um nach dem Server zu suchen. Während des Prozesses zur Ermittlung der Datenbank werden alle neuen Server im Recovery Services-Tresor registriert.

1. Das Menü **Konfiguration wiederherstellen** gehen Sie wie folgt vor:

    * Wählen Sie **Alternativer Standort** aus.
    * Wählen Sie für **Server** die SQL Server-Instanz aus, auf der die Datenbank wiederhergestellt werden soll.
    * Wählen Sie im Dropdownmenü **Instanz** eine SQL-Instanz aus.
    * Geben Sie im Dialogfeld **Name der wiederhergestellten Datenbank** den Namen der Zieldatenbank an.
    * Aktivieren Sie ggf. **Überschreiben, wenn die gleichnamige Datenbank bereits in der ausgewählten SQL-Instanz vorhanden ist**.
    * Klicken Sie auf **OK**, um die Konfiguration des Ziels abzuschließen und einen Wiederherstellungspunkt auszuwählen.

    ![Auswählen eines alternativen Speicherorts, einer Instanz und eines Namens im Menü „Konfiguration wiederherstellen“](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. Im Menü **Wiederherstellungspunkt auswählen** können Sie entweder den Wiederherstellungspunkt „Protokolle (Zeitpunkt)“ oder „Vollständig und differenziell“ auswählen. Wenn die Wiederherstellung auf ein Protokoll eines bestimmten Zeitpunkts erfolgen soll, fahren Sie mit diesem Schritt fort. Soll die Wiederherstellung einer vollständigen oder differenziellen Sicherung erfolgen, fahren Sie mit Schritt 3 fort.

    ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-menu.png)

    Die Zeitpunktwiederherstellung ist nur bei Transaktionsprotokollsicherungen für Datenbanken im Wiederherstellungsmodus für vollständige oder massenprotokollierte Sicherungen verfügbar. Bei einer Zeitpunktwiederherstellung gehen Sie wie folgt vor:

    1. Wählen Sie **Protokolle (Zeitpunkt)** als Wiederherstellungsoption.

        ![Auswählen des Wiederherstellungspunkttyps](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Klicken Sie unter **Datum/Uhrzeit für Wiederherstellung** auf das Kalendersymbol, um den Kalender zu öffnen. Die fett angezeigten Daten enthalten Wiederherstellungspunkte, und das aktuelle Datum ist hervorgehoben. Wählen Sie ein Datum im Kalender mit Wiederherstellungspunkten aus. Daten ohne Wiederherstellungspunkte können Sie nicht auswählen.

        ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Sobald Sie ein Datum ausgewählt haben, zeigt das Zeitachsendiagramm die verfügbaren Wiederherstellungspunkte in einem fortlaufenden Bereich an.

    3. Geben Sie entweder im Zeitachsendiagramm oder im Dialogfeld „Zeit“ eine bestimmte Zeit für den Wiederherstellungspunkt an, und klicken Sie auf **OK**, um den Schritt „Wiederherstellungspunkt“ abzuschließen.
    
       ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        Das Menü **Wiederherstellungspunkt auswählen** wird geschlossen, und das Menü **Erweiterte Konfiguration** wird geöffnet.

       ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Gehen Sie im Menü **Erweiterte Konfiguration** wie folgt vor:

        * Damit die Datenbank nach dem Wiederherstellen nicht betriebsbereit bleibt, wählen Sie im Menü **Mit NORECOVERY wiederherstellen** die Option **Aktiviert** aus.
        * Wenn Sie den Speicherort für die Wiederherstellung auf dem Zielserver ändern möchten, geben Sie einen neuen Pfad in der Spalte **Ziel** an.
        * Klicken Sie auf **OK**, um die Einstellungen zu übernehmen, und schließen Sie **Erweiterte Konfiguration**.

    5. Klicken Sie im Menü **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsauftrag zu starten. Den Fortschritt können Sie im Benachrichtigungsbereich verfolgen. Sie können den Fortschritt auch in den Wiederherstellungsaufträgen der Datenbank einsehen.

       ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-job-notification.png)

3. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** einen Wiederherstellungspunkt aus. Sie können entweder „Protokolle (Zeitpunkt)“ oder „Vollständig und differenziell“ wählen. Wenn Sie ein Zeitpunktprotokoll wiederherstellen möchten, gehen Sie zurück zu Schritt 2. Mit diesem Schritt wird einen bestimmter vollständiger oder differentieller Wiederherstellungspunkt wiederhergestellt. Über diese Option können Sie alle vollständigen und differenziellen Wiederherstellungspunkte der letzten 30 Tage anzeigen. Wenn Sie Wiederherstellungspunkte einsehen möchten, die älter als 30 Tage sind, klicken Sie auf **Filter**, um das Menü **Wiederherstellungspunkte filtern** zu öffnen. Wenn Sie einen differenziellen Wiederherstellungspunkt wählen, stellt Azure Backup zuerst den entsprechenden vollständigen Wiederherstellungspunkt wieder her und wendet dann den ausgewählten differenziellen Wiederherstellungspunkt an.

    ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** die Option **Vollständig und differenziell** aus.

       ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Die Liste der verfügbaren Wiederherstellungspunkte wird angezeigt.

    2. Wählen Sie aus der Liste der Wiederherstellungspunkte einen Wiederherstellungspunkt aus, und klicken Sie auf **OK**, um die Prozedur der Wiederherstellungspunkte abzuschließen. 

        ![Auswählen des vollständigen Wiederherstellungspunkts](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        Das Menü **Wiederherstellungspunkt** wird geschlossen, und das Menü **Erweiterte Konfiguration** wird geöffnet.

        ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Gehen Sie im Menü **Erweiterte Konfiguration** wie folgt vor:

        * Damit die Datenbank nach dem Wiederherstellen nicht betriebsbereit bleibt, wählen Sie im Menü **Mit NORECOVERY wiederherstellen** die Option **Aktiviert** aus. **Mit NORECOVERY wiederherstellen** ist standardmäßig deaktiviert.
        * Wenn Sie den Speicherort für die Wiederherstellung auf dem Zielserver ändern möchten, geben Sie einen neuen Pfad in der Spalte **Ziel** an.
        * Klicken Sie auf **OK**, um die Einstellungen zu übernehmen, und schließen Sie **Erweiterte Konfiguration**.

    4. Klicken Sie im Menü **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsauftrag zu starten. Den Fortschritt können Sie im Benachrichtigungsbereich verfolgen. Sie können den Fortschritt auch in den Wiederherstellungsaufträgen der Datenbank einsehen.

       ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Wiederherstellen und Überschreiben der Datenbank

Diese Vorgehensweise führt Sie durch das Wiederherstellen von Daten und das Überschreiben der Datenbank. Wenn die Wiederherstellung an einem alternativen Speicherort erfolgen soll, springen Sie zum Abschnitt [Wiederherstellen an einen anderen Speicherort](backup-azure-sql-database.md#restore-to-an-alternate-location). Für diese Vorgehensweise müssen Sie Ihren Recovery Services-Tresor geöffnet haben und sich im Menü **Konfiguration wiederherstellen** befinden (siehe folgende Abbildung). Sollte dies nicht der Fall sein, beginnen Sie mit dem Abschnitt [Wiederherstellen einer SQL-Datenbank-Instanz](backup-azure-sql-database.md#restore-a-sql-database).

![Klicken auf „Konfigurieren“ zum Öffnen des Menüs „Konfiguration wiederherstellen“](./media/backup-azure-sql-database/restore-overwrite-db.png)

Im Dropdownmenü **Server** werden nur die SQL Server-Instanzen angezeigt, die im Recovery Services-Tresor registriert sind. Wenn sich der gewünschte Server nicht in der Liste **Server** befindet, lesen Sie den Abschnitt [Ermitteln von SQL Server-Datenbanken](backup-azure-sql-database.md#discover-sql-server-databases), um nach dem Server zu suchen. Während des Prozesses zur Ermittlung der Datenbank werden alle neuen Server im Recovery Services-Tresor registriert.

1. Wählen Sie im Menü **Konfiguration wiederherstellen** die Option **Datenbank überschreiben** aus, und klicken Sie auf **OK**, um die Konfiguration des Ziels abzuschließen. 

   ![Klicken auf „Datenbank überschreiben“](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    Die Dialogfelder **Server**, **Instanz** und **Name der wiederhergestellten Datenbank** sind nicht erforderlich.

2. Im Menü **Wiederherstellungspunkt auswählen** können Sie entweder den Wiederherstellungspunkt „Protokolle (Zeitpunkt)“ oder „Vollständig und differenziell“ auswählen. Wenn die Wiederherstellung entsprechend eines Zeitpunktprotokolls erfolgen soll, fahren Sie mit diesem Schritt fort. Soll die Wiederherstellung einer vollständigen oder differenziellen Sicherung erfolgen, fahren Sie mit Schritt 3 fort.

    ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-menu.png)

    Die Zeitpunktwiederherstellung ist nur bei Transaktionsprotokollsicherungen für Datenbanken im Wiederherstellungsmodus für vollständige oder massenprotokollierte Sicherungen verfügbar. Bei einer Zeitpunktwiederherstellung einer bestimmten Sekunde gehen Sie wie folgt vor:

    1. Wählen Sie **Protokolle (Zeitpunkt)** als Wiederherstellungsoption.

        ![Auswählen des Wiederherstellungspunkttyps](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Klicken Sie unter **Datum/Uhrzeit für Wiederherstellung** auf das Kalendersymbol, um den Kalender zu öffnen. Die fett angezeigten Daten enthalten Wiederherstellungspunkte, und das aktuelle Datum ist hervorgehoben. Wählen Sie ein Datum im Kalender mit Wiederherstellungspunkten aus. Daten ohne Wiederherstellungspunkte können Sie nicht auswählen.

        ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Sobald Sie ein Datum ausgewählt haben, zeigt das Zeitachsendiagramm die verfügbaren Wiederherstellungspunkte an.

    3. Geben Sie entweder im Zeitachsendiagramm oder im Dialogfeld „Zeit“ eine bestimmte Zeit für den Wiederherstellungspunkt an, und klicken Sie auf **OK**, um den Schritt „Wiederherstellungspunkt“ abzuschließen.
    
       ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        Das Menü **Wiederherstellungspunkt auswählen** wird geschlossen, und das Menü **Erweiterte Konfiguration** wird geöffnet.

       ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Gehen Sie im Menü **Erweiterte Konfiguration** wie folgt vor:

        * Damit die Datenbank nach dem Wiederherstellen nicht betriebsbereit bleibt, wählen Sie im Menü **Mit NORECOVERY wiederherstellen** die Option **Aktiviert** aus.
        * Wenn Sie den Speicherort für die Wiederherstellung auf dem Zielserver ändern möchten, geben Sie einen neuen Pfad in der Spalte **Ziel** an.
        * Klicken Sie auf **OK**, um die Einstellungen zu übernehmen, und schließen Sie **Erweiterte Konfiguration**.

    5. Klicken Sie im Menü **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsauftrag zu starten. Den Fortschritt können Sie im Benachrichtigungsbereich verfolgen. Sie können den Fortschritt auch in den Wiederherstellungsaufträgen der Datenbank einsehen.

       ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-job-notification.png)

3. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** einen Wiederherstellungspunkt aus. Sie können entweder „Protokolle (Zeitpunkt)“ oder „Vollständig und differenziell“ wählen. Wenn Sie ein Zeitpunktprotokoll wiederherstellen möchten, gehen Sie zurück zu Schritt 2. Mit diesem Schritt wird einen bestimmter vollständiger oder differentieller Wiederherstellungspunkt wiederhergestellt. Über diese Option können Sie alle vollständigen und differenziellen Wiederherstellungspunkte der letzten 30 Tage anzeigen. Wenn Sie Wiederherstellungspunkte einsehen möchten, die älter als 30 Tage sind, klicken Sie auf **Filter**, um das Menü **Wiederherstellungspunkte filtern** zu öffnen. Wenn Sie einen differenziellen Wiederherstellungspunkt wählen, stellt Azure Backup zuerst den entsprechenden vollständigen Wiederherstellungspunkt wieder her und wendet dann den ausgewählten differenziellen Wiederherstellungspunkt an.

    ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** die Option **Vollständig und differenziell** aus.

       ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Die Liste der verfügbaren Wiederherstellungspunkte wird angezeigt.

    2. Wählen Sie aus der Liste der Wiederherstellungspunkte einen Wiederherstellungspunkt aus, und klicken Sie auf **OK**, um die Prozedur der Wiederherstellungspunkte abzuschließen. 

        ![Auswählen des vollständigen Wiederherstellungspunkts](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        Das Menü **Wiederherstellungspunkt** wird geschlossen, und das Menü **Erweiterte Konfiguration** wird geöffnet.

        ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Gehen Sie im Menü **Erweiterte Konfiguration** wie folgt vor:

        * Damit die Datenbank nach dem Wiederherstellen nicht betriebsbereit bleibt, wählen Sie im Menü **Mit NORECOVERY wiederherstellen** die Option **Aktiviert** aus. **Mit NORECOVERY wiederherstellen** ist standardmäßig deaktiviert.
        * Wenn Sie den Speicherort für die Wiederherstellung auf dem Zielserver ändern möchten, geben Sie einen neuen Pfad in der Spalte **Ziel** an.
        * Klicken Sie auf **OK**, um die Einstellungen zu übernehmen, und schließen Sie **Erweiterte Konfiguration**.

    4. Klicken Sie im Menü **Wiederherstellen** auf **Wiederherstellen**, um den Wiederherstellungsauftrag zu starten. Den Fortschritt können Sie im Benachrichtigungsbereich verfolgen. Sie können den Fortschritt auch in den Wiederherstellungsaufträgen der Datenbank einsehen.

       ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-job-notification.png)


## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Verwalten von Azure Backup-Vorgängen für SQL auf virtuellen Azure-Computern

Dieser Abschnitt enthält Informationen zu den verschiedenen Azure Backup-Verwaltungsvorgängen, die für SQL auf virtuellen Azure-Computern verfügbar sind. Es gibt die folgenden allgemeinen Vorgänge:

* Überwachen von Aufträgen
* Warnungen zu Sicherungen
* Beenden des Schutzes für eine SQL-Datenbank-Instanz
* Fortsetzen des Schutzes für eine SQL-­Datenbank-Instanz
* Auslösen eines Ad-hoc-Sicherungsauftrags
* Aufheben der Registrierung einer SQL Server-Instanz

### <a name="monitor-jobs"></a>Überwachen von Aufträgen
Azure Backup bietet als Lösung der Unternehmensklasse erweiterte Sicherungswarnungen und Benachrichtigungen bei Fehlern (siehe Abschnitt „Sicherungswarnungen“ weiter unten). Wenn Sie dennoch bestimmte Aufträge überwachen möchten, können Sie je nach Bedarf eine der folgenden Optionen verwenden:

#### <a name="use-azure-portal-for-all-adhoc-operations"></a>Verwenden des Azure-Portals für alle Ad-hoc-Vorgänge
Azure Backup zeigt alle manuell oder ad-hoc ausgelösten Aufträge im Portal für Sicherungsaufträge an. Die im Portal verfügbaren Aufträge umfassen: alle Vorgänge zum Konfigurieren von Sicherungen, manuell ausgelöste Sicherungsvorgänge, Wiederherstellungsvorgänge, Registrierungs- und Ermittlungsvorgänge für Datenbanken sowie Vorgänge zum Beenden von Sicherungen. 
![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Alle geplanten Sicherungsaufträge einschließlich vollständiger, differenzieller und Protokollsicherungen werden im Portal nicht angezeigt und können mit SQL Server Management Studio wie unten beschrieben überwacht werden.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>Verwenden von SQL Server Management Studio für Sicherungsaufträge
Azure Backup nutzt für alle Sicherungsvorgänge native SQL-APIs. Mit den nativen APIs können Sie alle Auftragsinformationen aus der [SQL-Sicherungssatztabelle](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in der MSDB-Datenbank abrufen.

Das folgende Beispiel zeigt eine Abfrage zum Abrufen aller Sicherungsaufträge für eine Datenbank mit dem Namen **DB1**. Passen Sie die Abfrage für eine erweiterte Überwachung an.
```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  
 
```

### <a name="backup-alerts"></a>Warnungen zu Sicherungen

Da Protokollsicherungen alle 15 Minuten stattfinden, kann die Überwachung der Sicherungsaufträge gelegentlich mühsam sein. Azure Backup ist auf diese potenziell langwierige Aufgabe vorbereitet, indem es E-Mail-Benachrichtigungen bereitstellt, die bei einem Sicherungsfehler ausgelöst werden. Benachrichtigungen werden auf Datenbankebene nach Fehlercode konsolidiert. Wenn beispielsweise eine Datenbank mehrere Sicherungsfehler aufweist, erhalten Sie statt einer Benachrichtigung für jeden einzelnen Fehler eine E-Mail beim ersten Fehler. Sie können sich dann im Azure-Portal anmelden, um nachfolgende Fehler für diese Datenbank zu überwachen. 

So überwachen Sie Sicherungswarnungen

1. Melden Sie sich bei Ihrem Azure-Abonnement im [Azure-Portal](https://portal.azure.com) an.

2. Öffnen Sie den registrierten Recovery Services-Tresor mit dem virtuellen SQL-Computer.

3. Wählen Sie im Menü „Recovery Services-Tresor“ die Option **Warnungen und Ereignisse** aus. 

   ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. Wählen Sie im Menü **Warnungen und Ereignisse** die Option **Sicherungswarnungen** aus, um eine Liste der Warnungen anzuzeigen.

   ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-on-a-sql-server-database"></a>Beenden des Schutzes für eine SQL Server-Datenbank

Wenn Sie den Schutz einer SQL Server-Datenbank beenden, werden Sie in Azure Backup gefragt, ob Sie die Wiederherstellungspunkte beibehalten möchten. Es gibt zwei Möglichkeiten, den Schutz der SQL-Datenbank-Instanz zu beenden:

* Beenden aller zukünftigen Sicherungsaufträge und Löschen aller Wiederherstellungspunkte
* Beenden aller zukünftigen Sicherungsaufträge und Beibehalten der Wiederherstellungspunkte 

Das Verlassen der Wiederherstellungspunkte ist mit Kosten verbunden, da für die Wiederherstellungspunkte für SQL der Tarif für die durch SQL geschützte Instanz und den genutzten Speicherplatz fällig wird. Weitere Informationen zu den Preisen von Azure Backup für SQL finden Sie auf der Seite mit der [Azure Backup-Preisübersicht](https://azure.microsoft.com/pricing/details/backup/). Zum Beenden des Schutzes für die Datenbank gehen Sie wie folgt vor:

1. Öffnen Sie den registrierten Recovery Services-Tresor mit dem virtuellen SQL-Computer.

2. Wählen Sie im Tresordashboard die Option **Verwendung** für die Sicherungselemente, um das Menü „Sicherungselemente“ zu öffnen.

    ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png)zu erstellen und zu verwalten.

3. Wählen Sie im Menü **Sicherungselemente** den Verwaltungstyp für die Sicherung **SQL auf virtuellem Azure-Computer**. 

    ![Klicken auf „+ Sicherung“, um das Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    In der Liste „Sicherungselemente“ werden nun die SQL-Datenbank-Instanzen angezeigt. 

4. Wählen Sie in der Liste der SQL-Datenbank-Instanzen die Datenbank aus, deren Schutz Sie beenden möchten.

    ![Auswählen von „SQL auf virtuellem Azure-Computer“ aus der Liste](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Bei Auswahl der Datenbank wird das entsprechende Menü geöffnet. 

5. Klicken Sie im Menü der ausgewählten Datenbank auf **Sicherung beenden**, um den Schutz für die Datenbank zu beenden.

    ![Auswählen von „Datenbank wiederherstellen“](./media/backup-azure-sql-database/stop-db-button.png)

    Das Menü **Sicherung beenden** wird geöffnet.

6. Wählen Sie im Menü **Sicherung beenden** die Option „Sicherungsdaten beibehalten“ oder „Sicherungsdaten löschen“. Optional können Sie einen Grund für das Beenden des Schutzes und einen Kommentar angeben.

    ![Auswählen von „Datenbank wiederherstellen“](./media/backup-azure-sql-database/stop-backup-button.png)

7. Klicken Sie auf **Sicherung beenden**, um den Schutz für die Datenbank zu beenden. 

### <a name="resume-protection-for-a-sql-database"></a>Fortsetzen des Schutzes für eine SQL-­Datenbank-Instanz

Wenn beim Beenden des Schutzes für die SQL-Datenbank-Instanz die Option **Sicherungsdaten beibehalten** ausgewählt wurde, kann der Schutz fortgesetzt werden. Wenn die Sicherungsdaten nicht beibehalten wurden, kann der Schutz nicht fortgesetzt werden. 

1. Um den Schutz für die SQL-Datenbank-Instanz fortzusetzen, öffnen Sie das Sicherungselement, und klicken Sie auf **Sicherung fortsetzen**.

    ![Fortsetzen des Datenbankschutzes](./media/backup-azure-sql-database/resume-backup-button.png)

   Das Menü „Sicherungsrichtlinie“ wird geöffnet.

2. Wählen Sie im Menü **Sicherungsrichtlinie** eine Richtlinie aus, und klicken Sie dann auf **Speichern**.

### <a name="trigger-an-adhoc-backup"></a>Auslösen einer Ad-hoc-Sicherung

Eine Ad-hoc-Sicherung können Sie jederzeit auslösen. Es gibt vier Arten von Ad-hoc-Sicherungen: Details zu den jeweiligen Arten finden Sie im Artikel [Sicherungsarten](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

* Vollständige Sicherung
* Nur vollständige Sicherung kopieren
* Differenzielle Sicherung
* Protokollsicherung

### <a name="unregister-a-sql-server"></a>Aufheben der Registrierung einer SQL Server-Instanz

So heben Sie die Registrierung einer SQL Server-Instanz nach dem Entfernen des Schutzes und vor dem Löschen des Tresors auf

1. Öffnen Sie den registrierten Recovery Services-Tresor mit dem virtuellen SQL-Computer.

2. Klicken Sie im Menü des Tresors im Abschnitt **Verwalten** auf die Option **Sicherungsinfrastruktur**.  

   ![Fortsetzen des Datenbankschutzes](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. Klicken Sie im Abschnitt **Verwaltungsserver** auf **Geschützte Server**.

   ![Fortsetzen des Datenbankschutzes](./media/backup-azure-sql-database/protected-servers.png)

    Das Menü „Geschützte Server“ wird geöffnet. 

4. Wählen Sie im Menü **Geschützte Server** den Server aus, für den die Registrierung aufgehoben werden soll. FALLS Sie den Tresor löschen möchten, müssen Sie die Registrierung für alle Server aufheben.

5. Klicken Sie im Menü „Geschützte Server“ mit der rechten Maustaste auf den geschützten Server, und wählen Sie **Löschen** aus. 

   ![Fortsetzen des Datenbankschutzes](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="sql-database-backup-faq"></a>FAQ zur SQL-Datenbanksicherung

Der folgende Abschnitt enthält zusätzliche Informationen zur SQL-Datenbanksicherung.

### <a name="can-i-throttle-the-speed-of-the-sql-backup-policy-so-it-minimizes-impact-on-the-sql-server"></a>Kann ich die Geschwindigkeit der SQL-Sicherungsrichtlinie drosseln, um die Auswirkungen auf den Computer mit SQL Server zu minimieren?

Ja, Sie können die Rate drosseln, mit der die Sicherungsrichtlinie ausgeführt wird. So ändern Sie die Einstellung:

1. Öffnen Sie auf dem Computer mit SQL Server **TaskThrottlerSettings.json** im Ordner `C:\Program Files\Azure Workload Backup\bin`.

2. Ändern sie in der Datei **TaskThrottlerSettings.json** die Angabe **DefaultBackupTasksThreshold** in einen kleineren Wert (z.B. 5).

3. Speichern Sie die Änderung, und schließen Sie die Datei.

4. Öffnen Sie auf dem Computer mit SQL Server den Task-Manager, und starten Sie den **Azure Backup Workload Coordinator-Dienst** neu.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kann ich eine vollständige Sicherung aus einem sekundären Replikat ausführen?

Nein, dieses Feature wird nicht unterstützt.

### <a name="do-successful-backup-jobs-create-alerts"></a>Erstellen erfolgreiche Sicherungsaufträge Warnungen?

Nein. Erfolgreiche Sicherungsaufträge generieren keine Warnungen. Warnungen werden nur für Sicherungsaufträge gesendet, bei denen ein Fehler aufgetreten ist.

### <a name="are-scheduled-backup-job-details-shown-in-the-jobs-menu"></a>Werden die Details geplanter Sicherungsaufträge im Menü „Aufträge“ angezeigt?

Nein. Im Menü „Aufträge“ werden die Details von Ad-hoc-Aufträgen angezeigt, nicht jedoch die Details geplanter Sicherungsaufträge. Wenn bei geplanten Sicherungsaufträgen ein Fehler auftritt, finden Sie alle Details in den Warnungen der fehlerhaften Aufträge. Wenn Sie alle geplanten und Ad-hoc-Sicherungsaufträge überwachen möchten, [verwenden Sie SQL Server Management Studio](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs).

### <a name="if-i-select-a-sql-server-will-future-databases-automatically-be-added"></a>Wenn ich einen Computer mit SQL Server auswähle, werden zukünftige Datenbanken dann automatisch hinzugefügt?

Nein. Wenn Sie Schutz für einen Computer mit SQL Server konfigurieren und das Kontrollkästchen auf Serverebene aktivieren, werden alle Datenbanken hinzugefügt. Wenn Sie jedoch nach der Konfiguration des Schutzes Datenbanken zum Computer mit SQL Server hinzufügen, müssen Sie die neuen Datenbanken manuell hinzufügen, um sie zu schützen. Die Datenbanken werden nicht automatisch in den konfigurierten Schutz eingeschlossen.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Wie kann ich den Schutz neu starten, wenn ich das Wiederherstellungsmodell geändert habe?

Wenn Sie das Wiederherstellungsmodell ändern, lösen Sie eine vollständige Sicherung aus, und die Protokollsicherungen beginnen dann wie erwartet.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über Azure Backup, und sehen Sie sich das PowerShell-Beispiel zum Sichern verschlüsselter virtueller Computer an.

> [!div class="nextstepaction"]
> [Sichern verschlüsselter virtueller Computer](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
