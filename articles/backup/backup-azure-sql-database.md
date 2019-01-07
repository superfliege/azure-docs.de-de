---
title: Sichern von SQL Server-Datenbanken in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie SQL Server in Azure sichern. In diesem Artikel wird auch die SQL Server-Wiederherstellung beschrieben.
services: backup
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/02/2018
ms.author: anuragm
ms.custom: ''
ms.openlocfilehash: e2e6742fb3eda0523c7333451e836beb069e57ca
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53410362"
---
# <a name="back-up-sql-server-databases-to-azure"></a>Sichern von SQL Server-Datenbanken in Azure

SQL Server-Datenbanken sind kritische Workloads, die eine niedrige Recovery Point Objective (RPO) und eine Langzeitaufbewahrung erfordern. Azure Backup bietet eine SQL Server-Sicherungslösung, die keine Infrastruktur benötigt, sodass kein komplexer Sicherungsserver, kein Verwaltungs-Agent und kein Sicherungsspeicher verwaltet werden muss. Azure Backup bietet eine zentrale Verwaltung Ihrer Sicherungen über alle SQL Server-Instanzen oder sogar über verschiedene Workloads hinweg.

In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Voraussetzungen für die Sicherung einer SQL Server-Instanz in Azure
> * Erstellen und Verwenden eines Recovery Services-Tresors
> * Konfigurieren von SQL Server-Datenbanksicherungen
> * Festlegen einer Richtlinie zur Sicherung (oder Aufbewahrung) für die Wiederherstellungspunkte
> * Wiederherstellen der Datenbank

Bevor Sie mit den Prozeduren in diesem Artikel beginnen, benötigen Sie eine SQL Server-Instanz, die in Azure ausgeführt wird. [Mit SQL-Marketplace-VMs können Sie schnell eine SQL Server-Instanz erstellen.](../sql-database/sql-database-get-started-portal.md)

## <a name="public-preview-limitations"></a>Einschränkungen der Public Preview

Die Public Preview unterliegt den folgenden Einschränkungen:

