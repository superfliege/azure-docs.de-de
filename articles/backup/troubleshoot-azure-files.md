---
title: Behandeln von Problemen beim Sichern von Azure-Dateifreigaben
description: Dieser Artikel enthält Informationen zum Behandeln von Problemen in Verbindung mit dem Schutz Ihrer Azure-Dateifreigaben.
services: backup
ms.service: backup
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: bdb35cf47b339ff2089b3849283a71aa9d8fbc3d
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807413"
---
# <a name="troubleshoot-problems-backing-up-azure-file-shares"></a>Behandeln von Problemen beim Sichern von Azure-Dateifreigaben
Die folgende Tabelle enthält Problembehandlungsinformationen für Probleme und Fehler, die bei der Verwendung der Sicherung von Azure-Dateifreigaben auftreten können.

## <a name="preview-boundaries"></a>Einschränkungen der Vorschauversion
Die Sicherung für Azure-Dateifreigaben befindet sich in der Vorschauphase. Folgende Sicherungsszenarien werden für Azure-Dateifreigaben nicht unterstützt:
- Schützen von Azure-Dateifreigaben in Speicherkonten mit Replikation vom [RA-GRS](../storage/common/storage-redundancy-grs.md) (Read-Access Geo-Redundant Storage, georedundanter Speicher mit Lesezugriff)*
- Schützen von Azure-Dateifreigaben in Speicherkonten mit aktivierten virtuellen Netzwerken oder aktivierter Firewall
- Sichern von Azure-Dateifreigaben mithilfe von PowerShell oder über die Befehlszeilenschnittstelle

\*Azure-Dateifreigaben in Speicherkonten mit Replikation vom Typ [RA-GRS](../storage/common/storage-redundancy-grs.md) (Read-Access Geo-Redundant Storage, georedundanter Speicher mit Lesezugriff) werden als GRS verwendet und zu GRS-Preisen abgerechnet.

Die Sicherung für Azure-Dateifreigaben in Speicherkonten mit Replikation vom Typ [ZRS](../storage/common/storage-redundancy-zrs.md) (Zone Redundant Storage, zonenredundanter Speicher) steht momentan nur in den Regionen „USA, Mitte“ (CUS) und „USA, Osten 2“ (EUS2) zur Verfügung.

### <a name="limitations"></a>Einschränkungen
- Die maximale Anzahl von geplanten Sicherungen pro Tag beträgt 1.
- Die maximale Anzahl von bedarfsgesteuerten Sicherungen pro Tag beträgt 4.
- Verwenden Sie Ressourcensperren für das Speicherkonto, um das versehentliche Löschen von Sicherungen in Ihrem Recovery Services-Tresor zu verhindern.
- Löschen Sie keine Momentaufnahmen, die mit Azure Backup erstellt wurden. Das Löschen von Momentaufnahmen kann zum Verlust von Wiederherstellungspunkten bzw. zu Wiederherstellungsfehlern führen.

## <a name="configuring-backup"></a>Konfigurieren der Sicherung
Die folgende Tabelle bezieht sich auf die Konfiguration der Sicherung:

| Konfigurieren der Sicherung | Problemumgehung oder Lösungstipps |
| ------------------ | ----------------------------- |
| Ich habe mein Speicherkonto zum Konfigurieren der Sicherung für die Azure-Dateifreigabe nicht gefunden. | <ul><li>Warten Sie, bis die Ermittlung abgeschlossen ist. <li>Überprüfen Sie, ob eine Dateifreigabe aus dem Speicherkonto bereits durch einen anderen Recovery Services-Tresor geschützt ist. **Hinweis:** Alle Dateifreigaben in einem Speicherkonto können nur unter einem einzelnen Recovery Services-Tresor geschützt werden. <li>Vergewissern Sie sich, dass sich die Dateifreigabe nicht in einem nicht unterstützten Speicherkonto befindet.|
| Im Portal tritt ein Fehler mit dem Hinweis auf, dass die Speicherkonten nicht erfolgreich erkannt werden konnten. | Wenn es sich bei Ihrem Abonnement um ein (CSP-fähiges) Partnerabonnement handelt, ignorieren Sie den Fehler. Falls Ihr Abonnement nicht CSP-fähig ist und Ihre Speicherkonten nicht erkannt werden, wenden Sie sich an den Support.|
| Die Überprüfung oder Registrierung des ausgewählten Speicherkontos war nicht erfolgreich.| Wiederholen Sie den Vorgang. Sollte das Problem weiterhin auftreten, wenden Sie sich an den Support.|
| Die Dateifreigaben im ausgewählten Speicherkonto konnten nicht aufgelistet werden oder wurden nicht gefunden. | <ul><li> Vergewissern Sie sich, dass das Speicherkonto in der Ressourcengruppe vorhanden ist und seit der letzten Überprüfung/Registrierung im Tresor nicht gelöscht oder verschoben wurde.<li>Vergewissern Sie sich, dass die Dateifreigabe, die Sie schützen möchten, nicht gelöscht wurde. <li>Vergewissern Sie sich, dass das Speicherkonto für die Sicherung von Dateifreigaben unterstützt wird.<li>Überprüfen Sie, ob die Dateifreigabe bereits im gleichen Recovery Services-Tresor geschützt wird.|
| Bei der Konfiguration der Dateifreigabesicherung (oder der Schutzrichtlinie) tritt ein Fehler auf. | <ul><li>Wiederholen Sie den Vorgang, um zu prüfen, ob das Problem weiterhin besteht. <li> Vergewissern Sie sich, dass die Dateifreigabe, die Sie schützen möchten, nicht gelöscht wurde. <li> Wenn Sie mehrere Dateifreigaben auf einmal schützen möchten und für einige der Dateifreigaben ein Fehler auftritt, konfigurieren Sie die Sicherung für die Dateifreigaben, bei denen ein Fehler aufgetreten ist, erneut. |
| Nach dem Aufheben des Schutzes einer Dateifreigabe kann der Recovery Services-Tresor nicht gelöscht werden. | Öffnen Sie im Azure-Portal Ihren Tresor, klicken Sie auf **Sicherungsinfrastruktur** > **Speicherkonten**, und klicken Sie anschließend auf **Registrierung aufheben**, um das Speicherkonto aus dem Recovery Services-Tresor zu entfernen.|


