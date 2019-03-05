---
title: Sichern von SQL Server-Datenbanken in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie SQL Server in Azure sichern. In diesem Artikel wird auch die SQL Server-Wiederherstellung beschrieben.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 61219fc4e1fc329708a7e58ee6a293e4e25cca31
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56887810"
---
# <a name="back-up-sql-server-databases-on-azure-vms"></a>Sichern von SQL Server-Datenbanken auf virtuellen Azure-Computern 

SQL Server-Datenbanken sind kritische Workloads, die eine niedrige Recovery Point Objective (RPO) und eine Langzeitaufbewahrung erfordern. Sie können auf virtuellen Azure-Computern ausgeführte SQL Server-Datenbanken mithilfe von [Azure Backup](backup-overview.md) sichern. 

In diesem Artikel wird veranschaulicht, wie Sie eine SQL Server-Datenbank, die auf einem virtuellen Azure-Computer ausgeführt wird, in einem Azure Backup Recovery Services-Tresor sichern. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen für die Sicherung einer SQL Server-Instanz
> * Erstellen und Konfigurieren eines Tresors
> * Erkennen von Datenbanken und Einrichten von Sicherungen
> * Einrichten des automatischen Schutzes für Datenbanken

> [!NOTE]
> Dieses Feature ist zurzeit als öffentliche Preview verfügbar.

## <a name="before-you-start"></a>Vorbereitung

Überprüfen Sie zunächst Folgendes:

1. Vergewissern Sie sich, dass in Azure eine SQL Server-Instanz ausgeführt wird. Im Marketplace können Sie [schnell eine SQL Server-Instanz erstellen](../sql-database/sql-database-get-started-portal.md).
2. Lesen Sie die folgenden Informationen zu Einschränkungen der öffentlichen Vorschauversion.
3. Lesen Sie die Informationen zur Szenariounterstützung.
4. [Lesen Sie häufig gestellte Fragen](faq-backup-sql-server.md) zu diesem Szenario.


## <a name="preview-limitations"></a>Einschränkungen der Vorschau

Für diese öffentliche Vorschauversion gelten einige Einschränkungen.

- Der virtuelle Computer, auf dem SQL Server ausgeführt wird, benötigt eine Internetverbindung, um auf öffentliche IP-Adressen von Azure zuzugreifen. 
- Sie können bis zu 2000 SQL Server-Datenbanken in einem Tresor sichern. Wenn Sie mehr haben, erstellen Sie einen weiteren Tresor. 
- Sicherungen von [verteilten Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) funktionieren nicht uneingeschränkt.
- SQL Server Always On-Failoverclusterinstanzen (Failover Cluster Instances, FCIs) werden für die Sicherung nicht unterstützt.
- Die SQL Server-Sicherung muss im Portal konfiguriert werden. Das Konfigurieren von Sicherungen mit Azure PowerShell, CLI oder den REST-APIs ist derzeit nicht möglich.
- Sicherungs-und Wiederherstellungsvorgänge für FCI-Spiegeldatenbanken, -Datenbankmomentaufnahmen und -Datenbanken werden nicht unterstützt.
- Datenbanken mit einer großen Anzahl von Dateien können nicht geschützt werden. Die maximale Anzahl von unterstützten Dateien ist nicht deterministisch. Sie hängt nicht nur von der Anzahl der Dateien, sondern auch von der Pfadlänge der Dateien ab. 

Lesen Sie [häufig gestellte Fragen](faq-backup-sql-server.md) zur Sicherung von SQL Server-Datenbanken.
## <a name="scenario-support"></a>Unterstützung von Szenarien

