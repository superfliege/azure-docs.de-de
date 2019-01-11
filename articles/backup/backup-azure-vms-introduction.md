---
title: Informationen zur Sicherung von Azure-VMs
description: Erfahren Sie mehr über die Sicherung von Azure-VMs und einige bewährte Methoden.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: raynew
ms.openlocfilehash: 9a80671a72f059e24a8cebc5de803af9261ad829
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53743951"
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
4. Die Sicherungserweiterung erstellt eine anwendungskonsistente Momentaufnahme auf Speicherebene.
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

- **Windows-VMs:** Bei Windows-VMs koordiniert sich der Azure Backup-Dienst mit dem Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um eine konsistente Momentaufnahme der VM-Datenträger zu erhalten.
    - Standardmäßig führt Azure Backup vollständig VSS-Sicherungen durch. [Weitere Informationen](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)
    - Wenn Sie die Einstellung so ändern möchten, dass Azure Backup Sicherungen von VSS-Kopien erstellt, legen Sie den folgenden Registrierungsschlüssel fest:
        ```
        [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
        ""USEVSSCOPYBACKUP"="TRUE"
        ```
- **Linux-VMs:** Um sicherzustellen, dass Ihre Linux-VMs App-konsistent sind, wenn Azure Backup eine Momentaufnahme erstellt, können Sie das Pre-Skript- und Post-Skript-Framework von Linux verwenden. Sie können eigene benutzerdefinierte Skripts zur Gewährleistung der Konsistenz beim Erstellen einer VM-Momentaufnahme schreiben.
    -  Azure Backup ruft nur von Ihnen geschriebene Pre- und Post-Skripts auf.
    - Wenn das Pre-Skript und das Post-Skript erfolgreich ausgeführt werden, markiert Azure Backup den Wiederherstellungspunkt als anwendungskonsistent. Allerdings sind Sie bei Verwendung benutzerdefinierter Skripts letztendlich für die Anwendungskonsistenz verantwortlich.
    - [Erfahren Sie mehr](backup-azure-linux-app-consistent.md) zum Konfigurieren von Skripts.


#### <a name="consistency-types"></a>Konsistenztypen

In der folgenden Tabelle werden die verschiedenen Konsistenztypen erläutert.

**Momentaufnahme** | **VSS-basiert** | **Details** | **Wiederherstellung**
--- | --- | --- | ---
**Anwendungskonsistent** | Ja (nur Windows) | Anwendungskonsistente Sicherungen erfassen Speicherinhalte und ausstehende E/A-Vorgänge. Anwendungskonsistente Momentaufnahmen verwenden VSS Writer (oder Pre-/Post-Skript für Linux), der die Konsistenz der Anwendungsdaten gewährleistet, ehe eine Sicherung erfolgt. | Wenn Sie die Wiederherstellung mit einer anwendungskonsistenten Momentaufnahme durchführen, wird die VM hochgefahren. Es kommt weder zu einer Beschädigung noch zum Verlust von Daten. Die Apps starten in einem konsistenten Zustand.
**Dateisystemkonsistent** | Ja (nur Windows) |  Dateikonsistente Sicherungen bieten konsistente Sicherungen von Datenträgerdateien, indem eine Momentaufnahme aller Dateien gleichzeitig erstellt wird.<br/><br/> Azure Backup-Wiederherstellungspunkte sind für Folgendes dateikonsistent:<br/><br/> – Linux-VM-Sicherungen, die keine Pre-/Post-Skripts haben oder bei denen das Skript nicht erfolgreich war.<br/><br/> – Windows VM-Sicherungen, bei denen VSS fehlerhaft war. | Wenn Sie die Wiederherstellung mit einer anwendungskonsistenten Momentaufnahme durchführen, wird die VM hochgefahren. Es kommt weder zu einer Beschädigung noch zum Verlust von Daten. Apps müssen einen eigenen Reparaturmechanismus implementieren, um sicherzustellen, dass wiederhergestellte Daten konsistent sind.
**Absturzkonsistent** | Nein | Absturzkonsistenz liegt oftmals vor, wenn eine Azure-VM zum Zeitpunkt der Sicherung heruntergefahren wird.  Nur die Daten, die zum Zeitpunkt der Sicherung bereits auf dem Datenträger vorhanden sind, werden erfasst und gesichert.<br/><br/> Ein absturzkonsistenter Wiederherstellungspunkt garantiert keine Datenkonsistenz für das Betriebssystem oder die App. | Es gibt keine Garantien, aber normalerweise wird die VM hochgefahren. Anschließend erfolgt eine Datenträgerprüfung, um Beschädigungen zu beheben. Alle Daten im Arbeitsspeicher oder Schreibvorgänge, die nicht vollständig auf den Datenträger übertragen wurden, gehen verloren. Apps implementieren ihre eigene Datenüberprüfung. Falls bei einer Datenbank-App ein Transaktionsprotokoll beispielsweise Einträge enthält, die nicht in der Datenbank vorhanden sind, führt die Datenbanksoftware ein Rollback durch, bis die Daten konsistent sind.