- Die SQL-VM benötigt eine Internetverbindung, um auf öffentliche IP-Adressen von Azure zuzugreifen. Weitere Informationen finden Sie unter [Herstellen der Netzwerkverbindung](backup-azure-sql-database.md#establish-network-connectivity).
- Sie können bis zu 2.000 SQL-Datenbanken in einem Recovery Services-Tresor sichern. Zusätzliche SQL-Datenbank-Instanzen sollten in einem separaten Recovery Services-Tresor gespeichert werden.
- [Für die Sicherung verteilter Verfügbarkeitsgruppen gelten Einschränkungen.](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017)
- SQL Server Always On-Failoverclusterinstanzen werden nicht unterstützt.
- Verwenden Sie das Azure-Portal, um Azure Backup zum Schutz von SQL Server-Datenbanken zu konfigurieren. Azure PowerShell, Azure CLI und REST-APIs werden derzeit nicht unterstützt.
- Sicherungs-und Wiederherstellungsvorgänge für Spiegeldatenbanken, Datenbankmomentaufnahmen und Datenbanken auf Failoverclusterinstanzen werden nicht unterstützt.
- Datenbanken, die eine erhebliche Anzahl von Dateien enthalten, können nicht geschützt werden. Die maximale Anzahl von unterstützten Dateien ist nicht deterministisch, da sie nicht nur von der Anzahl der Dateien, sondern auch von der Pfadlänge der Dateien abhängt. Dieser Fall tritt jedoch selten ein. Wir arbeiten an einer Lösung für diesen Umstand.

Weitere Informationen zu unterstützten bzw. nicht unterstützten Szenarien finden Sie im [Abschnitt mit den häufig gestellten Fragen](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#faq).

## <a name="support-for-azure-geos"></a>Unterstützte Azure-Regionen

Azure Backup wird für die folgenden geografischen Regionen unterstützt:

- Australien, Südosten (ASE)
- Brasilien, Süden (BRS)
- Kanada, Mitte (CNC)
- Kanada, Osten (CE)
- USA, Mitte (CUS)
- Asien, Osten (EA)
- Australien, Osten (AE)
- USA, Osten (EUS)
- USA, Osten 2 (EUS2)
- Indien, Mitte (INC)
- Indien, Süden (INS)
- Japan, Osten (JPE)
- Japan, Westen (JPW)
- Südkorea, Mitte (KRC)
- Korea, Süden (KRS)
- USA, Norden-Mitte (NCUS)
- Europa, Norden (NE)
- USA, Süden-Mitte (SCUS)
- Asien, Südosten (SEA)
- Vereinigtes Königreich, Süden (UKS)
- Vereinigtes Königreich, Westen (UKW)
- USA, Westen-Mitte (WCUS)
- Europa, Westen (WE)
- USA, Westen (WUS)
- USA, Westen 2 (WUS 2)

## <a name="support-for-operating-systems-and-sql-server-versions"></a>Unterstützte Betriebssysteme und SQL Server-Versionen

Dieser Abschnitt beschreibt die von Azure Backup unterstützten Betriebssysteme und SQL Server-Versionen. SQL-Marketplace-VMs in Azure und Nicht-Marketplace-VMs (in denen SQL Server manuell installiert ist) werden unterstützt.

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux wird derzeit nicht unterstützt.

### <a name="supported-sql-server-versions-and-editions"></a>Unterstützte SQL Server-Versionen und -Editionen

- SQL Server 2012 Enterprise, Standard, Web, Developer, Express
- SQL Server 2014 Enterprise, Standard, Web, Developer, Express
- SQL Server 2016 Enterprise, Standard, Web, Developer, Express
- SQL Server 2017 Enterprise, Standard, Web, Developer, Express

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Ihre SQL Server-Datenbank sichern können, müssen folgende Bedingungen erfüllt sein:

- Identifizieren oder [erstellen Sie einen Recovery Services-Tresor](backup-azure-sql-database.md#create-a-recovery-services-vault) in derselben Region bzw. demselben Gebietsschema wie die VM, die Ihre SQL Server-Instanz hostet.
- [Überprüfen Sie die Berechtigungen auf der VM](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms), die für die Sicherung von SQL-Datenbanken erforderlich sind.
- Überprüfen Sie, ob [die SQL-VM eine Netzwerkverbindung hat](backup-azure-sql-database.md#establish-network-connectivity).
- Überprüfen Sie, ob die SQL-Datenbanken gemäß den [Benennungsrichtlinien](backup-azure-sql-database.md#sql-database-naming-guidelines-for-azure-backup) für Azure Backup benannt wurden. Nur dann können Sicherungen erfolgreich erstellt werden.

> [!NOTE]
> Sie können jeweils nur eine Sicherungslösung nutzen, um SQL Server-Datenbanken zu sichern. Deaktivieren Sie alle anderen SQL-Sicherungen, bevor Sie diese Funktion verwenden – andernfalls werden die Sicherungen beeinträchtigt und schlagen fehl. Sie können Azure Backup für IaaS-VMs zusammen mit der SQL-Sicherungslösung ohne Konflikte aktivieren.
>

Wenn diese Bedingungen in Ihrer Umgebung vorhanden sind, fahren Sie mit [Konfigurieren der Sicherung für SQL Server-Datenbanken](backup-azure-sql-database.md#configure-backup-for-sql-server-databases) fort. Wenn eine der Voraussetzungen nicht erfüllt wird, lesen Sie weiter.


## <a name="establish-network-connectivity"></a>Herstellen der Netzwerkverbindung

Für alle Vorgänge benötigt der virtuelle SQL-Computer eine Verbindung, um auf die öffentlichen IP-Adressen von Azure zuzugreifen. SQL-VM-Vorgänge (z. B. Ermitteln von Datenbanken, Konfigurieren von Sicherungen, Planen von Sicherungen, Zurücksetzen von Wiederherstellungspunkten) schlagen ohne Verbindung zu öffentlichen IP-Adressen fehl. Verwenden Sie eine der folgenden Optionen, um einen eindeutigen Pfad für Sicherungsdatenverkehr bereitzustellen:

- Setzen Sie die IP-Bereiche der Azure-Rechenzentren auf die Whitelist: Ausführliche Informationen und Anweisungen zur Aufnahme der IP-Adressbereiche der Azure-Rechenzentren in eine Whitelist finden Sie auf der [Download Center-Website](https://www.microsoft.com/download/details.aspx?id=41653).
- Bereitstellen eines HTTP-Proxyservers für das Routing des Datenverkehrs: Wenn Sie eine SQL-Datenbank auf einer VM sichern, verwendet die Sicherungserweiterung auf der VM HTTPS-APIs, um Verwaltungsbefehle an Azure Backup und Daten an Azure Storage zu senden. Die Sicherungserweiterung verwendet auch Azure Active Directory (Azure AD) zur Authentifizierung. Leiten Sie den Datenverkehr der Sicherungserweiterung für diese drei Dienste über den HTTP-Proxy weiter. Die Erweiterung ist die einzige Komponente, die für den Zugriff auf das öffentliche Internet konfiguriert ist.

Sie müssen zwischen den folgenden Eigenschaften abwägen: Verwaltbarkeit, detaillierte Steuerung und Kosten.

> [!NOTE]
> Diensttags für Azure Backup sollten allgemein verfügbar sein.
>

| Option | Vorteile | Nachteile |
| ------ | ---------- | ------------- |
| Whitelist der IP-Bereiche | Keine zusätzlichen Kosten. <br/> Verwenden Sie zum Öffnen des Zugriffs in einer Netzwerksicherheitsgruppe (NSG) das Cmdlet **Set-AzureNetworkSecurityRule**. | Komplexe Verwaltung, da sich die betroffenen IP-Bereiche im Laufe der Zeit ändern. <br/>Zugriff auf alle Azure-Dienste, nicht nur auf Azure Storage.|
| Verwenden eines HTTP-Proxys   | Feinsteuerung im Proxy über die Speicher-URLs ist zulässig. <br/>Zentraler Punkt für Internetzugriff auf virtuelle Computer. <br/> Unterliegt keinen Azure-IP-Adressänderungen. | Zusätzliche Kosten für das Ausführen einer VM mit Proxysoftware. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Festlegen von Berechtigungen für Nicht-Marketplace-SQL-VMs

Zum Sichern einer VM benötigt Azure Backup die **AzureBackupWindowsWorkload**-Erweiterung. Wenn Sie Azure Marketplace-VMs verwenden, fahren Sie mit [Ermitteln von SQL Server-Datenbanken](backup-azure-sql-database.md#discover-sql-server-databases) fort. Wenn die VM, die Ihre SQL-Datenbanken hostet, nicht im Azure Marketplace erstellt wurde, gehen Sie wie folgt vor, um die Erweiterung zu installieren und die entsprechenden Berechtigungen festzulegen. Zusätzlich zur **AzureBackupWindowsWorkload**-Erweiterung benötigt Azure Backup Systemadministratorberechtigungen für SQL, um SQL-Datenbanken zu schützen. Azure Backup erstellt das Konto **NT Service\AzureWLBackupPluginSvc**, um Datenbanken auf der VM zu ermitteln. Dieses Konto wird zum Sichern und Wiederherstellen verwendet und muss über SQL-Systemadministratorberechtigungen verfügen. Darüber hinaus verwendet Azure Backup das Konto **NT AUTHORITY\SYSTEM** für die Ermittlung von Datenbanken und Anfragen an Datenbanken. Dieses Konto muss also über eine öffentliche Anmeldung in SQL verfügen.

So konfigurieren Sie Berechtigungen

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) den Recovery Services-Tresor, mit dem Sie SQL-Datenbanken schützen.

2. Wählen Sie auf dem Dashboard des **Recovery Services-Tresors** **Sichern** aus. Das Menü **Sicherungsziel** wird geöffnet.

   ![Auswählen von „Sichern“, um Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Übernehmen Sie im Menü **Sicherungsziel** für **Wo wird die Workload ausgeführt?** die Standardeinstellung: **Azure**.

4. Erweitern Sie das Dropdown-Listenfeld **Was möchten Sie sichern?** und wählen Sie **SQL Server in Azure-VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“ für die Sicherung](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Im Menü **Sicherungsziel** werden zwei Schritte angezeigt: **DBs in VMs ermitteln** und **Sicherung konfigurieren**. Über **DBs in VMs ermitteln** wird eine Suche nach Azure-VMs gestartet.

    ![Überprüfen der beiden Sicherungszielschritte](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Wählen Sie unter **DBs in VMs ermitteln** **Ermittlung starten** aus, um nach nicht geschützten VMs im Abonnement zu suchen. Es dauert eine Weile, alle VMs zu durchsuchen. Die Dauer der Suche hängt von der Anzahl von ungeschützten VMs im Abonnement ab.

    ![Ausstehende Sicherung bei der Suche nach Datenbanken in VMs](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Wählen Sie aus der Liste der virtuellen Computer die VM aus, die die zu sichernde SQL-Datenbank enthält, und wählen Sie **DBs ermitteln** aus.

    Beim Ermittlungsprozess wird die **AzureBackupWindowsWorkload**-Erweiterung auf dem virtuellen Computer installiert. Die Erweiterung ermöglicht es dem Azure Backup-Dienst, mit dem virtuellen Computer zu kommunizieren, um die SQL-Datenbank-Instanzen zu sichern. Nachdem die Erweiterung installiert wurde, erstellt Azure Backup das virtuelle Windows-Dienstkonto **NT Service\AzureWLBackupPluginSvc** auf der VM. Für das virtuelle Dienstkonto sind SQL-Systemadministratorberechtigungen erforderlich. Wenn bei der Installation des virtuellen Dienstkontos der Fehler `UserErrorSQLNoSysadminMembership` auftritt, finden Sie nützliche Informationen unter [Beheben von Problemen mit SQL-Systemadministratorberechtigungen](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

    Der Bereich **Benachrichtigungen** zeigt den Status der Datenbankermittlung an. Es dauert eine Weile, bis der Auftrag abgeschlossen ist. Die Dauer hängt davon ab, wie viele Datenbanken sich auf der VM befinden. Wenn die ausgewählten Datenbanken ermittelt wurden, wird eine Erfolgsmeldung angezeigt.

    ![Meldung über erfolgreiche Bereitstellung](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nachdem Sie die Datenbank mit dem Recovery Services-Tresor verknüpft haben, ist der nächste Schritt das [Konfigurieren des Sicherungsauftrags](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="fix-sql-sysadmin-permissions"></a>Beheben von Problemen mit SQL-Systemadministratorberechtigungen

Wenn bei der Installation der Fehler `UserErrorSQLNoSysadminMembership` angezeigt wird, verwenden Sie ein Konto mit SQL Server-Systemadministratorberechtigungen, um sich bei SQL Server Management Studio (SSMS) anzumelden. Wenn Sie keine speziellen Berechtigungen benötigen, sollte die Windows-Authentifizierung funktionieren.

1. Öffnen Sie auf der SQL Server-Instanz den Ordner „Sicherheit/Anmeldungen“.

    ![Öffnen von Ordner „Sicherheit/Anmeldungen“, um Konten anzuzeigen](./media/backup-azure-sql-database/security-login-list.png)

2. Klicken Sie mit der rechten Maustaste auf den Ordner „Anmeldungen“, und wählen Sie **Neue Anmeldung** aus. Wählen Sie im Dialogfeld **Anmeldung – Neu** **Suchen** aus.

    ![Auswählen von „Suchen“ im Dialogfeld „Anmeldung – Neu“](./media/backup-azure-sql-database/new-login-search.png)

3. Das virtuelle Windows-Dienstkonto **NT Service\AzureWLBackupPluginSvc** wurde bei der Registrierung der VM und der SQL-Ermittlungsphase erstellt. Geben Sie den Kontonamen ein, wie im Feld **Namen des auszuwählenden Objekts eingeben** dargestellt. Wählen Sie **Namen überprüfen** aus, um den Namen aufzulösen.

    ![Klicken auf „Namen überprüfen“, um den unbekannten Dienstnamen aufzulösen](./media/backup-azure-sql-database/check-name.png)

4. Wählen Sie **OK** aus, um das Dialogfeld zu schließen.

5. Überprüfen Sie, ob im Dialogfeld **Serverrollen** die Rolle **Sysadmin** ausgewählt ist. Wählen Sie **OK** aus, um das Dialogfeld zu schließen.

    ![Sicherstellen, dass die Serverrolle „Sysadmin“ ausgewählt ist](./media/backup-azure-sql-database/sysadmin-server-role.png)

    Die erforderlichen Berechtigungen sollten jetzt vorhanden sein.

6. Obwohl Sie den Berechtigungsfehler behoben haben, müssen Sie die Datenbank mit dem Recovery Services-Tresor verknüpfen. Klicken Sie im Azure-Portal in der Liste **Geschützte Server** mit der rechten Maustaste auf den fehlerhaften Server, und wählen Sie **Datenbanken neu ermitteln** aus.

    ![Überprüfen der entsprechenden Serverberechtigungen](./media/backup-azure-sql-database/check-erroneous-server.png)

    Der Bereich **Benachrichtigungen** zeigt den Status der Datenbankermittlung an. Es dauert eine Weile, bis der Auftrag abgeschlossen ist. Die Dauer hängt davon ab, wie viele Datenbanken sich auf der VM befinden. Wenn die ausgewählten Datenbanken gefunden wurden, wird eine Erfolgsmeldung angezeigt.

    ![Meldung über erfolgreiche Bereitstellung](./media/backup-azure-sql-database/notifications-db-discovered.png)

Nachdem Sie die Datenbank mit dem Recovery Services-Tresor verknüpft haben, ist der nächste Schritt das [Konfigurieren des Sicherungsauftrags](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

## <a name="sql-database-naming-guidelines-for-azure-backup"></a>Benennungsrichtlinien für SQL-Datenbanken in Azure Backup
Vermeiden Sie bei der Benennung von Datenbanken Folgendes, um reibungslose Sicherungen mit Azure Backup für SQL Server in IaaS-VMs zu ermöglichen:

  * Nachgestellte/führende Leerzeichen
  * Führende Ausrufezeichen

Obwohl ein Aliasing für nicht unterstützte Zeichen aus Azure-Tabellen möglich ist, wird empfohlen, diese Zeichen zu vermeiden. Weitere Informationen finden Sie in [diesem Artikel](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Ermitteln von SQL Server-Datenbanken

Azure Backup ermittelt alle Datenbanken einer SQL Server-Instanz. Sie können Datenbanken entsprechend Ihren Sicherungsanforderungen schützen. Gehen Sie wie folgt vor, um die VM zu identifizieren, die die SQL-Datenbanken hostet. Nachdem Sie die VM identifiziert haben, installiert Azure Backup eine einfache Erweiterung, um die SQL Server-Datenbanken zu ermitteln.

1. Melden Sie sich bei Ihrem Abonnement im [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie im Menü links **Alle Dienste** aus.

    ![Auswählen von „Alle Dienste“](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Geben Sie im Dialogfeld **Alle Dienste** **Recovery Services** ein. Bei der Eingabe wird die Ressourcenliste anhand Ihrer Eingabe gefiltert. Wählen Sie aus der Liste **Recovery Services-Tresore** aus.

  ![Eingeben und Auswählen von „Recovery Services-Tresore“](./media/backup-azure-sql-database/all-services.png) <br/>

    Die Liste mit den Recovery Services-Tresoren im Abonnement wird angezeigt.

4. Wählen Sie in der Liste der Recovery Services-Tresore den Tresor aus, mit dem Sie Ihre SQL-Datenbanken schützen möchten.

5. Wählen Sie auf dem Dashboard des **Recovery Services-Tresors** **Sichern** aus. Das Menü **Sicherungsziel** wird geöffnet.

   ![Auswählen von „Sichern“, um Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

6. Übernehmen Sie im Menü **Sicherungsziel** für **Wo wird die Workload ausgeführt?** die Standardeinstellung: **Azure**.

7. Erweitern Sie das Dropdown-Listenfeld **Was möchten Sie sichern?** und wählen Sie **SQL Server in Azure-VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“ für die Sicherung](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Im Menü **Sicherungsziel** werden zwei Schritte angezeigt: **DBs in VMs ermitteln** und **Sicherung konfigurieren**.

    ![Überprüfen der beiden Sicherungszielschritte](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Wählen Sie unter **DBs in VMs ermitteln** **Ermittlung starten** aus, um nach nicht geschützten VMs im Abonnement zu suchen. Es dauert eine Weile, alle VMs zu durchsuchen. Die Dauer der Suche hängt von der Anzahl von ungeschützten VMs im Abonnement ab.

    ![Ausstehende Sicherung bei der Suche nach Datenbanken in VMs](./media/backup-azure-sql-database/discovering-sql-databases.png)

    Wenn eine ungeschützte VM entdeckt wurde, erscheint sie in der Liste. Mehrere virtuelle Computer können den gleichen Namen haben. Virtuelle Computer mit dem gleichen Namen gehören jedoch verschiedenen Ressourcengruppen an. Ungeschützte virtuelle Computer werden nach ihrem Namen und ihrer Ressourcengruppe aufgelistet. Wenn eine erwartete VM nicht aufgeführt ist, überprüfen Sie, ob sie bereits in einem Tresor geschützt ist.

9. Wählen Sie aus der Liste der virtuellen Computer die VM aus, die die zu sichernde SQL-Datenbank enthält, und wählen Sie **DBs ermitteln** aus.

    Azure Backup ermittelt alle SQL-Datenbanken auf dem virtuellen Computer. Weitere Informationen dazu, was bei der Datenbankermittlung geschieht, finden Sie unter [Vorgänge im Hintergrund](backup-azure-sql-database.md#background-operations). Nachdem Sie die SQL-Datenbanken ermittelt haben, können Sie den [Sicherungsauftrag konfigurieren](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="background-operations"></a>Vorgänge im Hintergrund

Wenn Sie das Tool **DBs ermitteln** verwenden, führt Azure Backup die folgenden Vorgänge im Hintergrund aus:

- Registrierung des virtuellen Computers beim Recovery Services-Tresor für die Workloadsicherung. Alle Datenbanken auf dem registrierten virtuellen Computer können nur in diesem Recovery Services-Tresor gesichert werden.

- Installation der Erweiterung **AzureBackupWindowsWorkload** auf dem virtuellen Computer. Das Sichern einer SQL-Datenbank ist eine Lösung ohne Agent. Die Erweiterung wird auf dem virtuellen Computer installiert und auf der SQL-Datenbank wird kein Agent installiert.

- Erstellung des Dienstkontos **NT Service\AzureWLBackupPluginSvc** auf der VM. Für alle Sicherungs- und Wiederherstellungsvorgänge wird das Dienstkonto verwendet. **NT Service\AzureWLBackupPluginSvc** erfordert SQL-Systemadministratorberechtigungen. Auf allen SQL-Marketplace-VMs ist **SqlIaaSExtension** installiert. Die Erweiterung **AzureBackupWindowsWorkload** verwendet **SQLIaaSExtension**, um erforderliche Berechtigungen automatisch abzurufen. Wenn auf Ihrem virtuellen Computer **SqlIaaSExtension** nicht installiert ist, tritt bei der Datenbankermittlung die Fehlermeldung `UserErrorSQLNoSysAdminMembership` auf. Um die Systemadministratorberechtigung für Sicherungen hinzuzufügen, folgen Sie den Anweisungen unter [Festlegen von Azure Backup-Berechtigungen für Nicht-Marketplace-SQL-VMs](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

    ![Auswählen der VM und der Datenbank](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-databases"></a>Konfigurieren der Sicherung für SQL Server-Datenbanken

Azure Backup bietet Verwaltungsdienste zum Schutz Ihrer SQL Server-Datenbanken und zur Verwaltung von Sicherungsaufträgen. Die Verwaltungs- und Überwachungsfunktionen hängen von Ihrem Recovery Services-Tresor ab.

> [!NOTE]
> Sie können jeweils nur eine Sicherungslösung nutzen, um SQL Server-Datenbanken zu sichern. Deaktivieren Sie alle anderen SQL-Sicherungen, bevor Sie diese Funktion verwenden – andernfalls werden die Sicherungen beeinträchtigt und schlagen fehl. Sie können Azure Backup für IaaS-VMs zusammen mit der SQL-Sicherungslösung ohne Konflikte aktivieren.
>

So konfigurieren Sie den Schutz für Ihre SQL-Datenbank

1. Öffnen Sie den Recovery Services-Tresor, der mit der SQL-VM registriert ist.

2. Wählen Sie auf dem Dashboard des **Recovery Services-Tresors** **Sichern** aus. Das Menü **Sicherungsziel** wird geöffnet.

   ![Auswählen von „Sichern“, um Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Übernehmen Sie im Menü **Sicherungsziel** für **Wo wird die Workload ausgeführt?** die Standardeinstellung: **Azure**.

4. Erweitern Sie das Dropdown-Listenfeld **Was möchten Sie sichern?** und wählen Sie **SQL Server in Azure-VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“ für die Sicherung](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Im Menü **Sicherungsziel** werden zwei Schritte angezeigt: **DBs in VMs ermitteln** und **Sicherung konfigurieren**.

    Wenn Sie die Anleitung in der richtigen Reihenfolge befolgt haben, haben Sie die ungeschützten virtuellen Computer ermittelt, und dieser Tresor ist bei einem virtuellen Computer registriert. Nun können Sie den Schutz für die SQL-Datenbanken konfigurieren.

5. Wählen Sie im Menü **Sicherungsziel** **Sicherung konfigurieren** aus.

    ![Auswählen von „Sicherung konfigurieren“](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    Der Azure Backup-Dienst zeigt alle SQL Server-Instanzen mit eigenständigen Datenbanken sowie SQL Server Always On-Verfügbarkeitsgruppen an. Wählen Sie das Erweiterungschevron links neben dem Instanznamen aus, um die eigenständigen Datenbanken in der SQL Server-Instanz anzuzeigen. Wählen Sie ebenso das Chevron links neben der Always On-Verfügbarkeitsgruppe aus, um die Datenbankliste anzuzeigen. Auf der folgenden Abbildung wird ein Beispiel für eine eigenständige Instanz und eine Always On-Verfügbarkeitsgruppe dargestellt.

      ![Anzeigen aller SQL Server-Instanzen mit eigenständigen Datenbanken](./media/backup-azure-sql-database/list-of-sql-databases.png)

6. Wählen Sie in der Liste der Datenbanken alle Datenbanken aus, die Sie schützen möchten, und klicken Sie auf **OK**.

    ![Schützen der Datenbank](./media/backup-azure-sql-database/select-database-to-protect.png)

    Sie können bis zu 50 Datenbanken gleichzeitig auswählen. Um mehr als 50 Datenbanken zu schützen, wiederholen Sie den Vorgang mehrmals. Nachdem Sie die ersten 50 Datenbanken geschützt haben, wiederholen Sie diesen Schritt, um die nächsten Gruppe von Datenbanken zu schützen.

    > [!Note]
    > Um die Sicherungslasten zu optimieren, teilt Azure Backup große Sicherungsaufträge in mehrere Batches auf. Ein Sicherungsauftrag kann maximal 50 Datenbanken umfassen.
    >

      Alternativ können Sie durch Auswahl der Option **ON** (EIN) in der entsprechenden Dropdownliste in der Spalte **AUTOPROTECT** den [automatischen Schutz](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) für die gesamte Instanz oder Always On-Verfügbarkeitsgruppe aktivieren. Das Feature für den [automatischen Schutz](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) ermöglicht nicht nur den gleichzeitigen Schutz aller vorhandenen Datenbanken, sondern schützt auch automatisch alle neuen Datenbanken, die dieser Instanz oder Verfügbarkeitsgruppe in Zukunft hinzugefügt werden.  

      ![Aktivieren des automatischen Schutzes für die AlwaysOn-Verfügbarkeitsgruppe](./media/backup-azure-sql-database/enable-auto-protection.png)

7. Zum Erstellen oder Auswählen einer Sicherungsrichtlinie wählen Sie im Menü **Sicherung** **Sicherungsrichtlinie** aus. Das Menü **Sicherungsrichtlinie** wird geöffnet.

    ![Auswählen der Sicherungsrichtlinie](./media/backup-azure-sql-database/select-backup-policy.png)

8. Wählen Sie in der Dropdownliste **Sicherungsrichtlinie auswählen** eine Sicherungsrichtlinie aus, und wählen Sie dann **OK** aus. Weitere Informationen zum Erstellen einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-sql-database.md#define-a-backup-policy).

   > [!NOTE]
   > Sicherungsrichtlinien können während der Vorschauphase nicht bearbeitet werden. Wenn die in der Liste verfügbaren Richtlinien für Sie nicht ausreichen, müssen Sie selbst eine Richtlinie erstellen. Informationen zum Erstellen einer neuen Sicherungsrichtlinie finden Sie im Abschnitt [Definieren einer Sicherungsrichtlinie](backup-azure-sql-database.md#define-a-backup-policy).

    ![Auswählen einer Sicherungsrichtlinie aus der Liste](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    Im Menü **Sicherungsrichtlinie** haben Sie im Dropdown-Listenfeld **Sicherungsrichtlinie auswählen** folgende Optionen:
    - Auswahl der Standardrichtlinie: **HourlyLogBackup**.
    - Auswählen einer vorhandenen, zuvor für SQL erstellten Sicherungsrichtlinie
    - [Definieren einer neuen Richtlinie](backup-azure-sql-database.md#define-a-backup-policy) basierend auf Ihrer RPO und Ihrer Beibehaltungsdauer

    > [!Note]
    > Azure Backup unterstützt die Langzeitaufbewahrung, die auf dem Generationenprinzip (Großvater-Vater-Sohn-Prinzip) basiert. Dieses Prinzip optimiert die Back-End-Speicherung und erfüllt die Complianceanforderungen.
    >

9. Nachdem Sie eine Sicherungsrichtlinie ausgewählt haben, wählen Sie im Menü **Sicherung** **Sicherung aktivieren** aus.

    ![Aktivieren der ausgewählten Sicherungsrichtlinie](./media/backup-azure-sql-database/enable-backup-button.png)

    Den Konfigurationsprozess können Sie im Portalbereich **Benachrichtigungen** nachverfolgen.

    ![Benachrichtigungsbereich](./media/backup-azure-sql-database/notifications-area.png)


## <a name="auto-protect-sql-server-in-azure-vm"></a>Automatischer Schutz von SQL Server in Azure-VMs  

Durch den automatischen Schutz können Sie automatisch alle vorhandenen Datenbanken und alle Datenbanken schützen, die Sie zukünftig zu eigenständigen SQL Server-Instanzen oder SQL Server Always On-Verfügbarkeitsgruppen hinzufügen. Wenn Sie den automatischen Schutz **aktivieren** und eine Sicherungsrichtlinie auswählen, gilt diese für neu geschützte Datenbanken. Für vorhandene geschützte Datenbanken wird weiterhin die vorherige Richtlinie verwendet.

![Aktivieren des automatischen Schutzes für die AlwaysOn-Verfügbarkeitsgruppe](./media/backup-azure-sql-database/enable-auto-protection.png)

Es gibt keine Beschränkung für die Anzahl der Datenbanken, die mithilfe des Features für den automatischen Schutz gleichzeitig ausgewählt werden können. Die Option „Sicherung konfigurieren“ wird für alle Datenbanken ausgeführt und kann unter **Sicherungsaufträge** nachverfolgt werden.

Wenn Sie den automatischen Schutz für eine Instanz deaktivieren müssen, klicken Sie unter **Sicherung konfigurieren** auf den Namen der Instanz, um den Informationsbereich auf der rechten Seite anzuzeigen. Dort finden Sie am oberen Rand die Option **Automatischen Schutz deaktivieren**. Klicken Sie auf **Automatischen Schutz deaktivieren**, um den automatischen Schutz für diese Instanz zu deaktivieren.

![Deaktivieren des automatischen Schutzes für diese Instanz](./media/backup-azure-sql-database/disable-auto-protection.png)

Alle Datenbanken in dieser Instanz werden weiterhin geschützt. Dieser Vorgang deaktiviert jedoch automatischen Schutz für alle Datenbanken, die in der Zukunft hinzugefügt werden.

### <a name="define-a-backup-policy"></a>Definieren einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie definiert eine Matrix dafür, wann Sicherungen erstellt und wie lange sie aufbewahrt werden. Mit Azure Backup können Sie drei Arten von Sicherungen für SQL-Datenbanken planen:

* Vollständige Sicherung: Die gesamte Datenbank wird gesichert. Eine vollständige Sicherung enthält alle Daten in einer bestimmten Datenbank oder einem bestimmten Satz von Dateigruppen oder Dateien sowie ausreichende Protokolle, um diese Daten wiederherzustellen. Pro Tag kann höchstens eine vollständige Sicherung ausgelöst werden. Sie können wählen, ob Sie eine vollständige Sicherung in einem täglichen oder wöchentlichen Intervall durchführen möchten.
* Differenzielle Sicherung: Diese Sicherung basiert auf der letzten vollständigen Datensicherung. Bei einer differenziellen Sicherung werden nur die Daten erfasst, die seit der vollständigen Sicherung geändert wurden. Pro Tag kann höchstens eine differenzielle Sicherung ausgelöst werden. Sie können eine vollständige Sicherung und eine differenzielle Sicherung nicht am gleichen Tag konfigurieren.
* Sicherung des Transaktionsprotokolls: Eine Zeitpunktwiederherstellung ist bis zu einer bestimmten Sekunde möglich. Transaktionsprotokollsicherungen können höchstens alle 15 Minuten durchgeführt werden.

Die Richtlinie wird auf Ebene des Recovery Services-Tresors erstellt. Mehrere Tresore können die gleiche Sicherungsrichtlinie verwenden. Allerdings müssen Sie die Sicherungsrichtlinie auf jeden Tresor anwenden. Wenn Sie eine Sicherungsrichtlinie erstellen, entspricht die tägliche vollständige Sicherung der Standardeinstellung. Sie können eine differenzielle Sicherung nur hinzufügen, wenn Sie für die vollständige Sicherung festlegen, dass diese wöchentlich erfolgt. Die folgende Vorgehensweise erklärt, wie Sie eine Sicherungsrichtlinie für eine SQL Server-Instanz in einem virtuellen Azure-Computer erstellen.

> [!NOTE]
> In der Vorschauphase können Sie keine Sicherungsrichtlinien bearbeiten. Stattdessen müssen Sie eine neue Richtlinie mit den gewünschten Details erstellen.  

So erstellen Sie eine Sicherungsrichtlinie

1. Klicken Sie im Recovery Services-Tresor, der die SQL-Datenbank schützt, auf **Sicherungsrichtlinien** und dann auf **Hinzufügen**.

   ![Öffnen des Dialogfelds zum Erstellen einer neuen Sicherungsrichtlinie](./media/backup-azure-sql-database/new-policy-workflow.png)

   Das Menü **Hinzufügen** wird angezeigt.

2. Klicken Sie im Menü **Hinzufügen** auf **SQL Server in Azure-VM**.

   ![Auswählen eines Richtlinientyps für die neue Sicherungsrichtlinie](./media/backup-azure-sql-database/policy-type-details.png)

   Beim Auswählen von „SQL Server in Azure-VM“ wird der Richtlinientyp definiert, und das Menü „Sicherungsrichtlinie“ wird geöffnet. Im Menü **Sicherungsrichtlinie** werden die erforderlichen Felder für jede neue SQL Server-Sicherungsrichtlinie angezeigt.

3. Geben Sie unter **Richtlinienname** einen Namen für die neue Richtlinie ein.

4. Eine vollständige Sicherung ist obligatorisch: Sie können die Option **Vollständige Sicherung** nicht deaktivieren. Klicken Sie auf **Vollständige Sicherung**, um die Richtlinie anzuzeigen und zu bearbeiten. Auch wenn Sie die Sicherungsrichtlinie nicht ändern, sollten Sie die Richtliniendetails anzeigen.

    ![Neue Felder für Sicherungsrichtlinien](./media/backup-azure-sql-database/full-backup-policy.png)

    Wählen Sie im Menü **Richtlinie für vollständige Sicherung** für **Sicherungshäufigkeit** **Täglich** oder **Wöchentlich** aus. Wählen Sie für **Täglich** die Uhrzeit und die Zeitzone für den Beginn des Sicherungsauftrags aus. Sie können keine differenziellen Sicherungen für tägliche vollständige Sicherungen erstellen.

   ![Einstellung für das tägliche Intervall](./media/backup-azure-sql-database/daily-interval.png)

    Wählen Sie für **Wöchentlich** den Wochentag, die Uhrzeit und die Zeitzone für den Beginn des Sicherungsauftrags aus.

   ![Einstellung für das wöchentliche Intervall](./media/backup-azure-sql-database/weekly-interval.png)

5. Standardmäßig sind alle Optionen für die **Beibehaltungsdauer** aktiviert: „Täglich“, „Wöchentlich“, „Monatlich“ und „Jährlich“. Deaktivieren Sie die Beibehaltungsdauer, die Sie nicht benötigen. Legen Sie die zu verwendenden Intervalle fest. Wählen Sie im Menü **Richtlinie für vollständige Sicherung** **OK** aus, um die Einstellungen zu übernehmen.

   ![Intervalleinstellungen für Beibehaltungsdauer](./media/backup-azure-sql-database/retention-range-interval.png)

    Wiederherstellungspunkte werden unter Berücksichtigung ihrer Beibehaltungsdauer mit einer Markierung versehen. Wenn Sie beispielsweise eine tägliche vollständige Sicherung wählen, wird pro Tag nur eine vollständige Sicherung ausgelöst. Die Sicherung für einen bestimmten Tag wird auf Grundlage der wöchentlichen Beibehaltungsdauer und Ihrer wöchentlichen Aufbewahrungseinstellung markiert und beibehalten. Mit der monatlichen und jährlichen Beibehaltungsdauer verhält es sich ähnlich.

6. Um eine Richtlinie für eine differenzielle Sicherung hinzuzufügen, wählen Sie **Differenzielle Sicherung** aus. Das Menü **Richtlinie für differenzielle Sicherung** wird geöffnet.

   ![Öffnen des Menüs „Richtlinie für differenzielle Sicherung“](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Wählen Sie im Menü **Richtlinie für differenzielle Sicherung** **Aktivieren** aus, um die Einstellungen für Häufigkeit und Beibehaltung zu öffnen. Pro Tag kann höchstens eine differenzielle Sicherung ausgelöst werden.

    > [!Important]
    > Differenzielle Sicherungen können maximal 180 Tage aufbewahrt werden. Wenn Sie eine längere Aufbewahrung wünschen, müssen Sie vollständige Sicherungen verwenden.
    >

   ![Bearbeiten der Richtlinie für die differenzielle Sicherung](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Wählen Sie **OK** aus, um die Richtlinie zu speichern und um zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.

7. Um eine Richtlinie für eine Transaktionsprotokollsicherung hinzuzufügen, wählen Sie **Protokollsicherung** aus. Das Menü **Protokollsicherung** wird geöffnet.

    Wählen Sie im Menü **Protokollsicherung** **Aktivieren** aus, und legen Sie die Einstellungen für Häufigkeit und Aufbewahrung fest. Transaktionsprotokollsicherungen können alle 15 Minuten erfolgen und bis zu 35 Tage aufbewahrt werden. Wählen Sie **OK** aus, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.

   ![Bearbeiten der Richtlinie für die Transaktionsprotokollsicherung](./media/backup-azure-sql-database/log-backup-policy-editor.png)

8. Legen Sie im Menü **Sicherungsrichtlinie** fest, ob die **SQL-Sicherungskomprimierung** aktiviert wird. Die Komprimierung ist standardmäßig deaktiviert.

    Auf dem Back-End verwendet Azure Backup die native SQL-Sicherungskomprimierung.

9. Nachdem Sie die Sicherungsrichtlinie bearbeitet haben, wählen Sie **OK** aus.

   ![Akzeptieren der neuen Sicherungsrichtlinie](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Wiederherstellen einer SQL-Datenbank-Instanz
Azure Backup bietet Funktionen zur Wiederherstellung einzelner Datenbanken zu einem bestimmten Datum oder einer bestimmten Uhrzeit, (bis zu einer bestimmten Sekunde) unter Verwendung von Transaktionsprotokollsicherungen. Basierend auf Ihren Wiederherstellungszeiten ermittelt Azure Backup automatisch die geeigneten vollständigen Sicherungen, differenziellen Sicherungen und die Kette von Protokollsicherungen, die für die Wiederherstellung Ihrer Daten benötigt werden.

Außerdem können Sie eine bestimmte vollständige oder differenzielle Sicherung auswählen, um sie auf einen bestimmten Wiederherstellungspunkt und nicht auf einen bestimmten Zeitpunkt anzuwenden.

### <a name="pre-requisite-before-triggering-a-restore"></a>Voraussetzungen vor dem Auslösen einer Wiederherstellung

1. Sie können die Datenbank auf einer SQL Server-Instanz in derselben Azure-Region wiederherstellen. Der Zielserver muss bei demselben Recovery Services-Tresor wie die Quelle registriert werden.  
2. Um eine mit TDE-verschlüsselte Datenbank auf einer anderen SQL Server-Instanz wiederherzustellen, stellen Sie zuerst das Zertifikat auf den Zielserver wieder her, indem Sie die [hier](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017) dokumentierten Schritte durchführen.
3. Bevor Sie eine Wiederherstellung der Datenbank „master“ auslösen, starten Sie die SQL Server-Instanz im Einzelbenutzermodus mit der Startoption `-m AzureWorkloadBackup`. Das Argument für die Option `-m` ist der Name des Clients. Nur dieser Client kann die Verbindung öffnen. Beenden Sie für alle Systemdatenbanken („model“, „master“, „msdb“) vor dem Auslösen der Wiederherstellung den SQL Agent-Dienst. Schließen Sie alle Anwendungen, die ggf. versuchen, eine Verbindung mit einer dieser Datenbanken zu belegen.

### <a name="steps-to-restore-a-database"></a>Schritte zum Wiederherstellen einer Datenbank:

1. Öffnen Sie den Recovery Services-Tresor, der bei der SQL-VM registriert ist.

2. Wählen Sie im Dashboard **Recovery Services-Tresor** unter **Verwendung** **Sicherungselemente** aus, um das Menü **Sicherungselemente** zu öffnen.

    ![Öffnen des Menüs „Sicherungselemente“](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Wählen Sie im Menü **Sicherungselemente** unter **Sicherungsverwaltungstyp** **SQL Server in Azure-VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    Das Menü **Sicherungselemente** zeigt die Liste der SQL-Datenbanken an.

4. Wählen Sie in der Liste der SQL-Datenbanken die Datenbank aus, die Sie wiederherstellen möchten.

    ![Auswählen der wiederherzustellenden Datenbank](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Bei Auswahl der Datenbank wird das entsprechende Menü geöffnet. Dieses Menü enthält Sicherungsdetails für die Datenbank wie:

    * Die ältesten und neuesten Wiederherstellungspunkte
    * Den Protokollsicherungsstatus der letzten 24 Stunden (für Datenbanken in einem vollständigen und massenprotokollierten Wiederherstellungsmodell, falls für Transaktionsprotokollsicherungen konfiguriert)

5. Wählen Sie im Menü für die ausgewählte Datenbank **Datenbank wiederherstellen** aus. Das Menü **Wiederherstellen** wird geöffnet.

    ![Auswählen von „Datenbank wiederherstellen“](./media/backup-azure-sql-database/restore-db-button.png)

    Wenn das Menü **Wiederherstellen** geöffnet wird, erscheint auch das Menü **Konfiguration wiederherstellen**. Das Menü **Konfiguration wiederherstellen** ist der erste Schritt zur Konfiguration der Wiederherstellung. Wählen Sie in diesem Menü aus, wo die Daten wiederhergestellt werden sollen. Die Optionen sind:
    - **Alternativer Standort:** Die Datenbank wird an einem alternativen Speicherort wiederhergestellt und behält die ursprüngliche Quelldatenbank bei.
    - **Datenbank überschreiben:** Die Daten werden auf derselben SQL Server-Instanz wiederhergestellt, auf der sich auch die ursprüngliche Quelle befunden hat. Dadurch wird die ursprüngliche Datenbank überschrieben.

    > [!Important]
    > Wenn die ausgewählte Datenbank zu einer Always On-Verfügbarkeitsgruppe gehört, lässt SQL Server das Überschreiben der Datenbank nicht zu. In diesem Fall ist nur die Option **Alternativer Standort** aktiviert.
    >

    ![Menü „Konfiguration wiederherstellen“](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Wiederherstellen an einem alternativen Speicherort

Hier erfahren Sie, wie Sie Daten an einem alternativen Speicherort wiederherstellen. Um die Datenbank bei der Wiederherstellung zu überschreiben, lesen Sie weiter unter [Wiederherstellen und Überschreiben der Datenbank](backup-azure-sql-database.md#restore-and-overwrite-the-database). Zu diesem Zeitpunkt ist Ihr Recovery Services-Tresor geöffnet und das Menü **Konfiguration wiederherstellen** wird angezeigt. Wenn Sie sich nicht bei diesem Schritt befinden, starten Sie bei [Wiederherstellen einer SQL­-Datenbank](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> Sie können die Datenbank auf einer SQL Server-Instanz in derselben Azure-Region wiederherstellen. Der Zielserver muss beim Recovery Services-Tresor registriert werden.
>

Im Menü **Wiederherstellungskonfiguration** zeigt das Dropdown-Listenfeld **Server** nur die SQL Server-Instanzen an, die beim Recovery Services-Tresor registriert sind. Wenn sich der gewünschte Server nicht in der Liste befindet, lesen Sie in Abschnitt [Ermitteln von SQL Server-Datenbanken](backup-azure-sql-database.md#discover-sql-server-databases), wie Sie den Server finden. Beim Ermittlungsprozess werden neue Server beim Recovery Services-Tresor registriert.

1. Das Menü **Konfiguration wiederherstellen** gehen Sie wie folgt vor:

    * Wählen Sie unter **Speicherort der Wiederherstellung** **Alternativer Speicherort** aus.
    * Öffnen Sie das Dropdown-Listenfeld **Server**, und wählen Sie eine SQL Server-Instanz für die Datenbankwiederherstellung aus.
    * Öffnen Sie das Dropdown-Listenfeld **Instanz** und wählen Sie eine SQL Server-Instanz aus.
    * Geben Sie im Feld **Name der wiederhergestellten Datenbank** den Namen der Zieldatenbank ein.
    * Aktivieren Sie ggf. **Überschreiben, falls auf der ausgewählten SQL-Instanz bereits eine gleichnamige Datenbank vorhanden ist**.
    * Wählen Sie **OK** aus, um die Zielkonfiguration abzuschließen und einen Wiederherstellungspunkt auszuwählen.

    ![Angeben von Werten für das Menü „Konfiguration wiederherstellen“](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** **Protokolle (Zeitpunkt)** oder **Vollständig und differenziell** aus. Wenn die Wiederherstellung auf ein Protokoll zu einem bestimmten Zeitpunkts erfolgen soll, fahren Sie mit diesem Schritt fort. Um einen Wiederherstellungspunkt mit vollständiger oder differenzieller Sicherung wiederherzustellen, machen Sie weiter mit Schritt 3.

    ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-menu.png)

    Die Point-in-Time-Wiederherstellung ist nur für Transaktionsprotokollsicherungen für Datenbanken mit einem vollständigen und massenprotokollierten Wiederherstellungsmodell verfügbar. So führen Sie die Wiederherstellung zu einem bestimmten Zeitpunkt aus:

    1. Wählen Sie **Protokolle (Zeitpunkt)** als Wiederherstellungsoption aus.

        ![Auswählen des Wiederherstellungspunkttyps](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Wählen Sie unter **Datum/Uhrzeit für Wiederherstellung** den Minikalender aus, um den **Kalender** zu öffnen. Im **Kalender** enthalten die fett angezeigten Daten Wiederherstellungspunkte. Das aktuelle Datum wird hervorgehoben. Wählen Sie ein Datum mit Wiederherstellungspunkten aus. Daten ohne Wiederherstellungspunkte können nicht ausgewählt werden.

        ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Wenn Sie ein Datum ausgewählt haben, zeigt das Zeitachsendiagramm die verfügbaren Wiederherstellungspunkte in einem fortlaufenden Bereich an.

    3. Geben Sie mithilfe des Zeitachsendiagramms oder des Dialogfelds **Zeit** einen bestimmten Zeitpunkt für den Wiederherstellungspunkt an. Wählen Sie **OK** aus, um diesen Schritt abzuschließen.

       ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        Das Menü **Wiederherstellungspunkt auswählen** wird geschlossen, und das Menü **Erweiterte Konfiguration** wird geöffnet.

       ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Gehen Sie im Menü **Erweiterte Konfiguration** wie folgt vor:

        * Damit die Datenbank nach dem Wiederherstellen nicht betriebsbereit bleibt, wählen Sie für **Mit NORECOVERY wiederherstellen** **Aktiviert** aus.
        * Um den Speicherort für die Wiederherstellung auf dem Zielserver zu ändern, geben Sie einen neuen Pfad in der Spalte **Ziel** an.
        * Wählen Sie **OK** aus, um die Einstellungen zu übernehmen. Schließen Sie das Menü **Erweiterte Konfiguration**.

    5. Wählen Sie im Menü **Wiederherstellen** **Wiederherstellen** aus, um den Wiederherstellungsauftrag zu starten. Verfolgen Sie den Fortschritt der Wiederherstellung im Bereich **Benachrichtigungen**, oder wählen Sie im Datenbankmenü **Wiederherstellungsaufträge** aus.

       ![Fortschritt des Wiederherstellungsauftrags](./media/backup-azure-sql-database/restore-job-notification.png)

3. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** **Protokolle (Zeitpunkt)** oder **Vollständig und differenziell** aus. Wenn Sie ein Protokoll zu einem bestimmten Zeitpunkt wiederherstellen möchten, gehen Sie zurück zu Schritt 2. In diesem Schritt wird ein bestimmter Wiederherstellungspunkt mit vollständiger oder differentieller Sicherung wiederhergestellt. Sie können alle vollständigen und differenziellen Wiederherstellungspunkte der letzten 30 Tage anzeigen. Wenn Sie Wiederherstellungspunkte anzeigen möchten, die älter als 30 Tage sind, wählen Sie **Filter** aus, um das Menü **Wiederherstellungspunkte filtern** zu öffnen. Wenn Sie einen differenziellen Wiederherstellungspunkt wählen, stellt Azure Backup zuerst den entsprechenden vollständigen Wiederherstellungspunkt wieder her und wendet dann den ausgewählten differenziellen Wiederherstellungspunkt an.

    ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** die Option **Vollständig und differenziell** aus.

       ![Auswählen von „Vollständig und differenziell“](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Das Menü zeigt die Liste der verfügbaren Wiederherstellungspunkte an.

    2. Wählen Sie aus der Liste einen Wiederherstellungspunkt aus, und wählen Sie **OK** aus, um die Prozedur der Wiederherstellungspunkte abzuschließen.

        ![Auswählen eines vollständigen Wiederherstellungspunkts](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        Das Menü **Wiederherstellungspunkt** wird geschlossen, und das Menü **Erweiterte Konfiguration** wird geöffnet.

        ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Gehen Sie im Menü **Erweiterte Konfiguration** wie folgt vor:

        * Damit die Datenbank nach dem Wiederherstellen nicht betriebsbereit bleibt, wählen Sie für **Mit NORECOVERY wiederherstellen** **Aktiviert** aus. **Mit NORECOVERY wiederherstellen** ist standardmäßig deaktiviert.
        * Um den Speicherort für die Wiederherstellung auf dem Zielserver zu ändern, geben Sie einen neuen Pfad in der Spalte **Ziel** an.
        * Wählen Sie **OK** aus, um die Einstellungen zu übernehmen. Schließen Sie das Menü **Erweiterte Konfiguration**.

    4. Wählen Sie im Menü **Wiederherstellen** **Wiederherstellen** aus, um den Wiederherstellungsauftrag zu starten. Verfolgen Sie den Fortschritt der Wiederherstellung im Bereich **Benachrichtigungen**, oder wählen Sie im Datenbankmenü **Wiederherstellungsaufträge** aus.

       ![Fortschritt des Wiederherstellungsauftrags](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Wiederherstellen und Überschreiben der Datenbank

Hier erfahren Sie, wie Sie Daten wiederherstellen und eine Datenbank überschreiben. Wenn die Wiederherstellung an einem alternativen Speicherort erfolgen soll, lesen Sie weiter unter [Wiederherstellen an einem alternativen Speicherort](backup-azure-sql-database.md#restore-to-an-alternate-location). Zu diesem Zeitpunkt ist Ihr Recovery Services-Tresor geöffnet und das Menü **Konfiguration wiederherstellen** wird angezeigt (siehe folgendes Bild). Wenn Sie sich nicht bei diesem Schritt befinden, starten Sie bei [Wiederherstellen einer SQL­-Datenbank](backup-azure-sql-database.md#restore-a-sql-database).

![Menü „Konfiguration wiederherstellen“](./media/backup-azure-sql-database/restore-overwrite-db.png)

Im Menü **Wiederherstellungskonfiguration** zeigt das Dropdown-Listenfeld **Server** nur die SQL Server-Instanzen an, die beim Recovery Services-Tresor registriert sind. Wenn sich der gewünschte Server nicht in der Liste befindet, lesen Sie in Abschnitt [Ermitteln von SQL Server-Datenbanken](backup-azure-sql-database.md#discover-sql-server-databases), wie Sie den Server finden. Beim Ermittlungsprozess werden neue Server beim Recovery Services-Tresor registriert.

1. Wählen Sie im Menü **Konfiguration wiederherstellen** **Datenbank überschreiben** aus, und klicken Sie auf **OK**, um die Zielkonfiguration abzuschließen.

   ![Auswählen von „Datenbank überschreiben“](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    Die Einstellungen für **Server**, **Instanz** und **Name der wiederhergestellten Datenbank** sind nicht erforderlich.

2. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** **Protokolle (Zeitpunkt)** oder **Vollständig und differenziell** aus. Wenn die Wiederherstellung auf ein Protokoll zu einem bestimmten Zeitpunkts erfolgen soll, fahren Sie mit diesem Schritt fort. Um einen Wiederherstellungspunkt mit vollständiger oder differenzieller Sicherung wiederherzustellen, machen Sie weiter mit Schritt 3.

    ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-menu.png)

    Die Point-in-Time-Wiederherstellung ist nur für Transaktionsprotokollsicherungen für Datenbanken mit einem vollständigen und massenprotokollierten Wiederherstellungsmodell verfügbar. So können Sie eine Wiederherstellung zu einem sekundengenauen Zeitpunkt ausführen:

    1. Wählen Sie **Protokolle (Zeitpunkt)** als Wiederherstellungspunkt aus.

        ![Auswählen des Wiederherstellungspunkttyps](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Wählen Sie unter **Datum/Uhrzeit für Wiederherstellung** den Minikalender aus, um den **Kalender** zu öffnen. Im **Kalender** enthalten die fett angezeigten Daten Wiederherstellungspunkte. Das aktuelle Datum wird hervorgehoben. Wählen Sie ein Datum mit Wiederherstellungspunkten aus. Daten ohne Wiederherstellungspunkte können nicht ausgewählt werden.

        ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Wenn Sie ein Datum ausgewählt haben, zeigt das Zeitachsendiagramm die verfügbaren Wiederherstellungspunkte an.

    3. Geben Sie mithilfe des Zeitachsendiagramms oder des Dialogfelds **Zeit** einen bestimmten Zeitpunkt für den Wiederherstellungspunkt an. Wählen Sie **OK** aus, um diesen Schritt abzuschließen.

       ![Öffnen des Kalenders](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        Das Menü **Wiederherstellungspunkt auswählen** wird geschlossen, und das Menü **Erweiterte Konfiguration** wird geöffnet.

       ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Gehen Sie im Menü **Erweiterte Konfiguration** wie folgt vor:

        * Damit die Datenbank nach dem Wiederherstellen nicht betriebsbereit bleibt, wählen Sie für **Mit NORECOVERY wiederherstellen** **Aktiviert** aus.
        * Um den Speicherort für die Wiederherstellung auf dem Zielserver zu ändern, geben Sie einen neuen Pfad in der Spalte **Ziel** an.
        * Wählen Sie **OK** aus, um die Einstellungen zu übernehmen. Schließen Sie das Menü **Erweiterte Konfiguration**.

    5. Wählen Sie im Menü **Wiederherstellen** **Wiederherstellen** aus, um den Wiederherstellungsauftrag zu starten. Verfolgen Sie den Fortschritt der Wiederherstellung im Bereich **Benachrichtigungen**, oder wählen Sie im Datenbankmenü **Wiederherstellungsaufträge** aus.

       ![Fortschritt des Wiederherstellungsauftrags](./media/backup-azure-sql-database/restore-job-notification.png)

3. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** **Protokolle (Zeitpunkt)** oder **Vollständig und differenziell** aus. Wenn Sie ein Protokoll zu einem bestimmten Zeitpunkt wiederherstellen möchten, gehen Sie zurück zu Schritt 2. In diesem Schritt wird ein bestimmter Wiederherstellungspunkt mit vollständiger oder differentieller Sicherung wiederhergestellt. Sie können alle vollständigen und differenziellen Wiederherstellungspunkte der letzten 30 Tage anzeigen. Wenn Sie Wiederherstellungspunkte anzeigen möchten, die älter als 30 Tage sind, wählen Sie **Filter** aus, um das Menü **Wiederherstellungspunkte filtern** zu öffnen. Wenn Sie einen differenziellen Wiederherstellungspunkt wählen, stellt Azure Backup zuerst den entsprechenden vollständigen Wiederherstellungspunkt wieder her und wendet dann den ausgewählten differenziellen Wiederherstellungspunkt an.

    ![Menü „Wiederherstellungspunkt auswählen“](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. Wählen Sie im Menü **Wiederherstellungspunkt auswählen** die Option **Vollständig und differenziell** aus.

       ![Auswählen von „Vollständig und differenziell“](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Das Menü zeigt die Liste der verfügbaren Wiederherstellungspunkte an.

    2. Wählen Sie aus der Liste einen Wiederherstellungspunkt aus, und wählen Sie **OK** aus, um die Prozedur der Wiederherstellungspunkte abzuschließen.

        ![Auswählen eines vollständigen Wiederherstellungspunkts](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        Das Menü **Wiederherstellungspunkt** wird geschlossen, und das Menü **Erweiterte Konfiguration** wird geöffnet.

        ![Menü „Erweiterte Konfiguration“](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Gehen Sie im Menü **Erweiterte Konfiguration** wie folgt vor:

        * Damit die Datenbank nach dem Wiederherstellen nicht betriebsbereit bleibt, wählen Sie für **Mit NORECOVERY wiederherstellen** **Aktiviert** aus. **Mit NORECOVERY wiederherstellen** ist standardmäßig deaktiviert.
        * Um den Speicherort für die Wiederherstellung auf dem Zielserver zu ändern, geben Sie einen neuen Pfad in der Spalte **Ziel** an.
        * Wählen Sie **OK** aus, um die Einstellungen zu übernehmen. Schließen Sie das Menü **Erweiterte Konfiguration**.

    4. Wählen Sie im Menü **Wiederherstellen** **Wiederherstellen** aus, um den Wiederherstellungsauftrag zu starten. Verfolgen Sie den Fortschritt der Wiederherstellung im Bereich **Benachrichtigungen**, oder wählen Sie im Datenbankmenü **Wiederherstellungsaufträge** aus.

       ![Fortschritt des Wiederherstellungsauftrags](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Verwalten von Azure Backup-Vorgängen für SQL auf virtuellen Azure-Computern

Dieser Abschnitt enthält Informationen zu den verschiedenen Azure Backup-Verwaltungsvorgängen, die für SQL-VMs in Azure verfügbar sind. Es gibt die folgenden allgemeinen Vorgänge:

* Überwachen von Aufträgen
* Sicherungswarnungen
* Beenden des Schutzes für eine SQL-Datenbank-Instanz
* Fortsetzen des Schutzes für eine SQL-­Datenbank-Instanz
* Auslösen eines Ad-hoc-Sicherungsauftrags
* Aufheben der Registrierung für einen Server, der SQL Server ausführt

### <a name="monitor-backup-jobs"></a>Überwachen von Sicherungsaufträgen
Azure Backup bietet als Unternehmenslösung erweiterte Sicherungswarnungen und Benachrichtigungen bei Fehlern. Weitere Informationen finden Sie unter [Anzeigen von Sicherungswarnungen](backup-azure-sql-database.md#view-backup-alerts). Nutzen Sie je nach Belieben eine der folgenden Optionen, um bestimmte Aufträge zu überwachen.

#### <a name="use-the-azure-portal-for-adhoc-operations"></a>Verwenden des Azure-Portals für Ad-hoc-Vorgänge
Azure Backup zeigt alle manuell oder ad-hoc ausgelösten Aufträge im Portal **Sicherungsaufträge** an. Die im Portal **Sicherungsaufträge** verfügbaren Aufträge umfassen:
- Alle Vorgänge zum Konfigurieren von Sicherungen
- Manuell ausgelöst Sicherungsvorgänge
- Wiederherstellungsvorgänge
- Registrierungs- und Ermittlungsvorgänge für Datenbanken
- Vorgänge zum Beenden von Sicherungen

![Portal „Sicherungsaufträge“](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Alle geplanten Sicherungsaufträge, einschließlich der vollständigen, differenziellen und Transaktionsprotokollsicherung werden im Portal **Sicherungsaufträge** nicht angezeigt. Verwenden Sie SQL Server Management Studio, um geplante Sicherungsaufträge zu überwachen, wie im nächsten Abschnitt beschrieben.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>Verwenden von SQL Server Management Studio für Sicherungsaufträge
Azure Backup nutzt für alle Sicherungsvorgänge native SQL-APIs. Mit den nativen APIs können Sie alle Auftragsinformationen aus der [SQL-Sicherungssatztabelle](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in der MSDB-Datenbank abrufen.

Das folgende Beispiel zeigt eine Abfrage zum Abrufen aller Sicherungsaufträge für eine Datenbank mit dem Namen **DB1**. Passen Sie die Abfrage für eine erweiterte Überwachung an.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  

```

### <a name="view-backup-alerts"></a>Anzeigen von Sicherungswarnungen

Da Protokollsicherungen alle 15 Minuten stattfinden, kann die Überwachung von Sicherungsaufträgen mühsam sein. Azure Backup wirkt hierbei unterstützend. Alle Sicherungsfehler lösen E-Mail-Benachrichtigungen aus. Benachrichtigungen werden auf Datenbankebene nach Fehlercode konsolidiert. Eine E-Mail-Benachrichtigung wird nur für den ersten Sicherungsfehler in einer Datenbank gesendet. Melden Sie sich im Azure-Portal an, um alle Datenbankfehler zu überwachen.

So überwachen Sie Sicherungswarnungen

1. Melden Sie sich in Ihrem Azure-Abonnement im [Azure-Portal](https://portal.azure.com) an.

2. Öffnen Sie den Recovery Services-Tresor, der bei der SQL-VM registriert ist.

3. Wählen Sie im Dashboard **Recovery Services-Tresor** **Warnungen und Ereignisse** aus.

   ![Auswählen von „Warnungen und Ereignisse“](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. Wählen Sie im Menü **Warnungen und Ereignisse** **Sicherungswarnungen** aus, um eine Liste der Warnungen anzuzeigen.

   ![Auswählen von „Sicherungswarnungen“](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-for-a-sql-server-database"></a>Beenden des Schutzes für eine SQL Server-Datenbank

Wenn Sie den Schutz für eine SQL Server-Datenbank beenden, müssen Sie in Azure Backup festlegen, ob Sie die Wiederherstellungspunkte beibehalten möchten. Es gibt zwei Möglichkeiten, den Schutz für eine SQL-Datenbank zu beenden:

* Beenden aller zukünftigen Sicherungsaufträge und Löschen aller Wiederherstellungspunkte
* Beenden aller zukünftigen Sicherungsaufträge und Beibehalten der Wiederherstellungspunkte

Wenn Sie „Sicherung mit ‚Daten beibehalten‘ beenden“ auswählen, werden die Wiederherstellungspunkte gemäß der Sicherungsrichtlinie bereinigt. Sie zahlen die Gebühr für die geschützte SQL-Instanz zuzüglich des Speicherverbrauchs, bis alle Wiederherstellungspunkte bereinigt wurden. Weitere Informationen zu den Preisen von Azure Backup für SQL finden Sie auf der Seite mit der [Azure Backup-Preisübersicht](https://azure.microsoft.com/pricing/details/backup/).

Gehen Sie wie folgt vor, um den Schutz für eine Datenbank zu beenden:

1. Öffnen Sie den Recovery Services-Tresor, der bei der SQL-VM registriert ist.

2. Wählen Sie im Dashboard **Recovery Services-Tresor** unter **Verwendung** **Sicherungselemente** aus, um das Menü **Sicherungselemente** zu öffnen.

    ![Öffnen des Menüs „Sicherungselemente“](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Wählen Sie im Menü **Sicherungselemente** unter **Sicherungsverwaltungstyp** **SQL Server in Azure-VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    Das Menü **Sicherungselemente** zeigt die Liste der SQL-Datenbanken an.

4. Wählen Sie in der Liste der SQL-Datenbanken die Datenbank aus, für die Sie den Schutz beenden möchten.

    ![Auswählen der Datenbank zum Beenden des Schutzes](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Bei Auswahl der Datenbank wird das entsprechende Menü geöffnet.

5. Wählen Sie im Menü für die ausgewählte Datenbank **Sicherung beenden** aus.

    ![Auswählen von „Sicherung beenden“](./media/backup-azure-sql-database/stop-db-button.png)

    Das Menü **Sicherung beenden** wird geöffnet.

6. Wählen Sie im Menü **Sicherung beenden** **Sicherungsdaten beibehalten** oder **Sicherungsdaten löschen** aus. Optional können Sie einen Grund für das Beenden des Schutzes und einen Kommentar angeben.

    ![Menü „Sicherung beenden“](./media/backup-azure-sql-database/stop-backup-button.png)

7. Wählen Sie **Sicherung beenden** aus, um den Schutz für die Datenbank zu beenden.

  Beachten Sie, dass die Option **Sicherung beenden** nicht für eine Datenbank in einer [automatisch geschützten Instanz](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) funktioniert. Sie können den Schutz dieser Datenbank nur beenden, indem Sie den [automatischen Schutz](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) für die Instanz zunächst deaktivieren und dann die Option **Sicherung beenden** unter **Sicherungselemente** für diese Datenbank auswählen.<br>
  Nachdem Sie den automatischen Schutz deaktiviert haben, können Sie unter **Sicherungselemente** für die Datenbank die **Sicherung beenden**. Die Instanz kann nun erneut für den automatischen Schutz aktiviert werden.


### <a name="resume-protection-for-a-sql-database"></a>Fortsetzen des Schutzes für eine SQL-­Datenbank-Instanz

Wenn Sie beim Beenden des Schutzes für die SQL-Datenbank die Option **Sicherungsdaten beibehalten** auswählen, wird der Schutz fortgesetzt. Wenn die Sicherungsdaten nicht beibehalten wurden, kann der Schutz nicht fortgesetzt werden.

1. Um den Schutz für die SQL-Datenbank fortzusetzen, öffnen Sie das Sicherungselement, und wählen Sie **Sicherung fortsetzen** aus.

    ![Auswählen von „Sicherung fortsetzen“, um den Datenbankschutz fortzusetzen](./media/backup-azure-sql-database/resume-backup-button.png)

   Das Menü **Sicherungsrichtlinie** wird geöffnet.

2. Wählen Sie im Menü **Sicherungsrichtlinie** eine Richtlinie aus, und klicken Sie dann auf **Speichern**.

### <a name="trigger-an-adhoc-backup"></a>Auslösen einer Ad-hoc-Sicherung

Lösen Sie Ad-hoc-Sicherungen nach Bedarf aus. Es gibt vier Arten von Ad-hoc-Sicherungen:

* Vollständige Sicherung
* Vollständige Kopiesicherung
* Differenzielle Sicherung
* Protokollsicherung

Weitere Informationen zu den jeweiligen Arten finden Sie im Artikel [SQL-Sicherungsarten](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

### <a name="unregister-a-sql-server-instance"></a>Registrierung einer SQL Server-Instanz aufheben

So heben Sie die Registrierung einer SQL Server-Instanz auf, nachdem Sie den Schutz entfernt haben, und bevor Sie den Tresor löschen:

1. Öffnen Sie den Recovery Services-Tresor, der bei der SQL-VM registriert ist.

2. Wählen Sie im Dashboard **Recovery Services-Tresor** unter **Verwalten** **Sicherungsinfrastruktur** aus.  

   ![Auswählen von „Sicherungsinfrastruktur“](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. Wählen Sie unter **Verwaltungsserver** **Geschützte Server** aus.

   ![Auswählen von „Geschützte Server“](./media/backup-azure-sql-database/protected-servers.png)

    Das Menü **Geschützte Server** wird geöffnet.

4. Wählen Sie im Menü **Geschützte Server** den Server aus, dessen Registrierung Sie aufheben möchten. Um den Tresor zu löschen, müssen Sie die Registrierung aller Server aufheben.

5. Klicken Sie im Menü **Geschützte Server** mit der rechten Maustaste auf den geschützten Server, und wählen Sie **Löschen** aus.

   ![Auswählen von „Löschen“](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="faq"></a>Häufig gestellte Fragen

Der folgende Abschnitt enthält zusätzliche Informationen zur SQL-Datenbanksicherung.

### <a name="can-i-throttle-the-speed-of-the-sql-server-backup-policy"></a>Kann ich die Ausführungsgeschwindigkeit der SQL Server-Sicherungsrichtlinie drosseln?

Ja. Sie können die Geschwindigkeit drosseln, mit der die Sicherungsrichtlinie ausgeführt wird, um die Auswirkungen auf eine SQL Server-Instanz zu minimieren.
So ändern Sie die Einstellung:
1. Erstellen Sie auf der SQL Server-Instanz im Ordner *C:\Programme\Azure Workload Backup\bin* die Datei **ExtensionSettingsOverrides.json**.
2. Ändern Sie in der Datei **ExtensionSettingsOverrides.json** die Einstellung **DefaultBackupTasksThreshold** in einen kleineren Wert (z. B. „5“). <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Speichern Sie die Änderungen. Schließen Sie die Datei.
4. Öffnen Sie auf der SQL Server-Instanz **Task-Manager**. Starten Sie den Dienst **AzureWLBackupCoordinatorSvc** neu.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kann ich eine vollständige Sicherung aus einem sekundären Replikat ausführen?
 Nein. Diese Funktion wird nicht unterstützt.

### <a name="do-successful-backup-jobs-create-alerts"></a>Erstellen erfolgreiche Sicherungsaufträge Warnungen?

 Nein. Erfolgreiche Sicherungsaufträge generieren keine Warnungen. Warnungen werden nur für Sicherungsaufträge gesendet, bei denen ein Fehler aufgetreten ist.

### <a name="can-i-see-details-for-scheduled-backup-jobs-in-the-jobs-menu"></a>Werden Details von geplanten Sicherungsaufträgen im Menü „Aufträge“ angezeigt?

 Nein. Im Menü **Sicherungsaufträge** werden die Details von Ad-hoc-Aufträgen angezeigt, jedoch keine weiteren Informationen zu geplanten Sicherungsaufträgen. Wenn bei geplanten Sicherungsaufträgen Fehler auftreten, finden Sie die entsprechenden Details in den Warnungen der fehlerhaften Aufträge. Verwenden Sie [SQL Server Management Studio](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs), um alle geplanten und Ad-hoc-Sicherungsaufträge zu überwachen.

### <a name="when-i-select-a-sql-server-instance-are-future-databases-automatically-added"></a>Wenn ich eine SQL Server-Instanz auswähle, werden zukünftige Datenbanken automatisch hinzugefügt?

 Nein. Wenn Sie den Schutz für eine SQL Server-Instanz konfigurieren, werden alle Datenbanken hinzugefügt, wenn Sie die Severebenenoption auswählen. Wenn Sie einer SQL Server-Instanz nach der Schutzkonfiguration Datenbanken hinzufügen, müssen Sie die neuen Datenbanken manuell hinzufügen, um diese zu schützen. Die Datenbanken werden nicht automatisch in den konfigurierten Schutz eingeschlossen.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Wie kann ich den Schutz neu starten, wenn ich das Wiederherstellungsmodell ändere?

Lösen Sie eine vollständige Sicherung aus. Protokollsicherungen starten wie erwartet.

### <a name="can-i-protect-sql-always-on-availability-groups-where-the-primary-replica-is-on-premises"></a>Kann ich SQL AlwaysOn-Verfügbarkeitsgruppen mit einem lokalen primären Replikat schützen?

 Nein. Azure Backup schützt SQL Server-Instanzen, die in Azure ausgeführt werden. Wenn eine Verfügbarkeitsgruppe auf Azure und lokale Computer verteilt ist, kann die Verfügbarkeitsgruppe nur geschützt werden, wenn das primäre Replikat in Azure ausgeführt wird. Außerdem schützt Azure Backup nur die Knoten, die in derselben Azure-Region ausgeführt werden wie der Recovery Services-Tresor.

### <a name="can-i-protect-sql-always-on-availability-groups-which-are-spread-across-azure-regions"></a>Kann ich SQL AlwaysOn-Verfügbarkeitsgruppen schützen, die über Azure-Regionen verteilt sind?

Der Azure Backup Recovery Services-Tresor kann alle Knoten erkennen und schützen, die sich in der gleichen Region wie der Recovery Services-Tresor befinden. Wenn sich Ihre SQL Always On-Verfügbarkeitsgruppe über mehrere Azure-Regionen erstreckt, müssen Sie die Sicherung von der Region aus konfigurieren, die über den primären Knoten verfügt. Azure Backup kann alle Datenbanken in der Verfügbarkeitsgruppe gemäß der Sicherungseinstellung erkennen und schützen. Wenn die Sicherung nicht entsprechend der Einstellung erfolgt, tritt bei den Sicherungen ein Fehler auf, und Sie erhalten eine Fehlermeldung.

### <a name="while-i-want-to-protect-most-of-the-databases-in-an-instance-i-would-like-to-exclude-a-few-is-it-possible-to-still-use-the-auto-protection-feature"></a>Ich möchte die meisten Datenbanken in einer Instanz schützen, ein paar jedoch ausschließen. Ist es möglich, den automatischen Schutz weiterhin zu verwenden?

Nein, der [automatische Schutz](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) gilt für die gesamte Instanz. Sie können mithilfe des automatischen Schutzes nicht Datenbanken einer Instanz selektiv schützen.

### <a name="can-i-have-different-policies-for-different-databases-in-an-auto-protected-instance"></a>Sind unterschiedliche Richtlinien für verschiedene Datenbanken in einer automatisch geschützten Instanz möglich?

Wenn Sie bereits über einige geschützte Datenbanken in einer Instanz verfügen, werden diese auch noch durch ihre jeweiligen Richtlinien geschützt, nachdem Sie den [automatischen Schutz](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) **aktiviert** haben. Allerdings gilt für alle nicht geschützten Datenbanken sowie diejenigen, die Sie in der Zukunft hinzufügen würden, nur eine einzige Richtlinie, die Sie unter **Sicherung konfigurieren** nach Auswahl der Datenbanken definieren. Im Gegensatz zu anderen geschützten Datenbanken können Sie sogar nicht einmal die Richtlinie für eine Datenbank unter einer automatisch geschützten Instanz ändern.
Die einzige Möglichkeit, dies zu tun, besteht darin, den automatischen Schutz der Instanz zunächst zu deaktivieren und dann die Richtlinie für diese Datenbank zu ändern. Sie können dann den automatischen Schutz für diese Instanz erneut aktivieren.

### <a name="if-i-delete-a-database-from-an-auto-protected-instance-will-the-backups-for-that-database-also-stop"></a>Werden die Sicherungen für eine Datenbank, die ich aus einer automatisch geschützten Instanz lösche, auch beendet?

Nein, wenn eine Datenbank aus einer automatisch geschützten Instanz gelöscht wird, wird weiterhin versucht, die Datenbank zu sichern. Dies bedeutet auch, dass die gelöschte Datenbank fortan unter **Sicherungselemente** als fehlerhaft angezeigt und immer noch als geschützt behandelt wird.

Sie können den Schutz dieser Datenbank nur beenden, indem Sie den [automatischen Schutz](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) für die Instanz zunächst deaktivieren und dann die Option **Sicherung beenden** unter **Sicherungselemente** für diese Datenbank auswählen. Sie können dann den automatischen Schutz für diese Instanz erneut aktivieren.

###  <a name="why-cant-i-see-the-newly-added-database-to-an-auto-protected-instance-under-the-protected-items"></a>Warum sehe ich die einer automatisch geschützten Instanz neu hinzugefügte Datenbank nicht unter den geschützten Elementen?

Eine Datenbank, die einer [automatisch geschützten Instanz](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) neu hinzugefügt und sofort geschützt wird, wird möglicherweise nicht angezeigt. Dies liegt daran, dass die Ermittlung in der Regel alle 8 Stunden ausgeführt wird. Allerdings kann der Benutzer eine manuelle Ermittlung mit der Option **DBs wiederherstellen** durchführen, um neue Datenbanken sofort zu ermitteln und zu schützen, wie in der folgenden Abbildung gezeigt:

  ![Neu hinzugefügte Datenbank anzeigen](./media/backup-azure-sql-database/view-newly-added-database.png)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Backup finden Sie im Azure PowerShell-Beispiel zum Sichern verschlüsselter VMs.

> [!div class="nextstepaction"]
> [Sichern eines verschlüsselten virtuellen Computers](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
