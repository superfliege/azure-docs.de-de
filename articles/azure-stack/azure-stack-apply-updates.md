---
title: Anwenden von Updates in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Microsoft-Updates für ein integriertes Azure Stack-System importieren und installieren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: ff70b1f8eda7bd48f0fba25aa66db42c15acab54
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252131"
---
# <a name="apply-updates-in-azure-stack"></a>Anwenden von Updates in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme*

Sie können im Verwaltungsportal über die Kachel **Aktualisieren** Microsoft- oder OEM-Updatepakete für Azure Stack anwenden. Sie müssen das Updatepaket herunterladen, die Paketdateien in Azure Stack importieren und das Update dann installieren.

## <a name="download-the-update-package"></a>Herunterladen des Updates

Wenn ein Microsoft- oder OEM-Updatepaket für Azure Stack verfügbar ist, laden Sie das Paket an einen Speicherort herunter, auf den von Azure Stack aus zugegriffen werden kann. Überprüfen Sie den Paketinhalt. Ein Updatepaket besteht in der Regel aus folgenden Dateien:

- Eine selbstextrahierende `<PackageName>.exe`-Datei. Diese Datei enthält die Nutzlast für das Update, beispielsweise das aktuelle kumulative Update für Windows Server.

- Entsprechende `<PackageName>.bin`-Dateien. Diese Dateien dienen zum Komprimieren der Nutzlast, die der Datei *Paketname.exe* zugeordnet ist.

- Eine `Metadata.xml`-Datei. Diese Datei enthält wichtige Informationen zum Update, etwa Herausgeber, Name, Voraussetzungen, Größe und Supportpfad-URL.

## <a name="import-and-install-updates"></a>Importieren und Installieren von Updates

Das folgende Verfahren zeigt, wie Updatepakete im Administratorportal importiert und installiert werden.

> [!IMPORTANT]  
> Es wird dringend empfohlen, dass Sie die Benutzer über alle Wartungsvorgänge unterrichten und normale Wartungsfenster so weit wie möglich außerhalb der Geschäftszeiten planen. Wartungsvorgänge können sowohl Benutzerworkloads als auch Portalvorgänge beeinträchtigen.

1. Wählen Sie im Administratorportal **Alle Dienste** aus. Wählen Sie anschließend in der Kategorie **Daten und Speicher** die Option **Speicherkonten** aus. (Oder beginnen Sie im Filterfeld mit der Eingabe von **Speicherkonten**, und wählen Sie dann die entsprechende Option aus.)

    ![Hier sehen Sie, wo Sie Speicherkonten im Portal finden.](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Geben Sie in das Filterfeld **update** ein, und wählen Sie das Speicherkonto **updateadminaccount**.

3. Klicken Sie in den Speicherkontodetails unter **Dienste** auf **Blobs**.
 
    ![Hier sehen Sie, wie Sie „Blobs“ für das Speicherkonto auswählen.](media/azure-stack-apply-updates/ApplyUpdates3.png) 

4. Klicken Sie unter **Blob-Dienst** auf **+ Container**, um einen Container zu erstellen. Geben Sie einen Namen (etwa *Update-1811*) ein, und wählen Sie dann **OK**.
 
     ![Hier sehen Sie, wie Sie einen Container im Speicherkonto hinzufügen.](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Klicken Sie nach dem Erstellen des Containers auf den Containernamen und dann auf **Hochladen**, um die Paketdateien in den Container hochzuladen.
 
    ![Hier sehen Sie, wie Sie die Paketdateien hochladen.](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. Klicken Sie unter **Blob hochladen** auf das Ordnersymbol, navigieren Sie zur EXE-Datei des Updatepakets, und klicken Sie dann im Explorer-Fenster auf **Öffnen**.
  
7. Klicken Sie unter **Blob hochladen** auf **Hochladen**.
  
    ![Hier sehen Sie, wo die einzelnen Paketdateien hochgeladen werden.](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. Wiederholen Sie die Schritte 6 und 7 für die Dateien *Paketname.bin* und „Metadata.xml“. Falls die Datei „Supplemental Notice.txt“ enthalten ist, importieren Sie sie nicht.
9. Anschließend können Sie die Benachrichtigungen (Glockensymbol in der oberen rechten Ecke des Portals) überprüfen. In den Benachrichtigungen sollte angegeben sein, dass der Upload abgeschlossen ist.
10. Kehren Sie zur Kachel „Aktualisieren“ auf dem Dashboard zurück. Auf der Kachel sollte angegeben sein, dass ein Update verfügbar ist. Klicken Sie auf die Kachel, um das neu hinzugefügte Updatepaket zu überprüfen.
11. Um das Update zu installieren, wählen Sie das als **Bereit** markierte Paket aus. Klicken Sie dann entweder mit der rechten Maustaste auf das Paket, und wählen Sie **Jetzt aktualisieren**, oder klicken Sie im oberen Bereich auf die Aktion **Jetzt aktualisieren**.
12. Wenn Sie auf „Update wird installiert...“ klicken, können Sie den Status im Bereich **Update run details** (Updateausführungsdetails) anzeigen. Hier können Sie auch auf **Download full logs** (Vollständige Protokolle herunterladen) klicken, um die Protokolldateien herunterzuladen.
13. Wenn das Update abgeschlossen ist, wird auf der Kachel „Aktualisieren“ die aktualisierte Azure Stack-Version angezeigt.

Sie können Updates manuell aus dem Speicherkonto löschen, nachdem sie in Azure Stack installiert wurden. Azure Stack führt in regelmäßigen Abständen eine Überprüfung auf ältere Updatepakete durch und entfernt diese aus dem Speicher. Es kann in Azure Stack bis zu zwei Wochen dauern, bis die alten Pakete entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht zum Verwalten von Updates in Azure Stack](azure-stack-updates.md)
- [Azure Stack-Wartungsrichtlinie](azure-stack-servicing-policy.md)
