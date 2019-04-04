---
title: Löschen eines Recovery Services-Tresors in Azure
description: In diesem Artikel wird beschrieben, wie Sie einen Recovery Services-Tresor löschen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: raynew
ms.openlocfilehash: e83698af6bb1caab1568375b726753d34a8c8467
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57861348"
---
# <a name="delete-a-recovery-services-vault"></a>Löschen eines Recovery Services-Tresors

In diesem Artikel wird beschrieben, wie Sie einen Recovery Services-Tresor von [Azure Backup](backup-overview.md) löschen. Außerdem enthält er Anweisungen zum Entfernen von Abhängigkeiten und anschließendem Löschen eines Tresors sowie zum erzwungenen Löschen eines Tresors.




## <a name="before-you-start"></a>Vorbereitung

Bevor Sie beginnen, ist es wichtig zu verstehen, dass Sie keine Recovery Services-Tresore löschen können, in denen Server registriert oder Sicherungsdaten enthalten sind.


- Sie müssen die Registrierungen der enthaltenen Server aufheben und die Daten entfernen, um einen Tresor ordnungsgemäß zu löschen.
- Wenn Sie keine im Recovery Services-Tresor enthaltenen Daten beibehalten und den Tresor löschen möchten, können Sie das Löschen des Tresors erzwingen.
- Wenn Sie versuchen, einen Tresor löschen, dies jedoch nicht möglich ist, ist der Tresor noch für den Empfang von Sicherungsdaten konfiguriert.

