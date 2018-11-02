---
title: Speichern von Sicherungen von Azure SQL-Datenbank bis zu 10 Jahre lang| Microsoft-Dokumentation
description: Hier erfahren Sie, wie Azure SQL-Datenbank das Speichern vollständiger Datenbanksicherungen für bis zu zehn Jahre unterstützt.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 10/24/2018
ms.openlocfilehash: 7fe34423e706054daf84eaa8baf45fe201a661c9
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026176"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Speichern von Sicherungen von Azure SQL-Datenbank bis zu 10 Jahre lang

Viele Anwendungen dienen gesetzlichen, ordnungsgemäßen oder anderen geschäftlichen Zwecken, die voraussetzen, dass Datenbanksicherungen länger als der Zeitraum von 7–35 Tagen, der für [automatischen Sicherungen](sql-database-automated-backups.md) von Azure SQL-Datenbank zur Verfügung gestellt wird, aufbewahrt werden. Mithilfe des Features für die langfristige Aufbewahrung (Long-Term Retention, LTR) können Sie angegebene vollständige Sicherungen von SQL-Datenbank in [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)-Blobspeicher bis zu zehn Jahre lang speichern. Sie können dann jede Sicherung als neue Datenbank wiederherstellen.

> [!NOTE]
> LTR kann für die Datenbanken aktiviert werden, die auf logischen Azure SQL-Datenbankservern gehostet werden. Es ist noch nicht für Datenbanken, die in verwalteten Instanzen gehostet werden, verfügbar.
> 

## <a name="how-sql-database-long-term-retention-works"></a>Funktionsweise der langfristigen Aufbewahrung von SQL-Datenbank

Die langfristige Sicherungsaufbewahrung (LTR) nutzt die [automatischen](sql-database-automated-backups.md) vollständigen Datenbanksicherungen für die Point-in-Time-Wiederherstellung (Point In Time Restore, PITR). Diese Sicherungen werden in verschiedene Storage-Blobs kopiert, wenn eine LTR-Richtlinie konfiguriert wurde.
Sie können eine LTR-Richtlinie für jede SQL-Datenbank konfigurieren und angeben, wie häufig die Sicherungen in die langfristigen Storage-Blobs kopiert werden sollen. Um von dieser Flexibilität zu profitieren, können Sie in der Richtlinie eine Kombination aus vier Parametern verwenden: wöchentliche Sicherungsaufbewahrung (W), monatliche Sicherungsaufbewahrung (M), jährliche Sicherungsaufbewahrung (Y) und Woche des Jahres (WeekOfYear). Wenn Sie den Parameter „W“ angeben, wird eine wöchentliche Sicherung in den langfristigen Speicher kopiert. Wenn Sie den Parameter „M“ angeben, wird eine in der ersten Woche jedes Monats erstellte Sicherung in den langfristigen Speicher kopiert. Wenn Sie den Parameter „Y“ angeben, wird eine Sicherung, die in der durch „WeekOfYear“ angegebenen Woche erstellt wird, in den langfristigen Speicher kopiert. Jede Sicherung wird für den durch diese Parameter angegebenen Zeitraum im langfristigen Speicher aufbewahrt. 

Beispiele:

-  W=0, M=0, Y=5, WeekOfYear=3

   Die dritte vollständige Sicherung jedes Jahres wird fünf Jahre lang aufbewahrt.
- W=0, M=3, Y=0

   Die erste vollständige Sicherung jedes Monats wird drei Monate lang aufbewahrt.

- W=12, M=0, Y=0

   Jede wöchentliche vollständige Sicherung wird zwölf Wochen lang aufbewahrt.

- W=6, M=12, Y=10, WeekOfYear=16

   Jede wöchentliche vollständige Sicherung wird sechs Wochen lang aufbewahrt. Eine Ausnahme stellt die erste vollständige Sicherung jeden Monats dar: Sie wird zwölf Monate lang aufbewahrt. Eine weitere Ausnahme stellt die vollständige Sicherung dar, die in der 16. Woche des Jahres erstellt wurde: Sie wird zehn Jahre lang aufbewahrt. 

Die folgende Tabelle veranschaulicht den Rhythmus und den Ablauf der langfristigen Sicherungen für die folgenden Richtlinie:

W=12 (12 Wochen bzw. 84 Tage), M=12 (12 Monate bzw. 365 Tage), Y=10 (10 Jahre bzw. 3.650 Tage), WeekOfYear=15 (Woche nach dem 15. April)

   ![LTR-Beispiel](./media/sql-database-long-term-retention/ltr-example.png)


 
Wenn Sie die obige Richtlinie ändern und „W=0“ (keine wöchentlichen Sicherungen) festlegen, ändert sich der Rhythmus der Sicherungskopien wie in der obigen Tabelle durch die hervorgehobenen Daten gezeigt. Der zum Aufbewahren dieser Sicherungen benötigte Speicherplatz verringert sich entsprechend. 

> [!NOTE]
1. Die LTR-Kopien werden vom Azure Storage-Dienst erstellt, daher beeinträchtigt der Kopiervorgang nicht die Leistung der vorhandenen Datenbank.
2. Die Richtlinie gilt für zukünftige Sicherungen. Beispiel: Wenn die angegebene WeekOfYear beim Konfigurieren der Richtlinie in der Vergangenheit liegt, wird die erste LTR-Sicherung im nächsten Jahr erstellt. 
3. Zum Wiederherstellen einer Datenbank aus dem LTR-Speicher können Sie eine bestimmte Sicherung basierend auf ihrem Zeitstempel auswählen.   Die Datenbank kann auf einem beliebigen vorhandenen Server unter dem gleichen Abonnement wie die ursprüngliche Datenbank wiederhergestellt werden. 
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Georeplikation und langfristige Sicherungsaufbewahrung

Wenn Sie aktive Georeplikation und automatische Failovergruppen als Geschäftskontinuitätslösung verwenden, sollten Sie sich auf mögliche Failover vorbereiten und dieselbe LTR-Richtlinie auf der sekundären Geodatenbank konfigurieren. Dadurch erhöhen sich Ihre LTR-Speicherkosten nicht, da von sekundären Datenbanken keine Sicherungen erstellt werden. Nur, wenn die sekundäre Datenbank zur primäre Datenbank wird, werden die Sicherungen erstellt. Auf diese Weise gewährleisten Sie eine unterbrechungsfreie Erstellung der LTR-Sicherungen, wenn das Failover ausgelöst wird und die primäre Datenbank in den sekundären Bereich verschoben wird. 

> [!NOTE]
Wenn sich die ursprüngliche primäre Datenbank nach dem Ausfall der zu einem Failover geführt hat, wiederhergestellt wird, wird sie zu einer neuen sekundären Datenbank. Daher wird die Sicherungserstellung nicht fortgesetzt und die bestehende LTR-Richtlinie wird erst wieder wirksam, wenn sie wieder die primäre Datenbank ist. 
> 

## <a name="configure-long-term-backup-retention"></a>Konfigurieren der langfristigen Sicherungsaufbewahrung

Informationen zum Konfigurieren der langfristigen Aufbewahrung mit dem Azure-Portal oder mit PowerShell finden Sie unter [Konfiguration einer langfristig aufbewahrten Sicherung einer Azure SQL-Datenbank und die Wiederherstellung daraus](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Nächste Schritte

Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
