---
title: Informationen zur Sicherung von Azure-VMs
description: Erfahren Sie mehr über die Sicherung von Azure-VMs und einige bewährte Methoden.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: raynew
ms.openlocfilehash: cac219414418277ace09ba3a0b442f3bf74e6025
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107428"
---
# <a name="about-azure-vm-backup"></a>Informationen zur Sicherung von Azure-VMs

In diesem Artikel wird beschrieben, wie Azure-VMs mit dem Dienst [Azure Backup](backup-introduction-to-azure-backup.md) gesichert werden.

## <a name="backup-process"></a>Sicherungsprozess

Nachfolgende erfahren Sie, wie Azure Backup eine Sicherung für Azure-VMs durchführt.

1. Für Azure-VMs, die zur Sicherung ausgewählt sind, startet der Azure Backup-Dienst einen Sicherungsauftrag gemäß dem von Ihnen angegebenen Sicherungszeitplan.
2. Der Dienst löst die Sicherungserweiterung aus.
    - Windows-VMs verwenden die Erweiterung _VMSnapshot_.
    - Linux-VMs verwenden die Erweiterung _VMSnapshotLinux_.
    - Die Erweiterung wird während der ersten VM-Sicherung installiert.
    - Um die Erweiterung zu installieren, muss die VM ausgeführt werden.
    - Wenn die VM nicht ausgeführt wird, erstellt der Backup-Dienst eine Momentaufnahme des zugrunde liegenden Speichers (da keine Schreibvorgänge der Anwendung erfolgen, während die VM beendet ist).
4. Die Sicherungserweiterung erstellt eine Momentaufnahme auf Speicherebene, die absturz- und dateikonsistent ist.
5. Nachdem die Momentaufnahme erstellt wurde, werden die Daten in den Tresor übertragen. Um die Effizienz zu maximieren, werden vom Dienst nur diejenigen Datenblöcke bestimmt und übertragen, die seit der vorherigen Sicherung geändert wurden (das sog Delta).
5. Wenn die Datenübertragung abgeschlossen ist, wird die Momentaufnahme entfernt und ein Wiederherstellungspunkt erstellt.

![Architektur der Sicherung von virtuellen Azure-Computern](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="data-encryption"></a>Datenverschlüsselung

Azure Backup verschlüsselt die Daten während des Sicherungsvorgangs nicht. Azure Backup unterstützt die Sicherung von Azure-VMs, die mit Azure Disk Encryption (ADE) verschlüsselt wurden.

- Die Sicherung verwalteter und nicht verwalteter Azure-VMs wird unterstützt, die entweder nur mit BitLocker-Verschlüsselungsschlüssel (Bitlocker Encryption Key, BEK) oder mit BitLocker-Verschlüsselungsschlüssel (BEK) und Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK) verschlüsselt sind.
- Die gesicherten BEK-Geheimnisse und KEK-Schlüssel werden verschlüsselt, sodass sie nur gelesen und verwendet werden können, wenn sie von den autorisierten Benutzern im Schlüsseltresor wiederhergestellt werden.
- Da auch der BEK gesichert wird, können in Szenarien, in denen der BEK verloren geht, autorisierte Benutzer den BEK im Schlüsseltresor wiederherstellen und die verschlüsselte VM wiederherstellen. Schlüssel und Geheimnisse von verschlüsselten VMs werden in verschlüsselter Form gesichert, sodass weder unbefugte Benutzer noch Azure gesicherte Schlüssel und Geheimnisse lesen oder verwenden können. Nur Benutzer mit den entsprechenden Berechtigungen können verschlüsselte VMs sowie Schlüssel und Geheimnisse sichern und wiederherstellen.

## <a name="snapshot-consistency"></a>Konsistenz von Momentaufnahmen

Um Momentaufnahmen zu erstellen, während Apps ausgeführt werden, erstellt Azure Backup anwendungskonsistente Momentaufnahmen.

- **Virtuelle Windows-Computer:** Bei Windows-VMs koordiniert sich der Azure Backup-Dienst mit dem Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um eine konsistente Momentaufnahme der VM-Datenträger zu erhalten.
    - Standardmäßig führt Azure Backup vollständig VSS-Sicherungen durch. [Weitere Informationen](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)
    - Wenn Sie die Einstellung so ändern möchten, dass Azure Backup Sicherungen von VSS-Kopien erstellt, legen Sie den folgenden Registrierungsschlüssel fest:
        ```
        [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
        ""USEVSSCOPYBACKUP"="TRUE"
        ```
