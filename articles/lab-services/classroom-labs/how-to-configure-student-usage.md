---
title: Konfigurieren von Nutzungseinstellungen in Classroom-Labs von Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Anzahl der Benutzer für das Lab konfigurieren, die Benutzer beim Lab registrieren, die Anzahl der Stunden steuern, in denen sie den virtuellen Computer verwenden können, und vieles mehr.
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
ms.openlocfilehash: a91d5826f52b2beb05f2d9a08f3646bd776b294e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142768"
---
# <a name="configure-usage-settings-and-policies"></a>Konfigurieren von Nutzungseinstellungen und Richtlinien
In diesem Artikel wird beschrieben, wie Sie die Anzahl der Benutzer für das Lab konfigurieren, die Benutzer beim Lab registrieren, die Anzahl der Stunden steuern, in denen sie den virtuellen Computer verwenden können, und vieles mehr. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Angeben der Anzahl der im Lab zulässigen Benutzer

1. Wählen Sie **Nutzungsrichtlinie** aus. 
2. Geben Sie unter **Nutzungsrichtlinie** in „Einstellungen“ die **Anzahl der Benutzer** ein, die das Lab verwenden dürfen.
3. Wählen Sie **Speichern**aus. 

    ![Nutzungsrichtlinie](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-users"></a>Senden eines Registrierungslinks an Benutzer

1. Wechseln Sie zur Ansicht **Dashboard**. 
2. Klicken Sie auf die Kachel **Benutzerregistrierung**.

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Klicken Sie im Dialogfeld **Benutzerregistrierung** auf die Schaltfläche **Kopieren**. Der Link wird in die Zwischenablage kopiert. Fügen Sie ihn in einen E-Mail-Editor ein, und senden Sie eine E-Mail an den Teilnehmer. Teilnehmer verwenden den Link, um die Seite aufzurufen, auf der Sie sich beim Lab registrieren können. 

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Wählen Sie im Dialogfeld **Benutzerregistrierung** die Option **Schließen**. 

## <a name="view-users-registered-with-the-lab"></a>Anzeigen der beim Lab registrierten Benutzer

Wählen Sie **Benutzer** im linken Menü, um die Liste der Benutzer anzuzeigen, die beim Lab registriert sind. 

![Liste der beim Lab registrierten Benutzer](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Festlegen von Kontingenten pro Benutzer

1. Wählen Sie **Benutzereinstellungen** im linken Menü.
2. Wählen Sie die Kachel **Kontingent pro Benutzer**. 
3. Wählen Sie **Anzahl der Stunden begrenzen, in denen ein Benutzer eine VM verwenden kann**. 
4. Geben Sie unter **Wie viele Stunden möchten Sie den einzelnen Benutzern erlauben?** die Anzahl der Stunden ein, und wählen Sie **Speichern**. 

    ![Anzahl von Stunden pro Benutzer](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Jetzt sehen Sie die Anzahl der Stunden auf der Kachel **Kontingent pro Benutzer**. 

    ![Kontingent pro Benutzer](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="manage-user-vms"></a>Verwalten von Benutzer-VMs
Sobald sich die Studenten bei Azure Lab Services über den von Ihnen bereitgestellten Registrierungslink registrieren, sehen Sie die den Studenten zugewiesenen VMs auf der Registerkarte **Virtuelle Computer**. 

![Studenten zugewiesene virtuelle Computer](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Sie können folgende Aufgaben auf einer Studenten-VM ausführen: 

- Beenden einer VM, wenn diese ausgeführt wird. 
- Starten einer VM, wenn diese beendet wurde. 
- Stellen Sie eine Verbindung zur VM her. 
- Löschen Sie den virtuellen Computer. 
- Zeigen Sie die Anzahl der Stunden an, für die Benutzer den virtuellen Computer verwendet haben. 


## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit der Einrichtung eines Labs mit Azure Lab Services vertraut:

- [Einrichten eines Classroom-Labs](how-to-manage-classroom-labs.md)
- [Einrichten eines Labs](../tutorial-create-custom-lab.md)
