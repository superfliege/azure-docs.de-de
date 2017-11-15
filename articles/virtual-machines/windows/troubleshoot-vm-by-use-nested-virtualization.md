---
title: Behandeln von Problemen mit einem virtuellen Azure-Computer unter Verwendung der geschachtelten Virtualisierung in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Probleme mit einem virtuellen Azure-Computer unter Verwendung der geschachtelten Virtualisierung in Azure behandeln.
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/06/2017
ms.author: genli
ms.openlocfilehash: 35f52af5fbf0c945a766f5e5431c885d91df546a
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="troubleshoot-a-problem-azure-vm-by-using-nested-virtualization-in-azure"></a>Behandeln von Problemen mit einem virtuellen Azure-Computer unter Verwendung der geschachtelten Virtualisierung in Azure

In diesem Artikel erfahren Sie, wie Sie in Microsoft Azure eine geschachtelte Virtualisierungsumgebung erstellen, um den Datenträger des virtuellen Computers zur Problembehandlung auf dem Hyper-V-Host (virtueller Wiederherstellungscomputer) einbinden zu können.

## <a name="prerequisite"></a>Voraussetzung

Um den virtuellen Computer mit dem Problem einbinden zu können, muss der virtuelle Wiederherstellungscomputer folgende Voraussetzungen erfüllen:

-   Er muss sich am gleichen Standort befinden wie der virtuelle Computer mit dem Problem.

-   Er muss sich in der gleichen Ressourcengruppe befinden wie der virtuelle Computer mit dem Problem.

-   Er muss die gleiche Art von Speicherkonto (Standard oder Premium) verwenden wie der virtuelle Computer mit dem Problem.

## <a name="step-1-create-a-recovery-vm-and-install-hyper-v-role"></a>Schritt 1: Erstellen eines virtuellen Wiederherstellungscomputers und Installieren der Hyper-V-Rolle

1.  Erstellen Sie einen neuen virtuellen Wiederherstellungscomputer:

    -  Betriebssystem: Windows Server 2016 Datacenter

    -  Größe: Beliebige V3-Serie mit mindestens zwei Kernen, die die geschachtelte Virtualisierung unterstützen. Weitere Informationen finden Sie unter [Introducing the new Dv3 and Ev3 VM sizes](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) (Vorstellung der neuen VM-Größen Dv3 und Ev3).

    -  Gleicher Standort, gleiches Speicherkonto und gleiche Ressourcengruppe wie der virtuelle Computer mit dem Problem

    -  Speichertyp: Gleicher Typ wie bei dem Computer mit dem Problem (Standard oder Premium)

2.  Stellen Sie nach dem Erstellen des virtuellen Wiederherstellungscomputers eine Remotedesktopverbindung mit ihm her.

3.  Klicken Sie im Server-Manager auf **Verwalten** > **Rollen und Features hinzufügen**.

4.  Wählen Sie im Abschnitt **Installationstyp** die Option **Rollenbasierte oder featurebasierte Installation** aus.

5.  Vergewissern Sie sich im Abschnitt **Zielserver auswählen**, dass der virtuelle Wiederherstellungscomputer ausgewählt ist.

6.  Klicken Sie auf **Hyper-V-Rolle** > **Features hinzufügen**.

7.  Klicken Sie im Abschnitt **Features** auf **Weiter**.

8.  Sollte ein virtueller Switch verfügbar sein, wählen Sie ihn aus. Klicken Sie andernfalls auf **Weiter**.

9.  Klicken Sie im Abschnitt **Migration** auf **Weiter**.

10. Klicken Sie im Abschnitt **Standardspeicher** auf **Weiter**.

11. Aktivieren Sie das Kontrollkästchen für den automatischen Neustart des Servers, sofern erforderlich.

12. Wählen Sie **Installieren** aus.

13. Lassen Sie die Installation der Hyper-V-Rolle auf dem Server zu. Dieser Vorgang dauert einige Minuten. Anschließend wird der Server automatisch neu gestartet.

## <a name="step-2-create-the-problem-vm-on-the-recovery-vms-hyper-v-server"></a>Schritt 2: Erstellen des virtuellen Computers mit dem Problem auf dem Hyper-V-Server des virtuellen Wiederherstellungscomputers

1.  Notieren Sie sich den Namen des Datenträgers des virtuellen Computers mit dem Problem, und löschen Sie anschließend den virtuellen Computer mit dem Problem. Bewahren Sie alle angefügten Datenträger auf. 

2.  Fügen Sie den Betriebssystemdatenträger des virtuellen Computers mit dem Problem als Datenträger des virtuellen Wiederherstellungscomputers an.

    1.  Navigieren Sie nach dem Löschen des virtuellen Computers mit dem Problem zum virtuellen Wiederherstellungscomputer.

    2.  Klicken Sie auf **Datenträger** und anschließend auf **Datenträger hinzufügen**.

    3.  Wählen Sie den Datenträger des virtuellen Computers mit dem Problem aus, und klicken Sie anschließend auf **Speichern**.