**Unterstützung** | **Details**
--- | ---
**Unterstützte Bereitstellungen** | SQL-Marketplace-Azure-VMs und Nicht-Marketplace-VMs (manuelle SQL Server-Installation) werden unterstützt.
**Unterstützte geografische Räume** | Australien, Südosten (ASE); Brasilien, Süden (BRS); Kanada, Mitte (CNC); Kanada, Osten (CE); USA, Mitte US (CUS); Asien, Osten (EA); Australien, Osten (AE); USA, Osten (EUS); USA, Osten 2 (EUS2); Indien, Mitte (INC); Indien, Süden (INS); Japan, Osten (JPE); Japan, Westen (JPW); Korea, Mitte (KRC); Korea, Süden (KRS); USA, Norden-Mitte (NCUS); Europa, Norden (NE); USA, Süden-Mitte (SCUS); Asien, Südosten (SEA); Vereinigtes Königreich, Süden (UKS); Vereinigtes Königreich, Westen (UKW); USA, Westen-Mitte (WCUS); Europa, Westen (WE); USA, Westen (WUS); USA, Westen 2 (WUS 2)
**Unterstützte Betriebssysteme** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux wird derzeit nicht unterstützt.
**Unterstützte SQL Server-Versionen** | SQL Server 2017; SQL Server 2016, SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Ihre SQL Server-Datenbank sichern können, müssen folgende Bedingungen erfüllt sein:

1. Identifizieren oder [erstellen](backup-azure-sql-database.md#create-a-recovery-services-vault) Sie einen Recovery Services-Tresor in der gleichen Region oder dem gleichen Gebietsschema wie der virtuelle Computer, der die SQL Server-Instanz hostet.
2. [Überprüfen Sie die erforderlichen Berechtigungen für virtuelle Computer](#fix-sql-sysadmin-permissions) zum Sichern der SQL-Datenbanken.
3. Stellen Sie sicher, dass der virtuelle Computer über [Netzwerkkonnektivität](backup-azure-sql-database.md#establish-network-connectivity) verfügt.
4. Vergewissern Sie sich, dass die SQL Server-Datenbanken in Übereinstimmung mit den [Benennungsrichtlinien](backup-azure-sql-database.md) für Azure Backup benannt sind.
5. Stellen Sie sicher, dass Sie keine anderen Sicherungslösungen für die Datenbank aktiviert sind. Deaktivieren Sie alle anderen SQL Server-Sicherungen, bevor Sie dieses Szenario einrichten. Sie können Azure Backup für einen virtuellen Azure-Computer zusammen mit Azure Backup für eine auf dem virtuellen Computer ausgeführte SQL Server-Datenbank ohne Konflikte aktivieren.


### <a name="establish-network-connectivity"></a>Herstellen der Netzwerkverbindung

Der virtuelle SQL Server-Computer benötigt für alle Vorgänge eine Verbindung, um auf die öffentlichen IP-Adressen von Azure zuzugreifen. VM-Vorgänge (Ermitteln von Datenbanken, Konfigurieren von Sicherungen, Planen von Sicherungen, Zurücksetzen von Wiederherstellungspunkten usw.) schlagen ohne Verbindung zu öffentlichen IP-Adressen fehl. Richten Sie die Verbindung mit einer der folgenden Optionen ein:

- **Zulassen der IP-Adressbereiche des Azure-Rechenzentrums**: Lassen Sie die [IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=41653) im Download zu. Verwenden Sie für Zugriff in einer Netzwerksicherheitsgruppe (NSG) das Cmdlet **Set-AzureNetworkSecurityRule**.
- **Bereitstellen eines HTTP-Proxyservers für das Weiterleiten von Datenverkehr**: Wenn Sie eine SQL Server-Datenbank auf einem virtuellen Azure-Computer sichern, verwendet die Sicherungserweiterung auf dem virtuellen Computer die HTTPS-APIs, um Verwaltungsbefehle an Azure Backup und Daten an Azure Storage zu senden. Die Sicherungserweiterung verwendet auch Azure Active Directory (Azure AD) zur Authentifizierung. Leiten Sie den Datenverkehr der Sicherungserweiterung für diese drei Dienste über den HTTP-Proxy weiter. Die Erweiterung ist die einzige Komponente, die für den Zugriff auf das öffentliche Internet konfiguriert ist.

Jede Option hat Vor- und Nachteile.

**Option** | **Vorteile** | **Nachteile**
--- | --- | ---
Zulassen von IP-Adressbereichen | Keine zusätzlichen Kosten. | Komplexe Verwaltung, da sich die IP-Adressbereiche im Laufe der Zeit ändern. <br/><br/> Zugriff auf alle Azure-Dienste, nicht nur auf Azure Storage.
Verwenden eines HTTP-Proxys   | Feinsteuerung im Proxy über die Speicher-URLs ist zulässig. <br/><br/> Zentraler Punkt für Internetzugriff auf virtuelle Computer. <br/><br/> Unterliegt keinen Azure-IP-Adressänderungen. | Zusätzliche Kosten für das Ausführen einer VM mit Proxysoftware. 

### <a name="set-vm-permissions"></a>Einrichten von Berechtigungen für virtuelle Computer

Azure Backup führt eine Reihe von Aktionen durch, wenn Sie die Sicherung für eine SQL Server-Datenbank konfigurieren:

- Fügt die Erweiterung **AzureBackupWindowsWorkload** hinzu.
- Azure Backup erstellt das Konto **NT SERVICE\AzureWLBackupPluginSvc**, um Datenbanken auf dem virtuellen Computer zu ermitteln. Dieses Konto wird zum Sichern und Wiederherstellen verwendet und erfordert SQL-Systemadministratorberechtigungen.
- Azure Backup verwendet das Konto **NT AUTHORITY\SYSTEM** für die Ermittlung von Datenbanken und Anfragen an Datenbanken. Dieses Konto muss also über eine öffentliche Anmeldung in SQL verfügen.

Wenn Sie den virtuellen SQL Server-Computer nicht in Azure Marketplace erstellt haben, erhalten Sie möglicherweise den Fehler **UserErrorSQLNoSysadminMembership**. Gehen Sie in diesem Fall [wie folgt vor](#fix-sql-sysadmin-permissions):

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Überprüfen der Benennungsrichtlinien für SQL-Datenbanken in Azure Backup

Vermeiden Sie folgende Zeichen in Datenbanknamen:

  * Nachgestellte/führende Leerzeichen
  * Führende Ausrufezeichen
  * Schließende eckige Klammer „]“

Obwohl Aliasing für nicht unterstützte Zeichen aus Azure-Tabellen möglich ist, wird empfohlen, diese Zeichen zu vermeiden. [Weitere Informationen](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN)

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Ermitteln von SQL Server-Datenbanken

Ermitteln Sie auf dem virtuellen Computer ausgeführte Datenbanken. 
1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) den Recovery Services-Tresor, mit dem Sie die Datenbank sichern.

2. Wählen Sie auf dem Dashboard des **Recovery Services-Tresors** **Sichern** aus. 

   ![Auswählen von „Sichern“, um Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Legen Sie in **Sicherungsziel** die Einstellung **Wo wird die Workload ausgeführt?** auf **Azure** (Standard) fest.

4. Wählen Sie unter **Was möchten Sie sichern?** die Option **SQL Server in Azure VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“ für die Sicherung](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)


5. Wählen Sie in **Sicherungsziel** > **DBs in VMs ermitteln** die Option **Ermittlung starten** aus, um nach nicht geschützten virtuellen Computern im Abonnement zu suchen. Die Suche kann je nach Anzahl von ungeschützten virtuellen Computern im Abonnement eine Weile dauern.

    ![Ausstehende Sicherung bei der Suche nach Datenbanken in VMs](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Wählen Sie in der Liste der virtuellen Computer den virtuellen Computer mit der SQL Server-Datenbank und dann **Datenbankermittlung** aus.

7. Verfolgen Sie die Datenbankermittlung im Bereich **Benachrichtigungen**. Je nachdem, wie viele Datenbanken sich auf dem virtuellen Computer befinden, kann es eine Weile dauern, bis der Auftrag abgeschlossen ist. Wenn die ausgewählten Datenbanken ermittelt wurden, wird eine Erfolgsmeldung angezeigt.

    ![Meldung über erfolgreiche Bereitstellung](./media/backup-azure-sql-database/notifications-db-discovered.png)

    - Nicht geschützte virtuelle Computer sollten nach der Ermittlung sortiert nach Name und Ressourcengruppe in der Liste angezeigt werden.
    - Wenn ein virtueller Computer nicht wie erwartet aufgeführt wird, überprüfen Sie, ob er bereits in einem Tresor gesichert wird.
    - Mehrere virtuelle Computer können den gleichen Namen aufweisen, gehören dann aber verschiedenen Ressourcengruppen an. 

9. Wählen Sie den virtuellen Computer mit der SQL Server-Datenbank und dann **Datenbankermittlung** aus. 
10. Azure Backup ermittelt alle SQL Server-Datenbanken auf dem virtuellen Computer. Während der Ermittlung geschieht im Hintergrund Folgendes:

    - Azure Backup registriert den virtuellen Computer beim Tresor für die Workload-Sicherung. Alle Datenbanken auf dem registrierten virtuellen Computer können nur in diesem Tresor gesichert werden.
    - Azure Backup installiert die Erweiterung **AzureBackupWindowsWorkload** auf dem virtuellen Computer. In der SQL-Datenbank ist kein Agent installiert.
    - Azure Backup erstellt das Dienstkonto **NT Service\AzureWLBackupPluginSvc** auf dem virtuellen Computer.
      - Für alle Sicherungs- und Wiederherstellungsvorgänge wird das Dienstkonto verwendet.
      - **NT Service\AzureWLBackupPluginSvc** erfordert SQL-Systemadministratorberechtigungen. Die Erweiterung **SqlIaaSExtension** ist auf allen in Azure Marketplace erstellten SQL Server-VMs vorinstalliert. Die Erweiterung **AzureBackupWindowsWorkload** verwendet **SQLIaaSExtension**, um erforderliche Berechtigungen automatisch abzurufen.
    - Wenn Sie den virtuellen Computer nicht im Marketplace erstellt haben, ist die Erweiterung **SqlIaaSExtension** nicht auf dem virtuellen Computer installiert, und der Ermittlungsvorgang schlägt mit der Fehlermeldung **UserErrorSQLNoSysAdminMembership** fehl. Befolgen Sie die Anweisungen in [#fix-sql-sysadmin-permissions], um dieses Problem zu beheben.

        ![Auswählen der VM und der Datenbank](./media/backup-azure-sql-database/registration-errors.png)



## <a name="configure-a-backup-policy"></a>Konfigurieren einer Sicherungsrichtlinie 

Zum Optimieren von Sicherungslasten legt Azure Backup die maximale Anzahl von Datenbanken in einem Sicherungsauftrag auf 50 fest.

- Zum Schützen von mehr als 50 Datenbanken konfigurieren Sie mehrere Sicherungen.
- Alternativ können Sie den automatischen Schutz aktivieren. Der automatische Schutz schützt vorhandene Datenbanken in einer Aktion und schützt automatisch neue Datenbanken, die der Instanz der Verfügbarkeitsgruppe hinzugefügt werden.


Konfigurieren Sie die Sicherung wie folgt:

1. Wählen Sie auf dem Tresordashboard die Option **Sicherung**. 

   ![Auswählen von „Sichern“, um Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Legen Sie im Menü **Sicherungsziel** die Einstellung **Wo wird die Workload ausgeführt?** auf **Azure** fest.

4. Wählen Sie unter **Was möchten Sie sichern?** die Option **SQL Server in Azure VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“ für die Sicherung](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    
5. Wählen Sie in **Sicherungsziel** die Option **Sicherung konfigurieren** aus.

    ![Auswählen von „Sicherung konfigurieren“](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    ![Anzeigen aller SQL Server-Instanzen mit eigenständigen Datenbanken](./media/backup-azure-sql-database/list-of-sql-databases.png)

6. Wählen Sie alle Datenbanken aus, die Sie schützen möchten, und wählen Sie dann **OK**.

    ![Schützen der Datenbank](./media/backup-azure-sql-database/select-database-to-protect.png)

    - Alle SQL Server-Instanzen (eigenständige Instanzen und Verfügbarkeitsgruppen) werden angezeigt.
    - Wählen Sie links vom Instanznamen/von der Verfügbarkeitsgruppe den Pfeil nach unten aus, um zu filtern.

7. Wählen Sie im Menü **Sicherung** die Option **Sicherungsrichtlinie** aus. 

    ![Auswählen der Sicherungsrichtlinie](./media/backup-azure-sql-database/select-backup-policy.png)

8. Wählen Sie in **Sicherungsrichtlinie auswählen** eine Richtlinie aus, und klicken Sie dann auf **OK**.

    - Auswahl der Standardrichtlinie: **HourlyLogBackup**.
    - Auswählen einer vorhandenen, zuvor für SQL erstellten Sicherungsrichtlinie
    - [Definieren einer neuen Richtlinie](backup-azure-sql-database.md#configure-a-backup-policy) basierend auf Ihrer RPO und Ihrer Beibehaltungsdauer
    - Vorhandene Sicherungsrichtlinien können während der Vorschauphase nicht bearbeitet werden.
    
9. Wählen Sie im Menü **Sicherung** die Option **Sicherung aktivieren** aus.

    ![Aktivieren der ausgewählten Sicherungsrichtlinie](./media/backup-azure-sql-database/enable-backup-button.png)

10. Den Konfigurationsprozess können Sie im Portalbereich **Benachrichtigungen** nachverfolgen.

    ![Benachrichtigungsbereich](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie legt fest, wann Sicherungen erstellt und wie lange sie aufbewahrt werden. 

- Eine Richtlinie wird auf Tresorebene erstellt.
- Mehrere Tresore können die gleiche Sicherungsrichtlinie verwenden. Allerdings müssen Sie die Sicherungsrichtlinie auf jeden Tresor anwenden.
- Wenn Sie eine Sicherungsrichtlinie erstellen, entspricht eine tägliche vollständige Sicherung der Standardeinstellung.
- Sie können eine differenzielle Sicherung nur hinzufügen, wenn Sie für die vollständige Sicherung festlegen, dass diese wöchentlich erfolgt.
- [Erfahren Sie mehr](backup-architecture.md#sql-server-backup-types) über verschiedene Arten von Sicherungsrichtlinien.


So erstellen Sie eine Sicherungsrichtlinie

1. Klicken Sie im Tresor auf **Sicherungsrichtlinien** > **Hinzufügen**.
2. Klicken Sie im Menü **Hinzufügen** auf **SQL Server in Azure-VM**. Dadurch wird der Richtlinientyp definiert.

   ![Auswählen eines Richtlinientyps für die neue Sicherungsrichtlinie](./media/backup-azure-sql-database/policy-type-details.png)

3. Geben Sie unter **Richtlinienname** einen Namen für die neue Richtlinie ein. 
4. Wählen Sie in **Richtlinie für vollständige Sicherung** für **Sicherungshäufigkeit** **Täglich** oder **Wöchentlich** aus.

    - Wählen Sie für **Täglich** die Uhrzeit und die Zeitzone für den Beginn des Sicherungsauftrags aus.
    - Sie müssen eine vollständige Sicherung ausführen. Die Option **Vollständige Sicherung** kann nicht deaktiviert werden.
    - Klicken Sie auf **Vollständige Sicherung**, um die Richtlinie anzuzeigen. 
    - Sie können keine differenziellen Sicherungen für tägliche vollständige Sicherungen erstellen.
    - Wählen Sie für **Wöchentlich** den Wochentag, die Uhrzeit und die Zeitzone für den Beginn des Sicherungsauftrags aus.

    ![Neue Felder für Sicherungsrichtlinien](./media/backup-azure-sql-database/full-backup-policy.png)  

5. Für **Beibehaltungsdauer** sind standardmäßig alle Optionen aktiviert. Deaktivieren Sie alle Optionen für die Beibehaltungsdauer, die Sie nicht nutzen möchten, und legen Sie die zu verwendenden Intervalle fest. 

    - Wiederherstellungspunkte werden unter Berücksichtigung ihrer Beibehaltungsdauer mit einer Markierung versehen. Wenn Sie beispielsweise eine tägliche vollständige Sicherung wählen, wird pro Tag nur eine vollständige Sicherung ausgelöst.
    - Die Sicherung für einen bestimmten Tag wird auf Grundlage der wöchentlichen Beibehaltungsdauer und Ihrer wöchentlichen Aufbewahrungseinstellung markiert und beibehalten.
    - Mit der monatlichen und jährlichen Beibehaltungsdauer verhält es sich ähnlich.

   ![Intervalleinstellungen für Beibehaltungsdauer](./media/backup-azure-sql-database/retention-range-interval.png)

    

6. Wählen Sie im Menü **Richtlinie für vollständige Sicherung** **OK** aus, um die Einstellungen zu übernehmen.
7. Um eine Richtlinie für eine differenzielle Sicherung hinzuzufügen, wählen Sie **Differenzielle Sicherung** aus. 

   ![Intervalleinstellungen für Beibehaltungsdauer](./media/backup-azure-sql-database/retention-range-interval.png)
   ![Öffnen des Menüs „Richtlinie für differenzielle Sicherung“](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. Wählen Sie in **Richtlinie für differenzielle Sicherung** die Option **Aktivieren** aus, um die Einstellungen für Häufigkeit und Beibehaltung vorzunehmen. 

    - Pro Tag kann höchstens eine differenzielle Sicherung ausgelöst werden.
    - Differenzielle Sicherungen können maximal 180 Tage aufbewahrt werden. Wenn Sie eine längere Aufbewahrung wünschen, müssen Sie vollständige Sicherungen verwenden.

9. Wählen Sie **OK** aus, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.

10. Um eine Richtlinie für eine Transaktionsprotokollsicherung hinzuzufügen, wählen Sie **Protokollsicherung** aus.
11. Wählen Sie in **Protokollsicherung** die Option **Aktivieren** aus, und legen Sie die Einstellungen für Häufigkeit und Aufbewahrung fest. Transaktionsprotokollsicherungen können alle 15 Minuten erfolgen und bis zu 35 Tage aufbewahrt werden.
12. Wählen Sie **OK** aus, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.

   ![Bearbeiten der Richtlinie für die Transaktionsprotokollsicherung](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. Legen Sie im Menü **Sicherungsrichtlinie** fest, ob die **SQL-Sicherungskomprimierung** aktiviert wird.
    - Die Komprimierung ist standardmäßig deaktiviert.
    - Auf dem Back-End verwendet Azure Backup die native SQL-Sicherungskomprimierung.

14. Nachdem Sie die Sicherungsrichtlinie bearbeitet haben, wählen Sie **OK** aus.



## <a name="enable-auto-protection"></a>Aktivieren des automatischen Schutzes  

Aktivieren Sie den automatischen Schutz, um automatisch alle vorhandenen Datenbanken sowie alle Datenbanken zu sichern, die zukünftig zu eigenständigen SQL Server-Instanzen oder SQL Server Always On-Verfügbarkeitsgruppen hinzugefügt werden. 

- Wenn Sie den automatischen Schutz aktivieren und eine Richtlinie auswählen, verwenden die vorhandenen geschützten Datenbanken weiterhin die bisherige Richtlinie.
- Die Anzahl der Datenbanken, die Sie für den automatischen Schutz in einer Aktion auswählen können, ist nicht begrenzt.

Aktivieren Sie den automatischen Schutz wie folgt:

1. Wählen Sie in **Elemente für Sicherung** die Instanz aus, für die Sie den automatischen Schutz aktivieren möchten.
2. Wählen Sie in der Dropdownliste unter **Automatischer Schutz** die Option **Ein**. Klicken Sie dann auf **OK**.

    ![Aktivieren des automatischen Schutzes für die AlwaysOn-Verfügbarkeitsgruppe](./media/backup-azure-sql-database/enable-auto-protection.png)

3. Die Sicherung wird für alle Datenbanken gemeinsam konfiguriert und kann unter **Sicherungsaufträge** nachverfolgt werden.


Wenn Sie den automatischen Schutz deaktivieren müssen, klicken Sie unter **Sicherung konfigurieren** auf den Namen der Instanz, und wählen Sie **Automatischen Schutz deaktivieren** für die Instanz aus. Alle Datenbanken werden weiterhin gesichert. Künftige Datenbanken werden jedoch nicht automatisch geschützt.

![Deaktivieren des automatischen Schutzes für diese Instanz](./media/backup-azure-sql-database/disable-auto-protection.png)


## <a name="fix-sql-sysadmin-permissions"></a>Beheben von Problemen mit SQL-Systemadministratorberechtigungen

Wenn Sie Berechtigungen aufgrund des Fehlers **UserErrorSQLNoSysadminMembership** korrigieren müssen, gehen Sie folgendermaßen vor:

1. Verwenden Sie ein Konto mit SQL Server-Systemadministratorberechtigungen, um sich bei SQL Server Management Studio (SSMS) anzumelden. Wenn Sie keine speziellen Berechtigungen benötigen, sollte die Windows-Authentifizierung funktionieren.
2. Öffnen Sie auf der SQL Server-Instanz den Ordner **Sicherheit/Anmeldungen**.

    ![Öffnen von Ordner „Sicherheit/Anmeldungen“, um Konten anzuzeigen](./media/backup-azure-sql-database/security-login-list.png)

3. Klicken Sie mit der rechten Maustaste auf den Ordner **Anmeldungen**, und wählen Sie **Neue Anmeldung** aus. Wählen Sie in **Anmeldung – Neu** die Option **Suche** aus.

    ![Auswählen von „Suchen“ im Dialogfeld „Anmeldung – Neu“](./media/backup-azure-sql-database/new-login-search.png)

3. Das virtuelle Windows-Dienstkonto **NT SERVICE\AzureWLBackupPluginSvc** wurde bei der Registrierung des virtuellen Computers und der SQL-Ermittlungsphase erstellt. Geben Sie den Kontonamen ein, wie in **Namen des auszuwählenden Objekts eingeben** dargestellt. Wählen Sie **Namen überprüfen** aus, um den Namen aufzulösen. Klicken Sie auf **OK**.

    ![Klicken auf „Namen überprüfen“, um den unbekannten Dienstnamen aufzulösen](./media/backup-azure-sql-database/check-name.png)


4. Stellen Sie in **Serverrollen** sicher, dass die Rolle **sysadmin** ausgewählt ist. Klicken Sie auf **OK**. Die erforderlichen Berechtigungen sollten jetzt vorhanden sein.

    ![Sicherstellen, dass die Serverrolle „Sysadmin“ ausgewählt ist](./media/backup-azure-sql-database/sysadmin-server-role.png)

5. Ordnen Sie nun die Datenbank dem Recovery Services-Tresor zu. Klicken Sie im Azure-Portal in der Liste **Geschützte Server** mit der rechten Maustaste auf den fehlerhaften Server, und wählen Sie **Datenbanken neu ermitteln** aus.

    ![Überprüfen der entsprechenden Serverberechtigungen](./media/backup-azure-sql-database/check-erroneous-server.png)

6. Der Fortschritt kann im Bereich **Benachrichtigungen** verfolgt werden. Wenn die ausgewählten Datenbanken gefunden wurden, wird eine Erfolgsmeldung angezeigt.

    ![Meldung über erfolgreiche Bereitstellung](./media/backup-azure-sql-database/notifications-db-discovered.png)

Alternativ können Sie durch Auswahl der Option **ON** (EIN) in der entsprechenden Dropdownliste in der Spalte **AUTOPROTECT** den [automatischen Schutz](backup-azure-sql-database.md#enable-auto-protection) für die gesamte Instanz oder Always On-Verfügbarkeitsgruppe aktivieren. Das Feature für den [automatischen Schutz](backup-azure-sql-database.md#enable-auto-protection) ermöglicht nicht nur den gleichzeitigen Schutz aller vorhandenen Datenbanken, sondern schützt auch automatisch alle neuen Datenbanken, die dieser Instanz oder Verfügbarkeitsgruppe in Zukunft hinzugefügt werden.  

   ![Aktivieren des automatischen Schutzes für die AlwaysOn-Verfügbarkeitsgruppe](./media/backup-azure-sql-database/enable-auto-protection.png)

## <a name="next-steps"></a>Nächste Schritte

- [Informieren Sie sich über](restore-sql-database-azure-vm.md) das Wiederherstellen von gesicherten SQL Server-Datenbanken.
- [Informieren Sie sich über](manage-monitor-sql-database-backup.md) das Verwalten von gesicherten SQL Server-Datenbanken.

