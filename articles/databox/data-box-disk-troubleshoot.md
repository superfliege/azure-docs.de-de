---
title: Problembehandlung des Azure Data Box-Datenträgers | Microsoft Docs
description: Beschreibt, wie Sie Probleme mit dem Azure Data Box-Datenträger beheben können.
services: databox
author: alkohli
ms.service: databox
ms.topic: overview
ms.date: 10/09/2018
ms.author: alkohli
ms.openlocfilehash: 776108b109bc27e0f8059d287e87c67aeca9fbd2
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091849"
---
# <a name="troubleshoot-issues-in-azure-data-box-disk-preview"></a>Behandeln von Problemen mit dem Azure Data Box-Datenträger (Vorschau)

Dieser Artikel gilt für Microsoft Azure Data Box im Vorschaurelease. Dieser Artikel beschreibt einige der komplexen Workflows und Verwaltungsaufgaben, die für Azure Data Box und den Azure Data Box-Datenträger ausgeführt werden können. 

Die Data Box Disk-Instanz kann über das Azure-Portal verwaltet werden. Der Schwerpunkt dieses Artikels liegt auf den Aufgaben, die mithilfe des Azure-Portals ausgeführt werden können. Verwenden Sie das Azure-Portal, um Bestellungen und Geräte zu verwalten und den Status der Bestellung bis zum Abschluss nachzuverfolgen.

Dieser Artikel enthält folgende Lernprogramme:

- Herunterladen von Diagnoseprotokollen
- Abfragen von Aktivitätsprotokollen


> [!IMPORTANT]
> Data Box befindet sich in der Vorschau. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen.

## <a name="download-diagnostic-logs"></a>Herunterladen von Diagnoseprotokollen

Wenn während des Datenkopiervorgangs Fehler auftreten, zeigt das Portal einen Pfad zu dem Ordner an, in dem sich die Diagnoseprotokolle befinden. 

Folgende Diagnoseprotokolle sind möglich:
- Fehlerprotokolle
- Ausführliche Protokolle  

Um zum Pfad zum Kopieren der Protokolle zu navigieren, navigieren Sie zu dem Speicherkonto, das Ihrer Data Box-Bestellung zugeordnet ist. 

1.  Navigieren Sie zu **Allgemein > Bestelldetails**, und notieren Sie sich das Speicherkonto, das Ihrer Bestellung zugeordnet ist.
 

2.  Navigieren Sie zu **Alle Ressourcen**, und suchen Sie nach dem Speicherkonto, das Sie im vorherigen Schritt identifiziert haben. Wählen Sie das Speicherkonto aus, und klicken Sie darauf.

    ![Kopieren von Protokollen 1](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs1.png)

