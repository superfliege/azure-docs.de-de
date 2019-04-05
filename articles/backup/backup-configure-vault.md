---
title: Sichern von Windows-Computern mit dem Azure Backup-MARS-Agent
description: Verwenden Sie den Microsoft Recovery Services (MARS)-Agent von Azure Backup zum Sichern von Windows-Computern.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: 7a1bd6da68b49481429709c7e4fd37dd5c07ae2c
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58200785"
---
# <a name="back-up-windows-machines-with-the-azure-backup-mars-agent"></a>Sichern von Windows-Computern mit dem Azure Backup-MARS-Agent

Dieser Artikel erklärt, wie Sie Windows-Computer mit dem [Azure Backup](backup-overview.md)-Dienst und dem Microsoft Azure Recovery Services (MARS)-Agent, auch bekannt als Azure Backup-Agent, sichern.

In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen und Erstellen eines Recovery Services-Tresors
> * Herunterladen und Einrichten des MARS-Agents
> * Erstellen einer Sicherungsrichtlinie und eines Zeitplans
> * Durchführen einer Ad-hoc-Sicherung

## <a name="about-the-mars-agent"></a>Informationen zum MARS-Agent

Der MARS-Agent wird von Azure Backup verwendet, um Dateien, Ordner und den Systemstatus von lokalen Computern und Azure-VMs in einem Backup Recovery Services-Tresor in Azure zu sichern. Sie können den Agent wie folgt ausführen:

- Führen Sie den Agenten direkt auf lokalen Windows-Computern aus, damit sie direkt auf einen Backup-Recovery Services-Tresor in Azure sichern können.
- Führen Sie die Azure-VMs des Agents unter Windows aus (Seite an Seite mit der Azure-VM-Backup-Erweiterung), um bestimmte Dateien und Ordner auf der VM zu sichern.
- Führen Sie den Agent auf einem Microsoft Azure Backup Server (MABS) oder einem System Center Data Protection Manager (DPM)-Server aus. In diesem Szenario werden Computer und Workloads auf MABS/DPM gesichert, und dann wird MABS/DPM mit dem MARS-Agent in einem Tresor in Azure gesichert.
Was Sie sichern können, hängt davon ab, wo der Agent installiert ist.

