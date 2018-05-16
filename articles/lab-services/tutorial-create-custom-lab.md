---
title: Erstellen eines benutzerdefinierten Labs mithilfe von Azure DevTest Labs | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung erstellen Sie ein benutzerdefiniertes Lab mithilfe von Azure DevTest Labs.
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
ms.date: 04/09/2018
ms.author: spelluru
ms.openlocfilehash: df70322a2d6562fce8da17fde652dd0dfbfcaec4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-set-up-a-custom-lab-by-using-azure-devtest-labs"></a>Tutorial: Erstellen eines benutzerdefinierten Labs mithilfe von Azure DevTest Labs
In diesem Tutorial erstellen Sie ein benutzerdefiniertes Lab im Azure-Portal. Ein Lab-Administrator richtet ein Lab in einer Organisation ein, erstellt VMs im Lab und konfiguriert Richtlinien. Lab-Benutzer (z.B. Entwickler und Tester) fordern virtuelle Computer im Lab an, stellen eine Verbindung mit ihnen her und nutzen sie. 

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines benutzerdefinierten Labs
> * Hinzufügen virtueller Computer (VMs) zum Lab
> * Hinzufügen eines Benutzers zur Rolle „Lab-Benutzer“

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-a-custom-lab"></a>Erstellen eines benutzerdefinierten Labs
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

## <a name="add-a-vm-to-the-lab"></a>Hinzufügen einer VM zum Lab

1. Klicken Sie auf der Seite **DevTest Lab** auf der Symbolleiste auf **+ Hinzufügen**. 

    ![Schaltfläche „Hinzufügen“](./media/tutorial-create-custom-lab/add-vm-to-lab-button.png)
1. Suchen Sie auf der Seite **Basis wählen** mit dem Stichwort **Ubuntu**, und wählen Sie in der Liste eines der Basisimages aus. 
1. Gehen Sie auf der Seite **Virtueller Computer** wie folgt vor: 
    1. Geben Sie in **Name des virtuellen Computers** einen Namen für den virtuellen Computer ein. 
    2. Geben Sie für **Benutzername** den Namen des Benutzers ein, der Zugriff auf den virtuellen Computer hat. 
    3. Geben Sie unter **Wert eingeben** das Kennwort des Benutzers ein. 
    4. Wählen Sie **Erweiterte Einstellungen**.
    5. Wählen Sie für **Diesen Computer als abrufbar festlegen** die Option **Ja** aus.
    6. Vergewissern Sie sich, dass die **Anzahl der Instanzen** auf **1** festgelegt ist. Wenn Sie die Einstellung **2** wählen, werden 2 VMs mit den Namen `<base image name>00' and <base image name>01` erstellt. Beispiel: `win10vm00` und `win10vm01`. 
    7. Klicken Sie zum Schließen der Seite **Erweitert** auf **OK**. 
    8. Klicken Sie auf **Erstellen**. 

        ![Auswählen eines Basisimages](./media/tutorial-create-custom-lab/new-virtual-machine.png)
    9. Sie sehen den Status der VM in der Liste **Abrufbare virtuelle Computer**. Das Erstellen des virtuellen Computers dauert ca. 25 Minuten. Die VM wird in einer eigenen Azure-Ressourcengruppe angelegt, deren Name mit dem Namen der aktuellen Ressourcengruppe beginnt, zu der das Lab gehört. Wenn sich das Lab beispielsweise in `labrg` befindet, kann die VM in der Ressourcengruppe `labrg3988722144002` erstellt werden. 

        ![Status der VM-Erstellung](./media/tutorial-create-custom-lab/vm-creation-status.png)
1. Nach ihrer Erstellung sehen Sie sie in der Liste **Abrufbare virtuelle Computer**. 

## <a name="add-a-user-to-the-lab-user-role"></a>Hinzufügen eines Benutzers zur Rolle „Lab-Benutzer“

1. Wählen Sie im linken Menü die Option **Konfiguration und Richtlinien** aus. 

    ![Konfiguration und Richtlinien](./media/tutorial-create-custom-lab/configuration-and-policies-menu.png)
1. Klicken Sie im Menü auf **Zugriffssteuerung (IAM)** und dann auf der Symbolleiste auf **Hinzufügen**. 

    ![Zugriffssteuerung -> Schaltfläche „Benutzer hinzufügen“](./media/tutorial-create-custom-lab/access-control-add.png)
1. Gehen Sie auf der Seite **Berechtigungen hinzufügen** folgendermaßen vor:
    1. Wählen Sie für **Rolle** die Option **DevTest Labs-Benutzer**. 
    2. Wählen Sie den **Benutzer** aus, den Sie hinzufügen möchten. 
    3. Wählen Sie **Speichern** aus.

        ![Hinzufügen von Berechtigungen](./media/tutorial-create-custom-lab/add-lab-user.png)
4. Um **Konfigurations- und Richtlinien – Zugriffssteuerung (IAM)** zu schließen, klicken Sie rechts oben auf **X**. 

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen
Das nächste Tutorial zeigt, wie ein Lab-Benutzer eine VM im Lab abrufen und sich mit ihr verbinden kann. Wenn Sie dieses Tutorial nicht durchführen möchten und die im Rahmen dieses Tutorials erstellten Ressourcen bereinigen möchten, gehen Sie folgendermaßen vor: 

1. Wählen Sie im Azure-Portal im Menü **Ressourcengruppen** aus. 
2. Wählen Sie die Ressourcengruppe aus, in der Sie das Lab erstellt haben. 
3. Klicken Sie auf der Symbolleiste auf **Ressourcengruppe löschen**. Beim Löschen einer Ressourcengruppe werden alle Ressourcen in der Gruppe, einschließlich des Labs, gelöscht. 
4. Wiederholen Sie diese Schritte zum Löschen der zusätzlichen Ressourcengruppe, die für Sie mit dem Namen `<your resource group name><random numbers>` erstellt wurde. Beispiel: `splab3988722144001`. Die VMs werden in dieser Ressourcengruppe und nicht in der Ressourcengruppe erstellt, in der das Lab vorhanden ist. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein benutzerdefiniertes Lab mit einer VM erstellt und einem Benutzer Zugriff auf das Lab gewährt. Um zu erfahren, wie Sie als Lab-Benutzer auf das Lab zugreifen, fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Tutorial: Zugreifen auf das benutzerdefinierte Lab](tutorial-use-custom-lab.md)

