---
title: Integration von Azure Automation mit Event Grid | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie beim Erstellen eines neuen virtuellen Computers automatisch ein Tag hinzufügen und eine Benachrichtigung an Microsoft Teams senden.
keywords: Automatisierung, Runbook, Teams, Event Grid, virtueller Computer, VM
services: automation
documentationcenter: ''
author: eamonoreilly
manager: ''
editor: ''
ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2017
ms.author: eamono
ms.openlocfilehash: 9a4d6ecf19fc96a9c7b92cf246effbf3948fb478
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2017
ms.locfileid: "26349068"
---
# <a name="integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Integration von Azure Automation mit Event Grid und Microsoft Teams

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Importieren Sie ein Event Grid-Beispielrunbook.
> * Erstellen Sie einen optionalen Microsoft Teams-Webhook.
> * Erstellen Sie einen Webhook für das Runbook.
> * Erstellen Sie ein Event Grid-Abonnement.
> * Erstellen Sie einen virtuellen Computer, der das Runbook auslöst.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, ist ein [Azure Automation-Konto](../automation/automation-offering-get-started.md) erforderlich, um das vom Azure Event Grid-Abonnement ausgelöste Runbook aufzunehmen.

## <a name="import-an-event-grid-sample-runbook"></a>Importieren eines Event Grid-Beispielrunbooks
1. Wählen Sie Ihr Automation-Konto und dann die Seite **Runbooks** aus.

   ![Auswählen von Runbooks](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. Wählen Sie die Schaltfläche **Katalog durchsuchen** aus.

3. Suchen Sie nach **Event Grid** und wählen Sie **Integration von Azure Automation mit Event Grid** aus. 

    ![Importieren des Katalogrunbooks](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Wählen Sie **Importieren** aus und nennen Sie es **Watch-VMWrite**.

5. Nach dem Import wählen Sie **Bearbeiten** aus, um die Runbookquelle anzuzeigen. Wählen Sie die Schaltfläche **Veröffentlichen** aus.

## <a name="create-an-optional-microsoft-teams-webhook"></a>Erstellen eines optionalen Microsoft Teams-Webhooks
1. Wählen Sie in Microsoft-Teams die Option **Weitere Optionen** neben dem Kanalnamen und dann **Connectors** aus.

    ![Microsoft Teams-Verbindungen](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Scrollen Sie durch die Liste der Connectors zu **Eingehender Webhook** und wählen Sie **Hinzufügen** aus.

3. Geben Sie **AzureAutomationIntegration** für den Namen ein und wählen Sie **Erstellen** aus.

4. Kopieren Sie den Webhook in die Zwischenablage und speichern Sie ihn. Die Webhook-URL wird zum Senden von Informationen an Microsoft Teams verwendet.

5. Wählen Sie **Fertig** aus, um den Webhook zu speichern.

## <a name="create-a-webhook-for-the-runbook"></a>Erstellen eines Webhooks für das Runbook
1. Öffnen Sie das Runbook „Watch-VMWrite“.

2. Wählen Sie **Webhooks** und dann die Schaltfläche **Webhook hinzufügen** aus.

3. Geben Sie **WatchVMEventGrid** für den Namen ein. Kopieren Sie die URL in die Zwischenablage und speichern Sie sie.

    ![Konfigurieren des Webhooknamens](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. Wählen Sie **Parameter konfigurieren und Einstellungen ausführen** aus und geben Sie Microsoft Teams-Webhook-URL für **CHANNELURL** ein. Lassen Sie **WEBHOOKDATA** leer.

    ![Konfigurieren der Webhookparameter](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Wählen Sie **OK** aus, um den Webhook des Automation-Runbooks zu erstellen.


## <a name="create-an-event-grid-subscription"></a>Erstellen eines Event Grid-Abonnements
1. Wählen Sie auf der Übersichtsseite **Automation-Konto** die Option **Event Grid** aus.

    ![Auswählen von Event Grid](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. Wählen Sie die Schaltfläche **+ Ereignisabonnement** aus.

3. Konfigurieren Sie das Abonnement mit den folgenden Informationen:

    *   Geben Sie **AzureAutomation** für den Namen ein.
    *   Wählen Sie in **Thementyp** die Option **Azure-Abonnements** aus.
    *   Deaktivieren Sie das Kontrollkästchen **Alle Ereignistypen abonnieren**.
    *   Wählen Sie in **Ereignistypen** die Option für **Ressourcenschreiberfolg** aus.
    *   Geben Sie in **Abonnentenendpunkt** die Webhook-URL für das Runbook „Watch-VMWrite“ ein.
    *   Geben Sie in **Präfixfilter** die Abonnement- und Ressourcengruppe ein, in der Sie nach den neu erstellten virtuellen Computern suchen möchten. Diese sollte wie folgt aussehen: `/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`.

4. Wählen Sie **Erstellen** aus, um das Event Grid-Abonnement zu speichern.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Erstellen eines virtuellen Computers, der das Runbook auslöst
1. Legen Sie einen neuen virtuellen Computer in der Ressourcengruppe an, die Sie im Event Grid-Abonnementpräfixfilter angegeben haben.

2. Das Runbook „Watch-VMWrite“ sollte aufgerufen und dem virtuellen Computer ein neues Tag hinzugefügt werden.

    ![VM-Tag](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Eine neue Nachricht wird an den Microsoft Teams-Kanal gesendet.

    ![Microsoft Teams-Benachrichtigung](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial richten Sie die Integration zwischen Event Grid und Automation ein. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Importieren Sie ein Event Grid-Beispielrunbook.
> * Erstellen Sie einen optionalen Microsoft Teams-Webhook.
> * Erstellen Sie einen Webhook für das Runbook.
> * Erstellen Sie ein Event Grid-Abonnement.
> * Erstellen Sie einen virtuellen Computer, der das Runbook auslöst.

> [!div class="nextstepaction"]
> [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Event Grid](../event-grid/custom-event-quickstart.md)
