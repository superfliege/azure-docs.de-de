---
title: Problembehandlung bei System Center Data Protection Manager mit Azure Backup | Microsoft-Dokumentation
description: Behandeln von Problemen in System Center Data Protection Manager.
services: backup
documentationcenter: 
author: adigan
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;adigan
ms.openlocfilehash: 2244a39217f54eb5906d05990f19fc8ca2fdeb0e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Behandeln von Problemen in System Center Data Protection Manager

Die neuesten Versionshinweise für SC DPM finden Sie [hier](https://docs.microsoft.com/en-us/system-center/dpm/dpm-release-notes?view=sc-dpm-2016).
Die Unterstützungsmatrix für DPM-Schutz finden Sie [hier](https://docs.microsoft.com/en-us/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).

## <a name="replica-is-inconsistent"></a>Replikat inkonsistent

Dieser Fehler kann aus verschiedenen Gründen auftreten, z.B. Fehler bei Replikaterstellungsauftrag, Probleme mit dem Änderungsjournal, Volumeebenenfilterbitmap-Fehler, unerwartetes Herunterfahren des Quellcomputers, Überlauf des Synchronisierungsprotokolls oder das Replikat ist tatsächlich inkonsistent. Gehen Sie folgendermaßen vor, um das Problem zu beheben:
- Führen Sie zum Entfernen des inkonsistenten Status eine manuelle Konsistenzprüfung durch, oder planen Sie eine tägliche Konsistenzprüfung.
- Stellen Sie sicher, dass Sie die neueste Version von MAB Server oder System Center DPM verwenden
- Stellen Sie sicher, dass die automatische Konsistenzprüfung aktiviert ist
- Versuchen Sie, die Dienste über die Eingabeaufforderung neu zu starten („net stop dpmra“ gefolgt von „net start dpmra“)
- Stellen Sie sicher, dass Netzwerkkonnektivitäts- und Bandbreitenanforderungen erfüllt sind
- Überprüfen Sie, ob der Quellcomputer unerwartet heruntergefahren wurde
- Stellen Sie sicher, dass der Datenträger fehlerfrei und genügend Speicherplatz für das Replikat vorhanden ist
- Stellen Sie sicher, dass keine doppelten Sicherungsaufträge gleichzeitig ausgeführt werden

## <a name="online-recovery-point-creation-failed"></a>Fehler beim Erstellen eines Onlinewiederherstellungspunkts

Gehen Sie folgendermaßen vor, um das Problem zu beheben:
- Vergewissern Sie sich, dass Sie über die neueste Version des Azure Backup-Agents verfügen
- Versuchen Sie, manuell einen Wiederherstellungspunkt im Aufgabenbereich „Schutz“ zu erstellen
- Stellen Sie sicher, dass Sie die Konsistenzprüfung für die Datenquelle ausführen
- Stellen Sie sicher, dass Netzwerkkonnektivitäts- und Bandbreitenanforderungen erfüllt sind
- Die Replikatdaten befinden sich in einem inkonsistenten Zustand. Erstellen Sie einen Datenträger-Wiederherstellungspunkt für diese Datenquelle
- Stellen Sie sicher, dass das Replikat vorhanden ist und nicht fehlt
- Für das Replikat ist ausreichend Speicherplatz zum Erstellen des USN-Journals verfügbar

## <a name="unable-to-configure-protection"></a>Der Schutz kann nicht konfiguriert werden.

Dieser Fehler wird angezeigt, wenn der DPM-Server keinen Kontakt mit dem geschützten Server herstellen kann. Gehen Sie folgendermaßen vor, um das Problem zu beheben:
- Vergewissern Sie sich, dass Sie über die neueste Version des Azure Backup-Agents verfügen
- Stellen Sie sicher, dass eine Verbindung (Netzwerk/Firewall/Proxy) des DPM-Servers mit dem geschützten Server vorhanden ist
- Wenn Sie SQL Server schützen, stellen Sie sicher, dass in den Anmeldungseigenschaften für NT AUTHORITY\SYSTEM „sysadmin“ aktiviert ist

## <a name="this-server-is-not-registered-to-the-vault-specified-by-the-vault-credential"></a>Der Server ist nicht bei dem Tresor registriert, der durch die Tresoranmeldeinformationen angegeben ist.

Dieser Fehler wird angezeigt, wenn die ausgewählte Datei mit den Tresoranmeldeinformationen nicht zu dem Recovery Services-Tresor gehört, dem die System Center DPM- / Azure Backup Server-Instanz zugeordnet ist, auf der die Wiederherstellung versucht wird. Gehen Sie folgendermaßen vor, um das Problem zu beheben:
- Laden Sie die Anmeldeinformationsdatei für den Tresor aus dem Recovery Services-Tresor herunter, bei dem die System Center DPM- / Azure Backup Server-Instanz registriert ist.
- Versuchen Sie, den Server mit der neuesten heruntergeladenen Datei mit Tresoranmeldeinformationen bei dem Tresor zu registrieren.

## <a name="either-the-recoverable-data-is-not-available-or-the-selected-server-is-not-a-dpm-server"></a>Entweder sind die wiederherstellbaren Daten nicht verfügbar, oder der ausgewählte Server ist kein DPM-Server
Dieser Fehler tritt auf, wenn keine anderen System Center DPM- / Azure Backup Server-Instanzen bei dem Recovery Services-Tresor registriert sind, oder die Server haben ihre Metadaten noch nicht hochgeladen, oder der ausgewählte Server ist keine System Center DPM- / Azure Backup Server-Instanz.
- Wenn noch andere System Center DPM- / Azure Backup Server-Instanzen bei dem Recovery Services-Tresor registriert sind, müssen Sie sicherstellen, dass der neueste Azure Backup-Agent installiert ist.
- Sind noch andere System Center DPM- / Azure Backup Server-Instanzen bei dem Recovery Services-Tresor registriert, warten Sie nach der Installation einen Tag, um den Wiederherstellungsprozess zu starten. Während der Nacht werden die Metadaten für alle geschützten Sicherungen in die Cloud hochgeladen. Die Daten sind dann für die Wiederherstellung verfügbar.

## <a name="the-encryption-passphrase-provided-does-not-match-with-passphrase-associated-with-the-following-server"></a>Die angegebene Verschlüsselungspassphrase stimmt nicht mit der Passphrase überein, die dem folgenden Server zugeordnet ist

> [!NOTE]
>Wenn Sie die Verschlüsselungspassphrase vergessen/verloren haben, steht keine Option zum Wiederherstellen der Daten zur Verfügung. Ihre einzige Option ist, die Passphrase erneut zu generieren und zum Verschlüsseln zukünftiger Sicherungsdaten zu verwenden.
>
>

Dieser Fehler tritt auf, wenn die beim Verschlüsseln der wiederherzustellenden Daten auf der System Center DPM- / Azure Backup Server-Instanz verwendete Verschlüsselungspassphrase nicht mit der angegebenen Verschlüsselungspassphrase übereinstimmt. Der Agent kann die Daten nicht entschlüsseln. Daher schlägt die Wiederherstellung fehl. Gehen Sie folgendermaßen vor, um das Problem zu beheben:
- Geben Sie genau dieselbe Verschlüsselungspassphrase an, die der System Center DPM- / Azure Backup Server-Instanz zugeordnet ist, deren Daten wiederhergestellt werden sollen. 
