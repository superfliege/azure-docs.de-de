---
title: 'Azure Data Box Gateway: Verwalten von Benutzern | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie über das Azure-Portal Bandbreitenzeitpläne auf Ihrer Azure Data Box Gateway-Ressource verwalten.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/09/2018
ms.author: alkohli
ms.openlocfilehash: d1049ef66ad02987c6523e4b5ec5dd9959e0fa47
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49070360"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-data-box-gateway"></a>Verwalten von Bandbreitenzeitplänen auf Ihrer Azure Data Box Gateway-Ressource über das Azure-Portal  

In diesem Artikel erfahren Sie, wie Sie Benutzer auf Ihrer Azure Data Box Gateway-Ressource verwalten. Mithilfe von Bandbreitenzeitplänen können Sie die Nutzung der Netzwerkbandbreite durch mehrere tageszeitbasierte Zeitpläne konfigurieren. Diese Zeitpläne können auf die Upload- und Downloadvorgänge zwischen Ihrem Gerät und der Cloud angewendet werden. 

Sie können die Bandbreitenzeitpläne für Data Box Gateway über das Azure-Portal hinzufügen, ändern oder löschen.

> [!IMPORTANT]
> - Data Box Gateway ist in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Hinzufügen eines Zeitplans
> * Ändern eines Zeitplans
> * Löschen eines Zeitplans 


## <a name="add-a-schedule"></a>Hinzufügen eines Zeitplans

Gehen Sie im Azure-Portal wie folgt vor, um einen Benutzer hinzuzufügen:

1. Navigieren Sie im Azure-Portal für Ihre Data Box Gateway-Ressource zu **Bandbreite**.
2. Klicken Sie im rechten Bereich auf **+ Zeitplan hinzufügen**.

    ![Klicken auf „Benutzer hinzufügen“](media/data-box-gateway-manage-bandwidth-schedules/add-schedule-1.png)

3. Gehen Sie unter **Zeitplan hinzufügen** wie folgt vor: 

    1. Geben Sie den **Starttag**, den **Endtag**, die **Startzeit** und die **Endzeit** des Zeitplans an. 
    2. Wenn der Zeitplan für den gesamten Tag gilt, können Sie die Option **Ganztägig** aktivieren. 
    3. **Bandbreitenrate** ist die Bandbreite in Megabit pro Sekunde (MBit/s), die Ihr Gerät bei Cloudvorgängen (Uploads und Downloads) verwendet. Geben Sie eine Zahl zwischen 1 und 1000 in dieses Feld ein. 
    4. Aktivieren Sie die Option **Unbegrenzt**, wenn Sie Uploads und Downloads nicht drosseln möchten. 
    5. Klicken Sie auf **Hinzufügen**.

    ![Klicken auf „Benutzer hinzufügen“](media/data-box-gateway-manage-bandwidth-schedules/add-schedule-2.png)

3. Ein Zeitplan mit den angegebenen Parametern wird erstellt. Dieser Zeitplan wird dann im Portal in der Liste mit den Bandbreitenzeitplänen angezeigt.


## <a name="edit-schedule"></a>Bearbeiten des Zeitplans

Führen Sie die folgenden Schritte aus, um einen Bandbreitenzeitplan zu bearbeiten: 

1. Navigieren Sie im Azure-Portal zu Ihrer Data Box Gateway-Ressource und anschließend zu „Bandbreite“. 
2. Klicken Sie in der Liste mit den Bandbreitenzeitplänen auf einen Zeitplan, den Sie ändern möchten.
    ![Ändern eines Benutzers](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-1.png)

3. Nehmen Sie die gewünschten Änderungen vor, und speichern Sie sie.

    ![Ändern eines Benutzers](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-2.png)

4. Nach der Änderung des Zeitplans wird die Liste mit den Zeitplänen aktualisiert, um den geänderten Zeitplan widerzuspiegeln.

    ![Ändern eines Benutzers](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Löschen eines Zeitplans

Führen Sie die folgenden Schritte aus, um einen mit Ihrem Data Box Gateway-Gerät verknüpften Bandbreitenzeitplan zu löschen:

1. Navigieren Sie im Azure-Portal zu Ihrer Data Box Gateway-Ressource und anschließend zu **Bandbreite**.  

2. Wählen Sie in der Liste mit den Bandbreitenzeitplänen einen Zeitplan aus, den Sie löschen möchten. Klicken Sie mit der rechten Maustaste, um das Kontextmenü zu öffnen, und klicken Sie anschließend auf **Löschen**. 

   ![Löschen eines Benutzers](media/data-box-gateway-manage-bandwidth-schedules/delete-schedule-1.png)

3.  Nach der Löschung des Zeitplans wird die Liste mit den Zeitplänen aktualisiert.



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Bandbreite verwalten](data-box-gateway-manage-bandwidth-schedules.md).
