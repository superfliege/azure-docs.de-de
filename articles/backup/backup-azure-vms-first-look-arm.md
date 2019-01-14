---
title: Vorbereiten der Sicherung virtueller Azure-Computer mit dem Azure Backup-Dienst
description: Erfahren Sie, wie Sie Azure-VMs mit dem Azure Backup-Dienst sichern.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: raynew
ms.openlocfilehash: 0c394a92bff3ace210ee0db156f47bb8912bf45d
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53631582"
---
# <a name="back-up-azure-vms-with-the-azure-backup-service"></a>Vorbereiten der Sicherung virtueller Azure-Computer mit dem Azure Backup-Dienst

In diesem Artikel wird erläutert, wie Sie den Schutz für einen virtuellen Computer über das Menü für VM-Vorgänge oder den Recovery Services-Tresor konfigurieren. Recovery Services-Tresore schützen Folgendes:

* Mit Azure Resource Manager bereitgestellte virtuelle Computer
* Klassische virtuelle Computer
* Virtuelle Standardspeichercomputer
* Virtuelle Storage Premium-Computer
* Auf verwalteten Datenträgern ausgeführte virtuelle Computer
* Per Azure Disk Encryption verschlüsselte virtuelle Computer
* Anwendungskonsistente Sicherung von Windows-VMs mit VSS und Linux-VMs mit benutzerdefinierten Skripts (vor und nach der Momentaufnahme)

