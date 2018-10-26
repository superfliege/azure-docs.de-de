---
title: Verwalten von Classroom-Labs in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Classroom-Lab erstellen, alle Classroom-Labs anzeigen, den Registrierungslink für einen Lab-Benutzer freigeben oder ein Lab löschen.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2018
ms.author: spelluru
ms.openlocfilehash: eebdc98db5ecdf518d3b0b58e6757a2b7ecd5dd7
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043803"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Verwalten von Classroom-Labs in Azure Lab Services 
Dieser Artikel beschreibt das Erstellen und Konfigurieren eines Classroom-Labs, das Anzeigen aller Classroom-Labs und das Löschen eines Classroom-Labs.

## <a name="prerequisites"></a>Voraussetzungen
Zum Einrichten eines Classroom-Labs in einem Labkonto müssen Sie Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen eines Lab-Kontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Ein Besitzer des Labs kann der Rolle „Ersteller des Labs“ weitere Benutzer hinzufügen, indem er die Schritte im folgenden Artikel ausführt: [Hinzufügen eines Benutzers zur Rolle „Ersteller des Labs“](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). 
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. 
3. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie unter **Name** einen Namen für Ihr Lab an. 
    2. Geben Sie die maximale **Anzahl von Benutzern** an, die für das Lab zulässig sind. 
    6. Wählen Sie **Speichern**aus.

        ![Erstellen eines Classroom-Labs](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Führen Sie auf der Seite **Select virtual machine specifications** (Spezifikationen für virtuellen Computer auswählen) die folgenden Schritte aus:
    1. Wählen Sie eine **Größe** für die virtuellen Computer (VMs), die im Lab erstellt werden. 
    2. Wählen Sie die **Region** aus, in der die VMs erstellt werden sollen. 
    3. Wählen Sie das **VM-Image** aus, das zum Erstellen von VMs im Lab verwendet werden soll. 
    4. Klicken Sie auf **Weiter**.

        ![Angeben von VM-Spezifikationen](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Geben Sie auf der Seite **Anmeldeinformationen festlegen** die Standardanmeldeinformationen für alle VMs im Lab an. 
    1. Geben Sie den **Namen des Benutzers** für alle VMs im Lab an.
    2. Geben Sie das **Kennwort** für den Benutzer an. 

        > [!IMPORTANT]
        > Notieren Sie sich den Benutzernamen und das Kennwort. Diese Angaben werden nicht noch einmal angezeigt.
    3. Klicken Sie auf **Erstellen**. 

        ![Festlegen von Anmeldeinformationen](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Auf der Seite **Vorlage konfigurieren** wird der Status für den Prozess der Laberstellung angezeigt. Die Erstellung der Vorlage im Lab dauert bis zu 20 Minuten. 

    ![Konfigurieren der Vorlage](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Nachdem die Konfiguration der Vorlage abgeschlossen ist, wird die folgende Seite angezeigt: 

    ![Seite „Vorlage konfigurieren“ nach Abschluss des Vorgangs](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Die folgenden Schritte sind in diesem Tutorial optional: 
    1. Starten Sie die Vorlage für virtuelle Computer, indem Sie **Starten** wählen.
    2. Stellen Sie eine Verbindung mit der Vorlage für virtuelle Computer her, indem Sie **Verbinden** wählen. 
    3. Installieren und konfigurieren Sie die Software in der Vorlage für virtuelle Computer. 
    4. **Beenden** Sie den virtuellen Computer.  
    5. Geben Sie eine **Beschreibung** für die Vorlage ein.

        ![„Weiter“ auf der Seite „Vorlage konfigurieren“](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Wählen Sie auf der Vorlagenseite die Option **Weiter**. 
10. Führen Sie auf der Seite **Vorlage veröffentlichen** die folgenden Aktionen durch. 
    1. Aktivieren Sie zum sofortigen Veröffentlichen der Vorlage das Kontrollkästchen *I understand I can't modify the template after publishing. This process can only be done once and can take up to an hour* (Ich weiß, dass die Vorlage nach dem Veröffentlichen nicht mehr geändert werden kann. Dieser Prozess kann nur einmal durchgeführt werden und bis zu einer Stunde dauern.), und wählen Sie **Veröffentlichen**.  

        > [!WARNING]
        > Nachdem die Veröffentlichung erfolgt ist, kann sie nicht mehr rückgängig gemacht werden. 
    2. Wählen Sie **Für später speichern**, wenn Sie die Veröffentlichung später durchführen möchten. Sie können die Vorlage für virtuelle Computer veröffentlichen, nachdem der Assistent abgeschlossen wurde. Weitere Informationen zum Konfigurieren und Veröffentlichen nach Abschluss des Assistenten finden Sie im Abschnitt [Veröffentlichen der Vorlage](#publish-the-template) des Artikels [Verwalten von Classroom-Labs in Azure Lab Services](how-to-manage-classroom-labs.md).

        ![Vorlage veröffentlichen](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Für die Vorlage wird der **Status der Veröffentlichung** angezeigt. Dieser Vorgang kann bis zu einer Stunde dauern. 

    ![Veröffentlichen der Vorlage – Status](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Die folgende Seite wird angezeigt, wenn die Veröffentlichung der Vorlage erfolgreich war. Wählen Sie **Fertig**aus.

    ![Veröffentlichen der Vorlage – Erfolg](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Das **Dashboard** für das Lab wird angezeigt. 
    
    ![Dashboard für Classroom-Lab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Wechseln Sie zur Seite **Virtuelle Computer**, und vergewissern Sie sich, dass virtuelle Computer mit dem Status **Nicht zugewiesen** angezeigt werden. Diese virtuellen Computer sind noch keinen Teilnehmern zugewiesen. Sie sollten den Status **Beendet** aufweisen. Auf dieser Seite können Sie einen virtuellen Computer für einen Teilnehmer starten, eine Verbindung damit herstellen und ihn beenden und löschen. Sie können virtuelle Computer auf dieser Seite starten oder sie von Ihren Teilnehmern starten lassen. 

    ![Virtuelle Computer im Status „Beendet“](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="configure-usage-policy"></a>Konfigurieren der Nutzungsrichtlinie

1. Wählen Sie **Nutzungsrichtlinie** aus. 
2. Geben Sie unter **Nutzungsrichtlinie** in „Einstellungen“ die **Anzahl der Benutzer** ein, die das Lab verwenden dürfen.
3. Wählen Sie **Speichern**aus. 

    ![Nutzungsrichtlinie](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Einrichten der Vorlage
Eine Vorlage in einem Lab ist ein VM-Basisimage und dient zur Erstellung der virtuellen Computer aller Benutzer. Richten Sie den virtuellen Computer der Vorlage so ein, dass er genau das enthält, was Sie den Lab-Benutzern zur Verfügung stellen möchten. Sie können einen Namen und eine Beschreibung der Vorlage angeben, die den Lab-Benutzern angezeigt werden. Veröffentlichen Sie die Vorlage, um Instanzen der Vorlage für virtuelle Computer für Ihre Lab-Benutzer zur Verfügung zu stellen.  

### <a name="set-template-title-and-description"></a>Festlegen von Vorlagentitel und -beschreibung
1. Wählen Sie im Abschnitt **Vorlage** den Befehl **Bearbeiten** (Stiftsymbol) für die Vorlage aus. 
2. Geben Sie im Fenster **Benutzeransicht** einen **Titel** für die Vorlage ein.
3. Geben Sie eine **Beschreibung** für die Vorlage ein.
4. Wählen Sie **Speichern**aus.

    ![Beschreibung des Classroom-Labs](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>Einrichten der Vorlage für virtuelle Computer
 Sie können eine Verbindung mit der Vorlage für virtuelle Computer herstellen und darauf die erforderliche Software installieren, bevor Sie ihn für Ihre Teilnehmer bereitstellen. 

1. Warten Sie, bis die Vorlage für virtuelle Computer bereit ist. Wenn dies der Fall ist, wird die Schaltfläche **Starten** aktiviert. Wählen Sie die Option **Starten**, um den virtuellen Computer zu starten.

    ![Starten der Vorlage für virtuelle Computer](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Wählen Sie die Option **Verbinden**, und befolgen Sie die Anleitung, um eine Verbindung mit dem virtuellen Computer herzustellen. 

    ![Herstellen einer Verbindung mit der Vorlage für virtuelle Computer](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Installieren Sie die Software, die die Teilnehmer für das Lab benötigen (z.B. Visual Studio, Azure Storage-Explorer usw.). 
2. Trennen Sie die Verbindung mit der Vorlage für virtuelle Computer (indem Sie die Remotedesktopsitzung schließen). 
3. **Beenden** Sie die Vorlage für virtuelle Computer, indem Sie **Beenden** wählen. 

    ![Beenden der Vorlage für virtuelle Computer](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


## <a name="publish-the-template"></a>Veröffentlichen der Vorlage 
Wenn Sie eine Vorlage veröffentlichen, werden von Azure Lab Services im Lab mithilfe der Vorlage virtuelle Computer erstellt. Die Anzahl der in diesem Vorgang erstellten virtuellen Computer entspricht der maximalen Anzahl von Benutzern im Lab, die Sie in der Nutzungsrichtlinie des Labs festlegen können. Alle virtuellen Computer haben die gleiche Konfiguration wie die Vorlage. 

1. Wählen Sie im Abschnitt **Vorlage** die Option **Veröffentlichen**. 

    ![Veröffentlichen der Vorlage für virtuelle Computer](../media/tutorial-setup-classroom-lab/public-access.png)
1. Wählen Sie im Fenster **Veröffentlichen** die Option **Veröffentlicht**. 
2. Wählen Sie jetzt die Schaltfläche **Veröffentlichen**. Je nach Anzahl von zu erstellenden virtuellen Computern kann dieser Vorgang relativ lange dauern. Diese Anzahl entspricht der Anzahl von Benutzern, die für das Lab zulässig sind.
    
    > [!IMPORTANT]
    > Sobald eine Vorlage veröffentlicht wurde, kann dies nicht mehr rückgängig gemacht werden. 
4. Wechseln Sie zur Seite **Virtuelle Computer**, und vergewissern Sie sich, dass virtuelle Computer mit dem Status **Nicht zugewiesen** angezeigt werden. Diese virtuellen Computer sind noch keinen Teilnehmern zugewiesen. 

    ![Virtuelle Computer](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Warten Sie, bis die virtuellen Computer erstellt wurden. Sie sollten den Status **Beendet** aufweisen. Auf dieser Seite können Sie einen virtuellen Computer für einen Teilnehmer starten, eine Verbindung damit herstellen und ihn beenden und löschen. Sie können virtuelle Computer auf dieser Seite starten oder sie von Ihren Teilnehmern starten lassen. 

    ![Virtuelle Computer im Status „Beendet“](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="send-registration-link-to-students"></a>Senden eines Registrierungslinks an Teilnehmer

1. Wechseln Sie zur Ansicht **Dashboard**. 
2. Klicken Sie auf die Kachel **Benutzerregistrierung**.

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Klicken Sie im Dialogfeld **Benutzerregistrierung** auf die Schaltfläche **Kopieren**. Der Link wird in die Zwischenablage kopiert. Fügen Sie ihn in einen E-Mail-Editor ein, und senden Sie eine E-Mail an den Teilnehmer. 

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Wählen Sie im Dialogfeld **Benutzerregistrierung** die Option **Schließen**. 

## <a name="view-all-classroom-labs"></a>Anzeigen aller Classroom-Labs
1. Navigieren Sie zum [Azure Lab Services-Portal](https://labs.azure.com).
2. Stellen Sie sicher, dass alle Labs im ausgewählten Lab-Konto angezeigt werden. 

    ![Alle Labs](../media/how-to-manage-classroom-labs/all-labs.png)
3. Verwenden Sie die Dropdownliste im oberen Bereich, um ein anderes Lab-Konto auszuwählen. Die Labs im ausgewählten Lab-Konto werden angezeigt. 

## <a name="delete-a-classroom-lab"></a>Löschen eines Classroom-Labs
1. Wählen Sie auf der Kachel für das Lab die drei Punkte (...) in der Ecke aus. 

    ![Auswählen des Labs](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Klicken Sie auf **Löschen**. 

    ![Schaltfläche „Löschen“](../media/how-to-manage-classroom-labs/delete-button.png)
3. Klicken Sie im Dialogfeld **Lab löschen** auf **Löschen**. 

    ![Dialogfeld „Löschen“](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Verwalten von Studenten-VMs
Sobald sich die Studenten bei Azure Lab Services über den von Ihnen bereitgestellten Registrierungslink registrieren, sehen Sie die den Studenten zugewiesenen VMs auf der Registerkarte **Virtuelle Computer**. 

![Studenten zugewiesene virtuelle Computer](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Sie können folgende Aufgaben auf einer Studenten-VM ausführen: 

- Beenden einer VM, wenn diese ausgeführt wird. 
- Starten einer VM, wenn diese beendet wurde. 
- Stellen Sie eine Verbindung zur VM her. 
- Löschen Sie den virtuellen Computer. 

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit der Einrichtung eines Labs mit Azure Lab Services vertraut:

- [Einrichten eines Classroom-Labs](how-to-manage-classroom-labs.md)
- [Einrichten eines Labs](../tutorial-create-custom-lab.md)
