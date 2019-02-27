---
title: Informationen zur Sicherung von Azure-VMs
description: Erfahren Sie mehr über die Sicherung von Azure-VMs und einige bewährte Methoden.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: c38c457bbf428d7252cf57168685201a2ca227ba
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446799"
---
# <a name="about-azure-vm-backup"></a>Informationen zur Sicherung von Azure-VMs

In diesem Artikel wird beschrieben, wie Azure-VMs mit dem Dienst [Azure Backup](backup-introduction-to-azure-backup.md) gesichert werden.

## <a name="backup-process"></a>Sicherungsprozess

Nachfolgend erfahren Sie, wie Azure Backup eine Sicherung für Azure-VMs durchführt.

1. Für Azure-VMs, die zur Sicherung ausgewählt sind, startet der Azure Backup-Dienst einen Sicherungsauftrag gemäß dem von Ihnen angegebenen Sicherungszeitplan.
2. Während der ersten Sicherung wird auf dem virtuellen Computer eine Sicherungserweiterung installiert, sofern er ausgeführt wird.
    - Für virtuelle Windows-Computer wird die Erweiterung _VMSnapshot_ installiert.
    - Für virtuelle Linux-Computer wird die Erweiterung _VMSnapshotLinux_ installiert.
3. Für ausgeführte virtuelle Windows-Computer erstellt Backup in Zusammenarbeit mit VSS eine App-konsistente Momentaufnahme des virtuellen Computers.
    - Backup erstellt standardmäßig vollständige VSS-Sicherungen.
    - Wenn Backup keine App-konsistente Momentaufnahme erstellen kann, wird eine dateikonsistente Momentaufnahme des zugrunde liegenden Speichers erstellt (weil keine Schreibvorgänge der Anwendung stattfinden, solange die VM beendet ist).
4. Für virtuelle Linux-Computer erstellt Backup eine dateikonsistente Sicherung. Für App-konsistente Momentaufnahmen ist eine manuelle Anpassung von Pre- und Post-Skripts erforderlich.
5. Nachdem die Momentaufnahme erstellt wurde, werden die Daten in den Tresor übertragen. 
    - Zur Optimierung von Backup werden die einzelnen VM-Datenträger parallel gesichert.
    - Für jeden Datenträger, der gesichert wird, liest Azure Backup die Blöcke auf dem Datenträger und identifiziert und überträgt nur die Datenblöcke, die sich seit der vorherigen Sicherung geändert haben (das Delta).
    - Nachdem die Momentaufnahme erstellt wurde, werden die Daten in den Tresor übertragen.
    - Momentaufnahmedaten werden möglicherweise nicht sofort in den Tresor kopiert. Zu Spitzenzeiten vergehen unter Umständen mehrere Stunden. Bei täglichen Sicherungsrichtlinien beträgt die Gesamtdauer der Sicherung eines virtuellen Computers weniger als 24 Stunden.

6. Wenn die Datenübertragung abgeschlossen ist, wird die Momentaufnahme entfernt und ein Wiederherstellungspunkt erstellt.