3.  Navigieren Sie zu **Blob-Dienst > Blobs durchsuchen**, und suchen Sie nach dem Blob, das dem Speicherkonto entspricht. Navigieren Sie zu **diagnosticslogcontainer > waies**. 

    ![Kopieren von Protokollen 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

    Sowohl die Fehlerprotokolle als auch die ausführlichen Protokolle sollten zum Kopieren von Daten angezeigt werden. Wählen Sie jede Datei aus, und klicken Sie darauf, und laden Sie dann eine lokale Kopie herunter.

## <a name="query-activity-logs"></a>Abfragen von Aktivitätsprotokollen

Verwenden Sie die Aktivitätsprotokolle zum Ermitteln eines Fehlers oder zum Nachverfolgen, welche Änderungen an einer Ressource ein Benutzer in Ihrer Organisation vorgenommen hat. Mithilfe von Aktivitätsprotokollen können Sie Folgendes ermitteln:

- Welche Vorgänge in den Ressourcen in Ihrem Abonnement ausgeführt wurden.
- Der Benutzer oder das System, von dem der Vorgang initiiert wurde. 
- Wann der Vorgang ausgeführt wurde.
- Der Status des Vorgangs.
- Werte anderer Eigenschaften, mit denen Sie den Vorgang ggf. untersuchen können.

Das Aktivitätsprotokoll enthält alle Schreibvorgänge (z.B. PUT, POST, DELETE) für Ihre Ressourcen, nicht aber die Lesevorgänge (z.B. GET). 

Aktivitätsprotokolle werden 90 Tage lang aufbewahrt. Sie können beliebige Datumsbereiche abfragen, das Startdatum darf jedoch maximal 90 Tage zurückliegen. Sie können auch mit einer der integrierten Abfragen in Insights filtern. Klicken Sie z.B. auf „Fehler“, und klicken Sie dann auf bestimmte Fehler, um die Grundursache zu verstehen.

## <a name="data-box-disk-unlock-tool-errors"></a>Fehler des Data Box Disk-Tools zum Entsperren


| Fehlermeldung/Toolverhalten      | Empfehlungen                                                                                               |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Keine<br><br>Das Tool zum Entsperren von Data Box-Datenträgern stürzt ab.                                                                            | BitLocker ist nicht installiert. Stellen Sie sicher, dass auf dem Hostcomputer, der das Tool zum Entsperren des Data Box-Datenträgers ausführt, BitLocker installiert ist.                                                                            |
| Die aktuelle .NET Framework-Version wird nicht unterstützt. Die unterstützten Versionen sind 4.5 oder höher.<br><br>Das Tool wird mit einer Meldung beendet.  | .NET 4.5 ist nicht installiert. Installieren Sie .NET 4.5 oder höher auf dem Hostcomputer, der das Tool zum Entsperren des Data Box-Datenträgers ausführt.                                                                            |
| Volumes konnten nicht entsperrt oder überprüft werden. Wenden Sie sich an den Microsoft Support.  <br><br>Das Tool entsperrt bzw. überprüft keine gesperrten Laufwerke. | Das Tool konnte keines der entsperrten Laufwerke mit dem angegebenen Hauptschlüssel entsperren. Erkundigen Sie sich beim Support von Microsoft nach den weiteren Schritten.                                                |
| Folgende Volumes sind entsperrt und überprüft. <br>Volumelaufwerkbuchstaben: E:<br>Es konnten keine Volumes mit den folgenden Hauptschlüsseln entsperrt werden: werwerqomnf, qwerwerqwdfda <br><br>Das Tool entsperrt einige Laufwerke und zeigt die erfolgreichen und fehlerhaften Laufwerkbuchstaben an.| Teilweise erfolgreich. Einige der Laufwerke konnten mit dem angegebenen Hauptschlüssel nicht entsperrt werden. Erkundigen Sie sich beim Support von Microsoft nach den weiteren Schritten. |
| Es wurden keine gesperrten Laufwerke gefunden. Stellen Sie sicher, dass der von Microsoft erhaltene Datenträger ordnungsgemäß verbunden ist und einen gesperrten Zustand aufweist.          | Das Tool findet keine gesperrten Laufwerke. Die Laufwerke sind bereits entsperrt, oder sie werden nicht erkannt. Stellen Sie sicher, dass die Laufwerke verbunden und gesperrt sind.                                                           |
| Schwerwiegender Fehler: Ungültiger Parameter<br>Parametername: invalid_arg<br>SYNTAX:<br>DataBoxDiskUnlock /PassKeys:<Durch_Semicolons_getrennte_Liste_der_Hauptschlüssel><br><br>Beispiel: DataBoxDiskUnlock /PassKeys:Hauptschlüssel1;Hauptschlüssel2;Hauptschlüssel3<br>Beispiel: DataBoxDiskUnlock /SystemCheck<br>Beispiel: DataBoxDiskUnlock /Help<br><br>/PassKeys:       Abrufen dieses Schlüssels aus der Bestellung des Azure DataBox-Datenträgers. Der Hauptschlüssel entsperrt Ihre Datenträger.<br>/Help:           Diese Option bietet Hilfe für die Cmdlet-Verwendung sowie Beispiele.<br>/SystemCheck:    Diese Option überprüft, ob Ihr System die Anforderungen zum Ausführen des Tools erfüllt.<br><br>Drücken Sie zum Beenden eine beliebige Taste. | Ungültige Parametereingabe. Der einzigen zulässigen Parameter sind /SystemCheck, /PassKey und /Help.                                                                            |

## <a name="data-box-disk-split-copy-tool-errors"></a>Fehler des Data Box Disk-Tools zum Aufteilen/Kopieren

|Fehlermeldung/Warnungen  |Empfehlungen |
|---------|---------|
|[Info] Retrieving bitlocker password for volume: m <br>[Error] Exception caught while retrieving bitlocker key for volume m:<br> Sequence contains no elements.
([Information] BitLocker-Kennwort für folgendes Volume wird abgerufen: M.
[Fehler] Ausnahme beim Abrufen des BitLocker-Schlüssels für das Volume M:
Die Sequenz enthält keine Elemente.)|Dieser Fehler tritt auf, wenn das Data Box Disk-Ziel offline ist. <br> Verwenden Sie das Tool `diskmgmt.msc`, um Datenträger online zu schalten.|
|[Error] Exception thrown: WMI operation failed:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=The format of the recovery password provided is invalid. <br>BitLocker recovery passwords are 48 digits. <br>Verify that the recovery password is in the correct format and then try again.
([Fehler] Ausgelöste Ausnahme: Fehler beim WMI-Vorgang:
Methode: UnlockWithNumericalPassword, Rückgabewert: 2150694965,
Win32-Meldung: Das Format des angegebenen Wiederherstellungskennworts ist ungültig.
BitLocker-Wiederherstellungskennwörter müssen 48-stellig sein.
Vergewissern Sie sich, dass das Wiederherstellungskennwort das richtige Format hat, und wiederholen Sie den Vorgang.|Entsperren Sie die Datenträger zunächst mit dem Data Box Disk-Tool zum Entsperren, und führen Sie den Befehl anschließend erneut aus. Weitere Informationen finden Sie unter: <li> [Entsperren von Datenträgern auf Windows-Client](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) </li><li> [Entsperren von Datenträgern auf Linux-Client](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client) </li>|
|[Error] Exception thrown: A DriveManifest.xml file exists on the target drive. <br> This indicates the target drive may have been prepared with a different journal file. <br>To add more data to the same drive, use the previous journal file. To delete existing data and reuse target drive for a new import job, delete the DriveManifest.xml on the drive. Rerun this command with a new journal file.
([Fehler] Ausgelöste Ausnahme: Auf dem Ziellaufwerk befindet sich eine Datei vom Typ „DriveManifest.xml“.
Das Ziellaufwerk wurde möglicherweise mit einer anderen Journaldatei vorbereitet.
Wenn Sie dem gleichen Laufwerk weitere Daten hinzufügen möchten, verwenden Sie die vorherige Journaldatei. Wenn Sie die vorhandenen Daten löschen und das Ziellaufwerk für einen neuen Importauftrag verwenden möchten, löschen Sie die Datei „DriveManifest.xml“ auf dem Laufwerk, und führen Sie diesen Befehl mit einer neuen Journaldatei aus.)| Dieser Fehler tritt auf, wenn Sie versuchen, die gleiche Gruppe von Laufwerken für mehrere Importsitzungen zu verwenden. <br> Verwenden Sie pro Aufteilungs-/Kopiersitzung immer nur eine Gruppe von Laufwerken.|
|[Error] Exception thrown: CopySessionId importdata-sept-test-1 refers to a previous copy session and cannot be reused for a new copy session. ([Fehler] Ausgelöste Ausnahme: „CopySessionId importdata-sept-test-1“ bezieht sich auf eine vorherige Kopiersitzung und kann nicht für diese neue Kopiersitzung wiederverwendet werden.)|Dieser Fehler tritt auf, wenn Sie versuchen, den Auftragsnamen eines bereits erfolgreich abgeschlossenen Auftrags für einen neuen Auftrag zu verwenden.<br> Weisen Sie Ihrem neuen Auftrag einen eindeutigen Namen zu.|
|[Info] Destination file or directory name exceeds the NTFS length limit. ([Information] Der Name der Zieldatei oder des Zielverzeichnisses übersteigt die NTFS-Längenbeschränkung.) |Diese Meldung wird zurückgegeben, wenn die Zieldatei aufgrund eines langen Dateipfads umbenannt wurde.<br> Ändern Sie die Dispositionsoption in der Datei `config.json`, die dieses Verhalten zu steuern.|
|[Error] Exception thrown: Bad JSON escape sequence. ([Fehler] Ausgelöste Ausnahme: Ungültige JSON-Escapesequenz.) |Diese Meldung wird zurückgegeben, wenn das Format von „config.json“ ungültig ist. <br> Überprüfen Sie die Datei `config.json` vor dem Speichern mithilfe von [JSONLint](https://jsonlint.com/).|



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Verwalten des Data Box-Datenträgers über das Azure-Portal](data-box-portal-ui-admin.md).
