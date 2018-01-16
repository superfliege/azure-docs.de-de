---
title: "Beschränkungen in Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Beschreibung der Einschränkungen der Vorschau in Azure-Datenbank für MySQL"
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/11/2018
ms.openlocfilehash: f0f9a10f987f19d8ae77a07038cffe23446856fd
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Beschränkungen in Azure Database for MySQL
Der Azure-Datenbank für MySQL-Dienst ist in der öffentlichen Vorschau verfügbar. In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen sowie Beschränkungen bei der Unterstützung der Speicher-Engine und von Datenmanipulationsanweisungen im Datenbankdienst beschrieben. Sehen Sie sich auch die [allgemeinen Einschränkungen](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) an, die für die MySQL-Datenbank-Engine gelten.

## <a name="service-tier-maximums"></a>Maximalwerte der Diensttarife
Azure Database for MySQL weist mehrere Diensttarife auf, aus denen Sie bei der Erstellung eines Servers wählen können. Weitere Informationen finden Sie unter [Überblick über die verfügbaren Funktionen in jedem Diensttarif](concepts-service-tiers.md).  

In jedem Diensttarif ist eine maximale Anzahl von Verbindungen, Computeeinheiten und Speicherkapazitäten in der Vorschauphase verfügbar. Es gelten folgende Maximalwerte: 

|                            |                   |
| :------------------------- | :---------------- |
| **Max. Anzahl von Verbindungen**        |                   |
| 50 Compute-Einheiten (Basic)     | 50 Verbindungen    |
| 100 Compute-Einheiten (Basic)    | 100 Verbindungen   |
| 100 Compute-Einheiten (Standard) | 200 Verbindungen   |
| 200 Compute-Einheiten (Standard) | 400 Verbindungen   |
| 400 Compute-Einheiten (Standard) | 800 Verbindungen   |
| 800 Compute-Einheiten (Standard) | 1.600 Verbindungen  |
| **Max. Anzahl von Compute-Einheiten**      |                   |
| Basic-Dienstebene         | 100 Compute-Einheiten |
| Standard-Dienstebene      | 800 Compute-Einheiten |
| **Max. Speicherkapazität**            |                   |
| Basic-Dienstebene         | 1 TB              |
| Standard-Dienstebene      | 1 TB              |

Wenn die max. Anzahl von Verbindungen erreicht wird, wird möglicherweise folgende Fehlermeldung angezeigt:
> FEHLER 1040 (08004): Zu viele Verbindungen

## <a name="storage-engine-support"></a>Speicher-Engine-Unterstützung

### <a name="supported"></a>Unterstützt
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Nicht unterstützt
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIVE](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATED](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Berechtigungsunterstützung

### <a name="unsupported"></a>Nicht unterstützt
- DBA-Rolle Viele Serverparameter und -einstellungen können versehentlich die Serverleistung beeinträchtigen oder ACID-Eigenschaften des DBMS verschlechtern. Daher stellen wir die DBA-Rolle Kunden nicht zur Verfügung, um unsere Dienstintegrität und die SLA auf Produktebene aufrechtzuerhalten. Das Standardbenutzerkonto, das zusammen mit einer neuen Datenbankinstanz erstellt wird, gestattet Kunden die Ausführung der meisten DDL- und DML-Anweisungen in der verwalteten Datenbankinstanz. 
- SUPER-Berechtigung Auf ähnliche Weise ist die [SUPER-Berechtigung](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) ebenfalls eingeschränkt.

## <a name="data-manipulation-statement-support"></a>Unterstützung von Datenmanipulationsanweisungen

### <a name="supported"></a>Unterstützt
- LOAD DATA INFILE – Unterstützt, es muss aber der Parameter [LOCAL] angegeben werden, der auf einen UNC-Pfad (über XSMB bereitgestellter Azure Storage) weitergeleitet wird.

### <a name="unsupported"></a>Nicht unterstützt
- SELECT ... INTO OUTFILE

## <a name="preview-functional-limitations"></a>Funktionale Beschränkungen der Vorschau

### <a name="scale-operations"></a>Skalierungsvorgänge
- Die dynamische Skalierung von Servern über verschiedene Diensttarife hinweg, d.h. ein Wechsel zwischen den Diensttarifen „Basic“ und „Standard“, wird derzeit nicht unterstützt.
- Die dynamische bedarfsgesteuerte Steigerung des Speichers auf vorab erstellten Servern wird derzeit nicht unterstützt.
- Die Verringerung der Größe des Serverspeichers wird nicht unterstützt.

### <a name="server-version-upgrades"></a>Upgrades von Serverversionen
- Die automatisierte Migration zwischen Hauptversionen von Datenbank-Engines wird derzeit nicht unterstützt.

### <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung
- Die Wiederherstellung in anderen Diensttarifen und/oder Compute-Einheiten und Speichergrößen ist nicht zulässig.
- Die Wiederherstellung eines gelöschten Servers wird nicht unterstützt.

## <a name="functional-limitations"></a>Funktionale Beschränkungen

### <a name="subscription-management"></a>Abonnementverwaltung
- Die dynamische Verschiebung von vorab erstellten Servern zwischen Abonnement- und Ressourcengruppen wird derzeit nicht unterstützt.

## <a name="current-known-issues"></a>Aktuelle bekannte Probleme
- Die MySQL-Serverinstanz zeigt die falsche Serverversion an, nachdem die Verbindung hergestellt wurde. Um die richtige Serverinstanzversion abzurufen, verwenden Sie den Befehl „select version();“ an einer MySQL-Eingabeaufforderung.

## <a name="next-steps"></a>Nächste Schritte
- [Verfügbaren Funktionen auf den einzelnen Dienstebenen](concepts-service-tiers.md)
- [Unterstützte MySQL-Datenbankversionen](concepts-supported-versions.md)
