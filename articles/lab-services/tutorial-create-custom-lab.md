---
title: Erstellen eines Labs mithilfe von Azure DevTest Labs | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung erstellen Sie ein Lab mithilfe von Azure DevTest Labs.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/18/2019
ms.author: spelluru
ms.openlocfilehash: 84a6cdb5e91128bbade43ee9212cfa9658228964
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423291"
---
# <a name="tutorial-set-up-a-lab-by-using-azure-devtest-labs"></a>Tutorial: Erstellen eines Labs mithilfe von Azure DevTest Labs
In diesem Tutorial erstellen Sie ein Lab über das Azure-Portal. Ein Lab-Administrator richtet ein Lab in einer Organisation ein, erstellt VMs im Lab und konfiguriert Richtlinien. Lab-Benutzer (z.B. Entwickler und Tester) fordern virtuelle Computer im Lab an, stellen eine Verbindung mit ihnen her und nutzen sie. 

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Labs
> * Hinzufügen virtueller Computer (VMs) zum Lab
> * Hinzufügen eines Benutzers zur Rolle „Lab-Benutzer“

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-a-lab"></a>Erstellen eines Labs
Die folgenden Schritte beschreiben, wie Sie mithilfe des Azure-Portals ein Lab in Azure DevTest Labs erstellen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Hauptmenü auf der linken Seite oben in der Liste den Eintrag **Ressource erstellen** aus, zeigen Sie auf **Entwicklertools**, und klicken Sie auf **DevTest Labs**. 

    ![Neues Menü „DevTest Lab“](./media/tutorial-create-custom-lab/new-custom-lab-menu.png)
1. Führen Sie im Fenster **DevTest Lab erstellen** die folgenden Aktionen aus: 
    1. Geben Sie in **Lab-Name** einen Namen für das Lab ein. 
    2. Wählen Sie unter **Abonnement** das Abonnement aus, in dem Sie das neue Lab erstellen möchten. 
    3. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein. 
    4. Wählen Sie als **Standort** den Standort oder die Region aus, an dem bzw. in der das Lab erstellt werden soll. 
    5. Klicken Sie auf **Erstellen**. 
    6. Wählen Sie die Option **An Dashboard anheften** aus. Nach der Erstellung des Lab wird dieses im Dashboard angezeigt. 

        ![Erstellen eines Lab-Abschnitts für DevTest Labs](./media/tutorial-create-custom-lab/create-custom-lab-blade.png)
2. Vergewissern Sie sich anhand der Benachrichtigungen, dass das Lab erfolgreich erstellt wurde. Wählen Sie **Zu Ressource wechseln** aus.  

    ![Benachrichtigung](./media/tutorial-create-custom-lab/creation-notification.png)
3. Vergewissern Sie sich, dass die Seite **DevTest Lab** für Ihr Lab angezeigt wird. 

    ![Startseite für Ihr lab](./media/tutorial-create-custom-lab/lab-home-page.png)

## <a name="add-a-vm-to-the-lab"></a>Hinzufügen einer VM zum Lab

1. Klicken Sie auf der Seite **DevTest Lab** auf der Symbolleiste auf **+ Hinzufügen**. 

    ![Schaltfläche „Hinzufügen“](./media/tutorial-create-custom-lab/add-vm-to-lab-button.png)
1. Suchen Sie auf der Seite **Basis auswählen** nach einem Stichwort (Beispiel: Windows, Ubuntu), und wählen Sie in der Liste eines der Basisimages aus. 
1. Gehen Sie auf der Seite **Virtueller Computer** wie folgt vor: 
    1. Geben Sie in **Name des virtuellen Computers** einen Namen für den virtuellen Computer ein. 
    2. Geben Sie für **Benutzername** den Namen des Benutzers ein, der Zugriff auf den virtuellen Computer hat. 
    3. Geben Sie unter **Kennwort** das Kennwort für den Benutzer ein. 

        ![Auswählen einer Grundlage](./media/tutorial-create-custom-lab/new-virtual-machine.png)
