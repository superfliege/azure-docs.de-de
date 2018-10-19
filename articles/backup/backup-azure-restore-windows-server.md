---
title: Wiederherstellen von Daten in Azure auf einem Windows Server- oder Windows-Computer
description: Es wird beschrieben, wie Sie in Azure gespeicherte Daten auf einem Windows Server- oder Windows-Computer wiederherstellen.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 9/7/2018
ms.author: saurse
ms.openlocfilehash: 20d2f289f4d40d773fde9f6b770dc49b87c34804
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297246"
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Wiederherstellen von Dateien auf einem Windows-Server- oder Windows-Clientcomputer mit dem Resource Manager-Bereitstellungsmodell

In diesem Artikel wird das Wiederherstellen von Daten aus einem Sicherungstresor erläutert. Zum Wiederherstellen von Daten verwenden Sie den Assistenten zum Wiederherstellen von Daten im Microsoft Azure Recovery Services-Agent (MARS). Bei der Datenwiederherstellung ist Folgendes möglich:

* Wiederherstellen von Daten auf dem gleichen Computer, auf dem die Sicherungen erstellt wurden
* Wiederherstellen auf einem anderen Computer.

Im Januar 2017 hat Microsoft ein Vorschau-Update für den MARS-Agent veröffentlicht. Dieses Update bietet Fehlerbehebungen sowie eine sofortige Wiederherstellung, wodurch Sie eine Wiederherstellungspunkt-Momentaufnahme mit Schreibzugriff als Wiederherstellungsvolume bereitstellen können. Sie können dann das Wiederherstellungsvolume durchsuchen und Dateien auf einen lokalen Computer kopieren, wodurch Sie Dateien selektiv wiederherstellen können.

