---
title: Azure Backup-Architektur
description: Übersicht über die Architektur, die Komponenten und die Prozesse des Azure Backup-Diensts.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: raynew
ms.openlocfilehash: 4be483994bd7bc5bd97b1e59df230f66e9b4e24e
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430345"
---
# <a name="azure-backup-architecture"></a>Azure Backup-Architektur

Sie können den [Azure Backup-Dienst](backup-overview.md) verwenden, um Daten in der Microsoft Azure-Cloud zu sichern. In diesem Artikel werden die Architektur, die Komponenten und die Prozesse von Azure Backup zusammengefasst. 


## <a name="what-does-azure-backup-do"></a>Welche Funktion hat Azure Backup?

Azure Backup sichert Daten, den Computerzustand und Workloads, die auf lokalen Computern und auf virtuellen Azure-Computern ausgeführt werden. Der Dienst kann in verschiedenen Szenarien eingesetzt werden:

## <a name="how-does-azure-backup-work"></a>Wie funktioniert Azure Backup?

Zum Sichern von Computern und Daten gibt es verschiedene Methoden.

- **Sichern lokaler Computer:**
    - Sie können lokale Windows-Computer direkt in Azure mit dem MARS-Agent (Microsoft Azure Recovery Services) von Azure Backup sichern. Linux-Computer werden nicht unterstützt.
    - Sie können lokale Computer auf einem Sicherungsserver (System Center Data Protection Manager (DPM) oder Microsoft Azure Backup Server (MABS)), und dann den Sicherungsserver in einem Azure Backup Recovery Services-Tresor in Azure sichern.
- **Sichern virtueller Azure-Computer:**
    - Azure-VMs können direkt gesichert werden. Azure Backup installiert eine Sicherungserweiterung für den Azure-VM-Agent, der auf der VM ausgeführt wird. Damit wird die gesamte VM gesichert.
    - Sie können bestimmte Dateien und Ordner auf der Azure-VM sichern, indem Sie den MARS-Agent ausführen.
    - Sie können Azure-VMs auf MABS sichern, die in Azure ausgeführt werden, und dann wiederum MABS im Tresor sichern.

Weitere Informationen zu sicherbaren Elementen finden Sie [hier](backup-overview.md). Weitere Informationen zu unterstützten Sicherungsszenarien finden Sie [hier](backup-support-matrix.md).


## <a name="where-is-data-backed-up"></a>Wo werden die Daten gesichert?

Azure Backup speichert gesicherte Daten in einem Recovery Services-Tresor. Ein Tresor ist eine Onlinespeicherentität in Azure, die zum Speichern von Daten wie Sicherungskopien, Wiederherstellungspunkten und Sicherungsrichtlinien verwendet wird.

- Tresore vereinfachen die Organisation Ihrer Sicherungsdaten und minimieren gleichzeitig den Verwaltungsaufwand.
- In jedem Azure-Abonnement können bis zu 500 Tresore erstellt werden.
- Sie können gesicherte Elemente in einem Tresor überwachen (einschließlich virtueller Azure-Computer und lokaler Computer).
- Der Zugriff auf den Tresor kann mithilfe der [rollenbasierten Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) (Role-Based Access Control, RBAC) von Azure verwaltet werden.
- Sie können angeben, wie die Daten im Tresor repliziert werden sollen, um für Redundanz zu sorgen:
    - **LRS:** Sie können lokal redundanten Speicher (LRS) verwenden, um gegen Ausfälle in einem Datencenter gewappnet zu sein. LRS repliziert Daten in einer Speicherskalierungseinheit. [Weitere Informationen](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs)
    - **GRS:** Sie können georedundanten Speicher (GRS) verwenden: Bietet Schutz bei regionsweiten Ausfällen. Bei dieser Option werden Ihre Daten in einer sekundären Region repliziert. [Weitere Informationen](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs) 
    - Recovery Services-Tresore für Backup nutzen standardmäßig GRS. 




