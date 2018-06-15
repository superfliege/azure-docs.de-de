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
ms.openlocfilehash: a96ba4aec7c23c040921a647cc4986aaf53fb30c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651409"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Tutorial: Einrichten eines Classroom-Labs 
In diesem Tutorial richten Sie ein Classroom-Lab mit einer Reihe virtueller Computer ein, die von den Teilnehmern im Classroom verwendet werden.  

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Classroom-Labs
> * Konfigurieren des Classroom-Labs
> * Senden eines Registrierungslinks an Teilnehmer

## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com).
2. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie einen **Namen** für das Classroom-Lab an. 
    2. Wählen Sie die **Größe** des virtuellen Computers, den Sie im Classroom verwenden möchten.
    3. Wählen Sie das **Image**, mit dem der virtuelle Computer erstellt werden soll.
    4. Geben Sie **Standardanmeldeinformationen** zur Anmeldung am virtuellen Computer im Lab an. 
    7. Wählen Sie **Speichern**aus.

        ![Erstellen eines Classroom-Labs](../media/tutorial-setup-classroom-lab/new-lab-window.png)
1. Die **Startseite** Ihres Labs wird angezeigt. 
    
    ![Startseite des Classroom-Labs](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Konfigurieren der Nutzungsrichtlinie

1. Wählen Sie **Nutzungsrichtlinie** aus. 
2. Geben Sie unter **Nutzungsrichtlinie** in „Einstellungen“ die **Anzahl der Benutzer** ein, die das Lab verwenden dürfen.
3. Wählen Sie **Speichern**aus. 

    ![Nutzungsrichtlinie](../media/tutorial-setup-classroom-lab/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Einrichten der Vorlage 
Eine Vorlage in einem Lab ist ein VM-Basisimage und dient zur Erstellung der virtuellen Computer aller Benutzer. Richten Sie den virtuellen Computer der Vorlage so ein, dass er genau das enthält, was Sie den Lab-Benutzern zur Verfügung stellen möchten. Sie können einen Namen und eine Beschreibung der Vorlage angeben, die den Lab-Benutzern angezeigt werden. Legen Sie die Sichtbarkeit der Vorlage auf „Öffentlich“ fest, um Instanzen des virtuellen Vorlagencomputers für Ihre Lab-Benutzer zur Verfügung zu stellen. 

### <a name="set-title-and-description"></a>Festlegen von Titel und Beschreibung
1. Wählen Sie im Abschnitt **Vorlage** den Befehl **Bearbeiten** (Stiftsymbol) für die Vorlage aus. 
2. Geben Sie im Fenster **Benutzeransicht** einen **Titel** für die Vorlage ein.
3. Geben Sie eine **Beschreibung** für die Vorlage ein.
4. Wählen Sie **Speichern**aus.

    ![Beschreibung des Classroom-Labs](../media/tutorial-setup-classroom-lab/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Festlegen von Instanzen der Vorlage auf „Öffentlich“
Nachdem Sie die Sichtbarkeit einer Vorlage auf **Öffentlich** festgelegt haben, erstellt Azure Lab Services mithilfe der Vorlage virtuelle Computer im Lab. Die Anzahl der in diesem Vorgang erstellten virtuellen Computer entspricht der maximalen Anzahl von Benutzern im Lab, die Sie in der Nutzungsrichtlinie des Labs festlegen können. Alle virtuellen Computer haben die gleiche Konfiguration wie die Vorlage. 

1. Wählen Sie im Abschnitt **Vorlage** die Option **Sichtbarkeit** aus. 
2. Wählen Sie auf der Seite **Verfügbarkeit** die Option **Öffentlich** aus.
    
    > [!IMPORTANT]
    > Sobald eine Vorlage öffentlich verfügbar ist, kann der Zugriff darauf nicht in „Privat“ geändert werden. 
3. Wählen Sie **Speichern**aus.

    ![Verfügbarkeit](../media/tutorial-setup-classroom-lab/public-access.png)

## <a name="send-registration-link-to-students"></a>Senden eines Registrierungslinks an Teilnehmer

1. Klicken Sie auf die Kachel **Benutzerregistrierung**.
2. Klicken Sie im Dialogfeld **Benutzerregistrierung** auf die Schaltfläche **Kopieren**. Der Link wird in die Zwischenablage kopiert. Fügen Sie ihn in einen E-Mail-Editor ein, und senden Sie eine E-Mail an den Teilnehmer. 

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/registration-link.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Classroom-Lab erstellt und konfiguriert. Um zu erfahren, wie ein Teilnehmer auf einen virtuellen Computer im Labor über den Registrierungslink zugreifen kann, fahren Sie mit nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einem virtuellen Computer im Classroom-Lab](tutorial-connect-virtual-machine-classroom-lab.md)