> [!NOTE]
> Das [Azure Backup-Update vom Januar 2017](https://support.microsoft.com/en-us/help/3216528?preview) ist erforderlich, wenn Sie die sofortige Wiederherstellung verwenden möchten, um Daten wiederherzustellen. Außerdem müssen die Sicherungsdaten in Tresoren an einem der Orte geschützt sein, die im Supportartikel aufgeführt sind. Unter [Azure Backup-Update vom Januar 2017](https://support.microsoft.com/en-us/help/3216528?preview) finden Sie die neueste Liste der Regionen, in denen die sofortige Wiederherstellung unterstützt wird. Die sofortige Wiederherstellung ist derzeit **nicht** in allen Regionen verfügbar.
>

Verwenden Sie im Azure-Portal die sofortige Wiederherstellung mit Recovery Services-Tresoren. Wenn Sie Daten in Sicherungstresoren gespeichert haben, wurden sie automatisch in Recovery Services-Tresore geändert. Wenn Sie die sofortige Wiederherstellung einsetzen möchten, laden Sie das MARS-Update herunter und befolgen Sie die Verfahren, in denen die sofortige Wiederherstellung erwähnt wird.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Verwenden der sofortigen Wiederherstellung zum Wiederherstellen von Daten auf demselben Computer

Wenn Sie versehentlich eine Datei gelöscht haben und sie auf demselben Computer wiederherstellen möchten (auf dem die Sicherung erstellt wurde), können Sie die folgenden Schritte zum Wiederherstellen der Daten ausführen.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** . Falls Sie nicht wissen, wo das Snap-In installiert wurde, können Sie den für **Microsoft Azure Backup** verwendeten Computer oder Server durchsuchen.

    Die Desktop-App sollten in den Suchergebnissen angezeigt werden.

2. Klicken Sie auf **Daten wiederherstellen**, um den Assistenten zu starten.

    ![Daten wiederherstellen](./media/backup-azure-restore-windows-server/recover.png)

3. Wählen Sie im Bereich **Erste Schritte** die Option **Dieser Server (`<server name>`)**, um die Daten auf demselben Server oder Computer wiederherzustellen, und klicken Sie auf **Weiter**.

    ![Auswählen der Option „Dieser Server“ zum Wiederherstellen der Daten auf demselben Computer](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Wählen Sie im Bereich **Wiederherstellungsmodus auswählen** die Option **Einzelne Dateien und Ordner**, und klicken Sie auf **Weiter**.

    ![Dateien durchsuchen](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)
> [!IMPORTANT]
> Für die Option zum Wiederherstellen von *einzelnen Dateien und Ordnern* wird .NET Framework 4.5.2 oder höher benötigt. Falls die Option *Individual Files and Folders* (Einzelne Dateien und Ordner) nicht angezeigt wird, müssen Sie .NET Framework auf Version 4.5.2 oder höher aktualisieren und es erneut versuchen.

> [!TIP]
> Die Option *Einzelne Dateien und Ordner* ermöglicht den schnellen Zugriff auf Daten des Wiederherstellungspunkts. Sie eignet sich für die Wiederherstellung einzelner Dateien mit Größen bis maximal 80 GB und bietet dabei Übertragungs-/Kopiergeschwindigkeiten von bis zu 6 MB/s während der Wiederherstellung. Die Option *Volume* stellt alle gesicherten Daten auf einem bestimmten Volume wieder her. Mit dieser Option erreichen Sie höhere Übertragungsgeschwindigkeiten (bis zu 60 MB/s), sodass sie sich ideal für das Wiederherstellen großer Daten oder ganzer Volumes eignet.

5. Wählen Sie im Bereich **Volume und Datum auswählen** das Volume aus, dass die wiederherzustellenden Dateien bzw. Ordner enthält.

    Wählen Sie im Kalender einen Wiederherstellungspunkt aus. Sie können den Zustand eines beliebigen Wiederherstellungszeitpunkts wiederherstellen. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte verfügbar sind, müssen Sie nach dem Auswählen eines Datums im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt auswählen.

    ![Volume und Datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Klicken Sie auf **Bereitstellen**, nachdem Sie den Wiederherstellungspunkt ausgewählt haben, der wiederhergestellt werden soll.

    Azure Backup stellt den lokalen Wiederherstellungspunkt bereit und verwendet ihn als Wiederherstellungsvolume.

7. Klicken Sie im Bereich **Dateien suchen und wiederherstellen** auf **Durchsuchen**, um Windows-Explorer zu öffnen und nach den gewünschten Dateien und Ordnern zu suchen.

    ![Wiederherstellungsoptionen](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. Kopieren Sie in Windows-Explorer die wiederherzustellenden Dateien bzw. Ordner, und fügen Sie sie an einem lokalen Speicherort auf dem Server oder Computer ein. Sie können die Dateien direkt vom Wiederherstellungsvolume öffnen oder streamen und überprüfen, ob Sie die richtigen Versionen wiederherstellen.

    ![Kopieren und Einfügen von Dateien und Ordnern vom bereitgestellten Volume am lokalen Speicherort](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)


9. Klicken Sie im Bereich **Dateien suchen und wiederherstellen** auf **Bereitstellung aufheben**, wenn Sie die Wiederherstellung der Dateien bzw. Ordner abgeschlossen haben. Klicken Sie anschließend auf **Ja**, um zu bestätigen, dass Sie die Bereitstellung des Volumes aufheben möchten.

    ![Aufheben der Bereitstellung des Volumes und Bestätigen des Vorgangs](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Wenn Sie nicht auf „Bereitstellung aufheben“ klicken, bleibt die Bereitstellung des Wiederherstellungsvolumes ab dem Bereitstellungszeitpunkt sechs Stunden lang bestehen. Im Falle eines laufenden Dateikopiervorgangs wird die Bereitstellungszeit jedoch auf bis zu 24 Stunden verlängert. Während der Bereitstellung des Volumes werden keine Sicherungsvorgänge durchgeführt. Alle Sicherungsvorgänge, deren Ausführung für den Zeitraum der Volumebereitstellung geplant ist, werden ausgeführt, nachdem die Bereitstellung des Wiederherstellungsvolumes aufgehoben wurde.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Verwenden der sofortigen Wiederherstellung zum Wiederherstellen von Daten auf einem anderen Computer
Wenn der gesamte Server verloren geht, können Sie Daten aus Azure Backup dennoch auf einem anderen Computer wiederherstellen. Der Workflow wird in den folgenden Schritten beschrieben.


In diesen Schritten wird folgende Terminologie verwendet:

* *Quellcomputer* – Der ursprüngliche Computer, von dem die Sicherung erstellt wurde und der zurzeit nicht verfügbar ist.
* *Zielcomputer* – Der Computer, auf dem die Daten wiederhergestellt werden.
* *Beispieltresor* – Der Recovery Services-Tresor, bei dem der *Quellcomputer* und der *Zielcomputer* registriert sind. <br/>

> [!NOTE]
> Sicherungen können nicht auf einem Zielcomputer wiederhergestellt werden, auf dem eine ältere Version des Betriebssystems ausgeführt wird. Beispielsweise kann eine Sicherung, die von einem Windows 7-Computer erstellt wurde, auf einem Windows 8-Computer (oder höher) wiederhergestellt werden. Es ist aber nicht möglich, eine Sicherung, die von einem Windows 8-Computer erstellt wurde, auf einem Windows 7-Computer wiederherzustellen.
>
>

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** auf dem *Zielcomputer*.

2. Stellen Sie sicher, dass der *Zielcomputer* und der *Quellcomputer* bei demselben Recovery Services-Tresor registriert sind.

3. Klicken Sie auf **Daten wiederherstellen**, um den **Assistenten zum Wiederherstellen von Daten** zu öffnen.

    ![Daten wiederherstellen](./media/backup-azure-restore-windows-server/recover.png)

4. Wählen Sie im Bereich **Erste Schritte** die Option **Anderer Server**.

    ![Anderer Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Geben Sie die Datei mit den Tresoranmeldeinformationen an, die dem *Beispieltresor* entspricht, und klicken Sie auf **Weiter**.

    Wenn die Datei mit den Tresoranmeldeinformationen ungültig (oder abgelaufen) ist, laden Sie eine neue Anmeldeinformationsdatei für den Tresor aus dem *Beispieltresor* im Azure-Portal herunter. Nachdem Sie gültige Tresoranmeldeinformationen angegeben haben, wird der Name des entsprechenden Sicherungstresors angezeigt.


6. Wählen Sie im Bereich **Sicherungsserver auswählen** in der Liste mit den angezeigten Computern den *Quellcomputer* aus, und geben Sie die Passphrase an. Klicken Sie auf **Weiter**.

    ![Liste der Computer](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Wählen Sie im Bereich **Wiederherstellungsmodus auswählen** die Option **Einzelne Dateien und Ordner**, und klicken Sie auf **Weiter**.

    ![Suchen,](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Wählen Sie im Bereich **Volume und Datum auswählen** das Volume aus, dass die wiederherzustellenden Dateien bzw. Ordner enthält.

    Wählen Sie im Kalender einen Wiederherstellungspunkt aus. Sie können den Zustand eines beliebigen Wiederherstellungszeitpunkts wiederherstellen. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte verfügbar sind, müssen Sie nach dem Auswählen eines Datums im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt auswählen.

    ![Elemente suchen](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Klicken Sie auf **Bereitstellen**, um den Wiederherstellungspunkt lokal als Wiederherstellungsvolume auf Ihrem *Zielcomputer* bereitzustellen.

10. Klicken Sie im Bereich **Dateien suchen und wiederherstellen** auf **Durchsuchen**, um Windows-Explorer zu öffnen und nach den gewünschten Dateien und Ordnern zu suchen.

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. Kopieren Sie in Windows-Explorer die Dateien bzw. Ordner vom Wiederherstellungsvolume, und fügen Sie sie am Speicherort auf Ihrem *Zielcomputer* ein. Sie können die Dateien direkt vom Wiederherstellungsvolume öffnen oder streamen und überprüfen, ob die richtigen Versionen wiederhergestellt werden.

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Klicken Sie im Bereich **Dateien suchen und wiederherstellen** auf **Bereitstellung aufheben**, wenn Sie die Wiederherstellung der Dateien bzw. Ordner abgeschlossen haben. Klicken Sie anschließend auf **Ja**, um zu bestätigen, dass Sie die Bereitstellung des Volumes aufheben möchten.

    ![Verschlüsselung](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Wenn Sie nicht auf „Bereitstellung aufheben“ klicken, bleibt die Bereitstellung des Wiederherstellungsvolumes ab dem Bereitstellungszeitpunkt sechs Stunden lang bestehen. Im Falle eines laufenden Dateikopiervorgangs wird die Bereitstellungszeit jedoch auf bis zu 24 Stunden verlängert. Während der Bereitstellung des Volumes werden keine Sicherungsvorgänge durchgeführt. Alle Sicherungsvorgänge, deren Ausführung für den Zeitraum der Volumebereitstellung geplant ist, werden ausgeführt, nachdem die Bereitstellung des Wiederherstellungsvolumes aufgehoben wurde.
    >

## <a name="next-steps"></a>Nächste Schritte
* Nachdem Sie nun Ihre Dateien und Ordner wiederhergestellt haben, können Sie [Ihre Sicherungen verwalten](backup-azure-manage-windows-server.md).
