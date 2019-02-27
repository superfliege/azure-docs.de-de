---
title: Supportmatrix für die Sicherung mit Azure Backup von Computern, auf denen der Microsoft Azure Recovery Services (MARS)-Agents ausgeführt wird
description: Dieser Artikel fasst die Azure Backup-Unterstützung zusammen, wenn Sie Computer sichern, auf denen der Microsoft Azure Recovery Services (MARS)-Agent ausgeführt wird.
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 9d0f751e8d0bc0275cc5fd2c47aaba7a6058931c
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430934"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Supportmatrix für die Sicherung mit dem Microsoft Azure Recovery Services (MARS)-Agent

Mit dem [Azure Backup-Dienst](backup-overview.md) können Sie lokale Computer und Apps sowie virtuelle Azure-Computer sichern. Dieser Artikel fasst die Supporteinstellungen und -einschränkungen für die Sicherung von Computern mit dem Microsoft Azure Recovery Services (MARS)-Agent zusammen.

## <a name="about-the-mars-agent"></a>Informationen zum MARS-Agent

Der MARS-Agent wird von Azure Backup verwendet, um Daten von lokalen Computern und Azure-VMs in einem Backup Recovery Services-Tresor in Azure zu sichern. Sie können den MARS-Agent wie folgt verwenden:
- Führen Sie den Agenten auf lokalen Windows-Computern aus, damit sie direkt auf einen Backup-Recovery Services-Tresor in Azure sichern können.
- Führen Sie den Agent auf Windows Azure-VMs aus, damit er direkt in einen Tresor gesichert werden kann.
- Führen Sie den Agent auf einem Microsoft Azure Backup Server (MABS) oder einem System Center Data Protection Manager (DPM)-Server aus. In diesem Szenario werden Computer und Workloads auf MABS/DPM gesichert, und dann wird MABS/DPM mit dem MARS-Agent in einem Tresor in Azure gesichert. 

Was Sie sichern können, hängt davon ab, wo der Agent installiert ist.