> [!NOTE]
> Die primäre Methode für die Sicherung von Azure-VMs ist die Verwendung einer Azure Backup-Erweiterung auf der VM. Damit wird die gesamte VM gesichert. Sie können den MARS-Agent neben der Erweiterung installieren und verwenden, wenn Sie bestimmte Dateien und Ordner auf der VM sichern möchten. [Weitere Informationen](backup-architecture.md#architecture-direct-backup-of-azure-vms)

![Sicherungsprozessschritte](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Vorbereitung

- [Erfahren Sie](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders), wie Azure Backup Windows-Computer mit dem Azure Backup-MARS-Agent sichert.
- [Erfahren Sie mehr](backup-architecture.md#architecture-back-up-to-dpmmabs) über die Sicherungsarchitektur, die den MARS-Agent auf einem sekundären MABS- oder DPM-Server ausführt.
- [Überprüfen](backup-support-matrix-mars-agent.md), was unterstützt wird und was mit dem MARS-Agent gesichert werden kann.
- Überprüfen Sie den Internetzugang auf den Computern, die Sie sichern möchten.
- Um einen Server oder Client in Azure zu sichern, benötigen Sie ein Azure-Konto. Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

### <a name="verify-internet-access"></a>Überprüfen des Internetzugriffs

Wenn Ihr Computer über einen eingeschränkten Internetzugang verfügt, stellen Sie sicher, dass die Firewalleinstellungen auf dem Computer oder Proxy diese URLs und IP-Adressen zulassen:

**URLs**

- www\.msftncsi.com
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

**IP-Adresse**

- 20.190.128.0/18
- 40.126.0.0/18


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Ein Recovery Services-Tresor speichert alle Sicherungen und Wiederherstellungspunkte, die Sie im Laufe der Zeit erstellen, und enthält die Sicherungsrichtlinie, die auf gesicherte Computer angewendet wird. Erstellen Sie wie folgt einen Tresor:

1. Melden Sie sich mit Ihrem Azure-Abonnement am [Azure-Portal](https://portal.azure.com/) an.
2. Geben Sie im Suchfeld **Recovery Services** ein, und klicken Sie auf **Recovery Services-Tresore**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png)

3. Klicken Sie im Menü **Recovery Services-Tresore** auf **+Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

4. Geben Sie unter **Name**einen Anzeigenamen für den Tresor ein.

   - Der Name muss für das Azure-Abonnement eindeutig sein.
   - Er kann zwischen 2 bis 50 Zeichen enthalten.
   - Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.

5. Wählen Sie das Azure-Abonnement, die Ressourcengruppe und die geografische Region, in der der Tresor erstellt werden soll. Die Sicherungsdaten werden an den Tresor gesendet. Klicken Sie dann auf **Erstellen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

   - Es kann eine Weile dauern, bis der Tresor fertiggestellt wird.
   - Verfolgen Sie die Statusbenachrichtigungen rechts oben im Portal. Sollte der Tresor auch nach mehreren Minuten nicht angezeigt werden, klicken Sie auf **Aktualisieren**.

     ![Klicken auf die Schaltfläche „Aktualisieren“](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)

### <a name="set-storage-redundancy"></a>Festlegen der Speicherredundanz

Azure Backup übernimmt automatisch die Speicherung für den Tresor. Sie müssen angeben, wie dieser Speicher repliziert wird.

1. Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf den neuen Tresor. Klicken Sie im Abschnitt **Einstellungen** auf **Eigenschaften**.
2. Klicken Sie in **Eigenschaften** unter **Sicherungskonfiguration** auf **Aktualisieren**.

3. Wählen Sie den Speicherreplikationstyps aus, und klicken Sie auf **Speichern**.

      ![Speicherkonfiguration für neuen Tresor festlegen](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

- Wir empfehlen, dass Sie, wenn Sie Azure als primären Endpunkt für den Sicherungsspeicher verwenden, weiterhin die Standardeinstellung **Georedundant** verwenden.
- Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie **Lokal redundant** aus. Dadurch verringern sich die Kosten für Azure-Speicher.
- Erfahren Sie mehr über [Georedundanz](../storage/common/storage-redundancy-grs.md) and [lokale Redundanz](../storage/common/storage-redundancy-lrs.md).

## <a name="download-the-mars-agent"></a>Herunterladen des MARS-Agents

Laden Sie den MARS-Agent zur Installation auf den Computern herunter, die Sie sichern möchten.

- Wenn Sie den Agent bereits auf einem Computer installiert haben, stellen Sie sicher, dass Sie die neueste Version verwenden.
- Die neueste Version ist im Portal verfügbar, oder mit einem [direkten Download](https://aka.ms/azurebackup_agent).

1. Klicken Sie im Tresor unter **Erste Schritte** auf **Sicherung**.

    ![Öffnen des Blatts „Sicherungsziel“](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

2. Wählen Sie für **Wo wird Ihre Workload ausgeführt?** die Option **Lokal** aus. Sie sollten diese Option auch dann wählen, wenn Sie den MARS-Agent auf einer Azure-VM installieren möchten.
3. Wählen Sie für **Was möchten Sie sichern?** die Option **Dateien und Ordner** und/oder **Systemstatus** aus. Es gibt eine Reihe weiterer Optionen, die jedoch nur unterstützt werden, wenn Sie einen sekundären Sicherungsserver ausführen. Klicken Sie auf **Infrastruktur vorbereiten**.

      ![Konfigurieren von Dateien und Ordnern](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

4. Laden Sie den MARS-Agent auf **Infrastruktur vorbereiten** unter **Recovery Services-Agent installieren** herunter.

    ![Agent für Windows Server oder Windows-Client herunterladen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

5. Klicken Sie im Downloadpopupmenü auf **Speichern**. Die Datei **MARSagentinstaller.exe** wird standardmäßig in Ihrem Downloadordner gespeichert.

6. Aktivieren Sie jetzt das Kontrollkästchen für **Bereits heruntergeladen oder den neuesten Recovery Services-Agent verwenden**, und laden Sie dann die Anmeldeinformationen für den Tresor herunter.

    ![Herunterladen der Tresoranmeldeinformationen](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

7. Klicken Sie auf **Speichern**. Die Datei wird in Ihren Download-Ordner heruntergeladen. Sie können die Datei mit den Anmeldeinformationen für den Tresor nicht öffnen.

## <a name="install-and-register-the-agent"></a>Installieren und Registrieren des Agents

1. Führen Sie die Datei **MARSagentinstaller.exe** auf Computern aus, die Sie sichern möchten.
2. Wählen Sie im Setup-Assistenten des MARS-Agents > **Installationseinstellungen**, und Sie den Installationsort für den Agent und einen Ort für den Cache an. Klicken Sie auf **Weiter**.
   - Azure Backup verwendet den Cache, um Datenmomentaufnahmen zu speichern, bevor sie an Azure gesendet werden.
   - Der verfügbare Speicherplatz im Cachespeicherort sollte mindestens 5 % der Größe der zu sichernden Daten entsprechen.

     ![Installationseinstellungen im MARS-Assistenten](./media/backup-configure-vault/mars1.png)

2. Geben Sie unter **Proxykonfiguration** an, wie sich der Agent, der auf dem Windows-Computer ausgeführt wird, mit dem Internet verbinden soll. Klicken Sie auf **Weiter**.

   - Wenn Sie einen benutzerdefinierten Proxy verwenden, geben Sie die Proxyeinstellungen und ggf. die Anmeldeinformationen an.
   - Denken Sie daran dass der MARS-Agent Zugriff auf [diese URLs](#verify-internet-access) benötigt.

     ![Zugriff auf das Internet für den MARS-Assistenten](./media/backup-configure-vault/mars2.png)

3. Überprüfen Sie unter **Installation** die Voraussetzungen, und klicken Sie auf **Installieren**.
4. Klicken Sie nach der Installation des Agents auf **Mit Registrierung fortfahren**.
5. Wählen Sie unter **Serverassistenz registrieren** > **Tresoridentifikation** die Datei mit den Anmeldeinformationen, die Sie heruntergeladen haben. Klicken Sie auf **Weiter**.

    ![Registrieren – Tresoranmeldeinformationen](./media/backup-configure-vault/register1.png)

6. Geben Sie unter **Verschlüsselungseinstellung** eine Passphrase an, die zum Ver- und Entschlüsseln von Sicherungen für den Computer verwendet werden soll.

    - Speichern Sie die Verschlüsselungspassphrase an einem sicheren Ort.
    - Wenn Sie die Passphrase verlieren oder vergessen, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein. Speichern Sie die Datei daher an einem sicheren Ort. Sie benötigen sie zum Wiederherstellen einer Sicherung.

7. Klicken Sie auf **Fertig stellen**. Der Agent wurde jetzt installiert, und Ihr Computer wurde im Tresor registriert. Sie können die Sicherung jetzt konfigurieren und planen.

## <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Die Sicherungsrichtlinie legt fest, wann Momentaufnahmen der Daten zur Erstellung von Wiederherstellungspunkten gemacht werden sollen und wie lange die Wiederherstellungspunkte aufbewahrt werden.

- Sie konfigurieren eine Sicherungsrichtlinie mit dem MARS-Agent.
- Bei Azure Backup wird die Sommerzeit (DST) nicht automatisch berücksichtigt. Dies kann zu Abweichungen zwischen der tatsächlichen und der geplanten Sicherungszeit führen.

Erstellen Sie eine Richtlinie wie folgt:

1. Öffnen Sie den MARS-Agent auf jedem Computer. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup**suchen.
2. Klicken Sie unter **Aktionen** auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-configure-vault/schedule-first-backup.png)

3. Klicken Sie im Assistenten zum Planen der Sicherung auf **Erste Schritte** und dann auf **Weiter**.
4. Klicken Sie unter **Elemente für Sicherung auswählen** auf **Elemente hinzufügen**.
5. Wählen Sie unter **Elemente auswählen**, was Sie sichern möchten. Klicken Sie dann auf **OK**.
6. Klicken Sie auf der Seite **Elemente für Sicherung auswählen** auf **Weiter**.
7. Geben Sie auf der Seite **Sicherungszeitplan angeben** an, wann Sie tägliche oder wöchentliche Sicherungen erstellen möchten. Klicken Sie auf **Weiter**.

    - Ein Wiederherstellungspunkt wird gleichzeitig mit einer Sicherung erstellt.
    - Die Anzahl der in Ihrer Umgebung erstellten Wiederherstellungspunkte hängt von Ihrem Sicherungszeitplan ab.

1. Es können bis zu drei tägliche Sicherungen geplant werden. Der Screenshot zeigt beispielsweise zwei tägliche Sicherungen, eine um Mitternacht und eine um 18 Uhr.

    ![Täglicher Zeitplan](./media/backup-configure-vault/day-schedule.png)

9. Sie können auch wöchentliche Sicherungen ausführen. Der Screenshot zeigt beispielsweise Sicherungen, die jeden zweiten Sonntag und Mittwoch um 9:30 Uhr und 1:00 Uhr erstellt werden.

    ![Wöchentlicher Zeitplan](./media/backup-configure-vault/week-schedule.png)

8. Geben Sie auf der Seite **Aufbewahrungsrichtlinie auswählen** an, wie Sie historische Kopien Ihrer Daten speichern. Klicken Sie auf **Weiter**.

   - Die Aufbewahrungseinstellungen legen fest, welche Wiederherstellungspunkte gespeichert werden sollen und wie lange sie gespeichert werden.
   - Wenn Sie beispielsweise Einstellungen für die tägliche Aufbewahrung festlegen, geben Sie an, dass zu dem für die tägliche Aufbewahrung angegebenen Zeitpunkt der neueste Wiederherstellungspunkt für die angegebene Anzahl von Tagen beibehalten wird. Oder Sie können als weiteres Beispiel eine monatliche Aufbewahrungsrichtlinie angeben, die angibt, dass der am 30. eines jeden Monats erstellte Wiederherstellungspunkt für 12 Monate gespeichert werden soll.
   - Die tägliche und wöchentliche Aufbewahrung der Wiederherstellungspunkte stimmt in der Regel mit dem Sicherungszeitplan überein. Das bedeutet, dass bei planmäßiger Auslösung der Sicherung der von der Sicherung erstellte Wiederherstellungspunkt für die in der täglichen oder wöchentlichen Aufbewahrungsrichtlinie angegebene Dauer gespeichert wird.
   - Der folgende Screenshot zeigt ein Beispiel:
     - Tägliche Sicherungen um Mitternacht und 18 Uhr werden sieben Tage lang aufbewahrt.
     - Sicherungen, die an einem Samstag um Mitternacht und 18 Uhr erstellt werden, werden vier Wochen lang aufbewahrt.
     - Sicherungen, die am Samstag in der letzten Woche des Monats um Mitternacht und 18 Uhr erstellt werden, werden für 12 Monate aufbewahrt. - Sicherungen, die an einem Samstag in der letzten Märzwoche erstellt wurden, werden zehn Jahre lang aufbewahrt.

   ![Beispiel für die Aufbewahrung](./media/backup-configure-vault/retention-example.png)

11. Geben Sie in **Erstsicherungstyp auswählen** an, wie die erste Sicherung erstellt werden soll – über das Netzwerk oder offline. Klicken Sie auf **Weiter**.

10. Lesen Sie sich die Informationen unter **Bestätigung** durch, und klicken Sie dann auf **Fertig stellen**.
11. Klicken Sie auf **Schließen**, nachdem der Assistent die Erstellung des Sicherungszeitplans abgeschlossen hat.

### <a name="perform-the-initial-backup-offline"></a>Durchführen der ersten Sicherung offline

Sie können eine erste Sicherung automatisch über das Netzwerk oder offline durchführen. Wenn Sie bei der ersten Sicherung große Datenmengen haben, die viel Netzwerkbandbreite für die Übertragung benötigen, ist das Offlineseeding sehr praktisch. Eine Offline-Übertragung wird wie folgt ausgeführt:

1. Sie schreiben die Sicherungsdaten an einen Stagingspeicherort.
2. Mit dem Tool AzureOfflineBackupDiskPrep können Sie die Daten vom Stagingspeicherort auf eine oder mehrere SATA-Datenträger kopieren.
3. Das Tool erstellt einen Azure-Importauftrag. Erfahren Sie mehr über [Azure-Import und -Export](https://docs.microsoft.com/azure/storage/common/storage-import-export-service).
4. Sie senden die SATA-Datenträger an ein Azure-Rechenzentrum.
5. Im Rechenzentrum werden die Daten auf den Datenträgern in ein Azure-Speicherkonto kopiert.
6. Azure Backup kopiert die Daten aus dem Speicherkonto in den Tresor, und inkrementelle Sicherungen werden geplant.

[Erfahren Sie mehr](backup-azure-backup-import-export.md) über Offlineseeding.

### <a name="enable-network-throttling"></a>Aktivieren der Netzwerkdrosselung

Sie können steuern, wie die Netzwerkbandbreite vom MARS-Agent genutzt wird, indem Sie die Netzwerkdrosselung aktivieren. Eine Drosselung ist hilfreich, wenn Sie Daten während der Arbeitszeit sichern müssen, aber steuern möchten, wie viel Bandbreite für Sicherungs- und Wiederherstellungsaktivitäten verwendet wird.

- Die Azure Backup-Netzwerkdrosselung verwendet [Quality of Service (QoS)](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-top) auf dem lokalen Betriebssystem.
- Die Netzwerkdrosselung für die Sicherung ist ab Windows Server 2008 R2 und Windows 7 verfügbar. Die Betriebssysteme müssen mit den neuesten Service Packs ausgeführt werden.

Die Netzwerkdrosselung können Sie wie folgt aktivieren:

1. Klicken Sie im MARS-Agent auf **Eigenschaften ändern**.
2. Klicken Sie unter **Drosselung** auf die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**.

    ![Netzwerkdrosselung](./media/backup-configure-vault/throttling-dialog.png)
3. Geben Sie die zulässige Bandbreite während der Arbeit und außerhalb der Arbeitszeit an. Die Bandbreitenwerte beginnen bei 512 KBit/s und reichen bis zu 1.023 MBit/s. Klicken Sie dann auf **OK**.

## <a name="run-an-ad-hoc-backup"></a>Ausführen einer Ad-hoc-Sicherung

1. Klicken Sie im MARS-Agent auf **Jetzt sicher**. Damit wird die erste Replikation über das Netzwerk gestartet.

    ![Windows Server jetzt sichern](./media/backup-configure-vault/backup-now.png)

2. Überprüfen Sie unter **Bestätigung** die Einstellungen, und klicken Sie auf **Sichern**.
3. Klicken Sie auf **Schließen** , um den Assistenten zu schließen. Wenn Sie auf „Schließen“ klicken, bevor die Sicherung abgeschlossen ist, wird der Assistent im Hintergrund weiter ausgeführt.

Nach Abschluss des ersten Backups wird der Status des Auftrags in der Backup-Konsole als **Auftrag abgeschlossen** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](backup-azure-restore-windows-server.md) über das Wiederherstellen von Dateien.