### <a name="backup-agents"></a>Sicherungs-Agents

Azure Backup bietet verschiedene Agents für unterschiedliche Sicherungsarten.

**Agent** | **Details** 
--- | --- 
**Microsoft Azure Recovery Services-Agent (MARS)** | 1) Wird zum Sichern von Dateien, Ordnern und Systemstatus auf einzelnen lokalen Windows-Servern ausgeführt.<br/><br/> 2) Wird auf Azure-VMs zum Sichern von Dateien, Ordnern und dem Systemstatus ausgeführt.<br/><br/>  3) Wird auf DPM-/MABS-Servern ausgeführt, um den lokalen DPM-/MABS-Speicherdatenträger in Azure zu sichern. 
**Azure-VM-Erweiterung** | Wird auf Azure-VMs ausgeführt, um sie in einem Tresor zu sichern.


## <a name="backup-types"></a>Sicherungstypen

**Sicherungstyp** | **Details** | **Verwendung**
--- | --- | ---
**Vollständig** | Eine Sicherung enthält die gesamte Datenquelle. Vollständige Sicherungen beanspruchen mehr Netzwerkbandbreite. | Wird für die erste Sicherung verwendet.
**Differenziell** |  Speichert die Blöcke, die sich seit der ersten vollständigen Sicherung geändert haben. Beansprucht weniger Netzwerk- und Speicherressourcen und speichert keine redundanten Kopien von Daten, die sich nicht geändert haben.<br/><br/> Ineffizient, da auch Datenblöcke, die sich zwischen nachfolgenden Sicherungen nicht geändert haben, übertragen und gespeichert werden. | Wird von Azure Backup nicht verwendet.
**Inkrementell** | Hohe Speicher- und Netzwerkeffizienz. Speichert nur Datenblöcke, die sich seit der vorherigen Sicherung geändert haben. <br/><br/> Bei der inkrementellen Sicherung müssen keine zusätzlichen vollständigen Sicherungen erstellt werden. | Wird von DPM/MABS für Datenträgersicherungen sowie bei allen Sicherungen in Azure verwendet.

## <a name="sql-server-backup-types"></a>Typen von SQL Server-Sicherungen

**Sicherungstyp** | **Details** | **Verwendung**
--- | --- | ---
**Vollständige Sicherung** | Die gesamte Datenbank wird gesichert. Diese enthält alle Daten in einer bestimmten Datenbank oder einem bestimmten Satz von Dateigruppen oder Dateien sowie ausreichende Protokolle, um diese Daten wiederherzustellen. | Pro Tag kann höchstens eine vollständige Sicherung ausgelöst werden.<br/><br/> Sie können wählen, ob Sie eine vollständige Sicherung in einem täglichen oder wöchentlichen Intervall durchführen möchten.
**Differenzielle Sicherung** | Diese Sicherung basiert auf der letzten vollständigen Datensicherung.<br/><br/> Damit werden nur die Daten erfasst, die seit der vollständigen Sicherung geändert wurden. |  Pro Tag kann höchstens eine differenzielle Sicherung ausgelöst werden.<br/><br/> Sie können eine vollständige Sicherung und eine differenzielle Sicherung nicht am gleichen Tag konfigurieren.
**Sicherung des Transaktionsprotokolls** | Eine Zeitpunktwiederherstellung ist bis zu einer bestimmten Sekunde möglich. | Transaktionsprotokollsicherungen können höchstens alle 15 Minuten durchgeführt werden.


### <a name="comparison"></a>Vergleich

