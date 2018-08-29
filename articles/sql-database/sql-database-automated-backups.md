---
title: Automatische, georedundante Azure SQL-Datenbank-Sicherungen | Microsoft-Dokumentation
description: SQL-Datenbank erstellt alle paar Minuten automatisch eine lokale Datenbanksicherung und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: Active
ms.date: 07/25/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: ac548d90d5a5ed931dc199b6fed52c7cd8f25239
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42144651"
---
# <a name="learn-about-automatic-sql-database-backups"></a>Informationen zu automatischen Sicherungen von SQL-Datenbank

SQL-Datenbank erstellt automatisch Datenbanksicherungen und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen. Diese Sicherungen werden automatisch und ohne zusätzliche Kosten erstellt. Sie müssen keinerlei Aktionen durchführen. Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Falls es für Ihre Sicherheitsregeln erforderlich ist, dass Ihre Sicherungen über einen längeren Zeitraum verfügbar sind, können Sie eine Richtlinie für die langfristige Aufbewahrung von Sicherungen konfigurieren. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Was ist die Sicherung einer SQL-Datenbank-Instanz?

SQL-Datenbank verwendet die SQL Server-Technologie, um [vollständige Sicherungen](https://msdn.microsoft.com/library/ms186289.aspx), [differenzielle Sicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) und [Transaktionsprotokollsicherungen](https://msdn.microsoft.com/library/ms191429.aspx) zum Zwecke der Point-in-Time-Wiederherstellung (Point-In-Time Restore, PITR) zu erstellen. Die Transaktionsprotokollsicherungen erfolgen im Allgemeinen alle 5-10 Minuten und differenzielle Sicherungen alle 12 Stunden, wobei die Häufigkeit auf der Leistungsebene und dem Umfang der Datenbankaktivität basiert. Transaktionsprotokollsicherungen mit vollständigen und differenziellen Sicherungen ermöglichen die Wiederherstellung einer Datenbank zu einem bestimmten Zeitpunkt auf dem gleichen Server, der die Datenbank hostet. Die vollständigen und differenziellen Datenbanksicherungen werden auch in ein [gekoppeltes Rechenzentrum](../best-practices-availability-paired-regions.md) repliziert, um weiteren Schutz bei Rechenzentrumsausfällen zu bieten. Wenn Sie eine Datenbank wiederherstellen, ermittelt der Dienst, welche vollständigen und differenziellen Sicherungen bzw. Transaktionsprotokollsicherungen wiederhergestellt werden müssen.


Sie können diese Sicherungen für Folgendes verwenden:

* Stellen Sie eine Datenbank innerhalb der Aufbewahrungsdauer auf einen Zeitpunkt wieder her. Dieser Vorgang wird eine neue Datenbank auf dem gleichen Server wie die ursprüngliche Datenbank erstellen.
* Wiederherstellen einer gelöschten Datenbank auf den Zeitpunkt, zu dem sie gelöscht wurde, oder auf einen beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums. Die gelöschte Datenbank kann nur auf dem Server wiederhergestellt werden, auf dem die ursprüngliche Datenbank erstellt wurde.
* Wiederherstellen einer Datenbank in einer anderen geografischen Region. Dies ermöglicht die Wiederherstellung nach dem Ausfall einer geografischen Region, wenn Sie keinen Zugriff auf Ihren Server und Ihre Datenbank haben. Dabei wird eine neue Datenbank auf einem beliebigen Server an einem beliebigen Ort der Welt erstellt. 
* Wiederherstellen einer Datenbank auf der Grundlage einer spezifischen langfristigen Sicherung, wenn die Datenbank mit einer Richtlinie zur langfristigen Aufbewahrung (Long-Term Retention, LTR) konfiguriert wurde. Dadurch können Sie eine alte Version der Datenbank wiederherstellen, um eine Konformitätsanforderung zu erfüllen oder eine alte Version der Anwendung auszuführen. Weitere Informationen hierzu finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).
* Informationen zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md).

> [!NOTE]
> In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort zum anderen. Die *Datenbankreplikation* der SQL bezieht sich auf das Beibehalten von mehreren sekundären Datenbanken, die mit einer primären Datenbank synchronisiert werden. 
> 

