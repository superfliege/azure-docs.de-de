---
title: Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells in Azure SQL-Datenbank – Pools für elastische Datenbanken | Microsoft-Dokumentation
description: Diese Seite beschreibt einige allgemeine Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells für Pools für elastische Datenbanken in Azure SQL-Datenbank.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: 5503ffaf8a429221a0a0730fc999cb7a90f43785
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092119"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools"></a>Limits des V-Kern-basierten Kaufmodells für Pools für elastische Datenbanken in Azure SQL-Datenbank

Dieser Artikel enthält ausführliche Angaben zu Ressourcenlimits für Pools für elastische Datenbanken und in einem Pool zusammengefasste Datenbanken in Azure SQL-Datenbank mit dem auf virtuellen Kernen basierenden Kaufmodell.

Informationen zu Einschränkungen bei DTU-basierten Kaufmodellen finden Sie unter [Ressourcenlimits des DTU-basierten Kaufmodells in SQL-Datenbank – Pools für elastische Datenbanken](sql-database-dtu-resource-limits-elastic-pools.md).


## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Pool für elastische Datenbanken: Speichergrößen und Leistungsstufen

Die folgenden Tabellen enthalten die verfügbaren Ressourcen für Pools für elastische SQL-Datenbank-Instanzen auf jeder Dienstebene und Leistungsstufe. Sie können mit [Azure-Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases) oder [REST-API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases) Dienstebene, Leistungsstufe und Speichermenge festlegen.

> [!NOTE]
> Die Ressourcengrenzwerte einzelner Datenbanken in Pools für elastische Datenbanken entsprechen im Allgemeinen den Grenzwerten einzelner Datenbanken außerhalb von Pools, die sich auf der gleichen Leistungsstufe befinden. Auf eine GP_Gen4_1-Datenbank können z.B. max. 200 Worker gleichzeitig zugreifen. Entsprechend können auch maximal 200 Worker auf eine Datenbank in einem GP_Gen4_1-Pool zugreifen. Beachten Sie, dass die Gesamtanzahl gleichzeitiger Worker in einem GP_Gen4_1-Pool 210 beträgt.

### <a name="general-purpose-service-tier"></a>Universelle Dienstebene

#### <a name="generation-4-compute-platform"></a>Computeplattform der 4. Generation
|Leistungsstufe|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|Maximale Datengröße (GB)|512|756|1536|2048|3.584|4096|
|Maximale Protokollgröße|154|227|461|614|1075|1229|
|tempdb-Größe (Datenbank)|32|64|128|256|384|384|
|Ziel-IOPS (64 KB)|500|1000|2000|4000|7.000|7.000|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Max. gleichzeitige Worker (Anforderungen)|210|420|840|1680|3360|5.040|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Maximale Pooldichte|100|200|500|500|500|500|
|Minimale/maximale click-stop-Ereignisse in Pools für elastische Datenbanken|0, 0,25, 0,5, 1|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|
|Anzahl von Replikaten|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

#### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation
|Leistungsstufe|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|Maximale Datengröße (GB)|512|756|1536|2048|3072|4096|4096|4096|
|Maximale Protokollgröße|154|227|461|614|922|1229|1229|1229|
|tempdb-Größe (Datenbank)|64|128|256|384|384|384|384|384|
|Ziel-IOPS (64 KB)|500|1000|2000|4000|6000|7.000|7.000|7.000|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Max. gleichzeitige Worker (Anforderungen)|210|420|840|1680|2.520|3360|4.200|8.400
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|30000|
|Maximale Pooldichte|100|200|500|500|500|500|500|500|
|Minimale/maximale click-stop-Ereignisse in Pools für elastische Datenbanken|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
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
|tempdb-Größe (Datenbank)|32|64|128|256|384|384|
|Ziel-IOPS (64 KB)|5.000|10000|20000|40.000|80.000|120000|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Max. gleichzeitige Worker (Anforderungen)|210|420|840|1680|3360|5.040|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Maximale Pooldichte|N/V|50|100|100|100|100|
|Minimale/maximale click-stop-Ereignisse in Pools für elastische Datenbanken|N/V|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|
|Anzahl von Replikaten|3|3|3|3|3|3|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|JA|Ja|Ja|Ja|Ja|JA|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

#### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation
|Leistungsstufe|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|1,571|3,142|6,284|15,768|25,252|37,936|52,22|131,64|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maximale Protokollgröße|307|307|307|307|614|1229|1229|1229|
|tempdb-Größe (Datenbank)|64|128|256|384|384|384|384|384|
|Ziel-IOPS (64 KB)|5.000|10000|20000|40.000|60000|80.000|100.000|200.000
|Max. gleichzeitige Worker (Anforderungen)|210|420|840|1680|2.520|3360|5.040|8.400|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|30000|
|Maximale Pooldichte|N/V|50|100|100|100|100|100|100|
|Minimale/maximale click-stop-Ereignisse in Pools für elastische Datenbanken|N/V|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Anzahl von Replikaten|3|3|3|3|3|3|3|3|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

Wenn alle virtuellen Kerne eines Pools für elastische Datenbanken verwendet werden, erhält jede Datenbank im Pool gleich viel Computeressourcen zum Verarbeiten von Abfragen. Der SQL-Datenbank-Dienst bietet eine faire gemeinsame Nutzung von Ressourcen durch Datenbanken, indem gleiche Slices an Computezeit zugesichert werden. Diese faire gemeinsame Nutzung in Pools für elastische Datenbanken ergänzt die Ressourcen, die jeder Datenbank auf andere Weise garantiert werden, wenn die Mindestanzahl von virtuellen Kernen pro Datenbank auf einen Wert ungleich null festgelegt ist.

### <a name="database-properties-for-pooled-databases"></a>Eigenschaften von Datenbanken in einem Pool

Die folgende Tabelle beschreibt die Eigenschaften von Datenbanken in einem Pool.

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| Maximale Anzahl virtueller Kerne pro Datenbank |Die maximale Anzahl von virtuellen Kernen, die jede Datenbank im Pool verwenden kann, sofern basierend auf der Nutzung durch andere Datenbanken im Pool verfügbar. Die maximale Anzahl von virtuellen Kernen pro Datenbank ist keine Ressourcengarantie für eine Datenbank. Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Legen Sie die maximale Anzahl von virtuellen Kernen pro Datenbank hoch genug fest, sodass Lastspitzen bei der Datenbanknutzung verarbeitet werden können. Sie sollten ein gewisses Maß an Mehrlast einplanen, da für den Pool im Allgemeinen von Nutzungsmustern starker und schwacher Auslastung ausgegangen wird, bei der aber nicht alle Datenbanken gleichzeitig stark ausgelastet sind.|
| Minimale Anzahl virtueller Kerne pro Datenbank |Die minimale Anzahl von virtuellen Kernen, die für jede Datenbank im Pool garantiert werden. Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Die Mindestanzahl von virtuellen Kernen pro Datenbank kann auf 0 festgelegt werden. Dies ist auch der Standardwert. Diese Eigenschaft ist auf einen Wert zwischen 0 und der durchschnittlichen Nutzung der virtuellen Kerne pro Datenbank festgelegt. Das Produkt aus der Anzahl von Datenbanken im Pool und der Mindestzahl von virtuellen Kernen pro Datenbank darf die tatsächliche Anzahl der virtuellen Kerne pro Pool nicht übersteigen.|
| Max. Speicherkapazität pro Datenbank |Die maximale Datenbankgröße, die vom Benutzer für eine Datenbank in einem Pool festgelegt wird. In einem Pool zusammengefasste Datenbanken nutzen den zugeordneten Poolspeicher gemeinsam, daher ist die Größe, die eine Datenbank erreichen kann, auf den jeweils kleineren Wert des verbleibenden Poolspeichers oder der Datenbankgröße beschränkt. Die maximale Datenbankgröße bezieht sich auf die maximale Größe der Datendateien und umfasst nicht den von Protokolldateien belegten Speicherplatz. |
|||
 
## <a name="next-steps"></a>Nächste Schritte

- Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md).
- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
