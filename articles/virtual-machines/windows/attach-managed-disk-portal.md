---
title: "Anfügen eines verwalteten Datenträgers an einen virtuellen Windows-Computer – Azure | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zum Anfügen eines neuen verwalteten Datenträgers an einen virtuellen Windows-Computer im Azure-Portal mit dem Resource Manager-Bereitstellungsmodell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: 1c57ea841080fd87c7014e4e4520f17ccf15b156
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-attach-a-managed-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Anfügen eines verwalteten Datenträgers an eine Windows-VM im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal einen neuen verwalteten Datenträger an einen virtuellen Windows-Computer anfügen können. Lesen Sie die folgenden Tipps, bevor Sie mit diesem Vorgang fortfahren:

* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](sizes.md).
* Neue Datenträger werden von Azure beim Anfügen automatisch erstellt. Sie müssen den Datenträger also nicht manuell erstellen.

Sie können auch [einen Datenträger mithilfe von PowerShell anfügen](attach-disk-ps.md).



## <a name="add-a-data-disk"></a>Hinzufügen eines Datenträgers
1. Klicken Sie im Menü auf der linken Seite auf **Virtuelle Computer**.
2. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
3. Klicken Sie auf der Seite des virtuellen Computers auf **Datenträger**.
4. Klicken Sie auf der Seite **Datenträger** auf **+ Datenträger hinzufügen**.
5. Wählen Sie in der Dropdownliste für den neuen Datenträger **Datenträger erstellen** aus.
6. Geben Sie auf der Seite **Verwalteten Datenträger erstellen** einen Namen für den Datenträger ein, und passen Sie die anderen Einstellungen nach Bedarf an. Wenn Sie fertig sind, klicken Sie auf **Erstellen**.
7. Klicken Sie auf der Seite **Datenträger** auf **Speichern**, um die neue Datenträgerkonfiguration für den virtuellen Computer zu speichern.
6. Nachdem der Datenträger von Azure erstellt und an den virtuellen Computer angefügt wurde, wird der neue Datenträger in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger** angezeigt.


## <a name="initialize-a-new-data-disk"></a>Initialisieren eines neuen Datenträgers

1. Stellen Sie eine Verbindung mit der VM her.
1. Klicken Sie im virtuellen Computer auf **Start**, geben Sie **diskmgmt.msc** ein, und drücken Sie die EINGABETASTE. Das Snap-In für die Datenträgerverwaltung wird geöffnet.
2. Die Datenträgerverwaltung erkennt, dass es einen neuen nicht initialisierten Datenträger gibt, weshalb das Fenster **Datenträger initialisieren** eingeblendet wird.
3. Stellen Sie sicher, dass der neue Datenträger ausgewählt ist, und klicken Sie auf **OK**, um ihn zu initialisieren.
4. Der neue Datenträger wird als **Nicht zugeordnet** angezeigt. Klicken Sie mit der rechten Maustaste auf den Datenträger, und wählen **Neues einfaches Volume** aus. Der **Assistent zum Erstellen neuer einfacher Volumes** wird geöffnet.
5. Durchlaufen Sie den Assistenten, und behalten alle Standardeinstellungen bei. Klicken Sie abschließend auf **Fertig stellen**.
6. Schließen Sie die Datenträgerverwaltung.
7. In einem Popupfenster werden Sie informiert, dass Sie den neuen Datenträger formatieren müssen, ehe Sie ihn verwenden können. Klicken Sie auf **Datenträger formatieren**.
8. Überprüfen Sie im Dialogfeld **Neuen Datenträger formatieren** die Einstellungen, und klicken Sie dann auf **Starten**.
9. Sie erhalten die Warnung, dass bei Formatierung des Datenträgers alle Daten gelöscht werden. Klicken Sie auf **OK**.
10. Wenn die Formatierung abgeschlossen ist, klicken Sie auf **OK**.

## <a name="use-trim-with-standard-storage"></a>Verwenden von TRIM mit dem Standardspeicher

Wenn Sie den Standardspeicher (HDD) verwenden, sollten Sie TRIM aktivieren. Mit TRIM werden ungenutzte Blöcke auf dem Datenträger verworfen, sodass nur der Speicher in Rechnung gestellt wird, den Sie tatsächlich verwenden. Dies kann Kosten sparen, wenn Sie große Dateien erstellen und dann löschen. 

Sie können den folgenden Befehl ausführen, um die TRIM-Einstellung zu überprüfen. Öffnen Sie eine Eingabeaufforderung auf dem virtuellen Windows-Computer, und geben Sie Folgendes ein:

```
fsutil behavior query DisableDeleteNotify
```

Wenn „0“ zurückgegeben wird, ist TRIM ordnungsgemäß aktiviert. Wenn „1“ zurückgegeben wird, führen Sie den folgenden Befehl aus, um TRIM zu aktivieren:
```
fsutil behavior set DisableDeleteNotify 0
```

Nach dem Löschen von Daten vom Datenträger können Sie sicherstellen, dass die TRIM-Vorgänge ordnungsgemäß geleert werden, indem Sie die Defragmentierung mit TRIM ausführen:

```
defrag.exe <volume:> -l
```

Sie können auch sicherstellen, dass das gesamte Volume gekürzt wird, indem Sie das Volume formatieren.

## <a name="next-steps"></a>Nächste Schritte
Wenn Ihre Anwendung das Laufwerk „D:“ für die Datenspeicherung verwenden muss, können Sie [den Laufwerkbuchstaben des temporären Windows-Datenträgers ändern](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