Speicherverbrauch, RTO (Recovery Time Objective) und Netzwerkauslastung variieren je nach Art der Sicherung. Die Abbildung weiter unten zeigt einen Vergleich der Sicherungsarten.
- Die Datenquelle A besteht aus zehn Speicherblöcken (A1–A10), die monatlich gesichert werden.
- Die Blöcke A2, A3, A4 und A9 ändern sich im ersten Monat, der Block A5 ändert sich im nächsten Monat.
- Bei einer differenziellen Sicherung werden im zweiten Monat die geänderten Blöcke A2, A3, A4 und A9 gesichert. Im dritten Monat werden die gleichen Blöcke erneut gesichert – zusammen mit dem geänderten Block A5. Die geänderten Blöcke werden bis zur nächsten vollständigen Sicherung immer wieder gesichert.
- Bei einer inkrementellen Sicherung werden nach der vollständigen Sicherung für den ersten Monat die Blöcke A2, A3, A4 und A9 als geändert gekennzeichnet und in den zweiten Monat übertragen. Im dritten Monat wird nur der geänderte Block A5 gekennzeichnet und übertragen. 

![Vergleichsdarstellung von Sicherungsmethoden](./media/backup-architecture/backup-method-comparison.png)

## <a name="backup-features"></a>Sicherungsfeatures

Die folgende Tabelle enthält eine Zusammenfassung der Features für die verschiedenen Sicherungsarten:

**Feature** | **Lokale Windows-Computer (direkt)** | **Virtuelle Azure-Computer** | **Computer/Apps mit DPM/MABS**
--- | --- | --- | ---
Sicherung in einem Tresor | ![Ja][green] | ![Ja][green] | ![Ja][green] 
Sicherung auf DPM-/MABS-Datenträger und anschließend in Azure | | | ![Ja][green] 
Komprimierung der gesendeten Daten für die Sicherung | ![Ja][green] | Daten werden unkomprimiert übertragen. Etwas höherer Speicherbedarf, aber schnellere Wiederherstellung.  | ![Ja][green] 
Inkrementelle Sicherung |![Ja][green] |![Ja][green] |![Ja][green] 
Sicherung deduplizierter Datenträger | | | ![Teilweise][yellow]<br/><br/> Nur für lokal bereitgestellte DPM-/MABS-Server 

![Tabellenschlüssel](./media/backup-architecture/table-key.png)





## <a name="architecture-direct-backup-of-azure-vms"></a>Architektur: Direkte Sicherung virtueller Azure-Computer

1. Wenn Sie die Sicherung für einen virtuellen Azure-Computer aktivieren, wird gemäß dem von Ihnen angegebenen Zeitplan eine Sicherung ausgeführt.
2. Während der ersten Sicherung wird auf dem virtuellen Computer eine Sicherungserweiterung installiert, sofern er ausgeführt wird.
    - Für virtuelle Windows-Computer wird die Erweiterung „VMSnapshot“ installiert.
    - Für virtuelle Linux-Computer wird die Erweiterung „VMSnapshot Linux“ installiert.
3. Die Erweiterung erstellt eine Momentaufnahme auf Speicherebene. 
    - Für ausgeführte virtuelle Windows-Computer erstellt Backup in Zusammenarbeit mit VSS eine App-konsistente Momentaufnahme des virtuellen Computers. Backup erstellt standardmäßig vollständige VSS-Sicherungen. Sollte von Backup keine App-konsistente Momentaufnahme erstellt werden können, wird eine dateikonsistente Momentaufnahme erstellt.
    - Für virtuelle Linux-Computer erstellt Backup eine dateikonsistente Sicherung. Für App-konsistente Momentaufnahmen ist eine manuelle Anpassung von Pre- und Post-Skripts erforderlich.
    - Zur Optimierung von Backup werden die einzelnen VM-Datenträger parallel gesichert. Für jeden zu sichernden Datenträger liest Azure Backup die Blöcke auf dem Datenträger und speichert nur die geänderten Daten. 
