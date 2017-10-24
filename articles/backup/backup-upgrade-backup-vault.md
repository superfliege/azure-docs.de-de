---
title: "Durchführen eines Upgrades für einen Sicherungstresor zum Recovery Services-Tresor von Azure Backup | Microsoft-Dokumentation"
description: "Durchführen eines Upgrades für einen Sicherungstresor zum Recovery Services-Tresor, um neue Features wie die Sicherung von Resource Manager-VMs, erhöhte Sicherheit, VMware-VM-Sicherungen und Systemstatussicherung für Windows-Server zu erhalten."
services: backup
documentationcenter: 
author: trinadhk
manager: vijayts
editor: 
keyword: backup vault; upgrade vault; recovery services vault
ms.assetid: d037a8bf-49f2-4578-974a-3471d87ca278
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: trinadhk, sogup
ms.openlocfilehash: cfc2fde552b029412042474e31a1b28dd80b3021
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="backup-vault-upgraded-to-recovery-services-vault"></a>Zum Recovery Services-Tresor aktualisierter Sicherungstresor
Dieser Artikel bietet einen Überblick darüber, was der Recovery Services-Tresor bietet, enthält häufig gestellte Fragen zum Upgrade des vorhandenen Sicherungstresors zum Recovery Services-Tresor und nach dem Upgrade auszuführende Schritte. Ein Recovery Services-Tresor ist die Azure Resource Manager-Entsprechung eines Sicherungstresors, der Ihre Sicherungsdaten enthält. Bei den Daten handelt es sich in der Regel um Kopien von Daten oder Konfigurationsinformationen für virtuelle Computer (VMs), Workloads, Server oder Arbeitsstationen – lokal oder in Azure.

## <a name="what-is-a-recovery-services-vault"></a>Was ist ein Recovery Services-Tresor?
Ein Recovery Services-Tresor ist eine Onlinespeicherentität in Azure, die zum Speichern von Daten wie Sicherungskopien, Wiederherstellungspunkten und Sicherungsrichtlinien verwendet wird. Mit Recovery Services-Tresoren können Sie Sicherungsdaten für verschiedene Azure-Dienste speichern, z.B. IaaS-VMs (Linux oder Windows) und Azure SQL-Datenbanken. Recovery Services-Tresore unterstützen System Center DPM, Windows Server, Azure Backup Server etc. Recovery Services-Tresore vereinfachen die Organisation Ihrer Sicherungsdaten und minimieren gleichzeitig den Verwaltungsaufwand.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Vergleich zwischen Recovery Services- und Sicherungstresoren
Recovery Services-Tresore basieren auf dem Azure Resource Manager-Modell von Azure, wohingegen Sicherungstresore auf dem Azure Services Manager-Modell beruhen. Beim Upgrade eines Sicherungstresors auf einen Recovery Services-Tresor bleiben die Sicherungsdaten während und nach dem Upgradevorgang intakt. Recovery Services-Tresore bieten Funktionen, die nicht für Sicherungstresore verfügbar sind. Hierzu zählen z.B. Folgende:

- **Erweiterte Funktionen zum Schutz von Sicherungsdaten**: Durch Recovery Services-Tresore bietet Azure Backup Sicherheitsfunktionen zum Schutz von Cloudsicherungen. Mit diesen Sicherheitsfunktionen wird sichergestellt, dass Sie Ihre Sicherungen schützen und Daten sicher aus Cloudsicherungen wiederherstellen können, selbst wenn Produktions- und Sicherungsserver kompromittiert sind. [Weitere Informationen](backup-azure-security-feature.md)

