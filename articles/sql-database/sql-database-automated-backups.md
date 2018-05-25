---
title: Automatische, georedundante Azure SQL-Datenbank-Sicherungen | Microsoft-Dokumentation
description: SQL-Datenbank erstellt alle paar Minuten automatisch eine lokale Datenbanksicherung und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.workload: Active
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 37bbbf8ea5a5d8439b300d0740e4f1a048e98e91
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="learn-about-automatic-sql-database-backups"></a>Informationen zu automatischen Sicherungen von SQL-Datenbank

SQL-Datenbank erstellt automatisch Datenbanksicherungen und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen. Diese Sicherungen werden automatisch und ohne zusätzliche Kosten erstellt. Sie müssen keinerlei Aktionen durchführen. Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Wenn Sie Sicherungen in Ihrem eigenen Speichercontainer beibehalten möchten, können Sie eine Richtlinie für die langfristige Aufbewahrung der Sicherung konfigurieren. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

## <a name="what-is-a-sql-database-backup"></a>Was ist die Sicherung einer SQL-Datenbank-Instanz?

SQL-Datenbank verwendet die SQL Server-Technologie, um [vollständige Sicherungen](https://msdn.microsoft.com/library/ms186289.aspx), [differenzielle Sicherungen](https://msdn.microsoft.com/library/ms175526.aspx) und [Transaktionsprotokollsicherungen](https://msdn.microsoft.com/library/ms191429.aspx) zum Zwecke der Point-in-Time-Wiederherstellung (Point-In-Time Restore, PITR) zu erstellen. Die Transaktionsprotokollsicherungen erfolgen im Allgemeinen alle 5-10 Minuten, wobei die Häufigkeit auf der Leistungsebene und der Menge an Datenbankaktivität basiert. Transaktionsprotokollsicherungen mit vollständigen und differenziellen Sicherungen ermöglichen die Wiederherstellung einer Datenbank zu einem bestimmten Zeitpunkt auf dem gleichen Server, der die Datenbank hostet. Wenn Sie eine Datenbank wiederherstellen, ermittelt der Dienst, welche vollständigen und differenziellen Sicherungen bzw. Transaktionsprotokollsicherungen wiederhergestellt werden müssen.


Sie können diese Sicherungen für Folgendes verwenden:

* Stellen Sie eine Datenbank innerhalb der Aufbewahrungsdauer auf einen Zeitpunkt wieder her. Dieser Vorgang wird eine neue Datenbank auf dem gleichen Server wie die ursprüngliche Datenbank erstellen.
* Wiederherstellen einer gelöschten Datenbank auf den Zeitpunkt, zu dem sie gelöscht wurde, oder auf einen beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums. Die gelöschte Datenbank kann nur auf dem Server wiederhergestellt werden, auf dem die ursprüngliche Datenbank erstellt wurde.
* Wiederherstellen einer Datenbank in einer anderen geografischen Region. Dies ermöglicht die Wiederherstellung nach dem Ausfall einer geografischen Region, wenn Sie keinen Zugriff auf Ihren Server und Ihre Datenbank haben. Dabei wird eine neue Datenbank auf einem beliebigen Server an einem beliebigen Ort der Welt erstellt. 
* Wiederherstellen einer Datenbank auf der Grundlage einer spezifischen langfristigen Sicherung, wenn die Datenbank mit einer Richtlinie zur langfristigen Aufbewahrung konfiguriert wurde. Dadurch können Sie eine alte Version der Datenbank wiederherstellen, um eine Konformitätsanforderung zu erfüllen oder eine alte Version der Anwendung auszuführen. Weitere Informationen hierzu finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).
* Informationen zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md).

> [!NOTE]
> In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort zum anderen. Die *Datenbankreplikation* der SQL bezieht sich auf das Beibehalten von mehreren sekundären Datenbanken, die mit einer primären Datenbank synchronisiert werden. 
> 

## <a name="how-often-do-backups-happen"></a>Wie oft erfolgen Sicherungen?
Vollständige Datenbanksicherungen werden wöchentlich, differenzielle Datenbanksicherungen im Allgemeinen alle paar Stunden und Transaktionsprotokollsicherungen alle 5-10 Minuten ausgeführt. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird normalerweise innerhalb von 30 Minuten abgeschlossen, aber der Vorgang kann auch länger dauern, wenn es sich um eine sehr große Datenbank handelt. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Die genaue Zeitplanung für alle Datenbanksicherungen wird vom SQL-Datenbank-Dienst je nach der gesamten Systemworkload bestimmt. 

Die Georeplikation des Sicherungsspeichers erfolgt basierend auf dem Azure Storage-Replikationszeitplan.

