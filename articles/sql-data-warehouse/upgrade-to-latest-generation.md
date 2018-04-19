---
title: Upgrade auf Azure SQL Data Warehouse der neuesten Generation | Microsoft-Dokumentation
description: Schritte zum Durchführen eines Upgrades für Azure SQL Data Warehouse auf die Azure-Hardware- und -Speicherarchitektur der neuesten Generation.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.services: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/02/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 6ea45398b0bf7fca43c75797313b7e683972b1ab
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Optimieren der Leistung durch ein Upgrade von SQL Data Warehouse

Sie können jetzt im Azure-Portal nahtlos ein Upgrade auf die Leistungsstufe „Optimiert für Compute“ durchführen. Wenn Sie über ein Data Warehouse der Stufe „Optimiert für Elastizität“ verfügen, empfiehlt es sich, ein Upgrade auf die neueste Generation der Azure-Hardware und eine erweiterte Speicherarchitektur durchzuführen. Auf diese Weise können Sie von höherer Leistung und Skalierbarkeit sowie unbegrenztem spaltenbasiertem Speicher profitieren. 

## <a name="applies-to"></a>Anwendungsbereich
Dieses Upgrade gilt für Data Warehouses in der Leistungsstufe „Optimiert für Elastizität“.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="before-you-begin"></a>Voraussetzungen

> [!NOTE]
> Ab 30.3. muss die [Überwachung auf Serverebene](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing#subheading-8) deaktiviert sein, bevor Sie das Upgrade starten.
> 
>

> [!NOTE]
> Wenn sich Ihr vorhandenes Data Warehouse der Stufe „Optimiert für Elastizität“ nicht in einer Region befindet, in der „Optimiert für Compute“ verfügbar ist, können Sie über PowerShell eine [Geowiederherstellung auf „Optimiert für Compute](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) in einer unterstützten Region durchführen.
> 
>

1. Wenn das Data Warehouse „Optimiert für Elastizität“, für das ein Upgrade durchgeführt werden soll, angehalten wird, [setzen Sie das Data Warehouse fort](pause-and-resume-compute-portal.md).
2. Es tritt eine Ausfallzeit von einigen Minuten auf. 



## <a name="start-the-upgrade"></a>Starten des Upgrades

1. Wechseln Sie im Azure-Portal zu Ihrem für Elastizität optimierten Data Warehouse, und klicken Sie auf **Upgrade auf „Optimiert für Compute“ durchführen**: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png).

2. Wählen Sie standardmäßig **die vorgeschlagene Leistungsstufe** für das Data Warehouse aus, basierend auf Ihrer aktuellen Leistungsstufe für „Optimiert für Elastizität“. Verwenden Sie dafür die folgende Zuordnung:
    
| Optimiert für Elastizität | Optimiert für Compute |
| :----------------------: | :-------------------: |
|      DW100 – DW1000      |        DW1000c        |
|          DW1200          |        DW1500c        |
|          DW1500          |        DW1500c        |
|          DW2000          |        DW2000c        |
|          DW3000          |        DW3000c        |
|          DW6000          |        DW6000c        |


3. Stellen Sie sicher, dass die Ausführung Ihrer Workload abgeschlossen ist und die Workload stillgelegt wurde, bevor Sie das Upgrade durchführen. Bevor Ihr Data Warehouse auf der Stufe „Optimiert für Compute“ wieder online geschaltet wird, tritt eine Ausfallzeit von einigen Minuten auf. Klicken Sie auf **Upgrade durchführen**. Die Leistungsstufe „Optimiert für Compute“ ist in der Vorschauphase gegenwärtig zum halben Preis erhältlich:
    
    ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Überwachen Sie Ihr Upgrade**, indem Sie den Status im Azure-Portal überprüfen:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   Der erste Schritt des Upgradeprozesses erfolgt im Skalierungsvorgang („Upgrade – Offline“), in dem alle Sitzungen gelöscht und Verbindungen getrennt werden. 
   
   Der zweite Schritt des Upgradeprozesses ist die Datenmigration („Upgrade – Online“). Die Datenmigration ist ein im Hintergrund ausgeführter allmählicher Onlineprozess, der spaltenbasierte Daten langsam aus der alten Gen1-Speicherarchitektur in die neue Gen2-Speicherarchitektur verschiebt, um den lokalen Gen2-SSD-Cache zu nutzen. Während dieser Zeit ist Ihr Data Warehouse für Abfrage- und Ladevorgänge online. All Ihre Daten stehen für Abfragen zur Verfügung, unabhängig davon, ob sie bereits migriert wurden oder nicht. Die Datenmigration erfolgt mit wechselnder Geschwindigkeit je nach Größe Ihrer Daten, Ihrer Leistungsstufe und der Anzahl Ihrer Columnstore-Segmente. 

5. **Optionale Empfehlung**: Um den Hintergrundprozess der Datenmigration zu beschleunigen, empfiehlt es sich, die Datenverschiebung sofort zu erzwingen, indem Sie [ALTER INDEX REBUILD](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-tables-index) in allen Columnstore-Tabellen mit einem größeren SLO-Wert und einer größeren Ressourcenklasse auszuführen. Der Vorgang erfolgt offline, im Gegensatz zum allmählich ausgeführten Hintergrundprozess. Die Datenmigration erfolgt jedoch wesentlich schneller ausgeführt, wenn Sie die Gen2-Speicherarchitektur vollständig nutzen können, sobald diese mit hochwertigen Zeilengruppen fertig erstellt wurde. 

Die folgende Abfrage generiert die erforderlichen ALTER INDEX REBUILD-Befehle, um den Datenmigrationsprozess zu beschleunigen:

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON [' 
       + Schema_name(tbl.schema_id) + '].[' 
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE 
                                                         WHEN ( 
                                                     (SELECT Count(*) 
                                                      FROM   sys.partitions 
                                                             part2 
                                                      WHERE  part2.index_id 
                                                             = idx.index_id 
                                                             AND 
                                                     idx.object_id = 
                                                     part2.object_id) 
                                                     > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              ELSE '' 
                                                       END ) + '; SELECT ''[' + 
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' + 
              Object_name(idx.object_id) + '] ' + ( 
              CASE 
                WHEN ( (SELECT Count(*) 
                        FROM   sys.partitions 
                               part2 
                        WHERE 
                     part2.index_id = 
                     idx.index_id 
                     AND idx.object_id 
                         = part2.object_id) > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              + ' completed'';' 
              ELSE ' completed'';' 
                                                    END ) 
FROM   sys.indexes idx 
       INNER JOIN sys.tables tbl 
               ON idx.object_id = tbl.object_id 
       LEFT OUTER JOIN sys.partitions part 
                    ON idx.index_id = part.index_id 
                       AND idx.object_id = part.object_id 
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE'; 
```



## <a name="next-steps"></a>Nächste Schritte
Ihr aktualisiertes Data Warehouse ist online. Informationen zur optimalen Nutzung der erweiterten Architektur finden Sie unter [Ressourcenklassen für die Workloadverwaltung](resource-classes-for-workload-management.md).
 