## <a name="how-long-are-backups-kept"></a>Wie lange werden Sicherungen aufbewahrt?
Für jede SQL-Datenbank-Sicherung gilt eine Standardaufbewahrungsdauer, die auf dem Diensttarif der Datenbank basiert. Hierbei wird zwischen dem [DTU-basierten Kaufmodell](sql-database-service-tiers-dtu.md) und dem [V-Kern-basierten Kaufmodell](sql-database-service-tiers-vcore.md) unterschieden. Sie können die Aufbewahrungsdauer von Sicherungen für eine Datenbank aktualisieren. Ausführlichere Informationen finden Sie unter [Ändern des Zeitraums für die Aufbewahrung von Sicherungen](#how-to-change-backup-retention-period).

Wenn Sie eine Datenbank löschen, bewahrt SQL-Datenbank die Sicherungen auf die gleiche Weise wie für eine Onlinedatenbank auf. Beim Löschen einer Datenbank vom Typ „Basic“, für die eine Aufbewahrungsdauer von sieben Tagen gilt, wird eine vier Tage alte Sicherung weitere drei Tage lang aufbewahrt.

Falls Sie die Sicherungen länger als die maximale PITR-Aufbewahrungsdauer beibehalten müssen, können Sie die Sicherungseigenschaften so ändern, dass Ihrer Datenbank eine oder mehrere Zeiträume für die langfristige Aufbewahrung hinzugefügt werden. Ausführlichere Informationen finden Sie unter [Langfristiges Aufbewahren von Sicherungen](sql-database-long-term-retention.md).

> [!IMPORTANT]
> Wenn Sie die Azure SQL Server-Instanz löschen, auf der die SQL-Datenbanken gehostet werden, werden alle Pools für elastische Datenbanken und Datenbanken, die zum Server gehören, ebenfalls gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen. Wenn Sie aber die langfristige Aufbewahrung konfiguriert haben, werden die Sicherungen für die Datenbanken mit LTR nicht gelöscht, und diese Datenbanken können wiederhergestellt werden.

### <a name="pitr-retention-period"></a>PITR-Aufbewahrungszeitraum
Die Standardaufbewahrungsdauer für eine Datenbank, die mit dem DTU-basierten Kaufmodell erstellt wurde, hängt vom Diensttarif ab:

* Für den Tarif „Basic“ beträgt sie eine Woche.
* Für den Tarif „Standard“ beträgt sie fünf Wochen.
* Für den Tarif „Premium“ beträgt sie fünf Wochen.

Wenn Sie das [V-Kern-basierte Kaufmodell](sql-database-service-tiers-vcore.md) verwenden, ist die Aufbewahrung von Sicherungen bis zu 35 Tage konfigurierbar. 

Wenn Sie die aktuelle PITR-Aufbewahrungsdauer reduzieren, sind alle vorhandenen Sicherungen, die außerhalb der neuen Aufbewahrungsdauer liegen, nicht mehr verfügbar. 

Wenn Sie die aktuelle PITR-Aufbewahrungsdauer erhöhen, behält SQL-Datenbank die vorhandenen Sicherungen bei, bis die längere Aufbewahrungsdauer erreicht ist.

## <a name="how-often-do-backups-happen"></a>Wie oft erfolgen Sicherungen?
### <a name="backups-for-point-in-time-restore"></a>Sicherungen für die Point-in-Time-Wiederherstellung
SQL-Datenbank unterstützt den Self-Service für die Point-in-Time-Wiederherstellung (Point-in-Time Restore, PITR), indem automatisch vollständige Sicherungen, differenzielle Sicherungen und Transaktionsprotokollsicherungen erstellt werden. Vollständige Datensicherungen werden wöchentlich erstellt. Differenzielle Sicherungen erfolgen im Allgemeinen alle 12 Stunden und Transaktionsprotokollsicherungen alle 5-10 Minuten, wobei die Häufigkeit auf der Leistungsebene und dem Umfang der Datenbankaktivität basiert. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird normalerweise innerhalb von 30 Minuten abgeschlossen, aber der Vorgang kann auch länger dauern, wenn es sich um eine sehr große Datenbank handelt. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Die genaue Zeitplanung für alle Datenbanksicherungen wird vom SQL-Datenbank-Dienst je nach der gesamten Systemworkload bestimmt.

Die PITR-Sicherungen sind georedundant und per [regionsübergreifender Replikation für Azure Storage](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) geschützt.

Weitere Informationen finden Sie unter [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="backups-for-long-term-retention"></a>Sicherungen für die langfristige Aufbewahrung
SQL-Datenbank verfügt über eine Option zum Konfigurieren der langfristigen Aufbewahrung (LTR) von vollständigen Sicherungen für eine Dauer von bis zu zehn Jahren in Azure-Blobspeicher. Wenn die LTR-Richtlinie aktiviert ist, werden die wöchentlichen vollständigen Sicherungen automatisch in einen anderen RA-GRS-Speichercontainer kopiert. Zur Erfüllung unterschiedlicher Konformitätsanforderungen können Sie verschiedene Aufbewahrungsdauern für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Der Speicherverbrauch hängt von der ausgewählten Häufigkeit der Sicherungen und von den Aufbewahrungsdauern ab. Sie können den [LTR-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=sql-database) verwenden, um die Kosten für den LTR-Speicher zu schätzen. 

Wie bei PITR auch, sind die LTR-Sicherungen georedundant und per [regionsübergreifender Replikation für Azure Storage](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) geschützt.

Weitere Informationen finden Sie unter [Langfristiges Aufbewahren von Sicherungen](sql-database-long-term-retention.md).

## <a name="are-backups-encrypted"></a>Werden Sicherungen verschlüsselt?

Wenn Ihre Datenbank mit TDE verschlüsselt ist, werden die Sicherungen im Ruhezustand, einschließlich LTR-Sicherungen, automatisch verschlüsselt. Wenn Transparent Data Encryption (TDE) für eine Azure SQL-Datenbank aktiviert ist, werden die Sicherungen ebenfalls verschlüsselt. In allen neuen Azure SQL-Datenbanken ist TDE standardmäßig aktiviert. Weitere Informationen finden Sie unter [Transparent Data Encryption mit Azure SQL-Datenbank](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-do-automated-backups-impact-my-compliance"></a>Wie wirken sich automatisierte Sicherungen auf meine Konformität aus?

Wenn Sie Ihre Datenbank von einem DTU-basierten Diensttarif mit einer PITR-Standardaufbewahrung von 35 Tagen zu einem Diensttarif auf V-Kern-Basis migrieren, wird die PITR-Aufbewahrung beibehalten. So soll sichergestellt werden, dass die Datenwiederherstellungsrichtlinie Ihrer Anwendung nicht kompromittiert wird. Falls die Standardaufbewahrung Ihre Konformitätsanforderungen nicht erfüllt, können Sie die PITR-Aufbewahrungsdauer per PowerShell oder REST-API ändern. Ausführlichere Informationen finden Sie unter [Ändern des Zeitraums für die Aufbewahrung von Sicherungen](#how-to-change-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-backup-retention-period"></a>Ändern des Zeitraums für die Aufbewahrung von Sicherungen
Sie können die Standardaufbewahrungsdauer per REST-API oder PowerShell ändern. Die unterstützten Werte sind: 7, 14, 21, 28 oder 35 Tage. In den folgenden Beispielen wird veranschaulicht, wie Sie die PITR-Aufbewahrungsdauer in 28 Tage ändern. 

> [!NOTE]
> Diese APIs wirken sich nur auf die PITR-Aufbewahrungsdauer aus. Falls Sie für Ihre Datenbank LTR konfiguriert haben, ergeben sich keine Auswirkungen. Ausführliche Informationen zum Ändern von LTR-Aufbewahrungsdauern finden Sie unter [Langfristiges Aufbewahren von Sicherungen](sql-database-long-term-retention.md).

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Ändern der PITR-Aufbewahrungsdauer mit PowerShell
```powershell
Set-AzureRmSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```
> [!IMPORTANT]
> Diese API ist im PowerShell-Modul „AzureRM.Sql“ ab Version [4.7.0-preview](https://www.powershellgallery.com/packages/AzureRM.Sql/4.7.0-preview) enthalten. 

### <a name="change-pitr-retention-period-using-rest-api"></a>Ändern der PITR-Aufbewahrungsdauer mit der REST-API
**Beispiel für eine Anforderung**
```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```
**Anforderungstext**
```json
{
  "properties":{  
      "retentionDays":28
   }
}
```
**Beispiel für eine Antwort**

Statuscode: 200
```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```
Weitere Informationen finden Sie unter [Backup Short Term Retention Policies](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies) (Richtlinien für die kurzfristige Aufbewahrung von Sicherungen).

## <a name="next-steps"></a>Nächste Schritte

- Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von Azure SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt über das Azure-Portal finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-recovery-using-backups.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe von PowerShell finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](scripts/sql-database-restore-database-powershell.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen in Azure-Blobspeicher mit dem Azure-Portal finden Sie im Artikel über das [Verwalten der langfristigen Sicherungsaufbewahrung mit dem Azure-Portal](sql-database-long-term-backup-retention-configure.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen in Azure-Blobspeicher mit Azure PowerShell finden Sie im Artikel über das [Verwalten der langfristigen Sicherungsaufbewahrung mit PowerShell](sql-database-long-term-backup-retention-configure.md).