![Architektur der Sicherung von virtuellen Azure-Computern](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="encrypting-azure-vm-backups"></a>Verschlüsseln von Azure-VM-Sicherungen

Wenn Sie Azure-VMs mit Azure Backup sichern, werden ruhende VMs mit der Speicherdienstverschlüsselung (SSE) verschlüsselt. Zusätzlich kann Azure Backup virtuelle Azure-Computer sichern, die mit Azure Disk Encryption (ADE) verschlüsselt wurden.


**Verschlüsselung** | **Details** | **Unterstützung**
--- | --- | ---
**ADE** | ADE verschlüsselt sowohl Betriebssystemdatenträger als auch sonstige Datenträger für Azure-VMs.<br/><br/> ADE kann in BitLocker-Verschlüsselungsschlüssel (BEK), die als Geheimnisse in einem Schlüsseltresor geschützt werden, oder in Azure Key Vault-Schlüssel für Verschlüsselungsschlüssel (Key Encryption Keys, KEK) integriert werden. | Azure Backup unterstützt die Sicherung verwalteter und nicht verwalteter Azure-VMs, die nur mit BEK oder sowohl mit BEK als auch mit KEK verschlüsselt sind.<br/><br/> Sowohl BEK als auch gesichert und verschlüsselt.<br/><br/> Da KEK und BEK gesichert werden, können Benutzer mit Berechtigungen bei Bedarf Schlüssel und Geheimnisse im Schlüsseltresor und so die verschlüsselte VM wiederherstellen.<br/><br/> Verschlüsselte Schlüssel und Geheimnisse können von nicht dafür autorisierten Benutzern oder von Azure nicht gelesen werden.
**SSE** | Mit SSE bietet Azure Storage eine Verschlüsselung im Ruhezustand, indem Daten vor der Speicherung automatisch verschlüsselt und vor dem Abrufen entschlüsselt werden. | Azure Backup verwendet SSE zur Verschlüsselung ruhender Azure-VMs.

- Für verwaltete und nicht verwaltete Azure-VMs, die entweder nur mit BitLocker-Verschlüsselungsschlüssel (BEK) oder sowohl mit BitLocker-Verschlüsselungsschlüssel (BEK) als auch mit einem Schlüssel für Verschlüsselungsschlüssel (KEK) verschlüsselt sind, wird die Sicherung unterstützt.
- Der gesicherte BEK (Geheimnisse) und KEK (Schlüssel) werden verschlüsselt. Sie können nur gelesen und verwendet werden, wenn sie von autorisierten Benutzern im Schlüsseltresor wiederhergestellt werden. Weder nicht autorisierte Benutzer noch Azure können gesicherte Schlüssel oder Geheimnisse lesen oder verwenden.
- Da auch der BEK gesichert wird, können autorisierte Benutzer den BEK bei Verlust im Schlüsseltresor wiederherstellen und so auch die verschlüsselte VM wiederherstellen. 
- Nur Benutzer mit den entsprechenden Berechtigungen können verschlüsselte VMs sowie Schlüssel und Geheimnisse sichern und wiederherstellen.



## <a name="taking-snapshots"></a>Erstellen von Momentaufnahmen

Azure Backup-Momentaufnahmen gemäß Ihres Sicherungszeitplans. 

- **Virtuelle Windows-Computer:** Bei Windows-VMs koordiniert sich der Backup-Dienst mit dem Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um eine App-konsistente Momentaufnahme der VM-Datenträger zu erstellen.
    - Standardmäßig führt Azure Backup vollständig VSS-Sicherungen durch. [Weitere Informationen](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)
    - Wenn Sie die Einstellung so ändern möchten, dass Azure Backup Sicherungen von VSS-Kopien erstellt, legen Sie über eine Eingabeaufforderung den folgenden Registrierungsschlüssel fest: **REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d TRUE /f**.
- **Virtuelle Linux-Computer:** Wenn Sie App-konsistente Momentaufnahmen der Linux-VM erstellen möchten, verwenden Sie das Linux-Framework für vorher und nachher auszuführende Skripts (Pre- und Post-Skripts), um Ihre eigenen benutzerdefinierten Skripts zu schreiben und Konsistenz zu gewährleisten.
    -  Azure Backup ruft nur die von Ihnen geschriebenen Pre- und Post-Skripts auf.
    - Wenn das Pre-Skript und das Post-Skript erfolgreich ausgeführt werden, markiert Azure Backup den Wiederherstellungspunkt als anwendungskonsistent. Allerdings sind Sie bei Verwendung benutzerdefinierter Skripts letztendlich für die Anwendungskonsistenz verantwortlich.
    - [Erfahren Sie mehr](backup-azure-linux-app-consistent.md) zum Konfigurieren von Skripts.


### <a name="snapshot-consistency"></a>Konsistenz von Momentaufnahmen

In der folgenden Tabelle werden die verschiedenen Konsistenztypen erläutert.

**Momentaufnahme** | **Details** | **Wiederherstellung**: | **Aspekt**
--- | --- | --- | ---
**Anwendungskonsistent** | Anwendungskonsistente Sicherungen erfassen Speicherinhalte und ausstehende E/A-Vorgänge. Anwendungskonsistente Momentaufnahmen verwenden VSS Writer (oder Pre-/Post-Skript für Linux), der die Konsistenz der Anwendungsdaten gewährleistet, ehe eine Sicherung erfolgt. | Wenn Sie die Wiederherstellung mit einer anwendungskonsistenten Momentaufnahme durchführen, wird die VM hochgefahren. Es kommt weder zu einer Beschädigung noch zum Verlust von Daten. Die Apps starten in einem konsistenten Zustand. | Windows: Alle VSS-Writer wurden erfolgreich abgeschlossen.<br/><br/> Linux: Pre-/Post-Skripts sind konfiguriert und erfolgreich abgeschlossen.
**Dateisystemkonsistent** | Dateikonsistente Sicherungen bieten konsistente Sicherungen von Datenträgerdateien, indem eine Momentaufnahme aller Dateien gleichzeitig erstellt wird.<br/><br/> | Wenn Sie die Wiederherstellung mit einer anwendungskonsistenten Momentaufnahme durchführen, wird die VM hochgefahren. Es kommt weder zu einer Beschädigung noch zum Verlust von Daten. Apps müssen einen eigenen Reparaturmechanismus implementieren, um sicherzustellen, dass wiederhergestellte Daten konsistent sind. | Windows: Fehler bei einigen VSS-Writern <br/><br/> Linux: Standardmäßig (wenn Pre-/Post-Skripts nicht konfiguriert oder bei ihnen Fehler aufgetreten sind)
**Absturzkonsistent** | Absturzkonsistenz liegt oftmals vor, wenn eine Azure-VM zum Zeitpunkt der Sicherung heruntergefahren wird.  Nur die Daten, die zum Zeitpunkt der Sicherung bereits auf dem Datenträger vorhanden sind, werden erfasst und gesichert.<br/><br/> Ein absturzkonsistenter Wiederherstellungspunkt garantiert keine Datenkonsistenz für das Betriebssystem oder die App. | Es gibt keine Garantien, aber normalerweise wird die VM hochgefahren. Anschließend erfolgt eine Datenträgerprüfung, um Beschädigungen zu beheben. Alle Daten im Arbeitsspeicher oder Schreibvorgänge, die nicht vollständig auf den Datenträger übertragen wurden, gehen verloren. Apps implementieren ihre eigene Datenüberprüfung. Falls bei einer Datenbank-App ein Transaktionsprotokoll beispielsweise Einträge enthält, die nicht in der Datenbank vorhanden sind, führt die Datenbanksoftware ein Rollback durch, bis die Daten konsistent sind. | VM wird heruntergefahren.


## <a name="restore-considerations"></a>Aspekte bei der Wiederherstellung 



**Aspekt** | **Details**
--- | ---
**Datenträger** | Die Sicherung des VM-Datenträgers erfolgt parallel. Bei einem virtuellen Computer mit vier Datenträgern versucht der Dienst beispielsweise, alle vier Datenträger parallel zu sichern. Die Sicherung erfolgt inkrementell (nur geänderte Daten).
**Zeitplanung** |  Sichern Sie verschiedene virtuelle Computer zu unterschiedlichen Tageszeiten ohne Überschneidungen, um den Sicherungsdatenverkehr zu reduzieren. Wenn Sie VMs gleichzeitig sichern, entstehen Engpässe im Datenverkehr.
**Vorbereiten von Sicherungen** | Beachten Sie die Vorbereitungszeit für die Sicherung. Dazu gehören das Installieren oder Aktualisieren der Sicherungserweiterung sowie das Auslösen einer Momentaufnahme in Übereinstimmung mit dem Sicherungszeitplan.
**Datenübertragung** | Die Zeit, die der Sicherungsdienst zum Berechnen inkrementeller Änderungen aus der vorherigen Sicherung benötigt.<br/><br/> Bei einer inkrementellen Sicherung berechnet der Dienst die Prüfsumme des Blocks, um die Änderungen zu ermitteln. Wird ein Block geändert, wird er zum Senden an den Tresor identifiziert. Der Dienst untersucht die identifizierten Blöcke und versucht, die zu übertragenden Daten weiter zu minimieren. Nach Auswertung aller geänderten Blöcke werden die Änderungen an den Tresor übertragen.<br/><br/> Zwischen der Momentaufnahme und dem Kopiervorgang in den Tresor kann es zu einer Verzögerung kommen.<br/><br/> Zu Spitzenzeiten kann es bis zu acht Stunden dauern, bis Sicherungen verarbeitet werden. Bei der täglichen Sicherung beträgt die Sicherungsdauer eines virtuellen Computers weniger als 24 Stunden.
**Erste Sicherung** | Die Gesamtdauer der Sicherung von weniger als 24 Stunden gilt für inkrementelle Sicherungen, aber möglicherweise nicht für die erste Sicherung. Die benötigte Zeit hängt von der Größe der Daten und dem Zeitpunkt der Sicherung ab.
**Wiederherstellen der Warteschlange** | Azure Backup verarbeitet Wiederherstellungsaufträge von mehreren Speicherkonten gleichzeitig, und Wiederherstellungsanforderungen werden in eine Warteschlange eingereiht.
**Wiederherstellen einer Kopie** | Beim Wiederherstellungsvorgang werden Daten zuerst aus dem Tresor in das Speicherkonto kopiert.<br/><br/> Die Wiederherstellungszeit hängt von den IOPS und vom Durchsatz des Speicherkontos ab.<br/><br/> Wählen Sie ein Speicherkonto aus, das nicht mit den Schreib- und Lesevorgängen anderer Anwendungen geladen ist, um die Zeit für das Kopieren zu verringern.


### <a name="backup-performance"></a>Backupleistung

Diese gängigen Szenarien können die Sicherungsdauer beeinflussen:

- Hinzufügen eines neuen Datenträgers zu einer geschützten Azure-VM: Wenn für eine VM eine inkrementelle Sicherung durchgeführt und gleichzeitig ein neuer Datenträger hinzugefügt wird, kann die Sicherung aufgrund der ersten Replikation des neuen Datenträgers zusammen mit der Deltareplikation vorhandener Datenträger mehr als 24 Stunden dauern.
- Fragmentierte Datenträger: Sicherungsvorgänge sind schneller, wenn Datenträgeränderungen zusammengelegt werden. Wenn die Änderungen über einen Datenträger verteilt und fragmentiert werden, verläuft die Sicherung langsamer. 
- Datenträgeränderungen: Wenn geschützte Datenträger, für die eine inkrementelle Sicherung durchgeführt wird, täglich eine Änderungsrate von mehr als 200 GB aufweisen, kann der Sicherungsvorgang eine lange Zeit (mehr als acht Stunden) in Anspruch nehmen. 
- Backup-Versionen: Wenn Sie die neueste Version von Backup (namens „Instant Restore“) verwenden, wird ein besser optimierter Prozess als der Prüfsummenvergleich zum Vergleichen der Änderungen verwendet. Wenn Sie die neueste Version verwenden und eine Sicherungsmomentaufnahme gelöscht haben, wird die Sicherung auf den Prüfsummenvergleich umgestellt, und der Sicherungsvorgang dauert länger als 24 Stunden (oder verursacht einen Fehler).

## <a name="best-practices"></a>Bewährte Methoden 
Berücksichtigen Sie beim Konfigurieren von VM-Sicherungen die folgenden bewährten Methoden:

- Ziehen Sie in Betracht, die Standardzeit des Zeitplans in einer Richtlinie zu ändern. Wenn die Standardzeit in der Richtlinie beispielsweise auf 0:00 Uhr festgelegt ist, sollten Sie sie minutenweise erhöhen, damit die Ressourcen optimal verwendet werden.
- Bei Premium-VM-Sicherungen ohne das Feature für den sofortigen Wiederherstellungspunkt werden etwa 50 % des gesamten Platzes im Speicherkonto reserviert. Der Backup-Dienst benötigt diesen Speicherplatz, um die Momentaufnahme in dasselbe Speicherkonto zu kopieren und in den Tresor zu übertragen.
- Zum Wiederherstellen von VMs in einem einzigen Tresor wird dringend empfohlen, verschiedene [v2-Speicherkonten](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) zu verwenden, um sicherzustellen, dass das Zielspeicherkonto nicht gedrosselt wird. Beispielsweise muss jeder virtuelle Computer über ein anderes Speicherkonto verfügen. (Wenn 10 VMs wiederhergestellt werden, sollten Sie die Verwendung von 10 verschiedenen Speicherkonten in Betracht ziehen.)
- Wiederherstellungen von Speicherebene 1 (Momentaufnahme) werden in wenigen Minuten abgeschlossen (da es sich um dasselbe Speicherkonto handelt). Im Gegensatz dazu kann eine Wiederherstellung von Speicherebene 2 (Tresor) mehrere Stunden dauern. Wir empfehlen Ihnen, das Feature [Sofortige Wiederherstellung](backup-instant-restore-capability.md) zu verwenden, um eine schnellere Wiederherstellung zu erzielen, wenn Daten auf Ebene 1 verfügbar sind. (Wenn die Daten aus dem Tresor wiederhergestellt werden müssen, dauert dies einige Zeit.)
- Die maximale Anzahl von Datenträgern pro Speicherkonto ist davon abhängig, wie stark durch Anwendungen, die auf IaaS-VMs ausgeführt werden, auf den Datenträger zugegriffen wird. Überprüfen Sie, ob mehrere Datenträger in einem einzelnen Speicherkonto gehostet werden. Wenn 5 bis 10 Datenträger oder mehr in einem einzelnen Speicherkonto vorhanden sind, sollten Sie grundsätzlich die Last ausgleichen, indem Sie einige Datenträger in getrennte Speicherkonten verschieben.

## <a name="backup-costs"></a>Sicherungskosten

Für Azure-VMs, die mit Azure Backup gesichert werden, gelten [Azure Backup-Preise](https://azure.microsoft.com/pricing/details/backup/).

- Die Abrechnung erfolgt erst nach dem erfolgreichen Abschluss der ersten Sicherung. Zu diesem Zeitpunkt beginnt die Abrechnung für den Speicher und die geschützten VMs.
- Die Abrechnung erfolgt so lange, wie Sicherungsdaten für den virtuellen Computer in einem Tresor gespeichert werden. Wenn Sie den Schutz für einen virtuellen Computer beenden, aber Sicherungsdaten für den virtuellen Computer in einem Tresor vorhanden sind, wird die Abrechnung fortgesetzt.
- Die Abrechnung für eine VM endet erst, nachdem der Schutz beendet wurde und alle Sicherungsdaten gelöscht wurden.
- Wenn der Schutz beendet wird und es keine aktiven Sicherungsaufträge gibt, wird die Größe der letzten erfolgreichen VM-Sicherung als Größe der geschützten Instanz für die monatliche Rechnung verwendet.
- Die Berechnung der geschützten Instanzen basiert auf der *tatsächlichen* Größe des virtuellen Computers (also auf der Summe aller Daten auf dem virtuellen Computer – mit Ausnahme des temporären Speichers).
- Die Kosten basieren auf der tatsächlichen Menge der Daten, die auf dem Datenträger gespeichert sind.
- Die Sicherungskosten für VMs basieren nicht auf der maximal unterstützten Größe für die einzelnen Datenträger, die der VM angefügt sind.
- Gleichermaßen basiert die Abrechnung der Sicherungsspeicherung auf der Menge der in Azure Backup gespeicherten Daten, d.h. auf der Summe der tatsächlichen Daten an jedem Wiederherstellungspunkt.

Beispiel: Sie verwenden einen virtuellen Computer der Größe „A2 Standard“ sowie zwei zusätzliche Datenträger mit einer maximalen Größe von jeweils 4 TB. In der folgenden Tabelle sind die auf jedem Datenträger tatsächlich gespeicherten Daten aufgeführt:

**Datenträger** | **Max. Größe** | **Tatsächliche vorhandene Daten**
--- | --- | ---
Betriebssystem-Datenträger | 4095 GB | 17 GB 
Lokaler/temporärer Datenträger | 135 GB | 5 GB (bei der Sicherung nicht enthalten) 
Datenträger 1 | 4095 GB | 30 GB 
Datenträger 2 | 4095 GB | 0 GB 

- Die tatsächliche Größe der VM beträgt in diesem Fall 17 GB + 30 GB + 0 GB = 47 GB.
- Diese Größe der geschützten Instanz (47 GB) dient als Basis für die monatliche Rechnung.
- Mit zunehmender Datenmenge auf dem virtuellen Computer ändert sich entsprechend auch die Größe der geschützten Instanz, die für die Abrechnung verwendet wird.


## <a name="next-steps"></a>Nächste Schritte

[Bereiten Sie jetzt die Azure-VM-Sicherung vor](backup-azure-arm-vms-prepare.md).
