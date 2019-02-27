---
title: Matrix zur Azure Backup-Unterstützung
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für den Azure Backup-Dienst.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: ff4ee1d88bd13e647d0f6218d7e9c9b2c57a5a01
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429569"
---
# <a name="azure-backup-support-matrix"></a>Matrix zur Azure Backup-Unterstützung

Sie können den [Azure Backup-Dienst](backup-overview.md) verwenden, um Daten in der Microsoft Azure-Cloud zu sichern. Dieser Artikel enthält eine Zusammenfassung der allgemeinen Supporteinstellungen und Einschränkungen für Azure Backup-Szenarien und -Bereitstellungen.

Andere Support-Matrizen sind verfügbar:

[Supportmatrix](backup-support-matrix-iaas.md) für Azure VM-Sicherung[Supportmatrix](backup-support-matrix-mabs-dpm.md) für die Sicherung mit System Center DPM/Microsoft Azure Backup Server (MABS) [Supportmatrix](backup-support-matrix-mars-agent.md) für die Sicherung mit dem Microsoft Azure Recovery Services (MARS)-Agent

## <a name="vault-support"></a>Unterstützung für Tresore

Für Azure-Sicherungen werden Recovery Services-Tresore verwendet, um Sicherungen zu orchestrieren und zu verwalten und gesicherte Daten zu speichern.

