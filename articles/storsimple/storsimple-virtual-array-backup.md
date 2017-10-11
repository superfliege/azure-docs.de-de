---
title: Microsoft Azure StorSimple Virtual Array-backup-Lernprogramm | Microsoft Docs
description: Beschreibt, wie Sie StorSimple Virtual Array-Freigaben und Volumes sichern.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c926f0c80ce56cac3106ad97ec3ec2e18a8e2cc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>Sichern Sie Freigaben oder Volumes für Ihr virtuelles StorSimple-Array

## <a name="overview"></a>Übersicht

Das virtuelle StorSimple-Array ist ein Hybrid Cloud-Speicher auf lokale virtuelle Gerät, das als einen Dateiserver oder iSCSI-Server konfiguriert werden können. Die virtual Array kann der Benutzer zum Erstellen von geplanter und manueller Sicherungen aller Freigaben oder Volumes auf dem Gerät. Wenn als Dateiserver konfiguriert ist, können sie auch Wiederherstellung auf Elementebene aus. In diesem Lernprogramm wird beschrieben, wie geplante und manuelle Sicherungen erstellen und Ausführen der Wiederherstellung auf Elementebene zum Wiederherstellen einer gelöschten Datei auf Ihrem virtuellen Arrays ermöglicht wird.

Dieses Tutorial bezieht sich auf das virtuelle StorSimple-Arrays nur. Informationen zu 8000-Serie, wechseln Sie zu [erstellen Sie eine Sicherung für Gerät der 8000-Serie](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Sichern von Freigaben und volumes

Sicherungen Point-in-Time-Schutz bieten, verbessern die wiederherstellbarkeit und Minimierung der Wiederherstellungszeiten für Freigaben und Volumes. Sie können eine Freigabe oder ein Volume auf Sichern Ihres StorSimple-Geräts auf zwei Arten: **geplante** oder **manuell**. Jede der Methoden wird in den folgenden Abschnitten erläutert.

## <a name="change-the-backup-start-time"></a>Ändern Sie die backup-Startzeit

> [!NOTE]
> In dieser Version werden geplante Sicherungen durch eine Standardrichtlinie erstellt, die täglich zu einem bestimmten Zeitpunkt ausgeführt wird und sichert alle Freigaben oder Volumes auf dem Gerät. Es ist nicht möglich, benutzerdefinierte Richtlinien für geplante Sicherungen zu diesem Zeitpunkt zu erstellen.


Das virtuelle StorSimple-Array verfügt über eine Standard-backup-Richtlinie, die beginnt an einer angegebenen Uhrzeit (22:30) und sichert alle Freigaben oder Volumes auf dem Gerät einmal pro Tag. Sie können die Zeit ändern, an der die Sicherung gestartet wurde, aber die Häufigkeit und die Beibehaltungsdauer (die die Anzahl der Sicherungen beibehalten angibt) geändert werden können. Bei diesen Sicherungen wird das gesamte virtuelle Gerät gesichert. Dies kann potenziell Auswirkungen auf die Leistung des Geräts und Auswirkungen auf die arbeitsauslastungen auf dem Gerät bereitgestellt. Aus diesem Grund wird empfohlen, diese Sicherungen Spitzenzeiten zu planen.

 Um die Sicherung Standardstartzeit zu ändern, führen Sie die folgenden Schritte aus, in der [Azure-Portal](https://portal.azure.com/).

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>So ändern Sie die Startzeit für die Sicherung Standardrichtlinie

1. Wechseln Sie zu **Geräte**. Die Liste der Geräte bei Ihrem StorSimple-Geräte-Manager-Dienst registriert wird, werden angezeigt. 
   
    ![Navigieren Sie zu Geräten](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Wählen Sie aus, und klicken Sie auf Ihrem Gerät. Die **Einstellungen** Blatt wird angezeigt. Wechseln Sie zu **verwalten > Sicherungsrichtlinien**.
   
    ![Wählen Sie Ihr Gerät](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. In der **Sicherungsrichtlinien** Blatt die Standardstartzeit ist 22:30 Uhr fortgeführt. Sie können die neue Startzeit für den täglichen Zeitplan in der Zeitzone des Geräts angeben.
   
    ![Navigieren Sie zu backup-Richtlinien](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. Klicken Sie auf **Speichern**.

### <a name="take-a-manual-backup"></a>Erstellen einer manuellen Sicherung

Zusätzlich zu geplanten Sicherungen können Sie eine manuelle (bedarfsgesteuerten) Sicherung der Gerätedaten zu einem beliebigen Zeitpunkt ausführen.

#### <a name="to-create-a-manual-backup"></a>Zum Erstellen einer manuellen Sicherung

1. Wechseln Sie zu **Geräte**. Wählen Sie das Gerät, mit der Maustaste **...**  ganz rechts in der ausgewählten Zeile. Wählen Sie im Kontextmenü der **Sicherung**.
   
    ![Navigieren Sie zu der Sicherung erstellen](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. In der **Sicherung** Blatt, klicken Sie auf **Sicherung**. Auf diese Weise alle Freigaben auf dem Dateiserver oder alle Volumes auf dem iSCSI-Server sichern. 
   
    ![Sicherung starten](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    Startet eine bedarfsgesteuerte Sicherung, und Sie sehen, dass ein Sicherungsauftrag gestartet wurde.
   
    ![Sicherung starten](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    Wenn der Auftrag erfolgreich abgeschlossen wurde, werden Sie erneut benachrichtigt. Anschließend wird der Sicherungsvorgang gestartet.
   
    ![Der Sicherungsauftrag wurde erstellt](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. Zum Überwachen des Status von Sicherungen, und sehen Sie sich die Auftragsdetails, klicken Sie auf die Benachrichtigung. Dadurch gelangen Sie zu **Projektdetails**.
   
     ![Details des Sicherungsauftrags](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Nachdem die Sicherung abgeschlossen ist, navigieren Sie zu **Management > Sicherungskatalog**. Sie sehen eine Cloud-Momentaufnahme aller Freigaben (oder Datenträger) auf Ihrem Gerät.
   
    ![Vollständige Sicherung](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Zeigen Sie vorhandene Sicherungen an.
Um die vorhandenen Sicherungen anzuzeigen, führen Sie die folgenden Schritte im Azure-Portal ein.

#### <a name="to-view-existing-backups"></a>So zeigen Sie vorhandene Sicherungen an

1. Wechseln Sie zu **Geräte** Blatt. Wählen Sie aus, und klicken Sie auf Ihrem Gerät. In der **Einstellungen** wechseln Sie zum Blatt **Management > Sicherungskatalog**.
   
    ![Navigieren Sie zu Sicherungskatalog](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Geben Sie die folgenden Kriterien zum Filtern verwendet werden:
   
    - **Zeitraum** – kann **letzten 1 Stunde**, **vergangenen 24 Stunden**, **letzte 7 Tage**, **in den letzten 30 Tagen**, **vergangenen Jahre**, und **benutzerdefiniertes Datum**.
    
    - **Geräte** – wählen Sie aus der Liste der Dateiserver oder iSCSI-Servern, die bei Ihrem StorSimple-Geräte-Manager-Dienst registriert.
   
    - **Initiiert** – kann automatisch **geplante** (indem Sie eine Sicherungsrichtlinie) oder **manuell** initiiert (von Ihnen).
   
    ![Filtern von Sicherungen](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. Klicken Sie auf **Übernehmen**. Die gefilterte Liste von Sicherungen wird angezeigt, der **Sicherungskatalog** Blatt. Hinweis nur 100 backup Elemente können zu einem bestimmten Zeitpunkt angezeigt werden.
   
    ![Aktualisierte Sicherungskatalog](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [verwalten Ihr virtuelles StorSimple-Array](storsimple-ova-web-ui-admin.md).

