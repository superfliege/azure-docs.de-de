---
title: Anwenden von Updates in Azure Stack | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie Microsoft-Updates für ein integriertes Azure Stack-System importieren und installieren."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 8fbed3c15b385e385e64093331df37363a558185
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="apply-updates-in-azure-stack"></a>Anwenden von Updates in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme*

Als Azure Stack-Betreiber können Sie im Administratorportal über die Kachel „Aktualisieren“ Microsoft-Updatepakete für Azure Stack anwenden. Sie müssen das Microsoft-Updatepaket herunterladen, die Paketdateien in Azure Stack importieren und das Update dann installieren. 

## <a name="download-the-update-package"></a>Herunterladen des Updates

Wenn ein Microsoft-Updatepaket für Azure Stack verfügbar ist, laden Sie das Paket an einen Speicherort herunter, auf den von Azure Stack aus zugegriffen werden kann. Überprüfen Sie den Paketinhalt. Ein Updatepaket besteht in der Regel aus folgenden Dateien:

- Einer selbstextrahierenden EXE-Datei mit folgendem Benennungsmuster: *Paketname.exe*. Diese Datei enthält die Nutzlast für das Update, beispielsweise das aktuelle kumulative Update für Windows Server.   
- Entsprechenden BIN-Dateien mit folgendem Benennungsmuster: *Paketname.bin*. Diese Dateien dienen zum Komprimieren der Nutzlast, die der Datei *Paketname.exe* zugeordnet ist. 
- Der Datei „Metadata.xml“. Diese Datei enthält wichtige Informationen zum Update, etwa Herausgeber, Name, Voraussetzungen, Größe und Supportpfad-URL.

## <a name="import-and-install-updates"></a>Importieren und Installieren von Updates

Das folgende Verfahren zeigt, wie Updatepakete im Administratorportal importiert und installiert werden.

1. Klicken Sie im Administratorportal auf **Weitere Dienste**. Klicken Sie anschließend in der Kategorie **Daten und Speicher** auf **Speicherkonten**. (Oder beginnen Sie im Filterfeld mit der Eingabe von **Speicherkonten**, und wählen Sie dann die entsprechende Option aus.)

    ![Hier sehen Sie, wo Sie Speicherkonten im Portal finden.](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Geben Sie in das Filterfeld **update** ein, und wählen Sie das Speicherkonto **updateadminaccount**.

    ![Hier sehen Sie, wie Sie nach „updateadminaccount“ suchen.](media/azure-stack-apply-updates/ApplyUpdates2.png)

3. Klicken Sie in den Speicherkontodetails unter **Dienste** auf **Blobs**.
 
    ![Hier sehen Sie, wie Sie „Blobs“ für das Speicherkonto auswählen.](media/azure-stack-apply-updates/ApplyUpdates3.png) 
 
4. Klicken Sie unter **Blob-Dienst** auf **+ Container**, um einen Container zu erstellen. Geben Sie einen Namen (etwa *Update-1709*) ein, und klicken Sie dann auf **OK**.
 
     ![Hier sehen Sie, wie Sie einen Container im Speicherkonto hinzufügen.](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Klicken Sie nach dem Erstellen des Containers auf den Containernamen und dann auf **Hochladen**, um die Paketdateien in den Container hochzuladen.
 
    ![Hier sehen Sie, wie Sie die Paketdateien hochladen.](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. Klicken Sie unter **Blob hochladen** auf das Ordnersymbol, navigieren Sie zur EXE-Datei des Updatepakets, und klicken Sie dann im Explorer-Fenster auf **Öffnen**.
  
7. Klicken Sie unter **Blob hochladen** auf **Hochladen**. 
 
    ![Hier sehen Sie, wo die einzelnen Paketdateien hochgeladen werden.](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. Wiederholen Sie die Schritte 6 und 7 für die Dateien *Paketname.bin* und „Metadata.xml“. 
9. Anschließend können Sie die Benachrichtigungen (Glockensymbol in der oberen rechten Ecke des Portals) überprüfen. In den Benachrichtigungen sollte angegeben sein, dass der Upload abgeschlossen ist. 
10. Kehren Sie zur Kachel „Aktualisieren“ auf dem Dashboard zurück. Auf der Kachel sollte angegeben sein, dass ein Update verfügbar ist. Klicken Sie auf die Kachel, um das neu hinzugefügte Updatepaket zu überprüfen.
11. Um das Update zu installieren, wählen Sie das als **Bereit** markierte Paket aus. Klicken Sie dann entweder mit der rechten Maustaste auf das Paket, und wählen Sie **Jetzt aktualisieren**, oder klicken Sie im oberen Bereich auf die Aktion **Jetzt aktualisieren**.
12. Wenn Sie auf „Update wird installiert...“ klicken, können Sie den Status im Bereich **Update run details** (Updateausführungsdetails) anzeigen. Hier können Sie auch auf **Download full logs** (Vollständige Protokolle herunterladen) klicken, um die Protokolldateien herunterzuladen.
13. Wenn das Update abgeschlossen ist, wird auf der Kachel „Aktualisieren“ die aktualisierte Azure Stack-Version angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Stack-Wartungsrichtlinie](azure-stack-servicing-policy.md)
