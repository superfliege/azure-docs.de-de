---
title: Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells in Azure SQL-Datenbank – Einzeldatenbank | Microsoft-Dokumentation
description: Diese Seite beschreibt einige allgemeine Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/09/2019
ms.openlocfilehash: 894922a80ab874e5304ef441571e03ef559a34b0
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2019
ms.locfileid: "54215421"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Limits des auf virtuellen Kernen basierenden Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank

Dieser Artikel enthält ausführliche Angaben zu Ressourcenlimits für Einzeldatenbanken in Azure SQL-Datenbank, die das auf virtuellen Kernen basierende Kaufmodell verwenden.

Das DTU-basierte Kaufmodell weist Ressourcenlimits für Einzeldatenbanken auf einem logischen Server auf. Informationen dazu finden Sie unter [Übersicht über Ressourcenlimits auf einem logischen Server](sql-database-resource-limits-logical-server.md).

> [!IMPORTANT]
> Unter bestimmten Umständen müssen Sie ggf. eine Datenbank verkleinern, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

Sie können mit [Azure-Portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases) oder [REST-API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases) Diensttarif, Computegröße und Speichermenge für eine einzelne Datenbank festlegen.

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Universelle Dienstebene: Speicher- und Computegrößen

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Universelle Dienstebene: Computeplattform der 4. Generation (Teil 1)

|Computegröße|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|3|4|5|6|
|Arbeitsspeicher (GB)|7|14|21|28|35|42|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|
|Maximale Protokollgröße (GB)|307|307|307|461|461|461|
|tempdb-Größe (GB)|32|64|96|128|160|192|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Ziel-IOPS (64 KB)|500|1000|1500|2000|2500|3000|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|000
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Universelle Dienstebene: Computeplattform der 4. Generation (Teil 2)

|Computegröße|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|7|8|9|10|16|24|
|Arbeitsspeicher (GB)|49|56|63|70|112|168|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|
|Maximale Datengröße (GB)|1536|3072|3072|3072|4096|4096|
|Maximale Protokollgröße (GB)|461|922|922|922|1229|1229|
|tempdb-Größe (GB)|224|256|288|320|384|384|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)
|Ziel-IOPS (64 KB)|3500|4000|4500|5.000|7.000|7.000|
|Max. gleichzeitige Worker (Anforderungen)|1400|1600|1800|2000|3200|4800|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Universelle Dienstebene: Computeplattform der 5. Generation (Teil 1)

