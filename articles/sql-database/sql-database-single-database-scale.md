---
title: Skalieren von Einzeldatenbankressourcen – Azure SQL-Datenbank | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie die für eine Einzeldatenbank in Azure SQL-Datenbank verfügbaren Compute- und Speicherressourcen skaliert werden können.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 525416506a22f386de574ca02b2e919ac47b8737
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36311134"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Skalieren von Einzeldatenbankressourcen in Azure SQL-Datenbank

In diesem Artikel wird beschrieben, wie die für eine Einzeldatenbank in Azure SQL-Datenbank verfügbaren Compute- und Speicherressourcen skaliert werden können. 

## <a name="vcore-based-purchasing-model-change-storage-size"></a>Auf virtuellen Kernen basierendes Kaufmodell: Ändern der Speichergröße

- Speicher kann in Schritten von 1 GB bis zur maximalen Speichergröße bereitgestellt werden. Die konfigurierbare Mindestdatenspeichergröße beträgt 5 GB 
- Speicher für eine Einzeldatenbank kann durch Erhöhen oder Verringern der maximalen Größe im [Azure-Portal](https://portal.azure.com), in [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), in [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), in der[Azure CLI](/cli/azure/sql/db#az_sql_db_update) oder in der [REST-API](/rest/api/sql/databases/update) bereitgestellt werden.
- SQL-Datenbank belegt automatisch 30 % des zusätzlichen Speicherplatzes für die Protokolldateien und 32 GB pro virtuellem Kern für die tempdb-Datenbank, wobei 384 GB nicht überschritten werden. Die tempdb befindet sich in allen Dienstebenen auf einem angefügten SSD-Datenträger.
- Der Preis für den Speicher für eine einzelne Datenbank errechnet sich aus der Summe des Datenspeichers und des Protokollspeichers multipliziert mit dem Speichereinheitenpreis für die Dienstebene. Die Kosten für die tempdb-Datenbank sind im Preis für den virtuellen Kern enthalten. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="vcore-based-purchasing-model-change-compute-resources"></a>Auf virtuellen Kernen basierendes Kaufmodell: Ändern von Computeressourcen

Nach der anfänglichen Auswahl der Anzahl an virtuellen Kernen können Sie eine Einzeldatenbank je nach tatsächlichem Bedarf im [Azure-Portal](sql-database-single-database-scale.md#azure-portal-manage-logical-servers-and-databases), in [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), in [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), in der [Azure CLI](/cli/azure/sql/db#az_sql_db_update) oder in der [REST-API](/rest/api/sql/databases/update) dynamisch zentral hoch- oder herunterskalieren. 

Wenn Sie die Dienst- und/oder die Leistungsstufe einer Datenbank ändern, wird ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsebene erstellt und anschließend die Verbindung auf dieses Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Die Dauer der Umstellung kann variieren, aber sie liegt im Allgemeinen unter vier Sekunden und in 99 % der Fälle unter 30 Sekunden. Falls im Moment der Verbindungstrennung viele Transaktionen stattfinden, kann die Umstellung unter Umständen auch länger dauern. 

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte die zentrale Hochskalierung für eine 250-GB-Datenbank beim Wechsel in einen, aus einem oder innerhalb eines allgemeinen Diensttarifs innerhalb von sechs Stunden abgeschlossen sein. Für eine Datenbank der gleichen Größe, deren Leistungsstufe innerhalb des unternehmenskritischen Diensttarifs geändert wird, sollte das zentrale Hochskalieren innerhalb von drei Stunden beendet sein.

> [!TIP]
> Weitere Informationen zum Überwachen laufender Vorgänge finden Sie unter: [Verwalten von Vorgängen mithilfe der SQL-REST-API](/rest/api/sql/Operations/List), [Verwalten von Vorgängen mithilfe der Befehlszeilenschnittstelle](/cli/azure/sql/db/op) und [Überwachen von Vorgängen mit T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). Verwenden Sie auch diese beiden PowerShell-Befehle: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) und [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Wenn Sie ein Upgrade auf eine höhere Dienstebene oder Leistungsstufe durchführen, wird die maximale Datenbankgröße nicht erhöht, sofern Sie nicht ausdrücklich eine höhere Maximalgröße angeben.
* Für ein Downgrade einer Datenbank muss die verwendete Datenbankmenge kleiner als die maximal zulässige Größe der Zieldienstebene und Leistungsstufe sein. 
* Beim Upgrade einer Datenbank mit aktivierter [Georeplikation](sql-database-geo-replication-portal.md) führen Sie vor dem Upgrade der primären Datenbank zunächst ein Upgrade der zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe durch (allgemeine Richtlinie für optimale Leistung). Bei einem Upgrade auf eine andere Edition muss zunächst ein Upgrade der sekundären Datenbank durchgeführt werden.
* Beim Downgrade einer Datenbank mit aktivierter [Georeplikation](sql-database-geo-replication-portal.md) führen Sie vor dem Downgrade der sekundären Datenbank zunächst ein Downgrade der zugehörigen primären Datenbanken auf die gewünschte Leistungsstufe durch (allgemeine Richtlinie für optimale Leistung). Bei einem Downgrade auf eine andere Edition muss zunächst ein Downgrade der primären Datenbank durchgeführt werden.
* Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.

## <a name="dtu-based-purchasing-model-change-storage-size"></a>Auf DTUs basierendes Kaufmodell: Ändern der Speichergröße

- Der DTU-Preis für eine einzelne Datenbank enthält eine bestimmte Menge Speicher ohne zusätzliche Kosten. Zusätzlicher Speicher über die inbegriffene Speichermenge hinaus kann gegen zusätzliche Gebühren bis zur Obergrenze in Inkrementen von 250 GB bis zu 1 TB und dann in Inkrementen von 256 GB über 1 TB hinaus bereitgestellt werden. Informationen zu enthaltenen Speichermengen und Maximalgrößen finden Sie unter [Einzeldatenbank: Speichergrößen und Leistungsstufen](#single-database-storage-sizes-and-performance-levels).
- Zusätzlicher Speicher für eine einzelne Datenbank kann durch Erhöhen der maximalen Größe mithilfe von [Azure-Portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update) oder [REST-API](/rest/api/sql/databases/update) bereitgestellt werden.
- Der Preis für zusätzlichen Speicher für eine einzelne Datenbank errechnet sich aus der Menge an zusätzlich bereitgestelltem Speicher multipliziert mit dem Einheitenpreis für zusätzlichen Speicher für die Dienstebene. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="dtu-based-purchasing-model-change-compute-resources-dtus"></a>Auf DTUs basierendes Kaufmodell: Ändern von Computeressourcen (DTUs)

Nach erster Auswahl von Dienstebene, Leistungsstufe und Speichermenge können Sie eine einzelne Datenbank basierend auf der tatsächlichen Erfahrung mit [Azure-Portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update) oder [REST-API](/rest/api/sql/databases/update) dynamisch zentral hoch- oder herunterskalieren. 

Das folgende Video zeigt die dynamische Änderung der Leistungsstufe zum Heraufsetzen verfügbarer DTUs für eine einzelne Datenbank.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Wenn Sie die Dienst- und/oder die Leistungsstufe einer Datenbank ändern, wird ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsebene erstellt und anschließend die Verbindung auf dieses Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Die Dauer der Umstellung kann variieren, aber sie liegt in 99 Prozent der Fälle unter 30 Sekunden. Falls im Moment der Verbindungstrennung viele Transaktionen stattfinden, kann die Umstellung unter Umständen auch länger dauern. 

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte eine 250-GB-Datenbank beim Wechsel in einen, aus einem oder innerhalb eines Standarddiensttarifs innerhalb von sechs Stunden zentral hochskalieren. Eine Datenbank der gleichen Größe, die ihre Leistungsstufen innerhalb der Premium-Dienstebene ändert, sollte das zentrale Hochskalieren innerhalb von drei Stunden vollziehen.

> [!TIP]
> Weitere Informationen zum Überwachen laufender Vorgänge finden Sie unter: [Verwalten von Vorgängen mithilfe der SQL-REST-API](/rest/api/sql/Operations/List), [Verwalten von Vorgängen mithilfe der Befehlszeilenschnittstelle](/cli/azure/sql/db/op) und [Überwachen von Vorgängen mit T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). Verwenden Sie auch diese beiden PowerShell-Befehle: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) und [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Wenn Sie ein Upgrade auf eine höhere Dienstebene oder Leistungsstufe durchführen, wird die maximale Datenbankgröße nicht erhöht, sofern Sie nicht ausdrücklich eine höhere Maximalgröße angeben.
* Für ein Downgrade einer Datenbank muss die verwendete Datenbankmenge kleiner als die maximal zulässige Größe der Zieldienstebene und Leistungsstufe sein. 
* Bei einem Downgrade von **Premium** auf die **Standard**-Ebene fallen zusätzliche Speicherkosten an, wenn sowohl (1) die maximale Größe der Datenbank von der Zielleistungsstufe unterstützt wird als auch (2) die maximale Größe die enthaltene Speichermenge der Zielleistungsstufe überschreitet. Wenn z.B. eine P1-Datenbank mit einer maximalen Größe von 500 GB auf S3 reduziert wird, fallen zusätzliche Speicherkosten an, da S3 eine maximale Größe von 500 GB unterstützt und die enthaltene Speichermenge nur 250 GB beträgt. Daher beträgt die zusätzliche Speichermenge 500 GB – 250 GB = 250 GB. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/). Wenn die tatsächlich verwendete Speichermenge kleiner als die enthaltene Speichermenge ist, können Sie diese zusätzlichen Kosten vermeiden, indem Sie die maximale Datenbankgröße auf die enthaltene Menge reduzieren. 
* Beim Upgrade einer Datenbank mit aktivierter [Georeplikation](sql-database-geo-replication-portal.md) führen Sie vor dem Upgrade der primären Datenbank zunächst ein Upgrade der zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe durch (allgemeine Richtlinie für optimale Leistung). Bei einem Upgrade auf eine andere Edition muss zunächst ein Upgrade der sekundären Datenbank durchgeführt werden.
* Beim Downgrade einer Datenbank mit aktivierter [Georeplikation](sql-database-geo-replication-portal.md) führen Sie vor dem Downgrade der sekundären Datenbank zunächst ein Downgrade der zugehörigen primären Datenbanken auf die gewünschte Leistungsstufe durch (allgemeine Richtlinie für optimale Leistung). Bei einem Downgrade auf eine andere Edition muss zunächst ein Downgrade der primären Datenbank durchgeführt werden.
* Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade auf den Tarif **Basic** durchführen, verfügen Sie über einen kürzeren Aufbewahrungszeitraum von Sicherungen. Weitere Informationen finden Sie unter [Azure SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.

## <a name="dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Auf DTUs basierendes Kaufmodell: Einschränkungen von P11 und P15 bei Maximalgrößen über 1 TB

Eine maximale Größe von mehr als 1 TB für P11- und P15-Datenbanken wird in folgenden Regionen unterstützt: „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „USA, Mitte“, „Frankreich, Mitte“, „Deutschland, Mitte“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“ und „Europa, Westen“. Die folgenden Aspekte und Einschränkungen gelten für P11- und P15-Datenbanken mit einer maximalen Größe von mehr als 1 TB:

- Wenn Sie beim Erstellen einer Datenbank eine Maximalgröße von mehr als 1 TB wählen (Wert 4 TB oder 4.096 GB), tritt für den Erstellungsbefehl ein Fehler auf, falls die Datenbank in einer nicht unterstützten Region bereitgestellt wird.
- Für vorhandene P11- und P15-Datenbanken, die sich in einer der unterstützten Regionen befinden, können Sie den maximalen Speicher über 1 TB hinaus in Inkrementen von 256 GB auf bis zu 4 TB erhöhen. Um festzustellen, ob eine größere Größe in Ihrer Region unterstützt wird, verwenden Sie die Funktion [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql), oder überprüfen Sie die Größe der Datenbank im Azure-Portal. Die Aktualisierung einer vorhandenen P11- oder P15-Datenbank kann nur mithilfe einer Prinzipalanmeldung auf Serverebene oder von Mitgliedern der Datenbankrolle „dbmanager“ ausgeführt werden. 
- Bei Ausführung eines Upgradevorgangs in einer unterstützten Region wird die Konfiguration umgehend aktualisiert. Die Datenbank bleibt während des Upgradevorgangs online. Allerdings können Sie die vollständige Menge an Speicher über 1 TB erst dann nutzen, wenn die tatsächlichen Datenbankdateien auf die neue Maximalgröße aktualisiert wurden. Die erforderliche Dauer hängt von der Größe der zu aktualisierenden Datenbank ab. 
- Beim Erstellen oder Aktualisieren einer P11- oder P15-Datenbank können Sie nur in Inkrementen von 256 GB zwischen einer Maximalgröße von 1 TB und 4 TB wählen. Beim Erstellen einer P11-/P15-Datenbank ist die Standardspeicheroption von 1 TB vorausgewählt. Für Datenbanken, die sich in einer der unterstützten Regionen befinden, können Sie das Speichermaximum für eine neue oder vorhandene Einzeldatenbank auf maximal 4 TB erhöhen. Für alle anderen Regionen kann die Maximalgröße nicht mehr als 1 TB betragen. Der Preis ändert sich nicht, wenn Sie 4TB Speicher wählen.
- Wenn die maximale Größe einer Datenbank auf mehr als 1 TB festgelegt ist, kann sie auch dann nicht in 1 TB geändert werden, wenn die tatsächlich verwendete Speichermenge unter 1 TB liegt. Folglich können Sie eine P11- oder P15-Datenbank mit einer maximalen Größe über 1 TB nicht auf eine 1-TB-P11- oder 1-TB-P15- oder niedrigere Leistungsstufe (z.B. P1-P6) herabstufen. Diese Einschränkung gilt auch für Wiederherstellungs- und Kopierszenarien, einschließlich Point-in-Time- und Geowiederherstellung, langfristige Aufbewahrung von Sicherungen und Datenbankkopiervorgänge. Sobald eine Datenbank mit einer maximalen Größe über 1 TB konfiguriert wird, müssen alle Wiederherstellungsvorgänge dieser Datenbank für ein P11-/P15-Ziel mit einer maximalen Größe von über 1 TB ausgeführt werden.
- Szenarien für aktive Georeplikation:
   - Einrichten einer Georeplikationsbeziehung: Falls es sich bei der primären Datenbank um eine P11- oder P15-Datenbank handelt, müssen auch die sekundären Datenbanken vom Typ „P11“ oder „P15“ sein. Niedrigere Leistungsstufen werden als sekundäre Datenbanken abgelehnt, da sie mehr als 1 TB nicht unterstützen können.
   - Aktualisieren der primären Datenbank in einer Georeplikationsbeziehung: Die Änderung der maximalen Größe für eine primäre Datenbank in mehr als 1 TB löst die gleiche Änderung für die sekundäre Datenbank aus. Beide Upgrades müssen erfolgreich ausgeführt werden, damit die Änderung für die primäre Datenbank wirksam wird. Für die Option mit mehr als 1 TB gelten Regionseinschränkungen. Wenn sich die sekundäre Datenbank in einer Region befindet, die nicht mehr als 1 TB unterstützt, wird die primäre Datenbank nicht aktualisiert.
- Die Verwendung des Import/Export-Diensts zum Laden von Datenbanken des Typs „P11-P15“ mit mehr als 1 TB wird nicht unterstützt. Verwenden Sie „SqlPackage.exe“, um Daten zu [importieren](sql-database-import.md) und [exportieren](sql-database-export.md).


## <a name="next-steps"></a>Nächste Schritte

Allgemeine Ressourcenlimits finden Sie unter [Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells in SQL-Datenbank – Pools für elastische Datenbanken](sql-database-vcore-resource-limits-single-databases.md) und unter [Ressourcenlimits des auf DTUs basierenden Kaufmodells in SQL-Datenbank – Pools für elastische Datenbanken](sql-database-dtu-resource-limits-single-databases.md).