- **Virtuelle Linux-Computer:** Um sicherzustellen, dass Ihre Linux-VMs App-konsistent sind, wenn Azure Backup eine Momentaufnahme erstellt, können Sie das Pre-Skript- und Post-Skript-Framework von Linux verwenden. Sie können eigene benutzerdefinierte Skripts zur Gewährleistung der Konsistenz beim Erstellen einer VM-Momentaufnahme schreiben.
    -  Azure Backup ruft nur von Ihnen geschriebene Pre- und Post-Skripts auf.
    - Wenn das Pre-Skript und das Post-Skript erfolgreich ausgeführt werden, markiert Azure Backup den Wiederherstellungspunkt als anwendungskonsistent. Allerdings sind Sie bei Verwendung benutzerdefinierter Skripts letztendlich für die Anwendungskonsistenz verantwortlich.
    - [Erfahren Sie mehr](backup-azure-linux-app-consistent.md) zum Konfigurieren von Skripts.


#### <a name="consistency-types"></a>Konsistenztypen

In der folgenden Tabelle werden die verschiedenen Konsistenztypen erläutert.

**Momentaufnahme** | **VSS-basiert** | **Details** | **Wiederherstellung**:
--- | --- | --- | ---
**Anwendungskonsistent** | Ja (nur Windows) | Anwendungskonsistente Sicherungen erfassen Speicherinhalte und ausstehende E/A-Vorgänge. Anwendungskonsistente Momentaufnahmen verwenden VSS Writer (oder Pre-/Post-Skript für Linux), der die Konsistenz der Anwendungsdaten gewährleistet, ehe eine Sicherung erfolgt. | Wenn Sie die Wiederherstellung mit einer anwendungskonsistenten Momentaufnahme durchführen, wird die VM hochgefahren. Es kommt weder zu einer Beschädigung noch zum Verlust von Daten. Die Apps starten in einem konsistenten Zustand.
**Dateisystemkonsistent** | Ja (nur Windows) |  Dateikonsistente Sicherungen bieten konsistente Sicherungen von Datenträgerdateien, indem eine Momentaufnahme aller Dateien gleichzeitig erstellt wird.<br/><br/> Azure Backup-Wiederherstellungspunkte sind für Folgendes dateikonsistent:<br/><br/> – Linux-VM-Sicherungen, die keine Pre-/Post-Skripts haben oder bei denen das Skript nicht erfolgreich war.<br/><br/> – Windows VM-Sicherungen, bei denen VSS fehlerhaft war. | Wenn Sie die Wiederherstellung mit einer anwendungskonsistenten Momentaufnahme durchführen, wird die VM hochgefahren. Es kommt weder zu einer Beschädigung noch zum Verlust von Daten. Apps müssen einen eigenen Reparaturmechanismus implementieren, um sicherzustellen, dass wiederhergestellte Daten konsistent sind.
**Absturzkonsistent** | Nein  | Absturzkonsistenz liegt oftmals vor, wenn eine Azure-VM zum Zeitpunkt der Sicherung heruntergefahren wird.  Nur die Daten, die zum Zeitpunkt der Sicherung bereits auf dem Datenträger vorhanden sind, werden erfasst und gesichert.<br/><br/> Ein absturzkonsistenter Wiederherstellungspunkt garantiert keine Datenkonsistenz für das Betriebssystem oder die App. | Es gibt keine Garantien, aber normalerweise wird die VM hochgefahren. Anschließend erfolgt eine Datenträgerprüfung, um Beschädigungen zu beheben. Alle Daten im Arbeitsspeicher oder Schreibvorgänge, die nicht vollständig auf den Datenträger übertragen wurden, gehen verloren. Apps implementieren ihre eigene Datenüberprüfung. Falls bei einer Datenbank-App ein Transaktionsprotokoll beispielsweise Einträge enthält, die nicht in der Datenbank vorhanden sind, führt die Datenbanksoftware ein Rollback durch, bis die Daten konsistent sind.


## <a name="service-and-subscription-limits"></a>Grenzwerte für Dienste und Abonnements

Azure Backup weist verschiedene Grenzwerte für Abonnements und Tresore auf.

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>Backupleistung

### <a name="disk-considerations"></a>Datenträgeraspekte

Der Sicherungsvorgang wird durch das parallele Sichern aller Datenträger des virtuellen Computers optimiert. Bei einem virtuellen Computer mit vier Datenträgern versucht der Dienst beispielsweise, alle vier Datenträger parallel zu sichern. Für jeden zu sichernden Datenträger liest Azure Backup die Blöcke auf dem Datenträger und speichert nur die geänderten Daten (inkrementelle Sicherung).


