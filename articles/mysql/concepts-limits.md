---
title: "Beschränkungen in Azure Database for MySQL"
description: "Dieser Artikel beschreibt die Einschränkungen in Azure Database for MySQL, z.B. die Anzahl der Verbindungs- und Speichermoduloptionen."
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 85e57170c1cbd977d2de6e7e614916333c79e047
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Beschränkungen in Azure Database for MySQL
Der Azure-Datenbank für MySQL-Dienst ist in der öffentlichen Vorschau verfügbar. In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen sowie Beschränkungen bei der Unterstützung der Speicher-Engine und von Datenmanipulationsanweisungen im Datenbankdienst beschrieben. Sehen Sie sich auch die [allgemeinen Einschränkungen](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) an, die für die MySQL-Datenbank-Engine gelten.

## <a name="service-tier-maximums"></a>Maximalwerte der Diensttarife
Azure Database for MySQL weist mehrere Diensttarife auf, aus denen Sie bei der Erstellung eines Servers wählen können. Weitere Informationen finden Sie unter [Optionen und Leistung von Azure Database for MySQL: Übersicht über die verfügbaren Funktionen in den einzelnen Tarifen](concepts-pricing-tiers.md).  

In jedem Diensttarif ist eine maximale Anzahl von Verbindungen, Computeeinheiten und Speicherkapazitäten in der Vorschauphase verfügbar. Es gelten folgende Maximalwerte: 

|**Tarif**| **Computegeneration**|**Virtuelle Kerne**| **Max. Anzahl von Verbindungen**|
|---|---|---|---|
|Basic| Gen 4| 1| 50|
|Basic| Gen 4| 2| 100|
|Basic| Gen 5| 1| 50|
|Basic| Gen 5| 2| 100|
|Universell| Gen 4| 2| 200|
|Universell| Gen 4| 4| 400|
|Universell| Gen 4| 8| 800|
|Universell| Gen 4| 16| 1600|
|Universell| Gen 4| 32| 3200|
|Universell| Gen 5| 2| 200|
|Universell| Gen 5| 4| 400|
|Universell| Gen 5| 8| 800|
|Universell| Gen 5| 16| 1600|
|Universell| Gen 5| 32| 3200|
|Arbeitsspeicheroptimiert| Gen 5| 2| 600|
|Arbeitsspeicheroptimiert| Gen 5| 4| 1250|
|Arbeitsspeicheroptimiert| Gen 5| 8| 2500|
|Arbeitsspeicheroptimiert| Gen 5| 16| 5.000|
|Arbeitsspeicheroptimiert| Gen 5| 32| 10000| 

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
- DBA-Rolle: Viele Serverparameter und -einstellungen können unbeabsichtigterweise die Serverleistung beeinträchtigen oder ACID-Eigenschaften des DBMS verschlechtern. Um die Dienstintegrität und die SLA auf Produktebene aufrechtzuerhalten, stellt dieser Dienst die DBA-Rolle nicht zur Verfügung. Das Standardbenutzerkonto, das zusammen mit einer neuen Datenbankinstanz erstellt wird, gestattet dem Benutzer die Ausführung der meisten DDL- und DML-Anweisungen in der verwalteten Datenbankinstanz. 
- SUPER-Berechtigung: Auf ähnliche Weise ist die [SUPER-Berechtigung](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) ebenfalls eingeschränkt.

## <a name="data-manipulation-statement-support"></a>Unterstützung von Datenmanipulationsanweisungen

### <a name="supported"></a>Unterstützt
- LOAD DATA INFILE – Unterstützt, es muss aber der Parameter [LOCAL] angegeben werden, der auf einen UNC-Pfad (über XSMB bereitgestellter Azure Storage) weitergeleitet wird.

### <a name="unsupported"></a>Nicht unterstützt
- SELECT ... INTO OUTFILE

## <a name="preview-functional-limitations"></a>Funktionale Beschränkungen der Vorschau

### <a name="scale-operations"></a>Skalierungsvorgänge
- Die dynamische Skalierung von Servern über verschiedene Tarife hinweg wird zurzeit nicht unterstützt. Wechsel zwischen den Tarifen „Basic“, „Universell“ und „Arbeitsspeicheroptimiert“ werden also nicht unterstützt.
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
- [Verfügbaren Funktionen auf den einzelnen Dienstebenen](concepts-pricing-tiers.md)
- [Unterstützte MySQL-Datenbankversionen](concepts-supported-versions.md)
