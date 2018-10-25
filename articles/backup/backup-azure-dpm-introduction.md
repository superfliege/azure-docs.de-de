---
title: Verwenden von DPM zum Sichern von Workloads in Azure
description: Eine Einführung zur Sicherung von DPM-Daten in einem Azure Recovery Services-Tresor.
services: backup
author: adigan
manager: nkolli
keywords: System Center Data Protection Manager, Data Protection Manager, DPM-Sicherung
ms.service: backup
ms.topic: conceptual
ms.date: 8/22/2018
ms.author: adigan
ms.openlocfilehash: 2da5b04f56a5746fb77de6bc954bb5971eb4664b
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885169"
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Vorbereiten der Sicherung von Workloads in Azure mit DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

In diesem Artikel wird erläutert, wie Sie Daten aus System Center Data Protection Manager (DPM) in Azure sichern können, einschließlich:

* Sichern von DPM-Serverdaten in Azure
* Voraussetzungen für eine reibungslose Sicherung
* Typische Fehler und ihre Behandlung
* Unterstützte Szenarien

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel enthält die Informationen und Verfahren für die Wiederherstellung virtueller Computer, die über das Resource Manager-Bereitstellungsmodell bereitgestellt werden.
>
>

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview) sichert Datei- und Anwendungsdaten. Weitere Informationen zu unterstützten Workloads finden Sie [hier](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix). Daten, die in DPM gesichert werden, können auf Band, auf Festplatte gespeichert oder in Azure mit Microsoft Azure Backup gesichert werden. DPM interagiert mit Azure Backup wie folgt:

* **DPM als physischer Server oder auf einem lokalen virtuellen Computer bereitgestellt** – Wenn DPM als physischer Server oder als lokaler Hyper-V-Computer bereitgestellt wird, werden Daten zusätzlich zur Festplatten- und Bandsicherung im Recovery Services-Tresor gesichert.
* **DPM als virtueller Azure-Computer bereitgestellt** – Ab System Center 2012 R2 mit Update 3 können Sie DPM als virtuellen Azure-Computer bereitstellen. Wenn DPM als virtueller Azure-Computer bereitgestellt wird, können Sie Daten auf mit der VM verbundenen Azure-Datenträgern sichern, oder Sie können die Datenspeicherung durch Sichern in einem Recovery Services-Tresor auslagern.

## <a name="why-back-up-dpm-to-azure"></a>Gründe für die Sicherung von DPM in Azure
Durch die Sicherung von DPM-Servern in Azure ergeben sich folgende Geschäftsvorteile:

* Für die lokale DPM-Bereitstellung verwenden Sie Azure als Alternative zur langfristigen Bereitstellung auf Band.
* Bei der Bereitstellung von DTP in einer Azure-VM lagern Sie die Speicherung auf Azure-Datenträger aus. Die Speicherung älterer Daten in Ihrem Recovery Services-Tresor ermöglicht den Ausbau Ihres Unternehmens, indem Sie neue Daten auf Datenträgern speichern.

## <a name="prerequisites"></a>Voraussetzungen
Bereiten Sie Azure Backup wie folgt zum Sichern von DPM-Daten vor:

1. **Erstellen eines Recovery Services-Tresors** : Erstellen Sie einen Tresor im Azure-Portal.
2. **Herunterladen von Tresoranmeldeinformationen**: Laden Sie die Anmeldeinformationen herunter, die Sie zum Registrieren des DPM-Servers beim Recovery Services-Tresor verwenden.
3. **Installieren des Azure Backup-Agent**: Installieren Sie den Agent auf jedem DPM-Server.
4. **Registrieren des Servers**: Registrieren Sie den DPM-Server beim Recovery Services-Tresor.

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="key-definitions"></a>Wichtige Definitionen
Hier folgen einige wichtige Definitionen für die Sicherung in Azure für DPM:

1. **Tresoranmeldeinformationen**: Mithilfe der Tresoranmeldeinformationen wird der Computer zum Senden von Sicherungsdaten an einen identifizierten Tresor im Azure Backup-Dienst authentifiziert. Sie können aus dem Tresor heruntergeladen werden und sind 48 Stunden gültig.
2. **Passphrase**: Mithilfe der Passphrase werden die Sicherungen in der Cloud verschlüsselt. Speichern Sie die Datei an einem sicheren Speicherort, da sie bei einem Wiederherstellungsvorgang benötigt wird.
3. **Sicherheits-PIN**: Wenn Sie die [Sicherheitseinstellungen](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) des Tresors aktiviert haben, ist die Sicherheits-PIN zum Ausführen von Sicherungsvorgängen erforderlich. Diese Multi-Factor Authentication sorgt für eine zusätzliche Sicherheitsebene. 
4. **Wiederherstellungsordner**: Dies ist die Bezeichnung für den Ordner, in den die Sicherungen aus der Cloud während der Cloudwiederherstellungen vorübergehend heruntergeladen werden. Seine Größe sollte ungefähr der Größe der Sicherungselemente entsprechen, die Sie gleichzeitig wiederherstellen möchten.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

## <a name="edit-storage-replication"></a>Tresorspeicherreplikation bearbeiten

Bei der Speicherreplikation haben Sie die Wahl zwischen georedundantem Speicher und lokal redundantem Speicher. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Behalten Sie den georedundanten Speicher bei, wenn der Tresor Ihre primäre Sicherung ist. Befolgen Sie die unten aufgeführten Schritte, um einen lokal redundanten Speicher zu konfigurieren, wenn Sie eine günstigere und weniger langfristige Option wünschen. Weitere Informationen zu den Optionen für [georedundanten](../storage/common/storage-redundancy-grs.md) und [lokal redundanten](../storage/common/storage-redundancy-lrs.md) Speicher finden Sie in der [Übersicht über die Azure Storage-Replikation](../storage/common/storage-redundancy.md).

So bearbeiten Sie die Einstellung für die Speicherreplikation:

> [!NOTE] 
> Konfigurieren Sie die Speicherreplikation vor dem Auslösen der ersten Sicherung. Wenn Sie die Konfiguration für die Speicherreplikation nach der Sicherung des geschützten Elements ändern möchten, müssen Sie den Schutz für den Tresor vor dem Wechseln der Speicherkonfiguration beenden.
>  

1. Wählen Sie den Tresor aus, und öffnen Sie das dazugehörige Dashboard.

2. Klicken Sie im Abschnitt **Verwalten** auf die Option **Sicherungsinfrastruktur**.

3. Wählen Sie im Menü **Speicherkonfiguration** die Speicherreplikationsoption für Ihren Tresor aus.

    ![Liste der Sicherungstresore](./media/backup-azure-dpm-introduction/choose-storage-configuration-rs-vault.png)

    Wenn Sie die Speicheroption für Ihren Tresor ausgewählt haben, können Sie den virtuellen Computer dem Tresor zuordnen. Ermitteln und registrieren Sie die virtuellen Azure-Computer, um mit der Zuordnung zu beginnen.

## <a name="download-vault-credentials"></a>Herunterladen der Tresoranmeldedaten
Die Datei mit Tresoranmeldeinformationen ist ein Zertifikat, das vom Portal für jeden Sicherungstresor generiert wird. Das Portal lädt anschließend den öffentlichen Schlüssel in den Access Control Service (ACS) hoch. Während des Registrierungsworkflows für den Computer, wird der private Schlüssel des Zertifikats für den Benutzer zur Verfügung gestellt, der den Computer authentifiziert. Basierend auf der Authentifizierung sendet der Azure Backup-Dienst Daten an den festgelegte Tresor.

