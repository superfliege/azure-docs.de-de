---
title: Einrichten eines Classroom-Labs in Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial richten Sie ein Lab für die Verwendung in einem Classroom ein.
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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 163763bf1203a045326c7163b5f6da9aa417d8cf
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081855"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Tutorial: Einrichten eines Classroom-Labs 
In diesem Tutorial richten Sie ein Classroom-Lab mit virtuellen Computern ein, die von den Teilnehmern im Classroom verwendet werden.  

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Classroom-Labs
> * Konfigurieren des Classroom-Labs
> * Senden eines Registrierungslinks an Teilnehmer

## <a name="prerequisites"></a>Voraussetzungen
Zum Einrichten eines Classroom-Labs in einem Labkonto müssen Sie Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Ein Besitzer des Labs kann der Rolle „Ersteller des Labs“ einen Benutzer hinzufügen, indem er die Schritte im folgenden Artikel ausführt: [Hinzufügen eines Benutzers zur Rolle „Lab-Ersteller“](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com).
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. 
3. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie einen **Namen** für das Classroom-Lab an. 
    2. Wählen Sie die **Größe** des virtuellen Computers, den Sie im Classroom verwenden möchten.
    3. Wählen Sie das **Image**, mit dem der virtuelle Computer erstellt werden soll.
    4. Geben Sie **Standardanmeldeinformationen** zur Anmeldung am virtuellen Computer im Lab an. 
    7. Wählen Sie **Speichern**aus.

        ![Erstellen eines Classroom-Labs](../media/tutorial-setup-classroom-lab/new-lab-window.png)
1. Das **Dashboard** für das Lab wird angezeigt. 
    
    ![Dashboard für Classroom-Lab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Konfigurieren der Nutzungsrichtlinie

1. Wählen Sie **Nutzungsrichtlinie** aus. 
2. Geben Sie unter **Nutzungsrichtlinie** in „Einstellungen“ die **Anzahl der Benutzer** ein, die das Lab verwenden dürfen.
3. Wählen Sie **Speichern**aus. 

    ![Nutzungsrichtlinie](../media/tutorial-setup-classroom-lab/usage-policy-settings.png)


## <a name="set-up-the-template"></a>Einrichten der Vorlage 
Eine Vorlage in einem Lab ist ein VM-Basisimage und dient zur Erstellung der virtuellen Computer aller Benutzer. Richten Sie den virtuellen Computer der Vorlage so ein, dass er genau das enthält, was Sie den Lab-Benutzern zur Verfügung stellen möchten. Sie können einen Namen und eine Beschreibung der Vorlage angeben, die den Lab-Benutzern angezeigt werden. Veröffentlichen Sie die Vorlage, um Instanzen der Vorlage für virtuelle Computer für Ihre Lab-Benutzer zur Verfügung zu stellen. 

### <a name="set-title-and-description"></a>Festlegen von Titel und Beschreibung
1. Wählen Sie im Abschnitt **Vorlage** den Befehl **Bearbeiten** (Stiftsymbol) für die Vorlage aus. 
2. Geben Sie im Fenster **Benutzeransicht** einen **Titel** für die Vorlage ein.
3. Geben Sie eine **Beschreibung** für die Vorlage ein.
4. Wählen Sie **Speichern**aus.

    ![Beschreibung des Classroom-Labs](../media/tutorial-setup-classroom-lab/lab-description.png)

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


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Classroom-Lab erstellt und konfiguriert. Um zu erfahren, wie ein Teilnehmer auf einen virtuellen Computer im Labor über den Registrierungslink zugreifen kann, fahren Sie mit nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einem virtuellen Computer im Classroom-Lab](tutorial-connect-virtual-machine-classroom-lab.md)

