---
title: Auf virtuellen Kernen basierende Ressourceneinschränkungen für Azure SQL-Datenbank | Microsoft-Dokumentation
description: Diese Seite beschreibt einige allgemeine auf virtuellen Kernen basierende Ressourceneinschränkungen für Azure SQL-Datenbank.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 05/31/2018
ms.author: carlrab
ms.openlocfilehash: 324f9f35de37c717d57e46413cd881dce785f4fd
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737662"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-preview"></a>Einschränkungen des auf virtuellen Kernen basierenden Einkaufsmodells für Azure SQL-Datenbank (Vorschau)

> [!IMPORTANT]
> Informationen zu Einschränkungen bei DTU-basierten Einkaufsmodellen finden Sie unter [Einschränkungen des DTU-basierten Ressourcenmodells für Azure SQL-Datenbank](sql-database-dtu-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Einzeldatenbank: Speichergrößen und Leistungsstufen

Die folgende Tabelle enthält die verfügbaren Ressourcen für Einzeldatenbanken auf jeder Dienstebene und Leistungsstufe. Sie können mit [Azure-Portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), [Azure CLI](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli) oder [REST-API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api) Dienstebene, Leistungsstufe und Speichermenge für eine einzelne Datenbank festlegen.

### <a name="general-purpose-service-tier"></a>Universelle Dienstebene

#### <a name="generation-4-compute-platform"></a>Computeplattform der 4. Generation
|Leistungsstufe|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1536|3072|4096|4096|
|Maximale Protokollgröße|307|307|461|922|1229|1229|
|tempdb-Größe (Datenbank)|32|64|128|256|384|384|
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
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Maximale Protokollgröße|307|307|461|614|1229|1229|1229|1229|
|tempdb-Größe (Datenbank)|64|128|256|384|384|384|384|384|
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
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|1|2|4|8|20|36|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|
|Maximale Protokollgröße|307|307|307|307|307|307|
|tempdb-Größe (Datenbank)|32|64|128|256|384|384|
|Ziel-IOPS (64 KB)|5.000|10000|20000|40.000|80.000|120000|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|3200|4800|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|3|3|3|3|3|3|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

#### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation
|Leistungsstufe|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
|In-Memory-OLTP-Speicher (GB)|1,571|3,142|6,284|15,768|25,252|37,936|52,22|131,64|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maximale Protokollgröße|307|307|307|307|614|1229|1229|1229|
|tempdb-Größe (Datenbank)|64|128|256|384|384|384|384|384|
|Ziel-IOPS (64 KB)|5.000|10000|20000|40.000|60000|80.000|100.000|200.000
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|2400|3200|4000|8.000|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|3|3|3|3|3|3|3|3|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

## <a name="single-database-change-storage-size"></a>Einzeldatenbank: Ändern der Speichergröße

- Speicher kann in Schritten von 1 GB bis zur maximalen Speichergröße bereitgestellt werden. Die konfigurierbare Mindestdatenspeichergröße beträgt 5 GB 
- Speicher für eine Einzeldatenbank kann durch Erhöhen oder Verringern der maximalen Größe im [Azure-Portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), in [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), in [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), in der[Azure CLI](/cli/azure/sql/db#az_sql_db_update) oder in der [REST-API](/rest/api/sql/databases/update) bereitgestellt werden.
- SQL-Datenbank belegt automatisch 30 % des zusätzlichen Speicherplatzes für die Protokolldateien und 32 GB pro virtuellem Kern für die tempdb-Datenbank, wobei 384 GB nicht überschritten werden. Die tempdb befindet sich in allen Dienstebenen auf einem angefügten SSD-Datenträger.
- Der Preis für den Speicher für eine einzelne Datenbank errechnet sich aus der Summe des Datenspeichers und des Protokollspeichers multipliziert mit dem Speichereinheitenpreis für die Dienstebene. Die Kosten für die tempdb-Datenbank sind im Preis für den virtuellen Kern enthalten. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-vcores"></a>Einzeldatenbank: Ändern von virtuellen Kernen

Nach der anfänglichen Auswahl der Anzahl an virtuellen Kernen können Sie eine Einzeldatenbank je nach tatsächlichem Bedarf im [Azure-Portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), in [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), in [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), in der [Azure CLI](/cli/azure/sql/db#az_sql_db_update) oder in der [REST-API](/rest/api/sql/databases/update) dynamisch zentral hoch- oder herunterskalieren. 

Wenn Sie die Dienst- und/oder die Leistungsstufe einer Datenbank ändern, wird ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsebene erstellt und anschließend die Verbindung auf dieses Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Die Dauer der Umstellung kann variieren, aber sie liegt im Allgemeinen unter vier Sekunden und in 99 % der Fälle unter 30 Sekunden. Falls im Moment der Verbindungstrennung viele Transaktionen stattfinden, kann die Umstellung unter Umständen auch länger dauern. 

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte die zentrale Hochskalierung für eine 250-GB-Datenbank beim Wechsel in einen, aus einem oder innerhalb eines allgemeinen Diensttarifs innerhalb von sechs Stunden abgeschlossen sein. Für eine Datenbank der gleichen Größe, deren Leistungsstufe innerhalb des unternehmenskritischen Diensttarifs geändert wird, sollte das zentrale Hochskalieren innerhalb von drei Stunden beendet sein.

> [!TIP]
> Weitere Informationen zum Überwachen laufender Vorgänge finden Sie unter: [Verwalten von Vorgängen mithilfe der SQL-REST-API](/rest/api/sql/Operations/List), [Verwalten von Vorgängen mithilfe der Befehlszeilenschnittstelle](/cli/azure/sql/db/op) und [Überwachen von Vorgängen mit T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). Verwenden Sie auch diese beiden PowerShell-Befehle: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) und [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Wenn Sie ein Upgrade auf eine höhere Dienstebene oder Leistungsstufe durchführen, wird die maximale Datenbankgröße nicht erhöht, sofern Sie nicht ausdrücklich eine höhere Maximalgröße angeben.
* Für ein Downgrade einer Datenbank muss die verwendete Datenbankmenge kleiner als die maximal zulässige Größe der Zieldienstebene und Leistungsstufe sein. 
* Beim Upgrade einer Datenbank mit aktivierter [Georeplikation](sql-database-geo-replication-portal.md) führen Sie vor dem Upgrade der primären Datenbank zunächst ein Upgrade der zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe durch (allgemeine Richtlinie für optimale Leistung). Bei einem Upgrade auf eine andere Edition muss zunächst ein Upgrade der sekundären Datenbank durchgeführt werden.
* Beim Downgrade einer Datenbank mit aktivierter [Georeplikation](sql-database-geo-replication-portal.md) führen Sie vor dem Downgrade der sekundären Datenbank zunächst ein Downgrade der zugehörigen primären Datenbanken auf die gewünschte Leistungsstufe durch (allgemeine Richtlinie für optimale Leistung). Bei einem Downgrade auf eine andere Edition muss zunächst ein Downgrade der primären Datenbank durchgeführt werden.
* Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Pool für elastische Datenbanken: Speichergrößen und Leistungsstufen

Die folgenden Tabellen enthalten die verfügbaren Ressourcen für Pools für elastische SQL-Datenbank-Instanzen auf jeder Dienstebene und Leistungsstufe. Sie können mit [Azure-Portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-powershell), [Azure CLI](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-cli) oder [REST-API](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-rest-api) Dienstebene, Leistungsstufe und Speichermenge festlegen.

> [!NOTE]
> Die Ressourcengrenzwerte einzelner Datenbanken in Pools für elastische Datenbanken entsprechen im Allgemeinen den Grenzwerten einzelner Datenbanken außerhalb von Pools, die sich auf der gleichen Leistungsstufe befinden. Auf eine GP_Gen4_1-Datenbank können z.B. max. 200 Worker gleichzeitig zugreifen. Entsprechend können auch maximal 200 Worker auf eine Datenbank in einem GP_Gen4_1-Pool zugreifen. Beachten Sie, dass die Gesamtanzahl gleichzeitiger Worker in einem GP_Gen4_1-Pool 210 beträgt.

### <a name="general-purpose-service-tier"></a>Universelle Dienstebene

#### <a name="generation-4-compute-platform"></a>Computeplattform der 4. Generation
|Leistungsstufe|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
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
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
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
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|
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
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

#### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation
|Leistungsstufe|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
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
|Horizontale Leseskalierung|Ja|Ja|Ja|Ja|Ja|Ja|Ja|Ja|
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
 
## <a name="elastic-pool-change-storage-size"></a>Pool für elastische Datenbanken: Ändern der Speichergröße

- Speicher kann bis zur maximalen Speichergröße bereitgestellt werden: 
 - Für Speicher vom Typ „Standard“ erhöhen bzw. verringern Sie die Größe in Schritten von 10 GB.
 - Für Speicher vom Typ „Premium“ erhöhen bzw. verringern Sie die Größe in Schritten von 250 GB.
- Speicher für einen Pool für elastische Datenbanken kann durch Erhöhen oder Verringern der maximalen Größe im [Azure-Portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), in [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), in der [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) oder in der [REST-API](/rest/api/sql/elasticpools/update) bereitgestellt werden.
- Der Preis für den Speicher für einen Pool für elastische Datenbanken errechnet sich aus der Menge an bereitgestelltem Speicher multipliziert mit dem Speichereinheitenpreis für den Diensttarif. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="elastic-pool-change-vcores"></a>Pool für elastische Datenbanken: Ändern von virtuellen Kernen

Sie können die Leistungsstufe für einen Pool für elastische Datenbanken basierend auf den Ressourcenanforderungen im [Azure-Portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), in [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), in der [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) oder in der [REST-API](/rest/api/sql/elasticpools/update) erhöhen oder verringern.

- Beim erneuten Skalieren von virtuellen Kernen in einem Pool werden Datenbankverbindungen vorübergehend getrennt. Dies ist ebenfalls beim Neuskalieren von DTUs für eine einzelne (nicht zu einem Pool gehörende) Datenbank der Fall. Ausführliche Informationen zu Dauer und Auswirkungen der Trennung von Verbindungen für eine Datenbank während Neuskalierungsvorgängen finden Sie unter [Einzeldatenbank: Ändern von DTUs](#single-database-change-storage-size). 
- Die Dauer der Neuskalierung von virtuellen Kernen in einem Pool kann von der Gesamtmenge des Speicherplatzes abhängen, die von allen Datenbanken im Pool verwendet wird. Im Allgemeinen beträgt die Neuskalierungs-Latenzzeit durchschnittlich höchstens 90 Minuten pro 100 GB. Wenn beispielsweise der gesamte, von allen Datenbanken im Pool verwendete Speicherplatz 200 GB beträgt, beträgt die erwartete Neuskalierungs-Latenzzeit höchstens drei Stunden. In einigen Fällen kann die Neuskalierungs-Latenzzeit in der Ebene „Standard“ oder „Basic“ unabhängig von der Menge des verwendeten Speicherplatzes weniger als fünf Minuten betragen.
- Änderungen der minimalen oder maximalen Anzahl von virtuellen Kernen pro Datenbank dauern in der Regel höchstens fünf Minuten.
- Beim Herabstufen von virtuellen Kernen in Pools muss die verwendete Speichermenge im Pool kleiner sein als die maximal zulässige Größe der Zieldienstebene und der virtuellen Kerne im Pool.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Was ist die maximale Anzahl von Servern und Datenbanken?

| Maximum | Wert |
| :--- | :--- |
| Datenbanken pro Server | 5.000 |
| Anzahl von Servern pro Abonnement und Region | 20 |
|||

> [!IMPORTANT]
> Wenn sich die Anzahl der Datenbanken dem Grenzwert pro Server nähert, kann Folgendes geschehen:
> <br> • Höhere Latenz bei der Ausführung von Abfragen bei der Masterdatenbank.  Dies bezieht sich auch auf die Ansichten der Ressourcennutzungsstatistiken (z. B. sys.resource_stats) ein.
> <br> • Höhere Latenz bei Verwaltungsvorgängen und dem Rendern von Portalblickpunkten. Dazu gehört auch das Aufzählen von Datenbanken auf dem Server gehört.

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>Was geschieht, wenn die Ressourcengrenzen von Datenbank und Pool für elastische Datenbanken erreicht werden?

### <a name="compute-vcores"></a>Computing (virtuelle Kerne)

Bei zunehmender Computenutzung der Datenbank (gemessen an der Nutzung der virtuellen Kerne) steigt die Abfragelatenz, die auch durch einen Timeout beendet werden kann. Unter diesen Bedingungen können Abfragen durch den Dienst in eine Warteschlange gestellt werden, und ihnen werden Ressourcen für die Ausführung bereitgestellt, sobald diese frei werden.
Wenn eine hohe Computenutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:

- Erhöhen der Leistungsstufe der Datenbank oder des Pools für elastische Datenbanken, um mehr virtuelle Kerne für die Datenbank bereitzustellen. Informationen finden Sie unter [Einzeldatenbank: Ändern von virtuellen Kernen](#single-database-change-vcores) und [Pool für elastische Datenbanken: Ändern von virtuellen Kernen](#elastic-pool-change-vcores).
- Optimieren der Abfragen, um die Ressourcenverwendung für jede Abfrage zu reduzieren. Weitere Informationen finden Sie unter [Abfrageoptimierung/Abfragehinweise](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Speicher

Wenn der verwendete Datenbankspeicherplatz die maximale Größe erreicht, treten bei Einfügungen in die Datenbank und Updates, die die Datenmenge erhöhen, Fehler auf, sodass Clients eine [Fehlermeldung](sql-database-develop-error-messages.md) erhalten. Die Datenbankanweisungen SELECT und DELETE werden weiterhin erfolgreich ausgeführt.

Wenn eine hohe Speicherplatznutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:

- Erhöhen der maximalen Größe der Datenbank bzw. des Pools für elastische Datenbanken oder Ändern der Leistungsstufe, um den maximalen Speicher zu erhöhen. Informationen dazu finden Sie unter [Auf virtuellen Kernen basierende Ressourceneinschränkungen für Azure SQL-Datenbank](sql-database-vcore-resource-limits.md).
- Wenn sich die Datenbank in einem Pool für elastische Datenbanken befindet, kann sie auch aus dem Pool heraus verschoben werden, damit ihr Speicherplatz nicht mit anderen Datenbanken gemeinsam genutzt wird.

### <a name="sessions-and-workers-requests"></a>Sitzungen und Worker (Anforderungen) 

Die maximale Anzahl von Sitzungen und Workern wird durch Dienstebene und Leistungsstufe bestimmt. Neue Anforderungen werden abgelehnt, wenn Sitzung oder Worker Grenzwerte erreicht haben, und Clients erhalten eine Fehlermeldung. Während die Anzahl der verfügbaren Verbindungen von der Anwendung gesteuert werden kann, ist die Anzahl paralleler Worker oft schwieriger zu schätzen und zu steuern. Dies gilt insbesondere während der Spitzenlastzeiten, wenn Datenbankressourcen-Grenzen erreicht werden und Worker aufgrund länger laufender Abfragen Schlange stehen. 

Wenn eine hohe Sitzungs- oder Workernutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:
- Erhöhen der Dienstebene oder Leistungsstufe der Datenbank oder des Pools für elastische Datenbanken. Informationen finden Sie unter [Einzeldatenbank: Ändern der Speichergröße](#single-database-change-storage-size), [Einzeldatenbank: Ändern von virtuellen Kernen](#single-database-change-vcores), [Pool für elastische Datenbanken: Ändern der Speichergröße](#elastic-pool-change-storage-size) und [Pool für elastische Datenbanken: Ändern von virtuellen Kernen](#elastic-pool-change-vcores).
- Optimieren von Abfragen, um die Ressourcenverwendung durch die einzelnen Abfragen zu verringern, wenn die Ursache der zunehmenden Auslastung durch Worker Konflikte bezüglich der Computeressourcen sind. Weitere Informationen finden Sie unter [Abfrageoptimierung/Abfragehinweise](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Nächste Schritte

- Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md).
- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