4. Nachdem die Momentaufnahme erstellt wurde, werden die Daten in den Tresor übertragen. 
    - Es werden nur Datenblöcke kopiert, die sich seit der letzten Sicherung geändert haben.
    - Die Daten werden nicht verschlüsselt. Azure Backup kann virtuelle Azure-Computer sichern, die mit Azure Disk Encryption (ADE) verschlüsselt wurden.
    - Momentaufnahmedaten werden möglicherweise nicht sofort in den Tresor kopiert. Zu Spitzenzeiten vergehen unter Umständen mehrere Stunden. Bei täglichen Sicherungsrichtlinien beträgt die Gesamtdauer der Sicherung eines virtuellen Computers weniger als 24 Stunden.
5. Nachdem die Daten an den Tresor gesendet wurden, wird die Momentaufnahme entfernt und ein Wiederherstellungspunkt erstellt.

Beachten Sie, dass Azure-VMs für Steuerbefehle einen Internetzugang benötigen. Wenn Sie auf Workloads innerhalb der VM sichern (z.B. SQL Server-Sicherung), benötigen die Sicherungsdaten auch einen Internetzugang. 

![Sicherung virtueller Azure-Computer](./media/backup-architecture/architecture-azure-vm.png)

## <a name="architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders"></a>Architektur: Direkte Sicherung lokaler Windows-Computer/Azure-VM-Dateien/Ordner

1. Für dieses Szenario müssen Sie den MARS-Agent (Microsoft Azure Recovery Services) herunterladen und auf dem Computer installieren. Außerdem müssen Sie auswählen, was gesichert werden soll, wann Sicherungen erfolgen sollen und wie lange sie in Azure aufbewahrt werden sollen.
2. Die erste Sicherung wird gemäß Ihren Sicherungseinstellungen ausgeführt.
3. Der MARS-Agent verwendet den Windows-Volumeschattenkopie-Dienst (VSS), um eine Momentaufnahme der zu sichernden Volumes zu erstellen.
    - Der MARS-Agent verwendet bei der Erstellung der Momentaufnahme nur den Writer des Windows-Systems.
    - Der Agent verwendet keine anwendungsbezogenen VSS Writer und erfasst somit keine App-konsistenten Momentaufnahmen.
3. Nach Erstellung der Momentaufnahme mit VSS erstellt der MARS-Agent eine virtuelle Festplatte in dem Cacheordner, den Sie im Rahmen der Sicherungskonfiguration angegeben haben, und speichert Prüfsummen für die einzelnen Datenblöcke. 
4. Inkrementelle Sicherungen werden gemäß dem angegebenen Zeitplan ausgeführt – es sei denn, Sie führen eine Ad-hoc-Sicherung aus.
5. Bei inkrementellen Sicherungen werden geänderte Dateien identifiziert, und eine neue virtuelle Festplatte wird erstellt. Sie wird komprimiert, verschlüsselt und an den Tresor gesendet.
6. Nach Abschluss der inkrementellen Sicherung wird die neue virtuelle Festplatte mit der virtuellen Festplatte zusammengeführt, die nach der ersten Replikation erstellt wurde. So erhalten Sie den neuesten Zustand zum Vergleich für die laufende Sicherung. 

![Sicherung eines lokalen Windows-Servers mit MARS-Agent](./media/backup-architecture/architecture-on-premises-mars.png)

## <a name="architecture-back-up-to-dpmmabs"></a>Architektur: Sicherung mit DPM/MABS

1. Sie installieren den DPM- oder MABS-Schutz-Agent auf Computern, die Sie schützen möchten, und fügen die Computer einer DPM-Schutzgruppe hinzu.
    - Zum Schutz von lokalen Computern muss sich der DPM- oder MABS-Server in Ihrer lokalen Umgebung befinden.
    - Zum Schutz von Azure-VMs muss sich der MABS-Server in Azure befinden und als Azure-VM ausgeführt werden.
    - Mit DPM/MABS können Sie Sicherungsvolumes, Freigaben, Dateien und Ordner schützen. Sie können Computer (Systemstatussicherung/Bare-Metal-Sicherung) sowie spezifische Apps mit App-fähigen Sicherungseinstellungen schützen.