### <a name="scheduling-considerations"></a>Aspekte bei der Zeitplanung

Die Zeitplanung für Sicherungen wirkt sich auf die Leistung aus.

- Wenn Sie die Richtlinien so konfigurieren, dass alle VMs gleichzeitig gesichert werden, ist ein Datenstau vorprogrammiert, da der Sicherungsprozess versucht, alle Datenträger parallel zu sichern.
- Sichern Sie verschiedene virtuelle Computer zu unterschiedlichen Tageszeiten ohne Überschneidungen, um den Sicherungsdatenverkehr zu reduzieren.


### <a name="time-considerations"></a>Überlegungen zur Uhrzeit

Obgleich die meiste Sicherungszeit für das Lesen und Kopieren von Daten aufgewendet wird, können auch andere Vorgänge die Gesamtdauer einer VM-Sicherung verlängern:

- **Installieren einer Sicherungserweiterung**: Die Zeit, die zum Installieren oder Aktualisieren der Sicherungserweiterung benötigt wird.
- **Auslösen der Momentaufnahme**: Die zum Auslösen einer Momentaufnahme erforderliche Zeit. Momentaufnahmen werden kurz vor der geplanten Sicherungszeit ausgelöst.
- **Wartezeit in Warteschlange**: Da der Azure Backup-Dienst Aufträge für Speicherkonten mehrerer Kunden gleichzeitig ausführt, können Momentaufnahmedaten möglicherweise nicht sofort in den Recovery Services-Tresor kopiert werden. Zu Spitzenladezeiten kann es bis zu acht Stunden dauern, bis die Sicherungen verarbeitet werden. Die Gesamtdauer der VM-Sicherung wird jedoch bei täglichen Sicherungsrichtlinien weniger als 24 Stunden betragen.
- **Erste Sicherung**: Die Gesamtdauer der Sicherung von weniger als 24 Stunden gilt für inkrementelle Sicherungen, aber möglicherweise nicht für die erste Sicherung. Die benötigte Zeit hängt von der Größe der Daten und dem Zeitpunkt der Sicherung ab.
- **Datenübertragungszeit**: Die Zeit, die der Sicherungsdienst zum Berechnen von inkrementellen Änderungen aus der vorherigen Sicherung und der Übertragung dieser Änderungen in den Tresorspeicher benötigt.

### <a name="factors-affecting-backup-time"></a>Faktoren mit Einfluss auf die Sicherungsdauer

Die Sicherung besteht aus zwei Phasen: Erstellen von Momentaufnahmen und Übertragen der Momentaufnahmen in den Tresor. Der Backup-Dienst ist für Speicher optimiert.

- Wenn die Daten einer Momentaufnahme in den Tresor übertragen werden, überträgt der Dienst nur inkrementelle Änderungen aus den vorherigen Momentaufnahmen.
- Um inkrementelle Änderungen zu bestimmen, berechnet der Dienst die Prüfsumme der Blöcke.
    - Wenn ein Block geändert wird, wird der Block als Block identifiziert, der in den Tresor gesendet werden soll.
    - Der Dienst untersucht jeden identifizierten Block noch näher und sucht nach Möglichkeiten, die Menge der zu übertragenden Daten zu verringern.
    - Nach dem Evaluieren aller geänderten Blöcke fügt der Dienst die Änderungen wieder zusammen und sendet sie an den Tresor.

Die folgenden Situationen können sich auf die Sicherungsdauer auswirken:

- **Erste Sicherung eines zu einer bereits geschützten VM neu hinzugefügten Datenträgers**: Wenn eine inkrementelle Sicherung einer VM durchgeführt und dieser VM ein neuer Datenträger hinzugefügt wird, kann die Sicherungsdauer über 24 Stunden hinausgehen, da zusätzlich zur Deltareplikation der vorhandenen Datenträger eine erste Replikation für den neu hinzugefügten Datenträger durchgeführt werden muss.
- **Fragmentierung:** Das Sicherungsprodukt sucht nach inkrementellen Änderungen zwischen zwei Sicherungen. Sicherungsvorgänge sind schneller, wenn die Änderungen auf dem Datenträger nahe beieinander angeordnet und nicht über den Datenträger verteilt sind. 
- **Änderung:** Bei einer täglichen Änderungsrate (bei inkrementeller Replikation) von mehr als 200 GB pro Datenträger kann es mehr als acht Stunden dauern, den Vorgang abzuschließen. Wenn der virtuelle Computer mehrere Datenträger aufweist und für einen dieser Datenträger die Sicherung länger dauert, kann dies den gesamten Sicherungsvorgang beeinträchtigen (oder zu Fehlern führen). 
- **CC-Modus (Prüfsummenvergleich):** Der CC-Modus ist vergleichsweise langsamer als der optimierte Modus, der bei sofortigem Wiederherstellungspunkt verwendet wird. Wenn Sie bereits den sofortigen Wiederherstellungspunkt verwenden und die Momentaufnahmen der Ebene 1 gelöscht haben, wird die Sicherung in den CC-Modus umgeschaltet, sodass der Sicherungsvorgang möglicherweise länger als 24 Stunden dauert (oder Fehler auftreten).

## <a name="restore-considerations"></a>Aspekte bei der Wiederherstellung

Eine Wiederherstellung umfasst zwei zentrale Vorgänge: Kopieren von Daten aus dem Tresor in das ausgewählte Speicherkonto und Erstellen des virtuellen Computers. Die benötigte Zeit zum Kopieren von Daten aus dem Tresor hängt vom Speicherort der Sicherungen in Azure und des Speicherkontos ab. Die Dauer des Datenkopiervorgangs hängt von folgenden Faktoren ab:

- **Wartezeit in Warteschlange**: Da der Dienst Wiederherstellungsaufträge von mehreren Speicherkonten gleichzeitig verarbeitet, werden Wiederherstellungsanforderungen in eine Warteschlange eingereiht.
- **Dauer des Datenkopiervorgangs**: Daten werden zuerst aus dem Tresor in das Speicherkonto kopiert. Die Wiederherstellungsdauer hängt vom IOPS-Wert und dem Durchsatz des ausgewählten Speicherkontos ab, das der Azure Backup-Dienst verwendet. Wählen Sie ein Speicherkonto aus, das nicht mit anderen Schreib- und Lesevorgängen anderer Anwendungen geladen ist, um die Zeit für das Kopieren zu verringern.

## <a name="best-practices"></a>Bewährte Methoden

Berücksichtigen Sie beim Konfigurieren von VM-Sicherungen die folgenden bewährten Methoden:

- Aktualisieren Sie Tresore auf den sofortigen Wiederherstellungspunkt. Sehen Sie sich [diese Vorteile](backup-upgrade-to-vm-backup-stack-v2.md) und [Überlegungen](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade) an, und führen Sie anschließend ein Upgrade mithilfe [dieser Anweisungen](backup-upgrade-to-vm-backup-stack-v2.md#upgrade) durch.  
- Ziehen Sie in Betracht, die Standardeinstellung für die Richtlinienuhrzeit für die Erstellung der Datenmomentaufnahmen zu ändern, um sicherzustellen, dass Ressourcen optimal genutzt werden. Wenn Ihre Richtlinienuhrzeit beispielsweise standardmäßig 00:00 Uhr ist, sollten Sie sie um einige Minuten erhöhen.
- Bei Premium-VM-Sicherungen ohne das Feature für den sofortigen Wiederherstellungspunkt werden etwa 50 % des gesamten Platzes im Speicherkonto reserviert. Der Backup-Dienst benötigt diesen Speicherplatz, um die Momentaufnahme in dasselbe Speicherkonto zu kopieren und in den Tresor zu übertragen.
- Zum Wiederherstellen von VMs in einem einzigen Tresor wird dringend empfohlen, verschiedene  [v2-Speicherkonten](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) zu verwenden, um sicherzustellen, dass das Zielspeicherkonto nicht gedrosselt wird. Beispielsweise muss jeder virtuelle Computer über ein anderes Speicherkonto verfügen. (Wenn 10 VMs wiederhergestellt werden, sollten Sie die Verwendung von 10 verschiedenen Speicherkonten in Betracht ziehen.)
- Wiederherstellungen von Speicherebene 1 (Momentaufnahme) werden in wenigen Minuten abgeschlossen (da es sich um dasselbe Speicherkonto handelt). Im Gegensatz dazu kann eine Wiederherstellung von Speicherebene 2 (Tresor) mehrere Stunden dauern. Wir empfehlen Ihnen, das Feature für den [sofortigen Wiederherstellungspunkt](backup-upgrade-to-vm-backup-stack-v2.md) zu verwenden, um eine schnellere Wiederherstellung zu erzielen, wenn Daten auf Ebene 1 verfügbar sind. (Wenn die Daten aus dem Tresor wiederhergestellt werden müssen, dauert dies einige Zeit.)
- Die maximale Anzahl von Datenträgern pro Speicherkonto ist davon abhängig, wie stark durch Anwendungen, die auf IaaS-VMs ausgeführt werden, auf den Datenträger zugegriffen wird. Überprüfen Sie, ob mehrere Datenträger in einem einzelnen Speicherkonto gehostet werden. Wenn 5 bis 10 Datenträger oder mehr in einem einzelnen Speicherkonto vorhanden sind, sollten Sie grundsätzlich die Last ausgleichen, indem Sie einige Datenträger in getrennte Speicherkonten verschieben.

## <a name="backup-costs"></a>Sicherungskosten

Für Azure-VMs, die mit Azure Backup gesichert werden, gelten [Azure Backup-Preise](https://azure.microsoft.com/pricing/details/backup/).

- Die Abrechnung erfolgt erst nach dem erfolgreichen Abschluss der ersten Sicherung. Zu diesem Zeitpunkt beginnt die Abrechnung für die Speicherung und die geschützten Instanzen.
- Die Abrechnung erfolgt so lange, wie Sicherungsdaten für den virtuellen Computer in einem Tresor gespeichert werden. Wenn Sie den Schutz des virtuellen Computers beenden, aber VM-Sicherungsdaten in einem Tresor vorhanden sind, wird die Abrechnung fortgesetzt.
- Die Abrechnung für eine VM endet erst, nachdem der Schutz beendet wurde und alle Sicherungsdaten gelöscht wurden.
- Wenn der Schutz beendet wird und es keine aktiven Sicherungsaufträge gibt, wird die Größe der letzten erfolgreichen VM-Sicherung zur Größe der geschützten Instanz für die monatliche Rechnung.
- Die Berechnung der geschützten Instanzen basiert auf der *tatsächlichen* Größe des virtuellen Computers (also auf der Summe aller Daten auf dem virtuellen Computer – mit Ausnahme des temporären Speichers).
- Die Kosten basieren auf der tatsächlichen Menge der Daten, die auf dem Datenträger gespeichert sind.
- Die Kosten der Sicherung virtueller Computer basieren nicht auf der maximal unterstützten Größe für jeden an den virtuellen Computer angefügten Datenträger.
- Gleichermaßen basiert die Abrechnung der Sicherungsspeicherung auf der Menge der in Azure Backup gespeicherten Daten, d.h. auf der Summe der tatsächlichen Daten an jedem Wiederherstellungspunkt.

Beispiel: virtueller Computer mit A2-Standardgröße und zwei zusätzlichen Datenträgern mit einer maximalen Größe von jeweils 4 TB. In der folgenden Tabelle sind die auf jedem Datenträger tatsächlich gespeicherten Daten aufgeführt:

| Datenträgertyp | Max. Größe | Tatsächliche Daten |
| --------- | -------- | ----------- |
| Betriebssystem-Datenträger |4095 GB |17 GB |
| Lokaler Datenträger / temporärer Datenträger |135 GB |5 GB (bei der Sicherung nicht enthalten) |
| Datenträger 1 |4095 GB |30 GB |
| Datenträger 2 |4095 GB |0 GB |

- Die tatsächliche Größe der VM in diesem Fall ist 17 GB + 30 GB + 0 GB = 47 GB.
- Diese Größe der geschützten Instanz (47 GB) wird zur Basis für die monatliche Rechnung.
- Mit zunehmender Datenmenge auf dem virtuellen Computer ändert sich entsprechend auch die Größe der geschützten Instanz, die für die Abrechnung verwendet wird.


## <a name="next-steps"></a>Nächste Schritte

Gehen Sie nach Überprüfung des Sicherungsprozesses und der Überlegungen zur Leistung wie folgt vor:

- [Erfahren Sie mehr über](../virtual-machines/windows/premium-storage-performance.md) das Optimieren von Apps für eine optimale Leistung mit Azure-Speicher. Wenngleich der Schwerpunkt des Artikels auf Storage Premium liegt, gilt er auch für Standard-Datenträger.
- [Machen Sie die ersten Schritte](backup-azure-arm-vms-prepare.md) mit dem Backup-Dienst, indem Sie die Unterstützung für und Einschränkungen von VMs überprüfen, einen Tresor erstellen und VMs auf die Sicherung vorbereiten.
