---
title: Unterstützungsmatrix für die Sicherung virtueller Azure-Computer mit Azure Backup
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Sicherung virtueller Azure-Computer mit dem Azure Backup-Dienst.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: b99d6285942bafe5467827c30b5ba2e42094fdf3
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56431054"
---
# <a name="support-matrix-for-azure-vm-backup"></a>Unterstützungsmatrix für die Sicherung virtueller Azure-Computer
Mit dem [Azure Backup-Dienst](backup-overview.md) können Sie lokale Computer und Workloads sowie virtuelle Azure-Computer sichern. Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Sicherung virtueller Azure-Computer (VMs) mit Azure Backup.

Andere Unterstützungsmatrizen:

- [Allgemeine Unterstützungsmatrix](backup-support-matrix.md) für Azure Backup
- [Unterstützungsmatrix](backup-support-matrix-mabs-dpm.md) für die Sicherung mit Microsoft Azure Backup Server und DPM
- [Unterstützungsmatrix](backup-support-matrix-mars-agent.md) für die Sicherung mit dem MARS-Agent



## <a name="supported-scenarios"></a>Unterstützte Szenarien

Virtuelle Azure-Computer können wie folgt mit dem Azure Backup-Dienst gesichert und wiederhergestellt werden.


**Szenario** | **Sicherung** | **Agent** |**Wiederherstellen** 
--- | --- | --- | ---
**Direkte Sicherung virtueller Azure-Computer** | Sicherung des gesamten virtuellen Computers  | Auf dem virtuellen Azure-Computer ist kein Agent erforderlich. Azure Backup installiert und verwendet eine Erweiterung für den [Azure-VM-Agent](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows), der auf dem virtuellen Computer ausgeführt wird. | Die Wiederherstellung wird wie folgt durchgeführt:<br/><br/> - **Erstellen eines einfachen virtuellen Computers:** Dies ist nützlich, wenn der virtuelle Computer keine besondere Konfiguration (z.B. mehrere IP-Adressen) aufweist.<br/><br/> - **Wiederherstellen des VM-Datenträgers:** Stellen Sie den Datenträger wieder her. Fügen Sie ihn dann an einen vorhandenen virtuellen Computer an, oder erstellen Sie mit PowerShell über den Datenträger einen neuen virtuellen Computer.<br/><br/> - **Ersetzen des VM-Datenträgers:** Wenn ein virtueller Computer vorhanden ist, der verwaltete Datenträger (unverschlüsselt) verwendet, können Sie einen Datenträger wiederherstellen und zum Ersetzen eines vorhandenen Datenträgers auf dem virtuellen Computer verwenden.<br/><br/> - **Wiederherstellen bestimmter Dateien oder Ordner:** Sie können Dateien oder Ordner von einem virtuellen Computer anstelle des gesamten virtuellen Computers wiederherstellen.  
**Direkte Sicherung virtueller Azure-Computer (nur Windows)** | Sichern bestimmter Dateien, Ordner oder Volumes | Installieren Sie den [Microsoft Azure Recovery Services-Agent (MARS)](backup-azure-file-folder-backup-faq.md).<br/><br/> Sie können den MARS-Agent zusammen mit der Sicherungserweiterung für den Azure-VM-Agent ausführen, um den virtuellen Computer auf Datei- oder Ordnerebene zu sichern. | Wiederherstellen bestimmter Ordner oder Dateien
**Sichern virtueller Azure-Computer auf Sicherungsserver** |  Sichern von Dateien, Ordnern oder Volumes, von Systemstatus- oder Bare-Metal-Dateien und von Anwendungsdaten in System Center DPM oder Microsoft Azure Backup Server (MAB-Server)<br/><br/> DPM/MABS erstellt die Sicherung dann im Sicherungstresor. | Installieren Sie den MABS- oder DPM-Schutz-Agent auf dem virtuellen Computer. Der MARS-Agent wird in DPM oder MABS installiert.| Wiederherstellen von Dateien, Ordnern oder Volumes, von Systemstatus- oder Bare-Metal-Dateien und von Anwendungsdaten 