## <a name="how-long-do-you-keep-my-backups"></a>Wie lang werden meine Sicherungen aufbewahrt?
Für jede SQL-Datenbanksicherung gilt eine Beibehaltungsdauer, die auf dem Diensttarif der Datenbank basiert. Dabei wird zwischen dem [DTU-basierten Kaufmodell](sql-database-service-tiers-dtu.md) und dem [V-Kern-basierten Kaufmodell (Vorschauversion)](sql-database-service-tiers-vcore.md) unterschieden. 


### <a name="database-retention-for-dtu-based-purchasing-model"></a>Beibehaltung der Datenbank im DTU-basierten Kaufmodell
Die Beibehaltungsdauer für eine Datenbank im DTU-basierten Kaufmodell hängt vom Diensttarif ab. Nachstehend finden Sie die jeweilige Beibehaltungsdauer für eine Datenbank:

* Tarif „Basic“: 7 Tage
* Tarif „Standard“: 35 Tage
* Tarif „Premium“: 35 Tage
* Im Tarif „Universell“ können maximal 35 Tage konfiguriert werden (Standardwert: 7 Tage).*
* Im Tarif „Unternehmenskritisch“ (Vorschauversion) können maximal 35 Tagen konfiguriert werden (Standardwert: 7 Tage).*

\* Während der Vorschauphase kann der Aufbewahrungszeitraum nicht konfiguriert werden und ist auf sieben Tage festgelegt.

Wenn Sie eine Datenbank mit längerem Sicherungsaufbewahrungszeitraum in eine Datenbank mit kürzerem Aufbewahrungszeitraum konvertieren, sind alle vorhandenen Sicherungen, die älter sind als der Aufbewahrungszeitraum des Zieltarifs, nicht mehr verfügbar.

Wenn Sie eine Datenbank mit einem kürzeren Aufbewahrungszeitraum auf eine Datenbank mit einem längeren Aufbewahrungszeitraum aktualisieren, behält SQL-Datenbank vorhandene Sicherungen bei, bis der längere Aufbewahrungszeitraum erreicht ist. 

Wenn Sie eine Datenbank löschen, bewahrt SQL-Datenbank die Sicherungen auf die gleiche Weise auf wie für eine Onlinedatenbank. Ein Beispiel: Sie löschen eine Basic-Datenbank, die einen Aufbewahrungszeitraum von sieben Tagen aufweist. Eine vier Tage alte Sicherung wird drei weitere Tage gespeichert.

> [!IMPORTANT]
> Wenn Sie die Azure SQL Server-Instanz löschen, auf der die SQL-Datenbanken gehostet werden, werden alle Datenbanken, die zum Server gehören, gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen.

### <a name="database-retention-for-the-vcore-based-purchasing-model-preview"></a>Beibehaltung der Datenbank im V-Kern-basierten Kaufmodell (Vorschauversion)

Der Speicher für Datenbanksicherungen wird zugeordnet, um die Funktionen „Point-in-Time-Wiederherstellung“ (Point in Time Restore, PITR) und „Langfristige Aufbewahrung“ (Long Term Retention, LTR) von SQL-Datenbank zu unterstützen. Dieser Speicher wird für jede Datenbank separat zugeordnet und als zwei Arten von getrennten Datenbankgebühren berechnet. 

- **PITR**: Einzelne Datenbanksicherungen werden automatisch in RA-GRS-Speicher kopiert. Die Speichergröße wird dynamisch erhöht, wenn die neuen Sicherungen erstellt werden.  Der Speicher wird für wöchentliche vollständige Sicherungen, tägliche differenzielle Sicherungen und im 5-Minuten-Takt kopierte Sicherungen von Transaktionsprotokollen verwendet. Der Speicherverbrauch richtet sich nach der Änderungsrate der Datenbank und nach der Aufbewahrungsdauer. Sie können für jede Datenbank eine separate Aufbewahrungsdauer konfigurieren, die zwischen 7 und 35 Tagen liegt. Eine Mindestspeichermenge, die der Gesamtgröße der Daten entspricht, wird kostenlos zur Verfügung gestellt. Für die meisten Datenbanken reicht diese Menge aus, um Sicherungen für sieben Tage aufzubewahren. Weitere Informationen finden Sie unter [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore).
- **LTR**: SQL-Datenbank verfügt über eine Option zum Konfigurieren der langfristigen Aufbewahrung von vollständigen Sicherungen für eine Dauer von bis zu zehn Jahren. Wenn die LTR-Richtlinie aktiviert ist, werden diese Sicherungen automatisch in RA-GRS-Speicher gespeichert, aber Sie können steuern, wie häufig die Sicherungen kopiert werden. Zur Erfüllung unterschiedlicher Konformitätsanforderungen können Sie verschiedene Aufbewahrungsdauern für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Mit dieser Konfiguration wird definiert, wie viel Speicher für die LTR-Sicherungen verwendet wird. Sie können den LTR-Preisrechner verwenden, um die Kosten für den LTR-Speicher zu schätzen. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

