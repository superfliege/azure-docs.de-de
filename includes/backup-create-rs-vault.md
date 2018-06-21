---
title: Includedatei
description: Includedatei
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 5/14/2018
ms.author: markgal
ms.custom: include file
ms.openlocfilehash: 5590da80a1c217e7902e8e010688e40f5624898c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34664926"
---
## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die im Laufe der Zeit erstellt wurden. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinien, die den geschützten virtuellen Computern zugeordnet sind.

So erstellen Sie einen Recovery Services-Tresor

1. Melden Sie sich bei Ihrem Abonnement im [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie im linken Menü **Alle Dienste** aus.

    ![Auswählen der Option „Alle Dienste“ im Hauptmenü](./media/backup-create-rs-vault/click-all-services.png) <br/>

3. Geben Sie im Dialogfeld „Alle Dienste“ *Recovery Services* ein. Sobald Sie mit der Eingabe beginnen, wird die Ressourcenliste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Recovery Services-Tresore** aus, wenn der Eintrag angezeigt wird.

    ![Eingeben von Recovery Services im Dialogfeld „Alle Dienste“](./media/backup-create-rs-vault/all-services.png) <br/>

    Die Liste mit den Recovery Services-Tresoren im Abonnement wird angezeigt.
4. Wählen Sie im Menü **Recovery Services-Tresore** die Option **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-create-rs-vault/add-button-create-vault.png)

    Das Menü **Recovery Services-Tresore** wird geöffnet. Sie werden aufgefordert, Informationen für **Name**, **Abonnement**, **Ressourcengruppe** und **Speicherort** anzugeben.

    ![Bereich „Recovery Services-Tresore“](./media/backup-create-rs-vault/create-new-vault-dialog.png)
5. Geben Sie unter **Name**einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der mindestens zwei, aber nicht mehr als 50 Zeichen enthält. Der Name muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
6. Wählen Sie unter **Abonnement** das Abonnement aus, das Sie verwenden möchten. Wenn Sie nur in einem Abonnement Mitglied sind, wird dessen Name angezeigt. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Geschäfts-, Schul- oder Unikonto mehreren Azure-Abonnements zugeordnet ist.
7. Sie können unter **Ressourcengruppe** eine vorhandene Ressourcengruppe verwenden oder eine neue erstellen. Um eine Liste der verfügbaren Ressourcengruppen in Ihrem Abonnement anzuzeigen, wählen Sie **Vorhandene verwenden** aus, und klicken Sie auf das Dropdownmenü. Wählen Sie zum Erstellen einer neuen Ressourcengruppe **Neu erstellen** aus, und geben Sie den Namen ein. Umfassende Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).
8. Wählen Sie unter **Standort** die geografische Region für den Tresor aus. Wenn Sie einen Tresor zum Schutz virtueller Computer erstellen, *muss* sich dieser in derselben Region wie die virtuellen Computer befinden.

   > [!IMPORTANT]
   > Wenn Sie sich nicht sicher sind, an welchem Standort sich Ihr virtueller Computer befindet, schließen Sie das Dialogfeld zur Tresorerstellung, und wechseln Sie zur Liste der virtuellen Computer im Portal. Falls Sie über virtuelle Computer in mehreren Regionen verfügen, erstellen Sie in jeder Region einen Recovery Services-Tresor. Erstellen Sie den Tresor am ersten Standort, bevor Sie mit dem nächsten Standort fortfahren. Das Angeben von Speicherkonten zum Speichern der Sicherungsdaten ist nicht erforderlich. Der Recovery Services-Tresor und der Azure Backup-Dienst nehmen dies automatisch vor.
   >
   >

9. Wenn Sie den Recovery Services-Tresor erstellen möchten, klicken Sie auf **Erstellen**.

    ![Liste der Sicherungstresore](./media/backup-create-rs-vault/click-create-button.png)

    Es kann einige Zeit dauern, bis der Recovery Services-Tresor erstellt wurde. Verfolgen Sie die Statusbenachrichtigungen im Benachrichtigungsbereich (rechts oben im Portal). Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor weiterhin nicht angezeigt werden, klicken Sie auf **Aktualisieren**.

     ![Liste der Sicherungstresore](./media/backup-create-rs-vault/refresh-button.png)
