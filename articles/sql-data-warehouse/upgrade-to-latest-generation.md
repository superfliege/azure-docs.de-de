---
title: Upgrade auf Azure SQL Data Warehouse der neuesten Generation | Microsoft-Dokumentation
description: Führen Sie ein Upgrade für Azure SQL Data Warehouse auf die Azure-Hardware- und -Speicherarchitektur der neuesten Generation aus.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/22/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b7765182a89e08a2b2b8fcdbdfed82c64893fc27
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306979"
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Optimieren der Leistung durch ein Upgrade von SQL Data Warehouse
Führen Sie ein Upgrade für Azure SQL Data Warehouse auf die Azure-Hardware- und -Speicherarchitektur der neuesten Generation aus.

## <a name="why-upgrade"></a>Gründe für ein Upgrade
Sie können jetzt im Azure-Portal nahtlos ein Upgrade auf die Stufe „SQL Data Warehouse Compute Optimized Gen2“ durchführen. Ein Upgrade wird empfohlen, wenn Sie über ein Data Warehouse der Stufe „Optimiert für Compute Gen 1“ verfügen. Durch das Upgrade können Sie die neueste Generation der Azure-Hardware- und erweiterten Azure-Speicherarchitektur verwenden. Auf diese Weise profitieren Sie von höherer Leistung und Skalierbarkeit sowie unbegrenztem spaltenbasiertem Speicher. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Anwendungsbereich
Dieses Upgrade gilt für Data Warehouses der Stufe „Optimiert für Compute Gen 1“.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="before-you-begin"></a>Voraussetzungen
> [!NOTE]
> Wenn sich Ihr vorhandenes Data Warehouse der Stufe „Optimiert für Compute Gen 1“ nicht in einer Region befindet, in der „Optimiert für Compute Gen 2“ verfügbar ist, können Sie über PowerShell eine [Geowiederherstellung ](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) in einer unterstützten Region durchführen.
> 
>

1. Wenn das Data Warehouse mit der Stufe „Optimiert für Compute Gen 1“, für das ein Upgrade durchgeführt werden soll, angehalten wird, [setzen Sie das Data Warehouse fort](pause-and-resume-compute-portal.md).
2. Es tritt eine Ausfallzeit von einigen Minuten auf. 



## <a name="start-the-upgrade"></a>Starten des Upgrades

1. Navigieren Sie im Azure-Portal zu Ihrem Data Warehouse der Stufe „Optimiert für Compute Gen 1“, und klicken Sie auf **Upgrade auf Gen2**: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)

2. Wählen Sie standardmäßig **die vorgeschlagene Leistungsstufe** für das Data Warehouse aus, basierend auf Ihrer aktuellen Leistungsstufe für „Optimiert für Compute Gen 1“. Verwenden Sie dafür die folgende Zuordnung:
    
   | Stufe „Optimiert für Compute Gen 1“ | Stufe „Optimiert für Compute Gen 2“ |
   | :----------------------: | :-------------------: |
   |      DW100 – DW1000      |        DW1000c        |
   |          DW1200          |        DW1500c        |
   |          DW1500          |        DW1500c        |
   |          DW2000          |        DW2000c        |
   |          DW3000          |        DW3000c        |
   |          DW6000          |        DW6000c        |

3. Stellen Sie sicher, dass die Ausführung Ihrer Workload abgeschlossen ist und die Workload stillgelegt wurde, bevor Sie das Upgrade durchführen. Bevor Ihr Data Warehouse auf der Stufe „Stufe „Optimiert für Compute Gen 2“ wieder online geschaltet wird, tritt eine Ausfallzeit von einigen Minuten auf. Klicken Sie auf **Upgrade durchführen**. Die Leistungsstufe „Optimiert für Compute Gen 2“ ist in der Vorschauphase gegenwärtig zum halben Preis erhältlich:
    
   ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Überwachen Sie Ihr Upgrade**, indem Sie den Status im Azure-Portal überprüfen:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   Der erste Schritt des Upgradeprozesses erfolgt im Skalierungsvorgang („Upgrade – Offline“), in dem alle Sitzungen gelöscht und Verbindungen getrennt werden. 
   
   Der zweite Schritt des Upgradeprozesses ist die Datenmigration („Upgrade – Online“). Die Datenmigration ist ein im Hintergrund ausgeführter allmählicher Onlineprozess, der spaltenbasierte Daten langsam aus der alten Speicherarchitektur in die neue Speicherarchitektur verschiebt, indem der lokale SSD-Cache genutzt wird. Während dieser Zeit ist Ihr Data Warehouse für Abfrage- und Ladevorgänge online. All Ihre Daten stehen für Abfragen zur Verfügung, unabhängig davon, ob sie bereits migriert wurden oder nicht. Die Datenmigration erfolgt mit wechselnder Geschwindigkeit je nach Größe Ihrer Daten, Ihrer Leistungsstufe und der Anzahl Ihrer Columnstore-Segmente. 

5. **Optionale Empfehlung**: Um den Hintergrundprozess der Datenmigration zu beschleunigen, empfiehlt es sich, die Datenverschiebung sofort zu erzwingen, indem Sie [ ALTER INDEX REBUILD](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index) in allen primären Columnstore-Tabellen ausführen, die Sie mit einem größeren SLO-Wert und einer größeren Ressourcenklasse abrufen würden. Der Vorgang erfolgt **offline**, im Gegensatz zum allmählich ausgeführten Hintergrundprozess, der je nach Anzahl und Größe Ihrer Tabellen mehrere Stunden dauern kann. Die Datenmigration läuft jedoch wesentlich schneller ab, wenn Sie die neue erweiterte Speicherarchitektur vollständig nutzen können, sobald diese mit hochwertigen Zeilengruppen fertig erstellt wurde. 

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
 