- **Zentrale Überwachung Ihrer Hybrid-IT-Umgebung**: Mit Recovery Services-Tresoren können Sie nicht nur Ihre [Azure-IaaS-VMs](backup-azure-manage-vms.md), sondern auch Ihre [lokalen Ressourcen](backup-azure-manage-windows-server.md#manage-backup-items) über ein zentrales Portal überwachen. [Weitere Informationen](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)**: Die RBAC ermöglicht eine präzise Verwaltung der Zugriffssteuerung in Azure. [Azure bietet verschiedene integrierte Rollen](../active-directory/role-based-access-built-in-roles.md), während Azure Backup über drei [integrierte Rollen zum Verwalten von Wiederherstellungspunkten](backup-rbac-rs-vault.md) verfügt. Recovery Services-Tresore sind mit der RBAC kompatibel, die den Zugriff auf Sicherungen und Wiederherstellungen auf den definierten Satz von Benutzerrollen beschränkt. [Weitere Informationen](backup-rbac-rs-vault.md)

- **Schutz aller Konfigurationen von virtuellen Azure-Computern**: Recovery Services-Tresore schützen Resource Manager-basierte VMs, einschließlich Premium-Datenträger, verwaltete Datenträger und verschlüsselte VMs. Durch die Durchführung eines Upgrades eines Sicherungstresors auf einen Recovery Services-Tresor erhalten Sie die Möglichkeit, ein Upgrade für Ihre Service Manager-basierten VMs auf Resource Manager-basierte VMs durchzuführen. Während des Upgrades des Tresors können Sie die Wiederherstellungspunkte für Ihre Service Manager-basierten VMs beibehalten und den Schutz für die aktualisierten (Resource Manager-fähigen) VMs konfigurieren. [Weitere Informationen](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Sofortige Wiederherstellung für IaaS-VMs**: Durch Recovery Services-Tresore können Sie Dateien und Ordner von einer IaaS-VM wiederherstellen, ohne die gesamte VM wiederherstellen zu müssen. So werden die Wiederherstellungszeiten verkürzt. Die sofortige Wiederherstellung für IaaS-VMs ist für Windows- und Linux-VMs verfügbar. [Weitere Informationen](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

> [!NOTE]
> Wenn Sie in einem Sicherungstresor Elemente mit einem MARS-Agent vor 2.0.9083.0 registriert haben, [laden Sie die aktuelle Version des MARS-Agent herunter]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe), um die Vorteile aller Features des Recovery Services-Tresors zu nutzen. 
> 

## <a name="managing-your-recovery-services-vaults"></a>Verwalten von Recovery Services-Tresoren
Auf den folgenden Bildschirmen wird ein neuer Recovery Services-Tresor angezeigt, für den ein Upgrade vom Sicherungstresor im Azure-Portal durchgeführt wurde. Der aktualisierte Tresor befindet sich in einer Standardressourcengruppe mit dem Namen „Default-RecoveryServices-ResourceGroup-geo“. Beispiel: Wenn Ihr Sicherungstresor in „USA, Westen“ gespeichert wurde, wird er in einer standardmäßigen Ressourcengruppe mit dem Namen „Default-RecoveryServices-ResourceGroup-westus“ abgelegt.
> [!NOTE]
> Für CPS-Standardkunden wird die Ressourcengruppe nicht nach dem Upgrade des Tresors geändert und bleibt dieselbe wie vor dem Upgrade.

Auf dem ersten Bildschirm wird das Tresordashboard mit den Hauptentitäten für den Tresor angezeigt.
![Beispiel für einen Recovery Services-Tresor, für den ein Upgrade von einem Sicherungstresor durchgeführt wurde](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

Auf dem zweiten Bildschirm werden verfügbare Hilfelinks angezeigt, die Sie beim Einstieg in die Verwendung des Recovery Services-Tresors unterstützen sollen.

![Hilfelinks auf dem Blatt „Schnellstart“](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Schritte nach dem Upgrade
Der Recovery Services-Tresor unterstützt das Angeben von Zeitzoneninformationen in den Sicherungsrichtlinien. Nachdem der Tresor erfolgreich aktualisiert wurde, rufen Sie die Sicherungsrichtlinien im Menü „Tresoreinstellungen“ auf, und aktualisieren Sie die Zeitzoneninformationen für jede Richtlinie, die im Tresor konfiguriert wurde. Dieser Bildschirm zeigt bereits die Zeit des Sicherungszeitplans gemäß der Ortszeit an, die verwendet wurde, als Sie die Richtlinie erstellt haben. 

## <a name="enhanced-security"></a>Erweiterte Sicherheit
Wenn ein Sicherungstresor auf einen Recovery Services-Tresor aktualisiert wird, werden die Sicherheitseinstellungen für diesen Tresor automatisch aktiviert. Wenn die Sicherheitseinstellungen eingeschaltet sind, erfordern bestimmte Vorgänge wie z.B. das Löschen von Sicherungen oder das Ändern einer Passphrase einen [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)-PIN. Weitere Informationen zur erhöhten Sicherheit finden Sie im Artikel [Sicherheitsfeatures für den Schutz von Hybridsicherungen](backup-azure-security-feature.md). Wenn die erhöhte Sicherheit aktiviert ist, werden Daten bis zu 14 Tage beibehalten, nachdem die Wiederherstellungspunktinformationen aus dem Tresor gelöscht wurden. Den Kunden wird das Speichern dieser Sicherheitsdaten in Rechnung gestellt. Die Beibehaltung der Sicherheitsdaten gilt für Wiederherstellungspunkt, die für den Azure Backup-Agent, den Azure Backup Server und den System Center Data Protection-Manager aufgestellt wurden. 

## <a name="gather-data-on-your-vault"></a>Sammeln von Daten in Ihrem Tresor
Konfigurieren Sie nach einem Upgrade auf einen Recovery Services-Tresor Berichte für Azure Backup (für IaaS-VMs und Microsoft Azure Recovery Services-Agent), und verwenden Sie Power BI, um auf die Berichte zuzugreifen. Weitere Informationen zum Sammeln von Daten finden Sie im Artikel [Konfigurieren von Azure Backup-Berichten](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wirkt sich der Upgradeplan auf meine laufenden Sicherungen aus?**</br>
Nein. Laufende Sicherungen werden während und nach dem Upgrade ohne Unterbrechung durchgeführt.

**Was bedeutet dieser Upgradeplan für meine vorhandenen Tools?**</br>
Sie müssen Ihre bestehende Automatisierung oder die bestehenden Tools auf das Resource Manager-Bereitstellungsmodell aktualisieren, um sicherzustellen, dass sie nach dem Upgrade weiterhin funktionieren. Ziehen Sie hierzu die Referenzen zu PowerShell-Cmdlets für das [Service Manager-Bereitstellungsmodell](backup-client-automation-classic.md) und das [Resource Manager-Bereitstellungsmodell](backup-client-automation.md) zu Rate.

**Kann ich nach dem Upgrade ein Rollback ausführen?**</br>
Nein. Ein Rollback wird nicht unterstützt, nachdem erfolgreich ein Upgrade für die Ressourcen durchgeführt wurde.

**Kann ich meinen klassischen Tresor nach dem Upgrade anzeigen?**</br>
Nein. Sie können den klassischen Tresor nach dem Upgrade weder anzeigen noch verwalten. Sie können lediglich das neue Azure-Portal für alle Verwaltungsaktionen im Tresor verwenden.

**Warum kann ich nicht vom MARS-Agent geschützte Server in meinem aktualisierten Tresor sehen?**</br>
Sie müssen den aktuellen MARS-Agent installieren, um alle vom MARS-Agent in Ihrem Tresor geschützten Server zu sehen. Sie können die neueste Version des Agent [hier]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe) herunterladen.

**Ich kann die Sicherungsrichtlinie für die Server, die durch den MARS-Agent geschützt werden, nach dem Upgrade nicht sehen.**</br>
Die Tresorsicherungsrichtlinie ist möglicherweise nicht mehr aktuell und konnte daher nicht mit dem aktualisierten Tresor synchronisiert werden. Aktualisieren Sie die Richtlinie, um sicherzustellen, dass Sie weiterhin Ihre Richtlinien im aktualisierten Tresor sehen.
Um die Richtlinie zu aktualisieren, wechseln Sie zum MARS-Agent, und aktualisieren Sie die konfigurierte Sicherungsrichtlinie.

**Warum kann ich meine Sicherungsrichtlinie nach dem Upgrade nicht aktualisieren?**</br>
Dies geschieht, wenn Sie in einem alten Sicherungs-Agent eine Mindestbeibehaltungsdauer wählen, die den zulässigen Mindestwert unterschreitet. Wenn ein Sicherungstresor auf einen Recovery Services-Tresor aktualisiert wird, werden die Sicherheitseinstellungen für diesen Tresor automatisch aktiviert. Um sicherzustellen, dass immer eine gültige Zahl von Wiederherstellungspunkten verfügbar ist, muss eine Mindestbeibehaltungsdauer gemäß dem Sicherheitsfeature eingehalten werden. Nähere Informationen finden Sie [hier](backup-azure-security-feature.md).
Darüber hinaus müssen Sie Ihre Azure Backup-Agents auf die neueste Version aktualisieren, um die Vorteile der neuesten Features von Azure Backup zu nutzen.

**Ich habe meinen Agent aktualisiert, sehe aber noch Tage nach dem Upgrade keine Objekte, die synchronisiert wurden.**</br>
Überprüfen Sie, ob Sie den gleichen Computer bei mehreren Tresoren registriert haben. Stellen Sie sicher, dass Sie denselben Tresor betrachten, bei dem der MARS-Agent registriert ist. Um herauszufinden, bei welchem Tresor der MARS-Agent registriert ist, öffnen Sie die Windows-Registrierung, und überprüfen Sie den Wert für den Schlüssel „ServiceResourceName“ unter „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config“. Der bei diesem MARS-Agent registrierte Tresor wird dort angezeigt. Wenn der ServiceResourceName-Schlüssel nicht im System sichtbar ist, nennen Sie uns die Werte der Schlüssel ResourceId und MachineId unter „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config“, und wir helfen Ihnen, das Problem zu beheben.

**Warum kann ich die Auftragsinformationen für meine Ressourcen nach dem Upgrade nicht finden?**</br>
Die Überwachung für Sicherungen (MARS-Agent und IaaS) ist ein neues Feature, von dem Sie profitieren, wenn Sie Ihren Sicherungstresor auf den Recovery Services-Tresor upgraden. Es dauert 12 Stunden, bis die Überwachungsinformationen mit dem Dienst synchronisiert werden.

**Wie melde ich ein Problem?**</br>
Wenn irgendein Teil des Tresorupgrades fehlschlägt, beachten Sie die in der Fehlermeldung aufgeführte OperationID. Der Microsoft-Support wird proaktiv daran arbeiten, das Problem zu beheben. Bei Fragen oder Vorschlägen, senden Sie eine E-Mail an rsvaultupgrade@service.microsoft.com, unter Angabe Ihrer Abonnement-ID, des Tresornamens und der Vorgangs-ID. Es wird versucht, das Problem so schnell wie möglich zu beheben. Wiederholen Sie den Vorgang nicht, es sei denn, Sie werden von Microsoft explizit dazu aufgefordert.

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie die folgenden Artikel für:</br>
[Sichern einer IaaS-VM](backup-azure-arm-vms-prepare.md)</br>
[Sichern von Azure Backup Server](backup-azure-microsoft-azure-backup.md)</br>
[Sichern von Windows Server](backup-configure-vault.md)
