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
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 371751f2eb643c3699e5462fe44380d7792243ef
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54388302"
---
# <a name="configure-usage-settings-and-policies"></a>Konfigurieren von Nutzungseinstellungen und Richtlinien
In diesem Artikel wird beschrieben, wie Sie dem Lab Benutzer hinzufügen, diese beim Lab registrieren, die Anzahl der Stunden steuern, für die sie den virtuellen Computer verwenden können, und vieles mehr. 


## <a name="add-users-to-the-lab"></a>Hinzufügen von Benutzern zum Lab
Wenn Sie **Zugriff beschränken** aktiviert haben, fügen Sie der Liste Benutzer (E-Mail-Adressen) hinzu.

1. Klicken Sie im linken Menü auf **Benutzer**.
2. Klicken Sie auf der Symbolleiste auf **Benutzer hinzufügen**. 
3. Geben Sie auf der Seite **Benutzer hinzufügen** die E-Mail-Adressen von Benutzern in separaten Zeilen oder durch Semikolons getrennt in einer einzelnen Zeile ein. 

    ![Hinzufügen der E-Mail-Adressen von Benutzern](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Wählen Sie **Speichern** aus. In der Liste werden die E-Mail-Adressen und der Status (registriert oder nicht registriert) von Benutzern angezeigt. 

    ![Benutzerliste](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="send-registration-link-to-students"></a>Senden eines Registrierungslinks an Teilnehmer
Das folgende Verfahren enthält Schritte zum Senden eines Registrierungslinks an Benutzer. Wenn die **Zugriffsbeschränkung** für das Lab aktiviert ist, können sich nur Benutzer in der Liste der Benutzer über den Registrierungslink beim Lab registrieren. 

1. Wählen Sie im linken Menü die Option **Benutzer** aus, um zur Ansicht **Benutzer** zu wechseln. 
2. Wählen Sie die Kachel **Get registration link** (Registrierungslink abrufen) aus.

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Klicken Sie im Dialogfeld **Benutzerregistrierung** auf die Schaltfläche **Kopieren**. Der Link wird in die Zwischenablage kopiert. Fügen Sie ihn in einen E-Mail-Editor ein, und senden Sie eine E-Mail an den Teilnehmer. 

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Wählen Sie im Dialogfeld **Benutzerregistrierung** die Option **Schließen**. 
4. Teilen Sie den Registrierungslink mit einem Schüler bzw. Studenten, damit diese Person sich für die Klasse registrieren kann. Wenn Sie die Einstellung **Zugriff beschränken** aktiviert und der Liste Benutzer hinzugefügt haben, führen Sie die folgenden Schritte aus:
    1. Wählen Sie in der Liste die **E-Mail-Adresse** des Benutzers aus. 
    2. Sie sehen ein Fenster Ihres Standard-E-Mail-Programms, in dem das Adressfeld **An** ausgefüllt ist. 
    3. Fügen Sie die **Registrierungs-URL** ein, die Sie zuvor kopiert haben. 
    4. Senden Sie die **E-Mail**. 

## <a name="view-users-registered-with-the-lab"></a>Anzeigen der beim Lab registrierten Benutzer

Wählen Sie **Benutzer** im linken Menü, um die Liste der Benutzer anzuzeigen, die beim Lab registriert sind. 

![Liste der beim Lab registrierten Benutzer](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-per-user"></a>Festlegen von Kontingenten pro Benutzer
Mithilfe der folgenden Schritte können Sie Kontingente pro Benutzer festlegen: 

1. Klicken Sie im linken Menü auf **Benutzer**.
2. Wählen Sie auf der Symbolleiste die Option **Quota per user: unlimited** (Kontingent pro Benutzer: unbegrenzt) aus. 
3. Wählen Sie auf der Seite **Quota per user** (Kontingent pro Benutzer) die Option **Anzahl der Stunden begrenzen, in denen ein Benutzer eine VM verwenden kann** aus. 
4. Geben Sie unter **Wie viele Stunden möchten Sie den einzelnen Benutzern erlauben?** die gewünschte Stundenanzahl ein, und wählen Sie **Speichern** aus. 

    ![Anzahl von Stunden pro Benutzer](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Jetzt sehen Sie die Anzahl der Stunden auf der Symbolleiste: **Kontingent pro Benutzer: &lt;Anzahl Stunden&gt;**. 

    ![Kontingent pro Benutzer](../media/how-to-configure-student-usage/quota-per-user.png)

### <a name="add-users-by-uploading-a-csv-file"></a>Hinzufügen von Benutzern durch Hochladen einer CSV-Datei
Sie können auch eine CSV-Datei mit E-Mail-Adressen von Benutzern hochladen, um Benutzer hinzuzufügen.

1. Wählen Sie auf der Symbolleiste die Option **CSV hochladen** aus.
2. Wählen Sie die CSV-Datei mit den Benutzer-E-Mail-Adressen aus. Die E-Mail-Adressen müssen sich alle in einer einzelnen Spalte befinden, wenn Sie die Datei in Excel öffnen. 

## <a name="manage-user-vms"></a>Verwalten von Benutzer-VMs
Sobald sich die Studenten bei Azure Lab Services über den von Ihnen bereitgestellten Registrierungslink registrieren, sehen Sie die den Studenten zugewiesenen VMs auf der Registerkarte **Virtuelle Computer**. 

![Studenten zugewiesene virtuelle Computer](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Sie können folgende Aufgaben auf einer Studenten-VM ausführen: 

- Beenden einer VM, wenn diese ausgeführt wird. 
- Starten einer VM, wenn diese beendet wurde. 
- Stellen Sie eine Verbindung zur VM her. 
- Löschen Sie den virtuellen Computer. 
- Zeigen Sie die Anzahl der Stunden an, für die Benutzer den virtuellen Computer verwendet haben. 

## <a name="update-number-of-virtual-machines-in-lab"></a>Aktualisieren der Anzahl von virtuellen Computern im Lab
Um die Anzahl der virtuellen Computer im Lab zu aktualisieren, gehen Sie auf der Seite **Virtuelle Computer** folgendermaßen vor:

1. Wählen Sie im linken Menü **Virtuelle Computer** aus. 
2. Wählen Sie auf der Symbolleiste **Lab capacity (Labkapazität): &lt;Anzahl&gt; Computer** aus. 
3. Geben Sie die **Anzahl** von virtuellen Computern ein.
4. Wählen Sie **Speichern** aus.

    ![Virtuelle Computer im Lab](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labkonten als Administrator](how-to-manage-lab-accounts.md)
- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
