---
title: Aktivieren von Remotedesktop für Linux in Azure Lab Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services aktivieren.
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
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 28f963ab807cbd679b7efe8b034ab91f2893198e
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58659455"
---
# <a name="enable-and-use-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Aktivieren und Verwenden von Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services
In diesem Artikel wird gezeigt, wie Sie Folgendes durchführen:

- Aktivieren von Remotedesktop für einen virtuellen Linux-Computer
- Herstellen einer Verbindung mit der Vorlage für virtuelle Computer per Remotedesktopverbindung (RDP) für Lehrkräfte
- Herstellen einer Verbindung mit dem virtuellen Computer für Kursteilnehmer per RDP

## <a name="enable-remote-desktop-for-linux-vm"></a>Aktivieren von Remotedesktop für einen virtuellen Linux-Computer
Während der Lab-Erstellung können Lehrkräfte die **Remotedesktopverbindung** für **Linux**-Images aktivieren. Die Option **Remotedesktop aktivieren** wird angezeigt, wenn für die Vorlage ein Linux-Image ausgewählt wurde. Wenn diese Option aktiviert ist, können Lehrkräfte eine Verbindung mit der Vorlage für virtuelle Computer und mit virtuellen Computern für Kursteilnehmer per RDP (Remotedesktopverbindung) herstellen. 

![Aktivieren der Remotedesktopverbindung für ein Linux-Image](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

> [!IMPORTANT] 
> Beim Aktivieren der **Remotedesktopverbindung** wird auf Linux-Computern nur der **RDP**-Port geöffnet. Sie als Lehrkraft stellen eine Verbindung mit dem Linux-Computer beim ersten Mal per SSH her und installieren RDP- und GUI-Pakete, damit Sie später RDP nutzen können, um eine Verbindung mit dem Linux-Computer herzustellen. Anschließend **veröffentlichen** Sie das Image, damit die Kursteilnehmer per RDP eine Verbindung mit den virtuellen Linux-Computern herstellen können, die für sie bestimmt sind. 

Derzeit wird die Remotedesktopverbindung für die folgenden Betriebssysteme unterstützt:

- openSUSE Leap 42.3
- 7.5 (CentOS-basiert)
- Debian 9 „Stretch“
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Herstellen einer Verbindung mit der Vorlage für virtuelle Computer per RDP durch Lehrkräfte
Lehrkräfte müssen zunächst per SSH eine Verbindung mit der Vorlage für virtuelle Computer herstellen und darauf RDP- und GUI-Pakete installieren. Anschließend können die Lehrkräfte die folgenden Schritte ausführen, um per RDP eine Verbindung mit den virtuellen Linux-Computern herzustellen: 

Die Option **Remotedesktop** zum Herstellen einer Verbindung mit der Vorlage für virtuelle Computer wird bei der Erstellung des Labs angezeigt. 

![Herstellen einer Verbindung mit der Vorlage per RDP während der Erstellung](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Die Option **Remotedesktop** wird auf der Startseite des Labs angezeigt, nachdem das Lab erstellt und die Vorlage für virtuelle Computer gestartet wurde. Starten Sie die Vorlage für virtuelle Computer, falls sie nicht bereits gestartet wurde. 

![Herstellen einer Verbindung mit der Vorlage per RDP nach der Lab-Erstellung](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Bei Auswahl der Option **RDP** wird eine RDP-Datei heruntergeladen. Sie öffnen die Datei, um eine Verbindung mit dem Linux-Computer herzustellen. 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Herstellen einer Verbindung mit einem virtuellen Computer für Kursteilnehmer per RDP
Ein Labbesitzer (Lehrkraft/Hochschullehrkraft) kann eine Verbindung mit einem virtuellen Computer für Kursteilnehmer herstellen, indem er zur Ansicht **Virtuelle Computer** wechselt und das Symbol **Verbinden** wählt. Vorher müssen Lehrkräfte das Vorlagenimage mit darauf installierten RDP- und GUI-Paketen **veröffentlichen**. 

![Herstellen einer Verbindung mit dem virtuellen Computer für Kursteilnehmer durch Lehrkräfte](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

## <a name="students-connecting-to-the-student-vm"></a>Herstellen einer Verbindung mit dem virtuellen Computer durch Kursteilnehmer
Kursteilnehmer können per RDP eine Verbindung mit ihren virtuellen Linux-Computern herstellen, nachdem der Labbesitzer (Lehrkraft/Hochschullehrkraft) die Vorlage für virtuelle Computer mit darauf installierten RDP- und GUI-Paketen **veröffentlicht** hat. Gehen Sie wie folgt vor: 

1. Wenn sich ein Kursteilnehmer direkt am Labs-Portal (`http://labs.azure.com`) anmeldet oder einen Registrierungslink (`http://labs.azure.com/register/<registrationCode>`) verwendet, wird eine Kachel für jedes Lab angezeigt, auf das der Kursteilnehmer Zugriff hat. 
2. Wählen Sie auf der Kachel die Option **Starten**, falls der virtuelle Computer angehalten ist. 
3. Wählen Sie **Verbinden**aus. Mit dieser Aktion wird die RDP-Datei auf Ihren Computer heruntergeladen. Speichern und öffnen Sie sie, um per RDP eine Verbindung mit dem Linux-Computer herzustellen. 

    ![Kursteilnehmer-VM – RDP-Download](../media/how-to-enable-remote-desktop-linux/student-rdp-download.png)

    Sie können weiterhin auch per SSH eine Verbindung mit dem virtuellen Linux-Computer herstellen. Wählen Sie die Auslassungspunkte (**...**), um die SSH-Option anzuzeigen. 
    
    ![Kursteilnehmer-VM – SSH](../media/how-to-enable-remote-desktop-linux/student-ssh.png)

    Kopieren und speichern Sie die SSH-Verbindungszeichenfolge im Dialogfeld **Eine Verbindung zu Ihrem virtuellen Computer herstellen**. Verwenden Sie diese Verbindungszeichenfolge über ein SSH-Terminal (z. B. [Putty](https://www.putty.org/)), um eine Verbindung mit dem virtuellen Computer herzustellen. 

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labkonten als Administrator](how-to-manage-lab-accounts.md)
- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)