Weitere Informationen zum Schutz virtueller Storage Premium-Computer finden Sie im Artikel [Sichern und Wiederherstellen virtueller Storage Premium-Computer](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup). Weitere Informationen zur Unterstützung für virtuelle Computer auf verwalteten Datenträgern finden Sie unter [Sichern und Wiederherstellen von virtuellen Computern auf verwalteten Datenträgern](backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). Weitere Informationen zum Pre- und Post-Skript-Framework für die Linux-VM-Sicherung finden Sie unter [Anwendungskonsistente Sicherung von virtuellen Linux-Computern in Azure](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Informationen darüber, was Sie sichern und was Sie nicht sichern können, finden Sie unter [Vorbereiten der Umgebung für die Sicherung von mit Resource Manager bereitgestellten virtuellen Computern](backup-azure-arm-vms-prepare.md#before-you-start).

> [!NOTE]
> Der Backup-Dienst erstellt eine separate Ressourcengruppe neben der Ressourcengruppe des virtuellen Computers zum Speichern der Wiederherstellungspunktsammlung. Kunden sollten die für die Verwendung durch den Backup-Dienst erstellte Ressourcengruppe nicht sperren.
Das Namensformat der vom Backup-Dienst erstellten Ressourcengruppe sieht folgendermaßen aus: AzureBackupRG_`<Geo>`_`<number>`
<br>Beispiel: AzureBackupRG_northeurope_1
>
>

Je nach Anzahl von virtuellen Computern, die Sie schützen möchten, können Sie von unterschiedlichen Ausgangspunkten starten. Wenn Sie mit einem Vorgang mehrere virtuelle Computer sichern möchten, können Sie zum Recovery Services-Tresor navigieren und [den Sicherungsauftrag über das Tresordashboard initiieren](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-recovery-services-vault). Beim Sichern eines einzelnen virtuellen Computers [initiieren Sie den Sicherungsauftrag über das Menü für VM-Vorgänge](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-operations-menu).

## <a name="configure-the-backup-job-from-the-vm-operations-menu"></a>Konfigurieren des Sicherungsauftrags über das Menü für VM-Vorgänge

Führen Sie die folgenden Schritte aus, um den Sicherungsauftrag über das Menü für VM-Vorgänge zu konfigurieren. Die Schritte gelten nur für virtuelle Computer im Azure-Portal.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im Menü „Hub“ auf **Alle Dienste**, und geben Sie im Dialogfeld „Filter“ den Text **Virtuelle Computer** ein. Während der Eingabe wird die Liste mit den Ressourcen gefiltert. Wenn Sie den Eintrag „Virtuelle Computer“ sehen, können Sie ihn auswählen.

  ![Screenshot, der das Navigieren zu „Virtuelle Computer“ über „Alle Dienste“ zeigt](./media/backup-azure-vms-first-look-arm/open-vm-from-hub.png)

  Die Liste mit den virtuellen Computern (VMs) des Abonnements wird angezeigt.

  ![Die Liste mit den VMs des Abonnements wird angezeigt.](./media/backup-azure-vms-first-look-arm/list-of-vms.png)

3. Wählen Sie in der Liste eine zu sichernde VM aus.

  ![Die Liste mit den VMs des Abonnements wird angezeigt.](./media/backup-azure-vms-first-look-arm/list-of-vms-selected.png)

  Wenn Sie die VM auswählen, wird die Liste mit den virtuellen Computern nach links verschoben, und das Menü „VM-Verwaltung“ und das VM-Dashboard werden geöffnet.

4. Klicken Sie im Menü „VM-Verwaltung“ im Abschnitt **Vorgänge** auf **Sicherung**. </br>

  ![Option „Sicherung“ im Menü „VM-Verwaltung“](./media/backup-azure-vms-first-look-arm/vm-management-menu.png)

  Das Menü „Sicherung aktivieren“ wird geöffnet.

  ![Option „Sicherung“ im Menü „VM-Verwaltung“](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup.png)

5. Klicken Sie im Bereich für den Recovery Services-Tresor auf **Vorhandene auswählen**, und wählen Sie in der Dropdownliste einen Tresor aus.

  ![Aktivieren des Sicherungs-Assistenten](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

  Wenn keine Recovery Services-Tresore vorhanden sind oder wenn Sie einen neuen Tresor verwenden möchten, können Sie auf **Neu erstellen** klicken und den Namen für den neuen Tresor angeben. Ein neuer Tresor wird in derselben Ressourcengruppe und derselben Region wie der virtuelle Computer erstellt. Wenn Sie einen Recovery Services-Tresor mit anderen Werten erstellen möchten, helfen Ihnen die Informationen im Abschnitt [Erstellen eines Recovery Services-Tresors](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm) weiter.

6. Wählen Sie im Menü „Sicherungsrichtlinie auswählen“ eine Richtlinie aus. Die Details zur ausgewählten Richtlinie werden unterhalb des Dropdownmenüs angezeigt.

  Wenn Sie eine neue Richtlinie erstellen oder die bestehende Richtlinie bearbeiten möchten, klicken Sie auf **Neue Richtlinie erstellen (oder bearbeiten)**, um den Editor für Sicherungsrichtlinien zu öffnen. Eine Anleitung zum Definieren einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). Klicken Sie auf **OK**, um die Änderungen an der Sicherungsrichtlinie zu speichern und zum Menü „Sicherung aktivieren“ zurückzukehren.

  ![Sicherungsrichtlinie auswählen](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. Klicken Sie auf **Sicherung aktivieren**, um die Richtlinie bereitzustellen und den Recovery Services-Tresor sowie die Sicherungsrichtlinie auf den virtuellen Computer anzuwenden. Durch Bereitstellen der Richtlinie wird sie dem Tresor und den virtuellen Computern zugeordnet.

  ![Schaltfläche „Sicherung aktivieren“](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. Sie können den Konfigurationsprozess über die Benachrichtigungen nachverfolgen, die im Portal angezeigt werden. Im folgenden Beispiel wird veranschaulicht, dass die Bereitstellung gestartet wurde.

  ![Benachrichtigung zu „Sicherung aktivieren“](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Klicken Sie nach Abschluss des Konfigurationsprozesses im Menü „VM-Verwaltung“ auf **Sicherung**, um das Menü „Sicherungselement“ zu öffnen und die verfügbaren Details anzuzeigen.

  ![VM-Ansicht „Sicherungselement“](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

  Bis zum Abschluss des ersten Sicherungsvorgangs wird **Status der letzten Sicherung** als **Warnung (anfängliche Sicherung steht aus)** angezeigt. Klicken Sie unter **Zusammenfassung** auf den Namen der Richtlinie, um anzuzeigen, wann der nächste geplante Sicherungsauftrag ansteht. Das Menü „Sicherungsrichtlinie“ wird geöffnet und zeigt den Zeitpunkt der geplanten Sicherung an.

10. Um den virtuellen Computer zu schützen, klicken Sie auf **Jetzt sichern**.

  ![Klicken Sie jetzt auf „Sichern“, um die erste Sicherung durchzuführen.](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

  Das Menü „Jetzt sichern“ wird geöffnet.

  ![Blatt „Jetzt sichern“](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

11. Klicken Sie im Menü „Jetzt sichern“ auf das Kalendersymbol, verwenden Sie das Kalendersteuerelement zum Auswählen des letzten Aufbewahrungstags des Wiederherstellungspunkts, und klicken Sie auf **OK**.

  ![Festlegen des letzten Aufbewahrungstags für den Wiederherstellungspunkt für „Jetzt sichern“](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Anhand von Bereitstellungsbenachrichtigungen werden Sie darüber informiert, dass der Sicherungsauftrag ausgelöst wurde und dass Sie den Status des Auftrags auf der Seite „Sicherungsaufträge“ überwachen können.

## <a name="configure-the-backup-job-from-the-recovery-services-vault"></a>Konfigurieren des Sicherungsauftrags über den Recovery Services-Tresor
Führen Sie die folgenden Schritte aus, um den Sicherungsauftrag zu konfigurieren:

1. Erstellen Sie einen Recovery Services-Tresor für einen virtuellen Computer.
2. Verwenden Sie das Azure-Portal, um ein Szenario zu wählen, eine Sicherungsrichtlinie festzulegen und zu schützende Elemente anzugeben.
3. Führen Sie die erste Sicherung durch.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Erstellen eines Recovery Services-Tresor für eine VM
Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die im Laufe der Zeit erstellt wurden. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinie, die auf die geschützten VMs angewendet wird.

> [!NOTE]
> Das Sichern von VMs ist ein lokaler Vorgang. Es ist nicht möglich, virtuelle Computer einer Region in einem Recovery Services-Tresor in einer anderen Region zu sichern. In jeder Azure-Region, die über zu sichernde virtuelle Computer verfügt, muss also mindestens ein Recovery Services-Tresor vorhanden sein.
>
>

So erstellen Sie einen Recovery Services-Tresor

1. Melden Sie sich mit Ihrem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com/) an, sofern Sie noch nicht angemeldet sind.
2. Klicken Sie im Menü „Hub“ auf **Alle Dienste**, und geben Sie im Dialogfeld „Filter“ den Text **Recovery Services** ein. Während der Eingabe wird die Liste mit den Ressourcen gefiltert. Klicken Sie in der Liste auf „Recovery Services-Tresore“, wenn der Eintrag angezeigt wird.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Wenn Recovery Services-Tresore im Abonnement vorhanden sind, werden die Tresore aufgeführt.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-vms-first-look-arm/list-of-rs-vault.png)
3. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Das Menü „Recovery Services-Tresor“ wird geöffnet, und Sie werden aufgefordert, **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** anzugeben.

    ![Erstellen eines Recovery Services-Tresors – Schritt 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. Geben Sie unter **Name**einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.

5. Wählen Sie im Abschnitt **Abonnement** über das Dropdownmenü das Azure-Abonnement aus. Wenn Sie nur ein Abonnement verwenden, wird dieses Abonnement angezeigt, und Sie können mit dem nächsten Schritt fortfahren. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Organisationskonto mehreren Azure-Abonnements zugeordnet ist.

6. Gehen Sie im Abschnitt **Ressourcengruppe** wie folgt vor:

    * Wählen Sie **Neu erstellen** aus, wenn Sie eine Ressourcengruppe erstellen möchten.
    oder
    * Wählen Sie **Use existing** (Vorhandene verwenden) aus, und klicken Sie auf das Dropdownmenü, um eine Liste mit verfügbaren Ressourcengruppen anzuzeigen.

  Umfassende Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

7. Klicken Sie auf **Standort** , um die geografische Region für den Tresor auszuwählen. Die Auswahl bestimmt die geografische Region, an die Ihre Sicherungsdaten gesendet werden.

  > [!IMPORTANT]
  > Wenn Sie sich nicht sicher sind, in welcher Region sich Ihr virtueller Computer befindet, schließen Sie das Dialogfeld zur Tresorerstellung, und wechseln Sie zur Liste der virtuellen Computer im Portal. Falls Sie über virtuelle Computer in mehreren Regionen verfügen, erstellen Sie in jeder Region einen Recovery Services-Tresor. Erstellen Sie den Tresor in der ersten Region, bevor Sie mit der nächsten Region fortfahren. Es müssen keine Speicherkonten zum Speichern der Sicherungsdaten angegeben werden. Die Speicherung wird vom Recovery Services-Tresor und vom Azure Backup-Dienst automatisch abgewickelt.
  >

8. Klicken Sie im unteren Bereich des Menüs für den Recovery Services-Tresor auf **Erstellen**.

    Die Erstellung des Recovery Services-Tresors kann mehrere Minuten dauern. Verfolgen Sie die Statusbenachrichtigungen rechts oben im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor auch nach mehreren Minuten nicht angezeigt werden, klicken Sie auf **Aktualisieren**.

    ![Klicken auf die Schaltfläche „Aktualisieren“](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Sobald Ihr Tresor in der Liste mit den Recovery Services-Tresoren angezeigt wird, können Sie die Speicherredundanz festlegen.

Nachdem Sie Ihren Tresor erstellt haben, widmen wir uns nun dem Festlegen der Speicherreplikation.

### <a name="set-storage-replication"></a>Festlegen der Speicherreplikation
Bei der Speicherreplikation haben Sie die Wahl zwischen georedundantem Speicher und lokal redundantem Speicher. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Falls es sich bei dem Recovery Services-Tresor um Ihre primäre Sicherung handelt, behalten Sie für die Speicherreplikationsoption den georedundanten Speicher bei. Wählen Sie lokal redundanten Speicher, wenn Sie eine günstigere und weniger langfristige Option wünschen. Weitere Informationen zu den Optionen für [georedundanten](../storage/common/storage-redundancy-grs.md) und [lokal redundanten](../storage/common/storage-redundancy-lrs.md) Speicher finden Sie in der [Übersicht über die Azure Storage-Replikation](../storage/common/storage-redundancy.md).

So bearbeiten Sie die Einstellung für die Speicherreplikation:

1. Wählen Sie im Menü **Recovery Services-Tresore** den neuen Tresor aus.

  ![Auswählen des neuen Tresors in der Liste mit Recovery Services-Tresoren](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

  Wenn Sie den Tresor auswählen, werden das Menü mit den Einstellungen (*auf dem oben der Name des Tresors angegeben ist*) sowie das Tresordashboard geöffnet.

  ![Anzeigen der Speicherkonfiguration für den neuen Tresor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-update.png)

2. Scrollen Sie im Menü „Verwaltung“ des neuen Tresors mithilfe des vertikalen Schiebereglers nach unten zum Verwaltungsabschnitt, und klicken Sie auf **Sicherungsinfrastruktur**, um das Menü „Sicherungsinfrastruktur“ zu öffnen.

   ![Speicherkonfiguration für neuen Tresor festlegen](./media/backup-try-azure-backup-in-10-mins/set-storage-config-bkup-infra.png)

3. Klicken Sie im Menü „Sicherungsinfrastruktur“ auf **Sicherungskonfiguration**, um das Menü **Sicherungskonfiguration** zu öffnen.

    ![Speicherkonfiguration für neuen Tresor festlegen](./media/backup-try-azure-backup-in-10-mins/set-storage-open-infra.png)
4. Wählen Sie die passende Speicherreplikationsoption für Ihren Tresor aus.

    ![Speicherkonfigurationsoptionen](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden, verwenden Sie weiterhin die Option **Georedundant**. Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie **Lokal redundant** aus. Dadurch verringern sich die Kosten für Azure-Speicher. Weitere Informationen zu den Optionen für [georedundanten](../storage/common/storage-redundancy-grs.md) und [lokal redundanten](../storage/common/storage-redundancy-lrs.md) Speicher finden Sie in [dieser Übersicht über Speicherredundanz](../storage/common/storage-redundancy.md).


## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Auswählen eines Sicherungsziels, Festlegen der Richtlinie und Definieren von zu schützenden Elementen
Führen Sie zunächst den Ermittlungsprozess durch, bevor Sie einen virtuellen Computer registrieren, um sicherzustellen, dass alle neuen virtuellen Computer, die dem Abonnement hinzugefügt wurden, identifiziert werden. Bei diesem Vorgang wird Azure nach der Liste virtueller Computer im Abonnement abgefragt. Außerdem werden zusätzliche Informationen wie der Clouddienstname und die Region erfasst. „Szenario“ bezeichnet im Kontext des Azure-Portals das, was Sie dem Recovery Services-Tresor hinzufügen möchten. Die Richtlinie gibt die Häufigkeit und den Zeitpunkt für die Erstellung von Wiederherstellungspunkten an. Darüber hinaus enthält die Richtlinie die Beibehaltungsdauer für die Wiederherstellungspunkte.

1. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit Schritt 2 fortfahren. Klicken Sie andernfalls auf **Alle Dienste**. Geben Sie **Recovery Services** ein, und klicken Sie auf **Recovery Services-Tresore**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

    ![Ansicht der Liste mit den Recovery Services-Tresoren](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus, um das dazugehörige Dashboard zu öffnen.

     ![Tresormenü öffnen](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Klicken Sie im Menü des Tresordashboards auf **Sicherung**, um das Menü „Sicherung“ zu öffnen.

    ![Menü „Sicherung“ öffnen](./media/backup-azure-arm-vms-prepare/backup-button.png)

    Die Menüs „Sicherung“ und „Sicherungsziel“ werden geöffnet.

    ![Menü „Szenario“ öffnen](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)
3. Wählen Sie im Menü „Sicherungsziel“ im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Option „Azure“ aus. Wählen Sie im Dropdownmenü **Was möchten Sie sichern?** die Option „Virtueller Computer“, und klicken Sie dann auf **OK**.

    Dadurch wird die VM-Erweiterung im Tresor registriert. Das Menü „Sicherungsziel“ wird geschlossen, und das Menü **Sicherungsrichtlinie** wird geöffnet.

    ![Menü „Szenario“ öffnen](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. Wählen Sie im Menü „Sicherungsrichtlinie“ die Sicherungsrichtlinie aus, die Sie auf den Tresor anwenden möchten.

    ![Sicherungsrichtlinie auswählen](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    Die Details zur Standardrichtlinie werden unter dem Dropdownmenü aufgeführt. Wählen Sie im Dropdownmenü die Option **Neu erstellen** , wenn Sie eine Richtlinie erstellen möchten. Eine Anleitung zum Definieren einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Klicken Sie auf **OK**, um die Sicherungsrichtlinie mit dem Tresor zu verknüpfen.

    Das Menü „Sicherungsrichtlinie“ wird geschlossen, und das Menü **Virtuelle Computer auswählen** wird geöffnet.
5. Wählen Sie im Menü **Virtuelle Computer auswählen** die virtuellen Computer aus, die der angegebenen Richtlinie zugeordnet werden sollen, und klicken Sie auf **OK**.

    ![Workload auswählen](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    Der ausgewählte virtuelle Computer wird überprüft. Falls die erwarteten virtuellen Computer nicht angezeigt werden, sollten Sie überprüfen, ob sie sich an demselben Azure-Standort wie der Recovery Services-Tresor befinden und nicht bereits geschützt sind. Der Standort des Recovery Services-Tresors wird auf dem Tresordashboard angezeigt.

6. Nachdem Sie nun alle Einstellungen für den Tresor definiert haben, klicken Sie im Menü „Sicherung“ auf **Sicherung aktivieren**, um die Richtlinie für den Tresor und für die virtuellen Computer bereitzustellen. Das Bereitstellen der Sicherungsrichtlinie führt nicht zur Erstellung des ersten Wiederherstellungspunkts für den virtuellen Computer.

    ![Sicherung aktivieren](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nachdem die Sicherung erfolgreich aktiviert wurde, wird die Sicherungsrichtlinie nach dem Zeitplan ausgeführt. Fahren Sie hier aber mit dem Initiieren des ersten Sicherungsauftrags fort.

## <a name="initial-backup"></a>Erste Sicherung
Nachdem eine Sicherungsrichtlinie auf dem virtuellen Computer bereitgestellt wurde, bedeutet dies nicht, dass die Daten gesichert wurden. Die erste geplante Sicherung (gemäß Definition in der Sicherungsrichtlinie) ist die erste Sicherung. Bis zur Durchführung der ersten Sicherung wird der Status der letzten Sicherung im Menü **Sicherungsaufträge** als **Warnung (anfängliche Sicherung steht aus)** angezeigt.

![Sicherung ausstehend](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Falls die erste Sicherung nicht ohnehin in Kürze durchgeführt wird, empfiehlt es sich, **Jetzt sichern**auszuführen.

So führen Sie den ersten Sicherungsauftrag aus:

1. Klicken Sie im Tresordashboard auf die Zahl unter **Sicherungselemente**, oder klicken Sie auf die Kachel **Sicherungselemente**. <br/>
  ![Symbol „Einstellungen“](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Das Menü **Sicherungselemente** wird geöffnet.

  ![Sicherungselemente](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. Wählen Sie das Element im Menü **Sicherungselemente** aus.

  ![Symbol „Einstellungen“](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Die Liste **Sicherungselemente** wird geöffnet. <br/>

  ![Sicherungsauftrag ausgelöst](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. Klicken Sie in der Liste **Sicherungselemente** auf die Auslassungszeichen (**...**), um das Kontextmenü zu öffnen.

  ![Kontextmenü](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Das Kontextmenü wird angezeigt.

  ![Kontextmenü](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Klicken Sie im Kontextmenü auf **Jetzt sichern**.

  ![Kontextmenü](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Das Menü „Jetzt sichern“ wird geöffnet.

  ![Menü „Jetzt sichern“](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Klicken Sie im Menü „Jetzt sichern“ auf das Kalendersymbol, verwenden Sie das Kalendersteuerelement zum Auswählen des letzten Aufbewahrungstags des Wiederherstellungspunkts, und klicken Sie auf **Sicherung**.

  ![Festlegen des letzten Aufbewahrungstags für den Wiederherstellungspunkt für „Jetzt sichern“](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Anhand von Bereitstellungsbenachrichtigungen werden Sie darüber informiert, dass der Sicherungsauftrag ausgelöst wurde und dass Sie den Status des Auftrags auf der Seite „Sicherungsaufträge“ überwachen können. Je nach Größe Ihrer VM kann das Erstellen der ersten Sicherung einige Zeit dauern.

  > [!NOTE]
  > Alle von Azure Backup gesicherten Daten werden im Ruhezustand mit der [Speicherdienstverschlüsselung (Storage Service Encryption, SSE)](../storage/common/storage-service-encryption.md) verschlüsselt.
  >
  >

6. Klicken Sie im Tresordashboard auf der Kachel **Sicherungsaufträge** auf **In Bearbeitung**, um den Status der ersten Sicherung anzuzeigen oder nachzuverfolgen.

  ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Das Menü „Sicherungsaufträge“ wird geöffnet.

  ![Kachel „Sicherungsaufträge“](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  Im Menü **Sicherungsaufträge** wird der Status aller Aufträge angezeigt. Überprüfen Sie, ob der Sicherungsvorgang für Ihre VM noch aktiv ist oder beendet wurde. Wenn ein Sicherungsauftrag abgeschlossen ist, lautet der Status *Abgeschlossen*.

  > [!NOTE]
  > Als Teil des Sicherungsvorgangs gibt der Azure Backup-Dienst einen Befehl an die Sicherungserweiterung auf jeder VM aus, damit alle Schreibvorgänge geleert werden und eine konsistente Momentaufnahme erstellt wird.
  >
  >


[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Installieren des VM-Agents auf dem virtuellen Computer
Diese Informationen werden bereitgestellt, falls es erforderlich ist. Der Azure VM-Agent muss auf dem virtuellen Azure-Computer installiert werden, damit die Sicherungserweiterung funktioniert. Wenn Ihr virtueller Computer aber über den Azure-Katalog erstellt wurde, ist der VM-Agent auf dem virtuellen Computer bereits vorhanden. Auf virtuellen Computern, die aus lokalen Rechenzentren migriert werden, ist der VM-Agent nicht installiert. In diesem Fall muss der VM-Agent installiert werden. Falls beim Sichern des virtuellen Azure-Computers Probleme auftreten, vergewissern Sie sich, dass der Azure VM-Agent korrekt auf dem virtuellen Computer installiert ist (siehe Tabelle weiter unten). Wenn Sie einen benutzerdefinierten virtuellen Computer erstellen, sollten Sie den VM-Agent vor der Bereitstellung des virtuellen Computers installieren.

Erfahren Sie mehr über den [VM-Agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) und seine [Installation](../virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (in englischer Sprache).

Die folgende Tabelle enthält weitere Informationen zum VM-Agent für virtuelle Windows- und Linux-Computer.

| **Vorgang** | **Windows** | **Linux** |
| --- | --- | --- |
| Installieren des VM-Agent |<li>Laden Sie den [Agent-MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. <li>[Aktualisieren Sie die VM-Eigenschaft](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert wurde. |<li> Installieren Sie den neuesten [Linux-Agent](https://github.com/Azure/WALinuxAgent) aus GitHub. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. <li> [Aktualisieren Sie die VM-Eigenschaft](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert wurde. |
| Aktualisieren des VM-Agents |Das Aktualisieren des VM-Agents ist so einfach wie das Neuinstallieren der [Binärdateien für den VM-Agent](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |Folgen Sie den Anweisungen unter [Aktualisieren des Linux-VM-Agents](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |
| Überprüfen der VM-Agent-Installation |<li>Navigieren Sie auf dem virtuellen Azure-Computer zum Ordner *C:\WindowsAzure\Packages*. <li>Dieser Ordner enthält die Datei "WaAppAgent.exe".<li> Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der Produktversion sollte 2.6.1198.718 oder eine höhere Version angegeben sein. |N/V |

### <a name="backup-extension"></a>Backup-Erweiterung
Sobald der VM-Agent auf dem virtuellen Computer installiert wurde, installiert der Azure Backup-Dienst die Sicherungserweiterung für den VM-Agent. Der Azure Backup-Dienst installiert nahtlos und ohne zusätzlichen Benutzereingriff Upgrades und Patches für die Backup-Erweiterung.

Der Backup-Dienst installiert die Sicherungserweiterung auch dann, wenn der virtuelle Computer nicht ausgeführt wird. Bei einem ausgeführten virtuellen Computer besteht zudem die größte Chance auf einen anwendungskonsistenten Wiederherstellungspunkt. Der Azure Backup-Dienst setzt die Sicherung des virtuellen Computers jedoch auch dann fort, wenn dieser ausgeschaltet wurde und die Erweiterung nicht installiert werden konnte. Diese Art der Sicherung wird als Offline-VM bezeichnet, und der Wiederherstellungspunkt ist *absturzkonsistent*.

## <a name="troubleshooting-information"></a>Informationen zur Problembehandlung
Falls bei der Durchführung einiger Aufgaben in diesem Artikel Probleme auftreten, helfen Ihnen die [Anleitungen zur Problembehandlung](backup-azure-vms-troubleshoot.md)weiter.

## <a name="pricing"></a>Preise
Die Kosten für die Sicherung virtueller Azure-Computer hängen von der Anzahl geschützter Instanzen ab. Eine Definition für eine geschützte Instanz finden Sie unter [Was ist eine geschützte Instanz?](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). Backup-Preisinformationen finden Sie auf der [Seite mit den Preisen für Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

## <a name="next-steps"></a>Nächste Schritte

[Verwalten](backup-azure-manage-vms.md) Sie Ihre Sicherungen.

