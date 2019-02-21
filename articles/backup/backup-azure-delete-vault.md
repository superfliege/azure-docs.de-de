---
title: Löschen eines Recovery Services-Tresors in Azure
description: In diesem Artikel erfahren Sie, wie Sie einen Recovery Services-Tresor löschen. Für den Fall, dass beim Löschen eines Tresors Probleme auftreten, enthält der Artikel entsprechende Schritte zur Problembehandlung.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: d15e3773a9b6e3dceb0799d206070730675c211d
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310965"
---
# <a name="delete-a-recovery-services-vault"></a>Löschen eines Recovery Services-Tresors

In diesem Artikel wird beschrieben, wie Sie alle Elemente aus einem Recovery Services-Tresor entfernen und anschließend löschen. Sie können den Recovery Services-Tresor nicht löschen, wenn dieser in einem Server registriert ist und Sicherungsdaten enthält. Wenn Sie versuchen, einen Tresor löschen, dies jedoch nicht möglich ist, ist der Tresor noch für den Empfang von Sicherungsdaten konfiguriert.

Informationen zum Löschen eines Tresors finden im Abschnitt [Löschen eines Tresors aus dem Azure-Portal](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Wenn Sie keine Daten im Recovery Services-Tresor beibehalten und den Tresor löschen möchten, finden Sie im Abschnitt [Löschen des Tresors erzwingen](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force) weitere Informationen. Wenn Sie nicht sicher sind, was der Tresor enthält ist, und Sie sicherstellen müssen, dass Sie den Tresor löschen können, finden Sie im Abschnitt [Entfernen von Tresorabhängigkeiten und Löschen des Tresors](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault) entsprechende Informationen.

## <a name="delete-a-vault-from-azure-portal"></a>Löschen eines Tresors aus dem Azure-Portal

Wenn Sie den Recovery Services-Tresor bereits geöffnet haben, können Sie zum zweiten Schritt springen.

1. Öffnen Sie das Azure-Portal und dann über das Dashboard den Tresor, den Sie löschen möchten.

   Klicken Sie im Hubmenü auf **Alle Dienste**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein, wenn Sie den Recovery Services-Tresor nicht im Dashboard angeheftet haben. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Recovery Services-Tresore**, um die Liste der Tresore in Ihrem Abonnement anzuzeigen.

   ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Die Liste mit den Recovery Services-Tresoren wird angezeigt.

   ![Tresor in Liste auswählen](./media/backup-azure-delete-vault/choose-vault-to-delete-.png)

1. Wählen Sie in der Liste den Tresor aus, den Sie löschen möchten. Wenn Sie den Tresor auswählen, wird das zugehörige Dashboard geöffnet.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

1. Um einen Tresor im Dashboard zu löschen, klicken Sie auf **Löschen**. Sie werden aufgefordert, das Löschen des Tresors zu bestätigen.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

    Wenn die Fehlermeldung **Fehler beim Löschen von Tresor** angezeigt wird, können Sie entweder die Abhängigkeiten aus dem Tresor entfernen, oder Sie können PowerShell verwenden, um das Löschen des Tresors zu erzwingen. Dies wird in den folgenden Abschnitten erläutert.

    ![Fehler beim Löschen des Tresors](./media/backup-azure-delete-vault/vault-delete-error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Löschen des Recovery Services-Tresors erzwingen

Sie können PowerShell verwenden, um das Löschen eines Recovery Services-Tresors zu erzwingen. Erzwingen bedeutet, dass der Recovery Services-Tresor und alle dazugehörigen Sicherungsdaten dauerhaft gelöscht werden.

> [!Warning]
> Wenn Sie PowerShell verwenden, um einen Recovery Services-Tresor zu löschen, müssen Sie sicher sein, dass Sie alle Sicherungsdaten im Tresor dauerhaft löschen möchten.
>

So löschen Sie einen Recovery Services-Tresor:

1. Melden Sie sich beim Azure-Konto an.

   Melden Sie sich mit dem Befehl `Connect-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

   ```powershell
    Connect-AzureRmAccount
   ```
   Bei der ersten Verwendung von Azure Backup müssen Sie den Azure Recovery Service-Anbieter in Ihrem Abonnement mit [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) registrieren.

   ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

1. Öffnen Sie ein PowerShell-Fenster mit Administratorrechten.

1. Verwenden Sie `Set-ExecutionPolicy Unrestricted`, um alle Einschränkungen zu entfernen.

1. Führen Sie den folgenden Befehl aus, um das Azure Resource Manager-Clientpaket von chocolately.org herunterzuladen.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

1. Verwenden Sie den folgenden Befehl aus, um den Azure Resource Manager-API-Client zu installieren.

   `choco.exe install armclient`

1. Erfassen Sie Im Azure-Portal die Abonnement-ID und den Namen der dazugehörigen Ressourcengruppe für den Recovery Services-Tresor, den Sie löschen möchten.

1. Führen Sie in PowerShell mit Ihrer Abonnement-ID, dem Namen der Ressourcengruppe und dem Namen des Recovery Services-Tresors den folgenden Befehl aus. Wenn Sie den Befehl ausführen, werden der Tresor und alle Abhängigkeiten gelöscht.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
   Der Tresor muss leer sein, bevor Sie ihn löschen können. Andernfalls erhalten Sie eine Fehlermeldung: „Der Tresor kann nicht gelöscht werden, weil sich im Tresor vorhandene Ressourcen befinden“. Der folgende Befehl zeigt, wie Sie einen Container in einem Tresor entfernen:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

1. Melden Sie sich bei Ihrem Abonnement im Azure-Portal an, und überprüfen Sie, ob der Tresor gelöscht wurde.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Entfernen von Tresorabhängigkeiten und Löschen des Tresors

Um die Tresorabhängigkeiten manuell zu entfernen, löschen Sie die Konfiguration zwischen den einzelnen Elementen oder Servern und dem Recovery Services-Tresor. Wenn Sie das folgende Verfahren absolvieren, verwenden Sie das Menü **Sicherungselemente** im Menü (siehe Abbildung) für:

* Azure Storage (Azure Files)-Sicherungen
* Sicherungen von SQL Server auf einer Azure-VM
* Azure Virtual Machines-Sicherungen

Verwenden Sie das Menü **Sicherungsinfrastruktur** (siehe Abbildung) für:

* Azure Backup Server-Sicherungen
* System Center DPM-Sicherungen

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

1. Blättern Sie Tresor-Dashboardmenü nach unten zum Abschnitt „Geschützte Elemente“, und klicken Sie auf **Sicherungselemente**. In diesem Menü können Sie Azure-Dateiserver, SQL Server auf einer Azure-VM und Azure Virtual Machines anhalten und löschen. In diesem Beispiel entfernen wir Sicherungsdaten von einem Azure-Dateiserver.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/selected-backup-items.png)

1. Wählen Sie einen Sicherungstyp aus, um alle Elemente dieses Typs anzuzeigen.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

1. Klicken Sie bei allen Elementen der Liste mit der rechten Maustaste auf das Element, und wählen Sie im Kontextmenü die Option **Sicherung beenden** aus.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/stop-backup-item.png)

    Das Menü „Sicherung beenden“ wird geöffnet.

1. Wählen Sie im Menü **Sicherung beenden** unter **Option wählen** die Option **Sicherungsdaten löschen** aus, geben Sie den Namen des Elements ein, und klicken Sie auf **Sicherung beenden**.

    Geben Sie den Namen des Elements ein, um zu bestätigen, dass Sie es löschen möchten. Die Schaltfläche **Sicherung beenden** wird aktiviert, sobald Sie das Element bestätigt haben. Wenn Sie die Daten beibehalten, können Sie den Tresor nicht löschen.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Bei Bedarf können Sie einen Grund angeben, warum Sie die Daten löschen, und Sie können Kommentare hinzufügen. Um zu überprüfen, ob der Vorgang abgeschlossen wurde, können Sie die Azure-Meldungen anzeigen: ![delete backup data](./media/backup-azure-delete-vault/messages.png). <br/>
    Nach Abschluss des Vorgangs sendet der Dienst eine Nachricht mit dem Hinweis, *dass der Sicherungsvorgang beendet wurde und die Sicherungsdaten gelöscht wurden*.

1. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungselemente** und dann auf **Aktualisieren**, um die Elemente des Tresors anzuzeigen.

      ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wenn die Liste keine Elemente enthält, scrollen Sie im Tresormenü von Recovery Services zum Bereich **Zusammenfassung**. Hier sollten keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** angezeigt werden. Falls im Tresor weiterhin Elemente angezeigt werden, kehren Sie zu Schritt 3 oben zurück, um eine andere Elementtypliste auszuwählen.  

1. Klicken Sie auf **Löschen**, wenn in der Symbolleiste des Tresors keine Elemente mehr enthalten sind.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .

## <a name="removing-azure-backup-server-or-dpm"></a>Entfernen von Azure Backup Server oder DPM

1. Blättern Sie Tresor-Dashboardmenü nach unten zum Abschnitt „Verwalten“, und klicken Sie auf **Sicherungsinfrastruktur**.

1. Klicken Sie im Untermenü auf **Sicherungsverwaltungsserver**, um die Azure Backup Server und System Center DPM-Server anzeigen. Sie können Azure-Dateiserver, SQL Server auf einer Azure-VM und Azure Virtual Machines anhalten und löschen.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

1. Mit der rechten Maustaste auf das Element, das Sie löschen möchten, und wählen Sie im Untermenü **Löschen**.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

    Das Menü „Sicherung beenden“ wird geöffnet.

1. Wählen Sie im Menü **Sicherung beenden** unter **Option wählen** die Option **Sicherungsdaten löschen** aus, geben Sie den Namen des Elements ein, und klicken Sie auf **Sicherung beenden**.

    Um zu bestätigen, dass Sie löschen möchten, geben Sie den Namen des Elements ein. Die Schaltfläche **Sicherung beenden** wird aktiviert, sobald Sie das Element bestätigt haben. Wenn Sie die Daten beibehalten, können Sie den Tresor nicht löschen.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Optional können Sie einen Grund angeben, warum Sie die Daten löschen, und Sie können Kommentare hinzufügen. Um zu überprüfen, ob der Vorgang abgeschlossen wurde, können Sie die Azure-Meldungen anzeigen: ![delete backup data](./media/backup-azure-delete-vault/messages.png). <br/>
    Nach Abschluss des Vorgangs sendet der Dienst eine Nachricht mit dem Hinweis, dass der Sicherungsvorgang beendet wurde und die Sicherungsdaten gelöscht wurden.

1. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungselemente** und dann auf **Aktualisieren**, um die restlichen Elemente des Tresors anzuzeigen.

      ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wenn die Liste keine Elemente enthält, scrollen Sie im Tresormenü von Recovery Services zum Bereich **Zusammenfassung**. Hier sollten keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** angezeigt werden. Falls im Tresor weiterhin Elemente angezeigt werden, kehren Sie zu Schritt 3 oben zurück, um eine andere Elementtypliste auszuwählen.  
1. Wenn sich keine Elemente mehr in Tresor befinden, klicken Sie im Tresor-Dashboard auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .


## <a name="removing-azure-backup-agent-recovery-points"></a>Entfernen von Wiederherstellungspunkten für Azure Backup-Agent

1. Blättern Sie Tresor-Dashboardmenü nach unten zum Abschnitt „Verwalten“, und klicken Sie auf **Sicherungsinfrastruktur**.

1. Klicken Sie im Untermenü auf **Geschützte Server**, um eine Liste der geschützten Servertypen anzuzeigen, einschließlich Azure Backup-Agent.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/identify-protected-servers.png)

1. Klicken Sie in der Liste **Geschützte Server** auf Azure Backup-Agent.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

    Die Liste der Server, die mit Azure Backup-Agent geschützt werden, wird geöffnet.

    ![einen bestimmten geschützten Server auswählen](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

1. Klicken Sie in der Liste der Server auf einen Server, um dessen Menü zu öffnen.

    ![Dashboard des ausgewählten Servers anzeigen](./media/backup-azure-delete-vault/selected-protected-server.png)

1. Klicken Sie im Dashboardmenü des ausgewählten Servers auf **Löschen**.

    ![ausgewählten Server entfernen](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

1. Geben Sie im Menü **Löschen** den Namen des Elements ein, und klicken Sie auf **Löschen**.

    Geben Sie den Namen des Elements ein, um zu bestätigen, dass Sie es löschen möchten. Die Schaltfläche **Löschen** wird aktiviert, sobald Sie das Element bestätigt haben.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

    Optional können Sie einen Grund angeben, warum Sie die Daten löschen, und Sie können Kommentare hinzufügen. Um zu überprüfen, ob der Vorgang abgeschlossen wurde, können Sie die Azure-Meldungen anzeigen: ![delete backup data](./media/backup-azure-delete-vault/messages.png). <br/>
    Nach Abschluss des Vorgangs sendet der Dienst eine Nachricht mit dem Hinweis, dass der Sicherungsvorgang beendet wurde und die Sicherungsdaten gelöscht wurden.

1. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungselemente** und dann auf **Aktualisieren**, um die restlichen Elemente des Tresors anzuzeigen.

      ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wenn die Liste keine Elemente enthält, scrollen Sie im Tresormenü von Recovery Services zum Bereich **Zusammenfassung**. Hier sollten keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** angezeigt werden. Falls im Tresor weiterhin Elemente angezeigt werden, kehren Sie zu Schritt 3 oben zurück, um eine andere Elementtypliste auszuwählen.  
1. Wenn sich keine Elemente mehr in Tresor befinden, klicken Sie im Tresor-Dashboard auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Was passiert, wenn ich den Sicherungsprozess beende, die Daten aber beibehalten?

Wenn Sie den Sicherungsprozess beendet und die Daten versehentlich *beibehalten* haben, müssen Sie die Sicherungsdaten löschen, bevor Sie den Tresor löschen können. So löschen Sie die gesicherten Daten

1. Klicken Sie im Menü **Sicherungselemente** mit der rechten Maustaste auf das Element, und klicken Sie dann im Kontextmenü auf **Sicherungsdaten löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    Das Menü **Sicherungsdaten löschen** wird geöffnet.
1. Geben Sie im Menü **Sicherungsdaten löschen** den Namen des Elements ein, und klicken Sie auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Springen Sie nach dem Löschen der Daten zurück zu Schritt 4c, und setzen Sie den Vorgang fort.
