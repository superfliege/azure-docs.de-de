---
title: Matrix zur Azure Backup-Unterstützung
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für den Azure Backup-Dienst.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b1fa723863e6485e977e075986c3779efed1e689
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360628"
---
# <a name="azure-backup-support-matrix"></a>Matrix zur Azure Backup-Unterstützung

Sie können den [Azure Backup-Dienst](backup-overview.md) verwenden, um Daten in der Microsoft Azure-Cloud zu sichern. Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für Azure Backup-Szenarien und -Bereitstellungen.

## <a name="vault-support"></a>Unterstützung für Tresore

Für Azure-Sicherungen werden Recovery Services-Tresore verwendet, um Sicherungen zu orchestrieren und zu verwalten und gesicherte Daten zu speichern.

**Einstellung** | **Details**
--- | ---
Anzahl der Tresore | Bis zu 500 Recovery Services-Tresore pro Abonnement.
Computer in einem Tresor | Bis zu 1.000 virtuelle Azure-Computer pro Tresor.<br/><br/> Bis zu 50 lokale Computer, auf denen der Azure Backup-Agent (Microsoft Azure Recovery Services-Agent, MABS) ausgeführt wird, können pro Tresor registriert werden.
Datenquelle im Tresorspeicher | Maximal 54.400 GB. Für Azure-VM-Sicherungen gilt kein Grenzwert.
Sicherungen im Tresor | Azure-VMs: einmal pro Tag; Per DPM/MABS geschützte Computer: zweimal pro Tag; Computer mit direkter Sicherung per MARS-Agent: dreimal pro Tag.  
Tresor verschieben | Sie können für Sicherungen verwendete Recovery Services-Tresore über Abonnements und Ressourcengruppen hinweg verschieben. [Weitere Informationen](backup-azure-move-recovery-services-vault.md)
Daten zwischen Tresoren verschieben | Das Verschieben von gesicherten Daten zwischen Tresoren wird nicht unterstützt.
Speicherreplikationstyp | Sie können den Speicherreplikationstyp (GRS/LRS) für einen Tresor ändern, bevor Sicherungen gespeichert werden. Nachdem Sicherungsvorgänge im Tresor begonnen haben, kann der Replikationstyp nicht mehr geändert werden.



## <a name="on-premises-backup-support"></a>Unterstützung von lokalen Sicherungen 

Hier sind die Informationen zur Unterstützung beim Sichern von lokalen Computern angegeben.

**Computer** | **Ort** | **Sicherung** | **Funktionen**
--- | --- | --- | ---
**Windows: physisch/virtuell (kein Sicherungsserver)** | Dateien, Ordner, Systemstatus | Sicherung im Recovery Services-Tresor | Drei Sicherungen pro Tag.<br/><br/> Keine App-fähige Sicherung.<br/><br/> Wiederherstellungsdatei, -ordner, -volume.
**Linux: physisch/virtuell (kein Sicherungsserver)** | Sicherung wird nicht unterstützt.
**Physisch/virtuell mit DPM** | Dateien, Ordner, Volumes, Systemstatus, App-Daten. | Sicherung per DPM (auf einem Datenträger, der lokal an den DPM-Server angefügt ist, oder auf Band).<br/><br/> Der DPM führt dann die Sicherung in einem Tresor durch. | App-fähige Momentaufnahmen<br/><br/> Vollständige Granularität für Sicherung und Wiederherstellung.<br/><br/> Linux-Unterstützung für VMs (Hyper-V/VMware).<br/><br/>. Oracle wird nicht unterstützt.
**Physisch/virtuell mit MABS** | Dateien, Ordner, Volumes, Systemstatus, App-Daten. | Sicherung per MABS (auf einem Datenträger, der lokal an den MABS-Server angefügt ist). Bandsicherung wird nicht unterstützt.<br/><br/> MABS erstellt die Sicherung dann im Tresor. | App-fähige Momentaufnahmen<br/><br/> Vollständige Granularität für Sicherung und Wiederherstellung.<br/><br/> Linux-Unterstützung für VMs (Hyper-V/VMware).<br/><br/>. Oracle wird nicht unterstützt.


## <a name="azure-vms"></a>Virtuelle Azure-Computer

### <a name="azure-vm-limits"></a>Azure-VM-Grenzwerte

**Begrenzung** | **Details**
--- | ---
Azure-VM-Datenträger (für Daten) | Maximal 16.
Größe des Azure-VM-Datenträgers | Pro Datenträger bis zu 4.095 GB.


### <a name="azure-vm-backup-options"></a>Optionen für Azure-VM-Sicherungen

Hier ist die Unterstützung für den Fall angegeben, dass Sie Azure-VMs sichern möchten.

**Computer** | **Ort** | **Sicherung** | **Funktionen**
--- | --- | --- | ---
**Azure-VMs (kein Sicherungsserver)** | Dateien, Ordner, Systemstatus | Sicherung im Tresor. | Eine Sicherung pro Tag.<br/><br/> App-fähige Sicherung für Windows-VMs, dateikonsistente Sicherung für Linux-VMs. Sie können die App-Konsistenz für Linux-Computer mit benutzerdefinierten Skripts konfigurieren.<br/><br/> VM/Datenträger für Wiederherstellung.<br/><br/> Sicherung einer Azure-VM an einem lokalen Standort ist nicht möglich.
**Azure-VM mit DPM** | Dateien, Ordner, Volumes, Systemstatus, App-Daten. | Sicherung per DPM unter Azure (auf einem Datenträger, der lokal an den DPM-Server angefügt ist). Bandsicherung wird nicht unterstützt.<br/><br/> Der DPM führt dann die Sicherung in einem Tresor durch. | App-fähige Momentaufnahmen<br/><br/> Vollständige Granularität für Sicherung und Wiederherstellung.<br/><br/> Linux-Unterstützung für VMs (Hyper-V/VMware).<br/><br/>. Oracle wird nicht unterstützt.
**Azure-VM mit MABS** | Dateien, Ordner, Volumes, Systemstatus, App-Daten. | Sicherung per MABS unter Azure (auf einem Datenträger, der lokal an den MABS-Server angefügt ist). Bandsicherung wird nicht unterstützt.<br/><br/> MABS erstellt die Sicherung dann im Tresor. | App-fähige Momentaufnahmen<br/><br/> Vollständige Granularität für Sicherung und Wiederherstellung.<br/><br/> Linux-Unterstützung für VMs (Hyper-V/VMware).<br/><br/>. Oracle wird nicht unterstützt.