Die Tresoranmeldeinformationen werden nur während des Registrierungsworkflows verwendet. Es ist Aufgabe des Benutzers sicherzustellen, dass die Datei mit den Tresoranmeldeinformationen sicher aufbewahrt wird. Wenn die Anmeldeinformationen verloren gehen, können die Anmeldeinformationen des Tresors zum Registrieren anderer Computer als Tresor verwendet werden. Die Sicherungsdaten sind jedoch durch eine Passphrase verschlüsselt, die dem Kunden gehört. Daher sind vorhandene Sicherungsdaten nicht gefährdet. Um dieses Risiko zu minimieren, laufen die Tresoranmeldeinformationen nach 48 Stunden ab. Sie können die neuen Tresoranmeldeinformationen so oft wie erforderlich herunterladen. Allerdings kann nur die neueste Datei mit Tresoranmeldeinformationen während des Registrierungsworkflows verwendet werden.

Die Datei mit Tresoranmeldeinformationen wird über einen sicheren Kanal aus dem Azure-Portal heruntergeladen. Der Azure Backup-Dienst kennt nicht den privaten Schlüssel des Zertifikats, und der private Schlüssel ist weder im Portal noch im Dienst verfügbar. Gehen Sie folgendermaßen vor, um die Datei mit Tresoranmeldeinformationen auf einen lokalen Computer herunterzuladen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Öffnen Sie den Recovery Services-Tresor, den Sie bei einem DPM-Server registrieren möchten.