2. Wenn Sie den Schutz für einen Computer oder eine App in DPM/MABS einrichten, wählen Sie aus, dass eine kurzzeitige Sicherung auf dem lokalen DPM-/MABS-Datenträger und eine Sicherung in Azure (Onlineschutz) erstellt werden sollen. Außerdem geben Sie an, wann die Sicherung im lokalen DPM-/MABS-Speicher und wann die Onlinesicherung in Azure ausgeführt werden soll.
3. Der Datenträger der geschützten Workload wird gemäß dem angegebenen Zeitplan auf den lokalen MABS-/DPM-Datenträgern gesichert.
4. Der MARS-Agent, der auf dem DPM-/MABS-Server ausgeführt wird, sichert die DPM-/MABS-Datenträger im Tresor.

![Sicherung von Computern/Workloads mit DPM- oder MABS-Schutz](./media/backup-architecture/architecture-dpm-mabs.png)







## <a name="azure-vm-storage"></a>Azure-VM-Speicher

- Betriebssystem, Apps und Daten von virtuellen Azure-Computern werden auf Datenträgern gespeichert.
- Virtuelle Azure-Computer verfügen über mindestens zwei Datenträger: einen Betriebssystemdatenträger und einen temporären Datenträger. Darüber hinaus können auch Datenträger für App-Daten vorhanden sein. Datenträger werden als VHDs gespeichert.
- VHDs werden als Seitenblobs in Azure-Speicherkonten (Standard oder Premium) gespeichert.
    - Storage Standard: Zuverlässige, kostengünstige Datenträgerunterstützung für virtuelle Computer ohne wartezeitempfindliche Workloads. Storage Standard kann SSD Standard-Datenträger oder HDD Standard-Datenträger verwenden.
    - Storage Premium: Unterstützung von Hochleistungsdatenträgern. Verwendet SSD Premium-Datenträger.
- Für Datenträger sind verschiedene Leistungsstufen verfügbar:
    - HDD Standard-Datenträger: Basiert auf Festplattenlaufwerken und ermöglicht eine kostengünstige Speicherung.
    - SSD Standard-Datenträger: Kombiniert SSD Premium-Datenträger mit HDD Standard-Datenträgern und bietet im Vergleich zu HDD eine konsistentere Leistung und Zuverlässigkeit, ist aber immer noch kostengünstig.
    - SSD Premium-Datenträger: Bietet auf Basis von SSD hohe Leistung und geringe Wartezeit für virtuelle Computer mit E/A-intensiven Workloads.
- Datenträger können verwaltet oder nicht verwaltet sein:
    - Nicht verwaltete Datenträger: Herkömmliche Arten von Datenträgern, die von virtuellen Computern verwendet werden. Bei diesen Datenträgern erstellen Sie Ihr eigenes Speicherkonto und geben es beim Erstellen des Datenträgers an. Sie müssen sich selbst um die Maximierung der Speicherressourcen für Ihre virtuellen Computer kümmern.
    - Verwaltete Datenträger: Azure nimmt Ihnen die Erstellung und Verwaltung von Speicherkonten ab. Sie geben die Datenträgergröße und die Leistungsstufe an, und Azure erstellt und verwaltet die Datenträger für Sie. Wenn Sie Datenträger hinzufügen und virtuelle Computer skalieren, kümmert sich Azure um den Speicher.

Weitere Informationen:

- Informieren Sie sich über Datenträgerspeicher für virtuelle Computer unter [Windows](../virtual-machines/windows/managed-disks-overview.md) und [Linux](../virtual-machines/linux/managed-disks-overview.md).
- Erfahren Sie mehr über die verfügbaren [Datenträgertypen](../virtual-machines/windows/disks-types.md), wie Standard und Premium.


### <a name="backing-up-and-restoring-azure-vms-with-premium-storage"></a>Sichern und Wiederherstellen virtueller Azure-Computer mit Storage Premium 