|Computegröße|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|
|V-Kerne|2|4|6|8|10|12|14|
|Arbeitsspeicher (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|1536|
|Maximale Protokollgröße (GB)|307|307|307|461|461|461|461|
|tempdb-Größe (GB)|64|128|192|256|320|384|384|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Ziel-IOPS (64 KB)|500|1000|1500|2000|2500|3000|3500|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|1400|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|1|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Universelle Dienstebene: Computeplattform der 5. Generation (Teil 2)

|Computegröße|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|
|V-Kerne|16|18|20|24|32|40|80|
|Arbeitsspeicher (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Maximale Datengröße (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maximale Protokollgröße (GB)|922|922|922|1229|1229|1229|1229|
|tempdb-Größe (GB)|384|384|384|384|384|384|384|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Ziel-IOPS (64 KB)|4000|4500|5.000|6000|7.000|7.000|7.000|
|Max. gleichzeitige Worker (Anforderungen)|1600|1800|2000|2400|3200|4000|8.000|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|1|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

## <a name="business-critical-service-tier-storage-sizes-and-compute-sizes"></a>Diensttarif „Unternehmenskritisch“: Speicher- und Computegrößen

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>Diensttarif „Unternehmenskritisch“: Computeplattform der 4. Generation (Teil 1)

|Computegröße|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|3|4|5|6|
|Arbeitsspeicher (GB)|7|14|21|28|35|42|
|Columnstore-Unterstützung|N/V|N/V|N/V|N/V|N/V|N/V|
|In-Memory-OLTP-Speicher (GB)|1|2|3|4|5|6|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|
|Maximale Protokollgröße (GB)|307|307|307|307|307|307|
|tempdb-Größe (GB)|32|64|96|128|160|192|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Ziel-IOPS (64 KB)|5.000|10000|15000|20000|25000|30000|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|4|4|4|4|4|4|
|Multi-AZ|JA|Ja|Ja|Ja|Ja|JA|
|Horizontale Leseskalierung|JA|Ja|Ja|Ja|Ja|JA|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>Diensttarif „Unternehmenskritisch“: Computeplattform der 4. Generation (Teil 2)

|Computegröße|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|7|8|9|10|16|24|
|Arbeitsspeicher (GB)|49|56|63|70|112|168|
|Columnstore-Unterstützung|N/V|N/V|N/V|N/V|N/V|N/V|
|In-Memory-OLTP-Speicher (GB)|7|8|9.5|11|20|36|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|
|Maximale Protokollgröße (GB)|307|307|307|307|307|307|
|tempdb-Größe (GB)|224|256|288|320|384|384|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Ziel-IOPS (64 KB)|35000|40.000|45000|50000|80.000|120000|
|Max. gleichzeitige Worker (Anforderungen)|1400|1600|1800|2000|3200|4800|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|4|4|4|4|4|4|
|Multi-AZ|JA|Ja|Ja|Ja|Ja|JA|
|Horizontale Leseskalierung|JA|Ja|Ja|Ja|Ja|JA|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>Diensttarif „Unternehmenskritisch“: Computeplattform der 5. Generation (Teil 1)

|Computegröße|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|
|V-Kerne|2|4|6|8|10|12|14|
|Arbeitsspeicher (GB)|11|22|33|44|55|66|77|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|1,571|3,142|4,713|6,284|8,655|11,026|13,397|
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|1536|
|Maximale Protokollgröße (GB)|307|307|307|461|461|461|461|
|tempdb-Größe (GB)|64|128|192|256|320|384|384|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Ziel-IOPS (64 KB)|5.000|10000|15000|20000|25000|30000|35000|
|Max. gleichzeitige Worker (Anforderungen)|200|400|600|800|1000|1200|1400|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|4|4|4|4|4|4|4|
|Multi-AZ|JA|Ja|Ja|Ja|Ja|Ja|JA|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>Diensttarif „Unternehmenskritisch“: Computeplattform der 5. Generation (Teil 2)

|Computegröße|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|
|V-Kerne|16|18|20|24|32|40|80|
|Arbeitsspeicher (GB)|88|99|110|132|176|220|440|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|15,768|18,139|20,51|25,252|37,936|52,22|131,64|
|Maximale Datengröße (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maximale Protokollgröße (GB)|922|922|922|1229|1229|1229|1229|
|tempdb-Größe (GB)|384|384|384|384|384|384|384|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Ziel-IOPS (64 KB)|40.000|45000|50000|60000|80.000|100.000|200.000|
|Max. gleichzeitige Worker (Anforderungen)|1600|1800|2000|2400|3200|4000|8.000|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|4|4|4|4|4|4|
|Multi-AZ|JA|Ja|Ja|Ja|Ja|JA|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|

## <a name="hyperscale-service-tier-preview"></a>Diensttarif „Hyperscale“ (Vorschau)

### <a name="generation-4-compute-platform-storage-sizes-and-compute-sizes"></a>Computeplattform der 4. Generation: Speicher- und Computegrößen

|Leistungsstufe|HS_Gen4_1|HS_Gen4_2|HS_Gen4_4|HS_Gen4_8|HS_Gen4_16|HS_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|
|Maximale Datengröße (TB)|100 |100 |100 |100 |100 |100 |
|Maximale Protokollgröße (TB)|1 |1 |1 |1 |1 |1 |
|tempdb-Größe (GB)|32|64|128|256|384|384|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Ziel-IOPS (64 KB)|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|
|E/A-Wartezeit (ungefähr)|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|3200|4800|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|2|2|2|2|2|2|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|JA|Ja|Ja|Ja|Ja|JA|
|Enthaltener Sicherungsspeicher|7|7|7|7|7|7|
|||

### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation

|Leistungsstufe|HS_Gen5_2|HS_Gen5_4|HS_Gen5_8|HS_Gen5_16|HS_Gen5_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|10.2|20.4|40.8|81.6|122.4|163.2|204|408|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Maximale Datengröße (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|Maximale Protokollgröße (TB)|1 |1 |1 |1 |1 |1 |1 |1 |
|tempdb-Größe (GB)|64|128|256|384|384|384|384|384|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Ziel-IOPS (64 KB)|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|
|E/A-Wartezeit (ungefähr)|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|Noch festzulegen|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|2400|3200|4000|8.000|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|2|2|2|2|2|2|2|2|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|Enthaltener Sicherungsspeicher (Grenzwert der Vorschau)|7|7|7|7|7|7|7|7|
|||

## <a name="next-steps"></a>Nächste Schritte

- Antworten auf häufig gestellte Fragen finden Sie unter [SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md).
- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