**Einstellung** | **Details**
--- | ---
**Tresore im Abonnement** | Bis zu 500 Recovery Services-Tresore pro Abonnement.
**Computer in einem Tresor** | Bis zu 1.000 virtuelle Azure-Computer pro Tresor.<br/><br/> Bis zu 50 MABS-Server können in einem einzigen Tresor registriert werden.
**Datenquelle im Tresorspeicher** | Maximal 54.400 GB. Für Azure-VM-Sicherungen gilt kein Grenzwert.
**Sicherungen im Tresor** | Azure-VMs: einmal täglich<br/><br/>Computer, die von den DPM/MABS geschützt werden: zweimal pro Tag<br/><br/> Computer, direkt mit dem MARS-Agent gesichert werden: dreimal pro Tag. 
**Sicherungen zwischen Tresoren** | Die Sicherung erfolgt innerhalb einer Region.<br/><br/> Sie benötigen einen Tresor in jeder Azure-Region, die VMs enthält, die Sie sichern möchten. Eine Sicherung in eine andere Region ist nicht möglich. 
**Tresor verschieben** | Sie können Tresore über Abonnements hinweg oder zwischen Ressourcengruppen innerhalb desselben Abonnements [verschieben](https://review.docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault).
**Daten zwischen Tresoren verschieben** | Das Verschieben von gesicherten Daten zwischen Tresoren wird nicht unterstützt.
**Tresorspeichertyp ändern** | Sie können den Speicherreplikationstyp (GRS/LRS) für einen Tresor ändern, bevor Sicherungen gespeichert werden. Nachdem Sicherungsvorgänge im Tresor begonnen haben, kann der Replikationstyp nicht mehr geändert werden.



## <a name="on-premises-backup-support"></a>Unterstützung von lokalen Sicherungen

Hier sind die Informationen zur Unterstützung beim Sichern von lokalen Computern angegeben.

**Computer** | **Gesichert** | **Location** | **Funktionen**
--- | --- | --- | ---
**Direkte Sicherung von Windows-Computer mit MARS-Agent** | Dateien, Ordner, Systemstatus | Sichern in Recovery-Diensttresor | Drei Sicherungen pro Tag.<br/><br/> Keine App-fähige Sicherung.<br/><br/> Wiederherstellungsdatei, -ordner, -volume.
**Direkte Sicherung von Linux-Computer mit MARS-Agent** | Sicherung wird nicht unterstützt.
**DPM-Sicherung** | Dateien, Ordner, Volumes, Systemstatus, App-Daten. | Sichern im lokalen DPM-Speicher. Der DPM führt dann die Sicherung in einem Tresor durch. | App-fähige Momentaufnahmen<br/><br/> Vollständige Granularität für Sicherung und Wiederherstellung.<br/><br/> Linux-Unterstützung für VMs (Hyper-V/VMware).<br/><br/>. Oracle wird nicht unterstützt.
**MABS-Sicherung** | Dateien, Ordner, Volumes, Systemstatus, App-Daten. | Sichern in den lokalen MABS-Speicher. MABS erstellt die Sicherung dann im Tresor. | App-fähige Momentaufnahmen<br/><br/> Vollständige Granularität für Sicherung und Wiederherstellung.<br/><br/> Linux-Unterstützung für VMs (Hyper-V/VMware).<br/><br/>. Oracle wird nicht unterstützt.


## <a name="azure-vm-backup-support"></a>Support für die Azure-VM-Sicherung

### <a name="azure-vm-limits"></a>Azure-VM-Grenzwerte

**Begrenzung** | **Details**
--- | ---
Azure-VM-Datenträger (für Daten) | Maximal 16.
Größe des Azure-VM-Datenträgers | Pro Datenträger bis zu 4.095 GB.


### <a name="azure-vm-backup-options"></a>Optionen für Azure-VM-Sicherungen

Hier ist die Unterstützung für den Fall angegeben, dass Sie Azure-VMs sichern möchten.

**Computer** | **Gesichert** | **Location** | **Funktionen**
--- | --- | --- | ---
**Azure-VM-Sicherung mit VM-Erweiterung** | Gesamte VM | Sicherung in den Tresor | Die Erweiterung ist installiert, wenn Sie die Sicherung für eine VM aktivieren.<br/><br/> Eine Sicherung pro Tag.<br/><br/> App-fähige Sicherung für Windows-VMs, dateikonsistente Sicherung für Linux-VMs. Sie können die App-Konsistenz für Linux-Computer mit benutzerdefinierten Skripts konfigurieren.<br/><br/> VM/Datenträger für Wiederherstellung.<br/><br/> Sicherung einer Azure-VM an einem lokalen Standort ist nicht möglich.
**Azure-VM-Sicherung mit MARS-Agent** | Dateien/Ordner | Sicherung in den Tresor | Drei Sicherungen pro Tag.<br/><br/> Der MARS-Agent kann neben der VM-Erweiterung ausgeführt werden, wenn Sie bestimmte Dateien/Ordner und nicht die gesamte VM sichern möchten.
**Azure-VM mit DPM** | Dateien, Ordner, Volumes, Systemstatus, App-Daten. | Sichern in den lokalen Speicher der Azure-VM, auf der DPM ausgeführt wird. Der DPM führt dann die Sicherung in einem Tresor durch. | App-fähige Momentaufnahmen<br/><br/> Vollständige Granularität für Sicherung und Wiederherstellung.<br/><br/> Linux-Unterstützung für VMs (Hyper-V/VMware).<br/><br/>. Oracle wird nicht unterstützt.
**Azure-VM mit MABS** | Dateien, Ordner, Volumes, Systemstatus, App-Daten. | Sichern in den lokalen Speicher der Azure-VM, auf der MABS ausgeführt wird. MABS erstellt die Sicherung dann im Tresor. | App-fähige Momentaufnahmen<br/><br/> Vollständige Granularität für Sicherung und Wiederherstellung.<br/><br/> Linux-Unterstützung für VMs (Hyper-V/VMware).<br/><br/> Oracle wird nicht unterstützt.





## <a name="linux-backup-support"></a>Unterstützung für Linux-Sicherungen

Hier ist die Unterstützung für den Fall angegeben, dass Sie Linux-Computer sichern möchten.

**Sicherung** | **Linux (von Azure unterstützt)**
--- | ---
**Direkte Sicherung lokaler Linux-Computer** |  Nein. Der MARS-Agent kann nur auf Windows-Computern installiert werden.
**Sichern von Azure-VMs unter Linux (mithilfe von Agent-Erweiterung)** | App-konsistente Sicherung mit [benutzerdefinierten Skripts](backup-azure-linux-app-consistent.md).<br/><br/> Wiederherstellung auf Dateiebene.<br/><br/> Wiederherstellung, indem über einen Wiederherstellungspunkt oder Datenträger eine VM erstellt wird.
**Sichern von lokalen oder Azure-VMs unter Linux mithilfe von DPM** | Dateikonsistente Sicherung von Linux-Gast-VMs unter Hyper-V und VMWare<br/><br/> VM-Wiederherstellung von Hyper-V- und VMWare-Linux-Gast-VMs</br></br> Dateikonsistente Sicherungen sind für virtuelle Azure-Computer nicht verfügbar
**Sichern von lokalen Computern/Azure-VMs unter Linux mithilfe von MABS** | Dateikonsistente Sicherung von Linux-Gast-VMs unter Hyper-V und VMWare<br/><br/> VM-Wiederherstellung von Hyper-V- und VMWare-Linux-Gast-VMs</br></br> Dateikonsistente Sicherungen sind für virtuelle Azure-Computer nicht verfügbar.

## <a name="daylight-saving-support"></a>Unterstützung für die Sommerzeit

Azure Backup unterstützt keine automatische Uhrzeitanpassung an die Sommerzeit für die Sicherungen von Azure-VMs. Sicherungsrichtlinien können bei Bedarf angepasst werden.


## <a name="disk-deduplication-support"></a>Unterstützung für Datenträgerdeduplizierung

Unterstützung für die Datenträgerdeduplizierung:
- Die Datenträgerdeduplizierung wird lokal unterstützt, wenn Sie DPM oder MABS zum Sichern von Hyper-V-VMs verwenden, auf denen Windows ausgeführt wird. Windows Server führt die Datendeduplizierung (auf Hostebene) auf den virtuellen Festplatten (VHDs) durch, die als Sicherungsspeicher an den virtuellen Computer angefügt sind.
- Die Deduplizierung wird für Sicherungskomponenten in Azure nicht unterstützt. Wenn System Center DPM und Azure Backup Server in Azure bereitgestellt werden, können an die VM angefügte Speicherdatenträger nicht dedupliziert werden.


## <a name="securityencryption-support"></a>Unterstützung für Sicherheit/Verschlüsselung

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



## <a name="compression-support"></a>Unterstützung für die Komprimierung

Backup unterstützt die Komprimierung des Sicherungsdatenverkehrs, wie in der folgenden Tabelle zusammengefasst. 

- Für Azure-VMs liest die VM-Erweiterung die Daten über das Speichernetzwerk direkt aus dem Azure-Speicherkonto, sodass es nicht erforderlich ist, diesen Datenverkehr zu komprimieren.
- Bei Verwendung von DPM oder MABS können Sie die Daten komprimieren, bevor sie per DPM/MABS gesichert werden, um Bandbreite zu sparen.

**Computer** | **Komprimierung für MABS/DPM (TCP)** | **Komprimierung (HTTPS) für Tresor**
--- | --- | ---
**Direkte Sicherung lokaler Windows-Computer** | Nicht verfügbar | Ja
**Sichern von Azure-VMs mithilfe von VM-Erweiterung** | Nicht verfügbar | Nicht verfügbar
** Sichern auf lokalen/Azure-Computern mit MABS/DPM | ![Ja][green] | ![Ja][green]



## <a name="retention-limits"></a>Grenzwerte für die Aufbewahrung

**Einstellung** | **Einschränkungen**
--- | ---
Maximale Wiederherstellungspunkte pro geschützter Instanz (Computer/Workload) | 9999
Maximale Ablaufzeit für einen Wiederherstellungspunkt | Keine Begrenzung
Maximale Sicherungshäufigkeit für DPM/MABS | Alle 15 Minuten für SQL Server<br/><br/> Stündlich für andere Workloads.
Maximale Sicherungshäufigkeit für Tresor | Lokale Windows-Computer/Azure-VMs mit MARS: dreimal pro Tag<br/><br/> DPM/MABS: Zweimal pro Tag<br/><br/> Azure-VM-Sicherung: einmal täglich
Aufbewahrung des Wiederherstellungspunkts | Täglich, wöchentlich, monatlich, jährlich.
Maximale Aufbewahrungsdauer | Abhängig von der Sicherungshäufigkeit.
Wiederherstellungspunkte auf DPM-/MABS-Datenträger | 64 für Dateiserver, 448 für App-Server.<br/><br/> Für Bandwiederherstellungspunkte für lokalen DPM gelten keine Grenzwerte.

## <a name="next-steps"></a>Nächste Schritte

- [Schauen Sie sich die Unterstützungsmatrix](backup-support-matrix-iaas.md) für die Sicherung von Azure-VMs an.


[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
