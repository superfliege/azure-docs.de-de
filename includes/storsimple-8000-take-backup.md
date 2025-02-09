---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 48a3326dbe0e9eed4a5490e720248555586d189c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66118239"
---
### <a name="to-take-a-backup"></a>So erstellen Sie eine Sicherung

1. Wechseln Sie zu Ihrem StorSimple-Geräte-Manager-Dienst. Wählen Sie in der tabellarischen Geräteauflistung Ihr Gerät aus, klicken Sie darauf, und klicken Sie dann auf **Alle Einstellungen**. Wechseln Sie auf dem Blatt **Einstellungen** zu **Einstellungen > Verwalten > Sicherungsrichtlinie**.

    ![Sicherungsrichtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu1.png)

2. Klicken Sie auf dem Blatt **Sicherungsrichtlinie** auf **+ Richtlinie hinzufügen**.

    ![Sicherungsrichtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu2.png)

3. Geben Sie auf dem Blatt **Sicherungsrichtlinie erstellen** einen Namen für Ihre Sicherungsrichtlinie mit 3 bis 150 Zeichen ein.

4. Wählen Sie die Volumes aus, die gesichert werden sollen. Wenn Sie mehrere Volumes auswählen, werden diese gruppiert, um eine absturzkonsistente Sicherung zu erstellen.

    ![Sicherungsrichtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu4.png)

5. Auf dem Blatt **Ersten Zeitplan hinzufügen**:

    1. Wählen Sie den Sicherungstyp aus. Wählen Sie für schnellere Wiederherstellungen **lokale** Momentaufnahme aus. Wählen Sie für Datenresilienz **Cloud**-Momentaufnahme aus.
    2. Geben Sie die Sicherungshäufigkeit in Minuten, Stunden, Tagen oder Wochen an.
    3. Wählen Sie eine Aufbewahrungsdauer aus. Die Aufbewahrungsdauer hängt von der Sicherungshäufigkeit ab. Für eine tägliche Richtlinie kann die Aufbewahrungsdauer z. B. in Wochen angegeben werden, während die Aufbewahrungsdauer für eine monatliche Richtlinie Monate beträgt.
    4. Wählen Sie die Startzeit und das Datum für die Richtlinie aus.
    5. Klicken Sie auf **OK**, um die Sicherungsrichtlinie zu erstellen.

        ![Sicherungsrichtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. Klicken Sie auf **Erstellen**, um die Erstellung der Sicherungsrichtlinie zu starten. Sie werden benachrichtigt, wenn die Sicherungsrichtlinie erfolgreich erstellt wurde. Die Liste der Sicherungsrichtlinien wird ebenfalls aktualisiert.
      
      ![Sicherungsrichtlinie hinzufügen](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      Sie verfügen nun über eine Sicherungsrichtlinie, die geplante Sicherungen für Ihre Volumedaten erstellt.