1. Wählen Sie die Registerkarte **Erweiterte Einstellungen** aus.
    1. Wählen Sie für **Diesen Computer als abrufbar festlegen** die Option **Ja** aus.
    2. Vergewissern Sie sich, dass die **Anzahl der Instanzen** auf **1** festgelegt ist. Wenn Sie die Einstellung **2** wählen, werden 2 VMs mit den Namen `<base image name>00' and <base image name>01` erstellt. Beispiel: `win10vm00` und `win10vm01`.     
    3. Klicken Sie auf **Submit** (Senden). 

        ![Auswählen einer Grundlage](./media/tutorial-create-custom-lab/new-vm-advanced-settings.png)
    9. Sie sehen den Status der VM in der Liste **Abrufbare virtuelle Computer**. Das Erstellen des virtuellen Computers dauert ca. 25 Minuten. Die VM wird in einer eigenen Azure-Ressourcengruppe angelegt, deren Name mit dem Namen der aktuellen Ressourcengruppe beginnt, zu der das Lab gehört. Wenn sich das Lab beispielsweise in `labrg` befindet, kann die VM in der Ressourcengruppe `labrg3988722144002` erstellt werden. 

        ![Status der VM-Erstellung](./media/tutorial-create-custom-lab/vm-creation-status.png)
1. Nach ihrer Erstellung sehen Sie sie in der Liste **Abrufbare virtuelle Computer**. 

    > [!NOTE] 
    > Beim Hinzufügen einer Linux-VM zu einem Lab können Sie den SSH- und RDP-Zugriff auf die VM aktivieren. Wenn Sie den Zugriff nicht beim Erstellen der VM aktivieren, können Sie manuell Regeln in der Netzwerksicherheitsgruppe hinzufügen, die der VM zugeordnet ist, um Ports für SSH und RDP zu öffnen.

## <a name="add-a-user-to-the-lab-user-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Benutzer“

1. Wählen Sie im linken Menü die Option **Konfiguration und Richtlinien** aus. 

    ![Konfiguration und Richtlinien](./media/tutorial-create-custom-lab/configuration-and-policies-menu.png)
1. Wählen Sie im Menü **Zugriffssteuerung (IAM)** und dann auf der Symbolleiste **+ Rollenzuweisung hinzufügen** aus. 

    ![Rollenzuweisung hinzufügen (Schaltfläche)](./media/tutorial-create-custom-lab/add-role-assignment-button.png)
1. Gehen Sie auf der Seite **Berechtigungen hinzufügen** folgendermaßen vor:
    1. Wählen Sie für **Rolle** die Option **DevTest Labs-Benutzer**. 
    2. Wählen Sie den **Benutzer** aus, den Sie hinzufügen möchten. 
    3. Wählen Sie **Speichern** aus.

        ![Benutzer hinzufügen](./media/tutorial-create-custom-lab/add-user.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Das nächste Tutorial zeigt, wie ein Lab-Benutzer eine VM im Lab abrufen und sich mit ihr verbinden kann. Wenn Sie dieses Tutorial nicht durchführen möchten und die im Rahmen dieses Tutorials erstellten Ressourcen bereinigen möchten, gehen Sie folgendermaßen vor: 

1. Wählen Sie im Azure-Portal im Menü **Ressourcengruppen** aus. 

    ![Ressourcengruppen](./media/tutorial-create-custom-lab/resource-groups.png)
1. Wählen Sie die Ressourcengruppe aus, in der Sie das Lab erstellt haben. 
1. Klicken Sie auf der Symbolleiste auf **Ressourcengruppe löschen**. Beim Löschen einer Ressourcengruppe werden alle Ressourcen in der Gruppe, einschließlich des Labs, gelöscht. 

    ![Lab-Ressourcengruppe](./media/tutorial-create-custom-lab/lab-resource-group.png)
1. Wiederholen Sie diese Schritte zum Löschen der zusätzlichen Ressourcengruppe, die für Sie mit dem Namen `<your resource group name><random numbers>` erstellt wurde. Beispiel: `splab3988722144001`. Die VMs werden in dieser Ressourcengruppe und nicht in der Ressourcengruppe erstellt, in der das Lab vorhanden ist. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Lab mit einem virtuellen Computer erstellt und einem Benutzer Zugriff auf das Lab gewährt. Um zu erfahren, wie Sie als Lab-Benutzer auf das Lab zugreifen, fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Tutorial: Zugreifen auf das Lab](tutorial-use-custom-lab.md)

