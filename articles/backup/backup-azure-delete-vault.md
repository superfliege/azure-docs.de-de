---
title: Löschen eines Recovery Services-Tresors in Azure
description: In diesem Artikel erfahren Sie, wie Sie einen Recovery Services-Tresor löschen. Für den Fall, dass beim Löschen eines Tresors Probleme auftreten, enthält der Artikel entsprechende Schritte zur Problembehandlung.
services: service-name
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: markgal
ms.openlocfilehash: 844a70aa6fe003c6ad5816aaec9c32db9104c620
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605339"
---
# <a name="delete-a-recovery-services-vault"></a>Löschen eines Recovery Services-Tresors
In diesem Artikel erfahren Sie, wie Sie einen Recovery Services-Tresor über das Azure-Portal löschen. Wenn Sie zuvor Sicherungstresore verwendet haben, wurden diese automatisch in Recovery Services-Tresore umgewandelt.   

Das Löschen eines Recovery Services-Tresors ist ein Prozess, der aus nur einem Schritt besteht – *sofern der Tresor keine Ressourcen enthält*. Bevor Sie einen Recovery Services-Tresor löschen, müssen Sie alle Ressourcen im Tresor entfernen oder löschen. Wenn Sie versuchen, einen Tresor zu löschen, der Ressourcen enthält, erhalten Sie eine Fehlermeldung wie in der folgenden Abbildung:

![Fehler beim Löschen des Tresors](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Beim Klicken auf **Wiederholen** wird so lange der gleiche Fehler angezeigt, bis Sie die Ressourcen aus dem Tresor gelöscht haben. Wenn Sie bei dieser Fehlermeldung hängen bleiben, können Sie auf **Abbrechen** klicken und die nachstehenden Schritte ausführen, um die Ressourcen im Tresor zu löschen.

## <a name="removing-items-from-a-vault-protecting-a-vm"></a>Entfernen von Elementen aus einem Tresor zum Schutz eines virtuellen Computers
Wenn Sie den Recovery Services-Tresor bereits geöffnet haben, können Sie zum zweiten Schritt springen.

1. Öffnen Sie das Azure-Portal und dann über das Dashboard den Tresor, den Sie löschen möchten.

   Klicken Sie im Hubmenü auf **Weitere Dienste**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein, wenn Sie den Recovery Services-Tresor nicht im Dashboard angeheftet haben. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Recovery Services-Tresore**.

   ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Die Liste mit den Recovery Services-Tresoren wird angezeigt. Wählen Sie in der Liste den Tresor aus, den Sie löschen möchten.

   ![Tresor in Liste auswählen](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. Sehen Sie sich in der Tresoransicht den Bereich **Zusammenfassung** an. Wenn ein Tresor gelöscht werden soll, dürfen keine geschützten Elemente vorhanden sein. Falls für **Sicherungselemente** oder **Sicherungsverwaltungsserver** nicht der Wert 0 angezeigt wird, müssen Sie diese Elemente entfernen. Wenn der Tresor Daten enthält, kann er nicht gelöscht werden.

    ![Bereich „Zusammenfassung“ für geschützte Elemente anzeigen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    VMs und Dateien/Ordner werden als Sicherungselemente angesehen und sind im Bereich „Zusammenfassung“ unter **Sicherungselemente** aufgeführt. Ein DPM-Server ist unter „Zusammenfassung“ im Bereich **Sicherungsverwaltungsserver** aufgeführt. **Replizierte Elemente** bezieht sich auf den Azure Site Recovery-Dienst.
3. Suchen Sie nach den Elementen im Tresor, um mit dem Entfernen der geschützten Elemente aus dem Tresor zu beginnen. Klicken Sie auf dem Dashboard des Tresors auf **Einstellungen** und dann auf **Sicherungselemente**, um das entsprechende Menü zu öffnen.

    ![Tresor in Liste auswählen](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    Das Menü **Sicherungselemente** enthält separate Listen, die jeweils auf dem Elementtyp basieren: „Virtuelle Azure-Computer“ oder „Dateien/Ordner“ (siehe Abbildung). Standardmäßig wird für die Liste „Elementtyp“ die Option „Virtuelle Azure-Computer“ angezeigt. Wählen Sie im Dropdownmenü die Option **Dateien/Ordner** , um die Liste mit den Elementen vom Typ „Dateien/Ordner“ anzuzeigen, die im Tresor enthalten sind.
4. Bevor Sie ein Element aus dem Tresor zum Schutz einer VM löschen können, müssen Sie den Sicherungsauftrag des Elements beenden und die Wiederherstellungspunktdaten löschen. Gehen Sie für jedes Element im Tresor wie folgt vor:

    a. Klicken Sie im Menü **Sicherungselemente** mit der rechten Maustaste auf das Element, und wählen Sie im Kontextmenü die Option **Sicherung beenden** aus.

    ![Sicherungsauftrag beenden](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    Das Menü „Sicherung beenden“ wird geöffnet.

    b. Wählen Sie im Menü **Sicherung beenden** unter **Option wählen** die Option **Sicherungsdaten löschen** aus, geben Sie den Namen des Elements ein, und klicken Sie auf **Sicherung beenden**.

    Geben Sie den Namen des Elements ein, um zu bestätigen, dass Sie es löschen möchten. Die Schaltfläche **Sicherung beenden** wird aktiviert, sobald Sie das Element bestätigt haben. Wenn das Dialogfeld zum Eingeben des Namens eines Sicherungselements nicht angezeigt wird, haben Sie die Option **Sicherungsdaten beibehalten** gewählt.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Optional können Sie einen Grund angeben, warum Sie die Daten löschen, und Sie können Kommentare hinzufügen. Warten Sie nach dem Klicken auf **Sicherung beenden**ab, bis der Löschvorgang abgeschlossen ist, und versuchen Sie dann, den Tresor zu löschen. Um zu überprüfen, ob der Vorgang abgeschlossen wurde, können Sie die Azure-Meldungen anzeigen: ![delete backup data](./media/backup-azure-delete-vault/messages.png). <br/>
    Nach Abschluss des Vorgangs sendet der Dienst eine Nachricht mit dem Hinweis, dass der Sicherungsvorgang beendet wurde und die Sicherungsdaten gelöscht wurden.

    c. Klicken Sie nach dem Löschen eines Elements aus der Liste auf das Menü **Sicherungselemente** und dann auf **Aktualisieren**, um die restlichen Elemente des Tresors anzuzeigen.

      ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/empty-items-list.png)

      Wenn die Liste keine Elemente enthält, scrollen Sie im Tresormenü von Recovery Services zum Bereich **Zusammenfassung**. Hier sollten keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** angezeigt werden. Falls im Tresor weiterhin Elemente angezeigt werden, kehren Sie zu Schritt 3 oben zurück, um eine andere Elementtypliste auszuwählen.  
5. Klicken Sie auf **Löschen**, wenn in der Symbolleiste des Tresors keine Elemente mehr enthalten sind.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-vault.png)
6. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .

## <a name="what-if-i-stopped-the-backup-process-but-retained-the-data"></a>Was passiert, wenn ich den Sicherungsprozess beendet, die Daten aber beibehalten habe?
Wenn Sie den Sicherungsprozess beendet und die Daten versehentlich *beibehalten* haben, müssen Sie die Sicherungsdaten löschen, bevor Sie den Tresor löschen können. So löschen Sie die gesicherten Daten

1. Klicken Sie im Menü **Sicherungselemente** mit der rechten Maustaste auf das Element, und klicken Sie dann im Kontextmenü auf **Sicherungsdaten löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    Das Menü **Sicherungsdaten löschen** wird geöffnet.
2. Geben Sie im Menü **Sicherungsdaten löschen** den Namen des Elements ein, und klicken Sie auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Springen Sie nach dem Löschen der Daten zurück zu Schritt 4c, und setzen Sie den Vorgang fort.

## <a name="delete-a-vault-used-to-protect-a-dpm-server"></a>Löschen eines Tresors zum Schutz eines DPM-Servers
Bevor Sie einen Tresor löschen können, der als Schutz für einen DPM-Server verwendet wird, müssen Sie alle erstellten Wiederherstellungspunkte löschen und anschließend die Registrierung des Servers für den Tresor aufheben.

So löschen Sie die Daten, die einer Schutzgruppe zugeordnet sind

1. Klicken Sie in der DPM-Verwaltungskonsole auf **Schutz**, wählen Sie eine Schutzgruppe und dann ein Schutzgruppenmitglied aus, und klicken Sie im Menüband des Tools auf **Entfernen**.

  Wählen Sie das Schutzgruppenmitglied aus, um die Schaltfläche **Entfernen** im Menüband des Tools zu aktivieren. Im Beispiel lautet der Name des Mitglieds **dummyvm9**. Halten Sie zum Auswählen mehrerer Mitglieder der Schutzgruppe beim Klicken auf die Mitglieder die STRG-TASTE gedrückt.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    Das Dialogfeld **Schutz beenden** wird geöffnet.
2. Wählen Sie im Dialogfeld **Schutz beenden** die Option **Geschützte Daten löschen** aus, und klicken Sie auf **Schutz beenden**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    Um einen Tresor zu löschen, müssen Sie den Tresor der geschützten Daten deaktivieren oder löschen. Wenn viele Wiederherstellungspunkte (und Daten in der Schutzgruppe) vorhanden sind, dauert es einige Minuten, bis die Daten gelöscht wurden. Nach Abschluss des Auftrags erscheint das Dialogfeld **Schutz beenden**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. Setzen Sie diesen Vorgang für alle Mitglieder in allen Schutzgruppen fort.

    Entfernen Sie alle geschützten Daten und Schutzgruppen.
4. Wechseln Sie nach dem Löschen aller Mitglieder aus der Schutzgruppe zum Azure-Portal. Öffnen Sie das Dashboard des Tresors, und stellen Sie sicher, dass keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** vorhanden sind. Klicken Sie auf der Symbolleiste des Tresors auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-vault.png)

    Wenn für den Tresor Sicherungsverwaltungsserver registriert wurden, können Sie den Tresor auch dann nicht löschen, wenn er keine Daten enthält. Wenn Sie die Sicherungsverwaltungsserver des Tresors gelöscht haben und im Bereich **Zusammenfassung** trotzdem noch Server angezeigt werden, helfen Ihnen die Informationen unter [Suchen nach Sicherungsverwaltungsservern, die für den Tresor registriert sind](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault) weiter.
5. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .

## <a name="delete-a-vault-used-to-protect-a-production-server"></a>Löschen eines Tresors zum Schutz eines Produktionsservers
Bevor Sie einen Tresor löschen können, der als Schutz für einen Produktionsserver verwendet wird, müssen Sie den Server löschen oder seine Registrierung für den Tresor aufheben.

So löschen Sie den Produktionsserver, der dem Tresor zugeordnet ist

1. Öffnen Sie im Azure-Portal das Dashboard des Tresors, und klicken Sie auf **Einstellungen** > **Sicherungsinfrastruktur** > **Produktionsserver**.

    ![Menü „Produktionsserver“ öffnen](./media/backup-azure-delete-vault/delete-production-server.png)

    Das Menü **Produktionsserver** wird geöffnet und enthält eine Liste mit allen im Tresor enthaltenen Produktionsservern.

    ![Liste mit Produktionsservern](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. Klicken Sie im Menü **Produktionsserver** mit der rechten Maustaste auf den Server, und klicken Sie dann auf **Löschen**.

    ![Produktionsserver löschen ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    Das Menü **Löschen** wird geöffnet.

    ![Produktionsserver löschen ](./media/backup-azure-delete-vault/delete-blade.png)
3. Bestätigen Sie im Menü **Löschen** den Namen des Servers, und klicken Sie auf **Löschen**. Sie müssen den Namen des Servers richtig eingeben, um die Schaltfläche **Löschen** zu aktivieren.

    Nachdem der Tresor gelöscht wurde, erhalten Sie eine Meldung mit dem Hinweis, dass der Tresor gelöscht wurde. Scrollen Sie nach dem Löschen aller Server des Tresors zurück zum Bereich „Zusammenfassung“ des Tresor-Dashboards.
4. Stellen Sie im Dashboard des Tresors sicher, dass keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** vorhanden sind. Klicken Sie auf der Symbolleiste des Tresors auf **Löschen**.
5. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen.

    Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .

## <a name="find-the-backup-management-servers-registered-to-the-vault"></a>Suchen nach Sicherungsverwaltungsservern, die für den Tresor registriert sind
Wenn Sie für einen Tresor mehrere Server registriert haben, kann es schwierig sein, alle im Kopf zu behalten. So zeigen Sie die für den Tresor registrierten Server an und löschen sie

1. Öffnen Sie das Dashboard des Tresors.
2. Klicken Sie im Bereich **Zusammenfassung** auf **Einstellungen**, um das entsprechende Menü zu öffnen.

    ![Menü „Einstellungen“ öffnen](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. Klicken Sie im Menü **Einstellungen** auf **Sicherungsinfrastruktur**.
4. Klicken Sie im Menü **Sicherungsinfrastruktur** auf **Sicherungsverwaltungsserver**. Das Menü „Sicherungsverwaltungsserver“ wird geöffnet.

    ![Liste mit Sicherungsverwaltungsservern](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. Klicken Sie mit der rechten Maustaste auf den Namen des Servers, und klicken Sie dann auf **Löschen**, um einen Server aus der Liste zu löschen.
    Das Menü **Löschen** wird geöffnet.
6. Geben Sie im Menü **Löschen** den Namen des Servers an. Wenn es sich um einen langen Namen handelt, können Sie ihn auch in der Liste mit den Sicherungsverwaltungsservern kopieren und einfügen. Klicken Sie dann auf **Löschen**.  
