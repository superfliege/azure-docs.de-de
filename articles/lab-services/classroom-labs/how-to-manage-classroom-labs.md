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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 48056d6e2988dd674351aca83526032175c355b6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214391"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Verwalten von Classroom-Labs in Azure Lab Services 
Dieser Artikel beschreibt das Erstellen und Konfigurieren eines Classroom-Labs, das Anzeigen aller Classroom-Labs und das Löschen eines Classroom-Labs.

## <a name="prerequisites"></a>Voraussetzungen
Zum Einrichten eines Classroom-Labs in einem Labkonto müssen Sie Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen eines Lab-Kontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Ein Besitzer des Labs kann der Rolle „Ersteller des Labs“ weitere Benutzer hinzufügen, indem er die Schritte im folgenden Artikel ausführt: [Hinzufügen eines Benutzers zur Rolle „Ersteller des Labs“](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com).
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten.
3. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie einen **Namen** für das Classroom-Lab an. 
    2. Wählen Sie die **Größe** des virtuellen Computers, den Sie im Classroom verwenden möchten.
    3. Wählen Sie das **Image**, mit dem der virtuelle Computer erstellt werden soll.
    4. Geben Sie **Standardanmeldeinformationen** an, die zur Anmeldung am virtuellen Computer im Lab verwendet werden sollen.
    7. Wählen Sie **Speichern**aus.

        ![Erstellen eines Classroom-Labs](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. Das **Dashboard** für das Lab wird angezeigt. 
    
    ![Dashboard für Classroom-Lab](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

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


### <a name="publish-the-template"></a>Veröffentlichen der Vorlage 
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
