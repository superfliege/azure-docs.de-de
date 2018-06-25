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
ms.openlocfilehash: 705209becff7c8ad20e7d09f056aa1ae1577b7ae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659488"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Verwalten von Classroom-Labs in Azure Lab Services 
Dieser Artikel beschreibt das Erstellen und Konfigurieren eines Classroom-Labs, das Anzeigen aller Classroom-Labs und das Löschen eines Classroom-Labs.

## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com).
2. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie einen **Namen** für das Classroom-Lab an. 
    2. Wählen Sie die **Größe** des virtuellen Computers, den Sie im Classroom verwenden möchten.
    3. Wählen Sie das **Image**, mit dem der virtuelle Computer erstellt werden soll.
    4. Geben Sie **Standardanmeldeinformationen** an, die zur Anmeldung am virtuellen Computer im Lab verwendet werden sollen.
    7. Wählen Sie **Speichern**aus.

        ![Erstellen eines Classroom-Labs](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. Die **Startseite** Ihres Labs wird angezeigt. 
    
    ![Startseite des Classroom-Labs](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Konfigurieren der Nutzungsrichtlinie

1. Wählen Sie **Nutzungsrichtlinie** aus. 
2. Geben Sie unter **Nutzungsrichtlinie** in „Einstellungen“ die **Anzahl der Benutzer** ein, die das Lab verwenden dürfen.
3. Wählen Sie **Speichern**aus. 

    ![Nutzungsrichtlinie](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Einrichten der Vorlage
Eine Vorlage in einem Lab ist ein VM-Basisimage und dient zur Erstellung der virtuellen Computer aller Benutzer. Richten Sie den virtuellen Computer der Vorlage so ein, dass er genau das enthält, was Sie den Lab-Benutzern zur Verfügung stellen möchten. Sie können einen Namen und eine Beschreibung der Vorlage angeben, die den Lab-Benutzern angezeigt werden. Legen Sie die Sichtbarkeit der Vorlage auf „Öffentlich“ fest, um Instanzen des virtuellen Vorlagencomputers für Ihre Lab-Benutzer zur Verfügung zu stellen.  

### <a name="set-template-title-and-description"></a>Festlegen von Vorlagentitel und -beschreibung
1. Wählen Sie im Abschnitt **Vorlage** den Befehl **Bearbeiten** (Stiftsymbol) für die Vorlage aus. 
2. Geben Sie im Fenster **Benutzeransicht** einen **Titel** für die Vorlage ein.
3. Geben Sie eine **Beschreibung** für die Vorlage ein.
4. Wählen Sie **Speichern**aus.

    ![Beschreibung des Classroom-Labs](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Festlegen von Instanzen der Vorlage auf „Öffentlich“ 
Nachdem Sie die Sichtbarkeit einer Vorlage auf **Öffentlich** festgelegt haben, erstellt Azure Lab Services mithilfe der Vorlage VMs im Lab. Die Anzahl der in diesem Vorgang erstellten virtuellen Computer entspricht der maximalen Anzahl von Benutzern im Lab, die Sie in der Nutzungsrichtlinie des Labs festlegen können. Alle virtuellen Computer haben die gleiche Konfiguration wie die Vorlage.  

1. Wählen Sie im Abschnitt **Vorlage** die Option **Sichtbarkeit** aus. 
2. Wählen Sie auf der Seite **Verfügbarkeit** die Option **Öffentlich** aus.
    
    > [!IMPORTANT]
    > Sobald eine Vorlage öffentlich verfügbar ist, kann der Zugriff darauf nicht in „Privat“ geändert werden. 
3. Wählen Sie **Speichern**aus.

    ![Verfügbarkeit](../media/how-to-manage-classroom-labs/public-access.png)

## <a name="send-registration-link-to-students"></a>Senden eines Registrierungslinks an Teilnehmer

1. Klicken Sie auf die Kachel **Benutzerregistrierung**.
2. Klicken Sie im Dialogfeld **Benutzerregistrierung** auf die Schaltfläche **Kopieren**. Der Link wird in die Zwischenablage kopiert. Fügen Sie ihn in einen E-Mail-Editor ein, und senden Sie eine E-Mail an den Teilnehmer. 

    ![Registrierungslink für Teilnehmer](../media/how-to-manage-classroom-labs/registration-link.png)

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
 

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit der Einrichtung eines Labs mit Azure Lab Services vertraut:

- [Einrichten eines Classroom-Labs](how-to-manage-classroom-labs.md)
- [Einrichten eines Labs](../tutorial-create-custom-lab.md)