Sie können virtuelle Azure-Computer mit Storage Premium und Azure Backup sichern:

- Beim Sichern virtueller Computer mit Storage Premium erstellt der Backup-Dienst im Speicherkonto einen temporären Stagingspeicherort namens „AzureBackup-“. Die Größe des Stagingspeicherorts entspricht der Größe der Momentaufnahme des Wiederherstellungspunkts.
- Stellen Sie sicher, dass das Premium-Speicherkonto über genügend freien Speicherplatz für den temporären Stagingspeicherort verfügt. [Weitere Informationen](../storage/common/storage-scalability-targets.md#premium-storage-account-scale-limits) Ändern Sie den Stagingspeicherort nicht.
- Nach Abschluss des Sicherungsauftrags wird der Stagingspeicherort gelöscht.
- Die Kosten für den Speicher, der für den Stagingspeicherort genutzt wird, entsprechen den [Preisen für Storage Premium](../virtual-machines/windows/disks-types.md#billing).

Virtuelle Azure-Computer mit Storage Premium können in Storage Premium oder in Storage Standard wiederhergestellt werden. Üblicherweise wird Storage Premium als Wiederherstellungsziel verwendet. Es kann jedoch kostengünstiger sein, Storage Standard als Wiederherstellungsziel zu verwenden, wenn Sie nur eine Teilmenge der Dateien des virtuellen Computers benötigen.

### <a name="backing-up-and-restoring-managed-disks"></a>Sichern und Wiederherstellen verwalteter Datenträger

Sie können virtuelle Azure-Computer mit verwalteten Datenträgern sichern.
- Virtuelle Computer mit verwalteten Datenträgern werden auf die gleiche Weise gesichert wie andere virtuelle Azure-Computer. Sie können den virtuellen Computer direkt über die Einstellungen des virtuellen Computers sichern oder die Sicherung für virtuelle Computer im Recovery Services-Tresor aktivieren.
- Virtuelle Computer können auf verwalteten Datenträgern über RestorePoint-Sammlungen gesichert werden, die auf verwalteten Datenträgern aufbauen.
- Azure Backup unterstützt auch das Sichern von virtuellen Computern mit verwalteten Datenträgern, die mithilfe von Azure Disk Encryption (ADE) verschlüsselt wurden.

Virtuelle Computer mit verwalteten Datenträgern können als vollständiger virtueller Computer mit verwalteten Datenträgern oder in einem Speicherkonto wiederhergestellt werden.
- Azure kümmert sich während des Wiederherstellungsvorgangs um die verwalteten Datenträger. Bei Verwendung der Speicherkontooption verwalten Sie das Speicherkonto, das im Rahmen der Wiederherstellung erstellt wird.
- Wenn Sie einen verschlüsselten verwalteten virtuellen Computer wiederherstellen, müssen die VM-Schlüssel und Geheimnisse im Schlüsseltresor vorhanden sein, bevor Sie den Wiederherstellungsvorgang starten.




## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Supportmatrix](backup-support-matrix.md) an, um Informationen zu unterstützten Features und Einschränkungen für Sicherungsszenarien zu erhalten.
- Richten Sie die Sicherung für eines der folgenden Szenarien ein:
    - [Sichern virtueller Azure-Computer](backup-azure-arm-vms-prepare.md)
    - [Direktes Sichern von Windows-Computern](tutorial-backup-windows-server-to-azure.md) (ohne Sicherungsserver)
    - [Einrichten von MABS](backup-azure-microsoft-azure-backup.md) für die Sicherung in Azure und anschließendes Sichern von Workloads per MABS
    - [Einrichten von DPM](backup-azure-dpm-introduction.md) für die Sicherung in Azure und anschließendes Sichern von Workloads per DPM


[green]: ./media/backup-architecture/green.png
[yellow]: ./media/backup-architecture/yellow.png
[red]: ./media/backup-architecture/red.png

