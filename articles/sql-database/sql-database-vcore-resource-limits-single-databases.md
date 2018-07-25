---
title: Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells in Azure SQL-Datenbank – Einzeldatenbank | Microsoft-Dokumentation
description: Diese Seite beschreibt einige allgemeine Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: ac9070e328ef867a0b0e8d7d81f5147a50357928
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126407"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Limits des auf virtuellen Kernen basierenden Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank

Dieser Artikel enthält ausführliche Angaben zu Ressourcenlimits für Einzeldatenbanken in Azure SQL-Datenbank, die das auf virtuellen Kernen basierende Kaufmodell verwenden.

Informationen zu Einschränkungen bei DTU-basierten Einkaufsmodellen finden Sie unter [Einschränkungen des DTU-basierten Ressourcenmodells für Azure SQL-Datenbank](sql-database-dtu-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Einzeldatenbank: Speichergrößen und Leistungsstufen

Die folgende Tabelle enthält die verfügbaren Ressourcen für Einzeldatenbanken auf jeder Dienstebene und Leistungsstufe. Sie können mit [Azure-Portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-servers-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-servers-databases-manage.md#powershell-manage-logical-servers-and-databases), [Azure CLI](sql-database-servers-databases-manage.md#azure-cli-manage-logical-servers-and-databases) oder [REST-API](sql-database-servers-databases-manage.md#rest-api-manage-logical-servers-and-databases) Dienstebene, Leistungsstufe und Speichermenge für eine einzelne Datenbank festlegen.

### <a name="general-purpose-service-tier"></a>Universelle Dienstebene

#### <a name="generation-4-compute-platform"></a>Computeplattform der 4. Generation
|Leistungsstufe|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1536|3072|4096|4096|
|Maximale Protokollgröße|307|307|461|922|1229|1229|
|tempdb-Größe (GB)|32|64|128|256|384|384|
|Ziel-IOPS (64 KB)|500|1000|2000|4000|7.000|7.000|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|3200|4800|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|000
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

#### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation
|Leistungsstufe|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Maximale Protokollgröße|307|307|461|614|1229|1229|1229|1229|
|tempdb-Größe (GB)|64|128|256|384|384|384|384|384|
|Ziel-IOPS (64 KB)|500|1000|2000|4000|6000|7.000|7.000|7.000|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|2400|3200|4000|8.000|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|1|1|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

### <a name="business-critical-service-tier"></a>Diensttarif „Unternehmenskritisch“

#### <a name="generation-4-compute-platform"></a>Computeplattform der 4. Generation
|Leistungsstufe|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|1|2|4|8|20|36|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|
|Maximale Protokollgröße|307|307|307|307|307|307|
|tempdb-Größe (GB)|32|64|128|256|384|384|
|Ziel-IOPS (64 KB)|5.000|10000|20000|40.000|80.000|120000|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|3200|4800|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|3|3|3|3|3|3|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|JA|Ja|Ja|Ja|Ja|JA|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

#### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation
|Leistungsstufe|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|1,571|3,142|6,284|15,768|25,252|37,936|52,22|131,64|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maximale Protokollgröße|307|307|307|307|614|1229|1229|1229|
|tempdb-Größe (GB)|64|128|256|384|384|384|384|384|
|Ziel-IOPS (64 KB)|5.000|10000|20000|40.000|60000|80.000|100.000|200.000
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|2400|3200|4000|8.000|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|3|3|3|3|3|3|3|3|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

## <a name="next-steps"></a>Nächste Schritte

- Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md).
- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
