---
title: Löschen eines Recovery Services-Tresors in Azure
description: In diesem Artikel wird beschrieben, wie Sie einen Recovery Services-Tresor löschen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: raynew
ms.openlocfilehash: e7cea725a25d48ac9f1ffad6acc434e21145890e
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473243"
---
# <a name="delete-a-recovery-services-vault"></a>Löschen eines Recovery Services-Tresors

In diesem Artikel wird beschrieben, wie Sie einen Recovery Services-Tresor von [Azure Backup](backup-overview.md) löschen. Außerdem enthält er Anweisungen zum Entfernen von Abhängigkeiten und anschließendem Löschen eines Tresors sowie zum erzwungenen Löschen eines Tresors.


## <a name="before-you-start"></a>Vorbereitung

Bevor Sie beginnen, ist es wichtig zu verstehen, dass Sie keine Recovery Services-Tresore löschen können, in denen Server registriert oder Sicherungsdaten enthalten sind.

- Zum ordnungsgemäßen Löschen eines Tresors heben Sie die Registrierungen der darin enthaltenen Server auf, entfernen die Tresordaten und löschen anschließend den Tresor.
- Wenn Sie einen Tresor zu löschen versuchen, der immer noch abhängig ist, wird eine Fehlermeldung ausgegeben. Dann müssen Sie die Tresorabhängigkeiten manuell entfernen, einschließlich Folgendem:
    - Gesicherte Elemente
    - Geschützte Server
    - Sicherungsverwaltungsserver (Azure Backup Server, DPM) ![wählen Ihren Tresor aus, um dessen Dashboard zu öffnen](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png).
- Wenn Sie keine im Recovery Services-Tresor enthaltenen Daten beibehalten und den Tresor löschen möchten, können Sie das Löschen des Tresors erzwingen.
- Wenn Sie versuchen, einen Tresor löschen, dies jedoch nicht möglich ist, ist der Tresor noch für den Empfang von Sicherungsdaten konfiguriert.


## <a name="delete-a-vault-from-the-azure-portal"></a>Löschen eines Tresors aus dem Azure-Portal

1. Öffnen Sie das Dashboard des Tresors.  
2. Klicken Sie im Dashboard auf **Löschen**. Geben Sie an, dass Sie den Tresor löschen möchten.

    ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

Wenn eine Fehlermeldung angezeigt wird, entfernen Sie [Sicherungselemente](#remove-backup-items), [Infrastrukturserver](#remove-backup-infrastructure-servers) und [Wiederherstellungspunkte](#remove-azure-backup-agent-recovery-points). Löschen Sie anschließend den Tresor.

![Tresorfehler beim Löschen](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Löschen des Recovery Services-Tresors mit dem Azure Resource Manager-Client

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Installieren Sie chocolatey von [hier](https://chocolatey.org/) aus, und installieren Sie ARMClient mit folgendem Befehl:

   ` choco install armclient --source=https://chocolatey.org/api/v2/ `
2. Anmelden beim Azure-Konto unter Ausführung des folgenden Befehls

    ` ARMClient.exe login [environment name] `

3. Suchen Sie im Azure-Portal nach der Abonnement-ID und dem Ressourcengruppennamen für den Tresor, den Sie löschen möchten.

Weitere Informationen zum Befehl ARMClient finden Sie in diesem [Dokument](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Löschen des Recovery Services-Tresors mit dem Azure Resource Manager

1. Führen Sie mit Ihrer Abonnement-ID, dem Namen der Ressourcengruppe und dem Namen des Tresors den folgenden Befehl aus. Wenn Sie den Befehl ausführen, wird der Tresor gelöscht, wenn keine Abhängigkeiten vorliegen.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Wenn der Tresor nicht leer ist, wird die folgende Fehlermeldung angezeigt: „Vault cannot be deleted as there are existing resources within this vault“ (Der Tresor kann nicht gelöscht werden, weil darin Ressourcen enthalten sind). Gehen Sie folgendermaßen vor, um in einem Tresor enthaltene geschützte Elemente / Container zu entfernen:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Vergewissern Sie sich im Azure-Portal, dass der Tresor gelöscht wird.


## <a name="remove-vault-items-and-delete-the-vault"></a>Entfernen von Tresorelementen und Löschen des Tresors

Diese Verfahren enthalten einige Beispiele zum Entfernen von Sicherungsdaten und Infrastrukturservern. Nachdem der gesamte Inhalt eines Tresors entfernt wurde, können Sie ihn löschen.

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

3. Klicken Sie mit der rechten Maustaste auf das Element, und wählen Sie dann **Löschen** aus.
4. Überprüfen Sie die Azure-Meldungen, um sicherzustellen, dass der Löschvorgang abgeschlossen wurde. ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png).
5. Nach Abschluss des Vorgangs zeigt der Dienst die folgende Nachricht an: **the backup process was stopped and the backup data was deleted** (Der Sicherungsvorgang wurde beendet und die Sicherungsdaten wurden gelöscht).
6. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungsinfrastruktur** und dann auf **Aktualisieren**, um die Elemente im Tresor anzuzeigen.


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

     ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. Sie können optional einen Grund angeben, warum Sie die Daten löschen, und Kommentare hinzufügen.
8. Überprüfen Sie die Azure-Meldungen, um sicherzustellen, dass der Löschvorgang abgeschlossen wurde. ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png).
9. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungsinfrastruktur** und dann auf **Aktualisieren**, um die Elemente im Tresor anzuzeigen.

### <a name="delete-the-vault-after-removing-dependencies"></a>Löschen des Tresors nach dem Entfernen der Abhängigkeiten

1. Nachdem alle Abhängigkeiten entfernt wurden, scrollen Sie im Tresormenü zum Bereich **Zusammenfassung**.
2. Vergewissern Sie sich, dass keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierte Elemente** aufgelistet werden. Wenn weiterhin Elemente im Tresor angezeigt werden, entfernen Sie diese.

3. Wenn sich keine Elemente mehr in Tresor befinden, klicken Sie im Tresor-Dashboard auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen. Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Was passiert, wenn ich den Sicherungsprozess beende, die Daten aber beibehalten?

Wenn Sie den Sicherungsvorgang beenden und die Daten versehentlich beibehalten haben, müssen Sie die Sicherungsdaten wie in den obigen Abschnitten löschen.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über Recovery Services-Tresore](backup-azure-recovery-services-vault-overview.md)