## <a name="how-to-extend-the-backup-retention-period"></a>Wie kann der Aufbewahrungszeitraum für Sicherungen erweitert werden?

Wenn Ihre Anwendung erfordert, dass die Sicherungen für einen längeren Zeitraum als der maximale Aufbewahrungszeitraum für PITR-Sicherungen verfügbar sind, können Sie für einzelne Datenbanken eine Richtlinie für die langfristige Aufbewahrung von Sicherungen (LTR-Richtlinie, Long-Term Retention) konfigurieren. Dadurch können Sie den integrierten Aufbewahrungszeitraum von maximal 35 Tagen auf bis zu zehn Jahre erweitern. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

Sobald Sie die LTR-Richtlinie einer Datenbank mithilfe des Azure-Portals oder der API hinzufügen, werden die wöchentlichen vollständigen Datenbanksicherungen automatisch in einen separaten RA-GRS-Speichercontainer für die langfristige Aufbewahrung (LTR-Speicher) kopiert. Wenn Ihre Datenbank mit TDE verschlüsselt ist, werden die Sicherungen automatisch im Ruhezustand verschlüsselt. SQL-Datenbank löscht Ihre abgelaufenen Sicherungen basierend auf ihrem Zeitstempel und der LTR-Richtlinie automatisch. Nach Einrichten der Richtlinien müssen Sie also weder den Sicherungszeitplan verwalten noch sich Gedanken über die Bereinigung der alten Dateien machen. Sie können das Azure-Portal oder PowerShell verwenden, um diese Sicherungen anzuzeigen, wiederherzustellen oder zu löschen.

## <a name="are-backups-encrypted"></a>Werden Sicherungen verschlüsselt?

Wenn Transparent Data Encryption (TDE) für eine Azure SQL-Datenbank aktiviert ist, werden die Sicherungen ebenfalls verschlüsselt. In allen neuen Azure SQL-Datenbanken ist TDE standardmäßig aktiviert. Weitere Informationen finden Sie unter [Transparente Datenverschlüsselung in Azure SQL-Datenbank](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="are-the-automatic-backups-compliant-with-gdpr"></a>Sind die automatischen Sicherungen mit der Datenschutz-Grundverordnung (DSGVO) konform?
Wenn die Sicherung personenbezogene Daten enthält, die der DSGVO unterliegen, müssen Sie zusätzliche Sicherheitsmaßnahmen treffen, um die Daten vor nicht autorisiertem Zugriff zu schützen. Zur Einhaltung der DSGVO müssen Sie die Datenanforderungen von Datenbesitzern verwalten, ohne auf Sicherungen zugreifen zu müssen.  Für kurzfristige Sicherungen kann eine Lösung darin bestehen, das Sicherungsfenster auf maximal 30 Tage zu begrenzen. Dies ist der zum Ausführen von Datenzugriffsanforderungen zulässige Zeitraum.  Wenn längerfristige Sicherungen erforderlich sind, wird empfohlen, nur „pseudonymisierte“ Daten in Sicherungen zu speichern. Beispiel: Wenn Daten zu einer Person gelöscht oder aktualisiert werden müssen, müssen vorhandene Sicherungen nicht gelöscht oder aktualisiert werden. Weitere Informationen zu bewährten Methoden im Zusammenhang mit der DSGVO finden Sie unter [Data Governance for GDPR Compliance](https://info.microsoft.com/DataGovernanceforGDPRCompliancePrinciplesProcessesandPractices-Registration.html) (Datengovernance zur Einhaltung der DSGVO).

## <a name="next-steps"></a>Nächste Schritte

- Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von Azure SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt über das Azure-Portal finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-recovery-using-backups.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe von PowerShell finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](scripts/sql-database-restore-database-powershell.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen in einem Azure Recovery Services-Tresor mit dem Azure-Portal finden Sie im Artikel über das [Verwalten der langfristigen Sicherungsaufbewahrung mit dem Azure-Portal](sql-database-long-term-backup-retention-configure.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen in einem Azure Recovery Services-Tresor mit PowerShell finden Sie im Artikel über das [Verwalten der langfristigen Sicherungsaufbewahrung mit PowerShell](sql-database-long-term-backup-retention-configure.md).