## <a name="service-and-subscription-limits"></a>Grenzwerte für Dienste und Abonnements

Azure Backup weist verschiedene Grenzwerte für Abonnements und Tresore auf.

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>Sicherungsleistung

### <a name="disk-considerations"></a>Datenträgeraspekte

Azure Backup versucht, den Vorgang schnellstmöglich abzuschließen und verbraucht dabei so viele Ressourcen wie möglich.

- Zur Maximierung der Geschwindigkeit wird versucht, die einzelnen VM-Datenträger parallel zu sichern.
- Bei einer VM mit vier Datenträgern versucht der Dienst beispielsweise, alle vier Datenträger parallel zu sichern.
- Die Anzahl von Datenträgern, die gesichert werden, ist der entscheidende Faktor beim Bestimmen des Datenverkehrs einer Speicherkontosicherung.
- Sämtliche E/A-Vorgänge sind durch den *Zieldurchsatz für Einzelblobs* begrenzt, der auf 60 MB pro Sekunde beschränkt ist.
- Für jeden zu sichernden Datenträger liest Azure Backup die Blöcke auf dem Datenträger und speichert nur die geänderten Daten (inkrementelle Sicherung). Sie können die nachstehenden durchschnittlichen Durchsatzwerte verwenden, um die Zeit zu schätzen, die für die Sicherung eines Datenträgers einer bestimmten Größe benötigt wird.

    **Vorgang** | **Optimaler Durchsatz**
    --- | ---
    Erste Sicherung | 160 MBit/s |
    Inkrementelle Sicherung | 640 MBit/s  <br><br> Der Durchsatz sinkt erheblich, wenn die Deltadaten auf den Datenträger verstreut sind.|



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


- **Erste Sicherung eines zu einer bereits geschützten VM neu hinzugefügten Datenträgers**: Wenn für eine VM eine inkrementelle Sicherung erfolgt, kann es vorkommen, dass beim Hinzufügen eines neuen Datenträgers abhängig von dessen Größe die SLA von einem Tag nicht eingehalten wird.
- **Fragmentierte App**: Wenn eine App nicht ordnungsgemäß konfiguriert ist, lässt sie sich ggf. nicht optimal speichern:
    - Wenn die Momentaufnahme viele kleine, fragmentierte Schreibvorgänge enthält, verbringt der Dienst zusätzliche Zeit mit dem Verarbeiten der Daten, die von den Anwendungen geschrieben wurden.
    - Bei Anwendungen, die im virtuellen Computer ausgeführt werden, beträgt der von Azure empfohlene Anwendungsschreibblock mindestens 8 KB. Wenn Ihre Anwendung einen Block von weniger als 8 KB verwendet, wird die Sicherungsleistung beeinflusst.
- **Überlastetes Speicherkonto**: Eine Sicherung kann geplant werden, wenn die App in der Produktionsumgebung ausgeführt wird oder wenn mehr als fünf bis zehn Datenträger vom gleichen Speicherkonto gehostet werden.
- **Konsistenzüberprüfungsmodus**: Bei Datenträgern mit mehr als 1 TB kann sich die Sicherung aus verschiedenen Gründen im Konsistenzüberprüfungsmodus befinden:
    - Der verwaltete Datenträger wird als Teil des VM-Neustarts verschoben.
    - Die Momentaufnahme wird zu Basisblob höher gestuft.


## <a name="restore-considerations"></a>Aspekte bei der Wiederherstellung

Eine Wiederherstellung umfasst zwei zentrale Vorgänge: Kopieren von Daten aus dem Tresor in das ausgewählte Speicherkonto und Erstellen des virtuellen Computers. Die benötigte Zeit zum Kopieren von Daten aus dem Tresor hängt vom Speicherort der Sicherungen in Azure und des Speicherkontos ab. Die Dauer des Datenkopiervorgangs hängt von folgenden Faktoren ab:

- **Wartezeit in Warteschlange**: Da der Dienst Wiederherstellungsaufträge von mehreren Speicherkonten gleichzeitig verarbeitet, werden Wiederherstellungsanforderungen in eine Warteschlange eingereiht.
- **Dauer des Datenkopiervorgangs**: Daten werden zuerst aus dem Tresor in das Speicherkonto kopiert. Die Wiederherstellungsdauer hängt vom IOPS-Wert und Durchsatz des ausgewählten Speicherkontos ab, das der Azure Backup-Dienst verwendet. Wählen Sie ein Speicherkonto aus, das nicht mit anderen Schreib- und Lesevorgängen anderer Anwendungen geladen ist, um die Zeit für das Kopieren zu verringern.

## <a name="best-practices"></a>Bewährte Methoden

Berücksichtigen Sie beim Konfigurieren von VM-Sicherungen die folgenden bewährten Methoden:

- Planen Sie keine Sicherungen für mehr als 100 VMs über einen Tresor zur gleichen Zeit.
- Planen Sie VM-Sicherungen für Nebenzeiten ein. Auf diese Weise nutzt der Backup-Dienst den IOPS-Wert zum Übertragen von Daten aus dem Speicherkonto in den Tresor.
- Wenn Sie verwaltete Datenträger sichern, übernimmt der Azure Backup-Dienst die Speicherverwaltung. Beachten Sie beim Sichern nicht verwalteter Datenträger Folgendes:
    - Wenden Sie unbedingt eine Sicherungsrichtlinie auf VMs an, die auf mehrere Speicherkonten verteilt sind.
    - Mit einem Sicherungszeitplan sollten maximal 20 Datenträger eines einzelnen Speicherkontos geschützt werden.
    - Wenn ein Speicherkonto mehr als 20 Datenträger enthält, verteilen Sie die entsprechenden virtuellen Computer auf mehrere Richtlinien, damit in der Übertragungsphase der Sicherung die erforderlichen IOPS zur Verfügung stehen.
    - Stellen Sie eine in Storage Premium ausgeführte VM nicht unter dem gleichen Speicherkonto wieder her. Wenn der Wiederherstellungsvorgang zur gleichen Zeit stattfindet wie der Sicherungsvorgang, verringern sich dadurch die für die Sicherung verfügbaren IOPS.
    - Für die Sicherung von Premium-VMs im VM-Sicherungsstapel V1 sollten nur 50 % des gesamten Speicherplatzes des Speicherkontos zugeordnet werden, sodass der Backup-Dienst die Momentaufnahme in das Speicherkonto kopieren und Daten von diesem Kopierspeicherort im Speicherkonto in den Tresor übertragen kann.
- Es wird empfohlen, verschiedene Speicherkonten anstelle der derselben Speicherkonten zu verwenden, um VMs aus einem einzigen Tresor wiederherzustellen. Dadurch wird eine Drosselung verhindert und eine vollständige Wiederherstellung mit guter Leistung erreicht.
- Die Wiederherstellung von der Speicherebene im Tarif 1 erfolgt binnen Minuten, während die Wiederherstellung von der Speicherebene im Tarif 2 einige Stunden dauert. Wir empfehlen zur Beschleunigen von Wiederherstellungen die Verwendung des [Features der sofortigen Wiederherstellungspunkte](backup-upgrade-to-vm-backup-stack-v2.md). Dieses steht nur für verwaltete Azure-VMs zur Verfügung.


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

- Laden Sie das [Excel-Arbeitsblatt zur Kapazitätsplanung](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) herunter, um verschiedene Einstellungen für Datenträger und Zeitplanung auszuprobieren.
- [Erfahren Sie mehr über](../virtual-machines/windows/premium-storage-performance.md) das Optimieren von Apps für eine optimale Leistung mit Azure-Speicher. Wenngleich der Schwerpunkt des Artikels auf Storage Premium liegt, gilt er auch für Standard-Datenträger.
- [Machen Sie die ersten Schritte](backup-azure-arm-vms-prepare.md) mit dem Backup-Dienst, indem Sie die Unterstützung für und Einschränkungen von VMs überprüfen, einen Tresor erstellen und VMs auf die Sicherung vorbereiten.