3. Das Menü „Einstellungen“ wird standardmäßig geöffnet. Wenn es geschlossen ist, klicken Sie auf dem Tresordashboard auf **Einstellungen**, um das Menü zu öffnen. Klicken Sie im Menü **Einstellungen** auf **Eigenschaften**.

    ![Tresormenü öffnen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. Klicken Sie auf der Seite „Eigenschaften“ unter **Sicherungsanmeldeinformationen** auf **Herunterladen**. Das Portal generiert die Datei mit Tresoranmeldeinformationen, die zum Download zur Verfügung gestellt wird.

    ![Download](./media/backup-azure-dpm-introduction/vault-credentials.png)

Das Portal generiert Tresoranmeldeinformationen mit einer Kombination aus dem Tresornamen und dem aktuellen Datum. Klicken Sie auf **Speichern**, um die Tresoranmeldeinformationen in den Downloadordner des lokalen Kontos herunterzuladen. Sie können auch im Menü „Speichern“ die Option „Speichern unter“ auswählen, um einen anderen Speicherort anzugeben. Es dauert bis zu einer Minute, bis die Datei generiert ist.

### <a name="note"></a>Hinweis
* Stellen Sie sicher, dass die Tresoranmeldeinformationen-Datei an einem Ort gespeichert wird, der von Ihrem Computer aus zugänglich ist. Wenn sie in einer Dateifreigabe/einem SMB gespeichert sind, überprüfen Sie die Zugriffsberechtigungen.
* Die Datei mit den Tresoranmeldeinformationen wird nur während des Registrierungsworkflows verwendet.
* Die Datei mit den Tresoranmeldeinformationen läuft nach 48 Stunden ab und kann über das Portal heruntergeladen werden.

## <a name="install-backup-agent"></a>Installieren des Backup-Agents
Nach dem Erstellen des Azure Backup-Tresors sollte auf jedem Windows-Computer (Windows Server, Windows-Client oder System Center Data Protection Manager-Server oder Azure Backup Server-Computer) ein Agent installiert werden, der das Sichern von Daten und Anwendungen in Azure ermöglicht.

1. Öffnen Sie den Recovery Services-Tresor, bei dem Sie den DPM-Computer registrieren möchten.
2. Das Menü „Einstellungen“ wird standardmäßig geöffnet. Wenn es geschlossen ist, klicken Sie auf **Einstellungen**, um das Menü mit den Einstellungen zu öffnen. Klicken Sie im Menü „Einstellungen“ auf **Eigenschaften**.

    ![Tresormenü öffnen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Klicken Sie auf der Seite „Einstellungen“ unter **Azure Backup-Agent** auf **Herunterladen**.

    ![Download](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Sobald der Agent heruntergeladen ist, führen Sie „MARSAgentInstaller.exe“ aus, um die Installation des Azure Backup-Agents zu starten. Wählen Sie den Installationsordner und den Ablageordner aus, der für den Agent erforderlich ist. Der verfügbare Speicherplatz am angegebenen Speicherort muss mindestens 5 % der Sicherungsdaten betragen.

4. Wenn Sie einen Proxyserver für die Verbindung mit dem Internet verwenden, geben Sie im Bildschirm **Proxykonfiguration** die Details des Proxyservers ein. Wenn Sie einen authentifizierten Proxy verwenden, geben Sie in diesem Bildschirm die Informationen zum Benutzernamen und zum Kennwort ein.

5. Der Azure Backup-Agent installiert .NET Framework 4.5 und Windows PowerShell (falls noch nicht verfügbar), um die Installation abzuschließen.

6. Sobald der Agent installiert ist, **Schließen** Sie das Fenster.

   ![Close (Schließen)](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)

7. Zum **Registrieren des DPM-Servers** klicken Sie im Tresor auf der Registerkarte **Verwaltung** auf **Online**. Wählen Sie dann **Registrieren**. Der Registrierungssetup-Assistent wird geöffnet.

8. Wenn Sie einen Proxyserver für die Verbindung mit dem Internet verwenden, geben Sie im Bildschirm **Proxykonfiguration** die Details des Proxyservers ein. Wenn Sie einen authentifizierten Proxy verwenden, geben Sie in diesem Bildschirm die Informationen zum Benutzernamen und zum Kennwort ein.

    ![Proxykonfiguration](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. Suchen Sie im Bildschirm für die Tresoranmeldeinformationen nach der zuvor heruntergeladenen Datei mit den Tresoranmeldeinformationen, und wählen Sie sie aus.

    ![Tresoranmeldeinformationen](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    Die Datei mit den Tresoranmeldeinformationen gilt nur für 48 Stunden (nachdem sie aus dem Portal heruntergeladen wurde). Wenn in diesem Bildschirm Fehler auftreten (z. B. „Datei mit Tresoranmeldeinformationen abgelaufen“), melden Sie sich beim Azure-Portal an, und laden Sie die Datei mit den Tresoranmeldeinformationen erneut herunter.

    Stellen Sie sicher, dass die Datei mit den Tresoranmeldeinformationen an einem Speicherort verfügbar ist, der für die Setupanwendung zugänglich ist. Wenn Zugriffsfehler auftreten, kopieren Sie die Datei mit den Tresoranmeldeinformationen in einen temporären Speicherort auf diesem Computer, und wiederholen Sie den Vorgang.

    Wenn ein Fehler wegen ungültiger Tresoranmeldeinformationen angezeigt wird (z. B. „Ungültige Tresoranmeldeinformationen angegeben“), ist die Datei entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen, die dem Wiederherstellungsdienst zugeordnet sind. Wiederholen Sie den Vorgang, nachdem Sie eine neue Datei mit Tresoranmeldeinformationen vom Portal heruntergeladen haben. Dieser Fehler tritt i.d R. auf, wenn der Benutzer im Azure-Portal in schneller Folge auf die Option **Tresoranmeldedaten herunterladen** klickt. In diesem Fall ist nur die zweite Datei mit Tresoranmeldeinformationen gültig.

10. Um die Nutzung der Netzwerkbandbreite während Arbeitszeit und arbeitsfreien Stunden zu steuern, können Sie im Bildschirm **Einstellung für die Bandbreiteneinschränkung** die Grenzwerte der Bandbreitennutzung festlegen und Arbeitszeit und arbeitsfreie Stunden definieren.

    ![Einstellung für die Bandbreiteneinschränkung](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)

11. Wechseln Sie im Bildschirm **Einstellungen für den Wiederherstellungsordner** zu dem Ordner, in dem die aus Azure heruntergeladenen Dateien vorübergehend bereitgestellt werden.

    ![Einstellungen für den Wiederherstellungsordner](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)

12. Im Bildschirm **Verschlüsselungseinstellung** können Sie entweder eine Passphrase generieren oder eine Passphrase angeben (mindestens 16 Zeichen). Vergessen Sie nicht, die Passphrase an einem sicheren Speicherort zu speichern.

    ![Verschlüsselung](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Wenn die Passphrase verloren geht oder vergessen wird, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein. Der Endbenutzer besitzt die Verschlüsselungspassphrase, und Microsoft kann nicht auf die vom Endbenutzer verwendete Passphrase zugreifen. Speichern Sie die Datei an einem sicheren Speicherort, da sie bei einem Wiederherstellungsvorgang benötigt wird.
    >
    >

13. Nach dem Klicken auf die Schaltfläche **Registrieren** wird der Computer erfolgreich beim Tresor registriert, und Sie können mit der Sicherung in Microsoft Azure beginnen.

14. Bei Verwendung von Data Protection Manager können Sie die während des Registrierungsworkflows angegebenen Einstellungen ändern, indem Sie auf der Registerkarte **Verwaltung** unter **Online** auf die Option **Konfigurieren** klicken.

## <a name="requirements-and-limitations"></a>Anforderungen (und Einschränkungen)
* DPM kann als physischer Server oder als virtueller Hyper-V-Computer, der auf System Center 2012 SP1 oder System Center 2012 R2 installiert ist, ausgeführt werden. DPM kann auch als virtueller Azure-Computer unter System Center 2012 R2 mit mindestens DPM 2012 R2 Update Rollup 3 oder auf einem virtuellen Windows-Computer in VMware in System Center 2012 R2 mit mindestens Update Rollup 5 ausgeführt werden.
* Wenn Sie DPM mit System Center 2012 SP1 ausführen, sollten Sie Update Rollup 2 für System Center Data Protection Manager SP1 installieren. Dies ist erforderlich, bevor Sie den Azure Backup-Agent installieren können.
* Auf dem DPM-Server muss Azure PowerShell und .Net Framework 4.5 installiert sein.
* DPM kann die meisten Workloads in Azure Backup sichern. Eine vollständige Liste der Unterstützungen finden Sie in den Azure Backup-Elementen weiter unten.
* Mit der Option "Kopieren auf Band" können Daten, die in Azure Backup gespeichert sind, nicht wiederhergestellt werden.
* Sie benötigen ein Azure-Konto mit aktivierter Azure Backup-Funktion. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Erfahren Sie mehr über [Preisgestaltung von Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Für das Verwenden von Azure Backup muss der Azure Backup-Agent auf den Servern installiert werden, die Sie sichern möchten. Jeder Server muss mindestens 5% der zu sichernden Datengröße als freien lokalen Speicher bereitstellen. Beispielsweise erfordert das Sichern von 100GB an Daten mindestens 5GB freien Speicherplatz im Scratchverzeichnis.
* Daten werden im Azure-Tresorspeicher gespeichert. Es gibt keine Beschränkung der Datenmenge, die Sie in einem Azure Backup-Tresor sichern, aber die Größe einer Datenquelle (z. B. ein virtueller Computer oder eine Datenbank) sollte 54400 GB nicht überschreiten.

Diese Dateitypen werden für die Sicherung in Azure unterstützt:

* Verschlüsselt (nur vollständige Sicherungen)
* Komprimiert (inkrementelle Sicherungen werden unterstützt)
* Platzsparend (inkrementelle Sicherungen werden unterstützt)
* Komprimiert und platzsparend (als platzsparend behandelt)

Diese werden nicht unterstützt:

* Server auf Dateisystemen, die nach Groß-/Kleinschreibung unterscheiden, werden nicht unterstützt.
* Feste Links (übersprungen)
* Analysepunkte (übersprungen)
* Verschlüsselt und komprimiert (übersprungen)
* Verschlüsselt und platzsparend (übersprungen)
* Komprimierter Stream
* Platzsparender Stream

> [!NOTE]
> Ab System Center 2012 DPM mit SP1 oder höher können Sie geschützte Workloads in Azure sichern.
>
>