Erfahren Sie mehr über die Sicherung mithilfe eines Sicherungsservers (backup-architecture.md#architecture-back-up-to-dpmmabs) und die [Unterstützungsanforderungen](backup-support-matrix-mabs-dpm.md).


## <a name="supported-backup-actions"></a>Unterstützte Sicherungsaktionen

**Aktion** | **Unterstützung**
--- | ---
Aktivieren der Sicherung beim Erstellen eines virtuellen Windows-Azure-Computers | Unterstützung für:  Windows Server 2016 (Datacenter, Datacenter Core), Windows Server 2012 R2 Datacenter, Windows Server 2008 R2 SP1
Aktivieren der Sicherung beim Erstellen eines virtuellen Linux-Computers | Unterstützung für:<br/><br/> – Ubuntu Server: 1710, 1704, 1604 (LTS), 1404 (LTS)<br/><br/> – Red Hat: RHEL 6.7, 6.8, 6.9, 7.2, 7.3, 7.4<br/><br/> – SUSE Linux Enterprise Server: 11 SP4, 12 SP2, 12 SP3<br/><br/> – Debian: 8, 9<br/><br/> – CentOS: 6.9, 7.3<br/><br/> Oracle Linux: 6.7, 6.8, 6.9, 7.2, 7.3
Sichern eines heruntergefahrenen virtuellen Computers, einer Offline-VM oder eines virtuellen Computers mit Suchvorgängen |  Unterstützt.<br/><br/> Die Momentaufnahme ist nur ausfallkonsistent, jedoch nicht anwendungskonsistent.
Sicherung von Datenträgern nach Migration zu verwalteten Datenträgern |  Unterstützt.<br/><br/> Sicherung wird weiterhin durchgeführt. Es ist keine Aktion erforderlich.
Sicherung von verwalteten Datenträgern nach Aktivierung einer Ressourcengruppensperre | Nicht unterstützt.<br/><br/> Bei der Sicherung können die älteren Ressourcenpunkte nicht gelöscht werden, und bei den Sicherungen treten Fehler auf, wenn die maximale Anzahl der Wiederherstellungspunkte erreicht ist.
Ändern der Sicherungsrichtlinie für einen virtuellen Computer |  Unterstützt.<br/><br/> Der virtuelle Computer wird unter Verwendung der Zeitplan- und Aufbewahrungseinstellungen der neuen Richtlinie gesichert. Wenn die Aufbewahrungseinstellungen erweitert werden, werden vorhandene Wiederherstellungspunkte markiert und beibehalten. Wenn die Einstellungen reduziert werden, werden vorhandene Wiederherstellungspunkte im nächsten Bereinigungsauftrag gelöscht und schließlich endgültig entfernt.
Abbrechen eines Sicherungsauftrags | Wird während des Prozesses der Momentaufnahme unterstützt.<br/><br/> Wird nicht unterstützt, wenn die Momentaufnahme in den Tresor übertragen wird. 
Sicherung des virtuellen Computers in einer anderen Region oder einem anderen Abonnement |  Nicht unterstützt.
Sicherungen pro Tag (über die Azure-VM-Erweiterung) | Eine geplante Sicherung pro Tag<br/><br/> Sie können bis zu vier bedarfsgesteuerte Sicherungen pro Tag durchführen.
Sicherungen pro Tag (über den MARS-Agent) | Drei geplante Sicherungen pro Tag 
Sicherungen pro Tag (über DPM/MABS) | Zwei geplante Sicherungen pro Tag
Monatliche oder jährliche Sicherung   | Wird bei der Sicherung mit der Azure-VM-Erweiterung nicht unterstützt. Lediglich tägliche und wöchentliche Sicherungen werden unterstützt.<br/><br/> Sie können die Richtlinie so einrichten, dass tägliche oder wöchentliche Sicherungen in einem monatlichen oder jährlichen Aufbewahrungszeitraum beibehalten werden.
Automatische Uhrzeitanpassung | Nicht unterstützt.<br/><br/> In Azure Backup wird bei der Sicherung virtueller Computer die automatische Sommerzeitanpassung der Uhrzeit nicht berücksichtigt.<br/><br/>  Ändern Sie die Richtlinie nach Bedarf manuell.
[Sicherheitsfunktionen für Hybridsicherungen](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) |  Die Funktionen können nicht deaktiviert werden.



## <a name="operating-system-support-windows"></a>Unterstützte Betriebssysteme (Windows)

In der folgenden Tabelle sind die unterstützten Betriebssysteme für die Sicherung virtueller Windows-Azure-Computer aufgeführt.

**Szenario** | **Betriebssystemunterstützung** 
--- | ---
Sicherung mit der Azure-VM-Agent-Erweiterung | Windows-Client: Nicht unterstützt<br/><br/> Windows-Server: Windows Server 2008 R2 oder höher 
Sicherung mit dem MARS-Agent | [Unterstützte](backup-support-matrix-mars-agent.md#support-for-direct-backups) Betriebssysteme
Sicherung über DPM/MABS | Unterstützte Betriebssysteme für die Sicherung mit [MABS](backup-mabs-protection-matrix.md) und [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807)

## <a name="support-for-linux-backup"></a>Unterstützung für die Linux-Sicherung

Hier ist die Unterstützung für den Fall angegeben, dass Sie Linux-Computer sichern möchten.

**Aktion** | **Unterstützung**
--- | --- 
Sicherung virtueller Linux-Azure-Computer mit dem Linux-Azure-VM-Agent | Dateikonsistente Sicherung<br/><br/> App-konsistente Sicherung mit [benutzerdefinierten Skripts](backup-azure-linux-app-consistent.md).<br/><br/> Während der Wiederherstellung können Sie einen neuen virtuellen Computer erstellen oder einen Datenträger wiederherstellen und damit einen virtuellen Computer erstellen oder damit einen Datenträger auf einem vorhandenen virtuellen Computer ersetzen. Zudem können Sie einzelne Dateien und Ordner wiederherstellen.
Sicherung virtueller Linux-Azure-Computer mit dem MARS-Agent | Nicht unterstützt.<br/><br/> Der MARS-Agent kann nur auf Windows-Computern installiert werden.
Sicherung virtueller Linux-Azure-Computer mit DPM/MABS | Nicht unterstützt.

## <a name="operating-system-support-linux"></a>Unterstützte Betriebssysteme (Linux)

Für Sicherungen von virtuellen Linux-Azure-Computern unterstützt Azure Backup [die Liste der von Azure unterstützten Linux-Distributionen](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). Beachten Sie Folgendes:

- Azure Backup unterstützt CoreOS Linux nicht.
- Azure Backup unterstützt keine 32-Bit-Betriebssysteme.
- Andere Bring-Your-Own-Linux-Distributionen sollten funktionieren, sofern der [Azure-VM-Agent für Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) auf dem virtuellen Computer verfügbar ist und Python unterstützt wird.



## <a name="backup-frequency-and-retention"></a>Sicherungshäufigkeit und -aufbewahrung

**Einstellung** | **Einschränkungen** 
--- | --- 
Maximale Wiederherstellungspunkte pro geschützter Instanz (Computer/Workload) | 9999
Maximale Ablaufzeit für einen Wiederherstellungspunkt | Keine Begrenzung
Maximale Sicherungshäufigkeit für Tresor (Azure-VM-Erweiterung) | einmal täglich
Maximale Sicherungshäufigkeit für Tresor (MARS-Agent) | Drei Sicherungen pro Tag
Maximale Sicherungshäufigkeit für DPM/MABS | Alle 15 Minuten für SQL Server<br/><br/> Stündlich für andere Workloads.
Aufbewahrung des Wiederherstellungspunkts | Täglich, wöchentlich, monatlich, jährlich.
Maximale Aufbewahrungsdauer | Abhängig von der Sicherungshäufigkeit.
Wiederherstellungspunkte auf DPM-/MABS-Datenträger | 64 für Dateiserver, 448 für App-Server.<br/><br/> Für Bandwiederherstellungspunkte für lokalen DPM gelten keine Grenzwerte.


## <a name="supported-restore-methods"></a>Unterstützte Wiederherstellungsmethoden

**Wiederherstellungsmethode** | **Details**
--- | ---
**Erstellen eines neuen virtuellen Computers** | Sie können während des Wiederherstellungsvorgangs einen virtuellen Computer erstellen. <br/><br/> Bei dieser Option wird ein einfacher virtueller Computer erstellt und ausgeführt. Sie können den Namen des virtuellen Computers, die Ressourcengruppe, das virtuelle Netzwerk, das Subnetz und den Speicher angeben.  
**Wiederherstellen eines Datenträgers** | Sie können einen Datenträger wiederherstellen und zum Erstellen eines virtuellen Computers verwenden.<br/><br/> Wenn Sie diese Option auswählen, kopiert Azure Backup die Daten aus dem Tresor in ein ausgewähltes Speicherkonto. Mit dem Wiederherstellungsauftrag wird eine Vorlage generiert. Sie können diese Vorlage herunterladen und damit benutzerdefinierte VM-Einstellungen angeben und einen virtuellen Computer erstellen.<br/><br/> Bei dieser Option können Sie mehr Einstellungen als bei der vorherigen Option zum Erstellen eines virtuellen Computers angeben.<br/><br/>
**Ersetzen eines vorhandenen Datenträgers** | Sie können einen Datenträger wiederherstellen und dann einen derzeit vorhandenen Datenträger auf einem virtuellen Computer durch den wiederhergestellten Datenträger ersetzen.
**Wiederherstellen von Dateien** | Sie können Dateien von einem ausgewählten Wiederherstellungspunkt wiederherstellen. Sie laden ein Skript herunter, um den VM-Datenträger von dem Wiederherstellungspunkt einzubinden. Anschließend durchsuchen Sie die Datenträgervolumes nach Dateien und Ordnern, die wiederhergestellt werden sollen, und heben dann die Einbindung des Datenträgers auf.

## <a name="support-for-file-level-restore"></a>Unterstützung für die Wiederherstellung auf Dateiebene

**Wiederherstellen** | **Unterstützt**
--- | --- 
Wiederherstellen von Dateien unter verschiedenen Betriebssystemen | Sie können Dateien auf allen Computern wiederherstellen, die dasselbe (oder ein kompatibles) Betriebssystem wie der gesicherte virtuelle Computer haben. Siehe dazu die Tabelle mit [kompatiblen Betriebssystemen](backup-azure-restore-files-from-vm.md#system-requirements).
Wiederherstellen von Dateien auf klassischen virtuellen Computern | Nicht unterstützt. 
Wiederherstellen von Dateien von verschlüsselten virtuellen Computern | Nicht unterstützt.
Wiederherstellen von Dateien von Speicherkonten mit Netzwerkbeschränkung | Nicht unterstützt.
Wiederherstellen von Dateien auf virtuellen Computern mit Windows-Speicherplätzen | Die Wiederherstellung auf demselben Computer wird nicht unterstützt.<br/><br/> Führen Sie stattdessen die Wiederherstellung der Dateien auf einem kompatiblen virtuellen Computer durch.
Wiederherstellen von Dateien auf einem virtuellen Linux-Computer mit LVM/RAID-Arrays | Die Wiederherstellung auf demselben Computer wird nicht unterstützt.<br/><br/> Führen Sie die Wiederherstellung auf einem kompatiblen virtuellen Computer durch.
Wiederherstellen von Dateien mit spezifischen Netzwerkeinstellungen | Die Wiederherstellung auf demselben Computer wird nicht unterstützt. <br/><br/> Führen Sie die Wiederherstellung auf einem kompatiblen virtuellen Computer durch.

## <a name="support-for-vm-management"></a>Unterstützung für die Datenträgerverwaltung

In der folgenden Tabelle ist die Unterstützung für die Sicherung bei Verwaltungsaufgaben für virtuelle Computer aufgeführt, z.B. Hinzufügen oder Ersetzen von VM-Datenträgern.

**Wiederherstellen** | **Unterstützt** 
--- | --- 
Wiederherstellung in verschiedenen Abonnements, Regionen oder Zonen | Nicht unterstützt 
Wiederherstellung auf einem vorhandenen virtuellen Computer | Verwenden Sie die Option zum Ersetzen eines Datenträgers.
Wiederherstellen eines Datenträgers mit für die Azure-Speicherdienstverschlüsselung (SSE) aktiviertem Speicherkonto | Nicht unterstützt.<br/><br/> Führen Sie die Wiederherstellung in einem Konto aus, für das SSE nicht aktiviert ist.
Wiederherstellung in gemischten Speicherkonten | Nicht unterstützt.<br/><br/> Basierend auf dem Speicherkontotyp sind alle wiederhergestellten Datenträger entweder jeweils Premium- oder Standard-Datenträger und können nicht gemischt sein. 
Wiederherstellung in Speicherkonto mithilfe von ZRS | Nicht unterstützt
Wiederherstellen eines virtuellen Computers direkt in einer Verfügbarkeitsgruppe | Bei verwalteten Datenträgern können Sie den Datenträger wiederherstellen und die Option für die Verfügbarkeitsgruppe in der Vorlage verwenden.<br/><br/> Dies wird für nicht verwaltete Datenträger nicht unterstützt. Bei nicht verwalteten Datenträgern stellen Sie den Datenträger wieder her und erstellen dann einen virtuellen Computer in der Verfügbarkeitsgruppe.
Wiederherstellen einer Sicherung von nicht verwalteten virtuellen Computern nach dem Upgrade auf verwaltete virtuelle Computer |  Unterstützt.<br/><br/> Sie können Datenträger wiederherstellen und dann einen verwalteten virtuellen Computer erstellen.
Wiederherstellen eines virtuellen Computers an einem Wiederherstellungspunkt vor der Migration des virtuellen Computers zu verwalteten Datenträgern |  Unterstützt.<br/><br/> Sie führen die Wiederherstellung auf nicht verwalteten Datenträgern durch (Standardeinstellung), konvertieren die wiederhergestellten Datenträger in verwaltete Datenträger und erstellen einen virtuellen Computer mit den verwalteten Datenträgern.
Wiederherstellen eines gelöschten virtuellen Computers |  Unterstützt.<br/><br/> Sie können den virtuellen Computer aus einem Wiederherstellungspunkt wiederherstellen.
Wiederherstellen eines virtuellen Domänencontrollercomputers, der Teil einer Konfiguration mit mehreren Domänencontrollern ist, über das Portal | Wird unterstützt, wenn Sie unter Verwendung von PowerShell den Datenträger wiederherstellen und einen virtuellen Computer erstellen.
Wiederherstellen eines virtuellen Computers in einem anderen VNET |   Unterstützt.<br/><br/> Das VNET muss sich im selben Abonnement und derselben Region befinden.




## <a name="vm-compute-support"></a>Unterstützung für VM-Compute

**Compute** | **Unterstützung** 
--- | --- 
Größe des virtuellen Computers |   Jede Größe von virtuellen Azure-Computern mit mindestens 2 CPU-Kernen und 1 GB RAM<br/><br/> [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) 
Sicherung virtueller Computer in [Verfügbarkeitsgruppen](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets) |  Unterstützt.<br/><br/> Sie können einen virtuellen Computer in einer Verfügbarkeitsgruppe nicht mit der Option zum schnellen Erstellen eines virtuellen Computers wiederherstellen. Stattdessen müssen Sie bei der Wiederherstellung des virtuellen Computers einen Datenträger wiederherstellen und damit entweder einen virtuellen Computer bereitstellen oder einen vorhandenen Datenträger ersetzen. 
Sicherung virtueller Computer in [Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview) |  Nicht unterstützt.  
Sicherung von mit [Hybridvorteil (HUB)](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) bereitgestellten virtuellen Computern |  Unterstützt.
Sicherung von in einer [Skalierungsgruppe](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) bereitgestellten virtuellen Computern |   Nicht unterstützt   
Sicherung von über den [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?filters=virtual-machine-images) bereitgestellten virtuellen Computern<br/><br/> (Veröffentlicht von Microsoft oder Drittanbietern) |    Unterstützt.<br/><br/> Auf dem virtuellen Computer muss ein unterstütztes Betriebssystem ausgeführt werden.<br/><br/> Die Wiederherstellung von Dateien auf dem virtuellen Computer kann nur unter einem kompatiblen Betriebssystem (kein früheres oder späteres Betriebssystem) durchgeführt werden.
Sicherung von über ein benutzerdefiniertes Image bereitgestellten virtuellen Computern (Drittanbieter) |     Unterstützt.<br/><br/> Auf dem virtuellen Computer muss ein unterstütztes Betriebssystem ausgeführt werden.<br/><br/> Die Wiederherstellung von Dateien auf dem virtuellen Computer kann nur unter einem kompatiblen Betriebssystem (kein früheres oder späteres Betriebssystem) durchgeführt werden.
Sicherung von zu Azure migrierten virtuellen Computern   |  Unterstützt.<br/><br/> Für die Sicherung des virtuellen Computers muss der VM-Agent auf dem migrierten Computer installiert werden. 



## <a name="vm-storage-support"></a>Unterstützung für VM-Speicher

**Komponente** | **Unterstützung**
--- | ---
Azure-VM-Datenträger (für Daten) | Sichern eines virtuellen Computers mit maximal 16 Datenträgern
Datenträgergröße | Pro Datenträger bis zu 4.095 GB.<br/><br/> Wenn Sie die neueste Version der Azure-VM-Sicherung ausführen (als sofortige Wiederherstellung bezeichnet), werden Datenträgergrößen bis zu 4 TB unterstützt. [Weitere Informationen](backup-instant-restore-capability.md)
Speichertyp | HDD Standard, SSD Standard, SSD Premium <br/><br/> SSD Standard wird unterstützt, wenn Sie die neueste Version der Azure-VM-Sicherung (bezeichnet als sofortige Wiederherstellung) ausführen. [Weitere Informationen](backup-instant-restore-capability.md)
Verwaltete Datenträger | Unterstützt
Verschlüsselte Datenträger |  Unterstützt.<br/><br/> Virtuelle Azure-Computer mit aktiviertem Azure Disk Encryption (ADE) können (mit oder ohne Azure AD-App) gesichert werden.<br/><br/> Verschlüsselte virtuelle Computer können nicht auf Datei- oder Ordnerebene wiederhergestellt werden. Stattdessen muss der gesamte virtuelle Computer wiederhergestellt werden.<br/><br/> Sie können die Verschlüsselung auf virtuellen Computern aktivieren, die bereits durch Azure Backup geschützt werden.
Datenträger mit aktivierter Schreibbeschleunigung | Nicht unterstützt.<br/><br/> Wenn Sie die neueste Version der Azure-VM-Sicherung (bezeichnet als [sofortige Wiederherstellung](backup-instant-restore-capability.md)) ausführen, können Sie Datenträger mit aktivierter Schreibbeschleunigung von der Sicherung ausschließen.
Sicherung deduplizierter Datenträger | Nicht unterstützt.
Hinzufügen eines Datenträgers zu geschütztem virtuellen Computer |  Unterstützt.
Ändern der Datenträgergröße auf geschütztem virtuellen Computer |  Unterstützt.

## <a name="vm-network-support"></a>Netzwerkunterstützung bei virtuellen Computern


**Komponente** | **Unterstützung**
--- | ---
Anzahl von Netzwerkadaptern | Bis zur maximalen Anzahl von Netzwerkadaptern, die für eine bestimmte Größe von virtuellen Azure-Computern unterstützt werden<br/><br/> Netzwerkadapter werden erstellt, wenn der virtuelle Computer während des Wiederherstellungsvorgangs erstellt wird.<br/><br/> Die Anzahl von Netzwerkadaptern auf dem wiederhergestellten virtuellen Computer entspricht der Anzahl von Netzwerkadaptern auf dem virtuellen Computer bei Aktivierung des Schutzes. Wenn nach der Aktivierung Netzwerkadapter entfernt werden, wirkt sich das nicht auf die Anzahl aus.
Externer und interner Lastenausgleich |    Unterstützt. <br/></br> Weitere Informationen zum Wiederherstellen von virtuellen Computern mit spezifischen Netzwerkeinstellungen finden Sie [hier](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
Mehrere reservierte IP-Adressen |   Unterstützt. <br/></br> Weitere Informationen zum Wiederherstellen von virtuellen Computern mit spezifischen Netzwerkeinstellungen finden Sie [hier](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
Virtuelle Computer mit mehreren Netzwerkadaptern  |  Unterstützt. <br/></br> Weitere Informationen zum Wiederherstellen von virtuellen Computern mit spezifischen Netzwerkeinstellungen finden Sie [hier](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
Virtuelle Computer mit öffentlichen IP-Adressen    |  Unterstützt.<br/><br/> Sie müssen dem Netzwerkadapter eine vorhandene öffentliche IP-Adresse zuordnen oder eine Adresse erstellen und sie nach der Wiederherstellung dem Netzwerkadapter zuordnen.
Netzwerksicherheitsgruppe (NSG) für Netzwerkadapter oder Subnetz |    Unterstützt.
Reservierte IP-Adresse (statisch) | Nicht unterstützt.<br/><br/> Ein virtueller Computer mit einer reservierten IP-Adresse und keinem definierten Endpunkt kann nicht gesichert werden.
Dynamische IP-Adresse |     Unterstützt.<br/><br/> Wenn der Netzwerkadapter auf dem virtuellen Quellcomputer dynamische IP-Adressen verwendet, werden diese standardmäßig auch für den Netzwerkadapter auf dem wiederhergestellten virtuellen Computer verwendet.
Traffic Manager | Unterstützt<br/><br/>. Wenn sich der gesicherte virtuelle Computer in Traffic Manager befindet, müssen Sie den wiederhergestellten virtuellen Computer manuell in derselben Traffic Manager-Instanz hinzufügen. 
Azure DNS |  Unterstützt.
Benutzerdefinierter DNS |     Unterstützt.
Ausgehende Verbindungen über HTTP-Proxy |  Unterstützt.<br/><br/> Ein authentifizierter Proxy wird nicht unterstützt. 
VNET-Dienstendpunkte  |  Unterstützt.<br/><br/> In den Speicherkontoeinstellungen für die Firewall und das VNET muss der Zugriff über alle Netzwerke zugelassen werden. 



## <a name="vm-securityencryption-support"></a>Unterstützung für Sicherheit und Verschlüsselung bei virtuellen Computern

Azure Backup unterstützt die Verschlüsselung für Daten während der Übertragung und im Ruhezustand:

Netzwerkdatenverkehr für Azure:
- Sicherungsdatenverkehr von den Servern zum Recovery Services-Tresor wird per AES 256 (Advanced Encryption Standard) verschlüsselt.
- Sicherungsdaten werden über eine sichere HTTPS-Verbindung übertragen.
- Die Sicherungsdaten werden im Recovery Services-Tresor in verschlüsselter Form gespeichert.
- Nur Sie verfügen über die Passphrase zum Entsperren dieser Daten. Die Sicherungsdaten können zu keinem Zeitpunkt von Microsoft entschlüsselt werden.
    > [!WARNING]
    > Nachdem der Tresor eingerichtet wurde, haben nur Sie Zugriff auf den Verschlüsselungsschlüssel. Microsoft bewahrt keine Kopie auf und hat keinen Zugriff auf den Schlüssel. Wenn der Schlüssel verlegt wird, kann Microsoft die gesicherten Daten nicht wiederherstellen.
Datensicherheit:
- Beim Sichern von Azure-VMs müssen Sie die Verschlüsselung *auf dem virtuellen Computer* einrichten.
- Azure Backup unterstützt den Azure Disk Encryption-Dienst, der auf virtuellen Windows-Computern BitLocker und auf virtuellen Linux-Computern **dm-crypt** verwendet.
- Auf dem Back-End nutzt Azure Backup die [Speicherdienstverschlüsselung von Azure](../storage/common/storage-service-encryption.md), mit der ruhende Daten geschützt werden.


**Computer** | **Während der Übertragung** | **Ruhende Daten**
--- | --- | ---
Lokale Windows-Computer ohne DPM/MABS | ![Ja][green] | ![Ja][green]
Virtuelle Azure-Computer | ![Ja][green] | ![Ja][green] 
Lokal/Azure-VMs mit DPM | ![Ja][green] | ![Ja][green]
Lokal/Azure-VMs mit MABS | ![Ja][green] | ![Ja][green]



## <a name="vm-compression-support"></a>Unterstützung für die Komprimierung bei virtuellen Computern

Backup unterstützt die Komprimierung des Sicherungsdatenverkehrs, wie in der folgenden Tabelle zusammengefasst. Beachten Sie Folgendes:

- Für Azure-VMs liest die VM-Erweiterung die Daten über das Speichernetzwerk direkt aus dem Azure-Speicherkonto, sodass es nicht erforderlich ist, diesen Datenverkehr zu komprimieren.
- Bei Verwendung von DPM oder MABS können Sie die Daten komprimieren, bevor sie per DPM/MABS gesichert werden, um Bandbreite zu sparen. 

**Computer** | **Komprimierung für MABS/DPM (TCP)** | **Komprimierung (HTTPS) für Tresor**
--- | --- | ---
Lokale Windows-Computer ohne DPM/MABS | Nicht verfügbar | Ja
Virtuelle Azure-Computer | Nicht verfügbar | Nicht verfügbar
Lokal/Azure-VMs mit DPM | ![Ja][green] | ![Ja][green]
Lokal/Azure-VMs mit MABS | ![Ja][green] | ![Ja][green]




## <a name="next-steps"></a>Nächste Schritte

- [Sichern virtueller Azure-Computer](backup-azure-arm-vms-prepare.md)
- [Direktes Sichern von Windows-Computern](tutorial-backup-windows-server-to-azure.md) (ohne Sicherungsserver)
- [Einrichten von MABS](backup-azure-microsoft-azure-backup.md) für die Sicherung in Azure und anschließendes Sichern von Workloads per MABS
- [Einrichten von DPM](backup-azure-dpm-introduction.md) für die Sicherung in Azure und anschließendes Sichern von Workloads per DPM

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png

