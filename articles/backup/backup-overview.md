---
title: Was ist Azure Backup?
description: Hier finden Sie eine Übersicht über den Azure Backup-Dienst und erfahren, wie Sie ihn im Rahmen Ihrer Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity & Disaster Recovery, BCDR) bereitstellen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 7156042243b1ba28cea712dc3722600b9fc46c42
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360627"
---
# <a name="what-is-azure-backup"></a>Was ist Azure Backup?

Der Azure Backup-Dienst dient zum Sichern von Daten in der Microsoft Azure-Cloud. Sie können sowohl lokale Computer und Workloads als auch virtuelle Azure-Computer (Virtual Machines, VMs) sichern.


## <a name="why-use-azure-backup"></a>Gründe für Azure Backup

Azure Backup bietet im Wesentlichen folgende Vorteile:

- **Auslagerung lokaler Sicherungen:** Azure Backup bietet eine einfache Lösung, mit der Sie Ihre lokalen Ressourcen in der Cloud sichern können. Mit Azure Backup erhalten Sie kurz- und langfristige Sicherungen, ohne komplexe lokale Sicherungslösungen einrichten zu müssen. Bandsicherungen und externe Sicherungen gehören damit der Vergangenheit an.
- **Sicherung virtueller Azure-IaaS-Computer:** Azure Backup bietet unabhängige und isolierte Sicherungen zum Schutz vor dem versehentlichen Löschen von Originaldaten. Sicherungen werden in einem Recovery Services-Tresor mit integrierten verwalteten Wiederherstellungspunkten gespeichert. Konfiguration und Skalierung sind unkompliziert. Sicherungen werden außerdem optimiert und können bei Bedarf problemlos wiederhergestellt werden.
- **Unkomplizierte Skalierung:** Dank der Leistung und unbegrenzten Skalierbarkeit der Azure-Cloud bietet Azure Backup Hochverfügbarkeit ohne Wartungs- oder Überwachungsaufwand. Sie können Warnungen einrichten, um Informationen zu Ereignissen bereitzustellen, aber Sie müssen sich nicht um die hohe Verfügbarkeit für Ihre Daten in der Cloud kümmern.
- **Unbegrenzte Datenübertragungen:** Die Menge an übertragenen eingehenden und ausgehenden Daten wird von Azure Backup nicht beschränkt. Außerdem fallen bei Azure Backup keine Gebühren für die übertragenen Daten an. Aber wenn Sie den Azure Import/Export-Dienst nutzen, um große Datenmengen zu importieren, werden für eingehende Daten Kosten berechnet. Weitere Informationen zu diesen Kosten finden Sie unter [Workflow zur Offlinesicherung in Azure Backup](backup-azure-backup-import-export.md). Ausgehende Daten sind Daten, die während eines Wiederherstellungsvorgangs aus einem Recovery Services-Tresor übertragen werden.
- **Datenschutz:** Die Datenverschlüsselung ermöglicht eine sichere Übertragung und Speicherung Ihrer Daten in der öffentlichen Cloud. Sie speichern die Passphrase für die Verschlüsselung lokal, und sie wird niemals in Azure übertragen oder gespeichert. Wenn Daten wiederhergestellt werden sollen, sind nur Sie im Besitz der Passphrase für die Verschlüsselung bzw. des Schlüssels.
- **Anwendungskonsistente Sicherungen:** Eine anwendungskonsistente Sicherung bedeutet, dass ein Wiederherstellungspunkt alle erforderlichen Daten zum Wiederherstellen der Sicherungskopie enthält. Azure Backup umfasst anwendungskonsistente Sicherungen, sodass sichergestellt ist, dass zum Wiederherstellen der Daten keine zusätzlichen Fixes benötigt werden. Durch die Wiederherstellung von anwendungskonsistenten Daten wird die Wiederherstellungsdauer reduziert, sodass Sie schnell zum Zustand der normalen Ausführung zurückkehren können.
- **Kurz- und Langzeitaufbewahrung:** **Langfristige Aufbewahrung:** Sie können Recovery Services-Tresore für kurzfristige und langfristige Aufbewahrung verwenden. Die Zeit, für die Sie Daten im Recovery Services-Tresor aufbewahren können, wird von Azure nicht begrenzt. Sie können Daten also beliebig lange in einem Tresor aufbewahren. Bei Azure Backup gilt pro geschützter Instanz ein Limit von 9999 Wiederherstellungspunkten. Informationen zu den möglichen Auswirkungen auf Ihre Sicherungsanforderungen finden Sie im Abschnitt [Sicherung und Aufbewahrung](backup-introduction-to-azure-backup.md#backup-and-retention).
- **Automatische Speicherverwaltung**: Für Hybridumgebungen ist häufig heterogener Speicher erforderlich – teilweise lokal und teilweise in der Cloud. Bei Azure Backup fallen keine Kosten für die Verwendung von lokalen Speichergeräten an. Azure Backup sorgt im Rahmen eines Modells mit nutzungsbasierter Bezahlung für die automatische Zuteilung und Verwaltung von Sicherungsspeicher. Nutzungsbasierte Bezahlung bedeutet, dass Sie nur für den Speicher zahlen, den Sie verbrauchen. Weitere Informationen finden Sie im [Artikel zu den Preisen von Azure](https://azure.microsoft.com/pricing/details/backup).
- **Mehrere Speicheroptionen**: Ein Aspekt der hohen Verfügbarkeit ist die Speicherreplikation. Azure Backup bietet zwei Arten der Replikation: mit [lokal redundantem Speicher](../storage/common/storage-redundancy-lrs.md) und mit [georedundantem Speicher](../storage/common/storage-redundancy-grs.md). Wählen Sie die Sicherungsspeicheroption aus, die zu Ihren Anforderungen passt:
    - Lokal redundanter Speicher (Locally Redundant Storage, LRS) repliziert Ihre Daten dreimal in einer Speicherskalierungseinheit in einem Datencenter. (Es werden also drei Kopien Ihrer Daten erstellt.) Alle Kopien der Daten befinden sich in derselben Region. LRS ist eine kostengünstige Möglichkeit, um Daten vor lokalen Hardwarefehlern zu schützen.
    - Geografisch redundanter Speicher (Geo-Redundant Storage, GRS) ist die standardmäßige und empfohlene Replikationsoption. GRS repliziert Ihre Daten in einer sekundären Region, die mehrere hundert Kilometer vom primären Speicherort der Quelldaten entfernt ist. GRS führt zu höheren Kosten als LRS, bietet aber eine höhere Haltbarkeit Ihrer Daten (auch im Falle eines regionalen Ausfalls).


## <a name="whats-the-difference-between-azure-backup-and-azure-site-recovery"></a>Was ist der Unterschied zwischen Azure Backup und Azure Site Recovery?

Der Azure Backup-Dienst und der Azure Site Recovery-Dienst tragen beide zur Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity & Disaster Recovery, BCDR) Ihres Unternehmens bei. BCDR hat zwei weit gefasste Ziele:

- Schützen Ihrer Geschäftsdaten und Ermöglichen der Wiederherstellung bei einem Ausfall
- Gewährleisten der Verfügbarkeit von Apps und Workloads während geplanter und ungeplanter Downtime

Beide Dienste bieten unterschiedliche Funktionen, die sich gegenseitig ergänzen.

- **Azure Site Recovery:** Site Recovery bietet eine Notfallwiederherstellungslösung für lokale Computer und virtuelle Azure-Computer. Hierzu werden Computer eines primären Standorts an einem sekundären Standort repliziert. Bei einem Notfall wird für die Computer ein Failover zum sekundären Standort ausgeführt, sodass Sie von dort aus auf sie zugreifen können. Wenn später alles wieder wie gewohnt funktioniert, wird für die Computer ein Failback ausgeführt, um sie am primären Standort wiederherzustellen.
- **Azure Backup:** Der Azure Backup-Dienst sichert Daten von lokalen Computern und virtuellen Azure-Computern. Daten können präzise gesichert und wiederhergestellt werden. So können Sie unter anderem Dateien, Ordner und den Systemstatus des Computers sichern sowie App-fähige Datensicherungen erstellen. Azure Backup ermöglicht einen präziseren Umgang mit Daten als Site Recovery. Wenn also beispielsweise eine Präsentation auf Ihrem Laptop beschädigt wurde, können Sie sie mithilfe von Azure Backup wiederherstellen. Mit Site Recovery können Sie eine VM-Konfiguration und die Daten des virtuellen Computers schützen und deren Verfügbarkeit sicherstellen.  

Die folgende Tabelle enthält hilfreiche Punkte, um Sie bei der Ermittlung Ihrer BCDR-Anforderungen zu unterstützen: 

**Ziel** | **Details** | **Vergleich**
--- | --- | --- | --- |
**Datensicherung/-aufbewahrung** | Sicherungsdaten können mehrere Tage, Monate oder sogar Jahre aufbewahrt werden, falls dies aus Compliancegründen erforderlich ist. | Mit Sicherungslösungen wie Azure Backup können Sie exakt die Daten auswählen, die Sie sichern möchten, und die Sicherungs- und Aufbewahrungsrichtlinien präzise anpassen.<br/><br/> Diese Präzision steht bei Site Recovery nicht zur Verfügung.
**Recovery Point Objective (RPO)** | Dies ist der Umfang des zulässigen Datenverlusts, falls eine Wiederherstellung durchgeführt werden muss. | Bei Sicherungen kann das RPO variieren.<br/><br/> VM-Sicherungen haben in der Regel ein RPO von einem Tag, während es bei Datenbanksicherungen durchaus bei lediglich 15 Minuten liegen kann.<br/><br/> Site Recovery bietet ein niedriges RPO, da die Replikation kontinuierlich oder häufig stattfindet, was für ein geringes Delta zwischen Quelle und Replikatkopie sorgt.
**Recovery Time Objective (RTO)** |Der Zeitraum, der für eine Wiederherstellung erforderlich ist. | Aufgrund des höheren RPO-Werts ist die Datenmenge, die eine Sicherungslösung verarbeiten muss, meist wesentlich größer, was zu längeren RTOs führt. Beispielsweise kann das Wiederherstellen von Daten von Bändern Tage dauern, was davon abhängt, wie lange der Transport des Bands von einem standortexternen Aufbewahrungsort dauert. | Notfallwiederherstellungslösungen wie Site Recovery zeichnen sich durch ein niedriges RPO aus, da das Ziel dank kontinuierlicher/häufiger Replikation in der Regel besser mit der Quelle synchronisiert ist. |

## <a name="what-backup-scenarios-are-supported"></a>Welche Sicherungsszenarien werden unterstützt?

Mit Azure Backup können sowohl lokale Computer als auch virtuelle Azure-Computer gesichert werden.

**Computer** | **Sicherungsszenario**
--- | ---
**Lokale Computer (physisch/virtuell)** |  Sie können einzelne lokale Computer sichern.<br/><br/>Sie können lokale Computer sichern, die durch System Center Data Protection Manager (DPM) geschützt sind.<br/><br/> Sie können lokale Computer sichern, die durch Microsoft Azure Backup Server (MABS) geschützt sind.
**Virtuelle Azure-Computer** | Sie können einzelne virtuelle Azure-Computer sichern.<br/><br/> Sie können virtuelle Azure-Computer sichern, die durch DPM oder MABS geschützt sind.

### <a name="back-up-servers"></a>Sichern von Servern

Es empfiehlt sich ggf., lokale Server und Workloads oder virtuelle Azure-Computer und deren Workloads zunächst auf einem Sicherungsserver und anschließend in einem Recovery Services-Tresor zu sichern. 

**Sicherungsserver** | **Details**
--- | ---
**System Center Data Protection Manager (DPM)** | Sie können Azure Backup verwenden, um durch DPM geschützte Daten zu sichern:<br/><br/> - DPM kann lokal (physisch oder virtuell) oder in Azure ausgeführt werden.<br/><br/> - Sie können verschiedene Arten von Daten auf lokalen Computern und virtuellen Azure-Computern schützen, indem Sie die Daten auf dem DPM-Server sichern.<br/><br/> Der DPM-Server kann wiederum mithilfe des Azure Backup-Diensts in einem Recovery Services-Tresor gesichert werden.<br/><br/> Der DPM-Server und die von ihm geschützten Computer müssen sich im gleichen Netzwerk befinden. Lokale Computer können nur mithilfe eines lokalen DPM-Servers geschützt werden. Analog dazu muss DPM zum Schutz virtueller Azure-Computer in Azure ausgeführt werden.
**Microsoft Azure Backup Server (MABS)** | Sie können Azure Backup verwenden, um durch MABS geschützte Daten zu sichern:<br/><br/> - MABS kann lokal (physisch oder virtuell) oder in Azure ausgeführt werden.<br/><br/> - Sie können verschiedene Arten von Daten auf lokalen Computern und virtuellen Azure-Computern schützen, indem Sie die Daten in MABS sichern.<br/><br/> - MABS kann wiederum mithilfe des Azure Backup-Diensts in einem Recovery Services-Tresor gesichert werden.<br/><br/> - MABS bietet ähnliche Funktionen wie DPM. Mit MABS sind jedoch keine Bandsicherungen möglich. Für MABS ist keine System Center-Lizenz erforderlich.<br/><br/> Genau wie bei DPM können lokale Computer nur mithilfe einer lokalen MABS-Instanz geschützt werden. Virtuelle Azure-Computer können nur durch MABS in Azure geschützt werden.

Wenn Daten zuerst mit DPM/MABS und anschließend in einem Tresor gesichert werden, hat das folgende Vorteile:

- Die Sicherung mit DPM/MABS ermöglicht neben Datei-/Ordner-/Volumesicherungen und Sicherungen des Computerzustands (Bare-Metal-/Systemstatussicherungen) auch App-fähige Sicherungen, die für gängige Apps wie SQL Server, Exchange und SharePoint optimiert sind.
- Sie müssen nicht auf jedem Computer, den Sie sichern möchten, den Azure Backup-Agent installieren. Auf den einzelnen Computern wird jeweils der DPM-/MABS-Schutz-Agent ausgeführt, und der Microsoft Azure Recovery Services-Agent von Azure Backup wird nur auf dem DPM-Server/in der MABS-Instanz ausgeführt.
- Sie profitieren von mehr Flexibilität und präziseren Planungsoptionen für die Sicherungsausführung.
- Sie können Sicherungen für mehrere Computer in einer zentralen Konsole verwalten und die Computer dazu in Schutzgruppen zusammenfassen. Das ist besonders hilfreich, wenn Apps auf mehrere Computer verteilt sind und gemeinsam gesichert werden sollen.

## <a name="what-can-be-backed-up"></a>Was kann gesichert werden?

**Computer** | **Sicherungsserver** | **Sicherung**
--- | --- | ---
Lokale virtuelle Windows-Computer | Keine Sicherung mit DPM oder MABS | Sicherung von Dateien, Ordnern und Systemstatus
Virtuelle Azure-Computer (Windows und Linux) | Keine Sicherung mit DPM oder MABS | Sicherung von Dateien, Ordnern und Systemstatus<br/><br/> Sicherungen für Windows-Computer sind App-fähig, Sicherungen für Linux-Computer sind dateifähig.
Lokale virtuelle Computer/virtuelle Azure-Computer | Durch DPM geschützt | Sicherung von allem, was durch DPM geschützt wird (einschließlich Dateien, Ordner, Freigaben, Volumes und App-spezifische Daten). [Erfahren Sie](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807), was DPM sichern kann.
Lokale virtuelle Computer/virtuelle Azure-Computer | Durch MABS geschützt | Sicherung von allem, was durch MABS geschützt wird (einschließlich Dateien, Ordner, Freigaben, Volumes und App-spezifische Daten). Was mit MABS gesichert werden kann, erfahren Sie [hier](backup-mabs-protection-matrix.md).

## <a name="what-backup-agents-do-i-need"></a>Welche Sicherungs-Agents benötige ich?
**Szenario** | **Agent** | **Details**
--- | --- | ---
Lokale Computer (kein Sicherungsserver) | Der MARS-Agent (Microsoft Azure Recovery Services) wird auf dem Windows-Computer ausgeführt. | Der MARS-Agent wird im Rahmen der Bereitstellung von Azure Backup heruntergeladen und installiert.<br/><br/> Diese Option wird nur für Windows-Computer unterstützt.
Virtuelle Azure-Computer (kein Sicherungsserver) | Kein expliziter Agent erforderlich. Die Azure-VM-Erweiterung für die Sicherung wird auf dem virtuellen Azure-Computer ausgeführt. | Die Erweiterung wird installiert, wenn Sie die erste Azure-VM-Sicherung ausführen.<br/><br/> Diese Option wird für Windows und Linux unterstützt.
Lokale Computer/virtuelle Azure-Computer mit DPM-Schutz | Der MARS-Agent wird auf dem DPM-Server ausgeführt. | Der MARS-Agent muss nicht auf den einzelnen Computern vorhanden sein.<br/><br/> Wenn Sie DPM bereitstellen, wird der DPM-Schutz-Agent auf allen Computern installiert, die Sie schützen möchten. 
Sicherung von lokalen Computern und virtuellen Azure-Computern mit MABS-Schutz | Der MARS-Agent wird in der MABS-Instanz ausgeführt. | Der MARS-Agent muss nicht auf den einzelnen Computern vorhanden sein.<br/><br/> Wenn Sie MABS bereitstellen, wird der MABS-Schutz-Agent auf allen Computern installiert, die Sie schützen möchten. 


## <a name="which-backup-agent-should-i-use"></a>Welchen Sicherungs-Agent soll ich verwenden?

**Sicherung** | **Lösung** | **Einschränkung**
--- | --- | ---
Ich möchte lokale Windows-Computer sichern. Die Computer sind nicht durch DPM oder MABS geschützt. | Installieren Sie den MARS-Agent auf dem Computer. | Sie können Dateien, Ordner und den Systemstatus in Azure sichern. Die Sicherungen sind nicht App-fähig.
Ich möchte lokale Linux-Computer sichern. Die Computer sind nicht durch DPM oder MABS geschützt. | Sie müssen DPM oder MABS bereitstellen, um Sicherungen in Azure zu ermöglichen.
Ich möchte Apps sichern, die auf lokalen Windows-Computern ausgeführt werden. | Für App-fähige Sicherungen müssen Windows-Computer durch DPM oder MABS geschützt werden.
Ich möchte virtuelle Azure-Computer sichern. | Führen Sie eine Sicherung mit Azure Backup aus. Die Sicherungserweiterung wird auf dem virtuellen Azure-Computer unter Windows oder Linux automatisch konfiguriert. | Die VM-Datenträger werden gesichert.<br/><br/> Bei virtuellen Windows-Computern ist die Sicherung App-konsistent. Für Linux ist die Sicherung dateikonsistent. Wenn Sie eine App-fähige Sicherung benötigen, müssen Sie dies mithilfe benutzerdefinierter Skripts konfigurieren.
Ich möchte virtuelle Azure-Computer mit hoher Präzision und Flexibilität bei den Sicherungs-und Wiederherstellungseinstellungen sichern. | Schützen Sie virtuelle Azure-Computer mit DPM oder MABS (ausgeführt in Azure), um mehr Flexibilität bei der Sicherungsplanung sowie uneingeschränkte Flexibilität für den Schutz und die Wiederherstellung von Dateien, Ordnern, Volumes, Apps und Systemstatus zu erhalten.


## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich [hier](backup-architecture.md) mit der Architektur und den Komponenten für verschiedene Sicherungsszenarien vertraut.
- Informieren Sie sich [hier](backup-support-matrix.md) über unterstützte Features und Einstellungen für die Sicherung.

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png