- [Erfahren Sie mehr](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders) über die Sicherungsarchitektur mithilfe des MARS-Agents.
- [Erfahren Sie mehr]() über die MABS/DPM [-Sicherungsarchitektur](backup-architecture.md#architecture-back-up-to-dpmmabs) und die [Anforderungen](backup-support-matrix-mabs-dpm.md).


## <a name="supported-installation"></a>Unterstützte Installation

**Installieren** | **Details**
--- | ---
**Aktuellen MARS-Agent herunterladen** | Sie können die neueste Version des Agenten aus dem Tresor herunterladen oder [direkt herunterladen](https://aka.ms/azurebackup_agent).
**Direkt auf dem Computer installieren** | Sie können den MARS-Agent direkt auf einem lokalen Windows-Server oder einer Windows Azure-VM mit einem der [unterstützten Betriebssysteme]() installieren.
**Auf einem Sicherungsserver installieren** | Wenn Sie DPM oder MABS für die Sicherung auf Azure einrichten, laden Sie den MARS-Agent herunter und installieren ihn auf dem Server. Der Agent kann gemäß den [unterstützten Betriebssystemen](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) in der Supportmatrix des Sicherungsservers installiert werden.

> [!NOTE]
> Standardmäßig verfügen Azure-VMs, die für die Sicherung aktiviert sind, über eine Azure Backup-Erweiterungsinstallation. Diese Erweiterung sichert die gesamte VM. Sie können den MARS-Agent auf einer Azure-VM neben der Erweiterung installieren und ausführen, wenn Sie nur bestimmte Ordner und Dateien und nicht die gesamte VM sichern möchten.
> Wenn Sie den MARS-Agent auf einer Azure-VM ausführen, werden Dateien/Ordner gesichert, die sich im temporären Speicher auf der VM befinden. Sicherungen schlagen fehl, wenn die Dateien/Ordner aus dem temporären Speicher entfernt werden oder wenn der temporäre Speicher entfernt wird.


## <a name="cache-folder-support"></a>Unterstützung für Cacheordner

Wenn Sie mit dem MARS-Agent sichern, erstellt der Agent eine Momentaufnahme der Daten und speichert sie in einem lokalen Cacheordner, bevor er sie an Azure sendet. Für den Cacheordner (neu) gelten eine Reihe von Anforderungen.

**Cache** | **Details**
--- | ---
**Cachegröße** |  Der freie Speicherplatz im Cacheordner sollte mindestens 5-10 % der Gesamtgröße Ihrer Sicherungsdaten betragen. 
**Speicherort für Cache** | Der Cacheordner muss sich lokal auf dem Computer befinden, die gesichert wird, und er muss online sein.<br/><br/> Der Cacheordner sollte sich nicht auf einer Netzwerkfreigabe, auf Wechselmedien oder auf einem Offline-Volume befinden. 
**Cacheordner** | Der Cacheordner sollte verschlüsselt sein, auf einem deduplizierten Volume oder auf einem Ordner sein, der komprimiert ist/eine geringe Dichte/einen Analysepunkt hat
**Speicherort des Cache ändern** | Sie können den Cachespeicherort ändern, indem Sie die Sicherungs-Engine beenden (net stop bengine) und den Cacheordner auf ein neues Laufwerk kopieren (stellen Sie sicher, dass genügend Platz vorhanden ist). Dann aktualisieren Sie zwei Registrierungseinträge unter HKLM\SOFTWARE\Microsoft\Windows Azure Backup („Config/ScratchLocation“ und „Config/CloudBackupProvider/ScratchLocation“) auf den neuen Speicherort und starten die Engine neu.

## <a name="networking-and-access-support"></a>Netzwerk und Zugriffsunterstützung

### <a name="url-access"></a>URL-Zugriff

Der MARS-Agent benötigt Zugriff auf diese URLs:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

### <a name="throttling-support"></a>Unterstützung für Drosselung

**Feature** | **Details**
--- | ---
Bandbreitensteuerung | Unterstützt<br/><br/> Über **Eigenschaften ändern** können Sie die Bandbreite auf dem MARS-Agent anpassen.
Netzwerkdrosselung | Nicht verfügbar für gesicherte Computer mit Windows Server 2008 R2, Windows Server 2008 SP2 oder Windows 7.

## <a name="support-for-direct-backups"></a>Unterstützung für direkte Sicherungen

Die folgende Tabelle fasst zusammen, welche Betriebssysteme, die auf lokalen Computern und Azure VMs ausgeführt werden, mit dem MARS-Agent direkt in Azure gesichert werden können.

- Alle Betriebssysteme sind 64-Bit.
- Alle Betriebssysteme müssen mit den neuesten Service Packs und Updates ausgeführt werden.
- Weitere Informationen darüber, welche DPM- und MABS-Server mit dem MARS-Agent gesichert werden können, finden Sie in [dieser Tabelle](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).

**Betriebssystem** | **Dateien/Ordner** | **Systemstatus**
--- | --- | ---
Windows 10 (Enterprise, Pro, Home) | Ja | Nein 
Windows 8.1 (Enterprise, Pro)| Ja |Nein 
Windows 8 (Enterprise, Pro) | Ja | Nein 
Windows 7 (Ultimate, Enterprise, Pro, Home Premium/Basic, Starter) | Ja | Nein 
Windows Server 2016 (Standard, Datacenter, Essentials) | Ja | Ja
Windows Server 2012 R2 (Standard, Datacenter, Foundation, Essentials) | Ja | Ja
Windows Server 2012 (Standard, Datacenter, Foundation) | Ja | Ja
Windows Server 2008 R2 (Standard, Enterprise, Datacenter, Foundation) | Ja | Ja
Windows Server 2008 SP2 (Standard, Datacenter, Foundation) | Ja | Nein 
Windows Storage Server 2016/2012 R2/2012(Standard, Workgroup) | Ja | Nein 


## <a name="backup-limits"></a>Backup-Grenzwerte

Azure Backup begrenzt die Größe einer Datei/Ordner-Datenquelle, die gesichert werden kann. Die Größe der Elemente, die für die Sicherung von einem einzelnen Datenträger ausgewählt wurden, darf die in der Tabelle zusammengefassten Größen nicht überschreiten.

**Betriebssystem** | **Größenlimit**
--- | ---
Windows Server 2012 oder höher |  54.400 GB
Windows Server 2008 R2 SP1 |    1.700 GB
Windows Server 2008 SP2 | 1.700 GB
Windows 8 oder höher  | 54.400 GB
Windows 7   | 1.700 GB


## <a name="supported-file-types-for-backup"></a>Unterstützte Dateitypen für die Sicherung

**Typ** | **Unterstützt** 
--- | --- 
Verschlüsselt   | Ja 
Komprimiert | Ja
Platzsparend | Ja 
Komprimiert und geringe Dichte | Ja
Feste Links  | Nicht unterstützt<br/><br/> Übersprungen
Analysepunkt   | Nicht unterstützt<br/><br/> Übersprungen
Verschlüsselt und geringe Dichte |  Nicht unterstützt<br/><br/> Übersprungen
Komprimierter Stream   | Nicht unterstützt<br/><br/> Übersprungen
Stream mit geringer Dichte   | Nicht unterstützt<br/><br/> Übersprungen
Ein Laufwerk (synchronisierte Dateien sind Stream mit geringer Dichte)    | Nicht unterstützt 

## <a name="supported-drivesvolumes-for-backup"></a>Unterstützte Laufwerke/Volumes für die Sicherung

**Laufwerk/Volume** | **Unterstützt** | **Details**
--- | --- | ---
Schreibgeschützte Volumes   | Nicht unterstützt | Das Volume muss beschreibbar sein, damit der VSS funktioniert.
Offlinevolumes | Nicht unterstützt |   Das Volume muss online sein, damit der VSS funktioniert.
Netzwerkfreigabe   | Nicht unterstützt |   Das Volume muss sich lokal auf dem Server für die Sicherung befinden.
Mit BitLocker geschützte Volumes | Nicht unterstützt |   Das Volume muss entsperrt werden, damit die Sicherung erfolgen kann.
Dateisystemidentifikation  | Nicht unterstützt |   Nur NTFS.
Wechselmedien | Nicht unterstützt |   Alle Quellen für Sicherungselemente müssen ein festes Medium sein.
Deduplizierte Laufwerke | Unterstützt.<br/><br/> Azure Backup konvertiert die deduplizierten Daten in reguläre Daten. Es optimiert die Daten, verschlüsselt sie, speichert sie und sendet sie an den Tresor.

## <a name="support-for-initial-offline-backup"></a>Unterstützung für die erste Offlinesicherung

Azure Backup unterstützt „Offline-Seeding“, um erste Sicherungsdaten über Datenträger an Azure zu übertragen. Dies ist hilfreich, wenn Ihre erste Sicherung wahrscheinlich im Bereich von Terabyte (TBs) liegt. Die Offlinesicherung wird unterstützt für:

- Direkte Sicherung von Dateien und Ordnern auf lokalen Computern mit dem MARS-Agent.
- Sicherung von Workloads und Dateien von einem DPM-Server oder MABS.
- Die Offlinesicherung kann nicht für Systemstatusdateien verwendet werden.


## <a name="support-for-restore"></a>Unterstützung der Wiederherstellung

- Die neue Version der [sofortigen Wiederherstellung](/backup-instant-restore-capability.md) von Azure Backup ermöglicht es Ihnen, Daten wiederherzustellen, bevor sie in den Tresor kopiert wurden.<br/><br/> Um diese Funktion nutzen zu können, muss auf dem zu sichernden Computer .NET Framework 4.5.2 oder höher ausgeführt werden.
- Sicherungen können nicht auf einem Zielcomputer wiederhergestellt werden, auf dem eine ältere Version des Betriebssystems ausgeführt wird. Beispielsweise kann eine Sicherung, die von einem Windows 7-Computer erstellt wurde, auf Windows 8 (oder höher) wiederhergestellt werden. Es ist aber nicht möglich, eine Sicherung, die von einem Windows 8-Computer erstellt wurde, auf einem Windows 7-Computer wiederherzustellen.


## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders) über die Sicherungsarchitektur mithilfe des MARS-Agents.
- [Erfahren Sie](backup-support-matrix-mabs-dpm.md), was unterstützt wird, wenn Sie den MARS-Agent auf Microsoft Azure Backup Server (MABS) oder System Center DPM ausführen.