## <a name="error-messages-for-backup-or-restore-job-failures"></a>Fehlermeldungen für nicht erfolgreiche Sicherungs- oder Wiederherstellungsaufträge

| Fehlermeldungen | Problemumgehung oder Lösungstipps |
| -------------- | ----------------------------- |
| Vorgangsfehler, weil die Dateifreigabe nicht gefunden wurde. | Vergewissern Sie sich, dass die Dateifreigabe, die Sie schützen möchten, nicht gelöscht wurde.|
| Das Speicherkonto wurde nicht gefunden oder wird nicht unterstützt. | <ul><li>Vergewissern Sie sich, dass das Speicherkonto in der Ressourcengruppe vorhanden ist und seit der letzten Überprüfung nicht gelöscht oder aus der Ressourcengruppe entfernt wurde. <li> Vergewissern Sie sich, dass das Speicherkonto für die Sicherung von Dateifreigaben unterstützt wird.|
| Sie haben die maximale Anzahl von Momentaufnahmen für diese Dateifreigabe erreicht. Sie können weitere erstellen, sobald ältere abgelaufen sind. | <ul><li> Dieser Fehler kann auftreten, wenn Sie mehrere bedarfsgesteuerte Sicherungen für eine Datei erstellen. <li> Pro Dateifreigabe sind maximal 200 Momentaufnahmen zulässig – einschließlich derer, die von Azure Backup erstellt werden. Ältere geplante Sicherungen (oder Momentaufnahmen) werden automatisch bereinigt. Bedarfsgesteuerte Sicherungen (oder Momentaufnahmen) müssen gelöscht werden, wenn die maximal zulässige Anzahl erreicht wird.<li> Löschen Sie die bedarfsgesteuerten Sicherungen (Momentaufnahmen von Azure-Dateifreigaben) über das Azure Files-Portal. **Hinweis:** Das Löschen von Momentaufnahmen, die mit Azure Backup erstellt wurden, führt zum Verlust der Wiederherstellungspunkte. |
| Fehler bei der Dateifreigabesicherung/Dateifreigabewiederherstellung aufgrund der Speicherdienstdrosselung. Möglicherweise ist der Speicherdienst mit der Verarbeitung anderer Anforderungen für das angegebene Speicherkonto ausgelastet.| Wiederholen Sie den Vorgang nach einiger Zeit. |
| Fehler beim Wiederherstellen: Die Zieldateifreigabe wurde nicht gefunden. | <ul><li>Vergewissern Sie sich, dass das ausgewählte Speicherkonto vorhanden ist und die Zieldateifreigabe nicht gelöscht wurde. <li> Vergewissern Sie sich, dass das Speicherkonto für die Sicherung von Dateifreigaben unterstützt wird. |
| Azure Backup wird derzeit nicht für Azure-Dateifreigaben in Speicherkonten unterstützt, für die virtuelle Netzwerke aktiviert sind. | Deaktivieren Sie virtuelle Netzwerke für Ihr Speicherkonto, um erfolgreiche Sicherungs- und Wiederherstellungsvorgänge zu ermöglichen. |
| Fehler bei der Sicherung/Wiederherstellung, weil sich das Speicherkonto im gesperrten Zustand befindet. | Entfernen Sie die Sperre des Speicherkontos, oder verwenden Sie eine Löschsperre anstelle einer Lesesperre, und wiederholen Sie den Vorgang. |
| Fehler bei der Wiederherstellung, weil die Anzahl fehlerhafter Dateien den Schwellenwert übersteigt. | <ul><li> Die Ursachen für Fehler bei der Wiederherstellung werden in einer Datei aufgeführt. (Den Pfad finden Sie in den Auftragsdetails.) Beheben Sie die Fehler, und wiederholen Sie den Wiederherstellungsvorgang für die Dateien, bei denen ein Fehler aufgetreten ist. <li> Gängige Fehlerursachen beim Wiederherstellen von Dateien: <br/> - Die Dateien, bei denen ein Fehler aufgetreten ist, werden gerade verwendet. <br/> - Das übergeordnete Verzeichnis enthält ein Verzeichnis mit dem gleichen Namen wie die Datei, bei der ein Fehler aufgetreten ist. |
| Fehler bei der Wiederherstellung, weil keine Datei wiederhergestellt werden konnte. | <ul><li> Die Ursachen für Fehler bei der Wiederherstellung werden in einer Datei aufgeführt. (Den Pfad finden Sie in den Auftragsdetails.) Beheben Sie die Fehler, und wiederholen Sie die Wiederherstellungsvorgänge für die Dateien, bei denen ein Fehler aufgetreten ist. <li> Gängige Fehlerursachen beim Wiederherstellen von Dateien: <br/> - Die Dateien, bei denen ein Fehler aufgetreten ist, werden gerade verwendet. <br/> - Das übergeordnete Verzeichnis enthält ein Verzeichnis mit dem gleichen Namen wie die Datei, bei der ein Fehler aufgetreten ist. |
| Fehler beim Wiederherstellen: Eine der Dateien in der Quelle ist nicht vorhanden. | <ul><li> Die ausgewählten Elemente sind in den Wiederherstellungspunktdaten nicht vorhanden. Geben Sie die korrekte Dateiliste an, um die Dateien wiederherzustellen. <li> Die Dateifreigabemomentaufnahme für den Wiederherstellungspunkt wurde manuell gelöscht. Wiederholen Sie den Wiederherstellungsvorgang mit einem anderen Wiederherstellungspunkt. |
| Ein Wiederherstellungsauftrag wird zurzeit für dasselbe Ziel durchgeführt. | <ul><li>Die Dateifreigabesicherung unterstützt keine parallele Wiederherstellung in der gleichen Zieldateifreigabe. <li>Warten Sie, bis die aktuelle Wiederherstellung abgeschlossen ist, und wiederholen Sie den Vorgang. Sollten Sie im Recovery Services-Tresor keinen Wiederherstellungsauftrag finden, überprüfen Sie andere Recovery Services-Tresore im gleichen Abonnement. |
| Fehler beim Wiederherstellungsvorgang, weil die Zieldateifreigabe voll ist. | Erhöhen Sie das Größenkontingent der Zieldateifreigabe so, dass es für die Wiederherstellungsdaten ausreicht, und wiederholen Sie anschließend den Vorgang. |
| Der Wiederherstellungsvorgang war nicht erfolgreich: Beim Ausführen der Wiederherstellungsvorbereitung für mit der Zieldateifreigabe verknüpfte Ressourcen des File Sync-Diensts ist ein Fehler aufgetreten. | Wiederholen Sie den Vorgang nach einiger Zeit. Wenn das Problem weiterhin auftritt, wenden Sie sich an den Microsoft-Support. |
| Mindestens eine Datei konnte nicht erfolgreich wiederhergestellt werden. Weitere Informationen finden Sie in der Liste fehlerhafter Dateien im oben angegebenen Pfad. | <ul> <li> Die Ursachen für Fehler bei der Wiederherstellung werden in einer Datei aufgeführt. (Den Pfad finden Sie in den Auftragsdetails.) Beheben Sie die Ursachen, und wiederholen Sie den Wiederherstellungsvorgang für die Dateien, bei denen ein Fehler aufgetreten ist. <li> Häufige Fehlerursachen beim Wiederherstellen von Dateien: <br/> - Die Dateien, bei denen ein Fehler aufgetreten ist, werden gerade verwendet. <br/> - Das übergeordnete Verzeichnis enthält ein Verzeichnis mit dem gleichen Namen wie die Dateien, bei denen ein Fehler aufgetreten ist. |

## <a name="see-also"></a>Siehe auch
Weitere Informationen zum Sichern von Azure-Dateifreigaben finden Sie in den folgenden Artikeln:
- [Sichern von Azure-Dateifreigaben](backup-azure-files.md)
- [Fragen zum Sichern von Azure Files](backup-azure-files-faq.md)