Informationen zum Löschen eines Tresors finden im Abschnitt [Löschen eines Tresors aus dem Azure-Portal](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Weitere Informationen finden Sie unter [Delete the vault by force (Erzwingen des Löschens des Tresors)](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force). Wenn Sie nicht sicher sind, was der Tresor enthält ist, und Sie sicherstellen müssen, dass Sie den Tresor löschen können, finden Sie im Abschnitt [Entfernen von Tresorabhängigkeiten und Löschen des Tresors](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault) entsprechende Informationen.

## <a name="delete-a-vault-from-the-azure-portal"></a>Löschen eines Tresors aus dem Azure-Portal

1. Öffnen Sie die Liste Ihrer Recovery Services-Tresore im Portal.
2. Wählen Sie in der Liste den Tresor aus, den Sie löschen möchten. Das Dashboard des Tresors wird geöffnet.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

1. Klicken Sie im Dashboard des Tresors auf **Löschen**. Geben Sie an, dass Sie den Tresor löschen möchten.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

2. Wenn Tresorabhängigkeiten vorhanden sind, wird die Fehlermeldung **Fehler beim Löschen von Tresor** angezeigt: 

    ![Fehler beim Löschen des Tresors](./media/backup-azure-delete-vault/vault-delete-error.png)

    - Führen Sie die Anweisungen zum Entfernen der Abhängigkeiten aus, bevor Sie den Tresor löschen.
    - [Führen Sie diese Anweisungen aus](#delete-the-recovery-services-vault-by-force), um das Löschen des Tresors mit PowerShell zu erzwingen. 

## <a name="delete-the-recovery-services-vault-by-force"></a>Löschen des Recovery Services-Tresors erzwingen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sie können PowerShell verwenden, um das Löschen eines Recovery Services-Tresors zu erzwingen. Das bedeutet, dass der Tresor und alle zugehörigen Sicherungsdaten unwiderruflich gelöscht werden. 

> [!Warning]
> Wenn Sie PowerShell verwenden, um einen Recovery Services-Tresor zu löschen, müssen Sie sicher sein, dass Sie alle Sicherungsdaten im Tresor dauerhaft löschen möchten.
>

So löschen Sie einen Recovery Services-Tresor:

1. Melden Sie sich mit dem Befehl `Connect-AzAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die angezeigten Anweisungen.

   ```powershell
    Connect-AzAccount
   ```
2. Bei der ersten Verwendung von Azure Backup müssen Sie den Azure Recovery Service-Anbieter in Ihrem Abonnement mit [Register-AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider) registrieren.

   ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

3. Öffnen Sie ein PowerShell-Fenster mit Administratorrechten.
4. Verwenden Sie `Set-ExecutionPolicy Unrestricted`, um alle Einschränkungen zu entfernen.
5. Führen Sie den folgenden Befehl aus, um das Azure Resource Manager-Clientpaket von chocolately.org herunterzuladen.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

6. Verwenden Sie den folgenden Befehl aus, um den Azure Resource Manager-API-Client zu installieren.

   `choco.exe install armclient`

7. Suchen Sie im Azure-Portal nach der Abonnement-ID und dem Ressourcengruppennamen für den Tresor, den Sie löschen möchten.
8. Führen Sie in PowerShell mit Ihrer Abonnement-ID, dem Namen der Ressourcengruppe und dem Namen des Tresors den folgenden Befehl aus. Wenn Sie den Befehl ausführen, werden der Tresor und alle Abhängigkeiten gelöscht.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
9. Wenn der Tresor nicht leer ist, wird die folgende Fehlermeldung angezeigt: „Vault cannot be deleted as there are existing resources within this vault“ (Der Tresor kann nicht gelöscht werden, weil darin Ressourcen enthalten sind). Gehen Sie folgendermaßen vor, um in einem Tresor enthaltene Ressourcen zu entfernen:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

10. Melden Sie sich bei Ihrem Abonnement im Azure-Portal an, und überprüfen, ob der Tresor gelöscht wurde.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Entfernen von Tresorabhängigkeiten und Löschen des Tresors

Sie können die Tresorabhängigkeiten wie folgt manuell entfernen:

- Entfernen Sie die Abhängigkeiten im Menü **Sicherungselemente**:
    - Azure Storage (Azure Files)-Sicherungen
    - Sicherungen von SQL Server auf einer Azure-VM
    - Azure Virtual Machines-Sicherungen
- Entfernen Sie die Abhängigkeiten im Menü **Sicherungsinfrastruktur**:
    - Microsoft Azure Backup Server-Sicherungen (MABS)
    - System Center DPM-Sicherungen

![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

### <a name="remove-backup-items"></a>Entfernen von Sicherungselementen

Dieses Verfahren stellt ein Beispiel für das Entfernen von Sicherungsdaten aus Azure Files dar.

1. Klicken Sie auf **Sicherungselemente** > **Azure Storage (Azure Files)**.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

2. Klicken Sie mit der rechten Maustaste auf die einzelnen zu entfernenden Azure Files-Elemente, und wählen Sie **Sicherung beenden** aus.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/stop-backup-item.png)


3. Wählen Sie unter **Sicherung beenden** > **Option auswählen** die Option **Sicherungsdaten löschen** aus.
4. Geben Sie den Namen des Elements ein, und klicken Sie auf **Sicherung beenden**. 
   - Hiermit bestätigen Sie, dass Sie das Element löschen möchten.
   - Nach dieser Bestätigung wird die Schaltfläche **Sicherung beenden** aktiviert.
   - Wenn Sie die Daten beibehalten und nicht löschen, können Sie den Tresor nicht löschen.

     ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Sie können optional einen Grund angeben, warum Sie die Daten löschen, und Kommentare hinzufügen.
6. Überprüfen Sie die Azure-Meldungen, um sicherzustellen, dass der Löschvorgang abgeschlossen wurde. ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png).
7. Nach Abschluss des Vorgangs zeigt der Dienst die folgende Nachricht an: **the backup process was stopped and the backup data was deleted** (Der Sicherungsvorgang wurde beendet und die Sicherungsdaten wurden gelöscht).
8. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungselemente** und dann auf **Aktualisieren**, um die Elemente des Tresors anzuzeigen.

      ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/empty-items-list.png)


### <a name="remove-backup-infrastructure-servers"></a>Entfernen von Servern der Sicherungsinfrastruktur

1. Klicken Sie im Menü des Tresordashboards auf **Sicherungsinfrastruktur**.
2. Klicken Sie auf **Sicherungsverwaltungsserver**, um die Server anzuzeigen. 

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

2. Klicken Sie mit der rechten Maustaste auf das Element, und wählen Sie dann **Löschen** aus.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

3. . Wählen Sie unter **Sicherung beenden** > **Option auswählen** die Option **Sicherungsdaten löschen** aus.
4. Geben Sie den Namen des Elements ein, und klicken Sie auf **Sicherung beenden**. 
   - Hiermit bestätigen Sie, dass Sie das Element löschen möchten.
   - Nach dieser Bestätigung wird die Schaltfläche **Sicherung beenden** aktiviert.
   - Wenn Sie die Daten beibehalten und nicht löschen, können Sie den Tresor nicht löschen.

     ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Sie können optional einen Grund angeben, warum Sie die Daten löschen, und Kommentare hinzufügen.
6. Überprüfen Sie die Azure-Meldungen, um sicherzustellen, dass der Löschvorgang abgeschlossen wurde. ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png).
7. Nach Abschluss des Vorgangs zeigt der Dienst die folgende Nachricht an: **the backup process was stopped and the backup data was deleted** (Der Sicherungsvorgang wurde beendet und die Sicherungsdaten wurden gelöscht).
8. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungselemente** und dann auf **Aktualisieren**, um die Elemente des Tresors anzuzeigen.


### <a name="remove-azure-backup-agent-recovery-points"></a>Entfernen von Wiederherstellungspunkten für den Azure Backup-Agent

1. Klicken Sie im Menü des Tresordashboards auf **Sicherungsinfrastruktur**.
2. Klicken Sie auf **Sicherungsverwaltungsserver**, um die Infrastrukturserver anzuzeigen.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. Klicken Sie in der Liste **Geschützte Server** auf Azure Backup-Agent.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Wählen Sie den Server in der Liste der Server aus, die mit dem Azure Backup-Agent geschützt werden.

    ![einen bestimmten geschützten Server auswählen](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. Klicken Sie im Dashboard des ausgewählten Servers auf **Löschen**.

    ![ausgewählten Server entfernen](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Geben Sie im Menü **Löschen** den Namen des Elements ein, und klicken Sie auf **Löschen**.
   - Hiermit bestätigen Sie, dass Sie das Element löschen möchten.
   - Nach dieser Bestätigung wird die Schaltfläche **Sicherung beenden** aktiviert.
   - Wenn Sie die Daten beibehalten und nicht löschen, können Sie den Tresor nicht löschen.

     ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. Sie können optional einen Grund angeben, warum Sie die Daten löschen, und Kommentare hinzufügen.
8. Überprüfen Sie die Azure-Meldungen, um sicherzustellen, dass der Löschvorgang abgeschlossen wurde. ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png).
7. Nach Abschluss des Vorgangs zeigt der Dienst die folgende Nachricht an: **the backup process was stopped and the backup data was deleted** (Der Sicherungsvorgang wurde beendet und die Sicherungsdaten wurden gelöscht).
8. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungselemente** und dann auf **Aktualisieren**, um die Elemente des Tresors anzuzeigen.



### <a name="delete-the-vault-after-removing-dependencies"></a>Löschen des Tresors nach dem Entfernen der Abhängigkeiten

1. Nachdem alle Abhängigkeiten entfernt wurden, scrollen Sie im Tresormenü zum Bereich **Zusammenfassung**.

    - Hier sollten keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** angezeigt werden.
    - Wenn weiterhin Elemente im Tresor angezeigt werden, entfernen Sie diese.

2. Wenn sich keine Elemente mehr in Tresor befinden, klicken Sie im Tresor-Dashboard auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen. Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Was passiert, wenn ich den Sicherungsprozess beende, die Daten aber beibehalten?

Wenn Sie den Sicherungsvorgang beenden und die Daten versehentlich beibehalten haben, müssen Sie die Sicherungsdaten wie in den obigen Abschnitten löschen.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über Recovery Services-Tresore](backup-azure-recovery-services-vault-overview.md)
