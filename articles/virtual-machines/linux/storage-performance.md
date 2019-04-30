---
title: Optimieren der Leistung virtueller Azure-Computer der Lsv2-Reihe – Speicher | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Leistung für Ihre Lösung auf virtuellen Computern der Lsv2-Reihe optimieren.
services: virtual-machines-linux
author: laurenhughes
manager: jeconnoc
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: joelpell
ms.openlocfilehash: 7be86c8934b8766217f9fca432327d254204f0c4
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012932"
---
# <a name="optimize-performance-on-the-lsv2-series-virtual-machines"></a>Optimieren der Leistung auf virtuellen Computern der Lsv2-Reihe

Virtuelle Computer der Lsv2-Reihe unterstützen verschiedenste Workloads, die hohe E/A- und Durchsatzanforderungen an den lokalen Speicher stellen und in einem breiten Spektrum von Anwendungen und Branchen zur Anwendung kommen.  Die Lsv2-Reihe eignet sich perfekt für Big Data, SQL, NoSQL-Datenbanken, Data Warehousing und große Transaktionsdatenbanken wie Cassandra, MongoDB, Cloudera und Redis.

Virtuelle Computer der Lsv2-Reihe nutzen den AMD EPYC™ 7551-Prozessor optimal aus, um die bestmögliche Leistung zwischen Prozessor, Arbeitsspeicher, NVMe-Geräten und den virtuellen Computern zu erzielen. Neben der Maximierung der Hardwareleistung erfüllen virtuelle Computer der Lsv2-Reihe auch die Anforderungen von Linux-Betriebssystemen, um eine bessere Leistung mit der Hardware und Software zu erreichen.