3.  Stellen Sie nach dem erfolgreichen Anfügen des Datenträgers eine Remotedesktopverbindung mit dem virtuellen Wiederherstellungscomputer her.

4.  Öffnen Sie die Datenträgerverwaltung (diskmgmt.msc). Vergewissern Sie sich, dass der Datenträger des virtuellen Computers mit dem Problem den Status **Offline** hat.

5.  Öffnen Sie den Hyper-V-Manager: Wählen Sie im **Server-Manager** die **Hyper-V-Rolle** aus. Klicken Sie mit der rechten Maustaste auf den Server, und wählen Sie den **Hyper-V Manager** aus.

6.  Klicken Sie im Hyper-V-Manager mit der rechten Maustaste auf den virtuellen Wiederherstellungscomputer, und klicken Sie auf **Neu** > **Virtueller Computer** > **Weiter**.

7.  Geben Sie einen Namen für den virtuellen Computer ein, und klicken Sie auf **Weiter**.

8.  Wählen Sie **Generation 1** aus.

9.  Legen Sie den Startspeicher auf mindestens 1.024 MB fest.

10. Wählen Sie ggf. den erstellten Hyper-V-Netzwerkswitch aus. Navigieren Sie andernfalls zur nächsten Seite.

11. Wählen Sie **Virtuelle Festplatte später zuordnen** aus.

    ![Abbildung mit der Option „Virtuelle Festplatte später zuordnen“](./media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

12. Klicken Sie auf **Fertig stellen**, wenn der virtuelle Computer erstellt wurde.

13. Klicken Sie mit der rechten Maustaste auf den erstellten virtuellen Computer, und klicken Sie auf **Einstellungen**.

14. Klicken Sie auf **IDE-Controller 0** > **Festplatte** > **Hinzufügen**.

    ![Abbildung zum Hinzufügen der neuen Festplatte](./media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

15. Wählen Sie unter **Physische Festplatte** den Datenträger des virtuellen Computers mit dem Problem aus, den Sie an den virtuellen Azure-Computer angefügt haben. Sollten keine Datenträger aufgeführt werden, überprüfen Sie mithilfe der Datenträgerverwaltung, ob der Datenträger auf „Offline“ festgelegt ist.

    ![Abbildung zum Einbinden des Datenträgers](./media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


17. Klicken Sie auf **Apply** (Anwenden) und dann auf **OK**.

18. Doppelklicken Sie auf den virtuellen Computer, und starten Sie ihn.

19. Der virtuelle Computer kann nun als lokaler virtueller Computer verwendet werden. Sie können beliebige Problembehandlungsschritte ausführen.

## <a name="step-3-re-create-your-azure-vm-in-azure"></a>Schritt 3: Neuerstellen des virtuellen Azure-Computers in Azure

1.  Wenn Sie den virtuellen Computer wieder online geschaltet haben, fahren Sie den virtuellen Computer im Hyper-V-Manager herunter.

2.  Klicken Sie im [Azure-Portal](https://portal.azure.com) auf den virtuellen Wiederherstellungscomputer und anschließend auf „Datenträger“. Kopieren Sie dann den Namen des Datenträgers. Der Name wird im nächsten Schritt benötigt. Trennen Sie den eingebauten Datenträger vom virtuellen Wiederherstellungscomputer.

3.  Navigieren Sie zu **Alle Ressourcen**, suchen Sie nach dem Namen des Datenträgers, und wählen Sie den Datenträger aus.

     ![Abbildung zur Suche nach dem Datenträger](./media/troubleshoot-vm-by-use-nested-virtualization/search-disk.png)     

4. Klicken Sie auf **Virtuellen Computer erstellen**.

     ![Abbildung zur Erstellung des virtuellen Computers auf der Grundlage des Datenträgers](./media/troubleshoot-vm-by-use-nested-virtualization/create-vm-from-vhd.png) 

Der virtuelle Computer kann auch mithilfe von Azure PowerShell auf der Grundlage des Datenträgers erstellt werden. Weitere Informationen finden Sie unter [Erstellen des neuen virtuellen Computers](create-vm-specialized.md#create-the-new-vm). 

## <a name="next-steps"></a>Nächste Schritte

Wenn Probleme beim Herstellen einer Verbindung mit Ihrer VM auftreten, helfen Ihnen die Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](troubleshoot-rdp-connection.md) weiter. Konsultieren Sie [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](troubleshoot-app-connection.md) bei Problemen mit dem Zugriff auf Anwendungen, die auf Ihrer VM ausgeführt werden.