## <a name="linux-backup-support"></a>Unterstützung für Linux-Sicherungen

Hier ist die Unterstützung für den Fall angegeben, dass Sie Linux-Computer sichern möchten.

**Sicherung** | **Linux (von Azure unterstützt)**
--- | --- 
**Lokaler Linux-Computer (ohne DPM oder MABS)**. | Nein. Der MARS-Agent kann nur auf Windows-Computern installiert werden. 
**Azure-VM (ohne DPM oder MABS)** | App-konsistente Sicherung mit [benutzerdefinierten Skripts](backup-azure-linux-app-consistent.md).<br/><br/> Wiederherstellung auf Dateiebene.<br/><br/> Wiederherstellung, indem über einen Wiederherstellungspunkt oder Datenträger eine VM erstellt wird.
**Lokaler Computer/Azure-VM mit DPM** | Dateikonsistente Sicherung von Linux-Gast-VMs unter Hyper-V und VMWare<br/><br/> VM-Wiederherstellung von Hyper-V- und VMWare-Linux-Gast-VMs</br></br> Dateikonsistente Sicherungen sind für virtuelle Azure-Computer nicht verfügbar
**Lokaler Computer/Azure-VM mit MABS** | Dateikonsistente Sicherung von Linux-Gast-VMs unter Hyper-V und VMWare<br/><br/> VM-Wiederherstellung von Hyper-V- und VMWare-Linux-Gast-VMs</br></br> Dateikonsistente Sicherungen sind für virtuelle Azure-Computer nicht verfügbar.

## <a name="disk-support"></a>Unterstützung für Datenträger

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
Lokale Windows-Computer ohne DPM/MABS | ![JA][green] | ![JA][green]
Virtuelle Azure-Computer | ![JA][green] | ![JA][green] 
Lokal/Azure-VMs mit DPM | ![JA][green] | ![JA][green]
Lokal/Azure-VMs mit MABS | ![JA][green] | ![JA][green]



## <a name="compression-support"></a>Unterstützung für die Komprimierung

Backup unterstützt die Komprimierung des Sicherungsdatenverkehrs, wie in der folgenden Tabelle zusammengefasst. Beachten Sie Folgendes:

- Für Azure-VMs liest die VM-Erweiterung die Daten über das Speichernetzwerk direkt aus dem Azure-Speicherkonto, sodass es nicht erforderlich ist, diesen Datenverkehr zu komprimieren.
- Bei Verwendung von DPM oder MABS können Sie die Daten komprimieren, bevor sie per DPM/MABS gesichert werden, um Bandbreite zu sparen. 

**Computer** | **Komprimierung für MABS/DPM (TCP)** | **Komprimierung (HTTPS) für Tresor**
--- | --- | ---
Lokale Windows-Computer ohne DPM/MABS | Nicht verfügbar | JA
Virtuelle Azure-Computer | Nicht verfügbar | Nicht verfügbar
Lokal/Azure-VMs mit DPM | ![JA][green] | ![JA][green]
Lokal/Azure-VMs mit MABS | ![JA][green] | ![JA][green]



## <a name="retention-limits"></a>Grenzwerte für die Aufbewahrung

**Einstellung** | **Einschränkungen** 
--- | --- 
Maximale Wiederherstellungspunkte pro geschützter Instanz (Computer/Workload) | 9999
Maximale Ablaufzeit für einen Wiederherstellungspunkt | Keine Begrenzung
Maximale Sicherungshäufigkeit für DPM/MABS | Alle 15 Minuten für SQL Server<br/><br/> Stündlich für andere Workloads.
Maximale Sicherungshäufigkeit für Tresor | Lokale Windows-Computer/Azure-VMs mit MARS: dreimal pro Tag<br/><br/> DPM/MABS: Zweimal pro Tag<br/><br/> Azure-VM-Sicherung: Einmal pro Tag
Aufbewahrung des Wiederherstellungspunkts | Täglich, wöchentlich, monatlich, jährlich.
Maximale Aufbewahrungsdauer | Abhängig von der Sicherungshäufigkeit.
Wiederherstellungspunkte auf DPM-/MABS-Datenträger | 64 für Dateiserver, 448 für App-Server.<br/><br/> Für Bandwiederherstellungspunkte für lokalen DPM gelten keine Grenzwerte.

## <a name="next-steps"></a>Nächste Schritte

- [Sichern virtueller Azure-Computer](backup-azure-arm-vms-prepare.md)
- [Direktes Sichern von Windows-Computern](tutorial-backup-windows-server-to-azure.md) (ohne Sicherungsserver)
- [Einrichten von MABS](backup-azure-microsoft-azure-backup.md) für die Sicherung in Azure und anschließendes Sichern von Workloads per MABS
- [Einrichten von DPM](backup-azure-dpm-introduction.md) für die Sicherung in Azure und anschließendes Sichern von Workloads per DPM

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