Im Zuge der Optimierung der Software und Hardware ist die optimierte Version von [Ubuntu 18.04 und 16.04 von Canonical](https://azuremarketplace.microsoft.com/marketplace/apps/Canonical.UbuntuServer?tab=Overview) entstanden, die Anfang Dezember 2018 im Azure Marketplace veröffentlicht wurde und maximale Leistung auf den NVMe-Geräten in virtuellen Computern der Lsv2-Reihe unterstützt.

Mit den Tipps und Vorschlägen in diesem Artikel können Sie sicherstellen, dass Ihre Workloads und Anwendungen die maximale Leistung erreichen, für die die virtuellen Computer konzipiert wurden. Die Informationen auf dieser Seite werden immer wieder aktualisiert, wenn dem Azure Marketplace weitere Lsv2-optimierte Images hinzugefügt werden.

## <a name="amd-eypc-chipset-architecture"></a>Architektur des AMD EYPC™-Chipsatzes

Die von virtuellen Computern der Lsv2-Reihe verwendeten AMD EYPC™-Serverprozessoren basierend auf der Zen-Mikroarchitektur. AMD hat Infinity Fabric (IF) für EYPC™ als skalierbare Interconnect-Lösung für das NUMA-Modell entwickelt, die für die On-Die-, On-Package- und Multi-Package-Kommunikation verwendet werden kann. Verglichen mit QPI (Quick-Path Interconnect) und UPI (Ultra-Path Interconnect), die bei modernen Intel-Prozessoren mit monolithischem Die zum Einsatz kommen, kann die AMD-Architektur mit vielen NUMA-Knoten und kleinem Die sowohl Leistungsvorteile als auch Herausforderungen mit sich bringen. Die tatsächlichen Auswirkungen von Arbeitsspeicherbandbreite und Wartezeiteneinschränkungen können je nach Art der ausgeführten Workloads variieren.

## <a name="tips-to-maximize-performance"></a>Tipps zur Leistungsmaximierung

* Wenn Sie ein benutzerdefiniertes Linux-Gastbetriebssystem für Ihre Workload hochladen, beachten Sie, dass der beschleunigte Netzwerkbetrieb standardmäßig **AUS** ist. Falls Sie den beschleunigten Netzwerkbetrieb aktivieren möchten, sollten Sie diesen Schritt bei der Erstellung des virtuellen Computers ausführen, um die bestmögliche Leistung zu erzielen.

* Die Hardware, die den virtuellen Computern der Lsv2-Reihe zugrunde liegt, nutzt NVMe-Geräte mit acht E/A-Warteschlangenpaaren (Queue Pairs, QPs). Jede E/A-Warteschlange für ein NVMe-Gerät ist eigentlich ein Paar und besteht aus einer Übermittlungs- und einer Fertigstellungswarteschlange. Der NVMe-Treiber ist für die Optimierung der Nutzung dieser acht E/A-Warteschlangenpaare eingerichtet und verteilt Eingaben/Ausgaben nach einem Roundrobin-Schema. Führen Sie daher pro Gerät acht Aufträge aus, um die maximale Leistung zu erzielen.

* Vermeiden Sie während aktiver Workloads eine Vermischung von NVMe-Verwaltungsbefehlen (etwa zum Abfragen von NVMe SMART-Informationen) und NVMe-E/A-Befehlen. Lsv2-NVMe-Geräte basieren auf Hyper-V NVMe Direct-Technologie, die in den langsamen Modus wechselt, sobald NVMe-Verwaltungsbefehle ausstehen. In der Folge bricht die NVMe-E/A-Leistung für Lsv2-Benutzern unter Umständen dramatisch ein.

* Lsv2-Benutzer sollten sich nicht auf NUMA-Geräteinformationen (alle 0) verlassen, die innerhalb des virtuellen Computers für Datenlaufwerke gemeldet werden, um die NUMA-Affinität für ihre Apps zu bestimmen. Zur Verbesserung der Leistung empfiehlt es sich, Workloads nach Möglichkeit auf CPUs aufzuteilen.

* Die maximal unterstützte Warteschlangenlänge pro E/A-Warteschlangenpaar beträgt für ein NVMe-Gerät eines virtuellen Computers der Lsv2-Reihe 1.024 (in Gegensatz zum Amazon i3-Limit von 32). Lsv2-Benutzer sollten ihre (synthetischen) Benchmark-Workloads auf eine maximale Warteschlangenlänge von 1.024 beschränken, um die Auslösung der Bedingungen bei voller Warteschlange zu vermeiden, da dies zu Leistungseinbußen führen kann.

## <a name="utilizing-local-nvme-storage"></a>Nutzen von lokalem NVMe-Speicher

Der lokale Speicher auf dem 1,92 TB großen NVMe-Datenträger aller virtuellen Lsv2-Computer ist flüchtig. Während eines erfolgreichen Standardneustarts des virtuellen Computers bleiben die Daten auf dem lokalen NVMe-Datenträger erhalten. Wird der virtuelle Computer neu bereitgestellt oder gelöscht oder seine Zuordnung aufgehoben, gehen die Daten auf dem NVMe-Datenträger verloren. Die Daten bleiben nicht erhalten, wenn der virtuelle Computer (oder die ihm zugrunde liegende Hardware) fehlerhaft wird. In diesem Fall werden sämtliche Daten auf dem alten Host sicher gelöscht.

Manchmal muss der virtuelle Computer auch auf einen anderen Hostcomputer verschoben werden (beispielsweise im Zuge einer geplanten Wartung). Geplante Wartungsarbeiten und einige Hardwarefehler lassen sich mithilfe von [Scheduled Events](scheduled-events.md) antizipieren. Verwenden Sie Scheduled Events, um sich hinsichtlich prognostizierter Wartungs- und Wiederherstellungsvorgänge auf dem Laufenden zu halten.

Sollte der virtuelle Computer im Zuge eines geplanten Wartungsereignisses auf einem neuen Host mit leeren lokalen Datenträgern neu erstellt werden müssen, müssen die Daten neu synchronisiert werden. (Auch hier werden sämtliche Daten auf dem alten Host sicher gelöscht.) Der Grund: Virtuelle Computer der Lsv2-Reihe unterstützen derzeit keine Livemigration zum lokalen NVMe-Datenträger.

Für die geplante Wartung stehen zwei Modi zur Verfügung:

### <a name="standard-vm-customer-controlled-maintenance"></a>Standardmäßige, kundenseitig gesteuerte VM-Wartung

- Der virtuelle Computer wird innerhalb eines 30-tägigen Zeitfensters auf einen aktualisierten Host verschoben.
- Daten im lokalen Lsv2-Speicher gehen unter Umständen verloren. Daher empfiehlt es sich, die Daten vor dem Ereignis zu sichern.

### <a name="automatic-maintenance"></a>Automatische Wartung

- Findet statt, wenn der Kunde die kundenseitig gesteuerte Wartung nicht ausführt, oder im Falle von Notfallmaßnahmen (etwa bei einem Zero-Day-Sicherheitsereignis).
- Hat die Beibehaltung der Kundendaten zum Ziel, es besteht jedoch ein geringes Risiko, dass der virtuelle Computer einfriert oder neu gestartet wird.
- Daten im lokalen Lsv2-Speicher gehen unter Umständen verloren. Daher empfiehlt es sich, die Daten vor dem Ereignis zu sichern.

Verwenden Sie bei anstehenden Dienstereignissen den gesteuerten Wartungsprozess, und wählen Sie einen Aktualisierungstermin, der für Sie am besten geeignet ist. Vor dem Ereignis können Sie Ihre Daten ggf. in Storage Premium sichern. Nach Abschluss des Wartungsereignisses können Sie Ihre Daten wieder im lokalen NVMe-Speicher der aktualisierten virtuellen Lsv2-Computer platzieren.

Daten bleiben unter anderem in folgenden Szenarien auf lokalen NVMe-Datenträgern erhalten:

- Der virtuelle Computer wird ausgeführt und ist fehlerfrei.
- Der virtuelle Computer wird von Ihnen oder von Azure direkt neu gestartet.
- Der virtuelle Computer wird angehalten (also beendet, ohne die Zuordnung aufzuheben).
- Beim Großteil der geplanten Wartungsvorgänge.

Zu den Szenarien, in denen die Daten zum Schutz des Kunden sicher gelöscht werden, zählen folgende:

- Der virtuelle Computer wird erneut bereitgestellt, beendet (Aufhebung der Zuordnung) oder von Ihnen gelöscht.
- Der virtuelle Computer wird fehlerhaft und muss aufgrund eines Hardwareproblems zur Dienstreparatur zu einem anderen Knoten migriert werden.
- Ein geringer Anteil der geplanten Wartungsvorgänge, bei denen der virtuelle Computer für die Wartung zu einem anderen Host migriert werden muss.

Weitere Informationen zu Optionen für die Sicherung von Daten im lokalen Speicher finden Sie unter [Sicherung und Notfallwiederherstellung für Azure IaaS-Datenträger](backup-and-disaster-recovery-for-azure-iaas-disks.md).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

* **Wie beginne ich mit der Bereitstellung virtueller Computer der Lsv2-Reihe?**  
   Sie können ähnlich wie bei anderen virtuellen Computern das [Portal](quick-create-portal.md), die [Azure-Befehlszeilenschnittstelle](quick-create-cli.md) oder [PowerShell](quick-create-powershell.md) verwenden, um einen virtuellen Computer erstellen.

* **Führt ein einzelner NVMe-Datenträgerfehler bereits zu einem Fehler bei allen virtuellen Computern auf dem Host?**  
   Sollte für den Hardwareknoten ein Datenträgerfehler erkannt werden, befindet sich die Hardware in einem fehlerhaften Zustand. In diesem Fall wird automatisch die Zuordnung aller virtuellen Computer auf dem Knoten aufgehoben, und die virtuellen Computer werden auf einen fehlerfreien Knoten verschoben. Bei virtuellen Computern der Lsv2-Reihe hat dies auch die sichere Löschung der Kundendaten auf dem fehlerhaften Knoten zur Folge, sodass die Daten vom Kunden auf dem neuen Knoten neu erstellt werden müssen. Solange für Lsv2 noch keine Livemigration zur Verfügung steht, werden die Daten auf dem fehlerhaften Knoten proaktiv mit den virtuellen Computern verschoben, wenn diese auf einen anderen Knoten übertragen werden.

* **Muss ich zur Optimierung der Leistung die Einstellung „rq_affinity“ anpassen?**  
   Bei der Einstellung „rq_affinity“ handelt es sich um eine geringfügige Anpassung bei Verwendung der absoluten Obergrenze für E/A-Vorgänge pro Sekunde (Input/Output Operations Per Second, IOPS). Wenn alles andere ordnungsgemäß funktioniert, können Sie versuchen, die Einstellung „rq_affinity“ auf „0“ festzulegen, um zu sehen, ob dies einen Unterschied macht.

* **Muss ich die Einstellungen vom Typ „blk_mq“ ändern?**  
   RHEL/CentOS 7.x verwendet automatisch „blk_mq“ für die NVMe-Geräte. Es sind keine Konfigurations- oder Einstellungsänderungen erforderlich. Die Einstellung „scsi_mod.use_blk_mq“ ist nur für SCSI relevant und wurde im Rahmen der Vorschauversion von Lsv2 verwendet, da die NVMe-Geräte auf den virtuellen Gastcomputern als SCSI-Geräte sichtbar waren. Aktuell sind die NVMe-Geräte als NVMe-Geräte sichtbar, womit die SCSI-spezifische Einstellung „blk_mq“ nicht mehr relevant ist.

* **Muss ich „fio“ ändern?**  
   Um in den VM-Größen „L64v2“ und „L80v2“ die maximalen IOPS mit einem Leistungsmesstool wie „fio“ ermitteln zu können, müssen Sie die Einstellung „rq_affinity“ auf jedem NVMe-Gerät auf „0“ festlegen.  Die folgende Befehlszeile legt beispielsweise „rq_affinity“ für alle zehn NVMe-Geräte eines virtuellen Computers vom Typ „L80v2“ auf Null fest:

   ```console
   for i in `seq 0 9`; do echo 0 >/sys/block/nvme${i}n1/queue/rq_affinity; done
   ```

   Beachten Sie außerdem, dass die bestmögliche Leistung erzielt wird, wenn E/A-Vorgänge direkt für jedes der unveränderten NVMe-Geräte (also ohne Partitionierung, ohne Dateisysteme, ohne RAID 0-Konfiguration usw.) ausgeführt werden. Vergewissern Sie sich vor dem Starten einer Testsitzung, dass sich die Konfiguration in einem bekannten neuen/bereinigten Zustand befindet, indem Sie `blkdiscard` auf jedem der NVMe-Geräte ausführen.
   
## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die Spezifikationen für alle [speicheroptimierten virtuellen Computer](sizes-storage.md) in Azure an.
