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
ms.date: 10/15/2018
ms.openlocfilehash: 12074ad28e27a249a6dc378986f014ede1cd2ab3
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353579"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Limits des auf virtuellen Kernen basierenden Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank

Dieser Artikel enthält ausführliche Angaben zu Ressourcenlimits für Einzeldatenbanken in Azure SQL-Datenbank, die das auf virtuellen Kernen basierende Kaufmodell verwenden.

Das DTU-basierte Kaufmodell weist Ressourcenlimits für Einzeldatenbanken auf einem logischen Server auf. Informationen dazu finden Sie unter [Übersicht über Ressourcenlimits auf einem logischen Server](sql-database-resource-limits-logical-server.md).

> [!IMPORTANT]
> Unter bestimmten Umständen müssen Sie ggf. eine Datenbank verkleinern, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

Sie können mit [Azure-Portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases) oder [REST-API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases) Diensttarif, Computegröße und Speichermenge für eine einzelne Datenbank festlegen.

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Diensttarif „Universell“: Speicher- und Computegrößen

### <a name="generation-4-compute-platform"></a>Computeplattform der 4. Generation

|Computegröße|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1536|3072|4096|4096|
|Maximale Protokollgröße (GB)|307|307|461|922|1229|1229|
|tempdb-Größe (GB)|32|64|128|256|384|384|
|Ziel-IOPS (64 KB)|500|1000|2000|4000|7.000|7.000|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|3200|4800|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|000
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation

|Computegröße|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|Storage Premium (Remote)|
|E/A-Wartezeit (ungefähr)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|5-7 ms (Schreiben)<br>5-10 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Maximale Protokollgröße (GB)|307|307|461|614|1229|1229|1229|1229|
|tempdb-Größe (GB)|64|128|256|384|384|384|384|384|
|Ziel-IOPS (64 KB)|500|1000|2000|4000|6000|7.000|7.000|7.000|
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|2400|3200|4000|8.000|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|1|1|1|1|1|1|1|1|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

## <a name="business-critical-service-tier-storage-sizes-and-compute-sizes"></a>Diensttarif „Unternehmenskritisch“: Speicher- und Computegrößen

### <a name="generation-4-compute-platform"></a>Computeplattform der 4. Generation

|Computegröße|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|N/V|N/V|N/V|N/V|N/V|N/V|
|In-Memory-OLTP-Speicher (GB)|1|2|4|8|20|36|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|
|Maximale Protokollgröße (GB)|307|307|307|307|307|307|
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

### <a name="generation-5-compute-platform"></a>Computeplattform der 5. Generation

|Computegröße|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|5|5|5|5|5|5|5|5|
|V-Kerne|2|4|8|16|24|32|40|80|
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|1,571|3,142|6,284|15,768|25,252|37,936|52,22|131,64|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|E/A-Wartezeit (ungefähr)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|1-2 ms (Schreiben)<br>1-2 ms (Lesen)|
|Maximale Datengröße (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maximale Protokollgröße (GB)|307|307|307|307|614|1229|1229|1229|
|tempdb-Größe (GB)|64|128|256|384|384|384|384|384|
|Ziel-IOPS (64 KB)|5.000|10000|20000|40.000|60000|80.000|100.000|200.000
|Max. gleichzeitige Worker (Anforderungen)|200|400|800|1600|2400|3200|4000|8.000|
|Maximal zulässige Sitzungen|30000|30000|30000|30000|30000|30000|30000|30000|
|Anzahl von Replikaten|3|3|3|3|3|3|3|3|
|Multi-AZ|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Horizontale Leseskalierung|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Enthaltener Sicherungsspeicher|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|1 × Datenbankgröße|
|||

## <a name="hyperscale-service-tier-preview"></a>Diensttarif „Hyperscale“ (Vorschau)

### <a name="generation-4-compute-platform-storage-sizes-and-compute-sizes"></a>Computeplattform der 4. Generation: Speicher- und Computegrößen

|Leistungsstufe|HS_Gen4_1|HS_Gen4_2|HS_Gen4_4|HS_Gen4_8|HS_Gen4_16|HS_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |--: |
|Hardwaregeneration|4|4|4|4|4|4|
|V-Kerne|1|2|4|8|16|24|
|Arbeitsspeicher (GB)|7|14|28|56|112|168|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (TB)|100 |100 |100 |100 |100 |100 |
|Maximale Protokollgröße (TB)|1 |1 |1 |1 |1 |1 |
|tempdb-Größe (GB)|32|64|128|256|384|384|
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
|Arbeitsspeicher (GB)|11|22|44|88|132|176|220|440|
|Columnstore-Unterstützung|JA|Ja|Ja|Ja|Ja|Ja|Ja|JA|
|In-Memory-OLTP-Speicher (GB)|N/V|N/V|N/V|N/V|N/V|N/V|N/V|N/V|
|Speichertyp|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|Lokale SSD|
|Maximale Datengröße (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|Maximale Protokollgröße (TB)|1 |1 |1 |1 |1 |1 |1 |1 |
|tempdb-Größe (GB)|64|128|256|384|384|384|384|384|
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
