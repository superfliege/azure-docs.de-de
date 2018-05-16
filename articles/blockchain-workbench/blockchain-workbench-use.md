---
title: Verwenden von Anwendungen in Azure Blockchain Workbench
description: Erfahren Sie, wie Sie Anwendungsverträge in Azure Blockchain Workbench verwenden.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/26/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 35a45947c2766ccc5e64f4c3523ce163a9680680
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="using-applications-in-azure-blockchain-workbench"></a>Verwenden von Anwendungen in Azure Blockchain Workbench

Mithilfe von Blockchain Workbench können Sie Aktionen zu Verträgen erstellen und ausführen. Zudem können Sie Vertragsdetails wie Status und Transaktionsverlauf einsehen.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Blockchain Workbench-Bereitstellung. Weitere Informationen zu Details dieser Bereitstellung finden Sie unter [Azure Blockchain Workbench-Bereitstellung](blockchain-workbench-deploy.md).
* Eine bereitgestellte Blockchainanwendung in Blockchain Workbench. Siehe [Erstellen einer Blockchainanwendung in Blockchain Workbench]().

[Öffnen Sie die Blockchain Workbench](blockchain-workbench-deploy.md#blockchain-workbench-web-url) in Ihrem Browser.

![Blockchain Workbench](media/blockchain-workbench-use/workbench.png)

Sie müssen sich als Mitglied der Blockchain Workbench anmelden. Wenn keine Anwendungen aufgelistet sind, sind Sie Mitglied der Blockchain Workbench, aber nicht Mitglied einer Anwendung. Der Administrator der Blockchain Workbench kann den Anwendungen Mitglieder zuordnen.

## <a name="create-new-contract"></a>Erstellen eines neuen Vertrags 

Um einen neuen Vertrag zu erstellen, müssen Sie Mitglied der Rolle **AllowedInstanceRoles** sein. 

1. Wählen Sie im Abschnitt der Blockchain Workbench-Anwendung die Anwendungskachel aus, die den zu erstellenden Vertrag enthält. Sie erhalten eine Liste der aktiven Verträge.

2. Um einen neuen Vertrag zu erstellen, wählen Sie **Neuer Vertrag**.

    ![Schaltfläche „Neuer Vertrag“](media/blockchain-workbench-use/contract-list.png)

3. Das Fenster **Neuer Vertrag** wird angezeigt. Geben Sie die Werte der Anfangsparameter an. Klicken Sie auf **Erstellen**.

    ![Bereich „Neuer Vertrag“](media/blockchain-workbench-use/new-contract.png)

    Der neu angelegte Vertrag wird in der Liste mit den anderen aktiven Verträgen angezeigt.

    ![Liste der aktiven Verträge](media/blockchain-workbench-use/active-contracts.png)

## <a name="take-action-on-contract"></a>Ausführen von Aktionen für den Vertrag

1. Wählen Sie im Abschnitt der Blockchain Workbench-Anwendung die Anwendungskachel, die den Vertrag enthält, um die Aktion auszuführen.

    ![Anwendungsliste](media/blockchain-workbench-use/apps-list.png)

2. Wählen Sie den Vertrag in der Liste aus.

    ![Vertragsliste](media/blockchain-workbench-use/select-contract.png)

    Die Details zum Vertrag werden in verschiedenen Abschnitten angezeigt. 

    ![Vertragsdetails](media/blockchain-workbench-use/contract-details.png)

    | Abschnitt  | BESCHREIBUNG  |
    |---------|---------|
    | Status | Liste des aktuellen Fortschritts innerhalb der Vertragsstufen |
    | Details | Die aktuellen Werte des Vertrags |
    | anzuzeigen. | Details zur letzten Aktion |
    | Aktivität | Transaktionsverlauf des Vertrags |
    
3. Wählen Sie im Abschnitt **Aktion** die Option **Aktion ausführen**.

4. Die Details zum aktuellen Status des Vertrags werden in einem Bereich angezeigt. Wählen Sie im Dropdownfeld die gewünschte Aktion aus. 

    ![Ausführen einer Aktion](media/blockchain-workbench-use/take-action.png)

5. Wählen Sie **Ausführen**, um die Aktion auszuführen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Problembehandlung bei Azure Blockchain Workbench](blockchain-workbench-troubleshooting.md)